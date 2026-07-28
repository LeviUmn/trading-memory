---
name: feedback-realisiertes-rr
description: "Realisiertes Chancen-Risiko-Verhältnis liegt bei ~0,78:1 statt geplanter 1:2 — 0 von 12 Trades hat je ein TP erreicht. Wichtigste Kennzahl, die vor Trade #13 fehlte (Fable-5-Review 04.07.2026)"
metadata:
  node_type: memory
  type: feedback
  originSessionId: fable-review-2026-07-04
  modified: 2026-07-28T16:40:39.919Z
---

Nach 12 abgeschlossenen Trades (Stand 04.07.2026) hat **kein einziger Trade je ein Take-Profit erreicht**. Jeder Gewinn war ein vorzeitiger manueller Exit (Divergenz-Erkennung, 3h-Regel, Session-Ende, Quarter-End), jeder Verlust lief bis zum SL (bzw. Breakeven-Stop). Realisiert: Ø-Gewinn ~9,70€ vs. Ø-Verlust ~12,50€ → **realisiertes RR ≈ 0,78:1**, obwohl die Regel in [[project_risikomanagement]] mindestens 1:2 vorschreibt.

**Why:** Diese Kennzahl wurde in keiner der drei bisherigen Nachbesprechungen (Phasenübergang-Review nach Trade #10, Regime-Analyse nach Trade #12, Backtest 03.07.2026) berechnet — alle fokussierten auf Setup-Qualität und Regelbefolgung beim Entry, nicht auf das tatsächliche Exit-Verhalten. Mit einem realisierten RR von 0,78:1 braucht das System eine Win-Rate deutlich über 50% nur für Break-even — bei aktuell 50% Win-Rate ist das strukturell das eigentliche Ergebnis-Problem, nicht Chop oder Marktregime. Erkannt durch unabhängigen Fable-5-Modell-Review (04.07.2026), der alle Memory-Dateien und Trade-Logs frisch durchgegangen ist.

**Entschieden 04.07.2026:** Teilverkauf-Standard eingeführt — TP1 (näheres Level, RR ≥1:1) + TP2 (Fernziel, RR ≥1:2), 50% bei TP1 realisieren, Rest mit Breakeven-SL und allen bisherigen Schutzsignalen (Divergenz, Chartmuster, 3h-Regel) weiterlaufen lassen. Details siehe [[feedback_chartanalyse]] Punkt 8b, Kasten-Format in [[feedback_live_trading]] Punkt 8.

**How to apply:** Ab Trade #13 den Teilverkauf-Standard anwenden und danach erneut Ø-Win, Ø-Loss, realisiertes RR und Expectancy aus [[trade_log]] berechnen (nicht nur Win-Rate) — prüfen, ob sich das realisierte RR durch die neue Regel tatsächlich Richtung 1:1,5-2 bewegt oder ob weitere Anpassung nötig ist. Diese Kennzahl bei jedem künftigen Phasen-Review verpflichtend mit ausweisen, nicht nur Win-Rate/Drawdown.

**Verwandte Befunde aus demselben Review, bereits in andere Dateien eingearbeitet:** Trade #12 war ein übersehener Cooldown-Verstoß (siehe [[feedback_regime_wechsel]]), es fehlt eine automatische Cooldown-Durchsetzung (kein Skript, nur Memory-Erinnerung — Ursache für den #12-Verstoß), und die Stichprobengröße (n=12-15) ist zu klein, um Win-Rate-Schwankungen (86%→50%) als echtes Signal statt Rauschen zu werten.

**Fortschreibung 13.07.2026 (nach Trade #17, zweiter Fable-Review):** Über alle 17 Trades: Ø-Gewinn ≈+1,49%/Win-Trade, Ø-Verlust ≈-1,37%/Loss-Trade → **realisiertes RR jetzt ≈1,09:1** — verbessert ggü. den 0,78:1 vom 04.07., aber weiterhin klar unter der 1:2-Pflicht. Der zweite Fable-Review deckte bei Trade #17 einen konkreten, unflaggten Fall auf: TP1 nur 0,32:1, TP2 nur 0,56:1 zum SL — ein Beleg, dass die 8b-Pflicht (Schritt 4: auslassen, wenn TP1 kein ≥1:1 bietet) im Loop nicht konsequent durchgerechnet wurde. Fix siehe [[feedback_chartanalyse]] Punkt 7a/8c (RR-Rückkopplung bei SL-Anpassung jetzt zwingend).

**Erwartungswert-Rechnung mit Ziel-RR (User-Diskussion 13.07.2026):** Bei gehaltener Trefferquote (66,7%, BE ausgeklammert) und einem realisierten RR von 2:1 statt aktuell 1,09:1 würde der Ø-Ertrag/Trade von aktuell +0,46% auf rechnerisch ≈+1,37% steigen (fast 3x). **Wichtige Erwartungssteuerung:** Striktere RR-Pflicht erhöht NICHT automatisch die Trefferquote — RR (Auszahlung pro Gewinn) und Win-Rate (Trefferwahrscheinlichkeit) sind unabhängige Achsen. Realistisch erwartbare Win-Rate bei konsequenter RR-Disziplin: **55-65%**, nicht die aktuellen 66,7% (Gesamt) bzw. 80% (Trades #13-17, kleine Stichprobe) als neue Baseline erwarten. Der Hebel liegt im Ø-Gewinn pro Trade, nicht in einer weiter steigenden Trefferquote.

**How to apply (ergänzt):** Bei jedem künftigen Phasen-Review sowohl realisiertes RR als auch die Ø-%-Kennzahl pro Trade (nicht nur Win-Rate) ausweisen — und eine sinkende oder stagnierende Win-Rate bei gleichzeitig steigendem realisiertem RR nicht als Verschlechterung werten, sondern als erwartbaren, gesunden Effekt der RR-Disziplin einordnen.

**Fortschreibung 23.07.2026 (Fable-Audit, nach Trade #24 — nachgeholt, war seit Trade #17 überfällig):** Über alle 24 Trades (Basis: [[trades/trade_log]], Ø-% pro Win-/Loss-Trade, BE-Trades #2/#11 ausgeklammert wie bisher): **Ø-Gewinn ≈+1,36%/Win-Trade** (14 Wins, Summe 19,00%), **Ø-Verlust ≈-1,10%/Loss-Trade** (8 Losses, Summe -8,815%) → **realisiertes RR ≈1,23:1** — verbessert ggü. den 1,09:1 vom 13.07.2026 (nach Trade #17), aber weiterhin klar unter der 1:2-Pflicht aus Punkt 8b. Sowohl Ø-Gewinn (1,49%→1,36%) als auch Ø-Verlust (1,37%→1,10%) sind seit dem 13.07. leicht gesunken — die RR-Verbesserung kommt also überwiegend aus kleineren Verlusten (u.a. SL-Nachzug-Disziplin, proaktive Teilexits), nicht aus größeren Gewinnen. Zwei neue Verlust-Trades seit dem letzten Stand (#21 -1,04%, #24 -0,665%) sind bereits in dieser Zahl enthalten.

**Einordnung:** 1,23:1 ist ein echter Fortschritt, bleibt aber weit von der eigenen Rechnung entfernt (2:1 RR bei gehaltener Win-Rate würde den Ø-Ertrag/Trade fast verdreifachen, siehe oben). Diese Berechnung war laut eigener "How to apply"-Pflicht bereits ab dem nächsten Phasen-Review fällig und wurde erst mit 7 Trades Verspätung nachgeholt (siehe Fable-Audit 23.07.2026) — künftig konsequenter bei jedem Review mitführen, nicht erst auf Nachfrage.

**Fortschreibung 23.07.2026 (nach Trade #25, Phase-2-Abschluss):** Trade #25 (+1,96%, Win, TP1 real erreicht + diszipliniertes SL-Trailing statt TP2-Warten) hebt den Ø-Gewinn auf **≈1,40%/Win-Trade** (15 Wins, Summe ≈20,96%), Ø-Verlust unverändert ≈-1,10%/Loss-Trade (8 Losses) → **realisiertes RR jetzt ≈1,27:1**. Für das für morgen (24.07.2026) geplante Phase-2-Abschluss-Review als aktuellster Stand verwenden.

**Wichtiger neuer Befund 24.07.2026 (Fable Phase-2-Abschluss-Review, siehe [[project_phase2_abschluss_review_2026-07-24]]): die blendete Lifetime-Zahl verdeckt eine reale Verbesserung innerhalb von Phase 2.** Getrennt nach Phase gerechnet (nicht kumulativ):
- **Phase 1 (isoliert, Trades #1-15): RR ≈1,18:1**
- **Phase 2 (isoliert, Trades #16-25): RR ≈1,75:1** — Ø-Gewinn 1,15%/Win (7 Wins), Ø-Verlust nur 0,66%/Loss (3 Losses)

Die RR-Verbesserung kommt in Phase 2 überwiegend aus deutlich kleineren, disziplinierter geführten Verlusten (0,66% Ø ggü. 1,365% Ø in Phase 1) — nicht aus größeren Gewinnen. Das ist das ermutigendste Einzelsignal aus dem Phase-2-Review, war aber in der bisherigen Blended-Zahl (1,27:1) unsichtbar.

**How to apply (ergänzt 24.07.2026):** Ab Phase 3 (Trade #26) eine eigene, separat getrackte RR-Zahl NUR für Phase 3 führen, zusätzlich zur laufenden Lifetime-Zahl — damit künftige Reviews nicht wieder von einer trägen Blended-Zahl verzerrt werden, die reale Phasen-Fortschritte verdeckt.

**Fortschreibung 28.07.2026 (Fable-Tagesabschluss-Review, nach Trade #26/#27, erste 2 Phase-3-Trades):** Blended Lifetime über alle 27 Trades: Win-Sum 20,96% (15 Wins bis #25) + 1,81% (Trade #27) = 22,77% bei 16 Wins → **Ø-Gewinn ≈1,42%/Win-Trade**. Loss-Sum 8,815% (8 Losses bis #25) + 2,235% (Trade #26) = 11,05% bei 9 Losses → **Ø-Verlust ≈1,23%/Loss-Trade**. **Realisiertes RR blended jetzt ≈1,16:1** — leichter Rückgang gegenüber 1,27:1 vom 23.07., weil Trade #26 (RR-Grenzfall 1,0:1, SL griff voll) größer war als der bisherige Ø-Verlust und Trade #27 (+1,81%) kleiner als der bisherige Ø-Gewinn. Kein Alarmsignal bei n=2 — einzelne Trades bewegen die Blended-Zahl bei diesem Sample-Umfang noch spürbar, siehe [[project_robustheit_monte_carlo]] (Schwelle für belastbare Aussagen liegt bei ~50 Trades, aktuell 27).

**Phase 3 isoliert (Trades #26-27, n=2, laut how-to-apply oben separat zu führen):** Ø-Gewinn 1,81% (1 Win), Ø-Verlust 2,235% (1 Loss) → **RR Phase 3 bislang ≈0,81:1** (bestätigt durch `trade_stats.cjs --phase 3`). Bei n=2 nicht überinterpretieren — Phase 3 ist gerade erst gestartet (gestaffelter Einstieg, siehe [[project_risikomanagement]]), diese Zahl wird sich mit jedem weiteren Trade stark bewegen. Weiter mitführen, aber noch keine Rückschlüsse auf die Qualität der Phase-3-Reform (RR-Entkopplung TP1/TP2 vom 27.07.) ziehen.
