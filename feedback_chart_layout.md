---
name: feedback-chart-layout
description: "Levi bevorzugt NAS100 als Hauptchart; 2. Pane erlaubt für QQQ-Volumen-Referenz (seit 15.07.2026, vorher NQ1!, Ausnahme von der Single-Pane-Regel), 5-Indikatoren-Set ersetzt Volume/VWAP wegen fehlender CFD-Volumendaten. Seit 23.07.2026: Pivot Points gegen ATR(14) getauscht (Fable-Review)"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 028ce287-5d0c-400c-bf08-bdc1d4c4670c
  modified: 2026-08-22T09:22:57.305Z
---

Kein dauerhaftes 2-Pane-Layout mit VIX mehr verwenden. NAS100 bekommt grundsätzlich die volle Chart-Höhe (Layout "single"), VIX-Kurs wird bei Bedarf per `quote_get` abgefragt statt permanent gechartet zu sein.

**Why:** Am 02.07.2026 wurde festgestellt, dass die Split-Ansicht (NAS100 oben, VIX unten) die Sub-Panes (Preis/RSI/MACD) auf beiden Charts stark zusammenquetscht und schwerer lesbar macht. Für VIX werden ohnehin nur der reine Kurs/Levelstand gebraucht (siehe Intermarket-Cheat-Sheet in [[feedback_session_update]]), keine eigenen Indikatoren-Werte — `quote_get` reicht dafür völlig aus, kein eigenes Chart-Pane nötig.

**How to apply:** Bei Session-Start NAS100 im Single-Pane-Layout einrichten (`pane_set_layout: single`), nicht das alte 2-Pane-Schema aus früheren Sessions reproduzieren. VIX-Stand bei Bedarf separat per `quote_get(symbol: "CBOE:VIX")` abfragen (VIX selbst ist von der `quote_get`-Unzuverlässigkeit für NAS100, siehe [[feedback_datenquelle_nas100]], nicht betroffen — nur NAS100 braucht Chart-Bars).

**Ausnahme seit 03.07.2026, Pane-1-Symbol geändert 15.07.2026:** 2-Pane-Layout (`2v`) ist bewusst in Nutzung — Pane 0 = NAS100 (volles 5er-Indikatoren-Set), Pane 1 = `BATS:QQQ` (Invesco QQQ Trust, Cboe-One-Echtzeitfeed) mit **EMA50 (Länge 50) + Volume + VWAP** (bewusst 3 statt 2 Indikatoren, siehe [[feedback_live_trading]] Punkt 7b für die Rollentrennung EMA=Pflicht-Gate/VWAP+Volumen=unterstützend — ursprünglich am 15.07.2026 versehentlich auf nur Volume+VWAP reduziert, EMA50 nach Rückgriff auf die Trade-#13-Historie noch am selben Tag wiederhergestellt). Zweck: echte Volumen-Bestätigung für Breakout/Divergenz-Checks aus der TA-Checkliste ([[feedback_chartanalyse]]), die auf dem NAS100-CFD-Feed nicht verfügbar ist. **Vorher lief hier `CME_MINI_DL:NQ1!` — ersetzt am 15.07.2026, weil der Delayed-Feed strukturell 10 Min hinterherlief ([[feedback_nq1_feed_lag]]); QQQ läuft dagegen in Echtzeit synchron zu NAS100 (Zeitstempel-verifiziert am 15.07.2026). Einschränkungen: QQQ handelt nur zur US-Session (siehe [[feedback_live_trading]] Punkt 7e) und der Cboe-One-Feed zeigt nur einen Teil (~10-20%) des konsolidierten Volumens — für die relative Spike-Bewertung ausreichend, absolute Zahlen nicht als Marktgröße interpretieren.** `tab_new` funktioniert in der Desktop-App nicht zuverlässig (meldet Erfolg, aber `tab_count` bleibt bei 1) — Pane-Split ist der funktionierende Workaround.

**VWAP auf QQQ:** Anders als auf FOREXCOM:NAS100 funktioniert VWAP auf QQQ einwandfrei (echtes Volumen vorhanden). Der QQQ-VWAP ist am US-Handelsbeginn (9:30 ET / 15:30 MESZ) verankert — das ist die institutionell relevantere Referenz als der frühere Globex-verankerte NQ1!-VWAP. In den ersten ~15-30 Min nach US-Open ist der VWAP noch jung/dünn — mit Vorsicht verwenden. QQQ-Preise (~$-Bereich) und VWAP-Level gelten nur als Richtungs-Referenz (Kurs über/unter VWAP), nie als 1:1-Level für den NAS100-Chart — wie schon bei NQ1!.

## VWAP-Standardabweichungsbänder auf QQQ (ergänzt + live umgesetzt 22.08.2026, Fable-Kernstrategie-Review)

Ergänzt den bestehenden VWAP-Indikator auf QQQ (Pane 1, Entity-ID zum Umsetzungszeitpunkt `d1GyOo`) um Bänder — kein zusätzlicher Indikator-Slot, Bänder sind eine Sub-Option des bereits vorhandenen VWAP, kostenlos in Bezug auf das 5-Indikatoren-Limit.

**Warum:** Ein Preis, der die VWAP-Mittellinie nur knapp streift, sah bisher optisch identisch aus wie ein Preis, der eine echte, mit Volumen unterlegte Spannweite durchbricht — genau diese Unterscheidung soll das Whipsaw-an-der-EMA/VWAP-Problem entschärfen (Fable-Kernstrategie-Review 22.08.2026, ausgelöst durch Levis Frage nach Trades #19/#38, siehe [[feedback_chartanalyse]] 8a2/8a3).

**Konfiguration (live gesetzt via `indicator_set_inputs`, verifiziert per Screenshot am selben Tag):**
- Band 1: aktiviert, Multiplikator 1 Standardabweichung (`in_5: true`, `in_6: 1`)
- Band 2: aktiviert, Multiplikator 2 Standardabweichungen (`in_7: true`, `in_8: 2`)
- Band 3: bewusst deaktiviert (`in_9: false`), um die Pane nicht zu überladen
- Verifiziert am Screenshot (nicht nur "success" vertraut): VWAP-Zeile zeigte 5 Werte (VWAP + Band1 Ober/Unter + Band2 Ober/Unter) — z.B. 713,11 Mitte / 714,42–711,81 (±1,31) / 715,72–710,50 (±2,61). Band-2-Abstand exakt doppelt so groß wie Band-1-Abstand zur Mittellinie — bestätigt die korrekte 1×/2×-Relation.

**Lesart:** Preis innerhalb Band 1 = normale Rausch-Zone um VWAP, kein starkes Signal. Kurs schließt jenseits Band 2 = deutlich stärkeres Bestätigungssignal als ein reiner Mittellinien-Touch. Stand 22.08.2026: informeller Kontext-Check, noch KEIN eigenständiges Pflichtkriterium mit eigener Ausgabezeile — das wäre ein separater, hier bewusst nicht getroffener nächster Schritt.

**How to apply:** Die Konfiguration ist am Chart persistiert (siehe Split-Screen-Persistenz-Abschnitt unten) — kein Wiederholungsschritt beim nächsten Live-Trading-Start nötig, nur bei sichtbarem Fehlen per Screenshot gegenchecken und ggf. mit obiger Konfiguration neu setzen.

**Plan-Upgrade-Frage (Diskussionsnotiz, 22.08.2026, kein Beschluss):** Levi brachte die Möglichkeit eines TradingView-Plan-Upgrades ins Spiel (mehr als 5 Indikator-Slots auf NAS100, kein Tausch mehr nötig für einen neuen Indikator). Fable-Einschätzung: Ein zusätzlicher freier Slot würde ADX (Trendstärke) strategisch moderat wertvoll machen — er würde die bereits bestehende, aber informelle Chop-Erkennung (siehe [[feedback_chartanalyse]] Punkt 7, RSI/MACD-Flip + flacher VIX) in eine objektive, live mitlaufende Zahl übersetzen, ähnlich wie ATR die frühere manuelle "größte Rausch-Kerze"-Schätzung ersetzt hat — kein Durchbruch, aber eine echte, moderate Verbesserung gegen das wiederholt dokumentierte Muster "Chop erkannt, aber ignoriert". Kein Ersatz für die VWAP-Bänder/8a2/8a3 oben, adressiert eine andere Teilfrage (wie oft überhaupt handeln, nicht wie sauber der einzelne Trigger ist). Vorbehalt: mehr Slots lösen NICHT das separat dokumentierte Pflichtzeilen-Wachstumsproblem ([[feedback_live_trading]] Punkt 14, Trade #42 "Hinweis zu spät gelesen") — ein weiterer Indikator ist auch eine weitere Zahl, die unter Zeitdruck gelesen werden muss. Kostenfrage (Plan-Preis) ist reine Levi-Entscheidung, von Fable nicht bewertet. Keine Umsetzung, reine Diskussionsnotiz für eine spätere Entscheidung.

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

## QQQ-Pane nach jedem Check zurück auf 15min stellen, NICHT 5min (ergänzt 28.07.2026, User-Korrektur; noch am selben Tag korrigiert)

Beim MTF-Block auf QQQ (15min/60min lesen) bleibt die Pane 1 danach oft auf dem zuletzt gesetzten Timeframe stehen, wenn nur zurück auf Pane 0 fokussiert wird, ohne QQQ explizit zurückzusetzen.

**Korrigiert (noch 28.07.2026, direkt im Anschluss):** Erste Version dieser Regel sagte "immer auf 5min zurückstellen" — das war falsch. User-Klarstellung: "Dann kannst du QQQ Standardmäßig immer auf 15min stellen, dann macht 5min kein Sinn bei mir bei TradingView" — der Ruhezustand von QQQ soll **15min** sein, nicht 5min. Das deckt sich auch mit dem Pflicht-Gate selbst (QQQ-EMA50 wird auf 15min geprüft, siehe Abschnitt "QQQ-Pflicht-Gate: unterschiedliche Timeframes" weiter unten) — 15min als Ruhezustand ist also sowieso die relevante Ebene, keine zusätzliche Umschaltung nötig.

**Why:** User sieht QQQ live in der eigenen TradingView-Instanz mit, 5min ist dort visuell nicht der gewünschte Standard-Blick.

**How to apply:** Nach JEDEM QQQ-MTF-Check (15min+60min lesen) den letzten Schritt vor dem Zurück-Fokussieren auf Pane 0 immer `chart_set_timeframe(15)` auf Pane 1 sein (NICHT 5) — erst dann `pane_focus(0)`. Reihenfolge: pane_focus(1) → 15min lesen → 60min lesen → **chart_set_timeframe(15)** → pane_focus(0). Mit `pane_list` gegenchecken, dass QQQ auf "15" steht, bevor der Check als abgeschlossen gilt.

## QQQ-Pflicht-Gate: unterschiedliche Timeframes für NAS100 und QQQ (ergänzt 28.07.2026, User-Nachfrage)

NAS100 und QQQ werden bewusst NICHT auf demselben Timeframe geprüft: **NAS100-EMA50 läuft auf 5min** (nativer Trigger-Timeframe, dort fällt die Kerzenschluss-Entscheidung), **QQQ-EMA50 läuft auf 15min** (Haupt-Check) **plus 60min** als zusätzlicher Cross-Check (siehe [[feedback_live_trading]] Punkt 3a/7b). QQQ ist die langsamere, unterstützende Bestätigungsebene, kein eigener Trigger — ein 5min-QQQ-Signal wäre zu schnell/flackerig für diese Rolle (am 28.07.2026 live beobachtet: QQQ-5min-EMA50 wurde Stunden vor der 15min-Version erreicht).

**How to apply:** Bei Dual-Gate-Fragen NICHT davon ausgehen, dass beide Instrumente auf demselben Timeframe verglichen werden — NAS100 = 5min, QQQ = 15min/60min ist der korrekte, asymmetrische Standard.
