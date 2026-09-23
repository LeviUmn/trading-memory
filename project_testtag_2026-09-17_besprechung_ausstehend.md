---
name: project-testtag-2026-09-17-besprechung-ausstehend
description: "X1,X2,X4,X6a-c,X7 UMGESETZT+COMMITTET 21.09.2026 (Commit a969767, 2 Opus-Gegenchecks). X5 durch W5 (7cb915e) bereits abgedeckt. X3 (Q4-Kalibrierung) analysiert, Opus empfiehlt Q4_MODUS='schwelle' mit 0,35 -- bewusst zurueckgestellt bis mehr Daten aus der W4-Schattenmessung da sind. X8 (13 historische Nachtraege) separat offen, braucht echte Kursdaten + den jetzt fertigen X4-Bars-Parameter."
metadata:
  node_type: memory
  type: project
  status: "X1/X2/X4/X6/X7 UMGESETZT+COMMITTET 21.09.2026 (a969767); X3 bewusst zurueckgestellt; X8 offen"
  originSessionId: dba6a9d3-1e4e-49d5-b8aa-69d27ac1a2f2
  modified: 2026-09-21T13:30:57.220Z
---

# TODO Montag 21.09.2026 — Opus-Vorschläge X1-X8 (Testtag 17.09.2026)

**Abgrenzung:** Dies ist ein EIGENSTÄNDIGES Montag-TODO, unabhängig von [[project_testtag_2026-09-16_besprechung_ausstehend]] (dort: W1-W8 aus der FOMC-Testtag-Analyse 16.09.2026). Beide Listen stammen aus unabhängigen Testtagen und unabhängigen Opus-Analysen — nicht zusammenlegen, nicht die Nummerierung mischen. Volle Herleitung jedes Punkts: [[project_testtag_analyse_2026-09-17]] Abschnitt 6.

## Zu besprechen und zu entscheiden

- **X1 (hoch, Priorität).** SL-Anker-Reset nach neuem Impuls-Extrem fehlt strukturell — am 17.09. blieb der Anker nach dem Tageshoch um 19:00/19:05 DE 11 Voll-Checks lang unverändert, wodurch die SL-Distanz fälschlich als 6,57-8,25× ATR statt korrekt 1,50-1,95× ATR gemeldet wurde (12 PASS-fähige Slots als „Setup tot" verworfen, ohne Geldfolge). Vorschlag: Impuls-Extrem-Tracking in `gate_check.cjs --sl-vorpruefung`/`vollcheck.cjs`, Pflichtzeile „ANKER-RESET FÄLLIG" bei Überschreitung, `sl_anker_wechsel_log.jsonl` um `gegen_richtung: false`-Fälle erweitern.
- **X2 (hoch).** `--sl-vorpruefung` soll das zulässige TP1-Fenster inkl. Registerlevel mitrechnen und drucken — „TP1-Fenster leer" war am 17.09. der zweithäufigste Ablehnungsgrund (14 von 52 Voll-Checks) und stand in keinem einzigen Log als Rechnung, nur als Kopfrechnung im Fazit-Text.
- **X3 (hoch, Priorität, vor dem nächsten Validierungstag entscheiden).** Q4 (Runway ≥1,0) steht über die gesamte Loghistorie bei 22 von 22 Bewertungen NIE erfüllt (Wiedervorlage von W4 aus der 16.09.-Analyse). Am 17.09. deckelte das den Q-Score ab 15:50 DE strukturell auf ROT — kein Einstieg mehr möglich, unabhängig von der Chart-Geometrie. Ein als Validierungstag gefahrener Testtag mit diesem Zustand kann die fünf binären Pass-Kriterien aus [[project_validierungstesttag_naechster_handelstag]] nicht erfüllen, bevor diese Frage geklärt ist. Optionen zur Diskussion: Q4 als Schattenfaktor auslagern (Score dann aus Q1-Q3, Schwelle 3/3) oder Runway-Schwelle absenken (z. B. ≥0,5).
- **X4 (mittel).** `skipped_fiktiv.cjs --nachtrag` rechnet MFE/MAE gegen die übergebenen Tages-Hoch/Tief-Werte statt bar-für-bar bis zum tatsächlichen Exit-Zeitpunkt — am 17.09. für zwei SL-Hit-Fälle nachweislich falsch (MFE 40,55/44,50 Pkt geloggt, korrekt wären 14,75/0,00 Pkt). Vorschlag: optionaler `--bars <Pfad>`-Parameter für bar-für-bar-Rechnung; zusätzlich Pflichtzeile „Positionsmanagement-Vorbehalt" bei Haltedauer über ~6 Kerzen (der 16:07-Fall lief 2h53min bis TP1 mit Rückfall unter Entry dazwischen — das nominale +1R ist ohne Punkt-12-Anwendung nicht das realistisch erzielbare Ergebnis).
- **X5 (mittel, Wiedervorlage W5).** Screenshot-A3-Ausnahme braucht einen Zähler mit Hard-Exit nach n identisch begründeten Ausnahmen in Folge (Vorschlag n=6) — am 17.09. liefen 52 Voll-Checks in Folge mit der immer gleichen Begründung „kein visueller Zusatzwert", obwohl sich der Chart zwischendurch zweimal fundamental änderte (94,2-Pkt-Kerze 15:30 DE, neues Tageshoch 19:00/19:05 DE).
- **X6 (mittel).** (a) `register_touch.cjs` soll `--help` als echten Sonderflag ohne Touch-Ausführung behandeln (verursachte am 17.09. zwei inhaltsleere Touches); (b) Registerpflege zeitlich nicht mit dem Voll-Check-Kerzenschluss kollidieren lassen; (c) `vollcheck.cjs` soll bei einer ausgefallenen Slot-Lücke eine begründete Pflichtzeile ausgeben statt einer stillen Nummernlücke.
- **X7 (niedrig).** Dediziertes Quick-Tick-Log und Tweet-Fetch-Verlaufslog fehlen als Skript-Dateien — am 17.09. waren dadurch rund 270 Tick-Ereignisse (1-Min-Cron über 4,5 Stunden) nachträglich nicht mehr exakt rekonstruierbar.
- **X8 (niedrig).** 13 vorbestehende offene `skipped_setups_fiktiv.jsonl`-Nachträge aus dem 11.09. (12 Stück) und 15.09.2026 (1 Stück) abarbeiten — Backlog seit über einer Woche, von `gate_check.cjs` selbst als Tagesabschluss-Pflicht deklariert, inhaltlich Voraussetzung für die X3-Entscheidung (liefert zusätzliche Q4-Messpunkte).

## Ausdrücklich NICHT zur Diskussion gestellt (Opus-Empfehlung 17.09.)

Keine Änderung an Option D/13.1×Q-ROT-Präzedenz, keine Lockerung der 4×-ATR-„TOT bis Retest"-Diagnoseschwelle, keine Absenkung der RR-Schwelle, keine neue Cooldown-/Verstoßzähler-Mechanik, keine Änderung der SL-Anker-Definition selbst (nur der fehlende Reset-Trigger nach Extrembruch ist der Befund, siehe X1).

## Stand 21.09.2026 — X3-Analyse abgeschlossen, Umsetzung zurückgestellt

Opus hat die Q4-Historie ausgewertet (40 dedupl. Messpunkte aus `gate_check_log.jsonl`, 5 mit bekanntem Ergebnis, 219 historische Entry/TP1-Paare für die strukturelle Erreichbarkeitsanalyse). Kernbefund: Q4 ≥1,0 ist bei 96,8 % aller Setups per Konstruktion unerreichbar (jedes Entry/TP1-Paar kreuzt fast immer eine 50er-Rundzahl aus dem gepflegten Register) — kein Kalibrierungsfehler, sondern ein struktureller Widerspruch zwischen Q4≥1,0 (verlangt TP1 <50 Pkt entfernt) und RR≥1,0+8c-Floor (verlangt TP1 ≥1,5×ATR entfernt).

**Empfehlung:** `Q4_MODUS='schwelle'` mit `Q4_SCHWELLE_ALTERNATIV=0.35` (nicht 0,5 wie ursprünglich vorgeschlagen — 0,5 hätte bei den 5 bekannten Fällen 0 von 5 Ampeln verändert). Begründung: bei den beiden 16.09.-Verlusttrades war Q4 (0,10/0,21) der einzige Faktor, der volle Position verhindert hat — `'schatten'` (Q4 komplett raus) ist damit widerlegt, Q4 hat echte Schutzwirkung am unteren Ende. 0,35 liegt in der einzigen vom Datensatz gestützten Trennlinie zwischen den beiden Verlierern und dem einen (nominellen) Gewinner.

**Explizite Einschränkung (Opus selbst):** effektiv nur 3 belastbare Datenpunkte für die genaue Zahl 0,35 — "begründete Startannahme", keine statistisch gesicherte Schwelle. Nebenbefund für später: Q4 misst aktuell eher die Rundzahl-Gitterphase als echte Marktstruktur; ein saubererer Fix wäre Q4 künftig gegen strukturelle Level (Pivots/PDH/PDL/Wick-Zonen) statt Rundzahlen zu rechnen — eigene, größere Entscheidung, nicht Teil von X3.

**Levi-Entscheidung 21.09.2026:** NOCH NICHT umsetzen. Erst mehr Daten vom nächsten Testtag abwarten — die W4-Schattenmessung (`q4_schatten_log.jsonl`) läuft seit Commit 7cb915e mit, hat aber bisher 0 Zeilen (erste echten Daten erst beim nächsten Testtag). `Q4_MODUS` bleibt vorerst `'gating'`. **Bei der nächsten X3-Besprechung diesen Eintrag zuerst lesen, nicht neu von vorne analysieren** — die Empfehlung 0,35 und ihre Begründung stehen bereits fest, es fehlen nur mehr Datenpunkte zur Bestätigung.

## X1/X2/X4/X6a-c/X7 UMGESETZT+COMMITTET 21.09.2026 — Commit a969767

Ablauf: Fable implementiert (durch ein Rate-Limit unterbrochen, nach Reset per SendMessage nahtlos fortgesetzt, nichts wiederholt) → Opus-Gegencheck 1 (FREIGEGEBEN MIT AUFLAGEN: X1/X4/X5/X6/X7 einwandfrei + X3-Kompatibilität explizit bestätigt — kein neuer Code hängt an Q4/Runway, Verhalten unter allen drei Q4-Modi alt/neu identisch; 1 blockierender Fund A1) → Fable behebt A1+A4 → Opus-Gegencheck 2 (FREIGEGEBEN) → committet.

**A1 war der einzige echte Fund:** die neue X2-TP1-Kandidatenzeile widersprach sich selbst — sie konnte für ein und dasselbe, per `--tp1` übergebene Setup gleichzeitig "PASS-FAEHIG" und "kein PASS-faehiger Kandidat ... aussichtslos" ausgeben (live erreichbar über `vollcheck.cjs`, nicht nur ein Testrandfall). Behoben: der Filter berücksichtigt jetzt den Vorprüfungs-Modus, der bereits committete W3-Live-Pfad blieb unverändert.

**Nebenbefund während der Prüfung (durch den ersten Opus-Prüfagenten verursacht):** 8 Smoke-Test-Zeilen versehentlich in die Live-Datei `scripts/trigger_kandidaten_log.jsonl` geschrieben (fließt in die Tagesbilanz von `protokoll_bilanz.cjs` ein). Selbst bereinigt (Backup `scripts/trigger_kandidaten_log.jsonl.bak_vor_smoke_cleanup_20260921T130830Z`, exakt die 8 Zeilen vom 21.09. entfernt, vom zweiten Opus-Check gegen das Backup verifiziert). `.gitignore`-Lücke bei mehreren Live-Logs (`sl_anker_wechsel_log.jsonl`, `vollcheck_log.jsonl`, 8× `last_*.txt`) im selben Zug geschlossen, neue generische Regel `scripts/*.bak_*` für künftige Backups ergänzt.

**X5:** keine Code-Änderung — durch das bereits committete W5 (7cb915e) vollständig abgedeckt, von Fable und Opus unabhängig bestätigt.

**Was jetzt neu im System ist (kurz, für Alltagsgebrauch):**
- **X1:** `gate_check.cjs --sl-vorpruefung` druckt "ANKER-RESET FAELLIG", wenn seit dem Setzen des SL-Ankers ein neues Extrem ≥2×ATR entstanden ist — reine Diagnose, kein automatischer Wechsel, kein Hard-Exit.
- **X2:** `--sl-vorpruefung` rechnet und druckt jetzt das TP1-Fenster inkl. Registerlevel (vorher nur im Live-Gate/W3).
- **X4:** `skipped_fiktiv.cjs --nachtrag --bars <Pfad>` rechnet MFE/MAE bar-für-bar; Pflichtzeile "Positionsmanagement-Vorbehalt" bei Haltedauer >6 Kerzen. Eingabeformat = `data_get_ohlcv`-Ausgabe. Das ist die Grundlage für X8.
- **X6a:** `register_touch.cjs --help` funktioniert jetzt ohne Touch-Nebenwirkung.
- **X6b:** Registerpflege im selben 5-Min-Slot wie der Voll-Check-Kerzenschluss löst nicht mehr fälschlich D5 aus (auf den laufenden Slot begrenzt, kein Schlupfloch für echt veraltete Läufe — vom zweiten Prüfer mit vier gezielten Missbrauchssonden verifiziert).
- **X6c:** Nummernlücken im Voll-Check-Log tragen jetzt eine Begründung statt einer stillen Lücke.
- **X7:** neue Logs `scripts/quick_tick_log.jsonl` und `scripts/tweet_fetch_log.jsonl`/`_dax.jsonl`, neues Skript `scripts/quick_tick.cjs`.

**Offen bleibt:** X3 (s.o., zurückgestellt) und X8 (13 historische Nachträge, braucht echte Kursdaten — separater Arbeitsschritt, X4 liefert jetzt das nötige `--bars`-Format).
