---
name: project-restpunkte-y1y3y5y8-2026-09-22
description: "ABGESCHLOSSEN 22.09.2026: gesamte Y1-Y8-Restpunkte-Liste aus der Testtag-21.09.-Analyse durchgearbeitet. Paket 1 (ec56289), Paket 2 (f28bd98), Y8-b (9738c60) committet, je mehrfach Opus-gegengeprueft FREIGEGEBEN. Y3-a: alle 20/20 Skipped-Nachtraege vollzogen (7 vom 21.09. direkt, 13 historische 11./15.09. via TradingView-Bar-Abruf), Opus-gegengeprueft FREIGEGEBEN. Y1-a und Y4-c bewusst zurueckgestellt (echte Regelentscheidungen). Backlog: Z28 hat dieselbe TP1/SL-Horizont-Asymmetrie wie die korrigierten 11.09.-Faelle."
metadata:
  node_type: memory
  type: project
  modified: 2026-09-23T09:28:59.244Z
  originSessionId: 9aca5bf5-bdfc-4bb6-922b-17583cc90312
---

## Nachtrag 23.09.2026: Z28-Backlog nachgezogen + mfe_r-Bug gefunden und gefixt

Opus hat die 7 damals offenen Restpunkte (u.a. Y4-c, X8, Z28) erneut geprüft (Auftrag von Levi im Hauptchat). Ergebnis u.a.: X8 war bereits durch Y3-a miterledigt (obsolet), Z28 war noch offen.

Fable hat Z28 nachgezogen: Horizont-Hinweis ergänzt (SL 30341,25 bis zur letzten verfügbaren Bar 21:05-21:10 DE nicht getroffen, Tiefstwert ab 20:00 DE = 30414,35 selbst aus `scripts/nas100_5m.json` gerechnet), plus zwei bereits bekannte Altfehler vom 17.09. korrigiert (MFE 14,75/0,00 Pkt statt falscher 40,55/44,50 Pkt, Quelle: [[project_testtag_analyse_2026-09-17]] Z64-65). Summe R über alle 29 Einträge unverändert bei +8,99 R.

Dabei fand Fable einen strukturellen Bug: `scripts/skipped_fiktiv.cjs` berechnete das abgeleitete Feld `mfe_r` bei manuellem `--mfe-pkt`-Nachtrag nicht korrekt neu (blieb stale oder kam fälschlich aus `calc` statt dem manuellen Wert). Opus bestätigte den Fund, stellte fest dass kein anderes Skript `mfe_r` konsumiert (keine Geldfolge/Statistikverzerrung), und schrieb den Fix-Auftrag. Fable setzte um (Z. 299-303, manueller Wert → `mfe_r = mfe/|entry-sl|`, Risiko 0 → null), ergänzte einen strengen Regressionstest (6 Teilfälle), `npm test` 337/337 grün. Ein frischer Opus-Agent (ohne Kontext der eigenen Empfehlung) hat live am Code, an den Tests und an den Daten gegengeprüft — **FREIGEGEBEN, keine Auflagen**.

**Why:** Zeigt, dass das Autor≠Prüfer-Prinzip auch bei reiner Datenpflege greift — der Bug wäre ohne die Z28-Nacharbeit nicht aufgefallen, weil er nur bei manuellen Nachträgen mit vorherigem `calc`-Lauf sichtbar wird.
**How to apply:** Code-Fix (`scripts/skipped_fiktiv.cjs` + `tests/trading_scripts.test.js`) liegt Stand 23.09. noch ungecommittet im Haupt-Repo — Levi committet. Kein weiterer Handlungsbedarf, Z28 und der mfe_r-Bug sind beide abgeschlossen.

---

# Restpunkte Y1-Y3/Y5-Y8 nach Abschluss Y4 (22.09.2026)

Nach Committen von Y4-a+Y4-b (siehe [[project_testtag_2026-09-21_besprechung_ausstehend]]) hat Levi einen finalen, konsolidierten Opus-Bericht zu den verbleibenden Vorschlägen aus [[project_testtag_analyse_2026-09-21]] angefordert. Alle 7 Punkte (Y1, Y2, Y3, Y5, Y6, Y7, Y8) waren gegen den aktuellen Code-Stand (HEAD 9819ce0) verifiziert weiterhin offen — keiner durch Y4 miterledigt. Zwei Verschiebungen: **Y5 wurde dringlicher** (Y4-a öffnete die `--grund`-Tür, die Y5 wieder schließen soll), **Y8 wurde kleiner** (Enum-Validierung existiert schon über C1-Fix + Y4-Backlog#2, nur das "Bündeln" der Kaskaden bleibt offen als Y8-c).

## 3-Pakete-Struktur (Opus-Empfehlung, sequenziell wegen gemeinsamer Testdatei)

- **Paket 1** (`gate_check.cjs`, Priorität 1): Y5 (A3-Ausnahme-Sperre) + Y1-b (distanzbasierter X1-Trigger). **UMGESETZT+COMMITTET 22.09.2026 (Hash ec56289)**, 2 Runden Opus-Gegencheck (FREIGEGEBEN MIT AUFLAGEN → FREIGEGEBEN), 317/317 Tests grün.
- **Paket 2** (andere Dateien, Priorität 2): Y2 (`x_fetch_stamp.cjs`-Toleranz), Y6 (Screenshot-Frische+Zähler), Y7 (Rasterprüfung+Ableseprotokoll), Y8-a (`loop_stopp.cjs --grund` Pflicht), Y8-c (Sammelvalidierung statt Kaskade), plus neu Y3-c (loud, nicht blockierend bei offenen Nachträgen). **UMGESETZT+COMMITTET 22.09.2026 (Hash f28bd98)**, 2 Runden Opus-Gegencheck (FREIGEGEBEN MIT AUFLAGEN → FREIGEGEBEN), 326/326 Tests grün, `gate_check.cjs` unangetastet.
- **Y8-b** (P5-Fib maschinell ableiten): **UMGESETZT+COMMITTET 22.09.2026 (Hash 9738c60)**, 2 Runden Opus-Gegencheck (FREIGEGEBEN MIT AUFLAGEN → FREIGEGEBEN), 336/336 Tests grün. Neues Modul `fib_calc.cjs`, `--impuls-korrektur`/`--impuls-extrem`, maschineller Registernachtrag inkl. Measured-Move (F1-Fix), Levi-Entscheidung "Rechnung gewinnt bei Abweichung" umgesetzt.
- **Paket 3 / Y3-a (Skipped-Nachträge) — ABGESCHLOSSEN 22.09.2026:** alle 20/20 offenen Nachträge vollzogen (`scripts/skipped_setups_fiktiv.jsonl`, gitignored, kein Commit nötig).
  - **7 vom 21.09.** (16:07-19:28 DE) via `scripts/nas100_5m.json`: 2 Runden Opus-Gegencheck FREIGEGEBEN. **Wichtige Korrektur:** die ursprüngliche Analyse-Aussage "alle 7 erreichten TP1" war zu weit gefasst — der 19:07-Fall stand bei Loop-Stopp offen bei +0,42R und erreichte TP1 erst 20:20-20:25 DE (nach Fensterende). "Keines den SL getroffen" bleibt korrekt. `project_testtag_analyse_2026-09-21.md` entsprechend korrigiert.
  - **13 historische** (12× 11.09., 1× 15.09.) via frisch von TradingView geholten 5-Min-Bars (`scripts/nas100_5m_2026-09-11.json`/`_15.json`, gitignored) — TV wurde dafür gestartet (Levi-Freigabe). Fable nutzte einen `ui_evaluate`-Workaround (interne TV-API `zoomToBarsRange`/`mainSeries().bars()`, dieselben Aufrufe, die die Projekt-Standard-Tools selbst nutzen), weil `data_get_ohlcv` strukturell nur die letzten N Bars der geladenen Serie liefert, nie einen historischen Zeitraum. 2 Runden Opus-Gegencheck FREIGEGEBEN — Datenintegrität über Abgleich der live protokollierten Entry-Preise gegen die Kerzen-Spannen verifiziert (13/13 bestanden), technisch unbedenklich (Live-Loop nicht kontaminierbar), aber **künftige `ui_evaluate`-Eingriffe sollen Levi vorher angekündigt werden**. Textkorrektur nötig: TP1/SL-Horizont-Asymmetrie bei 6 OFFEN-Fällen vom 11.09. (SL wäre nach US-Cash-Close doch getroffen worden) — behoben.
  - **Mittelfristige Idee (Backlog, nicht dringend):** `getOhlcv` um einen `from`/`to`-Parameter erweitern, damit der `ui_evaluate`-Workaround für künftige historische Nachträge nicht mehr nötig ist.

**Restpunkte (Backlog, kein Blocker, keiner mit Geldfolge):**
- R1: Y8-c-Sammelvalidierung wirkt nur ohne `--state` vollständig — mit `--state` (Normalmodus) exitet ein unparsebares `--erster-vollcheck` weiterhin vor jeder Sammlung (keine Information geht verloren, aber die Kaskade bleibt für diese eine Eingabeklasse im echten Loop bestehen).
- R2: `--ref-<Ebene>` unparsebar ist nicht in der Y8-c-Sammlung (Parse passiert erst nach dem Sammel-Exit) — gleiche Lückenklasse wie A2, außerhalb des ursprünglichen Auftragsumfangs. Kleiner Fix möglich (`parseTsSoft` in der `refs`-Zeile).
- R3: veralteter Kommentar in `vollcheck.cjs` ("explizit vor a3Exit").
- F2: Fehlermeldungs-Reihenfolge in `register_touch.cjs` bei Doppelfehler (`--fib-faktoren` ungültig + Impulslänge 0) hat sich gedreht — kosmetisch, nur mit dem Mess-Flag `--fib-faktoren` erreichbar.
- F3: ohne `--state` ist ein unparsebarer `--impuls-ursprung` jetzt Hard-Exit 1 statt stumm ignoriert — strikt besser, aber unbeworbene Verhaltensänderung.
- F4: latent — `fibExtensionen` kann `faktor-ungueltig` liefern, `vollcheck.cjs` hat dafür keinen Ausgabezweig (aktuell unerreichbar, da Faktoren hartkodiert; würde `--fib-faktoren` je an `vollcheck.cjs` durchgereicht, Absturzrisiko). Billiger Guard möglich.
- F5: Baustein "Fib-Register n.a." erscheint auch wenn gar kein Impuls fällig ist — gehört zum offenen [[project_vollcheck_ausgabeformat_vereinfachung_todo_2026-09-16]].
- F6: Fibonacci-Zeilen-Suffix nennt "wird beim Nachtrag mitgeschrieben" auch im Zweig "bereits im Register", wo kein Nachtrag läuft — Wortlaut irreführend, kein Datenverlust. Zusätzlich offen (Regelwerksfrage, keine F1-Reparatur): der Registerabgleich prüft weiterhin nur die beiden Fib-Extension-Level, nicht den Measured-Move-Wert — ein veralteter MM im Register wird nie automatisch korrigiert.
- Z28 (21.09., Skipped-Eintrag 19:07): dieselbe TP1/SL-Horizont-Asymmetrie wie die korrigierten 11.09.-Fälle — TP1 wurde über den Loop-Stopp hinaus geprüft (Hit 20:20-20:25 DE), die SL-Seite nicht. Entwarnung in den verfügbaren Bars (SL 30341,25 nicht getroffen bis 21:10 DE), aber `nas100_5m.json` deckt das volle Session-Ende (23:00 DE) nicht ab — für eine belastbare Aussage müssten weitere Bars nachgeladen werden. Noch nicht nachgezogen.

## Gesamtabschluss (22.09.2026)

Die komplette Y1-Y8-Restpunkte-Liste aus der Testtag-21.09.-Analyse ist durchgearbeitet: alle 4 Code-Pakete (Y4, Paket1, Paket2, Y8-b) committet, alle 20 Skipped-Nachträge vollzogen — jeweils mehrfach Opus-gegengeprüft. Y1-a und Y4-c bleiben bewusst zurückgestellt. Offen sind nur die oben gelisteten kleinen Backlog-Punkte (keiner mit Geldfolge, keiner blockierend).

Y1-a und Y4-c bleiben bewusst zurückgestellt (echte Regelentscheidungen, keine Bugfixes).

## Levi-Entscheidungen (22.09.2026, alle Opus-Empfehlung gefolgt)

- **E-1 (Y1-a):** NICHT umsetzen — würde Anker-Definition (7b1 Schritt 3a) durchbrechen.
- **E-2 (Y3-b, 13 historische Nachträge 11.09.+15.09.):** erst frische 5-Min-Bars von TradingView probieren, bei Fehlschlag streichen. NICHT die Hoch/Tief-Näherung nutzen (X4 hat sie am 17.09. als fehlerhaft belegt).
- **E-3 (Y3-c):** `loop_stopp.cjs` soll bei offenen Nachträgen nur laut melden (Anzahl + fertige Kommandozeilen), NICHT blockieren — bleibt "kein Gate".
- **E-4 (Y5 Notausgang):** ersatzlos sperren, kein Fallback. Bereits in Paket 1 umgesetzt.
- **E-5 (Y8-b, P5-Fib-Pflicht):** maschinell ableiten (neues optionales `--impuls-korrektur`, Formel aus `register_touch.cjs:183-192` wiederverwenden), nicht streichen. **Noch nicht beauftragt** — Opus hatte es bewusst aus dem Kernauftrag herausgehalten, da es die Befüllungspflicht inhaltlich umdefiniert.

## Wichtige Detailbefunde aus dem finalen Opus-Bericht

- **Y3 Datenlage:** 20 offene Nachträge gesamt (12× 11.09., 1× 15.09., 7× 21.09.). Nur die 7 vom 21.09. sind mit lokalen Bardaten (`nas100_5m.json`, deckt 18.09.-21.09. ab) heute rechenbar.
- **Y6 Befund:** 9 von 45 Voll-Checks meldeten "Screenshot ✓" bei nur 3 tatsächlich neuen Dateien im Loop-Fenster — `vollcheck.cjs` prüft nur Existenz, nicht Frische; Zähler resettet bei Textvariation der Begründung.
- **Y2 Ursache exakt reproduziert:** VC#47-Abbruch war 6min08s nach Zeitanker, 8 Sekunden über der 6-Minuten-Toleranz.
- **Y7:** die Referenzfelder (`--ref-1h/-15m/-5m/-qqq`) existieren bereits, es fehlt nur die Rasterprüfung (liegt der Zeitstempel wirklich auf dem Kerzenraster) und die Persistenz der abgelesenen Werte im Log.

Nicht abschließend verifizierbar laut Opus: ob TradingView 5-Min-Bars 11 Tage zurück noch liefert (E-2, vor Paket 3 zu testen).

Bezug: [[project_testtag_analyse_2026-09-21]] (Ursprungsanalyse) · [[project_testtag_2026-09-21_besprechung_ausstehend]] (Y4-Abschluss) · [[feedback_dont_change_running_system]] (Y1-a/Y8-b bewusst nicht sofort umgesetzt) · [[feedback_verify_dont_cave]] (Opus verifiziert jeden Fable-Bericht am Code, nie nur übernommen).
