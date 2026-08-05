---
name: feedback-dax-trennung
description: "DAX (Beobachtung UND ab 05.08.2026 geplantes echtes Trading) muss STRUKTURELL von NAS100 getrennt bleiben — eigene Ordner/DB/Dokumente, nicht nur Disziplin"
metadata:
  node_type: memory
  type: feedback
  originSessionId: session-2026-07-10
  modified: 2026-08-05T11:58:42.863Z
---

DAX-Wissen (Beobachtungen UND, seit 05.08.2026 geplant, echtes Trading 10-15 Uhr) gehört strikt getrennt von NAS100-Haupttrading — eigene Ordner, eigene Dateien, eigene DB. NIEMALS in `memory/trades/`, `trade_log.md` oder `trades.db` (NAS100) einmischen.

**Why:** User-Wunsch 10.07.2026, explizit: "Ich will auf keinen Fall, dass es verschmilzt mit unserem Haupttrading auf Nas100." Bestätigt und verschärft am 05.08.2026, als DAX von reiner Papier-Beobachtung zum eigenständigen Trading-Block wurde — siehe [[project_dax_erweiterung]] für den vollen Plan.

**Verschärfung 05.08.2026 (Fable-Review):** Die bisherige Trennung war reine Disziplin (Levi hat DAX-Notizen bewusst in einen eigenen Ordner gelegt), aber NICHT strukturell erzwungen — `scripts/trade_db.cjs` hat keine `symbol`/`instrument`-Spalte, `scripts/cooldown_check.cjs` liest hart verdrahtet aus `memory/trades/trade_log.md`. Solange DAX nur Papier war, war das folgenlos. Sobald echtes DAX-Kapital dazukommt, reicht Disziplin nicht mehr — genau der Fehlertyp (Regel lebt nur im Memory, bricht unter Zeitdruck), der bereits den Cooldown-Bruch bei Trade #12 und den size.cjs-Bug verursachte.

**How to apply:**
- NAS100 bleibt exklusiv: `memory/trades/`, `trade_log.md`, `scripts/trades.db`/`trade_db.cjs`, `project_risikomanagement.md`, `feedback_session_update.md` ("start update dich #2"), `feedback_live_trading.md`
- DAX bekommt eigene, physisch getrennte Pendants (siehe [[project_dax_erweiterung]] Architektur-Abschnitt für den vollen Aufbau): `memory/dax_trades/`, eigenes `trade_log_dax.md`, eigene `trades_dax.db`/`trade_db_dax.cjs`, `project_risikomanagement_dax.md`, `feedback_session_update_dax.md` ("start update dich #1"), `feedback_live_trading_dax.md`
- Alte Beobachtungsnotizen bleiben in `memory/dax_beobachtung/` (siehe [[dax_beobachtung/uebersicht]]) — der Ordner wird für neue Paper-Sessions vor dem Live-Start weiterverwendet, aber sobald echtes Kapital fließt, gehören diese Trades in `memory/dax_trades/`, nicht in denselben Ordner
- Bei jeder Session aktiv den passenden Ordner/die passende Datei je nach gerade gehandeltem Markt wählen — nie NAS100-Regeln/-Zahlen für DAX-Entscheidungen heranziehen oder umgekehrt
