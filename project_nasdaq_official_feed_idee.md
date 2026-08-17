---
name: project-nasdaq-official-feed-idee
description: "Idee geprüft: offizieller nicht-verzögerter Nasdaq-Chart (TradingView Add-on) als Ersatz für QQQ/Dual-Gate — Fable-Review 07.08.2026, noch nicht entschieden"
metadata:
  type: project
  originSessionId: session-2026-08-07
  modified: 2026-08-07T08:39:24.821Z
---

Levi erwägt ein bezahltes TradingView-Add-on: ein offizieller, nicht zeitverzögerter Nasdaq-Chart, auf dem angeblich alle bisherigen Indikatoren (EMA/RSI/MACD/ATR/BB) UND VWAP UND echtes Volumen aus EINER Quelle laufen würden — aktuell verteilt auf zwei Instrumente (NAS100 CFD für Indikatoren, QQQ ETF für VWAP+Volumen, siehe [[feedback_dual_gate_confirmation]] und [[feedback_live_trading]] Punkt 7b/7e).

**Fable-Review 07.08.2026 (Kernaussagen):**
- Dual-Gate ist kein reiner Datenlücken-Workaround, sondern prüft strukturell zwei unabhängige Signalquellen: CFD-Preisaktion (NAS100) vs. echter Kapitalfluss (QQQ). Diese Unabhängigkeit ist der eigentliche Rauschfilter gegen Fehlausbrüche (Kerzenschluss-Pflicht aus Trade #13, [[feedback_dual_gate_confirmation]]).
- Vor jeder Umstellung MUSS technisch verifiziert werden, was der neue Feed wirklich ist: echter Cash-Index/Future oder wieder ein CFD/Proxy? Läuft er über echte Marktschließzeiten hinweg (Hinweis auf synthetisches statt echtes Volumen)? VWAP-Reset korrekt zum Session-Open? Zuverlässigkeit außerhalb RTH?
- Falls der neue Feed technisch sauber ist: MTF-Kaskade (1H→15min→5min) auf einem Einzelinstrument ersetzt NICHT das Cross-Instrument-Prinzip — sie filtert Zeit-Konsistenz, nicht Quellen-Unabhängigkeit. Empfehlung falls umgestellt wird: MTF-Kaskade beibehalten UND zusätzlich verschärften Bestätigungsstandard (z.B. 2 aufeinanderfolgende Kerzenschlüsse statt 1, oder leichter Referenz-Check gegen QQQ/SPY vor Trigger) als Ersatz für den verlorenen Cross-Check einbauen.
- **Empfehlung: nicht umstellen, sondern im Schatten-Modus testen** (2-3 Wochen, keine Live-Entries auf dem neuen Feed) — explizit gegen historische Fälle prüfen, in denen das QQQ-Gate einen Fehlausbruch verhindert hätte. Erst nach Validierung + geklärter Feed-Identität über Umstellung nachdenken, eher als Ergänzung denn als Ersatz.

**Why:** Passt zur bestehenden Grundregel [[feedback_dont_change_running_system]] — Verbesserungsideen am laufenden Live-System werden geprüft/vermerkt, nicht sofort umgesetzt.

**How to apply:** Noch keine Entscheidung getroffen, kein Kauf des Add-ons erfolgt. Falls Levi das Add-on bucht: zuerst Feed-Identität klären (`symbol_info`), dann Schatten-Test vor jeder Live-Umstellung. Dual-Gate/QQQ bleibt bis dahin unverändert das Live-System.

**Konkretisierung nach TradingView-Preistabelle (07.08.2026, Fable-Folgereview):** Levi hat die tatsächliche TradingView-Datenquellen-Preisliste gezeigt. Kernerkenntnis: Das "US-Börsenbündel" (€9,95/Monat, NYSE+NASDAQ+Arca+NASDAQ GIDS+OTC) ist eine Börsen-DATENLIZENZ, kein fertiges Einzelinstrument. Zwei Optionen darunter:
- **Echter Index (NDX/IXIC) über "NASDAQ Global Index Data Service" (€5/Monat, separat buchbar):** Echter Cash-Index statt FOREXCOM-CFD-Proxy, kein Delay — aber strukturell KEIN echtes Handelsvolumen (Index wird berechnet, nicht gehandelt; ein Volumen-Feld wäre nur aggregierte Komponenten-Summe, VWAP-untauglich). Löst das Volumen/VWAP-Problem nicht.
- **QQQ über NASDAQ-Feed statt aktuell BATS/Cboe-One:** Kein automatisches Upgrade — beides sind Einzelbörsen-Ausschnitte (nur dort ausgeführte Trades), keine konsolidierte SIP-Tape-Sicht. Müsste empirisch verglichen werden (Volumen-Magnitude), bevor als Verbesserung gewertet.

**Präzisierte Empfehlung:** Dual-Gate-Prinzip bleibt in der Substanz notwendig (Kernaussage unverändert). Isolierter, günstiger Test empfohlen: NUR "NASDAQ Global Index Data Service" (€5/Monat) buchen, NDX/IXIC parallel als Preis-Feed-Ersatz für die NAS100-CFD-Seite mitlaufen lassen (reines Preis-Upgrade, unabhängig vom Dual-Gate) — QQQ/Dual-Gate bleibt live unverändert. Das volle €9,95-Bundle lohnt nur, falls zusätzlich (niedrigere Priorität) das QQQ-NASDAQ-Feed-Experiment separat getestet werden soll.
