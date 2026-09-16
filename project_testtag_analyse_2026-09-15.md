---
name: project_testtag_analyse_2026-09-15
description: "Opus-Analyse Testtag 15.09.2026 (fiktiv), ausschliesslich gegen Skript-Rohdateien verifiziert (nicht gegen das aus dem Chat-Transkript rekonstruierte Faktenprotokoll). 48 Voll-Checks, 132 gate_check.cjs-Aufrufe (93 Vorpruefungen + 39 live, davon 3 --help), 31 FAIL/0 PASS/0 UNKNOWN, 0 Trades, 0 offene Position. Faktenprotokoll an mehreren Stellen widerlegt: 'ca. 5 Vorpruefungen' war real 93, 'VC#1-9 technisch ungueltig' bei VC#8 falsch (dort war Dual-Gate+1H-Override real gueltig, scheiterte an Geometrie nicht am Gate), Screenshots/Register-Touches/Skipped-Setups faelschlich als 'nicht auszaehlbar' aufgegeben (real exakt: 48+1 Screenshots, 9 Register-Touches, 1 Skipped-Setup-Eintrag). Praezedenzregel 13.1xQ-ROT (cc680a4, Option D+b1) vom selben Morgen: Teil b1 (k-Zaehler nur bei vollem Dual-Gate) dreifach live bestaetigt; Options-D-Kollisionszweig lief 0x, weil kein einziger PASS-Lauf am Tag vorkam -> Regel liefert null Evidenz fuer die eigentliche Praezedenz-Entscheidung. Schwerster Einzelbefund: Wiederhol-Zirkel -- 30 von 31 Live-Laeufen liefen gegen ein rechnerisch LEERES Entry-Fenster (SL-Distanz 5,8x-10,1x ATR statt <=3x), die AUSSICHTSLOS-Sperre (#6b) griff 0x weil rrGate/tpRealismGate nicht als entry-abhaengig gelten, und die Retest-Zeitbox wurde 24x durch minimale SL-Drift (29 verschiedene SL-Werte bei identischem Anker) als 'neuer Trigger' zurueckgesetzt statt VERFALLEN zu markieren. SL-Anker-Disziplin (V9 aus 11.09.) griff dagegen sauber: nur 1 Wechsel am Tag, vollstaendig begruendet, 2 Hard-Exits davor erzwangen die Begruendung. Status EINGESCHRAENKT (0 PASS-Laeufe macht den Tag als Test der neuen Regel wertlos, kein Echtgeld-Go ableitbar). 7 Vorschlaege V1-V7."
metadata:
  node_type: memory
  type: project
  status: "Analyse abgeschlossen; V1-V7 von Levi freigegeben und von Fable umgesetzt (16.09.2026, Tests 100/100, Code-Repo unkommittiert) — Opus-Gegencheck ausstehend"
  originSessionId: session_current
  modified: 2026-09-16T08:58:36.837Z
---

# Opus-Analyse Testtag 15.09.2026 (fiktiv)

*Quellenbasis: ausschließlich Skript-Rohdateien in `C:\Users\umnus\tradingview-mcp\scripts\` sowie `screenshots\`. Das Faktenprotokoll `memory\testtag\testtag_2026-09-15.md` (aus dem 74-MB-Chat-Transkript rekonstruiert, kein mitgeschriebenes Protokoll an dem Tag) wurde nur als Hypothese benutzt und Zahl für Zahl gegengerechnet.*

## 1. Kurzfazit

Der Tag war mechanisch weitgehend sauber und fachlich ein Nicht-Ereignis: 48 Voll-Checks, 132 `gate_check.cjs`-Aufrufe, 0 PASS, 0 UNKNOWN, 31 FAIL, 0 Trades, zu keinem Zeitpunkt eine offene Position. Der Regime-Flip LONG→SHORT um 16:10 DE (VC#10) ist im 1H-Schatten-Log exakt belegt und hielt bis Tagesende. Die wirkliche Geschichte des Tages ist aber nicht das Chasing, sondern die Entry/SL-Geometrie: ab 17:02 DE stand der Struktur-Anker eingefroren bei 29142.65, während der Kurs 200 Punkte tiefer lief — die SL-Distanz wuchs von 5,9× auf 10,1× ATR, und das Entry-Fenster war in 30 von 31 Live-Läufen mathematisch LEER. Das System hat das jedes Mal korrekt diagnostiziert ("UNLOESBAR", "Entry-Fenster LEER") und trotzdem rund drei Stunden lang denselben aussichtslosen Gate-Check 30× wiederholt, weil die Retest-Zeitbox durch minimale SL-Drift (29 verschiedene SL-Werte) immer wieder als "neuer Trigger" zurückgesetzt wurde. Die am Morgen committete Regeländerung (cc680a4) ist nur zur Hälfte geprüft: Teil b1 (k-Zähler nur bei vollem Dual-Gate) ist an drei Stellen im Tagesverlauf hart belegt, der Option-D-Kollisionszweig wurde mangels eines einzigen PASS-Laufs überhaupt nicht ausgeführt.

## 2. Zahlenbilanz (verifiziert, nicht übernommen)

| Behauptung Faktenprotokoll | Befund aus den Rohdateien | Urteil |
|---|---|---|
| Loop 15:25:13–20:01:53 DE | `vollcheck_state.json` erster_vollcheck 13:25:13Z; `loop_stopp_log.jsonl` letzter Satz 18:01:53Z, "Terminalzeit 20:00 DE erreicht" | bestätigt |
| 48 Voll-Checks, höchste Nr. 55, Lücken #6/#26/#32/#34/#39/#40/#48 | `vollcheck_state.json` 48 Einträge; `oneh_shadow_log.jsonl` 48 Einträge, identische VC-Nummernliste | bestätigt, doppelt unabhängig |
| 36 gate_check-Live-Aufrufe | `gate_check_log.jsonl`: 132 Sätze gesamt (93 `--sl-vorpruefung` + 39 `modus:live`, davon 3 `--help`). 39−3=36 substanzielle Live-Läufe | Zahl trifft zu, aber Gesamtzahl ist 132, nicht ~41 |
| "übrige ca. 5 Aufrufe waren Vorprüfungen" | real 93 Vorprüfungen | widerlegt (Faktor 18) |
| 31× FAIL, 0× PASS, 0× UNKNOWN | exakt bestätigt; Statusverteilung nur FAIL(31) + Abbruch/Exit1(8) | bestätigt — erste Extraktions-Korrektur war richtig |
| 0 Trades, keine offene Position | `trades.db` mtime 2026-09-11 (am 15.09. nicht angefasst), 43 Zeilen unverändert; loop_stopp bestätigt keine Position | bestätigt |
| Regime-Flip VC#10, 16:10 DE | `oneh_shadow_log.jsonl` VC#10 ts 14:10:12Z, bias short/short/short/long(1H), Richtung short | bestätigt (16:10:12 DE) |
| Chasing "ab ca. VC#20", k bis 30 | k=1 ab VC#20 (17:03:06 DE), k=2 ab VC#21 (17:05:42 DE), danach monoton bis k=30 bei VC#55 | bestätigt und präzisiert |
| "VC#1–#9: 2/2 LONG technisch UNGÜLTIG" | Widerlegt für VC#8 (16:04:10 DE): Gate-Lauf schreibt "Dual-Gate 2/2 + 1H-Override nicht dagegen" — k_ohne_signal stieg auf 1 (nur bei vollem Dual-Gate möglich unter b1) | widerlegt |
| "Screenshots nicht zuverlässig auszählbar" | 49 Dateien, 48 im Loop-Fenster = 1 pro Voll-Check + 1 Vorlauf-Screenshot | widerlegt — exakt zählbar |
| "Register-Touches nicht zuverlässig auszählbar" | 9 Touches, davon 3 mit inhaltlicher Änderung (Fib-Extensions, Session-Hoch) | widerlegt — exakt zählbar |
| "Skipped-Setups nicht ausgezählt" | genau 1 Eintrag (16:04 DE, LONG, manuell); demgegenüber 31/31 FAIL-Läufe forderten Erfassung auf | auszählbar: 1 von 31 |
| Quick-Ticks nicht auszählbar | bestätigt — keine Logdatei existiert dafür | bestätigt |
| Hard-Exit-Typen `--fib-1272`/`--retest-bedingung`/`--terminal-geprueft` | `vollcheck.cjs` führt kein Aufruf-Log | nicht prüfbar |
| SL-Anker durchgehend TAUGLICH, unverändert 29142.65 | 91/93 Vorprüfungen TAUGLICH; Anker-Werte: 29082.9(1×,long), 29016(12×,long), 29142.65(80×,short, ab 16:11:28 DE) | bestätigt |

Weitere Zahlen: 122→32 Einträge in `trigger_kandidaten_log.jsonl` (29× FAIL/short/chasing=yes, deckt sich exakt mit den 31 Gate-Kandidaten). 4 von 48 1H-Schattenmessungen mit `e7_zaehlt: true` (VC#16–#19, Blockade short, Gegenbewegung 74,7–96,4 Pkt).

## 3. Zentrale fachliche Befunde

**3.1 Präzedenzregel 13.1×Q-ROT (cc680a4): b1 belegt, Option D ungetestet.** b1 (k-Zähler läuft nur bei vollem Dual-Gate) dreifach live bestätigt: VC#7 (Dual-Gate gebrochen → k bleibt 0), VC#8 (Dual-Gate voll → k=1), VC#9 (Bruch → Reset auf 0), VC#10–19 (Dual-Gate voll SHORT aber 1H-Override dagegen → k bleibt 0 trotz erfülltem Kerzenkriterium), VC#20 (Override kippt → k startet bei 1). Der Options-D-Kollisionszweig (`kollisionKandidat` braucht PASS+chasing=yes) lief dagegen 0×, weil am ganzen Tag kein einziger PASS-Lauf vorkam — codekonform, aber der Tag liefert **null Evidenz** für die eigentliche Präzedenz-Entscheidung (Schattenmessung bleibt bei 0 Kollisionsmomenten). Plausibilitätswarnungen (N3, P6) feuerten korrekt beim einzigen LONG-Moment.

**3.2 Regime-Flip 16:10 — sauber, aber spät belohnt.** Zwischen VC#10 und VC#19 lief der Kurs 1H-Override-widrig um bis zu 96,4 Pkt — belastbarster Datenpunkt für die offene E7-Frage (1H-Override evtl. zu blockierend).

**3.3 TP1-UNLÖSBAR strukturell nachvollziehbar, aber früher da als behauptet.** War schon beim allerersten Live-Lauf (16:04:10 DE, LONG, 3,89×ATR) vorhanden, kein Chasing-Effekt. ATR fiel im Tagesverlauf von 30,5 auf 20,3 Pkt bei fixem SL-Anker → SL/ATR-Verhältnis stieg monoton. Bestes RR des Tages: 0,817:1 (17:51 DE), nie nahe 1:1. `gate_check.cjs` benannte die Ursache korrekt als Entry/SL-Geometrie, nicht TP-Wahl. **0 Trades war die einzig regelkonforme Bilanz.**

**3.4 Wiederhol-Zirkel — schwerster Einzelbefund.** 30/31 Live-Läufe gegen ein rechnerisch LEERES Entry-Fenster. Die AUSSICHTSLOS-Sperre (#6b, `NON_ENTRY_DEPENDENT_GATES`) griff 0×, weil rrGate/tpRealismGate formal als entry-abhängig gelten, obwohl das Fenster nachweislich leer war. Die Retest-Zeitbox (max. 2 Voll-Checks) wurde 24× durch "ZEITBOX-KOLLISION — ERSETZT durch neuen Trigger" zurückgesetzt, weil `--sl-auto` bei jedem neuen ATR-Wert leicht driftete (29 verschiedene SL-Werte bei identischem Anker 29142.65, identischem TP1 28855.53). Der Mechanismus, der nach dem 03.09. genau das verhindern sollte, wurde damit strukturell umgangen — das Setup wurde nie als VERFALLEN ausgewiesen.

**3.5 SL-Anker-Historie — deutlich besser als 11.09.** Nur 1 Wechsel am Tag (13:33:02Z, long, 29082.9→29016), vollständig begründet, 2 Hard-Exit-1-Abbrüche davor erzwangen die Begründung (V9 aus der 11.09.-Analyse greift wie gebaut). Randbefund: der Richtungswechsel long→short (16:11:28 DE) erscheint nicht im Wechsel-Log — V9 vergleicht offenbar nur innerhalb derselben Richtung.

**3.6 Registerpflege — formal erfüllt, inhaltlich eingeschlafen.** Letzte inhaltliche Änderung 15:46:22 DE, danach 4 Std. nur Zeitstempel-Refreshs. Die V11-INHALT-WARNUNG (>180 Min) stand ab ~18:46 DE in jedem Gate-Output und wurde bis Tagesende nicht beantwortet (entlastend: Session-Tief wurde nie unterboten, ein neues Level war nicht zwingend fällig).

**3.7 Tweet-Raster (Nebenbefund, nur 2 Datenpunkte).** Beide Abrufe (19:41, 19:52 DE) nicht auf dem 10-Min-Raster — gleiches Muster wie am 09.09. beanstandet, aber zu wenig Datenpunkte für einen Nachweis.

## 4. Prozessfehler / Brüche

1. **Keine Protokolldatei während des Tages** (bekannt, systemisch) — für VC#1–54 sind Hard-Exit-Details, Lücken-Ursachen, Quick-Ticks, Tweet-Check-Zahl unwiederbringlich außerhalb der Skript-Logs. Zugleich zeigt dieser Bericht: die wichtigsten Tageszahlen waren trotzdem exakt rekonstruierbar, weil `gate_check_log.jsonl`/`vollcheck_state.json`/`oneh_shadow_log.jsonl` intakt sind — die Rekonstruktion aus dem Chat-Transkript war der falsche Weg (3 Zahlen falsch gesetzt, 4 fälschlich als "nicht auszählbar" aufgegeben).
2. **Faktenprotokoll-Fehler** (neu gefunden, über die bereits korrigierte PASS/UNKNOWN-Sache hinaus): "~5 Vorprüfungen" (real 93), "VC#1-9 technisch ungültig" (bei VC#8 widerlegt), Screenshots/Register-Touches/Skipped-Setups fälschlich als nicht auszählbar deklariert.
3. **Wiederhol-Zirkel** (3.4): 30 Gate-Aufrufe gegen leeres Entry-Fenster, 24 Zeitbox-Ersetzungen, nie ein VERFALLEN.
4. **Skipped-Setup-Erfassung:** 31/31 FAIL-Läufe forderten Erfassung, nur 1 Eintrag geschrieben — das dominierende SHORT-Setup (30 Läufe) fehlt komplett in `skipped_setups_fiktiv.jsonl`.
5. **Register-INHALT-WARNUNG ignoriert** (3.6).
6. **Keine DB-Fehlschreibzugriffe** — anders als 11.09.: `trades.db` am 15.09. nicht angefasst, TESTTAG-GUARD hat sauber gehalten.
7. **Keine Log-Inkonsistenzen gefunden** — alle Querchecks (vollcheck_state↔oneh_shadow, gate_check_log↔trigger_kandidaten, last_gate_fail↔letzter Log-Satz) stimmen überein, kein Parse-Fehler in 342 JSONL-Zeilen.

## 5. Status

**EINGESCHRÄNKT.** Zählstände, Gate-Ergebnisse, Chasing-Verlauf, SL-Anker-Historie, Regime-Flip, Screenshot-/Register-Bilanz, DB-Unversehrtheit sind vollständig und doppelt belegt. Es fehlt die Voll-Check-Innenansicht für #1–54 (ohne Protokolldatei nicht heilbar). Der Tag entwertet sich als Regeltest selbst: kein einziger PASS-Lauf → die am selben Morgen freigegebene Präzedenzregel (Option D+b1) ist nur im Zähl-Teil geprüft, der Entscheidungszweig blieb ungetestet. **Kein Echtgeld-Go aus diesem Tag ableitbar** (die 5 binären Pass-Kriterien aus `project_validierungstesttag_naechster_handelstag.md` mangels Trade-Gelegenheit nicht beantwortet).

## 6. Konkrete Vorschläge (offen, Levi-Entscheidung)

- **V1 (hoch):** Aussichtslosigkeit bei leerem Entry-Fenster als Aufruf-Sperre — `fenster.unsolvable`/`entryFenster.leer` soll dieselbe AUSSICHTSLOS-Sperre auslösen wie `NON_ENTRY_DEPENDENT_GATES`, solange SL-Anker und TP1-Level unverändert sind.
- **V2 (hoch):** Fingerabdruck der Retest-Zeitbox auf `{sl_anker, tp1_level_price, cluster, dir}` umstellen statt auf den driftenden `--sl-auto`-SL-Wert (verhindert die 24 Zeitbox-Kollisionen durch 5-Punkte-Drift).
- **V3 (mittel):** Wiederholte identische Setups automatisch als `wiederholungen++` in `skipped_setups_fiktiv.jsonl` erfassen statt 31 Aufforderungen ins Leere laufen zu lassen.
- **V4 (hoch, Prozess):** `vollcheck.cjs` soll jeden Lauf zusätzlich selbst nach `vollcheck_log.jsonl` schreiben (analog `gate_check_log.jsonl`), unabhängig vom Chat-seitigen Protokoll.
- **V5 (mittel, zur Entscheidung):** Reine Anzeige-Schwelle "SL-Distanz > 4×ATR → Setup bis zum Retest tot" (kein Gate, nur Diagnose-Zeile).
- **V6 (niedrig):** V9-Ankerwechsel-Log auch bei Richtungswechsel schreiben (mit Vermerk, ohne Begründungspflicht).
- **V7 (hoch, Prozess):** Faktenprotokoll künftig primär aus den Skript-Logs erstellen, Chat-Transkript nur als Ergänzung für Freitext — Beleg: Transkript-Rekonstruktion hatte 3 Zahlen falsch und 4 fälschlich aufgegeben, alle waren aus den Skript-Logs exakt rekonstruierbar.

Vorgänger: [[testtag/testtag_2026-09-15]] (Faktenprotokoll, teilweise widerlegt/präzisiert), [[project_gegencheck_praezedenz_13_1_vs_qrot_fable_umsetzung_2026-09-15]] (Regeländerung desselben Tages), [[project_testtag_analyse_2026-09-11]] (Vorgänger-Analyse, V9-Ankerwechsel-Fix). Rohdaten: `scripts/vollcheck_state.json`, `gate_check_log.jsonl`, `oneh_shadow_log.jsonl`, `register_touch_log.jsonl`, `trigger_kandidaten_log.jsonl`, `skipped_setups_fiktiv.jsonl`, `sl_anker_wechsel_log.jsonl`, `trades.db`, `screenshots/tv_full_2026-09-15T*.png`.

## 7. Umsetzung V1-V7 (Fable, 16.09.2026 — Levi-Freigabe via Sonnet-Hauptchat; NICHT gegengecheckt, Opus-Gegencheck ausstehend)

Alle sieben Vorschläge umgesetzt. Tests: bestehende Suite + 5 neue Testfälle = **100/100 grün** (`node --test tests/trading_scripts.test.js`). Code-Repo-Änderungen (`scripts/gate_check.cjs`, `scripts/vollcheck.cjs`, `tests/trading_scripts.test.js`) **unkommittiert** — Commit erst nach Levi-Freigabe. Details je Punkt:

- **V1:** Neue `AUSSICHTSLOS (V1)`-Zeile im Retest-Zeitbox-Block, sobald `fenster.unsolvable` oder `entryFenster.leer` (nur live). Der Zustand wandert als `aussichtslos`-Flag in den Fingerabdruck von `last_gate_fail.json`; ein Wiederholungsaufruf mit unverändertem SL-Anker+TP1-Level druckt `AUSSICHTSLOS-SPERRE VERLETZT (V1)` — gleiche Konsequenz-Klasse wie #6b (Anzeige, kein Exit-Einfluss, wie dort etabliert).
- **V2:** Fingerabdruck der Retest-Zeitbox umgestellt auf `{sl_anker, tp1_level_price, cluster, dir}`; der SL-Wert unterscheidet nur noch mit Toleranz ±0,5×ATR (SL-Drift ≤ Toleranz = derselbe Trigger, echter Anker-Wechsel = neuer Trigger). failGates bleiben im Fingerabdruck für die #6b-Detailmeldung, entscheiden aber nicht mehr über die Identität. Alt-Format-Dateien zählen einmalig als anderer Trigger (dokumentiert, kein Migrationscode).
- **V3:** An fiktiven Testtagen (vollcheck_state testtag_modus=fiktiv, heutiges Datum) wird jeder Live-FAIL automatisch nach `skipped_setups_fiktiv.jsonl` geschrieben; unverändertes `{dir, sl_anker, tp1_level}` am selben DE-Tag → `wiederholungen++` (atomischer Rewrite). An echten Tagen bleibt die manuelle Pflicht unverändert (bewusste Abweichung/Präzisierung: Automatik nur für die fiktive Messreihe — Levi-Auftrag nannte die fiktiv-Datei explizit).
- **V4:** `vollcheck.cjs` schreibt jeden Lauf selbst nach `scripts/vollcheck_log.jsonl` (Nr., ts, Lücken-Liste, vollständig, Fazit, Konsequenz; bei Hard-Exit 1 den Grund aus einem stderr-Mitschnitt, 400 Zeichen). Greift über `process.on('exit')` auf ALLEN Exit-Pfaden; Dry-Runs werden markiert statt verschluckt; Schreibfehler kippen nie den Lauf.
- **V5:** Reine Anzeige-Zeile `SL-DISTANZ-DIAGNOSE (V5)` bei SL-Distanz > 4×ATR ("Setup bis zum Retest tot, keine Gate-Aufrufe; SL NICHT verengen") — Konstante `SL_DISTANZ_DIAGNOSE_ATR_MULT = 4`, kein Gate, keine Sizing-/Ampel-Wirkung, erscheint in den TP1-Fenster-Diagnosezeilen des Gate-Outputs (und damit im Voll-Check-Zitat).
- **V6:** `slAnkerWechselGuard` loggt Richtungswechsel jetzt ebenfalls nach `sl_anker_wechsel_log.jsonl` (Feld `richtungswechsel: "long->short"`, Vermerk statt Begründungspflicht, `gegen_richtung: null`) mit eigener Ausgabezeile `SL-ANKER-RICHTUNGSWECHSEL (V6)`. Bestehender V9-Test an die neue Semantik angepasst (5 statt 3 Log-Zeilen).
- **V7:** Prozessregel in `feedback_tagesabschluss.md` (Abschnitt Faktenprotokoll-Abschluss): Skript-Logs sind die PRIMÄRQUELLE jeder Rekonstruktion, das Chat-Transkript nur Ergänzung für Freitext; bei Widerspruch gilt das Log, Abweichung in Unterpunkt 4 ausweisen.
