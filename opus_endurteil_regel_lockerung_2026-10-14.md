---
name: opus_endurteil_regel_lockerung_2026-10-14
description: "Opus-Endurteil 14.10.2026 nach Fable-Stellungnahme: Fable in den Kernpunkten bestaetigt und schaerfer nachgerechnet - ausfuehrbares Kern-Regelwerk (Entry naechste Eroeffnung, ab 15:35) Oe nur ~0 bis +0,1 R vor KO-Spread, ~0 danach; 08.10.-Re-Entry: Fable hat recht (1-Positions-Regel); 'Filter kosteten Geld' zurueckgenommen; SL-Cap 3 ATR einzig robuste Zahl; finales 12-Regel-Werk mit Cross-Trigger, Spread-Gate 10 %, 8c2-Einzeiler; Testplan = Levis 10 Fiktivtage als Maximum mit binaeren Kriterien, Zwischenanalyse + Echtgeld halbes Risiko ab Tag 6"
metadata:
  type: project
  originSessionId: opus-endurteil-regel-lockerung-2026-10-14
  modified: 2026-10-14T10:28:54.490Z
---

# Opus-Endurteil Regel-Lockerung — 14.10.2026 (nach Fable-Stellungnahme)

Grundlagen: [[opus_vorschlag_regel_lockerung_2026-10-14]] (mein Bericht, 11:49-12:02, bleibt unveraendert als Zeitdokument), [[fable_stellungnahme_regel_lockerung_2026-10-14]] (vollstaendig gelesen). Jede Fable-Abweichung habe ich selbst auf den Rohdaten nachgerechnet (neues Lese-Skript `%TEMP%\opus_1014\ana\sim3.cjs` / `sim3g.cjs`: Entry wahlweise Gate-Kurs oder naechste 5m-Eroeffnung mit neu gerechnetem R und neu geprueftem Cap, Start/Cut, Dedup Datum+Richtung+Anker, Position belegt bis Ende der Exit-Kerze, Re-Entry-Pause, Spread).

**Zeitstempel:** Start 12:25:15 DE, Ende 12:40 DE. sha1 der 44 Originale (Logs, trades.db, Register, Bars, loop_archiv) 12:25:15 und 12:27:23 = Stand 11:49:54 **IDENTISCH**. Nur Kopie in `%TEMP%\opus_1014`; kein git, kein test:e2e, kein CDP, keine Aenderung an Skripten/Regeln/Memories, MEMORY.md nicht angefasst.

---

## Kurzfazit (10 Zeilen)

1. **Fable hat in den drei Zahlenpunkten recht, und meine Nachrechnung ist sogar etwas ungünstiger als seine.** Die +0,23 R aus meinem Bericht gelten fuer Entries zum Momentkurs mitten in der Kerze (inkl. 15:32), die mein eigenes Regelwerk nicht erlaubt.
2. **Ausfuehrbares Regelwerk (Entry naechste Eroeffnung, ab 15:35, SL-Cap neu geprueft, 2 Pkt Spread):** dedupliziert n 14, Σ +1,55 R, Ø **+0,11 R** (±0,40), ohne besten Tag +0,03; roh-sequenziell n 15, Ø **+0,04**; Entries bis 19:00: Ø +0,07 (dedup) bzw. -0,01 (roh). **Mit 9 Pkt Spread: Ø ≈ 0 (dedup) bis -0,29 (roh).** Fables Korridor (+0,17/+0,20) ist leicht optimistischer als meiner — die gueltige Zahl ist: **Kante ≈ 0 bis +0,1 R vor Kosten, nach realistischem Schein-Spread nicht positiv.**
3. **08.10.:** Fable hat recht. 18:16 war bei laufender 17:56-Position (SL erst spaeter in der Kerze) eine zweite Position derselben Idee — nach "1 Position zur Zeit" unzulaessig. Mein -2,00 war falsch, richtig ist ≈ +1,1 bis +1,2 R.
4. **"Die Filter kosteten eher Geld" nehme ich zurueck.** Ausfuehrbar gerechnet: FAIL-Ideen ≤ 3 ATR Ø -0,11 (n 12), Q-ROT-Ideen Ø +0,14 (n 9), Standardfehler je ±0,25 R — nicht unterscheidbar, in keine Richtung.
5. **Die Daten belegen zwei Dinge:** (a) das alte Werk erzeugt praktisch keine Trades (2 in 10 Tagen) und damit auch keine Erkenntnis; (b) der SL-Cap 3x ATR ist noetig (ohne Cap, ausfuehrbar: Ø -0,21 R). **Die Daten belegen NICHT**, dass das gelockerte Werk Geld verdient. **Die Lockerung ist Levis Entscheidung, keine Folgerung aus den Daten** — eine vertretbare, weil sie Messbarkeit herstellt.
6. **Uebernommen von Fable:** Cross-Trigger statt Zustand, eingefrorene Level-Liste, Entry = naechste Eroeffnung, Entries bis 19:00, BE = Entry ± Spread, WR := p plus p/q/c/Zeit-Exit-Anteil/manuelle Exits, Zeit-Exit als eigene Ausgangsklasse, 8c2 als Einzeiler, Spread-Gate, messbares Schock-Tier, 1m-Nachtrag, Spread-Protokoll.
7. **Verschaerft gegenueber Fable:** Spread-Gate **10 %** statt 15 % der SL-Distanz — bei einer Kante von hoechstens 0,1 R darf der Spread nicht 0,15 R fressen. Re-Entry-Pause **teilweise**: ersetzt durch "neuer Trigger UND neuer Anker" (der 18:16-Fall war ein 1-Positions-Fall, keine Zeitfrage).
8. **Finales Regelwerk: 12 Regeln** (unten), Kernformel unveraendert: TP1 = 1R (50 %), BE, TP2 = 2R, Zeit-Exit 20:00.
9. **Testplan (EINE Variante):** Levis 10 Fiktivtage als **Maximum**, vorab registrierte binaere Technik-Kriterien; sind sie an 5 gueltigen Tagen (mind. 1 Rangetag) erfuellt → Opus-Zwischenanalyse am Abend → **Echtgeld mit 37,50 € Risiko ab dem naechsten Handelstag**; sonst weiter bis Tag 10 und Analyse dann. Danach Stufen wie gehabt (10 Trades halb, bis 40 voll, Abbruch -8 R/-400 €).
10. **Ehrlich:** Wir lockern, um messen zu koennen, nicht weil die Rechnung Gewinn verspricht. Entscheidend sind Spread des Produkts, Verhalten des Cross-Triggers an Klebetagen und Levis Hand am Exit — das misst nur Echtgeld, gedeckelt auf 400 €.

---

## 1. Fable-Punkte → Urteil

| # | Fable-Punkt | Urteil | Begruendung / Zahl (eigene Nachrechnung) |
|---|---|---|---|
| A.1 | Zaehlung 56/31/23/2, 478/383/147 | **bestaetigt** | identisch |
| A.1 | Laeufe ≠ Ideen, Filterwirkung um Faktor 1,3-1,8 ueberzeichnet | **uebernommen** | 56 Laeufe = 43 Ideen (Fingerabdruck mit TP1) bzw. ~31 (Datum+Richtung+Anker). Die Prozessaussage ("2 Einstiege") bleibt unberuehrt |
| A.2 | EV-Formel korrekt; BE realisiert -c/2; Zeit-Exit traegt das Ergebnis; WR undefiniert | **uebernommen** | ausfuehrbare Variante: 5 von 14 Positionen enden per Zeit-Exit, nur 1 TP2 (q 1/6) — das Ergebnis haengt am 20:00-Exit, nicht an 1R/2R |
| A.3-1 | 3 der 4 TP2-Treffer sind 15:32/15:32/15:36-Entries, unter eigener Regel 3+6 nicht ausfuehrbar | **uebernommen, schaerfer** | Gate-Kurs + 15:35-Start: n 14, Ø +0,24 (Fable +0,24 ✓). Naechste Eroeffnung (dedup): n 14, Σ +1,55, **Ø +0,11**, ohne besten Tag +0,03, p 6/14, q 1/6; roh: Ø +0,04; Cut 19:00: +0,07 / -0,01. Fable: n 12-13, Ø +0,17/+0,20 — Abweichung durch Dedup/Sequenz, Richtung gleich, meine Zahl ungünstiger. 05.10. schrumpft wie bei Fable auf +0,96 |
| A.3-2 | 9 Pkt KO-Spread → ~0 | **uebernommen, schaerfer** | dedup Ø -0,00, roh (Cut 19:00) **Ø -0,29**; 5 Pkt: +0,03 |
| A.3-3 | 08.10. +1,10 statt -2,00 (Re-Entry-Konvention) | **uebernommen — Fable hat recht** | 18:16-Entry 30950,7 lag am Kerzen-Open 30951,15; der SL der 17:56-Position fiel erst spaeter in derselben Kerze → um 18:16 war die Position noch offen → zweite Position unzulaessig. Mit "Position belegt bis Ende der Exit-Kerze": 08.10. +1,20 (Gate-Kurs), Σ gesamt +6,33 (Fable +6,66) |
| A.3-4 | Ohne Cap negativ | **bestaetigt** | ausfuehrbar ohne Cap: n 16, Ø **-0,21**, ohne besten Tag -0,27 — robusteste Zahl des Ganzen |
| A.4 | "Filter sortierten nichts Schlechteres aus" nicht belastbar | **uebernommen, eigene Aussage korrigiert** | ausfuehrbar je Idee: FAIL ≤ 3 ATR n 12 Ø **-0,11**; Q-ROT ≤ 3 ATR n 9 Ø **+0,14** (07.10. -1,89, 08.10. +1,50); SE je ±0,25. Mit Gate-Kurs waren es +0,11/+0,47 — die Gruppenwerte kippen schon an der Entry-Konvention. Richtig ist mein Satz vom 08./13.10.: **kein Beleg in irgendeine Richtung.** Der 14.10.-Satz "kosteten eher Geld" war von der Fragestellung gefaerbt |
| B.2-1 | 8c2 als harter Einzeiler | **uebernommen** | juengster realer Verlustmechanismus (#42/#43), als automatische Pruefung ohne Pflichtfeld kostet er nichts |
| B.2-2 | Spread-Sanity-Check (22.06., 10-15 %) fehlte | **uebernommen, verschaerft** | Regel existiert (`project_risikomanagement.md` Z. 189), mein Inventar hat sie uebersehen. Schwelle **10 %**: Kante ≤ 0,1 R |
| B.2-5 / N2 | Re-Entry-Pause 15 Min nach SL | **teilweise** | im ausfuehrbaren Lauf ohne Wirkung (0 Faelle). Ersetzt durch "nach SL derselben Richtung nur mit neuem Trigger UND neuem Anker" + 1-Positions-Regel bis Ende der Exit-Kerze |
| B.3 | Verlust := Exit ≤ -0,5 R; EMA-Quelle Chartwert; Swing-Definition; Entries bis 19:00 | **uebernommen** | Cut 19:00 aendert Ø um -0,04 (Rauschen), Runway-Argument traegt. Swing-Definition: Fractal 1/1. Die von Fable angeregte "Floor-SL bei Level-Naehe"-Ausnahme **nicht** uebernommen (neue ungemessene Regel) |
| B.4 / N3 | Trigger: Cross statt Zustand, Verhalten bei 2/2-Bildung, Level-Liste einfrieren, Entry-Preis eindeutig | **uebernommen** | Festlegung in Regel 2-4 unten; 2/2-Bildung zaehlt als Trigger (das hat die Simulation gemessen) |
| B.5 | Positionsgroesse/Hebel, Scalable SL/TP-exklusiv-Luecke beim TP1 | **uebernommen** | Fill-Protokoll je Echtgeld-Trade (Dauer ohne SL, Slippage TP1) |
| B.6 | Zeit-Exit = Marktorder 19:58-20:00; Schock-Tier messbar; Slippage | **uebernommen** | Regel 7/10 |
| N1 | BE = Entry ± Spread | **uebernommen** | |
| N4 / N5 | 1m-Nachtrag Entry-Kerze; Spread-Protokoll | **uebernommen** | |
| N6 | WR := p + p/q/c/Zeit-Exit/manuelle Exits | **uebernommen** | |
| N7 | Schock-Tier ATR ≥ 2x 20-Tage-Median | **teilweise** | 20-Tage-Median ist mit 300 Bars nicht rechenbar; Bezug: Median ATR(5m) um 15:35 der letzten 10 gueltigen Tage aus den datierten Bars, Schwelle 2x vorlaeufig |
| C.3 | Testplan: Levis 10 Tage als Maximum, Uebergang ab Tag 5 | **uebernommen, ergaenzt** | + Opus-Zwischenanalyse vor dem Uebergang (Levis "danach erneut analysieren"), Spread-Gate 10 % auch im Fiktivtest, Rangetag Pflicht |
| Gesamt | "+0,23 R" haette nicht fetter stehen duerfen als "kein Beleg" | **uebernommen** | berechtigte Kritik an meiner Darstellung |

---

## 2. Finales Regelwerk (12 Regeln, je ein Satz)

1. **Richtung:** Long nur, wenn die letzten geschlossenen 15m-Kerzen von NAS100 und QQQ ueber ihrer 15m-EMA50 schliessen und die letzte geschlossene NAS100-1H-Kerze nicht unter der 1H-EMA50 schliesst (Short spiegelbildlich; EMA-Werte = Chartwerte).
2. **Trigger:** Bei gueltiger Richtung ist ein Trigger entweder (a) eine geschlossene 5m-Kerze, die in Richtung jenseits der 5m-EMA50 oder eines Levels aus Regel 3 schliesst, waehrend die Vorkerze auf der anderen Seite schloss (Cross), oder (b) der 15m-Schluss, mit dem die Richtung 2/2 wird, sofern die letzte 5m-Kerze in Richtung jenseits der 5m-EMA50 schliesst.
3. **Level-Liste:** wird im Start-Update eingefroren und besteht nur aus PDH, PDL, PP, R1, S1, Overnight-Hoch und Overnight-Tief — keine Rundzahlen, keine Nachtraege im Loop.
4. **Entry:** Marktorder zur Eroeffnung der naechsten 5m-Kerze nach dem Trigger, fruehestens 15:35, spaetestens 19:00 DE (im DST-Divergenzfenster 26.-30.10. beide Zeiten eine Stunde frueher).
5. **SL:** juengster 5m-Swing vor dem Entry (Swing = Kerze, deren Tief bzw. Hoch von der Kerze davor und danach nicht unter- bzw. ueberschritten wird) ∓ 0,5x ATR(14, 5m), mindestens 1,5x ATR vom Entry.
6. **Kein Trade,** wenn die SL-Distanz > 3,0x ATR ist, der Spread > 10 % der SL-Distanz betraegt, oder der SL innerhalb 0,5x ATR einer Zone liegt, in der heute bereits ein SL derselben Richtung per Docht ausgeloest wurde (8c2).
7. **Ausgaenge:** TP1 = Entry ± 1R → 50 % schliessen und SL auf Entry ± Spread; TP2 = Entry ± 2R; alles Uebrige 19:58-20:00 per Marktorder schliessen; keine weiteren manuellen Exits ausser Notfall (Broker-Stoerung, ungeplantes Event).
8. **Groesse:** Risiko 75 € (in der Validierung zuerst 37,50 €) geteilt durch SL-Distanz per size.cjs, ohne Halbierungen oder Q-Abstufungen.
9. **Tageslimits:** eine Position zur Zeit (belegt bis Ende der Exit-Kerze), max. 3 Trades, Stopp nach 2 Verlusten (Verlust := Exit ≤ -0,5 R) oder Tagessumme ≤ -2 R; nach einem SL dieselbe Richtung nur mit neuem Trigger UND neuem Anker.
10. **Sperrfristen:** kein Entry von 15 Min vor bis 30 Min nach FOMC, CPI, PPI, NFP, PCE, Zinsentscheid oder Fed-Chef-Rede (Liste im Start-Update), und kein Trade, wenn ATR(5m) um 15:35 ≥ 2x Median der letzten 10 gueltigen Tage ist (Schock-Tier).
11. **Broker:** SL sofort als Order, TP1/TP2 per Alarm, Stueckzahl rueckrechnen, Broker-Wert gilt, Bid/Ask des Produkts bei jedem Entry protokollieren.
12. **Messung ohne Wirkung:** Q-Score, "alt haette PASS/FAIL/ROT", Chop-Verdacht und 13.1-Lage werden automatisch geloggt und entscheiden nichts.

Kennzahlen jeder Auswertung: **p** (TP1 vor SL), **q** (TP2 je TP1), **c** (Spread+Slippage in R), **Anteil Zeit-Exits**, **Anteil manueller Exits (Soll 0)**, Ø R, Ø R ohne besten Tag, Trend/Range getrennt. "WR" = p.

---

## 3. Finale Entscheide

**E1-E12:**
1. **E1** Freeze beenden, lockern — **ja**, als Entscheidung zur Messbarkeit, nicht als Datenbefund.
2. **E2** TP1 = 1R, TP2 = 2R mechanisch — **ja**.
3. **E3** Levis Schema 50 % @1R, BE (= Entry ± Spread), Rest 2R, Zeit-Exit — **ja** (1R voll ausfuehrbar gleich: Ø +0,12 vs. +0,11; Levis eigenes Schema wird eher eingehalten).
4. **E4** SL-Cap 3,0x ATR — **ja**, robusteste Zahl (ohne Cap Ø -0,21).
5. **E5** Q-ROT-Veto streichen, Schatten — **ja**, ausdruecklich ohne Behauptung, dass es Geld kostete.
6. **E6** Streichliste wie im Bericht — **ja, ausser 8c2** (bleibt als automatischer Einzeiler, Regel 6).
7. **E7** Behalten Dual-Gate/1H/Anker/Floor/Risiko/Sperrfristen — **ja**, plus Spread-Gate 10 %.
8. **E8** Entries 15:35-19:00, Zeit-Exit 20:00 — **ja** (19:00 statt 19:30).
9. **E9** Kosten — **ja, verschaerft:** Spread-Gate 10 % hart; vor Echtgeld Produktvergleich (KO-Schein vs. CFD/Mini-Future), Ziel ≤ 3 Pkt.
10. **E10** Validierung — **Testplan Abschnitt 4** (ersetzt meinen Plan "3 Tage + Echtgeld").
11. **E11** Regel-Freeze ab Fiktivtag 1 bis Echtgeld-Trade 20, Aenderungsideen nur ins datierte Backlog — **ja**.
12. **E12** Umsetzung Fable (kern_gate + kern_nachtrag zuerst, dann kern_check/loop_prompt/position_tick, DST-Fix bis 19.10. inklusive), Opus-Gegencheck, Commit nur Levi — **ja**.

**N-Punkte (Fable N1-N7):** N1 BE = Entry ± Spread **ja** · N2 Re-Entry-Pause **ersetzt** durch Regel 9 (neuer Trigger + neuer Anker) · N3 Trigger-Definition **ja, wie Regel 2-4** · N4 1m-Nachtrag **ja** · N5 Spread-Protokoll **ja** (`--spread-pkt` Pflicht im kern_gate) · N6 WR := p + fuenf Kennzahlen **ja** · N7 Schock-Tier **ja mit 10-Tage-Median** (Regel 10).

**Neu (Opus):**
- **O1** Mein Bericht vom 14.10. bleibt stehen; seine Zahl "+0,23 R" gilt nur fuer Entries zum Gate-Momentkurs und ist fuer das Regelwerk durch "≈ 0 bis +0,1 R vor Kosten" ersetzt. Empfehlung: in jeder Besprechung nur die neue Zahl verwenden.
- **O2** Die Nachtrag-Methodik aller kuenftigen Auswertungen nutzt Entry = naechste Eroeffnung, Position belegt bis Ende der Exit-Kerze, gemessenen Spread. Empfehlung: ja.

---

## 4. Finaler Testplan (eine Variante)

**Vorab registriert (vor Tag 1, unveraenderbar):**
1. **Zweck:** Prozesstest, kein Kantentest (10 Tage ≈ 12-19 Trades, SE ≈ ±0,22 R; eine Kante von +0,1 R braeuchte > 100 Trades). Ergebnis-R ist Messgroesse, Go/No-Go nur ueber Technik und Abbruch.
2. **Binaere Technik-Kriterien (alle an 5 gueltigen Tagen, davon mind. 1 Rangetag):** (a) 0 Hard-Exits durch Pflichtfelder; (b) Trigger-Abgleich: jeder aus den Bars nachgerechnete Trigger nach Regel 2 hat einen kern_gate-Lauf und jeder Lauf einen Trigger (100 %); (c) jeder fiktive Trade hat einen 1m-Nachtrag der Entry-Kerze; (d) 100 % Zeit-Exits um 20:00 bei offenen Positionen; (e) Spread-Protokoll bei 100 % der Gates; (f) 0 Exits ausserhalb Regel 7.
3. **Zaehlregeln:** Tag gueltig bei ≥ 67 % Slot-Abdeckung 15:35-20:00 und lueckenlosen 5m-Bars; Ausfalltag zaehlt nicht und verlaengert nicht ueber 10 Kalender-Testtage; Tagestyp vor dem Ergebnis nach Effizienz 15:30-20:00 (≥ 0,55 Trend, sonst Range); ein Trade = eine Position (keine Dedup noetig); R mit gemessenem Spread.
4. **Schatten:** kern_gate loggt je Trigger "alt haette PASS/FAIL/ROT" und den Q-Score; zusaetzlich zaehlt kern_nachtrag die Momente, an denen das alte Werk ein Gate gefahren haette, das neue aber keinen Trigger hat.
5. **Abbruch (zurueck in Analyse, keine Zwischenanpassung):** ≥ 3 Tage mit -2R-Stopp innerhalb von 5 gueltigen Tagen (Trigger zu lose) · ≤ 3 Trades in 5 gueltigen Tagen (Trigger zu eng) · gemessener Spread Ø > 10 % der SL-Distanz (Produktfrage, nicht Regelfrage) · Technik-Kriterien nach 10 Tagen nicht erfuellt (Technik-Fix).
6. **Auswertungstag und Uebergang:** Sobald die Technik-Kriterien an 5 gueltigen Tagen erfuellt sind und kein Abbruch gefeuert hat → **Opus-Zwischenanalyse am Abend dieses Tages** (p, q, c, Zeit-Exits, Schatten, Trend/Range) → Echtgeld mit 37,50 € Risiko ab dem naechsten Handelstag, auch bei negativem fiktivem Ø R (Rauschen). Sonst Fiktivtest bis Tag 10, Analyse am Abend von Tag 10.
7. **Echtgeld-Stufen:** 10 Trades à 37,50 €, danach 75 € bis Trade 40; Fill-Protokoll je Trade (Slippage SL/TP1, Minuten ohne aktiven SL beim Teilverkauf).
8. **Echtgeld-Abbruch:** kumuliert -8 R oder -400 € (was zuerst kommt) · Ø ≤ -0,3 R nach 15 Trades · ≥ 2 Exits ausserhalb Regel 7 in 10 Trades · Spread Ø > 10 %.
9. **Echtgeld-Auswertungen:** nach Trade 20 ("kein Schaden": Ø ≥ 0 nach Kosten, kein Abbruch) — erste und einzige erlaubte Regelaenderung danach; nach Trade 40 Kante vorlaeufig belegt bei Ø ≥ +0,10 R UND Ø ohne besten Tag ≥ 0.
10. **Regel-Freeze:** ab Fiktivtag 1 bis Echtgeld-Trade 20; Aenderungsideen nur ins datierte Backlog.

Warum diese Variante und nicht meine (3 Tage + Echtgeld) oder Levis reine 10 Tage: 3 Tage sehen evtl. keinen Klebetag — gerade dort ist der neue Cross-Trigger am riskantesten; reine 10 Tage ohne Uebergang wiederholen das Muster "noch ein Testblock", obwohl die entscheidenden Groessen (Spread, Fill, Exit-Disziplin) fiktiv gar nicht messbar sind. Die gewaehlte Variante erfuellt Levis Wunsch (erst fiktiv testen, dann analysieren) und haelt den Weg ins Echtgeld kurz.

---

## Ehrliche Gesamtwertung

Fable hat meinen Bericht an der richtigen Stelle getroffen: Ich habe eine Zahl gerechnet, die mein eigenes Regelwerk nicht handeln koennte, und sie zu prominent gestellt. Nachgerechnet mit ausfuehrbaren Entries bleibt fuer das gelockerte Werk eine Kante von etwa null bis +0,1 R vor Kosten — mit einem KO-Schein-Spread von ~9 Punkten eher darunter. Beim 08.10. lag ich falsch, beim "die Filter kosteten Geld" habe ich die Daten im Licht des Auftrags gelesen; beides ist korrigiert.

Was davon unberuehrt bleibt: Das alte Werk handelt nicht, und ein System, das nicht handelt, liefert weder Geld noch Erkenntnis. Der SL-Cap ist die eine Regel, die die Daten wirklich stuetzen. Die Lockerung ist deshalb richtig — aber als bewusste Entscheidung, um mit gedeckeltem Risiko echte Zahlen zu bekommen, nicht als Folge einer belegten Kante. Levi, wenn du nach 40 echten Trades bei Ø ≈ 0 stehst, ist das kein Versagen der Lockerung, sondern das ehrliche Ergebnis, das diese Daten heute schon andeuten — dann sind Produktkosten und Einstiegsqualitaet die naechste Baustelle, nicht neue Filter. Die wichtigste Regel des ganzen Plans ist nicht der Trigger, sondern der Regel-Freeze bis Trade 20 und die Hand weg vom Exit.
