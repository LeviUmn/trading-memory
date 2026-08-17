---
name: feedback-live-trading
description: "Live-Trading-Protokoll für maximale Geschwindigkeit — Signal-Format, Check-in-Sequenz, Fast-Mode"
metadata:
  node_type: memory
  type: feedback
  originSessionId: trading-session-2026-06-26
  modified: 2026-08-17T16:28:21.937Z
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
und (60) → VWAP-Verhältnis + Volumen + eigene EMA50 lesen → chart_set_timeframe wieder auf 15 setzen
(QQQ-Pane bleibt sonst auf 60min stehen, ergänzt 31.07.2026 User-Korrektur) → zurück auf Pane 0 (NAS100).
Der 1H-Schritt (NAS100) UND der QQQ-Schritt (falls Gate offen) sind NICHT optional — ein
Voll-Check ohne den 60-Min-Wechsel bzw. ohne den QQQ-Durchlauf gilt als NICHT durchgeführt
und wird beim nächsten Fire nachgeholt und als Lücke gemeldet.
Sonst (Minute % 5 != 0) → Quick-Tick (Punkt 3).
```

**Erweiterung 27.07.2026 (nach zwei Prozessfehlern — Format-Drift zur Tabelle + zweimaliger Tweet-Fetch-Ausfall, siehe Punkt 9 "Struktureller Anti-Drift-Fix"):** Direkt im selben Pflichtbaustein zusätzlich wörtlich mitgeben, nicht als separate Erinnerung an anderer Stelle:

```
Am Ende JEDES Voll-Checks zusätzlich zwingend:
(1) Output-Vorlage aus [[feedback_vollcheck_format]] Zeile für Zeile kopieren und befüllen,
    NIE aus dem Gedächtnis neu formulieren/als Tabelle umbauen. Letzte Zeile immer
    "Format: Fließtext ✓".
(2) x_last_fetch.json lesen, Minuten seit letztem Tweet-Fetch aus dem Zeitstempel berechnen
    (NICHT mental mitzählen) → bei ≥10 Min Pflicht-Fetch + Pflicht-Ausgabezeile
    "Tweet-Check: ..." (siehe Punkt 9) — IMMER ausgeben, auch bei "nichts Neues" oder
    "noch nicht fällig", nie stillschweigend weglassen.
Fehlt eine der beiden Zeilen im Output, gilt der Voll-Check als NICHT durchgeführt —
exakt dieselbe Behandlung wie beim fehlenden 1H-/QQQ-Schritt oben.
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

## 2c. Positionsgrößen-Bestätigung bei Entry — Stückzahl gegen Positionsgröße rückrechnen (ergänzt 07.08.2026, Fable-Review nach Trade #35)

Bei Trade #35 (07.08.2026) nannte die Entry-Meldung 276 Stk., tatsächlich waren es 376 Stk. — über ~10 Nachrichten/Ticks (17:04-18:04 Uhr) wurden SL/TP/P&L-Anzeigen im Positions-Kasten mit der falschen Stückzahl berechnet, bis der User es beim ersten Teilverkauf korrigierte. Ökonomisch war das folgenlos (SL/TP sind preis-, nicht stückzahlbasiert, das reale Risiko war die ganze Zeit korrekt), aber prozessual riskant: unentdeckt hätte die falsche Zahl in eine Stacking- oder Limit-Entscheidung einfließen können, und kein bestehender Mechanismus hätte das automatisch aufgedeckt.

**Fix:** Bei der ersten Positions-Kasten-Ausgabe nach jeder manuellen Entry-Meldung die genannte Stückzahl × Zert.-Preis gegen die genannte Positionsgröße (€) rückrechnen. Bei Abweichung >1€ aktiv nachfragen, statt die Zahl unkommentiert zu übernehmen — nicht stillschweigend mit der zuerst genannten Zahl weiterrechnen.

**Bewusst NICHT übernommen (Sonnet 5 + Fable 5, nach gemeinsamer Prüfung):** Der Vorschlag, den SL-Nachzug auf Breakeven bereits bei Erkennung eines Momentum-Fade-Signals statt erst nach einem Teilverkauf durchzuführen, wurde verworfen. Begründung: Ein vorgezogener BE-SL hätte bei einem Whipsaw zwischen Fade-Erkennung und Ausführung die GESAMTE Position ohne realisierten Gewinn auslösen können — die tatsächlich gewählte Reihenfolge (erst Teilverkauf realisieren, dann Rest absichern) garantiert dagegen immer mindestens den Teilgewinn, unabhängig vom weiteren Kursverlauf. Es gibt kein Szenario, in dem BE-zuerst einen echten Vorteil bringt, aber eines (Whipsaw vor Ausführung), in dem es strikt schlechter ist — Fable hat den eigenen ursprünglichen Vorschlag nach dieser Gegenprüfung selbst zurückgenommen. Die bisherige Reihenfolge (Teilverkauf → SL-Nachzug) bleibt also unverändert Standard.

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

**Chart-Linien (`draw_shape`) für die 🟢/🔴-Trigger erst kurz vor Entry zeichnen, nicht schon in der reinen Setup-Suche (ergänzt 03.08.2026, User-Korrektur):** Der Text-Entscheidungsbaum selbst darf schon früh gezeigt werden (siehe oben), aber die zugehörigen Linien im Chart erst dann einzeichnen, wenn fast alle Indikatoren + das Dual-Gate (Punkt 7b) erfüllt sind — also kurz vor dem eigentlichen Trigger, nicht schon während beide Zonen noch weit auseinanderliegen und sich die Level-Einschätzung noch verschieben kann. **Why:** User-Zitat: "Sonst machen die Linien kein Sinn, weil wir die sonst ständig neu setzen müssten" — vorschnell gezeichnete Linien erzeugen unnötigen Redraw-Aufwand, wenn sich die Setup-Struktur vor Erreichen der heißen Phase noch ändert.

**7d0. Vor JEDER Trigger-Meldung prüfen, ob der letzte Bar aus `data_get_ohlcv` überhaupt schon geschlossen ist (ergänzt 03.08.2026, User-Korrektur nach Live-Fehler)**

Bei Trade-Vorbereitung #29 (03.08.2026) wurde ein SHORT-Trigger als "AUSGELÖST" gemeldet, weil der letzte Bar im OHLCV-Pull (Startzeit 15:30, 5-Min-Timeframe) einen Schlusskurs unter dem Trigger-Level zeigte — dieser Bar lief zum Zeitpunkt der Meldung aber noch bis 15:35, war also gar keine abgeschlossene Kerze, sondern nur der aktuelle Zwischenstand. User musste live korrigieren ("Kerze läuft noch bis 15:35, nicht geschlossen").

**Why:** `data_get_ohlcv` liefert für den letzten Bar immer den aktuellen Live-Stand, keine Kennzeichnung ob die Kerze bereits geschlossen ist. Ohne expliziten Zeit-Check sieht ein Wick/Zwischenstand optisch identisch aus wie ein echter Schlusskurs — genau die Verwechslung, die Punkt 7d (Kerzenfarbe/Schlusstyp) eigentlich schon adressiert, hier aber auf einer Ebene davor (ist die Kerze überhaupt fertig?).

**How to apply:** Vor jeder Trigger-Meldung (🟢/🔴 ausgelöst) den Zeitstempel des letzten Bars gegen die echte Uhrzeit aus `Bash date` prüfen: Bar-Startzeit + Timeframe-Dauer (z.B. 5min-Bar von 15:30 läuft bis 15:35) muss ≤ aktuelle Zeit sein, sonst ist es kein abgeschlossener Schluss, sondern ein Zwischenstand/Wick-Test. Bei offener Kerze im Status explizit "Test läuft, Kerze offen bis HH:MM" schreiben statt "Trigger ausgelöst".

**Strukturelle Verschärfung (05.08.2026, Fable-Tagesabschluss-Review nach 3. Rückfall in Folge — 03.08./04.08./05.08., am 05.08. sogar zweimal am selben Tag):** Ein reiner Prüfhinweis reicht nicht mehr, da die Prüfung wiederholt gedanklich übersprungen wurde, obwohl die Regel bekannt war — dasselbe Rückfall-Muster wie bei anderen strukturell abgesicherten Pflichtzeilen (Tweet-Check, Format-Zeile). Deshalb ab jetzt eine verpflichtende Ausgabezeile, exakt wie die bereits bestehenden Pflichtzeilen aus Punkt 9/[[feedback_vollcheck_format]] behandelt: **Vor JEDER Trigger-Meldung (Entry, TP-Erreichen, SL-Nachzug, Struktur-Bruch) explizit die Zeile "Kerze geschlossen: JA (Ende HH:MM)" oder "Kerze geschlossen: NEIN (läuft bis HH:MM)" ausgeben** — nicht nur intern prüfen und weglassen. Fehlt diese Zeile bei einer Trigger-Meldung, gilt die Meldung als nicht regelkonform, unabhängig davon ob der Trigger im Nachhinein zufällig richtig lag.

**Why:** Fable-Review 05.08.2026: "Eine Regel, die drei Tage in Folge bricht und nur durch User-Eingriff gerettet wird, ist keine stabile Regel mehr, sondern ein Prozess-Loch." Ein interner Check ohne sichtbaren Beleg ist nicht überprüfbar und wird unter Zeitdruck übersprungen — eine Pflichtzeile im Output macht die Prüfung erzwingbar, analog zum bereits bewährten Muster bei Punkt 9.

**7d. Kerzenfarbe/Schlusstyp bei jedem Preis-Trigger explizit benennen, nicht nur das Level (ergänzt 13.07.2026, Konkretisierung von [[feedback_chartanalyse]] Punkt 9c — kein eigenständiges neues Prinzip)**

Ein reines Preis-Level ("Bruch über 29.541") lässt offen, ob eine grün schließende Bestätigungskerze nötig ist (Schlusskurs über dem Level, bullische Kerze — stärkeres Signal) oder ob auch eine rot schließende Kerze reicht, solange der Schlusskurs über dem Level liegt (schwächeres, aber technisch noch gültiges Signal). Beide Fälle sind unterschiedlich aussagekräftig und dürfen nicht offen bleiben.

**Einordnung nach Fable-Review:** [[feedback_chartanalyse]] Punkt 9c verlangt bereits seit 01.07.2026 generell, JEDE Teilbedingung eines Setups explizit als erfüllt/nicht erfüllt zu benennen, statt nur die Preiszone zu nennen. Der heutige Vorfall war größtenteils Nichtbefolgung von 9c im Loop, nicht fehlendes Regelwissen — 7d hier ist die konkrete Anwendung von 9c auf den Sonderfall "Kerzenfarbe/Schlusstyp", kein separates neues Prinzip.

**Why:** User-Feedback 13.07.2026: "wie die Kerze schließen musste, ob es rot oberhalb einer Grenze oder eine grüne sein musste, das hast du mir nicht gesagt" — der Entscheidungsbaum nannte nur das Preis-Level, nicht die geforderte Kerzenfarbe/den Schlusstyp, wodurch die Bedingung mehrdeutig blieb.

**How to apply:** Bei JEDEM Trigger im Entscheidungsbaum (Punkt 7) explizit dazuschreiben, ob eine grüne Bestätigungskerze verlangt ist oder ein reiner Schlusskurs-Bruch (auch bei roter Kerze) ausreicht, z.B. "🟢 Wenn Kerze GRÜN über 29.541 schließt" vs. "🟢 Wenn Kerze (Farbe egal) über 29.541 schließt". Gilt für alle Preis-Trigger, nicht nur den finalen Entry-Trigger.

**7f. RR-Gate (Live, vorläufig) — laufende RR-Anzeige in der heißen Phase, nicht erst beim fertigen Entry-Signal (ergänzt 17.08.2026, Fable-Review nach Trade #36)**

Ergänzt das bestehende Wenn-Dann-Format aus Punkt 7 (Kurs / 🟢🔴 / Sonst warten, siehe oben) um eine zusätzliche Pflichtzeile, die den RR-Check aus Punkt 2 (bisher erst an das fertige Entry-Signal gekoppelt) schon während der laufenden Beobachtung in der heißen Phase sichtbar macht:

```
RR-Gate (Live, vorläufig): SL-Floor [X Pkt, ATR-Formel 8c] → TP1 [nächstgelegenes Pivot-Level aus 7a1a] → RR ca. [Y]:1 — Gate ✓/✗
```

**Beispiele (Trade #36, 17.08.2026):**

✗-Fall (vor Entry, Kurs 30.153, SL-Floor 30.105 = 48 Pkt, TP1-Kandidat R1 30.183,15 = 30 Pkt):
```
Kurs: 30.153 | Setup: Long-Anlauf an R1
🟢 Wenn Kurs R1 (30.183,15) mit bestätigtem Schlusskurs überwindet: LONG auslösen
→ Sonst: warten
RR-Gate (Live, vorläufig): SL-Floor 30.105 (48 Pkt, ATR 8c) → TP1 R1 30.183,15 (30 Pkt) → RR ≈0,63:1 — Gate ✗ NICHT erfüllt
```

✓-Fall (nach bestätigtem R1-Durchbruch, TP1 auf Tageshoch 30.246,10 aktualisiert):
```
Kurs: 30.192 | Setup: Long, R1 überwunden
🟢 Wenn Kurs sich über R1 hält / nächster Schlusskurs bestätigt: LONG auslösen
→ Sonst: warten
RR-Gate (Live, vorläufig): SL-Floor 30.105 (48 Pkt, ATR 8c) → TP1 Tageshoch 30.246,10 (93 Pkt) → RR ≈1,94:1 — Gate ✓ erfüllt
```

**Why:** Bei Trade #36 (17.08.2026) erfolgte der Entry manuell, bevor die RR-Prüfung überhaupt sichtbar/verfügbar war — der RR-Check war bisher ausschließlich an das fertige Entry-Signal aus Punkt 2 gekoppelt (dort: "wird VOR jedem Entry-Signal durchgerechnet, aber nicht als Zwischenschritt ausgesprochen"), nicht an die laufende Beobachtung während der heißen Phase in Punkt 7. Diese Lücke ermöglichte einen manuellen Vorgriff, ohne dass die RR-Zahl je gezeigt wurde. Diese Ergänzung macht das Gate schon während der Beobachtung durchgehend sichtbar, bevor ein solcher Vorgriff passieren kann.

**How to apply:** Die Zeile erscheint ab Eintritt in die heiße Phase (sobald Punkt 7 überhaupt aktiv wird, siehe "How to apply" weiter oben) bei jedem Update und aktualisiert sich automatisch, sobald sich das TP-Bild ändert (z.B. nach einem Levelbruch wie oben). Sie ersetzt NICHT den finalen RR-Check bei echtem Entry (der bleibt unverändert an die tatsächlich gewählte SL/TP-Struktur nach [[feedback_chartanalyse]] Punkt 8b/8c gekoppelt) — sie ist ein Frühindikator/Transparenz-Mechanismus. Kein relevanter Tempo-Mehraufwand, da Punkt 7 ohnehin ausschließlich in der heißen Phase läuft, nicht im minütlichen Quick-Tick (Punkt 3).

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

**Erweiterung auf parabolische/extreme Bewegungen — reduzierte Bestätigungsschwelle gilt auch OHNE offene Position (ergänzt 30.07.2026, nach dem 30.07.-Melt-up):** Am 30.07.2026 lief ab ~14:30 ein extremer, vermutlich Gamma-getriebener Melt-up (RSI 5min über ~45 Min. von ~75 auf 92, ohne jede Konsolidierung, NAS100 ~+3,5% am Tag). User-Feedback dazu: "wenn die Gegenbewegung kommt, müssen wir reagieren, auch wenn der Markt mit Indikatoren kein Short sagt — das ist eine Einladung." Das ist dieselbe Logik wie oben (Beschleunigung/Abschwächung ist selbst schon das Signal), aber bisher nur für zwei Fälle formuliert: (a) Exit/Reversal bei bereits offener Position, (b) taktischer Mini-Entry beim Testen einer bekannten S/R-Zone. Der User erweitert das jetzt explizit auf einen dritten Fall: **kein offener Trade, keine S/R-Zone, sondern eine Bewegung, die selbst durch ihre Extremität (RSI seit mehreren Kerzen >85-90, keine einzige Konsolidierung) zur Vorwarnstufe wird.**

**Why:** Je länger und ungebremster eine Bewegung in eine Extremzone läuft, desto höher die A-priori-Wahrscheinlichkeit einer scharfen Gegenbewegung, wenn sie kommt — der volle Reversal-Kriterienkatalog aus Punkt 11 (2-3 von 4, inkl. QQQ-Bruch und Folgekerzen-Bestätigung) ist für eine offene Position gedacht und reagiert bewusst träge, um Fehlsignale zu vermeiden. Nach einem derart extremen, unkonsolidierten Lauf ist dieselbe Trägheit aber zu langsam, um wenigstens die ERSTEN Anzeichen (kleinere Kerzen, RSI fällt vom Extrem, MACD-H-Wachstum stoppt) aktiv und prominent zu benennen — auch wenn daraus noch kein vollständiges Short-Signal wird.

**How to apply:** Sobald eine Bewegung als "parabolisch/extrem" erkannt ist (Faustregel: RSI 5min ≥3 aufeinanderfolgende Voll-Checks über 85, ohne jede Konsolidierungskerze), das erste Auftreten EINES der drei Vorwarn-Kriterien (MACD-H-Wachstum stoppt/dreht, RSI fällt vom lokalen Hoch, kleinere Kerzenkörper) sofort explizit und mit erhöhter Dringlichkeit meiden — nicht erst wenn 2-3 von 4 Punkt-11-Kriterien vorliegen. Das ist weiterhin kein automatisches Short-Signal (volle Bestätigung bleibt Pflicht für einen tatsächlichen Entry), aber der Übergang von "reine Beobachtung" zu "aktiv auf Kipp-Anzeichen fokussieren" passiert früher und wird dem User klar als solcher benannt, nicht erst rückwirkend.

**Review-Pflicht (ergänzt 30.07.2026, Fable-Tagesabschluss-Review):** Diese Erweiterung lief am 30.07.2026 zum ersten Mal live (n=1) und markierte den MACD-H-Whipsaw ab ~16:05 korrekt und rechtzeitig als dringliches Kipp-Anzeichen, ohne dass daraus ein verfrühter Short-Fehlalarm wurde (kein Entry erfolgte, die Bewegung blieb im 1H/15min-Bild bullisch). Nach den nächsten 2-3 Anwendungsfällen prüfen, ob die Schwelle (RSI ≥3 Checks über 85) zu oft false positives erzeugt (dringliche Meldung, aber keine echte Korrektur folgt) oder zu spät greift — dann Schwelle nachjustieren oder Regel streichen, analog zur Review-Pflicht anderer frisch eingeführter Regeln (z.B. Punkt 7a1, Punkt 12).

**Fünfte Erweiterung — Warnsignal → aktive Positions-Verteidigung, unabhängig von Punkt 11 (ergänzt 31.07.2026, nach Trade #28, User-Kritik + Fable-Tagesabschluss-Review)**

Bisher endete die Exit-Seite von 7c (siehe oben, "How to apply (Exit-Seite, unverändert)") bei einer reinen ⚠️-Markierung ("Momentum schwächt sich ab") — was danach folgen sollte, war in der Praxis implizit an Punkt 11 delegiert (2-3 von 4 Kriterien, inkl. EMA-Bruch+Folgekerze und QQQ-Bestätigung). Das ist strukturell falsch kalibriert: Punkt 11 ist laut eigener Vorrang-Klärung (siehe dort) ausdrücklich NUR für die Entscheidung "aktiv auf die Gegenrichtung drehen" gedacht — eine bewusst hohe, träge Schwelle, weil ein verfrühter Dreh eine neue Position mit echtem frischem Risiko eröffnet. Für die schwächere, andere Frage "die bereits offene (ggf. schon teilrealisierte) Position schützen, ohne zu drehen" ist dieselbe hohe Schwelle unpassend — die Kosten-Asymmetrie ist umgekehrt: ein zu früher Teilverkauf kostet nur entgangenen weiteren Gewinn, ein zu spät verteidigter Round-Trip zurück auf Breakeven/Verlust gibt bereits Erreichtes vollständig zurück.

**Konkreter Vorfall:** Bei Trade #28 (31.07.2026, Short NAS100) lief das MACD-Histogramm (5min) über 10+ aufeinanderfolgende 1-Min-Checks (ca. 16:38-16:49) ohne eine einzige Gegenbewegung kontinuierlich von ca. -40 über die Nulllinie hinaus weiter steigend. Der User fragte zweimal aktiv nach ("sieht komplett nach Reversal aus" / "drehen wir komplett?"), beide Male wurde mit dem Punkt-11-Zähler (1/4, dann 1,5/4) geantwortet — die eigentliche Frage war aber nie "sollen wir auf Long drehen", sondern "ist unsere offene Restposition in Gefahr". Die formale "2+ Checks bestätigt"-Auslegung aus Punkt-11-Kriterium 1 wurde dabei sogar strenger verstanden, als 7c selbst es für die reine Exit-*Warnung* verlangt (dort reichen bereits 2-3 Checks) — die Warnung hätte nach eigener 7c-Logik schon deutlich früher aktiv gemeldet werden müssen, wurde aber nie in einen konkreten Handlungsvorschlag übersetzt, weil dafür kein eigenständiger Trigger existierte.

**Neue Regel:** Sobald das 7c-Warnsignal (MACD-H bewegt sich 2-3+ Checks spürbar in eine Richtung gegen die Position, ODER RSI bewegt sich klar weg vom Extrem Richtung 50 in dieselbe Gegenrichtung) über **5 aufeinanderfolgende 1-Minuten-Checks ohne eine einzige Gegenbewegung** anhält, ist das für eine bereits offene Position ein EIGENSTÄNDIGER, aktionabler Trigger — Teilgewinn (25-50% der Restposition) UND/ODER SL-Nachzug vorschlagen (gleiche Handlungsform wie Punkt 12, NIE automatischer Vollexit) — unabhängig davon, ob die vollen Punkt-11-Kriterien (EMA-Bruch+Folgekerze, QQQ-Bestätigung) schon erfüllt sind. Punkt 11 bleibt unverändert die hohe Schwelle für einen aktiven Dreh auf eine neue Gegenrichtungsposition — dieser neue Trigger ersetzt das nicht, sondern schließt nur die Lücke davor (Position schützen ohne zu drehen).

**Verstärkter Trigger (niedrigere Schwelle) bei bereits teilrealisierter Position:** Läuft die Restposition bereits nach einem Punkt-12-Teilverkauf (formal oder diskretionär wie bei Trade #28) mit SL auf/nahe Breakeven, reduziert sich die Schwelle auf **3 aufeinanderfolgende 1-Minuten-Checks**. Begründung: Die Position hat bereits ein Erschöpfungssignal gezeigt (Punkt 12 hat schon ausgelöst) — ein zweites, unabhängiges Gegensignal (anhaltende adverse Bewegung) rechtfertigt eine schnellere Reaktion, weil hier konkret ein Round-Trip auf 0 statt eines gesicherten Zwischengewinns droht (exakt das Ergebnis bei Trade #28: 114 Stück endeten bei genau 0€ statt einem gesicherten Zwischengewinn während des Bounces).

**Why:** Löst die vom User beschriebene Asymmetrie zwischen Entry- und Verteidigungs-Situation auf, ohne Punkt 11 selbst aufzuweichen (das würde das Dreh-Risiko erhöhen, wofür es keinen Beleg gibt — Punkt 11 bleibt komplett unverändert). Macht außerdem den bisher unvollständigen Satz aus 7c ("Exit-Seite, unverändert" — Warnung ohne Handlungsschritt) endlich vollständig: Warnung UND konkrete Handlungsschwelle, nicht nur Warnung.

**Kein Ersatz für 9d1/4a oder Punkt 12:** Diese Regel prüft ausschließlich die Zahlen-Sequenz (MACD-H/RSI aus Punkt 3, keine Screenshot-/Musterprüfung nötig) und läuft deshalb auch im 1-Min-Quick-Tick, nicht nur im Voll-Check. Ein 9d1-Muster oder ein bestätigtes Punkt-11-Reversal bleiben davon unabhängige, weiterhin gültige eigenständige Trigger.

**Reconciliation mit Punkt 12, Unterpunkt 12.3 (ergänzt 31.07.2026, nach der finalen Trade-#28-Nachbesprechung):** Punkt 12 wurde im selben Zug verschärft (2 Kerzenschlüsse + RSI ≥5 Punkte statt 3/8-10). Beide Regeln — diese hier und Punkt 12/12.3 — bleiben bewusst als unabhängige, PARALLELE Trigger bestehen, kein Duplikat: Punkt 12 ist Kerzenschluss-basiert (mind. 10 Min bis zum Trigger) und erkennt reine Erschöpfung/Seitwärtslauf OHNE dass eine aktive Gegenbewegung nötig ist; diese 7c-Erweiterung ist 1-Minuten-Tick-basiert (kann in 3-5 Min auslösen) und erkennt speziell eine AKTIVE, anhaltende Gegenbewegung, auch wenn gar keine Kerzenschluss-Stall-Phase vorausging. Wer zuerst auslöst, löst den identischen Handlungsvorschlag aus (Teilgewinn/SL-Nachzug, nie Vollexit/Dreh) — siehe Punkt 12.3 für die vollständige Abgrenzung.

**Gegengeprüft und bewusst NICHT umgesetzt — TP1-Neukalibrierung für die Restposition:** Der zweite Kritikpunkt aus der Trade-#28-Nachbesprechung (TP1 sei für die Restposition nach dem Punkt-12-Teilverkauf falsch kalibriert gewesen) wurde unabhängig geprüft und als Hindsight Bias eingestuft, nicht übernommen: TP1 lag bei 27.849, der Kurs erreichte nur 27.952,55 als Extremum und drehte danach — TP1 wurde nie auch nur annähernd erreicht oder relevant. Ein näher gesetztes TP1 hätte am tatsächlichen Ablauf nichts geändert, weil der Round-Trip komplett VOR jedem TP-Level stattfand. Die eigentliche Lücke war das Fehlen eines aktiven Verteidigungs-Triggers während der Gegenbewegung (siehe oben), nicht die Distanz eines nie erreichten Ziel-Levels — ein neues TP-Rekalibrierungs-Prinzip würde das falsche Problem lösen und ist deshalb nicht Teil dieser Nachschärfung.

**Review-Pflicht (analog 7a1/Punkt 12/vierte 7c-Erweiterung 30.07.):** n=0 bei Einführung (Trade #28 selbst zeigte die Lücke, wurde aber noch nicht mit dieser Regel gehandhabt, da sie erst danach eingeführt wurde). Nach den nächsten 3-5 Anwendungsfällen prüfen: löst der Trigger zu oft bei einem normalen Pullback aus (false positives, die eine echte Fortsetzung vorzeitig abschneiden) oder verhindert er zuverlässig Round-Trips wie bei Trade #28? Bei Bedarf Schwelle (5 bzw. 3 Checks) nachjustieren oder Regel wieder streichen, statt sie als ungenutzte Karteileiche stehen zu lassen.

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

**Reihenfolge bei Voll-Check auf einen exakten Makro-Release-Zeitpunkt (ergänzt 03.08.2026, User-Vorgabe vor dem 16:00-ISM-Doppel-Event):** Fällt ein 5-Min-Voll-Check-Zeitpunkt exakt mit einem bekannten Makro-Release zusammen (z.B. 16:00 ISM Manufacturing PMI + Employment), zuerst die frischen Makrodaten einholen (Tweet-Accounts, ggf. vom User bereitgestellter Screenshot), DANACH erst den eigentlichen Voll-Check durchführen — die Indikatoren-/MTF-Bewertung soll die neue Zahl bereits einpreisen, nicht hinterherhinken. Reihenfolge also: Makro-Zahl → dann Voll-Check (nicht umgekehrt, nicht parallel ohne Reihenfolge).

**Realistischer Pflicht-Umfang pro Voll-Check (ersetzt den alten Wortlaut):**
- **MTF-Wechsel** (15min + 1H, NAS100 + QQQ falls Session-Gate offen) — bleibt uneingeschränkt Pflicht, keine Abstriche
- **Chartmuster:** Screenshot + visuelle Prüfung auf die 3-4 zum aktuellen Kontext wahrscheinlichsten Muster (z.B. Double-Top/-Bottom bei Test einer bereits bekannten Zone, Flag/Pennant bei Konsolidierung nach Impuls, Keil bei erkennbarer Trendabschwächung) — NICHT mehr die komplette ~15-Muster-Liste aus [[feedback_chartanalyse]] Punkt 9d bei jedem Voll-Check einzeln durchgehen
- **Fibonacci:** NUR berechnen, wenn eine erkennbare Trendbewegung/ein Impuls vorliegt, für die Retracement-/Extension-Level tatsächlich relevant sind (z.B. TP-Ableitung ohne bekanntes Level) — kein Routine-Bestandteil jedes Voll-Checks mehr

**Offenlegungspflicht (neu, nicht verhandelbar):** Im Voll-Check-Output immer kurz kennzeichnen, was geprüft wurde und was bewusst ausgelassen wurde, z.B. "Voll-Check: Double-Top/Flag geprüft (kein Muster erkennbar), Fibonacci nicht berechnet (kein neuer Impuls)." Ein stillschweigendes Auslassen ohne diese Kennzeichnung gilt ab jetzt selbst als Regelbruch — unabhängig vom Ergebnis des Trades, analog zur bestehenden Regelbruch-Klassifizierung in [[trades/trade_log]].

**Struktureller Anti-Drift-Fix nach zwei Prozessfehlern am 27.07.2026 (dritter Fable-Review, Tagesabschluss — siehe [[feedback_prozessfehler_27_07_fuer_fable]]):**

Am 27.07.2026 liefen über 100 Ticks in einem einzigen langen 1-Min-Loop. Dabei sind zwei im Regelwerk längst stehende Pflichten wiederholt untergegangen — das Voll-Check-Ausgabeformat (Drift zur Tabelle statt Fließtext, siehe [[feedback_vollcheck_format]]) und der Minute%10-Tweet-Fetch (siehe unten, "How to apply" 13.07.2026), Letzterer musste der User zweimal (18:11 UND erneut 19:16 Uhr, nach dazwischenliegenden Voll-Checks) aktiv einfordern. **Root-Cause — beide Fehler teilen dieselbe Struktur, unabhängig vom jeweiligen Inhalt:**

1. **Beide sind "stille" Prüfungen.** Der Output bei "nichts zu tun/nichts Neues" sah identisch aus wie der Output bei "gar nicht geprüft" — es gab keinen erzwungenen sichtbaren Beleg, dass die Prüfung überhaupt stattfand. Genau dieses Muster hat schon einmal denselben Fehlertyp verursacht (Punkt 2b, 1H-MTF-Vergessen bei Trade #18/#19) und wurde dort gelöst, indem der Schritt PFLICHT-sichtbar gemacht wurde ("gilt als NICHT durchgeführt, wenn er fehlt"). Für Format und Tweet-Fetch fehlte dieser erzwungene sichtbare Beleg bisher.
2. **Beide verlassen sich auf reine Ausführungsdisziplin über eine sehr lange, repetitive Tick-Folge**, statt auf einen persistierten, prüfbaren Zustand. Die Minute%10-Regel musste bei jedem der 100+ Ticks neu korrekt mental nachvollzogen werden, mitten in einer langen Liste anderer Punkt-9-Teilschritte (MTF, QQQ, Chartmuster, Fibonacci, Regime-Gate, Offenlegung) — ein einziges Aussetzen reicht, damit die Lücke entsteht, und blieb dann unbemerkt bis zur nächsten Zufalls-Nachfrage des Users.
3. Das erste Mal (18:11 Uhr) wurde die Lücke mit einem reinen "ab jetzt dran denken" geschlossen — genau das ist beim zweiten Mal (19:16 Uhr) trotzdem wieder passiert. Ein Versprechen ist kein struktureller Fix, solange die Ursache (still + rein gedächtnisbasiert über 100+ Ticks) unverändert bleibt.

**Der Fix — wendet den bei Punkt 2b bereits bewährten Mechanismus auf beide Fehler an, statt ein neues Prinzip zu erfinden:**

a) **Format wird Teil des Pflicht-Outputs selbst, nicht nur eine externe Stilregel.** Jeder Voll-Check-Output übernimmt ab jetzt die Vorlage aus [[feedback_vollcheck_format]] Zeile für Zeile (kopieren + befüllen, NIE aus dem Gedächtnis neu formulieren) und endet zwingend mit der Meta-Zeile `Format: Fließtext ✓` — eine Tabelle hat strukturell keinen natürlichen Platz für diese abschließende Prosa-Zeile, das Fehlen der Zeile ist selbst schon ein Warnsignal.
b) **Tweet-Fetch-Fälligkeit wird aus dem persistierten `x_last_fetch.json`-Zeitstempel berechnet, nicht aus mentalem Minute%10-Mitzählen.** Bei JEDEM Voll-Check zuerst die Datei lesen, Differenz zur echten Systemzeit (`Bash date`, Punkt 9a) bilden. ≥10 Minuten → Fetch fällig — robuster als reine Modulo-Prüfung, die nach nur einem übersprungenen Voll-Check sofort um 5 Minuten verschieben kann. Ergebnis erscheint als PFLICHT-Zeile im Output, in JEDEM Fall, auch bei "nichts Neues" oder "noch nicht fällig": `Tweet-Check: fällig+durchgeführt ✓ (nichts Neues)` / `Tweet-Check: noch nicht fällig (zuletzt vor Xmin)` / `Tweet-Check: ÜBERFÄLLIG ✗ → jetzt nachgeholt`. Eine fehlende Tweet-Check-Zeile gilt ab jetzt genauso als Regelbruch wie ein fehlender 1H-MTF-Schritt — das bisher erlaubte "bei nichts Neues reicht auch gar keine Erwähnung" (siehe unten, altes "How to apply") ist damit für den Fälligkeits-Status selbst aufgehoben; es gilt weiterhin NUR für den Tweet-Inhalt (keine Tweet-Liste bei Nicht-Relevanz), nicht mehr für die Statuszeile.
c) **Beide Pflicht-Zeilen werden zusätzlich wörtlich in den CronCreate-Pflichtbaustein aus Punkt 2b aufgenommen** (siehe dort, aktualisierte Fassung) — derselbe Mechanismus, der das 1H-MTF-Vergessen seit 16.07.2026 nachweislich gelöst hat (kein Vorfall seither dokumentiert), wird hiermit auf die beiden neuen Fehlerquellen erweitert, statt eine dritte, unabhängige Lösung zu erfinden.

**Ehrliche Einordnung (dem Auftrag entsprechend):** Ein Rest-Risiko bleibt — eine Pflicht-Zeile kann im Prinzip mechanisch mitgeschrieben werden, ohne dass die Prüfung dahinter ehrlich stattfand. Der Unterschied zum bisherigen Zustand: eine fehlende oder erkennbar falsch befüllte Pflicht-Zeile ist für den User ab jetzt sofort sichtbar und gegenprüfbar (z.B. "Tweet-Check: noch nicht fällig" lässt sich gegen die echte Uhrzeit checken) — vorher war der Fehler komplett unsichtbar, bis er zufällig nachfragte. Das verschiebt die Fehlerklasse von "unsichtbar, nur durch Zufall entdeckt" zu "sichtbar, prüfbar, aber theoretisch fälschbar" — eine echte Verbesserung, kein vollständiger Ausschluss. Falls sich in den nächsten Sessions zeigt, dass die Pflicht-Zeilen trotzdem mechanisch ohne echte Prüfung mitlaufen, ist das explizit als Fortbestehen des Problems zu benennen, nicht erneut nur als "ab jetzt besser aufpassen" zu verbuchen.

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

**Ergänzt 31.07.2026 (nach Trade #28):** Für eine anhaltende, aber noch nicht voll bestätigte Gegenbewegung gegen eine offene Position (Zahlen-Trend eindeutig, aber Muster/EMA/QQQ noch nicht so weit) ist WEDER Punkt 11 (zu hohe Schwelle, für Dreh gedacht) NOCH 4a/9d1 (braucht ein sichtbares Muster/Kerze) die richtige Regel — dafür siehe die neue Erweiterung in Punkt 7c ("Warnsignal → aktive Positions-Verteidigung"), die genau diese Lücke mit einer eigenen, niedrigeren Zahlen-basierten Schwelle für Teilgewinn/SL-Nachzug schließt, ohne Punkt 11 selbst zu verändern.

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

**1a. Zeitlich begrenztes Override-Ventil innerhalb der Vorrangklausel (ergänzt 27.07.2026, Fable-Review, nach Trade #25):** Die Vorrangklausel aus Punkt 1 gilt unverändert für die ersten 3 Kerzen — sie schützt echte kurze Konsolidierungen weiterhin vollständig. Ab der **4. aufeinanderfolgenden 5-Min-Kerze ohne neues Hoch/Tief** (Long, spiegelbildlich Tief bei Short), kombiniert mit RSI weiterhin **≥8-10 Punkte vom lokalen Extrem entfernt** (gleiche Schwelle wie Punkt 2, nur die Kerzenzahl auf 4 erhöht), greift der Teilgewinn-Vorschlag aus Punkt 2 **trotz technisch intaktem 9d1-Fortsetzungsmuster**. Das Fortsetzungsmuster verliert damit sein zeitlich unbegrenztes Veto, behält es aber vollständig für die ersten 3 Kerzen. Siehe [[trades/trading_2026-07-23]] für den auslösenden Fall.

**Why (1a):** Flags/Keile/Pennants haben in der klassischen TA-Lehre eine begrenzte "Lebensdauer" (Faustregel 5-20 Kerzen bis zur Auflösung) — ein technisch "noch nicht gebrochenes" Muster ist nach 4+ Kerzen ohne Auflösung kein verlässlicher Haltegrund mehr, das Fehlschlagsrisiko steigt mit der Dauer. Bei Trade #25 (23.07.2026) waren die Stall-Kriterien (3 Kerzen ohne neues Extrem + RSI-Abkühlung) während der Flag/Keil-Konsolidierung (17:35-17:54 Uhr) mehrfach zahlenmäßig fast erfüllt, wurden aber durch die bis dahin zeitlich unbegrenzte Vorrangklausel nie geprüft — kein Teilgewinn wurde realisiert, bevor der (glücklicherweise im Profit liegende) SL griff. Reine Kerzenzahl allein würde intakte, aber nur langsame Trends fälschlich abstrafen — deshalb Kopplung an dieselbe RSI-Schwelle wie das reguläre Stall-Kriterium, nicht ein reiner Zeitablauf.

2. Nur wenn kein solches Fortsetzungsmuster erkennbar ist (oder das Override aus 1a greift): Wenn **3 aufeinanderfolgende 5-Min-Kerzenschlüsse** kein neues Hoch (bei Long, spiegelbildlich Tief bei Short) über das bisherige Bewegungs-Hoch machen UND der RSI dabei **mindestens 8-10 Punkte von seinem lokalen Extrem** gefallen ist (fester Zahlenanker statt "spürbar") → aktiv einen **Teilgewinn (25-50% der Restposition, analog Punkt 8e)** vorschlagen, NICHT automatisch den Vollexit.
3. Rest-Position läuft mit den bestehenden Schutzsignalen (9d1, Punkt 11, SL) weiter — Punkt 12 ersetzt keinen Vollexit-Mechanismus, sondern ergänzt ihn um einen frühen Teilverkauf.

**Warum 3 Kerzen:** Lang genug, um normales kurzes Verschnaufen (mehrfach an diesem Tag beobachtet) nicht fälschlich als Stillstand zu werten, kurz genug (~15 Min), um bei echtem Bewegungsende nicht unnötig Gewinn verstreichen zu lassen.

**Warum Teilgewinn statt Vollexit:** Senkt die Kosten eines Fehlsignals — trifft die Regel eine normale Konsolidierung vor Fortsetzung (das reale Risiko laut Fable-Review), wird nur ein Teil zu früh realisiert, nicht die ganze Position.

**Timing-Klarstellung (ergänzt nach zweitem Fable-Review 21.07.2026):** Der Exit-/Teilgewinn-Vorschlag nach dieser Regel darf **frühestens BEIM tatsächlichen 3. Kerzenschluss selbst** ausgesprochen werden — niemals schon auf Basis der Zwischen-Tick-Zahlen (Punkt 3), auch wenn RSI/Kurs 1-2 Minuten vorher schon "reif" aussehen. Grund: Kurs/RSI werden jede Minute frisch abgerufen, während die Pflicht-Gegenprobe gegen die 9d1-Vorrangklausel (Fortsetzungsmuster-Check, braucht Screenshot) nur beim echten Kerzenschluss läuft. Ein vorgezogener Exit-Vorschlag würde diese Gegenprobe umgehen. Punkt 3b (kein Screenshot in der heißen Phase) ist hier NICHT als Freibrief zu lesen, die 9d1-Prüfung zu überspringen — eine offene Position mit nahendem Stall-Trigger bleibt trotzdem an den regulären Voll-Check-Rhythmus (Punkt 9: jeder 5-Min-Kerzenschluss) gebunden.

**Review-Pflicht (Bedingung für den Fortbestand dieser Regel, analog 7a1):** Nach den nächsten **5 Anwendungsfällen** explizit prüfen, ob die Regel tatsächlich einen Unterschied gemacht hat (Teilgewinn gesichert, der sonst verloren gegangen wäre, ohne dabei echte Fortsetzungen abzuschneiden). Zeigt sich kein klarer Nutzen oder überwiegen die abgeschnittenen Fortsetzungen, Regel wieder streichen statt als ungenutzte Karteileiche stehen zu lassen — Basis: n=1 bei dieser Überarbeitung, weiterhin nicht statistisch abgesichert.

**Terminierung (07.08.2026, Levi-Entscheidung nach Fable-Regelwerk-Audit, siehe [[project_regelwerk_audit_2026-08-07]]):** Die Fallzahl-Schwelle ist längst erreicht (mind. 5 relevante Trades seit 21.07.: #23/#25/#27/#28/#33), die Review wurde aber bewusst noch nicht durchgeführt. Levi terminiert sie explizit auf **nach Abschluss von Phase 3** (siehe [[project_risikomanagement]]), nicht sofort — gilt für diese Review-Pflicht UND für 12.3 (Review-Pflicht unten, Punkt "Nach den nächsten 3-5 Anwendungsfällen").

**Bewährt bei Trade #23 (erster, noch nicht nach neuer Fassung geprüfter Fall):** Nach dem Ausbruch über 29.076/29.144 (neues Hoch) gab es keine bekannten Widerstände mehr, TP1/TP2 lagen weit und ohne echtes Level. Die (damals noch als Vollexit gefasste) Stall-Regel griff nach der 3. Kerze ohne neues Hoch (kombiniert mit einsetzender Eskalations-News, also nicht rein durch die Stall-Logik selbst bestätigt) und ermöglichte einen Exit bei +0,81%.

**How to apply:** Bei jedem Check-in während einer offenen Position (zusätzlich zu Punkt 11) den Abstand zum letzten bestätigten Hoch/Tief mitzählen. Nach der 3. Kerze ohne neuen Extremwert, kombiniert mit erkennbarer RSI-Abkühlung, aktiv einen Teilgewinn vorschlagen — unabhängig davon, ob TP1/TP2 erreicht sind oder ein hartes Reversal-Kriterium (Punkt 11) bereits vorliegt. **Ergänzung 27.07.2026 (siehe 1a):** Blockiert die 9d1-Vorrangklausel den Vorschlag bei der 3. Kerze, das Mitzählen trotzdem fortsetzen — ab der 4. Kerze ohne neuen Extremwert (weiterhin mit RSI-Abkühlung) greift der Teilgewinn-Vorschlag auch dann, wenn 9d1 noch ein technisch intaktes Fortsetzungsmuster meldet.

**Deckelung — Rest bewusst weiterlaufen lassen, wenn der Trend intakt bleibt (ergänzt 23.07.2026, Fable-Review + Levi-Entscheidung):** Der Teilgewinn-Vorschlag nach Punkt 12 bleibt **auf 25-50% begrenzt, NIE Vollexit**, solange (a) der übergeordnete 1H/15min-Bias ([[feedback_chartanalyse]] Punkt 8) weiterhin klar in Positionsrichtung zeigt UND (b) keines der vier Punkt-11-Reversal-Kriterien erfüllt ist. Ein Vollexit vor TP2 ist unter diesen Bedingungen nur über Punkt 11 selbst (echtes Reversal, 2-3 von 4 Kriterien) oder ein hartes 9d1/4a-Muster+Kerze-Signal möglich, nicht über den Stall-Exit allein. **Why:** Fable-Audit 23.07.2026 stellte fest, dass die proaktiven Exit-Regeln (8e, 9d1/4a, 11, 12) einseitig Richtung "früher raus" ziehen, ohne dass irgendwo aktiv ausgesprochen wurde, wann der Rest bewusst weiterlaufen soll — das bremst das realisierte RR (siehe [[feedback_realisiertes_rr]]), obwohl Punkt 12 selbst schon immer nur "Teilgewinn, KEIN Vollexit" verlangte. Diese Ergänzung macht das nur explizit, ändert die bestehende Logik nicht, gilt zusätzlich zur bereits bestehenden Vorrangklausel (9d1-Fortsetzungsmuster schlägt Punkt 12).

**12.2 SL-Nachzug ist kein Ersatz für den Teilgewinn-Vorschlag (ergänzt 28.07.2026, Fable-Tagesabschluss-Review nach Trade #27):** Bei Trade #27 (28.07.2026) erfüllte der dritte Stall an der 27.900-27.918-Zone exakt den Punkt-12-Trigger (3+ Kerzenschlüsse ohne neues Hoch), ausgelöst wurde aber ausschließlich ein manueller SL-Nachzug (auf 27.842) — kein Teilgewinn-Vorschlag für die Restposition. Folge war eine ~20-minütige, im Trade-Log explizit als "extreme Anspannung" beschriebene Phase mit MACD-H-Pendeln um die Nulllinie (kein bestätigtes Punkt-11-Reversal), bis der SL schließlich auslöste. Kein Regelbruch — SL-Nachzug ist ein legitimes, an keiner Stelle verbotenes Werkzeug —, aber eine unvollständige Anwendung von Punkt 12, dessen eigentlicher Zweck (frühe Teilrealisierung, um genau diese Anspannungsphase zu verkürzen/vermeiden) dadurch nicht griff.

**How to apply:** Sobald der Punkt-12-Trigger (3+ Kerzenschlüsse ohne neues Extrem + RSI-Abstand) erfüllt ist, aktiv BEIDE Optionen gleichzeitig anbieten — Teilgewinn (25-50% der Restposition, wie in Punkt 12 vorgesehen) UND/ODER SL-Nachzug —, nicht nur eine davon automatisch wählen. Der User entscheidet dann bewusst, welche Kombination passt, statt dass der SL-Nachzug den Teilgewinn-Vorschlag stillschweigend ersetzt.

**Review-Pflicht:** Läuft wie die ursprüngliche Punkt-12-Fassung unter der bestehenden 5-Anwendungsfälle-Prüfung (siehe oben) mit — kein separater neuer Zähler, diese Präzisierung ist Teil derselben Regel.

**12.3 Verschärfte Schwelle: 2 Kerzenschlüsse + RSI ≥5 Punkte, inkl. Rekursions-Logik für Restpositionen (ergänzt 31.07.2026, nach Trade #28, User-Vorschlag + Fable-Review — ersetzt die Zahlen aus Punkt 2 oben ab jetzt, das Prinzip selbst bleibt unverändert)**

**Vorgeschichte / Weg dorthin (kurz, lehrreich für künftige Reviews):** Bei Trade #28 (31.07.2026, Short NAS100) lief nach dem Momentum-Extremum (16:15, RSI 24,3) eine ~19-minütige Gegenbewegung, die den alten Punkt-12-Teilverkauf (nach 3 Kerzen) erst bei 16:30 auslöste und danach die Restposition bis zum Breakeven-SL zurücklaufen ließ — 13,80€ Gesamtgewinn statt eines deutlich höheren, am Extremum verfügbaren Betrags. Zwei schnellere Alternativen wurden im Review davor geprüft und verworfen: (a) ein reiner "ab dem Momentum-Extremum an zählen"-Trigger ohne Kerzenschluss-Pflicht und ohne Magnitudenschwelle wurde verworfen, weil RSI wegen der bekannten Momentum-vor-Preis-Divergenz schon 2 Minuten nach dem Extremum keinen neuen Tiefstwert mehr machte, während der Kurs noch 4-5 Minuten weiterlief — ein solcher Trigger hätte nahezu bei jedem Extremum sofort ausgelöst, massives Fehlsignal-Risiko ohne echten Mehrwert; (b) eine eigenständige, beschleunigte Kopie von Punkt 12 nur für Restpositionen ("12b") wurde zugunsten der einfacheren, vom User selbst vorgeschlagenen Lösung verworfen: die bestehende, bereits bewährte Punkt-12-Logik (Kerzenschluss-Pflicht + RSI-Magnitudenschwelle als Rauschfilter) bleibt strukturell unverändert, nur beide Zahlen werden proportional halbiert.

**Neue Schwelle (ersetzt "3 Kerzen / 8-10 Punkte" aus Punkt 2 oben ab jetzt):**
- **2 aufeinanderfolgende 5-Min-Kerzenschlüsse** ohne neues Hoch/Tief (statt bisher 3)
- UND **RSI ≥5 Punkte vom lokalen Extrem entfernt** (statt bisher 8-10 — proportional zur halbierten Kerzenzahl)
- Beide Bedingungen bleiben UND-verknüpft, nicht oder — reine Kerzenzahl ohne Magnitude oder reine RSI-Bewegung ohne Kerzenschluss-Bestätigung reichen weiterhin NICHT (genau das war der Grund, warum Variante (a) oben verworfen wurde).
- **1a-Override sinkt proportional mit:** bisher ab der 4. Kerze, jetzt ab der **3. aufeinanderfolgenden Kerze** ohne neues Extrem (weiterhin mit RSI-Abstand ≥5 Punkten) verliert ein technisch intaktes 9d1-Fortsetzungsmuster sein Veto.

**Rekursion auf die Restposition (neu spezifiziert):**
- Das **ursprüngliche Extremum der Position** (Kurs-Hoch/-Tief, z.B. 27.952,55 bei Trade #28) bleibt über die GESAMTE Positionslaufzeit die feste Referenz für "neues Extrem" — kein Reset auf einen kleineren Zwischentiefpunkt pro Tranche. Ein tatsächlich neues, weiter reichendes Hoch/Tief verschiebt die Referenz wie bisher; ein Teilverkauf allein tut das NICHT.
- Der **Kerzen-Zähler UND die RSI-Referenz** resetten dagegen nach JEDEM ausgeführten Teilverkauf (egal ob nach dieser Regel oder diskretionär wie bei Trade #28): Der Kerzen-Zähler beginnt wieder bei 0, die RSI-Referenz ist ab jetzt der RSI-Stand zum Zeitpunkt des letzten Triggers (nicht mehr das Ur-Extremum) — sonst würde die Regel unmittelbar nach jedem Teilverkauf sofort erneut auslösen, weil die alten Referenzwerte schon erfüllt sind.
- Auf die Restposition wird danach exakt dieselbe Regel (2 Kerzen + RSI ≥5 Punkte von der neuen Referenz) erneut angewendet — kein separater, abweichender Mechanismus für spätere Tranchen.

**Scope-Klarstellung (wortwörtlich, weil das der eigentliche Root Cause bei Trade #28 war):** Diese Regel (Punkt 12 gesamt, inkl. dieser Verschärfung) gilt AUSSCHLIESSLICH für die Frage "ist unsere bestehende, offene Position in Gefahr, Gewinn sichern" — niemals für die Frage "sollen wir aktiv auf die Gegenrichtung drehen". Für Letzteres bleibt EXKLUSIV Punkt 11 mit der hohen 2-3-von-4-Schwelle zuständig, komplett unverändert durch diese Nachschärfung. Beide Fragen sind unterschiedliche Kategorien mit unterschiedlicher Kosten-Asymmetrie (siehe Punkt 11, Vorrang-Klärung) und dürfen nicht miteinander vermischt werden.

**Abgrenzung zu Punkt 7c, fünfte Erweiterung (Reconciliation):** Beide Regeln bleiben als unabhängige, parallele Trigger bestehen — wer zuerst auslöst, löst den Teilgewinn-/SL-Nachzug-Vorschlag aus, keine Duplikat-Regel:
- **Punkt 12 (hier):** Kerzenschluss-basiert (mind. 10 Min bis zum Trigger), erkennt "die Bewegung baut sich nicht mehr in unsere Richtung aus" — braucht KEINE aktive Gegenbewegung, reine Erschöpfung/Seitwärtslauf reicht.
- **Punkt 7c, fünfte Erweiterung:** 1-Minuten-Tick-basiert (kann schon nach 3-5 Min auslösen), erkennt speziell eine AKTIVE, anhaltende Gegenbewegung — greift auch dann, wenn gar keine Kerzenschluss-Stall-Phase vorausgeht, sondern der Kurs direkt nach dem Extremum scharf dreht.
- Beide führen zur identischen Handlung (Teilgewinn 25-50% und/oder SL-Nachzug, NIE Vollexit/Dreh) und unterliegen derselben Scope-Klarstellung oben.

**Why:** Löst das bei Trade #28 dokumentierte Problem (zu später Trigger durch den 15-Min-Kerzenschluss-Lag, plus keine Wiederholung für die Restposition), ohne die bewährten Rauschfilter der Regel (Kerzenschluss-Pflicht, RSI-Magnitudenschwelle) aufzugeben — beide Zahlen werden nur proportional halbiert, kein neues Prinzip.

**Review-Pflicht (analog Punkt 12 Basisregel):** Nach den nächsten **3-5 Anwendungsfällen** explizit prüfen, ob 2 Kerzen/RSI≥5 zu oft normale kurze Konsolidierungen fälschlich abschneidet (mehr Fehlsignale als die alte 3/8-10-Fassung) oder tatsächlich zuverlässig mehr Gewinn sichert. Bei Bedarf nachjustieren oder auf die alte Schwelle zurückfallen.

**12.4 Pflicht-Ausgabezeile "Stall-Check" bei jedem Check-in mit offener Position (ergänzt 06.08.2026, Fable-Tagesabschluss-Review nach Trade #33)**

**Vorfall:** Bei Trade #33 (06.08.2026, Long) war der Punkt-12.3-Trigger (2 Kerzenschlüsse ohne neues Extrem + RSI ≥5 Punkte vom lokalen Extrem) spätestens gegen 17:20-17:25 Uhr erfüllt, wurde aber erst um 17:34 Uhr auf Levis explizite Nachfrage ("Was war die Regel wenn wir nur seitwärts laufen?") überhaupt angesprochen — ein rein interner Check ohne sichtbaren Beleg im Output wurde unter der laufenden Quick-Tick-Kadenz schlicht übersprungen. In den ~10-15 Minuten dazwischen lief der Kurs vom Bewegungs-Hoch zurück Richtung Entry, genau das Fenster, in dem ein Teilgewinn/SL-Nachzug noch etwas gesichert hätte. Der Exit selbst kam nicht zu spät (Levi exitete selbständig bei QQQ-EMA50-Bruch), aber die Transparenz während der Position fehlte — dasselbe Rückfall-Muster wie bei den bereits bestehenden Pflichtzeilen (Tweet-Check, Kerze-geschlossen, Format), die genau aus diesem Grund eingeführt wurden.

**Regel:** Bei JEDEM Check-in (Quick-Tick UND Voll-Check) während einer offenen Position eine explizite Pflicht-Ausgabezeile ausgeben: **"Stall-Check: [X] Kerzen ohne neues Extrem, RSI [Y] Punkte vom Extrem entfernt → [nicht] getriggert"**. Referenz-Extrem (Bewegungs-Hoch/-Tief seit Entry bzw. seit letztem Teilverkauf, siehe Rekursions-Logik in 12.3) und aktueller RSI-Stand müssen bei jedem Tick mitgeführt werden, nicht nur beim Voll-Check.

**Why:** Ein interner Check ohne sichtbaren Beleg ist nicht überprüfbar und wird unter Zeitdruck/bei langer Quick-Tick-Kette übersprungen — eine Pflichtzeile im Output macht die Prüfung erzwingbar, exakt analog zum bewährten Muster bei Punkt 7d0 (Kerze geschlossen) und Punkt 9/[[feedback_vollcheck_format]] (Tweet-Check/Format). Fehlt diese Zeile bei einem Check mit offener Position, gilt der Check als nicht regelkonform durchgeführt.

**How to apply:** Sobald ein Trade offen ist, ab dem ersten Check-in danach die Stall-Check-Zeile in JEDES Signal-Format (auch das kompakte Quick-Tick-Format aus Punkt 2) aufnehmen — nicht nur im Voll-Check. Referenz-Extrem beim Entry auf den Entry-Kurs selbst setzen (noch kein Bewegungs-Extremum vorhanden), danach mit jedem neuen Hoch/Tief aktualisieren.

## 12.5 Review-Abschluss 12.08.2026 (Fable, Punkt 4 des Phase-1-3-Audits, siehe [[project_regelwerk_audit_2026-08-07]] — überfällige Review jetzt tatsächlich durchgeführt statt weiter aufgeschoben)

**Datenbasis:** Alle Tages-Dateien 21.07.-07.08.2026 auf Punkt-12/12.1a/12.2/12.3/12.4-relevante Fälle durchsucht (nicht nur die bereits im Audit genannten Trades #23/#33/#35). Gefundene Fälle: **n=7** (#23, #25, #27, #28, #32, #33, #35) — die eigene Schwelle (≥5 seit 21.07.) ist damit klar erreicht, die Review war überfällig.

**Fall für Fall:**
1. **#23 (21.07., Ursprungsfall):** Regel zum Zeitpunkt des Trades noch informell/nicht final (Vollexit statt Teilverkauf der Restposition — verstößt gegen die noch am selben Tag reformierte "nur Teilgewinn, nie Vollexit"-Fassung; zusätzlich mit Eskalations-News vermischt, "nicht rein durch die Stall-Logik selbst bestätigt" laut eigener damaliger Einordnung). Kein sauberer Test der finalen Regelform, sondern deren Auslöser.
2. **#25 (23.07.):** Trigger-Kriterien mehrfach knapp erfüllt, aber jedes Mal von der 9d1-Vorrangklausel überstimmt — kein Teilverkauf vor dem finalen SL. Deckte eine echte Design-Lücke auf (Vorrangklausel ohne zeitliche Grenze) und führte direkt zur 1a-Ergänzung (27.07., zeitlich begrenztes Override-Ventil). Ergebnis WIN, aber ohne dass Punkt 12 selbst je griff.
3. **#27 (28.07.):** Trigger klar erfüllt (3+ Kerzen ohne neues Hoch), aber nur ein manueller SL-Nachzug wurde ausgelöst, kein Teilgewinn-Vorschlag — unvollständige Anwendung. Führte zu 12.2 (beide Optionen aktiv gemeinsam anbieten). Finanziell glimpflich (Free-Roll-Prinzip via TP1), aber Prozess lückenhaft.
4. **#28 (31.07.):** Trigger korrekt nach der damaligen 3-Kerzen-Logik ausgelöst ("planmäßig"), aber mit ~19 Minuten Verzug zum eigentlichen Bewegungs-Extremum — spürbar suboptimaler Exit (+13,80€ statt eines am Extremum erreichbaren deutlich höheren Betrags). Führte zu 12.3 (2 Kerzen/RSI≥5 + Rekursions-Logik für Restpositionen).
5. **#32 (05.08.):** Erster vollständig sauberer Erfolgsfall unter der verschärften Fassung — Trigger (2 Kerzen + RSI-Abkühlung) rechtzeitig erkannt, Teilexit UND SL-Nachzug gemeinsam angeboten (12.2 und 12.3 beide korrekt angewendet). WIN +92,99€, kein Regelbruch.
6. **#33 (06.08.):** Trigger-Kriterien (12.3) bereits gegen 17:20-17:25 Uhr erfüllt, aber erst 17:34 Uhr auf explizite User-Nachfrage überhaupt angesprochen — ein interner Check ohne Output-Beleg wurde unter der laufenden Quick-Tick-Kadenz stillschweigend übersprungen. Führte zu 12.4 (Pflicht-Ausgabezeile "Stall-Check"). LOSS -30,69€ (durch 50%-ATR-Sizing gemildert, sonst ungefähr doppelt so hoch).
7. **#35 (07.08., einziger Fall NACH Einführung von 12.4):** Der tatsächliche Teilverkauf (18:02-18:03 Uhr) wird in der Tagesdatei explizit als **"Punkt-7c-Extension"** (4 aufeinanderfolgende Checks fallender MACD-H/RSI, tick-basierter Erschöpfungs-Trigger) geführt — **nicht** als Punkt-12/12.3-Auslösung (kerzenschluss-basiert). In der Tagesdatei gibt es keinen Beleg, dass in diesem Trade überhaupt ein echter Punkt-12-Trigger auftrat, und keinen Beleg, dass die Pflicht-Ausgabezeile "Stall-Check: ..." aus 12.4 bei den Check-ins tatsächlich ausgegeben wurde.

**Kernbefund:** Von 7 Fällen ist genau **einer (#32)** eine vollständig regelkonforme Anwendung der finalen (12.2+12.3-)Fassung. Drei Fälle (#25, #27, #28) deckten je eine eigene Lücke auf und führten zu einer gezielten Nachschärfung — so ist iteratives Regel-Tuning gedacht, zeigt aber auch, dass die Regel bis 31.07. durchgehend nachjustiert werden musste, nie über mehrere Fälle hinweg unverändert stabil lief. Der schwerste Fall (#33) deckte eine reine Transparenz-Lücke auf (Trigger erfüllt, aber nicht sichtbar gemacht) und führte zu 12.4 — **aber #35, der einzige Fall seit 12.4, testet diesen Fix nicht**, weil dort ein anderer, unabhängiger Mechanismus (Punkt 7c) den Teilverkauf auslöste, nicht Punkt 12 selbst.

**Verdikt: NICHT als vollständig validiert geschlossen — ein konkreter Rest-Gap bleibt offen.** Die 12.2+12.3-Fassung von Punkt 12 hat mit #32 einen sauberen Erfolgsbeleg und darf als grundsätzlich funktionsfähig gelten. Die 12.4-Pflichtzeile ist dagegen bislang **durch keinen einzigen Fall bestätigt** — #33 zeigte das Problem, das sie lösen soll, aber es existiert noch kein dokumentierter Fall, in dem 12.4 selbst (nach ihrer Einführung am 06.08.) bei einem echten, kerzenschluss-basierten Punkt-12-Trigger nachweislich gegriffen und die späte Erkennung verhindert hat. **Empfehlung:** Regel bleibt unverändert in Kraft, kein Hinweis auf grundsätzliche Fehlkalibrierung. Diese Review wird nicht als endgültig abgeschlossen geführt, sondern vertagt auf den nächsten Fall mit einem echten kerzenschluss-basierten Punkt-12-Trigger — dort explizit gegenchecken, ob die "Stall-Check: ..."-Zeile tatsächlich bei jedem Check-in im Output erscheint, bevor 12.4 als bestätigt gilt.

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

**13.1 Aktive Pflicht statt reiner Option, wenn die Bewegung hält (ergänzt 30.07.2026, nach dem 14:30-Post-PCE-Breakout):** Am 30.07.2026 lief ab ~15:00 eine Chasing-Situation nach obiger Definition (4-5+ klar gerichtete Kerzen ohne Konsolidierung, 1H/15min/QQQ-Dual-Gate alle sauber long) — es wurde nur "abwarten auf Pullback" kommuniziert, die 50%-Chasing-Option aus diesem Punkt wurde nicht aktiv angeboten. Die Bewegung lief in der Folge ungebremst weiter (RSI 5min bis ~90), ohne dass der erhoffte Rücksetzer kam — ein komplettes Auslassen erwies sich damit als spürbar schwächer als eine halbierte Teilnahme gewesen wäre.

**Why:** User-Feedback 30.07.2026: "wenn 15:30 alle Indikatoren eindeutig long sagen und es sich auch hält, dann greift die Regel 50% wegen Chasing. Aber ganz auslassen ist sehr schwach." Der User bestätigt damit den Grundmechanismus dieses Punktes, korrigiert aber die bisherige Praxis: die halbierte Position ist keine bloß erwähnte Option unter mehreren, sondern die aktiv einzunehmende Handlung, sobald die Chasing-Kriterien erfüllt UND durch das Halten der Bewegung (keine Kipp-Signale nach Punkt 11) bestätigt sind.

**How to apply (verschärft):** Sobald eine erkannte Chasing-Situation (siehe Kriterium oben) mit vollständig übereinstimmendem Dual-Gate (1H/15min/QQQ) UND ohne jedes Reversal-Anzeichen über mehrere Checks hält, aktiv die halbierte Position als Handlungsvorschlag formulieren ("Chasing-Situation, aber Trend hält — 50% Einstieg empfohlen"), nicht nur als abstrakte Regel im Hinterkopf behalten und stattdessen zum Abwarten raten. Reines "Pullback abwarten" ist nur noch angemessen, wenn die Chasing-Kriterien NICHT sauber erfüllt sind (z.B. Dual-Gate widersprüchlich) oder bereits ein Kipp-Signal vorliegt — nicht als Standardreaktion auf jede Chasing-Lage.

**Präzisierung 30.07.2026 (Fable-Tagesabschluss-Review, kritisch nachgeschärft):** "Hält" war in der obigen Fassung nicht vorwärts-entscheidbar — ob eine Bewegung "hält" weiß man erst im Nachhinein, im Moment selbst sieht ein Check, der später kippt, identisch aus wie einer, der weiterläuft. Feste, im Moment prüfbare Schwelle: **Chasing-Kriterien erfüllt UND über die nächsten 2 aufeinanderfolgende Voll-Checks (nicht Quick-Ticks) tritt kein einziges Punkt-11-Kriterium auf** → dann aktiv die halbierte Position vorschlagen. Vor Ablauf dieser 2 Checks bleibt "Chasing erkannt, beobachten, ob sich die Bewegung über 2 Voll-Checks hält" die korrekte Zwischenmeldung — kein Vollentry-Vorschlag auf Basis eines einzelnen Checks.

**Hindsight-Warnung (Fable-Review 30.07.2026):** Die Lehre aus dem 30.07.-Vorfall ("wir hätten reingehen sollen") ist eine **Prozess-Lehre** (die Option aktiv anbieten statt implizit wegzulassen) — **keine Ergebnis-Lehre**. Eine identische Ausgangslage (Chasing-Kriterien erfüllt, Dual-Gate sauber) hätte genauso gut schon nach dem 1. Voll-Check kippen können — dann wäre "wir hätten reingehen sollen" nie ausgesprochen worden. Diese Regel bewertet die Qualität der Entscheidung zum Zeitpunkt der Entscheidung (wurde die Option genannt?), nicht das tatsächliche Ergebnis der jeweiligen Bewegung.

**Review-Pflicht (ergänzt 30.07.2026):** Diese Regel (13.1) läuft mit Stichprobe n=1 (nur der 30.07.-Vorfall, dabei kein tatsächlicher Trade ausgeführt). Nach den nächsten 2-3 echten Anwendungsfällen (tatsächlich vorgeschlagene halbierte Chasing-Position) explizit prüfen, ob die 2-Check-Schwelle in der Praxis taugt (zu früh/zu spät) oder ob sie wieder gestrichen/angepasst werden muss — analog zur bestehenden Review-Pflicht-Praxis bei anderen frisch eingeführten Regeln (siehe Punkt 7a1, Punkt 12).
