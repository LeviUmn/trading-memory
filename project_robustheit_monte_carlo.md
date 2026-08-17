---
name: project-robustheit-monte-carlo
description: "Offene Erinnerung: Monte-Carlo-Robustheitscheck der Trade-Historie durchführen, sobald genug Stichprobe vorhanden ist (Schwelle ~50 abgeschlossene Trades)"
metadata:
  node_type: memory
  type: project
  originSessionId: session-2026-07-13
  modified: 2026-08-12T11:02:21.665Z
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

**1. Gate-Metrik festgeschrieben:** Das reale €-Endergebnis (nicht der maximale Drawdown) ist die alleinige Gate-relevante Metrik der Simulation. Drawdown bleibt Teil der Simulation und wird weiterhin berechnet/ausgewiesen (Schritt 2-3 oben), ist aber rein diagnostisch/beschreibend — kein eigenständiges Pass/Fail-Kriterium.

**2. 20%-Schwelle verankert (Cross-Link):** [[project_phase4_gates_2026-08-12]] Abschnitt 3 (Gate 6) legt bereits fest: das reale Endergebnis darf nicht in den unteren 20% der simulierten Verteilung liegen. Diese Zahl gilt ab jetzt als hier verankert, nicht nur dort — beide Dokumente tragen dieselbe Quelle. Schritt 3 oben ("Liegt das reale Ergebnis nahe am Median oder am Rand der Verteilung") wird durch diese konkrete Schwelle ersetzt/präzisiert: Rand = untere 20%.

**3. Methode explizit benannt:** Das in Schritt 2 oben beschriebene "Trade-Reihenfolge tausendfach zufällig neu mischen" ist ein **Permutationstest ohne Zurücklegen** — feste Stichprobe der vorhandenen Trade-Ergebnisse, nur die Reihenfolge wird randomisiert. Es ist **kein** Bootstrap mit Zurücklegen und **kein** Block-Resampling. Caveat (Cross-Link auf denselben "blinden Fleck" in [[project_phase4_gates_2026-08-12]] Abschnitt 4, Punkt 1 und Punkt 3): Diese Methode testet **Sequenzrobustheit** (ob die Ergebnis-Reihenfolge zufällig günstig war) — **nicht Regime-Robustheit** (ob der Edge auch unter künftig anderen Marktbedingungen hält). Ein bestandener Permutationstest ist kein Beleg dafür, dass der Edge über Regimewechsel hinweg trägt.

**4. Datenbasis fixiert:** Das reale €-Ergebnis pro Trade aus `scripts/trades.db`, Spalte `result_eur` (Tabelle `trades`, definiert in `scripts/trade_db.cjs`), ist die Datenbasis für Schritt 1 oben — nicht R-Multiples, nicht Prozentwerte (`result_pct` existiert als Spalte, wird hier bewusst nicht verwendet). Begründung: konsistent mit der bereits etablierten €-basierten Gate-Logik — die RR/EV/WR-Gates in [[project_phase4_gates_2026-08-12]] Abschnitt 3 sind ebenfalls €- bzw. %-basiert auf derselben `trades.db`-Quelle.
