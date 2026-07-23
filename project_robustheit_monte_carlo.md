---
name: project-robustheit-monte-carlo
description: "Offene Erinnerung: Monte-Carlo-Robustheitscheck der Trade-Historie durchführen, sobald genug Stichprobe vorhanden ist (Schwelle ~50 abgeschlossene Trades)"
metadata:
  node_type: memory
  type: project
  originSessionId: session-2026-07-13
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
