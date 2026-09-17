---
name: project-testtag-analyse-2026-09-17
description: "Opus-Analyse Testtag 17.09.2026 (fiktiv, EINGESCHRAENKT): 0 Trades, 0 Regelbruch. 58 vollcheck-Laeufe (53 Exit0/5 Hard-Exit), 113 gate_check-Saetze (105 SL-Vorpruefungen alle TAUGLICH, 8 live: 3 PASS+Q-ROT/UNBEKANNT ausgelassen, Option D korrekt gehalten). SCHWERSTER BEFUND: SL-Anker nach neuem Impuls-Extrem (19:00 DE, Hoch 29452,45 > altes Extrem 29447,80) nicht zurueckgesetzt -> 11 Voll-Checks (VC#45-#55, 19:07-19:56 DE) faelschlich 'Setup bis Retest TOT' bei SL-Distanz 6,57-8,25x ATR statt korrekt 1,50-1,95x ATR (12 PASS-faehige Slots verworfen) - ohne Geldfolge, alle 3 simulierten Spaeteinstiege waren bei Terminalzeit 20:00 DE noch offen. Q4 (Runway) jetzt 22 von 22 Bewertungen nie erfuellt (Wiedervorlage W4 vom 16.09.) - deckelte den Q-Score ab 15:50 DE strukturell auf ROT, kein Einstieg mehr moeglich unabhaengig von der Geometrie. 16:07-Fall (13.1xQ-ROT-Kollision, Option D, erster von ~10 vorgesehenen Kollisionsmomenten): TP1 zwar am 19:00 DE erreicht (nominal +1,00R), aber nach Punkt-12-Stall-Regeln (19 Kerzen ohne neues Extrem, Rueckfall 6,6 Pkt unter Entry bei 16:55-17:00 DE) realistisch nur 0 bis +0,5R erzielbar - 'verpasster Gewinn +1R' nicht belegbar, Option D war die richtige Entscheidung. Nebenbefunde: TP1-Fenster-Pruefung in --sl-vorpruefung nicht maschinell (14x unprotokollierte Kopfrechnung als Ablehnungsgrund), Screenshot-Praxis (nur 2 Screenshots, 52 gleichlautende Auslassungen in Folge) als Wiederholung von W5, 13 vorbestehende offene skipped_setups_fiktiv-Nachtraege aus 11./15.09. weiterhin unaufgeloest. 8 Vorschlaege X1-X8, KEINE sofortige Regelaenderung empfohlen (weder an Option D noch an der 4x-ATR-Diagnose noch an der RR-Schwelle)."
metadata:
  node_type: memory
  type: project
  status: "Analyse abgeschlossen (EINGESCHRAENKT), Vorschlaege X1-X8 offen (Levi-Entscheidung ausstehend)"
  originSessionId: dba6a9d3-1e4e-49d5-b8aa-69d27ac1a2f2
  modified: 2026-09-17T18:28:17.440Z
---

# Opus-Analyse Testtag 17.09.2026 (fiktiv)

*Quellenbasis: (a) Skript-Rohdateien in `C:\Users\umnus\tradingview-mcp\scripts\` (`vollcheck_log.jsonl`, `vollcheck_state.json`, `gate_check_log.jsonl`, `oneh_shadow_log.jsonl`, `skipped_setups_fiktiv.jsonl`, `register_touch_log.jsonl`, `sl_anker_wechsel_log.jsonl`, `trigger_kandidaten_log.jsonl`, `loop_stopp_log.jsonl`, `level_register.json`, `last_cooldown_check.txt`, `last_register_check.txt`, `x_last_fetch.json`), (b) live per CDP nachgeladene NAS100-5min-Bars (FOREXCOM:NAS100, 120 Bars 10:15-20:10 DE, gespeichert als `scripts/nas100_5m.json`), (c) `scripts/vollcheck.cjs` + `scripts/gate_check.cjs` im Quelltext (Nummerierungs- und Ampel-Logik selbst gelesen, nicht angenommen), (d) Regelwerk-Memory. EMA50(5min), ATR(14, 5min) und RSI(14, 5min) wurden aus den Rohbars selbst nachgerechnet. **Ehrliche Messgrenze:** zwischen den Ablesungen des Loops und dem spaeteren Re-Read der CFD-Bars liegt eine Feed-Drift von 2-15 Pkt (Beispiel: der Loop fuehrt „Kurs aus 5min-Bar-Schluss 16:05 DE" mit 29367,55, der Re-Read gibt fuer denselben Bar-Schluss 29353,45 bzw. fuer den 16:00-Schluss 29369,55). Wo eine Aussage innerhalb dieser Drift liegt, ist das ausdruecklich benannt. Prozessregel V7 (Skript-Logs sind Primaerquelle) wurde eingehalten.*

## 1. Kurzfazit

Ein Tag ohne Trade, ohne Regelbruch und ohne Verlust — und trotzdem mit zwei substanziellen Befunden, die beide dasselbe Muster haben: **das System hat die richtige Information erhoben und sie nicht mit der abhängigen Größe verbunden.** Genau die Diagnose aus der 16.09.-Analyse, nur ohne Geld dahinter.

Mechanisch war der Tag sauber: 53 erfolgreiche Voll-Checks bei 5 Hard-Exits (9 % gegen 30 % am 16.09.), 105 von 105 SL-Anker-Vorprüfungen mit Urteil TAUGLICH, 8 Live-Gate-Läufe statt 18 (16.09.) bzw. 39 (15.09.), 3 Trigger-Momente, alle drei vollständig in `skipped_setups_fiktiv.jsonl` erfasst, kein einziger Wiederhol-Zirkel, keine Log-Inkonsistenz, kein Eingriff in `trades.db`.

Fachlich stand der Tag unter einer **de-facto-Einstiegssperre von 3 Stunden 44 Minuten.** 43 der 53 Voll-Checks (81 %) tragen dieselbe Konsequenzzeile — „beobachten — Q-ROT geht vor (Option D)" — und alle 43 verweisen auf **einen einzigen** Gate-Lauf um 16:07 DE. Die Ursache ist nicht Option D, sondern eine Arithmetik, die den Q-Score an diesem Tag strukturell bei **2/4 = ROT** gedeckelt hat: Q4 (Runway) lag bei 0,39 / 0,53 / 0,56 und war damit — wie in allen 22 bisher bewerteten Fällen der gesamten Loghistorie — nicht erfüllbar, und Q1 (Ablehnungsmuster) fand den ganzen Tag kein Signal. Mehr als Q2+Q3 war nicht erreichbar. Da gleichzeitig `--chasing yes` ab 16:05 DE dauerhaft anlag (k stieg von 1 auf 45), wurde **jeder** PASS automatisch zur 13.1×Q-ROT-Kollision und damit zum Nicht-Einstieg. Das ist kein Kalibrierungsfehler von Option D — es ist der am 16.09. als W4 benannte Q4-Defekt, der jetzt seine erste vollständige Tageswirkung gezeigt hat.

Der **16:07-Fall ist nicht der verpasste Gewinn, als der er im Vorbericht steht.** TP1 29450 wurde erreicht — aber erst um **19:00 DE**, 2 Stunden 53 Minuten nach Entry, nach einem Rückfall 6,6 Pkt UNTER den Entry und einer 19-Kerzen-Stallphase. Der Punkt-12-Stall-Trigger wäre gegen 16:55 DE bei einem Stand von +6,5 Pkt (+0,08R) gefallen, nachdem der MFE um 16:40 DE bereits 74,3 Pkt (0,90R) betragen hatte. Realistisch liegt der Fall zwischen 0R und ca. +0,5R, nicht bei +1,00R (Abschnitt 4.1).

Der **härteste eigene Befund** betrifft dagegen das letzte Loop-Fenster: ab 19:00 DE überschritt der Kurs das Impuls-Extrem des Tages (29447,80 vom 14:30 DE) und machte bei 29452,45 / 29456,35 ein neues. Der SL-Anker hätte damit nach [[feedback_live_trading]] 7b1 Schritt 3a zurückgesetzt werden müssen. Er blieb auf dem Wert von **15:40:51 DE** stehen. Folge: die letzten **11 Voll-Checks (VC#45–#55, 19:07–19:56 DE)** meldeten „Setup bis Retest TOT" bei 6,57–8,25× ATR, während die korrekt verankerte Geometrie an **jedem** dieser 11 Slots PASS-fähig war (SL-Distanz am 8c-Floor, 1,50–1,95× ATR, RR 1,02–1,67, Registerlevel im Fenster). Der Loop hat das neue Tageshoch selbst zweimal protokolliert und es nicht mit dem Anker verbunden (Abschnitt 4.2). Ohne Geldfolge — alle drei nachsimulierten Späteinstiege standen um 20:10 DE noch offen — aber es ist eine belegbare Fehlanwendung einer harten Regel.

Umgekehrt gehört ins Protokoll: **für die Phase 16:40–18:55 DE hat die Rekonstruktion den Loop bestätigt.** Anker 29300,75 war dort korrekt (das Impuls-Extrem war noch nicht überschritten), und das zulässige TP1-Fenster war an jedem einzelnen Slot rechnerisch leer bzw. negativ. Eine erste Gegenrechnung hatte dort einen PASS-fähigen Moment um 17:05 DE zu finden geglaubt; nach Einrechnung des 8c2-Zuschlags (0,5× ATR unter dem Anker, den der Loop selbst verwendet) ist dieser Befund **hinfällig** und ausdrücklich korrigiert.

## 2. Zahlenbilanz (verifiziert, Abweichungen zur ursprünglichen Sonnet-Zählung fett)

| Größe | Befund | Quelle |
|---|---|---|
| Loop-Fenster | erster Voll-Check **15:27:47 DE** (`erster_vollcheck` 13:27:47Z), letzter **19:55:21 DE** (VC#55) | `vollcheck_state.json`, `vollcheck_log.jsonl` |
| Loop-Stopp | **19:59:39 DE**, `ergebnis: "keine"`, Grund „Testtag-Ende 20:00 DE (fiktiv)"; einzige Quelle `vollcheck_state` (19:55:21), `last_position_tick.json` als „Stempel 2026-09-16 … nicht verwertbar" korrekt verworfen | `loop_stopp_log.jsonl` |
| `vollcheck.cjs`-Läufe heute | **58** — davon **53 Exit 0**, **5 Hard-Exit 1** | `vollcheck_log.jsonl` (58 Zeilen mit ts-Präfix 2026-09-17, 0 Parse-Fehler) |
| Läufe mit gültiger `vc`-Nummer | **53, nicht 54** — darunter **eine Doppelvergabe: VC#2 zweimal** (13:31:37Z und 13:32:02Z, Re-Run zur Behebung der Lücke „Tweet-Check ✗"). Also **52 verschiedene** Voll-Checks | ebd. |
| Nummernlücken | #13, #19, #44 bestätigt. **Mechanismus abweichend:** `vc` ist nach `vollcheck.cjs` Zeile 239 rein zeitabgeleitet — die Lücken sind **drei real ausgefallene 5-Min-Slots: 16:25, 16:55 und 19:00 DE**, keine Zählartefakte | `vollcheck.cjs` Z. 225–241, `vollcheck_state.json` (`historie` = 52 Einträge, max `nr` 55) |
| Ausfall-Intervalle | 16:20→16:31 (11 Min), 16:50→17:00 (10 Min), 18:55→19:08 (13 Min) | `vollcheck_state.json` `historie` |
| Voll-Checks mit `vollstaendig: true` | **0 von 58** (53× `false`, 5× `null`) — gleiches Bild wie 16.09. (0 von 90) | `vollcheck_log.jsonl` |
| Hard-Exit-Ursachen | 2× fehlende Pflichtfelder ohne `--grund-` (15:27:47 `--impuls-pkt`/`--retest-bedingung`; 16:20:16 `--tweet-news`), 2× `--punkt11-signal` fehlt bei erfülltem Chasing-Kriterium (16:01:11 bei 4 Kerzen, 16:11:17 bei 6 Kerzen), 1× D5-Fix „`--jetzt` VERALTET" um 19:05:16 (Register 134 s nach dem Zeitanker erneuert). Jeder im direkt folgenden Aufruf behoben | ebd. |
| `gate_check.cjs`-Sätze heute | **113** = **105 `--sl-vorpruefung`** + **8 `live`** | `gate_check_log.jsonl` |
| Vorprüfungs-Urteile | **105 von 105 „SL-ANKER TAUGLICH"**, Exit 0 in allen 105 Fällen; erste 15:26:11, letzte 19:56:50 DE | ebd. |
| Live-Ergebnisse | **3× Abbruch (Exit 1)**, **2× UNKNOWN (Exit 2)**, **3× PASS (Exit 0)** | ebd., quergeprüft gegen `trigger_kandidaten_log.jsonl` (2 UNKNOWN-Einträge heute) |
| Live-Cluster | Trigger 1 = 15:26:27 Abbruch → 15:27:02 Abbruch → 15:27:13 UNKNOWN → **15:27:27 PASS**; Trigger 2 = **15:31:21 PASS** (einzelner Lauf); Trigger 3 = 16:07:16 Abbruch → 16:07:27 UNKNOWN → **16:07:39 PASS**. Die Kette Abbruch→UNKNOWN→PASS trat bei Trigger 1 UND 3 auf, nicht nur bei 16:07 | ebd. |
| Trades | **0** — kein PASS wurde in eine Position umgesetzt | `skipped_setups_fiktiv.jsonl`, `vollcheck_state.json` (`position` in allen 52 Historieneinträgen `null`) |
| Cooldown | „🟢 Kein Cooldown aktiv", letzter DB-Trade #43 vom 21.08.2026 → Regel 1 nicht anwendbar | `last_cooldown_check.txt` |
| UNTAUGLICH-Urteile / `add_skipped_setup.cjs` | **0** — `trades.db` heute nicht angefasst, Testtag-Guard gehalten | `gate_check_log.jsonl`, `trades.db` |
| 1H-Schattenmessung | **53 Einträge, `blockade_3v4` in allen 53 `false`**, `e7_zaehlt` in allen 53 `null`; `abstand_atr` 6,40–15,32 (Kurs den ganzen Tag 6–15× ATR über der 1H-EMA50) | `oneh_shadow_log.jsonl` |
| SL-Anker-Wechsel | **3 Einträge, alle 15:31:07 / 15:35:58 / 15:40:51 DE**, alle `gegen_richtung: true`, alle mit Kerzen-Beleg begründet. Danach 4 h 16 min kein Wechsel mehr — siehe 4.2 | `sl_anker_wechsel_log.jsonl` |
| Register-Touches | **8**, davon **2 mit `note: "ohne Notiz"`** (15:05:40 und 19:06:39 DE) — beide inhaltsneutral. Inhaltliche Änderungen: 15:26:06 (Wick-Zone 29403,6–29410 neu) und 19:07:30 (Session-Hoch 29447,8 → 29456,35) | `register_touch_log.jsonl` |
| Register-Frische | 21 Level (17 NAS100-relevant), 0 Fib-/Measured-Move-Einträge; Alter zum Tagesende 48 Min (Warnung ab 60) | `last_register_check.txt`, `level_register.json` |
| **Screenshots (Loop)** | **2** — `tv_full_2026-09-17T13-05-31-101Z.png` (15:05 DE) und `tv_chart_2026-09-17T13-27-46-818Z.png` (15:27 DE). 5 weitere PNG mit Tagesdatum (`fable_replay_*`) entstanden 11:13–11:15 bei CDP-Diagnose, nicht im Loop. Vergleich: 15.09. = 49, 16.09. = 0 | `screenshots/`, Dateizeitstempel |
| Lückentypen (53 Exit-0-Läufe) | **53× „8d LUECKE"**, **53× „Fib LUECKE"**, **52× „Screenshot LUECKE (ausgelassen)"**, **3× „Tweet-Check ✗"** (16.09.: 14×), 1× „Retest-Zeitbox VC+1/2 LUECKE" | `vollcheck_log.jsonl` |
| Konsequenzverteilung | **43× „beobachten — Q-ROT geht vor (Option D)"**, 8× „keine", 1× „beobachten (k=1/2)", 1× „50 %-Einstieg AKTIV vorschlagen (13.1)" | ebd. |
| Kollisions-Schattenmessung | **1** Eintrag mit `kollision_13_1_qrot: true` (16:07:39) — der erste überhaupt. Auswertung vorgesehen „nach ~10 Kollisionsmomenten" | `skipped_setups_fiktiv.jsonl`, [[project_praezedenz_13_1_vs_qrot_entscheidungsvorlage_2026-09-15]] |
| Skipped-Backlog | **13 offen** — 12 vom 11.09. (`entscheidung: null`) + 1 vom 15.09. (`ausgelassen`, `ergebnis_r: null`) | `skipped_setups_fiktiv.jsonl` |
| Tagesrange NAS100 im Loop-Fenster | Hoch 29456,35 (19:05 DE) / Tief 29300,75 (15:35 DE) = 155,6 Pkt; größte Einzelkerze 15:30 DE mit 94,2 Pkt Range (C = L = 29336,35) | `nas100_5m.json` |

**Ergebnis der drei ausgelassenen Setups, bar-für-bar unabhängig nachgerechnet (Ausgang bis Exit, nicht bis Tagesende):**

| Moment | Entry / SL / TP1 | RR | Ausgang | MFE / MAE (korrigiert) | Log-Werte (Erstnachtrag) |
|---|---|---|---|---|---|
| 15:27 DE | 29415,80 / 29378,35 / 29475,80 | 1,602 | **SL-HIT @15:30 DE** (Kerze C=L=29336,35) | 14,75 / 79,45 | 40,55 / 115,05 |
| 15:31 DE | 29411,85 / 29378,10 / 29475,80 | 1,895 | **SL-HIT @15:35 DE** (Tief 29300,75) | 0,00 / 111,10 | 44,50 / 111,10 |
| 16:07 DE | 29367,55 / 29285,40 / 29450,00 | 1,004 | **TP1-HIT @19:00 DE** (Hoch 29452,45) | 84,90 / 26,30 | 88,80 / 33,10 |

**Abweichungsbefund:** die im Erstnachtrag stehenden MFE/MAE-Werte sind gegen die Tagesextrema gerechnet (29456,35 bzw. 29300,75/29334,45), nicht gegen das Trade-Fenster bis zum Exit. Die R-Werte (−1 / −1 / +1) sind davon nicht betroffen, die MFE/MAE-Spalten sind für die ersten beiden Fälle unbrauchbar (siehe X4).

## 3. Der Tag in einem Satz Geometrie

Das zulässige TP1-Fenster ist arithmetisch **(3 − SL-Distanz/ATR) × ATR** breit (untere Grenze RR ≥ 1, obere Grenze Zone-3-Kappe bei 3× ATR). Gegenprobe am gedruckten Wert des 16:07-Laufs: `zulaessiges TP1-Fenster [29449.7 ; 29459.65] = 10 Pkt breit` passt exakt zur Formel bei SL-Distanz 82,1 und ATR 30,7.

| Fenster DE | SL-Distanz / ATR | TP1-Fenster | Bewertung |
|---|---|---|---|
| 15:26–15:46 | 1,50–1,62× | 32–42 Pkt breit | Normalbetrieb — hier fielen die zwei PASS |
| 15:52–16:11 | 2,49–2,76× | 7,5–16 Pkt | Nadelöhr. Der 16:07-PASS war der Zufall, dass die 50er-Rundzahl 29450 in ein 10-Pkt-Fenster fiel |
| 16:16–17:31 | 2,61–4,48× | meist negativ | unlösbar; nur 16:45–17:28 kurz wieder < 4× |
| 17:35–19:56 | 4,44–8,25× (Max 19:46) | −37 bis −96 Pkt | dauerhaft negativ = strukturell kein PASS möglich |

**Gegenprüfung zur ursprünglichen Aussage „ab ca. 17:36 DE wuchs die SL-Distanz von ca. 6,5× auf 8,25×":** Richtung und Endwert stimmen, der Startwert nicht — um 17:35/17:36 waren es 4,44×, die 6,5er-Marke wurde erst um 18:01 (6,47×) erreicht; Maximum 8,25× um 19:46, letzter Wert 8,19× um 19:56. Die 4×-Schwelle wurde zweimal überschritten: erstmals kurz 16:35–16:41 (4,25–4,48×), dann ab 17:31 dauerhaft. Dazwischen lag ein Fenster **16:45–17:28 DE (8 Messpunkte, 43 Min)**, in dem die SL-Distanz wieder auf 2,61–3,82× zurückfiel.

**Wurde in diesem Fenster etwas übersehen? Nein**, mit einer Einschränkung. Alle acht Messpunkte wurden mit den Zahlen des Loops selbst (Entry aus `argv`, SL-Distanz und ATR aus dem Output) und dem Registerstand zu genau jenem Zeitpunkt nachgerechnet: 16:45 (−25,0 Pkt), 16:50 (−2,3), 17:01 (10,0 Pkt, leer), 17:10 (3,2, leer), 17:15 (3,3), 17:20 (−11,7), 17:28 (−23,8). Einzig 17:05/17:06 DE ergibt mit den Loop-Zahlen ein Fenster [29441,85; 29453,55], in dem zwei registrierte Level liegen (Session-Hoch 29447,80, Rundzahl 29450) → RR 1,104 → wäre PASS gewesen. Mit dem unabhängigen Re-Read-Bar-Schluss (29375,85 statt 29363,85) ist dasselbe Fenster leer. Die Differenz liegt innerhalb der Feed-Drift und ist aus den Logs nicht auflösbar — der Fall wird deshalb nicht als verpasstes Setup gezählt, sondern als Beleg für Befund 4.3.

## 4. Zentrale Befunde

### 4.1 Der 16:07-Fall: Option D hat funktionsgemäß gearbeitet, „+1R verpasst" ist nicht belegbar

Der Lauf um 16:07:39 DE ist vollständig im Log (`gate_check_log.jsonl`, Feld `output`):

```
[PASS ✓] RR-Gate (8b): RR 1.004:1 (TP1 82.5 Pkt / SL 82.1 Pkt)
[PASS ✓] SL-Mindestdistanz (8c): 82.1 ÷ 30.7 = 2.68x
[PASS ✓] TP-Realismus (8b1): 2.69x ATR -> Zone 2
--> GESAMTSTATUS: PASS  [Sizing-Flag: halbe Position]
[Q1 = UNKLAR] --q1-reject nicht angegeben
[Q2 = JA]     0.97x ATR (Schwelle <=1,5x)
[Q3 = UNKLAR] --q3-coherence nicht angegeben
[Q4 = NEIN]   Runway-Ratio 0.39
--> Q-SCORE: 1/4 UNBEKANNT
--> Solo-Default (Option D): KEIN Einstieg
```

**Zur Q1/Q3-Frage:** Beide Faktoren wurden per A3-Grund als „nicht prüfbar" abgegeben, obwohl die Begründungstexte im `argv` die Antwort selbst enthalten (`--grund-q1-reject "kein Ablehnungssignal in dieser Session beobachtet"`, `--grund-q3-coherence "Dual-Gate 2/2 + 1H-Bias kohaerent long"`). 40 Minuten vorher, bei den Läufen 15:27/15:31, waren dieselben zwei Faktoren regulär bewertet worden (Q1 = NEIN, Q3 = JA) — eine Inkonsistenz innerhalb desselben Tages, die aber das Ergebnis nicht ändert: mit ehrlicher Bewertung (Q1 NEIN, Q2 JA, Q3 JA, Q4 NEIN) ergibt sich 2/4 = ROT, wieder Kollision mit 13.1, wieder Option-D-Nicht-Einstieg. Die A3-Deklaration ist ein Dokumentationsmangel, kein Kausalfaktor.

**Zum Ausgang:** TP1 29450 wurde erreicht (Hoch der 19:00-Kerze 29452,45), aber:

| Zeit DE | Kurs/Ereignis | Stand ggü. Entry 29367,55 |
|---|---|---|
| 16:10 | Hoch 29411,25 | +43,7 Pkt (0,53R) |
| 16:40 | Hoch 29441,85 = MFE | +74,3 Pkt (0,90R) |
| 16:45–18:15 | 19 Kerzen ohne neues Hoch, RSI(5min) 59,0 → 46,3 | |
| 16:55 | Kurs 29374,25 | +6,7 Pkt (+0,08R) |
| 17:00 | Tief 29360,95 | −6,6 Pkt, unter Entry |
| 19:00 | Hoch 29452,45 → TP1 | +1,00R |

Haltedauer bis TP1: 2 h 53 min. Punkt 12/12.3 ([[feedback_live_trading]]) triggert bei 2 Kerzenschlüssen ohne neues Extrem plus RSI ≥ 5 Punkte vom Extrem entfernt → Pflichtvorschlag 25–50 % Teilgewinn. Diese Bedingung war um 16:55 DE erfüllt (3 Kerzen ohne neues Hoch, RSI 47,9 gegen Extremwert 59,0 = 11,1 Punkte) — bei einem Stand von +0,08R.

**Bewertung:** Die +1,00R im Erstnachtrag sind ein reines Hoch/Tief-Simulat ohne Positionsmanagement. Unter den bindenden Punkt-12-Regeln liegt das realistische Ergebnis zwischen 0R und ca. +0,5R. Die Aussage „Option D hat einen validen Gewinn-Trade verhindert" ist in dieser Form nicht belegbar — verhindert wurde eine Position mit RR 1,004:1 (das dünnste zulässige Verhältnis) in einem 10-Pkt-Fenster.

**Gegenprobe:** Die beiden Q-ROT-Ablehnungen um 15:27/15:31 hatten Q2 = 4,08× und 3,44× ATR (Entry deutlich zu weit über der EMA50) — „Entry zu reif". Beide SL fielen innerhalb von einer bis zwei Kerzen (15:30-Kerze C=L, 94,2 Pkt Range; 15:35-Kerze weitere 35,6 Pkt tiefer). Der Q-Score hat hier zwei sichere −1R verhindert und in einem Fall eine Position mit unklarem, im besten Fall halbem R gekostet. Netto hat die Q-Score-/Option-D-Mechanik dem Tag mindestens +1,5R gespart.

**Regelwerkliche Einordnung:** [[project_praezedenz_13_1_vs_qrot_entscheidungsvorlage_2026-09-15]] hat Option D als „Default-Festlegung mit Revisionsklausel" beschlossen, Auswertung nach ~10 Kollisionsmomenten. Heute ist Kollisionsmoment 1 von ~10 erfasst — genau wie vorgesehen. Keine Regeländerung fällig.

### 4.2 Der SL-Anker wurde nach dem neuen Impuls-Extrem nicht zurückgesetzt — 11 Voll-Checks auf falscher Geometrie (schwerster eigener Befund)

[[feedback_live_trading]] 7b1 Schritt 3a: Anker = tiefstes Low / höchstes High aller abgeschlossenen 5min-Kerzen **seit dem letzten Impuls-Extrem**, inklusive Dochte.

| Zeit DE | Ereignis | Beleg |
|---|---|---|
| 14:30 | Impuls-Hoch 29447,80 (vor Loop-Start, als Session-Hoch im Register) | `level_register.json`, Bars |
| 15:40:51 | letzter Ankerwechsel: 29336,35 → 29300,75, begründet | `sl_anker_wechsel_log.jsonl` |
| 15:40–18:55 | Impuls-Hoch nicht überschritten (höchster Wert 29446,95 um 18:20 DE — 0,85 Pkt darunter) → Anker 29300,75 war korrekt | Bars |
| 19:00 | Hoch 29452,45 — Impuls-Extrem überschritten | Bars |
| 19:05 | Hoch 29456,35 — neues Tageshoch | Bars |
| 19:08:51 | VC#45-Fazit: „…neues Tageshoch 29456.35, Momentum wieder positiv" | `vollcheck_log.jsonl` |
| 19:07:30 | `register_touch.cjs`: „Session-Hoch: 29447.8 → 29456.35" | `register_touch_log.jsonl` |
| 19:07–19:56 | Anker bleibt 29300,75. SL-Distanz 6,57–8,25× ATR → 11× „Setup bis Retest TOT" | `gate_check_log.jsonl`, `vollcheck_log.jsonl` |

Nachgerechnet mit korrektem Reset (Anker = tiefstes Low seit dem Extrem, SL = Anker − 0,5× ATR, Register inkl. 29456,35 ab 19:07):

| Slot DE | Anker (korrekt) | SL-Distanz / ATR | TP1-Fenster | Registerlevel im Fenster | RR |
|---|---|---|---|---|---|
| 19:00 | 29433,25 | 1,50× | 31,0 Pkt | 29500 | 1,666 |
| 19:05 | 29439,95 | 1,50× | 30,5 Pkt | 29475,80 | 1,105 |
| 19:10 | 29425,25 | 1,50× | 30,2 Pkt | 29475,80 | 1,634 |
| 19:15 | 29425,25 | 1,50× | 29,3 Pkt | 29475,80 | 1,450 |
| 19:20 | 29416,45 | 1,50× | 29,4 Pkt | 29450 | 1,068 |
| 19:25 | 29415,55 | 1,50× | 28,7 Pkt | 29450 | 1,019 |
| 19:30 | 29411,75 | 1,50× | 28,3 Pkt | 29456,35 | 1,064 |
| 19:35 | 29411,75 | 1,50× | 27,7 Pkt | 29447,80 | 1,083 |
| 19:40 | 29411,75 | 1,61× | 25,3 Pkt | 29475,80 | 1,506 |
| 19:45 | 29411,75 | 1,69× | 23,1 Pkt | 29475,80 | 1,443 |
| 19:50 | 29411,75 | 1,50× | 25,8 Pkt | 29456,35 | 1,199 |
| 19:55 | 29411,75 | 1,95× | 17,7 Pkt | 29475,80 | 1,201 |

Zwölf aufeinanderfolgende Slots, zwölfmal PASS-fähige Geometrie, gemeldet als „UNLOESBAR / Setup bis Retest TOT". Faktor über 4 gegenüber der real geloggten SL-Distanz — zu groß für Feed-Drift.

**Warum der Anker nicht nachzog:** `sl_anker_wechsel_log.jsonl` erfasst heute nur Wechsel mit `gegen_richtung: true` (SL weiter weg). Ein Reset nach oben (näher, Risiko kleiner) ist in keinem Skript verankert und wurde nie protokolliert — kein Gate prüft, ob seit dem letzten Ankerwechsel ein neues Impuls-Extrem entstanden ist. Strukturell dieselbe Lücke wie der FOMC-Befund vom 16.09.: die Information (neues Tageshoch) wurde zweimal erhoben und nicht mit der abhängigen Größe verbunden.

**Kostenfolge: keine.** Simulation der drei besten Späteinstiege (19:10/19:20/19:40) gegen die Bars bis 20:10 DE: alle drei standen bei Loop-Stopp noch offen (weder TP1 noch SL erreicht). Der Befund ist ein Regelanwendungsfehler ohne Geldfolge — an einem anderen Tag hätte er eine andere Zahl bedeutet.

**Zur Ehrenrettung:** für 16:40–18:55 DE (28 Voll-Checks) bestätigt dieselbe Rekonstruktion die Diagnose „unlösbar" an jedem einzelnen Slot. Die „Setup tot"-Kette war 28 Voll-Checks lang inhaltlich richtig und erst in den letzten 11 falsch — und der Grund für die wachsende SL-Distanz an sich ist die Anker-Definition selbst (gewollt, solange das Extrem hält), nicht Nachlässigkeit.

### 4.3 „TP1-Fenster leer" war der häufigste Ablehnungsgrund des Tages — und ist nie maschinell geprüft worden

Die Formulierung „TP1-Fenster leer" steht in 14 von 52 Voll-Check-Fazits. `gate_check.cjs --sl-vorpruefung` rechnet das TP1-Fenster jedoch nicht (nimmt weder `--sl` noch `--tp1`) — die Diagnosezeile erscheint nur in den 3 Live-PASS-Sätzen. Alle 14 „Fenster leer"-Aussagen sind unprotokollierte Kopfrechnung. VC#21 (17:05:15 DE) formuliert das selbst: „TP1-Fenster trotz Register-Level im theoretischen Bereich weiterhin geometrisch leer" — ohne gedruckte Rechnung. Mit den 36 Sekunden später vom Loop selbst übergebenen Zahlen ergäbe sich ein Fenster, in dem zwei Registerlevel liegen (PASS-fähig); mit dem unabhängigen Bar-Re-Read ist es leer (Feed-Drift, nicht auflösbar). Befund: am häufigsten benutzten Ablehnungsgrund des Tages fehlt ein Log-Eintrag.

### 4.4 Q4 ist weiter unerfüllbar — jetzt 22 von 22 (Wiedervorlage W4)

Heute kamen drei Nicht-Erfüllungen hinzu: Runway-Ratio 0,53 (15:27), 0,56 (15:31), 0,39 (16:07), jeweils mit derselben Ursache „2 Register-Level zwischen Entry und TP1" (Rundzahlen). Damit 22 von 22, 0× ERFUELLT. Mit Q4 dauerhaft NEIN ist 4/4 unerreichbar und 3/4 nur über Q1+Q2+Q3 möglich; Q1 fand den ganzen Tag kein Ablehnungssignal. Ab dem ersten Gate-Lauf des Tages war die Obergrenze damit 2/4 = ROT festgelegt, und in Verbindung mit `--chasing yes` ab 16:05 DE (k bis 45) wurde jeder denkbare PASS automatisch zur 13.1×Q-ROT-Kollision. **Der Tag konnte ab 15:50 DE strukturell keinen Einstieg mehr erzeugen, unabhängig von der Geometrie.** Keine sofortige Änderung empfohlen, aber die Frage ist von „fällig" auf „überfällig" gestiegen (X3).

### 4.5 Screenshot-Auslassungspraxis — die Begründung ist prüfbar und hält nicht (Wiedervorlage W5)

2 Loop-Screenshots (15:05, 15:27 DE), danach 52 von 53 Voll-Checks „ausgelassen" mit identischer Begründung „kein visueller Zusatzwert, gleicher Chartausschnitt". Falsifiziert an mindestens zwei Stellen: der 94,2-Pkt-Absturz um 15:30 DE (3 Min nach dem letzten Screenshot) und das neue Tageshoch um 19:00/19:05 DE (genau das Ereignis aus 4.2). Richtung besser als 16.09. (0 Screenshots), Systematik dieselbe: unbegrenzte Kette identisch begründeter Ausnahmen.

### 4.6 Drei ausgefallene Voll-Check-Slots und der Registerpflege-Umweg

#13/#19/#44 sind ausgefallene 5-Min-Slots (16:25, 16:55, 19:00 DE), zeitlich zwei davon deckungsgleich mit Register-Nebenaufgaben. Die `--help`-Fehlbedienung von `register_touch.cjs` (kennt `--help` nicht als Sonderflag) führte um 19:06:39 zu einem echten, inhaltsleeren Touch; der folgende inhaltliche Touch um 19:07:30 lag 134 s nach dem VC#45-Zeitanker und löste korrekt den D5-Hard-Exit aus — das Skript verhielt sich richtig, der Fehler lag in der Reihenfolge Zeitanker → Registeränderung → Voll-Check.

### 4.7 Was mechanisch funktioniert hat

| Größe | 15.09. | 16.09. | 17.09. |
|---|---|---|---|
| Live-Gate-Läufe | 39 | 18 | 8 |
| FAIL-Läufe (live) | 31 | 4 | 0 |
| Hard-Exit-Quote `vollcheck.cjs` | — | 27/90 = 30 % | 5/58 = 9 % |
| Skipped-Setup-Erfassung | 1/31 | 6/6 | 3/3 |
| „Tweet-Check ✗" | — | 14 | 3 |
| Screenshots | 49 | 0 | 2 |
| Log-Inkonsistenzen | — | 0 | 0 |

V1 (Aussichtslos-Sperre), V5 (SL-Distanz-Diagnose >4× ATR) und V6 (Ankerwechsel-Log) griffen sauber; der Loop-Stopp-Guard verwarf den veralteten `position_tick`-Stempel korrekt.

### 4.8 Strukturelle Logging-Lücken (kein Sonnet-Fehler)

Kein Quick-Tick-Log, kein Tweet-Fetch-Verlaufslog als Datei — nur der aktuelle Watermark in `x_last_fetch.json`. Die Gesamtzahl der Quick-Ticks und Tweet-Fetches des Tages (rund 270 Tick-Ereignisse bei 4,5 h 1-Min-Cron) ist aus den Skript-Logs nicht rekonstruierbar. Werkzeugmangel, keine Nachlässigkeit.

### 4.9 Backlog: 13 unaufgelöste Skipped-Nachträge seit über einer Woche

12 vom 11.09. (alle `entscheidung: null`) + 1 vom 15.09. — seit sechs Handelstagen liegengeblieben, von `gate_check.cjs` selbst als Tagesabschluss-Pflicht deklariert und inhaltlich relevant für die Q4-Datenbasis (X3).

## 5. Status

**EINGESCHRÄNKT — mit klarem Ergebnis.** Vollständig belegt und mehrfach quergeprüft: alle Zählstände, die 113 Gate-Sätze, die Nummerierungsmechanik, die Ampel-Logik, der SL-Distanz-Verlauf über 105 Messpunkte, die Register-Historie, die Screenshot-Bilanz, der Loop-Stopp und die drei Skipped-Ausgänge. Nicht belegbar: die Innenansicht der 5 Hard-Exit-Läufe, welche Entry-Zahl um 17:05 DE richtig war (Feed-Drift), die exakte Zahl der Quick-Ticks/Tweet-Fetches, und rückwirkend nicht prüfbare 8a1/8a2/Q1/Q3/RVOL/VWAP-Bewertungen.

**Regelkonformität: JA — Verstoß: NEIN** für den Tag als Ganzes. Kein Trade, kein Blackout-Fenster, keine Sizing-Frage, keine Tagesverlust-Berührung. Befund 4.2 ist eine Fehlanwendung einer Regel in der Analyse, kein Entry-Regelbruch — kein Geld betroffen.

**Kein Echtgeld-Go aus diesem Tag** — die fünf binären Pass-Kriterien aus [[project_validierungstesttag_naechster_handelstag]] lassen sich an einem Tag mit 0 Trades nicht positiv erfüllen, und die 0 Trades sind hier nachweislich das Ergebnis einer strukturell gedeckelten Q-Score-Arithmetik (4.4), nicht der Marktlage. Ein Validierungstag mit unerfüllbarem Q4 kann kein Validierungstag sein — **X3 vor dem nächsten als Validierungstag gefahrenen Testtag entscheiden.**

## 6. Konkrete Vorschläge X1-X8 (offen, Levi-Entscheidung)

**X1 (hoch, Regelanwendung — wichtigster Punkt des Tages).** `gate_check.cjs --sl-vorpruefung` und `vollcheck.cjs` bekommen eine Impuls-Extrem-Prüfung: höchstes High (long) / tiefstes Low (short) seit dem letzten protokollierten Ankerwechsel mitführen; bei Überschreitung Pflichtzeile „ANKER-RESET FÄLLIG" ausgeben und den Wechsel in `sl_anker_wechsel_log.jsonl` mit `gegen_richtung: false` erfassen (das Log kennt heute nur Wechsel gegen die Richtung). Zur Diskussion: bei veraltetem Anker Hard-Exit 1 statt Warnzeile. Begründung: hätte heute 11 Voll-Checks mit falschem Verdikt verhindert.

**X2 (hoch, Diagnose-Lücke).** `--sl-vorpruefung` soll das zulässige TP1-Fenster inkl. Registerlevel mitrechnen und drucken (braucht nur Anker/ATR/Entry, kein `--tp1`). „TP1-Fenster leer" war heute der zweithäufigste Ablehnungsgrund (14 Voll-Checks) und stand in keinem Log als Rechnung.

**X3 (hoch, Kalibrierung — Wiedervorlage W4).** Q4 (Runway ≥1,0) steht bei 22 von 22 Nicht-Erfüllungen und deckelt den Q-Score strukturell. Vorschlag: Q4 als Schattenfaktor kennzeichnen und Score bis zur Entscheidung aus Q1-Q3 bilden (Schwelle 3/3), oder Schwelle zur Diskussion stellen (z. B. ≥0,5). Nicht ohne Levi-Entscheidung umsetzen, aber vor dem nächsten Validierungstag entscheiden.

**X4 (mittel, Datenqualität).** `skipped_fiktiv.cjs --nachtrag` rechnet MFE/MAE gegen die übergebenen Hoch/Tief-Werte, nicht bar-für-bar bis zum Exit — heute für die ersten beiden Fälle sichtbar falsch (40,55/115,05 statt korrekt 14,75/79,45). Vorschlag: optional `--bars <Pfad>` für bar-für-bar-Rechnung; zusätzlich Pflichtzeile „Positionsmanagement-Vorbehalt" bei Haltedauer >~6 Kerzen.

**X5 (mittel, Prozess — Wiedervorlage W5).** A3-Ausnahme `--screenshot` braucht Zähler mit Hard-Exit nach n Ausnahmen in Folge (Vorschlag n=6). Ergänzung: Pflicht-Screenshot bei jedem neuen Tages-/Session-Extrem (fällt mit X1 zusammen).

**X6 (mittel, Reibung).** (a) `register_touch.cjs` soll `--help` als echten Sonderflag ohne Touch behandeln; (b) Registerpflege nicht im Kerzenschluss-Fenster, sondern nach dem Voll-Check des Slots ausführen; (c) `vollcheck.cjs` gibt bei Slot-Lücke eine begründete Pflichtzeile aus.

**X7 (niedrig, Logging-Infrastruktur).** Dediziertes Quick-Tick-Log und Tweet-Fetch-Verlaufslog einführen — rund 270 Tick-Ereignisse des heutigen Tages sind sonst nicht rekonstruierbar (4.8).

**X8 (niedrig, Backlog).** Die 13 offenen Skipped-Nachträge (11./15.09.) abarbeiten — Voraussetzung für die Q4-Datenbasis von X3, mit `--bars` aus X4 in einem Durchgang möglich.

**Ausdrücklich NICHT vorgeschlagen:** keine Änderung an Option D/13.1×Q-ROT-Präzedenz (Schattenmessung läuft wie vorgesehen, heutiger Fall taugt nicht als Gegenbeispiel); keine Lockerung der 4×-ATR-Diagnose (28 von 39 betroffenen Voll-Checks lagen richtig, die 11 falschen Fälle gehen auf X1, nicht auf die Schwelle); keine Absenkung der RR-Schwelle; keine neue Cooldown-/Verstoßzähler-Mechanik (Levi-Ablehnung 12.08.2026 gilt unverändert); keine Änderung der Anker-Definition selbst (das Nicht-Mitwandern bei haltendem Extrem ist gewollt und hat 16:40-18:55 DE korrekt gebremst — der Fehler war der fehlende Reset nach dem Extrembruch, nicht die Definition).

---
Vorgänger: [[project_testtag_analyse_2026-09-16]] (W1-W8; W4=Q4 und W5=Screenshot-Zähler kehren hier als X3/X5 wieder) · [[project_praezedenz_13_1_vs_qrot_entscheidungsvorlage_2026-09-15]] (Option D, Revisionsklausel „~10 Kollisionsmomente" — heute Moment 1) · [[project_cdp_tab_fixes_committet_2026-09-17]] (Loop lief heute ohne Tab-Zwischenfall) · [[feedback_live_trading]] 7b1 Schritt 3a (Anker-Definition, Grundlage 4.2) + Punkt 12/12.2/12.3 (Stall, Grundlage 4.1) · [[feedback_chartanalyse]] (Anker-Zitat) · [[feedback_regeldisziplin]] (kein Entry-Regelbruch heute) · [[feedback_dont_change_running_system]] (Begründung gegen Option-D-Änderung).
