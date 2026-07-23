---
name: project-vision
description: "Langfristiges Ziel — professioneller Trader auf Bloomberg/GS-Niveau durch systematischen, datengetriebenen Aufbau über alle Phasen"
metadata:
  node_type: memory
  type: project
  originSessionId: trading-session-2026-06-26
---

## Das Ziel (formuliert 26.06.2026)

Wir wollen am Ende aller Phasen der bestmögliche Trader sein — professionell genug um theoretisch bei Bloomberg oder Goldman Sachs jeden Trader zu ersetzen. Das bedeutet:

- Zugriff auf alle relevanten Daten (makro, politisch, wirtschaftlich, technisch)
- Alle Chart-Signale und Linien erkennen und korrekt interpretieren
- Aus der eigenen Trackhistory systematisch lernen und das System stetig verbessern
- Emotionslos und regelbasiert handeln

**Why:** User möchte Trading nicht als Hobby betreiben sondern als professionelles Handwerk — mit dem Ziel Lebenshaltungskosten langfristig über Trading zu finanzieren (evtl. nach Schweiz-Umzug). Der Weg dorthin ist bewusst langsam und datengetrieben, nicht "reich über Nacht".

**How to apply:** Bei jeder Entscheidung (neues Tool, neue Regel, Phasenwechsel) immer prüfen: Bringt das uns näher an professionelles Niveau oder ist es Noise?

---

## Fahrplan nach Phasen

| Phase | Trading | Analyse-Tools | Datenzugang |
|---|---|---|---|
| 1 (abgeschlossen, 15/15, Stand 09.07.) | Regelbasiert, 1% Risiko, NAS100 | RSI/MACD/VWAP/EMA50, MTF 1H→15min→5min, **Intermarket (VIX/VXN/DXY/10Y/Öl) als täglicher Kontext** | X MCP (aktiv, löst RSS ab) + Makrokalender + FRED |
| 2 (Trade 16-30) | Größere Positionen, Multi-Setup | Fibonacci präziser, Orderflow-Ansätze | X API evaluieren (Breaking News Latenz) |
| 3 (Trade 31-55) | Mehrere Instrumente | Intermarket-Divergenzen aktiv traden | Sentiment-Daten, erweiterte Makro-Integration |
| 4 (ab Trade 55) | Vollsystem, 2% Risiko | Komplettes Toolkit | Alle Quellen integriert |

**Korrigiert 09.07.2026:** Phase-1-Verlängerung (02.07., 10→15 Trades) war hier noch nicht nachgezogen — Nummerierung jetzt konsistent mit [[project_risikomanagement]] (Phase 2 = 16-30, Phase 3 = 31-55, Phase 4 = ab 55).

---

## Was einen professionellen Trader ausmacht

1. **Disziplin vor Intuition** — Regeln schlagen Bauchgefühl. Bereits implementiert.
2. **Makrodaten als Entscheidungsmatrix lesen** — PCE/GDP nicht als Zahl, sondern als Fed-Signal verstehen.
3. **Systematisches Lernen aus Trackhistory** — Welche Setups haben höchste Win-Rate? Welche Uhrzeit? Welcher VIX-Bereich? Ab Phase 2 auswerten.
4. **Intermarket-Analyse** — Nasdaq nie isoliert sehen: DXY, 10Y Yield, VIX, Öl korrelieren immer.
5. **Emotionslosigkeit durch Regeln** — Jede Entscheidung muss im Regelwerk stehen, dann gibt es keine Emotion.

---

## Offene Evaluierungspunkte nach Phase

**Phase 2 (ab Trade 11):**
- **X API (Twitter):** Bereits aktiv seit 30.06.2026 (siehe [[project_news_system]]), löst RSS ab — dieser Punkt ist erledigt, nicht mehr offen.
- **Zweites Instrument:** DAX oder Gold als Ergänzung erst nach Trade 15-20 (laut [[feedback_instrumenten_fokus]])

**Phase 3 (ab Trade 26):**
- **UnusualWhales (~$50/Monat):** Kombiniert Dark Pool Flow + Options Data. Dark Pool = wo Institutionelle heimlich große Positionen aufbauen (vor Kursreaktion). Options = Put/Call Ratio, Unusual Activity, Max Pain, Gamma Exposure (GEX als unsichtbare S/R-Zonen). Erst sinnvoll wenn eigener Track Record statistisch belastbar ist. **User folgt UnusualWhales bereits auf X** — kennt den Content, Entscheidung für Phase 3 praktisch gesetzt.
  - **Integrationsplan (geklärt 10.07.2026):** UnusualWhales hat eine eigene REST-API (separater Tarif vom Consumer-Zugang). Technischer Weg: eigener kleiner MCP-Server analog zum bestehenden X/xapi-MCP, der die API wrapt (Tools wie `get_flow_alerts`, `get_dark_pool_prints`, `get_gex`) — damit live im Chat abrufbar statt manuellem Dashboard-Check. Bauen erst kurz vor Phase-3-Start sinnvoll (grob ab Trade ~25-28), nicht jetzt schon.
- **Intermarket-Divergenzen aktiv traden** (nicht nur als Kontext lesen)

**Dauerhaft gestrichen:**
- Bloomberg Terminal (~$2.000/Monat) — wirtschaftlich nicht realistisch für Retail-Phase

**Phase 4 (ab Trade 50):**
- **IBKR / Futures (MNQ):** Vorbereitung auf Schweiz-Umzug — parallel testen bevor Umzug ansteht. IBKR bietet keine deutschen Hebelzertifikate, Alternative wäre Micro Nasdaq Futures.
