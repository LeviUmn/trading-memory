---
name: project-risikomanagement-dax
description: Risikomanagement-System für den eigenständigen DAX-Trading-Block (10-15 Uhr) — physisch getrennt von project_risikomanagement.md (NAS100)
metadata: 
  node_type: memory
  type: project
  originSessionId: 51fca406-b869-4659-8541-11810063992f
  modified: 2026-08-07T11:10:59.113Z
---

Eigenständiges Risikomanagement-Dokument für den DAX-Block, angelegt 06.08.2026 im Zug von [[project_dax_erweiterung]] (To-do 6). Bewusst **kein Anhang** an [[project_risikomanagement]] (NAS100) — physische Trennung analog zur DB/Log-Trennung, siehe [[feedback_dax_trennung]].

**Wichtiger Grundsatz (aus der Architektur-Entscheidung 05.08.2026):** Die Mechanik/Struktur ist 1:1 von NAS100 übernehmbar (Cooldown-Logik, Positionsgrößen-Formel, Workflow-Schritte). Die **inhaltlichen Schwellen** (ATR-Kalibrierung, Level-Verhalten, Chop-Definition) gelten hier NICHT automatisch — DAX-Marktverhalten ist unbewiesen, siehe [[feedback_dax_trennung]]. Alles unten als "übernommen" Markierte bezieht sich nur auf die Mechanik, nicht auf die konkreten Zahlen.

### Status: Vor-Kapital-Phase
**DAX-Live-Trading (echtes Kapital) startet erst nach Abschluss aller NAS100-Phasen (1-4)**, siehe [[project_dax_erweiterung]] Timing-Klarstellung. Bis dahin ist dieses Dokument ein lebendiges Gerüst, das parallel zum NAS100-Live-Trading mitwächst (Infrastruktur-Aufbau + Paper-Beobachtung), nicht bereits scharf.

### Kapital & Budget
**Noch nicht festgelegt (Levi-Entscheidung 06.08.2026):** Der konkrete €-Betrag für das DAX-Startbudget wurde bewusst auf den NAS100-Phase-4-Abschluss verschoben — zu dem Zeitpunkt existiert vermutlich bereits der gemeinsame 50.000€-Gesamtpool (siehe [[project_vision]] Skalierungs-Fahrplan), in den DAX dann direkt einsteigen könnte, statt vorher ein separates Zwischenbudget zu brauchen. Diese Zeile bei Erreichen von NAS100 Phase 4 aktiv neu bewerten.

### Eigene Phase-1-Logik
Analog zum NAS100-Start: **eigene Phase 1 von vorne** (kleine Positionen, Bewährungsfenster), trotz vorhandener NAS100-Erfahrung — Marktverhalten/Level/Setups auf DAX sind unbewiesen. Keine automatische Übernahme des NAS100-Phasenstands (aktuell Phase 3). Konkrete Positionsgrößen/Risiko-% für DAX-Phase 1 werden erst mit dem Startbudget (siehe oben) definiert — Struktur folgt dem NAS100-Muster (siehe [[project_risikomanagement]] Skalierungs-Fahrplan), aber als eigene, unabhängige Zählung.

### Chartsymbol
`IG:DAX` ("Germany 40 Cash") — Echtzeit + echtes Volumen in einem Symbol, live verifiziert 06.08.2026. Details/Testmethodik siehe [[project_dax_erweiterung]] To-do 1+2.

### Handelsfenster
10-15 Uhr DE-Zeit, eigener Ablauf-Trigger "start update dich dax" (umbenannt 07.08.2026, vorher "#1"; siehe [[project_dax_erweiterung]], To-do 9: `feedback_session_update_dax.md`).

### Overlap-Regel (Levi-Entscheidung 05.08.2026)
**DAX-Position wird vor Start von "start update dich" (NAS100) geschlossen.** Kein paralleles Management zweier offener Positionen/Loops gleichzeitig — bewusst die sicherere Variante, auch wenn das den Nasdaq-Einstieg situativ verzögern kann. Vor dem ersten echten DAX-Trade mit Kapital soll dieses Szenario einmal bewusst live beobachtet werden ([[project_dax_erweiterung]] To-do 14), um zu prüfen, wie gut "erst schließen, dann Nasdaq" in der Praxis funktioniert.

### Korrelationsregel
Bestehende Regel aus [[feedback_instrumenten_fokus]] bleibt vollständig gültig: **nie gleichzeitig NAS100 und DAX in dieselbe Richtung** — makro-, nicht zeitlich begründet (beide reagieren auf dieselben globalen Katalysatoren wie Fed-Politik, Risk-On/Risk-Off). Besonders relevant im 15-15:30-Uhr-Übergangsfenster, wenn beide Positionen kurzzeitig parallel offen sein könnten.

### Trades-pro-Tag-Limit
**Gemeinsame Obergrenze über DAX+NAS100 zusammen** (3 regulär, 4. Ausnahme, 5. nie) — keine getrennte Zählung pro Markt, siehe [[project_risikomanagement]] Tabelle "Trades-pro-Tag-Regel". Grund: Gesamt-Exposure/Entscheidungsdichte soll auf dem bisherigen Niveau bleiben, nicht durch ein zweites Instrument effektiv verdoppelt werden.

### Cooldown-Mechanik (übernommen, eigene Datenquelle)
`cooldown_check.cjs` mit `TRADE_LOG_PATH` auf `memory/dax_trades/trade_log_dax.md` gezeigt (siehe [[project_dax_erweiterung]] To-do 5) — identische Regeln wie NAS100 (Tages-Cooldown bei 2 Verlusten in Folge, rollierender Schutz bei 4/5 Verlusten), aber unabhängig gezählt von der NAS100-Historie.

### Noch offen / unverifiziert
- Positionsgrößen-Formel (siehe [[project_risikomanagement]] "Positionsgrößen-Formel") ist mechanisch übertragbar, aber DAX-Zertifikat-Konditionen (Scalable Capital) noch nicht geprüft
- ATR-Schwellen, ausreichende SL-Breite, Chop-Definition — alles NAS100-kalibriert, für DAX unverifiziert (siehe [[project_dax_erweiterung]] Fable-Review-Kernfunde)
- ~~Hebel-Staffelung nach VIX~~ — **entschieden 06.08.2026 (Levi-Go):** Beide Vola-Indizes parallel nutzen. `VDAX-NEW` (`INDEX:DV1X`, live verifiziert, Wert 16,30) als DAX-instrumentspezifischer Wert (Rolle analog VXN beim Nasdaq) **UND** US-`VIX` weiterhin als globaler Risk-On/Risk-Off-Kontextwert (Levi-Begründung: US-Stimmung wirkt auf beide Märkte, Korrelationsregel gilt ja genau deshalb). `VSTOXX` bleibt ausgeschlossen (kein sauberes Index-Symbol auf TradingView, nur gehebelte Future-Zertifikate). **Noch offen:** eigene Hebel-Staffelungs-Tabelle analog [[project_risikomanagement]] mit `VDAX-NEW`-Schwellen kalibrieren (erst mit echten DAX-Paper-/Live-Daten sinnvoll, nicht blind die NAS100-VIX-Schwellen übernehmen) — VIX bleibt dabei zusätzlicher Kontext, nicht Ersatz für die DAX-eigene Schwelle.
