---
name: project_gegencheck_v1_v7_fable_umsetzung_2026-09-16
description: "Opus-Gegencheck 16.09.2026 der Fable-Umsetzung von V1-V7 aus der Testtag-15.09.-Analyse (scripts/gate_check.cjs, scripts/vollcheck.cjs, tests/trading_scripts.test.js, unkommittiert). Eigener Testlauf 100/100 bestaetigt, 7 eigene CLI-Gegenproben in Sandbox-Kopie. FREIGEGEBEN MIT AUFLAGEN, kein Blocker im Sinne von 'Regel/Test kaputt', aber 2 Auflagen mit echtem Live-Schaden falls unbehandelt: (1) V3-Auto-Erfassung an fiktiven Tagen schluckt PASS-Q-GELB/GRUEN-Freigaben in den PASS-Dedupe, live nachgewiesen dass --auswertung-q2a danach 'Freigaben: 0' meldet -- verfaelscht das 7b1a-Kriterium vor Echtgeld-Start; (2) V1-Sperrtext ist im unsolvable-Zweig sachlich falsch (behauptet entry-unabhaengig, ist es nicht -- naeherer Entry heilt es live nachweisbar) und widerspricht der bestehenden UNLOESBAR-Zeile im selben Block; zusaetzlich fehlt ATR im Sperr-Fingerabdruck, wodurch die Sperre nach einem ATR-Rueckgang faelschlich weiterhin ein reales PASS blockiert (live nachgewiesen). V2/V5/V6/V7 ohne Einschraenkung bestaetigt (V2-Toleranz haette den realen 15.09.-Fall mit 2-3x Sicherheitsabstand getragen, Testwert-Aenderung 5->3-4 ist KEINE Verschleierung sondern traf einen aelteren TODO-1-Guard). 4 weitere Nachbesserungen (V4 stderr-Mitschnitt nur letzte Zeile, V3 --dry-run nicht ausgenommen, Doku-Ergaenzung Tagesabschluss, Kosmetik)."
metadata:
  node_type: memory
  type: project
  status: "FREIGEGEBEN MIT AUFLAGEN 16.09.2026 -- Auflagen 1+2 UND Nachbesserungen 3-6 von Fable umgesetzt (16.09., Tests 102/102), Code-Diff weiterhin unkommittiert, Commit-Entscheidung bei Levi"
  originSessionId: session_current
  modified: 2026-09-16T09:52:07.242Z
---

# Opus-Gegencheck: Fable-Umsetzung V1-V7 (16.09.2026)

**Grundlage:** vollständiger `git diff` der drei geänderten Dateien im Code-Repo (`scripts/gate_check.cjs`, `scripts/vollcheck.cjs`, `tests/trading_scripts.test.js`, alle noch unkommittiert), eigener Testlauf, sieben eigene CLI-Gegenproben in einer Sandbox-Kopie (echtes Repo unangetastet). Bezug: [[project_testtag_analyse_2026-09-15]] Abschnitt 6 (V1-V7-Ursprung).

**Testlauf (eigenständig verifiziert):** `node --test tests/trading_scripts.test.js` → `tests 100 / suites 17 / pass 100 / fail 0`. Bestätigt.

## Gesamturteil: FREIGEGEBEN MIT AUFLAGEN, kein Blocker (keine bestehende Regel/kein bestehender Test bricht), aber 2 Auflagen mit echtem Live-Schaden falls unbehandelt

## Je Punkt

- **V1 (Aussichtslos-Sperre):** TEILWEISE. Mechanik funktioniert (eigene CLI-Gegenprobe bestätigt beide Zweige), aber der Sperrtext im `unsolvable`-Zweig ist sachlich falsch: er behauptet "KEIN Entry kann heilen", live nachgewiesen ist aber, dass ein näherer Entry (29450 statt 29500, identischer Anker/SL/TP1) das Fenster wieder öffnet — das widerspricht der bestehenden UNLOESBAR-Zeile im selben Ausgabeblock, die den Retest als Ausweg nennt. Zusätzlich fehlt ATR im Sperr-Fingerabdruck: bei gleichem Anker/SL/TP1 aber gesunkenem ATR (30→15) blieb die Sperre aktiv, obwohl derselbe Entry jetzt real ein PASS ergibt hätte — live nachgewiesen.
- **V2 (Fingerabdruck-Toleranz):** BESTÄTIGT. `±0,5×ATR`-Toleranz eigenständig an der Schwelle geprüft (0,30×ATR kein neuer Trigger, 0,70×ATR neuer Trigger). Die Testwert-Änderung 5→3-4 Punkte ist keine Verschleierung — der reale 15.09.-Fall (5 Pkt Spannweite bei ATR 20-30) wäre mit 2- bis 3-fachem Sicherheitsabstand abgedeckt gewesen; der ursprüngliche Testwert scheiterte an einem älteren, unabhängigen Guard (TODO-1, 11.09.).
- **V3 (Auto-Erfassung fiktiv):** TEILWEISE. Korrekt auf fiktiv-Modus begrenzt (live geprüft: `testtag_modus:"echt"` erzeugt keine Zeile). **Aber:** der ältere PASS-Freigabe-Dedupe filtert nicht gegen V3-FAIL-Zeilen — eine Q-GELB/GRÜN-Freigabe nach einem automatisch erfassten FAIL wird in dessen Zeile "geschluckt" statt eine eigene zu bekommen, live nachgewiesen mit dem Ergebnis, dass `skipped_fiktiv.cjs --auswertung-q2a` danach "Freigaben (GELB/GRUEN): 0" meldet, obwohl real eine Freigabe stattfand. Das verfälscht genau das Kriterium (7b1a), das vor dem nächsten Echtgeld-Start zählt. → **Auflage 1.**
- **V4 (`vollcheck_log.jsonl`):** TEILWEISE. Robust gegen Dry-Run, `--json`, Hard-Exit, Schreibfehler (alle einzeln nachgestellt). Mangel: der stderr-Mitschnitt behält nur die LETZTE `console.error`-Zeile — beim häufigsten Hard-Exit (A3-Pflichtfelder, mehrere `console.error` hintereinander) geht genau die Information verloren, für die V4 gebaut wurde (welche Felder fehlen). Bestehendes Vorbild (`gate_check_log.jsonl`) sammelt alle Zeilen. → Nachbesserung.
- **V5 (SL-Distanz-Diagnose >4×ATR):** BESTÄTIGT ohne Einschränkung. Schwellenprobe exakt an der Grenze (120,0/120,1/119,9 Pkt bei ATR 30) verhält sich korrekt, kein Gate-/Sizing-Einfluss.
- **V6 (Richtungswechsel-Log):** BESTÄTIGT, Testanpassung (3→5 erwartete Zeilen) ist gerechtfertigt — eigene Nachstellung zeigt exakt die zwei neuen Zeilen, keine kaschierte Regression, bestehender V9-Zweig unverändert.
- **V7 (Prozessregel Skript-Logs = Primärquelle):** BESTÄTIGT, reine Doku im Memory-Repo (`53cfbd7`), Wortlaut deckt Zusage inkl. Konfliktregel ab.

**Code-Qualität:** folgt durchgehend etablierten Mustern der Datei (atomischer Rewrite, `process.on('exit')`-Wrapper, P3-Diagnosezeilen-Format). Keine Vorzeichenfehler, kein Off-by-one, keine toten Parameter im schädlichen Sinn.

## Auflagen (vor dem nächsten fiktiven Testtag)

1. **V3/PASS-Dedupe entkoppeln:** Freigabe-Logger muss V3-FAIL-Zeilen ausschließen (symmetrisch zum bereits vorhandenen Filter in V3), sonst verschwindet eine Freigabe live aus dem 7b1a-Prüfkriterium.
2. **V1-Sperrtext korrigieren:** Für `fenster.unsolvable` die V5-Formulierung übernehmen ("mit dieser Entry-Geometrie tot, Weg ist der Retest = näherer Entry") statt der falschen "kein Entry heilt"-Aussage; zusätzlich ATR (oder das `leer`-Flag) in den Sperr-Fingerabdruck aufnehmen, sonst blockiert die Sperre nach einem ATR-Rückgang fälschlich ein reales PASS.

## Nachbesserungen (nicht blockierend)

3. V4: alle stderr-Zeilen mitschneiden statt nur der letzten (dem `gate_check_log.jsonl`-Muster folgen); optional `uncaughtException` abfangen.
4. V3: `--dry-run`-Läufe von der Auto-Erfassung ausnehmen (Konsistenz mit `gate_check_log.jsonl`).
5. Doku-Ergänzung: `feedback_tagesabschluss.md`/Abschnitt 7 sollte festhalten, dass V3-Zeilen im Tagesabschluss nachtragspflichtig sind und `protokoll_bilanz.cjs` deshalb künftig öfter mit Exit 1 endet.
6. Kosmetik: V5-Rundungsdarstellung an der Schwelle ("4x ATR > 4x ATR"), V2-Wiederholungszeilen-Wortlaut ("unveraendertem SL" trotz genannter Drift), V4-Schreibfehler ist still obwohl das Log seit V7 Primärquelle ist.

**Stand:** Code-Diff weiterhin unkommittiert (`scripts/gate_check.cjs`, `scripts/vollcheck.cjs`, `tests/trading_scripts.test.js`). Commit-Entscheidung bei Levi — Opus sieht nichts, das GEGEN einen Commit spricht, empfiehlt aber Auflage 1+2 vor dem nächsten fiktiven Testtag umzusetzen, weil sie sonst live Schaden anrichten (Auflage 1 verfälscht die Echtgeld-Freigabe-Messung, Auflage 2 kann ein reales PASS fälschlich sperren).

Vorgänger: [[project_testtag_analyse_2026-09-15]] (V1-V7-Ursprung), [[project_gegencheck_memory_aufraeumen_s2s3_w1w4_2026-09-16]] (vorheriger Gegencheck desselben Tages).

## Umsetzung der Auflagen (Fable, 16.09.2026 — Tests 102/102 gruen, Code unkommittiert)

- **Auflage 1:** PASS-Dedupe schliesst V3-FAIL-Zeilen jetzt aus (`!(e.ablehnungsgrund startsWith "FAIL")`, symmetrisch zum V3-Filter) — eine Q-GELB/GRUEN-Freigabe nach automatisch erfasstem FAIL desselben Setups bekommt ihre eigene Zeile mit q_ampel/q2_anker. Neuer Testfall stellt das Opus-Szenario nach (clusterGate-FAIL → PASS+Freigabe → 2 Zeilen, FAIL-Zeile wiederholungen=0).
- **Auflage 2:** (a) V1-Sperrtext differenziert: `unsolvable` (ohne leeres Entry-Fenster) nutzt jetzt die V5-Formulierung ("mit DIESER Entry-Geometrie tot, Weg ist der Retest = naeherer Entry"), nur `entryFenster.leer` behaelt die "kein Entry heilt"-Aussage. (b) ATR steht jetzt im Sperr-Fingerabdruck (Audit), und die Sperr-Zeile verlangt zusaetzlich, dass der AKTUELLE Lauf weiterhin aussichtslos ist (`result.aussichtslos`) — die Neuberechnung ist die Wahrheit, ein veraenderter ATR/naeherer Entry hebt die Sperre automatisch auf. Neuer Testfall: gleicher Anker/SL/TP1, ATR 30→80 → keine Sperr-Zeile mehr, normaler WIEDERHOLUNGSAUFRUF.
- **Nachbesserung 3:** V4-stderr-Mitschnitt sammelt jetzt ALLE console.error-Zeilen (je 400, gesamt 2000 Zeichen, ` | `-getrennt) statt nur der letzten.
- **Nachbesserung 4:** V3-Auto-Erfassung ausgenommen bei `--dry-run` (Konsistenz mit gate_check_log.jsonl).
- **Nachbesserung 5:** `feedback_tagesabschluss.md` haelt fest: V3-Zeilen sind nachtragspflichtig, `protokoll_bilanz.cjs` endet deshalb kuenftig oefter mit Exit 1 (gewollt).
- **Nachbesserung 6:** V5-Schwellendarstellung ("UEBER der 4x-ATR-Diagnose-Schwelle (X Pkt)" statt "4x ATR > 4x ATR"), #6b-Wortlaut praezisiert ("Drift innerhalb der 0,5x-ATR-Toleranz zaehlt als unveraendert"), V4-Schreibfehler schreibt jetzt eine sichtbare stderr-WARNUNG (V7-Primaerquelle) statt still zu bleiben.
- V2/V5/V6/V7 unveraendert (vom Gegencheck ohne Einschraenkung bestaetigt).
