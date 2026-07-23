---
name: feedback-mtf-voll-check-wiederholt-vergessen
description: "1H-Bias im Voll-Check-MTF-Blick wird wiederholt vergessen (nur 15-Min gecheckt) — zweiter bekannter Vorfall nach Trade #18, jetzt aktiv als Checklisten-Punkt behandeln"
metadata:
  type: feedback
  originSessionId: session-2026-07-15
---

Der MTF-Blick im Voll-Check (siehe [[feedback_live_trading]] Punkt 3a) verlangt explizit **beide** Timeframes — 15-Min UND 1H. In der Praxis wird beim Voll-Check-Tool-Aufruf regelmäßig nur der 15-Min-Wechsel gemacht, der 1H-Wechsel fällt weg, ohne dass es auffällt.

**Zwei bekannte Vorfälle:**
1. Trade #18 (14.07.2026): "MTF-Voll-Check (1H/15min) war anfangs im Positions-Loop vergessen worden, musste vom User nachgefragt werden" (siehe [[trades/trading_2026-07-14]])
2. Trade #19 (15.07.2026): Im Positions-Monitoring-Loop wurden mehrere Voll-Checks (16:15, 16:20, 16:25 UTC) durchgeführt, jeweils nur mit 15-Min-Wechsel — der 1H-Wechsel fehlte komplett, bis der User aktiv nachfragte "Hast du beim Vollcheck 1h/15 min bias?"

**Why:** Zwei unabhängige Vorfälle im Abstand von einem Tag zeigen: die bestehende Regel (Punkt 3a) allein reicht nicht, um das Vergessen zu verhindern — vermutlich weil der 15-Min-Wechsel als "der MTF-Check" wahrgenommen wird und der zusätzliche 1H-Schritt beim schnellen Tool-Batching im Loop untergeht.

**How to apply:** Bei JEDEM Voll-Check-Tool-Aufruf explizit BEIDE Timeframe-Wechsel als festen Zwei-Schritt-Block behandeln, nicht als optionalen Zusatz: `chart_set_timeframe(15)` → Werte lesen → `chart_set_timeframe(60)` → Werte + letzte 5 Bars auf HH-HL/LH-LL-Struktur lesen → zurück auf 5. Beim Aufsetzen eines neuen Positions-Loops (CronCreate-Prompt) diesen Zwei-Schritt-Block explizit im Prompt-Text ausschreiben (nicht nur "MTF-Blick" als Stichwort), damit er bei jedem Fire mit ausgeführt wird. Cron-Job für Trade #19 wurde am 15.07. um 16:26 UTC entsprechend korrigiert.

**Verankert 16.07.2026:** Der Zwei-Schritt-Block steht jetzt als wörtlicher Standard-Textbaustein für jeden CronCreate-Prompt in [[feedback_live_trading]] Punkt 2b — dort nachschlagen statt in jeder Session neu formulieren.
