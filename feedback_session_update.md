---
name: feedback-session-update
description: "Trigger 'start update dich' (NAS100-Hauptauslöser, unverändert; DAX-Pendant heißt seit 07.08.2026 'start update dich dax') → IMMER vollständiger 6-Schritt-Ablauf: Memory → Tweets → FRED → Live-Kalender → TV Intermarket → NAS100+QQQ Chart → Bias. Keinen Schritt überspringen."
metadata:
  node_type: memory
  type: feedback
  originSessionId: 607aa8f6-9958-4c1c-9c75-4afabcffb717
  modified: 2026-08-07T11:10:25.896Z
---

## KRITISCH: Vollständiger Ablauf bei "start update dich"

**Trigger:** NUR wenn User explizit **"start update dich"** schreibt — bleibt der unveränderte NAS100-Hauptauslöser für den 6-Schritt-Ablauf unten. **Umbenennung 07.08.2026:** Die kurzzeitig eingeführte "#2"-Zusatzform (siehe [[project_dax_erweiterung]] To-do 12) ist wieder entfallen — Levi wollte stattdessen eine eindeutigere Benennung. Das DAX-Pendant heißt jetzt **"start update dich dax"** (siehe [[feedback_session_update_dax]]), nicht mehr "#1"/"#2".

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
- CPI / Core CPI — ähnlich PCE (14:30 CET)
- PPI — Erzeugerpreise, Fed-relevant
- NFP (Non-Farm Payrolls) — erster Freitag des Monats, 14:30 CET — sehr stark
- Jobless Claims — wöchentlich, 14:30 CET
- ADP Employment — monatlich, 14:15 CET
- FOMC-Protokolle / Fed-Entscheidungen / Fed-Reden
- GDP-Daten (Revisionen beachten!)
- **Doppel-Events gleichzeitig 16:00:** Spike-Volatilität in beide Richtungen → kein Entry vor 16:15-16:30 — **gilt nur, wenn mindestens eines der beiden gleichzeitigen Events selbst auf dieser Liste steht** (siehe Korrektur 03.08.2026 unten)
- Grundregel: Kein Entry bis 30 min nach Veröffentlichung + Volatilität beruhigt

**Korrigiert 28.07.2026 (User-Feedback, live während Trading-Session):** Die Entry-Sperrfrist gilt NUR für die oben gelisteten Events — das sind die tatsächlich marktbewegenden Releases. Andere Kalender-Einträge, die investing.com formal als "High Impact" (3 Sterne) markiert — z.B. **CB Consumer Confidence, ISM Manufacturing/Services, JOLTs Job Openings** — haben laut User in der Praxis keine ausreichende Marktgewichtung, um eine Entry-Sperre zu rechtfertigen. Die investing.com-Stern-Bewertung ist NICHT die Entscheidungsgrundlage für die Sperrfrist. Konkreter Anlass: CB Consumer Confidence (28.07., 16:00 Uhr, Miss 90,8 vs. 92,4) hatte keine erkennbare Marktreaktion, die Sperrfrist wurde dennoch fälschlich angesetzt.

**Korrigiert 03.08.2026 (User-Feedback, live während Trading-Session):** Die "Doppel-Events gleichzeitig 16:00"-Zeile wurde fälschlich auf den ISM Manufacturing PMI + Employment Release (03.08., 16:00 Uhr) angewendet, obwohl ISM laut der 28.07.-Korrektur explizit NICHT blackout-pflichtig ist. Root Cause: die Doppel-Event-Zeile stand direkt unter den hart gelisteten Events und wurde dadurch fälschlich als eigenständige, event-unabhängige Regel gelesen. **Klarstellung:** Die Doppel-Event-Regel ist kein eigener Blackout-Auslöser — sie verstärkt nur die Sperrfrist, falls eines der gleichzeitig veröffentlichten Events selbst auf der Liste steht (z.B. wenn NFP und ein zweiter Report zufällig zusammenfallen). Zwei gleichzeitige Nicht-Blackout-Events (wie ISM PMI + ISM Employment) lösen keine Sperrfrist aus, auch nicht in Kombination.

**Fehler 30.07.2026 (User-Korrektur) — WebFetch-Kalenderzeiten gegen die eigene Standard-Liste prüfen, nicht blind übernehmen:** Ein Kalender-Fetch zeigte für GDP/PCE/Personal Income/Initial Jobless Claims "12:30 PM US" und wurde fälschlich als 18:30 DE-Zeit (12:30 ET) interpretiert. Tatsächlich laufen diese vier Reports als gebündelter Donnerstags-Release **immer um 8:30 Uhr ET = 14:30 Uhr DE-Zeit** — exakt die Standardzeit, die oben in diesem Memory bereits für PCE/CPI/NFP/Jobless Claims/ADP dokumentiert ist. Der Fehler entstand, weil die gefetchte Zeitangabe unhinterfragt übernommen wurde, statt sie gegen die eigene bereits korrekte Referenzliste (14:30 CET) gegenzuchecken. **How to apply:** Bei jedem Live-Kalender-Fetch die Uhrzeit der Standard-US-Reports (GDP/PCE/CPI/PPI/NFP/Jobless Claims/ADP) immer gegen die oben gelistete 14:30-CET-Regel spiegeln — weicht der Fetch davon ab, gilt das als Fetch-Artefakt/Fehlinterpretation, nicht als neue Information, außer der User bestätigt explizit eine Abweichung (z.B. Sommerzeit-Wechsel-Tag).

**Quelle für Event-Wichtigkeit ab jetzt tradingeconomics.com, nicht investing.com:** User bezieht sich explizit auf tradingeconomics' eigene Kennzeichnung der wichtigen Daten. Schritt 4 (Live-Kalender) künftig nach Möglichkeit von tradingeconomics.com abrufen bzw. dessen Impact-Kennzeichnung als Referenz für "gehört das auf die Sperrfrist-Liste" heranziehen, nicht investing.com's Sterne.

**Fehler 03.08.2026 — tradingeconomics.com-WebFetch zwei Zuverlässigkeitsprobleme an einem Tag, User musste korrigieren:** (1) Der Fetch zeigte für den noch nicht veröffentlichten ISM Manufacturing PMI (16:00 Uhr) bereits einen "Actual"-Wert an — technisch unmöglich vor Release, klarer Hinweis auf eine gecachte/veraltete Seite. (2) Der Fetch listete für Freitag "keine High-Impact-Events", obwohl an diesem Freitag (erster Freitag im Monat) die Non-Farm Payrolls + Unemployment Rate anstanden — einer der wichtigsten Termine überhaupt, komplett fehlend. User musste die vollständige Wochenübersicht (inkl. Di JOLTS, Mi ISM Services + EIA Crude Oil, Do Jobless Claims, Fr NFP+Unemployment) manuell nachliefern. **How to apply:** Den tradingeconomics-Fetch aus Schritt 4 nicht mehr als alleinige Quelle für die Wochenübersicht behandeln — die in Schritt 2 ohnehin bereits abgerufenen Tweet-Kalenderübersichten (DeItaone "THIS WEEK'S U.S. ECONOMIC CALENDAR", Kobeissi "Key Events This Week") aktiv als Cross-Check/Ergänzung heranziehen, besonders für Freitag-NFP-Wochen. Bei Diskrepanz zwischen den Quellen oder fehlenden erwarteten Standard-Terminen (NFP am 1. Freitag, ADP/Jobless Claims wöchentlich) aktiv beim User nachfragen statt den Fetch unhinterfragt als vollständig zu behandeln.

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
