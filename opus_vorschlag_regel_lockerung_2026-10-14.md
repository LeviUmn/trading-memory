---
name: opus_vorschlag_regel_lockerung_2026-10-14
description: "Opus-Analyse 14.10.2026 (Levi-Auftrag Regel-Lockerung): Kill-Statistik 10 Freeze-Tage (478 VC -> 56 Gate-Laeufe -> 2 zulaessige Einstiege; Killer = levelbasiertes TP1-Fenster 31 FAIL + Q-ROT-Veto 23), Gegenfaktik Kernregel (TP1=1R/TP2=2R fest, SL-Anker, SL-Cap 3xATR): ca. +0,2 R/Trade aber +-0,45 R Unsicherheit, 70 % von einem Tag; Levis 'WR 50 % + 1:1/1:2' rechnerisch nur bei TP2-Quote >= 50 % der TP1-Gewinner positiv; schlankes 10-Regel-Werk, Validierung mit halbem Risiko, 12 Entscheide + Fable-Auftragsentwurf"
metadata:
  type: project
  originSessionId: opus-regel-lockerung-2026-10-14
  modified: 2026-10-14T10:01:36.429Z
---

# Opus-Vorschlag Regel-Lockerung — 14.10.2026

Auftrag Levi (14.10.): Analyse der Testlaeufe, was sich aendern muss, damit wieder getradet wird; Kernregel "TP1 1:1, TP2 min. 1:2, WR min. 50 %", Rest ueberreguliert. Levi hat entschieden, dass gelockert wird — hier der durchgerechnete Weg, mit Widerspruch dort, wo die Rechnung nicht traegt.

**Zeitstempel:** Start Mi 14.10.2026 11:49:40 DE, Ende 12:02 DE. sha1 aller 44 Logs/DB/Register/Bars/loop_archiv 11:49:54 vorher = 11:57:56 und 12:01:29 nachher **IDENTISCH** (z. B. gate_check_log bc95bdc8…, kombi 5df86668…). Alle Skriptlaeufe auf der Kopie `%TEMP%\opus_1014\scripts`, eigene Lese-Skripte in `%TEMP%\opus_1014\ana` (sim.cjs, sim2.cjs, grp.cjs, kill.cjs). Kein git add/commit/push, kein test:e2e, kein CDP, kein h1-end-Lauf, keine Aenderung an Skripten/Prompts/Regeln; einzige Schreibvorgaenge: diese Datei + eine Indexzeile.

---

## Kurzfazit (10 Zeilen)

1. **Levi hat beim Befund Recht:** In 10 Freeze-Tagen (478 Voll-Checks) hat das Regelwerk genau **2 Einstiege** erlaubt (28.09. 19:32 GELB, 13.10. 19:36 GRUEN; zusammen -0,07 R). Mit 0,2 Trades/Tag kann der eigene Risikoplan (20 Trades/Monat) nie aufgehen.
2. **Was blockiert hat, ist klar messbar:** 56 Gate-Laeufe → 31 FAIL am **levelbasierten TP1-Fenster** (RR-Gate 22, TP-Realismus 6, beide 3) + 23 PASS durch das **Q-ROT/UNBEKANNT-Veto** gesperrt. Davor: 38 % aller qualifizierten Momente waren geometrisch tot (SL-Anker > 3x ATR), der Rest scheiterte an "kein Trigger", Zeitbox/Retest und vorweggenommenem Q-ROT.
3. **Ein Filter war richtig:** Ideen mit SL > 3x ATR liefen dedupliziert **Ø -0,40 R** (n 14). Diese Grenze muss bleiben — als SL-Cap statt als Level-Fenster.
4. **Die zwei Hauptkiller kosteten eher Geld als sie sparten:** abgelehnte Ideen mit SL ≤ 3x ATR liefen mit festen 1R/2R-Zielen Ø **+0,15 bis +0,27 R** (n 27 dedupliziert). Aber: ±0,4 R Unsicherheit, kein Beleg.
5. **Gegenfaktik "nur Kernregel" (Gate-Ideen, SL-Cap 3x ATR, 50 % @1R + BE, Rest @2R, 2 Pkt Spread):** n 16, Σ **+3,7 R**, Ø **+0,23 R** (95 %-Band ±0,45), **ohne besten Tag 05.10. nur +0,08 R**; Trendtage Ø +0,44, Rangetage Ø +0,07. "Jeder 2/2-Moment ohne Trigger" liefert ≈ 0 R.
6. **Levis Rechnung traegt so nicht:** "WR 50 % mit 1:1 und 1:2" ergibt nur dann +0,25 R, wenn JEDER TP1-Gewinner auch TP2 erreicht. Real (Historie: TP2/TP1 ≈ 0,27) ergibt das Schema bei 50 % Trefferquote **-0,1 bis -0,15 R je Trade** — genau das alte Payoff 0,94:1. Break-even bei 50 %: mindestens die **Haelfte der TP1-Gewinner muss TP2 erreichen** (plus Kosten).
7. **Kosten sind die unbekannte Groesse:** 2 Pkt Spread kosten im Schnitt ca. 0,03-0,1 R, 5 Pkt drehen die 1R-Variante ins Minus. Beim KO-Zertifikat kann 1 Cent Spread 5-20 NAS-Punkten entsprechen — **vor dem Echtgeld-Start messen**.
8. **Empfehlung:** Lockern ja — auf ein **10-Regel-Werk**: Dual-Gate 2/2 + 1H nicht dagegen, ein Trigger-Satz, SL = Anker + 0,5 ATR (Floor 1,5 / **Cap 3,0 ATR**), **TP1 = exakt 1R (50 %), danach SL auf BE, TP2 = exakt 2R**, Zeit-Exit 20:00, max. 3 Trades/2R Tagesverlust, Sperrfristen, 75 € Risiko. Gestrichen: Q-ROT-Veto, Level-Fenster, TP2-Realismus, 13.1/Chasing, Zeitbox/Aussichtslos, Stale, 8c2, Stall-Exit, Halbierungen.
9. **Validierung ohne Mammut-Freeze:** 3 fiktive Prozesstage (nur Technik), dann Echtgeld mit **halbem Risiko (37,50 €) fuer 10 Trades**, danach 75 €; harte Stopps bei -8 R kumuliert oder Ø ≤ -0,3 R nach 15 Trades.
10. **Ehrlich:** "Ueberreguliert" ist fuer den Prozess belegt, eine Kante fuer das gelockerte Werk ist es NICHT. Die Lockerung oeffnet die Tuer, beweist nichts — die naechsten 20-40 echten Trades entscheiden.

---

## 1. Kill-Statistik (Evidenz)

### 1.1 Trichter ueber die 10 bewertbaren Freeze-Tage (25.09.-13.10.)

Quellen: `momente_log.jsonl` (jeder VC = 1 Moment, Qualifikation = 15m-NAS = QQQ und 1H nicht dagegen), `vollcheck_log.jsonl` (Fazit), `gate_check_log.jsonl` (live, PASS/FAIL), `skipped_setups_fiktiv.jsonl`.

| Tag | VC | Dual-Gate gespalten | 1H dagegen | qualifiziert | davon ALT-SL > 3x ATR | Gate-Laeufe | FAIL rr allein / tp allein / beide | PASS Q-ROT/UNB (Veto) | PASS GRUEN/GELB | Typ (Effizienz 15:30-20:00) |
|---|---|---|---|---|---|---|---|---|---|---|
| 25.09. (Teiltag) | 27 | 0 | 2 | 25 | 25 | 0 | – | 0 | 0 | Range 0,35 |
| 28.09. | 52 | 8 | 9 | 35 | 30 | 6 | 1/0/1 | 3 | 1 (GELB) | Range 0,24 |
| 29.09. (Teiltag) | 23 | 0 | 10 | 13 | 0 | 3 | 2/0/0 | 1 | 0 | Trend 0,85 |
| 30.09. | 58 | 0 | 0 | 58 | 15 | 2 | 1/0/0 | 1 | 0 | Trend 0,72 |
| 01.10. (Teiltag) | 44 | 1 | 6 | 37 | 3 | 4 | 2/0/0 | 2 | 0 | Range 0,00 |
| 02.10. | 57 | 4 | 0 | 53 | 12 | 5 | 2/0/0 | 3 | 0 | Range 0,35 |
| 05.10. | 59 | 0 | 1 | 58 | 33 | 5 | 4/0/0 | 1 | 0 | Trend 0,87 |
| 07.10. | 63 | 6 | 15 | 42 | 6 | 12 | 2/2/1 | 7 | 0 | gemischt 0,54 |
| 08.10. (Teiltag) | 38 | 0 | 0 | 38 | 11 | 12 | 5/2/0 | 5 | 0 | Trend 0,58 |
| 13.10. | 57 | 24 | 9 | 24 | 12 | 7 | 3/2/1 | 0 | 1 (GRUEN) | Range 0,45 |
| **Summe** | **478** | **43** | **52** | **383** | **147 (38 %)** | **56** | **22/6/3 = 31** | **23** | **2** | 4 Trend / 6 Range |

**Trichter:** 478 VC → 383 qualifiziert (80 %) → 236 geometrisch offen (SL ≤ 3x ATR) → 56 Gate-Laeufe (≈ 43 Ideen dedupliziert, Datum+Richtung+Anker+TP1) → 25 PASS → **2 zulaessige Einstiege**.

### 1.2 Wer hat was getoetet — allein vs. mitursaechlich

| Regel | Ebene | allein ausschlaggebend | mitursaechlich / Hinweis |
|---|---|---|---|
| **RR-Gate 8b (TP1-Level ≥ 1R)** | Gate | **22 Laeufe** | +3 zusammen mit TP-Realismus |
| **TP-Realismus 8b1 (TP1 ≤ 3x ATR, Zonen)** | Gate | **6 Laeufe** | +3 mit RR; inhaltlich dieselbe Geometrie: SL weit + kein Level im Fenster |
| **Q-ROT/UNBEKANNT-Veto (7b1a Option A/D)** | nach PASS | **23 Laeufe** | zusaetzlich 112 VC-Fazits, in denen "Q-ROT-Lage"/Q2 als Grund fuer "kein Gate" mitgenannt ist (Vorwegnahme ohne Lauf) |
| SL-Anker-Geometrie > 3x ATR (Vorpruefung, "TP1-Fenster leer", AUSSICHTSLOS, V5 "TOT") | vor Gate | **147 qualifizierte Momente** objektiv (ALT-SL > 3x ATR) | 140 Fazits nennen Geometrie/Fenster/Anker |
| Dual-Gate gespalten | vor Gate | 43 VC | 13.10. allein 24 |
| 1H-Override (E2 / 3/4-Blockade) | vor Gate | 52 VC (E2 2 Episoden 13.10., IBF: Kurs nie jenseits 1H-EMA) | kostete laut Opus 13.10. nichts |
| "kein (frischer) Trigger" | vor Gate | 21 VC als einziger genannter Grund | 180 Nennungen |
| Zeitbox / Retest-Bedingung / VERFALLEN / Wiederholungs-Sperre | nach FAIL/PASS-ROT | 4 allein | 160 Nennungen (Folge-VCs nach jedem Gate) |
| 13.1 / Chasing / Punkt 11 | Kontext | 1 allein | 207 Nennungen (meist Konsequenz-Text "13.1 beobachten"); **0 echte 13.1-Konflikte** (rk13_1_filter) |
| Sperrfristen/Halbierung/Stichtag/Pre-Market | Zeit | 3 allein | 32 Nennungen |
| Stale-Vorbedingung | vor Gate | 0 allein | 1 Nennung; 2 Hard-Exits (`--stale-n`) |
| Abbruch (Exit 1, Eingabefehler) | technisch | – | Freeze 6 Laeufe; Vor-Freeze 36 |

Die Stichwortzaehlung der Fazit-Texte ist grob (Mehrfachnennung je VC die Regel, deshalb wenige "allein"-Faelle) — die harten Zahlen sind die Gate-Ebene und die ALT-Geometrie.

### 1.3 Fruehere fiktive Testtage

- **01.-04.09.:** keine gate_check-Rohdaten (Log beginnt 10.09.), nur Memos (0 Trades, 58-84 VC, 04.09. 0/4 Trigger PASS) → nicht nachrechenbar.
- **11.09.:** 49 Laeufe: FAIL rr 16 / tp 2, PASS 18 alle Q-ROT/UNB, 11 Abbrueche. **15.09.:** 31 FAIL (30x rr+tp, SL 3,9x ATR = Geometrie). **16.09. (FOMC):** 2 PASS GELB fiktiv eingestiegen, beide -1 R in der Sperrfrist (Ursprung der W1-Sperrfrist — eine echte Schutzregel). **17.09.:** 3 PASS Q-ROT/UNB. **21.09. (Trendtag +480 Pkt):** 6 PASS Q-ROT/UNB, fast alle TP1-HIT.
- Muster ueber alle Tage gleich: **FAIL = Geometrie/Level-Fenster, PASS = Q-ROT-Veto.**

---

## 2. Gegenfaktisch: Was haette die Kernregel gebracht?

### 2.1 Methode

- **Kernregel-Simulation** (`ana/sim2.cjs` auf Gate-Ideen, `ana/sim.cjs` auf allen qualifizierten Momenten): Entry = Gate-/VC-Kurs, SL = ALT-Anker + 0,5 ATR (mind. 1,5 ATR), **TP1 = exakt 1R, TP2 = exakt 2R** (nicht Level), Bar fuer Bar auf `nas100_5m_<datum>.json` ab der Kerze nach dem Moment, SL vor TP in derselben Kerze, Horizont 20:00 DE (offen = Markt-Bewertung), **eine Position zur Zeit, max. 3 Trades/Tag, Stopp bei Tages-R ≤ -2**. Spread: Short-SL/TP am Ask (Bid + Spread), Long als Kosten.
- Exit-Schemata: (A) 1R voll, (B) 2R voll, (C) **Levi: 50 % @1R, Rest SL auf BE, Ziel 2R**, (D) 50 % @1R, Rest mit Original-SL, Ziel 2R.
- **Methodenluecken (ehrlich):** Gate-Kerze nach Entry nicht intrabar geprueft (08.10. 18:16 war ein Intrabar-SL; in der 2-Pkt-Variante wird er als SL erfasst, in der 0-Pkt-Variante faelschlich +1,5 R); OFFEN-Werte sind Buchwerte um 20:00; Momente/Ideen nur von Tagen mit Bars; Ideen entstehen nur dort, wo Sonnet ein Gate fuhr (unter Kernregel gaebe es mehr Trigger — die Momente-Simulation ist die Obergrenze dafuer).

### 2.2 Ergebnisse Gate-Ideen (25.09.-13.10., 52 roh)

| Variante | n | Σ R | Ø R (95 %-Band) | ohne besten Tag | Bemerkung |
|---|---|---|---|---|---|
| ohne SL-Cap, Spread 0, Schema C | 15 | +1,18 | +0,08 (±0,49) | -0,08 | weite SL ziehen runter |
| **SL-Cap 3x ATR, Spread 0, Schema C** | 17 | +7,12 | +0,42 (±0,42) | +0,30 | enthaelt 18:16-Fehler (+1,5 statt -1) → korrigiert ≈ +4,6 / Ø +0,27 |
| **SL-Cap 3x ATR, Spread 2, Schema C** | 16 | **+3,71** | **+0,23 (±0,45)** | **+0,08** | Hauptwert; 18:16 korrekt als SL |
| SL-Cap 3x ATR, Spread 2, Schema A (1R voll) | 17 | +0,39 | +0,02 (±0,42) | -0,10 | |
| SL-Cap 3x ATR, Spread 2, Schema B (2R voll) | 14 | +2,74 | +0,20 (±0,62) | -0,04 | |
| SL-Cap 3x ATR, Spread 5, Schema C | 17 | +1,46 | +0,09 (±0,45) | -0,07 | Kosten fressen fast alles |
| SL-Cap 2,5x ATR, Spread 2, Schema C | 15 | +4,83 | +0,32 (±0,44) | +0,17 | |
| SL-Cap 3x ATR, Spread 2, max. 2 Trades/Tag | 15 | +3,07 | +0,20 (±0,47) | +0,04 | |

Hauptwert je Tag (Cap 3, Spread 2, Schema C): 28.09. -0,09 | 29.09. +0,44 | 30.09. +2,03 | 01.10. -0,50 | 02.10. +1,20 | **05.10. +2,59** | 07.10. +0,14 | 08.10. -2,00 | 13.10. -0,10. **Ein Tag (05.10.) traegt 70 % der Summe.** Trendtage (29.09., 30.09., 05.10., 08.10.): n 7, Σ +3,06, Ø +0,44. Rangetage (28.09., 01.10., 02.10., 07.10., 13.10.): n 9, Σ +0,65, Ø +0,07. Haltedauer: TP-Treffer meist 2-11 Kerzen (10-55 Min), 5 von 16 Positionen um 20:00 noch offen.

### 2.3 Je Ablehnungsgrund (dedupliziert, jede Idee einzeln, ohne Sequenz)

| Gruppe | n | Log (TP1 = Level, voll) Σ | Kern 1R voll Ø | Kern Schema C Ø |
|---|---|---|---|---|
| FAIL rr/tp, SL ≤ 3x ATR | 11 | +1,94 | +0,22 | +0,15 |
| **FAIL rr/tp, SL > 3x ATR** | 14 | **-6,32** | **-0,40** | **-0,41** |
| PASS Q-ROT/UNB (Veto), SL ≤ 3x ATR | 16 | +2,91 | +0,25 | +0,27 |
| PASS GRUEN/GELB (erlaubt) | 2 | -0,07 | -0,04 | -0,04 |

Spread 2 aendert die Gruppenwerte kaum (±0,01). Mit den Vor-Freeze-Tagen (11.09.-21.09.) wird Q-ROT ≤ 3 ATR noch positiver (n 21, Ø +0,40/+0,50) — getrieben vom Trendtag 21.09.

**Lesart:** Das einzige Kriterium, das in beiden Datensaetzen klar trennt, ist **SL-Distanz ≤ 3x ATR**. Level-Fenster und Q-ROT haben Ideen entfernt, die im Schnitt nicht schlechter waren als die erlaubten.

### 2.4 Ohne Trigger (jeder qualifizierte Moment, `sim.cjs`)

ALT-Anker, Cap 3x ATR, Spread 0, Entries 15:30-19:30: 1R voll n 19 Ø +0,24 (ohne besten Tag +0,09); **Schema C n 20 Ø -0,03**; Spread 2: Schema C Ø -0,05. **Entries erst ab 15:45: alle Schemata negativ** (Schema C Ø -0,27). FLOOR (1,5x ATR fest): Schema C Ø +0,07, ab 15:45 Ø -0,16. **Folge:** "einfach jede 2/2-Lage handeln" ist ≈ 0 und haengt an der Eroeffnungsphase — ein Trigger braucht es, und die 15:30-16:00-Entries sind in diesen Daten ein grosser Teil des Ergebnisses (n klein, keine Regel daraus ableiten, nur messen).

---

## 3. Was Levis Kernregel rechnerisch bedeutet

Bezeichnungen: p = Anteil Trades, die TP1 (1R) vor dem SL erreichen ("Trefferquote"); q = Anteil der TP1-Gewinner, die danach auch TP2 (2R) erreichen; c = Kosten in R (Spread/SL-Distanz).

| Exit-Schema | Erwartungswert je Trade | Break-even-Trefferquote p | bei p = 50 % |
|---|---|---|---|
| 1R voll | 2p - 1 - c | 50 % + c/2 (c 0,05 → 52,5 %) | **-c** |
| 2R voll | 3p - 1 - c (p = Trefferquote fuer 2R!) | 33 % + c/3 | +0,5 - c, aber p fuer 2R ist viel kleiner als fuer 1R |
| **Levi: 50 % @1R, Rest BE, Ziel 2R** | p(0,5 + q) - (1 - p) - c | (1 + c)/(1,5 + q) | **0,5q - 0,25 - c** |
| 50 % @1R, Rest Original-SL, Ziel 2R | p(1,5q') - (1 - p) - c | (1 + c)/(1 + 1,5q') | 0,75q' - 0,5 - c |

Fuer Levis Schema bei p = 50 %: **q = 0,27 (Historie TP2 3 / TP1 11) → -0,12 R - c; q = 0,43 (Simulation) → -0,04 R - c; q = 0,5 → -c; positiv erst ab q > 0,5 + 2c (≈ 0,6).** Break-even-p bei q 0,3: **55,6 %** (c 0,05: 58 %); bei q 0,5: 50 % (52,5 %).

**Ehrlicher Widerspruch an Levi:** "TP1 1:1 + TP2 1:2 bei WR 50 %" ist nur dann ein Gewinnsystem (+0,25 R), wenn praktisch jeder Gewinner beide Ziele erreicht. Realistisch erreicht nur ein Drittel bis knapp die Haelfte der TP1-Gewinner auch TP2 → der Durchschnittsgewinn liegt bei 0,8-0,95 R statt 1,5 R. Das ist **exakt das historische Payoff 0,94:1**. Mit WR 50 % ist dieses Schema ein Nullsummenspiel minus Kosten. Damit es traegt, braucht es **p ≥ 55-58 % ODER q ≥ 0,5-0,6** — oder beides etwas. In der Simulation (Cap 3, Spread 2) lag p (TP1 bis 20:00 erreicht) bei 7/16 = 44 %, q bei 3/7 = 43 %; dass trotzdem +0,23 R herauskam, liegt an den 5 offenen Positionen mit Buchgewinn und daran, dass nur 4/16 den SL trafen — also am 20:00-Zeit-Exit, nicht an der Trefferquote. **WR 50 % ist als Zielgroesse richtig gesetzt, als Erfolgsbeweis reicht sie nicht — gemessen werden muessen p, q und c getrennt.**

Kostenwarnung: Bei 2 Pkt Spread und SL 50-90 Pkt ist c ≈ 0,02-0,04 R; bei 5 Pkt bereits ≈ 0,06-0,1 R (Tabelle 2.2: Schema C faellt von +0,23 auf +0,09). Beim KO-Zertifikat (z. B. #43: 5,506 €, Hebel 6) entspricht 1 Cent Geld/Brief rechnerisch ≈ 0,18 % des Scheins ≈ 0,03 % des Basiswerts ≈ **9 NAS-Punkte** — Schaetzung, nicht gemessen. Das kann groesser sein als jede Kante in diesen Daten. **Vor dem Echtgeld-Start: realen Spread des genutzten Produkts in Punkten bestimmen.**

---

## 4. Regelinventar (aktive Regeln, Zweck, Herkunft, Kosten, Empfehlung)

Quellen: `last_loop_prompt_2026-10-13.txt` (488 Zeilen, 158 verschiedene CLI-Argumente, jede Minute voll gelesen), `gate_check.cjs` (517 KB, ein Live-Aufruf 13.10. mit **43 Argumenten**, 9 Gate-Pruefungen + 4 Q-Faktoren + 24 Anzeigezeilen), `vollcheck.cjs` (280 KB, ~50 Pflicht-/Situativfelder), `feedback_live_trading.md` (320 KB), `feedback_chartanalyse.md` (166 KB). Schon der Umfang ist ein Befund: 2 Hard-Exits pro Tag durch fehlende Pflichtfelder sind normal.

### 4a. Harte Risiko-/Kapitalschutzregeln — NICHT verwaessern

| Regel | Zweck | Herkunft/Evidenz | Kosten Testtage | Empfehlung |
|---|---|---|---|---|
| Risiko je Trade 1,5 % von 5.000 € = 75 € (size.cjs) | Verlustbegrenzung | Kapitalmodell 10.09. | 0 | **BEHALTEN** (Start: halbes Risiko, s. Abschnitt 7) |
| SL immer bei Order, Broker-SL/TP exklusiv (Scalable) | Katastrophenschutz | #14 SL-Blockade, Broker-Playbook | 0 | **BEHALTEN**; TP per Alarm/manuell |
| Tagesverlust 2R (150 €) / cooldown 2 Verluste in Folge | Tilt-Schutz | 10.09., #12 Cooldown-Verstoss | 0 | **BEHALTEN** |
| Max. 3 Trades/Tag, 1 Position gleichzeitig | Overtrading | 23.06. | 0 | **BEHALTEN** (1 Position statt "1-2") |
| Notbremse Stufe 1 (10 % = 500 €) | Drawdown | 10.09. | 0 | **BEHALTEN** |
| Entry-Sperrfrist Hochrisiko-Events (FOMC/CPI/NFP/PCE/Zinsentscheid, bis 30 Min danach) + Schock-Tier No-Trade | Event-Gaps | 16.09. FOMC 2x -1R; #42/#43 Opex/Schock | 3 Momente | **BEHALTEN** (Liste kurz, Kalender beim Start-Update) |
| SL-Floor ≥ 1,5x ATR (8c) | kein Rausch-Stop | ATR-Kalibrierung 23.07. | wenige | **BEHALTEN** |
| Order-Bestaetigung, Broker-Wert-Prioritaet, Stueckzahl-Rueckrechnung | Ausfuehrungsfehler | #35 u. a. | 0 | **BEHALTEN** (operativ) |
| Keine Entries nach Terminal 20:00 / Solo-Mandat | unbeaufsichtigte Position | 26.08. | 0 | **BEHALTEN** |

### 4b. Qualitaets-/Filterregeln

| Regel | Zweck | Herkunft | Kosten (Freeze) | Empfehlung |
|---|---|---|---|---|
| Dual-Gate 2/2 (NAS100-15m + QQQ-15m vs. EMA50) | Richtung | Kernsetup seit 07/2026 | 43 VC | **BEHALTEN** als Richtungsfilter; ohne VWAP/RVOL/Spike-Felder |
| 1H-Override / E2 (kein Trade gegen 1H-EMA50) | Trendkontext | 03.09. entschieden | 52 VC, E2 kostete 13.10. nichts | **BEHALTEN** (eine Zeile) |
| RR-Gate 8b mit TP1 = registriertes Level ≥ 1R | Mindest-Auszahlung | #17 (TP1 0,32:1) | **22 + 3 Laeufe** | **ERSETZEN**: TP1 = exakt 1R, RR damit per Konstruktion 1 |
| TP-Realismus 8b1 (TP1 ≤ 3x ATR, Zone 2 halbe Pos.) + TP1-Fenster/Level-Pflicht | Erreichbarkeit | #38 | **6 + 3 Laeufe**, 147 Momente | **ERSETZEN** durch SL-Cap ≤ 3x ATR (gleiche Geometrie, ohne Level); Zone-2-Halbierung STREICHEN |
| TP2-Realismus 8b2 (TP2 ≥ 2:1 an echtem Level) | TP2 nicht billig | 24.08. | indirekt | **ERSETZEN**: TP2 = exakt 2R |
| Zonen-RR-Selbsttest GC-5 | Konsistenz | Code-Selbsttest | 0 | **STREICHEN** |
| SL-Anker ALT (Struktur + 0,5 ATR, MAX mit Floor) | strukturierter SL | 07.09., einzige belegte Variante (Ø +0,14, knapp) | – | **BEHALTEN**, aber Anker in einem Satz definieren |
| SL-Cluster 8c2 (Wick-Zonen-Zuschlag, `--cluster-level` Pflicht) | Stop-Hunt | 21.08. (#42/#43) | Pflichtfeld, Hard-Exits | **NUR SCHATTEN** (Anzeige) |
| Anker-Reset-Pflicht P3/X1, Begruendungspflichten V9 | Anker frisch halten | 28./29.09. | Hard-Exits, 15 Anker-Log-Eintraege/Tag | **STREICHEN** → Regel "Anker = letzter bestaetigter 5m-Swing vor Entry" |
| **Q-Score Q1-Q4 + Q-ROT-Veto (Option A/D), GELB halbe Position** | "bessere Trades" | Studie 24.08. | **23 Laeufe + 112 vorweggenommene VC** | **STREICHEN als Gate**, Q-Score automatisch als Schatten mitloggen |
| 13.1 aktive Chasing-Pflicht, Punkt 13 Chasing halbe Pos., Punkt-11 Serien-Lesart, k-ohne-signal | Momentum nicht verpassen / nicht hinterherlaufen | 22./30.07., 04.09. | 0 echte Konflikte, viele Pflichtfelder | **STREICHEN** (Hinterherlaufen faengt der SL-Cap ab) |
| Retest-Zeitbox VC+2, AUSSICHTSLOS-/UNLOESBAR-Sperre, Wiederholungs-Fingerabdruck | Dubletten/Hartnaeckigkeit | 15.09. V1-V7 | 160 Nennungen | **STREICHEN** → "neue Idee nur bei neuem Trigger" |
| Stale-Vorbedingung 7b1c (`--stale-n`) | veraltetes Gate | 27.08. | 2 Hard-Exits, 1 Nennung | **STREICHEN** |
| 8a2 Zwei-Kerzen-Bestaetigung, 8a3 Volumen-Soft-Gate, 8a4/8a5 Anzeigeklassen | Fakeouts | 22.-31.08. | Teil "kein Trigger" | **ZUSAMMENFASSEN** in den Trigger-Satz; Rest Schatten |
| 8d Regime-Chop (manuell) + Chop-Verdacht | Range meiden | 02.07. | unkalibriert | **NUR SCHATTEN** (Rangetage Ø +0,07 — Daten reichen nicht fuer ein Gate) |
| Halbierung 15:30-16:00 | Open-Fakeouts | 22.06./23.07., eigene Review n 3 "nicht gedeckt" | Groesse | **STREICHEN**; nur Order-Sperre erste 5-Min-Kerze 15:30 behalten |
| Stall-Exit 12/12.1a/12.3, Punkt-11-Reversal-Exits, 3h-Regel | Gewinn sichern | 21.-31.07. | – | **STREICHEN** zugunsten fester Ausgaenge (Historie: "Verluste voll, Gewinne zu klein", Ø -0,51 R) — einzige Ausnahme Not-Exit bei Event/Broker-Problem |
| Register-Pflege B5, Fib-Extension, Rundzahl-Baender, V11 | Level fuer TP1/Q4 | 01.10. | 53 VC "MIT LUECKEN" (Fib) | **NUR INFO** — ohne Level-TP entfaellt die Pflicht |
| MTF-Vierschritt inkl. QQQ-1H, VWAP, RVOL, VIX, ADX, Tagesrange-Felder | Kontext | 07-09/2026 | Zeit/Hard-Exits | **REDUZIEREN** auf NAS 15m/1H + QQQ 15m + ATR |
| Tweet-Check alle 10 Min (Doppelbedingung, Stempel) | News | 30.06. | Prozesslast | **REDUZIEREN**: Kalender beim Start-Update + Check vor jedem Entry |
| Messfeld-Pflichten (A3, `--grund-*`, Kurzblock, Archiv, vc_nr) | Beweisbarkeit | Sept. | 43 Argumente je Gate | **REDUZIEREN** auf ~10 Felder je Check |

---

## 5. Schlankes Regelwerk (Vorschlag, 10 Regeln)

1. **Kernregel:** Jeder Trade hat TP1 = Entry ± 1R und TP2 = Entry ± 2R (R = SL-Distanz). Bei TP1: 50 % schliessen, SL auf Entry (BE). Rest bis TP2, BE-Stop oder Zeit-Exit.
2. **Richtung:** Nur wenn NAS100-15m-Close und QQQ-15m-Close auf derselben Seite ihrer EMA50 liegen (letzte geschlossene Kerze) und die letzte geschlossene NAS100-1H-Kerze nicht auf der Gegenseite der 1H-EMA50 schliesst.
3. **Trigger (ein Satz):** Eine geschlossene 5m-Kerze schliesst in Gate-Richtung jenseits der 5m-EMA50 oder jenseits eines Tageslevels (PDH/PDL, Pivot, Session-Hoch/-Tief) — Entry zum Schluss dieser Kerze bzw. zur naechsten Eroeffnung.
4. **SL:** letzter bestaetigter 5m-Swing (HL fuer Long, LH fuer Short) vor dem Entry + 0,5x ATR(14, 5m), mindestens 1,5x ATR. **Liegt der SL weiter als 3,0x ATR weg → kein Trade** (warten auf neuen Swing/Retest).
5. **Positionsgroesse:** 75 € Risiko (Start 37,50 €, s. Abschnitt 7) / SL-Distanz, per size.cjs; keine Halbierungen, keine Q-Abstufungen.
6. **Zeitfenster:** Entries 15:35-19:30 DE (im DST-Divergenzfenster 26.-30.10. eine Stunde frueher), Zeit-Exit aller Reste 20:00.
7. **Tageslimits:** max. 3 Trades, 1 Position zur Zeit, Stopp nach 2 Verlusten oder -2R Tagessumme.
8. **Sperrfristen:** kein Entry von 15 Min vor bis 30 Min nach FOMC/CPI/NFP/PCE/Zinsentscheid/Fed-Chef; Opex-/Schock-Tage (ATR ≥ 2x Basis) kein Trade.
9. **Broker:** SL sofort als Order; TP1/TP2 per Alarm bzw. manuell (SL/TP exklusiv); Stueckzahl rueckrechnen; Broker-Wert gilt.
10. **Messung statt Gate:** Q-Score, Chop-Verdacht, 13.1-Lage, Register-Level, AUTO/FLOOR werden automatisch mitgeloggt — sie entscheiden nichts.

**Was im Loop wegfaellt:** Q-ROT-Veto, Level-TP1-Fenster, TP2-Realismus, Zonen, 13.1/Chasing/Punkt 11, Zeitbox/Aussichtslos/Wiederholung, Stale, 8c2-Pflicht, Anker-Reset-Buerokratie, Fib/Register-Pflichten, Stall-Exit, Halbierungen, ~30 Pflichtargumente, Kurzblock-/Archiv-Zeremonie. Voll-Check bleibt alle 5 Min (Richtung + Trigger + SL-Vorschau), Quick-Tick jede Minute nur bei offener Position oder Trigger-Naehe.

**Anpassungsliste (nur Liste + Aufwand, KEINE Umsetzung):**
| Baustein | Aenderung | Aufwand |
|---|---|---|
| `scripts/kern_gate.cjs` (neu, statt Umbau des 517-KB-gate_check) | Eingaben: dir, entry, sl-anker, atr, blackout, testtag; rechnet SL/TP1/TP2/Groesse, PASS/FAIL nur fuer Cap/Floor/Sperrfrist/Tageslimit; loggt Q-Score u. a. als Schatten | 0,5-1 Tag |
| `scripts/kern_check.cjs` (neu, statt vollcheck.cjs) | ~10-12 Felder, Ausgabe 5-8 Zeilen | 1 Tag |
| `scripts/loop_prompt.cjs` | neues Template "kern", Ziel < 80 Zeilen | 0,5 Tag |
| `scripts/position_tick.cjs` | nur TP1/BE/TP2/Zeit-Exit + SL-Status | 0,5 Tag |
| Nachtrag/Messung (`skipped_nachtrag`/neu `kern_nachtrag.cjs`) | Bar-fuer-Bar mit Spread-Parameter, Gate-Kerze intrabar (1m-Bars), p/q/c-Report | 0,5-1 Tag |
| `size.cjs`, `cooldown_check.cjs`, `notbremse_check.cjs`, `trade_db/add_trade` | unveraendert (Risiko-Parameter halbes Risiko per Flag) | – |
| Memory: neues `regelwerk_kern_2026-10.md`; alte Regel-Memos als "archiviert, gilt nicht mehr" markieren (nicht loeschen) | Doku | 0,5 Tag |
| DST-Fix bis 19.10. | in den neuen Skripten von Anfang an (US-Open 14:30 DE 26.-30.10.) | im Bau enthalten |

---

## 6. Risiken und Gegenargumente — ehrlich

**Wo Levi Recht hat:**
- Das Regelwerk ist als Prozess nachweislich ueberreguliert: 2 Einstiege in 10 Tagen, 43 Argumente pro Gate, 488 Zeilen Prompt pro Minute, taeglich Hard-Exits durch Pflichtfelder, Q-ROT-Veto auf duenner Datenbasis (Studie 24.08., nie belegt). Der Freeze hat nach seinen eigenen Regeln genau eine Variante (ALT) knapp belegt — die Zusatzfilter hat er nicht belegt.
- Die beiden Hauptkiller (Level-Fenster, Q-ROT) haben in den vorhandenen Daten keine schlechteren Ideen aussortiert als die erlaubten.

**Wo Levi nicht Recht hat bzw. die Daten es nicht hergeben:**
- **"Lockern = wieder Gewinne" ist nicht belegt.** Die beste gegenfaktische Zahl (+0,23 R/Trade) hat ein 95 %-Band von ±0,45 R und haengt zu 70 % an einem Tag. Ohne den 05.10. bleibt +0,08 R. Mit 5 Pkt Kosten ≈ 0.
- **Marktlage ist mitursaechlich:** 6 von 10 Tagen waren Range-/Klebetage; dort lag auch die Kernregel bei Ø +0,07 R. Die Testserie hatte zu wenige Trendtage.
- **"WR 50 % + 1:1/1:2" reicht rechnerisch nicht** (Abschnitt 3). Ohne TP2-Quote ≥ 50 % der TP1-Gewinner oder Trefferquote ≥ 55-58 % ist das Schema nach Kosten negativ — die Historie (Payoff 0,94:1, Phase 3 WR 44 %, -57,77 €, Shorts Payoff 0,74:1) zeigt genau dieses Bild.
- **Die alten Verluste kamen nicht aus fehlenden Filtern allein:** #40, #42, #43 hatten Dual-Gate + RR-Gate + TP-Realismus PASS und liefen trotzdem voll in den SL; #36/#42 hatten Regelbrueche. Was #37/#39 (+0,15 R/+0,49 R) klein machte, waren fruehe manuelle Exits (Stall/Trailing). Die Lockerung loest das nur, wenn **feste Ausgaenge** wirklich eingehalten werden.

**Was schiefgehen kann:**
1. Rueckfall in manuelle Fruehausstiege → Gewinne wieder < 1R, Verluste voll (Ø -0,51 R). Gegenmittel: Regel 1 + keine Stall-/Punkt-11-Exits; Abweichung = Regelbruch.
2. Mehr Trades an Rangetagen → viele kleine Verluste. Gegenmittel: Tageslimits; Chop nur messen, nach 20 Trades pruefen.
3. Kosten unterschaetzt (KO-Spread) → Kante weg. Gegenmittel: Spread vor Start messen; Abbruchkriterium.
4. Short-Seite historisch schwach (Payoff 0,74:1) — die Serie war ueberwiegend short. Beobachten, kein Verbot.
5. Neue Regel-Spirale: nach den ersten Verlusten sofort wieder Filter draufsetzen. Gegenmittel: Regel-Freeze fuer die Validierungsphase (nur Abbruch oder Fehlerkorrektur), Punkt 14 (Regeltempo drosseln) gilt.

---

## 7. Validierung (ohne neuen Mammut-Freeze)

1. **Prozesstage (fiktiv), 3 Tage:** nur das neue Werk, Ziel Technik: 0 Hard-Exits durch Pflichtfelder, jeder Trigger mit Gate-Lauf, Nachtraege mit Spread. **Kein Ergebnis-Kriterium** (3 Tage beweisen nichts). Parallel laeuft das alte Regelwerk NICHT im Loop mit — stattdessen bewertet `kern_gate.cjs` jeden Kern-Trigger automatisch auch nach alter Logik (Level-Fenster, Q-Score) und loggt "alt haette: PASS/FAIL/ROT" → Schatten-Vergleich ohne Mehrarbeit.
2. **Echtgeld-Start, Stufe 1:** 10 Trades mit **halbem Risiko (37,50 €)**. Spread/Slippage je Trade in Punkten erfassen.
3. **Stufe 2:** ab Trade 11 volles Phase-3-Risiko 75 €, bis Trade 40.
4. **Abbruch (sofort zurueck in Analyse, kein Weiter-Lockern):** kumuliert ≤ **-8 R oder -400 €** (s. Punkt 7); oder nach 15 Trades Ø ≤ -0,3 R; oder ≥ 2 Regelbrueche bei Exits in 10 Trades; oder realer Spread > 0,1 R im Schnitt.
5. **Zwischenpruefung nach 20 Trades:** Ø R nach Kosten, p (TP1-Quote), q (TP2/TP1), Anteil Zeit-Exits, Trend/Range getrennt, Schatten "alt haette". Weiter nur bei Ø R ≥ 0 und keinem Abbruchkriterium — **nicht** als Beleg einer Kante (Standardfehler bei n 20 ≈ ±0,25 R), sondern als "kein Schaden".
6. **Entscheidung nach 40 Trades:** Kante gilt als vorlaeufig belegt bei Ø R ≥ +0,10 UND Ø R ohne besten Tag ≥ 0 (gleiche Messlatte wie der Freeze). Dann Exit-Schema anhand p/q nachjustieren (EINE Aenderung).
7. **Max. Verlust Echtgeld-Start:** hartes Verlustbudget der gesamten Validierung **400 €** ab Start (= 8 R in Stufe 1 bzw. ≈ 5 R bei 75 €), was zuerst erreicht wird: -8 R oder -400 € → Abbruch. Das liegt bewusst unter der Notbremse Stufe 1 (500 €).

---

## 8. Entscheidungsvorlage fuer Levi

1. **E1 Freeze beenden, Lockerung beschliessen.** Empfehlung: ja (10/10 Tage erfuellt, Befund "2 Einstiege in 10 Tagen" eindeutig). A: ja mit diesem Vorschlag. B: erst weitere Testtage alt — nicht empfohlen. Konsequenz: alte Freeze-Urteile (ALT knapp belegt, Rest nicht) werden Grundlage.
2. **E2 TP-Definition mechanisch.** Empfehlung: TP1 = exakt 1R, TP2 = exakt 2R. A: TP1 = naechstes Level ≥ 1R (alt) — kostet die meisten Einstiege. B: TP1 = 1R, TP2 = Level ≥ 2R. Konsequenz: RR-Gate, TP-Realismus, TP2-Realismus, Register-Pflicht entfallen.
3. **E3 Exit-Schema.** Empfehlung: Levis Schema (50 % @1R, BE, Rest 2R, Zeit-Exit 20:00) — mit der ausdruecklichen Messgroesse q (TP2/TP1) ≥ 0,5. A: 1R voll (einfacher, braucht p ≥ 53 %). B: 50 % @1R, BE erst ab 1,5R. Konsequenz: Daten reichen nicht, um zwischen Schemata zu waehlen (alle im Rauschen) — eines fest waehlen, nicht wechseln.
4. **E4 SL-Cap 3,0x ATR.** Empfehlung: ja (Ideen > 3x ATR: Ø -0,40 R, n 14). A: 2,5x (etwas besser, weniger Trades). B: ohne Cap — nicht empfohlen.
5. **E5 Q-ROT-Veto streichen**, Q-Score nur Schatten. Empfehlung: ja (Q-ROT-Ideen ≤ 3 ATR Ø +0,25 R). A: Q-ROT = halbe Position. B: Veto behalten — dann bleibt es bei ~0,2 Trades/Tag.
6. **E6 Streichliste** (13.1/Chasing/Punkt 11, Zeitbox/Aussichtslos/Wiederholung, Stale, 8c2-Pflicht, Anker-Reset-Pflicht, Stall-Exit, Halbierungen, Fib/Register-Pflichten). Empfehlung: alles streichen, Messwerte als Schatten. A: Stall-Exit als Option behalten — nicht empfohlen (Historie).
7. **E7 Behalten:** Dual-Gate 2/2 + 1H nicht dagegen, ein Trigger-Satz, ALT-Anker, Floor 1,5x ATR, alle Risiko-/Sperrfrist-Regeln. Empfehlung: ja.
8. **E8 Zeitfenster.** Empfehlung: Entries 15:35-19:30, Zeit-Exit 20:00, keine Halbierung 15:30-16:00. A: Entries erst ab 16:00 (in den Daten schlechter). B: Entries bis 19:00.
9. **E9 Kosten messen vor Echtgeld.** Empfehlung: Spread des genutzten Produkts (Schein/CFD) in NAS-Punkten bestimmen; > 4 Pkt → Produktwahl pruefen. Konsequenz: ohne diese Zahl ist jede R-Rechnung zu optimistisch.
10. **E10 Validierung** wie Abschnitt 7: 3 Prozesstage fiktiv → 10 Trades halbes Risiko → bis 40 Trades voll; Abbruch -8 R / Ø ≤ -0,3 R nach 15. Empfehlung: ja. A: direkt volles Risiko. B: 5 fiktive Tage statt 3.
11. **E11 Regel-Freeze waehrend der Validierung:** nur Abbruch oder Fehlerkorrektur, keine neuen Filter vor Trade 20. Empfehlung: ja.
12. **E12 Umsetzung** durch Fable nach Auftrag unten, Opus-Gegencheck vor Aktivierung, Commit/Push nur Levi; DST-Fix (Frist 19.10.) in den neuen Skripten mit erledigen. Empfehlung: ja.

### Umsetzungsauftrag-Entwurf (fuer Fable, nur Entwurf)

> **Auftrag Fable — Kern-Regelwerk (Levi-Entscheide E1-E12 vom 14.10.2026, Grundlage `opus_vorschlag_regel_lockerung_2026-10-14.md`).**
> Ziel: ein schlanker Loop, der das 10-Regel-Werk aus Abschnitt 5 umsetzt. Bestehende Skripte NICHT umbauen, sondern neue daneben stellen; alte bleiben fuer Auswertungen lauffaehig.
> 1. `scripts/kern_gate.cjs`: Eingaben `--dir --entry --sl-anker --atr --blackout <none|aktiv> --testtag <fiktiv|echt> --jetzt`; rechnet SL = Anker ∓ 0,5 ATR (Floor 1,5 ATR), FAIL bei SL > 3,0 ATR, Sperrfrist aktiv, Tageslimit (3 Trades/2 Verluste/-2R aus Log), TP1 = 1R, TP2 = 2R, Groesse via size.cjs (Flag `--risiko-halb`). Schattenzeilen: Q-Score (auto), alte Level-Fenster-Bewertung ("alt haette: PASS/FAIL/ROT"), Chop-Verdacht. Log `kern_log.jsonl`. Ausgabe ≤ 12 Zeilen.
> 2. `scripts/kern_check.cjs` (Voll-Check alle 5 Min): Felder NAS 15m Close/EMA50, QQQ 15m Close/EMA50, NAS 1H Close/EMA50, 5m Close/EMA50, ATR, letzter Swing, Kurs, `--jetzt`; Ausgabe Richtung, Trigger ja/nein, SL-Vorschau (Cap-Pruefung). DST-Fenster 26.-30.10. eingebaut.
> 3. `scripts/loop_prompt.cjs --modus kern`: Prompt < 80 Zeilen, keine Pflichtfelder ausser den obigen.
> 4. `scripts/position_tick.cjs --modus kern`: nur SL/TP1/BE/TP2/Zeit-Exit-Status.
> 5. `scripts/kern_nachtrag.cjs`: Bar-fuer-Bar-Ausgang mit `--spread`, Gate-Kerze intrabar (1m-Bars, sonst Kennzeichnung), Report p, q, c, Ø R, ohne besten Tag, Trend/Range.
> 6. Tests: nur neue Testdateien, Aufruf `node --test tests/kern_*.test.cjs` mit expliziter Pfadliste; KEIN `npm run test:e2e` ohne Levi-Vorwarnung.
> 7. Doku: neues Memory `regelwerk_kern_2026-10.md`; alte Regel-Memos mit Kopfvermerk "archiviert ab Aktivierung Kern-Regelwerk" (nicht loeschen).
> 8. Reihenfolge: (1)+(5) → (2) → (3)+(4) → Doku → Opus-Gegencheck (Prompt-Diff, Testlauf auf Bars 13.10. und 05.10.) → Levi-Freigabe → 3 Prozesstage fiktiv.
> 9. Nach Umsetzung "bereit zum Commit" melden — Commit/Push nur Levi.

---

## Ehrliche Gesamtwertung

Levi hat beim Befund Recht und ich bestaetige es ohne Abstriche: Ein Regelwerk, das in zehn Tagen zwei Einstiege erlaubt, 43 Argumente pro Entry verlangt und jede Minute 488 Zeilen gelesen haben will, ist kein Handelssystem mehr, sondern ein Pruefsystem. Die Daten zeigen auch, welche zwei Regeln das verursacht haben (Level-Fenster und Q-ROT-Veto) und dass genau eine Grenze echten Schutz geleistet hat (SL nicht weiter als 3x ATR). Das ist eine klare, gut begruendete Lockerung.

Was ich nicht bestaetigen kann: dass die Lockerung profitabel ist. Die beste ehrliche Zahl ist "leicht positiv, im Rauschen, von einem Trendtag getragen, kostenempfindlich". Und Levis Kernformel hat einen Rechenfehler, der schon einmal Geld gekostet hat: 1:1 und 1:2 bei 50 % Trefferquote ist nur dann ein Gewinnsystem, wenn die Haelfte der Gewinner bis TP2 durchlaeuft — historisch war es gut ein Viertel. Der Weg zurueck ins Traden ist richtig. Er muss mit festen Ausgaengen, gemessenen Kosten, halbem Startrisiko und einem harten Abbruchkriterium gegangen werden, und die naechsten 20-40 echten Trades sind die eigentliche Pruefung — nicht diese Simulation.
