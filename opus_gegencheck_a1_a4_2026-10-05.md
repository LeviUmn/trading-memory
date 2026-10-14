---
name: opus_gegencheck_a1_a4_2026-10-05
description: "Opus-Gegencheck 05.10.2026 (10:55-11:02 DE) der Fable-Umsetzung A1 (Memory-Nachtrag K1-K4/Hinweise 1-2) + A4 (scripts/analyse/rk13_1_filter.cjs). Urteil FREIGEGEBEN (2 Hinweise, keine Auflage). 125 Live-Gates ohne jetzt = echte Altdaten (jetzt:null, kein --jetzt im argv), kein Lesefehler; Zuordnung laeuft ueber Gate-ts und ist davon unabhaengig. Selbsttest 23/23, eigenes 10-Faelle-Szenario korrekt, Echtlauf 16.09.-02.10. NACH=0, npm test 424/424, sha1 identisch. Bereit zum Commit (nur Levi)."
metadata:
  node_type: memory
  type: project
  originSessionId: 627984e6-a710-48e6-9244-0efcc80854ad
  modified: 2026-10-05T09:02:31.224Z
---

# Opus-Gegencheck 05.10.2026: Fable-Umsetzung A1 + A4

**Zeitanker:** bare `date` Start Mo 05.10.2026 10:55:48 DE, Ende 11:01:33 DE. Repo HEAD 20d21aa, kein getrackter Diff, neu untracked nur `scripts/analyse/rk13_1_filter.cjs` (die übrigen `??` sind bekannte Altdateien). Grundlage: [[fable_auftrag_2026-10-05_auflagen_a1_a4]], [[opus_gegencheck_umsetzung_prio1_prio2_2026-10-05]], Nachtrag in [[fable_auftrag_2026-10-03_prio1_prio2]].

## Gesamturteil: FREIGEGEBEN
Kein Blocker, keine Auflage. Zwei Hinweise (H-1, H-2) gelten nur für die Lesart am 08.10. und verlangen keine Codeänderung. Commit nur durch Levi ([[feedback_commit_push_nur_levi]]): A4-Skript und die Memory-Änderungen sind **bereit zum Commit**.

## (1) A1: OK
- **Nur additiv:** `git diff` im Memory-Repo zeigt für `fable_auftrag_2026-10-03_prio1_prio2.md` nur +10 Zeilen am Dateiende (Überschrift + 9 Zeilen, also ≤ 12) und 0 Löschungen. In `opus_gegencheck_umsetzung_prio1_prio2_2026-10-05.md` (untracked) steht der A1-Zusatz an der Auflage A1 wie verlangt. MEMORY.md ist geändert, aber erst um 10:54:50, also nach Fables Ende um 10:53. Das war der Aufrufer, nicht Fable.
- **Quelle geprüft:** Transkript 9f4b9f35 (23.228 Z.), nur einzelne Zeilen mit `sed -n Np | node JSON.parse`. Z. 23062 (queue-operation, 14:51Z = 16:51 DE) enthält K1-K4 mit Korrekturtexten. Z. 23129 (17:12 DE) enthält "K1-K4 stimmen mit meinen Soll-Texten überein … Fables Satzumbau ist sinngleich" und unter (b) Hinweis 1 und 2 mit Wortlaut. Z. 23174 (17:18 DE) bestätigt, dass die Hinweise eingearbeitet sind. Z. 23202 ist der Commit-Text "Opus-Gegencheck: freigegeben (K1-K4 + zwei Zusatzhinweise eingearbeitet)".
- **Wortlaut gegen `loop_prompt.cjs`:** Per Teilstring geprüft:
  - Z. 218 enthält K1 ("Solange heute noch kein Voll-Check mit Exit 0 lief, vc_nr.cjs zusaetzlich mit demselben --erster-vollcheck <Wert> aufrufen, der an vollcheck.cjs geht"), K2 ("dessen --jetzt ist derselbe Wert <J>") und die Klammer aus Hinweis 1.
  - Z. 260 enthält "Im Slot ${tSlots…}" (K4). "Im/ab Slot" kommt nicht mehr vor.
  - Z. 271 enthält "Fehlte das Slot-Fire (Minute % 5 == 0) oder kam es spaeter" (K3) und die Klammer aus Hinweis 2. "verspaetet" kommt nicht mehr vor.
- **K3-Abweichung:** Sie stimmt. Der Opus-Soll war "das Slot-Fire (Minute % 5 == 0) fehlte oder kam spaeter", Fable hat den Satz umgebaut. Opus hat ihn am 03.10. selbst als sinngleich abgenommen (Z. 23129). Der Nachtrag meldet das korrekt und gleicht nicht an.
- **Sprache:** Jede Zeile trägt "Hilfe/Wortlaut, keine Pflicht, keine Regelwirkung". Es gibt keine Pflicht-Sprache.

## (2) A4 Code-Review: OK
- **Nur lesend:** Das einzige `writeFileSync` (Z. 232) und das `rmSync` (Z. 247) stehen im Selbsttest unter `fs.mkdtempSync(os.tmpdir()/rk13_1_selbsttest_*)` mit try/finally. Sonst gibt es kein write, append, rename oder unlink. Ein Selbsttest-Fall startet einen Kindprozess, der für kombi/skipped die Default-Pfade in `scripts/` nutzt. Der bricht aber schon am fehlenden gate-log ab und würde ohnehin nur lesen.
- **Filterregel** (Z. 97-111): Sie entspricht dem Auftrag.
  - Gezählt wird nur `modus === 'live'`.
  - Bezugs-Kandidaten sind Records mit `exit_code === 0` und `dry_run !== true` und gültigem `ts`.
  - Bezug ist der letzte Kandidat mit gleichem DE-Datum (`Intl` Europe/Berlin, ohne `TZ=`) und `ms <= Gate-ms`.
  - Gruppe NACH gilt nur bei `richtungskonflikt_13_1 === true` (striktes Boolean, so schreibt es `vollcheck.cjs` Z. 683/2428 nur in den Exit-0-Record).
  - Ein Hard-Exit kann nie Bezug sein, verdeckt also nichts.
- **ergebnis_r:** Es wird nur eingetragen, wenn genau ein Kombi-Eintrag `jetzt == gate.jetzt` hat, dessen `skipped_id` existiert und `ergebnis_r` eine endliche Zahl ist. Sonst steht "nicht zugeordnet (…)".
- **Formales:** Die Fußzeile ist zeichengleich mit dem Auftrag. Default ist `--ab 2026-10-05`, `--bis` bleibt offen. Exit 0 bei Erfolg, Exit 1 bei unbekannter Option, falschem Datum, `--ab > --bis`, fehlendem Pflicht-Log oder einem Fehler im Selbsttest. Kopfkommentar (Herkunft A4, "REINES LESE-SKRIPT", Filterregel, Freeze) ist vorhanden. Es urteilt nicht und schlägt keine Regel vor.

## (3) Eigene Verifikation: OK
- **Selbsttest:** `node scripts/analyse/rk13_1_filter.cjs --selbsttest` endet mit Exit 0, 23/23 bestanden, Fälle (i)-(vi) grün.
- **Echtlauf auf Kopien** in `$TEMP/.../scratchpad/logs`:
  - `--ab 2026-09-16 --bis 2026-10-02` endet mit Exit 0 und ergibt **NACH 13.1-KONFLIKT 0**, OHNE KONFLIKT 59 (yes 37 / no 18 / n/a 4), OHNE VC-BEZUG 6, zusammen 65 Live-Gates.
  - `--json` liefert dieselben Zahlen.
  - Der Default-Lauf meldet "keine Live-Gates im Zeitraum 2026-10-05 bis offen".
  - Eine unabhängige Nachrechnung mit eigenem Code ergibt dieselben 59/6.
  - Bei `chasing` stimmen argv und `gemessen.chasing` in allen Fällen überein (0 Abweichungen). Die 7 n/a sind Exit-1-Abbrüche ohne `--chasing`.
- **Eigenes Mini-Szenario** (synthetische Logs mit realer Feldstruktur im Scratchpad). Alle 10 Fälle sind korrekt eingeordnet:
  - Konflikt-VC -> Gate ergibt NACH (inkl. ergebnis_r +0,40).
  - Konflikt-VC -> Hard-Exit-VC -> Gate ergibt NACH, Bezug ist der Konflikt-VC.
  - Konflikt-VC am 07.10. 23:55 DE und Gate am 08.10. 00:05 DE (gleicher UTC-Tag) ergibt OHNE VC-BEZUG.
  - Ein dry_run-Konflikt-VC allein ergibt OHNE VC-BEZUG.
  - Ein Konflikt-VC erst nach dem Gate zählt nicht, das Gate landet in OHNE.
  - Ein Gate ohne VC mit `jetzt:null` ergibt OHNE VC-BEZUG.
  - DST-Ende 25.10.: Ein Konflikt-VC am 24.10. 23:30 MESZ und ein Gate am 25.10. 00:30 ergeben OHNE VC-BEZUG. Konflikt-VC 02:30 MEZ und Gate 02:40 ergeben NACH.
  - Steht `"true"` als String im Feld, ergibt das OHNE.
  - Ein sl-vorpruefung-Record wird nicht gezählt.
- **Feldstruktur (wichtigster Punkt):** Es liegt **kein Lesefehler** vor.
  - Alle 157 Live-Gates *haben* das Feld `jetzt`. Bei 125 ist es `null`, und in genau diesen 125 steht **kein `--jetzt` im argv**. Das sind echte Altdaten: Gates, die vom 10.09. bis 30.09. ohne `--jetzt` aufgerufen wurden. Verteilung null/String: 10.-17.09. alle null (119), 21.09. 4/8, 28.09. 1/7, 29.09. 0/3, 30.09. 1/3, 01.10. 0/4, 02.10. 0/7.
  - `argv` ist im Log ein String und wird richtig gelesen.
  - Wichtig: Die **Gruppenzuordnung hängt nicht an `jetzt`**. Sie nutzt `gate.ts` (`new Date().toISOString()`, `gate_check.cjs` Z. 4109, immer vorhanden) gegen `vc.ts` (= --jetzt-Anker des VC). `jetzt` wird nur für die ergebnis_r-Kette gebraucht. Ein künftiges `jetzt:null` führt nur zu "nicht zugeordnet", ändert aber nie die Gruppe. Für den 05.-08.10. funktioniert die Zuordnung deshalb über das richtige Feld.
  - Die VC-Records haben `ts`, `geschrieben`, `vc` und `exit_code` (Zahl), `dry_run` bisher nie. 6 Records mit `ts:null` sind alle exit 1 und werden korrekt ignoriert.

## (4) Betrieb: OK
- `npm test` (nur die Offline-Pfadliste aus package.json, kein e2e): 424/424, 0 fail, um 11:00 DE, also vor 14:00 und außerhalb des Loops.
- sha1 vorher und nachher **identisch** (`diff` leer) für loop_prompt.cjs 9165617f, vollcheck.cjs 1279067c, gate_check.cjs d22c580a, loop_stopp.cjs ff5633b2 und vc_nr.cjs 49979c19 (= Stand des Auftrags von 10:41), außerdem für alle `scripts/*.jsonl`, rk13_1_filter.cjs, feedback_live_trading.md, feedback_vollcheck_format.md und beide Fable-Auftragsdateien. vollcheck_state.json und loop_archiv/*.txt haben unverändertes mtime 02.10.
- `git status`: Getrackt ist nichts geändert, neu ist nur `?? scripts/analyse/rk13_1_filter.cjs`. Ich habe kein git add, commit oder push ausgeführt und nichts im Repo oder an den Fable-Dateien editiert.

## Hinweise (keine Auflage)
- **H-1 (Lesart 08.10.):** Der Bezug vergleicht die Schreibzeit des Gates (`gate.ts`) mit dem Anker des VC (`vc.ts`), so wie es der Auftrag festlegt. Liegt ein Gate im selben Slot vor dem Ende des VC-Laufs, gilt der VC dieses Slots als Bezug, obwohl er erst nach dem Gate geschrieben wurde. Historisch gab es das 7-mal, alle am 21.09., seit dem 28.09. nie. Fachlich ist das vertretbar, weil gleicher Slot und gleiche Marktlage. Falls am 08.10. ein NACH-Gate auffällt, kurz `geschrieben` des Bezugs-VC gegen die Gate-Uhrzeit prüfen. Eine Codeänderung ist nicht nötig, und vor der Auswertung soll nichts geändert werden.
- **H-2:** Gates ohne `--jetzt` (`jetzt:null`) bekommen nie ein ergebnis_r. Das ist korrekt konservativ. Wer am 08.10. viele "nicht zugeordnet" sieht, liegt richtig, wenn bei diesen Gates `--jetzt` fehlte.
