---
name: opus_backtest_2026-10-06
description: "Opus-Hindsight-Backtest Di 06.10.2026 15:30-20:00 (Testtag AUSGEFALLEN, zaehlt NICHT, Freeze 7/10 + AUTO 12/20 unberuehrt): 0 Trades; 9 Gate-Laeufe (7 FAIL rrGate, 2 PASS-Q-ROT), skipped nominal +1,13 R (n 8), Q-ROT -1,00/+1,20 R, Kombi gewichtet -0,18 R; 19:10/19:15 nur durch 13.1-Richtungskonflikt; Originale sha1-gleich"
metadata:
  node_type: memory
  type: project
  originSessionId: ff1e3978-30b2-4753-b0cc-63142350c564
  modified: 2026-10-06T18:19:00.956Z
---

# Opus-Backtest Di 06.10.2026 (Hindsight-Replay aus Kerzen, KEIN Testtag)

Erstellt 06.10. 20:07-20:30 DE. Loop lief heute nicht; dies ist eine Nachrechnung aus `nas100_5m/15m/60m_2026-10-06.json` und `qqq_15m/5m_2026-10-06.json` (20:05-Kerze nicht verwendet). Zaehlt NICHT als Testtag, geht NICHT in skipped/Kombi/momente der Originale ein.

## (a) Kurzantwort
1. Nach Regelwerk haette sich **kein freigegebener Trade** ergeben: 9 Live-Gates (7 FAIL rrGate, 2 PASS mit Q-Score 2/4 ROT, also ausgelassen), 0 Trades.
2. Nominal (RR-1-Mass, bar-fuer-bar bis zur 20:00-Bar): die beiden PASS-ROT ergaben **18:45 -1,00 R** (SL in der 19:10-Bar) und **19:15 +1,20 R** (TP1 31300 in der 20:00-Bar). Alle 8 skipped-Eintraege zusammen: **+1,13 R**. Kombi-Sicht (Groesse 0,5 bzw. 0,25, Stall 12.1a mechanisch, hat nicht gefeuert): **-0,50 + 0,32 = -0,18 R**.
3. Der 19:10/19:15-Strang entsteht nur ueber einen **13.1-Richtungskonflikt** (Kerzenserie SHORT unter der 5m-EMA50, Dual-Gate LONG; derselbe Mechanismus wie das Artefakt am 02.10. 17:36). Ohne ihn bleibt 1 PASS-ROT (-1,00 R), Kombi -0,50 R.

## (b) Tagesbild 15:30-20:00 (5m-Bars, DE)
- Vor dem Open schon hoch: 15:25-Schluss 31264,6, also ueber PDH 31119,85 und R1 31235,3 (Pivots aus der 05.10.-Tageskerze H 31119,85 / L 30709,2 / C 31087,7; PP 30972,25, R2 31382,9).
- Die 15:30-Kerze hatte 90 Pkt Range (Hoch 31317,35, Schluss 31238,45), Session-Tief 31208,95 in der 15:35-Kerze. Danach Trend bis zum **Session-Hoch 31365,55 (17:15)**, unterbrochen von einem Ruecksetzer auf 31231,65 (16:30).
- Ab 17:20 Abwaertsdrift. 5m-EMA50 per Schluss verloren um 18:15. Whipsaw 18:15-18:45 (5 Seitenwechsel), Tief 31232,05 (19:20). R1 und Session-Tief hielten per Schluss. Schluss der 20:00-Bar 31304,75; Session-Range 157 Pkt.
- 15m NAS100, QQQ 15m und 1H in **54/54 Slots ueber der EMA50**, also Dual-Gate 2/2 LONG, 1H nicht dagegen.
- Trigger-Ereignisse:
  - (b) Schluss ueber Session-Hoch: 16:05, 16:50, 17:00, 17:05 (Slots 16:10/16:55/17:05/17:10).
  - (a) 5m-Seitenwechsel LONG: 18:30, 18:40, 19:45. SHORT (gegen das Dual-Gate, kein Gate): 18:15, 18:35, 18:45.
- 13.1 AKTIV (k ≥ 2): 16:00-16:25, 16:50-17:35, 19:10-19:35 (die letzte Serie im Konflikt).

## (c) Gate-Laeufe (gate_check.cjs auf der Kopie, je mit --sl-vorpruefung davor; alle SL-Anker TAUGLICH, --cluster-level none)
| Slot | Ausloeser | Entry / SL (Anker) / TP1 | SL-Dist. | RR / Zone | Q1/Q2/Q3/Q4 | Ergebnis | hyp. R |
|---|---|---|---|---|---|---|---|
| 16:00 | 13.1 k2 | 31300,15 / 31194,93 (31208,95) / 31382,9 R2 | 3,75× | 0,79 / Z2 | ✗/✗2,37/✓/✗0,21 | FAIL, AUSSICHTSLOS | offen +0,04 |
| 16:10 | (b) + 13.1 | 31328,65 / 31194,11 (31208,95) / 31400 | 4,53× | 0,53 / Z2 | ✗/✗2,99/✓/✗0,30 | FAIL, AUSSICHTSLOS | offen -0,18 |
| 16:50 | 13.1 k2, neuer Anker | 31311,55 / 31215,50 (31231,65) / 31400 | 2,97× | 0,92 / Z2 | ✗/✗1,77/✓/✗0,25 | FAIL (Fenster 0,9 Pkt, ohne Level) | offen -0,07 |
| 16:55 | (b) + 13.1 | 31333,55 / 31215,78 (31231,65) / 31400 | 3,71× | 0,56 / Z2 | ✗/✗2,40/✓/✗0,03 | FAIL, AUSSICHTSLOS (skipped als Wiederholung) | (offen -0,24) |
| 18:35 | (a) 18:30 | 31291,25 / 31220,85 (31231,65) / 31350 | 3,26× | 0,84 / Z2 | ✗/✓0,05/✓/✗0,15 | FAIL, AUSSICHTSLOS | offen +0,19 |
| **18:45** | (a) 18:40 | 31291,35 / 31254,97 (31265,65) / 31350 | 1,70× | **1,61** / Z2 | ✗/✓0,06/✓/✗0,15 (RZ 31300) | **PASS, 2/4 ROT → ausgelassen** | **SL -1,00** (19:10-Bar, MAE = SL) |
| 19:10 | 13.1 k2 **KONFLIKT** | 31268,55 / 31221,51 (31231,65) / 31300 | 2,32× | 0,67 / Z1 | ✗/✓0,94/✗/✓2,59 | FAIL (Fenster ohne Level) | TP1 +0,67 (20:00-Bar) |
| **19:15** | Zeitbox-Neuaufruf, 13.1 **KONFLIKT** | 31257,05 / 31221,25 (31231,65) / 31300 | 1,72× | **1,20** / Z2 | ✗/✓1,41/✗/✓2,16 | **PASS, 2/4 ROT** (Q1/Q3), 13.1 × Q-ROT → Q-ROT geht vor | **TP1 +1,20** (20:00-Bar, MAE 25, PM-Vorbehalt 9 K) |
| 19:50 | (a) 19:45 | 31283,05 / 31222,52 (31232,05) / 31300 | 3,18× | 0,28 / Z1 | ✗/✓0,03/✓/✓3,95 | FAIL, AUSSICHTSLOS | TP1 +0,28 |

- **Gesperrt (AUSSICHTSLOS, gleicher Anker und gleiches TP1):** 16:05-16:25, 17:00-17:35 und die Trigger 17:05/17:10.
- **19:20-19:35:** nach dem 19:15-PASS-ROT nur noch "beobachten" (Option D, kein Sequenzbruch bis 19:40). Die Geometrien 19:20 (RR 1,9) und 19:25 (RR 1,6) waeren ohnehin Q3 ✗ gewesen.
- **Halbierungsfenster 15:30-16:00:** kein Lauf darin (erster Lauf um 16:01).
- **Kombi** (vom Skript geloggt, `kombi_fiktiv --nachtrag` auf der Kopie): 18:45 Groesse 0,5 (NEU-A 3/4 GELB) → SL -0,50 R; 19:15 Groesse 0,25 → TP1 + Rest offen +1,27 × 0,25 = +0,32 R. Stall 12.1a hat in beiden Faellen nicht gefeuert.

## (d) Gate-Praxis (Levi-Entscheid 2)
- 7 der 9 Laeufe hatten ein leeres bzw. unloesbares TP1-Fenster oder ein Fenster ohne Registerlevel (16:00, 16:10, 16:50, 16:55, 18:35, 19:10, 19:50).
- Unter der 05.10.-Heuristik ("kein Gate bei leerem Fenster") waeren diese 7 Laeufe entfallen, alle FAIL, ohne Geld- und Q-ROT-Wirkung. Nur 18:45 und 19:15 hatten ein Level im Fenster.
- Entscheid 2 haette also rund 7 zusaetzliche FAIL-Datenpunkte erzwungen.
- Ausnahme mit Folgewirkung: Der 19:15-PASS entstand ueber die Retest-Zeitbox des 19:10-FAIL (Entry danach im Entry-Fenster [31252,45; 31260,63]). Unter der Heuristik waere 19:15 aber ebenfalls gelaufen (13.1 AKTIV, Level im Fenster). Das Ergebnis bleibt gleich.
- Sperrlogik so gefuehrt: Jedes schon AUSSICHTSLOS gelaufene Paar (Anker, TP1) bleibt gesperrt, auch wenn das TP1 zwischendurch auf ein anderes Level und zurueck springt. Wird nach Buchstabe nur gegen den letzten Lauf verglichen, kaemen 16:15-16:25 noch 1-3 FAIL-Laeufe dazu (TP1-Flackern R2 ↔ 31400), ohne Ergebniswirkung.

## (e) Einschraenkungen (alles Naeherung)
- **Register rekonstruiert:**
  - Pivots/PDH/PDL aus der 60m-Tageskerze 05.10. (Definition am 02.10. gegen das Register geprueft: H/L exakt, C +0,1).
  - Session-Extrema aus geschlossenen 5m-Bars ab 15:30; Rundzahl-Band 31000-31600.
  - **Keine Fib-Extensions/Measured Moves**, keine Wick-Zonen, keine AVWAP-Baender. Weitere TP1-Level waren damit moeglich; Q4 haengt am rekonstruierten Register.
- **Register-Zeitstempel:** `updated` musste auf die Wanduhr gesetzt werden, weil gate_check das Alter gegen die Systemzeit misst.
- **Kein Kalender, keine Tweets/News** (nichts erfunden). `--blackout none` ist eine Annahme; eine Sperrfrist ist nicht ausgeschlossen.
- **Q-Werte:**
  - Q1 = "no" wie in Sonnets Praxis (5/5 am 05.10.).
  - Q3 nach EMA-Seiten-Definition (Entscheid 4).
  - Q3-AUTO war UNBEKANNT (kein oneh_shadow-Eintrag heute).
- **Indikatoren:**
  - Selbst gerechnet: ATR/EMA/RSI aus geschlossenen Bars (am 05.10. geprueft: 5m-EMA bis zur 19:50-Bar = 30969,73 ✓).
  - ADX(14) nach Standard-Wilder, MACD-H Standard; Live-Ablesungen wichen am 05.10. um 1-2 Pkt ATR ab.
  - Entry = Schluss der letzten geschlossenen Kerze, Bewertung ab der Folgekerze.
- **Anker:** juengstes bestaetigtes Fraktal-Tief (2/2), unter dem Entry und nicht unterschritten, also so, wie es P3/V9 "nach eigenem Standard" verlangt. Ein live zurueckhaengender Anker (Muster 05.10.) haette mehr Laeufe in die Unloesbarkeit geschoben.
- **13.1-k:** Punkt-11-Signal angenaehert als MACD-H(5m) gegen die Serienseite (wie Sonnet am 02./05.10.). Wird Punkt 11 gegen die Dual-Gate-Richtung bewertet, entfallen 19:10/19:15.
- **Retest-Zeitbox nach 18:45 PASS-ROT:** ("saubererer Entry mit Q-Score") nicht nachbildbar; kein Neuaufruf angenommen.
- **Nicht abbildbar:** Ermessen live (TP1-Wahl, Ankerwechsel, Punkt 12) und Schaetzungen aus Handwerten. 3 Ergebnisse treffen TP1 erst in der 20:00-Bar.

## (f) Integritaet
- Alle Laeufe nur in `%TEMP%\opus_bt0610\repo\scripts` (Kopie 20:07 DE). Eigene Skripte unter `analyse/opus_bt/`. Dort sind auch `vollcheck_state.json` und `level_register.json` fuer den Backtest ueberschrieben.
- **sha1 der Originale vorher = nachher (IDENTISCH):**

| Datei | sha1 (gekuerzt) |
|---|---|
| momente_log | 52a3f15f… |
| skipped_setups_fiktiv | d43357d9… |
| kombi_fiktiv_log | eaf290e1… |
| gate_check_log | d9a503f7… |
| vollcheck_log | a18afd6d… |
| vollcheck_state | 8b07cfe7… |
| level_register | 0209fdd3… |
| quick_tick_log | 5a79b54a… |

- Kein git, kein npm test, kein CDP, keine Aenderung an Skripten, Regelwerk oder Memory ausser dieser Datei.
- Hinweis: Im Original-Ordner ist `level_register.json.bis2026-10-05.bak` um 20:12 DE neu entstanden. Diese Datei stammt nicht aus diesem Backtest (keine Schreibzugriffe auf die Originale).
