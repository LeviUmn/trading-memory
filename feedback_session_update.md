---
name: feedback-session-update
description: "Trigger 'start update dich' → IMMER vollständiger 6-Schritt-Ablauf: Memory → Tweets → FRED → Live-Kalender → TV Intermarket → NAS100+QQQ Chart → Bias. Keinen Schritt überspringen."
metadata:
  node_type: memory
  type: feedback
  originSessionId: 607aa8f6-9958-4c1c-9c75-4afabcffb717
  modified: 2026-07-24T11:42:36.701Z
---

## KRITISCH: Vollständiger Ablauf bei "start update dich"

**Trigger:** NUR wenn User explizit **"start update dich"** schreibt.

Bei allen anderen Einstiegen (Begrüßungen, direkte Fragen, Chart-Anfragen) → NICHT automatisch starten.

**Why:** Das ist das wichtigste Update VOR dem Trading. Märkte laufen auch wenn wir nicht traden — Wochenenden, Pausen, Feiertage können geopolitische Events, Gap-Ups/Downs, neue Makrodaten bringen die den Kontext komplett verändern. Technische Analyse ohne fundamentalen Kontext ist wertlos. Jeder der 6 Schritte ist Pflicht.

**How to apply:** Alle 6 Schritte in dieser Reihenfolge, so viel wie möglich parallel ausführen.

---

## Der 6-Schritt-Ablauf

### Schritt 1 — Memory lesen
- MEMORY.md + letzte Trade-Datei aus `memory/trades/`
- Aktuelle Levels, Bias, Gesamtstatistik, offene Erkenntnisse

### Schritt 2 — Tweets der 3 Accounts (seit letztem Fetch)
- `scripts/x_last_fetch.json` lesen → `last_fetch` Timestamp
- `get_users_posts` für alle 3 Accounts aus `scripts/x_watchlist.json` mit `start_time = last_fetch`:
  - **@DeItaone** (ID: 2704294333) — Breaking News, Fed, Macro
  - **@KobeissiLetter** (ID: 3316376038) — Marktanalyse, Sentiment, Kontext
  - **@zerohedge** (ID: 18856867) — Risk-off Signale
- Tweets als strukturierten Digest ausgeben (Bullisch / Bearisch / Neutral aufgeteilt)
- Nach dem Lesen `x_last_fetch.json` mit aktuellem UTC-Timestamp aktualisieren

### Schritt 3 — FRED Makrodaten (historisch/strukturell)
- `python scripts/fred_snapshot.py` ausführen
- Gibt die letzten offiziellen Werte für: CPI, Core CPI, Core PCE, Fed Funds Rate, 10Y/2Y Treasury, 10Y Breakeven Inflation, Unemployment, Jobless Claims, GDP
- Zweck: Strukturellen Makro-Hintergrund verstehen (Zinsniveau, Inflation, Arbeitsmarkt)
- FRED = historische Daten, nicht für heutige Events

### Schritt 4 — Live Wirtschaftskalender (was steht HEUTE an)
- `WebFetch` von `https://www.investing.com/economic-calendar/`
- Prompt: Alle High und Medium Impact Events für heute, mit Uhrzeit, Konsens, Vorwert
- Zweck: FRED kennt keine Zukunft — der Live-Kalender zeigt was heute veröffentlicht wird
- **WICHTIG:** Konsens-Werte IMMER live holen, NIEMALS aus Trainingswissen annehmen (kann falsch/veraltet sein)
- Schritte 2+3+4 können alle parallel gestartet werden (unabhängig voneinander)

**Market-Moving Events (immer prüfen):**
- PCE / Core PCE — Fed-kritisch (typisch letzter Freitag des Monats, 14:30 CET)
- CPI — ähnlich PCE (14:30 CET)
- NFP (Non-Farm Payrolls) — erster Freitag des Monats, 14:30 CET — sehr stark
- ISM Manufacturing/Services — 16:00 CET (erster/dritter Werktag des Monats)
- JOLTs Job Openings — 16:00 CET, monatlich
- CB Consumer Confidence — 16:00 CET, monatlich
- ADP Employment — monatlich, 14:15 CET
- FOMC-Protokolle / Fed-Entscheidungen
- GDP-Daten (Revisionen beachten!)
- **Doppel-Events gleichzeitig 16:00:** Spike-Volatilität in beide Richtungen → kein Entry vor 16:15-16:30
- Grundregel: Kein Entry bis 30 min nach Veröffentlichung + Volatilität beruhigt

### Schritt 5 — TradingView öffnen: Intermarket-Scan
TradingView IMMER starten (`tv_launch`), nicht optional. Chart läuft auch vorbörslich.

**Split-Screen-Persistenz (Stand 24.07.2026):** NAS100+QQQ-Split übersteht Neustarts zuverlässig, siehe [[feedback_chart_layout]] für die Lösung und eine bekannte Falle (kurzzeitig falsches `pane_list`-Ergebnis direkt nach `tv_launch`, kein echtes Problem).

#### 5a — Intermarket-Dashboard (20 Instrumente via `quote_get`)
Alle Kurse abrufen und mit Cheat-Sheet auswerten:

| Kategorie | Instrument | Symbol |
|---|---|---|
| **Angst** | VIX | CBOE:VIX |
| | VXN (Nasdaq-Vola) | CBOE:VXN |
| **Dollar** | DXY | TVC:DXY |
| | EUR/USD | FX:EURUSD |
| | USD/JPY | FX:USDJPY |
| **Rohstoffe** | Gold | OANDA:XAUUSD |
| | WTI Öl | NYMEX:CL1! |
| | Brent | NYMEX:BB1! |
| **Zinsen** | US 2Y | TVC:US02Y |
| | US 10Y | TVC:US10Y |
| | US 30Y | TVC:US30Y |
| | Japan 10Y | TVC:JP10Y |
| **US Aktien** | SP500 | SP:SPX |
| | NAS100 | **nicht via `quote_get`** — siehe [[feedback_datenquelle_nas100]], stattdessen `data_get_ohlcv` (count 1-5) oder `chart_get_state` |
| | Dow Jones | DJ:DJI |
| | Russell 2000 | TVC:RUT |
| **Asien** | Nikkei | TSE:NI225 |
| | Hang Seng | HKEX:HSI |
| | SSE Composite | SSE:000001 |
| | KOSPI | KRX:KOSPI |
| **Europa** | DAX | XETR:DAX |
| | EuroStoxx50 | EUREX:FESX1! |

#### 5b — Intermarket-Cheat-Sheet

| Signal | Bullisch NAS100 | Bärisch NAS100 |
|---|---|---|
| VIX | Fällt / unter 15 | Steigt / über 20 |
| DXY | Schwächer | Stärker |
| US10Y Yield | Fällt / unter 4,0% | Steigt / über 4,5% |
| US2Y Yield | Fällt | Steigt (hawkish Fed) |
| USD/JPY | Stabil / schwächer | Stark steigend dann Absturz (Carry Unwind) |
| Öl | Stabil | Stark steigend (Inflation) |
| Gold | Stabil / fällt | Stark steigend (Risk-Off) |
| Russell 2000 | Steigt mit NAS100 | Fällt während NAS100 steigt (Divergenz = Warnung) |
| VXN/VIX-Ratio | Unter ~1,3x (normal) | Über ~1,5x = Tech-spezifischer Stress, unabhängig vom VIX-Niveau |

Wenn 2+ Faktoren bärisch → übergeordneter Gegenwind, Short-Bias verstärkt sich auch wenn NQ-Chart bullisch aussieht.

**VXN/VIX-Ratio-Regel (ergänzt 09.07.2026):** VIX allein kann täuschen — ein ruhiger VIX (z.B. ~16-17) sagt nichts über Tech-spezifische Nervosität aus. VXN misst die implizite Vola des Nasdaq-100 separat. Normal liegt VXN/VIX bei ~1,1-1,3x; ein Ratio ab ~1,5x (am 09.07.2026 bei 1,66x, höchstes seit 23 Jahren laut Kobeissi) zeigt konzentriertes Tech-Risiko (Mag7/AI-Positionierung, Bewertung, Sektor-Katalysatoren), das der breite Markt noch nicht einpreist — spricht für vorsichtigere Positionsgrößen/breitere Stops auf NAS100, auch wenn VIX entspannt wirkt.

**Korrigiert 04.07.2026 (Fable-5-Review):** Dieser Schritt widersprach bis eben [[feedback_datenquelle_nas100]], die `quote_get` für NAS100 grundsätzlich verbietet (zweimal live falsche Kurse geliefert, 01.07. und 02.07.). Die NAS100-Zeile der Tabelle wurde entsprechend korrigiert — im Intermarket-Scan den NAS100-Kurs aus den Chart-Bars holen, nicht aus `quote_get`.

### Schritt 6 — NAS100 + QQQ Chart + Indikatoren (korrigiert 16.07.2026: VWAP liegt nicht mehr auf NAS100, Pane 1 ist QQQ statt NQ1!)
- `chart_get_state` → aktuelles Symbol, Timeframe, Indikatorliste (Pane 0 = NAS100)
- `data_get_study_values` → RSI, MACD, EMA50, Bollinger Bands, ATR (aktuelle NAS100-Werte — **kein VWAP auf NAS100**, der CFD-Feed hat kein Volumen, siehe [[feedback_chart_layout]]; Pivot Points seit 23.07.2026 kein Chart-Indikator mehr, siehe [[feedback_chartanalyse]] Punkt 7a1a für die Berechnung)
- `data_get_ohlcv` mit `summary: true` + zusätzlich `count: 300` → vollständigen Verlauf seit letzter Session rekonstruieren
- **QQQ-Pane (Pane 1):** `pane_focus(1)` → EMA50/VWAP/Volume von QQQ lesen → zurück `pane_focus(0)`. Vorher Session-Gate/Zeitstempel-Check nach [[feedback_live_trading]] Punkt 7e — bei Pre-Market/geschlossenem Markt den QQQ-Stand explizit als "dünn" bzw. "Stand von gestern" kennzeichnen, nicht als aktuell ausgeben. Volumen-Spikes relativ zum eigenen QQQ-Durchschnitt bewerten (NAS100-CFD hat keine)
- Welche Key-Levels wurden getestet / gebrochen seit letzter Session?
- `capture_screenshot` für visuellen Überblick

---

## Ausgabe-Struktur des Briefings

Immer in dieser Reihenfolge ausgeben:

1. **Twitter-Accounts** (kurz auflisten zur Bestätigung, dann Digest: Bullisch / Bärisch)
2. **FRED Makrodaten** (Tabelle mit aktuellen Werten)
3. **Wirtschaftskalender heute** (Tabelle: Zeit, Event, Konsens, Vorwert)
4. **Intermarket-Scan** (Übersicht: wie viele Signale bullisch / bärisch)
5. **NAS100 + QQQ Chart** (Kurs, Indikatoren, Key-Levels, QQQ-Volumen-Kontext)
6. **Gesamtbias + Setup-Ideen** (Long/Short mit Bedingung, Entry, SL, TP)

---

## Regel: Verlauf seit letzter Memory immer rekonstruieren

Nicht einfach aktuellen Kurs mit letztem Memory-Kurs vergleichen — den KOMPLETTEN Verlauf dazwischen aus OHLCV-Daten rekonstruieren.

**Warum:** Kurse können durch Events am selben Tag massiv fallen UND steigen. Nur der aktuelle Kurs zeigt nicht das Bild. Beispiel 24.06.2026: Tief 28.993, dann Micron-Spike +900 Pkt — wer nur den Schlusskurs sieht, denkt "alles bullisch".
