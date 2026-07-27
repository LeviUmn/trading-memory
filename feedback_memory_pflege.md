---
name: feedback-memory-pflege
description: "Memory und Projektordner immer aktuell halten — nach jedem Update, Trade und neuer Erkenntnis. Seit 16.07.2026 zusätzlich: Zitierpflicht für Zahlenbehauptungen in Permanent-Records (frischer Tool-Call statt Gedächtnis-Rekonstruktion)"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 607aa8f6-9958-4c1c-9c75-4afabcffb717
  modified: 2026-07-27T11:55:24.604Z
---

Nach JEDER Änderung, neuem Trade oder neuer Erkenntnis sofort die Memory-Dateien und NEUSTART.md aktualisieren.

**Why:** User will langfristig eine Trading-Datenbank aufbauen die Backtesting verbessert und Strategien verfeinert. Nichts darf verloren gehen.

**How to apply — BEIM SESSION-START immer zuerst lesen (aktualisiert 16.07.2026):**
- `memory/MEMORY.md` → Index aller Erinnerungen
- `memory/trades/` → Letzte Trade-Dateien (letzte 3-5 Tage) → aktuelle Levels + Bias stehen im jeweiligen Tagesabschluss
- [[feedback_chart_layout]] + [[feedback_live_trading]] → Chart-Setup & Protokoll (ersetzt die frühere Pflicht-Lektüre `trading_session_nasdaq.md` — die ist seit 16.07.2026 nur noch Archiv)

Erst DANN mit Briefing und Trading beginnen — nie ohne Memory-Check starten!

**How to apply — Bei folgenden Ereignissen immer speichern:**

1. **Neues Script / Tool gebaut** → NEUSTART.md + MEMORY.md updaten
2. **Trade abgeschlossen** → trading_YYYY-MM-DD.md erstellen (via "Tag Zusammenfassung speichern") UND seit 27.07.2026 zusätzlich `node scripts/add_trade.cjs` im Projektordner ausführen (siehe unten) — die Markdown-Datei bleibt die narrative Quelle, die neue SQLite-DB ist für Zahlen/Filter/künftige Auswertungen
3. **Neue Strategie-Erkenntnis** → passende Regel-Datei updaten ([[feedback_live_trading]], [[feedback_chartanalyse]] etc.) oder neue Feedback-Datei (nicht mehr trading_session_nasdaq.md — Archiv)
4. **Neue API / Integration** → project_news_system.md oder neue Datei
5. **Fehler gemacht** → Als Feedback-Memory speichern damit es nicht nochmal passiert
6. **Levels identifiziert** → In die Tagesdatei `memory/trades/trading_YYYY-MM-DD.md` eintragen (nicht mehr trading_session_nasdaq.md — Archiv)

### Historische Trading-Daten Struktur
Jeder Trade-Tag bekommt eine eigene Datei:
`memory/trades/trading_YYYY-MM-DD.md`

Mit der Zeit entsteht eine vollständige Trading-Historie die für Backtesting genutzt werden kann:
- Welche Setups funktionieren in welchem Marktregime
- Win-Rate pro Strategie
- Optimale Entry/Exit-Punkte
- Fehler-Muster die wir vermeiden

### Strukturierte Trade-DB (ergänzt 27.07.2026, Fable-Build)

Zusätzlich zur Markdown-Historie gibt es jetzt eine echte, abfragbare SQLite-DB im Code-Projekt (`C:\Users\umnus\tradingview-mcp\scripts\`, NICHT im Memory-Ordner):
- `trade_db.cjs` — Schema (Node's eingebautes `node:sqlite`, keine neue Abhängigkeit)
- `trade_stats.cjs` — Win-Rate, Ø-Gewinn/-Verlust, realisiertes RR, Erwartungswert, Phasen-Aufschlüsselung, filterbar (`--phase`/`--outcome`/`--since`/`--violations`)
- `add_trade.cjs` — CLI zum Eintragen neuer Trades
- `migrate_trade_log.cjs` — einmaliger Import-Lauf der historischen 25 Trades (bereits erledigt, Verifikation gegen die dokumentierten Summen bestanden: 25 Trades, 15W/2BE/8L, +138,86€, 65,2%/60% Win-Rate)

**How to apply:** Nach jedem abgeschlossenen Trade zusätzlich zur gewohnten `trading_YYYY-MM-DD.md`-Notiz `node scripts/add_trade.cjs --date ... --phase ... --dir ... --result-eur ... --outcome ...` (weitere Felder siehe Kopfkommentar der Datei) im Projektordner ausführen. Die Markdown-Datei bleibt die Quelle für die volle Geschichte (Setup-Begründung, Regelbrüche im Detail, Lehren) — die DB ist nur für schnelle Zahlen-Abfragen und die künftige Monte-Carlo-Auswertung ab ~50 Trades (siehe [[project_robustheit_monte_carlo]]) gedacht, ersetzt die Markdown-Historie nicht.

## Zitierpflicht für Permanent-Records (ergänzt 16.07.2026, nach Trade #21 — Fable-Empfehlung)

Bei Trade #21 (16.07.2026) enthielt die erste Nachbesprechung zwei falsche Zahlenbehauptungen (ein Kerzentief der falschen 5-Min-Kerze zugeordnet, ein lokales Level fälschlich als "Tagestief" bezeichnet) — beide aus dem Konversationsgedächtnis rekonstruiert statt frisch verifiziert. Eine unabhängige Fable-Prüfung deckte beides auf; [[feedback_verify_dont_cave]] deckt Live-Widersprüche ab, aber keine Schreib-Zeitpunkt-Verifikation beim Verfassen eines Postmortems — echte Regellücke, nicht nur Disziplin.

**Regel:** Jede konkrete Zahlenbehauptung, die in eine `memory/trades/`-Datei oder eine Regeldatei geschrieben wird — Preislevel, Kerzen-Timestamps/OHLC-Werte, Kerzenzählungen ("nur X Kerzen", "3. Kerze in Folge"), Superlative ("Tagestief", "Tageshoch", "erstmals seit...") — braucht einen **frischen Tool-Call unmittelbar vor dem Schreiben**, nicht aus dem Gedächtnis/der laufenden Konversation rekonstruiert. Insbesondere Superlative wie "Tagestief/Tageshoch" erfordern einen `data_get_ohlcv`-Pull über die GESAMTE bisherige Session (ausreichend hoher `count`), nicht nur die letzte Stunde — genau dieser Kurzschluss führte zur "29.170 = Tagestief"-Verwechslung, während das echte Tagestief bei 29.065,75 lag.

**Leichter Selbst-Check statt Voll-Review:** Vor dem Finalisieren eines Postmortems den eigenen Entwurf auf Rotflaggen-Phrasen scannen ("Tagestief/-hoch", "nur X Kerzen", "erste/einzige Kerze", konkrete Preise/Zeitstempel) und jede davon gegen einen frischen Datenabruf gegenlesen. Das ist der Normalfall, kein Fable nötig.

**Wann eine unabhängige Fable-Zweitmeinung tatsächlich lohnt (nicht routinemäßig bei jedem Trade):**
1. Das Postmortem behauptet einen Regelverstoß oder führt zu einer Regeländerung (wie hier die Chop-Erkennung-Präzisierung)
2. Ungewöhnlich großer Verlust relativ zur Erwartung
3. Eine Statistik-Neuberechnung (Win-Rate, Gesamtbilanz) hängt am Ergebnis
4. Explizite User-Anfrage

Routine-Logs von normal verlaufenden Trades brauchen nur den Selbst-Check oben, keine doppelte Prüfung — das wäre Overhead ohne proportionalen Nutzen.
