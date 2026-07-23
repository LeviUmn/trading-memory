---
name: feedback-dax-trennung
description: "DAX-Beobachtungsnotizen (9-15 Uhr Fenster) müssen strikt getrennt von NAS100-Haupttrading bleiben, eigener Ordner memory/dax_beobachtung/"
metadata:
  node_type: memory
  type: feedback
  originSessionId: session-2026-07-10
---

DAX-Beobachtungen (9-15 Uhr DE-Zeit, vor dem Nasdaq-Fenster) gehören in den eigenen Ordner `memory/dax_beobachtung/` — NIEMALS in `memory/trades/` oder in Nasdaq-Statistiken/Trade-Log einmischen.

**Why:** User-Wunsch 10.07.2026, explizit: "Ich will auf keinen Fall, dass es verschmilzt mit unserem Haupttrading auf Nas100." Nasdaq bleibt laut [[feedback_instrumenten_fokus]] das Hauptinstrument und der Fokus liegt darauf, das System dort über alle Phasen/Reviews hinweg 100% funktionsfähig zu bekommen (User-Zitat 10.07.: "Nas100 ist unser Fav Indize und der Weltmarkt"). DAX ist reine Zukunftsvorbereitung, kein aktueller Bestandteil der Trading-Statistik.

**How to apply:**
- Trade-Log (`memory/trades/trade_log.md`) und alle Win-Rate-/Statistik-Berechnungen bleiben ausschließlich NAS100 — DAX-Beobachtungen fließen dort NICHT ein, auch nicht als "Paper Trade"-Zeile
- DAX-Notizen als eigene Dateien in `memory/dax_beobachtung/` ablegen (siehe [[dax_beobachtung/uebersicht]] als Einstiegspunkt)
- DAX ist aktuell (Stand 10.07.2026) noch nicht live — nur konzeptionell besprochen, kein Chart-Layout oder Beobachtungslauf gestartet. Erst auf explizites Signal des Users aktiv werden
- Review-Punkt für Live-Einstieg: nach ca. 10-15 weiteren NAS100-Trades, siehe [[feedback_instrumenten_fokus]] und [[dax_beobachtung/uebersicht]]
