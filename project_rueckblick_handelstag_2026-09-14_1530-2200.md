---
name: project_rueckblick_handelstag_2026-09-14_1530-2200
description: "Opus-Rueckblick auf den Handelstag 14.09.2026, Fenster 15:30-22:00 DE (kein Live-Loop gelaufen, nachtraeglich mit frisch abgerufenen Chart-Daten nachgerechnet). Ergebnis: GENAU EIN frisches 2/2-Trigger-Ereignis (17:55 Long) — Gate PASS, aber Q-Score 1/4 ROT (Q2-Reifegrad 4,24x ATR, Chasing) -> Auslassen empfohlen; genau dieser ausgelassene Trade haette TP1 erreicht (+1,4R, TP1 18:45). Daneben 1 beim Fensterstart bereits laufender 2/2-Short-Zustand (seit 10:10, kein frischer Cross; PASS + Q 3/4 GELB bei Q1=ja / 2/4 ROT bei Q1=nein) und 1 8a2-Reclaim 21:45 (PASS + Q 4/4 GRUEN, bestes Setup des Tages) — BEIDE waeren innerhalb von 5-15 Minuten ausgestoppt worden (je -1R). Antwort auf Levis Frage: keine saubere verpasste Chance; die einzige Bewegung, die gelaufen waere, war regelkonform ein Q-ROT-Chasing-Entry, und die beiden vom Q-Score freigegebenen Momente waeren Fehleinstiege mit SL-Treffer gewesen. 0 Trades war an diesem Tag besser als jede Teilmenge der Alternativen (0R vs. -0,6R bei allen drei vs. -2R bei nur den Q-freigegebenen). Methodik: alle Indikatoren aus Rohbars rekonstruiert und gegen Live-Werte kalibriert (Abweichung <= 0,05 Pkt); gate_check.cjs real ausgefuehrt, aber in einer Sandbox-Kopie — KEINE Produktionsdatei beruehrt."
metadata:
  node_type: memory
  type: project
  status: nachrechnung abgeschlossen, keine regelaenderung, keine produktionsdatei veraendert — 2 offene punkte fuer levi (register-defekt session-hoch, option-a-datenpunkte uebernehmen ja/nein)
  originSessionId: session_01E4zVYdeDA5XiMXhwykDWcw
  modified: 2026-09-14T21:27:33.589Z
---

## Kurzfazit

Methodisches Vorbild: [[project_testtag_analyse_2026-09-11]], Abschnitt „Verpasste Chancen". Relevante heutige Entscheidungen: [[project_q2_kalibrierung_entscheidungsvorlage_2026-09-11]] (Q2 Option A / Q4 Option Q4-a, seit heute verbindlich).

**Zuerst verifiziert (nicht angenommen): es lief heute tatsächlich KEIN Live-Loop.**
- `scripts/gate_check_log.jsonl` — letzter Eintrag 11.09.2026 17:56 UTC, 0 Einträge mit Datum 14.09.
- `scripts/skipped_setups_fiktiv.jsonl` — letzter Eintrag 11.09.2026, 0 Einträge mit `"datum": "2026-09-14"`.
- `scripts/trigger_kandidaten_log.jsonl` — 0 Einträge mit 2026-09-14.
- `scripts/vollcheck_state.json` — `datum_de: 2026-09-11`, letzter Voll-Check Nr. 52 vom 11.09.
- `scripts/trades.db` — 43 Trades gesamt, 0 mit `entry_time` am 14.09.

**Ergebnis im Fenster 15:30–22:00 DE:**

| | Anzahl |
|---|---|
| Frische 2/2-TRIGGER-EREIGNISSE (frischer bestätigter Cross, 1H nicht dagegen) | **1** (17:55 Long) |
| Davon SL-Anker-Vorprüfung TAUGLICH (7b1 Schritt 0) | 1 von 1 |
| Davon Q-Score-freigegeben (GELB/GRÜN) | **0** (17:55 = 1/4 ROT) |
| Zusätzlich: 2/2-Zustand ohne frischen Cross beim Fensterstart | 1 (15:30 Short, Zustand seit 10:10 DE) |
| Zusätzlich: technisches 2/2 mit 1H-Override DAGEGEN | 1 (17:25–17:50 Long, kein gültiges Dual-Gate) |
| Zusätzlich: 8a2-5min-Reclaim innerhalb des laufenden 2/2-Long | 1 (21:45 Long) |

**Hypothetische R-Bilanz (echte Folgebars, keine Schätzung):**

| Moment | Gate | Q-Score | Regelfolge | Tatsächlicher Ausgang | R |
|---|---|---|---|---|---|
| 15:30 Short 28.885,45 | PASS | 3/4 GELB (Q1=ja) bzw. 2/4 ROT (Q1=nein) | halbe Position bzw. auslassen | **SL 28.942 getroffen 15:45** (MFE nur 23,0 Pkt) | **−1** |
| 17:55 Long 29.169,15 | PASS | **1/4 ROT** | **Auslassen empfohlen** | TP1 29.250 erreicht 18:45; TP2 29.290,87 um 4,22 Pkt verfehlt (Hoch 29.286,65 um 19:55); Rückkehr auf Entry (BE) 21:50 | **+1,4** (nicht genommen) |
| 21:45 Long 29.204,35 | PASS | **4/4 GRÜN** (bestes Setup des Tages) | volle Freigabe (Sizing halb wg. Zone 2) | **SL 29.180,35 getroffen 21:50** (MFE 0,6 Pkt) | **−1** |

**Antwort auf Levis Frage, ohne Beschönigung:**
Es wurde **keine saubere Chance verpasst**. Die einzige Bewegung, die tatsächlich Geld gebracht hätte (17:55 Long, TP1 +1,4R), war zum Trigger-Zeitpunkt ein Entry 163 Punkte über der EMA50 (**Q2 = 4,24× ATR bei Schwelle 1,5×**) nach fünf gerichteten Kerzen ohne Konsolidierung — also genau der Chasing-Typ, den das Regelwerk bewusst aussortiert. Umgekehrt waren **beide Momente, die der Q-Score freigegeben hätte** (15:30 GELB, 21:45 GRÜN), **Fehleinstiege mit SL-Treffer innerhalb von 5–15 Minuten**.

Die Gesamtrechnung:
- Regelkonform (Q-ROT auslassen, kein Entry ohne frischen Cross): **0 Trades, 0R**
- Alle drei Momente genommen: +1,4 − 1 − 1 = **−0,6R**
- Nur die Q-Score-freigegebenen genommen: **−2R**

**0 Trades war an diesem Tag das beste der drei möglichen Ergebnisse.** Das ist kein Freispruch für den fehlenden Loop (der Loop hätte auch die Daten für die Kalibrierung geliefert), aber die Antwort auf „haben wir was verpasst?" lautet klar: nein, nichts Sauberes — und wären wir eingestiegen, wäre es mit hoher Wahrscheinlichkeit ein SL geworden.

---

## Methodik und Datengrundlage — mit Grenzen

### Datenquellen (alle heute Abend frisch per MCP abgerufen)
| Serie | Bars | Abdeckung |
|---|---|---|
| NAS100 5min | 304 | 11.09. 20:45 – 14.09. 23:00 DE (Montag ab 00:00 vollständig → 186 Bars Einschwing-Vorlauf vor 15:30) |
| NAS100 15min | 300 | 09.09. 17:30 – 14.09. 23:00 DE |
| NAS100 60min | 300 | 26.08. 23:00 – 14.09. 23:00 DE |
| QQQ 15min | 302 | 08.09. 11:45 – 14.09. 23:00 DE (ETH aktiv, Montags-Bars ab 10:00) |
| QQQ 5min | 300 | 11.09. 13:20 – 14.09. 23:05 DE |
| NAS100 Tages-Bars | 5 | für 8d/K1 (Range vs. ATR-D) |

Die Bar-Dateien liegen im Session-Scratchpad, **nicht** im Projekt — `scripts/qqq_5m.json`/`qqq_15m.json` wurden vor dem Abruf gesichert und danach bitgenau zurückgespielt, die temporär entstandenen `scripts/nas100_*.json` wieder gelöscht. `git status` ist vorher wie nachher leer.

### Indikator-Rekonstruktion und Kalibrierung
Alle historischen Indikatorwerte sind **selbst aus den Roh-Closes gerechnet** (EMA Standardformel mit SMA-Seed, ATR als Wilder-RMA(14), ADX als Wilder-DMI(14)/ADX(14)) — TradingView liefert keine historischen Studienwerte. Gegenprobe am jeweils letzten Bar gegen die Live-Werte aus `data_get_study_values`:

| Serie | Größe | rekonstruiert | live | Abweichung |
|---|---|---|---|---|
| NAS100 5min | EMA50 | 29.178,53 | 29.178,5 | 0,03 |
| NAS100 5min | ATR(14) | 13,15 | 13,2 | 0,05 (Anzeigerundung) |
| NAS100 5min | ADX | 33,63 | 33,6343 | 0,00 |
| NAS100 15min | EMA50 / ATR / ADX | 29.110,95 / 34,94 / 25,96 | 29.111,0 / 34,9 / 25,9634 | ≤0,05 |
| NAS100 60min | EMA50 / ATR / ADX | 29.153,24 / 89,57 / 25,93 | 29.153,2 / 89,6 / 25,9301 | ≤0,04 |
| QQQ 15min | EMA50 | 709,55 | 709,54 | 0,01 |

**Einordnung:** Die Rekonstruktion trifft die TradingView-Werte auf Anzeigegenauigkeit. Sie ist damit für diese Plausibilitätsprüfung belastbar — sie bleibt aber eine Nachbildung, keine Kopie der Pine-internen Berechnung. Wo eine Entscheidung an der zweiten Nachkommastelle hinge, wäre sie nicht belastbar; das ist an keiner Stelle dieser Analyse der Fall (der knappste Wert ist Q2 = 0,56 gegen Schwelle 1,5).

### Dual-Gate-Definition — welche Ebene gezählt wurde
Verbindlich ist die Zählkonvention aus [[feedback_live_trading]] 7b1 und die Umsetzung in `scripts/vollcheck.cjs` (Zeilen 387/784): **die zwei Beine sind NAS100 15min und QQQ 15min** (Kerzenschluss vs. eigener EMA50), die 5min-Ebene ist Trigger-Feinsteuerung und **kein Bein**; der 1H-Override ist der Bias des zuletzt GESCHLOSSENEN NAS100-1H-Bars (Close vs. EMA50). Genau so ist gerechnet worden — an jedem der 79 5-Min-Zeitpunkte im Fenster wurde der jeweils letzte abgeschlossene 15min-/60min-Bar herangezogen, nicht der laufende.

### Level-Register — Anachronismus offengelegt
Für die TP-Level wurde `scripts/level_register.json` verwendet, **Stand 22:41 DE (heutiges Session-Update)** — nicht der Stand, den ein Loop um 15:30 bzw. 17:55 gehabt hätte. Konkret:
- Für **17:55 und 21:45** ist das unkritisch: das Rundzahl-Band 29.000–29.300 und die Pivots decken die relevante Strecke ab.
- Für **15:30** ist es kritisch: das Register führt keine Rundzahlen unter 29.000, der Kurs stand aber bei 28.885. Für diesen Moment wurde ein **rekonstruiertes Register** verwendet (Rundzahlen 28.700–29.050 ergänzt, Session-Extrema auf den Stand 15:30 gesetzt), klar als Rekonstruktion markiert. Ohne diese Ergänzung hätte `gate_check.cjs` für 15:30 ohnehin `A2/8b2 UNKLAR` geliefert (kein registriertes TP1-Level) — der Moment wäre also live nur zu bewerten gewesen, wenn das Register gepflegt worden wäre, was ohne Loop nicht geschah.
- **Register-Defekt, unabhängig von dieser Analyse (für Levi):** Der Eintrag `Session-Hoch (laufend) 29.428,85` mit Abrufweg „heutiger Montags-Sessionverlauf" ist **falsch**. Das Montags-Hoch liegt auf der 5min-Serie bei **29.370,40** (00:00 DE), das Maximum über den gesamten abgerufenen 304-Bar-Satz bei 29.422,05 — und das ist ein **Freitag-Hoch** (11.09. 20:45). Der Wert stammt aus einem `summary`-Aufruf, dessen Fenster über den Wochenendwechsel hinausreichte. Dasselbe gilt für die Tagesrange: das Session-Update nennt „~630 Punkte", die echte Montags-Tagesrange ist **573,0 Pkt** (29.370,40 / 28.797,40), die Range der US-Session 15:30–22:00 **425,2 Pkt** (29.286,65 / 28.861,45).

### 8d-Regime (Schock/Chop) — eine Lücke, die das Ergebnis kippen könnte
- **K1 (Range/ATR-D > 2×): NEIN.** Tagesrange 573,0 Pkt ÷ ATR(14, Tages) 399,3 Pkt = **1,44×**. Der Tages-ATR ist durch die Vorwoche bereits stark erhöht — der Tag *fühlte* sich volatiler an, als er relativ zum eigenen Regime war.
- **K2 (≥2 qualifizierende Events): JA** (Latch) — Iran-Eskalation (Bab-el-Mandeb/Hormuz), Ölschock WTI Richtung 105 $, 10-Jahres-Rendite erstmals seit Okt. 2023 über 5 %, Trump/Iran-Deal-Schlagzeile mit Dementi. Quelle: heutiges Session-Update.
- **K3 (VIX-Range >5 % UND VIX ≥ VT-Schluss): NICHT GEMESSEN.** Der VIX ist auf keiner der beiden Panes gesetzt; ihn zu holen hätte einen Symbolwechsel auf Levis Layout erfordert — bewusst unterlassen. „nicht gemessen" ist nach 8d zulässig.
- **Folge: Schock-Tier NICHT bestätigt (1/3).** **Aber:** Wäre K3 erfüllt gewesen, läge 2/3 vor → Schock-Tier → nach 8b1a wären **alle drei** unten geprüften Momente **No-Trade ohne RR-Versuch** gewesen. Das ist die einzige offene Stelle, an der diese Nachrechnung mit einer Messung anders ausgehen könnte — und zwar in Richtung „noch weniger Trades", nicht „mehr".
- Chop-Verdacht (ADX <20 UND Trend-Effizienz <0,4 über ≥3 Voll-Checks): um 15:30 lagen ADX 18,9 und Trend-Effizienz 0,021 vor — beides unter Schwelle. Nach dem Rückbau vom 11.09. ist das **nur Verdacht-Ausweis, keine Rechtsfolge**; es stützt aber die Bewertung des 15:30-Moments als Rausch-Entry.

### 5-Minuten-Snapshot-Grenze
Innerhalb einer einzelnen 5-Min-Kerze ist die Reihenfolge „SL vor TP1" aus OHLC nicht bestimmbar. **In diesem Fall trat das Problem bei keinem der drei Momente auf** — die Simulation hat für jeden Trade geprüft, ob SL und TP1 in derselben Kerze berührt wurden; das war nie der Fall (15:30: MFE 23,0 Pkt gegen 85,5 Pkt TP1-Distanz; 17:55: MAE 43,7 Pkt gegen 57,8 Pkt SL-Distanz; 21:45: MFE 0,6 Pkt). Die Ausgänge sind insofern eindeutig.

### Was NICHT rekonstruierbar war
- **AVWAP-Historie (QQQ Session-Instanz):** Die Remote-Studie liefert nur Live-Werte. Das A3-Messfeld `--dual-gate-qqq-vwap-band1-diff-pct` wurde deshalb in allen Läufen per `--grund-…` als sichtbare Lücke geführt. VWAP/Volumen sind nach 7b ohnehin unterstützend, kein Gate.
- **RVOL-Historie** — dito, und seit 25.08. ohnehin GEMESSEN-KEIN-GATE.
- **Q1 (Ablehnung/Timing) und Q3 (Kohärenz)** sind im Live-Loop aus 8a2/8c2 bzw. dem MTF-Ergebnis vorbefüllt. Q3 war an allen drei Momenten eindeutig (alle vier Ebenen kohärent → ja). **Q1 ist an zwei Stellen eine Ermessensfrage** und unten jeweils als Annahme gekennzeichnet — beim 15:30-Moment entscheidet Q1 allein zwischen GELB (Einstieg) und ROT (Auslassen).

### Werkzeug-Isolation (keine Produktionsdatei berührt)
`gate_check.cjs` schreibt im Live-Modus in `gate_check_log.jsonl`, `skipped_setups_fiktiv.jsonl`, `trigger_kandidaten_log.jsonl`, `last_sl_vorpruefung.json` und `last_gate_fail.json`. `--dry-run` deckt davon nur einen Teil ab (der `skipped_setups_fiktiv`-Zweig ist **nicht** dry-run-geschützt). Deshalb wurde **das gesamte `scripts/`-Verzeichnis in den Session-Scratchpad kopiert** und jeder Aufruf dort ausgeführt (`NODE_PATH` auf das Projekt-`node_modules`). Alle Skript-Ausgaben in diesem Dokument stammen aus echten Läufen dieser Sandbox-Kopie. Verifikation nach Abschluss: `git status` leer, 0 neue Einträge in allen vier Produktionslogs, `last_sl_vorpruefung.json` unverändert vom 11.09., `trades.db` unverändert bei 43 Trades.

---

## Dual-Gate-Verlauf des Tages (Zustandswechsel, ganztägig)

| Zeit (DE) | Zustand | Kontext |
|---|---|---|
| 09:00 | 1/2 (NAS100 15min short / QQQ 15min long) | vor US-Session |
| **10:10** | **2/2 SHORT gültig** | QQQ kippt unter eigene EMA50; 1H short |
| *(15:30)* | *2/2 SHORT läuft weiter* | **Fensterstart — kein frischer Cross, Zustand 5 h 20 min alt** |
| 15:55 | 1/2 (NAS100 long / QQQ short) | NAS100 15min erobert EMA50 zurück (15:45-Schluss) |
| 17:25 | 2/2-technisch LONG, **1H DAGEGEN** | QQQ kreuzt long (17:15-Schluss), 1H-Bar 16:00 noch short → **kein gültiges Dual-Gate** |
| **17:55** | **2/2 LONG gültig** | 1H-Bar 17:00 schließt 29.169,15 über EMA50 29.139,84 → Override fällt weg → **einziges frisches Trigger-Ereignis im Fenster** |
| 21:55 | 1/2 | QQQ kippt zurück unter EMA50 (21:45-Schluss), 1H-Bar 21:00 short |

Zwischen 17:55 und 21:50 bestand durchgehend ein gültiger 2/2-Long-Zustand (47 Voll-Check-Zeitpunkte) — **ohne einen einzigen weiteren frischen Cross auf Bein-Ebene**. Das ist die Situationsklasse „kein frischer Cross" nach 7b1c (nicht „stale": der Markt lief die meiste Zeit MIT der Gate-Richtung).

**Q2-Verlauf in dieser Phase (Option A, |Entry − EMA50(5min)| / ATR):**

| 18:00 | 18:30 | 19:00 | 19:30 | 20:00 | 20:30 | 21:00 | 21:30 |
|---|---|---|---|---|---|---|---|
| 3,28 | 3,86 | 4,64 | 4,57 | 4,08 | 3,50 | 2,11 | 0,62 |

Q2 ≤ 1,5 wurde im gesamten Long-Zustand **erst ab 21:05** erreicht — also erst, nachdem die EMA50 den Kurs eingeholt hatte, in der letzten knappen Stunde vor Handelsschluss. Das ist dasselbe strukturelle Muster wie am 11.09. ([[project_testtag_analyse_2026-09-11]]), nur mit der neuen Formel: **der Reifegrad-Filter schließt den Trendteil der Bewegung aus und öffnet erst in der Erschöpfungsphase.** Heute war das einmal richtig (der Trendteil war zu weit gelaufen, aber er lief trotzdem noch 117 Punkte) und einmal falsch (die Öffnung um 21:05–21:45 traf exakt die Umkehr).

---

## Moment 1 — 15:30 DE, SHORT (2/2-Zustand ohne frischen Cross)

**Einordnung vorweg:** Das ist **kein 2/2-TRIGGER-EREIGNIS** nach der Definition in 7b1 („frischer bestätigter Cross"). Der Zustand bestand seit 10:10 DE. In der Bilanz zählt er als *2/2-Zustand ohne 7b1-Ablauf*. Er wird hier trotzdem voll durchgerechnet, weil ein um 15:30 startender Loop ihn beim ersten Voll-Check als 2/2 ausgewiesen hätte (`vollcheck.cjs` druckt die Schritt-0-Aufforderung bei jedem 2/2, unabhängig von der Frische) — und weil Levi genau wissen will, was passiert wäre.

**Rohwerte (rekonstruiert):**
- Entry-Kandidat 28.885,45 (Schluss der 15:25–15:30-Kerze), EMA50(5min) 28.888,93 → **Q2 = 0,12× ATR**
- ATR(14, 5min) 28,8 | ADX 18,9 | Trend-Effizienz(12) 0,021 (Netto −4,8 Pkt über 12 Kerzen)
- Bein NAS100 15min: Close 28.886,20 < EMA50 28.959,56 (15:15-Bar) ✓ short
- Bein QQQ 15min: Close 703,50 < EMA50 708,07 (15:15-Bar) ✓ short
- 1H-Override: 14:00-Bar, Close 28.862,10 < EMA50 29.150,53 → short, **nicht dagegen**
- Struktur-Anker (Short = höchstes HIGH abgeschlossener 5min-Kerzen seit dem Impuls-Tief 28.810,80 der 15:05-Kerze): **28.912,15, High der 15:30-Kerze**
- Wick-Zone (8c2): 28.885,10–28.927,60, die mehrfach per Docht getestete Zone der 14:15–14:30-Kerzen → `--cluster-level 28885.1-28927.6`

**Schritt 0 — SL-Anker-Vorprüfung (echter Lauf, wörtlich):**
```
SL-AUTO (A-2/A-3, 11.09.2026): SL = MAX(Struktur 28912.15 + 0,5x ATR = 28926.55 | Cluster-Kante + 0,5x ATR = 28942 | 8c-Floor Entry + 1.5x ATR = 28928.65) = 28942 -> bindet: sichere Cluster-Kante + 0.5x ATR (Zonen: 28885.1-28927.6 -> 28942)
  [PASS] 8c2: ... Zone 28885.1-28927.6 (Band): SL jenseits der sicheren Kante 28927.6, Abstand 14.4 Pkt -> PASS (Puffer 14.4 >= 14.4)
  [PASS] 8c: SL-Distanz 56.5 Pkt ÷ ATR(5min) 28.8 Pkt = 1.96x — Pflicht >=1.5x = 43.2 Pkt (normal)
  URTEIL: SL-ANKER TAUGLICH — 8c2 PASS, 8c PASS.
```
Bemerkenswert: die Cluster-Kante bindet mit **exakt 14,4 = 14,4** — der 8c2-Puffer geht gerade eben auf. Hätte die Zone 0,1 Punkt höher gereicht, wäre der Anker UNTAUGLICH gewesen und der Moment hätte nach Schritt 0 geendet (was, siehe Ausgang, besser gewesen wäre).

**Schritte 1–5 — Live-Gate-Check (echter Lauf, Auszug wörtlich):**
```
  [PASS ✓] RR-Gate (8b): RR 1.511:1 (TP1 85.5 Pkt / SL 56.5 Pkt) — Pflicht >=1:1
  [PASS ✓] SL-Mindestdistanz (8c): SL-Distanz 56.5 Pkt ÷ ATR(5min) 28.8 Pkt = 1.96x
  [PASS ✓] TP-Realismus (8b1, Drei-Zonen): TP1 85.5 Pkt = 2.97x ATR -> Zone 2 (halbe Position)
  [PASS ✓] TP2-Realismus (8b2): TP2-Level 28750 (135.5 Pkt) = RR 2.395:1 -> voll gueltig
  [PASS ✓] SL-Cluster-Zuschlag (8c2): ... Abstand 14.4 Pkt -> PASS
  --> GESAMTSTATUS: PASS  [Sizing-Flag: halbe Position]
  [Q-Faktor Q2 = JA] 0.12x ATR (|Entry 28885.45 - EMA50 28888.93| = 3.48 Pkt / ATR 28.8)
  [Q-Faktor Q3 = JA] Alle vier Ebenen kohaerent
  [Q-Faktor Q4 = NEIN] Runway-Ratio 0.41 (Gegenlevel Rundzahl 50er @ 28850, 35.45 Pkt vor TP1)
```
- **mit `--q1-reject yes`** (Annahme: die 15:25/15:30-Kerzen machen Hochs 28.903,90/28.912,15 und schließen beide darunter zurück = Wick-Rejection nach oben) → **Q-SCORE: 3/4 GELB (halbe Position)** → **Einstieg freigegeben.**
- **mit `--q1-reject no`** → **Q-SCORE: 2/4 ROT (Auslassen empfohlen).**

Zusätzlich hätte `gate_check.cjs` die Plausibilitätswarnung ausgegeben: *„--chasing no übergeben, aber --kerzen-nas100 15 >= 4 — das Punkt-13-Kriterium ist formal erfüllt"*. Und das Halbierungsfenster 15:30–16:00 hätte ohnehin gegriffen (Stacking: trotzdem nur einmal halbiert).

**Was tatsächlich passierte (echte Folgebars):**
- 15:35: Tief 28.862,45 → **MFE nur 23,0 Pkt** (TP1 hätte 85,5 Pkt gebraucht)
- 15:40: Hoch 28.938,25 — der engere 8c-Floor-SL 28.928,65 wäre hier gerissen
- **15:45: Hoch 29.019,45 → SL 28.942 getroffen. −1R nach 15 Minuten.**
- Danach lief der Kurs bis 17:40 weitere ~180 Punkte gegen die Position. MAE gegen den Entry: 146,3 Pkt.

**Bewertung:** Ein Rausch-Entry direkt an der EMA50 (Abstand 3,5 Pkt) in einer Phase mit ADX 18,9 und Trend-Effizienz 0,02 — die Maschine hätte hier bei Q1=ja eine halbe Position freigegeben. **Der Q-Score hat diesen Moment nicht zuverlässig gefiltert; er hing an einer Ermessensfrage (Q1).** Das ist der wichtigste Kalibrierungsbefund des Tages.

---

## Moment 2 — 17:55 DE, LONG (das einzige echte 2/2-Trigger-Ereignis)

**Warum genau hier:** Die beiden Beine standen schon seit 17:25 long (NAS100 15min seit dem 15:45-Schluss, QQQ 15min seit dem 17:15-Schluss), aber der 1H-Override stand bis dahin dagegen (16:00-Bar: Close 29.001,75 < EMA50 29.138,64). Mit dem Schluss des 17:00-Bars (Close 29.169,15 > EMA50 29.139,84) fiel der Override weg — **im Voll-Check um 17:55 wird das Dual-Gate erstmals gültig 2/2 LONG.**

**Rohwerte (rekonstruiert):**
- Entry 29.169,15 (Schluss der 17:50–17:55-Kerze), EMA50(5min) 29.005,84 → **Q2 = 4,24× ATR**
- ATR(14, 5min) 38,5 | ADX 36,7 | Trend-Effizienz(12) 0,601 (Netto **+204,7 Pkt** über 12 Kerzen = 5,3× ATR)
- Struktur-Anker (Long = tiefstes LOW abgeschlossener 5min-Kerzen seit dem Impuls-Hoch 29.201,85 der 17:40-Kerze): **29.157,85, Low der 17:50-Kerze**
- 8c2: keine per Docht getestete Zone im Trigger-Radius — der Kurs war heute vor 17:30 nie über 29.150 → `--cluster-level none`

**Schritt 0 (echter Lauf, wörtlich):**
```
SL-AUTO: SL = MAX(Struktur 29157.85 - 0,5x ATR = 29138.6 | Cluster-Kante - 0,5x ATR = n/a (keine Zone im Trigger-Radius) | 8c-Floor Entry - 1.5x ATR = 29111.4) = 29111.4 -> bindet: 8c-Floor
  URTEIL: SL-ANKER TAUGLICH — 8c2 PASS, 8c PASS.
  TP1-Fenster mit DIESEM SL: [29226.9 ; 29284.65] = 57.8 Pkt breit ... Register-Level im Fenster: Rundzahl 50er @ 29250
```

**Schritte 1–5 (echter Lauf, Auszug wörtlich):**
```
  [PASS ✓] RR-Gate (8b): RR 1.4:1 (TP1 80.8 Pkt / SL 57.8 Pkt)
  [PASS ✓] SL-Mindestdistanz (8c): 57.8 Pkt ÷ 38.5 = 1.5x
  [PASS ✓] TP-Realismus (8b1): TP1 80.8 Pkt = 2.1x ATR -> Zone 2 (halbe Position)
  [PASS ✓] TP2-Realismus (8b2): TP2-Level 29290.87 (121.7 Pkt) = RR 2.108:1 | Register-verifiziert: Pivot PP @ 29290.87
  --> GESAMTSTATUS: PASS  [Sizing-Flag: halbe Position]
  ENTRY-REIFEGRAD-ESKALATION (P2, kein Gate): Reifegrad (EMA50-Anker) 4.24x ATR > 1.5x, Q2-Budget -182.8% —
     Entry liegt weit von der dynamischen Struktur entfernt / IN der laufenden Bewegung.
  Chasing-Status (Punkt 13, P6): JA -> halbe Position
  [Q-Faktor Q1 = NEIN] Kein sauberes Ablehnungs-/Timing-Signal aus 8a2/8c2
  [Q-Faktor Q2 = NEIN] 4.24x ATR (163.31 Pkt / ATR 38.5), Schwelle <=1,5x
  [Q-Faktor Q3 = JA]  Alle vier Ebenen kohaerent
  [Q-Faktor Q4 = NEIN] Runway-Ratio 0.38 (Gegenlevel Rundzahl 100er @ 29200, 30.85 Pkt vor TP1)
  --> Q-SCORE: 1/4 ROT (Auslassen empfohlen)
```
*(Q1 = nein ist hier eine Annahme, aber eine gut belegte: es gab keinen gescheiterten Gegenversuch und kein Reclaim-Fenster — der Kurs lief in fünf Kerzen durch.)*

**Regelfolge:** PASS + Q-ROT → **Auslassen empfohlen**, Retest-Zeitbox 2 Voll-Checks (Bedingung: sauberer Entry mit Q ≥ 3/4 — Kurs im Entry-Fenster [29.169,15 ; 29.180,70] oder bestätigter Retest näher am Anker). **Diese Bedingung wurde in VC+1 (18:00) und VC+2 (18:05) nicht erfüllt** (Schlüsse 29.136,65 und 29.133,65, beide unter dem Entry-Fenster) → Setup wäre **verfallen**.

**Was tatsächlich passierte (echte Folgebars):**
- 18:00–18:10: Rücksetzer bis 29.125,45 → **MAE 43,7 Pkt** gegen SL-Distanz 57,8 Pkt. Der SL hielt, aber mit nur 14,1 Punkten Luft.
- **18:45: TP1 29.250 erreicht → +1,4R auf den TP1-Anteil.**
- 19:55: Tageshoch der US-Session 29.286,65 — **TP2 29.290,87 um 4,22 Punkte verfehlt.**
- 21:50: Rückkehr auf 29.167,35 → **nachgezogener BE-Stop hätte den Rest bei Breakeven geschlossen.**
- Netto nach der TP1+BE-Mechanik: etwa **+0,7R** auf eine ohnehin halbierte Position (Zone 2 + Chasing → einmalige Halbierung).

**Bewertung:** Das ist die eine „verpasste Chance" des Tages — und sie ist **regelkonform ausgelassen worden**. Der Entry lag 4,24× ATR über der EMA50, nach 204 Punkten in 60 Minuten ohne Konsolidierung, mit einer Rundzahl 29.200 direkt im Weg (Runway 0,38). Dass er trotzdem TP1 erreichte, ist ein echter Datenpunkt gegen die Q2-Schwelle — aber **einer, der den Rücksetzer bis auf 14 Punkte an den SL heran mitbringt und TP2 um 4 Punkte verfehlt.** Kein Lehrbuch-Trade, den man sich zurückwünschen müsste.

---

## Moment 3 — 21:45 DE, LONG (8a2-Reclaim im laufenden 2/2, bestes Q-Score des Tages)

**Warum überhaupt:** Zwischen 21:35 und 21:45 fiel der 5min-Schluss erstmals seit 15:40 unter die EMA50 (21:35: 29.193,15 vs. 29.194,94) und wurde in den beiden Folgekerzen zurückerobert (21:40: 29.196,65 / 21:45: 29.204,35) — ein **8a2-Zwei-Kerzen-Reclaim** innerhalb des bestehenden 2/2-Long. Die 5min-Ebene ist kein Dual-Gate-Bein, aber die Trigger-Feinsteuerung; das ist der einzige Moment im Fenster, an dem „frischer Cross" auf der Trigger-Ebene und Q2 ≤ 1,5 zusammenfielen.

Ein Einstieg genau bei 21:40 wäre am TP-Realismus gescheitert (TP1 29.250 = 53,35 Pkt = **3,27× ATR → Zone 3, Ausschluss**). Erst mit der Bestätigungskerze 21:45 (ATR 16,0) rutscht TP1 in Zone 2.

**Rohwerte:** Entry 29.204,35 | EMA50(5min) 29.195,37 → **Q2 = 0,56×** | ATR 16,0 | ADX 28,3 | Anker 29.191,45 (Low der 21:40-Kerze) | Wick-Zone 29.191,45–29.213,95 (Konsolidierung 21:20–21:45) | 1H-Override: 20:00-Bar, 29.239,75 > 29.152,61 → long, nicht dagegen.

**Echte Läufe (Auszug wörtlich):**
```
SL-AUTO: SL = MAX(Struktur 29191.45 - 0,5x ATR = 29183.45 | Cluster-Kante - 0,5x ATR = n/a | 8c-Floor = 29180.35) = 29180.35
  URTEIL: SL-ANKER TAUGLICH — 8c2 PASS, 8c PASS.

  [PASS ✓] RR-Gate (8b): RR 1.902:1 (TP1 45.7 Pkt / SL 24 Pkt)
  [PASS ✓] TP-Realismus (8b1): TP1 45.7 Pkt = 2.85x ATR -> Zone 2 (halbe Position)
  [PASS ✓] TP2-Realismus (8b2): TP2-Level 29290.87 (86.5 Pkt) = RR 3.605:1 | Register-verifiziert: Pivot PP
  [PASS ✓] SL-Cluster-Zuschlag (8c2): Abstand 11.1 Pkt -> PASS (Puffer 11.1 >= 8)
  --> GESAMTSTATUS: PASS  [Sizing-Flag: halbe Position]
  [Q-Faktor Q1 = JA] 8a2-Reclaim-Fenster
  [Q-Faktor Q2 = JA] 0.56x ATR (8.98 Pkt / ATR 16)
  [Q-Faktor Q3 = JA] Alle vier Ebenen kohaerent
  [Q-Faktor Q4 = JA] Runway-Ratio 1.9 (kein Register-Level zwischen Entry und TP1, Pivot PP @ 29290.87 jenseits TP1)
  --> Q-SCORE: 4/4 GRÜN (volle Position)
```

**Was tatsächlich passierte:**
- **21:50: Tief 29.167,35 → SL 29.180,35 getroffen. −1R nach 5 Minuten.** MFE davor: 0,6 Punkte.
- 21:55: weiter bis 29.127,25. Der Reclaim war ein Fehlausbruch unmittelbar vor dem Schlussabverkauf.

**Zwei Einschränkungen, die diesen Moment relativieren:**
1. **Terminalbedingung 21:45** ([[feedback_live_trading]] Punkt 15d): Auf Testtagen gilt genau zu dieser Uhrzeit die Time-Boxed-Exit-Prüfung für offene Positionen. Eine **neue** Position exakt am Terminal-Zeitpunkt zu eröffnen, widerspricht dem Sinn dieser Regel — verboten ist es nach dem Buchstaben nicht, aber es wäre eine Entscheidung gegen die eigene Terminierung.
2. **Trend-Effizienz(12) = 0,688 bei Netto −49,4 Punkten**: der Wert ist hoch, aber die 12-Kerzen-Richtung war zu diesem Zeitpunkt bereits **abwärts** — das Dual-Gate stand long, die kurzfristige Struktur lief schon dagegen. Der Q-Score misst das nicht.

**Bewertung:** Das ist der unangenehmste Befund. **Das einzige 4/4-GRÜN des Tages — volle Positionsgröße nach Q-Score — war der schlechteste Einstieg des Tages.** Q2 und Q4 waren „grün", weil die EMA50 nach fünf Stunden Aufwärtsbewegung endlich aufgeschlossen hatte und der Weg zur nächsten Rundzahl frei war — beides sind bei einer auslaufenden Bewegung genau die falschen Signale.

---

## Gesamtbewertung

### 1. Hätte heute eine gute Tradingchance bestanden, die durch den fehlenden Loop verpasst wurde?
**Nein.** Es gab genau **ein** frisches 2/2-Trigger-Ereignis (17:55 Long). Es hätte die harten Gates bestanden (RR 1,4:1, SL-Floor 1,5× ATR, Zone 2, 8c2 nicht einschlägig) und wäre dann am Q-Score gescheitert (1/4 ROT, „Auslassen empfohlen"). Genau dieser Trade hätte TP1 erreicht (+1,4R, ~+0,7R nach TP1+BE-Mechanik auf halber Position). Das ist der einzige Betrag, der heute auf dem Tisch lag — und er lag dort nur, wenn man die eigene Chasing-Regel bewusst gebrochen hätte.

### 2. Wären wir fälschlicherweise eingestiegen und hätten einen SL kassiert?
**Ja — und zwar an beiden Stellen, an denen der Q-Score grünes oder gelbes Licht gegeben hätte.**
- 15:30 Short: PASS + 3/4 GELB (bei Q1=ja) → SL nach 15 Minuten, −1R
- 21:45 Long: PASS + **4/4 GRÜN** → SL nach 5 Minuten, −1R

Beide waren Entries an der EMA50 — genau die Konstellation, die Q2 (Option A) als „reif" belohnt. Beide waren in Wahrheit Rausch- bzw. Erschöpfungs-Entries.

### 3. Kalibrierungs-Datenpunkte für das 7b1a-Prüfkriterium Option A (heute verbindlich beschlossen)
Das Prüfkriterium verlangt nach ≥15 Q-bewerteten Setup-Momenten: (a) Summe R der unter A freigegebenen (GELB/GRÜN) Momente > 0 UND (b) höchstens 1 von 3 Freigaben mit SL-Hit ≤ 30 Min.

Der heutige Tag liefert **2 Freigaben** (15:30 GELB bei Q1=ja, 21:45 GRÜN) mit:
- **Summe R = −2,0** → Kriterium (a) verletzt
- **2 von 2 mit SL-Hit ≤ 30 Min** (15 Min bzw. 5 Min) → Quote 100 %, Kriterium (b) klar verletzt

Und **1 Q-ROT-Ablehnung** (17:55), die nachträglich **falsch** war (+1,4R vermieden statt verloren) — das Gegenteil des 09.09.-Befunds, wo alle vier Q-ROT-Ablehnungen nachträglich korrekt waren.

**Wichtig zur Einordnung:** n = 2 bzw. 3 bei einem Kriterium, das 15 Momente verlangt. Das ist **kein Urteil über Option A**, sondern ein Datenpunkt. **Er zählt derzeit auch nicht mit**, weil er nicht in `skipped_setups_fiktiv.jsonl` steht — es lief kein Testtag, es lief kein Loop, und ich habe die Produktionsdateien bewusst nicht angefasst. *Offene Frage an Levi: sollen diese drei rekonstruierten Momente nachträglich als fiktive Setup-Momente in den Option-A-Nenner aufgenommen werden (dann wäre die Rekonstruktions-Herkunft zu kennzeichnen), oder bleibt der Nenner echten Loop-Tagen vorbehalten?* Meine Empfehlung: **nicht aufnehmen** — Q1 ist an zwei Stellen eine Ermessensentscheidung von mir statt eine Loop-Ablesung, und ein Nenner aus gemischtem Material ist genau das Problem, das 7b1c mit der Terminologie-Regel vom 28.08. schon einmal hatte.

### 4. Was der Tag strukturell zeigt
Der Tag bestätigt den Kernbefund vom 11.09. in neuer Form: **Entry-Timing ist der Hebel, nicht die Gate-Logik.** Die harten Gates (RR, 8c, 8c2, TP-Realismus) haben an allen drei Momenten sauber und widerspruchsfrei gearbeitet — 3 von 3 SL-Anker TAUGLICH, kein einziger Gate-FAIL. Was nicht funktionierte, war die Auswahl *innerhalb* der PASS-Menge: Der Q-Score ordnete den einzigen profitablen Moment als ROT ein und den verlustreichsten als GRÜN. Das liegt daran, dass Q2 (EMA50-Abstand) und Q4 (Runway) beide **monoton besser werden, je länger eine Bewegung schon gelaufen ist** — die EMA50 holt auf, und die nahen Gegenlevel sind bereits durchhandelt. An einem Trendtag belohnt diese Kombination systematisch den Zeitpunkt kurz vor der Umkehr.

**Ausdrücklich keine Regeländerung aus dieser Analyse** ([[feedback_dont_change_running_system]], [[feedback_live_trading]] Punkt 14): Option A ist seit heute verbindlich und wird planmäßig nach 15 Momenten ausgewertet. Ein einzelner nachgerechneter Tag ist kein Anlass, sie vorher anzufassen. Der Befund gehört in die Auswertung, nicht in eine Sofortmaßnahme.

### 5. Nebenbefunde für Levi
1. **Register-Defekt (siehe Methodik):** `Session-Hoch (laufend) 29.428,85` in `scripts/level_register.json` stammt nicht vom Montag. Korrekt wäre 29.370,40 (Montags-Hoch) bzw. 29.286,65, wenn man nur die US-Session 15:30–22:00 meint. Ein falsches Session-Hoch verschiebt TP2-Kandidaten und die Stale-Bedingung 4. **Nicht korrigiert** — das ist eine Produktionsdatei und gehört ins nächste Session-Update, nicht in eine Analyse.
2. **8d/K3 (VIX) ist nicht messbar, solange kein VIX auf einer Pane liegt.** Heute wäre es entscheidungserheblich gewesen: K2 war erfüllt, K1 knapp nicht (1,44×) — bei erfülltem K3 hätte 8b1a alle drei Momente ohne RR-Versuch ausgeschlossen. Falls der Schock-Tag-Test ernst gemeint bleibt, braucht er eine verlässliche VIX-Quelle im Loop.
3. **Die 8c2-Zone am 15:30-Moment ging mit exakt 0,0 Punkten Reserve auf** (Puffer 14,4 ≥ 14,4). Im Nachhinein hätte ein UNTAUGLICH dort Geld gespart. Das ist kein Argument für eine Schwellenänderung (n=1), aber ein Fall für die Sammlung.

---

## Nachtrag 14.09.2026 abends — Rückfrage Levi: „Warum nur Q-Score 1/4? War bei Chasing nicht 50 % der Position?"

**Levis Frage ist berechtigt und trifft eine echte Lücke in der Darstellung oben — aber nicht in der Zahl.** Die Prüfung im Einzelnen (Regeltexte + realer `gate_check.cjs`-Lauf, erneut in einer Sandbox-Kopie von `scripts/`, keine Produktionsdatei berührt):

### 1. Q-Score und Chasing (Punkt 13/13.1) sind zwei getrennte Mechanismen — der Bericht oben hat sie vermengt

Im Code (`gate_check.cjs`) sind sie **vollständig entkoppelt**: `--chasing yes` schiebt ausschließlich den String `'Chasing (Punkt 13)'` in `halbierungsGruende` (Zeile 2061) und beeinflusst **keinen** Q-Faktor. Q2 wird allein aus `--ema50-5min` gerechnet. Es gibt also **keine Doppelbestrafung im Rechenweg** — wohl aber eine inhaltliche Überschneidung: derselbe Sachverhalt („Einstieg spät in einer gelaufenen Bewegung") kostet einmal den Q2-Punkt und löst einmal die Halbierung aus.

Der reproduzierte Lauf für 17:55 gibt **beides gleichzeitig** aus (wörtlich):
```
  Chasing-Status (Punkt 13, P6): JA — Chasing-Situation -> halbe Position (Punkt 13.1, Einmal-Halbierung, ...)
  --> Q-SCORE: 1/4 ROT (Auslassen empfohlen)  [offen: Q1 (nicht erfuellt), Q2 (nicht erfuellt), Q4 (nicht erfuellt)]
  Kombinierter Sizing-Hinweis: halbe Position (EINMALIG, nicht kumulativ — Gruende: TP-Realismus Zone 2 (8b1) + Chasing (Punkt 13))
```
Diese letzte Zeile fehlt im Abschnitt „Moment 2" oben — sie ist der Beleg dafür, dass Q-ROT die Chasing-Halbierung **nicht** unterdrückt. Q-ROT beantwortet „steige ich ein?", Chasing beantwortet „wenn ja, wie groß?". Zwei Fragen, nicht eine.

**Wichtig zur Einordnung des Q-Scores hier:** Das ROT hängt nicht am Chasing/Q2. Q1 = NEIN und Q4 = NEIN (Runway 0,38, Rundzahl 29.200 nur 30,85 Pkt vor TP1) stehen unabhängig davon. Selbst mit Q2 = JA wären es 2/4 — nach der Ampel-Logik 7b1a (`≤2/4 → ROT`) **weiterhin ROT**.

### 2. Q-ROT ist KEIN hartes Veto — das steht wörtlich im Regelwerk und fehlte oben

[[feedback_live_trading]] 7b1a, Abschnitt „Scharfschaltungsstufe": *„Für die ersten 8 Trades des laufenden 15-Trade-Fensters wirkt der Q-Score AUSSCHLIESSLICH als Anzeige/Sizing-Signal (GELB → halbe Position) und Empfehlung (ROT → ‚Auslassen empfohlen'), **kein hartes Veto** — Levi/Sonnet behalten bei ROT trotzdem die Entscheidung, den Trade zu nehmen."* Das 15-Trade-Fenster läuft seit 24.08.2026; `trades.db` enthält **0 Trades ab dem 24.08.** (geprüft, read-only auf der Sandbox-Kopie) — wir stehen also bei 0 von 8 und sind eindeutig in dieser Stufe. Die Formulierung oben („er lag dort nur, wenn man die eigene Chasing-Regel bewusst gebrochen hätte") ist damit **sachlich falsch und wird hiermit zurückgenommen**: Die Chasing-Regel verbietet den Trade nicht, sie bemisst ihn. Wäre er mit 50 % genommen worden, wäre das eine **Anwendung** von Punkt 13.1 gewesen, kein Bruch.

### 3. Der letzte vorgeschriebene Prüfschritt wurde oben nicht ausgeführt — nachgeholt

`gate_check.cjs` druckt selbst den Weg, den der Abschnitt „Moment 2" mit „→ Setup wäre verfallen" abgebrochen hat (wörtlich aus dem Lauf):
```
  RETEST-ZEITBOX (P1): Nach VC+2 ohne erfuellte Bedingung: Setup VERFALLEN — dann nur noch 13.1-Pruefung
  (Chasing-Kriterien ueber dieselben 2 Checks sauber erfuellt -> 50 % vorschlagen) oder Auslassen.
```
Deckungsgleich [[feedback_chartanalyse]] 8b Schritt 4, Zeitbox-Punkte 4 und 5. **Die 13.1-Prüfung für VC+1 (18:00) und VC+2 (18:05) nachgeholt:**
- Dual-Gate über alle Ebenen weiterhin sauber: **ja** (2/2 long bis 21:50, 1H long).
- Punkt-11-Kippsignal (seit 14.09. verbindlich: ≥1 erfülltes Kriterium = Signal): Kriterien 2/3/4 klar **nicht** erfüllt (Kurs weit ÜBER EMA50, QQQ weiter long, 15min/1H weiter long).
- **Chasing-Kriterien selbst („4-5 klar gerichtete Kerzenschlüsse ohne Konsolidierung"): NICHT MEHR erfüllt.** 18:00 und 18:05 schlossen beide gegen die Richtung (29.136,65 / 29.133,65), Tief 18:10 bei 29.125,45 — also **32,4 Punkte unter den Struktur-Anker 29.157,85** und deutlich unter das Entry-Fenster [29.169,15 ; 29.180,70]. Die Bewegung hat in genau den zwei Checks, in denen 13.1 das „Halten" verlangt, **nicht gehalten**, sondern die Struktur des Triggers gebrochen.

→ Damit greift **Zeitbox-Punkt 5** („weder Retest noch saubere Chasing-Kriterien → Auslassen"), nicht Punkt 4. Und ein Wiederbeleben ist ausgeschlossen: *„Ein späterer, neuer 2/2-Trigger-Ereignis startet einen neuen Ablauf — kein Wiederbeleben des verfallenen"* — einen solchen neuen frischen Cross gab es bis 21:50 nicht.

### 4. Ergebnis: Zahl bleibt, Begründung wird ersetzt

**Keine Zahlenkorrektur.** Der 17:55-Moment bleibt „ausgelassen, 0R", die Gesamtrechnung (0R regelkonform / −0,6R alle drei / −2R nur Q-freigegeben) bleibt unverändert. **Aber die Begründung oben war die falsche:** „ausgelassen wegen Q-ROT" trägt nicht (Q-ROT ist Empfehlung, kein Veto). Es trägt: **„ausgelassen, weil die Retest-Zeitbox ohne erfüllte Bedingung ablief UND die Chasing-Kriterien in denselben zwei Voll-Checks brachen"** — der Rücksetzer unter den Struktur-Anker hat den 50-%-Pfad geschlossen, nicht der Q-Score.

Zur Größenordnung, damit die Erwartung stimmt: Bei 50 % Position bleibt das **R-Multiple** unverändert (+1,4R auf dem TP1-Bein, netto ≈ +0,7R nach TP1+BE) — halbiert wird der **Euro-Betrag**, nicht das R. Die Halbierung wäre ohnehin einmalig gewesen (Zone 2 und Chasing stapeln nicht).

### 5. Echter Regelwerksfund — offen, hier NICHT entschieden

Zwischen zwei Regeln, die beide eine Rechtsfolge zur Einstiegsentscheidung aussprechen, existiert **keine Präzedenz-Regel**:
- **13.1:** *„aktiv die halbierte Position als Handlungsvorschlag formulieren"* — eine **Pflicht**, sobald Chasing-Kriterien + vollständiges Dual-Gate + kein Punkt-11-Signal über 2 Checks vorliegen.
- **7b1a Q-ROT:** *„Auslassen empfohlen"* — ausdrücklich **kein hartes Veto**, die Entscheidung bleibt bei Levi/Sonnet.

Der einzige Stacking-Absatz in 7b1a regelt nur (a) 8b1a-Schock vor Q-Score und (b) die Nicht-Kumulation der Halbierung — **nicht**, was gilt, wenn 13.1 „50 % aktiv vorschlagen" und der Q-Score „auslassen empfohlen" gleichzeitig sagen. Heute ist die Kollision nur deshalb nicht scharf geworden, weil die Chasing-Kriterien in VC+1/VC+2 brachen. Zwei Teilfragen für Levi:
1. **Wer entscheidet bei PASS + Q-ROT + 13.1-Pflichtvorschlag** — und im Solo-Loop (Punkt 15), wenn Levi gar nicht ansprechbar ist? Die Scharfschaltungsstufe reserviert die Entscheidung für „Levi/Sonnet", benennt für den unbeaufsichtigten Betrieb aber keine Default-Handlung.
2. **Ab wann läuft der k/2-Zähler aus 13.1?** Der Wortlaut sagt „ab dem ersten Voll-Check mit erfüllten Chasing-Kriterien" — muss das vollständige Dual-Gate (inkl. 1H) während dieser 2 Checks schon stehen, oder erst im Moment des Vorschlags? Heute konkret: die Chasing-Kriterien waren schon ab ~17:35 erfüllt, das Dual-Gate erst ab 17:55. Bei der zweiten Lesart hätte 13.1 **bereits um 17:55** „50 % aktiv vorschlagen" verlangt — zeitgleich mit dem Q-ROT. Das ist genau die ungeklärte Kollision.

*(Beide Punkte bewusst nur benannt, nicht entschieden — [[feedback_dont_change_running_system]], [[feedback_live_trading]] Punkt 14. Keine Regeländerung, kein Commit.)*
