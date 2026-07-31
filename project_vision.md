---
name: project-vision
description: "Langfristiges Ziel — professioneller Trader auf Bloomberg/GS-Niveau durch systematischen, datengetriebenen Aufbau über alle Phasen"
metadata:
  node_type: memory
  type: project
  originSessionId: trading-session-2026-06-26
  modified: 2026-07-31T15:59:52.584Z
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
| 2 (abgeschlossen, 10/10, Stand 23.07.) | Größere Positionen (2.000-3.000€), Multi-Setup | Fibonacci präziser, Orderflow-Ansätze | X API evaluieren (Breaking News Latenz) |
| 3 (läuft, Trade 26-35, Stand 31.07. bei #28) | Mehrere Instrumente, gestaffelter Start #26-30 | Intermarket-Divergenzen aktiv traden | Sentiment-Daten, erweiterte Makro-Integration |
| 4 (Trade 36-40, 5 Trades) | 10.000€ pro Trade, 2% Risiko | Komplettes bisheriges Toolkit | UnusualWhales (konditional) |
| **Danach: normales Trading** (kein Phasensystem mehr) | 50.000€ Kapital, volle Motivation | — | Alle Quellen integriert |

**Korrigiert 31.07.2026 (User-Entscheidung, ersetzt die vorherige Korrektur vom 09.07.):** Nummerierung jetzt konsistent mit [[project_risikomanagement]] (Phase 2 = 16-25, Phase 3 = 26-35, Phase 4 = ab 36). Phase 4 ist jetzt final definiert als eigene 10.000€-Zwischenstufe (nicht mehr "Vollsystem") — siehe Abschnitt "Phase 4 & danach" unten für die volle Begründung.

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
- **UnusualWhales (~$50/Monat) — Integration verschoben (27.07.2026, Fable-Review + Levi-Entscheidung), NICHT mehr "praktisch gesetzt" für den Phase-3-Start:** Kombiniert Dark Pool Flow + Options Data (Put/Call Ratio, Unusual Activity, Max Pain, Gamma Exposure/GEX als unsichtbare S/R-Zonen). Ursprünglicher Plan (10.07.2026) sah den Bau kurz vor Phase-3-Start vor (grob ab Trade ~25-28) — dieser Trigger war aber ein reiner Kalender-Meilenstein, keine inhaltliche Reife-Prüfung. Fables Gegenargument (27.07.2026): Genau am Tag dieser Entscheidung mussten zwei Kernregeln (TP1/TP2-RR-Logik, Stall-Exit-Vorrangklausel) nachgeschärft werden, die Ø-Rendite verfehlt weiterhin das Ziel (0,61% vs. 1-1,5%), und dieses Projekt hat wiederholt das Muster "neue Datenquelle/Regel wird eingeführt, aber im heißen Loop nicht konsequent genutzt" gezeigt (Scheincompliance bei Punkt 9, MTF-Lücken, ausgelassene Voll-Checks). Eine vierte Datenquelle vor gesicherter Ausführungsdisziplin bei den bestehenden drei (Chart-TA, Intermarket, Dual-Gate) würde dieses Risiko verstärken, nicht die eigentliche Schwachstelle (Ausführung, nicht Datenmangel) beheben.
  - **Neuer Fahrplan:** Erst Phase 3 komplett durchlaufen (Trades #26-#35) → danach ein Gesamt-Review über ALLE Phasen (1-3), das bewertet, was am sinnvollsten für die Weiterentwicklung ist → NUR wenn danach UnusualWhales weiterhin sinnvoll erscheint, Integration angehen UND dafür eine eigene Test-Phase einplanen, die explizit prüft, ob es den Track Record wirklich verbessert (nicht automatisch als Daueranschaffung übernehmen).
  - **Priorisierung falls/wenn integriert (Fable-Empfehlung 27.07.2026):** 1) GEX/Max Pain (höchster Mehrwert, strukturell kompatibel mit 8b/7a1a — sind im Kern zusätzliche S/R-Level), 2) Dark Pool Prints (Frühindikator fürs Regime-Gate 8d, aber schwerer objektiv quantifizierbar), 3) Options Flow/Unusual Activity (niedrig, ohne eigenen Optionshandel schwer in harte Regeln übersetzbar), 4) Put/Call-Ratio (niedrigste Priorität, zu viel Redundanz mit VIX/VXN aus dem Intermarket-Scan). Einbauort falls umgesetzt: NIE im 1-Min-Loop, nur als zusätzlicher Schritt im täglichen "Start Update Dich"/Voll-Check (analog zur einmal-täglichen Pivot-Berechnung 7a1a). Günstiger Vortest vor jeder Automatisierung: GEX-Level manuell vom bereits genutzten UW-X-Account für 5-10 Trades gegenchecken, bevor $50/Monat + eigener MCP-Server-Bau investiert werden.
- **Intermarket-Divergenzen aktiv traden** (nicht nur als Kontext lesen)

**Dauerhaft gestrichen:**
- Bloomberg Terminal (~$2.000/Monat) — wirtschaftlich nicht realistisch für Retail-Phase

**Phase 4 (ab Trade 36) — final definiert 31.07.2026, User-Entscheidung:**
- **Zwei Bedingungen, beide an Phase 4 gekoppelt:** (1) UnusualWhales erfolgreich angebunden — konditional, siehe Priorisierung oben (GEX/Max Pain zuerst); scheitert die Integration, läuft Phase 4 trotzdem weiter, nur ohne UW. (2) 10.000€-Positionsgröße als eigener Bewährungsschritt vor dem großen Kapitalsprung.
- **Feste Trade-Range wie die vorherigen Phasen: 5 Trades (#36-40)** — korrigiert 31.07.2026, ursprünglich fälschlich als "offen/kein Enddatum" festgehalten.
- **Danach: Übergang ins normale Trading, kein Phasensystem mehr, 50.000€ Kapital, "volle Motivation"** (User-Formulierung). Das ist der eigentliche Zielpunkt des gesamten Fahrplans, kein weiterer Zwischenschritt.
- **Harte Vorbedingung für den 50k-Übergang:** Wochen- und Monatsabschlüsse müssen als Tracking-Ebene ergänzt werden (zusätzlich zu den bestehenden Tagesabschlüssen, siehe [[feedback_tagesabschluss]]), BEVOR auf 50.000€ gewechselt wird — nicht erst währenddessen nachgebaut. Volle Begründung + Detail-Regel in [[project_risikomanagement]], Abschnitt "Skalierungs-Fahrplan".
- **IBKR / Futures (MNQ):** Vorbereitung auf Schweiz-Umzug — parallel testen bevor Umzug ansteht. IBKR bietet keine deutschen Hebelzertifikate, Alternative wäre Micro Nasdaq Futures.
- **Scope-Klarstellung (27.07.2026, Levi-Entscheidung):** Falls IBKR (oder eine andere API-Anbindung) kommt, ist der Zweck ausschließlich **automatischer Fill-Rückabgleich** (echter Fill-Preis/Stückzahl/Timing direkt aus dem Konto lesen, löst das in der heutigen Fable-Bewertung genannte "Kein Fill-Abgleich"-Problem, siehe [[feedback_broker_wert_prioritaet]]) — NICHT automatische Order-Ausführung. Levi will die Order weiterhin bewusst selbst auslösen, das Vertrauen in eine autonome Order-Platzierung durch das System ist explizit (noch) nicht gewünscht. Nur die Rückübertragung der tatsächlichen Trade-Daten (statt manueller Chat-Meldung) soll automatisiert werden.
