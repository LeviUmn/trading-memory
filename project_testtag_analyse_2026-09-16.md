---
name: project-testtag-analyse-2026-09-16
description: "Opus-Analyse Testtag 16.09.2026 (fiktiv, FOMC-Tag), verifiziert gegen Skript-Rohdateien + live nachgerechnete Chartdaten (NAS100/QQQ 5m/15m/60m/D via CDP). Loop wegen CDP-Tab-Bug erst ab 17:17:29 DE, 90 vollcheck.cjs-Laeufe (63 Exit 0 / 27 Hard-Exit 1), 112 gate_check-Saetze (94 Vorpruefungen + 18 live), 2 PASS, 4 FAIL, 2 fiktive Trades, beide -1R, netto -75,28 EUR. SCHWERSTER BEFUND: Beide Trades lagen INNERHALB der FOMC-Entry-Sperrfrist (Grundregel feedback_session_update: 'Kein Entry bis 30 min nach Veroeffentlichung + Volatilitaet beruhigt' — Entscheid 20:00 DE, Trade #1 um 20:26 = 4 Min zu frueh, Trade #2 um 20:47 mit ATR 19,1->45,4 = +138 %, Pressekonferenz laufend). Der Loop zaehlte den FOMC-Countdown selbst mit ('in 14/9/4 Minuten'), verband ihn aber nie mit der Sperrfrist; kein Skript kennt das Wort Sperrfrist/Blackout (grep = 0 Treffer). Damit sind beide Verluste nach feedback_regeldisziplin Kategorie B (Regelbruch), nicht akzeptables Marktrisiko. ZWEITER BEFUND: Der 21:07-SHORT scheiterte NICHT am Markt, sondern am eingefrorenen Level-Register — das Rundzahl-Band stand seit 17:06 DE bei 29100 waehrend der Kurs 450 Pkt tiefer lief; gate_check druckte woertlich 'Rundzahlen im Fenster: 28800 — KEINE davon im Register' + den Reparaturbefehl, der nicht ausgefuehrt wurde. Mit TP1 28800 (RR 1,105, Zone 2) waere der Lauf PASS gewesen und TP1 um 21:15 DE gefallen (~+1,1R). PFLICHTAUFGABE 15:30-17:17 DE (Loop lief nicht): JA, ein valider Trade wurde verpasst — Dual-Gate 2/2 LONG durchgehend (NAS100 15m ueber EMA50 seit 05:00, QQQ seit 10:00), 1H-Bias LONG, 9 gate-konforme Entry-Momente nachgerechnet, ALLE 9 haetten TP1 vor SL erreicht; belastbarster Moment 16:30/16:35 DE (Entry 29136,45 / SL 29093,55 / TP1 29200 Register-Rundzahl, RR 1,48, TP1 um 17:00 DE) = ca. +1,1 bis +1,5R halbe Position. KEINE Lockerung der RR-Schwelle empfohlen (der 19:23-Fall RR 0,996 war ein TP1-Auswahl-/Reifegrad-Problem bei 7,65 Pkt breitem Fenster, kein Schwellenproblem). Q4 (Runway) ueber die gesamte Loghistorie 0x ERFUELLT — strukturell unerreichbar, Q-Score deckelt dauerhaft bei 3/4 GELB. V1-V7 (79573f5/c7b2077) haben alle nachweislich gegriffen: Live-Laeufe 39->18, FAILs 31->4, Skipped-Erfassung 1/31 -> 6/6. Prozesslucken: 0 Screenshots am ganzen Tag (15.09.: 49), 0 von 90 Voll-Checks als vollstaendig markiert, position_tick blieb seit 20:51 DE auf 'offen' stehen. 8 Vorschlaege W1-W8."
metadata:
  node_type: memory
  type: project
  status: "Analyse abgeschlossen, Vorschlaege W1-W8 offen (Levi-Entscheidung ausstehend)"
  originSessionId: session_01N2SiGMZjoJ78rA7Q5Awaxb
  modified: 2026-09-16T20:23:13.118Z
---

# Opus-Analyse Testtag 16.09.2026 (fiktiv, FOMC-Tag)

*Quellenbasis: (a) Skript-Rohdateien in `C:\Users\umnus\tradingview-mcp\scripts\` (`vollcheck_log.jsonl`, `vollcheck_state.json`, `gate_check_log.jsonl`, `oneh_shadow_log.jsonl`, `skipped_setups_fiktiv.jsonl`, `sl_anker_wechsel_log.jsonl`, `register_touch_log.jsonl`, `loop_stopp_log.jsonl`, `level_register.json`, `last_gate_check.txt`), (b) live per CDP nachgeladene Chartdaten (FOREXCOM:NAS100 5m/15m/60m/D, BATS:QQQ 5m/15m), (c) Regelwerk-Memory. Alle Indikatorwerte (EMA50, ATR14, RSI14, ADX14, MACD-H) habe ich selbst aus den Rohbars nachgerechnet und gegen die vom Loop geloggten Werte validiert (EMA50(5min) Abweichung 0,8–2,7 Pkt, ATR(5min) Abweichung 0–4 Pkt = jeweils eine Kerze Versatz). Prozessregel V7 (Skript-Logs sind Primaerquelle) wurde eingehalten.*

## 1. Kurzfazit

Der Tag hat zwei Gesichter. **Mechanisch** war er der bislang beste: die gestern committeten V1–V7 haben durchweg gegriffen — die Live-Gate-Aufrufe fielen von 39 (15.09.) auf 18, die FAIL-Wiederholungen von 31 auf 4, die Skipped-Setup-Erfassung von 1/31 auf 6/6, und der Wiederhol-Zirkel, der die 15.09.-Analyse dominierte, ist nicht ein einziges Mal wieder aufgetreten.

**Fachlich** war er der schlechteste seit Wochen, und zwar aus einem Grund, den keines der Skripte prüfen kann: **beide Trades des Tages lagen innerhalb der FOMC-Entry-Sperrfrist.** Die Grundregel aus [[feedback_session_update]] lautet wörtlich „Kein Entry bis 30 min nach Veröffentlichung + Volatilität beruhigt"; Fed-Entscheidungen stehen auf derselben Liste. Der Zinsentscheid kam um 20:00 DE (die 20:00-Kerze hat 102,9 Pkt Range bei einem ATR von 19,1 = 5,4× ATR in fünf Minuten — daran gibt es nichts zu deuten). Trade #1 wurde um **20:26 DE** eingegangen, also **vier Minuten vor Ablauf der harten 30-Minuten-Frist**. Trade #2 um **20:47 DE**, nach Ablauf der Frist, aber mit einem ATR(5min), der von 19,1 (19:23 DE) auf 45,4 gestiegen war (+138 %) und einer laufenden Pressekonferenz — die zweite Bedingung („Volatilität beruhigt") war messbar nicht erfüllt. Der Loop hat den FOMC-Countdown selbst mitgezählt („FOMC-Entscheidung in 14 Minuten", „in 9 Minuten", „in 4 Minuten", „FOMC-Entscheidung ist da" — alles in `vollcheck_log.jsonl`) und die Information dann fallen lassen. Das ist exakt das Fehlermuster, das am 07.08.2026 bei Trade #34 protokolliert wurde: *„Das Problem war keine fehlende Information, sondern eine fehlende Verbindung zwischen bereits erkannter Klassifikation und der abhängigen Formel."*

Damit sind die −75,28 € nach [[feedback_regeldisziplin]] **nicht** „Verlust trotz Regeleinhaltung", sondern Kategorie B: selbstgemacht. `gate_check.cjs` hat korrekt PASS gegeben — es kennt das Wort „Sperrfrist" nicht (`grep -i "sperrfrist\|blackout\|fomc" scripts/*.cjs` = 0 Treffer in allen drei Loop-Skripten). Die Regel war bekannt, sie stand in zwei Memory-Dateien, sie wurde im Moment nicht angewandt.

Der zweite große Befund betrifft die Post-FOMC-Phase: die dort durchgehend gemeldete „UNLÖSBARE" SHORT-Geometrie war **kein Marktbefund, sondern ein Registerpflege-Artefakt** (Abschnitt 4.2). Und der Chartverlauf 15:30–17:17 DE, den der Loop wegen des CDP-Bugs verpasst hat, enthielt tatsächlich einen sauberen, regelkonformen Long — Levis Pflichtfrage ist mit **Ja** zu beantworten (Abschnitt 3).

## 2. Zahlenbilanz (verifiziert, nicht übernommen)

| Größe | Befund | Quelle |
|---|---|---|
| Loop-Fenster | erster Voll-Check **17:17:29 DE** (`erster_vollcheck` 15:17:29Z), letzter Eintrag **22:00 DE** | `vollcheck_state.json`, `vollcheck_log.jsonl` |
| Terminal-Stopps | 2 Einträge: 21:15 DE („Terminalzeit 21:15 erreicht") und 22:00 DE (Verlängerung), beide `ergebnis: "keine"` | `loop_stopp_log.jsonl` |
| `vollcheck.cjs`-Läufe | **90** (V4-Log) — davon **63 Exit 0**, **27 Hard-Exit 1** | `vollcheck_log.jsonl` |
| gezählte Voll-Checks | 53 Einträge in `historie`, höchste Nr. **58** → 5 Nummernlücken | `vollcheck_state.json` |
| **Voll-Checks mit `vollstaendig: true`** | **0 von 90** (63× `false`, 27× `null` bei Hard-Exit) | `vollcheck_log.jsonl` |
| `gate_check.cjs`-Sätze heute | **112** = 94 `--sl-vorpruefung` + 18 `live` (davon 2 Testaufrufe um 11:05 DE vor Loop-Start) | `gate_check_log.jsonl` |
| Live-Ergebnisse | **2 PASS**, **4 FAIL**, **3 UNKNOWN**, **9 Abbruch (Exit 1)** | ebd. |
| Trades | 2, beide LONG, beide per SL, je −1R: −37,79 € + −37,49 € = **−75,28 €** | `skipped_setups_fiktiv.jsonl` |
| Tagesverlust-Limit | 2× Risiko = 150 € — **nicht erreicht** (50,2 % ausgeschöpft) | [[project_risikomanagement]] |
| Skipped-Setups | **6** Einträge, alle mit nachträglich verifiziertem Ergebnis; 4 davon V3-automatisch | `skipped_setups_fiktiv.jsonl` |
| 1H-Schattenmessung | 63 Einträge, **4× `blockade_3v4` (short)**, **0× `e7_zaehlt: true`** | `oneh_shadow_log.jsonl` |
| SL-Anker-Log | 6 Einträge: 5 Richtungswechsel-Vermerke (V6), **1 echter Ankerwechsel** (21:02 DE, 29076,75→29088,45, `gegen_richtung: true`, begründet) | `sl_anker_wechsel_log.jsonl` |
| Register-Touches | 9 — **letzte inhaltliche Änderung 17:06 DE**, danach nur 4 reine `geprueft`-Refreshs (18:11/19:12/20:15/21:20) | `register_touch_log.jsonl` |
| **Screenshots** | **0** für 2026-09-16 (jüngste Datei stammt vom 15.09.) — 63× „Screenshot LUECKE" protokolliert | `screenshots/`, `vollcheck_log.jsonl` |
| Weitere Lückentypen | 39× „Stale-Check LUECKE", 14× „Tweet-Check ✗" | `vollcheck_log.jsonl` |
| Hard-Exit-Ursachen | 11 von 27 = `--kerzen-nas100/--kerzen-qqq/--k-ohne-signal` weicht vom State ab; 5× fehlendes `--stale-n`; 4× fehlende Fib-/Retest-/Terminal-Pflichtfelder; 2× Register-Check | ebd. |
| Tagesrange NAS100 | 29240,45 / 28745,35 = **495,1 Pkt** = **1,16× ATR(14) Daily (425,5 Pkt)** → 8d-Kriterium 1 **nicht** erfüllt → **Schock-Tier korrekt nicht aktiv** | Tages-Bars + 5m-Bars |

**Ergebnis der ausgelassenen Setups (nachträglich bar-für-bar verifiziert):** 17:26 DE −1R · 19:23 DE **+0,996R** · 20:07 DE −1R · 21:07 DE 0R (TP1 um 3,2 Pkt verfehlt, MFE +221 Pkt = 1,47R). Summe ≈ **−1,0R**. Die Gates haben in Summe also rund **+1R gespart** — sie haben zwei sichere Verluste verhindert und einen Gewinn gekostet. Das ist ein gutes Zeugnis für die Gate-Logik als solche.

## 3. PFLICHTAUFGABE: Chartverlauf 15:30 – 17:17 DE (Loop lief nicht)

> Levi wörtlich: *„Bitte für Opus Analyse später den Chartverlauf von 15:30 bis ersten vollcheck mit Analysieren, ob wir hätten dort einen validen Trade gehabt. Das bitte notieren für den Bericht später"*

### 3.1 Antwort

**JA — mit hoher Sicherheit. In diesem Fenster gab es mindestens einen regelkonformen, geometrisch lösbaren 2/2-Dual-Gate-Long, der sein TP1 erreicht hätte. Belastbarster Moment: 16:30–16:35 DE.**

### 3.2 Richtungsgates — beide offen, den ganzen Zeitraum

Selbst nachgerechnet aus den Live-Bars (EMA50 auf Schlusskursen, bestätigte Kerzenschlüsse):

| Ebene | Befund im Fenster | Seitenwechsel |
|---|---|---|
| NAS100 15min vs. EMA50(15) | durchgehend **ÜBER** (C 29102,85–29215,45 vs. EMA50 29056,0–29085,2) | letzter Wechsel nach oben **05:00 DE** |
| QQQ 15min vs. EMA50(15) | durchgehend **ÜBER** (C 708,40–711,26 vs. EMA50 707,12–707,90) | letzter Wechsel nach oben **10:00 DE** |
| **Dual-Gate** | **2/2 LONG, lückenlos von 15:30 bis 17:17 DE** | — |
| 1H-Override NAS100 | zuletzt geschlossener Bar vor 15:30 = 14:00-Bar, C 29099,0 vs. EMA50(60) 29059,19 → **LONG-Bias** | steht **nicht** dagegen |

Kein einziger dieser Werte war knapp. Der 1H-Override, der am 15.09. zehn Voll-Checks lang blockiert hat, stand heute auf derselben Seite.

### 3.3 Geometrie — welche Entry-Momente wären durch die Gates gekommen

Rechenweg je Kerzenschluss: Entry = Close, SL-Distanz = MAX(Struktur-Anker aus dem tiefsten Tief der letzten 6 Kerzen, 8c-Floor 1,5× ATR(5min)); zulässiges TP1-Fenster = [Entry + SL-Distanz (RR≥1) ; Entry + 3× ATR (Zone-3-Grenze)]; TP1 nur aus echten Registerlevels (`level_register.json`: PDH 29179, Pivot R1 29132,28, Pivot R2 29293,47, Rundzahlen 29100/29150/29200/29250/29300 — alle bereits aus der Vortages-Tagesbar bzw. dem damals korrekten ±150-Pkt-Band). Ausgang simuliert mit Intrabar-High/Low, konservativ (SL zuerst bei Gleichzeitigkeit in derselben Kerze).

| Kerzenschluss | Entry | ATR(5m) | Q2 = \|E−EMA50\|/ATR | SL | TP1 (Registerlevel) | RR | TP-Zone | Ausgang |
|---|---|---|---|---|---|---|---|---|
| 15:30 | 29102,45 | 23,3 | 0,75 ✓ | 29063,25 | 29150 (Rundzahl) | 1,213 | 2,04× | **TP1 @15:45** |
| 15:35 | 29087,25 | 24,3 | **0,09 ✓** | 29050,85 | 29132,28 (Pivot R1) | 1,237 | 1,86× | **TP1 @15:45** |
| 15:40 | 29102,85 | 24,8 | 0,69 ✓ | 29065,55 | 29150 (Rundzahl) | 1,264 | 1,90× | **TP1 @15:45** |
| 16:00 | 29126,55 | 28,6 | 1,09 ✓ | 29070,45 | 29200 (Rundzahl) | 1,309 | 2,57× | **TP1 @17:00** |
| 16:10 | 29141,45 | 30,6 | 1,38 ✓ | 29095,55 | 29200 | 1,276 | 1,91× | **TP1 @17:00** |
| 16:25 | 29144,05 | 29,2 | 1,33 ✓ | 29100,35 | 29200 | 1,280 | 1,92× | **TP1 @17:00** |
| **16:30** | **29136,45** | **28,6** | **1,05 ✓** | **29093,55** | **29200 (Rundzahl)** | **1,481** | **2,22×** | **TP1 @17:00** |
| **16:35** | **29130,25** | **28,2** | **0,81 ✓** | **29087,85** | **29179 (PDH)** | **1,150** | **1,73×** | **TP1 @16:55** |
| 16:45 | 29149,35 | 28,0 | 1,37 ✓ | 29107,35 | 29200 | 1,206 | 1,81× | **TP1 @17:00** |

**Neun lösbare Momente, neun Mal TP1 vor SL.** Kein einziger SL wurde vorher berührt (kleinster Abstand zum Tief: der 15:35-Kandidat lag 19,6 Pkt über seinem SL im Tief der Folgekerze). Die Momente 15:45–15:55, 16:05, 16:40, 16:55–17:15 waren dagegen echte Fehlanzeigen: dort war das TP1-Fenster leer oder Q2 lag über der 1,5×-Schwelle (bis 3,31× um 17:05) — das System hätte dort korrekt „warten" gesagt.

### 3.4 Ehrliche Abzüge (was ich NICHT verifizieren konnte)

1. **Momentum-Bestätigung fehlte bei den frühen Kandidaten.** Um 15:30/15:35/15:40 war MACD-H(5min) noch **negativ** (−1,11 / −1,22 / −0,24) und ADX(14) bei 20,8–21,2. Erst die 15:45-Kerze drehte MACD-H auf +3,63 — genau dann war Q2 aber schon bei 2,40. Ein realer Voll-Check hätte die 15:30er-Gruppe vermutlich mit „Trend nicht bestätigt" abgelehnt. **Die 16:25–16:45-Gruppe ist deshalb die belastbare** (ADX 21,4–25,2, MACD-H −0,02 bis +3,23, RSI 52–58).
2. **Davor lag eine Chop-Phase.** Zwischen 14:35 und 15:25 DE wechselte der 5min-Schluss sechsmal die EMA50-Seite (Muster `ddUdUdUUUUd`), teils mit 0,2–0,4 Pkt Abstand. Der „frische Seitenwechsel" um 15:30 war rechnerisch der sechste Flip dieser Serie, kein sauberer Trigger. Ein aktiver `chop_streak`/Chop-Tier hätte die frühe Gruppe plausibel gebremst.
3. **Chasing.** Ab 15:45 war das 13.1-Kriterium (≥4 gerichtete Kerzen) formal erfüllt; die 16:25–16:45-Kandidaten wären damit Chasing-Einstiege → **50 % Position**. Zwischen 15:55 und 16:40 lag allerdings eine echte Konsolidierung (29126–29168 über neun Kerzen), die ein „`--chasing no`" tragen könnte. Für das Ergebnis unerheblich, nur für das Sizing.
4. **Q4 hätte auch hier gefehlt** (Runway-Ratio für den 16:30-Kandidaten: Gegenlevel Rundzahl 29150 nach 13,55 Pkt / TP1-Distanz 63,55 Pkt = **0,21** < 1,0) → Q-Score maximal **3/4 GELB → halbe Position**, genau wie bei beiden echten Trades. Siehe 4.5.
5. **Register-Verfügbarkeit.** `register_touch_log.jsonl` zeigt die ersten Einträge des Tages erst um **16:36 DE** (mit `updated_prev: "PLACEHOLDER"`), den ersten echten Inhalt um 17:06 DE. Ob um 15:30 DE ein benutzbares Register vorlag, lässt sich **nicht rekonstruieren** — ohne Register bricht `gate_check.cjs` mit Exit 1 ab. Da der CDP-Bug auch das Session-Update zerschossen hat, ist gut möglich, dass die frühen Kandidaten schon daran gescheitert wären. Für die 16:30/16:35-Kandidaten greift dieser Einwand **nicht** (Register ab 16:36 DE vorhanden, Inhalt ab 17:06 DE; die verwendeten Level PDH/R1/Rundzahlen stammen sämtlich aus der Vortages-Tagesbar und waren ab Sessionstart gültig).
6. **8a1/8a2/Q1/Q3, RVOL, VWAP-Lage und der Tweet-/News-Check** sind rückwirkend nicht prüfbar. Ich habe ausschließlich Dual-Gate, 1H-Override, Q2 und die vier harten Rechengates geprüft.

### 3.5 Beziffertes Ergebnis

Konservative, belastbare Schätzung auf Basis des 16:30-DE-Kandidaten:

> **Entry 29136,45 · SL 29093,55 (42,9 Pkt = 1,50× ATR) · TP1 29200 (Rundzahl 100er, im Register) · RR 1,48:1 · Zone 2 · TP1 erreicht um 17:00 DE (Hoch 29205,35).**
> Bei halber Position (Q-GELB, 37,5 € Risiko): **ca. +55 €.** Alternative 16:35/PDH-Variante: +1,15R ≈ +43 €.

**Bandbreite der Aussage: +1,1R bis +1,5R auf halbe Position, also grob +43 bis +55 €.** Damit hat die knapp zweistündige Loop-Verspätung nicht nur „nichts gebracht", sondern den Tag **von etwa +1,2R auf −2R gedreht** (in Halbpositions-R gerechnet) — und zwar zusätzlich zu dem, was der FOMC-Blackout-Verstoß gekostet hat.

### 3.6 Der eigentliche Schaden der Verspätung

Der Loop stieg um 17:17 DE **exakt am Tageshoch** ein. Der erste Live-Gate-Lauf um 17:26 DE hatte Entry 29202,95 bei SL-Anker 29110,45 — **106 Pkt = 3,93× ATR** SL-Distanz, RR 0,437, `aussichtslos: true`. Die Verspätung hat also nicht bloß ein Fenster übersprungen; sie hat den Loop in dem Moment gestartet, in dem die Geometrie am schlechtesten war, und ihn damit für die nächsten zwei Stunden in genau die „Entry zu reif → SL zu weit → TP1-Fenster leer"-Falle laufen lassen, die die 15.09.-Analyse als Hauptproblem identifiziert hatte.

## 4. Zentrale fachliche Befunde

### 4.1 FOMC-Entry-Sperrfrist verletzt — beide Trades (schwerster Befund)

Regelkette, alles wörtlich aus dem Memory:

- [[feedback_session_update]], Market-Moving-Events: „FOMC-Protokolle / Fed-Entscheidungen / Fed-Reden" — **„Grundregel: Kein Entry bis 30 min nach Veröffentlichung + Volatilität beruhigt"**.
- [[project_fomc_29_07_2026]]: „Fed-Entscheidungen zählen … zur höchsten Blackout-Kategorie … **volle Entry-Sperrfrist**, keine Ausnahme … typischerweise ab ~15-30 Min vor 20:00 bis **nach Abklingen der Pressekonferenz-Volatilität**".
- [[feedback_session_update]]: die Blackout-Regeln sind ausdrücklich **hart und bindend**, nicht Kontext („die bereits bestehenden harten Zeit-/Blackout-Regeln aus Schritt 4").

Tatsächlicher Ablauf, aus den Bars und Logs:

| Zeit DE | Ereignis | Beleg |
|---|---|---|
| 20:00 | Zinsentscheid. 5min-Bar O29165,95 **H29207,75 L29104,85** C29199,55 = **102,9 Pkt Range bei ATR 19,1 = 5,4× ATR** | 5m-Bars |
| 20:00–20:25 | Loop protokolliert „FOMC-Entscheidung ist da, Kurs stark volatil, kein Entry" | `vollcheck_log.jsonl` |
| **20:26** | **Trade #1 LONG, Entry 29161,95, SL 29095,65, PASS, Q 3/4 GELB, halbe Position** | `gate_check_log`, `skipped_setups_fiktiv` |
| 20:30 | Ablauf der 30-Minuten-Mindestfrist / Beginn Pressekonferenz | Regel + [[project_fomc_29_07_2026]] |
| 20:30–20:45 | Kerzen mit 100,1 / 99,3 / 109,3 / 95,4 Pkt Range; ATR steigt auf 45,4 | 5m-Bars |
| 20:51 | Trade #1 per SL beendet, −37,79 € | `sl_anker_wechsel_log` |
| **20:47** | **Trade #2 LONG, Entry 29139,85, SL 29054,05, PASS, Q 3/4 GELB** — 17 Min nach Beginn der Pressekonferenz, ATR 45,4 (+138 % ggü. 19:23) | ebd. |
| 21:07–21:25 | Abverkauf auf Tagestief 28745,35 | 5m-Bars |

**Bewertung.** Trade #1 verletzt den harten, mechanisch prüfbaren Teil der Regel (30 Minuten) um vier Minuten — da gibt es keinen Ermessensspielraum. Trade #2 hält die 30 Minuten ein, verletzt aber die zweite, gleichrangige Bedingung („Volatilität beruhigt") so deutlich, dass sie nicht als Grenzfall durchgeht: der ATR hatte sich seit dem letzten ruhigen Messpunkt mehr als verdoppelt und stieg weiter.

**Warum es passieren konnte.** Drei Dinge zusammen: (a) kein Skript kennt die Regel — `grep -i "sperrfrist\|blackout\|fomc"` liefert **0 Treffer** in `vollcheck.cjs`, `gate_check.cjs` und `position_tick.cjs`; (b) `gate_check.cjs` ist inzwischen so autoritativ, dass ein „GESAMTSTATUS: PASS" faktisch wie eine Freigabe gelesen wird, obwohl [[feedback_live_trading]] 7b1 das Gate ausdrücklich nur als *eine* Bedingung neben dem Regelwerk führt; (c) der Tag war ein Trendtag mit sauberem Dual-Gate — der Kontext hat aktiv in die falsche Richtung gezogen.

**Einordnung nach [[feedback_regeldisziplin]]: Kategorie B, selbstgemacht.** Nicht „Verlust trotz Regeleinhaltung". Das ist genau die Kategorie, die Fables 43-Trade-Vergleich vom 22.08.2026 als größten Verlustblock benannt hat (50,5 % aller Verlust-Euro) und zu der Levi gesagt hat, er habe sich Durchsetzung und Konsistenz „fest an die Stirn geschrieben". Der Pflichtsatz aus [[feedback_tagesabschluss]] muss für beide Trades lauten: `Regelkonformität geprüft: JA — Verstoß: JA`, und damit ist die Sofort-Review-Pflicht (Mechanismus b) ausgelöst.

**Kontrafaktisch, fairerweise:** Ohne die beiden Trades wäre der Tag bei **0 Trades / 0 €** gelandet (das 21:07-SHORT-Setup war zu diesem Zeitpunkt mit dem vorhandenen Register nicht freigabefähig, siehe 4.2). Die Sperrfrist hätte also exakt das verhindert, was eingetreten ist.

### 4.2 Die „UNLÖSBARE" Post-FOMC-Geometrie war ein Registerpflege-Artefakt, kein Marktbefund

Ab ca. 21:00 DE meldete jeder Voll-Check „SHORT-Geometrie AUSSICHTSLOS / für Register-TP1 unlösbar". Der letzte reale Live-Lauf um **21:07 DE** sagt in `scripts/last_gate_check.txt` aber etwas anderes, als das Fazit nahelegt:

```
[PASS ✓] RR-Gate (8b): RR 1.49:1 (TP1 224.2 Pkt / SL 150.5 Pkt) — Pflicht >=1:1
[FAIL ✗] TP-Realismus (8b1): TP1 224.2 Pkt = 3.95x ATR(5min)(=56.8) -> Zone 3 (Ausschluss)
TP1-Fenster (P3, Diagnose): zulaessiges TP1-Fenster [28795.95 ; 28815.85] = 19.9 Pkt breit
TP1-Fenster (P3, Diagnose): Rundzahlen (50er, arithmetisch, unabhaengig vom Register) im Fenster:
  28800 — KEINE davon im Register: Rundzahl-Band nachfuehren (Schritt-6-Regel Nr. 7, +/-150 Pkt um
  den Kurs, bei jedem Voll-Check; maschinell: node scripts/register_touch.cjs --rundzahlen ...)
RUNDZAHL-BAND-WARNUNG: Rundzahl-Band im Register endet bei 29100, das TP1-Fenster reicht bis
  28795.95 — Band deckt das Fenster NICHT ab (… am 03.09.2026 VC#14 stand das Sessionstart-Band
  75 Min unveraendert im Register).
```

Das Skript hat also **die fehlende Zahl (28800) namentlich genannt und den Reparaturbefehl gleich mitgeliefert.** Der Befehl wurde nicht ausgeführt. Nachgerechnet:

| TP1-Kandidat | Distanz | RR | Zone | Gate-Ergebnis |
|---|---|---|---|---|
| Pivot S2 28742,17 *(im Register, tatsächlich verwendet)* | 224,2 | 1,490 | 3,95× | **FAIL tpRealismGate** |
| Pivot S1 28856,63 *(im Register)* | 109,7 | 0,729 | 1,93× | FAIL rrGate |
| PDL 28903,35 *(im Register)* | 63,0 | 0,419 | 1,11× | FAIL rrGate |
| **Rundzahl 28800 — NICHT im Register** | **166,3** | **1,105** | **2,93×** | **PASS (Zone 2, halbe Position)** |

**Und der Ausgang:** mit TP1 28800 wäre das Ziel um **21:15 DE**, also acht Minuten nach dem Trigger, gefallen (Tief der 21:15-Kerze 28777,95); der SL 29116,85 war zu keinem Zeitpunkt in Gefahr (höchster Stand danach 28970,85). Ergebnis: **≈ +1,1R bei halber Position, ca. +41 €.**

**Ursache.** `level_register.json` trägt `updated: 2026-09-16T15:06:00Z` (= 17:06 DE) und `geprueft: 19:20:57Z` (= 21:20 DE). Das Rundzahl-Band wurde laut eigenem `abrufweg`-Feld „berechnet, Kursband um aktuellen Kurs 29192,95" — und enthielt folgerichtig nur 29100/29150/29200/29250/29300. Der Kurs lief danach **450 Punkte (≈ 8× ATR)** tiefer, ohne dass das Band nachgeführt wurde. Vier `geprueft`-Refreshs im Stundentakt haben das Register formal „frisch" gehalten, inhaltlich war es seit 17:06 DE tot. Das ist dieselbe Diagnose wie 15.09. Abschnitt 3.6 („Registerpflege — formal erfüllt, inhaltlich eingeschlafen"), nur mit einem messbaren Preis.

**Konsequenz für die Bewertung der AUSSICHTSLOS-Sperre (Levis Frage 4):** Die V1-Sperre selbst war **korrekt und hat sauber gearbeitet** — sie hat exakt einen Live-Lauf zugelassen und danach die Wiederholungen unterbunden (18 Live-Läufe heute gegen 39 am 15.09.). Sie hat außerdem den Ausweg korrekt benannt („… **oder ein anderes registriertes TP1-Level** öffnet einen Neu-Aufruf"). Sie war **nicht zu konservativ** und hat in der ersten Abverkaufsphase (SL-Distanz > 4× ATR, 21:2x DE) echten Kapitalschutz geleistet: ein SHORT dort hinein wäre ins Tagestief und die anschließende 200-Punkte-Erholung gelaufen. **Der Fehler lag eine Stufe davor**, in der Datenpflege, und die Sperre hat ihn nur unsichtbar gemacht, statt ihn zu verursachen. Sie deshalb zu lockern wäre die falsche Lehre.

### 4.3 Der 19:23-Fall (RR 0,996) — ausdrückliche Empfehlung: **KEINE Regeländerung**

Der Fall sieht auf den ersten Blick nach einer zu starren Schwelle aus: Entry 29150,55 / SL 29100,9 / TP1 29200, RR **0,996** statt der geforderten ≥1,0, und TP1 wäre tatsächlich vor dem SL gefallen. Nachgerechnet ergibt sich ein anderes Bild.

Das zulässige TP1-Fenster war **[29200,20 ; 29207,85] = 7,65 Pkt breit = 0,40× ATR**. `gate_check.cjs` hat das selbst gedruckt, inklusive Handlungsverweis („ENGES Fenster (< 1× ATR): Entry liegt weit von der SL-Struktur entfernt, 8b Schritt 4 Option 2 prüfen"). Ursache ist **nicht** die RR-Schwelle, sondern die **SL-Distanz von 49,65 Pkt = 2,60× ATR** — ein zu reifer Entry, der das Fenster auf einen Splitter zusammendrückt. Die Regel hat also nicht willkürlich 0,004 zu wenig bemängelt, sondern korrekt gemeldet: *dieser Entry ist zu weit von seiner Struktur weg.* Genau dafür existiert die Schwelle.

Zwei Gegenproben:

- Im Register lag mit **Session-Hoch 29205,35** ein Level **im** Fenster (RR 1,104, Zone 2,87× → PASS, TP1 ebenfalls erreicht). Der 19:23-Lauf ist damit auch ein **TP1-Auswahl**-Fehler: die glatte 29200 wurde der 5,35 Pkt höheren, gleichwertig registrierten Marke vorgezogen. *Ehrliche Einschränkung:* dieser Registereintrag war zu dem Zeitpunkt selbst veraltet (das reale Session-Hoch stand seit 17:26 DE bei 29240,45). Bei sauber gepflegtem Register wäre das Fenster wohl leer gewesen — was die Diagnose nur bestätigt: das Problem war der Entry-Reifegrad, nicht der Schwellenwert.
- Eine Aufweichung auf „RR ≥ 0,99" hätte heute +0,996R gebracht und gleichzeitig die beiden anderen rrGate-FAILs (RR 0,437 und 0,27) **nicht** freigegeben — sie hätte also nur diesen einen Fall gedreht. Bei n = 1 ist das keine Kalibrierung, sondern Kurvenanpassung, und sie trifft genau das Muster, vor dem [[feedback_live_trading]] Punkt 14 („Regeländerungs-Tempo-Bremse") und [[feedback_dont_change_running_system]] warnen — zumal die Änderung nach zwei Verlust-Trades käme, also im ungünstigsten denkbaren Moment.

**Empfehlung: RR-Schwelle unverändert bei ≥ 1,0 lassen.** Der richtige Hebel ist der Entry-Reifegrad (Retest abwarten statt Schwelle senken) und die TP1-Auswahl (W3 unten).

### 4.4 Q4 (Runway) ist strukturell unerfüllbar — Q-Score deckelt dauerhaft bei GELB

Über die **gesamte** Loghistorie in `skipped_setups_fiktiv.jsonl` (19 Einträge mit Q-Bewertung): **Q4 wurde 0× als ERFÜLLT bewertet** — 12× UNKLAR (11.09.), 2× NICHT ERFÜLLT (heute, Runway 0,208 und 0,097). Auch alle neun von mir nachgerechneten Fenster-Kandidaten aus Abschnitt 3 fallen durch (bester Wert 0,21).

Der Grund ist arithmetisch: Q4 verlangt Runway-Ratio ≥ 1,0, also *kein* Gegenlevel zwischen Entry und TP1. Das Register enthält 50er-Rundzahlen, TP1 liegt regelkonform 1,5–3× ATR (bei ATR 20–45 also 30–135 Pkt) entfernt — es liegt praktisch **immer** mindestens eine 50er dazwischen. Q4 kann damit nur erfüllt sein, wenn TP1 näher als die nächste 50er liegt, was das RR-Gate ausschließt. **Q4 und das RR-Gate schließen sich gegenseitig fast vollständig aus.**

Folge: Der Q-Score erreicht strukturell maximal 3/4 = GELB = halbe Position. Beide Trades heute, und jeder rekonstruierte Kandidat, landen dort. Das ist exakt derselbe Fehlertyp wie die am 11.09. gefundene „Q2-Formel unerfüllbar", die zur Option-A-Neukalibrierung führte. Zusätzlich relevant für die am 15.09. beschlossene Präzedenzregel 13.1×Q-ROT: ein dauerhaft gedeckelter Q-Score verschiebt die Verteilung systematisch Richtung GELB — heute gab es folgerichtig **0 Kollisionsmomente** (`kollision_13_1_qrot: false` bei beiden PASS-Läufen), die Schattenmessung steht weiter bei 0.

Ich empfehle **keine sofortige Änderung** (siehe W4: erst messen, dann kalibrieren — Q4 läuft seit dem 14.09., die Stichprobe ist zu klein). Aber der Befund gehört auf den Tisch: ein Faktor, der in 19 von 19 Auswertungen nie erfüllt war, misst derzeit nichts.

### 4.5 V1–V7 haben gewirkt — der Wiederhol-Zirkel ist weg

Direkter Vergleich zum Vortag, jeweils aus den Logs:

| Größe | 15.09. | 16.09. | Bewertung |
|---|---|---|---|
| Live-Gate-Läufe | 39 | **18** (16 im Loop-Fenster) | V1/V2 greifen |
| FAIL-Läufe | 31 | **4** | Zirkel eliminiert |
| Läufe gegen leeres Entry-Fenster | 30 von 31 | **1** (21:07, danach gesperrt) | V1 scharf |
| Zeitbox-Kollisionen durch SL-Drift | 24 | **0** | V2 scharf |
| Skipped-Setup-Erfassung | 1 von 31 | **6 von 6** (4 automatisch) | V3 scharf |
| Eigenes `vollcheck_log.jsonl` | nicht vorhanden | **90 Sätze** | V4 scharf |
| SL-Distanz-Diagnose > 4× ATR | nicht vorhanden | mehrfach ausgelöst (21:2x DE) | V5 scharf |
| Richtungswechsel im Anker-Log | fehlte | **5 Einträge** | V6 scharf |

Das ist die wichtigste positive Nachricht des Tages: die gestern committeten Fixes (79573f5 / c7b2077) sind alle sieben im Produktivbetrieb belegt, und der schwerste Befund der 15.09.-Analyse ist nicht wieder aufgetreten. Ohne V4 wäre dieser Bericht in weiten Teilen nicht schreibbar gewesen.

### 4.6 Punkt-11-Signal am Tagesende — Mechanik korrekt, Ergebnis leer

Ab ca. 21:50 DE drehte MACD-H(5min) positiv (bis +13,2) bei Kurs weiterhin unter EMA50(5min). `vollcheck.cjs` hat das korrekt als Punkt-11-Signal gegen SHORT gewertet und den k-Zähler zurückgesetzt („MACD-H(5min) dreht positiv — Punkt-11-Signal gegen SHORT, k-Zähler auf 0 zurückgesetzt"). Das ist regelkonform (Rechtsfolgen-Tabelle vom 14.09.: „ja" ab ≥1 erfülltem Kriterium — bewusst die konservative Schwelle). Ein Trade kam nicht zustande, weil der Kurs das rechnerische Entry-Fenster nie erreichte. Kein Handlungsbedarf; das ist die Regel, die wie gebaut arbeitet.

### 4.7 Prozesslücken

1. **0 Screenshots am gesamten Testtag** (15.09.: 49). 63 von 63 erfolgreichen Voll-Checks tragen „Screenshot LUECKE (ausgelassen)", 39 zusätzlich die A3-Ausnahme `--screenshot`. Damit fehlt für den ganzen Tag die visuelle Gegenprobe — ausgerechnet an einem FOMC-Tag. Das ist die klarste Einzelregression gegenüber gestern.
2. **0 von 90 Voll-Checks als `vollstaendig: true`.** Jeder einzelne Lauf trug mindestens eine protokollierte Lücke. Der Begriff „vollständiger Voll-Check" hat damit heute keinen einzigen Referenzfall.
3. **27 Hard-Exits (30 %)**, davon 11 wegen Abweichung der Kerzen-/k-Zähler vom State. Das ist Reibung, keine Sicherheit — die Zähler werden vom Aufrufer geschätzt und vom State korrigiert; der Lauf bricht ab, statt den State-Wert zu übernehmen und den Unterschied zu protokollieren.
4. **`position_tick` steht seit 20:51 DE auf „offen".** Beide `loop_stopp`-Sätze (21:15 und 22:00 DE) führen `position_tick` mit `wert: "offen"` und `ts: 18:51:23Z` als Quelle, während `vollcheck_state` „keine" sagt. Der Stopp war im Ergebnis richtig, aber nur weil `vollcheck_state` gewonnen hat — der SL-Hit von Trade #2 wurde von `position_tick` nie nachgezogen. Bei einem echten Handelstag wäre das die Art von Divergenz, die eine falsche Overnight-Entscheidung auslösen kann.
5. **14× „Tweet-Check ✗"**, einmal explizit „Über-Polling — Widerspruch zum Raster". Gleiches Muster wie am 09.09. und 15.09. beanstandet.
6. **Loop-Start 17:17 DE statt ~15:25 DE** wegen des CDP-Tab-Bugs. Die Fixes sind bereits als Folgeauftrag notiert ([[project_cdp_tab_fixes_fable_folgeauftrag_todo_2026-09-17]]) — der heutige Tag liefert dazu die Kostenrechnung (Abschnitt 3.5/3.6): rund 1,2R entgangen plus der Einstieg in die schlechteste Geometrie des Tages.
7. **Keine Log-Inkonsistenzen.** Alle Querchecks (`vollcheck_state` ↔ `vollcheck_log` ↔ `oneh_shadow_log` ↔ `gate_check_log` ↔ `skipped_setups`) stimmen überein, kein Parse-Fehler in den heutigen JSONL-Zeilen. `trades.db` wurde nicht angefasst — der TESTTAG-GUARD hat gehalten.

## 5. Regelkonformitäts-Einordnung der beiden Trades

| | Trade #1 (20:26 DE) | Trade #2 (20:47 DE) |
|---|---|---|
| Dual-Gate 2/2 | ✓ | ✓ |
| 1H-Override | nicht dagegen ✓ | nicht dagegen ✓ |
| RR-Gate | 1,238 ✓ | 1,214 ✓ |
| SL-Floor 8c | ✓ (2,24× ATR) | ✓ (1,89× ATR) |
| TP-Realismus 8b1 | Zone 2 ✓ | Zone 2 ✓ |
| Schock-Tier 8b1a | nicht aktiv (Range 1,16× ATR(D) < 2×) — korrekt | nicht aktiv — korrekt |
| Q-Score | 3/4 GELB → halbe Position ✓ | 3/4 GELB → halbe Position ✓ |
| Chasing 13.1 | `no`, frischer Seitenwechsel ✓ | `no` ✓ |
| Sizing/Risiko | 37,5 € = Hälfte von 75 € ✓ | ✓ |
| **FOMC-Entry-Sperrfrist** | **✗ VERLETZT** (4 Min vor Fristende) | **✗ VERLETZT** (Volatilität nicht beruhigt, PK laufend) |

**`Regelkonformität geprüft: JA — Verstoß: JA`** für beide Trades. Sofort-Review-Pflicht nach [[feedback_regeldisziplin]] Mechanismus b) ist damit ausgelöst.

Fairerweise gehört dazu: **alles, was mechanisiert war, hat funktioniert.** Die Geometrie war sauber, das Sizing korrekt, das Tagesverlust-Limit wurde eingehalten, und das MFE-Bild (Trade #1: 0 Pkt, Trade #2: +24,1 Pkt) zeigt, dass beide Setups auch inhaltlich nie getragen haben — das war kein knapper Fehlschlag, sondern Whipsaw, genau die Marktverfassung, für die die Sperrfrist existiert. Der Fehler war nicht handwerklich, sondern eine ausgelassene Regel.

## 6. Status

**EINGESCHRÄNKT — aber mit klarem Ergebnis.** Zählstände, Gate-Ergebnisse, Trade-Bilanz, Anker-Historie, Register-Historie, Screenshot-/Log-Bilanz und die Rekonstruktion des 15:30–17:17-Fensters sind vollständig belegt, mehrfach quergeprüft und in den zentralen Punkten (Dual-Gate-Lage, Geometrie, TP1-Ausgang) mit Live-Chartdaten unabhängig nachgerechnet. Nicht belegbar bleiben: die Voll-Check-Innenansicht für die 27 Hard-Exit-Läufe, die 8a/Q1/Q3-Bewertung im verpassten Fenster, und die Frage, ob um 15:30 DE ein benutzbares Level-Register vorlag.

**Kein Echtgeld-Go aus diesem Tag.** Die fünf binären Pass-Kriterien aus [[project_validierungstesttag_naechster_handelstag]] sind durch den Sperrfrist-Verstoß nicht erfüllbar — ein Tag mit zwei Regelbruch-Trades kann kein Validierungstag sein. Der nächste Testtag sollte gezielt als Validierungstag gefahren werden, mit sauberem Loop-Start und ohne Blackout-Event im Fenster.

## 7. Konkrete Vorschläge (offen, Levi-Entscheidung)

**W1 (hoch, Regelbruch-Prävention — der wichtigste Punkt des Tages).** `gate_check.cjs` bekommt ein Pflichtfeld `--blackout <none|aktiv|HH:MM-HH:MM>` mit `--grund-blackout`-Ausweg, analog zur A3-Befüllungspflicht. Bei `aktiv` → **GESAMTSTATUS FAIL mit eigenem Gate „Entry-Sperrfrist (Schritt 4)"**, nicht nur eine Warnzeile. Optional zweite Stufe: `--event-zeit HH:MM` + `--event-typ`, damit das Skript die 30-Minuten-Frist selbst rechnet und den ATR-Anstieg gegen den Vor-Event-Wert als „Volatilität beruhigt"-Indikator ausgibt. **Begründung:** Heute war die Information im System (der Loop zählte den Countdown), aber nirgends mit der Entscheidung verbunden — dieselbe Diagnose wie 07.08.2026 bei Trade #34, wo damals bewusst auf ein hartes Veto verzichtet wurde. Das Argument von damals („die Lücke war die Verbindung, nicht die Durchsetzung") hat sich heute als unzureichend erwiesen: die Verbindung allein über eine Pflichtzeile herzustellen hat nicht getragen.

**W2 (hoch, Datenpflege — hat heute ~1,1R gekostet).** Das Rundzahl-Band im Level-Register automatisch nachführen. Konkret: `vollcheck.cjs` ruft die Rundzahl-Neuberechnung (`register_touch.cjs --rundzahlen <Kurs> --atr <ATR> --dir <dir>`) selbst auf, sobald der aktuelle Kurs das im Register hinterlegte Band um mehr als ±150 Pkt verlässt — oder, minimal-invasiv: die bereits existierende `RUNDZAHL-BAND-WARNUNG` wird von einer Anzeigezeile zu einem **Hard-Exit 1 des Voll-Checks** hochgestuft, mit dem Reparaturbefehl in der Fehlermeldung. Das Skript nennt die fehlenden Zahlen bereits namentlich; es fehlt nur die Verbindlichkeit.

**W3 (mittel, TP1-Auswahl).** `gate_check.cjs` soll bei jedem FAIL die **Registerlevel im zulässigen TP1-Fenster** explizit auflisten (heute listet es nur die arithmetischen Rundzahlen und warnt bei Band-Unterdeckung). Der 19:23-Fall zeigt, warum: das passende Level (Session-Hoch 29205,35) lag im Register und im Fenster, wurde aber nicht gewählt. Zusätzlich sinnvoll: bei Fensterbreite < 1× ATR die Zeile „ENGES Fenster" von Diagnose auf **Handlungsaufforderung** heben („Retest abwarten, kein Neu-Aufruf mit diesem Entry").

**W4 (mittel, Kalibrierung — messen, nicht ändern).** Q4 (Runway ≥ 1,0) explizit als **Schattenmessung** kennzeichnen, solange der Zähler bei 0 Erfüllungen steht, und den Q-Score bis zur Entscheidung aus **Q1–Q3** bilden (Schwelle entsprechend auf 3/3). Alternativ Q4-Schwelle zur Diskussion stellen (z.B. ≥ 0,5 = „mindestens die Hälfte des Weges frei"). **Nicht ohne Levi-Entscheidung umsetzen** — aber nach 19 von 19 Nicht-Erfüllungen ist die Frage fällig, ob Q4 in seiner jetzigen Form etwas misst. Gleiches Vorgehen wie bei der Q2-Neukalibrierung nach dem 11.09.

**W5 (hoch, Prozess).** Screenshot-Pflicht wieder scharf stellen: `--screenshot` aus der A3-Ausnahmeliste nehmen, oder — wenn das Screenshot-Werkzeug am CDP-Bug hängt — die Ausnahme mit einem **Zähler** versehen, der nach n Ausnahmen in Folge einen Hard-Exit auslöst. 63 stillschweigende Ausnahmen in Folge dürfen nicht möglich sein.

**W6 (mittel, Konsistenz).** `position_tick` und `vollcheck_state` beim Positionsende zwingend synchronisieren; `loop_stopp.cjs` soll eine divergierende Quelle nicht nur protokollieren, sondern als **WARNUNG** ausgeben, wenn die ältere Quelle „offen" sagt. Heute lief es gut aus, die Divergenz stand aber über 69 Minuten unbemerkt im System.

**W7 (mittel, Reibung).** Die 11 Hard-Exits wegen `--kerzen-*`/`--k-ohne-signal`-Abweichung vom State auf eine mildere Konsequenz umstellen: State-Wert übernehmen, Abweichung als Lücke protokollieren, Lauf fortsetzen. Der Abbruch schützt vor nichts (der State ist ohnehin die maßgebliche Quelle) und kostete heute ein Drittel aller Läufe.

**W8 (niedrig, Dokumentation).** Das verpasste Fenster 15:30–17:17 DE (Abschnitt 3) als Datenpunkt für die CDP-Tab-Fixes in [[project_cdp_tab_fixes_fable_folgeauftrag_todo_2026-09-17]] vermerken: der Bug hat nicht nur „Zeit gekostet", er hat einen mit hoher Wahrscheinlichkeit erfolgreichen Trade gekostet und den Loop am Tageshoch gestartet. Das hebt die Priorität dieser Fixes.

**Ausdrücklich NICHT vorgeschlagen:** Lockerung der RR-Schwelle (siehe 4.3), Lockerung der AUSSICHTSLOS-Sperre (siehe 4.2), Anheben des TP-Realismus-Caps über 3× ATR (die 8b1a-Begründung vom 24.08. gilt unverändert), neue Cooldown-/Verstoßzähler-Mechanik (am 12.08.2026 von Levi abgelehnt).

---

Vorgänger: [[project_testtag_analyse_2026-09-15]] (V1–V7, heute erstmals produktiv belegt) · [[project_gegencheck_v1_v7_runde6_2026-09-16]] (Commit c7b2077) · [[project_cdp_tab_fixes_fable_folgeauftrag_todo_2026-09-17]] (Ursache des späten Loop-Starts) · [[project_fomc_29_07_2026]] (Sperrfrist-Präzedenz) · [[feedback_regeldisziplin]] (Kategorie-B-Einordnung) · [[feedback_dont_change_running_system]] + [[feedback_live_trading]] Punkt 14 (Begründung gegen die RR-Lockerung).
Rohdaten: `scripts/vollcheck_log.jsonl`, `vollcheck_state.json`, `gate_check_log.jsonl`, `last_gate_check.txt`, `oneh_shadow_log.jsonl`, `skipped_setups_fiktiv.jsonl`, `sl_anker_wechsel_log.jsonl`, `register_touch_log.jsonl`, `loop_stopp_log.jsonl`, `level_register.json`; Chartdaten live via CDP (FOREXCOM:NAS100 5m/15m/60m/D, BATS:QQQ 5m/15m).
