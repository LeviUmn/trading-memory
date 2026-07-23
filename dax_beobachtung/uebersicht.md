---
name: dax-beobachtung-uebersicht
description: "Übersicht/Einstiegspunkt für DAX-Beobachtungsnotizen (9-15 Uhr Fenster) — kein Live-Trading, getrennt von NAS100"
metadata:
  node_type: memory
  type: project
  originSessionId: session-2026-07-10
---

## Zweck

Passives Beobachtungs-System für DAX (XETR:DAX) im Zeitfenster 9-15 Uhr DE-Zeit, VOR dem Nasdaq-Handelsfenster (15:30-22 Uhr). Kein Kapitaleinsatz, keine echten Trades — nur Muster-/Level-Beobachtung und "Gedanken-Setups" ("hier hätte ich Long/Short genommen, warum").

**Warum eigener Ordner:** User-Wunsch 10.07.2026, explizit strikt getrennt von `memory/trades/` (echte Nasdaq-Trades) zu halten — siehe [[feedback_dax_trennung]] für die volle Begründung und Regeln.

## Status
Seit 10.07.2026 ~11 Uhr aktiv: Chart auf `XETR:DAX` (5-Min, gleiches Indikator-Set wie NAS100) umgeschaltet, erste Beobachtung läuft — siehe [[dax_beobachtung/notiz_2026-07-10]]. Reine Paper-Beobachtung im 9-15-Uhr-Fenster, kein echtes Kapital.

## Nächste Schritte (wenn User grünes Licht gibt)
1. Chart-Layout auf `XETR:DAX` mit identischem Indikator-Set anlegen
2. Erste Beobachtungsnotizen als `notiz_YYYY-MM-DD.md` in diesem Ordner ablegen (gleiche Struktur wie `memory/trades/trading_YYYY-MM-DD.md`, aber klar als "Beobachtung, kein echter Trade" markiert)
3. Review-Punkt: nach ca. 10-15 weiteren NAS100-Trades (siehe [[feedback_instrumenten_fokus]]) neu bewerten, ob DAX live dazukommt
