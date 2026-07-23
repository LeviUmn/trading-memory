---
name: project-performance-ziele
description: "Levis Performance-Ziele (70% Win-Rate, 1-1,5% Ø-Rendite/Trade), festgelegt 16.07.2026 nach Trade #20 — Referenzpunkt für ehrlichen Abgleich bei künftigen Reviews"
metadata:
  type: project
  originSessionId: session-2026-07-16
  modified: 2026-07-23T09:56:25.999Z
---

Levi hat am 16.07.2026 (nach Trade #20, +51,46€/+2,58%) zwei persönliche Performance-Ziele genannt, begründet mit der Disziplin durch Regeln/Memory/MCP-Verbindung:
1. **Win-Rate ~70%**
2. **Durchschnittsrendite 1-1,5% pro Trade**

**Ist-Stand bei Festlegung (16.07.2026, nach Trade #20):**
- Win-Rate gesamt (20 Trades, BE ausgeklammert): 66,7% (12/18)
- Win-Rate Phase 2 allein (#16-20): 80% (4/5) — **Stichprobe zu klein für Aussagekraft**, siehe [[project_robustheit_monte_carlo]]
- Ø-Rendite/Trade gesamt (alle 20): ≈0,53%
- Ø-Rendite/Trade Phase 2 allein (#16-20): ≈0,97% — knapp unter dem 1-1,5%-Ziel, stark von Trade #20 (+2,58%) getragen (ohne #20 nur ≈0,42%)

**Why:** Levi interpretierte die aktuellen Phase-2-Zahlen (80% Win-Rate) tendenziell schon als Bestätigung des 70%-Ziels. Bei n=5 ist das statistisch noch kein Beleg — Ziel dieser Datei ist, künftige Selbsteinschätzungen an echten, wachsenden Stichproben zu verifizieren statt an einzelnen guten Trades, konsistent mit [[feedback_verify_dont_cave]].

**How to apply:** Bei jedem Backtest/Review (siehe [[feedback_backtest_ablauf]]) und spätestens bei der ~50-Trade-Marke (Monte-Carlo-Schwelle) diese beiden Ziele explizit gegen die dann aktuelle Win-Rate und Ø-Rendite/Trade gegenrechnen — ehrlich benennen, ob der Trend trägt oder ob einzelne Ausreißer-Trades den Schnitt verzerren, nicht unkritisch als "Ziel erreicht" werten, solange die Stichprobe klein ist.

**Update 23.07.2026 (Fable-Audit, nach Trade #24 — nachgeholt, war seit Trade #20 überfällig):**
- Win-Rate gesamt (24 Trades, BE ausgeklammert): 63,6% (14/22) — leicht gesunken ggü. 66,7% am 16.07.
- Win-Rate Phase 2 allein (#16-24, n=9, keine BE in diesem Fenster): 66,7% (6/9) — deutlich gesunken ggü. 80% (4/5) am 16.07.; die Stichprobe war damals mit n=5 explizit als zu klein markiert, n=9 ist immer noch klein, aber die 80% waren erwartungsgemäß nicht haltbar
- Ø-Rendite/Trade gesamt (alle 24, inkl. BE im Nenner, gleiche Methode wie bei Festlegung): ≈0,41% — gesunken ggü. ≈0,53% am 16.07.
- Ø-Rendite/Trade Phase 2 allein (#16-24, n=9): ≈0,46% — deutlich gesunken ggü. ≈0,97% am 16.07., und jetzt klar unter statt knapp unter dem 1-1,5%-Ziel

**Ehrliche Einordnung:** Die Lücke zum Ziel (70% / 1-1,5%) hat sich seit der Festlegung am 16.07. in beiden Kennzahlen **vergrößert, nicht verkleinert** — getrieben durch zwei Verlust-Trades in Phase 2 seit dem letzten Stand (#21 -1,04%, #24 -0,665%). Das ist kein Grund für Alarmismus (n=9 in Phase 2 bleibt eine kleine Stichprobe, siehe [[project_robustheit_monte_carlo]] — die 80%/0,97%-Zahlen vom 16.07. waren selbst schon als "zu klein für Aussagekraft" gekennzeichnet und sind jetzt näher am realistischeren Gesamtbild), aber die Entwicklung seit der Zielsetzung stützt das Ziel bisher nicht. Siehe [[feedback_realisiertes_rr]] (23.07.2026 nachgetragen, RR jetzt ≈1,23:1 statt 1,09:1) für den eigentlichen Hebel — die RR-Verbesserung ist real, aber noch nicht groß genug, um die Rendite-Kennzahl mitzuziehen.
