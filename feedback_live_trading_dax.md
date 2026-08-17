---
name: feedback-live-trading-dax
description: "DAX-Pendant zu feedback_live_trading.md (To-do 11) — Mechanik per Referenz übernommen, DAX-spezifische Strukturabweichungen (kein Dual-Gate, eigenes Handelsfenster, eigene Skripte) explizit benannt, alle Zahlen-Schwellen als unverifiziert markiert"
metadata:
  node_type: memory
  type: feedback
  originSessionId: session-2026-08-07
  modified: 2026-08-07T11:11:18.654Z
---

## Grundprinzip

Dieses Dokument ist bewusst KEINE Volltext-Kopie von [[feedback_live_trading]] (515 Zeilen, wird laufend weiterentwickelt — allein am 07.08. kamen neue Ergänzungen dazu) — eine Kopie wäre sofort veraltet und unwartbar. Stattdessen bleibt [[feedback_live_trading]] die kanonische Quelle für die MECHANIK (Loop-Cadence, Tool-Batching-Regeln, Ausgabeformate, Entscheidungsbaum-Format, Positions-Kasten-Format, Stall-Exit-Logik-Struktur). Hier stehen nur: (a) ein Verweis, welche NAS100-Punkte 1:1 mechanisch gelten, (b) die DAX-spezifischen Strukturabweichungen, (c) die explizite Kennzeichnung, welche Zahlen unverifiziert sind.

**Grundsatz aus [[project_dax_erweiterung]] (Architektur-Entscheidung 05.08.2026):** Mechanik ist übernehmbar, Zahlen sind es nicht. Bei jedem Live-Einsatz aktiv gegenchecken, nicht blind übernehmen.

---

## 1. Mechanik 1:1 aus [[feedback_live_trading]] übernommen (unverändert gültig für DAX)

Ohne inhaltliche Änderung gültig — für den vollen Wortlaut siehe dort:
- Punkt 1 (Fast Mode), 2 (kompaktes Signal-Format), 2a/2b (CronCreate-Loop, 1-Min-Cadence, Pflicht-Baustein-Prinzip im CronCreate-Prompt)
- Punkt 3 (Tool-Sequenz pro Check-in: `data_get_ohlcv` + `data_get_study_values`, kein `quote_get`)
- Punkt 3b (Screenshot nur bei vollständigen Checkliste-Durchläufen, nicht bei jedem Tick)
- Punkt 4a (Kommunikation während aktivem Loop: nur Warte-Status/Go-Signal/Kipp-Info, kein Fließtext)
- Punkt 5 (Memory während aktiver Session nicht bei jedem Check-in neu lesen)
- Punkt 6 (kompaktes Format gilt auch ohne /fast)
- Punkt 7 (Entscheidungsbaum-Format), 7d0 (Pflichtzeile "Kerze geschlossen: JA/NEIN"), 7d (Kerzenfarbe/Schlusstyp bei jedem Trigger explizit benennen)
- Punkt 7a (Indikatoren proaktiv "freigeben", Kurs bleibt Users Job)
- Punkt 7c, alle Erweiterungen bis auf den QQQ-Teil (MACD-Momentum/RSI-Annäherung/BB-Squeeze als Vorwarnung, parabolische-Bewegungen-Erweiterung, aktive Positions-Verteidigung bei anhaltendem Warnsignal) — MECHANIK gilt vollständig, Zahlenschwellen unverifiziert, siehe Abschnitt 3
- Punkt 8 (Positions-Kasten-Format inkl. Zertifikatspreis-Näherungsformel — die Ratio-Berechnung `Hebel × Entry-Zert.-Preis / Entry-Kurs` ist instrumentunabhängig, gilt 1:1)
- Punkt 9 (Voll-Check-Rhythmus: 4 Trigger-Zeitpunkte, Offenlegungspflicht was geprüft/ausgelassen wurde), 9a (echte Systemzeit per `Bash date` statt Selbstzählung)
- Punkt 10 (Auto Mode für Tool-Calls, echte Order-Ausführung bleibt manuell)
- Punkt 12 gesamt inkl. 12.1a/12.2/12.3/12.4 (Stall-Exit-STRUKTUR: Vorrangklausel vs. 9d1-Fortsetzungsmuster, Teilgewinn statt Vollexit, Rekursions-Logik auf Restposition, Pflicht-Ausgabezeile "Stall-Check") — Zahlen unverifiziert, siehe Abschnitt 3
- Punkt 13 (Chasing-Situation → halbierte Position, kein separates Konsolidierungs-Add) — MECHANIK gilt, der QQQ-Teil der 13.1-Bedingung entfällt (siehe Abschnitt 2.1)

---

## 2. DAX-spezifische Strukturabweichungen

### 2.1 Kein Dual-Gate, kein zweites Instrument — Punkt 7b/7e entfallen ersatzlos
`IG:DAX` trägt Preis + natives, variierendes Volumen in einem Symbol (live verifiziert, siehe [[project_dax_erweiterung]] To-do 1+2) — kein Pane-Split wie NAS100/QQQ nötig. Für DAX entfallen daher vollständig:
- Punkt 7b (Zwei-Pane-Check-Frequenz, EMA50-vs-EMA50-Pflicht-Gate)
- Punkt 7e (QQQ-Session-Gate, Zeitstempel-Vergleich zwischen zwei Instrumenten)
- Der QQQ-Anteil von Punkt 11 (Reversal-Kriterium 3) und Punkt 13.1 (Dual-Gate-Bedingung im Chasing-Kriterium)

Kein `pane_focus`-Wechsel im DAX-Loop nötig — ein struktureller Geschwindigkeitsvorteil gegenüber NAS100.

### 2.2 Ersatz für die verlorene Cross-Instrument-Filterfunktion — Entscheidung, kein offener Punkt
`IG:DAX` ist wie `FOREXCOM:NAS100` ein Broker-CFD. Die Dual-Gate-Funktion bei NAS100 ist nicht nur Volumenbeschaffung, sondern ein struktureller Cross-Check gegen Fehlausbrüche (ein CFD kann kurz spiken, ohne dass echtes Kapital mitzieht). Für DAX gibt es kein gleichwertiges zweites Preisinstrument — `XETR:DAX` hat 15-Min-Delay und ist für Live-Trading ausgeschlossen (siehe [[project_dax_erweiterung]] To-do 1).

**Entscheidung (Fable-Empfehlung, Levi-bestätigt 07.08.2026):** Kein Ersatz-Dual-Gate. Stattdessen zwei abgeschwächte Kompensationsmaßnahmen:
1. **Verschärfter Kerzenschluss-Standard:** DAX-Entries verlangen **2 aufeinanderfolgende bestätigte 5-Min-Kerzenschlüsse** über/unter dem Trigger-Level (statt 1 bei NAS100) — kompensiert den fehlenden unabhängigen Cross-Check durch mehr Zeit-Bestätigung auf demselben Instrument.
2. **Leichter Korrelations-Sanity-Check, kein hartes Gate:** EuroStoxx50 (`EUREX:FESX1!`) und/oder CAC40 aus dem Intermarket-Scan ([[feedback_session_update_dax]] Schritt 5) vor dem Trigger auf Divergenz prüfen. Läuft DAX klar gegen diese Referenzen, gilt das als Warnsignal — unterstützend, nicht blockierend, analog zur Rolle von VWAP/Volumen bei NAS100/QQQ.

**Status:** Startannahme, unverifiziert. Nach den ersten DAX-Paper-/Live-Trades (To-do 13) explizit prüfen, ob der 2-Kerzen-Standard ausreichend gegen Fehlausbrüche schützt oder nachjustiert werden muss.

**Nebenwirkung, aktiv beobachten:** Die Reversal-Kriterien (Punkt 11 unten) laufen für DAX dadurch faktisch mit 3 statt 4 möglichen Kriterien (2-3 von 3 statt 2-3 von 4) — die relative Schwelle sinkt spürbar. Bei den ersten DAX-Trades gezielt beobachten, ob das zu vorschnellen Reversal-Meldungen führt.

### 2.3 Handelsfenster & Trigger
10-15 Uhr DE-Zeit statt 15:30-22 Uhr. Trigger "start update dich dax" statt "start update dich" (umbenannt 07.08.2026, vorher "#1"/"#2"; siehe [[feedback_session_update_dax]]).

### 2.4 Skript-Referenzen (angelegt 07.08.2026)
- `cooldown_check_dax.cjs` — Env-Var `TRADE_LOG_DAX_PATH`, Default `dax_trades/trade_log_dax.md`
- `add_trade_dax.cjs` / `trade_stats_dax.cjs` — Import auf `trade_db_dax.cjs`, physisch getrennt von der NAS100-DB
- `size.cjs --kapital <DAX-Betrag>` — jetzt Pflichtparameter (kein NAS100-15.000€-Fallback mehr). DAX-Betrag selbst noch offen bis Startbudget feststeht (siehe [[project_risikomanagement_dax]])

### 2.5 Chart-Layout
Noch nicht final — To-do 10 folgt als nächster Schritt. Layout-Details (welche der 5 übrigen Indikatoren, gespeichertes `layout_switch`-Layout) werden dort ergänzt, hier bewusst nicht vorweggenommen.

---

## 3. Explizit als unverifiziert markierte Zahlen-Schwellen (Startannahme, 1:1 aus NAS100 übernommen, DAX-Marktverhalten unbewiesen)

- ATR-Multiplikator für SL-Mindestdistanz (1,5-2x, siehe [[feedback_chartanalyse]] Punkt 8c)
- Stall-Exit-Schwellen (Punkt 12.3: 2 Kerzenschlüsse + RSI ≥5 Punkte vom Extrem)
- 7c-Vorwarnschwellen (RSI ≥85 über 3+ Checks für parabolische Bewegung, 5-/3-Check-Schwelle für aktive Positions-Verteidigung)
- Chop-Kriterien allgemein — NAS100-kalibriert, für DAX unverifiziert
- Zeitfenster-Halbierung (die NAS100-Regel 15:30-16:00, siehe [[feedback_trading_zeitfenster]], ist an NAS100-Handelszeiten gebunden und nicht direkt übertragbar — DAX braucht ein eigenes Pendant-Zeitfenster, sobald genug Paper-Daten vorliegen, To-do 13)
- Reversal-Kriterien Punkt 11 — für DAX faktisch **2-3 von 3** statt 2-3 von 4, da Kriterium 3 (QQQ-Bruch) strukturell entfällt (siehe 2.1/2.2)

**How to apply:** Bei jedem DAX-Voll-Check diese Zahlen wie geschrieben verwenden, aber im Hinterkopf behalten, dass sie NAS100-kalibriert sind. Nach den ersten Paper-/Live-Sessions (To-do 13) aktiv gegen echtes DAX-Verhalten prüfen und bei Abweichung anpassen — nicht stillschweigend als bestätigt behandeln.
