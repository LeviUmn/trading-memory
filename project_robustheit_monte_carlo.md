---
name: project-robustheit-monte-carlo
description: "Offene Erinnerung: Monte-Carlo-Robustheitscheck der Trade-Historie durchführen, sobald genug Stichprobe vorhanden ist (Schwelle ~50 abgeschlossene Trades)"
metadata:
  node_type: memory
  type: project
  originSessionId: session-2026-07-13
  modified: 2026-08-24T10:46:00.673Z
---

## Status: noch nicht fällig

**Stand 13.07.2026 (nach Trade #17):** 17 abgeschlossene Trades (siehe [[trades/trade_log]]). Schwelle für einen sinnvollen Monte-Carlo-Check: **~50 Trades**.

**Warum diese Schwelle:** Eine Monte-Carlo-Simulation (Trade-Reihenfolge zufällig neu mischen, um zu prüfen, ob Drawdown/Win-Rate robust sind oder nur eine glückliche Reihenfolge widerspiegeln) braucht genug Stichprobe, damit das Ergebnis statistisch aussagekräftig ist. Bei 16 Trades wäre jede Simulation im Wesentlichen Zufallsrauschen — kam aus einer Wochenend-Recherche des Users zu Robustheitsprüfung/Overfitting/Walk-Forward/Out-of-Sample-Tests, siehe [[feedback_backtest_ablauf]] für die Einordnung der übrigen Konzepte.

## Was zu tun ist, sobald die Schwelle erreicht ist

1. Aus `trades/trade_log.md` die Ergebnis-Spalte (€ oder R-Multiple pro Trade) extrahieren
2. Trade-Reihenfolge tausendfach zufällig neu mischen (z.B. per Skript), für jede Simulation Equity-Kurve und maximalen Drawdown berechnen
3. Prüfen: Wie breit ist der Streubereich von Drawdown und Endergebnis über alle Simulationen? Liegt das reale Ergebnis nahe am Median oder am Rand der Verteilung (= eher Glück in der Reihenfolge)?
4. Ergebnis als neue Memory-Datei speichern, Risikomanagement-Regeln (Positionsgröße, Cooldown-Schwelle) ggf. anhand des tatsächlichen Ruin-Risikos justieren

## Trigger

**Bei jedem "start update dich" oder Backtest-Review:** aktuellen Trade-Count aus `trades/trade_log.md` prüfen. Sobald ~50 abgeschlossene Trades erreicht sind, User aktiv darauf hinweisen, dass der Monte-Carlo-Check jetzt sinnvoll durchführbar ist — nicht warten, bis der User selbst danach fragt.

## Nachtrag 12.08.2026 — Vier Ambiguitäten aus der Fable-Vollprüfung Phase 1-3 aufgelöst

Teil der Fable-Vollprüfung Phase 1-3 vom 12.08.2026 ([[project_fable_vollpruefung_phase1-3_2026-08-12]]), Handlungsempfehlung Punkt 10 ("Monte-Carlo-Schwelle"). Levi hat diese Empfehlung exakt wie vorgeschlagen genehmigt. Dies ist eine reine Text-Klarstellung der bestehenden Methode oben — **kein Skript wird gebaut.** Der Build bleibt aufgeschoben; Trigger dafür steht in [[project_phase4_gates_2026-08-12]] (Abschnitt 5: nach dem #40-Review, ODER sobald #50 absehbar 2-3 Wochen entfernt ist — je nachdem, was zuerst eintritt).

**1. Gate-Metrik festgeschrieben — KORRIGIERT 24.08.2026 (C-4-Fix):** ~~Das reale €-Endergebnis (nicht der maximale Drawdown) ist die alleinige Gate-relevante Metrik der Simulation.~~ Diese ursprüngliche Festlegung war ein Bug: Bei fixer Positionsgröße (wie in Phase 3/4 durchgängig der Fall, siehe [[project_phase4_gates_2026-08-12]]) ist die Summe der `result_eur`-Werte kommutativ — ein Permutationstest ohne Zurücklegen ändert nur die Reihenfolge, nie die Summe. Das reale Endergebnis liegt damit in JEDER Permutation am selben (einzigen) Perzentil und kann nie in den unteren 20% landen — das Gate war ergebnisinvariant, konnte also nie fehlschlagen, unabhängig von der tatsächlichen Robustheit der Strategie. **Neu:** Der **maximale Drawdown** ist ab jetzt die alleinige Gate-relevante Metrik (variiert unter Permutation tatsächlich, je nachdem ob Verlust-Trades in der simulierten Reihenfolge gebündelt oder verteilt auftreten). Details/Begründung inkl. Bootstrap-Alternative-Ablehnung siehe [[project_phase4_gates_2026-08-12]] Abschnitt 8. Das reale €-Endergebnis bleibt weiterhin Teil der Ausgabe (diagnostisch), verliert aber seine Gate-Funktion — die übernimmt jetzt der Drawdown.

**2. 20%-Schwelle verankert (Cross-Link) — Richtung korrigiert 24.08.2026 (C-4-Fix):** [[project_phase4_gates_2026-08-12]] Abschnitt 3 (Gate 6) legt fest: der reale maximale Drawdown darf nicht in den oberen (schlechtesten) 20% der simulierten Drawdown-Verteilung liegen (vor dem C-4-Fix bezog sich diese Schwelle auf das €-Endergebnis und die untere 20%-Seite — siehe Korrektur in Punkt 1 oben, gleicher Prozentwert, andere Metrik und gespiegelte Richtung). Diese Zahl gilt ab jetzt als hier verankert, nicht nur dort — beide Dokumente tragen dieselbe Quelle. Schritt 3 oben ("Liegt das reale Ergebnis nahe am Median oder am Rand der Verteilung") wird durch diese konkrete Schwelle ersetzt/präzisiert: Rand = oberste (schlechteste) 20% des Drawdowns.

**3. Methode explizit benannt:** Das in Schritt 2 oben beschriebene "Trade-Reihenfolge tausendfach zufällig neu mischen" ist ein **Permutationstest ohne Zurücklegen** — feste Stichprobe der vorhandenen Trade-Ergebnisse, nur die Reihenfolge wird randomisiert. Es ist **kein** Bootstrap mit Zurücklegen und **kein** Block-Resampling. Caveat (Cross-Link auf denselben "blinden Fleck" in [[project_phase4_gates_2026-08-12]] Abschnitt 4, Punkt 1 und Punkt 3): Diese Methode testet **Sequenzrobustheit** (ob die Ergebnis-Reihenfolge zufällig günstig war) — **nicht Regime-Robustheit** (ob der Edge auch unter künftig anderen Marktbedingungen hält). Ein bestandener Permutationstest ist kein Beleg dafür, dass der Edge über Regimewechsel hinweg trägt.

**4. Datenbasis — KORRIGIERT 24.08.2026 (Opus-Vollcheck-Umsetzung Punkt 4, Befund C-4b, siehe [[project_opus_vollpruefung_2026-08-24]]):** ~~Das reale €-Ergebnis pro Trade aus `scripts/trades.db`, Spalte `result_eur`, ist die Datenbasis für Schritt 1 oben — nicht R-Multiples, nicht Prozentwerte (`result_pct` existiert als Spalte, wird hier bewusst nicht verwendet). Begründung: konsistent mit der bereits etablierten €-basierten Gate-Logik.~~ Diese ursprüngliche Festlegung war ein zweiter Fehler in derselben Methodik (neben dem bereits am 24.08. C-4-gefixten Gate-Metrik-Bug in Punkt 1 oben): Direktabfrage von `scripts/trades.db` (n=43) zeigt eine Positionsgrößen-Spanne von **399€ bis 5.020,85€ (Faktor 12,6)** über die Trade-Historie — Phase-1-Positionen (500-1.500€) stehen neben Phase-3-Positionen (4.000-5.000€) und dem aktuellen 15-Trade-Test-Fenster (2.000-2.500€, siehe [[project_risikomanagement]]). Ein €-Drawdown aus `result_eur` mischt damit unweigerlich die Skalierungshistorie der Positionsgröße mit der eigentlich zu prüfenden Sequenzrobustheit — ein simulierter Drawdown mit vielen Phase-3-Verlusten gebündelt sieht allein deshalb "schlimmer" aus als einer mit vielen Phase-1-Verlusten gebündelt, unabhängig von der tatsächlichen Reihenfolgen-Robustheit der Strategie selbst.

**Neu:** Datenbasis für Schritt 1 oben ist ab jetzt `result_pct` (Tabelle `trades`, Spalte `result_pct`, definiert in `scripts/trade_db.cjs`) — **Positions-Prozent** (`result_eur / position_eur × 100`, siehe C-1-Fix/Plausibilitätscheck in `scripts/add_trade.cjs`), NICHT Kapital-Prozent. Der maximale Drawdown (Gate-Metrik seit dem C-4-Fix, siehe Punkt 1 oben) wird entsprechend als kumulierte %-Kurve statt €-Kurve berechnet — normiert die Positionsgrößen-Historie heraus und misst tatsächlich Sequenzrobustheit (Reihenfolge der %-Ergebnisse), nicht die Skalierungsgeschichte des Kontos. Das reale €-Endergebnis (`result_eur`) bleibt weiterhin Teil der diagnostischen Ausgabe (z.B. für die 20%-Notbremse-Regel in [[project_risikomanagement]], die explizit €-basiert ist), verliert aber endgültig jede Rolle in der Monte-Carlo-Gate-Berechnung selbst.
