---
name: feedback-chart-layout
description: "Levi bevorzugt NAS100 als Hauptchart; 2. Pane erlaubt für QQQ-Volumen-Referenz (seit 15.07.2026, vorher NQ1!, Ausnahme von der Single-Pane-Regel), 5-Indikatoren-Set ersetzt Volume/VWAP wegen fehlender CFD-Volumendaten. Seit 23.07.2026: Pivot Points gegen ATR(14) getauscht (Fable-Review)"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 028ce287-5d0c-400c-bf08-bdc1d4c4670c
  modified: 2026-07-24T11:42:29.106Z
---

Kein dauerhaftes 2-Pane-Layout mit VIX mehr verwenden. NAS100 bekommt grundsätzlich die volle Chart-Höhe (Layout "single"), VIX-Kurs wird bei Bedarf per `quote_get` abgefragt statt permanent gechartet zu sein.

**Why:** Am 02.07.2026 wurde festgestellt, dass die Split-Ansicht (NAS100 oben, VIX unten) die Sub-Panes (Preis/RSI/MACD) auf beiden Charts stark zusammenquetscht und schwerer lesbar macht. Für VIX werden ohnehin nur der reine Kurs/Levelstand gebraucht (siehe Intermarket-Cheat-Sheet in [[feedback_session_update]]), keine eigenen Indikatoren-Werte — `quote_get` reicht dafür völlig aus, kein eigenes Chart-Pane nötig.

**How to apply:** Bei Session-Start NAS100 im Single-Pane-Layout einrichten (`pane_set_layout: single`), nicht das alte 2-Pane-Schema aus früheren Sessions reproduzieren. VIX-Stand bei Bedarf separat per `quote_get(symbol: "CBOE:VIX")` abfragen (VIX selbst ist von der `quote_get`-Unzuverlässigkeit für NAS100, siehe [[feedback_datenquelle_nas100]], nicht betroffen — nur NAS100 braucht Chart-Bars).

**Ausnahme seit 03.07.2026, Pane-1-Symbol geändert 15.07.2026:** 2-Pane-Layout (`2v`) ist bewusst in Nutzung — Pane 0 = NAS100 (volles 5er-Indikatoren-Set), Pane 1 = `BATS:QQQ` (Invesco QQQ Trust, Cboe-One-Echtzeitfeed) mit **EMA50 (Länge 50) + Volume + VWAP** (bewusst 3 statt 2 Indikatoren, siehe [[feedback_live_trading]] Punkt 7b für die Rollentrennung EMA=Pflicht-Gate/VWAP+Volumen=unterstützend — ursprünglich am 15.07.2026 versehentlich auf nur Volume+VWAP reduziert, EMA50 nach Rückgriff auf die Trade-#13-Historie noch am selben Tag wiederhergestellt). Zweck: echte Volumen-Bestätigung für Breakout/Divergenz-Checks aus der TA-Checkliste ([[feedback_chartanalyse]]), die auf dem NAS100-CFD-Feed nicht verfügbar ist. **Vorher lief hier `CME_MINI_DL:NQ1!` — ersetzt am 15.07.2026, weil der Delayed-Feed strukturell 10 Min hinterherlief ([[feedback_nq1_feed_lag]]); QQQ läuft dagegen in Echtzeit synchron zu NAS100 (Zeitstempel-verifiziert am 15.07.2026). Einschränkungen: QQQ handelt nur zur US-Session (siehe [[feedback_live_trading]] Punkt 7e) und der Cboe-One-Feed zeigt nur einen Teil (~10-20%) des konsolidierten Volumens — für die relative Spike-Bewertung ausreichend, absolute Zahlen nicht als Marktgröße interpretieren.** `tab_new` funktioniert in der Desktop-App nicht zuverlässig (meldet Erfolg, aber `tab_count` bleibt bei 1) — Pane-Split ist der funktionierende Workaround.

**VWAP auf QQQ:** Anders als auf FOREXCOM:NAS100 funktioniert VWAP auf QQQ einwandfrei (echtes Volumen vorhanden). Der QQQ-VWAP ist am US-Handelsbeginn (9:30 ET / 15:30 MESZ) verankert — das ist die institutionell relevantere Referenz als der frühere Globex-verankerte NQ1!-VWAP. In den ersten ~15-30 Min nach US-Open ist der VWAP noch jung/dünn — mit Vorsicht verwenden. QQQ-Preise (~$-Bereich) und VWAP-Level gelten nur als Richtungs-Referenz (Kurs über/unter VWAP), nie als 1:1-Level für den NAS100-Chart — wie schon bei NQ1!.

## Indikatoren-Set (Stand 23.07.2026, ATR ersetzt Pivot Points)

Das Symbol `FOREXCOM:NAS100` ist ein CFD-Feed **ohne echte Volumendaten**. `Volume` und `VWAP` liefern deshalb keine Werte (roter Fehler-Indikator im Vol-Pane, VWAP zeigt keine Zahl/Linie) — kein temporärer Bug, tritt nach jedem Neustart wieder auf.

**Aktuelles 5er-Set:**
- Moving Average Exponential (EMA, Länge 50)
- Bollinger Bands *(Ersatz für Volume)*
- Relative Strength Index
- MACD
- Average True Range (ATR, Länge 14) *(ersetzt seit 23.07.2026 Pivot Points Standard)*

**Plan-Limit (entdeckt 23.07.2026):** TradingView Essential-Abo erlaubt hart max. **5 Indikatoren pro Chart** (Premium: 25) — ein 6. Indikator schlägt mit `createStudy(...) failed: unknown` fehl, keine Fehlermeldung im Tool-Output selbst, nur sichtbar per Screenshot ("Sie haben 5 Indikatoren angewandt — das Maximum für Ihr Abonnement"). Bei jedem künftigen Indikator-Wunsch zuerst gegen dieses Limit prüfen, nicht direkt auf einen API-Fehler schließen.

**Warum Pivot Points raus, ATR rein (Fable-Review 23.07.2026):** [[feedback_chartanalyse]] Punkt 8c verlangt eine SL-Mindestdistanz von 1,5-2× der "größten Rausch-Kerze" — bisher eine manuelle Schätzung im 1-Min-Loop, die bei Trade #21 nachweislich zu eng ausfiel (SL 55 Pkt statt nötiger 109-145 Pkt, machte den RR-Check ungültig). ATR(14) liefert diese Zahl objektiv statt geschätzt. Pivot Points wurde getauscht, weil die S/R-Info ohnehin schon manuell über gezeichnete Linien getrackt wird (Checkliste Punkt 2) — der am wenigsten alleinstellungsmerkmal-tragende der 5 Indikatoren. Fables Einordnung des Effekts: kein Win-Rate-Hebel, nur niedriger einstelliger Bereich an vermiedenen SL-Distanz-Fehltrades (~1 von 20-25) — Risikohygiene, kein Ertragstreiber.

**Why (alter Kontext, weiterhin gültig für den Rest des Sets):** Am 02.07. schon einmal gegen Bollinger Bands getauscht, aber nach MCP-Neustart am 03.07. gingen beide Ersatz-Infos verloren (Chart lud initial ohne Indikatoren, dann kurz das alte gecachte Setup inkl. Pivot Points Standard + Bollinger Bands — dabei wurde erkannt, dass das der echte Vortages-Stand war).

**How to apply:** Bei jedem Neustart/`tv_launch` NICHT versuchen, `Volume` oder `VWAP` neu hinzuzufügen (schlägt fehl/bleibt leer) — direkt Bollinger Bands + ATR(14) als Teil des Sets aufbauen (nicht mehr Pivot Points). Indikatoren nacheinander (nicht parallel) hinzufügen — parallele `chart_manage_indicator`-Calls können sich die Such-UI gegenseitig kaputt machen.

## Split-Screen-Persistenz nach TradingView-Abo-Update — GELÖST (24.07.2026)

Nach einem TradingView-Update/Abo-Wechsel öffnete die Desktop-App kurzzeitig nicht mehr automatisch mit dem gespeicherten 2v-Split (NAS100+QQQ), sondern fiel auf Single-Chart zurück. Levi hat das Layout daraufhin manuell in TradingView selbst gespeichert (Layout-Speichern-Funktion in der App, nicht über MCP) — **seitdem übersteht der 2v-Split Neustarts wieder zuverlässig**, live doppelt verifiziert (24.07.2026, `tv_launch(kill_existing: true)` → Split war nach Neustart korrekt vorhanden, Screenshot bestätigt NAS100 oben mit 5er-Set + QQQ unten mit VWAP/EMA/Volume).

**Kein MCP-seitiger Fix nötig/vorhanden** — es gibt kein `layout_save`-Tool, die Lösung lag rein auf TradingView-Seite (User hat das Layout einmalig manuell gespeichert). Kein Pflicht-Rebuild-Schritt mehr bei `tv_launch` nötig.

**Bekannte Falle beim Prüfen, kein echter Bug (24.07.2026):** `pane_list` kann direkt nach `tv_launch` kurzzeitig fälschlich `layout: "s"` / `chart_count: 1` melden, obwohl der Chart tatsächlich schon korrekt im 2v-Split lädt (per Screenshot bestätigt) — beim zweiten `pane_list`-Aufruf wenige Sekunden später kommt korrekt `"2v"` zurück. Das ist eine Sync-Verzögerung des Tools direkt nach dem Start, kein Layout-Problem. **How to apply:** Zeigt `pane_list` direkt nach `tv_launch` unerwartet "Single-Chart", nicht sofort neu aufbauen — einmal per Screenshot oder erneutem `pane_list`-Call gegenchecken, bevor man von einem echten Problem ausgeht.
