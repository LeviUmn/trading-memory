---
name: project-fable-auftrag-kalendercheck-dst
description: "Fertiger, 1:1 an Fable weitergebbarer Auftrag für ZWEI der vier Zusatzlücken aus dem Opus-Gegencheck vom 14.09.2026: (A) Kalender-Fetch-Datumsprüfung gegen T0 in feedback_session_update.md Schritt 4 — Wiederholung des 03.08.2026-Musters ('Actual'-Wert für ein noch nicht veröffentlichtes Event = gecachte Seite) verhindern; (B) EU/US-Sommerzeit-Divergenz-Fenster in feedback_zeitzone.md — kanonische Merkliste der Fenster 2026-2028 (nächstes: Mo 26.10.–Fr 30.10.2026, US-Open dann 14:30 DE statt 15:30), Pflicht-Token in der T0-Kopfzeile, Verweise in 7e / Zeitfenster / Voll-Check-Format. Reine Regelwerksänderung, KEIN Skript. Der DAX-Wasserstand-Fund ist ausdrücklich NICHT Teil des Auftrags (Levi-Entscheidung, DAX ruht)."
metadata: 
  node_type: memory
  type: project
  status: "UMGESETZT 14.09.2026 durch Fable (Regeltext in feedback_session_update/feedback_zeitzone/feedback_live_trading 7e/feedback_trading_zeitfenster/feedback_vollcheck_format + MEMORY.md; kein Skript, keine DAX-Datei; T1-T7 bestanden), RE-VERIFIZIERT 15.09.2026 (zweiter Fable-Durchlauf). OPUS-GEGENCHECK 15.09.2026: FREIGEGEBEN MIT AUFLAGEN — 0 Blocker, 2 Nachbesserungen von Opus selbst gefixt (K1/K2 kumulativ + K1 allein = STALE; Rand-Sonntage der Fenster-Fußnote asymmetrisch korrigiert), Fenster-Tabelle unabhaengig nachgerechnet UND per Node-ICU nachgemessen (deckungsgleich), DAX-Perimeter eingehalten, MEMORY.md-Kuerzung sicher, Tests 90/90. Offen: vollcheck.cjs Z.402-404 (eigener Code-Auftrag, faellig vor 26.10.2026). Nicht committet, nicht gepusht — Befund in feedback_datum_verifikation_fehler_2026-09-14"
  modified: 2026-09-15T01:40:33.588Z
  originSessionId: a2b3dbfe-4ee4-466f-855e-65c519af6e1f
---

# Auftrag an Fable — Kalender-Datumscheck + Sommerzeit-Divergenz (Folgeauftrag zum Gegencheck 14.09.2026)

**Status (aktualisiert 14.09.2026 Abend):** **UMGESETZT durch Fable** — Teil A (Prompt-Pflichtangaben, K1/K2/K3, STALE-Konsequenz strenger Zweig, Pflichtzeile `Kalender-Datumscheck (T0): …`, Bindungstabellen-Halbsatz, DST-Ausnahme der 30.07.-Spiegelregel) in [[feedback_session_update]]; Teil B (kanonische Fenster-Tabelle 2026-2028 + Konsequenztabelle + Berechnungsregel als Doku + Node-Gegencheck + Verfallsregel) in [[feedback_zeitzone]], DST-Suffix an der T0-Kopfzeile in [[feedback_session_update]], Verweise in [[feedback_live_trading]] 7e, [[feedback_trading_zeitfenster]], [[feedback_vollcheck_format]]. Kein Skript, keine DAX-Datei berührt. **Opus-Gegencheck OFFEN**, nicht committet, nicht gepusht. Gemeldete offene Inkonsistenzen (außerhalb des Auftrags): `vollcheck.cjs` Z. 402-404 leitet Order-Sperre/Halbierungsfenster aus fester DE-Wanduhrzeit ab und kennt das Fenster nicht; DAX-Kalender-Gegencheck in `feedback_session_update_dax.md` (07.08.-Regel) hat keinen Datumscheck und keine DST-Klausel (nach Weisung nicht angefasst — DE-Reports wandern ohnehin nicht, nur die dortige Verweisung auf die NAS100-30.07.-Regel ist jetzt unvollständig). *(Ursprünglicher Stand: Auftragstext fertig, nichts umgesetzt; Opus hatte bewusst NICHTS am Regelwerk geändert.)*

**Re-Verifikation 15.09.2026, ca. 03:25 DE (Fable, zweiter Durchlauf desselben Auftrags):** Der Auftrag wurde mit dem Hinweis „letzte Nacht am Nutzungslimit gescheitert, noch NICHTS geändert" erneut gestellt — dieser Hinweis war falsch: Die Umsetzung vom 14.09. lag vollständig im ungepushten Arbeitsbaum. Statt sie doppelt zu bauen, wurde sie Punkt für Punkt gegen den Auftrag (A1-A5, B1-B6, Nicht-Ziele 1-8) abgeglichen und T1-T7 wurden erneut gefahren: alle bestanden (Node-Messung an 13 Stichtagen deckungsgleich mit der Tabelle, `trading_scripts.test.js` 90/90, Code-Repo sauber, keine DAX-Datei mit Teil-A/B-Inhalt — die drei „Divergenz"-Treffer in DAX-Dateien sind vorbestehende Intermarket-Divergenzen). Einzige inhaltliche Ergänzung des zweiten Durchlaufs: eine Präzisierung der Fußnote unter der Fenster-Tabelle in [[feedback_zeitzone]] (Wochenendtage zwischen den Umstellungssonntagen liegen ebenfalls im Fenster — relevant nur für Sonntagabend-Updates). Weiterhin: Opus-Gegencheck OFFEN, kein Commit, kein Push.

**Quelle:** [[feedback_datum_verifikation_fehler_2026-09-14]], Abschnitt „Opus-Gegencheck Schritt 0, 14.09.2026", Unterabschnitt „Die 4 gemeldeten Zusatzlücken — Einordnung für Levi (NICHT von Opus behoben)".

## Welche zwei der vier Funde — eindeutige Zuordnung (vor dem Lesen des Auftrags klären)

Im Gespräch hat Levi „Punkt 1 und Punkt 3" freigegeben und den DAX-Punkt ausdrücklich ausgeschlossen. Die Nummerierung wurde gegen den Originaltext geprüft — **beide Nummerierungen stimmen überein**, es gibt keine Verwechslungsgefahr, sie ist hier trotzdem ausgeschrieben:

| Nr. im Gegencheck | Fund | in diesem Auftrag |
|---|---|---|
| **1** | Kalender-Fetch prüft das Datum der abgerufenen Seite nicht (Wiederholung des 03.08.2026-Musters, „höchste Priorität der vier") | **JA — Teil A** |
| 2 | DAX-Wasserstand-Stempel (`feedback_session_update_dax.md` Z. 42, handgeschriebener UTC-Timestamp statt `x_fetch_stamp.cjs`) | **NEIN — ausdrücklich ausgeschlossen** |
| **3** | EU/US-Sommerzeit-Divergenz (~4 Wochen/Jahr, US-Open dann 14:30 DE statt 15:30, nächstes Fenster Ende Oktober 2026) | **JA — Teil B** |
| 4 | Veraltete `MEMORY.md`-Statuszeile | entfällt — mit dem Gegencheck bereits erledigt |

**Warum Nr. 2 raus ist:** Levi-Entscheidung vom 14.09.2026. Der DAX-Block ruht (Budget offen, siehe [[project_risikomanagement_dax]]); der Fund bleibt gültig und ist **vor** einer Reaktivierung des DAX-Blocks zu fixen, aber nicht jetzt. Fable fasst im Rahmen dieses Auftrags **keine einzige DAX-Datei an**.

---

--- AB HIER 1:1 ALS PROMPT AN FABLE ---

## Auftrag: Kalender-Datumscheck gegen T0 + Sommerzeit-Divergenz-Fenster

Du bist Fable im Trading-Projekt (`C:\Users\umnus\tradingview-mcp`, Memory unter `C:\Users\umnus\.claude\projects\C--Users-umnus-tradingview-mcp\memory\`). Sprache Deutsch, Levi wird geduzt.

Das hier ist eine **reine Regelwerksänderung an Memory-Markdown-Dateien**. **Baue KEIN neues Skript, keinen neuen CLI-Parameter, keinen Guard, keine Cron-Erweiterung.** Dieselbe Begründung wie beim Schritt-0-Auftrag: die maschinelle Absicherung existiert dort, wo sie hingehört (Statusdateien: `x_fetch_stamp.cjs`, `register_touch.cjs`, Frische-Guard in `gate_check.cjs`) — die hier zu schließende Lücke liegt bei den **erzählten** Aussagen im Briefing. Seit Schritt 0 existiert, ist jede der beiden Prüfungen eine Zeile. Wenn du fachlich anderer Meinung bist: schreib es in deine Rückmeldung, aber baue es nicht.

### Zwei Funde, mehr nicht — Abgrenzung zuerst lesen

Dieser Auftrag setzt **genau zwei** der vier im Opus-Gegencheck vom 14.09.2026 gemeldeten Zusatzlücken um:

- **Teil A — Kalender-Fetch-Datumsprüfung** (im Gegencheck als Fund 1 geführt, „höchste Priorität der vier")
- **Teil B — EU/US-Sommerzeit-Divergenz-Fenster** (im Gegencheck als Fund 3 geführt)

**AUSDRÜCKLICH NICHT Teil dieses Auftrags: der DAX-Wasserstand-Fund (Fund 2).** `feedback_session_update_dax.md` Z. 42 verlangt weiterhin, `x_last_fetch_dax.json` von Hand mit einem „aktuellen UTC-Timestamp" zu stempeln — also genau der Fehler, der auf der NAS100-Seite am 01.09.2026 den Wasserstand ~2 h in die Zukunft geschoben hat und dort seit 03.09.2026 durch `x_fetch_stamp.cjs` ersetzt ist. **Das bleibt stehen. Levi hat entschieden, dass der DAX-Block ruht.** Du änderst im Rahmen dieses Auftrags **keine** DAX-Datei — nicht `feedback_session_update_dax.md`, nicht `feedback_live_trading_dax.md`, nicht `project_dax_erweiterung.md`, nicht `project_risikomanagement_dax.md`, nicht `dax_beobachtung/*`, nicht `dax_trades/*`. Das ist ein hartes Abnahmekriterium (Test T1): `git status` muss beweisen, dass keine DAX-Datei berührt wurde. Fällt dir beim Arbeiten eine DAX-Stelle auf, die von Teil A oder B inhaltlich betroffen wäre (z.B. der eigene Kalender-Gegencheck in `feedback_session_update_dax.md` Z. 59), dann **benenne sie in deiner Rückmeldung als offene Inkonsistenz für Levi — und ändere sie nicht.**

### Zuerst lesen (vollständig, bevor du irgendetwas änderst)

1. `memory/feedback_datum_verifikation_fehler_2026-09-14.md` — komplett, insbesondere den Abschnitt „Opus-Gegencheck Schritt 0, 14.09.2026" am Ende (dort stehen beide Funde im Original)
2. `memory/feedback_session_update.md` — **Schritt 0 komplett** (T0/T6, Pflicht-Kopfzeile, Bindungstabelle, Ausweis-Pflicht) und **Schritt 4 komplett** (Kalender, inkl. der vier historischen Korrekturen vom 28.07., 30.07. und 03.08.2026)
3. `memory/feedback_zeitzone.md` — komplett, insbesondere die Zeitbasen-Konvention und den Abschnitt „TZ-Umgebungsvariable ist in dieser Sandbox WIRKUNGSLOS" (dort steht auch der Satz „ET = DE − 6 h, solange beide Sommerzeit haben" — genau der Satz, dem der Trigger für den Nicht-Fall fehlt)
4. `memory/feedback_live_trading.md` Punkt **7e** („QQQ-Session-Gate", Zonentabelle mit dem Zusatz „bei US-Winterzeit je +1h prüfen")
5. `memory/feedback_trading_zeitfenster.md` — die Zeitfenster-Tabelle und die 15:30-16:00-Halbierungsregel
6. `memory/feedback_vollcheck_format.md` — Abschnitt „Sperr-/Halbierungsfenster in der Format-Zeile"

---

# TEIL A — Kalender-Fetch-Datumsprüfung gegen T0

## Der Anlass (steht bereits im Regelwerk, hier nur verdichtet)

`feedback_session_update.md` Schritt 4, Absatz „Fehler 03.08.2026": Ein `tradingeconomics.com`-WebFetch zeigte für den **noch nicht veröffentlichten** ISM Manufacturing PMI (16:00 Uhr) bereits einen **„Actual"-Wert** an — technisch unmöglich vor Release, also der eindeutige Beleg für eine gecachte/veraltete Seite. Am selben Tag fehlten im selben Fetch die Non-Farm Payrolls für den Freitag komplett. Aufgefangen wurde beides nur durch einen **inhaltlichen** Cross-Check gegen die Tweet-Kalenderübersichten aus Schritt 2 plus Levis manuelle Nachlieferung.

**Die Lücke:** Ein Abgleich „steht auf der gefetchten Seite überhaupt das heutige Datum?" existiert bis heute nicht. Bis zum 14.09.2026 war er auch gar nicht formulierbar, weil es keine verbindliche Jetzt-Zeit gab, gegen die man hätte prüfen können. **Seit Schritt 0 existiert T0 — damit ist die Prüfung eine Zeile.** Genau deshalb jetzt.

## A1 — WebFetch-Prompt in Schritt 4 so erweitern, dass die Prüfung überhaupt möglich ist

Aktuell steht in Schritt 4:

> - `WebFetch` von `https://www.investing.com/economic-calendar/`
> - Prompt: Alle High und Medium Impact Events für heute, mit Uhrzeit, Konsens, Vorwert

Ein WebFetch-Ergebnis, das nur eine Eventliste zurückgibt, **enthält oft gar kein Datum** — dann ist jede Datumsprüfung mangels Datum nicht durchführbar und würde stillschweigend als „bestanden" durchgehen. Das ist der klassische Fall einer Prüfregel, die an der Datenlage scheitert statt am Befund.

**Ergänze deshalb den Prompt-Text verbindlich um zwei Pflichtangaben:** Der Fetch-Prompt muss zusätzlich verlangen: **(a)** das auf der Seite genannte Datum und den Wochentag der angezeigten Kalenderansicht (bzw. die Kalenderwoche bei einer Wochenübersicht), und **(b)** pro Event, ob bereits ein „Actual"/„Ist"-Wert ausgewiesen ist oder nicht. Formuliere das als festen Bestandteil des Prompts, nicht als Empfehlung. Gilt für beide Quellen (`tradingeconomics.com` ist laut Regelwerk die bevorzugte, `investing.com` die im Text noch genannte) — schreib die Anforderung quellenneutral.

**Wenn die Antwort trotz Nachfrage kein Datum enthält:** Das ist **nicht** „Prüfung bestanden", sondern **Prüfung nicht durchführbar** → dieselbe Behandlung wie ein STALE-Befund nach A3. Schreib das ausdrücklich hin; sonst frisst genau dieser Fall die ganze Regel auf.

## A2 — Neuer Pflichtblock in Schritt 4: „Kalender-Datumscheck gegen T0"

Füge in `feedback_session_update.md` **Schritt 4** einen neuen Block ein, **direkt nach** dem Absatz „Fehler 03.08.2026 — tradingeconomics.com-WebFetch zwei Zuverlässigkeitsprobleme an einem Tag" (er baut inhaltlich darauf auf und darf nicht davor stehen). Der Block hat drei Prüfkriterien, die **nach jedem** Kalender-Fetch abzuarbeiten sind — egal welche Quelle, egal ob Tages- oder Wochenansicht:

**K1 — Datumsabgleich gegen T0.** Nennt die Antwort ein Datum/einen Wochentag, das/der **nicht** zum T0-Datum passt (bei einer Tagesansicht: exakt der T0-Tag; bei einer Wochenübersicht: die Kalenderwoche, in der T0 liegt)? Dann ist die Seite verdächtig — typischerweise gecacht, möglicherweise auch eine falsche Zeitzone der Seitenansicht. **Der T0-Wert ist die Referenz, nie umgekehrt:** Ein Kalender-Fetch darf das Datum niemals „korrigieren" — Schritt 0 verbietet ausdrücklich, das Datum aus einem Kalender-Fetch abzuleiten.

**K2 — „Actual" vor Release (das exakte 03.08.2026-Muster).** Weist die Antwort für ein Event, dessen genannte Uhrzeit nach der T0-Uhrzeit liegt (= noch in der Zukunft), bereits einen „Actual"/„Ist"-Wert aus? Dann ist die Seite **beweisbar** veraltet — nicht „vielleicht", sondern logisch zwingend, weil ein Wert vor seiner Veröffentlichung nicht existieren kann. **Achtung, Zeitvergleich:** Die Uhrzeit des Events steht im Fetch oft in US-Zeit oder in einer unklaren Zone; rechne sie vor dem Vergleich in DE-Zeit um — und beachte dabei zwingend **Teil B** dieses Auftrags (im Divergenz-Fenster liegen die US-Releases eine Stunde früher in DE-Zeit). Reihenfolge deshalb: erst DST-Status nach Teil B klären, dann K2 rechnen.

**K3 — Vollständigkeit (bestehende Regel, nur verlinken, NICHT neu schreiben).** Fehlen erwartete Standard-Termine (NFP am 1. Freitag, Jobless Claims donnerstags, ADP monatlich)? Diese Prüfung existiert bereits aus der 03.08.2026-Korrektur — verweise darauf, statt sie zu duplizieren.

## A3 — Konsequenz bei Befund: STALE, und zwar in die strenge Richtung

Trifft K1, K2 oder A1-„kein Datum vorhanden" zu, gilt der Fetch als **STALE**. Schreib folgende Behandlung fest:

1. **Kein Blackout-/Sperrfrist-Urteil aus diesem Fetch ableiten** — weder „heute ist nichts" noch „das ist schon raus".
2. **Zweiter Versuch:** neu fetchen, bevorzugt über die jeweils andere Quelle (`tradingeconomics.com` ↔ `investing.com`).
3. **Cross-Check** gegen die Tweet-Kalenderübersichten aus Schritt 2 (DeItaone „THIS WEEK'S U.S. ECONOMIC CALENDAR", Kobeissi „Key Events This Week") — das ist die bereits bestehende 03.08.2026-Maßnahme, hier nur als zweite Stufe eingeordnet.
4. **Bleibt es unklar: Levi fragen** — ebenfalls bereits bestehende Regel.
5. **Bis zur Klärung gilt der strenge Zweig.** Das ist der Punkt, der bisher nirgends steht und der ausdrücklich hingeschrieben werden muss: Ein veralteter Kalender ist **in beide Richtungen** gefährlich — er kann ein Event verstecken (falsches „heute kein Blackout" → Entry direkt in die NFP hinein) **und** eine Veröffentlichung vortäuschen (falsches „ist schon raus" → Sperrfrist zu früh aufgehoben). Solange der Fetch STALE ist, werden blackout-relevante Events als **UNKLAR** behandelt und die **restriktivere** Auslegung gewählt (Sperrfrist gilt als möglicherweise aktiv, keine Entry-Freigabe auf Basis dieses Fetchs), nicht die bequemere. Ein STALE-Fetch ist keine Erlaubnis, sondern ein fehlender Beleg.

## A4 — Pflicht-Prüfzeile im Briefing

Ohne sichtbaren Ausweis ist eine Prüfung nicht überprüfbar (dieselbe Logik wie bei der T0-Kopfzeile und der wörtlichen `gate_check.cjs`-Zitierpflicht). Verankere **eine** Pflichtzeile in der Schritt-4-Ausgabe des Briefings, immer vorhanden, in beiden Ausprägungen:

```
Kalender-Datumscheck (T0): <Quelle> nennt <Datum bzw. KW>, T0 = <TT.MM.JJJJ> → passt ✓ | K2: kein Actual vor Releasezeit ✓
Kalender-Datumscheck (T0): ⚠️ STALE — <K1|K2|kein Datum>: <konkreter Befund> → <ergriffene Konsequenz nach A3>
```

Regeln dazu:
- Die Zeile trägt das Quellenkürzel **`(T0)`** (Ausweis-Pflicht aus Schritt 0; die Aussage rechnet gegen die Jetzt-Zeit, also kein `(Bar-ts)`).
- Die Zeile ist **immer** da — auch im Normalfall. Eine Zeile, die nur im seltenen Fehlerfall erscheint, erscheint erfahrungsgemäß nie (Beleg: `vollcheck.cjs` an einem Tag 0 von 18 Mal genutzt, [[project_opus_meilensteincheck_2026-09-10]]).
- **Fehlt die Zeile, gilt Schritt 4 als nicht vollständig** — dieselbe Behandlung wie ein fehlendes `level_register.json` in Schritt 6 oder eine fehlende `Tweet-Check:`-Zeile im Voll-Check.
- Konkrete Befunde statt Floskeln: „STALE" allein genügt nicht, der auslösende Wert gehört in die Zeile (welches Event, welche Uhrzeit, welches Datum stand auf der Seite).

## A5 — Zwei kleine Konsistenz-Nachzüge (nicht weglassen, aber klein halten)

- **Bindungstabelle in Schritt 0:** Die Zeile „Schritt 4 (Kalender)" nennt heute nur „welche Events heute noch ANSTEHEN vs. bereits veröffentlicht sind; jede Blackout-/Sperrfrist-Aussage". Ergänze dort den Datumscheck als weitere an T0 hängende Stelle — ein Halbsatz plus Verweis, keine Kopie des Blocks.
- **30.07.2026-Regel („Uhrzeiten gegen die eigene 14:30-CET-Referenzliste spiegeln"):** Diese Regel kollidiert mit Teil B und bekommt dort ihre Ausnahme (siehe **B5**). Bearbeite sie ausschließlich dort, nicht zweimal.

---

# TEIL B — EU/US-Sommerzeit-Divergenz-Fenster

## Das Problem

`feedback_zeitzone.md` sagt heute: „US-Zeit stattdessen aus DE-Zeit ableiten (**ET = DE − 6 h, solange beide Sommerzeit haben**)". Der Nebensatz ist korrekt — aber es gibt **keinen Trigger** für den Fall, dass sie es eben nicht beide haben. Genauso trägt die Zonentabelle in [[feedback_live_trading]] 7e nur den Hinweis „bei US-Winterzeit je +1h prüfen", ohne dass irgendwo steht, **wann** das ist und **wer** das prüft.

Die Umstellungstermine laufen auseinander:
- **EU:** Beginn letzter Sonntag im März, Ende letzter Sonntag im Oktober.
- **USA:** Beginn zweiter Sonntag im März, Ende erster Sonntag im November.

In den Lücken dazwischen beträgt der Versatz **ET = DE − 5 h statt − 6 h**, und die US-Session öffnet in DE-Zeit eine Stunde früher: **14:30 statt 15:30 DE**.

**Größenordnung (Präzisierung gegenüber dem Gegencheck):** Der Gegencheck nennt „≈3 Wochen März + 1 Woche Okt/Nov". Das **Herbstfenster ist konstruktionsbedingt immer exakt 7 Tage** (letzter So Okt → erster So Nov), das **Frühjahrsfenster schwankt dagegen zwischen 11 und 23 Tagen**, je nachdem, wo der zweite und der letzte Märzsonntag fallen (2026: 21 Tage, 2027: 14, 2028: 14). „Ungefähr drei Wochen im Frühjahr" war also die Beschreibung des Jahres 2026, keine allgemeine Regel. Genau diese Schwankung ist das Hauptargument für die Lösungsform unten.

## B1 — Lösungsform: **datierte Merkliste ist verbindlich, Berechnungsregel nur als Dokumentation** (entschieden, nicht zur Disposition)

Es gab zwei Kandidaten: (a) eine Berechnungsregel im Regeltext („zweiter Sonntag im März …"), (b) eine Merkliste mit konkreten Terminen. **Entschieden ist (b) als bindende Prüfgrundlage, (a) nur als dokumentierende Herleitung.** Begründung, die im Regeltext mitstehen soll:

1. **Eine Sonntagsberechnung im Kopf ist exakt die Fehlerklasse, gegen die Schritt 0 gebaut wurde.** Der Vorfall vom 14.09.2026 entstand dadurch, dass ein Wochentag aus dem Kopf gesetzt statt gemessen wurde. Eine Regel, die „welcher Sonntag ist der zweite im März" als täglichen Denkschritt einbaut, importiert dieselbe Fehlerquelle in die Prüfung, die sie verhindern soll. Ein **Datumsvergleich** („liegt T0 zwischen X und Y?") ist dagegen ein Vergleich, kein Rechenschritt.
2. **Die Länge des Frühjahrsfensters schwankt** (11–23 Tage, siehe oben). Eine Faustformel im Kopf führt zu einer falschen Fenstergrenze in den meisten Jahren.
3. **Eine Liste verfällt still** — das ist ihr einziger echter Nachteil, und der wird mit B3 (Verfallsregel) in einen sichtbaren Abbruch überführt statt in ein stilles Falschergebnis.
4. Zusätzlich gibt es einen **Messweg statt Rechenweg** (B4): Node bringt seine eigene Zonendatenbank mit und kann den tatsächlichen ET-Offset ausgeben. Das passt zum Projektprinzip „gemessen statt geschätzt" und ist bereits verifiziert (siehe B4).

## B2 — Kanonische Tabelle in `feedback_zeitzone.md` (genau EINE Stelle im ganzen Regelwerk)

Neuer Abschnitt in `feedback_zeitzone.md`, direkt nach dem TZ-Abschnitt. Inhalt:

**(1) Die Fenster-Tabelle.** Die folgenden Werte sind am 14.09.2026 per Node-ICU gemessen (nicht gerechnet) und können 1:1 übernommen werden — die Handelstage sind die Werktage zwischen den beiden Umstellungssonntagen:

| Fenster | EU stellt um | USA stellt um | betroffene Handelstage | Versatz im Fenster |
|---|---|---|---|---|
| **Herbst 2026 (nächstes)** | So **25.10.2026** (CEST→CET) | So **01.11.2026** (EDT→EST) | **Mo 26.10. – Fr 30.10.2026** (5 HT) | ET = DE − 5 h |
| Frühjahr 2027 | So 28.03.2027 (CET→CEST) | So 14.03.2027 (EST→EDT) | Mo 15.03. – Fr 26.03.2027 (10 HT) | ET = DE − 5 h |
| Herbst 2027 | So 31.10.2027 | So 07.11.2027 | Mo 01.11. – Fr 05.11.2027 (5 HT) | ET = DE − 5 h |
| Frühjahr 2028 | So 26.03.2028 | So 12.03.2028 | Mo 13.03. – Fr 24.03.2028 (10 HT) | ET = DE − 5 h |
| Herbst 2028 | So 29.10.2028 | So 05.11.2028 | Mo 30.10. – Fr 03.11.2028 (5 HT) | ET = DE − 5 h |

*(Das Frühjahrsfenster 2026 — Mo 09.03. bis Fr 27.03.2026, 15 Handelstage — ist vorbei und gehört nicht in die Tabelle; es erklärt nur, woher die Angabe „≈3 Wochen" im Gegencheck stammt. Optional als Fußnote.)*

**(2) Die Konsequenztabelle.** Im Fenster liegt **alles, was an einem US-Zeitpunkt hängt, eine Stunde früher in DE-Zeit**; alles, was an einem deutschen/europäischen Zeitpunkt hängt (XETRA-Open, ZEW, Ifo, EZB), verschiebt sich **nicht**. Konkret:

| Anker | normal (DE) | im Divergenz-Fenster (DE) |
|---|---|---|
| US-Open 9:30 ET | 15:30 | **14:30** |
| US-Close 16:00 ET | 22:00 | **21:00** |
| 8:30-ET-Releases (CPI, Core CPI, PCE, PPI, NFP, Jobless Claims, GDP) | 14:30 | **13:30** |
| ADP 8:15 ET | 14:15 | **13:15** |
| 10:00-ET-Releases (ISM, JOLTS, CB Consumer Confidence) | 16:00 | **15:00** |
| FOMC-Entscheid 14:00 ET / Pressekonferenz 14:30 ET | 20:00 / 20:30 | **19:00 / 19:30** |
| Order-Sperre 15:00–15:30 ([[feedback_vollcheck_format]]) | 15:00–15:30 | **14:00–14:30** |
| Halbierungsfenster erste 30 Min nach Open ([[feedback_trading_zeitfenster]]) | 15:30–16:00 | **14:30–15:00** |
| Kern-Entry-Zone (Open+30 Min bis Open+2,5 h) | 16:00–18:00 | **15:00–17:00** |
| Zweites Fenster (vor US-Close) | 20:00–22:00 | **19:00–21:00** |
| 7e-Zone „reguläre US-Session" | 15:30–22:00 | **14:30–21:00** |

**Hinweis für dich (bitte im Text als solcher kennzeichnen):** Dass die **sessionverankerten** Fenster mitwandern (Halbierungsfenster, Kern-Entry-Zone, Order-Sperre), ist eine **Auslegung** — sie folgt daraus, dass diese Fenster über ihren Abstand zum US-Open definiert und begründet sind (Open-Volatilität, Volumen-Peak am Open, siehe [[feedback_trading_zeitfenster]] und der RVOL-Absatz in [[feedback_live_trading]] Punkt 11), nicht über eine Wanduhrzeit. Schreib sie so hin **und markiere sie ausdrücklich als Auslegung**, die Levi im Gegencheck kippen kann. Eine inhaltliche Änderung der Regeln selbst ist das nicht: Die Halbierung bleibt Halbierung, die Sperre bleibt Sperre — nur ihre DE-Uhrzeit verschiebt sich in diesen fünf bzw. zehn Tagen.

**(3) Die Berechnungsregel als Dokumentation** (EU: letzter So März / letzter So Okt; USA: zweiter So März / erster So Nov) — mit dem ausdrücklichen Zusatz, dass sie der **Herleitung und Verlängerung** der Tabelle dient und **nicht** als täglicher Kopfrechenschritt benutzt wird.

**(4) Ein Satz zur Abgrenzung:** Die Zeitbasen-Konvention bleibt unberührt — Statusdateien weiter UTC mit `Z`-Suffix, Ausgaben weiter DE-Ortszeit. Die Divergenz betrifft ausschließlich die **Umrechnung DE ↔ ET** und die daraus abgeleiteten Session-/Release-Uhrzeiten.

**Nur diese eine Stelle trägt die Tabelle.** Alle anderen Dateien bekommen ausschließlich **Verweise** (Prinzip „per Verweis statt per Kopie" aus dem Schritt-0-Auftrag A6.3) — zwei Kopien einer Datumstabelle driften garantiert auseinander. Das wird in Test T7 gegengeprüft.

## B3 — Verfallsregel (der Preis der Listenlösung, ausdrücklich mitgeliefert)

Schreib in denselben Abschnitt: Die Tabelle deckt bis einschließlich **Herbst 2028** ab. Liegt T0 **nach dem letzten dort erfassten Termin** und wurde die Tabelle nicht verlängert, gilt der Fenster-Status als **UNBEKANNT** — dann ist der Node-Gegencheck aus B4 **Pflicht** (statt optional) und das Ergebnis wird als neue Tabellenzeile nachgetragen. „Unbekannt" wird nie stillschweigend als „kein Fenster" behandelt. Damit wird der einzige echte Nachteil der Merkliste — das stille Verfallen — in einen sichtbaren, handhabbaren Fall überführt.

## B4 — Node-Gegencheck (optional im Normalfall, Pflicht bei UNBEKANNT)

Der Offset ist **messbar**, nicht nur berechenbar — Node bringt seine eigene Zonendatenbank (ICU) mit, auch in dieser Sandbox (derselbe Grund, aus dem der Node-Fallback in Schritt 0 funktioniert, während `TZ=` still auf UTC fällt). Nimm diesen Einzeiler mit in den Abschnitt:

```
node -e "const d=new Date();const f=(tz)=>new Intl.DateTimeFormat('de-DE',{timeZone:tz,timeZoneName:'shortOffset',hour:'2-digit',minute:'2-digit',hour12:false}).format(d);console.log('DE',f('Europe/Berlin'),'| ET',f('America/New_York'))"
```

**Verifikation durch Opus am 14.09.2026 (bereits gelaufen, Ergebnis zitierfähig):** Derselbe Aufruf über feste Stichtage geprüft — `2026-10-28` → `DE … GMT+1 | ET … GMT-4` (Differenz **5 h**, Fenster bestätigt), `2026-11-01` → `ET … GMT-5` (Differenz wieder 6 h, Fenster beendet), `2026-03-08` → `DE GMT+1 | ET GMT-4` (Frühjahrsfenster 2026 offen), `2026-03-29` → `DE GMT+2` (beendet). Die Tabelle in B2 ist damit gemessen, nicht geschätzt.

Regel dazu: Im Normalfall genügt der Blick in die Tabelle (ein Datumsvergleich). Der Node-Aufruf ist der Gegencheck bei Zweifel, an den Fenster-Rändern (Umstellungswochenende) und **verpflichtend** im Fall B3. **Kein zusätzlicher Pflicht-Tool-Call im Normalbetrieb** — Schritt 0 bleibt bei genau einem Zeit-Call.

## B5 — Wo die Prüfung ausgelöst wird: ein Token in der bestehenden T0-Kopfzeile

**Keine neue Pflichtzeile.** Eine neue Zeile wäre eine neue Sache, die vergessen werden kann; stattdessen hängt sich die Prüfung an die bereits durchgesetzte T0-Kopfzeile in Schritt 0 an (fehlt sie, gilt das Session-Update ohnehin als nicht durchgeführt — die Durchsetzung wird also geerbt). Erweitere das Format um ein **immer vorhandenes** Suffix:

```
Zeitanker T0: <…wie bisher…> — Quelle: <…>, Schritt 0 | DST: normal (ET = DE − 6 h)
Zeitanker T0: <…wie bisher…> — Quelle: <…>, Schritt 0 | DST: ⚠️ DIVERGENZ-FENSTER 26.10.–30.10.2026 aktiv — ET = DE − 5 h, US-Open 14:30 DE, Halbierungsfenster 14:30–15:00 DE
```

- Das Suffix ist **immer** da, auch im Normalfall („normal") — aus demselben Grund wie bei A4: ein Token, das nur im Ausnahmefall erscheint, erscheint nie.
- Ermittelt wird es durch einen **Datumsvergleich von T0 gegen die Tabelle in [[feedback_zeitzone]]**, nicht durch Kopfrechnen.
- Im Divergenzfall gilt zusätzlich: Die betroffenen Uhrzeiten werden für diesen Tag im Briefing **explizit umgerechnet genannt** (US-Session, Release-Zeiten der heutigen Events, Halbierungs-/Sperrfenster), nicht nur pauschal „eine Stunde früher".

**Ergänzung der 30.07.2026-Regel in Schritt 4 (Verbindung zu Teil A, wichtig):** Dort steht heute, dass abweichende Fetch-Zeiten für die Standard-US-Reports gegen die eigene 14:30-CET-Referenzliste zu spiegeln sind und eine Abweichung als „Fetch-Artefakt" gilt, „außer der User bestätigt explizit eine Abweichung (z.B. Sommerzeit-Wechsel-Tag)". Im Divergenz-Fenster ist **13:30 DE die richtige Zeit** — die bestehende Regel würde also den korrekten Fetch als Artefakt verwerfen und dafür auch noch Levi behelligen. Ergänze deshalb: Liegt T0 in einem Divergenz-Fenster nach [[feedback_zeitzone]], verschiebt sich die Referenzliste selbst um eine Stunde nach vorn (14:30 → 13:30 DE usw.); die Rückfrage an Levi entfällt in diesem Fall, weil die Abweichung erklärt und belegt ist. **Reihenfolge festhalten: DST-Status (B5) steht VOR dem Kalender-Datumscheck (A2/K2)** — sonst produziert K2 im Fenster falsche STALE-Alarme.

## B6 — Verweise an den drei betroffenen Stellen (je ein bis zwei Sätze, keine Kopien)

- **[[feedback_live_trading]] Punkt 7e:** Der vorhandene Klammerzusatz „(MESZ; bei US-Winterzeit je +1h prüfen)" ist heute richtig, aber folgenlos. Ergänze einen Satz: In den Divergenz-Fenstern nach [[feedback_zeitzone]] liegen alle drei Zonen eine Stunde früher (reguläre Session 14:30–21:00 DE). **Die Delta-Messung aus 7e bleibt die verbindliche Prüfung, die Uhrzeit-Zonen bleiben reine Planungshilfe** — daran ändert das Fenster nichts, und genau deshalb ist der Fund hier auch kein Sicherheitsloch, sondern ein Planungs-/Kommunikationsfehler. Schreib das mit hin, damit die Ergänzung nicht als neue Gate-Logik missverstanden wird.
- **[[feedback_trading_zeitfenster]]:** Ein Satz unter der Zeitfenster-Tabelle: Alle Uhrzeiten dort gelten für den Normalfall (beide Seiten dieselbe Sommerzeitlage); in den Fenstern nach [[feedback_zeitzone]] verschiebt sich die gesamte Tabelle um eine Stunde nach vorn, weil sie am US-Open verankert ist. Zusätzlich die Zeile „US-Börsenöffnung = 15:30 Uhr deutscher Sommerzeit" im Abschnitt „Wichtig" um den Verweis ergänzen — sie ist heute unbedingt formuliert und damit an ~4 Wochen im Jahr schlicht falsch.
- **[[feedback_vollcheck_format]]:** Ein Satz beim Abschnitt „Sperr-/Halbierungsfenster in der Format-Zeile": Im Divergenz-Fenster tragen die Marker die verschobenen Zeiten **und** den Grund, z.B. `| Halbierungsfenster 14:30–15:00 aktiv (DST-Divergenz)`. Mehr nicht — die Format-Zeile ist bereits dicht.

---

## Tests / Abnahme (alle sieben ausführen, Ergebnis in der Rückmeldung berichten)

**T1 — Datei-Perimeter per `git status`/`git diff`.** Geändert sein dürfen ausschließlich: `feedback_session_update.md`, `feedback_zeitzone.md`, `feedback_live_trading.md`, `feedback_trading_zeitfenster.md`, `feedback_vollcheck_format.md`, `MEMORY.md` — plus diese Auftragsdatei (Status). **Keine einzige DAX-Datei**, kein Code im Repo `tradingview-mcp` (`git status` dort muss sauber bleiben). Berichte die Liste wörtlich.

**T2 — Trockenlauf Teil A gegen den echten 03.08.2026-Fall.** Rekonstruiere aus dem im Regelwerk dokumentierten Vorfall (ISM PMI 16:00 Uhr mit bereits ausgewiesenem „Actual", Fetch-Zeitpunkt vormittags) und schreibe hin: **(a)** welches Kriterium gegriffen hätte (erwartet: K2), **(b)** wie die Pflichtzeile nach A4 an diesem Tag ausgesehen hätte — vollständig ausformuliert, mit konkretem Befund, **(c)** welche Konsequenz nach A3 daraus gefolgt wäre. Wenn dein Trockenlauf zeigt, dass die Regel den Fall **nicht** fängt, ist die Regel falsch formuliert — dann nachbessern, nicht schönschreiben.

**T3 — Trockenlauf Teil B an drei Stichtagen.** Schreibe für jeden das DST-Suffix der T0-Kopfzeile nach B5 aus. Erwartung: `14.09.2026` → `DST: normal`; `28.10.2026` → `DIVERGENZ-FENSTER 26.10.–30.10.2026 aktiv, US-Open 14:30 DE`; `10.11.2026` → `DST: normal`. Zusätzlich für den 28.10.2026: Nenne die Release-Zeit eines 8:30-ET-Events in DE-Zeit (erwartet 13:30) und das Halbierungsfenster (erwartet 14:30–15:00).

**T4 — Node-Gegencheck live ausführen.** Setze den Einzeiler aus B4 für mindestens zwei Stichtage ab (einer im Fenster, einer außerhalb) und **zitiere die Rohausgabe** in der Rückmeldung. Sie muss die Tabelle in B2 bestätigen. Weicht sie ab: **nicht** die Tabelle anpassen, sondern melden — dann stimmt eine Annahme nicht und Levi entscheidet.

**T5 — Wikilinks.** Jedes neu gesetzte `[[…]]`-Ziel existiert als Datei im Memory-Ordner. Liste sie auf.

**T6 — Tests unverändert.** `node --test tests/trading_scripts.test.js` → erwartet **90/90 grün** (es wird kein Code angefasst, die Zahl darf sich nicht ändern). Der bekannte Fehlschlag in `tests/sanitization.test.js` (`source audit`, Windows-Pfadfehler `C:\C:\…`, vorbestehend seit Commit `5f6648d` vom 12.08.2026) ist **nicht** von dir zu fixen — nur erwähnen, falls er auftaucht.

**T7 — Keine Doppelpflege.** `grep -rn "Divergenz-Fenster\|DIVERGENZ" memory/*.md` — die **Fenster-Tabelle** darf genau einmal vorkommen (in `feedback_zeitzone.md`); überall sonst dürfen nur Verweise und das Kopfzeilen-Token stehen. Berichte die Fundstellen.

## Nicht-Ziele (ausdrücklich, jedes einzeln einhalten)

1. **Kein neues Skript, kein CLI-Parameter, kein Guard, kein Cron-Eintrag.** Beide Teile sind Regeltext.
2. **Keine DAX-Datei anfassen** (Fund 2 ist ausgeschlossen, siehe oben) — weder der Wasserstand-Stempel noch der DAX-Kalender-Gegencheck.
3. **Keine inhaltliche Änderung der Handelsregeln.** Halbierungsfenster bleibt Halbierungsfenster, Order-Sperre bleibt Order-Sperre, Kern-Entry-Zone bleibt zwei Stunden — es verschiebt sich ausschließlich deren DE-Uhrzeit innerhalb der Fenster.
4. **7e bleibt die verbindliche Messung.** Die Zonentabelle dort bleibt Planungshilfe; du baust kein uhrzeitbasiertes Gate und keine neue Gate-Logik.
5. **Schritt 0 wird nicht umgebaut.** Erlaubt sind genau zwei Eingriffe: das DST-Suffix an der Kopfzeile (B5) und der Halbsatz in der Bindungstabelle (A5). Format, Wörtlichkeitsregel, Quellen-Feld, Node-Fallback und T0/T6-Systematik bleiben unangetastet.
6. **Keine neue Kalenderquelle, keine API-Anbindung** (TradingEconomics-API ist eine eigene, unentschiedene Frage, siehe [[project_tradingeconomics_api_idee]]).
7. **Keine Normalisierung** der T0-Kopfzeile (die Wörtlichkeit ist bewusst, siehe die Begründung im Regeltext).
8. **Keine Tabellen-Kopien.** Genau eine kanonische Stelle, sonst Verweise (T7).

## Abschluss

**KEIN Commit, KEIN Push.** Wie bei jedem heutigen Auftrag committet Levi bzw. Sonnet erst **nach** dem Opus-Gegencheck. Lass den Arbeitsbaum stehen, wie er ist.

**Rückmeldung an Levi/Opus, kompakt:** Was du geändert hast (Datei + Stelle), die Ergebnisse von T1–T7 (inkl. der zitierten Node-Rohausgabe aus T4 und des ausformulierten Trockenlaufs aus T2/T3), jede Stelle, an der du bewusst von diesem Auftrag abgewichen bist (mit Begründung), und alle Inkonsistenzen, die dir aufgefallen sind, aber außerhalb des Auftrags lagen — **insbesondere DAX-Stellen, die du gesehen, aber nach Weisung nicht angefasst hast.**

--- ENDE DES PROMPTS AN FABLE ---

---

## Nachtrag für Levi (nicht Teil des Fable-Prompts)

- **Beide Teile sind bewusst zusammen in einem Auftrag**, weil sie sich an einer Stelle berühren: Der Kalender-Datumscheck (K2, „Actual vor Releasezeit") rechnet mit US-Release-Zeiten — und die sind in den Divergenz-Fenstern eine Stunde anders. Getrennt vergeben würden sich die beiden Regeln an ~4 Wochen im Jahr gegenseitig falsche Alarme erzeugen.
- **Der DST-Fund ist terminiert, nicht dringend:** Das nächste Fenster beginnt am **Montag, 26.10.2026** — bis dahin sind es rund sechs Wochen. Danach kommt erst wieder Mitte März 2027.
- **Ehrliche Einordnung des DST-Funds:** Er ist kein Sicherheitsloch im Gate — 7e misst das Bar-Delta und fängt einen falsch angenommenen Session-Start ohnehin ab. Der Schaden liegt in der Planung und der Kommunikation: ein Briefing, das „US-Open 15:30" schreibt, obwohl der Markt um 14:30 öffnet, verliert die erste Handelsstunde und setzt Halbierungs-/Sperrfenster auf die falsche Uhrzeit.
