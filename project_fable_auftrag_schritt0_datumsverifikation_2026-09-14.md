---
name: project-fable-auftrag-schritt0-datumsverifikation
description: "Fertiger, 1:1 an Fable weitergebbarer Auftrag: neuer Schritt 0 (Zeitanker Session) vor Schritt 1 in feedback_session_update.md — Pflicht-date-Aufruf als allererster Tool-Call, verbindliche Referenzzeit T0/T6 für den gesamten Durchlauf, Sperre für jede Wochentags-/Marktstatus-Aussage vor Schritt 0, Plausibilitäts-Gegencheck Bar-Zeitstempel schlägt Wochentags-Ableitung; plus Zitierpflicht-Erweiterung in feedback_memory_pflege.md und Konsistenz-Nachzüge zu 7e/9a/Loop-Übersicht/DAX. Reine Regelwerksänderung, KEIN neues Skript."
metadata: 
  node_type: memory
  type: project
  status: "UMGESETZT 14.09.2026 (Fable, 7 Dateien) — Opus-Gegencheck durchgeführt: FREIGEGEBEN MIT AUFLAGEN, 0 Blocker, 3 Nachbesserungen von Opus selbst gefixt. Befund: feedback_datum_verifikation_fehler_2026-09-14, Abschnitt 'Opus-Gegencheck Schritt 0, 14.09.2026'. Nicht committet, nicht gepusht."
  modified: 2026-09-14T21:19:50.897Z
  originSessionId: a2b3dbfe-4ee4-466f-855e-65c519af6e1f
---

# Auftrag an Fable — Schritt 0 „Zeitanker Session" (Vorfall 14.09.2026)

**Status (aktualisiert 14.09.2026 nach dem Gegencheck):** Der Auftrag ist von Fable **umgesetzt** (7 geänderte Memory-Dateien, kein Code) und von Opus unabhängig gegengeprüft — Ergebnis **FREIGEGEBEN MIT AUFLAGEN**, 0 Blocker, 3 Nachbesserungen direkt von Opus gefixt. Der vollständige Befund steht in [[feedback_datum_verifikation_fehler_2026-09-14]], Abschnitt „Opus-Gegencheck Schritt 0, 14.09.2026". Nichts davon ist committet oder gepusht.

*(Ursprünglicher Status bei Auftragserteilung: „Auftragstext fertig, nichts davon ist umgesetzt. Opus hat bewusst NICHTS am Regelwerk geändert — die Umsetzung ist vollständig dieser Auftrag.")*

**Quelle/Fehlerbild:** [[feedback_datum_verifikation_fehler_2026-09-14]] (Root-Cause-Analyse, TZ-Sandbox-Befund, unverbindlicher Erstentwurf für Schritt 0).

**Levis Zusatzanforderung, die diesen Auftrag gegenüber dem Erstentwurf erweitert:** „Was ist mit der Uhrzeit? Wir brauchen ja nicht nur aktuelles Datum, sondern auch die korrekte Uhrzeit beim start update." → Abschnitt **A2 (Referenzzeit-Bindung T0/T6)** unten ist die Antwort darauf: Die verifizierte Uhrzeit ist nicht nur eine Anzeigezeile am Anfang, sondern die verbindliche Bezugsgröße jeder späteren Zeitaussage im Durchlauf, an zwei definierten Stellen gemessen und an jeder Verwendungsstelle mit Quellenkürzel ausgewiesen.

**Zwei Abweichungen von Opus' eigenem Erstentwurf in [[feedback_datum_verifikation_fehler_2026-09-14]]** (bewusst, Begründung steht im Auftrag):
1. Die dort vorgeschlagene Kopfzeile verlangte NAS100-/QQQ-Bar-Zeitstempel bereits im Kopf des Briefings — das ist unausführbar, weil TradingView erst in Schritt 5 gestartet wird. Der Bar-Teil wandert deshalb in eine zweite Zeile in Schritt 6, wo die Bars ohnehin gelesen werden.
2. Der Erstentwurf hatte nur EINEN Zeitpunkt (Anfang). Ein Session-Update dauert real 10-30 Minuten — für die zeitkritischen Aussagen in Schritt 6 (Bar-Delta, Halbierungsfenster) wird deshalb ein zweiter Stempel verlangt.

---

--- AB HIER 1:1 ALS PROMPT AN FABLE ---

## Auftrag: Schritt 0 „Zeitanker Session" ins Session-Update-Regelwerk einbauen

Du bist Fable im Trading-Projekt (`C:\Users\umnus\tradingview-mcp`, Memory unter `C:\Users\umnus\.claude\projects\C--Users-umnus-tradingview-mcp\memory\`). Sprache Deutsch, Levi wird geduzt.

Das hier ist eine **reine Regelwerksänderung an Memory-Markdown-Dateien**. **Baue KEIN neues Skript, keinen neuen CLI-Parameter, keinen Guard.** Das wurde geprüft und bewusst verworfen (Begründung: [[feedback_datum_verifikation_fehler_2026-09-14]], Abschnitt „Einordnung" — ein Tool-Call plus eine Zeile ist das ganze Gegenmittel; die maschinelle Absicherung für Zeitstempel existiert für den Pfad, wo sie hingehört, nämlich Statusdateien: `x_fetch_stamp.cjs`, `register_touch.cjs`, der Frische-Guard in `gate_check.cjs`. Die Lücke liegt ausschließlich bei den ERZÄHLTEN Zeitaussagen im Briefing). Wenn du fachlich anderer Meinung bist: schreib es in deine Rückmeldung, aber baue es nicht.

### Zuerst lesen (vollständig, bevor du irgendetwas änderst)

1. `memory/feedback_datum_verifikation_fehler_2026-09-14.md` — Fehlerbild, Root Cause, TZ-Befund, Erstentwurf
2. `memory/feedback_session_update.md` — der zu ändernde 6-Schritt-Ablauf (Schritt 1 ab Zeile ~25, Schritt 6 ab Zeile ~136, „Ausgabe-Struktur des Briefings" ab Zeile ~168)
3. `memory/feedback_zeitzone.md` — insbesondere der Abschnitt „TZ-Umgebungsvariable ist in dieser Sandbox WIRKUNGSLOS"
4. `memory/feedback_memory_pflege.md` — Abschnitt „Zitierpflicht für Permanent-Records" (ab Zeile ~51)
5. `memory/feedback_live_trading.md` **Punkt 7e** (ab Zeile ~852, QQQ-Session-Gate) und **Punkt 9a** (ab Zeile ~989, „Zeitanker-Pflicht bei JEDEM Fire") — beide bleiben inhaltlich unverändert, du brauchst sie nur für die saubere Abgrenzung
6. `memory/feedback_loop_ablauf_uebersicht.md` — Szenario 1, Block „Einmalig bei Session-Start"
7. `memory/feedback_session_update_dax.md` — identische Struktur, identische Lücke

### Der Vorfall in zwei Sätzen

Am 14.09.2026 gegen 22:46 DE nahm der Live-Loop-Agent beim Trigger „start update dich" ohne einen einzigen `date`-Aufruf an, es sei **Sonntag** — real war **Montag**. Daraus folgte ein durchgehend kontaminiertes Briefing (NAS100 als „Wochenend-Vorbörse", QQQ als „Freitags-Kassaschluss" statt eines 46 Minuten alten Nachbörsen-Stands, Session-Extrema als „Wochenend-Rollfenster"); Levi musste es korrigieren.

**Root Cause, die du beheben sollst:** Keine Stelle im Regelwerk verlangt VOR Schritt 1, Datum/Wochentag/Uhrzeit frisch festzustellen. Der passende Mechanismus existiert (Punkt 7e, Zeitstempel-Vergleich), sitzt aber in Schritt 6 — zu spät, und er beantwortet ohnehin nur „ist das QQQ-Gate offen", nicht „welcher Tag/welche Uhrzeit ist jetzt". Der Loop hat seine Zeitdisziplin bereits (Punkt 9a, „Zeitanker-Pflicht bei JEDEM Fire": bare `date` vor jedem anderen Tool-Call). **Das Session-Update ist der einzige Einstiegspunkt ohne dieses Ritual — genau diese Asymmetrie schließt du.**

---

## A1 — Neuer „Schritt 0" in `feedback_session_update.md`

Einfügen **direkt vor** `### Schritt 1 — Memory lesen`, also als erster Block unter der Überschrift `## Der 6-Schritt-Ablauf`.

**Benennung (bitte exakt so, aus Konsistenzgründen):** `### Schritt 0 — Zeitanker Session (PFLICHT, vor allem anderen)`. Der Name spiegelt bewusst die „Zeitanker-Pflicht" aus [[feedback_live_trading]] Punkt 9a — dieselbe Disziplin, andere Ebene (Session statt Fire).

**Wichtig — Namensschema NICHT umbauen:** Der Ablauf heißt weiterhin „6-Schritt-Ablauf", Schritt 0 ist eine **Vorstufe**, kein siebter Schritt. Grund: „6-Schritt-Ablauf"/„alle 6 Schritte" wird an mindestens 10 Stellen in anderen Memory-Dateien zitiert (`MEMORY.md`, `project_testtag_plan_01_02_09_2026.md`, `project_dax_erweiterung.md`, `feedback_session_update_dax.md`, mehrere `testtag/`-Protokolle). Eine Umbenennung auf „7-Schritt-Ablauf" würde all diese Querverweise still falsch machen, ohne einen einzigen Fehler zu verhindern.

Der einzufügende Text (Formulierung ist ein Vorschlag, Inhalt ist verbindlich):

> ### Schritt 0 — Zeitanker Session (PFLICHT, vor allem anderen)
>
> **Der allererste Tool-Call des gesamten Session-Updates ist die Zeitabfrage — vor Memory, vor Tweets, vor jedem Chart-Call.** Genau ein Befehl, Ergebnis wörtlich übernehmen:
>
> - **Bash-Tool (Standard):** `date "+%A, %d.%m.%Y %H:%M:%S (%z)"; date -u "+UTC %H:%M"`
> - **PowerShell (gleichwertig, falls dort gearbeitet wird):** `Get-Date -Format 'dddd, dd.MM.yyyy HH:mm:ss K'; 'UTC ' + (Get-Date).ToUniversalTime().ToString('HH:mm')`
>
> Beide Wege sind am 14.09.2026 live gegeneinander verifiziert (identische Sekunde, identischer Offset; PowerShell liefert den Wochentag deutsch — „Montag" statt „Monday" —, das ist kein Unterschied in der Sache). **Es wird nur EIN Weg genutzt, nicht beide** — welcher, ist egal.
>
> **`TZ=` NIEMALS setzen.** Diese Sandbox hat keine Zonendatenbank; ein gesetztes `TZ="Europe/Berlin"` fällt **still** auf UTC zurück und liefert eine plausibel aussehende, um 2 h falsche Zeit (Messung siehe [[feedback_zeitzone]], Abschnitt „TZ-Umgebungsvariable ist in dieser Sandbox WIRKUNGSLOS"). Der bare `date` ist korrekt und bleibt der Standard.
>
> **Selbstkontrolle am Offset:** `%z` muss `+0200` (CEST) bzw. `+0100` (CET) zeigen und die UTC-Zeile muss entsprechend 2 h bzw. 1 h zurückliegen. Steht dort `+0000`, ist die Ausgabe UTC und unbrauchbar → Fallback mit echter Zonendatenbank: `node -e "console.log(new Date().toLocaleString('de-DE',{timeZone:'Europe/Berlin',weekday:'long',day:'2-digit',month:'2-digit',year:'numeric',hour:'2-digit',minute:'2-digit'}))"`. Lässt sich auch damit keine plausible DE-Zeit herstellen, wird das Session-Update **abgebrochen** und Levi gefragt — nicht geschätzt.
>
> **Pflicht-Kopfzeile, ganz oben im Briefing (vor Ausgabe-Punkt 1):**
> `Zeitanker T0: <Wochentag>, <TT.MM.JJJJ> <HH:MM:SS> DE (%z=<Offset> | UTC <HH:MM>) — Quelle: bare date, Schritt 0`
>
> Fehlt diese Zeile, gilt das Session-Update als **nicht durchgeführt** — dieselbe Behandlung wie ein fehlendes `level_register.json` in Schritt 6 oder die fehlenden Pflichtzeilen `Tweet-Check:`/`Format:` im Voll-Check.
>
> **Handelstag ja/nein wird NICHT aus dem Wochentag geschlossen.** T0 sagt nur, ob der Markt offen sein KÖNNTE. Ob er es IST, entscheidet der Zeitstempel-Vergleich aus [[feedback_live_trading]] Punkt 7e in Schritt 6 (Delta neuester Bar ↔ Jetzt-Zeit). Schritt 0 liefert die Grundannahme, 7e die Messung.

## A2 — Referenzzeit-Bindung: T0 gilt für den GESAMTEN Durchlauf (Levis Kernanforderung)

Das ist der Teil, der über den Erstentwurf hinausgeht, und der wichtigste des Auftrags. Levi hat ausdrücklich verlangt, dass nicht nur das Datum, sondern die **korrekte Uhrzeit** feststeht und **verbindlich bleibt** — nicht einmal angezeigt und dann vergessen. Baue in Schritt 0 diesen Block mit ein:

> **Verbindlichkeit von T0 (und T6):**
>
> Die in Schritt 0 gemessene Zeit heißt **T0** und ist ab dann die **einzige zulässige Zeitquelle** für den restlichen Durchlauf. Jede Aussage im Briefing, die von Datum, Wochentag, Uhrzeit oder Marktstatus abhängt, wird aus T0 abgeleitet oder gegen T0 geprüft — nie neu geschätzt, nie aus dem Konversationsgedächtnis, nie aus einem Memory-Eintrag, nie aus dem Datum eines Kalender-Fetchs.
>
> **Zweiter Stempel T6:** Ein Session-Update dauert real 10-30 Minuten. Deshalb wird zu Beginn von Schritt 6 der Zeitbefehl **ein zweites Mal** abgesetzt (`T6`) — dort hängen die zeitkritischsten Aussagen dran (Bar-Delta nach 7e, Halbierungsfenster, „heutige Range"). Für alles andere gilt T0. T6 wird in Schritt 6 ausgewiesen (siehe Pflichtzeile unten), nicht im Kopf. **Mehr als diese zwei Messungen sind nicht nötig** — wer dazwischen eine Minutenangabe braucht, leitet sie aus T0/T6 ab und kennzeichnet sie als abgeleitet.
>
> **Konkrete Bindungsstellen — an diesen Stellen ist die T0/T6-Herkunft im Briefing auszuweisen:**
> | Stelle | Was daran hängt | Quelle |
> |---|---|---|
> | Schritt 1 (Memory) | „letzter Trade vor N Tagen", „seit der letzten Session" | T0 |
> | Schritt 2 (Tweets) | Alter des Wasserstands/`last_poll`-Deltas in der Erzählung (die Rechnung selbst macht `x_fetch_stamp.cjs --check`) | T0 |
> | Schritt 4 (Kalender) | welche Events heute noch ANSTEHEN vs. bereits veröffentlicht sind; jede Blackout-/Sperrfrist-Aussage | T0 |
> | Schritt 5 (Intermarket) | ob ein Quote „aktuell", „vorbörslich", „nachbörslich" oder „Stand vom Vortag" ist | T0 |
> | Schritt 6 (Chart) | Bar-Delta/Session-Gate 7e, QQQ-Kennzeichnung („dünn"/„Stand von gestern"), „heutige Range bisher", Session-Extrema, Halbierungsfenster-Reminder 15:30-16:00 | **T6** |
> | Ausgabe-Punkt 6 (Bias) | jede „heute"/„gestern"/„Wochenende"/„vor Handelsbeginn"-Formulierung | T0 |
>
> **Ausweis-Pflicht:** Jede dieser Aussagen trägt im Briefing ein Quellenkürzel — `(T0)`, `(T6)` oder `(Bar-ts)`, wenn sie direkt an einem Bar-Zeitstempel hängt. Eine Zeitaussage ohne Kürzel gilt als geschätzt und damit als Regelbruch. Das ist derselbe Mechanismus wie bei der Wörtlich-Zitierpflicht für `gate_check.cjs`: eine Behauptung ist nicht dasselbe wie ein vorzeigbarer Rohwert.
>
> **Pflichtzeile in Schritt 6 (zusätzlich zur Kopfzeile):**
> `Zeitanker T6: <HH:MM:SS> DE | NAS100 letzter Bar <HH:MM DE> (Delta <n> Min) | QQQ letzter Bar <HH:MM DE> (Delta <n> Min) → Session-Gate 7e: offen/zu`
>
> Fehlt sie, gilt Schritt 6 als **nicht vollständig** — dieselbe Behandlung wie ein fehlender AVWAP-/RVOL-Check oder ein nicht geschriebenes `level_register.json`.

**Hinweis zur Einordnung, den du mit aufnehmen sollst:** Die Statusdateien (`level_register.json` `updated`, `x_last_fetch.json`) bleiben unberührt bei **UTC mit Z-Suffix** — die Zeitbasen-Konvention aus [[feedback_zeitzone]] gilt unverändert. T0/T6 sind DE-Ortszeit für die **Ausgabe/Erzählung**; wer daraus einen Statusdatei-Zeitstempel schreibt, rechnet um (bzw. lässt es weiter die Skripte machen, `register_touch.cjs` setzt `updated` ohnehin selbst).

## A3 — Sperre: keine Zeitaussage vor Schritt 0

Als eigener, hart formulierter Absatz in Schritt 0 (Vorbild für Ton und Bindungsgrad: die MTF-Block-Pflicht in [[feedback_live_trading]] Punkt 9 und die `--testtag`-Pflicht in `loop_prompt.cjs`):

> **Sperre bis Schritt 0 erledigt ist:** Bevor die Zeitabfrage abgesetzt und ihr Ergebnis gelesen wurde, ist **JEDE** Aussage über Wochentag, Datum, „heute"/„gestern"/„morgen", Wochenende, Feiertag, Handelstag, Vor-/Nachbörse oder Markt-Öffnungsstatus **gesperrt** — auch in Nebensätzen, auch in Level-Register-Begründungen (`abrufweg`), auch in Zwischenkommentaren, auch als scheinbar harmlose Einordnung („da ja gerade Wochenende ist"). Kein Memory-Eintrag, kein Dateiname, kein Kalender-Fetch und keine Trainings-Annahme ersetzt diesen einen Aufruf. Im Zweifel: erst `date`, dann reden.

## A4 — Plausibilitäts-Gegencheck (Kurs schlägt Kalenderlogik)

Ebenfalls in Schritt 0, als eigener Absatz — das ist der zweite, unabhängige Fehler vom 14.09. (die Behauptung „Markt seit Freitag zu" stand im direkten Widerspruch zu frisch abgerufenen, sich bewegenden Kursen, und der Widerspruch fiel niemandem auf):

> **Plausibilitäts-Gegencheck — der Zeitstempel gewinnt:** Widerspricht eine Aussage über den Marktstatus (offen/geschlossen/dünn/„Stand von gestern") den im selben Durchlauf frisch abgerufenen Daten — zum Beispiel sich bewegende Kurse oder ein wenige Minuten alter Bar trotz behaupteter Marktschließung —, dann gilt **der Bar-Zeitstempel als Wahrheit**, nicht die aus dem Wochentag abgeleitete Erwartung. Der Widerspruch wird **explizit im Briefing benannt** („erwartet wäre X, gemessen ist Y → gemessen gilt"), nie stillschweigend zugunsten einer der beiden Seiten aufgelöst und nie weggelassen. Ein solcher Widerspruch ist außerdem immer ein Anlass, T0 gegen `date -u` zu plausibilisieren, bevor weitergearbeitet wird.

## A5 — Zitierpflicht erweitern (`feedback_memory_pflege.md`)

Im Abschnitt **„Zitierpflicht für Permanent-Records"**:

**(a)** Im Regel-Absatz (der mit „Jede konkrete Zahlenbehauptung …" beginnt, ~Zeile 55) die bestehende Aufzählung (Preislevel, Kerzen-Timestamps/OHLC, Kerzenzählungen, Superlative) um eine **eigene, gleichrangige Kategorie** ergänzen — nicht als Nebensatz anhängen:

> **Datums-, Wochentags-, Uhrzeit- und Marktstatus-Angaben** („heute ist Montag", „Wochenende", „Markt geschlossen", „Stand von Freitag", „vorbörslich", „seit gestern") sind ebenso belegpflichtig. Beleg ist ein **frischer `date`-Aufruf** (bare, ohne `TZ=` — siehe [[feedback_zeitzone]]) bzw. der Bar-Zeitstempel-Vergleich nach [[feedback_live_trading]] 7e, nie das Konversationsgedächtnis und nie eine Kopfrechnung aus einem Dateinamen oder einem Memory-Datum. Diese Kategorie ist der gefährlichste Fall der ganzen Regel, weil an ihr jede Folgeaussage hängt: ein falscher Indikatorwert kostet eine Zeile, ein falscher Wochentag kontaminiert das komplette Briefing (Vorfall 14.09.2026, [[feedback_datum_verifikation_fehler_2026-09-14]]).

**(b)** In der Liste der **Rotflaggen-Phrasen** im Absatz „Leichter Selbst-Check statt Voll-Review" (~Zeile 57) die neuen Phrasen mit aufnehmen: „heute ist …", „gestern", „Wochenende", „Markt geschlossen/zu", „vorbörslich/nachbörslich", „Stand von <Wochentag>". Der Selbst-Check ist damit derselbe wie bisher, nur mit vollständiger Phrasenliste.

## A6 — Konsistenz-Nachzüge (klein, aber nicht weglassen)

1. **`feedback_live_trading.md` Punkt 7e — EINE Referenzzeile, keine Duplizierung.** 7e bleibt inhaltlich unverändert (es beantwortet „ist das QQQ-Gate offen", und das ist richtig so). Ergänze dort genau einen Satz, damit die Arbeitsteilung explizit ist:
   > *Die „Jetzt-Zeit", gegen die das Bar-Delta gerechnet wird, ist nie geschätzt: im Loop kommt sie aus der Zeitanker-Pflicht (Punkt 9a), beim Session-Update aus Schritt 0/T6 ([[feedback_session_update]]). 7e misst den Gate-Zustand, Schritt 0 liefert die Grundannahme „welcher Tag, welche Uhrzeit" — beides ist nötig, keins ersetzt das andere.*
   **Kopiere die Delta-Regel NICHT nach Schritt 0.** Sie soll genau einmal im Regelwerk stehen, so wie der `gate_check.cjs`-Aufrufweg auch nur einmal existiert.
2. **`feedback_loop_ablauf_uebersicht.md`, Szenario 1, Block „Einmalig bei Session-Start":** neuen Punkt **0** vor „1. Pivot-Level berechnen" einfügen: *„0. **Zeitanker T0** — bare `date` als allererster Call, siehe [[feedback_session_update]] Schritt 0. Entspricht auf Session-Ebene der Zeitanker-Pflicht aus [[feedback_live_trading]] Punkt 9a."* Ein Satz, kein Regeltext.
3. **`feedback_session_update_dax.md`:** identische Struktur, identische Lücke. Denselben Schritt 0 dort einfügen, aber **per Verweis statt per Kopie**: Überschrift plus zwei Sätze („Wortgleich zu [[feedback_session_update]] Schritt 0, inklusive Pflicht-Kopfzeile, T0/T6-Bindung, Sperre und Plausibilitäts-Gegencheck — dort nachschlagen, nicht hier neu formulieren."). Grund: Doppelte Volltexte driften auseinander, das hat dieses Regelwerk schon mehrfach erlebt (zwei `gate_check.cjs`-Templates, zwei Indikator-Checklisten).
4. **`feedback_zeitzone.md`:** Der TZ-Abschnitt von heute ist am 14.09.2026 22:56 DE erneut gegengemessen und **weiterhin korrekt** (`date "+%A, %d.%m.%Y %H:%M:%S (%z)"` → `Monday, 14.09.2026 22:56:24 (+0200)`; PowerShell `Get-Date -Format 'dddd, dd.MM.yyyy HH:mm:ss K'` → `Montag, 14.09.2026 22:56:33 +02:00`). **Inhaltlich nichts ändern.** Ergänze nur einen Verweis-Halbsatz im „Konsequenz"-Block, dass der Befehl seit 14.09.2026 als Schritt 0 im Session-Update verankert ist ([[feedback_session_update]]). Sollte deine eigene Messung abweichen (z. B. `+0000` beim baren `date`): **nichts umschreiben, sondern abbrechen und Levi melden** — dann hat sich die Sandbox geändert und das ist eine eigene Entscheidung.
5. **Frontmatter pflegen:** In `feedback_session_update.md` und `feedback_session_update_dax.md` die `description` um einen Halbsatz erweitern („… vorgeschalteter Schritt 0 (Zeitanker Session, Pflicht-`date` als allererster Call, T0/T6 als verbindliche Referenzzeit) seit 14.09.2026 …") und `metadata.modified` aktualisieren. In `feedback_memory_pflege.md` die `description` um die neue Kategorie ergänzen. **Die drei Stellen im Fließtext, die „alle 6 Schritte"/„jeder der 6 Schritte" sagen (Zeilen ~17, ~19 in `feedback_session_update.md`), bekommen jeweils den Zusatz „nach dem vorgeschalteten Schritt 0" — der Zahlenname 6 bleibt.**
6. **`MEMORY.md`:** Indexzeile von [Session Update Ablauf] um „+ Schritt 0 Zeitanker" ergänzen. Die Indexzeile für [[project_fable_auftrag_schritt0_datumsverifikation_2026-09-14]] und [[feedback_datum_verifikation_fehler_2026-09-14]] hat Opus bereits gesetzt — nicht doppeln, nur prüfen.

---

## Tests / Abnahme (alle fünf ausführen und Ergebnis berichten)

- **T1 — Befehle real ausführen, nicht abschreiben.** Beide Varianten (Bash + PowerShell) laufen lassen, Ausgaben in die Rückmeldung kopieren. Prüfen: Offset `+0200`/`+02:00`, UTC-Zeile exakt 2 h zurück, Wochentag plausibel. Zusätzlich `TZ="Europe/Berlin" date` einmal laufen lassen und bestätigen, dass es weiterhin falsch (UTC) liefert — wenn nicht, ist der TZ-Abschnitt in [[feedback_zeitzone]] veraltet und du meldest das, statt ihn eigenmächtig umzuschreiben.
- **T2 — Trockenlauf der Kopfzeile.** Erzeuge aus der T1-Ausgabe die fertige `Zeitanker T0: …`-Zeile exakt im vorgeschriebenen Format und zeig sie in der Rückmeldung. Wenn das Format aus deiner eigenen Ausgabe nicht 1:1 befüllbar ist, ist das Format falsch spezifiziert → korrigiere es im Regeltext und sag es dazu.
- **T3 — Reihenfolge-Check im Dokument.** `grep -n "Schritt 0\|Schritt 1 — Memory" memory/feedback_session_update.md` — Schritt 0 muss vor Schritt 1 stehen und unter `## Der 6-Schritt-Ablauf` hängen.
- **T4 — Querverweise prüfen.** Alle in den geänderten Dateien neu gesetzten `[[…]]`-Links auflösen (Zieldatei existiert?). Zusätzlich `grep -rn "6-Schritt\|alle 6 Schritte" memory/` — es darf keine Stelle geben, die jetzt inhaltlich falsch ist.
- **T5 — Regressions-Sanity.** `node --test tests/` (bzw. der im Projekt übliche Aufruf) muss unverändert grün bleiben (Stand zuletzt 60/60). Es ändert sich kein Code — ein roter Test wäre ein Zeichen, dass du versehentlich mehr angefasst hast als beauftragt.

## Nicht-Ziele (ausdrücklich)

- Kein neues Skript, kein neuer CLI-Parameter, kein Guard, keine `vollcheck.cjs`-Erweiterung.
- Keine wiederholten Datums-Checks im laufenden Loop — Punkt 9a deckt den Loop bereits ab, T0 + T6 decken das Session-Update ab. Mehr wäre Overhead.
- Punkt 7e wird **nicht** umgeschrieben, nur um einen Verweissatz ergänzt.
- Kein `TZ=`-Workaround, keine Umstellung auf Node als Standardweg (nur als dokumentierter Fallback).
- Der Name „6-Schritt-Ablauf" bleibt.

## Abschluss

Commit wie im Projekt üblich (aussagekräftige deutsche Commit-Message, Bezug auf den Vorfall 14.09.2026), **nicht pushen**. In der Rückmeldung an Levi: welche Dateien geändert wurden (mit Zeilenbezug), die T1-Rohausgaben, die fertige T2-Kopfzeile, und ob dir beim Lesen eine weitere Stelle im Regelwerk aufgefallen ist, an der eine Zeitaussage ohne Beleg entstehen kann (nur melden, nicht ungefragt mitändern — [[feedback_dont_change_running_system]]).

--- ENDE DES FABLE-PROMPTS ---
