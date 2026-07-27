---
name: feedback-zeitzone
description: "User lebt in Deutschland, will alle Uhrzeiten in Ausgaben (Loop-Ticks, Session-Updates, Timestamps) in deutscher Ortszeit, nicht UTC"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: b9585154-6b19-4cde-b4d2-f0df64bb9338
  modified: 2026-07-27T12:55:05.089Z
---

Ab 22.07.2026: Alle Uhrzeiten in Claude-Ausgaben (Loop-Ticks, Voll-Checks, Session-Updates, Trade-Notizen) in deutscher Ortszeit angeben, nicht UTC.

**Why:** User lebt in Deutschland, UTC-Zeitstempel mussten bisher immer gedanklich umgerechnet werden (z.B. "16 Uhr DE" vs. "14:00 UTC" in denselben Nachrichten).

**Korrigiert 27.07.2026 (echter Fehler live aufgetreten):** Die alte Annahme unten ("`date` liefert immer UTC, manueller +2h-Offset nötig") ist NICHT mehr korrekt — Stand 27.07.2026 liefert der bare `date`-Befehl (ohne `-u`) bereits die korrekte DE-Ortszeit direkt (verifiziert gegen `date -u`: Differenz exakt +2h CEST, UND gegen die vom User live bestätigte tatsächliche Uhrzeit). Ursache der Diskrepanz zur alten Notiz unbekannt (evtl. Sandbox-Update, oder die alte Annahme war nie ganz korrekt) — **Konsequenz:** Ab sofort den bare `date`-Befehl (ohne `-u`, ohne manuellen Offset) direkt als DE-Ortszeit verwenden. NICHT mehr zusätzlich +2h/+1h manuell addieren — das führte am 27.07.2026 zu einem realen 2-Stunden-Fehler (16:44 statt korrekt 14:54 behauptet), vom User live korrigiert.

**How to apply (korrigiert):**
- `date` (Bash-Tool, ohne `-u`) direkt als DE-Ortszeit verwenden — kein manueller Offset mehr
- Vor jeder wichtigen zeitkritischen Aussage (Session-Start, Loop-Beginn) den Wert kurz gegen `date -u` plausibilisieren (Differenz sollte +2h in CEST/+1h in CET sein) — weicht das spürbar ab, aktiv beim User nachfragen statt stur der Rechnung zu vertrauen (siehe [[feedback_verify_dont_cave]])
- Interne Zeitstempel-Vergleiche (z.B. QQQ-Session-Gate, Bar-Timestamps aus `data_get_ohlcv`) bleiben technisch in UTC, da die Chart-APIs UTC liefern — nur die für den User sichtbare Anzeige ist die bereits lokal-korrekte `date`-Ausgabe
- Winterzeit-Umstellung (letzter Oktober-Sonntag CEST→CET): danach erneut gegen `date -u` verifizieren, ob die Differenz noch stimmt (+1h statt +2h)

~~Alte, überholte Annahme (bis 27.07.2026 fälschlich angewendet):~~ "Sandbox hat keine Zeitzonendatenbank, `date` liefert immer UTC, manueller Offset per `date -u -d '+2 hours'` nötig" — diese Annahme war Stand 27.07.2026 nicht mehr zutreffend und führte zu einem realen Fehler.
