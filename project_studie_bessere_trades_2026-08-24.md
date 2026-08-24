---
name: project-studie-bessere-trades-2026-08-24
description: "Gemeinsame Fable+Opus-Studie 24.08.2026 (Abend): Wie mehr genommene Trades TP statt SL erreichen, ohne Frequenz zu senken. Kernbefund verifiziert (TP1-Breakeven 29,8% exakt bestätigt), Retest-Entry-vs-13.1-Chasing-Konflikt via Zeitbox gelöst, Q-Score kritisch geprüft (Pflichtzeilen-Last), DB-Erweiterung + skipped_setups auf Machbarkeit geprüft, check_violation_sync.cjs als zwei getrennte Probleme entlarvt (SL/ATR-Teil erledigt, DB-Text-Sync-Teil offen). Phase 0 (Abschnitt 6) am 24.08. Abend UMGESETZT (Nachtrag Abschnitt 7): tp1_hit-Fix #31/#32, sieben neue DB-Spalten, exit_type rückwirkend für 42/43 Trades, skipped_setups-Tabelle+Skript, Tagesabschluss-Pflichtpunkt 4, gate_check.cjs-Verankerung bei 8c/7b1. Phase 1 (Retest-Zeitbox + Q-Score) am 24.08. Nacht UMGESETZT (Nachtrag Abschnitt 8): Retest-Zeitbox in 8b Schritt 4 + Verzahnung mit 13.1, Q-Score (Q1-Q4) in gate_check.cjs als Entry-Freigabe-Suffix, Anzeige-Modus für Trade 1-8, Auswertung nach Trade 15 offen"
metadata:
  node_type: memory
  type: project
  originSessionId: 7fd689fe-360d-4b72-83f7-c6bb3d69c2dc
  modified: 2026-08-24T12:30:15.598Z
---

Siehe [[project_opus_vollpruefung_2026-08-24]] für die volle Historie des 24.08. (C-1 bis C-4, gate_check.cjs, die 11-Punkte-Umsetzung vom Abend). Diese Studie ist Opus' Entwurf vom späten 24.08., von Fable an der DB verifiziert und an mehreren Stellen weiterentwickelt/korrigiert — als EIN Dokument, [Fable-Ergänzung]/[Fable-Korrektur] markiert die Stellen mit eigenem Beitrag.

# STUDIE (final v1.0) — Wie das System den Markt besser liest, damit mehr genommene Trades TP statt SL erreichen

Auftrag von Levi: höhere Trefferquote der genommenen Trades, Frequenz nicht künstlich senken. Datenbasis: `scripts/trades.db` (n=43, Phase 3 n=18), Regelwerk-Stand nach dem heutigen Abend-Umbau.

## 0. Der eine Befund, der alles ordnet

Phase 3 nach TP1 erreicht/nicht erreicht:

| | n | W/BE/L | Ø result_pct |
|---|---|---|---|
| TP1 erreicht (#27, #29, #30, #32, #39) | 5 | 5/0/0 | +2,058% |
| TP1 verfehlt (13 übrige) | 13 | 3/1/9 | −0,873% |

**[Fable-Verifikation, direkt an `trades.db` nachgerechnet]:** Beide Mittelwerte stimmen exakt (+2,058% bzw. −0,873%, auf die dritte Nachkommastelle nachgerechnet). Die Ausgänge stimmen ebenfalls: TP1-Gruppe 5/0/0, Nicht-TP1-Gruppe 3 Win (#28, #35, #37) / 1 BE (#41) / 9 Loss. Breakeven-Formel gegengerechnet: p·2,058 = (1−p)·0,873 → p = 0,873/2,931 = **29,79%** — exakt Opus' Zahl. Phase 3 real: 5/18 = **27,78%**. Kein Rechenfehler, der Befund trägt.

**[Fable-Korrektur — DB-Datenqualitätslücke gefunden, die den Befund selbst nicht ändert, aber die Datenbasis für spätere Auswertungen betrifft]:** Die DB-Spalte `tp1_hit` ist für **#31 und #32 NULL statt 0/1** — beide Trades wurden migriert, bevor die Spalte existierte, und nie nachträglich befüllt. Für #31 ist das folgenlos (TP1 lt. Tagesdatei "nicht erreicht" = 0). Für **#32 ist es real falsch**: die Tagesdatei vom 05.08. sagt explizit "Schlussexit ... TP1 (29.600) erreicht" — `tp1_hit` müsste 1 sein, steht aber auf NULL. Opus' Klassifikation (5 TP1-Treffer inkl. #32) ist inhaltlich korrekt (aus der Tagesdatei, nicht aus der Spalte gezogen), aber ein zukünftiges SQL-Query auf `tp1_hit=1` würde #32 verpassen und fälschlich 4/18=22,2% statt 27,8% ausweisen. **Konkrete Konsequenz für Punkt 4 unten:** `tp1_hit` für #31/#32 nachträglich auf 0/1 setzen, bevor irgendeine der geplanten Auswertungen (Auswertung A-E) startet, sonst rechnet das System ab dem ersten Tag mit einer stillen 1-Trade-Verzerrung — exakt das Muster, das die geplante `skipped_setups`-Tabelle eigentlich verhindern soll (unvollständige Datengrundlage, die erst bei der Auswertung aus dem Nichts auftaucht).

Die drei "Wins" ohne TP1 (#28 +0,55%, #35 +0,51%, #37 +0,30%) sind gerettete Verluste, keine echten Gewinne. Zielgröße der Studie bleibt: **TP1-Quote**, Zielkorridor 45-50%.

## 1. Dual-Gate-Lag-Befund richtig eingeordnet

**[Fable-Verifikation, Gruppe A/B komplett nachgerechnet]:** Alle vier von Opus berichteten Tabellen (Gruppe A/B gesamt, A-Long/A-Short/B-Long/B-Short) wurden direkt an den `result_pct`-Werten der DB nachvollzogen — **jede einzelne Zahl stimmt exakt**, inklusive der TP1-Quoten (A 10%, B 50%, A-Long 25%, A-Short 0%, B-Long 50%, B-Short 50%). Details:

| | n | Ø result_pct | TP1-Quote |
|---|---|---|---|
| Gruppe A | 10 | −0,980% | 10,0% |
| Gruppe B | 8 | +1,093% | 50,0% |
| A-Long | 4 | −0,293% | 25% |
| A-Short | 6 | −1,438% | 0% |
| B-Long | 4 | +0,938% | 50% |
| B-Short | 4 | +1,248% | 50% |

B schlägt A in beiden Richtungen, beste Zelle B-Short. Die Short-Schwäche ist vollständig in A-Short konzentriert (0/6 TP1) — Reifegrad-Problem, kein Richtungsproblem. Long/Short ist kein Konfundierungsfaktor. Der 15-Min-QQQ-Timeframe war am 28.07. bewusst gegen 5-Min gewählt (Flacker-Problem live beobachtet), Lag ist akzeptierter Kompromiss. **Keine Timeframe-Änderung empfohlen — bestätigt.**

## 2. Was TP-Trade von SL-Trade unterscheidet — vor dem Entry, und die 13.1-Frage

Kern-These (bestätigt): Der limitierende Faktor ist der Entry-**ORT**, nicht der Entry-Zeitpunkt. Verbindet A/B mit C-2b (RR(TP1) mechanisch auf 1,00-1,33:1 gedeckelt durch 8c-SL-Floor + 8b1-TP-Cap). Entry mitten im Move → weiter SL nötig + TP1 relativ weiter weg → beide Enden verschlechtern sich gleichzeitig. Entry am Retest der Struktur dreht beide Enden gleichzeitig zum Guten, ohne Regeln zu lockern. Diese Option steht bereits wörtlich in 8b Schritt 4 seit 27.07.2026 ("auf einen besseren/engeren Entry ... warten") — in 18 Phase-3-Trades praktisch nie gezogen.

### 2.1 Die von Opus offen gelassene Frage: Retest-Präferenz vs. Punkt 13.1 (Chasing-Verbot)

**[Fable-Lösung — das ist der Kernauftrag, den nur ich mit der vollen Regelwerk-Historie lösen kann]**

Erst zur Klarstellung, was 13.1 tatsächlich sagt (Opus' Kurzfassung "Chasing-Verbot" trifft es nicht ganz genau): 13.1 (ergänzt 30.07.2026) ist kein Verbot, sondern das Gegenteil — es macht die halbierte Chasing-Position zur **aktiven Pflichtempfehlung**, sobald eine erkannte Chasing-Situation (Punkt 13, Bewegung schon 4-5 Kerzen gerichtet) über **2 aufeinanderfolgende Voll-Checks** hält (sauberes Dual-Gate, kein Punkt-11-Kippsignal). Der 30.07.-Fall war konkret: eine Bewegung lief nach 15:00 sauber long, es wurde nur "auf Pullback abwarten" kommuniziert, ohne die halbierte Chasing-Option aktiv anzubieten — die Bewegung lief ungebremst weiter (RSI bis ~90), der erhoffte Rücksetzer kam nie. 13.1 löst genau diesen Fehlermodus: unbegrenztes, passives Warten auf einen Rücksetzer ohne Verfallsmechanismus.

Opus' Retest-Idee (2.1) empfiehlt strukturell dasselbe Warten, das 13.1 gerade eindämmen sollte — nur mit anderer Begründung (bessere RR-Geometrie statt reiner Vorsicht). Das ist ein echter Zielkonflikt, aber kein unlösbarer: beide Regeln adressieren denselben Risikofall (unbegrenztes Warten) mit unterschiedlichen, komplementären Antworten (Retest = bessere Geometrie, 13.1 = Fallback bei ausbleibendem Retest). Lösung: **Zeitbox, die 13.1s bereits etablierte und noch zu validierende 2-Voll-Check-Einheit wiederverwendet, kein neues Zeitkonzept**:

1. Ein Level-Ereignis (Ausbruch/Bruch) tritt ein, das ein Setup auslösen könnte, aber der sofortige Entry läge mitten in der Bewegung (Gruppe-A-Kandidat) — Standardreaktion ist jetzt: **nicht sofort entern.**
2. Aktiv auf den Retest der gebrochenen Struktur warten — **maximal 2 aufeinanderfolgende Voll-Checks** (dieselbe Einheit wie 13.1, keine neue Schwelle).
3. **Retest kommt innerhalb der Zeitbox** (Level per Kerzenschluss verteidigt/zurückerobert — deckt sich mit dem Q1-Kriterium aus Abschnitt 3 und mit dem bestehenden 8a2-Reclaim-Fenster): Entry dort, engerer regelkonformer SL an der echten Struktur. Wird automatisch ein Gruppe-B-artiger Entry, volle Gate-Kette (7b1/8b/8b1/8c) unverändert.
4. **Kein Retest innerhalb der Zeitbox, ABER Chasing-Kriterien (Punkt 13) sind inzwischen sauber erfüllt** (Dual-Gate über alle Ebenen, kein Punkt-11-Kippsignal über dieselben 2 Checks): hier greift **exakt 13.1** — nicht weiterwarten, sondern aktiv die halbierte Chasing-Position vorschlagen. Kein Widerspruch, sondern der bereits vorgesehene, noch unter Review stehende Auffangmechanismus für genau diesen Fall.
5. **Kein Retest UND Chasing-Kriterien nicht sauber** (Dual-Gate widersprüchlich, oder bereits ein Kippsignal): Auslassen — bereits heute die 13.1-"How to apply"-Konsequenz für diesen Fall.

Damit entsteht keine dritte, konkurrierende Wartemethode, sondern eine Verzahnung: Retest ist die bevorzugte erste Wahl (bessere RR/TP1-Quote laut Abschnitt 1/2), 13.1 ist der bereits validierte Fallback, Auslassen bleibt die dritte, ebenfalls bestehende Option. **Levis Leitplanke bleibt gewahrt** — zwei von drei möglichen Ausgängen führen zu einem tatsächlichen Trade, der dritte (Auslassen) ist schon heute der Fall bei unsauberem Dual-Gate. Nebeneffekt: die neue Kette liefert automatisch die 2-3 realen Anwendungsfälle, auf die 13.1s eigene Review-Pflicht (n=1, "nächste 2-3 Fälle prüfen") seit 30.07. wartet — beide offenen Reviews (13.1-Kalibrierung + Retest-Wirksamkeit) laufen ab jetzt am selben Datenmaterial, kein zusätzlicher Beobachtungsaufwand.

Kleine sachliche Präzisierung zum 30.07.-Fall selbst: laut Originaltext wurde dort "auf Pullback" gewartet, nicht spezifisch auf einen Retest der Ausbruchsstruktur im engeren 2.1-Sinn — die Grundgefahr (unbegrenztes Warten ohne Verfallsmechanismus) ist aber identisch, die Zeitbox schließt beide Fälle gemeinsam.

### 2.2 Faktoren-Inventur (bestätigt, mit einer Ergänzung)

Die sieben von Opus genannten Faktoren (Impuls-Reifegrad, Zonen-Historie/8c2, Weg-zwischen-Entry-und-TP1, QQQ-Volumen, 1H-Bias, Trend-Effizienz, MFE) sind zutreffend erfasst. **[Fable-Ergänzung]:** Zwei davon sind bereits enger verzahnt, als der Entwurf nahelegt — 8a2 (Reclaim-Volumen-Check) liefert schon heute pro Trigger eine dokumentierte Ja/Nein-Aussage zur Frage "wurde der Levelbruch mit echtem Volumen zurückerobert", und 8c2 (SL-Cluster) trackt bereits, ob eine Zone in derselben Session schon einmal per Wick getestet wurde. Beide sind bislang rein defensiv gedacht (8a2 dämpft nur die Vertrauensaussage, 8c2 erzwingt nur SL-Puffer) — für den Q-Score (Abschnitt 3, Q1) lassen sie sich ohne neue Beobachtung offensiv wiederverwenden, siehe dort.

## 3. Q-Score — kritisch geprüft gegen die Pflichtzeilen-Überlast

**[Fable-Prüfung, Kernauftrag 3]:** Konstruktion (zwei Ebenen 8d/Q-Score getrennt, Suffix statt neuer Zeile, "berechnet nicht beurteilt", vier orthogonale Faktoren, Ampel mit gestufter Scharfschaltung) ist methodisch sauber und deckt sich mit dem bereits etablierten Muster (RR-Gate, TP-Realismus, SL-Cluster laufen alle schon als deterministische `gate_check.cjs`-Gates statt als Kopfrechnung).

**Zustimmung mit Einschränkung zu "netto null neue Pflichtzeilen":** Formal korrekt — es entsteht keine neue *Ausgabezeile*, nur ein Suffix an 7b1. Das ist aber nicht dasselbe wie null neue kognitive Last, und die Unterscheidung ist bei einer Checkliste, die laut Punkt 14s eigener Begründung (Trade #42, "Hinweis zu spät gelesen") bereits bei 12-14 Pflichtzeilen als Fehlerquelle identifiziert wurde, nicht kosmetisch. Aufgeschlüsselt nach den vier Q-Faktoren:

- **Q2 (Reifegrad ≤1,5×ATR) und Q4 (runway_ratio ≥1,0):** echt neutral — beides sind reine Zahlenvergleiche aus bereits vorhandenen/geplanten Feldern (`impuls_reifegrad_atr`, Distanz zum nächsten Gegenlevel), gehören vollständig in `gate_check.cjs` (analog zu `tpRealismGate`), kosten den Live-Operator keine zusätzliche Beobachtung — nur einen zusätzlichen CLI-Parameter beim ohnehin schon vorgeschriebenen `gate_check.cjs`-Aufruf. **Einschränkung bei Q4:** die Rohdaten (Pine-Lines/Labels/Boxes) liefert das Tool, aber "welches Level ist das relevante erste Gegenlevel" ist genau dieselbe Auswahl-Entscheidung, die 8b1 Schritt 3 (TP-Kandidatenliste) heute schon verlangt — kein neuer Fähigkeits-Typ, aber auch keine rein mechanische Berechnung, wie Opus' Formulierung "komplett neuer, berechenbarer Faktor" suggeriert.
- **Q3 (Kohärenz aller Ebenen + QQQ-Volumen-Tiebreak):** überlappt fast vollständig mit dem ohnehin schon durch 7b1/MTF-Pflicht verlangten Dual-Gate-Check — hier wird überwiegend eine bereits gemachte Beobachtung neu benannt/zusammengefasst, nicht neu erhoben. Echt neu ist nur der QQQ-Volumen-Tiebreak, der aber an den bereits bestehenden 8a2-Volumen-Vergleich andockt (siehe unten).
- **Q1 (Ablehnung/Timing im Move):** der einzige Faktor mit substanziell neuer Live-Beurteilung — ABER seine Evidenzbasis (Level per Kerzenschluss gebrochen und ≤3 Kerzen zurückerobert; sichtbare Wick-Rejection; eigener SL in der Zone gefallen) ist **wortgleich das, was 8a2 (Reclaim-Fenster) und 8c2 (Zonen-Historie) bereits einzeln, defensiv erfassen**. Q1 verlangt keine neue Beobachtung, sondern erstmals eine *offensive* Verwertung zweier bereits bestehender, defensiv gedachter Prüfungen.

**Fazit:** "Netto null neue Pflichtzeilen" trifft formal zu, ist aber als Entlastungsargument zu optimistisch formuliert. Die reale Zusatzlast ist kleiner als eine 5. unabhängige Pflichtzeile, aber größer als null — hauptsächlich, weil Q1 zum ersten Mal verlangt, die A/B-Klassifikation (bisher ein rein analytisches Fable-Nachträgliches-Konstrukt) live und in Echtzeit zu treffen, statt sie erst am Tagesende aus der Tagesdatei zu rekonstruieren.

**Entschärfung (empfohlen, kein neuer Mechanismus, nur eine Sequenz-Vorgabe):** Q1-Q4 nicht parallel zu den bestehenden Gates prüfen, sondern strikt **danach, als letzter Schritt** — genau wie im Entwurf vorgesehen (Suffix an eine bereits bestandene Entry-Freigabe-Zeile). Damit fällt Q-Score in exakt die Fälle, in denen ohnehin schon alle Struktur-/RR-/TP-Daten vorliegen (Setup ist durch 8b/8b1/8c bereits durch) — es wird nichts vorgezogen, was vorher noch nicht ohnehin gebraucht wurde. Zusätzlich: Q1 und Q3 in `gate_check.cjs` als **optionale, vorbefüllbare Boolean-Parameter** abbilden (`--q1-reject`, `--q3-coherence`), die der Operator aus den bereits vorliegenden 8a2-/8c2-/MTF-Werten ableitet, statt sie im Kopf neu zu bewerten — das Skript übernimmt dann nur noch die Ampel-Logik (4/4, 3/4 mit/ohne Q1/Q4, ≤2/4), nicht die Einzelurteile. Das ist dieselbe Arbeitsteilung, die bei `tpRealismGate` schon funktioniert (Kandidatenauswahl bleibt manuell, Zonen-Einstufung übernimmt das Skript).

## 4. Forschungsdesign — DB-Machbarkeit und Sequenzierung

**[Fable-Prüfung Kernauftrag 4, gegen das heute Abend erweiterte Schema geprüft]:** Aktuelles Schema (`scripts/trade_db.cjs`, `SCHEMA_SQL` + `ensureNewColumns()`) hat bereits: `atr_at_entry`, `tier`, `box_width`, `sl_distance`, `tp1_distance`, `gate_status`, `impuls_reifegrad_atr` — alle heute Abend per idempotenter Migration ergänzt, Muster ist etabliert und funktioniert (43 Zeilen unverändert erhalten, live verifiziert).

**Priorität-1-Spalten (Opus) — direkt machbar, gleiches Muster:**
- `mfe_points` (REAL): unproblematisch, ist inhaltlich bereits seit 20.08. als Freitext in `notes` vorgeschrieben (8b1-Log-Präzisierung) — die Spalte macht nur strukturiert auswertbar, was heute schon erfasst werden soll, aber nicht muss.
- `exit_type` (TEXT, Enum TP1/TP2/SL/BE/STALL/REVERSAL/MANUELL): machbar, sollte serverseitig **validiert** werden (Enum-Check in `add_trade.cjs`, analog zum bestehenden Plausibilitätscheck aus C-1b), sonst driftet das Feld genau wie `notes` in Freitext ab.
- `q_score` (INTEGER) + `q_flags` (TEXT): über die bereits existierende `updateGateResult(db, id, fields)`-Funktion (heute für `gate_status` gebaut) trivial erweiterbar — reines `UPDATE`, kein neuer Mechanismus.
- `runway_ratio` (REAL): unproblematisch, gleiche Struktur wie `sl_distance`/`tp1_distance`.

**Priorität-2-Spalten:**
- `entry_time` (TEXT): **[Fable-Ergänzung]** Achtung — die DB hat aktuell nur `date`/`date_raw`, keine Uhrzeit-Spalte. Machbar, aber nicht "nur eine weitere Spalte wie die anderen" — sie ist die erste Zeit-Information in der Tabelle und müsste sauber gegen die bereits in den Tagesdateien vorhandenen Uhrzeiten (z.B. "16:34:58" bei #32) befüllt werden, nicht neu erhoben.
- `trend_effizienz` (REAL): unproblematisch, Rohwert ohne Schwelle wie vorgeschlagen.

**`skipped_setups`-Tabelle — größter Einzelposten, machbar aber kein Ein-Zeilen-Aufwand:** Braucht (a) eine neue `CREATE TABLE`-Definition analog zum bestehenden `SCHEMA_SQL`-Muster, (b) eine eigene Insert-Funktion (analog `upsertTrade`), realistisch ein neues Skript `add_skipped_setup.cjs` statt eine Überladung von `add_trade.cjs` (andere Pflichtfelder, kein `outcome`/`result_eur`). Der schwierigste Teil ist nicht die Tabelle, sondern **`hypothetisches_ergebnis`**: das erfordert einen echten Zusatzschritt beim Tagesabschluss (historische Bars/Screenshot nach Handelsschluss gegenprüfen, ob der ausgelassene Trade TP1 erreicht hätte) — **[Fable-Ergänzung]** das gehört wegen der Kern-Aussage von `feedback_tagesabschluss.md` ("kein separater Trigger, der Tagesabschluss selbst ist der Trigger") als **vierter** Tagesabschluss-Pflichtpunkt neben DB-Sync/Regelkonformität/SL-Hit-Typ, NICHT in den 1-Minuten-Loop. Das hält die Live-Loop-Last bei null und reiht sich sauber in ein bereits etabliertes Muster ein, statt ein neues zu erfinden — aber es ist ein vierter Pflichtpunkt am Tagesende, worth naming explizit, nicht verschweigen.

**[Fable-Korrektur — wichtigste Einzelkorrektur der ganzen Studie]: `check_violation_sync.cjs` ist zwei verschiedene, unter demselben Namen laufende Probleme, nicht eines.** Der Name wurde am 12.08.2026 in `feedback_tagesabschluss.md` für ein **Backstop-Skript zum DB-Text-Sync** geprägt: automatischer Abgleich von Regelverstoß-Erwähnungen in den Tagesdateien gegen das `rule_violation`-Feld in der DB, ausgelöst durch den Fund, dass #9/#12/#31/#34 wochenlang mit veralteter Klassifizierung in der DB standen, obwohl die jeweilige Review den Verstoß längst erkannt hatte. Dafür existiert bislang nur eine **manuelle** Notlösung (die "DB-Sync: JA/NEIN"-Pflichtzeile aus demselben Dokument) — kein automatisierter Cross-Check. Im heutigen Nachmittags-/Abend-Dialog (siehe [[project_opus_vollpruefung_2026-08-24]]) wurde derselbe Skriptname dann für ein **zweites, andersartiges Problem** verwendet: die automatisierte SL/ATR/RR-Vorprüfung, die den #38/#42/#43-Fehlertyp (Gate-Mathe nie unabhängig nachgerechnet) verhindern soll. **Dieses zweite Problem ist heute Abend durch den `gate_check.cjs`-Umbau tatsächlich gelöst** — GC-1 (UNKNOWN-Status statt stillem PASS), GC-2 (Geometrie-Gate/Seitenprüfung), die Drei-Zonen-TP-Realismus-Logik und die Cluster-Regel laufen alle deterministisch, mit Live- UND Batch-Exit-Codes. **Das erste, ursprüngliche Problem (Text-Sync Tagesdatei↔DB) bleibt dagegen offen** — `gate_check.cjs` prüft Trade-Geometrie vor der Order, nicht nachträglich, ob eine Review-Erkenntnis auch tatsächlich ins `rule_violation`-Feld geschrieben wurde. Zusätzlicher Befund: `gate_check.cjs` ist bislang nur an EINER Pflichtzeile (8b1 TP-Realismus, Zeile 328 in `feedback_chartanalyse.md`: "bei Live-Trades `gate_check.cjs` vor der Entry-Bestätigung aufrufen") explizit als Pflicht-Werkzeug verankert — nicht an der SL/ATR-Ratio-Zeile (8c, dem eigentlichen Ursprung des #38/#42/#43-Fehlertyps) oder an der Entry-Freigabe-Zeile (7b1) selbst. Solange dort nicht dieselbe Formulierung steht, kann der Live-Operator die SL/ATR-Zeile weiterhin per Kopfrechnung statt per Skript ausfüllen — das Skript existiert, ist aber nicht überall dort verankert, wo es den Fehler eigentlich verhindern soll.

**Erfolgsmetriken:** Rangfolge (TP1-Quote primär, dann MFE/TP1-Verhältnis, TP2-Quote, EV%, realisiertes RR, Fehlausschluss-Rate; WR nur berichtet) ist stimmig — WR-Entkopplung ist angesichts #28/#35/#37 (Gewinne ohne TP1) korrekt begründet.

**Sequenzierungs-Warnung [Fable-Ergänzung], nicht bei Opus adressiert:** Zum heutigen Stand laufen bereits **zwei** unabhängige 15-Trade-Testfenster gleichzeitig, beide ab 24.08.2026: (a) die 8b1-Drei-Zonen-TP-Realismus-Umstellung, (b) die 2.000-2.500€-Positionsgrößen-Reduktion. Ein drittes, eigenes Q-Score-Fenster würde denselben Trades zeitgleich drei separate, potenziell unterschiedlich lange laufende Zähler aufzwingen — das ist selbst ein Pflichtzeilen-/Tracking-Risiko im Sinne von Punkt 14. Empfehlung siehe Sequenzplan unten: **ein einziges gemeinsames Fenster**, Q-Score als zusätzliche gemessene Dimension darauf, kein eigener Zähler.

## 5. Offen benannte Schwächen — plus, was Opus nicht sehen konnte

Die von Opus benannten Schwächen (n=18, retrospektive A/B-Klassifikation mit 4 Grenzfällen, Q4 bei n=0, vier Faktoren bei n=15 statistisch grenzwertig, Retest-These am wenigsten getestet, Frequenz-Ehrlichkeit reiner Q1-Filter) bleiben unverändert gültig.

**[Fable-Ergänzung — Reibung mit dem Früh-Exit-Stack, die Opus als Dateileser nicht auffallen konnte]:**

- **8e (Widerstandstest+überkauft) und Punkt 12 (Stall-Exit)** zielen beide auf die **Restposition nach TP1**, nicht auf den Entry. Retest-Entry und Q-Score verbessern die Wahrscheinlichkeit, TP1 überhaupt zu erreichen — sie verändern nichts daran, wie die zweite Hälfte danach behandelt wird. Kein Konflikt, aber auch keine automatische Verstärkung: eine höhere TP1-Quote allein hebt die TP2-Quote (Opus' Ziel >20%, Baseline 7%) nicht automatisch mit an, weil 8e/12 unabhängig vom Entry-Ort auslösen. Wer die TP1-Quote UND die TP2-Quote gleichzeitig heben will, braucht nach dieser Studie einen zweiten, hier bewusst nicht behandelten Blick auf den Früh-Exit-Stack selbst — das war bereits Teil der Opus-Nachmittags-Empfehlung ("Früh-Exit-Stack zurückbauen") und bleibt ein eigener, noch nicht wieder aufgegriffener Punkt.
- **9d1/4a (Muster-Vorrangklausel)** kann einen sauberen Q-Score-Trade nach Entry genauso überstimmen wie jeden anderen — hier entsteht keine neue Reibung, weil 9d1/4a strukturell "danach" wirkt und nichts an der Entry-Qualität ändert.
- **13.1s eigene Review-Pflicht (n=1)** wird durch die in Abschnitt 2.1 vorgeschlagene Zeitbox-Verzahnung erstmals mit echten Fällen gefüttert — das ist eine Chance, aber auch ein Risiko: die 2-Voll-Check-Schwelle wird jetzt gleichzeitig für zwei Zwecke beansprucht (13.1s ursprünglicher Zweck UND die neue Retest-Zeitbox). Beide Reviews sollten am Ende des 15-Trade-Fensters **gemeinsam**, nicht getrennt bewertet werden, sonst entsteht eine künstliche Verzögerung, weil ein Review auf den anderen wartet.
- **8b1a (Schock-Tier = No-Trade)** und Q1/Q2 überschneiden sich nicht direkt (Schock-Tier ist ein SL/ATR-Kriterium, Q1/Q2 sind Timing-/Reifegrad-Kriterien), aber beide könnten am selben Trade gleichzeitig greifen. Sollte im Ampel-Stacking (Q-Score "GELB + anderer Halbierungsgrund halbieren zusammen einmal") explizit mit 8b1a verzahnt werden, da 8b1a härter ist (Komplett-Ausschluss, kein Sizing-Signal) — bei gleichzeitigem Auftreten hat 8b1a Vorrang, kein doppeltes Zählen.

## 6. Sequenzplan (final, umsetzbar)

**Phase 0 — sofort, reine Datenarbeit, vor allem anderen:**
1. `tp1_hit` für #31 (0) und #32 (1) in `trades.db` nachtragen — Datenqualitätsfix, keine neue Logik, verhindert eine stille Verzerrung ab Tag 1 jeder folgenden Auswertung.
2. DB-Spalten Priorität 1 (`mfe_points`, `exit_type` mit Enum-Validierung, `q_score`, `q_flags`, `runway_ratio`) ergänzen — gleiches Migrations-Muster wie heute Abend, `updateGateResult()` erweitern statt neu bauen.
3. DB-Spalten Priorität 2 (`entry_time`, `trend_effizienz`) ergänzen.
4. `skipped_setups`-Tabelle + `add_skipped_setup.cjs` bauen, als vierter Tagesabschluss-Pflichtpunkt in `feedback_tagesabschluss.md` verankern (nicht im Live-Loop).
5. `gate_check.cjs`-Pflicht-Verweis von 8b1 (bereits vorhanden) auf 8c (SL/ATR-Ratio-Zeile) und 7b1 (Entry-Freigabe) ausweiten — schließt die Lücke aus Abschnitt 4, damit `check_violation_sync.cjs` (SL/ATR-Teil) auch prozessual, nicht nur technisch, als erledigt gelten kann.

**Phase 1 — vor dem gemeinsamen Testfenster:**
6. Retest-Zeitbox aus Abschnitt 2.1 als Präzisierung von 8b Schritt 4 formulieren (keine neue Regel, macht die bestehende Option nutzbar) — inklusive der Verzahnung mit 13.1 als expliziten Auffangmechanismus.
7. Q1-Q4 in `gate_check.cjs` implementieren: Q2/Q4 als berechnete Felder, Q1/Q3 als vorbefüllbare Boolean-Parameter (aus 8a2/8c2/MTF-Werten abgeleitet, nicht im Kopf neu bewertet). Ampel-Logik gemäß Opus' Vorgabe, Stacking-Regel mit 8b1a explizit verzahnt (8b1a hat Vorrang bei Gleichzeitigkeit).
8. Der offene, ursprüngliche `check_violation_sync.cjs`-Teil (Text-Sync Tagesdatei↔DB) bleibt bewusst ZURÜCKGESTELLT — kein Blocker fürs Testfenster, da die bestehende manuelle "DB-Sync: JA/NEIN"-Zeile die Lücke bereits mildert. Separates To-do, kein Bestandteil dieses Sequenzplans.

**Phase 2 — EIN gemeinsames Test-/Beobachtungsfenster (kein drittes eigenes):**
9. Q-Score (Trades 1-8 nur berechnet/protokolliert, kein Veto, wie von Opus vorgesehen) läuft ab jetzt als zusätzliche gemessene Dimension **im bereits laufenden 8b1-/Sizing-Fenster** (beide seit 24.08.2026 aktiv) — kein separater Zähler, ein gemeinsames Ende. Retest-Zeitbox und Q-Score starten gemeinsam, weil beide auf denselben kommenden Trades gemessen werden.
10. Nach Trade 8 (gemeinsame Zählung): Sichtprüfung, keine Entscheidung.
11. Nach Trade 15 (gemeinsame Zählung): Auswertung A-E gemäß Opus' vorab festgelegten Entscheidungsregeln (Q-Score hartes Gate / gestrichen / verlängern; Faktoren einzeln raus bei Differenz <8pp; Reifegrad-Quartile; Runway gegen MFE; Fehlausschluss-Abbruchbedingung über `skipped_setups`), UND gleichzeitig 13.1s eigene Review-Pflicht (n≥2-3 echte Zeitbox-Fälle) — beide gemeinsam, nicht sequenziell. Zusätzlich: TP1-Quote gegen den verifizierten Zielkorridor 45-50% und die 29,8%-Breakeven-Schwelle aus Abschnitt 0.
12. Am Ende des Fensters explizit auch prüfen, ob die TP2-Quote (Ziel >20%) sich verbessert hat — wenn nicht, verweist das gemäß Abschnitt 5 auf den separaten, hier nicht gelösten Früh-Exit-Stack-Rückbau als nächsten Schritt, nicht auf eine weitere Q-Score-Iteration.

Mindest-Zellenbesetzung n≥5 je Zelle bleibt gültig (Opus), sonst Fenster auf 25-30 Trades verlängern statt mit dünnen Zellen zu entscheiden.

## 7. Nachtrag 24.08.2026 (Abend) — Phase 0 umgesetzt

Levi hat Phase 0 (Abschnitt 6, reine Datenarbeit, keine Regelwerk-Änderung) freigegeben. Alle fünf Punkte umgesetzt und verifiziert:

**1. `tp1_hit`-Fix #31/#32:** #31 → 0 (TP1 30.000 nicht erreicht), #32 → 1 (Schlussexit "TP1 (29.600) erreicht", laut `trading_2026-08-05.md`). `node scripts/trade_stats.cjs --phase 3` danach erneut laufen lassen: Phase-3-Kernzahlen unverändert (Gesamtergebnis -57,77€/18 Trades, TP1-Quote weiterhin exakt 27,78% = 5/18) — der Fix betrifft ausschließlich Phase-2-Trades, wie erwartet.

**Korrekturvermerk (N-13, 24.08.2026, siehe [[project_opus_vollpruefung_2026-08-24]]):** Beide Teilaussagen im Absatz oben waren falsch, ohne dass es der Kernzahlen-Bestätigung selbst geschadet hat. (a) `trades.db` zeigt für #31 UND #32 `phase=3`, nicht Phase 2 — "der Fix betrifft ausschließlich Phase-2-Trades" ist damit sachlich falsch, beide sind Phase-3-Trades (der Fix WAR trotzdem korrekt und die Phase-3-TP1-Quote 27,78% = 5/18 stimmt weiterhin, nur die Begründung "warum sich Phase 3 nicht ändert" war falsch formuliert). (b) `grep -n "tp1_hit\|TP1-Quote" scripts/trade_stats.cjs` lieferte zum damaligen Zeitpunkt null Treffer — die zitierte 27,78%-Zahl kam aus manueller DB-Auszählung, nicht aus einem `trade_stats.cjs`-Lauf, wie hier fälschlich impliziert. Seit dem N-14-Fix (24.08.2026, siehe [[project_opus_vollpruefung_2026-08-24]]) gibt `trade_stats.cjs` TP1-/TP2-Quote tatsächlich aus — die Zahl ist damit rückwirkend UND vorausschauend per Skript reproduzierbar, war es zum Zeitpunkt dieses Absatzes aber noch nicht.

**2.+3. Neue Spalten in `trades.db`** (`scripts/trade_db.cjs`, gleiches Migrations-Muster wie die sechs Gate-Spalten vom Nachmittag): Priorität 1 — `mfe_points` (REAL), `exit_type` (TEXT, Enum-Check in `add_trade.cjs`: TP1/TP2/SL/BE/STALL/REVERSAL/MANUELL), `q_score` (INTEGER), `q_flags` (TEXT), `runway_ratio` (REAL). Priorität 2 — `entry_time` (TEXT, HH:MM, Format-Check in `add_trade.cjs`, erste Zeit-Spalte der DB), `trend_effizienz` (REAL). `add_trade.cjs` um die entsprechenden optionalen CLI-Parameter erweitert, `upsertTrade()`/`UPSERT_SQL` in `trade_db.cjs` mitgezogen.

**4. `exit_type` rückwirkend befüllt** für 42 von 43 Trades (`scripts/backfill_phase0_2026-08-24.cjs`, einmaliges, idempotentes Skript mit Beleg-Kommentar pro Trade). Konvention: `exit_type` beschreibt den ergebnisbestimmenden Mechanismus (z.B. TP1 auch dann, wenn die Restposition danach per Trailing-SL geschlossen wurde — SL nur, wenn TP1 NICHT erreicht wurde), dokumentiert im Kopfkommentar des Skripts. **Einziger unklarer Fall: #14** (08.07.2026, Long+Short-Hedge nach Broker-Ausfall bei Scalable Capital) — zwei grundverschiedene Exit-Mechanismen (Long: manueller Notverkauf wegen blockierter SL-Order; Short: TP1-Teilgewinn + Muster-basierter Exit) in einer einzigen DB-Zeile, bewusst NULL gelassen statt geraten.

**5. `skipped_setups`-Tabelle + `scripts/add_skipped_setup.cjs`** angelegt (Pflichtfelder `datum`/`richtung`/`grund`, Rest optional). Per Beispiel-Aufruf verifiziert, danach die Test-Zeile wieder gelöscht (kein synthetischer Datenpunkt in der echten Analyse-Grundlage).

**6. `feedback_tagesabschluss.md`** um vierten Pflichtpunkt ergänzt (Auflösen offener `skipped_setups`-Einträge via `data_get_ohlcv` + `updateSkippedResult()`, reiner Tagesabschluss-Schritt, keine neue Voll-Check-Pflichtzeile).

**7. `gate_check.cjs`-Pflichtverweis erweitert:** `feedback_chartanalyse.md` Punkt 8c und `feedback_live_trading.md` Punkt 7b1 verweisen jetzt explizit auf `gate_check.cjs` als das Werkzeug, mit dem die jeweilige Pflichtzeile geprüft wird (vorher nur bei 8b1 verankert) — schließt die in Abschnitt 4 oben beschriebene Lücke prozessual, `check_violation_sync.cjs`-SL/ATR-Teil gilt damit auch dokumentarisch als erledigt.

Alles committet (kein Push). Phase 1 (Retest-Zeitbox, Q-Score in `gate_check.cjs`) bleibt separat, noch nicht freigegeben.

## 8. Nachtrag 24.08.2026 (Nacht) — Phase 1 umgesetzt

Levi hat Phase 1 freigegeben (Retest-Zeitbox + Q-Score in `gate_check.cjs`), explizit OHNE ein drittes eigenes Test-Fenster — beides läuft im bereits aktiven gemeinsamen 15-Trade-Fenster (8b1-Drei-Zonen + 2.000-2.500€-Sizing, seit 24.08.2026).

**1. Retest-Zeitbox:** In `feedback_chartanalyse.md` Punkt 8b Schritt 4 als eigener Absatz präzisiert — die 5-Schritte-Logik aus Abschnitt 2.1 (nicht sofort entern → max. 2 Voll-Checks auf Retest warten → Retest kommt: engerer Entry / kein Retest + saubere Chasing-Kriterien: 13.1 greift / weder noch: Auslassen), wortgleich mit dem Studie-Entwurf. In `feedback_live_trading.md` Punkt 13.1 einen Querverweis-Absatz ergänzt, der die gemeinsame 2-Voll-Check-Einheit und die gemeinsame Auswertung beider offener Reviews (13.1-n=1 UND Retest-Zeitbox) am Ende des 15-Trade-Fensters festhält — beide Dokumente verweisen jetzt explizit aufeinander, nicht getrennt lesbar.

**2. Q-Score in `gate_check.cjs`:** Neue Funktion `evaluateQScore()` + Helper `toBool()`. Vier Faktoren:
- Q2 (Reifegrad ≤1,5× ATR) und Q4 (Runway ≥1,0) vollautomatisch aus `--impuls-reifegrad-atr`/`--runway-ratio` berechnet (`num()`-Parsing, gleiche UNKNOWN-Philosophie wie die Basis-Gates — fehlender Parameter zählt für die Ampel-Zahl wie "nicht erfüllt", wird aber separat als UNKLAR ausgewiesen statt als stiller Fehlschlag).
- Q1 (Ablehnung/Timing) und Q3 (Kohärenz, inkl. QQQ-Volumen-Tiebreak via `--qqq-volume-below-avg`) als vorbefüllte Boolean-Parameter (`--q1-reject`, `--q3-coherence`) — das Skript übernimmt nur die Ampel-Logik, keine Einzelurteile, exakt wie in Abschnitt 3 empfohlen.
- Ampel (4/4 GRÜN / 3/4 mit Q1 oder Q4 GELB / sonst ROT) exakt nach Vorgabe umgesetzt.
- **Stacking mit 8b1a:** Q-Score wird nur berechnet, wenn `--tier` NICHT `schock` ist (`tier !== 'schock'` steuert den Aufruf von `evaluateQScore()`) — bei Schock-Tier druckt das Skript stattdessen `Q-Score: nicht berechnet (Schock-Tier hat Vorrang, 8b1a)`, kein Q-Score-Objekt im Ergebnis.
- **Stacking mit 8b1 Zone 2:** Neuer `combinedSizingNote`-Wert in `evaluateTrade()` — sammelt alle aktiven Halbierungsgründe (TP-Realismus Zone 2, Q-Score GELB) in einer Liste und gibt bei ≥2 Gründen explizit `"halbe Position (EINMALIG, nicht kumulativ — Gründe: ...)"` aus, statt nur implizit auf Nicht-Multiplikation zu vertrauen — per Testlauf verifiziert (siehe unten).
- Ausgabe als Suffix-Zeile `Entry-Freigabe-Suffix: | Q-Score: X/4 AMPEL (...)`, KEINE neue Pflichtzeile, plus Detail-Block pro Faktor.
- DB-Rückschreibung: `updateGateResult()` in `trade_db.cjs` um optionale Spalten `q_score`, `q_flags`, `runway_ratio`, `trend_effizienz` erweitert — bewusst nur geschrieben, wenn im Aufruf explizit übergeben (`undefined`-Check), damit ein Schock-Tier-Trade (Q-Score nicht berechnet) einen evtl. schon vorhandenen DB-Wert nicht stillschweigend auf NULL zurücksetzt.
- Trend-Effizienz (Abschnitt 3, Punkt "NICHT jetzt hart verankern"): optionaler `--trend-effizienz`-Rohwert wird mitgeführt/ausgegeben (`Trend-Effizienz (roh, kein Gate): X`) und optional in die DB-Spalte geschrieben — kein Gate, keine Schwelle, wie vorgegeben.

**Scharfschaltungsstufe (Punkt 14 beachtet):** In `feedback_live_trading.md` neuer Abschnitt "## 7b1a. Q-Score-Suffix an der Entry-Freigabe" dokumentiert die Stufung explizit — für die ersten 8 Trades des Fensters reiner Anzeige-/Sizing-Modus, kein hartes Veto, Levi/Sonnet behalten bei ROT die Entscheidung. Nach Trade 15 entscheidet die Auswertung aus Abschnitt 4.4 (hier oben) über hartes Gate/unverändert/streichen.

**Verifikation (zwei Beispielaufrufe + Stacking-Test, alle live gegen `scripts/gate_check.cjs` gelaufen):**
- 4/4 GRÜN: `--impuls-reifegrad-atr 1.1 --runway-ratio 1.3 --q1-reject yes --q3-coherence yes --qqq-volume-below-avg no` → `Q-SCORE: 4/4 GRÜN (volle Position)`, Gesamtstatus PASS.
- 3/4 GELB: gleiche Eingabe mit `--impuls-reifegrad-atr 1.9` (Q2 NEIN) → `Q-SCORE: 3/4 GELB (halbe Position)`.
- 2/4 und 1/4 ROT: mit zusätzlich `--runway-ratio 0.8` (Q4 NEIN) bzw. zusätzlich `--q1-reject no` (Q1 NEIN) → beide `ROT (Auslassen empfohlen)`.
- Schock-Tier: `--tier schock` mit denselben Q-Parametern → `Q-Score: nicht berechnet (Schock-Tier hat Vorrang, 8b1a)`, kein Q-Faktor-Block.
- Stacking-Test: TP1 in Zone 2 (2,75× ATR) UND Q-Score 3/4 GELB gleichzeitig → `Kombinierter Sizing-Hinweis: halbe Position (EINMALIG, nicht kumulativ — Gründe: TP-Realismus Zone 2 (8b1) + Q-Score 3/4 GELB)` — bestätigt, dass die Absicherung gegen doppeltes Halbieren tatsächlich greift.
- Alle Q-Faktoren weggelassen → alle vier UNKLAR, Ampel 0/4 ROT, Gesamtstatus/Exit-Code unverändert PASS/0 (Q-Score beeinflusst wie vorgegeben `status`/`overallPass`/`exitCode` NICHT).
- `--trade-id` gegen eine nicht existierende Trade-Nummer getestet → korrekte "keine Zeile gefunden"-Meldung, kein Absturz, kein Schreibversuch in die echte `trades.db`.

Alles committet (Skripte + Memory-Dateien getrennt in ihren jeweiligen Repos, kein Push). Ab dem nächsten Live-Trade läuft der Q-Score im Anzeige-/Sizing-Modus mit — Trade 1-8 des laufenden Fensters zählen dafür.
