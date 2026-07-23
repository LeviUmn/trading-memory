---
name: trading-session-nasdaq
description: "ÜBERHOLT seit 16.07.2026 (letzter Stand 30.06.) — historisches Archiv der frühen Nasdaq-Sessions; aktuelle Levels/Bias in trades/-Tagesdateien, Setup in feedback_chart_layout, Regeln in feedback_live_trading"
metadata: 
  node_type: memory
  type: project
  originSessionId: 607aa8f6-9958-4c1c-9c75-4afabcffb717
---

## ⚠️ ÜBERHOLT — nur noch historisches Archiv (Banner 16.07.2026, Fable-Review)

Diese Datei wurde vom 12.06. bis 30.06.2026 als lebendes Strategie-/Level-Dokument geführt und danach nicht mehr aktualisiert. **Alle Levels, Bias-Angaben und das Indikatoren-Set hier sind veraltet** (u.a. nennt sie noch Volume/VWAP auf NAS100 — seit 03.07. durch Bollinger/Pivots ersetzt). Aktuelle Quellen stattdessen:

- **Aktuelle Levels + Bias:** jeweils letzte Tagesdatei in `memory/trades/` (Tagesabschluss-Abschnitt) und [[trades/trade_log]]
- **Chart-Setup (2-Pane NAS100+QQQ, 5er-Indikatoren-Set):** [[feedback_chart_layout]]
- **Live-Trading-Regeln/Protokoll:** [[feedback_live_trading]], [[feedback_chartanalyse]]
- **Risiko/Phasen/Positionsgrößen:** [[project_risikomanagement]]

Das Grundprodukt (GS-Faktor-Zertifikate auf Scalable) und das Grundschema (Support-Entry, Teilgewinn, SL nachziehen) gelten weiter, sind aber längst detaillierter in den Regel-Dateien oben dokumentiert. Diese Datei NICHT mehr als Pflicht-Lektüre bei Session-Start lesen ([[feedback_memory_pflege]] wurde entsprechend angepasst) und keine neuen Levels mehr hier eintragen.

---

Erster echter Trade am 12.06.2026 erfolgreich abgeschlossen. +20.32€ Gewinn auf 518€ Einsatz (+3.92%).

**Why:** Bewährtes System — Support-Entry, Teilgewinn an Resistance, SL nachziehen. Hat heute perfekt funktioniert.

**How to apply:** Dieses Schema bei jedem Nasdaq-Trade anwenden.

### Produkt
- **Faktor 5x Long Nasdaq — Goldman Sachs (GS), Open-End**
- Handelbar auf **Scalable Capital** (Börse München / gettex)
- Ca. 6-7€ pro Stück, Budget ~260€ = ~40 Stück pro Tranche

### Bewährtes Trade-Schema (12.06.2026)
1. **Entry:** 40 Stück bei Support (~29.400) → erste Tranche
2. **Nachkauf:** 40 Stück beim Rücksetzer an Support → zweite Tranche
3. **TP1:** 40 Stück beim ersten Breakout verkaufen (+~10€)
4. **SL nachziehen** auf restliche 40 Stück → Breakeven, dann höher
5. **SL greift** → Gewinn gesichert, kein Overnight-Risiko

### Key Support/Resistance Levels (Stand 12.06.2026)
- **Support:** 29.280 (Tagestief), 29.400, 29.550
- **Resistance:** 29.620, 29.660, 29.700, 29.750
- **Nächste Woche Entry-Zone:** 29.350–29.500 bei Rücksetzer

### Regeln die funktioniert haben
- Kein Trade Freitagnachmittag nach 17:00 Uhr
- Kein Overnight mit Faktor-Zertifikat (Wochenende)
- SL immer nachziehen wenn Position im Gewinn
- Teilgewinn bei erstem Widerstand mitnehmen
- Nachkauf nur an klarem Support mit Bounce-Bestätigung

### Gesamtergebnis 12.06.2026
- Kauf 1: 40 × 6.489€ = 259.56€
- Kauf 2: 40 × 6.482€ = 259.28€
- TP1: 40 × 6.744€ = 269.76€ → +10.20€
- SL: 40 × 6.735€ = 269.40€ → +10.12€
- **Gesamt: +20.32€ (+3.92%)**

### Update 22.06.2026 — Levels vom 12.06. komplett veraltet
Nasdaq ist seit dem 12.06. (Entry 29.400) um +1000 Punkte auf ~30.400 gelaufen — alte Support/Resistance-Zone (29.280-29.750) liegt jetzt weit unter dem Kurs und ist irrelevant für neue Trades.

**Größerer Kontext (60 Tage):** Massive Erholungsrally +32,6% von 22.812 (Tief, ca. Ende März) auf 30.773 (Hoch). Aktuell Konsolidierung der letzten 5 Tage zwischen 29.605-30.674.

**Neue Key-Levels (Stand 22.06.2026, 16:36 CET):**
- Resistance: 30.673 (Tageshoch/Session-Top), darüber 30.773 (60-Tage-Hoch)
- Support: 30.358 (Tagestief, gerade mit Bounce-Kerze getestet), 30.045 (Vortagestief), 29.605 (5-Tage-Tief)
- Aktueller Kurs: ~30.430

**Bias 22.06.2026:** Übergeordneter Trend stark bullisch, aber Tageskerze zeigt scharfe Abweisung von 30.673 mit hohem Volumen (124K, höchstes Volumen der letzten Stunden) — möglicher Bull-Trap oder gesunde Konsolidierung. RSI 1H neutral (53,5), MACD noch bullisch aber Momentum lässt nach (Histogram nur noch +15,4). Letzte 15min-Kerze zeigt Bounce von 30.358 zurück auf 30.434 — Support hält bisher.

### Update 23.06.2026 — DeepSeek-/AI-Ausverkauf bricht alle Levels vom 22.06.
Globaler Tech-Sell-off (DeepSeek-News, China-AI-Modell) drückt Nasdaq über Nacht von ~30.380 auf 29.547 (-2,74%, vom Hoch 30.674 sogar -3,85%). Details siehe [[trade-2026-06-23]].

**Alle Bear-Trigger-Levels vom 22.06. gebrochen:** 30.255 ❌, 30.045 ❌, 29.605 ❌ — Kurs notiert jetzt unter allen dreien.

**Neue Key-Levels (Stand 23.06.2026, 12:30 CET):**
- Resistance: 29.605 (gebrochener Support → neuer Widerstand), darüber 30.045, 30.255, weit oben 30.674
- Support: 29.494 (frisches Tagestief), darunter psychologisch 29.000
- Aktueller Kurs: ~29.547

**VIX 19,79** (manuell geprüft, siehe Bug-Notiz in [[trade-2026-06-23]]) — noch "Normal"-Zone (15-20), kein Risk-Off-Crash trotz scharfem Move. Spricht für geordnete Korrektur statt Trendwende.

**Bias 23.06.2026:** Bearish kurzfristig, übergeordneter 60-Tage-Aufwärtstrend erstmals ernsthaft angeschlagen. RSI 5min 37,4 (Richtung überverkauft, nicht extrem), MACD bärisch aber Histogram flacht ab. Kein Entry im Morgen-Briefing-Fenster — abwarten auf Reaktion nach US-Open (15:30) bzw. beste Entry-Zone 16:00-18:00.

### Update 24.06.2026 (Mittwoch) — Tiefstes Tief seit DeepSeek, gerettet durch Micron
Kein Trade. Sehr bearishe Session: Selloff den ganzen Tag, absolutes Tief **28.993** (erstmals unter 29.000 seit DeepSeek-Tief). Direkt nach US-Open (15:30 CET) Flush auf 29.253 mit 50-60K Volumen/Kerze. Nachmittag weiteres Grinden Richtung 29.000.

**Rettung durch Micron-Earnings ~22:00 CET:** Rekord-Quartal Q3 FY2026 ($41,46 Mrd. Revenue, +16% Beat, Aktie +13,1% AH) → Nasdaq-Spike von 29.242 auf 29.884 in wenigen Minuten. Session-Schluss ~29.823.

### Update 25.06.2026 (Donnerstag) — Kein Trade, Makro-Datentag
Kein Trade. User nachmittags nicht verfügbar. Kurs öffnete bei ~29.823 (Micron-Niveau), stieg langsam auf ~29.910 bis 13:00 CET.

**Makrodaten 14:30 CET (tatsächliche Ergebnisse):**
| Datenpunkt | Aktuell | Konsens | Vorher |
|---|---|---|---|
| GDP Q1 2026 Final QoQ | **+2,1%** | 1,6% | 0,5% (zweite Schätzung) |
| Core PCE Price Index MoM | **0,3%** | 0,2% | 0,3% |
| Personal Income MoM | ~0,7% | — | — |
| Jobless Claims 4-week Avg | ~224K | — | ~223K |

**Marktreaktion:** Stagflations-Angst ("higher for longer") — GDP massiv nach oben revidiert (0,5% → 2,1%) + PCE über Erwartung = Fed hat keinen Grund zu senken. High-Multiple-Tech (Nasdaq) reagiert am stärksten auf Zinserwartungen. Ergebnis: Asien verkaufte diese Zahlen in der Nacht ab, US-Futures folgten.

**Neue Key-Levels (Stand 25.06.2026, 13:00 CET):**
- Resistance: 29.955 (Tageshoch), 30.000 (psychologisch), 30.045 (technisch), 30.255, 30.674
- Support: 29.764, 29.605, 29.335, 29.000
- Kurs vor Makro-Release: ~29.910

### Update 26.06.2026 (Freitag) — Kein Trade, Makro-Nachklang + Asien-Sell-off
Kein Trade. User nicht verfügbar.

**Preisbewegung:** Asien verkaufte die starken US-Makrodaten vom 25.06. in der Nacht durch → Continuation Selling heute früh. Kurzes Hope-Spike auf **29.997** (fast 30.000, präzise abgewiesen), dann scharfer Sell-off auf **28.909** (psychologische 29.000 durchbrochen). Aktuell Erholung auf ~29.172.

| | |
|---|---|
| Tages-Eröffnung | ~29.918 |
| Tages-Hoch | 29.997 (30.000 abgewiesen) |
| Tages-Tief | **28.909** (29.000 durchbrochen!) |
| Aktuell | ~29.172 |
| Tages-Change | −746 Pkt / −2,49% |

**Indikatoren (15-min, ~Nachmittag):** RSI 48,23 (neutral), RSI-MA 51,32 (darüber → bearish), MACD −10,05 / Signal −11,48 / Histogram +1,43 (Micro-Bounce-Signal).

**Neue Key-Levels (Stand 26.06.2026):**
- Resistance: 29.335 (Flash-Low 23.06.), 29.494–29.605 (starke Zone), 30.000 (heute präzise abgewiesen)
- Support: 29.000 (psychologisch, heute durchbrochen), **28.909** (heutiges Tief), darunter ~28.700 (keine klare Zone bekannt)

**Bias:** Bearish. Kein Setup heute (Freitagsregel + kein Entry-Kriterium erfüllt). Nächste Woche ab Montag (29.06.) neu einschätzen.

### Update 30.06.2026 (Dienstag) — 3 Trades, +9,72€, Quarter-End

**Quarter-End-Tag.** NQ erholte sich weiter von Freitags-Tief (28.909). 3 Trades abgeschlossen, alle Wins.

| | |
|---|---|
| Tages-Eröffnung | ~29.918 |
| Tages-Hoch | 30.221 |
| Tages-Tief | ~29.714 (EU-Session) |
| Letzter Kurs ~18:00 CET | ~30.209 |
| VIX | 16,87 (fallend, Risk-On) |

**Trades heute:** Short +3€ (Abprall 30.118) → Long +1,12€ → Long +5,60€ (Breakout über 30.175).

**Key-Levels (Stand 30.06.2026):**
- Resistance: 30.221 (Session-High), 30.255 (TP1 nicht erreicht), 30.400
- Support: 30.120 (letzter Long-Entry), 30.000 (psychologisch), 29.764, 29.335

**Zertifikat-Ratio kalibriert:** ~0,0069€/NQ-Punkt (GS 5x Long, ~40€ Cert, ~30.200 NQ). NICHT 0,01€/Punkt!

**Bias für 01.07.2026:** Bullisch. VIX fallend, Trend aufwärts, EMA50 bei 30.028 (steigend). Entscheidend: Hält NQ über 30.120 (Unterstützung), nächstes Ziel 30.255 → 30.400.

**Setup-Ideen für morgen:**
- Long Pullback: Entry 30.050–30.120, SL 29.900, TP1 30.255, TP2 30.400
- Short nur wenn Kurs 30.300+ mit bärischer Abweisung + RSI >70 Divergenz

**Gesamtstatistik Phase 1 (Stand 30.06.2026):** 7 Trades | +58,16€ | 86% Win-Rate | 0 Verluste

### Update 29.06.2026 (Montag) — Kein Trade, starke Erholung, Session-Review + System-Verbesserungen
Kein Trade. User erst ab ~16:30 verfügbar, Session endete ~19:00 CET → zu spät für sauberen Entry.

**Preisbewegung:** Starke Erholung von Freitags-Tief (28.909). Tages-Tief heute: 29.043 (nochmal kurz unter 29.000 in der Nacht/EU-Session), dann kontinuierliche Aufwärtsbewegung auf Tageshoch **29.673**.

| | |
|---|---|
| Tages-Eröffnung | ~29.474 |
| Tages-Hoch | **29.673** |
| Tages-Tief | 29.043 |
| Kurs ~16:30 CET | 29.644 |
| Tages-Change | +173 Pkt / +0,59% |

**Indikatoren (15-min, ~16:30 CET):** RSI 61,58 (MA 52,62 → bullisch), MACD Histogram +25,34 (stark bullisch), VWAP 29.387 / Upper Band 29.510 (Kurs deutlich darüber), EMA 50: 29.443 (Kurs +200 Pkt darüber).

**Key-Levels (Stand 29.06.2026):**
- Resistance: **29.673** (Tageshoch, mehrfach getestet), 29.764, 30.000 (psychologisch)
- Support: 29.494–29.510 (Upper VWAP Band + alte S/R-Zone), 29.387 (VWAP), 29.335, 29.043 (Tages-Tief)

**Bias für 30.06.2026:** Bullisch. Alle 5 Indikatoren ausgerichtet. Freitags-Tief 28.909 scheint als Boden zu halten. Entscheidend: Schafft der Kurs einen sauberen Breakout über 29.673, oder kommt Rücksetzer auf 29.494–29.510 als besseren Long-Entry?

**Setup-Ideen für morgen (30.06.2026):**
- Long Pullback: Entry 29.494–29.510, SL unter 29.387, TP1 29.673, TP2 29.764 (RRR ~1:1,6 / 1:2,1)
- Long Breakout: Close über 29.673 mit Volumen, SL 29.510, TP 30.000 (RRR ~1:2,0)

**System-Verbesserungen heute beschlossen:**
- Update-Ablauf korrigiert: Makro API + RSS parallel → danach TradingView
- Intermarket-Dashboard erweitert: 20 Instrumente (VIX, DXY, EUR/USD, USD/JPY, Gold, WTI, Brent, US2Y/10Y/30Y, JP10Y, SP500, NAS100, US30, US2000, Nikkei, HSI, SSE, KOSPI, DAX, EuroStoxx50)
- Indikatoren-Setup bestätigt: aktuelle 5 sind optimal, kein Upgrade nötig
