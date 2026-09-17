---
name: project-testtag-2026-09-17-besprechung-ausstehend
description: "TODO Montag 21.09.2026: Opus-Vorschlaege X1-X8 aus der Testtag-Analyse 17.09.2026 mit Levi besprechen und entscheiden. GETRENNT vom bereits bestehenden Montag-TODO zu W1-W8 aus der 16.09.-Analyse (project_testtag_2026-09-16_besprechung_ausstehend.md) - beide TODOs sind unabhaengige Listen aus unabhaengigen Testtagen, nicht vermischen. Prioritaet: X1 (SL-Anker-Reset nach neuem Impuls-Extrem, fehlte 11 Voll-Checks lang am 17.09., ohne Geldfolge) und X3 (Q4/Runway steht bei 22 von 22 Nicht-Erfuellungen, deckelte den Q-Score am 17.09. ab 15:50 DE strukturell auf ROT - vor dem naechsten Validierungstag entscheiden, sonst ist dessen Ergebnis vorherbestimmt)."
metadata:
  node_type: memory
  type: project
  status: "TODO offen, Levi-Entscheidung ausstehend"
  originSessionId: dba6a9d3-1e4e-49d5-b8aa-69d27ac1a2f2
  modified: 2026-09-17T18:29:00.923Z
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
