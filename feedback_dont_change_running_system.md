---
name: feedback-dont-change-running-system
description: "Verbesserungen am laufenden Live-Trading-Setup (Symbole, Indikatoren, Kernmechanik) werden vermerkt, aber nicht sofort umgesetzt — Umsetzung erst an designierten Review-Checkpoints"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 51fca406-b869-4659-8541-11810063992f
  modified: 2026-08-06T09:40:36.896Z
---

Wenn während des laufenden Live-Tradings eine potenzielle Verbesserung am Kern-Setup auffällt (z.B. besseres Datensymbol, neue Datenquelle, Regel-Feinschliff) — auch wenn sie live verifiziert und plausibel ist — nicht sofort umsetzen. Stattdessen dokumentieren und auf den nächsten passenden Review-Checkpoint verschieben (z.B. Gesamt-Review nach Phase-Abschluss, mit Fable).

**Why:** Levi-Zitat 06.08.2026 zum `IG:NASDAQ`-vs-`QQQ`-Fund: "Aktuell heißt es: don't change a running system und das sollten wir weiter machen." Die bestehende NAS100-Kernmechanik (`FOREXCOM:NAS100`, ATR-Schwellen, Level-Logik) ist über Monate Trade für Trade kalibriert — ein Wechsel mitten im laufenden System riskiert, diese Kalibrierung zu entwerten, selbst wenn der Ersatz auf den ersten Blick überlegen wirkt. Gleiches Prinzip bereits einmal angewandt bei UnusualWhales (siehe [[project_vision]]): Integration bewusst auf "nach Phase 3, Gesamt-Review" verschoben statt sofort umgesetzt.

**How to apply:** Bei jedem während des Live-Tradings gefundenen Verbesserungspotenzial (egal wie gut verifiziert) zuerst fragen: ist das ein Notfall-Fix (Bug, der Trades verfälscht) oder eine Optimierung? Notfall-Fixes sofort umsetzen (z.B. [[feedback_size_script_short_bug]], [[feedback_draw_list_getchartapi_bug]]). Optimierungen dagegen als Vermerk im passenden Projekt-Memory ablegen (mit Kontext: was gefunden, warum sinnvoll, wann erneut aufgreifen) und NICHT von mir aus vorschlagen umzusetzen, bis der designierte Review-Zeitpunkt erreicht ist.
