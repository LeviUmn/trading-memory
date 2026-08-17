---
name: feedback-session-update-dax
description: "Trigger 'start update dich dax' (umbenannt 07.08.2026, vorher '#1') → DAX-Pendant zu feedback_session_update.md (NAS100 bleibt schlicht 'start update dich'). Eigener 6-Schritt-Ablauf, physisch getrennte Datenquellen."
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 51fca406-b869-4659-8541-11810063992f
  modified: 2026-08-07T11:53:12.416Z
---

## KRITISCH: Vollständiger Ablauf bei "start update dich dax"

**Trigger:** NUR wenn Levi explizit **"start update dich dax"** schreibt (DAX-Block, 10-15 Uhr). Das NAS100-Pendant bleibt schlicht **"start update dich"** (siehe [[feedback_session_update]]) — **umbenannt 07.08.2026** (Levi-Wunsch): vorher "#1"/"#2", jetzt eindeutiger per Namenszusatz "dax" statt Zahl.

**Mechanik 1:1 von NAS100 übernommen** (Struktur, Reihenfolge, "warum jeder Schritt Pflicht ist"). **Inhaltliche Schwellen/Zeiten für Deutschland/Euroraum sind unverifiziert, in Bewährung** — siehe [[project_dax_erweiterung]] Architektur-Entscheidung. Bei jedem Live-Einsatz aktiv gegenchecken, nicht blind übernehmen.

**How to apply:** Alle 6 Schritte in dieser Reihenfolge, so viel wie möglich parallel ausführen.

---

## Der 6-Schritt-Ablauf

### Schritt 1 — Memory lesen
- MEMORY.md + letzte Datei aus `memory/dax_trades/` (sobald vorhanden — aktuell noch leer, siehe [[project_dax_erweiterung]])
- [[project_risikomanagement_dax]] für aktuellen Stand (Budget, Phase, offene Punkte)

### Schritt 2 — Tweets der DAX-Watchlist (seit letztem Fetch)
- `scripts/x_last_fetch_dax.json` lesen → `last_fetch` Timestamp (eigene Datei, getrennt von `x_last_fetch.json`/NAS100)
- `get_users_posts` für alle Accounts aus `scripts/x_watchlist_dax.json` mit `start_time = last_fetch`:
  - **@handelsblatt** (ID 5776022) — Breaking News Deutschland
  - **@Schuldensuehner** (ID 40129171) — Sentiment/Makro, Holger Zschäpitz
  - **@ecb** (ID 83466368) — Institutionell, offizielle EZB-Statements
  - **@zerohedge** (ID 18856867) — Risk-off, global (wiederverwendet aus NAS100)
  - **@DeItaone** (ID 2704294333) — Breaking global (wiederverwendet aus NAS100)
  - **@CWRoehl** (ID 350752895) — Market Commentary
  - **@mario_lochner** (ID 857779712) — Market Commentary
- Tweets als strukturierten Digest ausgeben (Bullisch / Bearisch / Neutral)
- Nach dem Lesen `x_last_fetch_dax.json` mit aktuellem UTC-Timestamp aktualisieren

### Schritt 3 — EZB/Euroraum-Makrodaten
- `python scripts/eu_snapshot_dax.py` ausführen
- Liefert: EZB Hauptrefinanzierungssatz + Einlagesatz (live über ECB Data Portal API, siehe [[reference_ecb_data_portal_api]]), Euroraum + Deutschland HICP, deutsches Real-BIP, deutsche Arbeitslosenquote, deutsche 10J-Bundrendite (über FRED)
- Strukturellen Makro-Hintergrund verstehen, wie `fred_snapshot.py` für die USA — keine Tagesevents

### Schritt 4 — Live-Kalender (was steht HEUTE an)
- Gleiche Kalenderquelle/Methodik wie NAS100 (siehe [[feedback_session_update]] für die Zuverlässigkeits-Learnings zu tradingeconomics/investing.com), zusätzlich auf deutsche/Euroraum-Events prüfen
- **Unverifiziert, live gegenchecken (Standard-Release-Zeiten, NICHT blind übernehmen):**
  - ZEW-Konjunkturerwartungen (Deutschland) — monatlich, ca. 3. Dienstag, ~11:00 Uhr DE-Zeit
  - Ifo-Geschäftsklimaindex — monatlich, ca. 22.-25., ~10:00 Uhr DE-Zeit
  - Deutsche Industrieproduktion / Fabrikaufträge — monatlich, ~08:00 Uhr DE-Zeit
  - EZB-Zinsentscheidung + Pressekonferenz — 8x/Jahr, ca. 14:15/14:45 Uhr DE-Zeit (Termine + genaue Zeiten vor jedem Meeting live gegenchecken, analog FOMC-Sperrfrist bei NAS100)
- US-Events aus dem NAS100-Kalender bleiben relevant (Levi-Zitat 05.08.: "US regiert die Märkte weltweit") — Schritt 4 deckt beide Ebenen ab, nicht nur deutsche Termine
- Bei ZEW/Ifo/EZB-Entscheidung: Blackout-Logik analog NAS100 (siehe [[feedback_session_update]] Market-Moving-Events-Tabelle) — noch nicht kalibriert, welche DAX-Events tatsächlich marktbewegend genug für eine Entry-Sperre sind. Erste Session-Erfahrung nutzen, um das analog zur 28.07.-NAS100-Korrektur (CB Consumer Confidence keine echte Marktreaktion) zu schärfen

**Pflicht-Gegencheck bei jedem Live-Kalender-Fetch (ergänzt 07.08.2026, nach Testlauf-Vorfall, analog [[feedback_session_update]] 30.07.-Regel):** Beim ersten `start update dich dax`-Testlauf zeigte der tradingeconomics-Fetch für deutsche Handelsbilanz/Exporte/Importe/Industrieproduktion "06:00 Uhr" an — obwohl direkt oben in diesem Dokument bereits korrekt "~08:00 Uhr DE-Zeit" als Referenzzeit steht. Der Fetch wurde ungeprüft übernommen, statt gegen die eigene Referenzliste gespiegelt zu werden — exakt derselbe Fehlertyp wie beim NAS100-GDP/PCE-Vorfall vom 30.07.2026. **How to apply:** Bei JEDEM Live-Kalender-Fetch die Uhrzeit deutscher Standard-Reports (Handelsbilanz/Exporte/Importe/Industrieproduktion ~08:00, ZEW ~11:00, Ifo ~10:00, EZB-Entscheidung ~14:15/14:45) gegen diese Liste hier spiegeln — weicht der Fetch ab, gilt das als Fetch-Artefakt/Fehlinterpretation, nicht als neue Information, außer der Zeitpunkt wird explizit von Levi bestätigt.

### Schritt 5 — TradingView öffnen: Intermarket-Scan (DAX-Variante)
`tv_launch`, dann Intermarket-Dashboard mit europäischem statt US-Fokus. **Cheat-Sheet unten ist ein erster Entwurf, NICHT kalibriert** — Analogieschluss aus der NAS100-Logik, nicht durch echte DAX-Trades bestätigt.

**Erweitert 07.08.2026 (Levi-Vorgabe nach dem ersten Testlauf, alle Symbole live per `quote_get`/`symbol_search` verifiziert):**

| Kategorie | Instrument | Symbol |
|---|---|---|
| **Angst** | VIX (global) | CBOE:VIX |
| | VXN (Nasdaq-Vola, Kontext) | CBOE:VXN |
| | VDAX-NEW (DAX-Vola) | INDEX:DV1X |
| | MOVE (Anleihen-Vola) | ICE:MOVE |
| **Deutschland** | DAX | `IG:DAX` (Haupt-Chart, siehe Schritt 6) |
| | MDAX | XETR:MDAX |
| | SDAX | XETR:SDXP |
| | TecDAX | XETR:TDXP |
| **Europa** | EuroStoxx50 | EUREX:FESX1! |
| | Stoxx Europe 600 | TVC:SXXP |
| | CAC40 (Frankreich) | TVC:CAC40 |
| | FTSE100 (UK) | FTSE:UKX |
| **Asien (Übernacht-Werte, vor DAX-Open 10 Uhr relevant — nach ca. 10-11 Uhr DE-Zeit bereits geschlossen, `quote_get` kann dann per Timeout fehlschlagen, kein Symbolfehler)** | Nikkei | TSE:NI225 |
| | Hang Seng | HKEX:HSI |
| | SSE Composite | SSE:000001 |
| | KOSPI | KRX:KOSPI |
| **US** | NAS100 | via Chart-Bars, nicht `quote_get` (siehe [[feedback_datenquelle_nas100]]) |
| | S&P 500 | SP:SPX |
| | Dow Jones (Industrie-/Blue-Chip-Überlappung mit DAX-Zusammensetzung, engerer Bezug als NAS100) | DJ:DJI |
| **Währung** | EUR/USD | FX:EURUSD |
| | DXY | TVC:DXY |
| **Rohstoffe** | Gold | OANDA:XAUUSD |
| | Silber | OANDA:XAGUSD |
| | WTI Öl (USOIL) | TVC:USOIL |
| | Brent (europäischer Ölpreis-Standard) | NYMEX:BB1! |
| **Zinsen Deutschland** | DE 2 Jahre (Schatz) | TVC:DE02Y |
| | DE 10 Jahre (Bund) | TVC:DE10Y |
| | DE 30 Jahre | TVC:DE30Y |
| **Zinsen USA** | US 10Y | TVC:US10Y |
| | US 30Y | TVC:US30Y |
| **Zinsen Japan** | JP 10Y (Yen-Carry-Trade-Kontext) | TVC:JP10Y |

**Entscheidung 07.08.2026 (Rückfrage im Chat geklärt):** DXY, S&P 500 und Dow Jones auf Levis Wunsch ergänzt. Dow Jones zusätzlich fachlich begründet (Industrie-/Blue-Chip-Nähe zu DAX, engerer Bezug als der tech-lastige NAS100). Russell 2000 (`TVC:RUT`, live verifiziert, aber bewusst NICHT aufgenommen) — dient im NAS100-Regelwerk primär als US-Binnenwirtschafts-Breite-Signal, für DAX zu indirekt/US-zentriert, keine ausreichend klare Übertragbarkeit. Bei Bedarf jederzeit nachträglich ergänzbar.

**Cheat-Sheet-Entwurf (unverifiziert):**

| Signal | Bullisch DAX (Hypothese) | Bärisch DAX (Hypothese) |
|---|---|---|
| VDAX-NEW | Fällt / ruhig | Steigt stark |
| EUR/USD | Schwächer (Exporteure profitieren) | Stärker (Exportbremse) |
| Bund-Rendite | Stabil / leicht fallend | Stark steigend (EZB-hawkish-Signal) |
| VIX (global) | Fällt | Steigt (globales Risk-off trifft auch DAX) |
| EuroStoxx50/CAC40 | Laufen mit DAX synchron | Divergenz = Warnsignal (DAX-spezifische Schwäche) |

**Why unverifiziert:** Die NAS100-Cheat-Sheet-Werte (VIX-Schwellen, VXN/VIX-Ratio etc.) wurden über Monate an echten Trades kalibriert. Für DAX gibt es diese Kalibrierung noch nicht — insbesondere die EUR/USD-Richtung ist nicht trivial (schwacher Euro hilft Exporteuren, kann aber gleichzeitig ein Zeichen für Euroraum-Schwäche/Kapitalflucht sein, je nach Ursache). Erste echte DAX-Paper-/Live-Trades nutzen, um das zu schärfen, siehe [[project_dax_erweiterung]] To-do 13.

### Schritt 6 — DAX-Chart (`IG:DAX`) + Indikatoren
- `chart_get_state` → aktuelles Symbol/Timeframe/Indikatorliste
- `data_get_study_values` → RSI, MACD, EMA, Bollinger Bands, ATR, Volume (alle nativ auf `IG:DAX` — **kein zweites Pane für Volumen nötig**, anders als NAS100/QQQ, siehe [[project_dax_erweiterung]] To-do 1+2)
- `data_get_ohlcv` mit `summary: true` + zusätzlich `count: 300` für vollständigen Verlauf seit letzter Session
- Welche Key-Levels wurden getestet/gebrochen seit letzter Session?
- `capture_screenshot` für visuellen Überblick
- **Chart-Layout:** eigenes gespeichertes Layout, per `layout_switch("DAX")` ladbar (siehe [[project_dax_erweiterung]] To-do 10, erledigt 07.08.2026)

### Schritt 6a — Pivot-Berechnung (automatisch, analog [[feedback_chartanalyse]] Punkt 7a1a)

Anders als bei NAS100 (dort separater manueller Schritt beim Session-Start) ist die Pivot-Berechnung für DAX **fester, automatischer Bestandteil von "start update dich dax"** — DAX hat nur ein Handelsfenster pro Tag (10-15 Uhr), die Kopplung an den Session-Start deckt damit automatisch genau einen Berechnungslauf pro Handelstag ab.

1. `data_get_ohlcv` auf `IG:DAX`, Tages-Timeframe → High/Low/Close des letzten abgeschlossenen Handelstags
2. Standard-Pivot-Formel (identisch zu NAS100): PP = (H+L+C)/3, R1 = 2×PP−L, S1 = 2×PP−H, R2 = PP+(H−L), S2 = PP−(H−L)
3. Die 1-2 kursnächsten Level zusätzlich per `draw_shape` (horizontal_line) auf den `IG:DAX`-Chart zeichnen — belegt keinen Indikator-Slot (gleiches Prinzip wie NAS100, siehe [[feedback_chart_layout]] Essential-Plan-Limit)
4. Gültig bis zum nächsten "start update dich dax"-Lauf. Berechnung ist idempotent (hängt nur vom letzten abgeschlossenen Handelstag ab) — mehrfaches Ausführen am selben Tag liefert dieselben Werte, kein Schutz gegen Doppelausführung nötig

**Why automatisch statt manuell:** NAS100 läuft fast durchgehend über mehrere Tage/Sessions, Pivot-Berechnung ist dort bewusst ein separater, disziplinierter Einzelschritt. DAX hat nur ein festes Tagesfenster — die Kopplung an den Session-Update-Trigger deckt den Bedarf vollständig ab, ohne zusätzliches Erinnerungsrisiko (vergleichbar mit anderen Pflichtzeilen-Mechanismen aus [[feedback_live_trading]], die stilles Vergessen strukturell verhindern).

**Ergebnis fließt in Schritt 6b (Gesamtbias-Synthese) als Preis-Trigger-Basis ein.**

### Schritt 6b — Gesamtbias-Synthese: Short / Neutral / Long + Preis-Bedingungen

**Grundproblem, das diese Methodik löst:** DAX hat kein Dual-Gate (kein zweites unabhängiges Preisinstrument wie QQQ bei NAS100, siehe [[feedback_live_trading_dax]] Abschnitt 2.1/2.2 — `IG:DAX` ist ein CFD ohne gleichwertiges Cross-Check-Instrument). Die Bias-Synthese ersetzt die fehlende Instrument-Unabhängigkeit durch ein **Quorum aus mehreren unabhängigen Kontext-Signalen** statt eines einzelnen zweiten Preisfeeds — strukturell schwächer als ein echtes Dual-Gate, aber besser als reine Chart-Technik allein.

**Drei Ebenen, in dieser Reihenfolge:**

**1. Primäre Ebene — DAX-Chart-Technik (Schritt 6), entscheidet die Grundrichtung:**
- **Long-Bias-Grundlage:** Kurs über EMA50 UND RSI(14) > 50 UND MACD-Histogram positiv (oder erkennbar Richtung positiv drehend, siehe [[feedback_live_trading]] Punkt 7c-Logik)
- **Short-Bias-Grundlage:** spiegelbildlich (Kurs unter EMA50 UND RSI < 50 UND MACD-H negativ)
- Widersprechen sich die drei Bedingungen (z.B. Kurs über EMA50, aber RSI < 50) → **Neutral**, kein ausreichend klares technisches Bild, Ebene 2 wird trotzdem informativ durchlaufen, aber ergibt dann höchstens einen schwachen Beobachtungs-Bias, kein Setup

**2. Sekundäre Ebene — Intermarket-Kern-Set (Tier 1), bestätigt/schwächt die Grundrichtung — das ist der Dual-Gate-Ersatz:**
Die bestehenden 5 Cheat-Sheet-Signale zählen als Quorum: VDAX-NEW, EUR/USD, Bund-Rendite (DE10Y), VIX (global), EuroStoxx50/CAC40-Divergenz. Für jedes Signal einzeln bullisch/bärisch/neutral gegen die Chart-Technik-Grundrichtung bewerten:
- **≥3 von 5 bestätigen** → Bias bestätigt, volle Überzeugung, regulärer 2-Kerzen-Bestätigungsstandard aus [[feedback_live_trading_dax]] 2.2 reicht
- **2 von 5 oder gemischt** → Bias bleibt bestehen, aber nur "schwach bestätigt" — Positionsgröße vorsichtiger ansetzen (analog Chop-Sizing-Logik aus [[feedback_chartanalyse]] Punkt 10), 2-Kerzen-Standard bleibt Minimum, kein Abschwächen
- **0-1 von 5 bestätigen (Mehrheit widerspricht)** → Bias wird auf **Neutral herabgestuft**, unabhängig davon was die Chart-Technik allein zeigt — kein Entry. Das ist der eigentliche Kompensationsmechanismus: reine Chart-Technik ohne Intermarket-Rückendeckung darf bei DAX (anders als bei NAS100 mit Dual-Gate) nicht allein einen Bias tragen.

**3. Tertiäre Ebene — Tier 2 + Makro/News, kein Score, nur Einordnung/Veto:**
- **Tier 2** (MDAX/SDAX/TecDAX, Stoxx Europe 600/FTSE100, DXY, Gold/Silber, US10Y/US30Y, JP10Y): fließt NICHT in den Quorum-Score ein — zu indirekt/unkalibriert für einen mechanischen Score-Eintrag. Dient der Erklärung ("warum" hinter dem Bias, z.B. "MDAX/SDAX laufen mit, kein reines DAX-Einzelthema") und als Veto bei echten Extremwerten (z.B. DXY-Spike, JP10Y-Sprung als Yen-Carry-Unwind-Signal, Öl-Schock) — dann Bias-Herabstufung auf Neutral trotz erfüllter Ebene 1+2, mit expliziter Begründung.
- **Asien-Übernacht-Werte (Nikkei/HSI/SSE/KOSPI):** nur als Pre-Open-Kontext vor 10 Uhr relevant, fließen NICHT in den laufenden Score während der 10-15-Uhr-Session ein (nach Handelsschluss ohnehin oft nicht mehr aktualisiert, siehe Symbolliste in Schritt 5).
- **NAS100/S&P500/Dow:** primär für die bestehende Korrelationsregel relevant (nie DAX+NAS100 gleiche Richtung parallel, siehe [[feedback_instrumenten_fokus]]), nicht als DAX-Bias-Input. Dow zusätzlich als Blue-Chip-Kontext (engere Branchen-Nähe zu DAX als NAS100).
- **Makro/News (Tweets, EZB-Daten, Kalender):** kann den Bias NIEMALS gegen Ebene 1+2 erzwingen, aber bestätigender/widersprechender Kontext wird explizit benannt (z.B. "EZB-hawkish-Kommentar heute stützt den technischen Short-Bias zusätzlich").

**Warum diese Gewichtung (Begründung für die Tier-Aufteilung):** Tier 1 sind die Signale mit dem direktesten strukturellen DAX-Bezug (Eigen-Vola, Eigen-Währungspaar, Eigen-Zinskurve, engste Korrelationsindizes) — das sind die 5, die in der bisherigen Analyse bereits als Kern identifiziert wurden. Tier 2 ist einen Schritt indirekter (Marktbreite, globaler Kontext) — wertvoll zur Einordnung, aber ohne Kalibrierung zu unsicher für einen harten Score-Beitrag. Diese Aufteilung ist **Startannahme, unverifiziert**, siehe unten.

**Preis-Bedingungen ("was müsste passieren"):** Aufbauend auf den Pivots aus Schritt 6a + EMA50 + Bollinger-Bands, im selben Wenn-Dann-Format wie [[feedback_live_trading]] Punkt 7:

```
DAX: <aktueller Kurs>
🟢 Wenn Kurs über [nächstgelegener Pivot-Widerstand aus Schritt 6a, z.B. R1] schließt
    (2 aufeinanderfolgende bestätigte 5-Min-Kerzenschlüsse, siehe [[feedback_live_trading_dax]] 2.2)
    UND Ebene 1+2 oben mindestens "schwach bestätigt": LONG möglich
🔴 Wenn Kurs unter [nächstgelegener Pivot-Support, z.B. S1] schließt (gleicher 2-Kerzen-Standard)
    UND Ebene 1+2 mindestens "schwach bestätigt": SHORT möglich
→ Sonst: Neutral, beobachten
```

**Ausgabe (ersetzt den bisherigen Platzhalter-Punkt 6):** Immer alle drei Ebenen kurz sichtbar machen, nicht nur das Endergebnis — z.B. "Chart-Technik: Short-Grundlage (EMA50/RSI/MACD-H) | Intermarket-Quorum: 3/5 bestätigen Short | Tier-2/Makro: kein Veto → Bias: SHORT (schwach-bis-voll bestätigt) | Trigger: 2 Kerzen unter S1 (18.230)". Damit bleibt nachvollziehbar, WARUM der Bias steht, nicht nur DASS er steht.

**Ausdrücklich unverifiziert (Startannahme, wie das übrige Cheat-Sheet):** Die Score-Schwellen (≥3/5, 2/5, 0-1/5), die Tier-1/Tier-2-Aufteilung selbst, und die Kombinationslogik der drei Ebenen insgesamt — das ist ein erster methodischer Entwurf, keine kalibrierte Regel. **Review-Pflicht:** Nach den ersten 5-10 Anwendungsfällen (Paper- oder Live-Sessions, siehe [[project_dax_erweiterung]] To-do 13) explizit prüfen, ob die Quorum-Schwellen zu streng/zu locker sind und ob die Tier-2-Signale doch in den Score gehören — analog zur bestehenden Review-Pflicht-Praxis bei frisch eingeführten NAS100-Regeln (z.B. [[feedback_live_trading]] Punkt 12, 7c).

---

## Ausgabe-Struktur des Briefings

Gleiche Reihenfolge wie NAS100 (siehe [[feedback_session_update]]):

1. **Twitter-Accounts** (kurz auflisten, dann Digest: Bullisch / Bärisch)
2. **EZB/Euroraum-Makrodaten** (Tabelle mit aktuellen Werten)
3. **Wirtschaftskalender heute** (Deutschland/Euroraum + relevante US-Termine)
4. **Intermarket-Scan** (Übersicht, mit dem Hinweis "Cheat-Sheet unverifiziert" bei jeder Bias-Aussage)
5. **DAX-Chart + Pivots** (Kurs, Indikatoren inkl. natives Volumen, Key-Levels, Tages-Pivots aus Schritt 6a)
6. **Gesamtbias-Synthese** (Schritt 6b: alle drei Ebenen sichtbar — Chart-Technik, Intermarket-Quorum, Tier-2/Makro-Veto — plus Long/Short/Neutral-Urteil mit konkreten Preis-Bedingungen, Entry/SL/TP)

---

## Regel: Verlauf seit letzter Memory immer rekonstruieren

Gleiche Regel wie NAS100 (siehe [[feedback_session_update]]) — nicht nur aktuellen Kurs vs. letzten Memory-Kurs vergleichen, sondern den kompletten Verlauf aus OHLCV-Daten rekonstruieren.
