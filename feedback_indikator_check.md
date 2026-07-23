---
name: feedback-indikator-check
description: "Vor Vertrauen in data_get_study_values/quote_get immer aktives Pane/Symbol prüfen (Vorfall 2 weiter gültig). Vorfall 1 (isFailed-EMA) am 16.07.2026 als MCP-Code-Bug root-gecaust und strukturell gefixt — Remove+Add-Workaround nach MCP-Neustart nicht mehr nötig"
metadata:
  node_type: memory
  type: feedback
  originSessionId: session-2026-07-06
---

Am 06.07.2026 traten zwei stille Fehlerquellen live auf, die beide unbemerkt zu falschen Trading-Entscheidungen hätten führen können.

**Vorfall 1 — Defekter Indikator (KORRIGIERTE DIAGNOSE, siehe Code-Fix unten):** Die EMA50-Studie zeigte auf dem Chart ein Warn-Icon (⚠️), lieferte aber überhaupt keine Fehlermeldung über die normalen Tools — `data_get_study_values` hat sie einfach stillschweigend übersprungen (weil ihr Wert `∅` war), nicht als Fehler gemeldet. Erst über `ui_evaluate` (direkter Zugriff auf `dataSources()` im Chart-Modell) ließ sich `isFailed: true` feststellen. Damals als mysteriöser TradingView-Glitch eingestuft und per Remove+Add „repariert" — das war eine Fehldiagnose: Die Ursache lag die ganze Zeit im eigenen MCP-Code (siehe unten), weshalb der Fehler nach jedem Neustart wiederkam.

**Vorfall 2 — Pane-Mixup:** Im 2-Pane-Setup (heute NAS100 + QQQ, damals NAS100 + NQ1!, siehe [[feedback_chart_layout]]) sprang der aktive Fokus unbemerkt von Pane 0 (NAS100) auf Pane 1 (damals NQ1! Futures) — `quote_get`/`data_get_study_values` ohne Symbol-Parameter griffen dann automatisch auf NQ1! zu (Kurs ~30.020 statt NAS100 ~29.800, ein Unterschied von ~200 Punkten durch den Futures-Fair-Value-Aufschlag). Wäre unbemerkt geblieben, hätte es zu einer Positionsgrößen-/Level-Berechnung auf Basis des falschen Instruments geführt.

**Why:** Beide Fehler sind für den User unsichtbar, wenn nur die Zahlen ohne Kontext gemeldet werden — ein `∅`-Wert wird stillschweigend weggelassen statt als Fehler markiert, und ein Pane-Wechsel ändert die zurückgegebenen Werte, ohne dass das Symbol-Feld standardmäßig geprüft wird.

**How to apply:**
1. Bei `quote_get`/`data_get_ohlcv`-Ergebnissen das `symbol`-Feld im Response aktiv gegencheck, besonders im 2-Pane-Setup — nicht blind annehmen, dass es noch NAS100 ist.
2. Wenn `data_get_study_values` weniger Studien zurückgibt als laut `chart_get_state` aktiv sein sollten (z.B. 3 statt 5), das als Hinweis auf eine defekte/leere Studie werten, nicht als normal.
3. ~~Bei Verdacht auf eine defekte Studie: remove + add~~ — nach dem Code-Fix vom 16.07.2026 (unten) sollte dieser Zustand gar nicht mehr entstehen: `chart_manage_indicator` verifiziert jetzt selbst die Studien-Gesundheit und meldet einen Fehler statt still eine kaputte Studie zu hinterlassen. Taucht trotzdem eine `isFailed`-Studie auf, ist das ein NEUER Bug und sollte untersucht statt nur weggeklickt werden.

## Struktureller Code-Fix (16.07.2026, Fable) — echte Ursache von Vorfall 1

**Root Cause im MCP-Code gefunden:** `chart_manage_indicator` (add) in `C:\Users\umnus\tradingview-mcp\src\core\chart.js` hat das `inputs`-Objekt (z.B. `{"length": 50}`) als `[{id, value}]`-Array direkt an `chart.createStudy(...)` übergeben. Der 4. Parameter von `createStudy` erwartet aber ein positionales Array roher WERTE — das falsch geformte Array korrumpiert die Pine-Kompilierung der Studie dauerhaft (`isFailed: true`, Status `{type: 3, errorDescription: "Can't parse pine"}`). Kein TradingView-Glitch, kein Race — deterministisch reproduzierbar bei jedem Add mit Inputs, daher das Wiederauftreten in jeder Session.

**Zweite Entdeckung — zwei interne Schemas pro Indikator-Name:** Derselbe Indikator existiert in zwei Varianten mit unterschiedlichen Input-IDs: per `createStudy` (also per MCP-Tool) erzeugte Studien sind Legacy-`tv-basicstudies` mit sprechenden IDs (`length`, `source`), per TradingView-UI-Suche hinzugefügte sind moderne Pine-Skripte (`Script$STD;EMA@tv-scripting`) mit positionalen IDs (`in_0`, `in_1`, …). Eine feste ID-Tabelle pro Indikator ist deshalb unmöglich. Nebenwirkung vorher: `indicator_set_inputs` mit nicht-passenden Keys war ein **stiller No-Op** (meldete Erfolg, änderte nichts — so blieb am 16.07. eine QQQ-EMA unbemerkt auf Länge 9 statt 50).

**Fix implementiert (3 Dateien):**
- Neu `src/core/study-inputs.js`: dynamischer Resolver — Friendly-Keys (`length`, `fast_length`, `stddev`, `anchor`, …) werden gegen das ECHTE Input-Schema der Ziel-Studie aufgelöst (exakte ID → Alias-Tabelle gegen Input-NAMEN → Namens-Match; RSI-Sonderfall „RSI Length" vs. Glättungs-„Length" abgedeckt). Alias-Tabelle live verifiziert (27/27 Fälle) gegen EMA (beide Varianten), RSI, MACD, Bollinger Bands, Pivot Points Standard, VWAP, Volume.
- `src/core/chart.js` (`manageIndicator` add): Studie wird IMMER ohne Inputs erzeugt, Inputs danach per `setInputValues` mit Resolver angewendet. Danach Health-Verifikation (Polling auf `hasError()`/`status()`, gleiches Verify-Muster wie der `pane_focus`-Fix in [[feedback_pane_sync_bug]]): bei `isFailed` einmaliger Clean-Retry, sonst Fehler + Aufräumen — **nie mehr `success: true` mit kaputter Studie**. Sind angeforderte Inputs nicht auflösbar, wird die Studie entfernt und ein Fehler mit der Liste verfügbarer Inputs gemeldet (statt still mit Default-Werten weiterzulaufen).
- `src/core/indicators.js` (`setInputs`): gleicher Resolver; wenn KEIN Key passt → Fehler mit verfügbaren Inputs statt stillem No-Op; bricht die Studie nach einer Wert-Änderung (auch falsche WERTE können Pine brechen), werden die vorherigen Werte automatisch wiederhergestellt.

**Status: Code-Fix LIVE E2E-VERIFIZIERT (16.07.2026)** — per direktem CDP-Zugriff mit dem neuen Code gegen das laufende TradingView (18/18 Checks: Add mit Inputs, Set-Inputs beide Schema-Varianten, Fehlerpfade inkl. Aufräumen, Endzustand beider Panes gesund, QQQ-EMA50 korrekt). **Die MCP-Tools selbst liefern das neue Verhalten erst nach Server-Neustart** (`/mcp` reconnect + danach zwingend `tv_launch` mit `kill_existing: false`, siehe [[feedback_pane_sync_bug]] „Wichtiger Nebenfund").

**Zusätzlich im echten 1-Min-Loop bestätigt (16.07.2026, 14:35-14:40 UTC):** Über 2 volle Voll-Checks (NAS100 + QQQ, je 15min/60min/5min) zeigte `data_get_study_values` durchgehend die vollständige Studienzahl (NAS100 4, QQQ 3) ohne einen einzigen `isFailed`/leeren Wert. Fix gilt als vollständig bestätigt — Vorfall 1 aus diesem Dokument ist erledigt, nur Vorfall 2 (Pane-Mixup, siehe [[feedback_pane_sync_bug]]) bleibt als generelle Vorsichtsregel gültig.
