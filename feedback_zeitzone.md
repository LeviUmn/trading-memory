---
name: feedback-zeitzone
description: "User lebt in Deutschland, will alle Uhrzeiten in Ausgaben (Loop-Ticks, Session-Updates, Timestamps) in deutscher Ortszeit, nicht UTC"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: b9585154-6b19-4cde-b4d2-f0df64bb9338
  modified: 2026-07-22T13:39:53.905Z
---

Ab 22.07.2026: Alle Uhrzeiten in Claude-Ausgaben (Loop-Ticks, Voll-Checks, Session-Updates, Trade-Notizen) in deutscher Ortszeit angeben, nicht UTC.

**Why:** User lebt in Deutschland, UTC-Zeitstempel mussten bisher immer gedanklich umgerechnet werden (z.B. "16 Uhr DE" vs. "14:00 UTC" in denselben Nachrichten).

**Technisches Problem:** Die Sandbox (Git Bash unter Windows) hat keine Zeitzonendatenbank (`/usr/share/zoneinfo` existiert nicht) — `TZ=Europe/Berlin date` hat daher keine Wirkung und liefert weiterhin UTC. Workaround: manueller Offset per `date -u -d '+2 hours' +%Y-%m-%dT%H:%M:%S` (Sommerzeit CEST = UTC+2).

**How to apply:**
- Sommerzeit (CEST, ca. letzter Sonntag März bis letzter Sonntag Oktober): +2h auf UTC
- Winterzeit (CET, Rest des Jahres): +1h auf UTC — Offset beim nächsten Zeitumstellungs-Fenster (letzter Oktober-Sonntag) korrigieren, nicht vergessen
- Bei jeder Zeitangabe in Live-Trading-Loops (siehe [[feedback_live_trading]] Punkt 9a) den deutschen Ortszeit-Wert verwenden statt der rohen `date -u`-Ausgabe
- Interne Zeitstempel-Vergleiche (z.B. QQQ-Session-Gate, Bar-Timestamps aus `data_get_ohlcv`) bleiben technisch in UTC, da die Chart-APIs UTC liefern — nur die für den User sichtbare Anzeige wird umgerechnet
