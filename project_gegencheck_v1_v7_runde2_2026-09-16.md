---
name: project_gegencheck_v1_v7_runde2_2026-09-16
description: "Opus-Gegencheck Runde 2 (16.09.2026) der Fable-Korrektur zu Auflage 1+2 aus [[project_gegencheck_v1_v7_fable_umsetzung_2026-09-16]]. FREIGEGEBEN MIT AUFLAGEN, kein Blocker -- beide Runde-1-Auflagen unabhaengig live bestaetigt (inkl. eines vom Pruefer selbst konstruierten Falls, den Fable nicht getestet hatte: fallendes ATR 30->15 macht ein reales PASS moeglich, Sperre hebt sich jetzt korrekt auf). V1-Kernfunktion nicht geschwaecht (5/5 Wiederholungen mit nahezu identischen Werten weiterhin gesperrt). 102/102 Tests eigenstaendig nachgefahren. 2 NEUE, nicht blockierende Auflagen gefunden: A) Dry-Run-Inkonsistenz an zwei Stellen -- der PASS/Q-Logger schuetzt Freigaben NICHT gegen --dry-run (nur V3-FAIL-Zeilen wurden das per Nachbesserung 4), verzerrt die 7b1a-Messreihe permissiv; UND last_gate_fail.json (Traeger der V1-Sperre) wird bei --dry-run ueberschrieben, ein Probelauf mit anderer Geometrie kann die Sperre still entschaerfen und durch eine gegenteilige ZEITBOX-KOLLISION-Meldung ersetzen (begrenztes Risiko, Live-Loop + --sl-vorpruefung betroffen nicht). B) vollcheck.cjs stderr-Mitschnitt kappt pro Zeile bei 400 Zeichen statt nur das 2000-Gesamtbudget durchzusetzen -- schneidet ausgerechnet die A3-Pflichtfelder-Liste (haeufigster Hard-Exit) nach 27 von 40 Feldern ab, obwohl das Gesamtbudget nur zu <1/3 genutzt wird."
metadata:
  node_type: memory
  type: project
  status: "FREIGEGEBEN MIT AUFLAGEN 16.09.2026 -- Auflage A+B von Fable umgesetzt (16.09., Tests 102/102), Kosmetik v4.ts erledigt, GATE_BASE-Test-Smell bewusst offen; Code-Diff weiterhin unkommittiert (Levi)"
  originSessionId: session_current
  modified: 2026-09-16T10:19:10.791Z
---

# Opus-Gegencheck Runde 2: Korrektur der Auflagen 1+2 (16.09.2026)

**Prüfling:** unkommittierter Diff `scripts/gate_check.cjs` (+167/-14), `scripts/vollcheck.cjs` (+42), `tests/trading_scripts.test.js` (+145/-9) — der jetzige Endzustand als Ganzes. **Methode:** eigener Testlauf + 14 eigene CLI-Gegenproben in Sandbox-Kopie, echtes Repo unangetastet. Bezug: [[project_gegencheck_v1_v7_fable_umsetzung_2026-09-16]] (Runde 1).

## Gesamturteil: FREIGEGEBEN MIT AUFLAGEN, kein Blocker

**Tests:** `node --test tests/trading_scripts.test.js` → 102/102 eigenständig bestätigt, plus `pine_analyze.test.js`+`cli.test.js` 29/29.

## Auflage 1 (Runde 1) — BESTÄTIGT behoben

Eigenes Repro (clusterGate-FAIL → identische Geometrie danach PASS+Q-GELB): 2 Zeilen entstehen, FAIL-Zeile bleibt `wiederholungen=0` unverändert, Freigabe bekommt eigene Zeile mit `q_ampel`/`q2_anker`. `--auswertung-q2a` meldet danach korrekt "Freigaben: 1" statt der vorherigen "0". Zusätzlich geprüft: FAIL nach bestehender Freigabe (Freigabe bleibt unberührt, neue FAIL-Zeile) und Freigabe nach bestehendem FAIL ohne erneuten FAIL — beide Richtungen symmetrisch korrekt, keine Kollision mit `manuell`/`Q-ROT`/`Q-UNBEKANNT`-Einträgen anderer Logger.

## Auflage 2a (Text-Differenzierung) — BESTÄTIGT, kein toter Zweig

Eigene Herleitung: `unsolvable` hängt am Entry, `entryFenster.leer` nicht — die Mengen sind nicht deckungsgleich (Bereich `3·ATR < entry−sl < tp1−sl ≤ 6·ATR` macht `unsolvable` wahr und `leer` falsch). Fables Anmerkung zum eigenen 200-Pkt-Testfall war korrekt, aber nur ein Spezialfall. Eigener Gegenbeweis mit dem 15.09.-Muster (eingefrorener Anker, TP1 näher am Markt) zeigt: der schwächere "Retest hilft"-Text ist live erreichbar und widerspruchsfrei zur bestehenden UNLOESBAR-Zeile — genau der Widerspruch aus Runde 1 ist weg.

## Auflage 2b (ATR im Fingerabdruck + Neuberechnung) — BESTÄTIGT

Fables ATR-30→80-Fall unabhängig bestätigt (Sperre hebt sich auf). Zusätzlich der in Runde 1 beanstandete Fall selbst konstruiert (ATR fällt 30→15, dadurch wird ein reales PASS möglich) — auch hier hebt sich die Sperre jetzt korrekt auf: Lauf 2 endet mit `GESAMTSTATUS: PASS`, keine Sperr-Zeile mehr. Das ist der Schaden aus Runde 1, jetzt behoben.

## Regressionscheck — BESTÄTIGT, V1 nicht geschwächt

6 Läufe mit dem 15.09.-Muster (nahezu identische Werte, minimale Drift) → 5/5 Wiederholungen weiterhin per AUSSICHTSLOS-SPERRE geblockt, keine einzige fälschliche ZEITBOX-KOLLISION. Die "Neuberechnung ist die Wahrheit"-Logik höhlt die Kernfunktion nicht aus.

## Zwei NEUE, nicht blockierende Auflagen (vor dem nächsten fiktiven Testtag)

**Auflage A — Dry-Run-Inkonsistenz an zwei Stellen:**
1. Der PASS-/Q-Logger in `gate_check.cjs` hat (anders als V3 seit Nachbesserung 4) keinen `--dry-run`-Schutz — ein Probelauf mit PASS+Q-GELB/GRÜN wird weiterhin als Freigabe gezählt, während ein Probelauf mit FAIL seit Nachbesserung 4 nicht mehr gezählt wird. Verzerrt die 7b1a-Messreihe (Echtgeld-Kriterium) in permissive Richtung.
2. `last_gate_fail.json` (Träger der neuen V1-Sperre) wird ebenfalls bei `--dry-run` überschrieben — ein Probelauf mit anderer Geometrie zwischen zwei Live-Aufrufen entschärft die Sperre still und ersetzt die Meldung durch die gegenteilige "ZEITBOX-KOLLISION — neuer Trigger ERSETZT". Live nachgewiesen. Risiko begrenzt: der reguläre Loop ruft nicht mit `--dry-run`, und `--sl-vorpruefung` fasst die Datei nachweislich nicht an — der Pfad braucht einen manuellen Probelauf zwischen zwei Live-Aufrufen.

**Auflage B — V4-Zeilenkappung zu aggressiv:**
Der stderr-Mitschnitt in `vollcheck.cjs` kappt jede einzelne Zeile bei 400 Zeichen, bevor das 2000-Zeichen-Gesamtbudget greift. Beim häufigsten Hard-Exit (A3-Pflichtfelder-Liste) bricht das ausgerechnet die Feldliste nach 27 von 40 Feldern mitten im Wort ab, obwohl vom Gesamtbudget nur 617/728 Zeichen genutzt wurden — genau die Information, für die V4 gebaut wurde. Vorschlag: Zeilenkappung anheben oder streichen, nur Gesamtbudget durchsetzen.

## Kosmetik/Info (kein Auflagenrang)

- `v4.ts` trägt bei frühen Hard-Exits die echte Systemzeit, sonst den `--jetzt`-Anker — zwei Bedeutungen in einem Feld, ein separates `run_ts` wäre sauberer für eine Primärquelle.
- `stateDiagStderr()` schreibt bewusst per `fs.writeSync(2,…)` statt `console.error` (Windows-TTY-Grund) und wird vom neuen Mitschnitt-Wrapper nicht erfasst — bewusste Bestandskonstruktion, keine Regression, aber relevant falls das Log vollständige Primärquelle sein soll.
- `GATE_BASE`/`parseArgs`-"letztes Argument gewinnt" ist ein milder Test-Smell (Fables Auflage-1-Test hängt `--cluster-level`/`--q1-reject` als letztes Argument an), aber ungefährlich — bräche laut, nicht still, falls `parseArgs` sich je ändert. Empfehlung: `GATE_BASE` um `o.cluster`/`o.q1Reject` erweitern statt auf Flag-Präzedenz zu bauen.
- Nachbesserungen 5+6 aus Runde 1 bestätigt (Doku, V5-Schwellenformulierung, `#6b`-Wortlaut, Schreibfehler-Warnung).
- `MEMORY.md` trug an einer Stelle noch "100/100" statt "102/102" — Indexkosmetik.

**Stand:** Code-Diff weiterhin unkommittiert. Aus Opus-Sicht spricht nichts gegen den Commit — Auflage A+B können vor dem nächsten fiktiven Testtag nachgezogen werden, sind aber kein Grund, den Commit selbst zurückzuhalten.

Vorgänger: [[project_gegencheck_v1_v7_fable_umsetzung_2026-09-16]] (Runde 1), [[project_testtag_analyse_2026-09-15]] (V1-V7-Ursprung).

## Umsetzung Auflage A+B (Fable, 16.09.2026 — Tests 102/102 gruen, Code unkommittiert)

- **A1:** Der komplette ROT/UNBEKANNT/Freigabe-Logger traegt jetzt denselben `--dry-run`-Schutz wie die V3-FAIL-Erfassung (ein Probelauf ist kein Setup-Moment, egal mit welchem Ausgang) — die 7b1a-Messreihe kann nicht mehr permissiv verzerrt werden.
- **A2:** `last_gate_fail.json` wird bei `--dry-run` nur noch GELESEN, nie geschrieben — ein Probelauf kann die V1-Sperre nicht mehr still entschaerfen oder eine ZEITBOX-KOLLISION vortaeuschen. Anzeige-Zeilen bleiben auch im Dry-Run erhalten.
- **B:** vollcheck.cjs-stderr-Mitschnitt ohne Zeilenkappung, nur noch das 2000-Zeichen-Gesamtbudget — die A3-Feldliste kommt vollstaendig ins Log.
- **Kosmetik:** `vollcheck_log.jsonl` trennt jetzt `ts` (--jetzt-Zeitanker, null bei fruehem Hard-Exit) von `geschrieben` (echte Schreibzeit). MEMORY.md-Zahl war bereits auf 102/102 aktualisiert. Der GATE_BASE/parseArgs-Test-Smell (angehaengte Parameter ueberschreiben Basiswerte) bleibt bewusst offen — nicht zeitkritisch laut Bericht.
- **Folgeanpassung Tests:** 7 Alt-Tests der Suiten 11.09./Q2-Q4/Option-D verliessen sich auf das alte Verhalten "dry-run schreibt fiktiv-Log/Gedaechtnis" — 21 GATE_BASE-Aufrufe dieser Tests auf `live: true` umgestellt (Testabsicht war das Logging selbst, nicht der Dry-Run) plus 4 Aufraeumzeilen fuer V9-Altreferenzen (`last_sl_vorpruefung.json` aus frueheren Suiten traf jetzt auf den Live-V9-Guard).
