---
name: fable_auftrag_kern_regelwerk_2026-10-14
description: "Fable-Auftrag (Opus-Entwurf 14.10.2026, kopierbar, selbsttragend): Kern-Regelwerk (12 Regeln) als neue kern_*-Skripte DANEBEN, altes Regelwerk eingefroren (Variante B), getrennte kern_*-Logs, Schatten 'alt haette' via gate_check --dry-run, --jetzt automatisch, Gate-Aufruf <= 8 Pflichtargumente, Kern-Loop-Prompt < 120 Zeilen eingebettet, kern_nachtrag (O2, 1m), kern_bilanz (binaere Technik-Kriterien), DST-Fix bis 19.10.; Pakete K0-K9 mit Akzeptanz/Trockenlauf 05./08./13.10.; Fiktivtest erst nach Opus-Gegencheck; Echtgeld nur nach Levi-GO"
metadata:
  type: project
  originSessionId: opus-fable-auftrag-kern-2026-10-14
  modified: 2026-10-14T10:43:04.946Z
---

# Auftrag an Fable — Kern-Regelwerk (Entwurf Opus 14.10.2026, von Levi freizugeben)

> **Kopierbarer Prompt ab hier.**

Du bist Fable und setzt im Repo `C:\Users\umnus\tradingview-mcp` ein neues, schlankes Regelwerk ("Kern") fuer den fiktiven NAS100-Testbetrieb um. Memory-Ordner: `C:\Users\umnus\.claude\projects\C--Users-umnus-tradingview-mcp\memory\`. Antworte auf Deutsch und sprich Levi mit "du" an.

## 0. Kontext (zuerst lesen)

- `opus_endurteil_regel_lockerung_2026-10-14.md`: finales Regelwerk und Testplan. Das ist die verbindliche Grundlage.
- `fable_stellungnahme_regel_lockerung_2026-10-14.md`: deine eigene Stellungnahme (Trigger-Praezisierung, Spread, 8c2).
- `opus_vorschlag_regel_lockerung_2026-10-14.md`: Kill-Statistik und Regelinventar.
- `opus_antwort_umbau_oder_freeze_2026-10-14.md`: die Architektur-Entscheidung (Variante B).
- Dauerregeln:
  - `feedback_commit_push_nur_levi.md`: kein Commit und kein Push durch dich, sondern "bereit zum Commit" melden.
  - `feedback_npm_test_destruktiv_incident_2026-09-17.md`: kein `npm run test:e2e`; `node --test` nur mit expliziter Pfadliste.
  - `feedback_zeitzone.md`: DE-Zeit, DST-Divergenzfenster 26.-30.10.2026, US-Open 14:30 DE.
  - `project_vollcheck_dst_fix_todo_2026-10.md`
  - `project_risikomanagement.md`

**Levis Entscheide (14.10.):**
1. Lockerung auf die 12 Kern-Regeln.
2. Test = maximal 10 fiktive Testtage, mit Regel-Freeze der neuen Regeln ab Tag 1.
3. Binaere Technik-Kriterien an 5 gueltigen Tagen (mindestens 1 Rangetag), danach eine Opus-Zwischenanalyse.
4. **Echtgeld gibt es nicht automatisch.** Es ist nur nach Levis ausdruecklichem GO moeglich. Im Code darf es keinen Echtgeld-Uebergang und keinen automatischen Modus-Wechsel geben, und `testtag_modus` bleibt `fiktiv`, bis Levi selbst etwas anderes setzt.
5. Ein Spread-/Produktvergleich ist nicht Teil des Fiktivtests. Im Fiktivtest wird nur Bid/Ask protokolliert.

## 1. Verbindliche Regeln (Kern 1.0)

Definitionen:
- Alle Kerzen sind **geschlossene** Kerzen.
- EMA = EMA50 mit SMA-Seed. ATR = ATR(14, Wilder). Beide werden mit `atr_qqq.cjs` `computeEma`/`computeAtr` aus den gespeicherten Bars berechnet (60m mit `count 500`).
- Zeiten in DE. Das DST-Fenster liefert `kern_zeit.cjs`.

1. **Richtung.** LONG gilt, wenn alle drei Bedingungen erfuellt sind:
   - der Close der letzten 15m-Kerze von NAS100 liegt ueber der 15m-EMA,
   - der Close der letzten 15m-Kerze von QQQ liegt ueber der 15m-EMA (Extended-Hours-Bars wie bisher),
   - der Close der letzten 1H-Kerze von NAS100 liegt nicht unter der 1H-EMA.

   SHORT gilt spiegelbildlich. Sonst gilt "keine".
2. **Trigger** (nur bei Richtung ≠ keine):
   - (a) **5m-Cross:** Die Vorkerze schloss auf der Gegenseite der 5m-EMA oder eines Levels aus Regel 3, die aktuelle 5m-Kerze schliesst in Richtung jenseits davon. Im Log steht `typ: "ema-cross"` bzw. `"level-cross"` mit dem Level.
   - (b) **Gate-Bildung:** Die Richtung war bei der vorherigen 15m-Kerze "keine" oder die Gegenrichtung und ist mit dem aktuellen 15m-Schluss gueltig, und der letzte 5m-Close liegt in Richtung jenseits der 5m-EMA. Im Log steht `typ: "gate-bildung"`.

   Je Kerzenschluss gibt es hoechstens einen Trigger.
3. **Level-Liste.**
   - Sie wird beim Start-Update aus `level_register.json` eingefroren nach `kern_level_<datum>.json` und enthaelt genau: PDH, PDL, PP, R1, S1, Overnight-Hoch, Overnight-Tief.
   - Keine Rundzahlen, Fib, Wick-Zonen oder US-Session-Extrema.
   - Im Loop wird nichts nachgetragen. Fehlt ein Typ, steht er als `null` in der Datei.
4. **Entry.**
   - Die Messgroesse ist die Eroeffnung der naechsten 5m-Kerze nach dem Trigger.
   - Zulaessig sind Trigger, deren Entry-Kerze zwischen 15:35 und 19:00 DE eroeffnet. Im DST-Divergenzfenster liegt das Fenster bei 14:35-19:00 (Zeit-Exit und Terminal bleiben wie bei tagesmomente auf 20:00/20:02).
   - Der Live-Kurs beim Gate wird zusaetzlich geloggt (`entry_live`).
5. **SL.**
   - Basis ist der juengste bestaetigte 5m-Swing auf der SL-Seite. Fuer LONG ist das die juengste Kerze i mit `low_i <= low_{i-1}` und `low_i <= low_{i+1}` (i+1 geschlossen) und `low_i < Entry`. SHORT gilt spiegelbildlich mit den Hochs.
   - SL = Swing ∓ 0,5×ATR(5m).
   - Mindestabstand vom Entry: 1,5×ATR. Ist der Abstand kleiner, gilt Entry ∓ 1,5×ATR.
   - R = |Entry − SL|.
6. **Kein Trade (harte Gates), wenn eine dieser Bedingungen zutrifft:**
   - R > 3,0×ATR (`cap`).
   - Spread in Pkt > 10 % von R (`spread`). Im Fiktivbetrieb laeuft diese Pruefung nur als Schatten, siehe 1a.
   - Der SL liegt innerhalb 0,5×ATR des Preises, an dem heute bereits ein SL derselben Richtung ausgeloest wurde (`8c2`). Grundlage ist die SL-Liste in `kern_state.json`.
7. **Ausgaenge.**
   - TP1 = Entry ± 1R: Dort werden 50 % geschlossen und der SL wird auf BE gesetzt. BE = Entry + Spread (LONG) bzw. Entry − Spread (SHORT).
   - TP2 = Entry ± 2R.
   - Der Rest wird um 19:58-20:00 als Zeit-Exit geschlossen (auch im DST-Fenster).
   - Weitere manuelle Exits sind nur als `notfall` mit Grund erlaubt.
   - Ausgangsklassen: `SL`, `TP1+BE`, `TP1+TP2`, `TP1+ZEIT`, `ZEIT`, `NOTFALL`.
8. **Groesse.**
   - `size.cjs --risiko 75` (Phase 3) ohne Halbierungen.
   - Im Fiktivbetrieb wird die Groesse nur ausgewiesen.
9. **Tageslimits.**
   - Eine Position zur Zeit. Sie ist belegt bis zum Ende der 5m-Kerze, in der der Exit erfolgt.
   - Maximal 3 Trades am Tag.
   - Stopp nach 2 Verlusten (Verlust := Exit ≤ −0,5 R) oder bei einer Tagessumme ≤ −2 R.
   - Nach einem SL ist dieselbe Richtung nur mit **neuem Trigger UND neuem Swing-Anker** erlaubt (Re-Entry-Konvention).
10. **Sperrfristen.**
    - `--blackout` wird im Start-Update als Fenster in `kern_state.json` gesetzt (FOMC/CPI/PPI/NFP/PCE/Zinsentscheid/Fed-Chef). Gesperrt sind 15 Min vor bis 30 Min nach dem Event.
    - Schock-Tier: ATR(5m) der 15:30-Kerze (DST: 14:30) ≥ 2× Median desselben Werts aus den letzten 10 vorhandenen `nas100_5m_<datum>.json`. Dann gibt es keinen Trade.
    - Bei weniger als 5 Vergleichstagen steht das Ergebnis auf `nicht bestimmbar` und blockiert nicht.
11. **Broker/Protokoll.** Bei jedem Gate werden `--spread-pkt` oder `--bid/--ask/--hebel` protokolliert. Die Umrechnung lautet `spread_pkt = (ask − bid) / bid × Kurs / hebel`.
12. **Schatten ohne Wirkung.** Zum Schatten gehoeren: "alt haette" (siehe K4), Q-Score, ADX/Effizienz und im Fiktivbetrieb das Spread-Gate.

**1a Fiktivbetrieb.**
- Das Spread-Gate wird gerechnet und geloggt (`spread_gate: "waere FAIL"`), blockiert aber nicht. Das ist der Default, bis Levi anders entscheidet (offener Punkt L1).
- Ab einem echten GO wird es hart. Der Schalter dafuer ist eine Konstante in `kern_config.cjs`, kein CLI-Flag.

## 2. Architektur (verbindlich, Variante B)

- Es entstehen neue Dateien `scripts/kern_*.cjs`. Die alten Skripte bleiben unveraendert.
- **Einzige Ausnahme** ist der DST-Fix in `vollcheck.cjs` Z. 401-404 (Fehlerkorrektur, siehe K1).
- Alte Logs werden vom Kern-Pfad weder gelesen noch geschrieben. Ausnahme: der Schatten via `gate_check.cjs --dry-run`, der nachweislich nichts schreibt.
- Alle Kern-Logs tragen den Praefix `kern_`, das Feld `regelwerk: "kern-1.0"` und `testtag_modus`.
- `--jetzt` ist **kein Pflichtargument mehr**. Die Zeit kommt zentral aus der Systemuhr ueber `kern_zeit.cjs`. `--jetzt-override` gibt es nur fuer Tests und Replays; damit entfaellt der Z8/D5-Konflikt.
- Rollback: Cron-Prompt wieder aus `node scripts/loop_prompt.cjs`.

## 3. Arbeitspakete (in dieser Reihenfolge)

**K0 Sicherung (0,1 Tag).**
- `scripts/_freeze_alt_2026-10-14.sha1` mit den sha1 aller bestehenden `scripts/*.cjs`, `scripts/analyse/*.cjs`, `scripts/*.jsonl`, `level_register.json`, `vollcheck_state.json` und `tests/*.js`.
- Akzeptanz: Die Datei existiert. Ein Abgleich am Ende von K9 zeigt Gleichheit fuer alles ausser `vollcheck.cjs` (DST) und `package.json` (Testliste).

**K1 Zeit + DST (0,3 Tag, Frist 19.10.).**
- `scripts/kern_zeit.cjs` exportiert:
  - `jetzt()`: Systemuhr, Override per `KERN_JETZT` env oder Funktionsargument.
  - `deParts(ms)`
  - `istDivergenz(ms)`: ICU-Offset `Intl.DateTimeFormat` mit `shortOffset` fuer Europe/Berlin und America/New_York; Differenz 5 h = Divergenz.
  - `fenster(datum)`: `{sessionStart, entryVon, entryBis, zeitExit, terminal}`.
- In `vollcheck.cjs` die Stunden 15/16 in Z. 401-404 durch `kern_zeit.fenster()` ersetzen (minimal, 4-6 Zeilen).
- Akzeptanz: Unit-Tests fuer 13.10.2026 (Normal: Start 15:30, Entry 15:35-19:00), 27.10.2026 (Divergenz: Start 14:30, Entry 14:35-19:00, Zeit-Exit 20:00), 02.11.2026 (Normal) und 30.03.2026 (Fruehjahr) gruen. Alle bestehenden Tests in `tests/trading_scripts.test.js` gruen.

**K2 Regel-Kern als reine Funktionen (0,8 Tag).**
- `scripts/kern_regeln.cjs` ohne I/O, mit diesen Funktionen:
  - `richtung({nas15, qqq15, nas60})`
  - `trigger({bars5, bars15, levels, richtung, richtungVorher})`
  - `swing(bars5, dir, entry)`
  - `slPlan({entry, swing, atr})`
  - `gates({R, atr, spread, slListe, sperre, schock, tagesState})`
  - `simuliere({bars5, bars1?, dir, entry, sl, R, spreadPkt, zeitExitMs})`

  Die Simulation laeuft Bar fuer Bar, SL vor TP in derselben Kerze, Short-Ausloesung am Ask = Bid + Spread, Long-Kosten = Spread, TP2 fruehestens ab der Kerze nach TP1.
- `scripts/kern_config.cjs` enthaelt alle Konstanten (CAP 3,0; FLOOR 1,5; PUFFER 0,5; SPREAD_MAX 0,10; SPREAD_GATE_HART false; MAX_TRADES 3; VERLUST_R −0,5; TAGESSTOPP_R −2; SCHOCK_FAKTOR 2; Level-Typen).
- Akzeptanz (Fixtures aus der Opus-Gegenrechnung, Spread 2 Pkt, Horizont 20:00; Bars `nas100_5m_<datum>.json`). Eingabe ist jeweils Entry zur naechsten Eroeffnung und der angegebene SL:

| Fall | Entry | SL | R | Erwartung |
|---|---|---|---|---|
| 30.09. Long | 15:35, 30493,75 | 30380,00 | 113,75 | `TP1+BE` +0,48 (Exit 19:25) |
| 05.10. Long | 16:55, 30984,45 | 30925,15 | 59,30 | `TP1+ZEIT` +0,96 |
| 08.10. Short | 18:45, 30952,75 | 31033,90 | 81,15 | `TP1+TP2` +1,50 (Exit 18:50) |
| 08.10. Short | 18:00, 30950,65 | 31017,50 | – | `SL` −1,00 (Exit-Kerze 18:15) |
| 13.10. Short | 15:35, 30424,65 | 30476,90 | 52,25 | `SL` −1,00 in der Entry-Kerze |
| 13.10. Short | 17:05, 30487,55 | 30589,20 | – | `ZEIT` −0,11 |
| 05.10. Long, Trigger 15:30-Kerze | 15:35-Eroeffnung, Anker 30752,8 | – | – | **Cap-FAIL** (R ≈ 7×ATR) |

  Toleranz ±0,02 R. Zusaetzlich: `richtung()` reproduziert fuer den 13.10. den Zeitstrahl aus `opus_bericht_testtag_2026-10-13.md` Abschnitt 2 (Wechselzeitpunkte 15:45, 16:00, 16:15, 17:00, 18:15, 18:30, 19:00, 19:30, 19:45, 20:00; die 1H bleibt den ganzen Tag bearisch). `trigger()` findet die 5m-EMA-Crosses des 13.10. nach 15:30 zu den Zeiten 15:35↑, 16:50↓, 17:10↑, 17:15↓, 18:00↑, 19:15↓, 19:35↑, 19:45↓ (als Crosses, unabhaengig davon, ob die Richtung sie zulaesst).

**K3 Start-Update-Baustein (0,3 Tag).**
- `scripts/kern_start.cjs --datum <D> [--blackout HH:MM-HH:MM ...] [--testtag fiktiv]`
- Das Skript friert die Level ein (Regel 3), berechnet die Schock-Referenz, legt `kern_state.json` fuer den Tag an (testtag_modus, blackout, Zaehler, slListe leer) und druckt die Tagesfenster aus `kern_zeit`.
- Ein zweiter Lauf am selben Tag ist nur mit `--neu-grund` erlaubt.
- Akzeptanz:
  - Mit `level_register.json.bis2026-10-13.bak` als Eingabe entstehen 7 Level-Felder.
  - Die Datei ist byte-identisch bei Wiederholung.
  - `testtag_modus` kann nur `fiktiv` sein. Jeder andere Wert ergibt Exit 1 mit dem Hinweis "Echtgeld nur nach Levi-GO, Konstante in kern_config".

**K4 Voll-Check + Gate (1,0 Tag).**
- `scripts/kern_check.cjs` laeuft alle 5 Min.
  - Eingaben: keine Pflicht-Zahlen. Das Skript liest die vom Loop gespeicherten Bars `scripts/nas100_5m.json`, `nas100_15m.json`, `nas100_60m.json`, `qqq_15m.json` (Frische ≤ 6 Min, sonst Exit 1 "Bars veraltet").
  - Optional: `--position offen|keine` (sonst aus `kern_state`) und `--notiz "<Satz>"`.
  - Ausgabe ≤ 10 Zeilen: Zeit, Richtung mit drei Beinen, 5m-Lage, ATR, Trigger ja/nein mit Typ, SL-Vorschau mit R/ATR und Cap-Status, Sperre/Schock, Tageszaehler, **Handlung** (`GATE JETZT` / `warten` / `kein Trade: <Grund>`).
  - Log: `kern_check_log.jsonl` mit ts, vc_nr (Slot-Nummer aus kern_state), richtung, beine, ema/atr-Werte, trigger, swing, sl_vorschau, cap, sperre, schock, position, handlung, bars_hashes. Zusaetzlich Append in `scripts/kern_archiv/<datum>.txt`; kein /tmp-Redirect mehr noetig.
  - Beim ersten Check des Tages zusaetzlich die Pruefzeile "EMA Chart vs. Bars", wenn `--ema-chart-15m/--ema-chart-1h` uebergeben werden. Abweichung > 5 Pkt (15m) bzw. > 10 Pkt (1H) ergibt WARNUNG.
- `scripts/kern_gate.cjs` laeuft beim Trigger.
  - Pflicht: `--dir`, `--entry-live`, `--spread-pkt` (oder `--bid --ask --hebel`; ohne Werte `--grund-spread "<Text>"`).
  - Optional: `--notfall`, `--notiz`. **Hoechstens 8 Argumente.**
  - Alles andere (Swing, ATR, SL, R, TP1, TP2, Gates, Tageslimits, Re-Entry, 8c2) rechnet das Skript selbst aus Bars und `kern_state`.
  - Ausgabe: ENTRY-KASTEN mit hoechstens 12 Zeilen.
  - Log: `kern_gate_log.jsonl` mit ts, trigger_ref, dir, entry_live, entry_kerze_open_ts, anker, atr, sl, R, R_atr, tp1, tp2, be, spread_pkt, spread_anteil, gates{cap, spread(hart|schatten), c8c2, sperre, schock, limit, position, reentry}, status PASS/FAIL, groesse_info (size.cjs), schatten{alt, q_score, adx, effizienz}.
  - Bei PASS: Eintrag in `kern_trades_fiktiv.jsonl` (status offen), `kern_state.position = offen`.
- **Schatten "alt haette":** `scripts/kern_alt_schatten.cjs` (Modul) ruft `node scripts/gate_check.cjs --dry-run --json` mit den Werten auf, die Kern kennt (Entry, SL, TP1 = naechstes Register-Level in Richtung, Anker, ATR, EMA50-5m, `--blackout`), und uebergibt fuer nicht gemessene A3-Felder `--grund-<feld> "kern-schatten"`. Ergebnis: `{status, q_ampel, fail: [...]}` oder `{status: "nicht bewertbar", grund}`. Timeout 10 s.
- Akzeptanz:
  - (a) Ein Gate-Aufruf hat ≤ 8 Argumente, ein Check-Aufruf ≤ 4.
  - (b) Ein Trockenlauf mit `--jetzt-override` gegen die datierten Bars vom 13.10. 19:36 (Short) ergibt Richtung SHORT, Trigger vorhanden, ein Cap-Status und eine Schatten-Zeile.
  - (c) Drei `kern_gate`-Laeufe mit Schatten veraendern die sha1 von `gate_check_log.jsonl`, `kombi_fiktiv_log.jsonl`, `skipped_setups_fiktiv.jsonl`, `q2_schatten_log.jsonl` und `q4_schatten_log.jsonl` **nicht**. Das wird per Test mit Hash vorher/nachher geprueft. Schreibt `--dry-run` doch etwas, wird der Schatten auf "nicht bewertbar: gate_check schreibt im dry-run" gestellt und gemeldet. **Niemals alte Logs veraendern.**
  - (d) Bei `--entry-live` ausserhalb des Entry-Fensters ist das Ergebnis FAIL `sperre-zeit`.

**K5 Positions-Tick (0,4 Tag).**
- `scripts/kern_position_tick.cjs` laeuft jede Minute bei offener Position.
  - Liest `kern_state` und 1m-/5m-Bars `scripts/nas100_1m.json` bzw. `nas100_5m.json`.
  - Optional: `--kurs` und `--teilverkauf ja` (bestaetigt TP1-Ausfuehrung).
  - Ausgabe 3 Zeilen: SL 🔴 / TP1 🟢 / TP2 🟢, Abstand in R, Handlung (`TP1 erreicht → 50 % + SL auf BE x` / `Zeit-Exit faellig` / `halten`).
  - Log: `kern_position_log.jsonl` und `last_kern_position.json` (Format kompatibel zu `loop_stopp.cjs --tick`).
- `quick_tick.cjs` wird ohne offene Position **nicht** aufgerufen. Wer die Minutenzeile will: `--log scripts/kern_quick_tick_log.jsonl`.
- Akzeptanz: Ein Replay des 08.10.-18:45-Falls aus den 5m-Bars meldet TP1 und TP2 in der 18:45/18:50-Kerze, ein Replay 05.10. 16:55 meldet um 19:58 "Zeit-Exit faellig".

**K6 Nachtrag + Tagesabschluss + Bilanz (1,0 Tag).**
- `scripts/kern_nachtrag.cjs --datum <D>` (Konvention O2):
  - Entry = Eroeffnung der naechsten 5m-Kerze.
  - Position belegt bis Ende der Exit-Kerze.
  - Entry-Kerze und Exit-Kerze intrabar mit `nas100_1m_<datum>.json` (beim Tagesabschluss `data_get_ohlcv` timeframe 1, count 360). Fehlt die Datei, steht `intrabar: "nicht geprueft"`.
  - R zweimal: `r_spread2` (2 Pkt) und `r_spread_gemessen`, dazu MFE/MAE, Haltedauer, Ausgangsklasse, `manuell: false|true`, `p_flag` (TP1 vor SL), `q_flag` (TP2 nach TP1).
  - Schreibt atomar in `kern_trades_fiktiv.jsonl`; eine Wiederholung ist byte-identisch.
- `scripts/kern_bilanz.cjs --datum <D>` (Tagesabschluss) und `--auswertung` (mehrere Tage):
  - Slot-Abdeckung 15:35-20:00, also ≥ 67 % fuer einen gueltigen Tag.
  - Bars lueckenlos.
  - Tagestyp nach Effizienz |Close 20:00 − Open 15:30| / Range 15:30-20:00: ≥ 0,55 Trend, sonst Range; wird vor dem Ergebnis gedruckt.
  - **Technik-Kriterien je Tag binaer:**
    - (a) 0 Exit-1 in `kern_check`/`kern_gate` wegen fehlender Pflichtfelder
    - (b) Trigger-Abgleich: alle aus den Bars per `kern_regeln.trigger()` nachgerechneten zulaessigen Trigger haben einen `kern_gate`-Lauf im Slot ±1, und jeder Lauf hat einen Trigger (100 %)
    - (c) jeder Trade mit `intrabar` ≠ "nicht geprueft"
    - (d) 100 % Zeit-Exit bei offenen Positionen
    - (e) 100 % Gates mit Spread-Angabe
    - (f) 0 Exits ausserhalb Regel 7
  - Kennzahlen: n, p, q, c (Ø Spread/R), Anteil Zeit-Exits, Anteil manueller Exits, Ø R (spread2 / gemessen), Ø R ohne besten Tag, getrennt nach Trend/Range, Schatten "alt haette" (PASS/FAIL/ROT-Verteilung).
  - `--auswertung` meldet: Anzahl gueltiger Tage, "Technik-Kriterien an 5 gueltigen Tagen inkl. ≥ 1 Rangetag erfuellt: ja/nein", **Abbruch-Kriterien**: ≥ 3 Tage mit Tagesstopp −2R in 5 gueltigen Tagen; ≤ 3 Trades in 5 gueltigen Tagen; Ø Spread > 10 % von R; nach 10 Tagen Technik nicht erfuellt.
  - **Kein Echtgeld-Hinweis, keine automatische Freigabe.** Der Text lautet "Zwischenanalyse durch Opus faellig" bzw. "Fiktivtest beendet".
- Tagesabschluss-Reihenfolge:
  1. Bars datiert sichern (5m/15m/60m NAS, 15m QQQ, 1m NAS).
  2. `kern_nachtrag`
  3. `kern_bilanz --datum`
  4. `loop_stopp.cjs --state scripts/kern_state.json --tick scripts/last_kern_position.json --log scripts/kern_loop_stopp_log.jsonl --fiktiv-log scripts/kern_trades_fiktiv.jsonl`. Funktioniert das wegen Feldnamen nicht, schreibst du `kern_stopp.cjs` (ca. 60 Zeilen) mit derselben Logik. **`loop_stopp.cjs` nicht aendern.**
- Akzeptanz:
  - (a) `kern_nachtrag` auf einer Testdatei mit den 7 Faellen aus K2 liefert dieselben Werte.
  - (b) `kern_bilanz` auf einem synthetischen Tag mit fehlendem Gate-Lauf meldet Kriterium (b) = nein.
  - (c) Eine zweimalige Ausfuehrung ist byte-identisch.

**K7 Kern-Loop-Prompt (0,5 Tag).**
- `scripts/kern_loop_prompt.cjs [--datum <D>]` druckt den vollstaendigen Cron-Prompt (**< 120 Zeilen**, eingebettet; der Prompt wird nicht ueber einen Datei-Zeiger nachgeladen).
- Inhalt des Prompts:
  1. Bei jedem Fire bare `date`.
  2. Minute % 5 == 0: Bars speichern (`data_get_ohlcv` NAS100 5m/15m/60m(500) + QQQ 15m mit `save_path`), dann `node scripts/kern_check.cjs`, Ausgabe woertlich.
  3. Bei `GATE JETZT`: Bid/Ask des Produkts ablesen, `node scripts/kern_gate.cjs --dir … --entry-live … --spread-pkt …`, den ENTRY-KASTEN woertlich ausgeben.
  4. Bei offener Position jede Minute `kern_position_tick.cjs`.
  5. Sonst keine Minutenausgabe ausser "—".
  6. Terminal 20:02 (auch im DST-Fenster): Tagesabschluss K6, danach Loop-Stopp.
  7. Regel-Freeze-Satz: "Keine Regel, Schwelle oder Konstante aendern; Ideen ins Backlog `kern_backlog.md` mit Datum."
- **Entfaellt gegenueber dem alten Prompt:** MTF-Vierschritt mit Tool-Ablesung, alle `--grund-*`-Pflichten, Tweet-Raster alle 10 Min (nur noch Kalender beim Start-Update plus ein X-Check vor jedem Gate), Register-Pflege/B5, Fib, `--stale-n`, `--cluster-level`, `--sl-anker`-Handwerte, Anker-Reset, Q-Score-Eingaben, 13.1/Chasing/Punkt 11, Zeitbox, Kurzblock-Zeremonie, `vc_nr`/`tmp`-Redirect, Kombi-/Skipped-Nachtraege.
- **Bleibt:** Start-Update (Kalender, Tweets, Register-Pflege fuer die Level-Liste, Screenshot), `kern_start`, Cron-Raster 1 Min ab 15:20 (DST 14:20), Terminal 20:02, Screenshot nur bei `GATE JETZT` und um 20:00.
- Akzeptanz: Der Prompt hat < 120 Zeilen, enthaelt kein Pflichtargument ausser den in K4 genannten und erwaehnt kein Echtgeld.

**K8 Tests (0,5 Tag, parallel zu K1-K7).**
- Neu ist `tests/kern.test.js` (ca. 40 Tests): K1-Zeitfenster, K2-Fixtures, Trigger 13.10., Gates (Cap, Floor, Spread-Schatten, 8c2, Re-Entry, Tageslimit, Sperre, Schock-Grenze), K3-Idempotenz, K4-Argumentgrenzen und Hash-Unveraenderlichkeit alter Logs, K5-Replays, K6-Bilanz-Kriterien, K7-Zeilenzahl.
- Alle Tests laufen im Sandbox-Verzeichnis (Muster `tests/trading_scripts.test.js`) und schreiben nie in `scripts/*.jsonl`.
- `package.json`: `tests/kern.test.js` in `test`, `test:unit` und `test:all` aufnehmen (sonst nichts aendern).
- Aufruf ausschliesslich: `node --test tests/kern.test.js tests/trading_scripts.test.js tests/tagesmomente.test.js`.
- **Kein `npm run test:e2e`.**
- Akzeptanz: Alle drei Dateien sind gruen, die bestehenden 246 Tests sind unveraendert gruen.

**K9 Abschluss (0,2 Tag).**
- Neues Memory `regelwerk_kern_2026-10.md` mit den 12 Regeln, Dateiliste, Ablauf, Rollback und Freeze-Liste.
- In `feedback_live_trading.md`, `feedback_chartanalyse.md` und `feedback_trading_zeitfenster.md` **nur** einen Kopfvermerk ergaenzen: "Gilt nicht im Kern-Testbetrieb ab <Datum>, siehe regelwerk_kern_2026-10.md". Nichts loeschen.
- sha1-Abgleich mit K0.
- Bericht "bereit zum Commit" mit Dateiliste, Zeilenzahlen und Testergebnis.
- **Nicht committen.**

## 4. Grenzen

- Kein git add/commit/push.
- Keine Aenderung an bestehenden Logdateien und an alten Skripten ausser dem DST-Fix in `vollcheck.cjs` und der Testliste in `package.json`.
- Kein CDP-Zugriff ausser fuer Bars-Testabrufe, falls noetig (lieber datierte Dateien nutzen).
- Kein `test:e2e`.
- Kein Echtgeld-Pfad und kein Modus `echt` im Kern.
- Keine Rueckfragen an Levi waehrend der Umsetzung. Unklares fuehrst du als "offener Punkt" im Bericht auf und waehlst die im Auftrag genannte Default-Variante.
- Rollback: alter Loop-Prompt (`node scripts/loop_prompt.cjs`). Die Kern-Dateien bleiben liegen.

## 5. Zeitplan

- K1 (DST) **bis spaetestens Mo 19.10.2026**, auch wenn der Rest spaeter fertig wird.
- K0-K9 in ca. 4-5 Arbeitstagen.
- Danach Opus-Gegencheck (Prompt im Anhang), Levi-Freigabe und Commit durch Levi.
- **Der erste Kern-Fiktivtag ist fruehestens der Handelstag nach dem Opus-Gegencheck.** Liegt er im DST-Fenster 26.-30.10., gelten die `kern_zeit`-Fenster (Start 14:30).

## 6. Testtag-Betrieb (was gilt, was entfaellt)

- **Gilt sinngemaess:**
  - Start-Update vor dem Loop (Kalender, Sperrfristen, X-News, Register-Pflege, Screenshot) + `kern_start.cjs`.
  - Loop-Cron im 1-Min-Raster.
  - Voll-Check im 5-Min-Slot.
  - Terminal 20:02.
  - Tagesabschluss mit datierten Bars.
  - Opus-Analyse nach dem Tag (kurz: Technik-Kriterien und Auffaelligkeiten).
- **Entfaellt:** alle in K7 gelisteten alten Pflichten, `tagesmomente --datum`, `kombi_fiktiv --nachtrag`, `skipped_fiktiv --nachtrag`, `protokoll_bilanz`, `loop_archiv`.
- **Neu:**
  - Bid/Ask je Gate.
  - 1m-Bars zum Tagesabschluss.
  - `kern_bilanz`.
  - Regel-Freeze der Kern-Regeln ab Fiktivtag 1 (nur Fehlerkorrekturen, jede dokumentiert mit Opus-Kurzcheck).
- Ende des Tests: spaetestens nach 10 gueltigen Fiktivtagen oder sobald `kern_bilanz --auswertung` "Technik-Kriterien an 5 gueltigen Tagen inkl. ≥ 1 Rangetag erfuellt: ja" meldet. Dann kommt die **Opus-Zwischenanalyse**, und Levi entscheidet frei. **Es gibt keinen automatischen Uebergang ins Echtgeld.**

## 7. Entwurf fuer spaeter (gilt nur, falls Levi nach der Zwischenanalyse GO gibt; NICHT umsetzen)

- Vor einem GO: Spread-/Produktvergleich (KO-Schein vs. CFD/Mini-Future, Ziel ≤ 3 Pkt), Spread-Gate 10 % wird hart.
- Echtgeld-Stufen: 10 Trades mit 37,50 € Risiko, danach 75 € bis Trade 40, Fill-Protokoll je Trade.
- Abbruch: −8 R oder −400 €, Ø ≤ −0,3 R nach 15 Trades, ≥ 2 Exits ausserhalb Regel 7 in 10 Trades.
- Auswertungen: nach Trade 20 "kein Schaden", nach Trade 40 gilt eine Kante als vorlaeufig belegt bei Ø ≥ +0,10 R und ohne besten Tag ≥ 0.

> **Ende des kopierbaren Prompts.**

---

## Anhang A — Opus-Gegencheck-Prompt (nach Fables "bereit zum Commit")

> Du bist Opus, Pruefer. Repo `C:\Users\umnus\tradingview-mcp`, Memory-Ordner wie oben. Pruefe Fables Umsetzung des Auftrags `fable_auftrag_kern_regelwerk_2026-10-14.md` gegen die 12 Regeln in `opus_endurteil_regel_lockerung_2026-10-14.md`. Nur lesen und auf einer Kopie des `scripts`-Ordners in `%TEMP%` rechnen. sha1 der Originale vorher und nachher. Kein git, kein test:e2e; `node --test` nur mit Pfadliste `tests/kern.test.js tests/trading_scripts.test.js tests/tagesmomente.test.js`.
>
> Pruefe:
> 1. K0-Hashes: Alte Skripte und Logs sind unveraendert (ausser `vollcheck.cjs` DST und `package.json`).
> 2. DST: `kern_zeit` fuer 13.10./27.10./02.11.; Diff von `vollcheck.cjs` ≤ 10 Zeilen.
> 3. Jede der 12 Regeln: Wo steht sie im Code, und ist sie exakt so umgesetzt (Trigger Cross/Gate-Bildung, Swing-Definition, Floor/Cap, Spread-Schatten im Fiktivbetrieb, BE ± Spread, Re-Entry, belegt bis Ende Exit-Kerze, Zeit-Exit, Schock-Median)?
> 4. Die 7 K2-Fixtures selbst nachrechnen (Toleranz 0,02 R).
> 5. `kern_regeln.trigger()` auf 13.10. gegen die 8 EMA-Crosses und den Richtungs-Zeitstrahl.
> 6. Ein Replay `kern_check`/`kern_gate`/`kern_nachtrag`/`kern_bilanz` fuer 05.10., 08.10. und 13.10. aus den datierten Bars: Anzahl Trigger, Trades, Ausgangsklassen, Σ R plausibel zur Opus-Gegenrechnung (ausfuehrbar: Ø etwa 0 bis +0,1 R; Abweichungen erklaeren).
> 7. Schatten "alt haette": `gate_check --dry-run` schreibt nachweislich nichts (Hash).
> 8. Argumentzahl (Gate ≤ 8, Check ≤ 4), Prompt < 120 Zeilen, kein Echtgeld-Pfad, `testtag_modus` nur fiktiv.
> 9. Tests gruen, 246 alte unveraendert.
> 10. Rollback-Probe: Der alte `loop_prompt.cjs` laeuft unveraendert (byte-identische Ausgabe zur Vorher-Kopie bei gleichem Datum).
>
> Ausgabe: `opus_gegencheck_kern_regelwerk_<datum>.md` mit FREIGEGEBEN / MIT AUFLAGEN / NICHT FREIGEGEBEN und einer Auflagenliste. Kein Commit.

## Anhang B — offene Punkte, die Levi vor dem Start entscheiden sollte (Defaults im Auftrag)

- **L1 Spread-Gate im Fiktivtest:** Default ist Schatten (rechnen und loggen, nicht blockieren). Alternative: schon fiktiv hart mit dem gemessenen Schein-Spread. Empfehlung Opus: Schatten, weil der Fiktivtest den Trigger messen soll und nicht das Produkt.
- **L2 EMA-Quelle:** Default sind die Bars (deterministisch, nachrechenbar) mit taeglicher Chart-Pruefzeile. Die Alternative waeren Chartwerte als Pflichteingabe, was Argumente und Fehler zurueckbringt. Empfehlung: Bars.
- **L3 Produkt fuer die Bid/Ask-Protokollierung im Fiktivtest:** welcher Schein bzw. welches Produkt jeweils abgelesen wird. Ohne Angabe wird der zuletzt real gehandelte KO-Schein-Typ verwendet.
- **L4 Erster Kern-Fiktivtag:** Ohne Entscheid gilt der erste Handelstag nach Opus-Freigabe; Levi muss ihn nur bestaetigen (Cron/Remote Control vorher an, siehe `feedback_testtag_start_verlaesslichkeit.md`).
