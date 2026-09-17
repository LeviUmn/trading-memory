---
name: project-testtag-2026-09-16-besprechung-ausstehend
description: "Testtag 16.09.2026 BESPROCHEN 17.09. Umsetzung der Opus-Vorschlaege W1-W8 (Prioritaet W1+W2) als TODO fuer Montag 21.09.2026 vertagt -- Fable-Kontingent heute (17.09.) nach den CDP-Tab-Fixes auf 6% aufgebraucht, Levi 18.09.-20.09. nicht da."
metadata:
  node_type: memory
  type: project
  originSessionId: 4bedc910-34ac-4303-800f-158b4b77d9c8
  modified: 2026-09-17T10:28:21.979Z
---

Testtag 16.09.2026 ist abgeschlossen (Loop 15:17-22:00 DE, 2 fiktive Trades je -1R, netto -75,28€). Opus-Analyse in [[project_testtag_analyse_2026-09-16]]. Am 17.09. mit Levi besprochen (Zusammenfassung im Chat durchgegangen: FOMC-Sperrfrist-Verstoss, Registerpflege-Artefakt, Pflichtfrage-Antwort, W1-W8).

**Entscheidung 17.09.:** Umsetzung der Vorschlaege NICHT heute — Levis Fable-Kontingent war nach den drei CDP-Tab-Fixes-Runden auf 6% aufgebraucht, zu riskant fuer eine Regelwerk-Aenderung (Gefahr: mitten in W1/W2 haengenbleiben, halb umgesetzt+ungetestet). Levi ist 18.-20.09. nicht da. **Umsetzung auf Montag, 21.09.2026 vertagt.**

**Why:** [[feedback_live_trading]] Regeländerungs-Tempo-Bremse + [[feedback_dont_change_running_system]] gelten ohnehin (vor neuer Regel fragen, nicht sofort umsetzen); zusaetzlich hier ein hartes Ressourcen-Argument (Fable-Budget).

## TODO Montag 21.09.2026: Opus-Vorschlaege W1-W8 aus Testtag 16.09. umsetzen

Reihenfolge laut Levi/Opus-Empfehlung: **W1+W2 zuerst** (die einzigen zwei mit echtem, beziffertem Preisschild), Rest danach nach Bedarf. Rollenteilung wie immer: Opus hat bereits analysiert/vorgeschlagen (Bericht liegt fertig vor) → Fable setzt um → frischer Opus-Gegencheck vor Commit (Regelwerk-Aenderung, Autor≠Pruefer-Pflicht).

1. **W1 (hoch, PRIORITAET — wichtigster Punkt des Tages).** `gate_check.cjs` bekommt Pflichtfeld `--blackout <none|aktiv|HH:MM-HH:MM>` + `--grund-blackout`-Ausweg (analog A3-Befuellungspflicht). Bei `aktiv` → GESAMTSTATUS FAIL mit eigenem Gate "Entry-Sperrfrist (Schritt 4)", nicht nur Warnzeile. Optional Stufe 2: `--event-zeit`+`--event-typ`, Skript rechnet 30-Min-Frist + ATR-Anstieg gegen Vor-Event-Wert selbst. Grund: heute zaehlte der Loop den FOMC-Countdown selbst mit, verband es aber nie mit der Entscheidung — beide Trades (20:26/20:47 DE) verletzten die Sperrfrist, Kategorie-B-Regelbruch, −75,28€ waeren bei 0 Trades gelandet.
2. **W2 (hoch, kostete am 16.09. ~1,1R).** Rundzahl-Band im Level-Register automatisch nachfuehren (`register_touch.cjs --rundzahlen` bei >±150 Pkt Abweichung selbst aufrufen), ODER minimal-invasiv: bestehende RUNDZAHL-BAND-WARNUNG von Anzeigezeile zu Hard-Exit 1 hochstufen (Reparaturbefehl steht schon in der Meldung). Grund: Band stand seit 17:06 DE bei 29100 waehrend Kurs 450 Pkt tiefer lief, ein machbarer PASS (~+1,1R) galt faelschlich als UNLOESBAR.
3. **W3 (mittel).** `gate_check.cjs` listet bei FAIL die Registerlevel im zulaessigen TP1-Fenster explizit auf; bei Fensterbreite <1× ATR "ENGES Fenster" von Diagnose zu Handlungsaufforderung heben.
4. **W4 (mittel, nur messen, NICHT ohne Levi-Entscheidung umsetzen).** Q4 (Runway) als Schattenmessung kennzeichnen (0 von 19 Bewertungen je erfuellt — strukturell unerfuellbar), Q-Score bis zur Entscheidung aus Q1-Q3 bilden.
5. **W5 (hoch).** Screenshot-Pflicht scharf stellen — `--screenshot`-Ausnahme mit Zaehler versehen, der nach n Ausnahmen in Folge Hard-Exit ausloest (heute: 63 stillschweigende Ausnahmen in Folge, 0 Screenshots am ganzen Testtag).
6. **W6 (mittel).** `position_tick`/`vollcheck_state` beim Positionsende zwingend synchronisieren; `loop_stopp.cjs` gibt WARNUNG bei divergierender aelterer Quelle statt nur Protokoll (heute: 69 Min unbemerkte Divergenz).
7. **W7 (mittel).** Hard-Exits bei `--kerzen-*`/`--k-ohne-signal`-Abweichung vom State entschaerfen: State-Wert uebernehmen, Abweichung als Luecke protokollieren, Lauf fortsetzen statt abbrechen (heute 11 von 27 Hard-Exits, ein Drittel aller Laeufe).
8. **W8 (niedrig, bereits erledigt).** Verpasstes Fenster 15:30-17:17 DE als Prioritaets-Datenpunkt fuer CDP-Fixes vermerkt — CDP-Tab-Fixes sind seit 17.09. bereits vollstaendig committet, siehe [[project_cdp_tab_fixes_committet_2026-09-17]].

**Ausdruecklich NICHT vorgeschlagen (laut Opus, nicht umsetzen):** RR-Schwelle lockern, AUSSICHTSLOS-Sperre lockern, TP-Realismus-Cap ueber 3× ATR anheben, neue Cooldown-/Verstoszzaehler-Mechanik.

**How to apply:** Montag 21.09.2026 als ersten Punkt ansprechen. Start mit W1+W2 als ein gemeinsamer Fable-Auftrag (beide betreffen `gate_check.cjs`/Register-Pflege, thematisch nah beieinander), danach Opus-Gegencheck, danach Levi-Entscheidung ueber W3/W5/W6/W7 nach Bedarf. W4 separat, weil explizite Levi-Entscheidung noetig (Kalibrierungsfrage, keine reine Bugfix).
