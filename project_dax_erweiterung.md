---
name: dax-erweiterung
description: "Ausbau DAX von reiner Papier-Beobachtung zu eigenständigem Vormittags-Trading-Block (10-15 Uhr), parallel zu NAS100 (15:30-22 Uhr) — Entscheidungen, Architektur, To-do-Liste"
metadata:
  type: project
  originSessionId: session-2026-08-05
  modified: 2026-08-05T12:41:28.218Z
---

Levi will das System am 05.08.2026 um einen eigenständigen DAX-Trading-Block erweitern: **10-15 Uhr DAX** (eigenes "start update dich #1"), danach **ab 15 Uhr NAS100** wie bisher (wird zu "start update dich #2"). Das ist ein Strategiewechsel gegenüber der bisherigen Festlegung in [[feedback_instrumenten_fokus]] (DAX bisher nur situatives Ergänzungsinstrument, nicht paralleler Tagesblock mit eigenem Kapital) — bewusst von Levi entschieden, nicht implizit übernommen.

**Why:** Levi-Zitat 05.08.2026: "Ziel für mich langfristig ist, dass wir ab 10 Uhr morgens bereits auf den Dax traden, genauso simultan wie wir es auf dem Nasdaq machen." Begründung für die Datenerweiterung: "Die Daten die wir täglich ziehen sind ja schon bereits wichtig auch für den Dax, weil USA regiert die Märkte weltweit" — US-Makrodaten bleiben relevant, EZB/deutsche Daten kommen als DAX-spezifische Ergänzung hinzu.

## Fable-Review (05.08.2026) — Kernfunde

Volles Review lief als Hintergrund-Agent, hier nur die Punkte mit Konsequenz für die Umsetzung:

- **Strukturelle Lücke entdeckt, nicht nur inhaltliche:** `scripts/trade_db.cjs` hat keine `symbol`/`instrument`-Spalte im Schema — die Tabelle kann zwei Instrumente nicht sauber unterscheiden. `scripts/cooldown_check.cjs` hat `LOG_PATH` hart auf `memory/trades/trade_log.md` verdrahtet (nur per Env-Var `TRADE_LOG_PATH` überschreibbar). Bisherige DAX/NAS100-Trennung war reine Disziplin (Levi hat DAX-Notizen bewusst in `dax_beobachtung/` gehalten), nicht strukturell erzwungen — das reicht laut Fable nicht mehr, sobald echtes DAX-Kapital dazukommt. Bezug zu bekannten Vorfällen: genau dieser Fehlertyp (Regel lebt nur im Memory, bricht unter Zeitdruck) verursachte bereits den Cooldown-Bruch bei Trade #12 und den size.cjs-Short-SL-Bug.
- **DAX-Volumen-Proxy ist unverifiziert:** Ob `XETR:DAX` (echter Xetra-Kassaindex-Feed) dasselbe Volumen-Problem hat wie `FOREXCOM:NAS100` (CFD, nachweislich kein Volumen) ist NICHT geprüft — vor jeder Planung eines QQQ-Äquivalents für DAX erst live testen (`data_get_study_values` mit Volume-Indikator auf DAX).
- **Loop-Mechanik (CronCreate, Modulo-5-Weiche, 5-Tool-Sequenz) ist 1:1 übertragbar** — symbol-agnostische Tooling-Infrastruktur. **Inhaltliche Kalibrierung ist es NICHT:** ATR-Schwellen/Rausch-Kerzen-Mindestdistanz, Chop-Zeitfenster (15:30-16:00-Regel) und die ganze Entscheidungsbaum-/Stall-Logik wurden über Monate speziell an NAS100-Verhalten nachgeschärft (z.B. ATR-Wahl nach Trade #21). Für DAX gilt das erstmal als unverifizierte Annahme, nicht als übernommene Wahrheit.
- **Chart-Layout-Engpass:** 5-Indikatoren-Limit/Chart gilt weiterhin. DAX-Block braucht ein eigenes, separat gespeichertes Layout (analog NAS100+QQQ-2v-Split), das per `layout_switch` beim 15-Uhr-Übergang gewechselt wird — kein Live-Umbau des bestehenden Layouts (fehleranfällig, kostet Zeit genau im Übergangsmoment).
- **EZB-Daten über FRED sind Sekundärquelle:** FRED führt Euroraum-Serien (ECB-Sätze, HICP), aber verzögert gegenüber EZB SDW/eurostat direkt. Für den strukturellen Hintergrund-Zweck (wie FRED es für USA auch nur ist) als pragmatischer erster Schritt ausreichend, aber keine gleichwertige Primärquelle — bei Bedarf später auf echte EZB-Quelle upgraden.

## Levis Entscheidungen (05.08.2026)

| Frage | Entscheidung |
|---|---|
| Kapitalbudget | DAX bekommt vorerst ein **eigenes, separates Budget** (Betrag noch festzulegen). Sobald NAS100 Phase 4 abgeschlossen ist, verschmilzt das zu einem **gemeinsamen 50.000€-Gesamtpool für alle Märkte** (DAX + NAS100) — das war schon länger als Zielbild für NAS100 allein geplant (siehe [[project_risikomanagement]] Skalierungs-Fahrplan), jetzt explizit auf DAX erweitert. Bis dahin läuft DAX NICHT im bestehenden 15.000€-NAS100-Pool mit. |
| Phasenlogik | **Eigene Phase 1 von vorne**, analog zum NAS100-Start (kleine Positionen, Bewährungsfenster) — trotz vorhandener Erfahrung/Regelwerk, weil Marktverhalten/Level/Setups auf DAX unbewiesen sind. Keine automatische Übernahme des NAS100-Phasenstands. |
| Overlap 15 Uhr | **DAX-Position wird vor Start von "start update dich #2" geschlossen.** Kein paralleles Management zweier offener Positionen/Loops gleichzeitig — bewusst die sicherere Variante, auch wenn das den Nasdaq-Einstieg situativ verzögern kann. |
| Korrelationsregel | Die bestehende Regel aus [[feedback_instrumenten_fokus]] ("nie gleichzeitig NAS100 und DAX in dieselbe Richtung") **bleibt vollständig gültig** — war makro-, nicht zeitlich begründet (beide reagieren auf dieselben globalen Katalysatoren), besonders relevant im 15-15:30-Uhr-Übergangsfenster. |
| Trades/Tag-Limit | **Gemeinsame Obergrenze** über DAX+NAS100 zusammen (3 regulär, 4. Ausnahme, 5. nie) — keine getrennte Zählung pro Markt, um die Gesamt-Exposure/Entscheidungsdichte auf dem bisherigen Niveau zu halten. |

## Architektur-Entscheidung: strukturelle statt disziplinbasierte Trennung

Nach Fables Empfehlung — physische Dateitrennung statt gemeinsamer Tabelle mit Instrument-Spalte-Filter (zu fehleranfällig):

- Eigene DB: `scripts/trades_dax.db` + eigenes Modul `trade_db_dax.cjs` (Kopie der Struktur aus `trade_db.cjs`)
- Eigenes Trade-Log: `memory/dax_trades/trade_log_dax.md`, `cooldown_check.cjs` für DAX über die bereits vorhandene `TRADE_LOG_PATH`-Env-Var auf diese Datei zeigen lassen
- Neuer Ordner `memory/dax_trades/` für echte DAX-Trades (ersetzt/ergänzt `dax_beobachtung/`, das explizit als "Papier, kein Kapital" benannt war — bei echtem Kapital ist der alte Name irreführend)
- Eigenständiges `project_risikomanagement_dax.md` (kein Anhang ans bestehende Dokument)
- Eigenständiges `feedback_session_update_dax.md` mit eigenem Trigger "start update dich #1"
- Eigenständiges `feedback_live_trading_dax.md` — Mechanik-Teile aus [[feedback_live_trading]] übernehmen, inhaltliche Schwellen explizit als "unverifiziert, in Bewährung" markieren
- `add_trade.cjs`/`trade_stats.cjs`/`migrate_trade_log.cjs`: eigene DAX-Pendants statt Parameter-Flag (robuster gegen Vergessen bei dieser Codebasis-Größe)

Siehe [[feedback_dax_trennung]] für die fortgeschriebene Trennungsregel.

## Timing-Klarstellung (05.08.2026, direkt im Anschluss)

**DAX-Live-Trading (echtes Kapital) startet erst, wenn NAS100 alle Phasen (1-4) durchlaufen hat.** Bis dahin läuft nur der Infrastruktur-Aufbau (To-do-Liste unten) parallel zum normalen NAS100-Live-Trading — kein DAX-Kapitaleinsatz vor Abschluss von NAS100 Phase 4. Levi-Zitat: "Da müssen wir ja erstmal nachhaltig durch die Phasen [beim Nasdaq]. Das andere bauen wir dann parallel auf und starten dann mit dem Dax Trading erst, wenn wir beim Nasdaq durch alle Phasen sind."

**Konsequenz für die Kapitalbudget-Zeile oben:** Die Frage nach einem separaten DAX-Startbudget (To-do 3) entschärft sich dadurch — DAX startet ohnehin erst zeitlich nach NAS100 Phase 4, also potenziell direkt in der Nähe des ohnehin geplanten gemeinsamen 50.000€-Gesamtpools, statt vorher ein eigenes Zwischenbudget zu brauchen. Bei Erreichen von NAS100 Phase 4 erneut prüfen, ob überhaupt noch ein separates Vorab-Budget nötig ist oder ob DAX direkt in den dann vorhandenen Gesamtpool startet (mit eigener Phase-1-Positionsgröße gemäß der bereits getroffenen Entscheidung).

**Was bis dahin weiterläuft:** Infrastruktur-Aufbau (To-do-Liste, technische Trennung, Datenquellen, Chart-Layout) UND fortgesetzte Paper-Beobachtung in `dax_beobachtung/` zur Schwellen-Kalibrierung (To-do 13) — beides parallel zum normalen NAS100-Trading, ohne den NAS100-Fokus zu verwässern.

## To-do-Liste (priorisiert)

**Zuerst (vor jeder Code-Änderung):**
1. Live-Check: Hat `XETR:DAX` echtes Volumen? (`data_get_study_values` mit Volume-Indikator testen)
2. Auswahl DAX-Volumen-Proxy-Instrument (ETF/Future) basierend auf Test-Ergebnis 1
3. Konkreten €-Betrag für das separate DAX-Startbudget festlegen

**Technisch/strukturell:**
4. `trades_dax.db` + `trade_db_dax.cjs` anlegen
5. `memory/dax_trades/` Ordner + `trade_log_dax.md` anlegen, `cooldown_check.cjs` DAX-Aufruf mit `TRADE_LOG_PATH` einrichten
6. `project_risikomanagement_dax.md` aufsetzen (Budget, eigene Phase-1-Logik, gemeinsames Trades/Tag-Limit, Overlap-Regel, Korrelationsregel referenzieren)
7. EZB/Euroraum-Datenquelle: `fred_snapshot.py` um Euroraum-FRED-Serien erweitern (pragmatischer erster Schritt)
8. Deutsche/EU-relevante X-Accounts recherchieren (analog Rollentrennung Breaking/Sentiment/Risk-off), `x_watchlist_dax.json` anlegen
9. `feedback_session_update_dax.md` schreiben: eigener 6-Schritt-Ablauf, Trigger "start update dich #1", EZB-Daten + deutsche Tweets + DAX-Kalender (ZEW, Ifo, deutsche Industrieproduktion — bisher nirgends erfasst)
10. DAX-Chart-Layout einrichten: eigenes gespeichertes 2v-Layout (DAX + Volumen-Proxy), Indikator-Set neu evaluieren
11. `feedback_live_trading_dax.md` schreiben (Mechanik 1:1, Schwellen als unverifiziert markiert)
12. `feedback_session_update.md` (NAS100) um "#2"-Kennzeichnung ergänzen für konsistente Trigger-Benennung

**Vor dem ersten echten DAX-Trade mit Kapital:**
13. Mehrere neue Paper-Beobachtungssessions im DAX-Fenster (Fortsetzung von [[dax_beobachtung/uebersicht]], ruht seit 10.07.2026) — Schwellen gegen echtes Marktverhalten kalibrieren
14. Overlap-Szenario einmal bewusst live beobachten (DAX-Position noch offen bei 15 Uhr) — prüfen, wie gut "erst schließen, dann Nasdaq" in der Praxis funktioniert

**Status:** To-do-Liste steht, noch nicht abgearbeitet. Nächster Schritt laut Liste: Punkt 1 (Live-Volumen-Check auf XETR:DAX).
