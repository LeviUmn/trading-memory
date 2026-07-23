---
name: feedback-backtest-ablauf
description: "Trigger 'Lass uns den Backtest machen' → alle 12+ Trades rückwirkend mit aktueller vollständiger Checkliste neu bewerten, um neue Regeln zu validieren"
metadata: 
  node_type: memory
  type: project
  originSessionId: 028ce287-5d0c-400c-bf08-bdc1d4c4670c
---

**Trigger:** User sagt "Lass uns den Backtest machen" (oder sinngemäß "Backtest Zeit", "lass uns backtesten") → sofort folgenden Ablauf starten, ohne dass der Kontext nochmal komplett erklärt werden muss.

**Anlass (geplant am 02.07.2026 für 03.07.2026, US-Feiertag):** Nach einer Verlustserie (5 von 5 Trades #8-12 Verluste) wurden mehrere neue Regeln ergänzt (Chartmuster-Check 9d, Markt-Regime-Gate 8d, Schock-Tag-SL-Multiplikator 8c, zweistufiges Trigger-System 8a1, Freitags-Verstärkungs-Regel — alle in [[feedback_chartanalyse]]). Diese wurden noch nicht gegen historische Daten geprüft.

## Ablauf

1. **Memory lesen:** `trades/trade_log.md` (alle Trades kompakt) + alle `trades/trading_YYYY-MM-DD.md` Dateien (Details) + aktuellen Stand von [[feedback_chartanalyse]] (vollständige Checkliste)

2. **Pro Trade neu bewerten** (historische OHLCV-Bars vom jeweiligen Handelstag ziehen, falls nötig via `data_get_ohlcv` mit passendem Datumsbereich):
   - Chartmuster-Check (9d): War die Zielzone schon am selben Tag getestet worden? Double-Top/Bottom erkennbar?
   - Chop-Erkennung (Punkt 7): Hätte die Chop-Definition den Trade verhindert?
   - Markt-Regime-Gate (8d): War es ein Schock-Tag (2+ von 3 Kriterien erfüllt)? Hätte das Gate den Trade verhindert/verkleinert?
   - Zweistufiges Trigger-System (8a1): Falls relevant, hätte das den Entry-Zeitpunkt verändert?
   - Schock-Tag-SL-Multiplikator (8c): Hätte ein weiterer SL den Trade überlebt oder nur den Verlust vergrößert?

3. **Kernfrage pro neuer Regel:** Hätte sie die 5 Verlust-Trades (#8-12) verhindert/abgeschwächt? Hätte sie fälschlich einen der 7 Gewinn-Trades (#1-7) verhindert (False Positive)?

4. **Ergebnis pro Regel:** Behalten / anpassen (mit konkretem Vorschlag) / verwerfen — nicht nur pauschal "hat geholfen", sondern mit Beleg aus den Daten.

5. **Speichern:** Ergebnisse als neue Memory-Datei (z.B. `feedback_backtest_ergebnis_YYYY-MM-DD.md`), MEMORY.md Index aktualisieren, ggf. betroffene Regeln in [[feedback_chartanalyse]] direkt anpassen.

**Why:** Automatisiertes Backtesting via TradingView Strategy Tester (Pine Script) wäre wegen der vielen diskretionären Elemente (Chartmuster, Chop-Einschätzung) aufwändig — die manuelle Neubewertung der eigenen Trade-Historie mit der aktuellen Checkliste ist der pragmatische Kompromiss, um neue Regeln vor dem nächsten Live-Handelstag zu validieren.

## Overfitting-Schutz: Regeländerungen als Hypothese behandeln (ergänzt 13.07.2026)

Ausgangspunkt: User hat sich am Wochenende mit Robustheitsprüfung, Overfitting, Walk-Forward-Analyse, Out-of-Sample-Test und Monte-Carlo-Simulation beschäftigt und gefragt, was davon für die eigene Strategie relevant ist. Ergebnis der Einordnung: Diese Konzepte sind für codierte Strategien mit großen Stichproben gebaut — bei 16 diskretionären Trades (Stand 13.07.2026, siehe [[trades/trade_log]]) sind Walk-Forward-Optimierung und Monte-Carlo-Simulation im eigentlichen Sinn noch nicht sinnvoll anwendbar (siehe [[project_robustheit_monte_carlo]] für den Zeitpunkt, ab dem das nachgeholt wird). Der Overfitting-Gedanke selbst ist aber sofort relevant, siehe Regel unten.

1. **Regeländerungen aus einem Backtest-Review gelten als Hypothese, nicht als bewiesen.** Eine aus wenigen historischen Trades abgeleitete Regel (z.B. die NQ1!-EMA50-Gate-Rekalibrierung am 04.07.2026 nach nur 5 Verlust-Trades) hat sich erst dann als echte Verbesserung bestätigt, wenn sie über die nächsten ~10 Folge-Trades (out-of-sample, also NEUE Trades nach der Regeländerung, nicht die Trades, aus denen sie abgeleitet wurde) standhält.
2. **Keine Regel wird nach nur 1-2 Trades erneut geändert.** Das wäre Overfitting an Rauschen statt an echtem Muster. Der bestehende Phasenübergang-Rhythmus (Review alle ~15-16 Trades, siehe [[project_risikomanagement]]) dient dabei als informelles Walk-Forward-Fenster — Regeln werden dort gebündelt geprüft, nicht nach jedem einzelnen Ausreißer-Trade.

**Why:** Verhindert, dass eine Regel an eine kleine, verrauschte Stichprobe angepasst und fälschlich für "gelöst" gehalten wird (Gefahr bereits einmal real gewesen bei der NQ1!-Gate-Anpassung, siehe [[feedback_regime_wechsel]]) — ein einzelner Erfolgs- oder Verlust-Trade nach einer Regeländerung beweist nichts.

**How to apply:** Bei jedem Backtest-Review und jeder daraus folgenden Regeländerung explizit als "Hypothese, wird über die nächsten ~10 Trades validiert" kennzeichnen. Bei Trade-Nachbesprechungen nicht vorschnell an einer frischen Regel drehen, nur weil der erste oder zweite Folge-Trade nicht optimal lief.
