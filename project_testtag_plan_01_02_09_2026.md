---
name: project-testtag-plan-01-02-09-2026
description: "Fertige, sofort ausführbare Standing Instructions für die zwei unbeaufsichtigten fiktiven Testtage 01.09. und 02.09.2026 (Levi beide Tage unterwegs, keine Rückfrage möglich). AUSLÖSER unten lesen, falls Levi morgens ein Go gibt."
metadata: 
  node_type: memory
  type: project
  modified: 2026-09-03T10:47:11.638Z
  originSessionId: e40d2c87-d383-4377-a725-334d9fa865f3
---

## AUSLÖSER — zuerst lesen, wenn diese Datei relevant sein könnte

Sagt Levi morgens (voraussichtlich 01.09.2026) sinngemäß **"Go Testtag 01.09"** (oder eine erkennbar gleichbedeutende Formulierung wie "Go für die zwei Testtage" / "leg los mit dem Testtag-Plan"), gilt das als das vorab vereinbarte Startsignal für den kompletten unten stehenden Plan. Levi hat diesen Plan bereits inhaltlich freigegeben (Session vom 31.08.2026 Abend) — am Morgen selbst muss NICHTS mehr inhaltlich verhandelt werden, nur ausgeführt.

**Bei Eingang des Go-Signals, in dieser Reihenfolge:**
1. Aktuelle Uhrzeit (DE-Ortszeit) bestimmen.
2. **CronCreate für 01.09.2026 14:45 Uhr** (heute, falls das Go vor 14:45 kommt) mit dem kompletten Abschnitt "TAG 1" unten als Prompt aufsetzen. **Ausnahme:** Kommt das Go bereits NACH 14:45 Uhr — den Abschnitt "TAG 1" SOFORT selbst beginnen (kein Cron nötig, direkt loslegen), Rest des Ablaufs unverändert.
3. **CronCreate für 02.09.2026 14:45 Uhr** mit dem kompletten Abschnitt "TAG 2" unten als Prompt aufsetzen — BEIDES in einem Zug, da die Session laut Levis Vorgabe durchgehend offen bleibt (kein Neustart möglich, während er unterwegs ist) und beide Cron-Jobs nur session-gebunden existieren (kein Disk-Persistenz — geht das Terminal zwischendurch doch zu, verfallen beide Jobs ersatzlos und müssten neu aufgesetzt werden).
4. Kurz an Levi bestätigen: beide Jobs stehen, mit Uhrzeit/Datum, und dass das Terminal jetzt bis mindestens 02.09. 22:00 Uhr durchgehend offen bleiben muss.

Falls die Datei aus einem anderen Anlass gelesen wird (z.B. Levi fragt "was war nochmal der Plan für die Testtage") — einfach den Inhalt erklären, nicht automatisch starten.

---

## Hintergrund (für Levi UND für eine komplett kontextlose Session gleichermaßen verständlich)

Zwei fiktive NAS100-Solo-Testtage, während Levi unterwegs und nicht erreichbar ist. Ziel: nicht "ein Trade muss zustande kommen", sondern dass der komplette 7b1-Ablauf bei jedem Dual-Gate-2/2-Moment sauber und mit maschinell belegten Leveln durchläuft — das neue Session-Levelregister (`scripts/level_register.json`, fertiggestellt und zweifach Opus-verifiziert am 31.08.2026 Abend) soll genau die Lücke schließen, die am 31.08.2026 zu einer fälschlichen UNKNOWN-Ablehnung führte (ein real vorhandenes Level wurde im Trigger-Moment schlicht nicht gefunden).

---

## TAG 1 — Standing Instruction für den CronCreate-Prompt (01.09.2026, 14:45 Uhr)

FIKTIVER Testtag 01.09.2026, Solo-Loop bis 22:00 Uhr (Levi unterwegs, nicht im Chat, keine Rückfrage möglich). ZUERST vollständiges "start update dich" durchführen (voller 6-Schritt-Ablauf, siehe [[feedback_session_update]]) — dabei explizit prüfen, ob heute (erster Handelstag des Monats) ISM Manufacturing PMI ansteht (üblicher Slot 16:00 DE-Zeit) und als Blackout-relevantes Ereignis vermerken falls ja; Chart-Zustand prüfen (Indikatoren-Set komplett, RVOL nach ggf. nächtlichem TV-Neustart neu hinzufügen falls fehlt); offene Positionen checken (sollte keine geben); Pivots (7a1a) berechnen UND per `draw_shape` einzeichnen; Tweet-Watchlist-Check; `x_last_fetch.json` Ausgangsstand lesen. **Schritt 6 (Erhebungs-Fahrplan) ist PFLICHT und schreibt `scripts/level_register.json` vollständig — inkl. der Mindestbelegungs-Regel (Pivots/PDH-PDL/Session-Extrema/Rundzahlen dürfen dort nie fehlen).** Alles wie gewohnt protokollieren.

DANACH ab 15:00 Uhr: CronCreate für einen minütlichen Loop (`*/1 * * * *`) aufsetzen, der diese komplette Standing Instruction als Prompt bekommt, bis 22:00 Uhr. Jeden Fire als neuen Abschnitt in `memory/testtag/testtag_2026-09-01.md` protokollieren (anhängen, nicht überschreiben). KEIN echter Trade — nie `trades.db` beschreiben, `add_trade.cjs` nur mit `--dry-run`.

**ORDER-SPERRE 15:00–15:30 Uhr (ergänzt 01.09.2026 08:xx, Levi vor Abfahrt):** Ab 15:00 Uhr läuft der volle Loop inkl. Voll-Checks/Quick-Ticks/Setup-Suche wie gewohnt — aber JEDE Order-Ausführung (7b1-5-Schritte-Ablauf bis zum tatsächlichen Trade) ist bis 15:30 Uhr gesperrt, auch bei vollständigem Dual-Gate (2/2). Bei einem 2/2-Trigger vor 15:30 Uhr: Protokollzeile "Trigger erkannt, Order-Ausführung wegen 15:00–15:30-Sperre ausgesetzt" — kein `cooldown_check.cjs`/`gate_check.cjs`-Aufruf. Ab 15:30 Uhr gilt der normale Ablauf ohne Einschränkung.

### ZIEL DES TAGES

NICHT: dass ein Trade zustande kommt. SONDERN: dass JEDER Moment mit vollständigem Dual-Gate (2/2) den kompletten 7b1-5-Schritte-Ablauf durchläuft und dabei JEDE Gate-Eingabe mit benannter Quelle belegt ist. Ein Tag mit 0 Trades und sauber belegten Ablehnungen ist ein ERFOLGREICHER Testtag. Ein Trade, dessen Gate-Eingaben nicht sauber belegt sind, ist ein GESCHEITERTER Testtag — auch bei fiktivem Gewinn. Kein Druck, heute einen Trade zu produzieren.

### LEVELSUCHE TP1/TP2 — Session-Levelregister (ersetzt die frühere manuelle Checkliste)

Levelsuche findet NICHT mehr im Trigger-Moment statt. Vor jedem `gate_check.cjs`-Aufruf: `scripts/level_register.json` lesen (beim Session-Update/letzten Voll-Check geschrieben) und die Pflichtzeile "Levelsuche TP1/TP2: ..." mit allen 8 Levelarten ausgeben (siehe [[feedback_live_trading]] 7b1 Schritt 4b). "—" heißt ausschließlich "kein Register-Eintrag dieser Levelart" — bei Pivots/PDH-PDL/Session-Extrema/Rundzahlen ist "—" NIEMALS legitim (Mindestbelegungs-Regel, [[feedback_session_update]] Schritt 6); erscheint dort trotzdem ein "—", das Register VOR dem Entry vervollständigen, nicht durchwinken. `gate_check.cjs` verifiziert die gewählten TP-Preise maschinell gegen das Register (±2,0 Pkt Toleranz).

**Preisformat-Falle (seit 31.08.2026 Abend scharf geschaltet):** An `gate_check.cjs` gehen alle Preis-/Zahlenwerte IMMER als Punkt-Dezimal ohne Tausendertrennzeichen (`29200.40`), NIEMALS im deutschen Anzeigeformat (`29.200,40`) — Letzteres bricht mit Exit-Code 1 ab. Das deutsche Format bleibt für Chart-Anzeige/Protokollzeilen korrekt, nur die CLI-Übergabe muss umgewandelt werden.

### EXIT-CODE-1-KLAUSEL (erweitert 31.08.2026 Abend nach den Preisformat-/Register-Fixes)

Exit-Code 1 von `gate_check.cjs` hat jetzt drei mögliche Ursachen:
1. **Fehlendes A3-Pflicht-Messfeld ohne `--grund-<feld>`** → sofort korrigieren (Feld nachtragen ODER `--grund-<feld> "..."` ergänzen) und im selben Trigger-Moment erneut ausführen.
2. **Preis im deutschen Format übergeben** → auf Punkt-Dezimal ohne Tausenderpunkt umwandeln und im selben Trigger-Moment erneut ausführen.
3. **`level_register.json` fehlt/unlesbar** → das ist KEIN im Trigger-Moment behebbares CLI-Problem, sondern ein Session-Update-Prozessfehler (Schritt 6 wurde nicht/fehlerhaft ausgeführt). In diesem Fall NICHT versuchen, den Trigger-Moment künstlich zu retten — als Protokollbruch dokumentieren, Register schnellstmöglich (regulärer Weg: erneuter Schritt-6-Durchlauf) nachziehen, dieser Trigger-Moment bleibt ohne Order.

Bei Ursache 1/2 gilt weiterhin: "Aufruf korrigieren und sofort erneut ausführen" — NICHT vorschnell "Setup ablehnen, kein Trade". Immer die Bash-Variante verwenden:
```
node scripts/gate_check.cjs <Parameter> > scripts/last_gate_check.txt 2>&1; echo "Exit-Code: $?"
```
In PowerShell identisch mit `2>&1` im Redirect (seit 31.08.2026 Abend Pflicht, sonst bleibt die Datei bei Exit 1 leer) und `Write-Output "Exit-Code: $LASTEXITCODE"` statt der `echo`-Zeile. Nie `$LASTEXITCODE` ohne das `2>&1`, um Verwechslungen unter Zeitdruck zu vermeiden.

### TERMINALBEDINGUNG OFFENE POSITION (Solo-Mandat-Ergänzung, da Levi nicht erreichbar)

Ist um 21:45 Uhr eine fiktive Position offen: Time-Boxed-Exit zum dann aktuellen Marktpreis, protokolliert als regulärer manueller Exit (Ereignis-AVWAP-Reset, `add_trade.cjs --dry-run` wie gewohnt). Diese Regel gilt als vorab erteilte Freigabe — keine Rückfrage an Levi nötig oder möglich.

### STANDARD-ABLAUF (wie an den bisherigen fiktiven Testtagen)

Minute % 5 bestimmen. != 0 → Quick-Tick (eine kompakte Zeile). == 0 → Voll-Check (MTF-Vierschritt 1H/15min/5min/QQQ inkl. Pflichtzeile (11b) QQQ-EMA50-Lage, Regime-Gate 8d mit Frische-Zeitstempel, ADX GEMESSEN-KEIN-GATE, Level-Ping-Pong-Pflichtzeile 8a5 ab dem 3. Reclaim-Zyklus, Tweet-Check auf dem 10-Minuten-Raster, `level_register.json`-Update-Pflicht (Punkt 9), Screenshot, Format-Zeile). Bei jedem Trigger-Moment (frischer bestätigter Kerzenschluss-Cross ODER Widerlegungs-Gegenschluss ODER Level-Bruch): Spike-Ausnahme/Basis-Reclaim-Klasse/Stale-Check (7b1c, harte Zwei-Kriterien-Vorbedingung).

Bei jedem vollständigen Dual-Gate (2/2): voller 7b1-5-Schritte-Ablauf inkl. Levelsuche-Pflichtzeile (Register), `cooldown_check.cjs` und `gate_check.cjs` wörtlich mit Exit-Code ins Protokoll, Chop-Check, 9b-Divergenz-Check, Q-Score-Suffix. Bei Order-Ausführung: Entry-AVWAP-Anker (Ereignis-Instanz). Bei Positionsschluss: Ereignis-AVWAP-Reset + `add_trade.cjs --dry-run`.

Protokollbruch-Offenlegung bei jeder Lücke/jedem Doppel-Fire (nie rekonstruieren). Nachhol-Regel für ausgefallene Voll-Check-Slots (Nr. X (nachgeholt, +N Min), nur bei ≤2 Min bis zum nächsten Slot entfällt er ganz; Quick-Ticks werden nie nachgeholt).

### FAKTENPROTOKOLL-ABSCHLUSS (ab Uhrzeit >= 22:00 Uhr)

6 Abschnitte wie gewohnt (Zahlenbilanz, Ereignisliste, Skript-Aufrufe wörtlich, Brüche/Unterlassungen, Offene/unklare Punkte, Pflicht-Abschlusszeilen), PLUS zwei Pflichtzeilen in Abschnitt 1:
- Anzahl Momente mit Dual-Gate 2/2: __
- `gate_check.cjs` GESAMTSTATUS-Verteilung: PASS __ / FAIL __ / UNKNOWN __ / **Exit-1-Abbrüche __ (je mit Ursache 1/2/3 aus der Exit-Code-1-Klausel)** (bei jedem UNKNOWN: welches Gate, welche Eingabe fehlte, was zeigte die Levelsuche-Pflichtzeile für diese Levelart)

Fester Schlusssatz wie gewohnt. Status im Frontmatter auf "completed" setzen.

### NACH DEM FAKTENPROTOKOLL-ABSCHLUSS

1. CronDelete auf den heutigen Loop-Job.
2. Starte einen NEUEN, unabhängigen Subagenten (Agent-Tool, model: "opus", explizit KEIN "fork" — kalt lesen, ohne Kontext dieser Loop-Session) mit dem Auftrag: `memory/testtag/testtag_2026-09-01.md` vollständig lesen und eine Analyse erstellen, im Stil/Umfang der Opus-Analyse vom 31.08.2026 (kritisch, eigenständig, mit konkreter Punch-Liste falls Verbesserungsbedarf) — inkl. expliziter Bewertung, ob das Session-Levelregister die 31.08.-Lücke tatsächlich geschlossen hat oder ob am 01.09. eine neue, unvorhergesehene Lücke sichtbar wurde.
3. Die Analyse als ENTWURF speichern unter `memory/project_testtag_analyse_2026-09-01.md`, Frontmatter `status: draft`. KEINE Regeländerung, KEIN MEMORY.md-Index-Eintrag, KEIN Fable-Auftrag — das bespricht Levi am 03.09. persönlich.
4. NICHT weitermachen, NICHT eigenständig in einen neuen Loop gehen. Der nächste Fire ist der bereits separat aufgesetzte 02.09.-Cronjob (siehe TAG 2 unten) — der läuft unabhängig vom eigenen Zutun.

---

## TAG 2 — Standing Instruction für den CronCreate-Prompt (02.09.2026, 14:45 Uhr)

FIKTIVER Testtag 02.09.2026, Solo-Loop bis 22:00 Uhr (Levi weiterhin unterwegs, keine Rückfrage möglich).

### ABBRUCH-GATE (zwingend VOR dem "start update dich" zu prüfen)

Lies zuerst `memory/project_testtag_analyse_2026-09-01.md` (die Opus-Entwurfsanalyse von Tag 1). Existiert die Datei nicht: Tag 1 ist komplett ausgefallen — das zählt selbst bereits als Abbruch-Kriterium. Prüfe sonst anhand dieser Datei und `memory/testtag/testtag_2026-09-01.md`:

(a) Endete der Loop am 01.09. vorzeitig/unvollständig (kein Faktenprotokoll-Abschluss, Status nicht "completed")?
(b) Waren mehr als 10% der laut Formel fälligen Voll-Checks ausgefallen (nicht nachgeholt)?
(c) Wurde ein 2/2-Dual-Gate-Moment erneut mit GESAMTSTATUS UNKNOWN abgelehnt, OBWOHL das Session-Levelregister nachweislich korrekt geschrieben und die Levelsuche-Pflichtzeile vollständig befolgt wurde — d.h. ein NEUER, durch das Register nicht behobener struktureller Fehler?
(d) Trat an Tag 1 wiederholt (≥2×) Exit-Code 1 Ursache 3 auf (Register fehlte/unlesbar im Trigger-Moment) — d.h. Schritt 6 wurde strukturell nicht zuverlässig ausgeführt?

Trifft (a), (b), (c) ODER (d) zu: Heutiger Tag läuft NUR im BEOBACHTUNGSMODUS — voller Loop, volle Voll-Check-Protokolle, aber KEIN 7b1-Order-Ablauf, auch nicht bei vollständigem Dual-Gate (2/2). Bei jedem 2/2-Trigger nur die Protokollzeile "Trigger erkannt, Order-Prüfung wegen Abbruch-Gate [Kriterium a/b/c/d] ausgesetzt" — kein `cooldown_check.cjs`/`gate_check.cjs`-Aufruf. Trifft keines zu: normaler Ablauf wie am 01.09.

In BEIDEN Fällen: das Abbruch-Gate-Ergebnis (ausgelöst ja/nein, welches Kriterium, kurze Begründung) als allererste Zeile in `memory/testtag/testtag_2026-09-02.md` schreiben, vor dem eigentlichen Session-Update.

### START UND ABLAUF (identisch zu Tag 1, hier nur referenziert)

Danach vollständiges "start update dich" wie an Tag 1 (voller 6-Schritt-Ablauf inkl. Erhebungs-Fahrplan/Register-Schreibpflicht in Schritt 6). Ab 15:00 Uhr Loop bis 22:00 Uhr, Protokoll in `memory/testtag/testtag_2026-09-02.md`.

Alle inhaltlichen Regeln IDENTISCH zu Tag 1 (siehe Abschnitt "TAG 1" oben, hier nicht wiederholt): ZIEL DES TAGES (kein Erfolgsdruck für einen Trade — außer im Beobachtungsmodus, dann ist ohnehin kein Trade möglich), Levelsuche über das Session-Levelregister (nur relevant wenn nicht im Beobachtungsmodus), Preisformat-Falle, EXIT-CODE-1-KLAUSEL (alle 3 Ursachen), TERMINALBEDINGUNG OFFENE POSITION um 21:45 Uhr (nur relevant wenn nicht im Beobachtungsmodus), Standard-MTF-Voll-Check-Format inkl. (11b) und 8a5, Protokollbruch-Offenlegung, Nachhol-Regel.

### FAKTENPROTOKOLL-ABSCHLUSS (ab Uhrzeit >= 22:00 Uhr)

Wie Tag 1 (6 Abschnitte + die zwei Pflichtzeilen zu Dual-Gate-2/2-Momenten und GESAMTSTATUS/Exit-1-Verteilung). Zusätzlich: Vermerk, ob der Tag im Beobachtungsmodus lief und warum (welches Kriterium a/b/c/d). Status im Frontmatter auf "completed" setzen.

### NACH DEM FAKTENPROTOKOLL-ABSCHLUSS

1. CronDelete auf den heutigen Loop-Job.
2. Starte einen NEUEN, unabhängigen Subagenten (Agent-Tool, model: "opus", explizit KEIN "fork") mit dem Auftrag: `memory/testtag/testtag_2026-09-02.md` UND `memory/project_testtag_analyse_2026-09-01.md` (Vortagesanalyse) vollständig lesen und eine Analyse für den 02.09. erstellen — inkl. Vergleich zu Tag 1 (wurde ein am 01.09. gefundenes Problem am 02.09. wiederholt? Griff das Abbruch-Gate korrekt, falls ausgelöst?).
3. Die Analyse als ENTWURF speichern unter `memory/project_testtag_analyse_2026-09-02.md`, Frontmatter `status: draft`. KEINE Regeländerung, KEIN MEMORY.md-Index-Eintrag, KEIN Fable-Auftrag.
4. Danach NICHT weitermachen, NICHT eigenständig einen neuen Loop starten. Der Loop für diese beiden fiktiven Testtage ist damit beendet. Levi bespricht beide Analysen am 03.09. persönlich und geht die Ergebnisse dann mit Fable durch.

---

## ABBRUCH-GATE v2 — verbindlich für ALLE künftigen Testtage (ergänzt 03.09.2026, Fable-Finalentscheidung #12 nach Opus-Gegencheck, siehe [[project_fable_finalentscheidung_2026-09-03]])

**Die Fassung der Kriterien (c)/(d) im Tag-2-Abschnitt oben ist damit HISTORISCH** (sie galt für den 02.09. und bleibt dort unverändert dokumentiert). Jeder künftige Testtag-Plan übernimmt die Kriterien aus DIESEM Abschnitt. Dringlichkeitsgrund: Die am 03.09. umgesetzten Skript-Fixes #1/#2/#20 schaffen neue UNKNOWN- und Exit-1-Pfade, die im alten Wortlaut von (c)/(d) landen — ein einzelner vergessener `--cluster-level`-Parameter an einem 2/2-Moment hätte buchstabengetreu Kriterium (c) erfüllt und den Folgetag in den Beobachtungsmodus geschickt, wegen eines fehlenden Kommandozeilenparameters.

**(a) und (b): unverändert** (vorzeitiges/unvollständiges Ende ohne Faktenprotokoll-Abschluss; >10 % der fälligen Voll-Checks ausgefallen und nicht nachgeholt).

**(c) NEU:** Wurde ein 2/2-Dual-Gate-Moment mit GESAMTSTATUS UNKNOWN abgelehnt, OBWOHL das Session-Levelregister nachweislich korrekt geschrieben und die Levelsuche-Pflichtzeile vollständig befolgt wurde, UND stammt das UNKNOWN aus einem **strukturellen Verifikationsfehler** (ein korrekt registriertes Level wurde vom Registerabgleich nicht getroffen — die 31.08.-Fehlerklasse, die das Register beheben sollte)? **Klarstellung (i):** Ein UNKNOWN, das allein aus einer fehlenden oder per `--grund-<feld>` ersetzten Messfeld-Angabe stammt (z.B. `--cluster-level` mit Grund weggelassen → 8c2 UNKNOWN), ist ein **Dokumentationsmangel des Aufrufs, KEIN struktureller Fehler** — es zählt für (c) NICHT, wird aber im Faktenprotokoll (UNKNOWN-Aufschlüsselung) ausgewiesen.

**(d) NEU:** Trat wiederholt (≥2×) ein **registerbezogener Exit-Code 1** auf? Registerbezogen sind: Register fehlt/unlesbar (bisherige Ursache 3), `updated`-Zeitstempel in der Zukunft, Registeralter über der harten 90-Min-Obergrenze (beide Punkt-2-Fix 03.09.2026). **Klarstellung (ii):** Ein Exit 1 — gleich welcher Ursache —, der nach der EXIT-CODE-1-KLAUSEL sofort korrigiert und **im selben Trigger-Moment erfolgreich wiederholt** wurde, zählt für (d) NICHT; er erscheint nur in der Faktenprotokoll-Bilanzzeile "Exit-1-Abbrüche". **Klarstellung (iii):** Nicht-registerbezogene Exit-1-Fälle (fehlendes A3-Messfeld, Preisformat) zählen für (d) generell nicht — sie sind im Trigger-Moment behebbare CLI-Fehler, keine Session-Strukturfehler.

**(e) NEU — Prüfer-Feld (stille Defekte):** Die Opus-Vortagesanalyse trägt ab jetzt ein **Pflichtfeld in ihrem Kopf- oder Schlussabschnitt**: `Abbruch-Empfehlung: KEIN EINWAND / EINGESCHRÄNKT (<konkret defekter Mechanismus>) / ABBRUCH (<Begründung>)`. Das Abbruch-Gate übernimmt es 1:1 maschinell, ohne eigene Interpretation der Analyse:
- **ABBRUCH** → Beobachtungsmodus (wie bisher bei (a)-(d)).
- **EINGESCHRÄNKT** → Tag läuft normal, ABER der benannte Mechanismus muss VOR dem ersten 7b1-Order-Ablauf einmal nachweislich funktionieren (Testlauf/Protokollbeleg, als eigene Zeile im Protokoll) — die mittlere Antwort verhindert den Alarmanlagen-Effekt eines rein binären Felds (ein Gate, dessen einzige Reaktion maximal ist, wird zurückhaltend ausgelöst).
- **KEIN EINWAND** → normaler Ablauf.
- **Fail-Closed:** Fehlt das Feld, ist es unklar formuliert ODER existiert die Analyse-Datei gar nicht → wie ABBRUCH behandeln. ("Analyse fehlt" war schon in v1 selbst ein Abbruch-Kriterium — diese Logik wird hier verallgemeinert.)
- **Geltungsbereich:** (e) gilt NUR für Testtage OHNE frisches Levi-Go zwischen Vortagesanalyse und Tagesstart (Cron-Folgetage). Liegt ein frisches Go vor, ersetzt Levis Kenntnisnahme der Analyse das Kriterium — der Mensch ist das Gate für Tag 1, die Maschine für Folgetage (Opus-Option D, übernommen).

**Prüfer-Auftrag im selben Zug geändert (zwingende Folgeänderung — sonst widerspricht das Feld der Rolleninstruktion):** Jeder künftige Analyse-Auftrag an die Opus-Instanz (Schritt 2 im Abschnitt "NACH DEM FAKTENPROTOKOLL-ABSCHLUSS") enthält ab jetzt den Satz: *"Zusätzlich PFLICHT: eine Zeile `Abbruch-Empfehlung: KEIN EINWAND / EINGESCHRÄNKT (<Mechanismus>) / ABBRUCH (<Begründung>)` — dieses eine Urteil ist ausdrücklich Teil deiner Zuständigkeit und kein Verstoß gegen die Regel, keine Lösungsvorschläge zu machen; ein stiller Defekt (nachweislich falsche Faktenprotokoll-Aussage, still deaktiviertes Hard-Gate, korrumpierte Statusdatei) rechtfertigt mindestens EINGESCHRÄNKT."* Die bisherigen Analysen formulierten das Gegenteil ("ich schlage nichts vor") — ohne diese Auftragsänderung würde jeder künftige Prüfer das Feld weglassen und Fail-Closed bei jedem Lauf greifen.

**Warum Prüfer-Feld statt Loop-Selbstprüfung (Option A abgelehnt, mit korrigierter Begründung):** Fables ursprüngliches Argument ("stille Defekte sind vom Ausführenden nicht erkennbar") trägt für das Gate nicht — beim Abbruch-Gate LIEST der Loop eine fremde, fertige Analyse, das ist eine Lese-, keine Detektionsaufgabe (Opus-Einwand, akzeptiert). Option A scheitert stattdessen an der Auslegungsbreite von "stiller Defekt" (welcher Befund ist gravierend genug?) — ein Ermessensproblem, das das binäre/dreiwertige Prüfer-Feld beseitigt, indem der Prüfer das Ermessen ausübt, der die Sachlage kennt.

**Register-Zeitstempel-Konvention für künftige (auch fiktive) Testtage (Folge aus Finalentscheidung #22):** `level_register.json` wird IMMER mit echter UTC geschrieben (`updated` = Zeit des gerade ABGESCHLOSSENEN Voll-Checks), nie mit Simulationsdatum oder DE-Ortszeit-mit-Z — seit dem Punkt-2-Fix bricht sonst jeder Gate-Aufruf hart ab (Zukunfts-Zeitstempel bzw. 90-Min-Obergrenze). Details: [[feedback_session_update]] Schritt 6.
