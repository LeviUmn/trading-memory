---
name: project-testtag-analyse-2026-09-03
description: "Opus-Analyse des Testtags 03.09.2026 auf Levis Anfrage: warum kein Long-Trade trotz 2 sauberen Dual-Gate-2/2-Triggern (+476 Pkt Trendtag), Chasing-Check-Bewertung, Meta-Frage ob Regelwerk zu eng ist. Kernbefund: RR-Decke 1,33:1 (voll)/2,0:1 (halb) macht Levis Zielbild strukturell unerreichbar bei voller Größe; 2 konkrete Prozessfehler (veralteter SL-Anker, nie angewendete 50%-Chasing-Pflicht aus Punkt 13.1) + 1 echte Skriptlücke (leeres TP-Fenster bei SL>3xATR, undokumentiert)."
metadata:
  node_type: memory
  type: project
  status: open
  originSessionId: session_01GAqDaLvWGcEg7c44Lreoaj
  modified: 2026-09-10T10:59:40.777Z
---

## Auslöser

Levi fragte am 04.09. nach Opus-Analyse des Testtags 03.09. (58 Voll-Checks, +476 Pkt Trend, 2 Dual-Gate-2/2-Trigger, aber 0 Trades, beide gate_check.cjs FAIL am RR-Gate). Vier konkrete Fragen: (1) warum kein Long 15:30-16:10, (2) warum kein Long ~16:55/VC#23, (3) ist Chasing-Check zu vorsichtig, (4) ist das Regelwerk generell zu eng geworden (Levis Zielbild: ~50% WR bei RR 1,5-2:1 reicht für Profitabilität).

## Kernbefunde

**F1 (15:30-16:10):** Levis Faktenbehauptung stimmt (14-15-Uhr-1H-Bar schloss über EMA50). Blocker war NICHT EMA, sondern Sonnets Eigenauslegung "nur 2. bestätigter 1H-Bar-Schluss zählt" (siehe [[project_1h_kriterium_offene_frage_2026-09-03]]). Verpasstes chasing-freies Fenster: 15:32-15:41 (VC#5/#6), danach zusätzlich Chasing-Check aktiv. Als das 1H-Kriterium um 16:02 endlich erfüllt war, stand der Kurs bereits 110 Pkt unter dem Session-Hoch — das Kriterium erfüllte sich exakt dann, wenn es wertlos war.

**F2 (VC#23, 17:04):** RR-Gate FAIL (0,699:1) hatte zwei Ursachen: (a) echte Skriptlücke — bei SL >3×ATR ist RR≥1:1 arithmetisch unerreichbar, weil 8b1 TP1 bei 3×ATR hart kappt (Zone 3 FAIL); das Skript meldet das nicht als solches. (b) Prozessfehler — SL wurde an einer 50 Min. alten Zone (29.199,15) verankert statt am frischen Verlaufstief (29.239,35); mit SL 29.230 wäre RR 1,055:1 gewesen → PASS. Punkt 7a (tiefer Chop-Anker) gilt laut Wortlaut nur für Chop-Tage; der 03.09. war seit VC#7 durchgehend als Trend klassifiziert.

**F3 (Chasing-Check):** gate_check.cjs kennt gar keinen Chasing-Check (0 Treffer im Code) — läuft rein manuell im Voll-Check. Die seit 30.07.2026 vorgeschriebene 50%-Position bei anhaltendem Chasing (Punkt 13.1, "aktiv vorzuschlagende Handlung", nicht nur Option) wurde am 03.09. trotz erfüllter Bedingung (Chasing ab VC#7, >2 Checks ohne Punkt-11-Kippsignal) kein einziges Mal zitiert oder angewendet. Grund: 13.1 setzt "vollständig übereinstimmendes Dual-Gate" voraus — die 1H-Sperre aus F1 hat diesen Auffangmechanismus mit deaktiviert (unentdeckter Regel-Interlock).

**F4 (Meta/Regelwerk zu eng?):** Muster ist systematisch: 7 gate_check-Läufe über 3 Tage (01./02./03.09.), 0 PASS, 6/7 FAIL am RR-Gate. Arithmetischer Kernbefund: Regelwerk deckelt RR bei voller Größe (Zone 1) auf max. 1,33:1 (TP-Cap 2×ATR / SL-Floor 1,5×ATR), bei halber Größe (Zone 2) auf max. 2,0:1. **Levis Zielbild RR 1,5-2:1 ist bei voller Positionsgröße im aktuellen Regelwerk arithmetisch nicht erreichbar** — nur in Zone 2 (halbe Position). Trotzdem: Opus stimmt Levis These nur teilweise zu — an beiden konkreten Trigger-Punkten des 03.09. gab es einen regelkonformen, nicht genommenen Pfad (siehe F1/F2/F3). Diagnose: Problem liegt überwiegend an Nicht-Anwendung bestehender Regeln, nicht an zu strengen Regeln selbst — mit 3 Ausnahmen, die echte Regelwerkslücken sind (RR-Decke, leeres TP-Fenster >3×ATR, nur rückwärtsgerichtete Levelquellen — Fib-Extension nie genutzt trotz Regelwerksnennung seit 04.07.2026).

> **F4 "RR-Decke" — ERLEDIGT/BEANTWORTET (markiert 10.09.2026):** Die Frage "RR = TP1-Zahl oder Blend?" war faktisch bereits am 04.09. mit Levis Zielbild-Klarstellung + P7 beantwortet (RR = TP1-Zahl als Hard Gate ≥1:1, kein RR-Ziel, Maßstab ist die realisierte Kapitalrendite im Monatsdurchschnitt) und wurde nur nie als erledigt markiert. Am 10.09.2026 abschließend verankert in [[feedback_live_trading]] 7b1 P7; Vorlage [[project_rr_decke_entscheidungsvorlage_2026-09-10]] damit geschlossen. Kein Blend-RR, keine Schwellenänderung.

## Vorschläge (V1-V7, priorisiert, noch NICHT umgesetzt)

- V1 (höchste Prio, kleinster Eingriff): Leeres-Fenster-Guard in gate_check.cjs — Diagnosezeile "max. erreichbares RR bei dieser SL-Distanz" wenn SL >3×ATR
- V2: SL-Anker-Aktualitätsregel in 7b1 Schritt 4 — bei Regime=Trend SL am jüngsten Verlaufsextrem verankern statt am tiefsten Session-Punkt (7a bleibt auf Chop/Schock beschränkt)
- V3: Fib-Extension/Measured Move als Pflicht-TP-Levelquelle (vorwärtsgerichtet statt nur rückwärtsgerichtete Register-Level)
- V4: 1H-Kriterium — Überschuss-Klausel (≥1,0×ATR(5min) Überschuss über ≥2 bestätigte 5min-Schlüsse gehalten = gilt als bestätigt), Gegenprobe gegen 01./02.09. nötig (Overfitting-Warnung)
- V5: Punkt 13.1 verankern — Pflicht-Ausgabezeile "Chasing-Status/13.1-Konsequenz" pro Voll-Check, Entkopplung von "vollständigem" Dual-Gate wenn nur 1H-Formalkriterium fehlt
- V6: Retest-Zeitbox nach Gate-FAIL erzwingen (max. 2 Voll-Checks, war schon 2026 vorgesehen, nie durchgesetzt — stattdessen 35 VCs "kein erneuter Aufruf")
- V7 (NICHT empfohlen, außer nach Datenlage): 8c2-Puffer lockern — stammt aus echten Verlusten (#42/#43), war am 03.09. nicht die bindende Restriktion. Einzige erwägenswerte Lockerung: 3×ATR-Außengrenze in 8b1 anheben, aber erst nach V1-V3 mit neuen Daten.

## Fable-Gegencheck (04.09., auf Levis ausdrücklichen Wunsch vor jeder Umsetzung)

Fable hat live gegen den echten NAS100-Feed nachgerechnet (nicht nur das Protokoll gelesen) und widerspricht Opus an zwei entscheidenden Stellen:

**Zu F2/V2 (SL-Anker) — WIDERSPRUCH:** Opus' "frisches Verlaufstief 29.239,35" war ein Schlusskurs, nicht das Tief. Das echte Tief war 29.204,25 (16:55-Kerze, Docht, nur 8 Min. vor dem Trigger, direkt in der 8c2-Cluster-Zone). Ein SL bei Opus' 29.230 hätte 26 Pkt über einem 8 Minuten alten Docht gelegen — exakt das Muster, für das 8c2 nach #42/#43 gebaut wurde. **V2 wie von Opus formuliert würde diesen Fehler regelseitig legalisieren — nicht umsetzen.** Zudem: selbst mit Opus' SL wäre GESAMTSTATUS UNKNOWN (nicht PASS) gewesen, weil `--tp2-level-price` fehlte — kein Level ≥29.550 im Register. "Wir wären nachweislich im Trade gewesen" ist so nicht belegt, es hätte zwei Fixes gebraucht. Fables Ersatzvorschlag: SL-Anker-Definition klarstellen ("tiefstes Low, nie Close, seit letztem Impulshoch"; 8c2 bleibt regime-unabhängig, 7a-Chop-Ausnahme bleibt eng begrenzt).

**Zu F4 (RR-Decke) — TEILWIDERSPRUCH, wichtigste offene Frage:** Fable bestreitet nicht die Arithmetik (1,33/2,0 stimmt), aber die Diagnose: das ist eine **TP1-Decke**, keine Trade-Decke — 8b2 verlangt für TP2 sogar RR≥2:1, ein Blend aus TP1(1,2)+TP2(2,5) ergibt ~1,85:1 bei VOLLER Größe. Levis Zielbild wäre damit ggf. schon erreichbar, je nachdem ob er RR als TP1-Zahl oder als Trade-Blend meint — **das ist eine offene Entscheidungsfrage an Levi, keine Regelwerksfrage.** Außerdem: die Decke hat am 03.09. nie gebunden (beide FAILs lagen bei 0,15/0,70, weit unter 1,33) — bindend war das TP1-Fenster (3×ATR minus SL-Distanz), das mit der Entry-Distanz zur Struktur schrumpft. Fable fand zusätzlich einen von Opus übersehenen Fehler: bei VC#14 war das Rundzahl-Band im Register veraltet (verletzte die eigene ±150-Pkt-Pflegepflicht) — mit korrektem Band wäre TP1=29.400 verfügbar gewesen, RR 1,085 PASS (aber weiterhin UNKNOWN wegen TP2). Fables Fazit: **Sicherheitsschwellen (8c-Floor, 8c2, RR≥1, Zone-3-Cap) nicht anfassen** — keine davon war am 03.09. die bindende Restriktion, alle stammen aus echten Verlust-Vorfällen. Der echte Hebel liegt in Register-Vorwärtspflege (Rundzahl-Band-Pflicht durchsetzen, Fib-Extension bei erkennbarem Impuls erzwingen) + einer echten Fenster-Diagnose-Zeile im Skript (erweitert V1: Fenstergrenzen, Register-Treffer im Fenster, Entry-Obergrenze für RR≥1) statt einer Schwellenänderung.

**Zu F3 (Chasing) — ERGÄNZUNG, kein Widerspruch:** Bestätigt Opus' Kernbefund, aber: der Interlock liegt VOR dem Skript, nicht im Skript — gate_check.cjs wurde im Chasing-Fenster (15:41-16:00) gar nicht aufgerufen, weil der 2/2-Trigger wegen der 1H-Auslegung nie erkannt wurde. Fables Wurzel-Fix: 1H-Override im Regelwerk klarstellen als "Bias des zuletzt GESCHLOSSENEN 1H-Bars" (kein Schluss-über-Marke-Zusatzkriterium) — das löst F1 UND den F3-Interlock gleichzeitig und macht V4 überflüssig. Zusätzlich empfiehlt Fable ein echtes Code-Feld `--chasing yes|no` (A3-Pflichtfeld) statt nur Protokollzeile, robuster/erzwingbar. Warnt aber: ein Half-Entry beim frühestmöglichen 13.1-Zeitpunkt (VC#9, 15:51) wäre um 16:00 reingelaufen und ausgestoppt worden — 13.1 hätte den Tag nicht "gerettet", das chasing-freie Fenster VC#5/6 (15:32-15:41) bleibt der eigentliche verpasste Fall.

**V4 (1H-Überschussklausel):** Fable rät ab — überflüssig, wenn der 1H-Override korrekt als reiner Bias (kein Schluss-Kriterium) definiert wird, statt eine neue kalibrierte Schwelle einzuführen.

**Fables Priorisierung:** Zuerst (klein, risikolos): 1H-Override-Klarstellung, erweiterte Fenster-Diagnose + Rundzahl-Band-Warnung im Skript, `--chasing`-Feld, SL-Anker-Definitionsklarstellung. Danach: V3 (Fib-Extension, im Skript selbst aus Impuls-Hoch/-Tief berechnet statt Register-abhängig), V6 (Retest-Zeitbox). Nicht jetzt: alle Sicherheitsschwellen (Zone-1-Cap, 8c-Floor, 8c2, RR≥1) — erst beim 15-Trade-Review mit neuer Fenster-Statistik. Gar nicht: V2 wie von Opus formuliert, V4, V7.

## Levis Zielbild-Klarstellung (04.09., nach Fable-Gegencheck)

Kein RR-Ratio-Ziel — Levis Maßstab ist **Ø 1,5% Kapitalrendite pro Gewinn-Trade, gemittelt über einen Monat, geblendet über TP1+TP2/BE**. Das ist der Maßstab, an dem jede künftige Regelwerksänderung zu messen ist, nicht eine RR-Zahl.

## Finale Opus-Adjudikation nach Fable-Gegencheck (04.09.)

Opus wurde mit Fables Faktenkorrektur (echtes Tief 29.204,25 um 16:55, nur 8 Min. vor VC#23, stand bereits wörtlich im Protokoll) und Levis Zielbild-Klarstellung konfrontiert und hat final adjudiziert:

**Zieht vollständig zurück:** Befund B (SL an veralteter Zone), "wir wären nachweislich im Trade gewesen" (auch mit korrigiertem SL wäre GESAMTSTATUS UNKNOWN wegen fehlendem TP2-Level gewesen), V2 in Opus' ursprünglicher Fassung. Übernimmt Fables SL-Anker-Definitionsklarstellung unverändert (tiefstes Low, nie ein Close; 8c2 regime-unabhängig).

**Verschiebt sich dadurch:** VC#23 war — wie VC#14 — strukturell unvermeidbar. Ursache liegt eine Stufe früher: Entry lag bei 2,79×ATR vom Impulsursprung (Schwelle 1,5×), Q2-Budget −86% — beide Werte standen im Output, blieben aber folgenlos, weil reines Anzeige-Signal. Kausalkette: Entry zu weit von Struktur entfernt → SL zwangsläufig >3×ATR → TP-Fenster leer, unabhängig vom TP-Level. **Kein Schwellenproblem, ein Entry-Timing-Problem.**

**F4 neu gerechnet mit Levis echtem Ziel:** Rendite = (Risiko%) × RR_blended — reine %-Rechnung, unabhängig von der Kapitalgröße (nur die €-Beträge dahinter skalieren mit). Bei 1,5% Risiko: theoretischer Blend (RR 1,85) → 2,78%, **historischer realer Blend (RR 1,16 aus 27 Trades) → 1,74%** — beide über dem 1,5%-Ziel bei VOLLER Position. Bei halber Position (Zone 2/Chasing) fällt es unter das Ziel (1,39% theoretisch, 0,87% real). ~~**KAPITAL-UPDATE 04.09.2026:** Gesamtkapital ist von 15.000€ auf 50.000€ gestiegen (siehe [[project_risikomanagement]]) — die %-Schlussfolgerung bleibt unverändert gültig, nur die absoluten €-Beträge ändern sich (1,5%-Risiko: 225€→750€). P7 unten entsprechend mit 50.000€-Basis umzusetzen.~~ **KORRIGIERT 10.09.2026 (Levi):** Die 50.000 € sind kein aktuelles Kapital, sondern der Zielwert nach Abschluss aller Phasen; die Kapitalbasis ist phasenabhängig (Phase 3 = 5.000 €, Risiko 1,5 % = 75 €). Die %-Schlussfolgerung (Rendite = Risiko% × Payoff) bleibt davon unberührt. **Kernaussage: nicht die RR-Schwellen sind der Hebel — jede einzelne Positions-Halbierung drückt einen Trade unter Levis Ziel.** V7 (3×ATR-Grenze anheben) damit endgültig verworfen, nicht nur verschoben — bringt für das reale Ziel nichts. Einschränkung: TP2 wird empirisch selten realisiert (erste 12 Trades: nie), das gehört separat auf die Agenda (P9).

## FINALE gemeinsame Prioritätenliste (Opus+Fable konvergiert, bereit für Fable-Umsetzung)

**Stufe 1 — sofort, reine Diagnose/Prozess, kein Gate wird gelockert:**
- P1: Retest-Zeitbox nach Gate-FAIL erzwingen (war V6, jetzt zentral — adressiert beide Trigger des Tages, dieselbe Ursache)
- P2 (neu): Entry-Reifegrad-Eskalationszeile — Pflichthinweis bei Impuls-Reifegrad >1,5×ATR, verweist auf 8b-Schritt-4-Option 2 (engerer Entry/Retest)
- P3: Leeres-Fenster-Guard in gate_check.cjs (war V1) — Diagnosezeile bei SL>3×ATR: "UNLÖSBAR, Ursache ist Entry/SL-Geometrie, nicht TP-Wahl"
- P4: SL-Anker-Definitionsklarstellung (Fables Fassung) — tiefstes Low nie Close, OHLC+Zeitstempel-Pflicht, 8c2 regime-unabhängig

**Stufe 2 — direkt danach, adressiert Ertragsseite:**
- P5: Register-Vorwärtspflege (war V3) — Fib-Extension 1.272/1.618 + rollendes Rundzahl-Band als Pflichtfelder (in 58 VCs nie als Extension genutzt, nur als Retracement). Kein ATR-Vielfaches als TP1-Ersatz (bricht 8b1-Prinzip).
- P6: Punkt 13.1 verankern (war V5) — Pflichtzeile bei Chasing ab Check 2, Entkopplung vom formalen 1H-Kriterium solange nur dieses fehlt
- P7 (neu): Risiko-Ausnutzungs-Zeile vor jeder Freigabe — Risiko€/Kapital%, erwartete Rendite bei TP1/TP2-Blend vs. 1,5%-Ziel, alle aktiven Halbierungsgründe

**Stufe 3 — erst mit Daten, nicht jetzt:**
- P8: 1H-Überschuss-Klausel (war V4, herabgestuft) — muss erst gegen 01./02.09. gegengerechnet werden (dort blockierte dasselbe Kriterium #20/#34 korrekt)
- P9 (neu): TP2-Realisierungsquote messen (real 1,16 vs. theoretisch 1,85 Blend) — separates 15-Trade-Review

**Explizit NICHT umsetzen:** 8c2 lockern (band am 03.09. korrekt), 3×ATR-Außengrenze anheben (wirkungslos für Levis reales Ziel), RR-Gate-Schwelle 1:1 antasten (war nie Levis Maßstab), Opus' ursprüngliches V2.

## Umsetzung abgeschlossen (04.09.2026, Fable)

P1-P7 vollständig umgesetzt und getestet (9/9 Tests grün), P8/P9 bewusst nur als Stufe-3-Vormerkung dokumentiert, nicht scharfgeschaltet. Details:

- **Code** (`scripts\gate_check.cjs`, +231 Zeilen): P3 TP1-Fenster-Diagnose (inkl. arithmetischer Rundzahlen im Fenster, Entry-Ober-/Untergrenze, UNLOESBAR-Meldung bei SL>3×ATR), P2 Entry-Reifegrad-Eskalationszeile, P1 Retest-Zeitbox-Pflichtzeilen (nur live), P6 `--chasing yes|no` als neues A3-Pflichtfeld (Hard-Exit ohne Wert) + Plausibilitäts-Warnung, P7 Risiko-Ausnutzungs-Zeile (Default 50.000€/1,5%, nur bei PASS). Keine bestehende Schwelle verändert, kein neues Hard-Gate.
- **Regelwerk:** 1H-Override in `feedback_live_trading.md` klargestellt (Bias des letzten geschlossenen Bars, kein Schluss-über-Marke-Kriterium — löst F1 und den F3-13.1-Interlock strukturell); SL-Anker-Definition (P4); 13.1 verankert mit Pflichtzeile+Entkopplung (P6); 8b Retest-Zeitbox (P1), 8b1 TP1-Fenster-Diagnose (P3), 8b2 Register-Vorwärtspflege/Fib-Extension-Pflicht (P5) in `feedback_chartanalyse.md`; Rundzahl-Band-Reichweite in `feedback_session_update.md`; Voll-Check-Template in `feedback_vollcheck_format.md` erweitert. `project_1h_kriterium_offene_frage_2026-09-03.md` auf `status: resolved` gesetzt (Option 2 gewählt, P8 zurückgestellt).

**Offene Rückfragen von Fable, noch mit Levi zu klären:**
1. ~~`scripts\size.cjs` zeigt in einer Fehlermeldung noch "15000" — Update hängt an der noch offenen Phasen-€-Bandbreiten-Entscheidung (siehe [[project_risikomanagement]]).~~ **ERLEDIGT 10.09.2026:** `size.cjs` liest die Kapitalbasis phasenabhängig aus `kapital_constants.cjs` (kein `--kapital` mehr für NAS100), die Bandbreiten-Frage ist damit gegenstandslos — Basis = Positions-Obergrenze der Phase ([[project_risikomanagement]]).
2. ~~**P7-Zielurteil-Basis ungeklärt:** aktuell hängt ERREICHT/VERFEHLT (1,5%-Ziel) am theoretischen 50/50-TP1/TP2-Blend (RR 1,85) — die konservative Zeile (TP1+Rest BE) steht informativ daneben, entscheidet aber nicht. Levi muss festlegen, welche Basis für das Urteil zählen soll.~~ **ERLEDIGT (04.09. entschieden, 10.09.2026 markiert):** P7-Zeile wurde am 04.09. komplett aus `gate_check.cjs` entfernt (Renditeziel ist kein Entry-Kriterium); RR = TP1-Zahl, kein Blend — siehe F4-Kasten oben und [[feedback_live_trading]] 7b1 P7.
3. `--chasing` ist jetzt Hard-Exit-Pflichtfeld — der CronCreate-Prompt für den nächsten Testtag muss das neue Template aus 2b/7b1 übernehmen, sonst bricht der erste gate_check.cjs-Aufruf mit Exit 1 ab.
4. Fib-Extension wird weiterhin manuell von Sonnet berechnet und ins Register geschrieben (nicht skriptseitig) — optionaler Folgeschritt (~20 Zeilen) auf Wunsch.

## Opus-Schlusskontrolle + P7-Grundsatzentscheidung (04.09.2026)

Opus hat P1-P7 live gegen beide 03.09.-Trigger nachgetestet: P1-P6 korrekt und vollständig, P4 (SL-Anker) explizit als "meine eigene Fassung war falsch, Fables ist richtig" bestätigt. 2 echte Bugs gefunden (B1: P7 leckte in Batch-Modus; B2: P1-Entry-Grenze prüft nur RR≥1, nicht Zone-3-Schranke) + Memory-Hygiene-Lücken (B4: veraltete Links/Descriptions, fehlender MEMORY.md-Eintrag, Dateiablage außerhalb kanonischem Store) + 2 unwichtige Altlasten (B3: stale RR≥1,5:1-Text, B5: stale 15.000€-Description).

**P7-Grundsatzentscheidung (Levi-Frage, Opus-Antwort):** Levi stellte klar, dass die Entry-Entscheidung ausschließlich an den Hard-Gates hängt (TP1 RR≥1:1 etc.), TP2 RR≥2:1 ist dabei KEIN Hard-Gate sondern nur Sizing-Signal (löst laut 8b2 "halbe Position" aus, nie FAIL) — sein 1,5%-Monatsdurchschnittsziel ist für den Entry-Moment irrelevant. Opus stimmt uneingeschränkt zu und zieht seine eigene vorherige P7-Empfehlung (neutrale Diagnosezeile im Skript) zurück: **P7 wird komplett aus `gate_check.cjs` entfernt** (Objekt, 4 Ausgabeblöcke, 2 CLI-Parameter, 3 Konstanten — damit erledigt sich B1 ersatzlos mit). Grund: die Zahl ist am Entry redundant (alle Faktoren stehen bereits anderswo im Output) und semantisch gefährlich (wirkt trotz neutraler Formulierung als drittes Kriterium; einzige Wege sie zu verbessern — TP hinausschieben/SL verengen — sind im selben Output verboten). **Das eigentliche Renditeziel wird ausschließlich in `trade_stats.cjs` aus realisierten Ergebnissen gemessen** (Monatsdurchschnitt, nicht Einzeltrade).

**Wichtiger Strukturbefund, der bleibt (als Regelwerks-Prosa, nicht als Ausgabezeile):** Bei 1,5% Risiko/Trade liefert "TP1+Rest-BE" max. 1,0% (volle Position) / 0,75% (halbe Position) — das 1,5%-Ziel ist nur über tatsächlich realisiertes TP2 erreichbar (~3,1% bei RR2≈3). **Hängt an genau einer Größe: der TP2-Realisierungsquote (P9) — dadurch von "Stufe 3, später" auf "unmittelbar nach dem nächsten Testtag" hochgestuft.**

**Finale Fix-Liste an Fable (04.09.2026, letzte Runde vor nächstem Testtag):**
1. P7 komplett aus gate_check.cjs entfernen (vor Testtag)
2. `feedback_live_trading.md:638` durch Opus' Klarstellungstext ersetzen (vor Testtag)
3. B2 Entry-Grenze um Zone-3-Schranke ergänzen (vor Testtag)
4. B4 Memory-Hygiene (vor Testtag)
5. `trade_stats.cjs` Monatsdurchschnitts-Zeile (NACH dem nächsten Testtag)
6. B3/B5 Altlasten (bei Gelegenheit, kein Termindruck)

Status: **Fix-Liste an Fable geschickt, Umsetzung ausstehend.** Nach Abschluss: bereit für nächsten Live-Testtag (Opus-Gesamturteil: kein Blocker, nach Fix 1-4 startklar).

## Finale Abnahme (04.09.2026, Opus) — ABGESCHLOSSEN

Fix 1-4 umgesetzt und von Opus unabhängig verifiziert (Code selbst gelesen, Skript real ausgeführt — Long/Short/Grenzfälle/Batch/`--json`, Dateisystem+Regelwerkstext geprüft). Ergebnis: **alle vier Punkte vollständig, korrekt, ohne Reste.** P7 sauber entfernt (0 Treffer bei Grep auf alte Felder/Konstanten außerhalb Kommentaren, keine gebrochenen Referenzen in add_trade.cjs/trade_db.cjs/etc.), B2-Entry-Fenster für Long+Short korrekt gespiegelt getestet, B4-Dateien liegen jetzt im kanonischen Store, alle Wikilinks lösen auf, Regelwerkstext wörtlich und konsistent platziert.

**Ein neuer, nicht-blockierender Fund (B6):** Das Entry-Fenster aus B2 deckt nur 2 von 3 entry-abhängigen Hard-Gates ab (RR≥1 und Zone-3-Grenze), die 8c-SL-Floor-Schranke fehlt noch. Dadurch kann das Fenster einen Teilbereich als PASS-tauglich ausweisen, der tatsächlich am SL-Floor scheitern würde — 8c fängt das beim nächsten gate_check.cjs-Aufruf zuverlässig ab (kein falscher Trade möglich), es ist nur ein irreführender Hinweis in der Retest-Zeitbox-Empfehlung. Fix ist trivial (~2 Zeilen, Spec von Opus vorgegeben), aber optional/nicht zeitkritisch.

**GESAMTURTEIL (Opus): Bereit für den nächsten Live-Testtag. Kein zwingender Fix vor dem nächsten Trigger.**

**Verbleibende Punkte, nach Dringlichkeit:**
- B6 (Entry-Fenster um 8c-Schranke ergänzen) — optional, vor dem Testtag nur falls Fable ohnehin dranbleibt, sonst danach
- P9 (TP2-Realisierungsquote) — **direkt nach dem nächsten Testtag**, einziger verbliebener Hebel auf Levis Renditeziel
- `trade_stats.cjs` Monats-Renditezeile (Punkt 5) — nach dem Testtag
- B3/B5 (stale Textreste) — bei Gelegenheit
- Phasen-€-Bandbreiten nach Kapital-Update — bewusst offene Levi-Entscheidung, kein Termindruck

**Hinweis fürs nächste Testtag-Debriefing:** P1-P3 wirken erst über die tatsächliche Reaktion darauf (ausgefüllte Retest-Zeitbox-Pflichtzeilen nach einem FAIL) — das ist das eigentliche Abnahmekriterium in der Praxis, nicht nur die Skript-Ausgabe selbst.

## Letzte Runde abgeschlossen (04.09.2026) — B6, trade_stats.cjs, B3/B5

Levi-Entscheidung zu den 5 offenen Punkten: B6 sofort umsetzen, P9 bleibt nach dem Testtag, `trade_stats.cjs`-Zeile sofort umsetzen (nicht mehr aufschieben), B3/B5 sofort mit erledigen, Phasen-€-Bandbreiten bleiben unverändert.

- **B6:** Entry-Fenster in `gate_check.cjs` um die dritte Schranke (8c-SL-Floor, `sl ± 1,5x/2,5x ATR`) erweitert — genau nach Opus' Spec. Test bestätigt Opus' eigenen Befund: das VC#14-Fenster, das B2 noch als teilweise gültig auswies, ist mit der dritten Schranke jetzt korrekt `LEER`.
- **`trade_stats.cjs`:** neuer Rendite-Block (`Ø Rendite je GEWINN-Trade, Monat X` vs. 1,5%-Ziel + Gesamt-Durchschnitt), gegen DB-Rohsummen verifiziert. **Ehrliches Ergebnis über alle 43 realen Trades: Ø nur +0,20% je Gewinn-Trade — das 1,5%-Ziel wurde bisher in keinem Monat annähernd erreicht** (erwartet, da Phase 1/2 am Positions-Limit statt am Risiko-Limit hingen, siehe `project_risikomanagement.md`). Kapital-Split korrekt: Trades vor 04.09. mit 15.000€, danach mit 50.000€, nicht rückwirkend umbasiert.
- **B3/B5:** stale RR≥1,5:1-Referenz und stale 15.000€-Description korrigiert, historische Fließtext-Rechnungen bewusst unangetastet gelassen.
- Selbst stichprobenartig im Code verifiziert (Grep auf neue Konstanten/Funktionen) — Fables Bericht deckt sich mit dem tatsächlichen Stand.

**Status: Zyklus vollständig abgeschlossen.** Alle vereinbarten Punkte (P1-P7, B2/B4/B6, trade_stats.cjs, B3/B5) umgesetzt und getestet. Offen bleibt nur P9 (nach dem nächsten Testtag) und die bewusst zurückgestellte Phasen-€-Bandbreiten-Entscheidung. System bereit für den nächsten Live-Testtag.
