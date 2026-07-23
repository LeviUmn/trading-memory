---
name: project-fable-tiefendiagnose-2026-07-15
description: "VOLLSTÄNDIG ABGESCHLOSSEN 16.07.2026 — Fable-5-Tiefendiagnose vom 15.07.2026: alle 8 Fixes umgesetzt UND der pane_focus-Code-Fix live gegen TradingView verifiziert. Keine offenen Punkte mehr, reine Referenz."
metadata:
  type: project
  originSessionId: session-2026-07-15
---

## Status: VOLLSTÄNDIG ABGESCHLOSSEN (16.07.2026) — alle 8 Fixes umgesetzt UND live verifiziert

Levi hat am 15.07.2026 (nach dem NQ1!→QQQ-Umbau, siehe [[feedback_nq1_feed_lag]] und [[feedback_live_trading]] Punkt 7b/7e) eine dritte Fable-5-Tiefendiagnose beauftragt: kompletter Regel-Konsistenz-Check, alle 19 Trades gegen aktuelle Regeln neu bewertet, QQQ-Backtest ab Trade #13, priorisierte Optimierungsvorschläge. Am 16.07.2026 wurden alle Fixes 1-8 umgesetzt (Fixes 2, 4-7 durch Fable selbst, Fix 8 nach zwei expliziten Levi-Entscheidungen, teils mit Fables Empfehlung eingeholt) — Details bei den einzelnen Punkten unten.

**Zusatz (nicht Teil der ursprünglichen 8 Fixes, aber am selben Tag entstanden):** Bei der Verifikation von Fix 4 fiel eine weitere Unschärfe auf (VWAP-Verhältnis in der 1H-Bias-Checkliste, obwohl NAS100 kein VWAP mehr hat) — auf Levis Vorschlag hin wurde daraus eine strukturelle Erweiterung: der komplette MTF-Bias-Check ([[feedback_chartanalyse]] Punkt 8, [[feedback_live_trading]] Punkt 3a/2b) läuft jetzt auf NAS100 UND QQQ, nicht mehr nur NAS100 — löst die Asymmetrie zum längst zweiseitigen Entry-Dual-Gate (Punkt 7b) auf.

**Live-Verifikation abgeschlossen (16.07.2026):** Nach `/mcp reconnect` + `tv_launch(kill_existing:false)` lieferte `pane_focus` in beide Richtungen `verified: true` mit korrektem `symbol` (siehe [[feedback_pane_sync_bug]] für Details) — der Code-Fix funktioniert live. Diese Diagnose-Datei ist damit vollständig abgearbeitet und dient nur noch als historische Referenz, keine offenen To-Dos mehr.

---

## 1. Wichtigster offener Entscheid: Trade #19 BE vs. Loss

Nach der bestehenden BE-Definition ([[feedback_be_definition]]: BE nur bei SL auf Entry/Breakeven nachgezogen) ist Trade #19 (15.07., Short NAS100, SL nachgezogen auf 29.300 = 25 Pkt in der Verlustzone, nicht am Entry 29.275) **kein BE, sondern ein regulärer Loss**. Slippage beim Exit war sogar positiv (3,76€ statt geplant 3,75€, verkleinerte den Verlust).

**Korrekte Statistik:** 11 Win / 2 BE / 6 Loss, **Win-Rate 64,7% (11/17)** — nicht 68,75% wie zuletzt in [[trades/trade_log]] und [[MEMORY]] kommuniziert.

**Zu entscheiden:** (a) Trade #19 in [[trades/trade_log]] und [[trades/trading_2026-07-15]] auf Loss korrigieren, Statistik-Kopfzeilen anpassen — oder (b) [[feedback_be_definition]] bewusst um eine vierte Kategorie erweitern ("SL in Gewinnrichtung nachgezogen, kleiner Restverlust bei Slippage"). Fable empfiehlt (a), warnt aber davor, die BE-Definition stillschweigend zu dehnen — das ist dasselbe Muster wie der alte #2-vs-#11-Konflikt, den die Definition eigentlich beenden sollte.

**ERLEDIGT 16.07.2026:** Levi hat sich für (a) entschieden. Trade #19 in [[trades/trade_log]] und [[trades/trading_2026-07-15]] auf Loss korrigiert, Kopfzeile auf 11 Win / 2 BE / 6 Loss / 64,7% Win-Rate (exkl. BE) aktualisiert.

## 2. QQQ-Backtest-Ergebnis (Kurzfassung)

- Historische QQQ-5-Min-Bars vor dem 14.07.2026 waren im Desktop-Client nicht mehr ladbar (6 Versuche, dokumentierte Datenlücke) — Trades #13-#17 nur qualitativ bewertet, #1-#12 bewusst "n/a" (kein Dual-Gate vor 07.07.).
- **#18 und #19 gemessen:** QQQ hätte beide Entries identisch bestätigt wie damals NQ1!. Bei #19 zusätzlich: QQQ blieb während des SL-reißenden Spikes (29.335,95) durchgehend unter eigener EMA50/VWAP — die Short-These kippte auf dem Echtzeit-Feed nie, der Spike war ein CFD-lokaler Stop-Hunt, kein echter Marktreversal.
- **Gesamtfazit:** Das neue QQQ-Setup hätte die bisherige Bilanz kaum verändert bis leicht verbessert — der Wert liegt in der beseitigten 10-Min-Verzerrung (erklärt rückwirkend auch die "Datenanomalie" vom 09.07., siehe Punkt 4 unten), nicht in besseren Signalen an sich. Größter Bilanz-Hebel bleibt RR-/Regeldisziplin: 5 von 6 Verlusten UND 3 der Gewinn-Trades (#13, #15, #17) trugen dokumentierte Prozess-/Regelverstöße.

## 3. Aktive Regel-Widersprüche (noch nicht bereinigt)

1. [[feedback_live_trading]] Punkt 3 nennt noch `quote_get` als Schritt 1 — widerspricht dem seit 04.07. geltenden Verbot in [[feedback_datenquelle_nas100]].
2. Punkt 2 (Status-Format-Beispiel) zeigt noch eine VWAP-Zeile, obwohl NAS100 seit 03.07. kein VWAP mehr hat (das läuft auf Pane 1).
3. TP2-Farbmarker: [[feedback_positions_status_pflicht]] (07.07., "ohne Marker") vs. [[feedback_positions_farbcode]] (14.07., "immer Grün-Marker") — neuere gilt praktisch, ältere nicht als überholt markiert.
4. [[feedback_session_update]] Schritt 6 verlangt noch VWAP von NAS100 und heißt "NQ Chart" statt QQQ.
5. `trading_session_nasdaq.md` (Stand 30.06., veraltete Levels + falsches Indikatoren-Set) ist laut [[feedback_memory_pflege]] noch Pflicht-Lektüre bei Session-Start — sollte archiviert oder aktualisiert werden. MEMORY.md-Indexzeile dazu ebenfalls veraltet.
6. Toter Link: `[[feedback-instrumenten-fokus]]` in `project_vision.md` Zeile 52 (Bindestriche statt Unterstriche) — kosmetisch.

## 4. QQQ-Umbau-Lücken (heutiger Umbau noch nicht vollständig durchgezogen)

- [[feedback_dual_gate_confirmation]] ist komplett NQ1!-formuliert, inkl. der Spike-Ausnahme-Regel, die auf faktisch 10 Min alten "Live-Ticks" kalibriert wurde — muss inhaltlich neu bewertet werden, nicht nur umbenannt.
- `feedback_live_trading_workflow_bewaehrt.md` Punkte 3/7 referenzieren noch NQ1!.
- [[feedback_pane_sync_bug]] / [[feedback_indikator_check]] beschreiben das Setup noch als NAS100+NQ1!.
- Rückwirkende Fehldeutung nie korrigiert: Die "NAS100-Datenanomalie" in `trading_2026-07-09.md` (NQ1! "reagiert nicht", "untypisches Führungsmuster") war mit heutigem Wissen exakt die 10-Min-Lag-Signatur — ein Nachtrag in der Datei würde verhindern, dass künftige Sessions daraus eine falsche Markt-Lehre ziehen.

## 5. Verlorene/nie übernommene Learnings (Muster: Tagesdatei → Regeldatei kommt nicht an)

- 23.06. (Trade #2): "SL nach Extension-Move mit kleinem Puffer, nicht exakt Breakeven" — wird zweimal als "in [[feedback_chartanalyse]] gespeichert" referenziert, ist dort aber **nicht vorhanden** und kollidiert latent mit dem 8b-Standard (Rest-SL sofort auf BE). Bewusst entscheiden: verwerfen oder einarbeiten.
- 30.06.→08.07.: SL/TP-Exklusivität bei Scalable war schon am 30.06. in einer Tagesdatei notiert, wurde aber erst nach dem Trade-#14-Vorfall am 08.07. zur echten Regel ([[feedback_broker_sl_tp_exklusiv]]) — 8 Tage ungenutztes Wissen.
- Der heutige EMA50-Fund (Trade #13 → nie in Punkt 7b übernommen) war laut Fable "Nummer drei" dieses Musters — Hinweis, dass das grundsätzliche Verfahren "Learnings aus Trade-Notizen in Regeldateien überführen" lückenhaft ist, nicht nur der EMA50-Einzelfall.

## 6. Priorisierte Vorschläge (Fables Reihenfolge)

1. ✅ **ERLEDIGT 16.07.** Trade #19 reklassifizieren + trade_log/Statistik bereinigen (siehe Punkt 1 oben)
2. ✅ **ERLEDIGT 16.07. (Fable):** QQQ-Umbau abgeschlossen — [[feedback_dual_gate_confirmation]] inhaltlich neu bewertet (nicht nur umbenannt: Spike-Ausnahme-Regel bleibt in Kraft, weil ihr Kern — Intrabar-Fehlausbruch-Filter — vom Feed-Lag unabhängig ist; neu dokumentiert, dass die NQ1!-"Live-Ticks" bei der Kalibrierung tatsächlich 10 Min alt waren, plus Punkt-7e-Zeitstempel-Check als Zusatzvoraussetzung); `feedback_live_trading_workflow_bewaehrt` Punkte 3/7/8 auf QQQ; [[feedback_pane_sync_bug]]/[[feedback_indikator_check]] kosmetisch auf NAS100+QQQ
3. ✅ **ERLEDIGT 16.07.** Feed-Lag-Nachträge in `trading_2026-07-09.md` und trade_log (#13-#18, konsolidierte Fußnote statt Einzeleinträge) ergänzt
4. ✅ **ERLEDIGT 16.07. (Fable):** [[feedback_live_trading]] Punkt 3 (`quote_get` → `data_get_ohlcv`), Punkt-2-Statuszeilen-Beispiel ohne VWAP (+ gleiche Korrektur in Punkt 9 "Zwischen zwei Voll-Checks"); [[feedback_session_update]] Schritt 6 auf NAS100 (ohne VWAP) + expliziten QQQ-Pane-Schritt mit 7e-Session-Gate umgebaut, "NQ Chart" → "NAS100+QQQ Chart"; TP2-Farbmarker-Altregel in [[feedback_positions_status_pflicht]] als überholt markiert (Verweis [[feedback_positions_farbcode]])
5. ✅ **ERLEDIGT 16.07. (Fable) — als echter Code-Fix, plus zusätzlicher Indikator-Resolver-Fix (`study-inputs.js`), beide im echten 1-Min-Loop verifiziert:** `pane_focus` (`src/core/pane.js`, `focus()`) hat eine eingebaute Verifikations-/Retry-Schleife (Variante a — sichert alle nachgelagerten Daten-Tools auf einmal ab). Zusätzlich wurde derselbe Tag ein zweiter, verwandter Bug root-gecaust und gefixt: `chart_manage_indicator`/`indicator_set_inputs` übergaben Friendly-Keys (z.B. "length") falsch an TradingViews API, was Studien dauerhaft in einen `isFailed`-Zustand brachte (siehe [[feedback_indikator_check]] für die volle Historie). Beide Fixes wurden am Ende nicht nur isoliert, sondern über einen kompletten `CronCreate`-1-Min-Loop mit 2 vollen Voll-Checks (14:35-14:40 UTC, NAS100+QQQ, je 15/60/5min) verifiziert: 6/6 Pane-Wechsel `verified: true`, durchgehend keine `isFailed`-Studie. Workaround aus [[feedback_pane_sync_bug]] ist damit obsolet, nicht mehr nötig
6. ✅ **ERLEDIGT 16.07. (Fable):** Zwei-Schritt-CronCreate-Textbaustein (15min→60min explizit, "Voll-Check ohne 60-Min-Wechsel gilt als nicht durchgeführt") als Pflicht-Baustein in [[feedback_live_trading]] Punkt 2b verankert, Rückverweis in [[feedback_mtf_voll_check_wiederholt_vergessen]]
7. ✅ **ERLEDIGT 16.07. (Fable):** `trading_session_nasdaq.md` mit ÜBERHOLT-Banner archiviert (verweist auf trades/-Tagesdateien, [[feedback_chart_layout]], [[feedback_live_trading]], [[project_risikomanagement]]); [[feedback_memory_pflege]] (Session-Start-Lektüre + Speicherziele 3/6) und MEMORY.md-Indexzeile korrigiert; toter Link in `project_vision.md` (3.6) gefixt
8. ✅ **ERLEDIGT 16.07.2026 — von Levi entschieden, nicht von Fable vorentschieden:**
   - **(a) SL-Nachzug vs. sichtbare Pivot-Level:** Levi fragte explizit nach Fables eigener Empfehlung, statt selbst zwischen den drei Optionen (bindend/Heuristik/nur Watch-Note) zu wählen. Fables Rat: weiche, konkret anwendbare Heuristik statt hartem Muss oder zu schwacher Notiz — übernommen als **Punkt 7a1** in [[feedback_chartanalyse]]: bei ≤10 Pkt Nähe zu einem sichtbaren Pivot/runder Marke den SL 10-15 Pkt dahinter setzen (außer RR/1,5%-Grenze verletzt), mit eingebauter Review-Pflicht nach 5 Anwendungsfällen (sonst wieder streichen — n=1 bei Einführung, bewusst nicht als endgültig gesetzt).
   - **(b) 23.06.-BE-Puffer-Learning:** Levi hat sich für **Verwerfen** entschieden — 8b (Rest-SL exakt auf Breakeven nach TP1) bleibt unverändert einfach, keine Ausnahme-Regel für Extension-Moves eingebaut.

## Referenz
Voller Report liegt im Konversationsverlauf der Session vom 15.07.2026 (Fable-Agent, dritter Review des Tages, ~276K Tokens, 104 Tool-Calls). Diese Datei ist die kondensierte, aktionierbare Fassung für den Wiedereinstieg.
