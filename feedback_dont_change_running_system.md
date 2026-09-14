---
name: feedback-dont-change-running-system
description: "Verbesserungen am laufenden Live-Trading-Setup (Symbole, Indikatoren, Kernmechanik) werden vermerkt, aber nicht sofort umgesetzt — Umsetzung erst an designierten Review-Checkpoints"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 51fca406-b869-4659-8541-11810063992f
  modified: 2026-09-07T21:26:16.884Z
---

Wenn während des laufenden Live-Tradings eine potenzielle Verbesserung am Kern-Setup auffällt (z.B. besseres Datensymbol, neue Datenquelle, Regel-Feinschliff) — auch wenn sie live verifiziert und plausibel ist — nicht sofort umsetzen. Stattdessen dokumentieren und auf den nächsten passenden Review-Checkpoint verschieben (z.B. Gesamt-Review nach Phase-Abschluss, mit Fable).

**Why:** Levi-Zitat 06.08.2026 zum `IG:NASDAQ`-vs-`QQQ`-Fund: "Aktuell heißt es: don't change a running system und das sollten wir weiter machen." Die bestehende NAS100-Kernmechanik (`FOREXCOM:NAS100`, ATR-Schwellen, Level-Logik) ist über Monate Trade für Trade kalibriert — ein Wechsel mitten im laufenden System riskiert, diese Kalibrierung zu entwerten, selbst wenn der Ersatz auf den ersten Blick überlegen wirkt. Gleiches Prinzip bereits einmal angewandt bei UnusualWhales (siehe [[project_vision]]): Integration bewusst auf "nach Phase 3, Gesamt-Review" verschoben statt sofort umgesetzt.

**How to apply:** Bei jedem während des Live-Tradings gefundenen Verbesserungspotenzial (egal wie gut verifiziert) zuerst fragen: ist das ein Notfall-Fix (Bug, der Trades verfälscht) oder eine Optimierung? Notfall-Fixes sofort umsetzen (z.B. [[feedback_size_script_short_bug]], [[feedback_draw_list_getchartapi_bug]]). Optimierungen dagegen als Vermerk im passenden Projekt-Memory ablegen (mit Kontext: was gefunden, warum sinnvoll, wann erneut aufgreifen) und NICHT von mir aus vorschlagen umzusetzen, bis der designierte Review-Zeitpunkt erreicht ist.

## Erneute Anwendung 07.09.2026 — Geschwindigkeit/Effektivität-Studien zurückgestellt

Levi-Zitat: "Ich möchte das erstmal nicht machen. Ich will erstmal, dass alles so weiter läuft wie bisher und sich erprobt, damit wir da sicher sein können, dass wir ein funktionierendes System haben." Nach dem gerade erst abgeschlossenen 4-Runden-Zyklus zur 04.09.-Auftragsliste (Commits `eb7caeb`/`c72a836`/`6a42fa5`) und den beiden unabhängigen Opus+Fable-Studien zu Geschwindigkeit/Effektivität (siehe [[project_studie_geschwindigkeit_effektivitaet_opus_2026-09-07]], [[project_studie_geschwindigkeit_effektivitaet_fable_2026-09-07]], gefiltert in [[project_todo_sicher_geschwindigkeit_effektivitaet_opus_2026-09-07]], gegengeprüft in [[project_gegencheck_todo_sicher_fable_2026-09-07]]) — **keiner der S1-S12-Punkte wird umgesetzt.** Das gerade neu geänderte System (SL-Anker-Vorprüfung, TP2-Pflichtfeld, 8c2-Zonenband etc.) soll erst laufen und sich bewähren, bevor weitere Änderungen draufkommen. Erneut aufgreifen: nach genügend weiteren Testtagen/Live-Trades, die das 07.09.-Regelwerk bestätigen — kein festes Datum, Levi entscheidet den nächsten Review-Zeitpunkt.
