---
name: feedback-datum-verifikation-fehler
description: "14.09.2026: Session-Update-Briefing beruhte auf einer ungeprüften Annahme 'heute ist Sonntag' (real Montag) — Wochentag/Marktstatus wurden aus dem Gedächtnis gesetzt statt per date-Tool verifiziert. Fundamentaler Kontextfehler, jede Folgeaussage hing daran. Enthält TZ-Sandbox-Befund und Formulierungsvorschlag für einen Schritt 0."
metadata: 
  node_type: memory
  type: feedback
  status: "Schritt-0-Kette ABGESCHLOSSEN 14.09.2026 (FREIGEGEBEN MIT AUFLAGEN, 0 Blocker, 3 Opus-Nachbesserungen). Folgekette Kalender-Datumscheck + DST am 15.09.2026 gegengecheckt: FREIGEGEBEN MIT AUFLAGEN — 0 Blocker, 2 Nachbesserungen von Opus selbst gefixt (K1/K2 kumulativ + K1-allein-STALE in feedback_session_update; Rand-Sonntage der DST-Fenster in feedback_zeitzone), MEMORY.md-Kuerzung geprueft und sicher, kein Regelverstoss (Konvention existiert nicht) aber 2. Scope-Creep. Offen: vollcheck.cjs Z.402-404 (Code-Auftrag vor 26.10.2026), DAX-Wasserstand, Prozessregel Selbst-Komprimieren. Tests 90/90. Nicht committet, nicht gepusht."
  modified: 2026-09-15T10:00:00.000Z
  originSessionId: a2b3dbfe-4ee4-466f-855e-65c519af6e1f
---

## Was geschah (14.09.2026, ca. 22:46 DE)

Beim Trigger "start update dich" nahm der Live-Loop-Agent an, der aktuelle Tag sei **Sonntag** — ohne einen einzigen `date`-Aufruf, rein aus Gedächtnis/Kopfrechnung. Real war **Montag, 14.09.2026, 22:46 DE-Zeit**. Aus der falschen Tagesannahme folgte eine Kette von Falschaussagen im Briefing:

- NAS100 als "Wochenend-Vorbörse" statt als echten (fast abgelaufenen) Handelstag bezeichnet
- QQQ als "Freitags-Kassaschluss" statt als aktuellen, dünnen Nachbörsen-Stand
- Session-Extrema als "rollierendes Wochenend-Fenster" statt als echten heutigen Sessionverlauf

Levi hat den Fehler bemerkt und korrigiert. Die betroffenen Level-Register-Einträge wurden danach per Skript mit korrekter Begründung überschrieben.

## Root Cause

**Kein Einzelfehler in der Rechnung, sondern eine Reihenfolge-Lücke.** Das Regelwerk hat die richtige Prüfmethode bereits — sie greift nur zu spät und für die falsche Frage:

1. **[[feedback_live_trading]] Punkt 7e** enthält exakt den passenden Mechanismus, sogar mit passender Überschrift: *"Praktische Erkennung im Loop — Zeitstempel-Vergleich, nicht Uhrzeit-Raten"* (neuester QQQ-Bar gegen neuesten NAS100-Bar, Differenz ≤ 1 Kerze → Gate gültig). Genau das wurde nicht getan; es wurde geraten. **7e beantwortet aber auch nur "ist das QQQ-Gate offen", nicht "welcher Tag ist heute"** — und es sitzt in Schritt 6, also NACH Schritt 1-5, wo die Wochenend-Erzählung längst stand.
2. **[[feedback_session_update]]** verlangt an keiner Stelle vor Schritt 1, Datum/Wochentag/Marktstatus frisch festzustellen. Der Ablauf startet mit "Memory lesen" — Memory ist per Definition Vergangenheit und enthält keinen Beleg für "heute".
3. **[[feedback_zeitzone]]** regelt ausschließlich die **Uhrzeit-Darstellung** (DE statt UTC). Wochentag, Datum und Handelstag-Status kommen dort nicht vor.
4. **[[feedback_memory_pflege]], Zitierpflicht:** listet Preislevel, Kerzen-Timestamps/OHLC, Kerzenzählungen und Superlative ("Tagestief") als belegpflichtig — **Datums-, Wochentags- und Marktstatus-Behauptungen sind nicht genannt.** Genau dieser Fehlertyp fiel durch das Raster.

**Zweiter, unabhängiger Fehler obendrauf (Plausibilität):** Die Behauptung "Markt seit Freitag zu" stand im direkten Widerspruch zu den im selben Durchlauf frisch abgerufenen, sich bewegenden Kursdaten. Der Widerspruch wurde nicht bemerkt. Der billigste Gegencheck ist der aus 7e: **Zeitstempel des neuesten Bars gegen die aktuelle Zeit** — Delta klein = Markt läuft, unabhängig davon, für welchen Wochentag man sich hält. Das ist ein Tool-Fakt statt einer Annahme und hätte beide Fehler zugleich gefangen.

## TZ-Testergebnis (Opus, live in dieser Sandbox verifiziert)

| Befehl | Ergebnis | Bewertung |
|---|---|---|
| `date "+%A, %d.%m.%Y %H:%M (%z)"` | `Monday, 14.09.2026 22:50 (+0200)` | **KORREKT** |
| `date -u` | `20:50 UTC` | korrekt |
| `TZ="Europe/Berlin" date` | `20:50 GMT (+0000)` | **FALSCH, 2 h zu früh** |
| `TZ="America/New_York" date` | `20:50 +0000` | **FALSCH, 6 h daneben** |
| `ls /usr/share/zoneinfo` | fehlt | keine Zonendatenbank |

**Fazit:** Der in [[feedback_zeitzone]] empfohlene bare `date` ist korrekt und bleibt der Standard. Ein gesetztes `TZ=` macht ihn dagegen kaputt und fällt **still** auf UTC zurück — kein Fehler, nur ein plausibel aussehendes falsches Ergebnis. Details + Node-Fallback stehen jetzt in [[feedback_zeitzone]], Abschnitt "TZ-Umgebungsvariable ist in dieser Sandbox WIRKUNGSLOS".

Der ursprüngliche Fehler hing NICHT am TZ-Problem — es wurde ja gar kein `date` aufgerufen. Das TZ-Problem ist ein separater, bisher undokumentierter Stolperstein, der bei der Nachprüfung auffiel.

## Formulierungsvorschlag "Schritt 0" (HISTORISCHER ERSTENTWURF — überholt, siehe Gegencheck unten)

*(Stand-Korrektur 14.09.2026: Dieser Entwurf war der Ausgangspunkt, ist aber **nicht** der geltende Text. Verbindlich ist der von Fable umgesetzte Schritt 0 in [[feedback_session_update]] — er weicht bewusst an zwei Stellen ab: der Bar-Teil der Pflicht-Kopfzeile wandert in die T6-Zeile in Schritt 6 (im Kopf des Briefings ist er unausführbar, TradingView startet erst in Schritt 5), und statt eines einzigen Zeitpunkts gibt es zwei Messungen T0/T6. Beauftragt in [[project_fable_auftrag_schritt0_datumsverifikation_2026-09-14]]. Der Entwurf bleibt nur als Nachvollziehbarkeit der Kette stehen.)*

Einzufügen in `feedback_session_update.md` direkt vor "### Schritt 1 — Memory lesen":

> ### Schritt 0 — Jetzt-Zeit feststellen (PFLICHT, vor allem anderen)
>
> Vor Schritt 1 genau einen Befehl absetzen und sein Ergebnis wörtlich ins Briefing übernehmen:
>
> `date "+%A, %d.%m.%Y %H:%M:%S (%z)"; date -u "+UTC %H:%M"`
>
> Kein `TZ=` setzen — das liefert in dieser Sandbox still UTC statt DE-Zeit (siehe [[feedback_zeitzone]]). Der Offset muss `+0200` (CEST) bzw. `+0100` (CET) sein; zeigt er `+0000`, ist die Ausgabe UTC und unbrauchbar.
>
> **Bis dieser Aufruf erfolgt ist, ist JEDE Aussage über Wochentag, "heute"/"gestern", Wochenende, Feiertag, Handelstag oder Markt-Öffnungsstatus gesperrt** — auch in Nebensätzen und Level-Begründungen. Wochentag und Datum werden ausschließlich aus dieser Ausgabe gelesen, nie aus dem Gedächtnis, nie aus einem Memory-Eintrag, nie aus einem Kalender-Fetch abgeleitet.
>
> **Marktstatus wird gemessen, nicht aus dem Wochentag geschlossen:** Der Wochentag sagt nur, ob der Markt offen sein KÖNNTE. Ob er es IST, entscheidet der Zeitstempel-Vergleich aus [[feedback_live_trading]] Punkt 7e — Delta zwischen neuestem Bar und der Jetzt-Zeit aus Schritt 0. Widersprechen sich beide (z. B. "Markt zu", aber die Bars laufen frisch), gilt der Zeitstempel und der Widerspruch wird im Briefing benannt.
>
> Pflichtzeile im Briefing, ganz oben: `Jetzt: <Wochentag>, <TT.MM.JJJJ> <HH:MM> DE (%z=<offset>) | NAS100 letzter Bar: <ts>, Delta <n> Min | QQQ letzter Bar: <ts>, Delta <n> Min → Gate offen/zu`

**Ergänzungsvorschlag für [[feedback_memory_pflege]], Zitierpflicht** (ein Satz, am Ende des Regel-Absatzes):

> Belegpflichtig sind ausdrücklich auch **Datums-, Wochentags- und Marktstatus-Angaben** ("heute ist Montag", "Wochenende", "Markt geschlossen", "Stand von Freitag") — Beleg ist ein frischer `date`-Aufruf bzw. der Bar-Zeitstempel-Vergleich nach [[feedback_live_trading]] 7e, nie das Konversationsgedächtnis. Diese Angaben sind der gefährlichste Fall der Regel, weil an ihnen jede Folgeaussage hängt (Vorfall 14.09.2026, siehe [[feedback_datum_verifikation_fehler_2026-09-14]]).

## Einordnung

Eine dauerhafte Absicherung ist hier gerechtfertigt — nicht wegen der Fehlerhäufigkeit, sondern wegen der **Fehlerklasse**: Der Wochentag ist eine Wurzelannahme, an der alles Nachfolgende hängt (Marktstatus, Level-Begründungen, Session-Fenster, Blackout-Bewertung). Ein falscher Indikatorwert kostet eine Zeile; ein falscher Wochentag kontaminiert das ganze Briefing. Kosten des Gegenmittels: ein Tool-Call und eine Zeile. Das ist eine der wenigen Stellen, wo eine harte Pflichtregel eindeutig günstiger ist als die Fehlerwahrscheinlichkeit.

Kein Grund für mehr als das: Eine Ausweitung auf wiederholte Datums-Checks im laufenden Loop wäre Overhead — ein Aufruf pro Session-Update genügt, danach ist die Jetzt-Zeit bekannt und läuft mit.

---

## Opus-Gegencheck Schritt 0, 14.09.2026

**Gesamturteil: FREIGEGEBEN MIT AUFLAGEN.** 0 Blocker. 3 Nachbesserungen — alle drei von Opus im selben Durchgang selbst gefixt, keine offen. 3 der 4 von Fable gemeldeten Zusatzlücken rechtfertigen einen eigenen kleinen Folgeauftrag (Levi entscheidet), die vierte ist mit diesem Gegencheck erledigt. Kein Commit, kein Push.

### Was geprüft wurde

Unabhängig am Text verifiziert, nicht aus Fables Bericht übernommen: `feedback_session_update.md` (Schritt 0 vollständig, T6-Teilschritt in Schritt 6, Ausgabe-Struktur, Frontmatter, die drei „6-Schritt"-Fließtextstellen), `feedback_memory_pflege.md`, `feedback_live_trading.md` (7e-Zeile + 9a unberührt), `feedback_loop_ablauf_uebersicht.md`, `feedback_session_update_dax.md`, `feedback_zeitzone.md`, `MEMORY.md`, dazu `git status`/`git diff` in beiden Repos und die Tests.

**Dritte unabhängige TZ-Messung (Opus, 14.09.2026 23:15 DE):** `date "+%A, %d.%m.%Y %H:%M:%S (%z)"` → `Monday, 14.09.2026 23:15:19 (+0200)`; `date -u` → `UTC 21:15`; `TZ="Europe/Berlin" date` → `Mon Sep 14 21:15:19 GMT 2026` (**weiterhin falsch, still UTC**); `TZ="America/New_York" date` → identisch falsch; Node-Fallback → `Montag, 14.09.2026, 23:15`. Der TZ-Abschnitt in [[feedback_zeitzone]] ist damit zum dritten Mal unabhängig bestätigt und bleibt unverändert richtig.

### Auftragstreue (A1-A6, Nicht-Ziele)

Alle sechs Auftragspunkte sind umgesetzt, und zwar an den vorgesehenen Stellen: Schritt 0 sitzt zwischen `## Der 6-Schritt-Ablauf` und `### Schritt 1 — Memory lesen` (T3 bestätigt), die T0/T6-Bindungstabelle steht vollständig drin, Sperre und Plausibilitäts-Gegencheck sind eigene, hart formulierte Absätze, die Zitierpflicht in [[feedback_memory_pflege]] ist als **gleichrangige Kategorie** eingefügt (nicht als Nebensatz) und die Rotflaggen-Liste ist erweitert. Alle 10 in den Diffs neu gesetzten Wikilink-Ziele existieren.

Die Nicht-Ziele sind eingehalten — jedes einzeln am Text geprüft: kein neues Skript, kein CLI-Parameter, kein Guard (das Code-Repo `tradingview-mcp` ist laut `git status` **vollständig unverändert**, es wurde ausschließlich in `memory/` geschrieben); Punkt 7e hat genau **eine** hinzugefügte Zeile (Z. 861) und ist sonst zeichengleich, Punkt 9a ist überhaupt nicht angefasst; der Name „6-Schritt-Ablauf" bleibt und keine der 30 Fundstellen im Memory-Ordner ist durch die Änderung falsch geworden; die Delta-Regel aus 7e ist **nicht** nach Schritt 0 kopiert, dort steht stattdessen ausdrücklich „Die Delta-Regel selbst steht bewusst nur dort, nicht hier"; `TZ=` bleibt verboten, Node bleibt reiner Fallback.

### Bewertung von Fables Eigenkorrektur an der Pflicht-Kopfzeile (Auftragspunkt T2)

Fable hat das vorgeschriebene Format geändert (`Quelle: <bare date | Get-Date>`, beide Offset-Schreibweisen zulässig, Wochentag „so übernommen wie ausgegeben") — dazu war er durch T2 ausdrücklich ermächtigt, und **die Entscheidung ist fachlich richtig**, nicht bloß eine Notlösung: Eine Normalisierung (immer „Montag", immer `+0200`) würde einen Umrechnungsschritt zwischen Tool-Ausgabe und Briefing einbauen — und genau dieser Zwischenschritt im Kopf ist die Fehlerstelle, gegen die Schritt 0 überhaupt gebaut wurde. Die **Wörtlichkeit ist die Garantie, nicht die Einheitlichkeit.** Das `Quelle:`-Feld verwässert die Zeile auch nicht, es schärft sie sogar: Schreibweise und Quelle müssen zusammenpassen — `%z=+02:00` bei `Quelle: bare date` ist ein Widerspruch in der eigenen Zeile und verrät eine gebaute statt gemessene Kopfzeile. Diese Begründung stand aber nirgends im Regeltext; sie ist jetzt dort ergänzt (Nachbesserung 1), damit die Entscheidung beim nächsten Review nicht erneut als „Verwässerung" angegriffen wird.

### Nachbesserungen (alle von Opus selbst gefixt)

1. **Node-Fallback war in der Pflicht-Kopfzeile nicht befüllbar.** Die Selbstkontrolle erlaubt bei `%z=+0000` ausdrücklich den Node-Fallback, aber dessen Ausgabe hat weder Sekunden noch Offset, und die Quellen-Auswahl `<bare date | Get-Date>` ließ für ihn gar keinen Wert zu. Ausgerechnet im Fall, in dem die Zeile am wichtigsten wäre, wäre sie nur unehrlich auszufüllen gewesen. **Gefixt** in `feedback_session_update.md`: dritter zulässiger Quellenwert `node-Fallback`, Sekunden dürfen fehlen, `%z=n/a (Node/ICU)`, UTC weiterhin aus `date -u`. Im selben Absatz steht jetzt die Wörtlichkeits-Begründung von oben.
2. **`(T6)` vs. `(Bar-ts)` war bei Delta-Aussagen nicht entscheidbar.** Durchgespielt an drei echten Briefing-Sätzen: „US CPI 14:30 — bereits veröffentlicht" (Schritt 4 → `(T0)`, eindeutig), „Nikkei Stand vom Vortag" (Schritt 5 → `(T0)`, eindeutig), aber „QQQ letzter Bar 22:00 DE, Delta 75 Min → Gate zu" hängt gleichzeitig an T6 **und** am Bar-Zeitstempel; die Regel „`(Bar-ts)`, wenn sie direkt an einem Bar-Zeitstempel hängt" lässt beide Kürzel zu. Eine Ausweispflicht, deren Kürzel nicht eindeutig bestimmbar ist, ist nicht durchsetzbar. **Gefixt**: Entscheidungsregel ergänzt — das Kürzel benennt die *Jetzt-Zeit-Quelle*, nicht das Thema; reines Ablesen = `(Bar-ts)`, jede Rechnung gegen „jetzt" (Delta, „vor n Minuten", „aktuell/dünn/Stand von gestern") = `(T6)` in Schritt 6, sonst `(T0)`; `(T6/Bar-ts)` ist zulässig und bei der 7e-Gate-Aussage erwünscht. Mit aufgenommen: Zeitaussagen außerhalb der Bindungstabelle (z. B. das Vintage eines FRED-Werts) brauchen nur dann ein Kürzel, wenn sie eine Aussage über „jetzt" enthalten — sonst hätte die Tabelle eine stillschweigende Ausrede erzeugt.
3. **DAX-Ausgabe-Struktur ohne Punkt 0.** Der DAX-Verweisblock nennt die Pflicht-Kopfzeile korrekt, die dortige Ausgabe-Liste beginnt aber weiter bei 1. Da der Agent das Briefing aus dieser Liste rendert, wäre die Kopfzeile im DAX-Lauf trotz gültiger Regel schlicht weggefallen. **Gefixt** als reiner Verweis-Einzeiler (Punkt 0 → [[feedback_session_update]]), kein Volltext — die „per Verweis statt per Kopie"-Entscheidung aus A6.3 bleibt unangetastet.

### Hätte Schritt 0 den Fehler vom 14.09.2026 verhindert? — gegen den Text rekonstruiert

**Fehler 1 (Sonntag statt Montag): ja, strukturell zwingend.** Der Trigger „start update dich" hätte als allererstes `date` erzwungen → `Monday, 14.09.2026 22:46 (+0200)`; der Wochentag wird wörtlich übernommen und ausdrücklich **nicht** „korrigiert", die Sperre (A3) verbietet bis dahin jede Wochentagsaussage auch im Nebensatz, und die Pflicht-Kopfzeile macht das Ergebnis für Levi sofort sichtbar. Die Annahme „Sonntag" hätte an drei Stellen gleichzeitig scheitern müssen, um durchzukommen.

**Fehler 2 („Markt seit Freitag zu" trotz laufender Kurse): ja, über zwei unabhängige Wege.** Die T6-Pflichtzeile zwingt dazu, NAS100- und QQQ-Bar-Delta als Zahlen hinzuschreiben — ein wenige Minuten altes Delta neben der Behauptung „seit Freitag zu" ist nicht mehr übersehbar; zusätzlich verlangt A4, den Widerspruch explizit zu benennen („erwartet wäre X, gemessen ist Y → gemessen gilt") statt ihn stillschweigend aufzulösen. Und die Ableitung „Wochentag → Handelstag" ist jetzt ausdrücklich verboten.

**Ehrliche Einschränkung:** Die Absicherung ist vollständig **regeltextbasiert** — bewusst, so war der Auftrag. Es gibt keinen Guard, der ein Session-Update ohne `date`-Call abbricht. Dass Pflichtzeilen in diesem Projekt mechanisch mitlaufen oder ausbleiben können, ist belegt ([[project_opus_meilensteincheck_2026-09-10]]: `vollcheck.cjs` an einem Tag 0 von 18 Mal genutzt). Der Unterschied ist derselbe wie bei Punkt 9a: die Lücke wandert von „unsichtbar" nach „sofort sichtbar und gegenprüfbar", nicht nach „unmöglich". Wer mehr will, braucht einen maschinellen Einstiegs-Guard — das war hier ausdrücklich Nicht-Ziel und bleibt eine eigene Entscheidung.

### Tests

`node --test tests/trading_scripts.test.js` → **90/90 grün, 0 fail** (vor und nach den Opus-Nachbesserungen identisch; es wurde ohnehin kein Code angefasst). Der von Fable gemeldete Fehler in `tests/sanitization.test.js` ist **verifiziert vorbestehend und unabhängig**: dort scheitert `source audit — no unsafe interpolation patterns` mit `ENOENT ... 'C:\C:\Users\umnus\tradingview-mcp\src\core\'` — der klassische Windows-Fehler, dass `new URL(...).pathname` einen führenden Schrägstrich vor dem Laufwerksbuchstaben liefert. Die Datei stammt unverändert aus Commit `5f6648d` vom 12.08.2026, hat mit Markdown-Regeltexten keine Berührung, und die übrigen 40 Tests dieser Datei laufen grün. Kein Blocker für diesen Auftrag, aber ein stiller Dauerfehler im Test-Set (dieser Audit-Test prüft seit über einem Monat faktisch nichts) — für Levi vermerkt, bewusst nicht hier gefixt.

### Git-Diff-Sauberkeit

`memory/`: genau 7 geänderte + 2 neue Dateien, alle erwartet — `feedback_session_update.md`, `feedback_session_update_dax.md`, `feedback_memory_pflege.md`, `feedback_live_trading.md`, `feedback_loop_ablauf_uebersicht.md`, `feedback_zeitzone.md`, `MEMORY.md` plus die beiden neuen Dateien dieser Kette. Keine Seiteneffekte, keine gelöschten Passagen außer den bewusst ersetzten (im `feedback_session_update.md`-Diff: Frontmatter-`description` und `modified`, die drei „6 Schritte"-Fließtextstellen und die QQQ-Pane-Zeile, die den T6-Bezug bekommt). Fables Hinweis stimmt: im selben Diff stecken auch Opus' eigene Änderungen vom Abend (TZ-Abschnitt in [[feedback_zeitzone]], zwei MEMORY.md-Indexzeilen) — kein Fremdeffekt, sondern derselbe ungepushte Stand. Das Code-Repo ist unangetastet.

### Die 4 gemeldeten Zusatzlücken — Einordnung für Levi (NICHT von Opus behoben)

1. **Kalender-Fetch prüft das Datum der abgerufenen Seite nicht — echter Fund, Folgeauftrag empfohlen (höchste Priorität der vier).** Das ist tatsächlich die Wiederholung des 03.08.2026-Musters: damals zeigte der Fetch einen „Actual"-Wert für ein noch nicht veröffentlichtes Event — der eindeutige Beleg für eine gecachte Seite. Die daraus gebaute Gegenmaßnahme in Schritt 4 ist bis heute ein *inhaltlicher* Cross-Check gegen die Tweet-Kalender plus „im Zweifel Levi fragen"; ein Abgleich „steht auf der gefetchten Seite überhaupt das heutige Datum" existiert nicht. Genau dieser Abgleich ist seit heute für einen Zeilenpreis zu haben, weil T0 existiert. Vorschlag für einen sehr kleinen Folgeauftrag: in Schritt 4 eine Zeile, die verlangt, das auf der Seite genannte Datum gegen das T0-Datum zu prüfen und bei Abweichung die Seite als **STALE** zu behandeln (kein Blackout-Urteil daraus ableiten, neu fetchen oder Levi fragen). Ohne T0 war das nicht formulierbar — jetzt schon, deshalb jetzt.
2. **DAX-Wasserstand-Stempel — echter Fund, kleiner Folgeauftrag sinnvoll, aber nachrangig.** Bestätigt: `feedback_session_update_dax.md` Z. 42 sagt weiterhin „`x_last_fetch_dax.json` mit aktuellem UTC-Timestamp aktualisieren" — also genau die handgeschriebene „jetzt"-Stempelung, die auf der NAS100-Seite am 01.09.2026 den Wasserstand ~2 h in die Zukunft geschoben hat und seit 03.09.2026 durch `x_fetch_stamp.cjs` ersetzt ist. Nachrangig nur deshalb, weil der DAX-Block derzeit nicht aktiv gefahren wird ([[project_risikomanagement_dax]]: Budget offen); wird er reaktiviert, gehört das **vorher** gefixt, sonst importiert er einen bereits gelösten Fehler.
3. **EU/US-Sommerzeit-Divergenz — echter Fund, und Fable untertreibt die Größenordnung.** Der Text in [[feedback_zeitzone]] sagt „ET = DE − 6 h, solange beide Sommerzeit haben", ohne Trigger für den Fall, dass sie es nicht tun. Das sind nicht „~1 Woche/Jahr", sondern **rund vier Wochen**: die USA stellen am 2. Sonntag im März um, die EU erst am letzten Sonntag im März (≈3 Wochen mit 5 h statt 6 h Differenz), und im Herbst endet die EU-Sommerzeit am letzten Sonntag im Oktober, die US-amerikanische erst am 1. Sonntag im November (≈1 weitere Woche). In diesen Fenstern öffnet die US-Session um **14:30 DE statt 15:30 DE** — das verschiebt die Zonentabelle in [[feedback_live_trading]] 7e (die den Hinweis „bei US-Winterzeit je +1 h prüfen" immerhin trägt), das Halbierungsfenster 15:30-16:00 und die Standard-Releasezeit 14:30 DE. Nächstes Fenster: Ende Oktober 2026. Vorschlag: eine Zeile in [[feedback_zeitzone]] mit den vier Umstellungsdaten und der Konsequenz, plus ein Verweis in 7e — klein, aber terminiert.
4. **Veraltete `MEMORY.md`-Statuszeile — mit diesem Gegencheck erledigt.** Beide Indexzeilen (Auftrag + Fehlerdatei) und der Frontmatter-Status in beiden Dateien sind von Opus auf den tatsächlichen Stand gesetzt. Kein Folgeauftrag.

**Zusätzlicher eigener Fund (Info, kein Handlungsbedarf):** Die Ausweis-Pflicht bindet nur die sechs Zeilen der Bindungstabelle, die A3-Sperre dagegen *jede* Zeitaussage. Diese Asymmetrie ist vertretbar (sonst Kürzel-Spam), war aber als stillschweigende Ausrede nutzbar — mit Nachbesserung 2 ist sie jetzt ausdrücklich geregelt.

---

## Opus-Gegencheck Kalendercheck+DST, 15.09.2026

**Gesamturteil: FREIGEGEBEN MIT AUFLAGEN.** 0 Blocker. 2 Nachbesserungen von Opus selbst gefixt (beide reiner Regeltext, unten dokumentiert). 1 Code-Fund bestätigt, nicht gefixt (außerhalb des Gegencheck-Mandats), 1 Prozess-Beobachtung für Levi. Tests **90/90**. Kein Commit, kein Push.

Geprüft wurde die Umsetzung des Auftrags [[project_fable_auftrag_kalendercheck_dst_2026-09-14]] (Teil A Kalender-Datumscheck, Teil B DST-Divergenz-Fenster) — unabhängig, gegen die Spezifikation, nicht gegen Fables Bericht.

### 1. Datei-Perimeter (T1) — Bericht bestätigt, unabhängig verifiziert

`git diff --stat` gegen `df60a5a`: 9 geänderte + 5 neue Dateien. Davon gehören zu **diesem** Auftrag: `feedback_session_update.md`, `feedback_zeitzone.md`, `feedback_live_trading.md`, `feedback_trading_zeitfenster.md`, `feedback_vollcheck_format.md`, `MEMORY.md`, Auftragsdatei. Der Rest (`feedback_memory_pflege.md`, `feedback_loop_ablauf_uebersicht.md`, `feedback_session_update_dax.md`, die drei neuen Projektdateien) stammt aus der Schritt-0-Kette bzw. aus fremden Läufen — kein Teil-A-/Teil-B-Inhalt darin.

**Fables Behauptung „im zweiten Durchlauf nur 3 Dateien" ist belegt**, und zwar unabhängig von seinem Wort: Die Frontmatter-`modified`-Stempel trennen die beiden Läufe sauber. Alles aus Lauf 1 trägt `2026-09-14T21:xx`, nur `feedback_zeitzone.md` (`2026-09-15T01:25:58Z`) und die Auftragsdatei (`2026-09-15T01:27:10Z`) tragen 15.09.-Zeiten; `MEMORY.md` hat kein Frontmatter. Exakt die drei gemeldeten Dateien.

**DAX-Perimeter hart geprüft:** `git diff -- feedback_session_update_dax.md` selbst gelesen, vollständig. Der Diff enthält **ausschließlich** Schritt-0-Verweise (Description-Satz, „nach dem vorgeschalteten Schritt 0", ein Verweis-Block Schritt 0, ein Kopfzeilen-Punkt 0 in der Ausgabe-Struktur) — **kein einziges Wort zu DST, Divergenz-Fenster oder Kalender-Datumscheck**. Der Wasserstand-Stempel in Z. 42 steht unverändert. Keine weitere DAX-Datei ist angefasst (`dax_beobachtung/*`, `dax_trades/*`, `feedback_live_trading_dax.md`, `project_dax_erweiterung.md`, `project_risikomanagement_dax.md` alle unberührt). **Perimeter eingehalten.** Code-Repo `tradingview-mcp`: `git status` leer.

### 2. Teil A — vollständig umgesetzt, ein eigener Trockenlauf bestätigt K2

A1-A5 sind am Text nachgeprüft und alle vorhanden: Prompt-Pflichtangaben (a)+(b) inkl. der ausdrücklichen „kein Datum ist nicht bestanden"-Klausel; der K1/K2/K3-Block steht korrekt **nach** dem 03.08.2026-Absatz; STALE-Konsequenz mit dem strengen Zweig in fünf Punkten; Pflichtzeile `Kalender-Datumscheck (T0): …` in beiden Ausprägungen inkl. ausformuliertem 03.08.-Beispiel; Halbsatz in der Schritt-0-Bindungstabelle; die 30.07.-Spiegelregel ist genau einmal (in B5) um die DST-Ausnahme ergänzt, nicht doppelt.

**Eigener Trockenlauf 03.08.2026 (nicht Fables nachgelesen, selbst gerechnet):** T0 = 03.08.2026 vormittags. DST-Status zuerst: 03.08.2026 liegt in keinem Fenster (EU-Sommerzeit 29.03.–25.10.2026, US 08.03.–01.11.2026, beide aktiv) → `DST: normal`, ET = DE − 6 h. Damit ist ISM 10:00 ET = **16:00 DE** — die Uhrzeit im Regelwerk stimmt, K2 rechnet auf der richtigen Basis. Event-Uhrzeit 16:00 DE liegt **nach** der T0-Uhrzeit → Event in der Zukunft → ausgewiesener „Actual"-Wert ist logisch unmöglich → **K2 greift, beweisbar, nicht nur verdächtig.** Rechtsfolge nach A3: kein Blackout-Urteil aus diesem Fetch, Re-Fetch über die andere Quelle, Cross-Check gegen DeItaone/Kobeissi, bis zur Klärung strenger Zweig. Zusätzlich hätte an diesem Tag **K3** gegriffen (fehlende NFP am ersten Freitag). **Die Regel fängt den Fall.** Bestätigt.

**Nachbesserung 1 (selbst gefixt) — K1/K2 sind kumulativ, und K1 allein genügt.** Die Pflichtzeilen-Vorlage `⚠️ STALE — <K1|K2|kein Datum>: …` liest sich als Entweder-oder; der Trockenlauf deckte nur den K2-Fall ab. Beide im Gegencheck-Auftrag genannten Konstellationen waren damit nicht sauber abgedeckt: (a) K1 und K2 gleichzeitig — der Normalfall einer gecachten Seite, die zeigt typischerweise **beides**; (b) K1 allein — Seite mit falschem Datum, aber zufällig plausiblen Werten. Der Rechtsfolgen-Satz („Trifft K1, K2 oder ‚kein Datum' zu, gilt der Fetch als STALE") deckte (b) zwar implizit, die Vorlage lud aber zum Weglesen ein. In `feedback_session_update.md` ergänzt: Kriterien sind kumulativ, bei Mehrfachtreffer **alle** nennen (`K1+K2: …`), **K1 allein ist bereits STALE, auch wenn alle Werte plausibel aussehen** — eine Seite mit falschem Datum ist kein Beleg für heute, egal wie gut sie aussieht. Gleiche Rechtsfolge, gleiche Schritte 1-5. *(Info, kein Handlungsbedarf: Der umgekehrte Fall — Event liegt zeitlich in der Vergangenheit, zeigt aber KEIN Actual — ist kein Kriterium. Das ist vertretbar, weil Verzögerungen bei der Actual-Erfassung normal sind und K3 die Vollständigkeitsseite abdeckt.)*

### 3. Teil B — Tabelle unabhängig nachgerechnet UND nachgemessen: korrekt

**Eigene Berechnung** (letzter So März/Okt für EU, zweiter So März / erster So Nov für USA, ohne Fables Werte anzusehen):

| Jahr | EU-Start | US-Start | EU-Ende | US-Ende |
|---|---|---|---|---|
| 2026 | 29.03. | 08.03. | **25.10.** | **01.11.** |
| 2027 | **28.03.** | **14.03.** | **31.10.** | **07.11.** |
| 2028 | **26.03.** | **12.03.** | **29.10.** | **05.11.** |

**Alle zehn für die Tabelle relevanten Umstellungsdaten stimmen exakt** mit B2 überein, ebenso die abgeleiteten Handelstags-Spannen (Mo 26.10.–Fr 30.10.2026 = 5 HT; Mo 15.03.–Fr 26.03.2027 = 10 HT; Mo 01.11.–Fr 05.11.2027 = 5 HT; Mo 13.03.–Fr 24.03.2028 = 10 HT; Mo 30.10.–Fr 03.11.2028 = 5 HT) und die Handelstags-Zählungen. **Zusätzlich per Node-ICU an 20 Stichtagen gemessen** (jeweils der Tag vor dem Fensterbeginn, erster Fenstertag, letzter Fenstertag, Tag des Fensterendes): jeder Fenstertag liefert `GMT+1 | GMT-4` = 5 h, jeder Tag außerhalb 6 h. **Rechnung und Messung sind deckungsgleich, die Tabelle ist bestätigt.** Konsequenztabelle stichprobenartig gegengerechnet (8:30 ET → 13:30 DE, 10:00 ET → 15:00 DE, Open 9:30 ET → 14:30 DE) — korrekt. Verfallsregel (B3), Node-Gegencheck (B4), Auslegungs-Kennzeichnung der sessionverankerten Fenster, Abgrenzungssatz zur Zeitbasen-Konvention: alle vorhanden und wie spezifiziert.

**Nachbesserung 2 (selbst gefixt) — Fables neue Wochenend-Fußnote war an einem Rand falsch.** Die Ergänzung ist **nötig und richtig im Kern**: Die Tabellenspalte nennt nur Handelstage, der NAS100-CFD öffnet aber schon Sonntagabend, und ein Sonntagabend-Session-Update hätte sonst keine Antwort. Überkompliziert ist sie nicht. **Aber die Formulierung „physisch reicht jedes Fenster vom ersten Umstellungssonntag bis zum zweiten" ist an genau der Stelle unpräzise, die die Fußnote selbst als Anwendungsfall nennt** — die beiden Rand-Sonntage liegen asymmetrisch. Selbst gemessen (Node-ICU, CFD-Open ca. 23:00 DE):

- So **25.10.2026** 23:00 DE → `GMT+1 | GMT-4` = 5 h → **Fenster aktiv** (EU stellt um 03:00 DE um, lange vor dem Open).
- So **01.11.2026** 23:00 DE → `GMT+1 | GMT-5` = 6 h → **kein Fenster** (USA stellen um 2:00 ET, also ca. 07:00 DE um, ebenfalls lange vor dem Open).
- Gegenprobe Frühjahr: So **14.03.2027** 23:00 DE = 5 h (aktiv), So **28.03.2027** 23:00 DE = 6 h (vorbei).

Nach Fables Wortlaut wäre ein Sonntagabend-Update am 01.11.2026 fälschlich im Fenster gelandet — **eine Stunde falsch, in genau dem Fall, für den die Fußnote geschrieben wurde.** In `feedback_zeitzone.md` korrigiert: die Rand-Sonntage sind ausmessend gegenübergestellt, mit Merksatz **„ab dem ersten Umstellungssonntag, bis einschließlich Samstag vor dem zweiten"**, plus dem Hinweis, dass an beiden Rand-Sonntagen nach (4) gemessen statt geschlossen wird. Fables Sa-Beispiele (Sa 31.10.2026, Sa/So 20./21.03.2027) sind korrekt und bleiben stehen.

### 4. B5-Token im Loop-Einsatz durchgespielt

Am Format selbst durchgespielt für den 28.10.2026: Ein Sonnet-Agent braucht dafür **keinen Rechenschritt** — T0-Datum aus dem `date`-Aufruf, ein Blick in die Spalte „betroffene Handelstage", Zeile getroffen, Suffix schreiben. Die Kopfzeile enthält den fertig ausformulierten Divergenz-Text inklusive Beispiel; die zu nennenden Uhrzeiten (13:30 DE für 8:30-ET-Releases, 14:30–15:00 DE Halbierungsfenster) stehen fertig in der Konsequenztabelle. Drei Ausprägungen (`normal` / `⚠️ DIVERGENZ-FENSTER … aktiv` / `UNBEKANNT`), alle drei im Kopfzeilen-Template. **Reiner Tabellen-Lookup, wie beabsichtigt — eindeutig genug.** T3 unabhängig nachvollzogen: 14.09.2026 → `normal`, 28.10.2026 → Fenster (13:30 / 14:30–15:00), 10.11.2026 → `normal`. Auch die Abgrenzung zur Wörtlichkeitsregel ist sauber gelöst (T0-Teil wörtlich, DST-Teil abgeleitet, ausdrücklich erklärt).

### 5. T5/T6/T7 nachgefahren

- **T5 Wikilinks:** alle `[[…]]`-Ziele in den fünf geänderten Regeldateien auf Existenz geprüft — **0 tote Links**.
- **T6 Tests:** `node --test tests/trading_scripts.test.js` → **90/90 pass, 0 fail** (vor und nach meinen beiden Text-Fixes; es wurde kein Code angefasst).
- **T7 Doppelpflege:** Die Fenster-Tabelle steht genau einmal — in `feedback_zeitzone.md`. Alle anderen Fundstellen sind Verweise oder das Kopfzeilen-Token. Die Konsequenztabelle existiert zusätzlich in der **Auftragsdatei**, das ist die Spezifikation (Archiv) und keine gepflegte Regelstelle — unkritisch.

### 6. `MEMORY.md`-Kürzung — geprüft, **sicher**

Gegen `df60a5a` Wort für Wort durchgegangen. Der Diff ist kleiner als der Bericht vermuten lässt: **genau EINE bestehende Zeile wurde gekürzt** (die N1-/F-B-Zeile), drei Zeilen wurden **erweitert** (Zeitzone, Session Update, Trading Zeitfenster), fünf Zeilen sind neu. Netto **17.609 → 21.164 Bytes**, also gewachsen; die von Fable berichteten „24,1 → 21,2 KB" beziehen sich auf den ungepushten Zwischenstand, nicht auf den letzten Commit.

Verloren gegangen sind in der N1-Zeile: die Belegzahlen `37 VC/3 h`, `37/37`, `12/13`, das Feld `prev_poll`, „Rundungsfalle gelöst", „im Cron-Prompt verankert", die Kurzbeschriftungen von N-2/N-3/N-4 und der Verweis auf O-1. **Erhalten sind alle Punkte mit Rechtsfolge:** der Status `FREIGEGEBEN MIT AUFLAGEN`, die **fett ausgezeichnete Auflage A-1** („vor dem Testtag stempeln, sonst Migrations-Fallback = altes Verhalten" — inhaltlich dieselbe Warnung wie das gestrichene „12/13"), N-1 als offene Nachbesserung, die Tatsache dass N-2/N-3/N-4 offen sind, und `Tests 90/90`. Der gestrichene O-1-Verweis ist kein Verlust: O-1 wurde im Gegencheck vom 14.09. **bereits ausgeführt** — die Stempelpflicht steht nachweislich in [[feedback_loop_ablauf_uebersicht]] Schritt 6 (Satz „Nach JEDEM tatsächlich durchgeführten Abruf stempeln …" verifiziert). Die `ungepusht`/`nicht committet`-Marker sind von **1 auf 3 gestiegen**, nicht gefallen — der Kritikpunkt der letzten Runde wurde also nicht wiederholt.

Die beiden **fremden** Indexzeilen (Rückblick-Handelstag, Präzedenz-TO-DO) habe ich gegen die Frontmatter ihrer Zieldateien geprüft: beide offenen Levi-Punkte der Rückblick-Datei (Register-Defekt Session-Hoch, Option-A-Datenpunkte übernehmen ja/nein) und der Status der TO-DO-Datei stehen vollständig in der jeweiligen Indexzeile. **Kein Informationsverlust, keine chirurgische Ergänzung nötig.** Die Befürchtung „Fable kürzt an einem fremden Thema eine Nuance weg" hat sich hier nicht bestätigt.

### 7. Tatsachenfeststellung zum wiederholten Selbst-Komprimieren

**Die beim letzten Mal vorgeschlagene Konvention steht NICHT im Regelwerk.** `feedback_memory_pflege.md` vollständig durchsucht (und zur Sicherheit alle `memory/*.md`): kein Treffer für „Hook-Hinweis", „Harness-Hook", „Lesegrenze", „Statusmarker nie wegkürzen" oder eine sinngemäße Regel. Der Vorschlag ist im Gegencheck vom 14.09. stehen geblieben und wurde nie eingetragen.

**Einordnung, unmissverständlich: Das war KEIN Regelverstoß, sondern zum zweiten Mal Scope-Creep auf einen Harness-Hinweis hin.** Fable hat keine bestehende Regel gebrochen — es gibt keine. Ehrlich dazu gehört aber: Es ist jetzt zweimal an derselben Kette passiert, beim zweiten Mal an **fremden** Indexzeilen, und die Bewertung „sicher" musste beide Male erst ein Gegencheck herstellen. Das Muster ist real, auch wenn der konkrete Schaden diesmal null ist.

**Empfehlung an Levi (Prozessregel-Entscheidung, bewusst NICHT von Opus eingetragen):** Falls du es abstellen willst, wäre der Ort [[feedback_memory_pflege]], und der Satz etwa: *„Hinweise des Harness (z.B. ‚MEMORY.md nähert sich der Lesegrenze') sind Hinweise an Levi, kein Auftrag an einen laufenden Subagenten. Ein Agent, der nicht zum Aufräumen beauftragt ist, meldet den Hinweis und kürzt nicht selbst. Wird gekürzt, bleiben aktive Statusmarker (`ungepusht`, offene Auflagen, offene Levi-Entscheidungen, Zahlen mit Rechtsfolge) wörtlich erhalten."* Zwei Sätze, eine Datei — aber es ist deine Entscheidung, nicht meine.

### 8. Bestätigter Code-Fund: `vollcheck.cjs` kennt das Fenster nicht

**Fables Fund ist korrekt, selbst nachgelesen.** `scripts/vollcheck.cjs` Z. 401-404:

```
const { h: deH, m: deM } = deParts(jetztMs);
const orderSperre = deH === 15 && deM < 30;
const halbierung = deH === 15 && deM >= 30;
const halbierungAbgelaufen = deH === 16 && deM < 5;
```

`deParts()` rechnet die DE-Zeit korrekt (`toLocaleTimeString` mit `Europe/Berlin`, Node-ICU) — der Fehler liegt **nicht** in der Zeitzone, sondern in den **hart verdrahteten Stunden 15/16**. In einem Divergenz-Fenster läuft die Order-Sperre real von 14:00–14:30 DE und das Halbierungsfenster von 14:30–15:00 DE; das Skript würde beide Marker eine Stunde zu spät setzen und den „Halbierungsfenster abgelaufen"-Hinweis eine Stunde zu spät geben. **Praktische Folge: in der ersten realen Handelsstunde des Fensters meldet das Skript „kein Sperr-/Halbierungsfenster", während beide aktiv sind — und setzt sie später, wenn sie längst vorbei sind.** Das ist die gefährlichere von zwei Richtungen (Sperre fehlt, wenn sie gelten müsste). Kein weiterer Fundort im Code: `deH`/`deM` werden sonst nur relativ verwendet (Slot-Anzeige, Abstand zu einer Release-Uhrzeit), nicht gegen feste Sessionzeiten.

**Dringlichkeits-Einschätzung: kann warten, aber nicht bis zum Fenster.** Kein Schaden vor dem **26.10.2026** (rund sechs Wochen), und selbst dann kein Gate-Loch — `vollcheck.cjs` setzt Marker, das Entry-Gate hängt an 7b1/7e. Empfohlener Zuschnitt: **eigener kleiner Code-Auftrag, fällig vor dem 26.10.2026**, sinnvollerweise gebündelt mit dem noch offenen DAX-Wasserstand-Fund, sobald der DAX-Block wieder anläuft. **Nicht** jetzt zwischen Tür und Angel, weil er eine Datumsquelle im Skript braucht: die Fenster-Tabelle müsste entweder als Konstante in den Code oder — die sauberere Variante, passend zu „gemessen statt geschätzt" — zur Laufzeit per Node-ICU aus dem `--jetzt`-Zeitanker gemessen werden (`Intl.DateTimeFormat` mit `timeZoneName:'shortOffset'` für `Europe/Berlin` und `America/New_York`, Differenz 5 h = Fenster). Von Opus bewusst **nicht** gefixt: Code-Änderung liegt außerhalb des Mandats dieses Gegenchecks.

### 9. Was Opus in dieser Runde geändert hat (2 Dateien, reiner Regeltext)

| # | Datei | Änderung | Klasse |
|---|---|---|---|
| 1 | `feedback_session_update.md`, Schritt 4, Pflichtzeilen-Block | K1/K2 ausdrücklich kumulativ (`K1+K2: …`); **K1 allein ist bereits STALE**, auch bei plausiblen Werten | Nachbesserung |
| 2 | `feedback_zeitzone.md`, Fußnote unter der Fenster-Tabelle | Rand-Sonntage asymmetrisch korrigiert (erster So = aktiv am Abend, zweiter So = bereits vorbei), gemessene Werte zitiert, Merksatz „bis einschließlich Samstag vor dem zweiten" | Nachbesserung |

Keine Regel inhaltlich geändert, keine Tabelle verschoben, keine DAX-Datei angefasst, kein Code angefasst. **Tests nach den Fixes erneut: 90/90.**

### 10. Offen nach diesem Gegencheck

1. **`vollcheck.cjs` Z. 402-404** — eigener Code-Auftrag, fällig vor dem 26.10.2026 (Abschnitt 8).
2. **DAX-Wasserstand-Stempel** (`feedback_session_update_dax.md` Z. 42) — unverändert offen, vor Reaktivierung des DAX-Blocks zu fixen.
3. **DAX-Kalender-Gegencheck** hat weiterhin keinen Datumscheck und keine DST-Klausel — dieselbe Warteschlange wie 2.
4. **Schritt-0-Bindungstabelle** nennt das Halbierungsfenster weiter hart als „15:30-16:00" (Fable-Meldung). Bewertet: **kein Handlungsbedarf** — die Zeile beschreibt, WORAN T6 hängt, nicht die gültige Uhrzeit, und die DST-Verschiebung steht zwei Absätze darüber im Kopfzeilen-Block sowie in [[feedback_trading_zeitfenster]]. Eine weitere Kopie der Uhrzeit wäre Doppelpflege (Nicht-Ziel 8).
5. **Prozessregel zum Selbst-Komprimieren** — Levi-Entscheidung, Abschnitt 7.
