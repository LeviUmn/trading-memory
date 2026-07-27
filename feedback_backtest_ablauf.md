---
name: feedback-backtest-ablauf
description: "Trigger 'Lass uns den Backtest machen' → alle 12+ Trades rückwirkend mit aktueller vollständiger Checkliste neu bewerten, um neue Regeln zu validieren. Seit 27.07.2026: zweistufiges Framework — leichter DB-Delta-Check (Standard) vs. echter TradingView-Replay-Backtest (nur bei Reformen/Phasenübergängen/ungeklärten Verlusten)"
metadata: 
  node_type: memory
  type: project
  originSessionId: 028ce287-5d0c-400c-bf08-bdc1d4c4670c
  modified: 2026-07-27T12:42:18.602Z
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

## Systematisches Backtesting-Framework: DB-Delta-Check vs. echter Replay-Backtest (ergänzt 27.07.2026, Fable-Review, nach Bau der SQLite-Trade-DB)

Seit dem Bau der strukturierten Trade-DB (`scripts/trade_db.cjs`/`trade_stats.cjs`, siehe [[feedback_memory_pflege]]) und der Entdeckung, dass TradingViews Replay-Modus (`replay_start`/`replay_step`/`replay_autoplay`/`replay_trade`/`replay_status`/`replay_stop`) echte historische Kursdaten vorhält, gibt es zwei unterschiedlich aufwändige Backtest-Stufen — welche greift, hängt von der Größe der Regeländerung ab, nicht jede Änderung braucht die aufwändigere Stufe.

### Stufe 1 (Standard) — Leichter DB-Delta-Check
Für jede kleinere Regelpräzisierung (z.B. die Stall-Exit-1a-Ergänzung vom 27.07.2026): über `trade_stats.cjs`/gezielte Filter (Outcome, RR, `rule_violation`-Flag) gegen die bestehende Trade-DB prüfen, ob/wie die neue Regel das historische Ergebnis verändert hätte. Kein Replay nötig — die strukturierten Felder beantworten die Frage meistens ausreichend.

### Stufe 2 (Ausnahme) — Echter Replay-Backtest
Nur bei (a) einer **Reform**, die eine ganze Entscheidungslogik ersetzt (wie die TP1/TP2-RR-Entkopplung vom 27.07.2026), (b) einem **Phasenübergangs-Review** (ohnehin geplanter Auswertungszeitpunkt alle 10-15 Trades), oder (c) einem **ungeklärten Einzelverlust**, den die DB-Felder allein nicht eindeutig erklären. Analog zur bestehenden Fable-Zweitmeinungs-Regel oben (nur bei Regeländerung/großem Verlust/Statistik-Neuberechnung, nicht routinemäßig).

**Ablauf pro Trade (8 Schritte, inkl. Fable-Selbst-Check-Korrekturen vom 27.07.2026):**
1. Trade-Daten aus der DB ziehen (`entry_text`, `date`, `setup`, tatsächlicher `outcome`/`result_pct`).
2. **Historisches Indikator-/Pane-Layout rekonstruieren** (NEU, Fable-Selbst-Check): Das Indikator-Set hat sich über die Zeit verändert (QQQ-Pane erst seit 15.07.2026, ATR erst seit 23.07.2026, Pivot-Linien werden täglich neu gezeichnet statt dauerhaft gespeichert) — vor dem Replay klären, welches Setup zum historischen Zeitpunkt tatsächlich sichtbar war, sonst zeigt der heutige Replay ein Chart, das es damals so nicht gab.
3. `replay_start` mit Datum kurz VOR dem historischen Entry (genug Vorlauf für MTF-Bias-Bildung).
4. Mit `replay_step`/`replay_autoplay` zum tatsächlichen Entry-Zeitpunkt vorspulen.
5. Am Entry-Punkt: `data_get_ohlcv` + `data_get_study_values` + Screenshot ziehen — die NEUE Regel exakt so anwenden wie im Live-Loop.
6. Entscheidung treffen (genommen/verändert/ausgelassen), bei Bedarf `replay_trade` simulieren.
7. Mit `replay_step` bis SL/TP/Exit vorspulen, Ergebnis erfassen. **Bei Teilexit-Trades (TP1/TP2-Teilverkauf, Punkt 8e/11/12-Exits) den Vergleich als narrative Einschätzung führen, NICHT als exakten Zahlen-Fill-Abgleich** (Fable-Selbst-Check: `replay_trade` simuliert voraussichtlich nur volle Buy/Sell/Close-Aktionen, keine prozentualen Teilexits — ein numerischer Vergleich würde eine Präzision vortäuschen, die nicht besteht).
8. `replay_stop`, Ergebnis in `feedback_backtest_ergebnis_YYYY-MM-DD.md` protokollieren — **mit explizitem Hindsight-Bias-Hinweis** (NEU, Fable-Selbst-Check): Da der historische Ausgang beim Replay bereits aus der DB/den Tages-Dateien bekannt ist, ist die "neue" Entscheidung nie wirklich blind getroffen — das verzerrt besonders diskretionäre Einschätzungen (Chartmuster, Chop-Beurteilung) unbewusst Richtung des bekannten Ergebnisses. Jedes Replay-Protokoll muss diese Einschränkung explizit nennen, nicht nur implizit voraussetzen.

**Stichproben-Strategie statt Vollreplay — nie alle 25+ Trades:**
- **Bei einer Reform:** Nur die tatsächlich betroffenen Trades (z.B. bei der RR-Reform vom 27.07.2026 nur #17, #21, #23 — dokumentierte RR-Konflikte).
- **Bei Phasenübergängen:** Zufallsstichprobe von 3-5 Trades pro Phase statt Vollreplay.
- **Grundsatz:** Replay-Aufwand skaliert mit der Zahl der tatsächlich betroffenen Trades, nie mit der Gesamt-Trade-Zahl.

**Why:** Levi wollte nach dem Bau der Trade-DB und der Skill-Einschätzung des Systems (27.07.2026, siehe [[project_vision]]) ein systematisches Backtesting-Framework verankern. Ein reiner Kerzen-Replay aller historischen Trades bei jeder Regeländerung wäre unverhältnismäßig aufwändig (viele Tool-Calls pro Trade) — die zweistufige Lösung nutzt den leichten DB-Check für den Normalfall und reserviert den aufwändigeren, aber echten Replay-Backtest für die Fälle, in denen er wirklich etwas beweist. Fables eigener Selbst-Check deckte drei Schwachstellen im ersten Entwurf auf (Hindsight-Bias, historisches Indikator-Layout, Teilexit-Simulationsgrenze) — alle drei sind oben eingearbeitet, nicht nur als Randnotiz.
