---
name: fable_stellungnahme_regel_lockerung_2026-10-14
description: "Fable-Stellungnahme 14.10.2026 (zweiter Gutachter) zu Opus' Regel-Lockerungsvorschlag: Zaehlung 56/31/23/2 bestaetigt; Erwartungswert-Formel korrekt; Gegenrechnung REPRODUZIERT an 8 von 9 Tagen, aber pfadabhaengig (08.10. Re-Entry 18:16 = 3 R Schwankung) und 3 der 4 TP2-Treffer sind 15:32/15:36-Entries, die Opus' eigene Regel 6 verbietet -> realistische Kante +0,1-0,2 R vor KO-Spread, nach Spread ~0; Behauptung 'Filter sortierten nichts Schlechteres aus' nicht belastbar; 10-Regel-Werk ok mit Korrekturen (8c2 behalten, Trigger praezisieren, Re-Entry-Pause, Spread-Hard-Gate); Testplan: Levis 10 Fiktivtage als Prozesstest mit binaeren Kriterien, Uebergang ab Tag 5 moeglich; E1-E12 bewertet, 7 neue Entscheide"
metadata:
  type: project
  originSessionId: fable-regel-lockerung-gegencheck-2026-10-14
  modified: 2026-10-14T10:23:35.028Z
---

# Fable-Stellungnahme zur Regel-Lockerung â€” 14.10.2026

Auftrag: unabhaengiger zweiter Gutachter zu [[opus_vorschlag_regel_lockerung_2026-10-14]] (Opus, 11:49-12:02). Opus' Bericht ist ein Gutachten, kein Beschluss â€” ich habe seine tragenden Zahlen gegen die Rohlogs nachgerechnet, nicht uebernommen.

**Zeitstempel:** Start Mi 14.10.2026 12:07:21 DE, Ende 12:23:45 DE (sha1-Endkontrolle 12:18:18). sha1 aller 86 Logs/Bars/DB/Register in `scripts/` vorher (12:07:58) = nachher (12:18:18) **IDENTISCH** (gate_check_log BC95BDC8â€¦, kombi 5DF86668â€¦, momente 5373D665â€¦, skipped 97E11CC2â€¦, trades.db 9990BA61â€¦). Alle Skriptlaeufe nur auf der Kopie `%TEMP%\...\scratchpad\fable_1014\scripts`, eigene Lese-Skripte in `...\fable_1014\ana` (`count.cjs`, `sim_fable.cjs`). Kein git add/commit/push, kein test:e2e, kein node --test, kein CDP, kein h1-end-Lauf, keine Aenderung an Skripten/Prompts/Regeln/Memories; MEMORY.md nicht angefasst. Einziger Schreibvorgang: diese Datei.

---

## Kurzfazit (10 Zeilen)

1. **Wo ich Opus recht gebe:** Der Befund stimmt bis auf die Nachkommastelle (56 Gate-Laeufe, 31 FAIL = 22 RR / 6 TP / 3 beide, 23 Q-ROT/UNBEKANNT-Veto, 2 Einstiege, 478/383/147 Momente). Die Erwartungswert-Formel ist richtig, der SL-Cap 3x ATR ist der einzige Filter, der in den Daten klar trennt, und feste Ausgaenge statt Stall-/Punkt-11-Exits sind die richtige Antwort auf Payoff 0,94:1.
2. **Wo ich widerspreche (Zahlen):** Opus' Hauptwert **+3,71 R / Ã˜ +0,23** ist nicht falsch, aber **pfadabhaengig**: ich reproduziere 8 von 9 Tageswerten exakt; der 08.10. kippt von -2,00 (Opus) auf +1,10 (ich), weil Opus 18:16 â€” dieselbe Idee wie 17:56, gleicher Anker, gleiches TP1, in derselben Kerze, in der der 17:56-SL fiel â€” als neuen Trade nimmt und damit den Tagesstopp vor den Gewinnern 18:41/19:01 ausloest. Eine Konvention bewegt die Summe um **3 R**.
3. **Wichtiger Widerspruch:** **3 der 4 TP2-Treffer** der Gegenrechnung sind Entries um **15:32, 15:32 und 15:36** â€” mitten in der ersten US-Kerze. Opus' eigene Regel 6 ("Entries 15:35-19:30") und Regel 3 ("geschlossene 5m-Kerze â€¦ Entry zur naechsten Eroeffnung") erlauben genau diese Entries nicht. Unter Opus' eigenem Regelwerk nachgerechnet (Entry naechste Eroeffnung, SL bleibt am Anker, Cap neu geprueft): **n 12-13, Î£ +2,0 bis +2,5 R, Ã˜ +0,17 bis +0,20, ohne besten Tag +0,10 bis +0,14, p 33-38 %, q 20-25 %.** Der 05.10. (bei Opus 70 % der Summe) schrumpft von +2,59 auf +0,96, weil der 15:32-Trade zur 15:35-Eroeffnung einen SL von 7,2x ATR haette.
4. **Nach KO-Spread ist das null:** Mit 9 Pkt Spread (Opus' eigene Schaetzung fuer 1 Cent beim 5,5-EUR-Schein, Hebel 6) faellt schon die optimistische Variante auf Ã˜ +0,15; die realistische liegt dann bei ~0. Die Kante, die Opus zeigt, ist in dieser Datenlage **nicht von null unterscheidbar** â€” das sagt Opus selbst in Zeile 10, aber die Ueberschrift "+0,23 R" steht fetter als dieser Satz.
5. **Behauptung 4 (gestrichene Filter sortierten nichts Schlechteres aus, n 27):** nicht belastbar. Gruppen von 11 und 16 Ideen mit Â±0,4 R Band koennen weder "schlechter" noch "nicht schlechter" zeigen; dazu Regimeabhaengigkeit (Q-ROT 07.10. -3,63 / 08.10. +1,84). Opus hat das am 08.10. selbst so geschrieben ("kein Beleg in irgendeine Richtung") und formuliert es am 14.10. mit denselben Daten als "kosteten eher Geld".
6. **Regelwerk:** Ich stimme der Streichliste weitgehend zu (Q-ROT-Veto, Level-Fenster, Stall-Exit, 13.1, Zeitbox, Stale, Halbierungen). **Behalten als harte Einzeiler:** 8c2 (SL-Cluster, direkt aus #42/#43), Spread-Sanity-Check (bestehende Regel 22.06., fehlt in Opus' Inventar), Sperrfristen. **Neu noetig:** Re-Entry-Pause nach SL (der 18:16-Fall), BE-Stop = Entry +/- Spread, und ein Trigger-Satz, der "Cross" von "Zustand" trennt â€” so wie er steht, erzeugt er an Rangetagen (13.10.: 8 EMA-Crosses) bis zu 8 Trigger und damit neue Beliebigkeit.
7. **Levis Kernsatz "1:1 / 1:2 bei WR 50 %":** Opus hat recht, dass das rechnerisch nur traegt, wenn â‰¥ 50-60 % der TP1-Gewinner auch TP2 erreichen. Historisch 27 %, in meiner realistischen Simulation 20-25 %. Die Zielgroesse ist in Ordnung, der Beweis muss ueber p, q, c getrennt laufen â€” nicht ueber "WR".
8. **Testplan:** Levis 10 Fiktivtage sind nicht schlechter als Opus' 3 Tage + Echtgeld â€” sie sind nur **etwas anderes**: ein Prozesstest (Technik, Trigger-Frequenz je Tagestyp, Disziplin der festen Ausgaenge im Loop), **kein Kantentest** (bei ~1,5 Trades/Tag sind es 12-17 Trades, Standardfehler Â±0,22 R, Nachweis einer Kante von +0,2 R braeuchte ~50 Trades). Spread/Fill und Levis eigene Hand am Exit testet nur Echtgeld.
9. **Empfehlung Testplan:** Levis Variante, aber mit vorab registrierten **binaeren Kriterien**, Zaehlregeln, Schatten "alt haette", taeglicher Bid/Ask-Spread-Messung des realen Scheins, **Verbot von Zwischenanpassungen** und einem **vorgezogenen Uebergang ab Tag 5**, wenn alle Technik-Kriterien erfuellt sind (10 Tage = Maximum, nicht Pflicht). Danach Echtgeld mit halbem Risiko wie Opus.
10. **Gesamtwertung:** Opus' Diagnose und Richtung sind **gut**; die Zahl +0,23 R ist **zu optimistisch dargestellt** (Re-Entry-Pfad, nicht ausfuehrbare Open-Entries, Spread); das **Risiko** liegt nicht in der Lockerung, sondern darin, dass sie als Beweis einer Kante gelesen wird und nach den ersten 5 Verlusten die naechste Filterspirale beginnt.

---

## A. Unabhaengige Stichproben-Pruefung

### A.1 Zaehlung (verifiziert, deckungsgleich mit Opus)

Eigenes Skript `ana/count.cjs` auf der Kopie, Freeze-Tage 25.09.-13.10. (10 Tage, 06.10. ausgefallen):

| Groesse | Opus | Fable (Rohlog) | Quelle |
|---|---|---|---|
| Live-Gate-Zeilen Freeze | â€“ | 62 (modus `live`), davon 6 Abbrueche (Exit 1) | gate_check_log.jsonl |
| Gate-Laeufe | 56 | **56** | = 62 - 6 |
| FAIL | 31 | **31** = RR allein 22 / TP allein 6 / beide 3 | FAIL-Zeilen im `output` |
| PASS | 25 | **25** | exit 0 |
| PASS mit Q-ROT/UNBEKANNT (Veto) | 23 | **23** (22 ROT + 1 UNBEKANNT) | skipped_setups_fiktiv (52 Freeze-Zeilen: 20 rrGate, 4 tpRealism, 3 beide, 22 Q-ROT, 1 UNBEKANNT, 2 EINSTIEG) |
| zulaessige Einstiege | 2 | **2**: 28.09. 19:32 GELB (OFFEN +0,05) / 13.10. 19:36 GRUEN (OFFEN -0,12) = -0,07 R | skipped + kombi_fiktiv_log (25 Freeze-Eintraege) |
| Momente VC / qualifiziert / ALT > 3x ATR | 478 / 383 / 147 | **478 / 383 / 147** | momente_log.jsonl |

Je-Tag-Werte (Gate-Laeufe 8/3/4/4/7/5/12/12/7, FAIL/PASS je Tag) stimmen ebenfalls. **Verifiziert.** Hinweis: Die 56 Laeufe sind nur ~31 Ideen (Datum+Richtung+Anker) bzw. 43 (Opus' Fingerabdruck mit TP1); die Kill-Statistik zaehlt Laeufe, nicht Ideen â€” das ueberzeichnet die Wirkung der Filter um den Faktor 1,3-1,8 (Wiederholungslaeufe desselben Setups).

### A.2 Erwartungswert-Formel (nachgerechnet, korrekt)

Levis Schema: 50 % bei TP1 = +0,5 R realisiert; Rest: mit Wahrscheinlichkeit q TP2 (+0,5 x 2 = +1,0 R), sonst BE-Stop (0 R). Verlust vor TP1: -1 R. Also

EV = p Â· [q Â· 1,5 + (1-q) Â· 0,5] - (1-p) Â· 1 - c = **p(0,5 + q) - (1-p) - c**. Bei p = 0,5: **0,5q - 0,25 - c**. Break-even q = 0,5 + 2c; Break-even p = (1+c)/(1,5+q) (q 0,3 â†’ 55,6 %). **Opus' Formel und Zahlen stimmen.**

Randfaelle, die Opus nicht ausschreibt:
- **-0,25 ist korrekt**, aber nur wenn der BE-Stop wirklich bei 0 ausloest. Beim Scalable-Schein liegt der Rueckkauf am Ask, der SL-Trigger ebenfalls â†’ BE-Stop realisiert **-c/2** (halbe Position mal Spread). Mit 9 Pkt Spread auf R 60 Pkt sind das -0,075 R je BE-Fall. **Folge:** BE-Stop auf Entry +/- Spread legen, sonst wird "BE" systematisch zum Kleinverlust (Historie #11: BE mit Slippage -0,27 %).
- **Trailing statt BE:** EV = p Â· [0,5 + E(Rest)] - (1-p) - c, E(Rest) zwischen 0 und 1 R. Trailing ist nur besser als BE/2R, wenn E(Rest) > q Â· 1,0 â€” bei q 0,25 also wenn das Trailing im Schnitt > 0,25 R aus dem Rest holt. Historisch (#37 +0,15 R Stall, #39 +0,49 R TP1) hat genau das NICHT funktioniert. Festes 2R ist deshalb richtig, nicht weil es optimal ist, sondern weil es unmanipulierbar ist.
- **Zeit-Exit 20:00 als dritte Ausgangsklasse:** In der Simulation enden 5-6 von 13-17 Positionen OFFEN mit Buchgewinn. Die positive Summe kommt **ueberwiegend aus diesen Zeit-Exits**, nicht aus TP-Treffern â€” nach reiner Formel mit p 0,38 / q 0,2 waere der Erwartungswert -0,35 R. Opus sagt das in Abschnitt 3 richtig; es gehoert aber in die Kernaussage: die Kante, falls vorhanden, ist "Trend bis zum Schluss", nicht "1R/2R".
- **"WR 50 %" ist nicht definiert:** Zaehlt ein TP1+BE-Trade (+0,5 R) als Gewinn? Dann ist WR = p. Zaehlt nur der Netto-Gewinn, ebenfalls p. Zaehlt "nicht im Minus geschlossen", kommen BE-Stopps mit Spread-Verlust als Verluste dazu. Vorab festlegen: **WR := p = Anteil Trades mit TP1-Treffer vor SL.**

### A.3 Gegenrechnung n 16 (eigene Simulation `ana/sim_fable.cjs`, Bars `nas100_5m_<datum>.json`)

Methode wie Opus (Entry = Gate-Kurs, SL = ALT-Anker âˆ“ 0,5 ATR, Floor 1,5 / Cap 3,0 ATR, TP1 = 1R, TP2 = 2R, Schema C, SL vor TP in derselben Kerze, Short-Exits am Ask, 1 Position, max 3/Tag, Stopp -2R, Horizont 20:00, OFFEN = letzte Close). Zwei Dedup-Varianten: roh-sequenziell (52 Zeilen, wie Opus) und dedupliziert (Datum+Richtung+Anker, 31 Ideen).

| Variante | n | Î£ R | Ã˜ R (95 %) | ohne besten Tag | p / q | Exits |
|---|---|---|---|---|---|---|
| **Opus Hauptwert** (Cap 3, Spread 2, roh) | 16 | +3,71 | +0,23 (Â±0,45) | +0,08 | 44 % / 43 % | 5 OFFEN |
| Fable roh-sequenziell, Spread 2 | 17 | **+6,66** | +0,39 (Â±0,42) | +0,27 | 53 % / 44 % | OFFEN 5, TP2 4, BE 2, SL 3, TP1-OFFEN 3 |
| Fable dedupliziert, Spread 2 | 16 | +6,31 | +0,39 (Â±0,44) | +0,27 | 50 % / 50 % | OFFEN 5, TP2 4, SL 3, TP1-OFFEN 3, BE 1 |
| dedup, Spread 0 | 16 | +6,78 | +0,42 | | | |
| dedup, Spread 5 | 17 | +3,45 | +0,20 | | | |
| dedup, **Spread 9** (KO-Schaetzung) | 17 | +2,46 | **+0,15** | | | |
| dedup, **ohne Cap** | 15 | **-0,37** | -0,03 (Â±0,52) | -0,23 | | SL 6 |
| dedup, Gate-Kerze intrabar (SL zuerst) | 16 | +5,31 | +0,33 | +0,19 | | |
| dedup, **Entries ab 15:35** (Opus Regel 6) | 14 | +3,38 | +0,24 | +0,18 | 43 % / 33 % | |
| dedup, **Entry naechste Eroeffnung, R neu** (Opus Regel 3) | 13 | **+2,53** | **+0,20 (Â±0,39)** | **+0,14** | 38 % / 20 % | OFFEN 6, BE 2, TP1-OFFEN 2, SL 2, TP2 1 |
| dedup, **ab 15:35 + naechste Eroeffnung** (= Regel 3 + 6) | 12 | **+2,05** | **+0,17 (Â±0,42)** | **+0,10** | 33 % / 25 % | |
| roh, ab 15:35 | 15 | +3,73 | +0,25 | +0,20 | 47 % / 29 % | |
| dedup, Entries ab 16:00 | 12 | +2,93 | +0,24 | +0,16 | 42 % / 20 % | |

**Tageswerte (roh-sequenziell, Spread 2) gegen Opus:** 28.09. -0,11 (Opus -0,09) Â· 29.09. +0,42 (+0,44) Â· 30.09. +2,03 (+2,03) Â· 01.10. -0,53 (-0,50) Â· 02.10. +1,20 (+1,20) Â· 05.10. +2,59 (+2,59) Â· 07.10. +0,08 (+0,14) Â· **08.10. +1,10 (Opus -2,00)** Â· 13.10. -0,12 (-0,10). **8 von 9 Tagen reproduziert.**

**Der 08.10.-Unterschied (3,1 R):** Zeilen 17:56, 18:01 und 18:16 haben denselben Anker 31003,35 und dasselbe TP1 30877 â€” nach Opus' eigenem Fingerabdruck **eine** Idee. Der 17:56-Short faellt in der 18:15-Kerze (Hoch 31061,95) auf den SL 31017,5. Opus laesst um 18:16 â€” in **derselben** Kerze â€” denselben Short noch einmal eroeffnen (Entry 30950,7, SL 31017,1), der dann ebenfalls intrabar faellt â†’ zwei Verluste â†’ Tagesstopp â†’ 18:41 (+1,46, TP2 in 2 Kerzen) und 19:01 (+0,67) werden nie gehandelt. Bei mir ist die Position bis Kerzenende 18:20 belegt, 18:16 wird uebersprungen. Mit 1m-Bars waere es entscheidbar, mit 5m-Bars nicht. **Beide Lesarten sind vertretbar; Opus' ist die konservativere.** Aber: Dass eine Re-Entry-Konvention an einem Tag Â±3 R von einer Gesamtsumme von 4-7 R ausmacht, zeigt, wie duenn die Datenbasis ist â€” und es liefert direkt eine Regel: **nach einem SL keine Wiedereroeffnung derselben Richtung mit demselben Anker innerhalb von 15 Min** (siehe D, N2).

**Stichproben bar-fuer-bar (verifiziert):**
- **05.10. 15:32 Long** (Entry 30797,35, Anker 30752,8, ATR 22,7 â†’ R 55,9, SL 30741,45, TP1 30853,25, TP2 30909,15): Gate-Kerze 15:30 O 30805,2 H 30908,05 L 30794,35 C 30905,15 â€” SL nicht beruehrt, **TP1 schon in der Gate-Kerze**, TP2 um 1,1 Pkt verfehlt; 15:35-Kerze H 30923,65 â†’ TP2. **+1,46 R stimmt.** Aber: Der Entry liegt 2 Minuten in der ersten US-Kerze, 8 Pkt ueber deren Tief, in einer Kerze mit 114 Pkt Range. Unter Regel 3 (Schluss der Trigger-Kerze / naechste Eroeffnung) waere der Entry 30905,15 â†’ SL-Distanz 152+11 = 164 Pkt = **7,2x ATR â†’ kein Trade**. Unter Regel 6 (ab 15:35) ebenfalls kein Trade. **Der groesste Einzelgewinn der Gegenrechnung ist mit dem vorgeschlagenen Regelwerk nicht ausfuehrbar.**
- **30.09. 15:32 Long** (Entry 30450,25, R 70,2, TP1 30520,45, TP2 30590,65): 15:30-Kerze L 30410,25 (MAE 40 Pkt, SL 30380 haelt), 15:55 H 30569,65 â†’ TP1, 16:00 H 30600,65 â†’ TP2. **+1,47 stimmt.** Zur 15:35-Eroeffnung (30493,75) wird R 113,7 (2,93x ATR, knapp unter Cap), TP1 erst 19:25 als **TP1-BE +0,48** statt +1,47.
- **02.10. 15:36 Long** (+1,47): zur naechsten Eroeffnung TP1-BE +0,48.
- **08.10. 18:41 Short** (Entry 30995,7, R 51,2): 18:45 L 30829,15 â†’ TP1 **und** TP2 in derselben Kerze (MFE 4,67 R) â€” **+1,46 stimmt**, robust auch zur naechsten Eroeffnung (+1,48). Echter Wasserfall-Trade.
- **08.10. 18:16 Short** (Opus' Intrabar-Fall): 18:15-Kerze O 30951,15 H 31061,95 â€” Entry 30950,7 nahe Open, SL 31017,1 innerhalb der Kerze ueberschritten â†’ SL -1 ist plausibel (nicht beweisbar ohne 1m). Zur 18:20-Eroeffnung (30982,65) haette derselbe Trade den SL um **0,25 Pkt** (18:25-Hoch 31016,85) verfehlt und waere in den Wasserfall gelaufen. Messers Schneide â€” genau die Spread-Empfindlichkeit, die Opus am 13.10. selbst dokumentiert hat.

**Einstiegs-Kerzen-Intrabar-Risiko:** Mit "Gate-Kerze mitpruefen, SL zuerst" faellt Ã˜ von +0,39 auf +0,33 (ein TP2 wird BE). Das ist klein â€” das eigentliche Problem der Gate-Kerze ist nicht der SL, sondern dass der **Entry-Kurs im Log ein Momentkurs aus der laufenden Kerze** ist, den kein Kerzenschluss-Trigger liefert.

**Fazit A.3:** Opus' Methode ist sauber und ich reproduziere sie. Opus' Zahl ist am 08.10. konservativer als meine, insgesamt aber **optimistisch**, weil (1) drei Open-Entries mitlaufen, die das eigene Regelwerk ausschliesst, (2) der Entry-Kurs nicht der ausfuehrbare Kurs ist, (3) der Spread mit 2 statt ~9 Pkt angesetzt ist. **Realistischer Korridor fuer Opus' eigenes 10-Regel-Werk: Ã˜ +0,1 bis +0,2 R vor Spread, ~0 nach KO-Spread, 95 %-Band Â±0,4.** Was robust bleibt: **ohne Cap wird es negativ** (Ã˜ -0,03, ohne besten Tag -0,23) â€” E4 ist die am besten belegte Einzelentscheidung des Berichts.

### A.4 Behauptung "gestrichene Filter sortierten keine schlechteren Ideen aus" (n 27)

Nicht belastbar, aus vier Gruenden:
1. **Stichprobe:** Gruppen 11 (FAIL â‰¤ 3 ATR) und 16 (Q-ROT â‰¤ 3 ATR) mit SD ~0,9 R haben Standardfehler Â±0,27 bzw. Â±0,22 R; ein Unterschied von 0,1-0,2 R zur "erlaubten" Gruppe (n 2!) ist nicht messbar. Die Aussage ist weder beweisbar noch widerlegbar.
2. **Regime:** Q-ROT-Ideen lieferten 07.10. -3,63 und 08.10. +1,84 (Opus 08.10., dedupliziert). In meiner Simulation tragen die 8 gehandelten Q-ROT-Ideen +5,35 von +6,31 R â€” fast die ganze Summe, davon 4 Trades an zwei Trendtagen. "Q-ROT kostete Geld" heisst in Wahrheit "Q-ROT kostete an zwei Trendtagen Geld und sparte an einem Klebetag".
3. **Selektion:** Ideen existieren nur dort, wo Sonnet ein Gate fuhr â€” nach den alten Regeln. Welche zusaetzlichen Ideen das gelockerte Werk an Rangetagen erzeugt (13.10.: 8 EMA-Crosses), ist in den Daten nicht enthalten. Opus' Momente-Simulation (2.4) zeigt: "jede 2/2-Lage handeln" â‰ˆ 0 bis negativ. Die Wahrheit des neuen Triggers liegt irgendwo zwischen beiden.
4. **Konsistenz:** Opus am 08.10. ueber dieselben Daten: "kein Beleg, dass das Veto Geld kostet, und keiner, dass es Geld spart"; am 13.10.: "Q-ROT bleibt Veto â€¦ Lockerung nur als neue, vorab registrierte Hypothese fiktiv"; am 14.10.: "Die zwei Hauptkiller kosteten eher Geld als sie sparten". Die Daten haben sich zwischen 13.10. und 14.10. nicht geaendert, der Auftrag schon. Das ist kein Vorwurf an Opus â€” es zeigt, wie stark die Fragestellung die Lesart derselben Â±0,4-R-Zahl bestimmt. **Levi sollte wissen, dass die Lockerung eine Entscheidung ist, keine Folgerung aus den Daten.**

**Was ich verifiziert habe:** Zaehlung komplett; Formel; 9 Tageswerte der Simulation (8 exakt, 1 erklaert); 5 Trades bar-fuer-bar; Spread-Sensitivitaet; Cap-Wirkung. **Was ich nicht verifiziert habe:** Opus' Stichwortzaehlung der VC-Fazits (112/140/180/207 Nennungen â€” grob, von Opus selbst so markiert), die Vor-Freeze-Zahlen (11.-21.09.), die Momente-Simulation 2.4 (nur plausibilisiert), die Trend/Range-Klassifikation der Tage.

---

## B. Fachliche Meinung zum Regelwerk

### B.1 Streichliste â€” Zustimmung

- **Q-ROT-Veto (E5):** streichen, als Schatten loggen. Nicht weil es Geld kostete (unbewiesen), sondern weil es nie kalibriert war (Studie 24.08., Q2 am 11.09. ab Sekunde 1 unerfuellbar, Scharfschaltung ohne die vorgesehene 15-Trade-Auswertung) und 23 von 25 PASS verhinderte. Ein Filter, der 92 % der Freigaben killt, ist kein Filter mehr, sondern das Regelwerk.
- **Level-TP1-Fenster / TP-Realismus 8b1 / TP2-Realismus (E2):** ersetzen durch TP1 = 1R, TP2 = 2R, Cap 3 ATR. Die Geometrie "SL weit + kein Level im Fenster" ist dieselbe wie der Cap â€” Opus hat das richtig erkannt.
- **Stall-Exit 12/12.1a/12.3 + Punkt-11-Exits:** streichen. Eigene Review 12.08.: 7 Faelle, **1 sauberer**, Regel bis 31.07. durchgehend nachjustiert; die Gewinne, die der Stall-Exit "sicherte" (#23 +0,81 %, #28 +0,55 %, #37 +0,15 R), sind exakt die zu kleinen Gewinne aus [[feedback_realisiertes_rr]]. Der Stall-Exit war die Regel-Form des "wir cutten uns selber".
- **13.1 / Chasing-Pflicht / Punkt 11 Serien-Lesart:** streichen. 0 echte Konflikte in der gesamten Serie (rk13_1_filter), aber Pflichtfelder, Zaehler k/2, Praezedenzregeln, Kollisionsmarker â€” Prozesslast ohne eine einzige Entscheidung.
- **Retest-Zeitbox / AUSSICHTSLOS / Wiederholungs-Fingerabdruck / Stale:** streichen; ersetzen durch "ein Entry je Trigger" plus Re-Entry-Pause (N2).
- **Halbierung 15:30-16:00 und Zone-2-Halbierung:** streichen (Review 12.08.: n 3, nicht gedeckt). Aber ehrlich: Mit Regel 6 "ab 15:35" verschwinden drei der vier TP2-Treffer der Gegenrechnung. Das ist richtig so (sie waren nicht ausfuehrbar) â€” nur darf niemand spaeter sagen "die Simulation hat +0,4 R versprochen".

### B.2 Streichliste â€” Widerspruch: das wuerde ich als Schutz behalten

1. **8c2 SL-Cluster â€” behalten als harten Einzeiler, nicht nur Schatten.** Herkunft #42/#43 (21.08., Opex/PMI): zwei Wick-Stopps in derselben Zone 29.240-254, 7 Punkte auseinander, -83,53 EUR am letzten echten Handelstag. Das ist der juengste reale Verlustmechanismus. Opus streicht ihn als "Pflichtfeld mit Hard-Exits" â€” das Problem war das Pflichtfeld `--cluster-level`, nicht die Regel. Einzeiler: "Fiel heute ein SL per Wick, liegt kein neuer SL derselben Richtung innerhalb 0,5x ATR dieser Zone." Kosten: eine Zeile im kern_gate.
2. **Spread-Sanity-Check (project_risikomanagement, 22.06.2026: Spread max. 10-15 % des SL-Risikos, sonst auslassen) â€” fehlt in Opus' Inventar komplett.** Das ist genau die Regel, die E9 braucht, und sie existiert schon. Bei 9 Pkt Spread und SL 60 Pkt sind es 15 % â†’ Grenzfall; bei SL 40 Pkt (Floor 1,5x bei ATR 27) 22 % â†’ kein Trade. **Als hartes Gate in kern_gate: Spread (Pkt) / SL-Distanz (Pkt) > 0,15 â†’ FAIL.** Das zwingt automatisch zu weiteren SLs bei breitem Spread und ist die einzige Regel, die die Kostenfrage im Loop loest statt in der Nachanalyse.
3. **Sperrfristen + Schock-Tier-No-Trade (Regel 8):** behalten â€” Opus behaelt sie; ich unterstreiche: 16.09. FOMC 2x -1 R, 21.08. Opex/PMI. Die Liste muss im Start-Update stehen (CPI heute 14.10. 14:30 ist das Beispiel) und als `--blackout`-Pflicht im kern_gate bleiben, kein Freitext.
4. **Whipsaw-Schutz (EMA + VWAP, feedback_chartanalyse):** Opus reduziert auf EMA50-Dual-Gate ohne VWAP/RVOL. Einverstanden als Richtungsfilter; VWAP war ohnehin "unterstuetzend, kein Blocker". Was aber fehlt, ist der Whipsaw-Schutz im **Trigger**: 8a2 (Zwei-Kerzen-Bestaetigung) wird "zusammengefasst" â€” siehe B.4, der Trigger-Satz braucht genau diese Praezisierung, sonst ist der 13.10. mit 8 Crosses der Normalfall.
5. **Re-Entry-Pause nach SL (neu):** keine Wiedereroeffnung in derselben Richtung mit demselben Anker innerhalb von 15 Min (3 Kerzen) nach einem SL. Herkunft: 08.10. 18:16 (A.3) und #42/#43. Das ist die Zeitbox in einer Zeile.

### B.3 Behaltene Regeln â€” zu streng oder zu unklar

- **Regel 7 "Stopp nach 2 Verlusten ODER -2R":** Mit Schema C ist ein TP1-BE-Trade +0,5 R; zwei SL = -2 R = Stopp. Klar. Aber: Zaehlt ein BE-Stop mit Spread-Verlust (-0,04 R) als "Verlust" fuer den Cooldown? Festlegen: Verlust := Exit â‰¤ -0,5 R.
- **Regel 2 "1H nicht dagegen":** E2 hat am 13.10. zweimal richtig blockiert â€” behalten. Unklar bleibt die 1H-EMA-Berechnung (Opus' eigene Rechnung lag 6-10 Pkt neben dem VC-Wert, Seed-Effekt). Fuer kern_check: EMA-Quelle = Chart-Wert (`data_get_study_values`), nicht eigene Berechnung aus 300 Bars.
- **Regel 4 SL "letzter bestaetigter 5m-Swing":** "bestaetigt" ist nicht definiert (zwei niedrigere Hochs rechts? ein Schluss darunter?). Der 13.10. zeigte den Gegenfall: nach dem Impuls-Extrem entstand kein LH â†’ Anker blieb 150 Pkt weg â†’ alles tot. Definition noetig: "Swing = Hoch/Tief mit je einer Kerze links und rechts, die nicht hoeher/tiefer schliesst; liegt der letzte Swing > 3 ATR weg, gilt Floor-Pruefung: SL = 1,5x ATR nur, wenn der Entry innerhalb 0,5 ATR eines Tageslevels liegt, sonst kein Trade." Das ist eine Entscheidung fuer Levi, keine Rechenfrage â€” ohne sie wird kern_gate an Trendtagen (Wasserfall ohne Swing) staendig FAIL liefern, wie heute.
- **Regel 6 Zeitfenster 15:35-19:30:** richtig. Hinweis: Entries 19:00-19:30 haben nur 30-60 Min bis zum Zeit-Exit â€” bei R 60-100 Pkt ist TP2 dort praktisch unerreichbar. Entweder Entries bis 19:00 (Opus Alternative B) oder akzeptieren, dass spaete Trades fast immer Zeit-Exits sind. Ich wuerde **bis 19:00** nehmen.

### B.4 Der Trigger-Satz â€” nicht praezise genug

"Eine geschlossene 5m-Kerze schliesst in Gate-Richtung jenseits der 5m-EMA50 oder jenseits eines Tageslevels (PDH/PDL, Pivot, Session-Hoch/-Tief)." Vier Luecken:

1. **Zustand vs. Ereignis:** "jenseits" ist ein Zustand. Im 2/2-Trend ist der 5m-Schluss praktisch immer jenseits der EMA50 â†’ jede Kerze ist ein Trigger â†’ Entry sofort bei 2/2-Bildung (genau die "Dual-Gate wird 2/2"-Auslegung, die Opus am 13.10. als undefiniert markiert hat). Noetig: **Cross** = Vorkerze schloss auf der anderen Seite, diese Kerze schliesst jenseits. Und: Was gilt, wenn bei 2/2-Bildung der Kurs schon jenseits ist? Entweder (a) Entry zum Schluss der 2/2-Kerze (dann ist das der Trigger, ehrlich benannt) oder (b) Warten auf Retest + erneuten Cross. (a) ist das, was die Simulation gemessen hat.
2. **Welche Level:** "Pivot" (PP/R1/S1?), "Session-Hoch" (Overnight oder US â€” der B5-Streit vom 13.10.), Rundzahlen (5/7 Gates am 13.10. nutzten sie, Opus: "generierte Kategorie"). Die Liste muss im Start-Update **eingefroren** werden (level_register), im Loop nichts dazu.
3. **Frequenz:** 13.10. hatte 8 EMA-Crosses nach 15:30. Mit "ein Entry je Cross", max 3/Tag und 2-Verlust-Stopp ist ein Rangetag in 60-90 Min bei -2 R. Das ist akzeptabel **wenn** es als bekannter Preis des Systems registriert wird (Opus' "Rangetage Ã˜ +0,07" misst das nicht, weil Sonnet dort kaum Gates fuhr). Die Momente-Simulation (2.4) sagt fuer "jede Lage handeln" â‰ˆ 0 bis negativ.
4. **Entry-Preis:** "zum Schluss dieser Kerze bzw. zur naechsten Eroeffnung" â€” das sind zwei verschiedene Preise (05.10. 15:30-Kerze: 30905 vs. 30905; 08.10. 18:15-Kerze: 30982 vs 30982 â€” gleich; aber bei Gaps/Spikes nicht). Fuer die Nachrechnung muss **einer** gelten: naechste Eroeffnung (Bid fuer Short, Ask fuer Long).

Ohne diese vier Festlegungen schafft der Satz neue Beliebigkeit â€” kleiner als heute (43 Argumente), aber an der entscheidenden Stelle (wann genau Entry).

### B.5 SL-Cap 3 ATR / Floor 1,5 ATR und Positionsgroesse

ATR(5m) lag an den Testtagen bei 20-62 Pkt â†’ SL 30-186 Pkt. Phase 3: 5.000 EUR Position, Hebel 6, NAS ~30.500 â†’ **0,98 EUR je Punkt**; Risiko 75 EUR bindet ab **SL > 76 Pkt** (darunter bindet das Positionslimit, Risiko 30-75 EUR). Bei SL 150 Pkt (3x ATR 50) â†’ Position 2.550 EUR. Mit halbem Risiko (37,50 EUR) halbiert sich alles. **Plausibel und mit size.cjs direkt rechenbar.** Zwei Haken:
- **KO-Schein-Spread:** 1 Cent auf 5,5 EUR bei Hebel 6 â‰ˆ 9 NAS-Pkt (Opus' Rechnung stimmt). Bei SL 40-60 Pkt sind das 15-22 % â€” der bestehende Spread-Sanity-Check (B.2 Nr. 2) wuerde die engen Trades verbieten. Das ist richtig, aber es heisst auch: **der Floor 1,5x ATR ist bei ATR < 35 Pkt praktisch 2x**, weil der Spread die Mindestdistanz nach oben zieht. Produktwahl (CFD/Mini-Future mit 1-2 Pkt Spread) ist bei diesem System wichtiger als jede Regel.
- **SL/TP exklusiv bei Scalable:** TP1-Teilverkauf = SL stornieren, Haelfte verkaufen, SL auf BE neu setzen â€” drei Schritte, 1-2 Minuten ohne aktiven SL. Bei 08.10. 18:41 (TP1 und TP2 in **einer** Kerze) ist das manuell kaum sauber ausfuehrbar; bei 05.10. 15:32 (TP1 in 2 Min) ebenfalls nicht. Die Simulation unterstellt perfekte Teilverkaeufe am Punkt. Realistisch: TP1 per Alarm +2-5 Pkt Slippage und ein Zeitfenster ohne SL. Das gehoert in die Echtgeld-Messung (Fill-Protokoll je Trade).

### B.6 Was der Bericht uebersieht

- **Kosten als Hard-Gate** (bestehende Regel, siehe B.2).
- **Gap-/Overnight-Logik:** durch Zeit-Exit 20:00 gedeckt â€” aber nur, wenn der Zeit-Exit wirklich mechanisch ist. 5-6 von 13 Positionen enden als Zeit-Exit; das ist die haeufigste Ausgangsklasse und zugleich die, bei der Levi historisch am ehesten "noch ein bisschen" haelt. Regel: Zeit-Exit = Marktorder 19:58-20:00, kein Ermessen.
- **Event-Tage:** CPI/PPI/Fed-Redner sind gedeckt; **nicht gedeckt sind Tage mit ATR â‰¥ 2x Basis ohne Kalender-Event** (Schlagzeilen-Chop 20./21.08.). Opus' "Schock-Tier No-Trade" braucht eine messbare Definition im kern_check (ATR(5m) um 15:30 vs. 20-Tage-Median).
- **Slippage:** siehe B.5; kein Wort im Bericht ausser "Spread".
- **Shorts:** Payoff historisch 0,74:1, die Testserie war ueberwiegend short, die positiven Sim-Trades sind 50/50. Nur messen, kein Verbot â€” einverstanden.
- **Psychologie, Levis eigener Befund:** Fables Analyse 22.08. fand 50,5 % der Verlust-Euro in bekannten, nicht angewandten Regeln (Durchsetzungsproblem). Ein 10-Regel-Werk ist dafuer der beste Hebel im ganzen Bericht â€” **weniger Regeln = weniger Regelbrueche = messbarer Prozess**. Die Kehrseite: "wir cutten uns selber" meint zwei Dinge â€” die Filter (Levi) **und** die manuellen Fruehausstiege (#23, #25, #27, #28, #30, #32: alle vor TP2 von Hand). Die Lockerung loest das erste; das zweite loest nur Levi selbst, indem er im Echtgeld die Hand von Exits laesst. Dafuer gibt es keine Regel, nur die Messung "Anteil manueller Exits = 0".

---

## C. Levis Testplan (10 Tage fiktiv) vs. Opus (3 fiktiv + 10 Echtgeld halb)

### C.1 Was 10 Fiktivtage zeigen koennen â€” und was nicht

- **Erwartete Trade-Zahl:** Simulation 13-17 Trades in 9 Bar-Tagen â‰ˆ **1,3-1,9/Tag** (vorher 0,2). 10 Tage â†’ 12-19 Trades, davon an Rangetagen vermutlich mehr (neuer Trigger erzeugt dort Entries, die in den Daten fehlen).
- **Konfidenz:** SD je Trade 0,7-0,9 R â†’ Standardfehler bei n 15 â‰ˆ Â±0,22 R, 95 %-Band Â±0,45. Ein Mittelwert von +0,2 R ist von 0 **nicht** unterscheidbar; dafuer braeuchte es ~50 Trades (einseitig 95 %). Ein Mittelwert von -0,5 R (System kaputt) waere dagegen erkennbar.
- **Was fehlt:** Fill, Spread, Slippage, der dreistufige manuelle TP1-Prozess, Levis Hand am Exit, die 20:00-Marktorder unter Druck. Alles, was den Payoff historisch kaputt gemacht hat, ist im Fiktivtest **nicht im Testumfang**.
- **Was 10 Tage gut koennen:** Technik (0 Hard-Exits), Trigger-Frequenz je Tagestyp (Trend/Range), Schatten-Vergleich alt/neu, Anteil Zeit-Exits, p/q-Vorab-Schaetzung, und â€” mit minimalem Aufwand â€” **Spread-Messung** (Bid/Ask des realen Scheins bei jedem fiktiven Gate notieren, 10 Sekunden).

### C.2 Vergleich

| | Levi: 10 fiktiv, dann Analyse | Opus: 3 fiktiv + 10 Echtgeld halb |
|---|---|---|
| Kosten | 0 EUR, 2 Wochen | max. -8 R x 37,50 = -300 EUR, 1-2 Wochen |
| Testet Technik/Trigger | ja, inkl. Tagestyp-Mix | nur 3 Tage, Rangetag-Serie moeglicherweise nicht enthalten |
| Testet Spread/Fill/Disziplin | nein | ja, ab Trade 1 |
| Kantennachweis | nein (n ~15) | nein (n 10) |
| Risiko neuer Overfit-Freeze | **hoch** (Levi: "eingeengt") | niedrig |
| Risiko "Lockerung = Gewinn"-Illusion | mittel (fiktive +R ohne Kosten) | niedrig (echte Kosten sichtbar) |

**Ehrlich:** Levis Variante ist gegenueber Opus' **nicht besser fuer die Erkenntnis**, aber **besser fuer die Sicherheit** â€” und sie ist nach zehn Fiktivtagen ohne Trade psychologisch riskanter, weil sie das Muster "noch ein Testblock" wiederholt. Opus' 3 Tage sind zu wenig, um den neuen Trigger an einem Klebetag zu sehen (13.10.-Typ), und das ist das groesste Einzelrisiko des neuen Werks.

### C.3 Empfehlung: Levis 10 Tage als Maximum, Uebergang ab Tag 5

**Vorab registrieren (vor Tag 1, nicht veraenderbar):**
1. **Zweck:** Prozesstest. Ergebnis-R ist Messgroesse, **kein** Go/No-Go-Kriterium (ausser Abbruch unten).
2. **Binaere Technik-Kriterien (alle muessen an 5 aufeinanderfolgenden gueltigen Tagen stehen):** (a) 0 Hard-Exits durch Pflichtfelder; (b) jeder Trigger nach Definition hat einen kern_gate-Lauf, jeder Lauf einen Trigger (Abgleich aus Bars, nicht aus Chat); (c) jeder fiktive Trade hat einen Nachtrag mit 1m-Bars fuer die Entry-Kerze; (d) Zeit-Exit 20:00 in 100 % der offenen Positionen; (e) Spread-Protokoll (Bid/Ask Schein) bei 100 % der Gates.
3. **Zaehlregeln:** Tag gueltig ab 67 % Slot-Abdeckung (Praezedenz 08.10.); Ausfalltag zaehlt nicht und verlaengert nicht ueber 10 hinaus; Tagestyp (Trend/Range nach Effizienz 15:30-20:00) wird **vor** dem Ergebnis klassifiziert; mindestens 2 Rangetage muessen enthalten sein, sonst bis max. 10 Tage weiter.
4. **Schatten:** kern_gate bewertet jeden Trigger auch nach alter Logik ("alt haette PASS/FAIL/ROT") und loggt Q-Score/Chop/13.1 â€” ohne Wirkung. Zusaetzlich umgekehrt: alte Momente, die der neue Trigger **nicht** erzeugt (verpasste alte Gates).
5. **Geloggt je Trade:** Trigger-Typ (EMA-Cross/Level), Entry naechste Eroeffnung, Anker, R in Pkt und ATR-Vielfachen, Spread Pkt, Exit-Art (SL/TP1-BE/TP1-TP2/TP1-OFFEN/OFFEN/Zeit), p/q/c-Report taeglich.
6. **Abbruch (zurueck in die Analyse, keine Zwischenanpassung):** â‰¥ 3 Tage mit -2R-Stopp in 5 Tagen (Trigger zu lose an Rangetagen) ODER â‰¤ 3 Trades in 5 Tagen (Trigger zu eng) ODER gemessener Spread Ã˜ > 15 % der SL-Distanz (Produkt wechseln, nicht Regel).
7. **Uebergang:** Sind alle Technik-Kriterien an 5 gueltigen Tagen erfuellt und kein Abbruch gefeuert â†’ **Echtgeld mit halbem Risiko (37,50 EUR) fuer 10 Trades** (Opus Stufe 1), auch wenn das fiktive Ã˜ R negativ ist (das ist Rauschen). Sind sie nach 10 Tagen nicht erfuellt â†’ Technik-Fix, nicht Regel-Fix.
8. **Regel-Freeze:** Waehrend der Fiktivtage und der ersten 20 Echtgeld-Trades **keine** Regelaenderung ausser Fehlerkorrektur (Opus E11). Jede Aenderungsidee geht in ein Backlog mit Datum, nicht in den Loop. **Das ist die wichtigste Regel des ganzen Plans** â€” die letzten acht Wochen sind der Beleg, was ohne sie passiert.

Das kostet Levi gegenueber seinem Plan nichts (Maximum bleibt 10 Tage) und gegenueber Opus' Plan 2 Tage.

---

## D. Gesamtempfehlung â€” E1-E12 und neue Entscheide

1. **E1 Freeze beenden, lockern â€” zustimmen.** 2 Einstiege in 10 Tagen sind kein Handelssystem; Freeze-Definition ist nach 10-Tage-Regel formal erfuellt.
2. **E2 TP1 = 1R, TP2 = 2R â€” zustimmen.** Mechanisch, unmanipulierbar, macht RR-Gate/TP-Realismus/Register-Pflicht ueberfluessig.
3. **E3 Levis Schema (50 % @1R, BE, Rest 2R, Zeit-Exit) â€” zustimmen, mit Aenderung:** BE-Stop = Entry +/- Spread (nicht exakt Entry), und q â‰¥ 0,5 als registrierte Messgroesse, nicht als Hoffnung. 1R voll waere einfacher und in den Daten gleich gut; Levis Schema hat den Vorteil, dass Levi es selbst formuliert hat â€” Regeln, die man selbst gesetzt hat, haelt man eher.
4. **E4 SL-Cap 3,0x ATR â€” zustimmen, am staerksten belegt:** ohne Cap kippt jede Variante ins Minus (Ã˜ -0,03 / -0,23 ohne besten Tag).
5. **E5 Q-ROT-Veto streichen, Schatten â€” zustimmen,** aber mit der Klarstellung, dass das eine Entscheidung ist, kein Datenbefund (A.4).
6. **E6 Streichliste â€” aendern:** alles streichen **ausser 8c2**, das als Einzeiler hart bleibt (B.2); Zeitbox durch Re-Entry-Pause ersetzen (N2).
7. **E7 Behalten (Dual-Gate, 1H, Anker, Floor, Risiko, Sperrfristen) â€” zustimmen, ergaenzen** um den bestehenden Spread-Sanity-Check als hartes Gate (B.2 Nr. 2).
8. **E8 Zeitfenster 15:35-19:30, Exit 20:00 â€” zustimmen, Aenderung:** Entries bis **19:00** (Alternative B), weil 19:00-19:30-Entries bei R 60-100 Pkt fast nur Zeit-Exits produzieren.
9. **E9 Kosten messen vor Echtgeld â€” zustimmen, verschaerfen:** nicht nur messen, sondern Spread/SL-Distanz > 15 % = FAIL im kern_gate; Produktwahl (CFD/Mini-Future statt KO) ernsthaft pruefen, der Spread ist groesser als die Kante.
10. **E10 Validierung â€” aendern:** Levis 10 Fiktivtage als Maximum mit binaeren Kriterien und Uebergang ab Tag 5 (C.3), dann Opus' Stufen 1-3 unveraendert (10 Trades halb, bis 40 voll, Abbruch -8 R / Ã˜ â‰¤ -0,3 R nach 15 / 400 EUR).
11. **E11 Regel-Freeze waehrend Validierung â€” zustimmen, verstaerken:** gilt schon ab Fiktivtag 1; Aenderungs-Backlog statt Loop; eine Aenderung nach 20 Trades, nicht mehr.
12. **E12 Umsetzung Fable, Opus-Gegencheck, Commit nur Levi â€” zustimmen.** Reihenfolge: kern_gate + kern_nachtrag zuerst (damit die Fiktivtage ueberhaupt messbar sind), dann kern_check/loop_prompt; DST-Fix (26.-30.10., Frist 19.10.) von Anfang an. Aufwand 3-4 Tage ist realistisch; der 517-KB-gate_check bleibt fuer den Schatten unveraendert.

**Neue Entscheide:**
- **N1 BE-Stop-Definition:** BE = Entry +/- Spread (Long: Entry + Spread, Short: Entry - Spread); ein BE-Exit mit Spread-Verlust zaehlt nicht als Verlust fuer Regel 7.
- **N2 Re-Entry-Pause:** nach SL keine Wiedereroeffnung derselben Richtung mit demselben Anker innerhalb von 15 Min; neue Idee braucht neuen Trigger **und** neuen Anker.
- **N3 Trigger-Definition (vier Punkte aus B.4):** Cross statt Zustand; Verhalten bei 2/2-Bildung (Entry am 2/2-Schluss ODER Retest â€” Levi waehlt eines); Level-Liste im Start-Update eingefroren, Rundzahlen ja/nein; Entry-Preis = naechste Eroeffnung.
- **N4 1m-Bars je fiktivem Trade** (`data_get_ohlcv` Timeframe 1, Entry-Kerze +/- 3 Kerzen) im Nachtrag, damit Intrabar-Faelle wie 08.10. 18:16 entscheidbar werden.
- **N5 Spread-Protokoll:** Bid/Ask des real gehandelten Scheins bei jedem Gate notieren (Pflichtfeld `--spread-pkt`), Umrechnung in NAS-Pkt im kern_gate.
- **N6 WR-Definition:** WR := p (TP1 vor SL); p, q, c, Anteil Zeit-Exits und Anteil manueller Exits (Soll 0) sind die fuenf Kennzahlen jeder Zwischenpruefung â€” nicht "WR 50 %".
- **N7 Schock-Tier messbar:** ATR(5m) 15:30 â‰¥ 2x 20-Tage-Median â†’ kein Trade, automatisch im kern_check (ersetzt die manuelle Chop-/Schock-Einstufung).

### Ehrliche Gesamtwertung

**Gut:** Opus hat den Prozessbefund sauber belegt, die zwei Killer korrekt identifiziert, mit dem SL-Cap den einzigen tragfaehigen Filter gefunden und mit festen Ausgaengen die richtige Antwort auf das Payoff-Problem gegeben. Das 10-Regel-Werk ist in der Struktur richtig; die Validierung mit halbem Risiko und harten Abbruchkriterien ist vernuenftig. Levi, dein Befund "ueberreguliert" stimmt â€” nicht als Gefuehl, sondern als Zahl: 92 % der Freigaben wurden von einem nie kalibrierten Veto gestoppt.

**Nicht gut:** Die Gegenrechnung wird als "+0,23 R" praesentiert, obwohl drei ihrer vier TP2-Treffer unter dem vorgeschlagenen Regelwerk nicht ausfuehrbar sind, eine Re-Entry-Konvention die Summe um 3 R bewegt und der realistische KO-Spread sie auf null drueckt. Nach Opus' eigenen Regeln nachgerechnet bleiben +0,1 bis +0,2 R vor Kosten bei Â±0,4 â€” das ist **kein Nachweis, dass das gelockerte System Geld verdient**, und auch kein Nachweis des Gegenteils. Opus schreibt das in Zeile 10; es muesste in Zeile 1 stehen. Und Levis Kernformel traegt bei 50 % Trefferquote rechnerisch nicht, solange nicht die Haelfte der Gewinner bis 2R laeuft â€” das war historisch ein Viertel.

**Risiko:** Nicht die Lockerung. Das Risiko ist, dass die Lockerung als Beweis gelesen wird, dass die ersten 10-15 echten Trades mit SD 0,9 R voellig normal -3 bis -4 R liefern koennen, und dass dann die naechste Filterrunde beginnt â€” oder umgekehrt, dass ein Trendtag mit +3 R als Bestaetigung gilt. Beides ist Rauschen. Was das System wirklich entscheidet, sind drei Dinge, die keine Simulation misst: der reale Spread deines Produkts, ob der Trigger an Klebetagen 3 Verluste am Tag produziert, und ob du bei TP1-Teilverkauf und 20:00-Marktorder die Hand vom Rest laesst. Dafuer sind 40 echte Trades mit halbem Risiko der einzige Test, und der kostet im schlechtesten Fall 300-400 EUR â€” weniger als der 21.08. allein.

Mein Rat in einem Satz: Lockern ja, genau so wie Opus â€” mit 8c2, Spread-Gate, Re-Entry-Pause und einem Trigger, der "Cross" heisst; die 10 Fiktivtage als Prozesstest mit Ausstieg ab Tag 5; und ab dem ersten echten Trade kein Regelwort mehr bis Trade 20.

**Zeitstempel Ende:** 14.10.2026 12:23:45 DE (sha1-Endkontrolle 12:18:18, Originale unveraendert).
