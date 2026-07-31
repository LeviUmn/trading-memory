---
name: project-risikomanagement
description: "Festgelegtes Risikomanagement-System — 15.000€ Kapital, Ziel ~3.000€/Monat (20%)"
metadata: 
  node_type: memory
  type: project
  originSessionId: 607aa8f6-9958-4c1c-9c75-4afabcffb717
  modified: 2026-07-31T15:59:26.052Z
---

Risikomanagement-System am 22.06.2026 gemeinsam festgelegt.

**Why:** User will strukturiert von 250€/Tag-Wunschdenken zu einem realistischen, durchgerechneten System kommen das auf echten Zahlen vom ersten Trade (12.06.) basiert.

**How to apply:** Dieses System bei jeder Positionsgrößen-Entscheidung anwenden.

### Kapital & Ziele
- **Gesamtkapital:** 15.000€
- **Zielrendite:** ~15-20% / Monat (Ziel: ~3.000€, Bestfall ~3.900€)
- **Realistische Bandbreite:** 2.100€ (schwacher Monat) bis 3.900€ (guter Monat)

### Risiko-Regeln
**Achtung (klargestellt 04.07.2026, Fable-5-Review):** Die Tabelle unten zeigt das **Phase-4-Zielsystem** (volle Kapitalbasis). Aktuell (Phase 1) gilt 1% = 150€, siehe Skalierungs-Fahrplan weiter unten — nicht die 2%-Zeile. Zusätzlich real gemessen (12 Trades, Stand 02.07.): Das tatsächliche Risiko pro Trade lag durchgängig bei ~0,07-0,17% des Kapitals (10-25€), weil das Positions-Limit (500-1.500€) in Phase 1 immer vor dem 1%-Risiko-Limit (150€) bindet. Phase 1 testet damit bisher nur Prozessdisziplin, nicht das Risikosystem selbst — das Risiko-Limit wird erst in höheren Phasen bindend und ist bislang ungetestet.

| Regel | Wert |
|---|---|
| Risiko pro Trade | 2% = 300€ max. Verlust (Phase-4-Zielwert, siehe Hinweis oben) |
| Max. Positionsgröße | ~40% des Kapitals = 5.000-6.500€ |
| Hebel | Situationsabhängig 5-10x, siehe VIX-Staffelung unten |
| Min. Risk-Reward | TP1 ≥1:1 (hart, Auswahlfilter), TP2 offen ohne RR-Mindestpflicht (kein Deckel) — Teilverkauf-Standard seit 04.07.2026, RR-Entkopplung von TP2 seit 27.07.2026 (Fable-Review), siehe [[feedback_chartanalyse]] Punkt 8b |
| Max. Tagesverlust | 4% = 600€ → Trading-Stopp für den Tag |
| Max. gleichzeitige Positionen | 1-2 |
| Trades pro Tag | **Bis zu 3 Trades regulär erlaubt** (geändert 23.06.2026, ersetzt frühere "1 Haupttrade"-Regel). 4. Trade nur nach vorheriger Prüfung + wirklich starkem technischem Signal (Ausnahme, nicht Standard). 5. Trade an einem Tag generell nicht erlaubt. |

### Erwartungswert-Rechnung (Basis für Zielerreichung)
Bei 50% Win-Rate und 1:2 RR: Erwartungswert ≈ 150€/Trade × 20 Trades/Monat ≈ 3.000€
Bei 55% Win-Rate: ≈ 195€/Trade × 20 ≈ 3.900€
Bei 45% Win-Rate (schwacher Monat): ≈ 105€/Trade × 20 ≈ 2.100€

### Reinvest-Entscheidung
Gewinne werden reinvestiert (Stand 22.06.2026) — Kapital wächst, Positionsgrößen skalieren mit.

### Langfristige Philosophie (festgelegt 22.06.2026)
Ziel ist NICHT dauerhaft aggressive Monatsrenditen (15-20%) zu fahren, sondern das Kapital so zu skalieren, dass die benötigte Rendite für das gleiche Euro-Ziel (~3.000€/Monat) sinkt und damit das Risiko pro Trade sinkt:
- 15.000€ → 20% nötig (aggressiv)
- 30.000€ → 10% nötig
- 50.000€ → 6% nötig
- 100.000€ → 3% nötig (konservativ, nachhaltig)

**Why:** User möchte langfristig Lebenshaltungskosten über Trading finanzieren (evtl. nach Schweiz-Umzug). Wichtig: das ist NICHT nach einem einzigen Trade (12.06.) realistisch einzuschätzen — erst nach 6-12 Monaten echter Trade-Historie + Backtesting kann seriös beurteilt werden ob die Strategie als Haupteinkommen trägt. Risikomanagement-Disziplin (2%/Trade, 1:2 RR) bleibt immer gleich, nur Kapitalbasis wächst.

**How to apply:** Nicht versuchen mit kleinem Kapital über höheren Hebel ans Ziel zu kommen — stattdessen Kapital organisch wachsen lassen (Reinvest + ggf. Kapitalspritzen) bei gleichbleibender Risikodisziplin.

**Sinn der 3 Trainings-Phasen (bestätigt 01.07.2026):** User-Zitat: "Deswegen haben wir 3 Trainings-Phasen, damit wir, wenn alles abgeschlossen ist, mit großen € Positionen traden können (>5-stellig)." Die Phasen 1-3 sind explizit kein Selbstzweck, sondern der Nachweis, dass System + Disziplin (Doppelkriterien, Chop-Erkennung, Regeldisziplin — siehe [[feedback_regeldisziplin]]) auch bei kleinen Positionen zuverlässig funktionieren, BEVOR mit deutlich größeren Positionen (mehr als 5-stellig, also >10.000€ pro Position) gehandelt wird. Verluste in dieser Trainingszeit (wie 01.07.2026) sind Teil des Zwecks, nicht ein Rückschlag vom eigentlichen Ziel.

**Bild "3. Lehrjahr einer Ausbildung" (ergänzt 28.07.2026, User-Formulierung nach Trade #26):** User beschreibt Phase 3 explizit als erweiterten Lernmodus, nicht als Endstufe — Analogie zum letzten Ausbildungsjahr, in dem man schon viel kann, aber noch nicht "ausgelernt" ist. Anlass: Trade #26 verlor trotz vollständig regelkonformer Ausführung (V-Reversal direkt nach Entry), gleichzeitig wurden am selben Tag zwei echte Systemfehler live gefunden und behoben (Vorzeichen-Bug in size.cjs bei Short-SL-Preisen, Klarstellung welche Kalender-Events wirklich eine Entry-Sperrfrist rechtfertigen). User wertet das explizit positiv: genau diese Art Erfahrung — Regeln 100% befolgen, dabei trotzdem verlieren, daraus lernen, kein FOMO/Chasing trotz Rückschlag — ist der eigentliche Zweck der Phase, nicht ein Umweg davon.

**How to apply:** Verluste trotz korrekter Regelanwendung nicht als Rückschlag framen, sondern als erwarteten, wertvollen Teil des "3. Lehrjahrs" — der Fokus bleibt auf Prozessqualität (wurden die Regeln eingehalten?), nicht auf das einzelne Trade-Ergebnis. Live gefundene Systemfehler (wie der size.cjs-Bug) sind ebenfalls Teil des Lernfortschritts, nicht Anlass zur Sorge über die Systemreife.

### Skalierungs-Fahrplan (festgelegt 22.06.2026) — AKTUELLER STATUS: Phase 3 gestartet (24.07.2026, Fable-GO mit Bedingungen), gestaffelter Einstieg für Trades #26-#30
Statt sofort mit vollem 15.000€-System zu starten, schrittweiser Aufbau anhand echter Trade-Daten.

| Phase | Trades | Positionsgröße | Risiko/Trade | Ziel |
|---|---|---|---|---|
| 1 — Bewährung | 1-15 (verlängert von ursprünglich 1-10) | ~500-1.500€ | 1% vom Gesamtkapital (150€) | Win-Rate & Erwartungswert ermitteln |
| 2 — Aufbau | 16-25 | ~2.000-3.000€ | 1,5% (225€) | Konstanz über mehr Trades beweisen |
| 3 — Skalierung | 26-35 | ~4.000-5.000€ | 2% (300€) | Näher ans Vollsystem |
| 4 — Datenerweiterung | 36-40 (5 Trades) | ~10.000€ pro Trade | 2% vom gewachsenen Kapital | UnusualWhales-Integration (konditional) unter echten Live-Bedingungen testen, bevor der Kapital-Sprung auf 50.000€ kommt |

**Phase 4 neu definiert (ergänzt 31.07.2026, User-Entscheidung; Trade-Anzahl korrigiert 31.07.2026 nach User-Korrektur — Phase 4 hat wie die anderen Phasen eine feste Trade-Range, 5 Trades):** Ersetzt die alte "Vollsystem ab Trade 36"-Zeile. Phase 4 startet erst NACH Phase-3-Abschluss (#35) UND einem großen, mehrfach hinterfragten Gesamt-Review über alle bisherigen Phasen (siehe [[project_vision]] für den bereits dokumentierten Review-Fahrplan vor UnusualWhales). Zwei Bedingungen für Phase 4:
1. **UnusualWhales erfolgreich angebunden** (konditional — siehe [[project_vision]] Priorisierung GEX/Max Pain zuerst; falls die Integration technisch/inhaltlich scheitert, wird Phase 4 ohne UW mit reinem 10k€-Kapitaltest fortgesetzt, kein Blocker für den Kapital-Fortschritt)
2. **10.000€-Positionsgröße** als eigener Bewährungsschritt, bevor der große Sprung auf 50.000€ Realkapital kommt

**Danach: Übergang ins normale Trading (kein Phasensystem mehr), 50.000€ Kapital.** Sobald Phase 4 erfolgreich abgeschlossen ist, endet die Phasen-Logik komplett — kein "Phase 5", sondern der eigentliche Übergang zum regulären Trading mit voller Motivation und vollem Kapital. **Harte Vorbedingung für diesen Übergang:** Vor dem Wechsel auf 50.000€ muss die Tracking-Infrastruktur erweitert werden — bisher gibt es nur Tagesabschlüsse ([[feedback_tagesabschluss]]), für die 50k-Stufe braucht es zusätzlich **Wochenabschlüsse und Monatsabschlüsse**, damit die Performance auf dieser Kapitalstufe ehrlich und greifbar trackbar ist (Win-Rate/Expectancy/Drawdown nicht nur pro Tag, sondern aggregiert über Woche/Monat, analog zum bestehenden Phasen-Review-Muster). Diese Wochen-/Monats-Abschluss-Struktur muss VOR dem 50k-Start stehen, nicht erst währenddessen nachgebaut werden.

**Why:** User-Zitat 31.07.2026: "Weil dann müssen wir alles ehrlich tracken, damit wir eine greifbare Performance hinbekommen." Bei 50.000€ Kapital sind die Beträge pro Trade groß genug, dass ungenaues/lückenhaftes Tracking echte Konsequenzen hätte — die bisherige Tagesabschluss-Granularität reicht für diese Stufe nicht mehr aus.

**Korrektur 21.07.2026 (User-Klarstellung):** Jede Phase war im Ursprungsplan (22.06.2026) einheitlich auf **10 Trades** ausgelegt (Phase 1 ursprünglich 1-10, Phase 2 11-20, Phase 3 21-30, Phase 4 ab 31). Die Tabelle hatte Phase 2/3 zwischenzeitlich fälschlich mit 15 bzw. 25 Trades Breite geführt (16-30 / 31-55) — vermutlich eine unbemerkte Übernahme der Phase-1-Verlängerung auf die Folge-Phasen, ohne dass dafür je eine bewusste Entscheidung getroffen wurde (anders als bei Phase 1, wo die Verlängerung nach Trade #10 explizit begründet und entschieden wurde, siehe unten). Jetzt korrigiert: Phase 1 bleibt bei ihrer bewusst verlängerten Breite (15 Trades, 1-15), Phase 2 und 3 sind wieder auf die ursprüngliche 10-Trades-Breite zurückgesetzt, nur um die 5 Trades der Phase-1-Verlängerung nach hinten verschoben (16-25, 26-35). Falls sich nach dem Phase-2-Abschluss-Review zeigt, dass auch hier eine Verlängerung nötig ist (analog Phase 1 nach der Verlustserie um Trade #10), wird das genauso gemeinsam und bewusst entschieden, nicht automatisch mitgezogen.

**Bestätigt 04.07.2026:** Nächste Session Montag, 06.07.2026, mit Trade #13 (Teilverkauf-Standard TP1/TP2 erstmals live, siehe [[feedback_chartanalyse]] Punkt 8b). Nach Trade #15 erneute Tiefenanalyse (Win-Rate, realisiertes RR, Expectancy) vor dem Übergang in Phase 2 — kein automatisches Hochstufen, siehe Phasenübergang-Review-Regel unten.

**Phase 2 formal abgeschlossen (23.07.2026, nach Trade #25):** Alle 10 Trades der Phase 2 (#16-25) komplett — siehe [[trades/trade_log]] für die vollständige Tabelle. Endstand: 7 Win / 0 BE / 3 Loss in diesem Fenster, Win-Rate 70% (7/10), Ø-Rendite 0,61%/Trade (korrigiert 24.07., siehe [[project_performance_ziele]]) — trifft das Win-Rate-Ziel exakt, verfehlt das Rendite-Ziel (1-1,5%) weiterhin deutlich.

**Phase-3-Übergang-Review abgeschlossen (24.07.2026, Fable — volles Ergebnis siehe [[project_phase2_abschluss_review_2026-07-24]]):** GO für Phase 3, aber mit Bedingungen statt vollem Sprung auf die maximale Bandbreite. **Gestaffelter Phase-3-Start:** Erste 3-5 Trades (#26-#30) am UNTEREN Ende der Phase-3-Bandbreite (~4.000€, nicht sofort 5.000€/volles 2%-Risiko) — erst nach diesem Bewährungsfenster (und nur wenn kein schwerer Regelbruch vom #21-Typ auftritt) auf volle Phase-3-Größe hochfahren. Weitere Bedingungen: RR-Minima als echte Hard-Gates (kein "offen deklariert = okay" mehr wie bei Trade #23), symmetrische Regelbruch-Konsequenz auch bei Gewinn-Trades, User-Eigenentries brauchen sofortiges Loop-Retro-Check, Phase-3-RR separat von der Lifetime-Zahl tracken.

**Phase-1-Abschluss erreicht (08.07.2026):** Trade #15 abgeschlossen, damit sind alle 15 Trades der (verlängerten) Phase 1 komplett — siehe [[trades/trade_log]] für die vollständige Tabelle. **Großes Phasen-Review geplant für morgen Mittag, 09.07.2026**, danach Entscheidung über den Übergang in Phase 2.

**Fable-5-Review (09.07.2026) vor dem Phase-2-Übergang — abgearbeitet:** Unabhängiges Review deckte Rechenfehler in der Kopfzeilen-Statistik (9/1/5 statt korrekt 8/2/6 vor BE-Neuklassifizierung), eine Diskrepanz bei Trade #13 (42,96€ vs. 46,25€) und fehlendes Broker-Blockade-Playbook auf. Alle behoben: [[feedback_be_definition]], [[feedback_broker_wert_prioritaet]], [[feedback_broker_blockade_playbook]] (Fables einzige als "hart blockierend" eingestufte Auflage — erfüllt), [[feedback_regeldisziplin]] (Regelbruch-Tracking jetzt auch bei Gewinn-Trades, siehe Trade #13/#15 im Log). Korrigierter Endstand Phase 1: 8 Win / 2 BE / 5 Loss, Win-Rate 61,5% (BE ausgeklammert), Gesamtgewinn +24,62€, realisiertes RR ≈0,93:1.

**Phase 2 formal gestartet (09.07.2026):** Phase 1 offiziell abgeschlossen (15/15 Trades, korrigierter Endstand siehe oben). Ab Trade #16 gilt Phase 2: Positionsgröße 2.000-3.000€, Risiko 1,5% (225€), alle unten stehenden Phase-2-Regeln aktiv.

**Trade #16 abgeschlossen (09.07.2026):** Erster Phase-2-Trade, Long NAS100, +15,29€ (+1,56%), siehe [[trades/trading_2026-07-09]] für vollen Ablauf. Positionsgröße 977,40€ (statt vollem 2.000-2.500€-Rahmen) war **bewusstes 50%-Chop-Sizing** wegen choppy Session-Start — User bestätigte das explizit im Debriefing als richtige Regelanwendung (siehe Chop-Erkennung-Regel oben), kein Fehler. User-Gesamtfazit: "super Trading Tag", alle Regeln befolgt bis auf zwei Punkte: MTF-Voll-Check-Lücke nach TP1 und 1-Min-Loop-Drift (beide in [[feedback_live_trading]] verschärft).

**Phase-2-Start-Entscheidung (09.07.2026):** Fables Vorschlag einer Positions-Zwischenstufe (erste 5 Trades nur 2.000€, volle Bandbreite erst nach 3 exakten TP1-Treffern) wurde vom User abgelehnt — Phase 2 startet direkt mit der vollen vereinbarten Bandbreite 2.000-3.000€ (siehe Skalierungs-Fahrplan oben), kein künstlicher Unterband-Zwischenschritt. Stattdessen wurde klargestellt: Die Qualitätsbewertung eines Trades ("sauber ausgeführt?") hängt NICHT an exakten TP1/TP2-Punktzielen. Ein pragmatischer Exit näher am Ziel (analog Trade #13: 29.210 statt exakt 29.214) zählt weiterhin als valider, bewertbarer Trade, wenn sich die Gelegenheit ergibt — kein unnötiges Warten auf den exakten Punkt. Zusätzlicher Rückstufungs-Trigger für Phase 2 (ergänzt zum bestehenden rollierenden 4-von-5-Schutz): **3 Verluste in Folge in Phase 2 → sofortige Rückstufung auf Phase-1-Positionsgröße**, Wiederaufstieg erst nach erneutem Review (analog Cooldown-Auflösung).

User-Einschätzung des Trends (08.07.2026, vor dem Review): Die Verlustserie (#8-#12) wurde durch gezielte Regel-/Learning-Arbeit überwunden — sichtbar deutlich ab Trade #13 (+42,96€, TP1+TP2 komplett). Trade #14 wird trotz Netto-Verlust als "teilweise positiv" eingeordnet, weil der Short-Hedge den technisch/broker-bedingten Schaden von potenziell deutlich mehr auf nur -1,92% begrenzt hat. Trade #15 erneut sauber (+1,15%). Tendenz seit Trade #13 wird als klar positiv und selbst-bestätigend für die Systemverbesserungen bewertet — explizite Rückenwind-Stimmung vor dem Phase-2-Übergang.

**Phase 1 verlängert auf 15 Trades (entschieden 02.07.2026, nach Trade #10):** Ursprünglich war Trade #11 der Start von Phase 2. Nach der Phasenübergang-Review zu Trade #10 (3 Verlust-Trades in Folge als expliziter Pause-Trigger, siehe unten) wurde entschieden, Phase 1 um 5 weitere Trades (bis Trade #15) zu verlängern, um die neu ergänzten Checks (Chartmuster-Check 9d, News-Schock-Präzisierung 9e in [[feedback_chartanalyse]]) erst mit echten Trades zu validieren, bevor die Positionsgröße in Phase 2 erhöht wird.

**Hochstufen wenn:** Erwartungswert der Phase positiv, max. Drawdown < 10%, Win-Rate ≥ 45-50%
**Zurückstufen/Pause wenn:** 3 Verlust-Trades in Folge wider Erwarten, Drawdown > 10% in einer Phase → Review + Strategie-Anpassung via Backtesting

**Phase-2-spezifischer Rückstufungs-Trigger (ergänzt 09.07.2026, Fable-5-Review):** In Phase 2 zusätzlich zum obigen allgemeinen Kriterium ein schnellerer, mechanischer Trigger: **3 Verluste in Folge → sofortige Rückstufung auf Phase-1-Positionsgröße** (500-1.500€), unabhängig vom rollierenden 4-von-5-Schutz (der greift erst später). Wiederaufstieg auf Phase-2-Größe erst nach erneutem Review, analog zur Cooldown-Auflösung (siehe oben).

**Klarstellung Auflöse-Bedingung Rollierender Verlustschutz (06.07.2026):** `scripts/cooldown_check.cjs` prüft rein mechanisch die letzten 5 Trades und bleibt rot, bis ein neuer Gewinn-Trade die Serie bricht — das Skript kennt "Review abgeschlossen" nicht. Die tatsächliche Auflöse-Bedingung laut User (06.07.2026) ist NICHT "warte auf einen Gewinn-Trade", sondern: **Pause (mind. 1 Handelstag) + abgeschlossene Analyse (bereits am 04./05.07. erfolgt) + danach ein wirklich sauberes, vollständig bestätigtes Setup** — dann darf der Cooldown manuell übersteuert werden, mit explizitem User-Go unmittelbar vor der Ausführung. Das Skript bleibt trotzdem als Pflicht-Check bestehen (Punkt 0 im Workflow) — sein Rot-Status ist kein automatisches Verbot mehr, sobald diese Bedingungen erfüllt sind, sondern ein Reminder, die Freigabe aktiv einzuholen statt sie zu vergessen.

**How to apply:** Bei rotem Cooldown-Status nicht automatisch blockieren, sondern prüfen: Ist die Pflicht-Review bereits dokumentiert (siehe [[feedback_regime_wechsel]])? Ist seit dem letzten Verlust mindestens 1 Handelstag vergangen? Zeigt sich jetzt ein wirklich sauberes Setup? Nur wenn alle drei zutreffen, den User aktiv um explizite Übersteuerung bitten.

**Why:** User will nicht von Tag 1 mit vollem Risiko auf 15.000€ starten — Track Record soll Kapital-Hochskalierung rechtfertigen, nicht umgekehrt.

**How to apply:** Bei jeder Positionsgrößen-Entscheidung aktuelle Phase prüfen (Trade-Counter aus memory/trades/ zählen). Nach jedem Trade-Block (10/25/50) gemeinsam Review machen ob hoch-/zurückgestuft wird. Dieser Fahrplan ist lebendig — jederzeit anpassbar mit neuen Daten.

**Phasenübergang-Review (ergänzt 23.06.2026):** Vor jedem Phasenwechsel (z.B. Trade #10 → Phase 2) nicht nur die Zahlen prüfen (Win-Rate, Drawdown, Erwartungswert), sondern aktiv alle gesammelten Learnings aus den einzelnen Tages-Zusammenfassungen (trades/trading_YYYY-MM-DD.md) der Phase zusammenfassen und bewusst in die nächste Phase mitnehmen — z.B. wiederkehrende Fehler, bewährte Setups, Regeln die sich in der Praxis geändert haben (wie die Trades-pro-Tag-Anpassung von 1 auf 3 am 23.06.). Ziel: Phase 2 startet nicht nur mit mehr Kapital, sondern mit einem geschärften System.

**Why:** User möchte sich und das System aktiv weiterentwickeln, nicht nur Kapital hochskalieren — "wir trainieren uns gemeinsam durch die Phasen" (User-Zitat 23.06.2026).

**Konkret für den Übergang nach Trade #10 (festgelegt 01.07.2026):** Sobald Trade #10 (voraussichtlich am nächsten Handelstag) abgeschlossen ist, KEIN Trade #11 (Phase 2) bevor die vollständige Phasenübergang-Analyse gemacht wurde. User-Zitat: "vorher alles perfekt analysiert haben müssen und uns hinterfragt haben mit der Strategie." Direkt nach Trade #10 in den Analyse-Modus wechseln (Win-Rate, Drawdown, Erwartungswert, alle Learnings aus den 10 Trades zusammenfassen, Strategie kritisch hinterfragen) — erst danach über Hoch-/Zurückstufung und einen möglichen Trade #11 entscheiden.

### Positionsgrößen-Formel (festgelegt 22.06.2026)
Mathematische Brücke von Chart-Level zur konkreten Stückzahl — verhindert Bauchgefühl-Entscheidungen.

1. SL-Level im Chart festlegen (technisch, z.B. Support)
2. SL-Abstand in % vom Nasdaq-Entry umrechnen
3. Mit Hebel multiplizieren → SL-Abstand % am Zertifikat
4. SL-Preis je Stück berechnen (Entry-Preis × (1 − SL-Abstand%))
5. SL-Abstand pro Stück in € (Entry-Preis − SL-Preis)
6. Stückzahl aus Risiko-Limit = Risiko€ ÷ SL-Abstand pro Stück
7. Stückzahl aus Positions-Limit (Phase) = Max-Positionsgröße ÷ Entry-Preis je Stück
8. **Finale Stückzahl = MIN(Schritt 6, Schritt 7)** — die konservativere Grenze gewinnt immer

Beispiel: Entry Nasdaq 29.400, SL 29.200 (200 Pkt = 0,68%), Hebel 5x → SL-Abstand Zertifikat 3,4%, Entry-Preis 6,50€ → SL-Preis 6,28€ → SL-Abstand/Stück 0,22€. Phase 1: Risiko-Limit 150€ → 681 Stück; Positions-Limit 1.500€ → 230 Stück. Finale Wahl: 230 Stück (konservativer).

### Workflow Trade-Ausführung (festgelegt 22.06.2026, Schritte 0 + 2 ergänzt 04.07.2026 nach Fable-5-Review)
0. **Vor jedem neuen Entry-Entscheid:** `node scripts/cooldown_check.cjs` ausführen. Bei Exit-Code 2 / roter Status ist kein neuer Trade erlaubt, unabhängig vom Setup — nicht mehr nur aus dem Gedächtnis prüfen (siehe Trade #12, 02.07.2026, wo der Cooldown-Bruch übersehen wurde).
1. Claude findet Setup im Chart (Support/Resistance, Entry-Level, SL-Level)
2. Claude berechnet die Positionsgröße über `node scripts/size.cjs --entry <NAS100> --sl <NAS100> --dir long|short --hebel <x> --zert <Stückpreis> --phase <1|2|3>` statt Kopfrechnen — liefert Stückzahl, SL-Preis fürs Produkt und einen Formel-Gegencheck der €/Punkt-Ratio (siehe [[feedback_zertifikat_pnl]])
3. User sucht passendes Produkt auf Scalable Capital (Faktor-Zertifikat Nasdaq, passender Hebel) und meldet aktuellen Stückpreis an Claude
4. Claude berechnet finale Stückzahl = berechnete Positionsgröße ÷ Stückpreis, plus SL/TP in € fürs konkrete Produkt
5. User führt Order aus

Beispiel: Berechnete Positionsgröße 1.000€, Stückpreis 5,00€ → 200 Stück.

**Why (Schritt 0):** Der Fable-5-Review (04.07.2026) hat aufgedeckt, dass Trade #12 (02.07.2026) nach zwei vorangegangenen Tagesverlusten hätte gesperrt sein müssen — die Cooldown-Regel existierte nur im Memory und wurde unter Zeitdruck nicht angewendet. Ein Skript-Check ist deterministisch und vergisst nichts.

### Fester Ablauf nach Order-Ausführung (ergänzt 01.07.2026)
1. User gibt: Stückzahl, Kaufpreis, NAS100-Kurs beim Fill
2. Claude berechnet SL-Preis fürs konkrete Produkt (in € und als % Abstand zur Entry-Zone)
3. User trägt SL beim Broker ein und bestätigt mit "SL gesetzt"

Siehe auch [[feedback_live_trading]] Punkt 8 für das Anzeigeformat (Positions-Kasten).

### Weitere Regeln (festgelegt 22.06.2026)
| Regel | Wert |
|---|---|
| News-Blackout | Keine neuen Entries 30 Min vor/nach FOMC, CPI, NFP — bestehende Positionen nur absichern |
| Verlustserien-Cooldown | 2 Verluste in Folge am selben Tag → Trading-Stopp für den Rest des Tages (auch wenn 4%-Tageslimit noch nicht erreicht) — Schutz vor Revenge-Trading |
| Spread-Sanity-Check | Spread darf max. 10-15% des geplanten SL-Risikos ausmachen, sonst Trade auslassen |
| Overnight-Politik | Grundsätzlich keine Overnight-Positionen mit Faktor-Zertifikaten (Mo-So) — Finanzierungskosten + Event-Risiko. Ausnahme: sehr klarer Trend mit engem nachgezogenem SL |
| Steuer-Rücklage | Nicht nötig — keine separate Rücklage gewünscht (Stand 22.06.2026) |
| Wochenreview | Jeden **Montagmorgen** vor Session-Start — Win-Rate, größte Fehler, bestätigte Levels der Vorwoche |
| Chop-Erkennung (ergänzt 01.07.2026, live bewährt 09.07.2026 Trade #16) | Wenn RSI/MACD innerhalb weniger 5-Min-Kerzen mehrfach hin- und zurückkippen und VIX dabei flach bleibt (Definition siehe [[feedback_chartanalyse]] Marktregime-Filter) → **Position halbieren ODER Trade komplett auslassen**. Ein technisch erfülltes Kriterium kurz nach erkanntem Chop überschreibt diese Einschätzung NICHT automatisch. Bei Trade #16 (choppy Session-Start, Short/Long lange unklar) wurde die Position bewusst auf 50% reduziert (977€ statt 2.000€) — User bestätigte das im Debriefing explizit als richtige Anwendung, nicht als Fehler. |
| Rollierender Verlustserien-Schutz (ergänzt 02.07.2026) | Wenn **4 von den letzten 5 Trades Verluste sind — unabhängig vom Kalendertag** (also über mehrere Tage rollierend gezählt, nicht nur der tagesbezogene Cooldown) → sofortige Pause + Pflicht-Review (Ruhe verschaffen, jeden Verlust einzeln klassifizieren: Prozessfehler vs. Marktregime, siehe [[feedback_regime_wechsel]]). Erst NACH abgeschlossener Analyse und nur wenn sich danach ein erkennbar besseres/klareres Setup ergibt, wieder weitermachen — kein automatisches Fortsetzen nach Zeitablauf. |

### Stacking-Regel für gleichzeitige Halbierungs-Gründe (ergänzt 23.07.2026, Fable-Audit + Levi-Entscheidung; vierter Trigger ergänzt 27.07.2026)

Es gibt mittlerweile vier unabhängig voneinander entstandene Regeln, die jede für sich "Position halbieren" verlangen: Chop-Erkennung (Tabelle oben), Chasing-Situation ([[feedback_live_trading]] Punkt 13), das 15:30-16:00-Zeit-Gate ([[feedback_trading_zeitfenster]]) und seit 27.07.2026 **SL-Breite** (Schock-/Chop-ATR-Tier 2,5-3× statt 1,5-2×, siehe [[feedback_chartanalyse]] Punkt 8c). **Trifft mehr als einer dieser Gründe gleichzeitig zu, wird trotzdem nur EINMAL halbiert — 50% der jeweiligen Phasen-Positionsgröße ist der Floor, kein Multiplizieren mehrerer Gründe (kein 25%, kein 12,5%).**

**Why:** Fable-Audit 23.07.2026 deckte auf, dass keine der drei ursprünglichen Regeln beim Entstehen definierte, was bei gleichzeitigem Zutreffen passiert — z.B. ein Chasing-Entry im 15:30-16:00-Fenster an einem Chop-Tag. Dieselbe Art Lücke wie der frühere 8b/7a1-RR-Konflikt bei Trade #17, nur präventiv statt nach einem Vorfall aufgelöst. Levi hat sich am 23.07.2026 bewusst für einen festen Floor statt Multiplikation entschieden. **Ergänzung 27.07.2026 (Fable-Review, im Zug der RR-Entkopplungs-Reform bei TP1/TP2):** Ein notwendig breiter SL (Schock-/Chop-ATR-Tier) hält das €-Risiko über die Positionsgrößen-Formel zwar bereits automatisch konstant, ist aber zusätzlich ein Konfidenz-Abschlag (unschärfere Level, mehr Fehlausbrüche) — deshalb reiht sich "SL-Breite" als vierter, gleichrangiger Trigger in dieselbe Stacking-Logik ein, statt eine neue eigenständige Schwelle zu erfinden.

**Nicht doppelt zählen:** Stammen Chop-Erkennung UND der Schock-ATR-Tier aus demselben Marktregime (häufiger Fall, da beide oft gemeinsam auftreten), gilt trotzdem nur EINMAL der 50%-Floor — sie zählen als EIN Trigger-Grund, nicht als zwei.

**How to apply:** Bei der Positionsgrößen-Berechnung (Workflow Schritt 0-2 oben) prüfen, ob einer oder mehrere der vier Halbierungs-Trigger zutreffen — bei jeder Anzahl ≥1 gilt derselbe Floor: 50% der Phasen-Positionsgröße, unabhängig davon wie viele Gründe gleichzeitig vorliegen.

### Definition "1 Trade" (festgelegt 22.06.2026)
Ein kompletter Trade-Zyklus zählt als EIN Trade für die Phasen-Zählung (1-10, 11-25, 26-50) — unabhängig von der Anzahl der Teil-Orders.

Beispiel 12.06.2026: Entry-Tranche 1 + Entry-Tranche 2 (Nachkauf) + TP1-Teilverkauf + SL-Ausführung = **1 Trade gesamt** (nicht 4).

Ein neuer Trade beginnt erst, wenn die Position komplett geschlossen ist (0 Stück offen) und ein neuer, unabhängiger Entry-Entscheid getroffen wird.

### Trades-pro-Tag-Regel — aktualisiert 23.06.2026
Ersetzt die frühere starre "1 Haupttrade pro Tag"-Regel vom 22.06.

| Trade-Nummer am Tag | Status |
|---|---|
| 1-3 | Regulär erlaubt, keine besondere Rechtfertigung nötig |
| 4 | Nur nach vorheriger Prüfung + wirklich starkem technischem Signal — Ausnahme, kein Standard |
| 5+ | Nicht erlaubt, Tag ist beendet |

**Why:** Am 23.06.2026 wurden an einem volatilen News-Tag 3 reguläre + 1 Ausnahme-Trade durchgeführt (alle diszipliniert mit SL/RR-Prüfung, Ergebnis: +28,12€, keine Verluste). User hat danach entschieden, dass 3 Trades/Tag der reale, akzeptable Normalfall sind statt der ursprünglich zu konservativ angesetzten 1-Trade-Grenze.

**How to apply:** Trade-Counter pro Kalendertag mitzählen (siehe jeweilige trading_YYYY-MM-DD.md). Bei Trade 4 aktiv nachfragen/prüfen statt automatisch zuzulassen. Trade 5 ablehnen, unabhängig vom Setup.

### Psychologischer Check-in (festgelegt 22.06.2026)
Vor jeder Trading-Session kurz prüfen (User-seitig, Claude fragt aktiv nach falls Hinweise auf Stress erkennbar):
- Ausgeruht und nicht abgelenkt?
- Kein Zeitdruck?
- Nicht im "Revenge-Modus" nach einem Verlust-Tag?

**Why:** Revenge-Trading entsteht oft von der menschlichen Seite, nicht vom System. Schutzmechanismen (Cooldown etc.) greifen erst NACH Verlusten — dieser Check soll VORHER ansetzen.

**How to apply:** Bei Anzeichen von Stress/Eile/Frust in der Konversation aktiv nachfragen ob heute wirklich getradet werden soll, statt automatisch loszulegen.

### Strukturierte Trade-Datensammlung (festgelegt 22.06.2026)
Zusätzlich zu den ausführlichen Tages-Dateien (memory/trades/trading_YYYY-MM-DD.md) führen wir eine kompakte Tabelle für schnelle Statistik-Auswertung: memory/trades/trade_log.md mit einer Zeile pro Trade (Datum, Setup, Entry, SL, TP, Hebel, Positionsgröße, Ergebnis €/%, Win/Loss, RR erreicht). Wird nach jedem Trade ergänzt, Basis für Win-Rate-Berechnung ab Phase-Reviews.

### Nachkauf-Regeln (festgelegt 22.06.2026)
Drei Arten von Nachkauf unterschieden — nur eine Art ist erlaubt.

| Typ | Beschreibung | Erlaubt? |
|---|---|---|
| A) Nachkauf bei Rücksetzer innerhalb der These | Kurs zieht auf neues tieferes Level zurück, zeigt dort Bestätigungskerze | ✅ Ja, mit Bedingungen |
| B) Nachkauf bei gebrochener These ("Verbilligen"/Averaging Down) | Kurs fällt einfach weiter ohne neues Level/Bestätigung | ❌ NIEMALS |
| C) Pyramidieren im Profit (Nachkauf bei neuen Hochs) | Aufstocken bei starkem Momentum, bereits gesichertem Gewinn | ⚠️ Optional, fortgeschritten — aktuell NICHT aktiv, erst nach mehr Erfahrung einführen |

**Bedingungen für Typ A (alle müssen erfüllt sein):**
1. Ursprünglicher SL wurde noch nicht ausgelöst
2. Kurs erreicht neues, tieferes ECHTES technisches Level (nicht beliebig)
3. Bestätigungskerze an diesem Level (Hammer, Engulfing etc.)
4. Kombinierte Positionsgröße (Tranche 1+2) bleibt innerhalb Phasen-Limit
5. Kombiniertes Risiko (mit ggf. angepasstem SL) bleibt innerhalb 1-2% Risiko-Limit

**Wichtig:** Wenn der ursprüngliche SL bereits ausgelöst wurde → KEIN Nachkauf. Position ist geschlossen. Ein neuer Trade danach wäre ein komplett neuer, unabhängiger Entry-Entscheid mit neuer Analyse — kein "Rette die Position"-Nachkauf.

**Why:** Typ B (Averaging Down/Martingale-Prinzip) ist einer der häufigsten Kontokiller im Trading — Risiko wächst exponentiell ohne neue Bestätigung. Am 12.06.2026 haben wir korrekt Typ A angewendet (Nachkauf beim Rücksetzer auf neuen Support mit Bounce-Bestätigung).

### Hebel-Staffelung (korrigiert 22.06.2026 — ersetzt frühere fixe 5-8x-Regel)
Situationsabhängig nach VIX UND Setup-Qualität, max. 10x (Weg A+C Vereinbarung).

| VIX-Bereich | Marktlage | Erlaubter Hebel |
|---|---|---|
| < 15 | Sehr ruhig, Risk-On stark | bis 10x möglich |
| 15-20 | Normal | 5-8x |
| 20-25 | Erhöhte Nervosität | 5x max. |
| > 25 | Risk-Off / Krise | Kein Trade oder max. 3x bei sehr starkem Setup |

Zusätzlich Setup-Qualität beachten — oberer Hebel-Bereich nur bei Top-Setup (starkes Level + Bestätigungskerze + hohes Volumen + sauberer Trend), bei Unsicherheiten unteres Ende der erlaubten Spanne oder auslassen.

### Erweiterte Regeln Runde 2 (festgelegt 22.06.2026)
| Regel | Wert |
|---|---|
| Entry-Trigger | Kein Entry nur weil Kurs Level berührt — Bestätigungskerze nötig (Hammer, Engulfing, Kerze schließt wieder über/unter Level) |
| Order-Typ | Limit-Order für Entry (kein schlechter Fill bei Spikes), Market-Order nur für SL-Ausführung (Sicherheit vor Preis beim Ausstieg) |
| Max. Haltedauer | Nach 3 Stunden ohne TP/SL-Treffer bei seitwärts dümpelndem Markt → manuell raus, kein "Hoffen" |
| Markt-Regime-Filter | Keine klare Struktur (kein Trend, keine sauberen Levels, enge Range ohne Ausbruchsversuch) → kein Trade an dem Tag |
| Gesamt-Drawdown-Notbremse | Gesamtkapital fällt um 20% (15.000€ → 12.000€) → komplette Trading-Pause + vollständiges Strategie-Review, unabhängig von Phase |
| Daten-Sanity-Check | TradingView zeigt "?" oder klar veraltete Kurse → kein Trade bis verifiziert |

### Broker
Scalable Capital (Börse München/gettex) — bereits bewährt am 12.06.2026, bleibt erste Wahl für Faktor-Zertifikate.

**PRIME Broker+ Tarif:** Keine Ordergebühr ab 250€ Ordervolumen — bei unseren Positionsgrößen (5.000-6.500€) faktisch gebührenfrei, nur Spread-Kosten relevant. Damit aktuell günstiger als Alternativen wie Trade Republic (1€ Flatrate). Kein Wechselgrund aus Kostensicht (Stand 22.06.2026).

**Interactive Brokers (IBKR):** Für später relevant wegen geplantem Umzug nach Schweiz (Ende 2026/2027) — global, kein Brokerwechsel bei Umzug nötig. ABER: bietet keine deutschen Hebelzertifikate/Optionsscheine — Alternative wäre Micro Nasdaq Futures (MNQ) oder Optionen auf QQQ/NQ. Plan: parallel ausprobieren bevor Umzug ansteht, kein Zeitdruck aktuell.
