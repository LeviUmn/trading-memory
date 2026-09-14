---
name: project-fable-umsetzung-2026-09-03
description: "Fable-Umsetzungsbericht zur Auftragsliste 03.09.2026 (25 Punkte aus den Testtag-Analysen 01./02.09.): 18 umgesetzt, 2 abweichend/präzisiert umgesetzt, 0 zurückgestellt, 5 Entscheidungsvorlagen (Regelwerk-Fragen #3/#11/#12/#21/#22). Nichts committet — Levi sichtet zuerst."
metadata:
  node_type: memory
  type: project
  status: draft
  originSessionId: e1a9edc1-4a4d-4c86-b03e-98d36d93627e
  modified: 2026-09-03T09:46:09.207Z
---

# Fable-Umsetzung der Auftragsliste 03.09.2026

**Quelle:** [[project_fable_auftragsliste_2026-09-03]] (25 Punkte). Levis Auftrag: alles umsetzen, dabei selbst prüfen, ob es so passt. **Nichts ist committet** — alle Änderungen liegen ungesichtet im Arbeitsverzeichnis (Repo: `scripts/` + 2 neue Skripte; Regelwerk: 8 memory-Dateien).

**Bilanz: 18 umgesetzt | 2 abweichend/präzisiert umgesetzt (#1, #8) | 0 zurückgestellt | 5 Entscheidungsvorlagen (#3, #11, #12, #21, #22).**

Jeder Befund wurde vor der Umsetzung gegen den tatsächlichen Code bzw. die Quellanalysen geprüft; alle 25 Befunde haben sich bestätigt (u.a. Zeile-995-Verhalten von 8c2 reproduziert, Q2-Budget-Formel empirisch gegen VC#40 vom 01.09. verifiziert: (1,5−4,53)/1,5×100 = −202 ✓).

---

## SKRIPT-FIX (7 Punkte)

**#1 — `--cluster-level` darf 8c2 nicht mehr still abschalten: ABWEICHEND/PRÄZISIERT UMGESETZT** (`scripts/gate_check.cjs`). Befund verifiziert (stiller PASS bei fehlendem Parameter; zusätzlich gefunden und mitgefixt: der Fall "Zone angegeben, aber ATR fehlt" lief ebenfalls still auf PASS mit falscher Detailzeile). Neue Semantik (nur live, Batch unverändert): `--cluster-level` steht auf der A3-Pflichtliste (fehlt es ohne Grund → Exit 1); fehlt es mit `--grund-cluster-level "..."` → 8c2 = UNKNOWN (pass:null) mit Grund im Output → GESAMTSTATUS UNKNOWN. **Abweichung von der Liste:** zusätzlich der explizite Wert `--cluster-level none` (auch `keine`) = "geprüft, KEINE Wick-Zone in dieser Session vorhanden" → PASS mit Kennzeichnung. Grund: Die Liste verlangt UNKNOWN bei fehlendem Parameter — ohne den none-Weg wäre an jedem Tag ohne Wick-getestete Zone (Normalfall früh in der Session) PASS strukturell unerreichbar und jeder Trade blockiert. `none` erhält die geforderte Unterscheidbarkeit ("geprüft, keine Zone" ≠ "nicht geprüft") ohne diesen Kollateralschaden. Templates in 7b1 entsprechend ergänzt. Getestet: alle vier Pfade (Preis/none/Grund/fehlend).

**#2 — Frische-Guard Levelregister: UMGESETZT** (`scripts/gate_check.cjs`). Negatives Registeralter (< −1 Min) → Hard-Exit 1 mit Ursachen-Hinweis (DE-Zeit-mit-Z / Simulationsdatum) statt bloßem Druck der Zahl. Zusätzlich harte Obergrenze `LEVEL_REGISTER_HARD_MAX_AGE_MINUTES` → Hard-Exit 1; **Provisorium 1440 Min (24 h)**, weil der endgültige Wert laut Liste Regelfrage ist (siehe Entscheidungsvorlage #22 — ein Register älter als ein Handelstag ist unter jeder künftigen Auslegung defekt, das Provisorium greift der Entscheidung also nicht vor). 60-Min-Warnung unverändert. Getestet (Zukunfts- und 2-Tage-Register → Exit 1).

**#7 — Plausibilitätsguard `x_last_fetch.json`: UMGESETZT** (neu: `scripts/x_fetch_stamp.cjs`). Kein Skript schrieb die Datei bisher — sie wurde von Hand per echo/Write gesetzt; deshalb ein Guard-Helper als einziger Schreibweg: `--set <ISO-UTC>` erzwingt Z-Suffix-Format, lehnt Zukunfts-Zeitstempel (> jetzt+2 Min) und Rückwärtsläufe hart ab (Exit 1; Rückwärts-Korrektur eines korrupten Stands bewusst nur mit `--force`); `--check` liefert Delta + "fällig JA/NEIN" gerechnet (dient zugleich #17). Semantik "Wert = Zeitstempel des jüngsten gesehenen Tweets" im Kopfkommentar + [[feedback_session_update]] Schritt 2 verankert (Schreibweg dort auf den Helper umgestellt). Getestet (5 Pfade).

**#8 — Rundzahl-Kategorie entzirkularisieren: ABWEICHEND NUR OPTION B UMGESETZT** (`scripts/gate_check.cjs`). Von den zwei Listen-Optionen (unabhängiger Abrufweg definieren ODER ehrlich labeln) bewusst das Labeln: Rundzahlen SIND reine Arithmetik, ein "unabhängiger Abrufweg" existiert für sie prinzipiell nicht — jede Konstruktion wäre Scheinverifikation. Register-Treffer auf `levelart` ~ "Rundzahl" heißen jetzt in allen drei Ausgabestellen (TP1-Herkunftszeile, 8b1-Gate-Detail, TP2-Detail) "REGISTER-TREFFER, GENERIERTE KATEGORIE — KEINE unabhängige Verifikation" statt "VERIFIZIERT"; Gate-Semantik unverändert (Rundzahlen bleiben zulässige Levelart). Getestet.

**#9 — Q2-Budget-Formel: UMGESETZT** (`scripts/gate_check.cjs`). Formel verbindlich dokumentiert und empirisch verifiziert: `q2BudgetPct = (1,5 − impulsReifegradAtr) / 1,5 × 100` (VC#40 01.09.: Reifegrad 4,53 → −202 % ✓). Fehlt der Wert im Live-Aufruf, berechnet das Skript ihn selbst aus `--impuls-reifegrad-atr` (sichtbare Zeile "Q2-BUDGET SELBST BERECHNET"); sind beide übergeben, Konsistenz-WARNUNG bei >1 Prozentpunkt Abweichung. Die Ausnahme-Begründung "Formel nicht rekonstruierbar" ist damit gegenstandslos (auch in 7b1 vermerkt). Batch unverändert. Getestet.

**#10 — Gegenprüf-Helper: UMGESETZT** (neu: `scripts/quote_check.cjs`). `--protokoll <Datei> [--ref scripts/last_gate_check.txt|last_cooldown_check.txt]` prüft, ob der komplette Referenz-Output als zusammenhängender Block wörtlich im Protokoll steht (Normalisierung nur CR/Zeilenend-Whitespace); Exit 0/1 mit Report (erste Abweichung, nirgends vorkommende Zeilen, "einzeln vorhanden, aber nicht zusammenhängend" = redigiert). Getestet (wörtlich → JA; ohne MESSFELD-AUSNAHME-Zeilen → NEIN mit korrekter Fehlliste). Pflicht-Einbindung: 7b1 Ein-Fassungs-Regel + Tagesabschluss.

**#20 — Pflichtfeld `--entry-source`: UMGESETZT** (`scripts/gate_check.cjs`). Auf der A3-Pflichtliste (fehlt ohne `--grund-entry-source` → Exit 1); neue Pflicht-Ausgabezeile `Entry-Herkunft: …` (Kategorien: bestätigter Kerzenschluss / Registerlevel / Live-Kurs bei Gate-Aufruf); reine Doku, kein Gate; Batch ausgenommen. Templates in 7b1 ergänzt. Getestet.

**Hinweis zu #1/#20:** Beide erweitern die A3-Pflichtliste — ein Live-Aufruf nach altem Template bricht ab jetzt mit Exit 1 (gewollt; die EXIT-CODE-1-KLAUSEL "korrigieren und sofort erneut ausführen" gilt). Beide 7b1-Kommandotemplates wurden deshalb im selben Zug aktualisiert.

---

## PROMPT/PROZESS-ERGÄNZUNG (13 Punkte)

**#4 — Zeitanker: UMGESETZT.** [[feedback_live_trading]] Punkt 9a, neuer Abschnitt "Zeitanker-Pflicht bei JEDEM Fire" (4 Pflichten: `date` zuerst, sekundengenauer Stempel in jeder Überschrift, Minutenlogik/Nummer nur aus Realzeit — nie Fire-Zähler, Lücken sichtbar); [[feedback_vollcheck_format]]: Kopfzeile im Template auf HH:MM:SS umgestellt + Why-Abschnitt.

**#5 — "Wörtlich" heißt wörtlich: UMGESETZT.** [[feedback_live_trading]] 7b1, neuer Abschnitt "Ein-Fassungs-Regel + maschinelle Gegenprüfung": vollständiger Dateiinhalt inkl. aller Nebenzeilen, genau EINE Fassung pro Lauf, Kurzformen zwingend als "gekürzt" markiert, plus Pflicht-Lauf von `quote_check.cjs` direkt nach jedem Zitat mit `WOERTLICH: JA/NEIN`-Zeile im Protokoll (dritte reine Disziplin-Wiederholung wäre wirkungslos geblieben — deshalb maschinell flankiert, #10).

**#6 — Halbierungsfenster 15:30–16:00: UMGESETZT.** [[feedback_vollcheck_format]]: Format-Zeilen-Template + Abschnitt "Sperr-/Halbierungsfenster in der Format-Zeile" (Pflichtangabe im Fenster, explizite Ablauf-Markierung beim ersten Voll-Check ab 16:00); Querverweis in [[feedback_trading_zeitfenster]].

**#13 — Faktenprotokoll auszählen: UMGESETZT.** [[feedback_tagesabschluss]], Abschnitt "Auszähl-Pflicht für alle Bilanzzahlen": jede Zahl am fertigen Dokument ausgezählt, "lückenlos" nur mit Minutenraster-Gegenrechnung, sonst "X von Y erfasst".

**#14 — Voll-Check-Nummer in der Format-Zeile: UMGESETZT.** [[feedback_vollcheck_format]]: Format-Zeile verbindlich `Format: Fließtext ✓ | Voll-Check #<N> vollständig (MTF ✓ | …)`; ohne Nummer gilt die Pflichtzeile als fehlend.

**#15 — "NEUER bestätigter Schluss" nur mit Timestamp-Nachweis: UMGESETZT.** [[feedback_loop_ablauf_uebersicht]] Szenario 1 Schritt 2: Unix-Open-Timestamp je Ebene mitführen, Meldung nur bei Timestamp-Wechsel, Unzulässigkeit zu Nicht-Schlusszeiten (20:55-Beispiel).

**#16 — Terminalbedingung im 21:45-Voll-Check: UMGESETZT.** [[feedback_live_trading]] Punkt 15, neue Pflicht (d): Pflichtzeile im Voll-Check des Terminal-Zeitpunkts selbst (Fließtext UND Format-Zeile), nicht erst im Abschluss.

**#17 — Tweet-Fälligkeit rechnerisch: UMGESETZT.** [[feedback_vollcheck_format]]: "fällig" nur bei Raster-Slot UND Delta ≥10 Min (per `x_fetch_stamp.cjs --check` gerechnet); Über-Polling als Pflicht-Hypothese bei leeren `@DeItaone`-Antworten. Veralteter 21.08.-Mechanik-Absatz in [[feedback_loop_ablauf_uebersicht]] Schritt 6 als Historie markiert (dort standen sonst erneut widersprüchliche Fälligkeitsmechaniken nebeneinander — exakt der 24.08.-Befund).

**#18 — 2/2-Zustand vs. Trigger-Ereignis: UMGESETZT.** [[feedback_live_trading]] 7b1, Abschnitt "Begriffs- und Zählkonvention" (Definitionen, VC#7-Wortlaut vom 02.09. als Muster); getrennte Zählung + Begründungspflicht für verworfene Zustände in [[feedback_tagesabschluss]].

**#19 — Laufende Messgrößen: UMGESETZT.** [[feedback_vollcheck_format]]: neue Pflicht-Template-Zeile `Zählstände:` (8a5-Zyklus mit Bezugslevel, Kerzenzähler NAS100(5min)/QQQ(15min) fortlaufend) — macht die Messfeld-Ausnahme "nicht rückverfolgbar" strukturell unmöglich.

**#23 — Screenshot-Namenskonvention: UMGESETZT.** [[feedback_live_trading]] Punkt 9: EIN Schema pro Tag = der unveränderte `capture_screenshot`-Default-Name (reale UTC-Zeit); Verbot simulierter Zeiten und doppelter Endungen; Ausnahme nur mit realer Zeit, genau einem `.png` und Offenlegung.

**#24 — Zeitbasen-Konvention: UMGESETZT.** [[feedback_zeitzone]]: Protokolle DE-Ortszeit, Statusdateien UTC mit Z, jede Abweichung gekennzeichnet + DE-Umrechnung.

**#25 — Einheitliche Zählweise: UMGESETZT.** Teil des 7b1-Konventionsabschnitts: verbindlich 2-beinig (NAS100 15min + QQQ 15min, 1H-Override, 5min kein Bein), "x von 3" unzulässig.

---

## REGELWERK-FRAGE (5 Entscheidungsvorlagen — NICHTS davon umgesetzt)

### #3 — Definition 8d-Kriterium 2 (Makro-Häufung)
- **Option A (Zustand):** K2 gilt als erfüllt, sobald am Handelstag ≥2 Events der Standard-Blackout-Liste stattgefunden haben — fortbestehend bis Tagesende.
- **Option B (Ereignis mit Verfall):** K2 nur für ein Zeitfenster (z.B. 60-90 Min) nach dem 2. Event erfüllt, danach verfallen.
- **Option C (Tag-2-Praxis festschreiben):** = Option A plus die präzise Event-Definition, mit der Tag 2 fehlerfrei über 84 Checks lief: "≥2 Events von der Standard-Blackout-Liste am selben Tag; bloße Marktreaktionen/News/Nicht-US-Events zählen nicht."
- **Empfehlung: C.** Der Tag-1-Fehler war nicht die Zustandslogik, sondern die fehlende Festschreibung (stillschweigende Umdeutung #31-#40, gate-entscheidend für --tier). Tag 2 belegt, dass genau diese Formulierung konsistent durchhaltbar ist und an vier Stellen aktiv gegen Neuinterpretation verteidigt wurde. Ein Verfallsfenster (B) wäre eine NEUE, unkalibrierte Regel — 8d-K1 (Tagesrange) bildet nachlassende Wirkung ohnehin ab. Nach Entscheidung: Definition wörtlich in [[feedback_chartanalyse]] 8d + als feste Zeile in den Loop-Prompt.

### #11 — Formel 8d-Kriterium 3 (VIX)
Befund bestätigt: (Hoch−Tief)/Tief feuerte am 02.09. den ganzen Tag und wurde bei monoton FALLENDEM VIX (16,82→15,12) immer "deutlicher" erfüllt (5,52 %→11,24 %) — misst Entspannung als Schock.
- **Option A (Richtungsfilter, minimal-invasiv):** Bestehende Formel bleibt, K3 zählt nur, wenn zusätzlich der aktuelle VIX-Stand ≥ Vortagesschluss ODER näher am Tageshoch als am Tagestief liegt (ein fallender/beruhigter VIX kann K3 nie erfüllen).
- **Option B (Anstiegsmaß):** K3 = (aktueller VIX − Tagestief)/Tagestief ≥ Schwelle — misst nur den Aufwärtsanteil, braucht aber eine neu kalibrierte Schwelle.
- **Option C (Niveau + Delta):** K3 = VIX ≥ Absolutschwelle (z.B. 22) ODER Tagesanstieg ≥ x % ggü. Vortagesschluss — robusteste Semantik, größter Kalibrierungsaufwand.
- **Empfehlung: A.** Behebt exakt den belegten Fehlmodus (Feuern auf Entspannung) ohne neue Schwellenkalibrierung; die bestehende %-Schwelle bleibt vergleichbar mit der bisherigen Messreihe. B/C erst erwägen, wenn A in der Praxis weiter Fehlsignale liefert.

### #12 — Abbruch-Gate um stille Defekte erweitern
Befund bestätigt: Tag 1 hätte mit drei stillen Defekten (falsche Faktenprotokoll-Aussagen, still deaktiviertes 8c2, korrupte x_last_fetch) alle vier Kriterien (a)-(d) ohne Treffer passiert.
- **Option A (5. Kriterium mit Defektliste):** (e) = "Vortagesanalyse belegt ≥1 stillen Defekt: nachweislich falsche Faktenprotokoll-Aussage / still deaktiviertes Hard-Gate / korrumpierte Statusdatei" — der Loop prüft es selbst anhand der Analyse.
- **Option B (Prüfer-Feld):** Die Opus-Vortagesanalyse bekommt ein Pflichtfeld "Abbruch-Empfehlung: JA/NEIN (+Kriterium)", das Abbruch-Gate übernimmt es 1:1 maschinell.
- **Option C (unverändert):** Gate misst weiter nur harte, selbst prüfbare Kriterien.
- **Empfehlung: B.** Stille Defekte sind per Definition vom Ausführenden nicht erkennbar ("Der Loop kann Fehler finden, die er macht, aber nicht Fehler, die er beim Berichten macht") — nur der externe Prüfer sieht sie; Option A ließe den Loop die Analyse selbst interpretieren (neue Deutungsspielräume, exakt die Fehlerklasse, die das Gate verhindern soll). B ist eine Ein-Zeilen-Ergänzung im Analyse-Auftrag + eine Übernahme-Zeile im Gate. Beachte: Die Fixes #2/#7 dieser Runde machen zwei der drei Tag-1-Defektarten künftig ohnehin zu lauten Exit-1-Fehlern.

### #21 — Register: Herkunft vs. Tragfähigkeit
Befund bestätigt: TP1 = PDL 28.954,35 am 02.09. war "VERIFIZIERT" (Herkunft), obwohl das Tagestief 72 Pkt darunter lag — das Level war intraday durchhandelt.
- **Option A (Kennzeichnen):** `gate_check.cjs` prüft, ob das TP-Level innerhalb der heutigen Session-Range liegt bzw. durchhandelt wurde (Session-Extrema stehen im Register), und ergänzt die Herkunftszeile um "intraday bereits durchhandelt" — Anzeige, kein Gate.
- **Option B (Abwerten):** Durchhandeltes Level als TP1 → Gate UNKLAR, nur mit expliziter Zusatzbegründung freigebbar.
- **Option C (nichts ändern):** Tragfähigkeit bleibt Sache der Chartanalyse (8b1-Zonen prüfen die Distanz ohnehin).
- **Empfehlung: A.** n=1; ein Hard-Gate (B) wäre unkalibriert und würde legitime Retest-Setups blockieren (ein durchhandeltes Level kann als Magnet/Retest-Ziel valide sein). Die Kennzeichnung macht das Muster sichtbar und liefert die Messreihe für eine spätere B-Entscheidung. Umsetzungsaufwand: klein (Session-Hoch/-Tief stehen im Register).

### #22 — Frischefenster für Gate-Eingaben
Befund bestätigt: `element[0]` aus `count:2` schwankt um genau eine Kerze (1H bis zu 60 Min); der VC#13-Gate-Snapshot war nicht zeitgleich (5min-Referenz 5 Min älter als 1H/15min).
- **Option A (Ausweisen, kein Gate):** Pflicht, im MTF-Block das Alter der Referenzkerze je Ebene auszuweisen (folgt aus der #15-Timestamp-Pflicht praktisch gratis); zulässige Obergrenze je Ebene = 1 Kerzenlänge + 2 Min Toleranz (5min ≤ 7, 15min ≤ 17, 1H ≤ 62 Min), Überschreitung = Protokollhinweis "Referenz veraltet, neu lesen".
- **Option B (hartes Gate):** Überschreitung blockiert den Gate-Aufruf.
- **Zusatzentscheidung (gehört hierher, von mir provisorisch vorbelegt):** endgültige HARTE Obergrenze für das Registeralter in `gate_check.cjs` (#2). Provisorium aktuell 1440 Min; **Vorschlag: 90 Min hart** (Register wird bei jedem 5-Min-Voll-Check erneuert — 90 Min = 18 verpasste Voll-Checks, eindeutig defekte Session), 60-Min-Warnung bleibt.
- **Empfehlung: A + Register-Obergrenze 90 Min.** Die Kerzen-Frische ist eine Eigenschaft der (bewährten, am 02.09. fehlerfreien) count:2-Regel, kein Defekt — Ausweisen genügt; ein Hard-Gate würde bei Feed-Rollover-Pech legitime Trigger-Momente kosten.

---

## Nicht angefasste Dateien (bewusst)

`scripts/cooldown_check.cjs` und `scripts/add_trade.cjs` waren im Auftrag als mögliche Zieldateien genannt, aber KEIN Listenpunkt verlangt eine Änderung an ihnen — beide bleiben unverändert (quote_check.cjs deckt `last_cooldown_check.txt` per `--ref` mit ab).

## Geänderte/neue Dateien (Übersicht für Levis Sichtung)

Repo (`C:\Users\umnus\tradingview-mcp`):
- `scripts/gate_check.cjs` — #1, #2, #8, #9, #20 (getestet: Live-/Batch-/Guard-Pfade)
- `scripts/x_fetch_stamp.cjs` — NEU (#7, #17)
- `scripts/quote_check.cjs` — NEU (#10, flankiert #5)

Regelwerk (`memory/`):
- `feedback_live_trading.md` — #4 (9a), #5 (7b1), #16 (15d), #18/#25 (7b1), #23 (Punkt 9), 7b1-Templates (#1/#20)
- `feedback_vollcheck_format.md` — #4, #6, #14, #17, #19 (Template + 5 Abschnitte)
- `feedback_loop_ablauf_uebersicht.md` — #15, Historie-Markierung Tweet-Mechanik (#17)
- `feedback_tagesabschluss.md` — #13, #18-Zählung, quote_check-Einbindung
- `feedback_zeitzone.md` — #24
- `feedback_trading_zeitfenster.md` — #6 (Querverweis)
- `feedback_session_update.md` — #7 (Schreibweg Schritt 2)

*Entwurf, nichts committet, kein MEMORY.md-Eintrag durch diese Session — Levi sichtet und entscheidet über die 5 Regelwerk-Fragen.*
