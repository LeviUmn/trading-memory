---
name: project_gegencheck_v1_v7_runde4_2026-09-16
description: "Opus-Gegencheck Runde 4 (16.09.2026), letzte Runde der V1-V7-Fix-Kette. FREIGEGEBEN OHNE AUFLAGE -- erstmals in dieser Kette. Kernpruefung: sind die zwei neuen C1-Regressionstests wirklich scharf? Durch gezielte Mutation in Sandbox-Kopie bestaetigt: 4/4 Mutanten gefangen (A1-Guard entfernt -> Test 1 rot, A2-Guard entfernt -> Test 2 UND Test 1 rot, V3-FAIL-Guard entfernt -> Test 1 rot, N1-Marker-Zeile auskommentiert -> Test 2 rot). Test 2 vergleicht nachweislich den vollen serialisierten Dateiinhalt (strictEqual auf rohen JSON-String, nicht nur ein Feld), Marker-Assertion ist positionsgenau (Regex erzwingt Marker auf derselben Zeile wie die Meldung). N1 (Dry-Run-Marker) auf allen 5 moeglichen Gate-Gedaechtnis-Zeilen per eigener CLI-Gegenprobe einzeln bestaetigt, N2/N3 bestaetigt. 3x Volllauf 104/104 identisch, keine Reihenfolgeabhaengigkeit fuer die neuen Tests, echtes Repo unangetastet. Fazit: Auflage C1 hat genau die in Runde 3 gefundene Luecke (Guards unbemerkt entfernbar bei gruener Suite) geschlossen. Fuenf Restpunkte (N4 Doku-Nutzungshinweis unvollstaendig, N5 Intra-Suite-Kopplung bei einem AELTEREN Test -- nicht den neuen C1-Tests --, N6 Dry-Run gibt bei V3-Fiktiv-Erfassung keinen Hinweis, B-Rest Gesamtbudget-Kappung bei extrem langen Werten, NEU N7 V1-AUSSICHTSLOS-Zeile im Dry-Run ohne Marker) bleiben offen, sind aber ausdruecklich reine Anzeige-/Doku-/Testhygiene ohne Gate-/Sizing-/Exit-Wirkung -- kein Commit-Hindernis. Diff ist aus Opus-Sicht commit-reif."
metadata:
  node_type: memory
  type: project
  status: "FREIGEGEBEN OHNE AUFLAGE 16.09.2026 -- Diff commit-reif, 5 nicht blockierende Restpunkte fuer eine spaetere Aufraeumrunde notiert. Code-Diff weiterhin unkommittiert (Levi). Hinweis: untracked Dateien unter scripts/ vor Commit auf .gitignore pruefen"
  originSessionId: session_current
  modified: 2026-09-16T11:09:03.305Z
---

# Opus-Gegencheck Runde 4: Schärfe der C1-Regressionstests + N1-N3 (16.09.2026)

**Prüfling:** unkommittierter Gesamtdiff gegen `cc680a4` — `scripts/gate_check.cjs` (+185), `scripts/vollcheck.cjs` (+47), `tests/trading_scripts.test.js` (+235/−39). **Methode:** eigener Testlauf (3×), `npm run test:all`, vier gezielte Mutationsproben in Sandbox-Kopie, eigene CLI-Gegenproben je N1-Meldezeile. Bezug: [[project_gegencheck_v1_v7_runde3_2026-09-16]] (Runde 3, fand Auflage C1).

## Gesamturteil: FREIGEGEBEN OHNE AUFLAGE — erste auflagenfreie Runde in dieser Kette

**Tests:** 104/104, dreimal identisch. `npm run test:all`: 133/134, einziger Fehler `e2e.test.js` (kein laufendes TradingView, umgebungsbedingt, unabhängig vom Diff).

## Das Wichtigste: sind die neuen Tests scharf? — BESTÄTIGT, 4/4 Mutanten gefangen

Gezielte Mutation in Sandbox-Kopie, je ein Guard entfernt, nur der zugehörige Test dagegen laufen lassen:

| Mutation | Ergebnis |
|---|---|
| A1-Guard entfernt (Zeile ~3723) | Test 1 ROT |
| A2-Guard entfernt (Zeile ~3929) | Test 2 ROT **und** Test 1 ROT |
| V3-FAIL-Guard entfernt (Zeile ~3809, Nachbesserung 4 aus Runde 1) | Test 1 ROT |
| N1-Marker-Zeile auskommentiert | Test 2 ROT |

Baseline (unveränderte Datei): beide Tests isoliert grün — die Rotfärbung ist also kausal, kein Artefakt. Damit sind jetzt alle drei Dry-Run-Schreibsperren plus der neue Marker regressionsgesichert — mehr, als Auflage C1 verlangt hatte.

## Test 1 (A1-Guard) — BESTÄTIGT, beide Zweige echt geprüft

Assertions prüfen wörtlich beide Zweige (Dry-Run-FAIL und Dry-Run-PASS+Q-ROT) auf `!existsSync`, dazu ein Live-Vergleich auf genau 1 Zeile. Kontamination durch Dedupe/Intra-Suite-Kopplung ausgeschlossen: Test beginnt mit `clean()`, assertiert Nicht-Existenz zweimal vor dem Live-Lauf, läuft isoliert grün. Kleine Randnotiz (kein Mangel): die eigentliche Trennschärfe sitzt in den `existsSync`-Assertions, nicht in der Zeilenzahl.

## Test 2 (A2-Guard) — BESTÄTIGT, echter Serialisierungsvergleich + positionsgenauer Marker

"Voller Inhaltsvergleich" ist `assert.equal` (strictEqual) auf den rohen JSON-String vorher/nachher, kein Feld-Stichprobencheck — der A2-Mutant zeigt das empirisch (Fehlermeldung enthält den kompletten Body). Die Marker-Assertion nutzt `[^\n]*` und erzwingt damit, dass Marker und Meldung auf derselben Zeile stehen — kein bloßer Substring-Treffer irgendwo im Output. Der eigentliche Runde-2-Schadensfall (AUSSICHTSLOS-SPERRE vs. ZEITBOX-KOLLISION) ist beidseitig abgesichert (`assert.match` + `assert.doesNotMatch`).

## N1 (Dry-Run-Marker) — BESTÄTIGT auf allen 5 möglichen Gate-Gedächtnis-Zeilen

Eigene CLI-Gegenprobe je Meldung (ZEITBOX-KOLLISION-Ersetzung, korrupte-Datei-Warnung, AUSSICHTSLOS-SPERRE VERLETZT, WIEDERHOLUNGSAUFRUF, Retest-Zeitbox-Hinweis) — Marker erscheint überall im Dry-Run, fehlt korrekt im Live-Lauf mit derselben Korruption. Fables "ALLE Gate-Gedächtnis-Zeilen" ist zutreffend.

**Neuer Randbefund N7 (kosmetisch, nicht blockierend):** Die V1-`AUSSICHTSLOS`-Zeile selbst (aus dem Retest-Zeitbox-Block, nicht dem Gate-Gedächtnis) trägt den Marker nicht — Fables Aussage bleibt literal richtig (andere Codestelle), aber ein Dry-Run-Aussichtslos-Lauf suggeriert eine Sperre, die mangels Schreibzugriff faktisch nicht scharf wird. Empfehlung für später: denselben Marker auch dort anhängen.

## N2/N3 — BESTÄTIGT

Doppelter `live: true`-Schlüssel entfernt (0 Treffer bei erneutem Scan), Kopfkommentar der Testdatei beschreibt jetzt korrekt das A2-Verhalten.

## Regressionscheck — BESTÄTIGT

3× Volllauf 104/104 identisch, keine Flakes. Beide neuen Tests sauber isoliert (räumen mit `clean()` auf, keine Nebenwirkung auf Folgetests). Echtes Repo am Ende byte-gleich zum Start.

## Offene Altpunkte aus Runde 1-3 (alle bereits als nicht blockierend eingestuft, unverändert bestehend)

- **N4:** Nutzungshinweis in `gate_check.cjs` nennt die neuen Dry-Run-Ausnahmen (A1/A2/V3) noch nicht.
- **N5:** Intra-Suite-Kopplung bei einem ÄLTEREN Test der 15.09.-Suite (nicht den neuen C1-Tests) — der "V2+V1: SL-Drift"-Test braucht einen Vorlauf.
- **N6:** Dry-Run gibt bei der V3-Fiktiv-Erfassung selbst keinen Hinweis auf das Überspringen (nur bei den Gate-Gedächtnis-Zeilen per N1 gelöst).
- **B-Rest:** Gesamtbudget-Kappung in `vollcheck.cjs` bei extrem langen Werten weiterhin unmarkiert/front-loaded (praktisch kaum erreichbar).
- **N7 (neu):** s. o.

Keiner dieser Punkte betrifft Gate-, Sizing- oder Exit-Verhalten — reine Anzeige-/Doku-/Testhygiene, gehören in eine spätere Aufräumrunde.

## Fazit zum Commit

**Ja — aus Opus-Sicht ist ein guter Punkt erreicht, nichts Substanzielles fehlt mehr.** Genau die in Runde 3 gefundene Lücke (Guards unbemerkt entfernbar bei grüner Suite) ist geschlossen und durch Mutation bewiesen, nicht nur behauptet.

**Hinweis am Rande (kein Blocker):** die untracked Dateien unter `scripts/` (`last_*.txt`, `sl_anker_wechsel_log.jsonl`) lagen schon vor dieser Prüfrunde so vor — vor dem Commit kurz prüfen, ob sie in `.gitignore` gehören.

Vorgänger: [[project_gegencheck_v1_v7_runde3_2026-09-16]], [[project_gegencheck_v1_v7_runde2_2026-09-16]], [[project_gegencheck_v1_v7_fable_umsetzung_2026-09-16]], [[project_testtag_analyse_2026-09-15]] (V1-V7-Ursprung).
