---
name: fable_umsetzung_kern_regelwerk_2026-10-14
description: "Fable-Abschlussbericht 14.10.2026 zur Umsetzung des Kern-Regelwerks (Auftrag fable_auftrag_kern_regelwerk_2026-10-14, Variante B, K0-K9): 14 neue scripts/kern_*.cjs (1.657 Zeilen) + tests/kern.test.js (44 Tests) + DST-Fix vollcheck.cjs (9 Zeilen) + package.json; Tests 290/290 gruen (246 alte unveraendert); K0-sha1 81 Dateien, nur vollcheck.cjs/package.json abweichend; 7 K2-Fixtures innerhalb +/-0,02 R, 13.10.-Zeitstrahl und 8 Crosses exakt; Abweichungen: Schatten in isolierter Temp-Kopie (gate_check --dry-run schreibt trigger_kandidaten_log), Akzeptanz K4(b) um 19:31 statt 19:36 (Trigger am 15m-Schluss 19:30), kern_stopp statt loop_stopp --fiktiv-log; NICHT committet; bereit zum Commit; Opus-Gegencheck (Anhang A) offen; Echtgeld nirgends."
metadata:
  type: project
  originSessionId: fable-kern-regelwerk-umsetzung-2026-10-14
  modified: 2026-10-14T15:41:06.591Z
---

# Fable-Umsetzung Kern-Regelwerk — 14.10.2026 (Auftrag [[fable_auftrag_kern_regelwerk_2026-10-14]], Levi-Freigabe 14.10.)

**Status: K0-K9 umgesetzt, 290/290 Tests gruen, NICHT committet (Commit/Push nur Levi), Opus-Gegencheck (Anhang A) ausstehend.** Regelwerk-Memo: [[regelwerk_kern_2026-10]]. Abschnitt 7 des Auftrags (Echtgeld-Entwurf) NICHT umgesetzt; im Kern-Code gibt es nur `testtag_modus = fiktiv` (`TESTTAG_MODI = ['fiktiv']`), jeder andere Wert = Exit 1 "Echtgeld nur nach Levi-GO, Konstante in kern_config".

Arbeitsweise: kein git add/commit/push, kein `npm run test:e2e`, kein `npm test`; `node --test` nur mit der Pfadliste `tests/kern.test.js tests/trading_scripts.test.js tests/tagesmomente.test.js`; kein CDP, kein X-Aufruf; alle Trockenlaeufe in Sandbox-Kopien unter `%TEMP%\...\scratchpad\sb*` bzw. in den Test-Sandboxes — unter `scripts/` sind nur Code-Dateien neu, keine `kern_*`-Laufzeitdateien (geprueft). MEMORY.md nicht angefasst.

## 1. Paket-Status

| Paket | Status | Kern |
|---|---|---|
| K0 Sicherung | fertig | `scripts/_freeze_alt_2026-10-14.sha1` (81 Dateien: scripts/*.cjs, analyse/*.cjs, *.jsonl, level_register.json, vollcheck_state.json, tests/*.js, package.json). Abgleich am Ende: **nur `scripts/vollcheck.cjs` und `package.json` abweichend** (`sha1sum -c --quiet` → genau diese 2 FAILED) |
| K1 Zeit + DST | fertig (Frist 19.10. eingehalten) | `kern_zeit.cjs` (jetzt/deParts/istDivergenz per ICU shortOffset/fenster/deZeitMs); `vollcheck.cjs` Z. 750-756 (die Stelle liegt heute bei Z. 751-753, nicht 401-404 wie im Memo): Order-Sperre/Halbierung aus `kern_zeit.fenster(jetztMs).sessionStartMin`; Diff 9 Zeilen (3 ersetzt, 3 neu, 3 Kommentar) |
| K2 Regel-Kern | fertig | `kern_regeln.cjs` (reine Funktionen), `kern_config.cjs` (Konstanten), dazu `kern_lage.cjs` (Marktlage aus Bars, gemeinsam fuer Check/Gate/Bilanz) |
| K3 Start-Update | fertig | `kern_start.cjs` |
| K4 Check + Gate + Schatten | fertig | `kern_check.cjs`, `kern_gate.cjs`, `kern_alt_schatten.cjs`, Hilfsmodul `kern_common.cjs` |
| K5 Positions-Tick | fertig | `kern_position_tick.cjs` |
| K6 Nachtrag/Bilanz/Stopp | fertig | `kern_nachtrag.cjs`, `kern_bilanz.cjs`, `kern_stopp.cjs` (loop_stopp.cjs unveraendert, s. Abweichung 3) |
| K7 Loop-Prompt | fertig | `kern_loop_prompt.cjs`: 59 Zeilen (48 nicht leer), Selbsttest |
| K8 Tests | fertig | `tests/kern.test.js` 44 Tests; `package.json` test/test:unit/test:all + `tests/kern.test.js` |
| K9 Abschluss | fertig | `regelwerk_kern_2026-10.md` neu; Kopfvermerk in `feedback_live_trading.md`, `feedback_chartanalyse.md`, `feedback_trading_zeitfenster.md` (je 1 Zeile oben, nichts geloescht); dieser Bericht |

## 2. Dateiliste (bereit zum Commit)

Neu (Zeilen): `scripts/kern_zeit.cjs` 99 · `scripts/kern_config.cjs` 60 · `scripts/kern_regeln.cjs` 306 · `scripts/kern_common.cjs` 133 · `scripts/kern_lage.cjs` 67 · `scripts/kern_start.cjs` 131 · `scripts/kern_check.cjs` 123 · `scripts/kern_gate.cjs` 163 · `scripts/kern_alt_schatten.cjs` 95 · `scripts/kern_position_tick.cjs` 81 · `scripts/kern_nachtrag.cjs` 84 · `scripts/kern_bilanz.cjs` 170 · `scripts/kern_stopp.cjs` 39 · `scripts/kern_loop_prompt.cjs` 106 · `tests/kern.test.js` 409 · `scripts/_freeze_alt_2026-10-14.sha1` 81. Summe Code 1.657 Zeilen kern_*.cjs (Opus-Schaetzung ~2.800).
Geaendert: `scripts/vollcheck.cjs` (+6/−3), `package.json` (3 Zeilen Testliste).
Memory: neu `regelwerk_kern_2026-10.md` (72 Z.), `fable_umsetzung_kern_regelwerk_2026-10-14.md` (dieser); Kopfvermerk in 3 Memos. MEMORY.md unveraendert (Index pflegt Sonnet).

## 3. Akzeptanz je Paket (binaer, Befehl → Ergebnis)

**K0** · Datei existiert (81 Zeilen) ✓ · `sha1sum -c --quiet scripts/_freeze_alt_2026-10-14.sha1` → nur `package.json: FAILED`, `scripts/vollcheck.cjs: FAILED` ✓ (auch `trigger_kandidaten_log.jsonl`, `gate_check_log.jsonl`, `level_register.json` identisch).
**K1** · Unit-Tests 13.10. (Normal 15:30/15:35-19:00) ✓ · 27.10. (Divergenz 14:30/14:35-19:00, Zeit-Exit 20:00, Terminal 20:02) ✓ · 02.11. Normal ✓ · 30.03.2026 Normal ✓ (+16.03.2026 Divergenz, 28.10. Divergenz, 01.11. 22 Z keine) · `tests/trading_scripts.test.js` 217 Tests gruen ✓ (vollcheck-Logik auf Normaltagen identisch: 15:00-15:29/15:30-15:59/16:00-16:04).
**K2** (Spread 2, Horizont 20:00, `kern_regeln.simuliere` auf `nas100_5m_<datum>.json`):

| Fall | Erwartung | Ergebnis | Δ |
|---|---|---|---|
| 30.09. Long 15:35 @30493,75 SL 30380 | TP1+BE +0,48 (Exit 19:25) | **TP1+BE +0,49**, Exit 19:25, TP1 16:05 | 0,01 |
| 05.10. Long 16:55 @30984,45 SL 30925,15 | TP1+ZEIT +0,96 | **TP1+ZEIT +0,96**, TP1 19:40 | 0,00 |
| 08.10. Short 18:45 @30952,75 SL 31033,90 | TP1+TP2 +1,50 (Exit 18:50) | **TP1+TP2 +1,50**, TP1 18:45, TP2 18:50 | 0,00 |
| 08.10. Short 18:00 @30950,65 SL 31017,50 | SL −1,00 (Exit 18:15) | **SL −1,00**, Exit-Kerze 18:15 | 0,00 |
| 13.10. Short 15:35 @30424,65 SL 30476,90 | SL −1,00 Entry-Kerze | **SL −1,00**, Exit-Kerze 15:35 | 0,00 |
| 13.10. Short 17:05 @30487,55 SL 30589,20 | ZEIT −0,11 | **ZEIT −0,13** (Rueckkauf am Ask 30498,55 + 2) | 0,02 (Toleranzgrenze) |
| 05.10. Long Trigger 15:30-Kerze, Anker 30752,8 | Cap-FAIL (≈7×ATR) | **FAIL cap**: Entry 30905,15, ATR 29,22, R 166,96 = **5,71× ATR** | FAIL ✓; ATR-Basis anders (Opus: ATR ≈22,7 vor der 15:30-Kerze; Kern: ATR nach Schluss der 15:30-Kerze mit 114 Pkt Range) |

· 8 EMA-Crosses 13.10. nach 15:30: **exakt** 15:35↑ 16:50↓ 17:10↑ 17:15↓ 18:00↑ 19:15↓ 19:35↑ 19:45↓ ✓ · Richtungs-Zeitstrahl 13.10.: Wechsel **exakt** 15:45, 16:00, 16:15, 17:00, 18:15, 18:30, 19:00, 19:30, 19:45, 20:00; 1H an allen 19 Stichzeiten bearisch; 2/2 LONG (16:00, 18:30) = "keine" ✓ (15m-/QQQ-/1H-EMA-Werte in `tests/kern.test.js` protokolliert, z.B. 19:30: 15m 30482,95 < 30505,30; QQQ 741,51 < 742,94; 1H 30520,05 < 30666,49).
**K3** · `kern_start --datum 2026-10-13 --register level_register.json.bis2026-10-13.bak` → 7 Felder PDH 31270,1 | PDL 30902,65 | PP 31110,85 | R1 31319,05 | S1 30951,6 | ON_HOCH 31126,35 (Register `Session-Hoch`) | ON_TIEF 30552,45 ✓ · Wiederholung (mit `--neu-grund`) sha1 `b0553edd…` = `b0553edd…` byte-identisch ✓ · zweiter Lauf ohne `--neu-grund` Exit 1 ✓ · `--testtag echt` → Exit 1 "Echtgeld nur nach Levi-GO, Konstante in kern_config" ✓ · Schock-Referenz aus 10 datierten Tagen (25.09.-08.10.), Median 28,9; `--event 14:30:CPI` → Fenster 14:15-15:00 ✓.
**K4** · (a) Gate-Aufruf 3 bzw. 5 Argumente, 9 Argumente → Exit 1; Check-Aufruf 2 Argumente (max. 4) ✓ · (b) Trockenlauf 13.10. (datierte Bars, `--jetzt-override`): um **19:31** Richtung SHORT (2/2 short, 1H bearisch), Trigger JA gate-bildung (15m-Schluss 19:30, Richtung vorher keine), Entry 30482,95 (19:30-Open), Swing 30523,35 (19:05 H), SL 30533,93, R 50,98 = 2,41× ATR **Cap ✓**, Schatten-Zeile vorhanden ✓ — um **19:36** korrekt "Trigger: nein" (s. Abweichung 2) · (c) 3 Gate-Laeufe mit Schatten: sha1 von gate_check_log, kombi_fiktiv_log, skipped_setups_fiktiv, q2_schatten_log, q4_schatten_log **und trigger_kandidaten_log** vorher = nachher ✓ (Test + Sandbox) · (d) Entry-Kerze 19:30 ausserhalb 15:35-19:00 → **FAIL sperre-zeit** ✓ · PASS-Fall 17:02 (Gate-Bildung 16:55, Entry 30484,85, SL 30590,19, R 105,34 = 2,89× ATR) → Trade `2026-10-13_1702_short` offen, Zaehler 1/3; zweites Gate bei offener Position FAIL position ✓ · Spread aus bid 5,50/ask 5,51/hebel 6 bei Kurs 30482,15 = 9,24 Pkt = 18,1 % von R → "Schatten: waere FAIL", blockiert nicht ✓ · Bars veraltet → Exit 1 ✓ · ohne kern_state → Exit 1 ✓.
**K5** · Replay 08.10. 18:45 Short: Tick 18:46 "TP1 erreicht → 50 % + SL auf BE 30950,75" (18:45-Kerze); Tick 18:50:30 "TP2 erreicht → Rest schliessen (1,5 R)" (18:50-Kerze), belegt bis 18:55, Tages-R +1,5 ✓ · Replay 05.10. 16:55 Long: 19:45 TP1 erreicht (BE 30986,45), **19:58 "Zeit-Exit faellig → Rest (50 %) per Marktorder"**, 20:00:20 "Zeit-Exit ausgefuehrt (0,96 R)" ✓ · 13.10. 15:35 Short: SL in der Entry-Kerze → slListe [short 30476,9], letzterSl, Verluste 1 ✓ · Ausgabe 3 Zeilen, `last_kern_position.json` im loop_stopp-Format (ts/dir/entry/sl/tp1/tp2/kurs/geschlossen) ✓.
**K6** · (a) `kern_nachtrag` auf Testdatei mit den 6 Trade-Faellen (4 Tage): Klassen identisch, R innerhalb ±0,02, Entry = Eroeffnung der Entry-Kerze (`entry_korrigiert: true` bei Dummy-Entry), `intrabar: "nicht geprueft"` ohne 1m-Bars; mit synthetischen 1m-Bars `"geprueft"` ✓ · (b) synthetischer Tag 13.10.: Check 17:01 ohne Gate → "(b) NEIN — 1 zulaessige Trigger (16:55 gate-bildung short), ohne Gate-Lauf 1"; nach Gate 17:02 → "(b) ja" ✓ · (c) Nachtrag zweimal → sha1 identisch; Bilanz zweimal → stdout identisch ✓ · Tagestyp 13.10. Range (Effizienz 0,45), 05.10. Trend ✓ · `kern_stopp` Exit 2 bei offener Position, 0 nach Zeit-Exit, Exit 1 ohne `--grund` ✓ · `--auswertung`: Technik-Satz, Abbruch-Kriterien, Text ohne Echtgeld ✓ · `--notfall "<Grund>"` → Klasse NOTFALL, manuell true ✓.
**K7** · Prompt 59 Zeilen (< 120) ✓ · enthaelt nur die Argumente dir/entry-live/spread-pkt/bid/ask/hebel/grund-spread/notfall/notiz/position/ema-chart-15m/ema-chart-1h/kurs/teilverkauf/datum/event/blackout/grund/log/auswertung ✓ · kein "Echtgeld"/"echt" ✓ · Divergenztag 27.10. zeigt Loop 14:20, Start 14:30, Entry 14:35-19:00 ✓ · Selbsttest schlaegt bei Fremdargument/Echtgeld/≥120 Zeilen an ✓.
**K8** · `node --test tests/kern.test.js tests/trading_scripts.test.js tests/tagesmomente.test.js` → **tests 290, pass 290, fail 0** (Baseline vor der Umsetzung: 246/246 fuer die zwei alten Dateien; neu 44). Sandbox-Garantie: `after()` vergleicht sha1 aller echten `scripts/*.jsonl` + `level_register.json` vorher/nachher ✓.
**K9** · Memo neu ✓ · 3 Kopfvermerke ✓ · sha1-Abgleich ✓ · dieser Bericht ✓ · nicht committet ✓.

## 4. Abweichungen vom Auftrag (mit Grund)

1. **Schatten "alt haette" laeuft in einer isolierten Temp-Kopie von gate_check.cjs + Closure (atr_qqq, register_constants, trade_db)**, nicht als `node scripts/gate_check.cjs`. Grund (nachgewiesen, Sandbox 14.10. 12:5x): `--dry-run` schreibt den Beinahe-Trigger nach `trigger_kandidaten_log.jsonl` (gate_check.cjs Z. 4255: "auch im Dry-Run weiterhin … vorbestehender Stand"); die Auftragsvorgabe "NIEMALS alte Logs veraendern" wiegt schwerer. Zusaetzlich sha1-Vergleich von 6 Alt-Logs vor/nach jedem Schatten-Lauf → bei Abweichung "nicht bewertbar: gate_check schreibt im dry-run". Das Register fuer die Kopie wird aus der eingefrorenen Kern-Level-Liste gebaut (7 Level, `updated` = Systemzeit, weil der 90-Min-Frischeguard des alten Skripts gegen die Systemuhr misst); deshalb bekommt gate_check kein `--jetzt`, die Sperrfrist uebergibt der Kern vorentschieden als `none|aktiv`. TP1 alt = naechstes Kern-Level in Richtung, TP2 alt = naechstes Level jenseits TP1 (sonst `--grund-tp2-level-price`, dann Status immer UNKNOWN). Folge: ohne Register-Level in Richtung ist der Schatten "nicht bewertbar: kein Register-Level in …-Richtung" (im 13.10.-Trockenlauf der Fall, weil der `.bak` nur 08.10.-Level ueber dem Kurs traegt; Test mit synthetischem S1 30400 liefert FAIL (rrGate), Q 1/4).
2. **Akzeptanz K4 (b) um 19:31 statt 19:36.** Regel 2b ist ein Ereignis am 15m-Schluss (19:30; Opus-Bericht 13.10.: Lauf 19:32:19 "Dual-Gate wird 2/2 (15m 19:15)"), Entry-Kerze = 19:30-Open. Ein Check um 19:36 hat einen weiteren 5m-Schluss (19:30-Kerze) hinter sich → "kein Trigger" — sonst wuerde dieselbe Gate-Bildung an drei 5m-Schluessen je einen Trigger erzeugen (genau die Zustand/Ereignis-Vermischung aus der Fable-Stellungnahme B.4). Implementiert als `richtungVorher` = Stand zum vorherigen 5m-Schluss. Der 19:36-Aufruf ist als Negativprobe im Test (Check "Trigger: nein", Gate "FAIL (trigger, sperre-zeit)"). **Opus bitte bestaetigen oder kippen.**
3. **`kern_stopp.cjs` statt `loop_stopp.cjs --fiktiv-log`** (vom Auftrag ausdruecklich erlaubt): `kern_state.json` traegt `datum_de` + `letzter.position/jetzt/nr`, `pruefePosition()` aus loop_stopp.cjs wird als Modul genutzt; der Y3-c-Block von loop_stopp erwartet aber das Skipped-Schema und haette jeden Kern-Trade als "offen" gemeldet.
4. **Zusaetzliche Dateien** `kern_common.cjs`, `kern_lage.cjs`, `kern_exit_log.jsonl` (jeder Exit 1 eines kern_*-Skripts wird protokolliert — Quelle fuer Bilanz-Kriterium (a), das sonst nicht aus Logs zaehlbar waere).
5. **`kern_check --position`** ueberschreibt nur Anzeige/Handlung und warnt bei Widerspruch; den Zustand fuehren kern_gate (PASS/--notfall) und kern_position_tick (Auftrag: "sonst aus kern_state" — der State bleibt die Wahrheit).
6. **Entry-Notwert:** Liegt die Entry-Kerze beim Gate noch nicht in den Bars (Gate exakt zur vollen 5-Min-Grenze), rechnet kern_gate mit `--entry-live` und loggt `entry_quelle`; kern_nachtrag ersetzt ihn durch die Eroeffnung.
7. **Bilanz (b) "zulaessiger Trigger"** (Auftrag nennt keine Definition): Trigger nach Regel 2 + Richtung gueltig + Entry-Kerze im Fenster + Cap ok + keine laut Trades belegte Position + Tageslimit (3 Trades / 2 Verluste / −2 R aus den Trades) nicht erreicht + kein Schock-Tier laut Check-Log. Sperrfenster sind nicht aus den Logs rekonstruierbar → ein Trigger in der Sperrfrist zaehlt als zulaessig; der Gate-Lauf (FAIL sperre) ist dann der geforderte Lauf.
8. **Zeilen-Memo:** Die DST-Zeilen standen bei Z. 751-753 (Memo: 401-404); Fix 9 Diff-Zeilen (≤ 10).
9. **Testzahl 44** statt "ca. 40" (K8), ein Teil mit mehreren Asserts.

## 5. Bekannte Luecken / Hinweise

- Technik-Kriterium (c) ist bis zum ersten `nas100_1m_<datum>.json` (Tagesabschluss, `data_get_ohlcv` timeframe 1 count 360 — steht im Prompt) immer "NEIN"; 1m-Logik nur mit synthetischen Bars getestet.
- Endurteil-Testplan Punkt 4 ("kern_nachtrag zaehlt Momente, an denen das alte Werk ein Gate gefahren haette, das neue aber keinen Trigger hat") ist NICHT Teil der Auftrags-Pakete und nicht umgesetzt.
- `groesse_info` nutzt size.cjs nur, wenn `--bid/--ask/--hebel` (Ask = Zertifikatpreis) oder `kern_config.PRODUKT.hebel/zert` vorliegen; sonst EUR/Pkt-Formel mit Hinweis L3.
- Schock-Referenz: ATR(14) nach Schluss der Session-Start-Kerze (15:30 bzw. 14:30), also inkl. der ersten US-Kerze; beim 13.10. 23,9 vs. Median 28,9 (inaktiv). Opus' Endurteil nennt "um 15:35" — identische Lesart.
- Spread-Modell in `simuliere`: Long = Kauf am Ask (Kosten Spread/R einmal), Exits am Bid; Short = Exits am Ask (Bid-Marke um Spread verschoben), BE = Entry ∓ Spread realisiert 0. Beide Fixture-Abweichungen (30.09. +0,49; 13.10. 17:05 −0,13) kommen daher und liegen in der Toleranz.
- `kern_position_tick` ohne offene Position = Exit 1 (bewusst, laut Prompt "—" ausgeben); diese Exits zaehlen nicht in Kriterium (a) (nur kern_check/kern_gate).
- Keine Datei schreibt in alte Logs; `kern_archiv/<datum>.txt` ersetzt den /tmp-Redirect vollstaendig.

## 6. Was Opus im Gegencheck besonders pruefen sollte

1. Trigger-(b)-Semantik "einmal am 15m-Schluss" (`kern_lage.cjs` richtungVorher) gegen den Wortlaut von Regel 2b und die 19:31/19:36-Frage (Abweichung 2).
2. `kern_regeln.simuliere`: Spread-Modell, BE-Realisierung, Zeit-Exit zum Schluss der 19:55-Kerze (Nachtrag) vs. Live-Tick (erst ab 20:00 ausgefuehrt, 19:58 "faellig"), TP2 ab Kerze nach TP1, SL vor TP.
3. Swing-Definition nicht-strikt (≤/≥), `i+1 geschlossen`, Floor-Fallback ohne Swing.
4. Isolierter Schatten (Abweichung 1): Register-Nachbau aus 7 Level, fehlendes `--jetzt`, TP2-alt-Regel; ob "nicht bewertbar" bei fehlendem Level akzeptabel ist.
5. Bilanz (b)-Definition und Tagesstopp-Rechnung (gemessener Spread vor spread2).
6. vollcheck.cjs-Diff (9 Zeilen) auf Normaltagen identisch; Divergenztag-Verhalten (Order-Sperre 14:00-14:29, Halbierung 14:30-14:59).
7. Rollback-Probe: `loop_prompt.cjs` sha1 `9165617f…` unveraendert (Freeze-Datei).
8. K2-Cap-Fall: 5,71× statt ≈7× ATR — Ursache ATR-Stichzeit, Ergebnis FAIL unveraendert.

## Auflagen-Umsetzung 14.10. (Opus-Gegencheck [[opus_gegencheck_kern_regelwerk_2026-10-14]] A1-A13 inkl. Nachtrag A4; Levi-Freigabe 14.10.)

**Ergebnis:** alle Auflagen umgesetzt (A14, hartes Spread-Gate, Bid/Ask-Messung, Abbruch-Kriterium Spread bewusst NICHT — erst vor Echtgeld-GO, vermerkt in `regelwerk_kern_2026-10.md`). Tests `node --test tests/kern.test.js tests/trading_scripts.test.js tests/tagesmomente.test.js` → **298/298 gruen** (246 alte unveraendert, kern.test.js 52). K0-Abgleich: weiterhin nur `vollcheck.cjs` + `package.json` abweichend; erweiterte Liste (A9) 112/112 identisch. Keine Kern-Laufzeitdateien im Repo. Nicht committet.

| Auflage | Status | Datei:Zeile (Kern) | Test (tests/kern.test.js) |
|---|---|---|---|
| A1 Overnight aus Bars | erledigt | `kern_start.cjs:54-66` `overnightAusBars` (Nacht = Vortags-US-Close 22:00 DE / DST 21:00 → Session-Start −30 Min = 15:00 / 14:00), `:36-45` Register exakt nur PDH/PDL/PP/R1/S1 + Abgleich `Nacht-*`/Anzeige `Session-*`, `kern_config.cjs:21-30` | "A1: 05.10. Overnight-Tief = 30709,2 … NICHT 30794,35; 08.10. = 31222,7/30877"; K3-Test 13.10. → 30683,4/30386,6 |
| A2 Bilanz (b) = Check-Logik | erledigt | `kern_bilanz.cjs:60-92` (gleiche `R.gates()` mit Sperre/Schock aus Check-Log, 8c2/Re-Entry/Limit/Position aus Trades; "regelkonform blockiert"), `kern_check.cjs:47-49` `gatesVorschau`, Check-Log traegt `blackout`/`gates_vorschau`/`levels` (`kern_check.cjs:113-116`) | "A2: 08.10. mit CPI-Sperre 17:40 … 17:30 regelkonform blockiert"; 8c2/Re-Entry-Rekonstruktion aus Trades (Unit-Test gates 8c2/reentry bestehend) |
| A3 Exit vor Trigger-Pruefung | erledigt | `kern_check.cjs:66-71` ruft `positionAbrechnen` (export `kern_position_tick.cjs:24-50`) vor der Lage; Prompt `kern_loop_prompt.cjs:42-43` Tick vor Check; Tick-`--kurs`-Hinweis `kern_position_tick.cjs:57-61` | "A3: Exit in Kerze X … BE in der 18:15-Kerze"; Regel-9-Grenze `positionBelegtBisMs == entryMs` PASS |
| A4-Rest Annahme-Spread | erledigt | `kern_config.cjs:46-53` (`SPREAD_REFERENZ_PKT 2`, `_QUELLE`, `SPREAD_KO_SCHAETZUNG_PKT 9`), `kern_gate.cjs:65-76` `spreadBestimmen` (ohne Argument → ANNAHME, kein Exit 1), Kasten `:131`, Trade/State `:119/:123`, `kern_regeln.cjs:156-160` Schatten-Text ANNAHME, `kern_check.cjs:87` Handlung ohne Spread, `kern_bilanz.cjs:97` (e) n/a, `:106-111` c ANNAHME + KO-Schaetzzeile `:130`, `:148-153` Abbruch nur mit Messwerten, `kern_nachtrag.cjs:31-34` gemessen nur bei Messwert, Prompt `kern_loop_prompt.cjs:51-53` | "A4: kern_gate 17:02 OHNE Spread-Argument … ANNAHME"; Nachtrag `r_spread_gemessen null`/Messwert-Pfad; Bilanz "(e) n/a"; Prompt-Selbsttest verbietet `--spread-pkt` |
| A5 DST-Fenster | erledigt (Levi-Entscheid) | `kern_zeit.cjs:22-24` (Doku), Fenster 14:35-19:00 unveraendert; Prompt `kern_loop_prompt.cjs:23` | "A5: Entry-Fenster im DST-Divergenzfenster 14:35-19:00" |
| A6 Register-Frische | erledigt | `kern_start.cjs:123-133` (`updated` ≠ heute → Exit 1; mit `--register` Warnung) | "A6: Register nicht vom heutigen Handelstag → Exit 1" |
| A7 Spread ≤ 0 | erledigt | `kern_gate.cjs:68-73`, `kern_regeln.cjs:305-309` (`spreadPkt` null bei ask ≤ bid), `kern_nachtrag.cjs:60` | "(a)/A12/A7 Argumentgrenzen und Spread-Eingaben" |
| A8 Zeit-Exit-Prompt | erledigt | `kern_loop_prompt.cjs:60-62` ("NICHTS aufrufen … Tick ab 20:00 verbucht automatisch", `--notfall` nur einmal, nie fuer Zeit-Exit), Selbsttest `:96-99`; Tick-Text `kern_position_tick.cjs:55` | K7-Test (`--notfall` genau 1×, keine Zeit-Exit+notfall-Zeile) + Selbsttest-Negativprobe |
| A9 Freeze erweitert | erledigt | `scripts/_freeze_alt_2026-10-14_erweitert.sha1` (112 Dateien: trades.db, datierte Bars, .bak, last_*, setup_karte, x_last_fetch/x_watchlist, loop_archiv/*); K0-Liste unveraendert | "A9: erweiterte Freeze-Liste … stimmt" (sha1 aller Eintraege nachgerechnet) |
| A10 Rollback-Notiz | erledigt | `regelwerk_kern_2026-10.md` Abschnitt Rollback (kern_zeit.cjs nicht loeschen; mtime-Stempel loop_prompt) | — (Memo) |
| A11 Gap-Fill | erledigt | `kern_regeln.cjs:225-230` (Stop-Fill = schlechteres aus Open und Marke, `gap: true`) | "A11: Gap ueber den SL fuellt zum schlechteren Open" (Long/Short/BE/ohne Gap); 7 Fixtures + 3 Replays neu gelaufen: **alle unveraendert** (+0,49 / +0,96 / +1,50 / −1,00 / −1,00 / −0,13 / Cap-FAIL 5,71×) |
| A12 Kleinpunkte | erledigt | Check-Grenze `kern_check.cjs:53` (`argGrenze`, `kern_common.cjs:130-131`), Gate `kern_gate.cjs:82`; Nachtrag Cap/Floor neu `kern_nachtrag.cjs:44-46` (`cap_nachtrag`, Information); `dedupeBars` `kern_common.cjs:94-96` in allen Bars-Pfaden | "(a)/A12/A7 …" (Check 5 Argumente Exit 1), Nachtrag `cap_nachtrag` "CAP VERLETZT" bei ATR 20, dedupeBars-Unit |
| A13 Schatten / Testplan 4 | erledigt (minimal) | `kern_alt_schatten.cjs:90-96` (UNKNOWN = "nicht bewertbar" mit Gate-Liste), `kern_bilanz.cjs:73` Alt-Zustandsmomente (Zaehlung + Liste), Ausgabe `:131`, Auswertung `:166` | "A12/A13: … schattenText UNKNOWN"; Bilanz-Zeile "Alt-Zustandsmomente ohne Kern-Trade" |
| A14 size.cjs-Limit | NICHT umgesetzt (Levi: vor Echtgeld-GO) | Hinweis im Groessentext `kern_gate.cjs:35` | — |

**Abweichungen / Entscheide beim Umsetzen:**
- A1 Nachtfenster-Ende = Session-Start −30 Min (nicht Open): deterministisch unabhaengig vom Zeitpunkt des Start-Updates (15:00-15:20), entspricht der Order-Sperre. Register-Nachtwerte 08.10. werden exakt reproduziert (31222,7/30877), 13.10. = Opus-Werte (30683,4/30386,6). Fehlen Bars, steht null mit lautem HINWEIS (Regel 3), kein Abbruch.
- A11 "BE in der TP1-Kerze pruefen" (Opus-Nebenbefund) **nicht** umgesetzt: in der TP1-Kerze ist die Reihenfolge Entry-Seite/TP1 nicht entscheidbar; ein BE-Check dort haette den 08.10.-18:45-Fixture von +1,50 auf +0,50 gekippt (Entry-Kerze = TP1-Kerze, Open = Entry ≥ BE). BE und TP2 gelten ab der Kerze nach TP1; gemeldet statt verbogen.
- A2: Sperrfenster/Schock kommen fuer die Bilanz aus dem Check-Log (nicht aus kern_state, das nur den aktuellen Tag traegt); 8c2/Re-Entry werden aus den Trades des Tages rekonstruiert (SL-Klasse). Ein Check-Logeintrag mit Trigger + `gates_vorschau FAIL` zaehlt zusaetzlich als verarbeitet.
- A3: Ein Exit in der laufenden Kerze wird im Check sofort verbucht (Bars enthalten die laufende Kerze); ein Trigger, dessen Entry-Kerze vor Ende der Exit-Kerze eroeffnet, ist regelkonform "position"-blockiert (08.10. 18:10-Level-Cross bei BE in der 18:15-Kerze), ein Trigger am Exit-Kerzen-Schluss ist frei (Unit-Test).
- A4: `--spread-pkt` bleibt als Handeingabe eines echten Messwerts im Skript moeglich, ist aber im Prompt verboten (Selbsttest), damit niemand eine Zahl schaetzt. `--grund-spread` wird nicht mehr gebraucht (ignoriert).
- Testzahl 52 statt 44 (8 neue/erweiterte Tests zu A1-A13).

**Bekannte Luecken (unveraendert/neu):** Technik (c) bleibt NEIN bis 1m-Bars existieren; A13-Zaehlung ist eine Vereinfachung des alten Trigger-Satzes (Zustand 2/2 + 1H + 5m jenseits EMA, ohne Level-Fenster/Q-Score); 8c2/Re-Entry in der Bilanz-Nachrechnung nur aus nachgetragenen Trades (ein noch nicht nachgetragener Tag liefert keine SL-Liste → (b) rechnet ohne 8c2).

**Fuer den Opus-Kurzcheck (Datei:Zeile):** A1 `kern_start.cjs:54-66,36-45` · A2 `kern_bilanz.cjs:60-92`, `kern_check.cjs:47-49,113-116` · A3 `kern_check.cjs:66-71`, `kern_position_tick.cjs:24-50`, `kern_loop_prompt.cjs:42-43` · A4 `kern_gate.cjs:65-76,131`, `kern_bilanz.cjs:97,106-111,130,148-153`, `kern_nachtrag.cjs:31-34`, `kern_config.cjs:46-53`, `kern_loop_prompt.cjs:51-53` · A5 `kern_zeit.cjs:22-24` · A6 `kern_start.cjs:123-133` · A7 `kern_gate.cjs:68-73`, `kern_regeln.cjs:305-309` · A8 `kern_loop_prompt.cjs:60-62,96-99` · A9 `_freeze_alt_2026-10-14_erweitert.sha1` · A11 `kern_regeln.cjs:225-230` · A12 `kern_check.cjs:53`, `kern_common.cjs:94-96,130-131`, `kern_nachtrag.cjs:44-46` · A13 `kern_alt_schatten.cjs:90-96`, `kern_bilanz.cjs:73,131`.

## Restauflagen R1-R4/A15 (Opus-Kurzcheck 14.10.2026, Abschnitt "Kurzcheck Auflagen-Umsetzung"; Levi-Freigabe 14.10.)

Tests `node --test tests/kern.test.js tests/trading_scripts.test.js tests/tagesmomente.test.js` → **302/302 gruen** (246 alte unveraendert, kern.test.js 56 = 52 + 4). K0: nur `vollcheck.cjs` + `package.json`; erweiterte Liste 112/112. 7 Fixtures + Replays erneut gerechnet: **unveraendert** (+0,49 / +0,96 / +1,50 / −1,00 / −1,00 / −0,13 / Cap-FAIL 5,71×; 08.10. 18:45 bleibt TP1+TP2 +1,5). Kein Scope-Creep: ausser den genannten Stellen nichts geaendert.

| Rest | Status | Datei:Zeile | Test (tests/kern.test.js, describe "Restauflagen R1-R3 + A15") |
|---|---|---|---|
| R1 Bars-Frische in kern_start | erledigt | `kern_start.cjs:141-149` (letzte 5m-Kerze muss vom `--datum` sein und ≤ `BARS_START_MAX_ALTER_MIN` 30 Min alt, `kern_config.cjs:20`; ohne `--bars` Exit 1, mit `--bars` WARNUNG (R1) — analog A6) | "R1: … alte 13.10.-Bars am 14.10. → Exit 1; mit --bars Warnung; frische Bars (letzte 15:00, jetzt 15:05) laufen durch, ON_TIEF 30386,6" |
| R2 Abweichung Check/Nachrechnung | erledigt | `kern_bilanz.cjs:84-91` (`checkFailZu`/`abweichungen`: Check FAIL + Nachrechnung PASS wird NICHT mehr still als verarbeitet gewertet, sondern getrennt gezaehlt), Ausgabe eigene Zeile `:130` ("Abweichung Check/Nachrechnung (R2, pruefen): HH:MM-Trigger …: Check HH:MM:SS sagte FAIL <Grund>, Nachrechnung PASS"), (b)-Text "Abweichungen Check/Nachrechnung n", Auswertung `:178` | "R2: konstruierter Check-Logeintrag FAIL sperre bei Nachrechnung PASS (08.10. 18:10-Level-Cross) → Zeile + Zaehler 1; nicht als fehlender Gate-Lauf" |
| R3 `--spread-pkt` | erledigt (strengere Variante: ABWEISEN) | `kern_gate.cjs:65-78` (`--spread-pkt` → Exit 1 "Handeingabe ist kein Messwert, Schaetzung verboten"; Messweg nur `--bid/--ask/--hebel`; neues Feld `spread_quelle_typ` bid_ask\|annahme in Trade `:119`, Log `:154`), Nachtrag `--spread-gemessen` als "Messwert (Nachtrag, Quelle vom Operator zu belegen)" `kern_nachtrag.cjs:50` | "R3: --spread-pkt 2 → Exit 1, kein Log/Trade; bid/ask → spread_quelle_typ bid_ask"; A7-Test angepasst (−5/9/Schalter → Exit 1) |
| R4 Replay-Aussage | korrigiert | `regelwerk_kern_2026-10.md` Abschnitt "Replay-Stand (R4)"; hier unten | — |
| A15 1m-Abweichung | erledigt (nur Kennzahl) | `kern_bilanz.cjs:110-112` (`abweichung1m` aus `intrabar_detail.abweichung`), Zeile `:140` "Trades mit 1m-Abweichung zur 5m-Konvention (A15 …): x von n intrabar geprueften (Liste)", Auswertung `:178`; BE/TP2-Konvention und Fixtures unveraendert | "A15: 0 von 0 → nach konstruiertem intrabar_detail 1 von 1 mit Liste; Summen unveraendert; 08.10. 18:45 bleibt TP1+TP2 +1,5" |

**R4 — Berichtigung meiner Replay-Aussage:** Die Angabe im Nachtrag "Auflagen-Umsetzung 14.10." ("3 Replays neu gelaufen: alle unveraendert", Replay 05./08./13.10. n 3, Σ +1,85 R) war fuer A11 richtig, aber fuer A1 falsch: mit den korrekten Overnight-Leveln hat der 05.10. einen Level-Cross ueber ON_HOCH 30970,1 um 16:45 (Long-Entry 16:50, TP1+ZEIT **+1,11 R**). Richtig ist (Opus-Kurzcheck): **Replay 05./08./13.10. n 4, Σ +2,96 R; echtzeitnahes 8-Tage-Replay n 11, Σ −2,65 R, Ø −0,24 R.** Mein Replay lief mit `kern_start` ohne datierte `--bars` (Overnight null). Der Cross-Trigger bleibt ungemessen und liegt bei kleinem n um null — daraus wird keine Erwartungswert-Zahl abgeleitet.

**Abweichungen/Entscheide:** R3 strenger als "als ANNAHME loggen": eine Handeingabe wird abgewiesen, damit nie ein Zahlenwert ohne Quelle im Log landet; `spread_quelle_typ` kennt nur `bid_ask` und `annahme` (ein Pfad "schaetzung" existiert nicht). R2: Abweichungen zaehlen nicht als fehlender Gate-Lauf (Opus: "nicht als NEIN"), sind aber in (b)-Zeile, eigener Zeile und Auswertung sichtbar. R1: Toleranz 30 Min ab Schluss der letzten Kerze (Start-Update 15:00-15:20 → Bars um 15:00 gespeichert sind frisch).

**Fuer den Opus-Kurzcheck (Datei:Zeile):** R1 `kern_start.cjs:141-149`, `kern_config.cjs:20` · R2 `kern_bilanz.cjs:84-91, 130, 178` · R3 `kern_gate.cjs:65-78, 119, 154`, `kern_nachtrag.cjs:50` · A15 `kern_bilanz.cjs:110-112, 140, 178` · Tests `tests/kern.test.js` describe "Restauflagen R1-R3 + A15" (Z. ~525-573) + A7-Anpassung Z. ~289-294.

## Nachtrag R5/R6 (Opus-Kurzcheck 2, 14.10.2026; Levi-Freigabe 14.10.)

- **R5 (strengere Variante: abweisen):** `kern_nachtrag --spread-gemessen` → Exit 1 "kein Messwert ohne bid/ask-Beleg aus dem Gate" (`kern_nachtrag.cjs:63-67`, Kopf `:16-18`); `spread_basis` kennt nur "ANNAHME …" oder "Messwert … (bid/ask im Gate)" (`:50`). Opus-Probe im Test: 13.10. mit `--spread-gemessen 9` → Exit 1, Datei unveraendert; Nachtrag ohne Argument: ZEIT −0,15, `r_spread_gemessen null`, Bilanz Tagesstopp nein (Tag −0,15 R, Verluste 0), c ANNAHME. Damit stimmt der Satz "kein Zahlenwert ohne Quelle im Log" jetzt fuer Gate UND Nachtrag.
- **R6:** `kern_start`: Standard-5m-Bars fehlen/leer/unlesbar → Exit 1 wie R1 (`kern_start.cjs:149-156`), kein State/Level-File; mit explizitem `--bars` nur WARNUNG (R6) und ON = null. Test: fehlend, leer (`{bars:[]}`), unlesbar → je Exit 1; `--bars gibt_es_nicht.json` → Exit 0 mit Warnung. Der K3-Test "fehlender Typ = null" nutzt jetzt eine leere `--bars`-Datei.
- Tests **304/304** (kern.test.js 58 = 56 + 2); K0 nur `vollcheck.cjs`/`package.json`, erweiterte Liste 112/112; 7 Fixtures/Replays erneut gerechnet: unveraendert. Sonst nichts geaendert.
- Opus-Kurzblick: `kern_nachtrag.cjs:16-18, 50, 65-69` · `kern_start.cjs:22-23, 151-158` · `tests/kern.test.js` Tests "R5" und "R6" am Dateiende (Z. ~574-598), K3-Test "Wiederholung … fehlender Typ = null" (leer.json, Z. ~228-230).

## 7. Offene Punkte fuer Levi

- **L3 Produkt:** `kern_config.PRODUKT = { name: '', hebel: null, zert: null }` — Levi traegt Name/Hebel ein (zert kommt live aus `--ask`).
- **L4 Erster Kern-Fiktivtag:** offen, Levi legt ihn nach dem Opus-Gegencheck fest (Cron/Remote Control vorher an; im DST-Fenster 26.-30.10. gelten automatisch die 14:30-Fenster).
- **L1 Spread-Gate** bleibt Schatten (`SPREAD_GATE_HART false`); L2 EMA aus Bars + Chart-Pruefzeile beim ersten Check.
- Entscheidung zu Abweichung 2 (Trigger am 15m-Schluss; Check-Kadenz :00/:05 faengt ihn).
- Tagesabschluss braucht den 1m-Bars-Abruf (Technik c).
- **Commit:** bereit — Dateiliste Abschnitt 2; Commit/Push nur durch Levi.
