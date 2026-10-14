---
name: opus_bericht_testtag_2026-10-13
description: "Opus-Analyse fiktiver Testtag Di 13.10.2026 (NAS100), erstellt 13.10. 20:30-20:44: GUELTIG (54/54 Slots, 57 VC, 2 Hard-Exits mit Wiederholung, E9 232/232 Fires voll gelesen) -> 10. bewertbarer Tag, FREEZE-ENDE nach 10-Tage-Regel erreicht (AUTO 16/20 = zu selten); 7 Live-Gates (6 FAIL, 1 PASS 4/4 GRUEN = erstes GRUEN der Serie, OFFEN -0,12 R) alle nachgerechnet, skipped -2,54 R bestaetigt (3 Ausgaenge haengen an 0,05-2,05 Pkt am 18:30-Hoch = Spread-Empfindlichkeit); E2 erstmals 2x echt getestet und eingehalten; Brueche: B5/Z6 US-Session-Hoch 30594,35 nie im Register, 5/7 Gates ohne --jetzt, Z8 --jetzt 55/57 VC 42-201 s nach Fire, Fib-P5-Luecke 53 VC, 2 Gates ohne Ankuendigung (18:03, 19:36); 20:00-Grenzfall korrekt ohne Gate; ALT kumuliert knapp belegt (Oe +0,14 / ohne besten Tag +0,05); Endauswertungs-Vorlage ab Mi 14.10. + 11 alte und 10 neue Entscheide"
metadata:
  type: project
  originSessionId: opus-analyse-testtag-2026-10-13
  modified: 2026-10-13T18:44:28.213Z
---

# Opus-Analyse fiktiver Testtag Di 13.10.2026 (NAS100) — erstellt Di 13.10.2026 abends

**Urteil: GUELTIG.** Der Tag ist voll bewertbar: `tagesmomente --auswertung` zaehlt ihn als **10. bewertbaren Tag** ("57 VC, Bars ok, Abdeckung 100 % (54/54 Slots)"). Jeder 5-Min-Slot 15:20-20:00 hat genau einen Voll-Check, Nummern #1-#57 slot-treu, 0 Dateinamen-Abweichungen, Terminal 20:02 und Loop-Stopp sauber, 0 echte Trades, alle 7 Gates und alle 5 skipped-/1 Kombi-Nachtraege rechnen nach. Die Brueche des Tages (B5-Hoch nie registriert, 5 Gates ohne `--jetzt`, Z8, Fib-Luecke, zwei Gates ohne Ankuendigung) sind Protokoll-/Handwertfehler **ohne Wirkung auf ein Gate-Ergebnis oder eine R-Zahl**. Mit diesem Tag ist die **10-Tage-Regel der Freeze-Definition formal erfuellt** (Freeze-Ende ja, AUTO "zu selten").

Inhaltlich war der 13.10. ein **Range-/Klebetag unter bearischem 1H**: US-Open-Flush auf 30369,65, V-Squeeze auf 30594,35, danach Level-Ping-Pong um 5m-EMA50/S1 30493,2 mit 8 EMA-Crosses nach 15:30. Das Dual-Gate wechselte sieben Mal; zweimal stand es technisch 2/2 LONG gegen den 1H — **E2 wurde damit erstmals real getestet und beide Male eingehalten**. Alle Short-Ideen ausser dem 15:32-Flush-Short scheiterten am Fenster (SL 3-5x ATR) oder liefen seitwaerts; der erste **Q-GRUEN-PASS der gesamten Testserie** (19:36) endete OFFEN -0,12 R.

**Zeitstempel / Arbeitsweise:**
- Start 13.10.2026 20:30:12 DE, Ende 20:44:23 DE (sha1-Endkontrolle 20:44, identisch).
- Zuerst Abschluss-Memo `project_testtag_2026-10-13_abschluss.md` und Tagesmemo `project_testtag_2026-10-13_ablauf_crons.md` gelesen, dann jede Aussage gegen Rohlogs/Bars geprueft. Primaerquellen: Skript-Logs, 59 `/tmp/vc_2026-10-13_*`, `loop_archiv/2026-10-13.txt` (inkl. Faktenprotokoll-Abschluss + Sicherheitsnetz-Nachtrag), datierte Bars, `analyse/abschluss_2026-10-13/`. Aus der Session-Datei habe ich **nur Tool-Aufruf-Records** gelesen (CronCreate-Wortlaut Z1-Z10 und Zaehlung der `Read`-Aufrufe der Prompt-Datei je Cron-Fire), keine Chat-Aussagen.
- sha1 der Originale um 20:30:46 notiert, kompletter `scripts`-Ordner nach `%TEMP%\opus_1013\scripts`, VC-Dateien nach `%TEMP%\opus_1013\vc`. Skriptlaeufe **nur auf der Kopie**: `analyse/rk13_1_filter --ab/--bis 2026-10-13`, `protokoll_bilanz --datum 2026-10-13 --position-offen nein`, `tagesmomente --auswertung`, `kombi_fiktiv --auswertung`, `analyse/h1_auswertung --echt --zaehlstand` (keine R-Werte, **KEIN end-Lauf**), `analyse/ibf_schatten --tage 2026-10-13` und Gesamtlauf. Eigene Lese-Skripte (EMA50 SMA-Seed, ATR Wilder, Zeitstrahl) in `%TEMP%\opus_1013\ana`.
- **sha1 Originale vorher = nachher (IDENTISCH, geprueft 20:39):** gate_check_log bc95bdc8…, kombi_fiktiv_log 5df86668…, momente_log 5373d665…, quick_tick f4a758de…, register_touch bca91c13…, skipped_setups_fiktiv 97e11cc2…, vollcheck_log 6d7c220b…, level_register fef0552e…, nas100_5m_2026-10-13 d4c4eef3…, loop_archiv 10-13 77a4772c… (29 Dateien insgesamt, alle gleich). Alle 59 `/tmp/vc_2026-10-13_*` byte-gleich mit den Kopien. Kein git add/commit/push (HEAD 19b2581, keine getrackten Aenderungen), kein test:e2e, kein CDP, keine Skript-/Regel-Aenderung, kein Memory ausser diesem Bericht + Index-Zeile.
- Kurse nur aus `nas100_5m/15m/60m_2026-10-13.json`, `qqq_15m_2026-10-13.json` (Bar-Zeit = Eroeffnung DE). 15m/QQQ/5m-EMA50 treffen die VC-Werte auf < 1 Pkt (z.B. 15m 19:15-Bar 30505,3 vs VC#51 30504,4). **1H-EMA50: meine Rechnung liegt 6-10 Pkt ueber den VC-Werten** (19:00-Bar 30659,9 vs 30653,5) — Seed-Effekt aus nur 300 Bars, ohne Wirkung (Close lag den ganzen Tag 90-250 Pkt unter der EMA). Die 5m-EMA im VC ist wie am 08.10. der Wert der laufenden Kerze (VC#57 30505,0 = meine 20:00-Bar).

---

## 1. Gueltigkeit und Abdeckung (bis Stichtag 20:00)

| Punkt | Befund (aus Logs gezaehlt) |
|---|---|
| Start-Update 12:xx (vorgezogen) | Tweet-Set 12:13:46, Register 12:15:40/41 (Rundzahlen + Fib/MM, 24→27 Level), Backup `.bis2026-10-13.bak`, Screenshot `start_update_2026-10-13_1200.png`. TV war down, per `tv_launch` + `layout_switch NAS100` wiederhergestellt (Tagesmemo, plausibel). |
| Start-Update 15:00 | Tweet-Set 15:02:35, Register 15:04:34 (Rundzahlband 30300/30350), Backup `.bis2026-10-13_1500.bak`, Screenshot `_1500.png` — puenktlich. |
| Loop-Start (Cron 15:20) | Start-Fire 15:20:00, `last_loop_prompt_2026-10-13.txt` mtime 15:20 (488 Zeilen, err leer), */1-Job ab 15:20:05; **VC#1 15:24:45 im Slot 15:20** (Formel "15:20→15:20"), erster Quick-Tick 15:27:27. |
| Voll-Checks | `vollcheck_log` 59 Records: **57 Nummern (#1-#57), alle mit Exit-0-Record**, plus **2 Hard-Exit-Records**: VC#30 17:46:33 (`--stale-n` fehlte, Vorbedingung erst in diesem VC erreicht) → `_30b` 17:46:58; VC#45 19:02:21 (`--terminal-zeit` vergessen) → `_45b` 19:02:29. Memo stimmt. |
| Slots | Slots 15:20-20:00 (57) je genau ein VC im eigenen Slot. tagesmomente: **Abdeckung 15:30-19:55 54/54 = 100 %**. Keine nacherfundenen Slots. |
| Dateiname ↔ Kopfzeile | 57/57 korrekt; `_30.txt`/`_45.txt` enthalten nur die Hard-Exit-Meldung, `_30b/_45b` die Kopfzeile. **0 Abweichungen.** 57/57 VC-Screenshots im Original vorhanden (VC#7 erst 15:51:38, 10 s nach dem Skript — offengelegt). |
| Quick-Ticks | **173**. Doppel-Fire-Minuten **9** (15:44, 15:48, 16:28, 16:48, 16:53, 17:17, 17:33, 18:38, 19:43) — jeweils nach einer ausgefallenen Minute (verzoegerter Fire). Fehlende Minuten ausserhalb VC/Gate: ~20 (15:21-15:26 Loop-Anlauf, 16:27, 16:47, 17:09, 17:32, 18:31, 18:37, 18:58, 19:42 u.a.). **0 vordatierte Eintraege** (ts nie nach `geschrieben`; 08.10.: 26) — der 08.10.-Fehler ist behoben. 2 Eintraege > 30 s verspaetet (17:31:56 +57 s, 18:16:48 +64 s). Memo "Quick-Ticks je Nicht-VC-Minute" ist zu glatt. |
| VC-`--jetzt` (Z8) | Z8 verlangt den bare-`date`-Wert **des Fires**. Gemessen gegen den Cron-Fire-Zeitpunkt: **55/57 VC-`--jetzt` liegen 42-201 s nach Fire-Beginn** (Median ~65 s), nur VC#28 (6 s) und VC#30b (5 s) nehmen den Fire-Wert. Das betrifft den **ganzen Tag**, nicht nur VC#49-57 (Memo). Richtung: nachdatiert, nie vordatiert; Slot-Zuordnung blieb ueberall richtig. Hintergrund ist ein echter Regelkonflikt Z8 ↔ D5 (Tweet-/Register-Stempel nach Fire-Beginn), den Sonnet offengelegt hat (s. Entscheid N5). |
| Live-gate_check `--jetzt` | **5 von 7 Live-Laeufen ohne `--jetzt`** (15:32, 17:02, 17:22, 17:42, 18:03 → `jetzt: null`); nur 19:32/19:36 mit. Ursache (Memo: "ungeprueft"): das Argument fehlt schlicht in der argv. Folge: tagesmomente ordnet nur 2 Live-Laeufe zu ("Live-Laeufe 2") → Q4 der uebrigen Momente "nicht rekonstruierbar"; **keine Wirkung auf ALT/AUTO/FLOOR-R**, nur auf die ampel_min/max-Gruppierung. Am 07./08.10. hatten alle Gates `--jetzt` — Rueckfall. |
| Bars | 5m 12.10. 18:05 → 13.10. 20:00-Bar (einzige Luecke Handelspause 23:00), 15m/60m/QQQ nur Nacht-/WE-/Extended-Luecken. Pflichtfenster 66/66 ("Bars ok"). **Z10:** datierte Kopien 20:02-20:03; die undatierten `nas100_15m/60m.json`, `qqq_5m.json` stammten den ganzen Tag vom 08.10. und wurden beim Abschluss mit 13.10.-Daten ueberschrieben (Z10 "undatierte nicht ueberschreiben" formal verletzt, kein Datenverlust — 08.10.-Kopien existieren; die Gates lasen nur das frische `qqq_15m.json`). |
| Tweet-Check | Jedes 10-Min-Raster 15:30-20:00 hat ein `set` oder `polled`, jeweils nach `--check` (Z-Zusatz). Die 19 "Nachholen"-Flags sind das bekannte F-B-Artefakt (Stempel vor Skript), kein Slot-Ausfall. **Vollstaendig.** |
| Terminal 20:02 (P2.2) | VC#57 20:01:49 im Slot 20:00 mit `--terminal-geprueft ja`, Exit 0; `loop_stopp_log` **20:02:19 "keine"**, Anlass "Terminalzeit 20:02 erreicht". Sauber. |
| Tagesabschluss | Nachtraege skipped 20:03:17-22, Kombi 20:03:29; `tagesmomente --datum` lief **nicht** im Terminal-Fire (R4 "zuerst"), erst 20:15 durch den Sicherheitsnetz-Cron — offengelegt, ohne Folgen (momente_log danach unveraendert). |
| protokoll_bilanz (Kopie) | 57 Kopfzeilen #1-#57 ohne Luecke, 57 Format-Zeilen, gate_check 7 (FAIL 6 / PASS 1), Vorpruefungen 57 (56 TAUGLICH, 1 Abbruch VC#56), `--chasing` yes 3 / no 4, `--tp2-level-price` 7/7, skipped 5 Momente = 7 Laeufe, Kombi 1/1 mit Nachtrag, 1H-Schatten 57 (9 Blockaden). Die zwei ABWEICHUNGEN auf meiner Kopie (Screenshots) sind Kopie-Artefakt; im Original 59 Dateien. |
| E9 (Datei bei jedem Fire voll lesen) | Aus den Tool-Records gezaehlt: **232 eindeutige Cron-Fires 15:20-20:02, 232 mit vollstaendiger Lesung aller 488 Zeilen** (3 Bloecke). **Erfuellt — erstmals nachweisbar ueber den ganzen Tag.** |
| Freeze-Zaehlung | `tagesmomente --auswertung` (Kopie): **10 bewertbare Tage** (25.09., 28.09., 29.09., 30.09., 01.10., 02.10., 05.10., 07.10., 08.10., 13.10.), **AUTO 16/20** (heute AUTO n 0) → Ausgabe "**FREEZE-ENDE ERREICHT: ja** (Abbruchregel: 10 bewertbare Testtage >= 10, AUTO 16 < 20 -> AUTO 'zu selten, nicht belegt')". Nachgerechnet: Definition 28.09. (>= 5 Tage UND AUTO >= 20, sonst Ende nach 10 bewertbaren Tagen) → **Ende nach der 10-Tage-Regel**. Achtung: das gilt nur, wenn die vier Teiltage (25.09., 29.09., 01.10., 08.10.) zaehlen (Levi 30.09. + offener Entscheid 1 vom 08.10.). |

**Skriptbefund (wichtig fuer die Besprechung):** Der Block "ENDAUSWERTUNG (Levi-Entscheid 02.10.2026, Stichtag 2026-10-08)" in `tagesmomente --auswertung` zaehlt weiter nur bis 08.10. und druckt "Freeze-Ende laut Definition 28.09.: NICHT erreicht (9/10 Tage)" — **direkt unter** der korrekten Zeile "FREEZE-ENDE ERREICHT: ja". Fuer die Besprechung gilt die obere Zeile; der Block ist ein veralteter Stichtag-Text (s. Entscheid N9). Ebenso zaehlt `h1_auswertung` korrekt nur bis 08.10. (13.10. "nach Stichtag ignoriert: momente 57, kombi 1").

## 2. Tagesbild und Dual-Gate

**Kalender/Sperrfrist (Start-Update):** heute kein Blackout-Event (NFIB frueh, Existing Home Sales 16:00 nicht listenpflichtig, Fed-Redner ohne gesicherte Zeit); alle 7 Gates `--blackout none` korrekt. **Morgen Mi 14.10. CPI 14:30 DE (Blackout)**, Do PPI/Retail + Warsh (Termin unklar). Makro: KOSPI -5,5 %, KI-Finanzierungssorgen, Bank-Earnings gemischt, Renditen hoch (10Y 5,20), Geopolitik Hormuz/Houthi.

**Marktverlauf (5m):** Nacht vom Tageshoch 30683,4 (00:20) auf 30386,6 (04:45), Erholung bis 30551,2 (11:25), dann zaeh abwaerts (30420,7 um 14:45), 34 Kerzen unter der 5m-EMA. **15:30-Bar: Flush auf 30369,65** (US-Session- und Tagestief), **15:35-Bar +113 Pkt (Squeeze)**, Session-Hoch **30594,35 (16:00-Bar)**. Abverkauf 16:45-17:35 auf 30415,45, Rueckkehr 18:00-18:30 auf 30587,15 (knapp unter Session-Hoch, weit unter 1H-EMA), dann Seitwaertsband um EMA50/S1 (30493-30530) bis 20:00; Schluss 19:55-Bar **30498,55**. Tagesrange 313,75 Pkt (Memo 313,8 ✓), US-Range 224,7. 5m-EMA-Crosses nach 15:30: 15:35↑, 16:50↓, 17:10↑, 17:15↓, 18:00↑, 19:15↓, 19:35↑, 19:45↓ (8). ADX 5m fiel von 38 (15:32) auf 18 (19:36).

**Dual-Gate-Zeitstrahl (geschlossene Bars, nachgerechnet; 1H den ganzen Tag unter EMA50 → Override bearisch):**
- 15:20-15:45 (VC#1-#5): **2/2 SHORT gueltig** (15m 15:15 30397,9 < 30497,7; QQQ 739,51 < 743,56).
- 15:45-16:00 (VC#6-#8): 1/2 (NAS-15m 15:30 30554,45 > 30500,0).
- 16:00-16:15 (VC#9-#11): **2/2 LONG mit 1H-Override dagegen** (QQQ 15:45 743,70 > 743,56) → E2-Fall 1.
- 16:15-17:00 (VC#12-#20): 1/2 (QQQ 16:00 743,28 < 743,54).
- 17:00-18:15 (VC#21-#35): **2/2 SHORT gueltig** (15m 16:45 30484,85 < 30506,7).
- 18:15-18:30 (VC#36-#38): 1/2 (15m 18:00 30517,45 > 30501,4).
- 18:30-19:00 (VC#39-#44): **2/2 LONG mit Override dagegen** (QQQ 18:15 743,16 > 743,05, 18:30 743,33 > 743,06 — Abstand nur 0,1-0,3) → E2-Fall 2.
- 19:00-19:30 (VC#45-#50): 1/2 (QQQ 18:45 742,40 < 743,04).
- 19:30-19:45 (VC#51-#53): **2/2 SHORT gueltig** (15m 19:15 30482,95 < 30505,3).
- 19:45-20:00 (VC#54-#56): 1/2 (15m 19:30 30523,55 > 30506,0).
- 20:00:00 (VC#57): 2/2 SHORT (15m 19:45 30498,55 < 30505,7) — **Nach-Stichtag** (s. 6).
Alle VC-Zustaende stimmen mit dieser Rechnung ueberein (jeder Wechsel geprueft).

## 3. Die 7 Live-gate_check-Laeufe (nachgerechnet)

| Lauf | Ausloeser | Entry / SL (Anker) / TP1 | SL-Dist. | RR / Zone | Q1/Q2/Q3/Q4 (Ampel) | Ergebnis | hypoth. bis 19:55-Bar |
|---|---|---|---|---|---|---|---|
| 15:32:42 (VC#3, **ohne `--jetzt`**) | 13.1 k 3 (34 Short-Kerzen), nach Order-Sperre 15:30 | 30403,45 / 30476,9 (30465,8 = 14:25-LH) / 30334,3 S2 | 3,31× | 0,941 / **Z3** | ✗/✗2,44/✓/✗0,05 (1/4 ROT) | **FAIL** rr+tp, AUSSICHTSLOS | SL -1 (15:35-Squeeze) |
| 17:02:39 (VC#21, ohne `--jetzt`) | Dual-Gate wird 2/2 (15m 16:45); 5m-Schluss 16:50 unter EMA + S1 | 30488,65 / 30589,2 (30571,95 = 16:40-LH) / 30400 | 2,91× | **0,882** / Z2 | ✓/✓0,27/✓/✗0,44 (3/4 GELB) | **FAIL** rr (Fenster [30385,15;30388,1], Entry 5,9 Pkt ausserhalb) | OFFEN -0,10 (SL um **2,05 Pkt** verfehlt, 18:30-Hoch 30587,15) |
| 17:22:23 (VC#25, ohne `--jetzt`) | (a) 5m 17:15 unter EMA **+ S1-Re-Bruch nach Reclaim 17:10** — **gleicher Anker + TP1** | 30474,55 / 30587,6 / 30400 | 3,61× | **0,659** / Z2 | ✓/✓0,72/✓/✗0,33 (3/4 GELB) | **FAIL** rr, UNLOESBAR (wdh 1) | (OFFEN -0,21; SL um 0,45 Pkt verfehlt) |
| 17:42:36 (VC#29, ohne `--jetzt`) | 13.1 k 2 (5 Short-Kerzen), **neues TP1** Session-Tief | 30435,65 / 30587,1 / 30369,65 | **5,0×** | **0,436** / Z2 | ✓/✗1,69/✓/✗0,54 (2/4 ROT) | **FAIL** rr, AUSSICHTSLOS | SL -1 (18:30-Bar, **0,05 Pkt** ueber SL) |
| 18:03:39 (nach VC#33, ohne `--jetzt`) | 13.1 k 6 nach Zeitbox-Verfall — **gleicher Anker + TP1 wie 17:02** | 30503,55 / 30586,9 / 30400 | 2,79× | 1,242 / **Z3** | ✓/✓0,60/✓ manuell, **AUTO ✗**/✗0,03 (3/4 GELB) | **FAIL** tp (wdh 2) | (SL -1, 18:30-Bar 0,25 Pkt ueber SL) |
| 19:32:19 (VC#51) | Dual-Gate wird 2/2 (15m 19:15) **+ S1-Bruch 19:25-Schluss** | 30482,15 / 30533,6 (30523,35 = 19:05-LH) / 30400 | 2,51× | 1,597 / **Z3** | ✓/✓1,12/✓/✗0,39 (3/4 GELB) | **FAIL** tp (Fenster [30420,65;30430,7] ohne Level) | OFFEN -0,32 (SL um 3,25 Pkt verfehlt) |
| 19:36:43 (nach VC#52, **nicht angekuendigt**) | Retest +11,7 Pkt → **anderes TP1** 30450 im Fenster (Zeitbox 19:32 ersetzt, #6c) | 30493,85 / 30533,3 / 30450 | 1,98× | 1,112 / Z2 | ✓/✓0,55/✓/✓2,14 (**4/4 GRUEN**) | **PASS** (halbe Pos. wg. Z2) | OFFEN **-0,12** (MFE 25,9 = 0,66 R, MAE 36,5 = 0,93 R) |

- **Alle 5 skipped-Eintraege bar-fuer-bar bestaetigt:** 15:32 SL -1 | 17:02 OFFEN -0,10 (wdh 2) | 17:42 SL -1 | 19:32 OFFEN -0,32 | 19:36 OFFEN -0,12 → **Σ -2,54 R** wie im Memo. Kombi 19:36 (Groesse 1, GRUEN) OFFEN -0,12, kein Stall-Exit (12.1a nicht ausgeloest) — bestaetigt.
- **Rest der Gate-Kerze nach dem Entry (Methodenluecke 08.10. 18:16):** fuer alle 7 Laeufe geprueft — in keiner Gate-Kerze wurde nach dem Entry SL oder TP1 beruehrt (19:35-Kerze: Hoch 30519,55 < SL 30533,3; 17:00/17:20/17:40/18:00/19:30-Kerzen weit weg). Einzige Unschaerfe: beim **15:32-Lauf** ist die MFE im Log 0, das 15:30-Bar-Tief 30369,65 kann aber nach 15:32:42 entstanden sein (Register-Touch 15:34:18) → MFE 0-33,8 Pkt (0-0,46 R), R unveraendert -1. Das Memo-Ergebnis des Sicherheitsnetzes ("keine Abweichung") stimmt.
- **Neu: Spread-Empfindlichkeit.** Das 18:30-Hoch **30587,15** entscheidet vier Ausgaenge: 17:42 SL-Hit um 0,05 Pkt, 18:03 um 0,25 Pkt, 17:22 verfehlt um 0,45 Pkt, 17:02 verfehlt um 2,05 Pkt; 19:32/19:36 verfehlen den SL um 3,25/2,95 Pkt (19:40-Hoch 30530,35). Der Chart zeigt Bid; ein Short-SL wird real am Ask ausgeloest. **Mit ≥ 2,1 Pkt Spread waere 17:02 ein SL (-1 statt -0,10) → skipped -3,44 R.** Die Methode (Bid-Bars, kein Spread-Zuschlag) ist systematisch optimistisch fuer Short-Nachtraege — Punkt fuer die Nachtrag-Methodik (Entscheid N8), heute ohne Logaenderung.
- **Ideen statt Laeufe (gleicher Anker + gleiches TP1 = 1 Idee):** 7 Laeufe = **5 Ideen** (15:32 | 17:02/17:22/18:03 | 17:42 | 19:32 | 19:36) = genau die 5 skipped-Zeilen. 19:32 und 19:36 sind nach Fingerabdruck zwei Ideen, marktseitig **eine** Bewegung (gleicher Anker, TP1 30400 vs 30450); unabhaengige Bewegungen heute also 4.
- **Q-ROT heute: n 0** (kein PASS-ROT). Q-ROT hatte heute keine Wirkung; Q1 erstmals 6/7 ✓ (Retest-Ablehnungen an S1/EMA), Q4 ✓ nur 19:36, Q2 ✓ 5/7.
- **Erstes Q-GRUEN der gesamten Serie** (skipped-Historie: GRUEN n 1, GELB n 3, ROT n 38): Ergebnis -0,12 R OFFEN zum Stichtag — ein einzelner Datenpunkt ohne Aussagekraft, aber ehrlich: der Markt hat die "perfekte" 4/4-Idee nicht bezahlt.

## 4. Umsetzung der Levi-Entscheide (E1-E9 vom 08.10.) und Zusatz Z1-Z10

- **E2 (kein Gate bei Trigger mit 1H-Override dagegen): umgesetzt — erstmals echt getestet, 2/2 eingehalten.** VC#9 (16:00, QQQ-Flip = Long-Trigger bei 2/2 LONG) und VC#39 (18:30, QQQ-Flip) jeweils "kein Gate (E2)"; VC#40-#44 ohne Gate. IBF-Schatten: beide Blockade-Episoden "Kurs nie jenseits der 1H-EMA" (Hoch 30587 vs EMA ~30665) → auch mit IBF kein Gate. **Hypothetisch** waeren die Long-Ideen schwach gewesen: 16:00 Entry ≈ 30580, SL unter 30369,65 (7×ATR) → Fenster leer; 18:30 Entry ≈ 30575, Long-Anker 30421,55 (6,5×ATR) → Fenster leer; beide Male fiel der Kurs danach um 100-150 Pkt. E2 hat heute nichts gekostet.
- **E3 (Zeitbox-Neuaufrufe unveraendert, dedupliziert): umgesetzt.** 17:22 und 18:03 laufen als `wiederholungen` 1/2 an der 17:02-Zeile; GATE-GEDAECHTNIS meldet 17:22 als WIEDERHOLUNGSAUFRUF, 19:36 als ZEITBOX-KOLLISION (#6c, neuer Fingerabdruck). Der Wortlaut "gleicher Anker + gleiches TP1" (Z7) steht im Tagesmemo, im `loop_archiv` nur bei VC#30/#31 und 19:36 — fuer 17:22/18:03 nur ueber die Skriptzeilen.
- **E4 / Z6 (B5: jedes neue US-Session-Hoch/-Tief ab 15:30 SOFORT ins Register): halb erfuellt.** Session-Tief 30369,65 um 15:34:18 eingetragen (≤ 4 Min) ✓. **Das US-Session-Hoch wurde nie eingetragen** — weder die Zwischenstaende 15:35-15:55 (30538,55 → 30585,85) noch das Endhoch **30594,35** (16:00-Bar). Sonnets Begruendung (Register-Feld "Session-Hoch" haelt das Overnight-Hoch 30683,4, ein Touch haette es ueberschrieben) widerspricht dem Wortlaut von Z6 und der Loesung vom 08.10. (Nacht-Extrema als eigene Level erhalten, US-Extrema separat). Die Touch-Notizen VC#31/#43/#55 nennen "US-Hoch 30594,35 bestaetigt", ohne dass es im Register steht (Memo-Punkt "Notiz ungenau" offengelegt). Ab 15:51 DE keine inhaltliche Register-Aenderung mehr (V11-Warnung ab VC#44, 185-250 Min). **Wirkung heute: keine** — alle Gates waren Short mit Zielen unterhalb, Long war durch E2 gesperrt, Q4 rechnete gegen 30400/30450/30369,65. Bruch bleibt Bruch.
- **E5 / Z5 (Punkt-11-Bezug = Gate-Richtungs-Lesart, in `--grund-chasing` benannt; Serien-Lesart als Notiz): 7/7 erfuellt.** Alle sieben `--grund-chasing` nennen die Gate-Richtungs-Lesart **und** "Serien-Lesart haette k=N ergeben" (08.10.: 10/12, Serien-Notiz 0) — Verbesserung.
- **Beinahe-Konflikte nach Serien-Lesart (5m-Serie gegen die Dual-Gate-Richtung bei gueltigem 2/2):** In den vier gueltigen SHORT-Fenstern lief die 5m-Serie nur kurz gegen die Gate-Richtung: VC#5 (15:35-Schluss, 1 Kerze), VC#24 (17:10, 1), VC#34/#35 (18:00/18:05, 2 Kerzen; ab 18:15 1/2), VC#53 (19:35, 1). Chasing-Kriterium (≥ 4) nie erfuellt → **0 Beinahe-Konflikte, nach beiden Lesarten identisch.** In den 2/2-LONG-Fenstern mit Override dagegen zaehlt k ohnehin nicht (b1). Erster Testtag seit dem 01.10. ohne den Lesart-Mechanismus.
- **E9 / Z2: erfuellt, 232/232 Fires** (s. 1).
- **Z1 (danach keine Entries): eingehalten** (VC#57 20:01:49 ohne Gate, s. 6).
- **Z8: gebrochen** (s. 1, 55/57 VC; Gates 5/7 ganz ohne `--jetzt`). **Z10: formal gebrochen** (undatierte ueberschrieben, kein Schaden). **Z9:** kein Event vor 20:00, korrekt.

**Bruchliste 13.10.:** (1) B5/Z6 US-Session-Hoch nie registriert; (2) 5/7 Live-Gates ohne `--jetzt`; (3) Z8 VC-`--jetzt` 55/57 nach Fire-Beginn (Definitionskonflikt mit D5); (4) Fib-P5-Luecke (`--impuls-korrektur`) in 53 VC — eine Skriptwarnung, die ab VC#5 jeden VC "MIT LUECKEN" macht (nur 4/57 vollstaendig), ohne dass sie je geschlossen wurde; (5) zwei Gate-Laeufe ohne Ankuendigung (18:03, 19:36), Fazit sagt jeweils "warten"; (6) VC#52 `--retest-bedingung nicht` falsch (Fenster mit Rundzahl 30450 lag vor); (7) VC#56 Anker-Wechsel ohne `--grund-sl-anker` (Vorpruefung Abbruch); (8) VC#53 `--punkt11-signal ja` mit Schlagzeile begruendet (kein Punkt-11-Kriterium; Z4 verlangt MACD-H/RSI-Wortlaut) — ohne Wirkung (k 0); (9) 2 Hard-Exits (VC#30 `--stale-n`, VC#45 `--terminal-zeit`); (10) Z10 undatierte ueberschrieben; (11) tagesmomente `--datum` nicht im Terminal-Fire.

## 5. 13.1-Richtungskonflikte

- **`rk13_1_filter --ab/--bis 2026-10-13` (Kopie):** 7 Live-Gates, **NACH 13.1-KONFLIKT n 0**, OHNE KONFLIKT n 7 (chasing yes 3 / no 4), OHNE VC-BEZUG 0. vollcheck_log gesamt: **0 Records mit `richtungskonflikt_13_1`** (686 Exit-0-Records). Die Prio-1-Warnzeile wurde im Live-Betrieb **nie** ausgeloest (vierter Tag ohne Anlass).
- 13.1 AKTIV heute: VC#2-#4 (k 2-3, Serie Short = Gate-Richtung → Gate 15:32), VC#29-#33 (k 2-6, Serie Short → Gates 17:42 und 18:03). Kein Konflikt moeglich.
- **Regelkollision 13.1 ↔ Zeitbox (neu, offengelegt):** VC#30-#32 (k 3-5, "50 %-Einstieg AKTIV vorschlagen") liefen **ohne** Gate, weil die AUSSICHTSLOS-Sperre des 17:42-Fingerabdrucks (Anker 30571,95 + TP1 30369,65) galt; VC#33 lief mit TP1 30400 (anderer, laengst verfallener Fingerabdruck). Nach Buchstabe waere derselbe 13.1-Lauf mit TP1 30400 schon in VC#31 (Zeitbox verfallen) bzw. VC#32 zulaessig gewesen. Hypothetisch VC#32 (17:57): Entry 30467,55, SL ≈ 30586,4, TP1 30400 → RR 0,57 FAIL; Ausgang auf Messers Schneide (18:30-Hoch 30587,15 vs SL). Keine Wirkung, ein Konsistenzpunkt (Entscheid N7).

## 6. Gate-Praxis nach Buchstabe (Z3), Stichtag, Q3

**Umgesetzt:** Alle 6 FAIL-Laeufe hatten leere, unloesbare oder levellose Fenster und wurden trotzdem gefahren. **Re-Bruch nach Reclaim = Trigger (b): angewendet** (17:15-Schluss bricht S1 30493,2 erneut nach dem 17:10-Reclaim → Gate 17:22) — das VC#48-Muster vom 08.10. ist heute nicht wiederholt. Gegenrichtungs-Crosses bei 2/2 SHORT (15:35, 17:10, 18:00, 19:35) nicht gegatet — durchgehaltene Lesart. S1-Brueche bei 1/2 (19:25, 19:50) korrekt ohne Gate.

**Trigger-Auslegung "Dual-Gate wird 2/2" (17:02, 19:32):** Sonnet wertet den 15m-Bein-Flip als Trigger (a) und nennt es selbst Auslegung. 19:32 ist ohnehin durch den frischen S1-Bruch (19:25-Schluss 30482,95 < 30493,2 nach 19:20 30496,85 darueber) gedeckt; 17:02 durch den S1/EMA-Bruch 16:50 (eine Kerze vor dem 2/2) — vertretbar, aber nicht definiert (Entscheid N6).

**Grenzfaelle ohne Gate:**
1. **17:25-Schluss unter Rundzahl 30450** (VC#26/#27): als Level-Bruch (b) lesbar; der Neuaufruf mit gleichem Anker + TP1 war gesperrt (AUSSICHTSLOS 17:22), mit TP1 Session-Tief waere RR ≈ 0,45 FAIL gewesen (genau das lief dann 17:42 ueber 13.1). Rundzahlen sind "generierte Kategorie" — ob ihr Bruch Trigger (b) ist, ist nicht festgelegt.
2. **20:00-Stichtag (VC#57):** 2/2 SHORT entstand mit dem 15m-Schluss um 20:00:00; Z1 "danach keine Entries" und der Nachtrag-Horizont 20:00 machen einen Lauf um 20:01:49 unzulaessig bzw. unmessbar. **Sonnets Entscheidung war richtig.** Hypothetisch (Opus-Naeherung): Entry 30495, Anker 30530,35, SL ≈ 30541, TP1 30450 → RR 0,99 FAIL (bzw. 30400 Zone 3 FAIL).

**Ankuendigung:** 5/7 im Block angekuendigt (VC#3, #21, #25, #29, #51). **18:03** (VC#33 "→warten: kein bestaetigter Trigger", Konsequenz "13.1 AKTIV") und **19:36** (VC#52 "→warten … Bedingung nicht erfuellt") liefen 73 s bzw. 22 s nach einem Block, der das Gegenteil sagt — dasselbe Muster wie VC#47/#51 am 08.10. Beide Laeufe sind **inhaltlich regelkonform** (13.1 bzw. Neuaufruf-Bedingung "anderes TP1 im Fenster" laut Skriptzeile von 19:32); vermerkt sind sie nur im Tagesmemo, nicht im `loop_archiv`. 0 nicht eingeloeste Ankuendigungen.

**19:36-PASS im Detail:** `--chasing no` bei 4 Kerzen (P6-Warnung). Die 19:30-Kerze war gruen (30482,95 → 30487,15), die Pause ist also belegbar, aber im Grund nicht genannt. Wirkung keine (halbe Position ohnehin wegen Zone 2, k 1 → kein 13.1). Q1 ✓ (19:30-Hoch 30489,95 unter S1 = Ablehnung von unten) plausibel.

**Q3 manuell vs Q3-AUTO:** 7/7 `--q3-coherence yes`, 6× KONSISTENT, **1× WIDERSPRUCH (18:03)**: manuell nach geschlossenem 5m-Bar (17:55-Schluss 30484,05 < EMA 30484,8, 0,75 Pkt) short — **nach Levis Ablesevorschrift richtig**; Q3-AUTO nimmt den Entry 30503,55 (laufende 18:00-Kerze ueber der EMA) → "5m long". **Zweiter reiner Definitionsfall** nach 08.10. 18:41. Serie seit 01.10.: **47 Laeufe, 0 UNBEKANNT, 4 Widersprueche** (05.10./07.10. Ablesefehler, 08.10./13.10. Definition).

## 7. Handwerte

- **Anker:** Alle Gate-Anker waren bestaetigte Strukturpunkte (30465,8 14:25-LH; 30571,95 16:40-LH mit zwei niedrigeren Hochs rechts; 30523,35 19:05-LH; Long-Anker 30369,65 V-Boden, 30421,55 HL 17:45). **Kein Wasserfall-Muster.** Das Problem war das Gegenteil: nach dem Impuls-Extrem 30415,45 (17:35) entstand kein LH (Hochs 30444 → 30445 → 30474 → 30486 → 30523), also blieb 30571,95 korrekt stehen (`--grund-kein-reset` 17:41/18:11 zutreffend) — und machte 17:42/18:03 geometrisch tot (5,0×/2,8× ATR). Anker-Log: 15 Eintraege (7 Wechsel, 1 ohne Grund verworfen VC#56, 5 reset-faellig, 2 reset-abgelehnt). 15:41 Short-Anker 30683,4 (Overnight-Hoch, 217 Pkt Sprung) nach Bruch des 14:25-LH: formal begruendet, ohne Gate-Wirkung.
- **`--stale-n`:** Vorbedingung nur auf der Gate-Seite gezaehlt (VC#1-#4, #30b-#33; bei 2/2 LONG mit Override "entfaellt") — der 08.10.-Befund (Gegenseite) trat heute nicht auf. VC#33 `--stale-n 4` per Live-Lesart (geschlossen 2/5, offengelegt) — ohne Folgen, aber gegen den Geschlossene-Bar-Grundsatz.
- **`--punkt11-signal`:** VC#34/#35 (MACD-H +1,3/+4,1/+5,6 ueber 2-3 Checks) korrekt `ja`; VC#28-#33 `nein` korrekt; VC#45-#50 `ja` (QQQ unter EMA) formal ok, k 0; **VC#53 `ja` per Iran-Schlagzeile = falsche Grundlage**.
- **`--chasing`:** 15:32 yes (34), 17:42 yes (5), 18:03 yes (9), 17:02/17:22/19:32 no (2/1/3) korrekt; 19:36 s. 6.
- **Retest-Bedingung:** VC#22/#23/#26/#27/#30/#31 korrekt "nicht erfuellt"; **VC#52 falsch "nicht"** (TP1-KANDIDAT 30450 im Fenster stand in derselben Skriptausgabe).
- **TP1-Wahl:** 15:32 TP1 S2 30334,3 statt naeher liegender Level (Tagestief 30386,6/Rundzahl 30400) — jede Wahl FAIL, ohne Wirkung. Rundzahl-TP1 (30400/30450) in 5/7 Laeufen = generierte Kategorie ohne Verifikation (Skript sagt es).

## 8. H1 / IBF / Auswertungsskripte (Kopie)

- **H1 `--echt --zaehlstand`:** unveraendert zum 08.10. (Stichtag 08.10.; 13.10. ignoriert): d = 5, Zelle A FLOOR n 19 aus 4 Tagen, Mindest-n nein. H1 ist mit dem end-Lauf 08.10. 20:13 **abgeschlossen ("zu wenig Daten", nicht belegt)** — heute korrekt kein zweiter end-Lauf. momente_log sha1 vorher = nachher.
- **IBF `--tage 2026-10-13`:** 9 Blockade-Momente in 2 Episoden (16:01-16:11, 18:31-18:56), IBF greift nicht (Kurs nie jenseits der 1H-EMA). Gesamt weiter **n 1 zaehlende Episode (29.09.) → ZU SELTEN, NICHT BELEGT**.
- **tagesmomente heute** (aus `abschluss_2026-10-13/tagesmomente`, bestaetigt): ALT n 2 Σ **-1,10** (SL 1/OFFEN 1), AUTO n 0, FLOOR n 7 Σ **-1,00** (TP 3/SL 4). Kumuliert s. 10.
- **kombi_fiktiv --auswertung:** 25 Eintraege, 16 unabhaengige Bewegungen, 9 Tage; Stufenbericht **25/30 → nicht berichtsfaehig**; GRUEN n 1 Ø -0,12, GELB n 3 Ø +0,10, ROT n 20 Ø +0,31 (gewichtet +0,08); beide Hypothesen weder bestaetigt noch gescheitert.

## 9. Abschluss-Memo 13.10. — Korrekturliste (nicht ueberschreiben)

**Abweichungen Memo/Tagesmemo vs Logs:**
- "`--jetzt` in VC#49-57 54-86 s nach Fire-Beginn" → **55/57 VC, 42-201 s, ab VC#1** (nur VC#28 und #30b mit Fire-Wert).
- "5 von 7 Live-Eintraegen `jetzt=null`, Ursache ungeprueft" → Ursache: `--jetzt` fehlt in der argv der fuenf Laeufe (Bedienfehler, kein Skriptfehler); Wirkung nur auf die Q4-Zuordnung in tagesmomente.
- "Quick-Ticks: je Nicht-VC-Minute" → ~20 Nicht-VC-Minuten ohne Tick, 9 Doppel-Fires.
- "B5-Auslegung offen, US-Hoch < Register-Hoch → kein register_touch" → **Z6-Bruch** (Wortlaut "jedes neue US-Session-Hoch"); Loesung vom 08.10. existierte.
- "Gate 19:36 Zeitbox-Neuaufruf … `--retest-bedingung` per Hand falsch" → bestaetigt; der Lauf selbst war durch die Neuaufruf-Bedingung "anderes TP1" der 19:32-Zeitbox gedeckt.
- "13.1-Regelkollision vs Zeitbox (VC#30-#32)" → nach Buchstabe waere ein 13.1-Lauf mit TP1 30400 ab VC#31/#32 zulaessig gewesen (so wie 18:03).
- "Datei bei jedem Fire voll gelesen" → **bestaetigt** (232/232).
- Memo nennt die Spread-Empfindlichkeit nicht (vier Ausgaenge an 0,05-2,05 Pkt).

**Fehlend im Memo:** E2 erstmals real getestet (2 Faelle, eingehalten); Re-Bruch-Regel 17:15 korrekt angewendet; 0 Beinahe-Konflikte nach Serien-Lesart; erstes GRUEN der Serie; Q3-Widerspruch 18:03 als **Definitions**fall (nicht Ablesefehler); tagesmomente-Endauswertungsblock zeigt veraltet "9/10".

**Bestaetigt:** 57 VC (#1-#57) + 2 b-Laeufe, 0 Dateinamen-Abweichungen; 7 Gates (6 FAIL / 1 PASS 4/4 GRUEN) mit allen Zeiten, Werten, UTC-Ids; skipped -2,54 R mit allen Einzelwerten; Kombi -0,12; Gate-Kerzen-Rest ohne Befund; Tagesrange 313,8, US-Hoch 30594,35, Tief 30369,65, Schluss 30498,55; 1H-Schatten 57/9; Register-Updates 10; Trigger-Kandidaten 30; Terminal 20:02 / Loop-Stopp keine; VC#56-Eigenfehler; 20:00-Grenzfall; Z10-Vorgang; tagesmomente-Nachholung 20:15 mit ALT -1,10 / FLOOR -1,00.

## 10. Vorbereitung der Besprechung/Endauswertung ab Mi 14.10.2026

**Zaehlstand nach dem 13.10. (bewertbare Tage 10/10, AUTO 16/20):**

| Kriterium | Stand | Urteil (Vorschlag) |
|---|---|---|
| Freeze-Definition 28.09. (≥ 5 Tage UND AUTO ≥ 20, sonst Ende nach 10 Tagen) | **10/10 Tage**, AUTO 16/20 | **Freeze-Ende erreicht (10-Tage-Regel)** — haengt an der Zaehlung der 4 Teiltage |
| ALT (Vorpruefungs-Anker) | n 32, Σ +4,40, **Ø +0,14**, ohne besten Tag (30.09.) **+0,05**; ohne Teiltage n 21 Ø +0,20 | **belegt nach Regel — knapp**; der 13.10. (-1,10) hat das Polster halbiert (08.10.: +0,18/+0,10) |
| AUTO (erster Swing nach Extrem) | n 16, Σ -3,74, Ø -0,23 | **zu selten, nicht belegt** (und negativ) |
| FLOOR (1,5×ATR) | n 63, Σ -2,77, Ø -0,04, ohne besten Tag -0,09 | **nicht belegt** |
| skipped seit 28.09. | roh n 52 Σ **-3,49**; dedupliziert (Datum+Richtung+Anker+TP1) n 43 Σ **-1,54**; roh mit 18:16-Korrektur **-5,60** (dedupliziert unveraendert, 18:16 faellt als Dublette weg); heute -2,54 (Spread-Variante -3,44) | Messung, kein Urteil |
| Q-ROT seit 28.09. | unveraendert: roh n 22 Σ **+2,45**, ohne Artefakt 02.10. 17:36 +2,70; dedupliziert ohne Artefakt n 17 Σ **+2,25, Ø +0,13** (ohne besten Tag ≈ +0,03); roh mit 18:16-Korrektur +0,34 / ohne Artefakt +0,59 | Vorzeichen positiv, regimeabhaengig, kein Beleg in irgendeine Richtung |
| Q-GRUEN / GELB (Option A) | GRUEN n 1 (-0,12), GELB n 3 (+0,05 skipped seit 28.09.) | viel zu selten |
| Kombi | n 25, Σ gewichtet **+1,56** (ohne Stall +0,80); mit 18:16-Korrektur **+1,17** / +0,41; 16 unabhaengige Bewegungen | **nicht berichtsfaehig** (25/30) |
| H1 | end-Lauf 08.10.: Zelle A n 19 aus 4 Tagen | **zu selten, nicht belegt** → Q-ROT bleibt Veto |
| IBF | n 1 | **zu selten, nicht belegt** |
| O12 (Prio-2-Paket) | heute 0 Hard-Exits wegen `--terminal-geprueft`/`--grund-chasing`, 0 Dateinamen-Abweichungen → **Tag 4/4 erfuellt** (05., 07., 08., 13.10.) | formal erfuellt; Fussnote: VC#45-Hard-Exit `--terminal-zeit` (Nachbarfeld) |
| Q3-AUTO-Vorab | 47 Laeufe, 0 UNBEKANNT, 4 Widersprueche (2 Ablese, 2 Definition) | formal verfehlt; vor Live-Nutzung 5m-Definition klaeren |

**Entscheidungsvorlage fuer Levi (ab Mi 14.10.; Mi = CPI-Blackout 14:30, kein Testtag noetig):**
- **Variante A (Empfehlung): Freeze ist nach der 10-Tage-Regel beendet** — Urteile wie in der Tabelle annehmen, ohne Vorbehalt "Definition nicht erfuellt" (das ist seit heute erfuellt), mit Vorbehalt "Teiltage zaehlen". ALT bleibt die einzige belegte Variante, knapp. Keine weiteren Testtage im alten Freeze-Rahmen.
- **Variante B: weitere Testtage, um AUTO auf 20 zu bringen** — nicht empfohlen: AUTO lieferte an 10 Tagen 16 Bewegungen (heute 0), Ø -0,23; auch 4 weitere Bewegungen machen daraus keinen Beleg.
- **Was zu beschliessen ist:** (1) 13.10. zaehlt (und 08.10. als Teiltag — davon haengt "10/10" ab); (2) Freeze-Ende nach A; (3) SL-Anker-Variante: **ALT behalten** (AUTO/FLOOR nicht belegt); (4) Q-ROT bleibt Veto (H1 nicht belegt; Serie dedupliziert Ø +0,13); Lockerung nur als neue, vorab registrierte Hypothese fiktiv; (5) Reihenfolge der Liste "nach der Endauswertung" (unten), mit **DST-Fix bis 19.10.** zuerst; (6) Bedingungen fuer den Wiedereinstieg ins Echtgeld: Regelwerk inkl. Q-ROT-Veto, Gate-`--jetzt` Pflicht, B5 als eigenes Level; (7) die 11 offenen Entscheide vom 08.10. + die neuen unten.

**Abgleich der 11 offenen Entscheide vom 08.10. mit dem 13.10.:**
1. 08.10. zaehlt (Teiltag) — **heute wichtiger geworden:** ohne ihn waere es 9/10 und kein Freeze-Ende. Empfehlung unveraendert ja.
2. 18:16-Korrektur — heute keine Gate-Kerzen-Wirkung; **neu: Spread-Empfindlichkeit** als zweite Methodenluecke (N8). Bestaetigt die Methodik-Baustelle.
3. VC#48 Trigger ohne Gate — heute **nicht wiederholt** (Re-Bruch 17:15 gegatet) → Z3-Erinnerung hat gewirkt.
4. Fazit/Handlung-Widersprueche — **bestaetigt** (18:03, 19:36).
5. Wasserfall-Anker — heute kein Fall; dafuer "kein LH nach Extrem → Anker bleibt → Setup tot" (17:42/18:03). Anker-Klaerung bleibt noetig.
6. E9 — **heute widerlegt, dass der Datei-Zeiger nicht haelt**: 232/232. Empfehlung abgeschwaecht: Zeiger + ausdrueckliche Pflicht im Cron-Text reicht; Einbettung nur, wenn es wieder bricht.
7. Endauswertung A/B — B ist passiert; jetzt ist die Formalie da, Empfehlung A (oben).
8. Q-ROT-Veto — keine neuen ROT-Daten; unveraendert.
9. Punkt-11-Bezugsrichtung — heute 0 Faelle, keine neue Evidenz.
10. Q3-AUTO-Definition — **bestaetigt** durch 18:03 (zweiter Definitionsfall).
11. Abschluss-Memo nicht ueberschreiben — gilt auch fuer den 13.10.

**Liste "erst nach der Endauswertung" (fortgeschrieben):**
- **DST-Fix `vollcheck.cjs` bis 19.10.2026** (harte Frist vor dem Divergenzfenster 26.-30.10.).
- **Nachtrag-Methodik:** (a) Rest der Gate-Kerze (1m/Quick-Tick), (b) **Spread-Zuschlag fuer Short-SL (Ask) bzw. Long-SL**, (c) MFE in der Gate-Kerze.
- **Punkt-11-Bezugsrichtung fuer 13.1** und Verhaeltnis 13.1 ↔ 1H-Override; **13.1 ↔ Zeitbox-Sperre** (VC#30-#32 vs 18:03).
- **Prompt-Einbettung statt Datei-Zeiger** — nach dem 13.10. niedriger priorisiert (Zeiger hielt).
- **X-MCP-Ausfallweg** (heute kein Ausfall; 08.10. offen).
- **Live-gate_check `--jetzt` als A3-Pflichtfeld**; **Z8 ↔ D5 aufloesen** (`--jetzt` = `date` unmittelbar vor dem Skriptaufruf, nicht Fire-Beginn).
- **B5-Werkzeug:** register_touch mit eigenem Typ "US-Session-Hoch/-Tief" neben Overnight-Extrema.
- Trigger-Definition: 15m-Bein-Flip als Trigger (a)? Rundzahl-Bruch als Trigger (b)?
- Stichtag-Definition (Kerze, die um 20:00:00 schliesst = nach Stichtag) schriftlich.
- Fib-P5 (`--impuls-korrektur`): entweder Pflicht durchsetzen oder Warnschwelle realistisch setzen (53 VC "MIT LUECKEN" entwerten das Format-Signal).
- tagesmomente-Endauswertungsblock (Stichtag 08.10. hart) aktualisieren.
- Bestehend: Zeitbox-Neuaufrufe nach PASS-ROT, Dubletten-Fingerprint, Gegenrichtungs-Cross, Gate bei leerem Fenster (Erfassung statt Lauf), Anker-Klaerung (Wasserfall / kein LH nach Extrem), Q3-AUTO 5m-Bein, Template "Gate nach VC im Block nennen", Auflagen A2/A3, EMA-Handwerte vom geschlossenen Bar, Screenshot-Pfad aus vc_nr, Wiederaufnahme-Checkliste.

## 11. Entscheide fuer Levi (nummeriert, je mit Empfehlung)

Die 11 Entscheide aus dem Bericht 08.10. (Abschnitt 11) bleiben offen; Abgleich s. 10. Neu fuer den 13.10.:

- **N1. Zaehlt der 13.10.?** Empfehlung: **ja** (100 % Abdeckung, alle Kennzahlen nachgerechnet). Konsequenz: 10/10 Tage → Freeze-Ende nach 10-Tage-Regel, AUTO "zu selten".
- **N2. Freeze-Ende und Urteile.** Empfehlung: **Variante A** — Freeze beendet; ALT belegt (knapp, Ø +0,14 / ohne besten Tag +0,05), AUTO zu selten, FLOOR nicht belegt, H1/IBF nicht belegt, Kombi nicht berichtsfaehig. ALT als SL-Anker-Variante behalten.
- **N3. Stichtag-Grenzfall 20:00.** Empfehlung: Sonnets Entscheidung bestaetigen; Definition festschreiben: "Signale aus einer Kerze, die um 20:00:00 schliesst, liegen nach dem Stichtag" (Nachtrag-Horizont endet 20:00, Entry waere danach). Hypothetisch waere es ohnehin FAIL gewesen.
- **N4. B5/Z6 US-Session-Hoch nie registriert.** Empfehlung: als Bruch werten (ohne Wirkung); fuer die Zukunft US-Session-Extrema als **eigenen Leveltyp** neben dem Overnight-Hoch fuehren (Werkzeug nach der Endauswertung).
- **N5. `--jetzt`.** Empfehlung: (a) die 5 Gates ohne `--jetzt` nicht nachtragen (Logs bleiben), in der Endauswertung vermerken; (b) `--jetzt` fuer Live-gate_check zur A3-Pflicht machen; (c) Z8 umformulieren: "`date` unmittelbar vor dem Skriptaufruf, nie geschaetzt" — der Fire-Beginn kollidiert mit D5, sobald Stempel dazwischen geschrieben werden.
- **N6. Trigger-Auslegung 15m-Flip (17:02, 19:32) und Rundzahl-Bruch (17:25).** Empfehlung: heute beide Gates gelten lassen (19:32 durch S1-Bruch ohnehin gedeckt); Definition nach der Endauswertung festlegen.
- **N7. 13.1 nach Zeitbox-Verfall (VC#30-#32 ohne, VC#33 mit Gate).** Empfehlung: Regel klaeren — "13.1 AKTIV + Verfall → Lauf mit nicht gesperrtem Fingerabdruck im selben VC" oder "Sperre gilt fuer den Anker insgesamt". Keine Nachbuchung.
- **N8. Spread-Empfindlichkeit der Nachtraege.** Empfehlung: Logs nicht aendern; in der Endauswertung die skipped-Summe 13.10. mit Spread-Variante zeigen (-2,54 / -3,44); Spread-Zuschlag in die Nachtrag-Methodik aufnehmen (zusammen mit 18:16).
- **N9. tagesmomente-Endauswertungsblock zeigt "9/10".** Empfehlung: fuer die Besprechung nur die Zeile "FREEZE-ENDE ERREICHT: ja (10 Tage)" verwenden; Block-Text nach der Endauswertung auf den tatsaechlichen Stichtag umstellen.
- **N10. Abschluss-Memo 13.10.** Empfehlung: nicht ueberschreiben, Korrekturliste in Abschnitt 9 genuegt. Die Kennzahlen stimmen alle; abweichend sind wieder Umfangsangaben (Z8 "VC#49-57" statt ganzer Tag) und Auslegungen (B5) — [[feedback_sonnet_eigenbericht_unzuverlaessig]] bestaetigt sich nur noch bei Details.

---

## Ehrliche Gesamtwertung

- **Gut:**
  - Technisch der sauberste Testtag der Serie: 54/54 Slots, jede Nummer slot-treu, 0 vordatierte Ticks, Tweet-Raster lueckenlos, Terminal/Loop-Stopp sauber, Nachtraege inklusive Gate-Kerzen-Pruefung korrekt.
  - **E9 vollstaendig gehalten (232/232)** und Z5 komplett (Serien-Notiz in jedem Gate).
  - **E2 bestand seinen ersten echten Test** zweimal, ohne Kosten; die Re-Bruch-Regel (Z3) wurde angewendet, wo sie am 08.10. fehlte.
  - Sonnet hat die eigenen Fehler (VC#56, Z8, Z10, 19:36-Retest, Stichtag) weitgehend selbst offengelegt — und beim 20:00-Grenzfall richtig entschieden.
- **Nicht gut:**
  - B5 wurde **umgedeutet statt befolgt**: das US-Session-Hoch stand nie im Register, obwohl Z6 woertlich "jedes neue US-Session-Hoch" verlangt und der 08.10. die Loesung zeigte.
  - 5 von 7 Gates ohne `--jetzt` — ein Rueckschritt gegenueber 07./08.10., den niemand waehrend des Tages bemerkt hat.
  - Zwei Gates ohne Ankuendigung; VC#52 schreibt "Bedingung nicht erfuellt", 22 s spaeter PASS.
  - Eine Skriptwarnung (Fib P5) wurde 53-mal ignoriert; ein Format-Signal, das immer rot ist, ist keins mehr.
- **Disziplin:** Kein Trade gegen das Regelwerk, kein Long gegen den 1H, kein Entry nach 20:00. Der einzige GRUEN-PASS wurde korrekt als fiktiver Einstieg erfasst und bis zum Stichtag nur beobachtet.
- **Inhaltlich:** Der 13.10. war ein Tag ohne Kante: ein Flush, ein Squeeze, dann vier Stunden EMA-Ping-Pong. Die Short-Ideen scheiterten an der Geometrie (alter Anker, kein neues LH), nicht an Q-ROT; der einzige Moment, in dem Struktur, EMA-Naehe und Runway zusammenpassten (19:36, 4/4 GRUEN), lief 26 Pkt in die Richtung und kam zurueck. Fuer die Endauswertung heisst das: Das Regelwerk filtert an solchen Tagen fast alles weg — richtig so —, und ALT haengt nach zehn Tagen mit Ø +0,14 R an einem duennen Faden. Ehrlich gesagt: Belegt ist nach der Freeze-Regel genau eine Variante, und auch die nur knapp. Was die Testserie wirklich gezeigt hat, ist ein sauber laufender, regeltreuer Prozess — nicht eine Kante, die Echtgeld rechtfertigt. Das sollte am Mittwoch der Ausgangspunkt sein, nicht die Frage, welches Veto man lockert.
