---
name: feedback-live-trading
description: "Live-Trading-Protokoll für maximale Geschwindigkeit — Signal-Format, Check-in-Sequenz, Fast-Mode"
metadata:
  node_type: memory
  type: feedback
  originSessionId: trading-session-2026-06-26
  modified: 2026-07-23T10:20:39.680Z
---

Beim Live-Trading auf maximale Geschwindigkeit optimieren ohne auf Fähigkeiten zu verzichten.

**Why:** Im Live-Trading zählen Sekunden. Langsame Responses oder Approval-Prompts kosten Entry-Qualität.

**How to apply:** Folgende Regeln beim Live-Trading konsequent anwenden.

---

## 1. Fast Mode
User tippt `/fast` zu Beginn jeder Live-Trading-Session → schnellere Ausgabe.
Nach Session wieder deaktivieren mit `/fast`.

## 2. Kompaktes Signal-Format
Im Live-Trading NUR dieses Format — keine langen Erklärungen:

**Entry-Signal:**
`LONG: Entry 29.172 | SL 29.050 | TP 29.500 | 45 Stück | RR 2,7:1 ✅`
`SHORT: Entry 29.605 | SL 29.720 | TP 29.335 | 370 Stück | RR 2,2:1 ✅`

**Status-Update (VWAP-Beispiel entfernt 16.07.2026 — NAS100 hat seit 03.07. kein VWAP mehr, das läuft auf der QQQ-Pane; die Pro-Tick-Zeile ist NAS100-only, siehe Punkt 7b):**
`Kurs 29.234 | RSI 52 | MACD H +3,1 | EMA50 29.450 (darunter ⚠️)`

**SL-Nachzug:**
`SL nachziehen auf 29.300 (Breakeven) — RSI 71, Kurs +280 Pkt seit Entry`

**Prüfungen laufen intern, nur das Ergebnis erscheint im Signal (ergänzt 13.07.2026, nach Fable-Review):** Dual-Gate-Check, RR-Check gegen [[feedback_chartanalyse]] Punkt 8b (SL→TP1≥1:1→TP2≥1:2→sonst auslassen) und die Rausch-Kerzen-Mindestschwelle (Punkt 8c) werden VOR jedem Entry-Signal durchgerechnet — aber nicht als Zwischenschritt ausgesprochen oder Schritt für Schritt erklärt. Der User sieht nur das fertige, bereits geprüfte Entry-Signal (Format oben, inkl. `RR X:X ✅`). Erst wenn eine Prüfung scheitert (RR zu niedrig, Dual-Gate nicht bestätigt, SL zu eng am Rauschen), wird das kurz benannt — als Grund fürs Auslassen, nicht als laufender Rechenweg.

**Keine Tick-Zähler/Labels (ergänzt 10.07.2026, User-Korrektur; erweitert 16.07.2026):** Im Loop NICHT mit "Tick 3", "Tick 4 (Voll-Check)" o.ä. einleiten — das kostet nur Zeit/Platz ohne Mehrwert. Stattdessen direkt mit der Uhrzeit einsteigen, z.B. `12:37: Kurs 29.641 | RSI 65 | MACD-H +4,0 | EMA50 29.608`. Gilt für Quick-Ticks genauso wie für Voll-Checks. **Ausdrücklich eingeschlossen: die interne Minute-%5-Rechnung selbst NIE ausgeben** (z.B. "54%5=4, Quick-Tick" oder "1%5=1, Quick-Tick") — das ist reine interne Weichenstellung, kein Informationswert für den User. Diese Rechnung intern durchführen, aber nur das Ergebnis (Uhrzeit + Werte, oder Voll-Check-Inhalt) zeigen. Zweiter Vorfall 16.07.2026 trotz bestehender Regel — Grund war vermutlich, dass die Regel nur "Tick-Zähler" nannte, nicht explizit die Modulo-Anzeige; jetzt ausdrücklich ergänzt.

## 2a. Loop-Intervall: 1 Minute bei 5-Min-Kerzen (ergänzt 01.07.2026)
User tradet auf 5-Min-Kerzen. Bei aktiver Trigger-Beobachtung (Cron-Loop via `/loop`) Intervall auf **1 Minute** setzen statt 2 Minuten — das ergibt 5 Checks pro Kerze statt 2-3 und reduziert die Verzögerung beim Melden von Indikator-Kipppunkten (siehe Punkt 7a) auf maximal 1 Minute.

**How to apply:** Bei jedem neuen Setup-Loop (CronCreate) Standard-Intervall `*/1 * * * *` verwenden, nicht `*/2`. Gilt für aktive Setup-Beobachtung und offene Positionen gleichermaßen.

**Falle: Loop läuft still aus bei direkten Zwischenfragen (ergänzt 02.07.2026):** Wenn der Loop über `ScheduleWakeup` (nicht CronCreate) läuft und der User zwischendurch direkte Nachrichten schreibt (Fragen, Kommentare), die man normal beantwortet ohne den Loop-Kontext erneut aufzurufen, wird `ScheduleWakeup` in diesen Antworten leicht vergessen — der Loop läuft dann still aus, ohne dass das auffällt, bis der User nachfragt "läuft der Loop noch?".

**Why:** Am 02.07.2026 wurden mehrere direkte User-Nachrichten (TP-Diskussion, SL-Bestätigung, Kerzen-Kommentare) beantwortet, ohne jedes Mal `ScheduleWakeup` neu zu setzen — der Loop war vermutlich mehrere Minuten inaktiv, bis der User explizit nachfragte.

**How to apply:** Während eine aktive Position offen ist oder ein Setup aktiv beobachtet wird, bei JEDER Antwort (auch bei direkten Zwischenfragen, nicht nur bei Loop-Reinvocations) am Ende prüfen: läuft noch ein gültiger `ScheduleWakeup`? Falls unsicher oder falls die letzte Antwort keinen neuen `ScheduleWakeup`-Call hatte, lieber einmal zu oft neu planen als den Loop unbemerkt auslaufen lassen.

## 2b. Echte 1-Minuten-Cadence gelöst: `CronCreate` statt `ScheduleWakeup` (final, 10.07.2026 nach zwei Testreihen)

**Vorgeschichte kurz:** Ein erster Test mit `ScheduleWakeup` (flaches `delaySeconds: 60`) zeigte, dass diese Methode mechanisch NIE schneller als alle 2 echten Minuten feuern kann — das war keine Kalibrierungsfrage, sondern eine feste Untergrenze dieses Mechanismus (siehe Testprotokoll 10.07.2026, ScheduleWakeup-Reihe). Ein zweiter Test mit `CronCreate` (bereits als Zielmechanismus in Punkt 2a vorgesehen, in der Praxis aber durch `ScheduleWakeup` ersetzt worden) hat das Problem vollständig gelöst: **10 von 10 echten Minuten am Stück getroffen, keine einzige übersprungen.**

**Was `CronCreate` ist (einfach erklärt):** Statt jedes Mal selbst "wecke mich in 60 Sekunden" zu sagen (und dabei jedes Mal neu zu rechnen, mit der beschriebenen Rundungs-Falle), wird EINMAL ein Dauer-Wecker gestellt: "feuere ab jetzt jede volle Minute, automatisch, bis ich dich stoppe" (`cron: "* * * * *"`, `recurring: true`). Das ist kein Unterschied in der Häufigkeit, sondern im Prinzip: kein wiederholtes Neu-Berechnen mehr, das sich mit der Zeit verschieben kann.

**Warum das die Rundungs-Falle auflöst:** Das Problem bei `ScheduleWakeup` war, dass zwischen "Uhr ablesen" und "Wecker stellen" immer ein paar Sekunden vergehen — und weil dieser Wecker nie kürzer als 60 Sekunden gestellt werden darf, hat er sich dadurch fast immer selbst auf die übernächste Minute geschoben. `CronCreate` läuft nach demselben Prinzip wie ein Wecker, der wirklich an der Wand hängt und jede volle Minute unabhängig losgeht, egal wie lange die letzte Bearbeitung gedauert hat — es gibt keine Neuberechnung, die sich verschieben könnte.

**Gemessene Werte (10.07.2026, 10 Minuten am Stück, 12:14-12:23):** jede Zielminute getroffen, Abweichung durchgehend +11 bis +24 Sekunden (Ø ~14s) — das ist die Zeit, die der Wecker selbst braucht, um nach der vollen Minute tatsächlich die erste Tool-Aktion auszulösen. Diese Verzögerung ist **real, aber konstant** — sie addiert sich nicht von Minute zu Minute auf und führt zu keinem einzigen Überspringen. Ehrlich kommuniziert: die Cadence ist "jede echte Minute, plus ~10-25 Sekunden Anlauf", nicht auf die Sekunde exakt — aber ohne Drift, ohne Skip.

**Operatives Vorgehen:**
1. **Beim Start jeder Live-Trading- bzw. Setup-Beobachtungs-Session** einmalig `CronCreate` mit `cron: "* * * * *"`, `recurring: true` aufrufen. Der mitgegebene Prompt enthält direkt die Weiche: `wenn aktuelle Minute % 5 == 0 → Voll-Check (siehe Punkt 9), sonst → Quick-Tick (siehe Punkt 3)`. Keine r-/Boundary-Berechnung mehr nötig — die Modulo-5-Prüfung auf die aktuelle Minute reicht, weil jede volle Minute ohnehin zuverlässig kommt.
2. **Bei jedem Fire** normal reagieren: Quick-Tick → kompakte Zahlen-Sequenz (Punkt 3), Voll-Check (jede 5. Minute) → volle Tiefe inkl. Screenshot, MTF-Wechsel, Tweet-Fetch (siehe unten).
3. **Beim Ende der Session/Position** `CronDelete` explizit aufrufen. Der Job läuft sonst bis zu 7 Tage im Hintergrund weiter und feuert weiterhin jede Minute einen Prompt — das unbemerkt laufen zu lassen wäre die neue Variante der alten "Loop läuft still aus"-Falle aus Punkt 2a, nur umgekehrt (er läuft still WEITER statt still AUS).

**Pflicht-Baustein im CronCreate-Prompt: MTF-Zwei-Schritt-Block wörtlich ausschreiben (verankert 16.07.2026 nach zwei 1H-Vergessens-Vorfällen, siehe [[feedback_mtf_voll_check_wiederholt_vergessen]]):** Der 1H-Bias-Check fiel bei Trade #18 UND #19 im Voll-Check weg (nur 15-Min gemacht), weil der 15-Min-Wechsel als "der MTF-Check" wahrgenommen wird und der 1H-Schritt beim Tool-Batching untergeht. Deshalb beim Aufsetzen JEDES Setup-/Positions-Loops den Voll-Check-Teil des CronCreate-Prompts NICHT nur mit dem Stichwort "MTF-Blick" beschreiben, sondern diesen Standard-Textbaustein wörtlich in den Prompt übernehmen:

```
Wenn aktuelle Minute % 5 == 0 → Voll-Check (Punkt 9), darin als fester Vier-Schritt-Block:
chart_set_timeframe(15) auf NAS100 → Werte lesen →
chart_set_timeframe(60) auf NAS100 → Werte + letzte 5 Bars auf HH-HL/LH-LL-Struktur lesen →
zurück auf chart_set_timeframe(5) auf NAS100 →
FALLS QQQ-Session-Gate (Punkt 7e) offen: pane_focus auf Pane 1 (QQQ), dort chart_set_timeframe(15)
und (60) → VWAP-Verhältnis + Volumen + eigene EMA50 lesen → zurück auf Pane 0 (NAS100).
Der 1H-Schritt (NAS100) UND der QQQ-Schritt (falls Gate offen) sind NICHT optional — ein
Voll-Check ohne den 60-Min-Wechsel bzw. ohne den QQQ-Durchlauf gilt als NICHT durchgeführt
und wird beim nächsten Fire nachgeholt und als Lücke gemeldet.
Sonst (Minute % 5 != 0) → Quick-Tick (Punkt 3).
```

Diese Formulierung wurde am 15.07.2026 live erarbeitet (Cron-Job-Korrektur 16:26 UTC während Trade #19), am 16.07.2026 um den QQQ-Schritt erweitert (siehe Punkt 3a), und ist ab jetzt der Standard — nicht in jeder Session neu erfinden.

**Technische Rahmenbedingungen (wichtig für die Einordnung):**
- Der Job ist **session-only** — er übersteht das Ende der Session nicht und wird nirgends auf Platte gespeichert.
- Er **feuert nur, wenn gerade nichts anderes läuft** (REPL ist idle) — dauert eine Bearbeitung (z.B. ein Voll-Check) länger als bis zur nächsten Minute, wird dieser eine Fire verzögert nachgeholt, nicht übersprungen — genau das "eine Delle, kein Drift"-Verhalten, das bei `ScheduleWakeup` gerade fehlte.
- Der Job **läuft maximal 7 Tage**, danach läuft er von selbst aus — für eine einzelne Trading-Session irrelevant, aber relevant falls eine Session unerwartet lange offen bleibt.

**Bezug zu Punkt 2a:** Das bringt die Praxis zurück zu dem, was Punkt 2a von Anfang an vorsah (`CronCreate` mit `*/1 * * * *`), aber zwischenzeitlich durch `ScheduleWakeup` ersetzt wurde. Ab jetzt gilt `CronCreate` als das verbindliche Standard-Werkzeug für den Loop, `ScheduleWakeup` nur noch als Fallback, falls `CronCreate` in einer Session nicht verfügbar ist (dann gilt die 2-Minuten-Realität aus dem ScheduleWakeup-Test als Notlösung, nicht als neues Ziel).

**Wichtig — nur der Wecker-Mechanismus ändert sich, der Inhalt jedes Ticks bleibt exakt wie bisher dokumentiert:** Der isolierte Cadence-Test lief bewusst mit reduziertem Inhalt (nur Zahlen/Screenshot), um ausschließlich den Timing-Mechanismus zu prüfen. Das war NUR für den Test. Im echten Live-Trading gelten unverändert:
- Tweet-Fetch der 3 X-Accounts bei jedem 2. Voll-Check, alle 10 Minuten (siehe weiter unten, angepasst 10.07.2026)
- Volle Setup-Scan-Checkliste beim Voll-Check, solange keine Position offen ist (Punkt 9, [[feedback_chartanalyse]])
- Positions-Kasten mit SL/TP1/TP2 bei jedem Status-Update, sobald eine Position offen ist (Punkt 8, [[feedback_positions_status_pflicht]])
- Entscheidungsbaum-Format in der heißen Phase (Punkt 7)

Diese Inhaltsregeln sind vom Wecker-Mechanismus komplett unabhängig — `CronCreate` bestimmt nur WANN reagiert wird, nicht WAS in der Reaktion steht. Keine dieser Regeln wird durch den Mechanismus-Wechsel verkürzt oder ersetzt.

**X-Tweets als Bestandteil jedes 2. Voll-Checks, alle 10 Minuten (ergänzt 10.07.2026, angepasst 10.07.2026 nach Live-Test):** Bisher wurden die 3 X-Accounts (DeItaone/KobeissiLetter/zerohedge) nur beim Session-Update (Schritt 2, siehe [[feedback_session_update]]) und ad-hoc auf Nachfrage abgerufen — dazwischen konnte über Stunden eine marktbewegende Breaking-News (Fed-Kommentar, Geopolitik) unbemerkt bleiben. Ursprünglich bei JEDEM Voll-Check vorgesehen (alle 5 Min), nach dem Live-Loop-Test aber auf **jeden 2. Voll-Check** (alle 10 Min) reduziert: `get_users_posts` für alle 3 Accounts seit dem `x_last_fetch.json`-Timestamp abrufen und den Timestamp danach aktualisieren, aber nur bei jedem zweiten Voll-Check-Durchlauf, nicht bei jedem.

**Why:** User-Vorschlag 10.07.2026: "damit falls was Neues gekommen ist, was die Märkte dreht, wir sofort wissen" — der Voll-Check ist ohnehin der Punkt, an dem laut Punkt 9 bewusst etwas mehr Zeit investiert wird. Korrektur nach dem Live-Test (gleicher Tag): User-Feedback "alle 5 Minuten macht keinen Sinn, kostet zu viel Speed" — 10 Minuten reichen, um bei Breaking News zeitnah informiert zu sein, ohne den Voll-Check unnötig zu verlangsamen.

**How to apply (korrigiert 13.07.2026 — ersetzt die alte "intern mitzählen"-Anweisung, die zum verpassten 16:00-Tweet-Fetch führte):** NICHT mehr per eigener Zählung abwechseln — stattdessen die per Punkt 9a per `Bash date` ermittelte echte Minute nehmen: Minute % 10 == 0 → Tweet-Fetch einbauen, sonst entfällt dieser Schritt (nur Screenshot + finale Kerze + MTF + Musterprüfung). Das macht den Tweet-Rhythmus von derselben verlässlichen Zeitquelle abhängig wie den Voll-Check-Rhythmus selbst, statt einer separaten, fehleranfälligen Zählung.

**How to apply:** Tweet-Fetch läuft parallel zu den anderen Voll-Check-Tools (Screenshot/MTF-Wechsel/Pattern-Check), nicht sequenziell danach. Nur bei echten marktbewegenden Inhalten eine Meldung im kompakten Format (siehe Punkt 4a: nur bei echten News, kein Fließtext) — bei "nichts Neues/nichts Relevantes" reicht ein sehr kurzer Vermerk oder gar keine explizite Erwähnung, keine Tweet-Liste ausgeben.

**Geschwindigkeit PRO Check-in ist der eigentliche Engpass, nicht das Intervall:** Ein 1-Min-Intervall bringt nichts, wenn die Auswertung selbst 1-2 Minuten dauert — dann ist die Kerze schon vorbei, bevor reagiert wird. Deshalb bei JEDEM Check-in während einer offenen Position:
1. Alle Tools aus Punkt 3 (5-Tool-Sequenz) **parallel** aufrufen, nie nacheinander
2. Die 9d1/4a-Muster-/Kerzen-Prüfung (siehe [[feedback_chartanalyse]]) ist Teil dieser schnellen Standard-Sequenz, kein zusätzlicher separater Denkschritt — direkt am aktuellen `data_get_ohlcv`/Screenshot ablesbar, nicht neu recherchieren
3. Sofort die kompakte Kästen-Ausgabe (Punkt 2/7/8), keine ausführliche Begründung vor der eigentlichen Meldung
4. Nur bei echtem Kipp-Punkt (Punkt 4a) überhaupt Text schreiben — bei "alles unverändert" reicht ein sehr kurzer Status, kein neuer Analyse-Absatz

**Why:** User hat am 03.07.2026 explizit befürchtet, dass eine 2-minütige Denkpause während der Muster-Erkennung die Kerze bereits "verschwinden" lässt, bevor reagiert wird — die eigentliche Gefahr ist also nicht ein zu langes Intervall, sondern eine zu langsame Auswertung pro Check-in.

## 3. Tool-Sequenz bei jedem Check-in — Umfang phasenabhängig (aktualisiert 03.07.2026, `quote_get` entfernt 16.07.2026)
Kernwerte immer parallel, kein Nachdenken:
1. `data_get_ohlcv` (summary: true) → aktueller Kurs + Preisstruktur
2. `data_get_study_values` → RSI/MACD/EMA50 (NAS100, Pane 0)
3. Auswertung sofort danach

**Korrigiert 16.07.2026 (Fable-Review):** Schritt 1 war bis dahin `quote_get` — das widersprach dem seit 04.07.2026 geltenden Verbot aus [[feedback_datenquelle_nas100]] (`quote_get` lieferte für NAS100 zweimal live falsche/stale Kurse). Der aktuelle Kurs kommt jetzt aus den Chart-Bars (`data_get_ohlcv`), die ohnehin Teil der Sequenz waren — kein separater Kurs-Call mehr nötig.

`capture_screenshot` ist NICHT Teil jedes einzelnen Ticks mehr (siehe Punkt 3b) — nur bei vollständigen Checkliste-Durchläufen.

## 3a. Multi-Timeframe-Bias — echter Timeframe-Wechsel ist in der ruhigen Phase Pflicht (aktualisiert 03.07.2026, User-Korrektur; auf zwei Instrumente erweitert 16.07.2026)
Für Schritt 1 der Multi-Timeframe-Checkliste ([[feedback_chartanalyse]] Punkt 8, 1H-Bias: Trend/HH-HL, S/R-Zonen, EMA50) **in der ruhigen Beobachtungsphase tatsächlich per `chart_set_timeframe` auf 15min/1H wechseln, Werte lesen, zurück auf 5min** — nicht nur aus 5-Min-Bars aggregieren. Die frühere Aggregations-Notlösung (Bars selbst zu 15min/1H-Blöcken zusammenrechnen) ist überholt: der User will die echten 1H/15min-Werte, nicht nur eine Näherung aus 5-Min-Daten.

**Seit 16.07.2026: zusätzlich auf QQQ (Pane 1).** Der MTF-Bias lief bisher nur auf NAS100, während der finale Entry-Trigger längst beide Instrumente prüft (Punkt 7b) — diese Asymmetrie wird hier aufgelöst. In der ruhigen Phase bei jedem vollständigen Checkliste-Durchlauf zusätzlich `pane_focus` auf Pane 1 (QQQ), dort ebenfalls `chart_set_timeframe` auf 15min/1H, VWAP-Verhältnis + Volumen + eigene EMA50 lesen, dann zurück auf NAS100/Pane 0 und 5min. Entfällt ersatzlos, wenn das QQQ-Session-Gate (Punkt 7e) gerade geschlossen ist.

**Kosten des Wechsels — NUR in der ruhigen Phase, nie in der heißen Trigger-Phase.** Ein `chart_set_timeframe`-Wechsel wartet intern auf den fertig gerenderten Chart bei der neuen Auflösung — grobe Schätzung (nicht exakt gemessen) 3-8 Sekunden pro Richtung, also ~6-15 Sekunden für Hin- und Rückweg auf NAS100. Der zusätzliche QQQ-Durchlauf (Pane-Wechsel + eigener Timeframe-Wechsel, hin und zurück) kommt oben drauf — bei einer 300-Sekunden-Kerze weiterhin ein kleiner Anteil, aber in der heißen Phase kurz vor einem Trigger (Entscheidungsbaum-Format aus Punkt 7) ist selbst das ein Risiko — siehe Trade #10, wo der komplette Reversal innerhalb einer einzigen 5-Min-Kerze ablief. Deshalb bleibt der QQQ-Teil strikt auf die ruhige Phase beschränkt, genau wie der NAS100-Timeframe-Wechsel selbst.

**How to apply:** Ruhige Phase → bei jedem vollständigen Checkliste-Durchlauf (nicht bei jedem 1-Min-Tick) auf NAS100 tatsächlich auf 15min/1H wechseln und zurück, UND zusätzlich auf QQQ (Pane 1) denselben Timeframe-Wechsel durchführen. Sobald die heiße Phase beginnt (Kurs nähert sich der Zone, erste Bedingungen bestätigen sich laut Punkt 7a), sofort und strikt auf 5-Min/NAS100 stehen bleiben, kein `chart_set_timeframe`- oder `pane_focus`-Wechsel mehr, bis das Setup entweder ausgelöst hat oder wieder in die ruhige Phase zurückkippt.

**Why:** User-Korrektur 03.07.2026: "das muss auf jeden Fall gemacht werden mit dem 1H/15min Bias, in ruhigen Phasen ein Muss, dann springen wir nur noch auf 5-Min wenn wir kurz vor Entry sind" — die reine Aggregation wurde als unzureichend zurückgewiesen, echte Werte sind in der Phase ohne Zeitdruck wichtiger als der Geschwindigkeitsvorteil. Erweiterung 16.07.2026 (User-Vorschlag): wenn der MTF-Bias beide Instrumente einbeziehen soll, dann konsequent auch auf 1H/15min-Ebene, nicht nur beim finalen Entry-Gate.

## 3b. Screenshot nicht bei jedem Tick — nur für Muster/Candlestick-Checks (ergänzt 03.07.2026, User-bestätigt)
`capture_screenshot` ist nicht notwendig für reine Zahlen-Checks (Kurs/RSI/MACD/EMA) — die API-Werte reichen dafür vollständig aus.

**How to apply:**
- **Ruhige Phase:** Screenshot nur bei den vollständigen Checkliste-Durchläufen (zusammen mit dem 1H/15min-Bias-Check aus Punkt 3a), nicht bei jedem 1-Min-Tick dazwischen — dient dort der Chartmuster- (9d) und Candlestick-Prüfung (4) aus [[feedback_chartanalyse]], die sich schlecht aus reinen Zahlen ablesen lassen.
- **Heiße Phase:** komplett weglassen, nur Zahlen-Tools — Tempo hat Priorität, siehe Punkt 2b/7.

## 4. Permission-Prompts
Alle Standard-Trading-Tools sind in settings.local.json allowlisted — keine Approval nötig für:
quote_get, data_get_study_values, data_get_ohlcv, chart_get_state, capture_screenshot,
chart_set_symbol, chart_set_timeframe, data_get_pine_*, alert_*, watchlist_get, alle ui_* Tools

## 4a. Zusammenfassung: Kommunikation während aktivem Loop (ergänzt 01.07.2026, User-bestätigt)
Während der Cron-Loop aktiv beobachtet (Punkt 2a), ist jede Meldung genau eine von drei Sorten — sonst nichts, kein Fließtext:
1. **Warte-Status:** Entscheidungsbaum-Kasten (Punkt 7) zeigt, worauf noch gewartet wird
2. **Go-Signal:** Indikatoren erfüllt ✓, nur noch Kurs-Level abwarten (Punkt 7a)
3. **Kipp-Info:** Indikatoren nicht mehr erfüllt ✗, zurück in Wartephase (Punkt 7a Rückmeldung)

Nur bei echten News (Setup ändert sich, Indikatoren kippen, Kurs erreicht Zone) melden — das ist der ganze Zweck von wenig Text + Kästen in dieser Phase.

## 5. Memory während aktiver Session
Wenn wir bereits live in der Session sind — Memory NICHT nochmal komplett neu lesen bei jedem Check-in. Nur lesen wenn User explizit nach Levels/Regeln fragt.

## 6. Kompaktes Format gilt auch ohne /fast (bestätigt 01.07.2026)
Die kompakten Signal-Formate aus Punkt 2 gelten nicht nur im expliziten `/fast`-Modus, sondern immer beim Einstieg/Trigger-Momenten, auch in normalen Sessions.

**Why:** User hat am 01.07.2026 explizit gesagt "brauche zum Einstieg bitte weniger Text und klarere Ansagen" — auch außerhalb einer /fast-Session. Lange Erklärungen rund um den eigentlichen Trigger-Moment lenken ab und verzögern die Entscheidung.

**How to apply:** Bei Entry-Trigger-Momenten (Setup wird gültig/ungültig) immer zuerst die kompakte Kernaussage (Format aus Punkt 2), danach optional 1-2 Sätze Begründung falls nötig — nicht umgekehrt. Ausführliche Analysen bleiben für Session-Updates und Nachfragen reserviert, nicht für den Trigger-Moment selbst.

## 7. Entscheidungsbaum-Format kurz vor dem Entry (ergänzt 01.07.2026)
User-Wunsch: "nur Entrykurs eintragen, dann wenn X erreicht → dann Y, sonst warten bis auf Z" — explizites Wenn-Dann-Format statt Fließtext, gerade in der Phase kurz vor dem Trigger.

**Format (mit Farb-Markern, ergänzt 01.07.2026 — 🟢 = Long, 🔴 = Short, da echte Textfarben im Chat nicht möglich sind):**
```
Kurs: <aktueller Kurs>
🟢 Wenn <Bedingung 1> erreicht: LONG auslösen
🔴 Wenn <Bedingung 2> erreicht: SHORT auslösen
→ Sonst: warten
```

**Why:** In der Phase kurz vor dem Entry muss für den User sofort klar sein, was er bei welchem Kursstand tun soll, ohne den Text erst interpretieren zu müssen — Emotionen und Zeitdruck erschweren das Lesen von Fließtext in dem Moment (siehe [[feedback_regeldisziplin]] zur Emotionsregelung).

**How to apply:** Ab jetzt in der heißen Phase (Kurs nähert sich einer Trigger-Zone) dieses Wenn-Dann-Format statt Prosa verwenden. Sobald ein Trade offen ist oder noch keine Zone in Sichtweite ist, reicht das normale kompakte Format aus Punkt 2.

**7d. Kerzenfarbe/Schlusstyp bei jedem Preis-Trigger explizit benennen, nicht nur das Level (ergänzt 13.07.2026, Konkretisierung von [[feedback_chartanalyse]] Punkt 9c — kein eigenständiges neues Prinzip)**

Ein reines Preis-Level ("Bruch über 29.541") lässt offen, ob eine grün schließende Bestätigungskerze nötig ist (Schlusskurs über dem Level, bullische Kerze — stärkeres Signal) oder ob auch eine rot schließende Kerze reicht, solange der Schlusskurs über dem Level liegt (schwächeres, aber technisch noch gültiges Signal). Beide Fälle sind unterschiedlich aussagekräftig und dürfen nicht offen bleiben.

**Einordnung nach Fable-Review:** [[feedback_chartanalyse]] Punkt 9c verlangt bereits seit 01.07.2026 generell, JEDE Teilbedingung eines Setups explizit als erfüllt/nicht erfüllt zu benennen, statt nur die Preiszone zu nennen. Der heutige Vorfall war größtenteils Nichtbefolgung von 9c im Loop, nicht fehlendes Regelwissen — 7d hier ist die konkrete Anwendung von 9c auf den Sonderfall "Kerzenfarbe/Schlusstyp", kein separates neues Prinzip.

**Why:** User-Feedback 13.07.2026: "wie die Kerze schließen musste, ob es rot oberhalb einer Grenze oder eine grüne sein musste, das hast du mir nicht gesagt" — der Entscheidungsbaum nannte nur das Preis-Level, nicht die geforderte Kerzenfarbe/den Schlusstyp, wodurch die Bedingung mehrdeutig blieb.

**How to apply:** Bei JEDEM Trigger im Entscheidungsbaum (Punkt 7) explizit dazuschreiben, ob eine grüne Bestätigungskerze verlangt ist oder ein reiner Schlusskurs-Bruch (auch bei roter Kerze) ausreicht, z.B. "🟢 Wenn Kerze GRÜN über 29.541 schließt" vs. "🟢 Wenn Kerze (Farbe egal) über 29.541 schließt". Gilt für alle Preis-Trigger, nicht nur den finalen Entry-Trigger.

### 7a. Indikatoren proaktiv "freigeben", Kurs bleibt Users Job (ergänzt 01.07.2026, erweitert 02.07.2026)
User kann Indikatoren (RSI, MACD etc.) selbst nicht gut einschätzen, den NAS100-Kurs aber problemlos selbst ablesen. Klare Arbeitsteilung: Claude überwacht und meldet proaktiv, sobald die GESAMTE nicht-preisliche Seite einer Bedingung erfüllt ist — der User muss dann nur noch auf das Erreichen des Kurs-Levels warten, nicht selbst irgendetwas anderes interpretieren.

**Erweitert 02.07.2026:** "Indikatoren" umfasst hier die komplette Checkliste aus [[feedback_chartanalyse]] — nicht nur RSI/MACD/EMA, sondern auch Chartmuster (9d), Candlestick-Signale (4), Fibonacci (3), Multi-Timeframe-Bias (8) und RRR (9). Erst wenn ALLE diese Ebenen passen, gilt die Bedingung als "erfüllt ✓" — bei nur teilweise erfüllten Ebenen (z.B. RSI passt, aber Double-Top-Risiko besteht) explizit sagen was noch fehlt, nicht als vollständig "erfüllt" melden.

**Why:** User-Zitat: "Ich kann selber die Indikatoren nicht gut prüfen... du sagst mir dann, wann die Indikatoren passen und ich dann nur noch den Kurs abwarten muss." Reduziert die kognitive Last in stressigen Phasen auf das, was der User gut selbst kann.

**How to apply:** Sobald z.B. RSI>50 oder ein MACD-Crossover eintritt, das explizit und sofort melden (z.B. "Indikatoren jetzt erfüllt ✓ — warte nur noch auf Kurs über 30.020"), auch wenn der Kurs die Zone noch nicht erreicht hat. Im Entscheidungsbaum-Kasten (Punkt 7) den Indikator-Teil der Bedingung als bereits erfüllt/nicht erfüllt kennzeichnen, damit der User auf einen Blick sieht: "nur noch Kurs X abwarten".

**Rückmeldung bei Kippen (ergänzt 01.07.2026):** Wenn eine bereits als "erfüllt ✓" gemeldete Indikator-Bedingung wieder kippt (z.B. RSI fällt zurück unter 50), das genauso umgehend und explizit melden (z.B. "Achtung, Indikatoren nicht mehr erfüllt ✗ — RSI wieder unter 50, zurück in Wartephase"), nicht erst beim nächsten regulären Check. Gilt in beide Richtungen: erfüllt→nicht erfüllt und nicht erfüllt→erfüllt, jeweils sofort bei Eintritt.

## 7b. Zwei-Pane-Setup (NAS100 + QQQ) — Check-Frequenz phasenabhängig (ergänzt 03.07.2026, umgestellt auf QQQ 15.07.2026 nach Fable-Review)
Seit 03.07.2026 läuft ein 2-Pane-Layout (`2v`): Pane 0 = NAS100 (Preis/Entry, EMA/BB/RSI/MACD/ATR — Pivots seit 23.07.2026 durch ATR ersetzt, siehe [[feedback_chart_layout]]), Pane 1 = `BATS:QQQ` (Invesco QQQ Trust, Cboe-One-Echtzeitfeed, mit EMA50 + Volume + VWAP, siehe [[feedback_chart_layout]]). **Bis 15.07.2026 lief hier `CME_MINI_DL:NQ1!` — ersetzt, weil der NQ1!-Feed strukturell 10 Min verzögert war (siehe [[feedback_nq1_feed_lag]]).** Da NAS100 auf FOREXCOM-CFD kein echtes Volumen hat, ist QQQ die Quelle für echte Volumen-/VWAP-Bestätigung — ein Wert von dort auslesen erfordert weiterhin `pane_focus`-Wechsel, was Zeit kostet (wie der Timeframe-Wechsel aus Punkt 3a). **Gültigkeit: QQQ liefert nur während der US-Handelszeiten Daten — siehe Punkt 7e (Session-Gate). Volumen-Bewertung immer relativ zum eigenen QQQ-Durchschnitt auf demselben Chart, nie absolute Zahlen mit alten NQ1!-Werten vergleichen.**

**Zwei-Ebenen-Kriterium, präzisiert 15.07.2026 (löst eine 6 Tage alte Regel-Lücke auf):** Am 07.07.2026 (Trade #13) wurde EMA50-vs-EMA50 (beide Instrumente über/unter eigener EMA50) als Fix eingeführt, weil der reine VWAP-Vergleich bei einem frischen Reclaim zu träge/zu weit vom Kurs entfernt war — dieses Learning stand nie in Punkt 7b, nur in der Trade-Notiz. Bis 14.07.2026 wurde stattdessen wieder VWAP-Mittellinie als Kriterium verwendet, ohne dokumentierte Begründung (vermutlich unbemerkte Drift, keine bewusste Korrektur). Klärung 15.07.2026: **beide Kriterien bleiben, mit unterschiedlicher, sich ergänzender Rolle statt gegenseitigem Ersatz:**
- **EMA50-vs-EMA50 = Pflicht-Gate.** Reaktiv, bestätigt schnell, ob QQQ in dieselbe Richtung zieht wie das NAS100-Signal. Ohne diese Bestätigung kein Entry.
- **VWAP + Volumen = unterstützende Bestätigung, kein harter Blocker.** Beantwortet, ob die Bewegung durch echtes Volumen gedeckt ist — der eigentliche Mehrwert von QQQ gegenüber dem volumenlosen NAS100-CFD. Verstärkt das Signal, blockiert aber einen Entry nicht allein, wenn EMA50 und die übrige Checkliste (NAS100-Kerzenschluss, RSI, MACD-H, Muster) bereits stimmen — besonders relevant früh in der Session, wenn VWAP noch jung/dünn ist (siehe Punkt 7e).

**Ruhige Beobachtungsphase (noch kein Setup in Trigger-Nähe):** BEIDE Panes bei jedem Check-in checken — NAS100 volle Indikatoren-Sequenz (Punkt 3) UND QQQ Volume/VWAP (sofern Session-Gate aus Punkt 7e gerade offen ist). Kein Zeitdruck, also lieber vollständig prüfen als schnell.

**Heiße Phase (erste Indikatoren/Regeln aus der Checkliste [[feedback_chartanalyse]] bestätigen sich, Setup nähert sich dem Trigger):** Sofort zurückschalten auf NAS100-only pro Tick (Punkt 7/7a Format) — KEIN Pane-Wechsel zu QQQ mehr pro Loop-Durchlauf, um den Entry nicht durch Zeitverlust zu verpassen. Volume/VWAP-Bestätigung von QQQ dann nur noch EINMAL kurz vor dem eigentlichen Trigger als letzter Check, nicht wiederholt.

**Übergang zurück:** Sobald das Setup wieder kippt (Indikatoren nicht mehr erfüllt, zurück in Wartephase laut Punkt 7a Rückmeldung), zurück in die ruhige Phase wechseln → beide Panes wieder regelmäßig checken.

**Why:** User-Wunsch 03.07.2026: "in der ruhigen Phase ständig beides checken, wenn es heiß wird konkreter werden, damit wir den Entry nicht verpassen" — explizit zwei Modi, nicht dauerhaft nur NAS100 oder dauerhaft beide Panes.

**Kritisch — der letzte QQQ-Check vor dem Trigger ist ein echtes Gate, keine Formsache (ergänzt 03.07.2026, User-bestätigt; Symbol auf QQQ umgestellt 15.07.2026):** Auch in der heißen Phase gilt: KEIN Entry, solange nicht ALLES grün ist — Indikatoren auf NAS100 UND QQQ, Chartmuster, Candlestick-Signale, Trading-Regeln, History (die komplette Checkliste aus 7a/[[feedback_chartanalyse]]). Der eine finale QQQ-Check kurz vor dem Trigger (statt laufend) ist NICHT "einmal geprüft, gilt dann als erledigt" — wenn QQQ zu diesem Zeitpunkt nicht mehr bestätigt (Volumen/VWAP haben sich seit dem letzten Check gedreht), zählt das genauso als Kippen wie jede andere Bedingung aus Punkt 7a und blockiert den Entry, auch wenn der NAS100-Kurs die Zone gerade erreicht. Lieber einen Entry verpassen als blind gegen eine nicht mehr bestätigte QQQ-Volumen-Lage reingehen. **Gilt nur, wenn das Session-Gate (Punkt 7e) offen ist — außerhalb der US-Session entfällt der QQQ-Teil des Gates ersatzlos, siehe dort.**

**How to apply:** Im Entscheidungsbaum-Kasten (Punkt 7) den finalen Trigger-Moment erst als "grünes Licht" bestätigen, wenn explizit auch der letzte QQQ-Check (Volume + VWAP) zum Zeitpunkt des Kurs-Erreichens noch positiv war — nicht auf Basis eines älteren Checks aus der ruhigen Phase entscheiden.

**Kompakt-Statuszeile angepasst:** VWAP steht NICHT mehr in der Pro-Tick-Zeile aus Punkt 2 (die bleibt NAS100-only: Kurs/RSI/MACD/EMA50). VWAP erscheint nur noch zusammen mit Volume als eigene Zeile bei den QQQ-Checks (ruhige Phase laufend, heiße Phase einmalig).

## 7e. QQQ-Session-Gate — wann das Dual-Gate verfügbar ist (ergänzt 15.07.2026, Fable-Review)

QQQ (Pane 1) liefert nur während der US-Handelszeiten Daten, NAS100 (CFD) läuft dagegen fast 24/5. Drei Zonen (MESZ; bei US-Winterzeit je +1h prüfen):
- **15:30-22:00 (reguläre US-Session):** Dual-Gate voll gültig. Levis Kern-Entry-Fenster 16:00-18:00 MESZ liegt vollständig darin — im Normalbetrieb ändert sich nichts.
- **10:00-15:30 und 22:00-02:00 (Pre-/Post-Market, nur falls Extended Hours am QQQ-Chart aktiviert):** eingeschränkt — VWAP-Richtung als Kontext ja, Volumen-Spike-Kriterium nein (zu dünn). Kein hartes Gate.
- **02:00-10:00 und Wochenende/US-Feiertage:** kein QQQ-Gate verfügbar.

**Praktische Erkennung im Loop — Zeitstempel-Vergleich, nicht Uhrzeit-Raten:** Bei jedem QQQ-Check den Zeitstempel des neuesten QQQ-Bars (`data_get_ohlcv` → `time`) gegen den neuesten NAS100-Bar prüfen (dasselbe Verfahren wie beim NQ1!-Lag, [[feedback_nq1_feed_lag]]). **Differenz ≤ 1 Kerze (5 Min) → Gate gültig. Differenz > 1 Kerze → Gate NICHT verfügbar** — egal ob Ursache "Markt zu", "Feiertag/Half-Day" oder ein Feed-Problem ist; die Behandlung ist identisch. Die Uhrzeit-Zonen oben dienen nur der Vorab-Planung, der Zeitstempel-Check ist die verbindliche Prüfung (fängt US-Feiertage und verkürzte Handelstage automatisch mit). Wichtig: Bars von der Pane lesen (`data_get_ohlcv`), nicht `quote_get` — der liefert bei geschlossenem Markt kommentarlos den letzten Schlusskurs (vgl. [[feedback_datenquelle_nas100]]).

**Umgesetzt 15.07.2026:** Extended Hours auf der QQQ-Pane aktiviert (Sitzung-Menü, Button unten rechts im Chart: "RTH" → "Verlängerte Handelszeit"/ETH). Bestätigt per Screenshot (Anzeige wechselte RTH→ETH, Volumen stieg sichtbar durch den erweiterten Bereich). Damit haben auch die Vormittags-Updates (~14:30 MESZ, US-Pre-Market) einen QQQ-Kontext statt komplett eingefrorener Daten — Zeitstempel-Check aus diesem Punkt bleibt trotzdem Pflicht, weil ETH dünner/volatiler ist als RTH.

**Konsequenz, wenn das Gate nicht verfügbar ist:** Das Dual-Gate gilt als NICHT bestätigt (nicht als "übersprungen"). Entries, die laut Punkt 7b die Volumen-Bestätigung brauchen — also alle —, entfallen dann. Kein Entry außerhalb der US-Session ohne Volumen-Gate; das deckt sich mit dem bestehenden Trading-Zeitfenster ([[feedback_trading_zeitfenster]]). Für die Reversal-Kriterien (Punkt 11) gilt die dortige Ausnahme (Kriterium 3 entfällt, 2 von 3 verbleibenden nötig). Bei Vormittags-Session-Updates den fehlenden/dünnen QQQ-Stand explizit kennzeichnen ("QQQ Pre-Market, dünn" bzw. "QQQ geschlossen, Stand von gestern 22:00").

### 7c. MACD-Momentum-Beschleunigung/-Abschwächung ist selbst schon das Signal, nicht erst der Nulldurchgang (ergänzt 03.07.2026, User-Feedback nach Loop-Test; auf Entries erweitert 23.07.2026, Fable-Review)
Beim Loop-Test am 03.07.2026 schrumpfte das MACD-Histogram über mehrere Check-ins kontinuierlich (+7,0 → +0,1 → 0,0 → −0,3 → −1,0). Erst beim tatsächlichen Vorzeichenwechsel (H negativ) wurde es als ⚠️ Kipp-Info markiert — bei den Checks davor, als H schon klar Richtung 0 lief (+0,1), stand es nur neutral in der Statuszeile ohne Warnsymbol.

**Why:** User-Feedback: "war schon zu langsam, weil MACD schon direkt kam" — die eigentliche Vorwarnzeit liegt in der Abschwächung selbst (mehrere Checks vor dem Cross erkennbar), nicht erst im fertigen Vorzeichenwechsel. Wer erst beim Cross reagiert, hat den eigentlichen Informationsvorsprung verschenkt.

**How to apply (Exit-Seite, unverändert):** Bei jedem Check-in das MACD-Histogram nicht nur auf sein Vorzeichen prüfen, sondern auf die Richtung/Geschwindigkeit der Veränderung gegenüber dem letzten Check. Wenn |H| über 2-3 aufeinanderfolgende Checks spürbar schrumpft und sich der 0-Linie nähert (unabhängig vom aktuellen Vorzeichen), das SOFORT mit ⚠️ als "Momentum schwächt sich ab, möglicher Cross in Kürze" flaggen — nicht erst warten, bis der Cross tatsächlich passiert ist. Gilt analog für RSI-Annäherung an 50 oder an Überkauft/Überverkauft-Zonen: die Bewegung Richtung Schwelle ist die Vorwarnung, nicht erst das Erreichen der Schwelle selbst.

**Erweiterung auf Entries (23.07.2026, Fable-Review nach Levis Late-Entry-Beobachtung nach Trade #24):** Dieselbe Logik gilt spiegelbildlich für die Entry-Seite, bisher nur für offene Positionen genutzt. Während der Kurs eine bekannte S/R-Zone testet (noch VOR dem vollen Kerzenschluss-Trigger): Beschleunigt MACD-H über 2-3 aufeinanderfolgende Checks spürbar in die erwartete Ausbruchsrichtung (nicht nur Vorzeichenwechsel, sondern zunehmende Steigung), UND RSI bewegt sich dabei klar Richtung 50 bzw. von einer Extremzone weg in Trendrichtung — das ist die Vorwarnung für einen möglichen Ausbruch, nicht erst der fertige Kerzenschluss darüber. Kein eigenständiges neues Kriterium, sondern die Grundlage für einen tactischen Mini-Entry nach dem bereits bestehenden Zwei-Stufen-System (Punkt 8a1 in [[feedback_chartanalyse]]): kleinere taktische Teilposition an der Zone auf Basis dieser Beschleunigung, volle Position erst bei regulärer Bestätigung (Dual-Gate, Kerzenschluss). Ersetzt NICHT die reguläre Bestätigungspflicht für die volle Positionsgröße — reduziert nur den Lag für den taktischen Teil.

**Warnung (Fable-Review):** Weniger Bestätigung bedeutet zwangsläufig mehr Fehlsignale — das ist kein Trick, um den Lag ohne Kosten zu umgehen, sondern ein bewusster Tausch. Siehe Trade #8 (02.07., "Entry etwas zu früh, RSI erst 48,64") und Trade #12 als Beleg, dass ein zu früher Einstieg genauso zu Verlusten führt wie ein zu später. Der taktische Mini-Entry nach diesem Punkt braucht deshalb zwingend reduzierte Positionsgröße (analog Chop-Sizing/Punkt 13), nicht volle Größe — er ist ein Sizing-Kompromiss, kein Ersatz für die Bestätigung selbst.

**Drittes Vorwarn-Kriterium: Bollinger-Band-Squeeze (ergänzt 23.07.2026, Fable-Review):** Zusätzlich zu MACD-H-Beschleunigung und RSI-Annäherung: Verengen sich die Bollinger Bands (Ober-/Unterband rücken spürbar näher zusammen) während eines Zonentests, ist das ein drittes, richtungsneutrales Vorwarnsignal — enge Bänder erhöhen die Ausbruchswahrscheinlichkeit in absehbarer Zeit, sagen aber nichts über die Richtung. Kein neuer Indikator nötig (BB ist bereits im Set), nur eine zusätzliche Lesart der bestehenden Werte. Gilt genauso als Vorwarnung, nicht als Trigger — fließt in dieselbe taktische Mini-Entry-Logik wie oben ein.

**Bewusst NICHT als Vorwarn-Kriterium aufgenommen (Fable-Review, geprüft und abgelehnt):** ATR-Anstieg selbst (ATR ist 14-Perioden-geglättet, steigt strukturell erst NACH bereits größeren Kerzen — verschiebt das Lag-Problem nur, statt es zu lösen; ATR bleibt bei seiner Rolle als SL-Distanz-Maß, Punkt 8c). EMA50-Steigungsänderung (redundant zu MACD-H-Beschleunigung, kein eigenständiger Mehrwert). Nach MACD-H, RSI und BB-Squeeze ist diese Vorwarn-Liste bewusst geschlossen — keine weiteren Kandidaten aus dem aktuellen Indikator-Set nachschieben, um nicht erneut einen unter Zeitdruck brechenden "Vollständigkeits"-Block aufzubauen (vgl. Rückbau der Muster-/Fibonacci-Pflicht am 21.07.2026).

## 8. Positions-Kasten direkt unter dem Entry-Kasten (ergänzt 01.07.2026, TP1/TP2 ergänzt 04.07.2026, Zertifikatspreis-Pflicht ergänzt 16.07.2026)
Zweiter fester Block, der zusammen mit dem Entscheidungsbaum aus Punkt 7 gezeigt wird, sobald eine Positionsgröße feststeht (z.B. beim Trigger-Moment oder auf Nachfrage).

**Format (mehrzeiliger Block, um TP1/TP2-Teilverkauf-Standard ergänzt — siehe [[feedback_chartanalyse]] Punkt 8b):**
```
Position: <Betrag>€
Hebel: <Nx>
SL: <-X,XX%> unter Entry
SL: <NAS100-Kurs> (≈<Zert.-Preis>€)
TP1: <NAS100-Kurs> (≈<Zert.-Preis>€) — 50% schließen, Rest-SL auf Breakeven
TP2: <NAS100-Kurs> (≈<Zert.-Preis>€) — Rest, mit laufenden Schutzsignalen
```

**Why:** User will Positionsgröße, Hebel und SL-Abstand in % genauso klar und knapp sehen wie die Entry-Bedingungen selbst — beide Kästen zusammen ergeben das vollständige Bild für die Entscheidung im heißen Moment. TP1/TP2 stehen seit 04.07.2026 mit im Kasten, weil der Teilverkauf jetzt Standard ist, nicht mehr Ausnahme.

**Zertifikatspreis-Pflicht (16.07.2026, User-Korrektur nach Trade #21):** SL/TP1/TP2 IMMER zusätzlich zum NAS100-Kurs als geschätzter Zertifikatspreis in € angeben, nicht nur als Indexpunkte — der User braucht den €-Wert, um am Broker (Scalable) tatsächlich zu handeln, nicht den Indexstand. Kein MCP-Tool liefert den echten Broker-Zertifikatspreis, deshalb per linearer Näherung aus dem bekannten Entry-Paar hochrechnen: `Ratio = Hebel × Entry-Zert.-Preis / Entry-NAS100-Kurs` (€ pro Punkt), dann `Zert.-Preis(Level) ≈ Entry-Zert.-Preis ± Ratio × (Level − Entry-NAS100)` (Vorzeichen je nach Long/Short beachten — bei Short steigt der Zert.-Preis, wenn der Index fällt). Immer als Näherung kennzeichnen (Spread/Finanzierungskosten nicht erfasst) und den echten Broker-Wert als Quelle der Wahrheit benennen (siehe [[feedback_broker_wert_prioritaet]]) — sobald der User den echten aktuellen Zertifikatspreis nennt, die Ratio damit neu kalibrieren.

**How to apply:** Direkt nach/unter dem Entscheidungsbaum-Kasten (Punkt 7) anzeigen, sobald eine Positionsgröße berechnet wurde. Der SL-Wert immer als Prozent-Abstand zur Entry-Zone angeben, nicht nur als absoluter Kurswert. Bei TP1-Erreichen während einer offenen Position aktiv melden ("TP1 erreicht — 50% schließen, Rest-SL auf Breakeven"), nicht erst auf Nachfrage.

## 9. Voll-Check-Rhythmus — löst den Widerspruch zwischen Checklisten-Tiefe und Tempo auf (ergänzt 04.07.2026, Fable-5-Review)

**Das Problem:** [[feedback_chartanalyse]] verlangte bisher die volle Checkliste (Chartmuster 9d, Candlestick 4, Fibonacci 3, echter MTF-Wechsel 8) bei JEDEM Check, auch im 1-Min-Loop — das widerspricht Punkt 3a/3b/7b hier, die in der heißen Phase Screenshot, Timeframe-Wechsel und Pane-Wechsel verbieten. Ohne diese Tools ist Muster-/Candlestick-/MTF-Prüfung nicht durchführbar. Die "vollständige Prüf-Tiefe im Hintergrund" war damit teilweise eine unbelegte Behauptung statt echter Prüfung.

**Die Lösung: Tempo und Tiefe entkoppeln, statt einen Kompromiss zwischen beiden zu suchen.** Die volle Checkliste läuft nicht im 1-Min-Takt, sondern an klar definierten Zeitpunkten — dazwischen gilt ihr letztes Ergebnis als "eingefroren" und wird im schnellen Zahlen-Loop (Punkt 3) einfach mitgeführt, nicht neu erhoben.

**Verbindliche Voll-Check-Zeitpunkte (jeder einzelne löst einen vollständigen Durchlauf inkl. Screenshot + Chartmuster + Candlestick + Fibonacci + echtem MTF-Wechsel aus):**
1. **Jeder 5-Min-Kerzenschluss** — auch während eine Position bereits offen ist, durchgehend bis zum Exit, nicht nur vor dem Entry
2. **Übergang ruhig→heiß** — der Moment, in dem der Entscheidungsbaum (Punkt 7) zum ersten Mal für dieses Setup gezeigt wird. Das ist der bisher fehlende Fixpunkt: bei Trade #10 (02.07.2026) gab es keinen definierten "letzten Voll-Check unmittelbar vor der heißen Phase", weshalb das Double-Top nicht auffiel
3. **Unmittelbar vor dem tatsächlichen Entry** — finales Gate, ein älterer Check (auch vom letzten Kerzenschluss) zählt nicht mehr, wenn seither Zeit vergangen ist
4. **Sofort bei extremer Zahlen-Bewegung zwischen zwei Kerzen** — RSI/MACD-Histogram wechselt das Vorzeichen, oder Kurs bewegt sich seit dem letzten Voll-Check um mehr als die aktuelle Rausch-Schwelle (siehe [[feedback_chartanalyse]] Punkt 8c). Das fängt neue Muster ab, die sich mitten in der heißen Phase bilden, ohne auf den nächsten Kerzenschluss zu warten

**Zwischen zwei Voll-Checks:** Nur die schnelle Zahlen-Sequenz aus Punkt 3 (Kurs/RSI/MACD-Histogram inkl. Richtung nach 7c/EMA50 — VWAP nur bei QQQ-Checks, siehe Punkt 7b) gegen den zuletzt eingefrorenen Muster-/Candlestick-/MTF-Kontext bewerten — keine neue Bilderkennung, kein neuer Timeframe-Wechsel.

**Why:** Löst das Dilemma "vollständige TA vs. Entry nicht verpassen" nicht durch Abstriche an der Tiefe, sondern durch einen anderen Takt: die schwere visuelle Analyse ist nie älter als eine Kerze (max. 5 Min) oder ein echtes Zahlen-Ereignis, während der 1-Min-Loop für die eigentliche Reaktionsgeschwindigkeit weiterhin nur Arithmetik macht.

**How to apply:** Bei jedem der 4 Trigger-Punkte oben aktiv einen vollständigen Durchlauf machen (auch wenn das kurz Zeit kostet) und das Ergebnis explizit als Kontext für die folgenden schnellen Ticks mitführen, z.B. "Voll-Check 15:32: kein Chartmuster gegen die Position, MTF-Bias unverändert Long — gilt bis zum nächsten Kerzenschluss/Trigger." In den schnellen Ticks dazwischen NICHT so tun, als wäre gerade neu auf Muster geprüft worden — nur die Zahlen bewerten und explizit sagen, dass die visuelle Einschätzung vom letzten Voll-Check stammt.

**Verschärfung nach Trade #16 (ergänzt 09.07.2026):** Nach TP1-Erreichen (Teilverkauf) wurde der MTF-Voll-Check komplett vernachlässigt — nur der schnelle Zahlen-Loop lief bis zum SL-Exit, kein einziger echter 1H/15min-Timeframe-Wechsel während der gesamten Restlaufzeit der Position. Erst eine explizite User-Nachfrage deckte die Lücke auf. **TP1-Erreichen ändert NICHTS an der Voll-Check-Pflicht** — die Rest-Position (nach Teilverkauf) unterliegt denselben 4 Trigger-Punkten wie die volle Position, bis sie komplett geschlossen ist. User-Zitat: "das muss unbedingt beachtet werden."

**Korrektur 21.07.2026 (zweiter Fable-Review nach Trade #23) — der Pflichtwortlaut "inkl. ... Fibonacci ... Chartmuster" war aspirativ, nicht real eingehalten.** Audit ergab: Fibonacci wurde nur opportunistisch (einmalig für eine TP-Ableitung) statt bei jedem Voll-Check berechnet, und die 9d-Musterliste wurde nur reaktiv benannt (erst nach explizitem User-Hinweis auf Double-Top-Verdacht), nicht proaktiv komplett durchgegangen — beides ohne Offenlegung. Fables Entscheidung (begründet mit dem wiederkehrenden Muster aus Punkt 9a/Trade #16: "vollständige" Pflichtlisten brechen strukturell unter Zeitdruck, egal wie sie formuliert sind): **lieber ein ehrlich reduzierter, realistisch einhaltbarer Standard als eine Vollständigkeit, die nur auf dem Papier steht.**

**Realistischer Pflicht-Umfang pro Voll-Check (ersetzt den alten Wortlaut):**
- **MTF-Wechsel** (15min + 1H, NAS100 + QQQ falls Session-Gate offen) — bleibt uneingeschränkt Pflicht, keine Abstriche
- **Chartmuster:** Screenshot + visuelle Prüfung auf die 3-4 zum aktuellen Kontext wahrscheinlichsten Muster (z.B. Double-Top/-Bottom bei Test einer bereits bekannten Zone, Flag/Pennant bei Konsolidierung nach Impuls, Keil bei erkennbarer Trendabschwächung) — NICHT mehr die komplette ~15-Muster-Liste aus [[feedback_chartanalyse]] Punkt 9d bei jedem Voll-Check einzeln durchgehen
- **Fibonacci:** NUR berechnen, wenn eine erkennbare Trendbewegung/ein Impuls vorliegt, für die Retracement-/Extension-Level tatsächlich relevant sind (z.B. TP-Ableitung ohne bekanntes Level) — kein Routine-Bestandteil jedes Voll-Checks mehr

**Offenlegungspflicht (neu, nicht verhandelbar):** Im Voll-Check-Output immer kurz kennzeichnen, was geprüft wurde und was bewusst ausgelassen wurde, z.B. "Voll-Check: Double-Top/Flag geprüft (kein Muster erkennbar), Fibonacci nicht berechnet (kein neuer Impuls)." Ein stillschweigendes Auslassen ohne diese Kennzeichnung gilt ab jetzt selbst als Regelbruch — unabhängig vom Ergebnis des Trades, analog zur bestehenden Regelbruch-Klassifizierung in [[trades/trade_log]].

## 9a. Minute für den Voll-Check-Rhythmus IMMER per echter Systemzeit bestimmen, nie selbst hochzählen (ergänzt 13.07.2026)

Bei jedem `CronCreate`-Fire die tatsächliche Minute NICHT aus einer eigenen mitgeführten Zählung ("letzter Fire war 16:41, also ist das jetzt 16:42") ableiten — das driftet unbemerkt auseinander von der echten Uhrzeit, weil Cron-Jitter, Bearbeitungsdauer und gelegentliche Zwischenfragen die Zählung verschieben können, ohne dass ein Fire übersprungen wird.

**How to apply:** Bei jedem Fire zuerst per `Bash date` (oder gleichwertig) die echte Systemzeit abfragen, daraus die Minute % 5 (Voll-Check) bzw. % 10 (Tweet-Fetch) bestimmen. Bar-Timestamps aus `data_get_ohlcv` sind ebenfalls eine verlässliche Realzeit-Quelle als Gegenprobe, falls `date` mal nicht verfügbar ist.

**Why:** Am 13.07.2026 driftete die selbst mitgeführte Minutenzählung während einer Live-Session unbemerkt vor die echte Uhrzeit (ein "16:41"-Voll-Check kam tatsächlich schon bei 16:44 dran) — der User musste das live korrigieren. Reine Selbstzählung ist strukturell unzuverlässig für einen Vorgang, bei dem die Minute selbst die Entscheidungsgrundlage ist (Voll-Check vs. Quick-Tick, Tweet-Fetch-Rhythmus).

## 10. Permission-Modus: Auto statt Manual (ergänzt 10.07.2026)
User hat Claude Code auf **Auto Mode** umgestellt (statt Manual Mode) explizit für Live-Trading-Sessions, damit Tool-Calls (Kurs/Indikatoren/OHLCV parallel abrufen, Alerts, Drawings, Voll-Checks aus Punkt 9) nicht durch Permission-Prompts verzögert werden.

**Why:** User-Begründung: "wir brauchen Speed und er muss alles was an Regeln hinterlegt ist automatisch abchecken" — deckt sich mit Punkt 2b (Geschwindigkeit pro Check-in ist der eigentliche Engpass).

**Wichtige Abgrenzung:** Auto Mode beschleunigt nur Claudes eigene Tool-Calls (TradingView-MCP-Tools, Chart-Lesen/-Steuern, Scripts). Es gibt **kein** MCP-Tool zur echten Order-Ausführung beim Broker (Scalable Capital) — reale Trades bleiben manuell und erfordern weiterhin aktive Bestätigung durch den User, siehe [[feedback_order_bestaetigung]]. Auto Mode ändert daran nichts.

**How to apply:** In Live-Trading-Sessions Auto Mode als Standard voraussetzen (keine wiederholten Permission-Nachfragen erwarten). Bei tatsächlichen Order-Schritten beim Broker trotzdem immer aktiv nach Bestätigung fragen (Richtung/Stückzahl/Preis) — dieser Schritt läuft außerhalb von Claude Code und wird von Auto Mode nicht berührt.

## 11. Proaktives Reversal-Management bei offener Position (ergänzt 13.07.2026, umbenannt von "9b" nach Fable-Review 13.07.2026)

**Umbenennungs-Hinweis:** Diese Regel hieß ursprünglich "Punkt 9b" — das kollidierte mit dem bereits bestehenden [[feedback_chartanalyse]] Punkt 9b (Divergenz-Regel, seit 23.06.2026), der etwas anderes bedeutet. Als eigenständiger Punkt 11 hier verschoben, um die Nummern-Kollision aufzulösen.

**Vorrang-Klärung zu bestehenden Exit-Regeln (ergänzt nach Fable-Review):** Diese Regel überschneidet sich mit [[feedback_chartanalyse]] Punkt 4a (Kerze+Chartmuster gegen die Position = hartes Exit-Signal) und Punkt 9d1 (laufende Muster-Beobachtung während offener Position) — beide waren schon vorher gültig und decken "nicht passiv bis zum SL warten" bereits ab. **Klarstellung:** Punkt 4a/9d1 bleiben das primäre, schnellere Signal für reine Gewinnsicherung/Exit (eine erkannte Umkehrkerze + Muster reicht dort allein). Punkt 11 hier ist eine zusätzliche, striktere Schwelle NUR für die Entscheidung, aktiv auf die Gegenrichtung zu drehen (also selbst eine neue Position in Gegenrichtung vorzuschlagen) — dafür braucht es die volle 2-3-von-4-Bestätigung, nicht nur ein einzelnes Muster-Signal. Ein Exit kann also schon bei 4a/9d1 ausgelöst werden, ohne dass automatisch schon ein Dreh auf die Gegenrichtung gerechtfertigt ist.

User-Vorgabe: Solange die Long-These (bzw. Short-These) intakt ist, bleibt die Position offen und läuft normal Richtung SL/TP1/TP2. Sobald sich die These aber erkennbar dreht, BEVOR der SL erreicht wird, aktiv vorschlagen, die Position vorzeitig (im Zweifel leicht im Minus vor dem eigentlichen SL) zu schließen und auf die Gegenrichtung zu drehen — nicht passiv bis zum SL abwarten, wenn sich abzeichnet, dass die Gegenseite übernimmt.

**Konkrete Kipp-Kriterien "These dreht" (bei Long, spiegelbildlich bei Short):**
1. MACD-H (5-Min) kreuzt negativ UND bestätigt sich über 2+ aufeinanderfolgende Checks (kein einzelner Ausreißer)
2. UND Kurs bricht klar unter EMA50, mit einer Folgekerze, die den Bruch bestätigt (kein reiner Wick/Intrabar-Touch)
3. UND QQQ bricht im selben Zeitraum unter eigener EMA50 (Pflicht-Teil, siehe Punkt 7b) — VWAP/Lower Band mit relativ erhöhtem Volumen verstärkt das Signal zusätzlich, ist aber kein eigenständiges Muss (Dual-Gate-Bestätigung auch für die Gegenrichtung, seit 15.07.2026 auf QQQ statt NQ1!). Falls das QQQ-Gate laut Punkt 7e gerade nicht verfügbar ist (außerhalb US-Session), entfällt dieses Kriterium ersatzlos — dann müssen 2 der verbleibenden 3 Kriterien erfüllt sein.
4. 15-Min/1H beschleunigen weiter in die Gegenrichtung, statt nur abzukühlen

Erst wenn mindestens 2-3 dieser Kriterien gleichzeitig erfüllt sind, gilt das als echtes Reversal-Signal — einzelne schwächelnde Kerzen oder normale Pullbacks reichen nicht.

**Why:** User-Zitat 13.07.2026: "wenn sich Long auf Short dreht, dann müssen wir handeln und im Minus vor SL im Zweifel verkaufen und auf Short drehen, aber solange noch Long in Takt ist, bleiben wir drin." Der SL ist die Kapital-Notbremse, kein Ziel — bei einem erkennbaren Regimewechsel innerhalb der Position soll aktiv reagiert werden statt den vollen SL-Verlust abzuwarten, wenn die eigentliche These schon vorher erkennbar kippt.

**How to apply:** Bei jedem Check-in während einer offenen Position die 4 Kriterien oben gegen den aktuellen Stand prüfen. Sobald 2-3 gleichzeitig kippen, das explizit und sofort als Handlungsaufforderung melden (nicht nur als normalen Status), inklusive Vorschlag für Exit-Preis und ggf. neuer Gegenrichtungs-Setup — User entscheidet dann über die tatsächliche Order.

**Belegpflicht für jeden Punkt-11-Exit (ergänzt 22.07.2026, Fable-Review nach Trade #24):** Bei Trade #24 wurde ein 50%-Exit mit "Reversal-Kriterien erfüllt" begründet, obwohl keines der vier Kriterien oben mit einem konkreten Wert (MACD-H-Zahl, EMA50-Bezug, QQQ-Stand, 15min/1H-Richtung) belegt war — nur "zwei Fehlversuche an einer Zone" (das ist ein 9d1-Chartmuster-Signal, kein Punkt-11-Kriterium). Ab jetzt gilt: Ein Punkt-11-Exit/Dreh-Vorschlag ist nur zulässig, wenn im Output explizit steht, WELCHE der vier Kriterien mit welchem konkreten Wert erfüllt sind (z.B. "MACD-H 5min: -2,1, seit 2 Checks negativ ✓ | EMA50: Kurs 29.006 unter 29.041, Folgekerze bestätigt ✓ | QQQ: über eigener EMA50, Kriterium NICHT erfüllt ✗ | 15min: MACD-H flach, keine Beschleunigung ✗ → 2/4, Exit gerechtfertigt"). Ein Exit ohne diese Auflistung gilt als nicht belegt und ist stattdessen über 9d1/4a (reines Muster-Exit, dort reicht ein einzelnes Signal) oder Punkt 12 (Stall) zu begründen, nicht über Punkt 11.

## 12. Stall-Exit-Regel — proaktiver Teilgewinn ohne vollen Reversal abzuwarten (ergänzt 21.07.2026, Trade #23; überarbeitet nach Fable-Review 21.07.2026)

Ergänzt Punkt 11 um eine weichere, häufiger greifende Regel für Fälle, in denen der Kurs schlicht die Puste ausgeht, ohne dass eine harte Umkehr (Punkt 11) oder ein klassisches Umkehr-Chartmuster (9d1) vorliegt — besonders relevant, wenn TP1/TP2 (wie bei Trade #23) rein RR-mathematisch statt technisch fundiert sind und weit entfernt liegen.

**Fable-Review-Ergebnis (21.07.2026):** Die ursprüngliche Fassung kollidierte ungeklärt mit [[feedback_chartanalyse]] Punkt 9d1 (ein 3-Kerzen-Seitwärtslauf sieht strukturell wie eine Flag/Pennant-Konsolidierung aus, die 9d1 explizit als Halten-Signal wertet) und nutzte mit "RSI kühlt spürbar ab" keinen belastbaren Zahlenanker. n=1 (nur Trade #23, dort zusätzlich mit Eskalations-News vermischt) reicht nicht, um die Regel schon als endgültig zu gelten. Alle vier Punkte unten setzen die Fable-Empfehlung um.

**Regel (überarbeitet):**
1. **Vorrangklausel zuerst prüfen:** Zeigt der Chart parallel ein erkennbares Fortsetzungsmuster nach 9d1 (Flag, Pennant, Rectangle, Channel) in Positionsrichtung? Wenn ja, **gewinnt 9d1 (halten)** — Punkt 12 greift in diesem Fall NICHT, unabhängig vom Kerzen-/RSI-Stand.
2. Nur wenn kein solches Fortsetzungsmuster erkennbar ist: Wenn **3 aufeinanderfolgende 5-Min-Kerzenschlüsse** kein neues Hoch (bei Long, spiegelbildlich Tief bei Short) über das bisherige Bewegungs-Hoch machen UND der RSI dabei **mindestens 8-10 Punkte von seinem lokalen Extrem** gefallen ist (fester Zahlenanker statt "spürbar") → aktiv einen **Teilgewinn (25-50% der Restposition, analog Punkt 8e)** vorschlagen, NICHT automatisch den Vollexit.
3. Rest-Position läuft mit den bestehenden Schutzsignalen (9d1, Punkt 11, SL) weiter — Punkt 12 ersetzt keinen Vollexit-Mechanismus, sondern ergänzt ihn um einen frühen Teilverkauf.

**Warum 3 Kerzen:** Lang genug, um normales kurzes Verschnaufen (mehrfach an diesem Tag beobachtet) nicht fälschlich als Stillstand zu werten, kurz genug (~15 Min), um bei echtem Bewegungsende nicht unnötig Gewinn verstreichen zu lassen.

**Warum Teilgewinn statt Vollexit:** Senkt die Kosten eines Fehlsignals — trifft die Regel eine normale Konsolidierung vor Fortsetzung (das reale Risiko laut Fable-Review), wird nur ein Teil zu früh realisiert, nicht die ganze Position.

**Timing-Klarstellung (ergänzt nach zweitem Fable-Review 21.07.2026):** Der Exit-/Teilgewinn-Vorschlag nach dieser Regel darf **frühestens BEIM tatsächlichen 3. Kerzenschluss selbst** ausgesprochen werden — niemals schon auf Basis der Zwischen-Tick-Zahlen (Punkt 3), auch wenn RSI/Kurs 1-2 Minuten vorher schon "reif" aussehen. Grund: Kurs/RSI werden jede Minute frisch abgerufen, während die Pflicht-Gegenprobe gegen die 9d1-Vorrangklausel (Fortsetzungsmuster-Check, braucht Screenshot) nur beim echten Kerzenschluss läuft. Ein vorgezogener Exit-Vorschlag würde diese Gegenprobe umgehen. Punkt 3b (kein Screenshot in der heißen Phase) ist hier NICHT als Freibrief zu lesen, die 9d1-Prüfung zu überspringen — eine offene Position mit nahendem Stall-Trigger bleibt trotzdem an den regulären Voll-Check-Rhythmus (Punkt 9: jeder 5-Min-Kerzenschluss) gebunden.

**Review-Pflicht (Bedingung für den Fortbestand dieser Regel, analog 7a1):** Nach den nächsten **5 Anwendungsfällen** explizit prüfen, ob die Regel tatsächlich einen Unterschied gemacht hat (Teilgewinn gesichert, der sonst verloren gegangen wäre, ohne dabei echte Fortsetzungen abzuschneiden). Zeigt sich kein klarer Nutzen oder überwiegen die abgeschnittenen Fortsetzungen, Regel wieder streichen statt als ungenutzte Karteileiche stehen zu lassen — Basis: n=1 bei dieser Überarbeitung, weiterhin nicht statistisch abgesichert.

**Bewährt bei Trade #23 (erster, noch nicht nach neuer Fassung geprüfter Fall):** Nach dem Ausbruch über 29.076/29.144 (neues Hoch) gab es keine bekannten Widerstände mehr, TP1/TP2 lagen weit und ohne echtes Level. Die (damals noch als Vollexit gefasste) Stall-Regel griff nach der 3. Kerze ohne neues Hoch (kombiniert mit einsetzender Eskalations-News, also nicht rein durch die Stall-Logik selbst bestätigt) und ermöglichte einen Exit bei +0,81%.

**How to apply:** Bei jedem Check-in während einer offenen Position (zusätzlich zu Punkt 11) den Abstand zum letzten bestätigten Hoch/Tief mitzählen. Nach der 3. Kerze ohne neuen Extremwert, kombiniert mit erkennbarer RSI-Abkühlung, aktiv einen Exit vorschlagen — unabhängig davon, ob TP1/TP2 erreicht sind oder ein hartes Reversal-Kriterium (Punkt 11) bereits vorliegt.

**Deckelung — Rest bewusst weiterlaufen lassen, wenn der Trend intakt bleibt (ergänzt 23.07.2026, Fable-Review + Levi-Entscheidung):** Der Teilgewinn-Vorschlag nach Punkt 12 bleibt **auf 25-50% begrenzt, NIE Vollexit**, solange (a) der übergeordnete 1H/15min-Bias ([[feedback_chartanalyse]] Punkt 8) weiterhin klar in Positionsrichtung zeigt UND (b) keines der vier Punkt-11-Reversal-Kriterien erfüllt ist. Ein Vollexit vor TP2 ist unter diesen Bedingungen nur über Punkt 11 selbst (echtes Reversal, 2-3 von 4 Kriterien) oder ein hartes 9d1/4a-Muster+Kerze-Signal möglich, nicht über den Stall-Exit allein. **Why:** Fable-Audit 23.07.2026 stellte fest, dass die proaktiven Exit-Regeln (8e, 9d1/4a, 11, 12) einseitig Richtung "früher raus" ziehen, ohne dass irgendwo aktiv ausgesprochen wurde, wann der Rest bewusst weiterlaufen soll — das bremst das realisierte RR (siehe [[feedback_realisiertes_rr]]), obwohl Punkt 12 selbst schon immer nur "Teilgewinn, KEIN Vollexit" verlangte. Diese Ergänzung macht das nur explizit, ändert die bestehende Logik nicht, gilt zusätzlich zur bereits bestehenden Vorrangklausel (9d1-Fortsetzungsmuster schlägt Punkt 12).

## 13. Chasing-Situation vor Entry — halbierte Position statt Vollentry, kein separates Konsolidierungs-Add (ergänzt 22.07.2026, nach Trade #24, Fable-Review)

**Ausgangslage:** Bei Trade #24 wurde exakt am vereinbarten Zeit-Gate (16:00) in eine bereits 4-5 Kerzen lang klar gerichtete Bewegung hinein eingestiegen, ohne frische Bestätigung am aktuellen Kurs (Chasing-Entry). User-Vorschlag danach: in so einer Situation nur 50% der Position sofort nehmen und die zweite Hälfte gezielt bei einer Konsolidierung/einem kleinen Rücksetzer nachkaufen.

**Fable-Einordnung:** Ein separates "Konsolidierungs-Add" auf Vorrat löst das Chasing-Problem nicht, sondern chast nur mit halber Größe — das ist Risikobegrenzung, keine Signalverbesserung, und würde ohne Pflicht-Bestätigungskerze gefährlich nah an den verbotenen Nachkauf-Typ B (Verbilligen ohne neue Bestätigung, siehe [[project_risikomanagement]]) heranrücken. Kein neuer, eigenständiger Regelmechanismus nötig — die bestehenden Werkzeuge decken den legitimen Kern bereits ab.

**Regel:**
1. **Chasing-Situation erkannt** (4-5 klar gerichtete Kerzenschlüsse ohne Konsolidierung, keine frische Bestätigung genau am aktuellen Kurs): kein Vollentry. Bei gewünschter Teilnahme nur die bereits bestehende halbierte Position (wie bei Chop-/Schock-Kontext, siehe [[feedback_chartanalyse]] Punkt 10) mit EINEM SL für die gesamte kleinere Position — kein zweiter, separat geplanter Konsolidierungs-Tranche.
2. Ein Nachkauf auf die zweite Hälfte ist ausschließlich unter den bestehenden Nachkauf-Typ-A-Bedingungen erlaubt ([[project_risikomanagement]]: ursprünglicher SL nicht ausgelöst, echtes tieferes technisches Level, Bestätigungskerze, Gesamtrisiko neu über `size.cjs` berechnet und weiter ≤1,5%/225€) — nicht als vorab geplantes "Add bei irgendeinem Rücksetzer".
3. Löst die halbierte Position bereits vorher über Punkt 11/12 aus (Reversal/Stall), bevor ein gültiges Typ-A-Nachkauf-Signal kommt, entfällt die zweite Tranche ersatzlos — kein Nachjagen der ursprünglichen Vollgröße um jeden Preis.

**Why:** User-Vorschlag 22.07.2026 nach Trade #24 als Reaktion auf den erkannten Chasing-Entry. Fable-Review bestätigt das Grundmotiv (Risiko begrenzen, wenn man doch teilnehmen will), lehnt aber einen eigenständigen neuen "Scale-in"-Mechanismus ab, weil er strukturell identisch mit vorhandenen Werkzeugen (Chop-Sizing + Nachkauf-Typ-A) ist und ohne deren Bedingungen (insb. Bestätigungskerze, Gesamtrisiko-Neuberechnung) ein Rückfall in Typ-B-Averaging wäre.

**How to apply:** Vor jedem Entry aktiv prüfen, ob eine Chasing-Situation vorliegt (siehe Kriterium oben). Falls ja, im Entry-Signal (Punkt 2) explizit kennzeichnen ("Chasing-Situation — halbierte Position, kein Vollentry") und Positionsgröße entsprechend halbieren. Ein späteres Aufstocken nur mit vollständiger Typ-A-Prüfung, nie automatisch.

**Stacking-Hinweis (ergänzt 23.07.2026):** Trifft gleichzeitig Chop (siehe [[feedback_chartanalyse]] Punkt 7) oder das 15:30-16:00-Zeit-Gate (siehe [[feedback_trading_zeitfenster]]) zu, wird trotzdem nur einmal halbiert — kein Multiplizieren mehrerer Gründe. Zentrale Regel dazu in [[project_risikomanagement]], Abschnitt "Stacking-Regel für gleichzeitige Halbierungs-Gründe".
