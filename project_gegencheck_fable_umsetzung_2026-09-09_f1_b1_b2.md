---
name: project_gegencheck_fable_umsetzung_2026-09-09_f1_b1_b2
description: "Opus-Gegencheck der F1/F2-Fixes, B1 (save_path-Auflagen) und B2 (Retest-Zeitbox-Regeltext): F1/F2 bestätigt behoben, aber Opus fand + fixte selbst eine neue Regression G1 (Re-Run drehte frisch gesetzten Impuls still zurück). B1 bestätigt stark (54 Pfad-Missbrauchstests bestanden), Opus fand + fixte G2 (Stale-File-Falle bei veralteten qqq_*.json). B2 inhaltlich korrekt, aber G5 [Entscheidung nötig]: Zeitbox-Erweiterung feuert auch bei offener Position und behauptet dann fälschlich 'ausgelassen'. Status FREIGEGEBEN — G5 entschieden+verifiziert (Option d), TODO-7-Trockenlauf live nachgestellt und von Opus unabhängig nachgerechnet (15min/5min-Verdacht ausgeräumt, EMA-Mathematik gegen TradingView validiert, G8 gefunden+gefixt, G9 entschärft). K2/G9-Restfrage offen, kein Blocker."
metadata:
  node_type: memory
  type: project
  status: freigegeben
  originSessionId: f86d6469-f4c5-46be-9c92-a1637d9a3ef6
  modified: 2026-09-09T20:45:18.180Z
---

## Kurzfazit

Dritte Gegencheck-Runde des Tages (nach der initialen Testtag-Analyse und der ersten TODO-Umsetzung). Opus prüfte Fables Fixes zu F1/F2 (blockierender State-Defekt), B1 (`save_path`-Auflagen) und B2 (Retest-Zeitbox-Regeltext) unabhängig per Live-Tests und Missbrauchsversuchen. F1/F2 bestätigt behoben. Bei allen drei Punkten fand Opus jeweils einen zusätzlichen, von Fable nicht entdeckten Defekt/Fund — zwei davon (G1, G2) hat Opus selbst trivial gefixt, einer (G5) ist eine echte Regelentscheidung und bleibt offen.

## F1/F2 — bestätigt behoben + neue Regression G1 gefixt

Standardablauf (VC#1 `--impuls-pkt`, VC#2-4 ohne) und Tageswechsel live reproduziert: Exit 0, saubere Fortschreibung mit Herkunftsvermerk "FORTGESCHRIEBEN — kein Ursprung". Fib-Pflicht wirkt korrekt auf fortgeschriebenem Wert. Modus-Wechsel mitten im Loop (Docht→Ursprung und umgekehrt) bricht nichts.

**G1 [P1, von Opus gefixt]:** Re-Run (der dokumentierte Regelfall nach `register_touch.cjs`-Retry) drehte einen im Erstlauf desselben Voll-Checks neu gesetzten `--impuls-ursprung`/`--impuls-pkt` still auf den Vorwert zurück — Codereihenfolge-Fehler (`stateBase` vor `state.letzter` geprüft, Fallback griff nur wenn er nicht gebraucht wurde). Live reproduziert (2 Fälle), gefixt (2 Zeilen in `vollcheck.cjs`), regressionsgeprüft.

Restschwächen (nicht gefixt, Entscheidung/P2): **G3** — Fib-Zeile trägt bei fortgeschriebenem Impuls keinen Herkunftsvermerk, ein neuer größerer Impuls könnte unbemerkt übersehen werden, solange niemand `--impuls-ursprung` neu setzt. **G4** — Asymmetrie: ein manueller Wert ohne Ursprung überlebt beliebig lange, mit Ursprung nur einen Voll-Check (alter Pfad, durch F1-Fix nur sichtbarer geworden). Nebenbefund: `--dry-run` schreibt den State nicht fort — ein versehentlicher Dry-Run-Loop reproduziert das F1-Symptom, wird aber über den 1H-Schatten-Baustein (TODO 8) sichtbar.

## B1 — bestätigt stark + Stale-File-Falle G2 gefixt

54 gezielte Pfad-Missbrauchstests (Windows-Backslash-Traversal, NTFS-ADS, Null-Bytes, UNC/Extended-Length-Pfade, Homoglyphen, reservierte Gerätenamen, Zustandsdateien namentlich) — alle abgewiesen. Architektur (Whitelist statt Denylist, Basis aus `import.meta.url`, Validierung vor Chart-Zugriff) als richtig bestätigt.

Erfolgsfall (echtes Schreiben) war für Opus wie für Fable nicht live testbar — Ursache identifiziert: TradingView Desktop läuft aktuell gar nicht (`tv_health_check` → CDP-Verbindung fehlgeschlagen), zusätzlich hat der laufende MCP-Server noch das alte Tool-Schema geladen (kein `save_path`). Opus hat die Kette AB der Bar-Datei bewiesen (synthetische Datei im exakten Format von `core.getOhlcv` erzeugt → `atr_qqq.cjs --bars`/`--ema-bars` → `vollcheck.cjs --qqq-bars/--qqq-bars-5m`, alles Exit 0) — nur der CDP-Lesevorgang selbst (unveränderter Bestandscode) bleibt unverifiziert.

**G2 [P1, von Opus gefixt]:** Mit `save_path` entsteht `scripts/qqq_*.json` bei jedem Aufruf neu und bleibt liegen (gitignored, keine Bereinigung). Wird der Speicheraufruf vor der Rechnung vergessen, rechnet `atr_qqq.cjs` klaglos aus der alten Datei weiter — kein Hinweis, plausible aber falsche Zahl. Reproduziert (Datei vom Vortag, Stichzeit am Folgetag). Gefixt: neuer Frische-Guard in `atr_qqq.cjs` (Schwelle wiederverwendet aus Finalentscheidung #22: 2 Kerzenlängen + 2 Min), hängt sich additiv ins bestehende `warnings[]`-Feld ein, das `vollcheck.cjs`/`gate_check.cjs` bereits rendern.

## B2 — inhaltlich korrekt, aber G5 [Entscheidung nötig]

Q-Score-Schwelle "≥3/4 GELB/GRÜN" stimmt exakt mit der Ampel-Logik in `gate_check.cjs`/`feedback_live_trading.md` 7b1a überein, Code und Text sind sauber verzahnt.

**G5 [P1, Levi/Fable-Entscheidung nötig]:** Der Regeltext bindet die Zeitbox an "sofern der Trigger tatsächlich ausgelassen wurde" — der Code kann das aber nicht wissen und setzt `retestGrund: 'PASS+Q-ROT'` bei JEDEM Live-PASS mit Q-ROT, auch wenn der Trade genommen wurde (Q-ROT ist laut 7b1a in den ersten 8 Trades kein hartes Veto). Live nachgestellt: bei `--position offen` verlangt der nächste Voll-Check trotzdem `--retest-bedingung` (neuer Exit-1-Pfad genau dort, wo laut `feedback_positionsfokus_bei_offener_position.md` nur Risikoschutz geprüft werden soll) UND druckt "PASS+Q-ROT (ausgelassen)", obwohl die Position offen ist — eine Unwahrheit im Protokoll. Drei Optionen zur Wahl:
- (a) Zeitbox bei `--position offen` unterdrücken
- (b) neues Feld `--pass-q-rot-ausgelassen ja|nein` einführen
- (c) "sofern ausgelassen" aus dem Regeltext streichen, Box greift immer, `(ausgelassen)`-Text aus `retestGrundText` entfernen

Nicht von Opus entschieden (Regelwerksfrage, kein Trivialfix).

**G5 GELÖST (09.09.2026, spätabends):** Auf Levis Nachfrage hat Opus eine konkrete Empfehlung gegeben — Option (d), eine gezielte Kombination: Text "(ausgelassen)" streichen (c) UND A3-Pflicht bei offener Position aussetzen (gezielte a), ABER Verfall/Deadline bleibt unabhängig von `--position` unverändert bestehen (das hätte (a) pur falsch gemacht). Begründung: `position_tick.cjs` ist kein Alternativpfad zu `vollcheck.cjs`, sondern nur ein Zusatzaufruf — der Voll-Check inkl. Retest-Zeitbox läuft bei offener Position unverändert komplett durch, G5 war also ein echter Defekt. Levi hat (d) bestätigt, Fable hat es umgesetzt und in 8 Testszenarien verifiziert (Baseline-Defekt reproduziert, Normalfall ohne "(ausgelassen)"-Text, offene Position ohne Exit-1 mit "n.a."-Status, Verfall greift trotz offener Position sowohl innerhalb als auch außerhalb des 15-Min-Fensters, Einmaligkeit intakt). `feedback_chartanalyse.md` und `feedback_vollcheck_format.md` entsprechend nachgezogen.

**Von Sonnet direkt behoben (trivial, ohne Fable-Runde):** G6 (Fehlverweis "8b1a" → korrekt "7b1a" in `feedback_chartanalyse.md`), G7 (`feedback_vollcheck_format.md` Zeile 126/127 um PASS+Q-ROT-Fall ergänzt), F6 aus der Vorrunde (`scripts/skipped_setups_fiktiv.jsonl` zu `.gitignore` ergänzt).

## Von Opus angewendete Fixes (kein Commit)
- `scripts/vollcheck.cjs` — G1 (Re-Run-Vorrang für `state.letzter`)
- `scripts/atr_qqq.cjs` — G2 (Frische-Guard gegen veraltete Bar-Dateien)

## Vierte Gegencheck-Teilrunde: G5-Umsetzung (Option d) verifiziert

Opus hat Fables Umsetzung von Option (d) unabhängig geprüft: alle drei Klauseln bestätigt korrekt im Code (kein retrospektiver "(ausgelassen)"-Text mehr, `--retest-bedingung` nur ohne offene Position Pflichtfeld, Verfallsmechanismus positionsunabhängig) und entsprechen der ursprünglichen Opus-Absicht — auch die von Fable offen benannte Randentscheidung (Verfall gewinnt bei offener Position immer, auch bei `erfuellt`) wurde bestätigt.

**G6 [gefixt von Opus]:** Fables Behauptung, ein bei offener Position trotzdem übergebenes `--retest-bedingung` werde "nur informativ zitiert", stimmte nicht — es war toter Code, der Wert wurde in 100% der Fälle still verschluckt (0 Treffer für den Zitat-Text). Ursache: `resolveA3` überspringt Felder, deren `wenn()`-Bedingung false ist, bevor der Wert überhaupt gelesen wird. Gefixt (Lesequelle von `v[]` auf `args[]` umgestellt), nachgetestet ohne Seiteneffekt.

**K2 [P2, Entscheidung offen, kein Blocker]:** Pre-existing seit Commit `25b18e6`, durch den G5-Fix nur sichtbar geworden: Bei überschrittener Deadline OHNE offene Position gewinnt eine nachträglich als "erfuellt" gemeldete Bedingung noch vor der Deadline-Prüfung — die Wanduhr-Deadline ist im Fenster zwischen Deadline und dem 15-Minuten-Gesamtfenster durchbrechbar. Opus empfiehlt: Deadline-Prüfung vor die Bedingungsprüfung ziehen ("Deadline soll gewinnen"). Nicht selbst gefixt (Regelentscheidung).

Regelwerk nachgezogen (von Sonnet direkt, trivial): `feedback_chartanalyse.md` Zeilen 312/317 (Überschriften-Wortlaut eingeengt — "sofern keine Position offen" bezog sich unklar auch auf den Verfallsmechanismus, jetzt nur noch auf die Pflichtprüfung), `feedback_live_trading.md` Zeilen 697 und 923 (trugen noch den Stand vom 04.09. ohne PASS+Q-ROT-Erweiterung und G5-Korrektur).

**Status dieser Teilrunde: FREIGEGEBEN mit Auflagen** — Live-Loop-Pfad reif für den nächsten Testtag. K2 ist tolerierbar, solange bewusst so entschieden.

**TODO 7 Trockenlauf ERFOLGREICH (09.09.2026, spätabends, nach MCP-Neustart):** `tv_launch` neu gestartet (Schema jetzt mit `save_path`), 2-Pane-Layout (NAS100 5min + QQQ 15min) war bereits aktiv. `data_get_ohlcv save_path count=250` live für QQQ 15min UND 5min gespeichert (Pane danach wieder auf 15min zurückgesetzt). `atr_qqq.cjs --bars qqq_15m.json --ema-bars qqq_5m.json` lief ohne Fehler/Warnung: ATR(14,15min)=0.966, EMA50(5min)=716.307. `gate_check.cjs --dry-run --qqq-bars ... --qqq-bars-5m ...` (restliche Pflichtfelder per `--grund-*`-Dummy) berechnete beide Werte korrekt selbst (`--dual-gate-qqq-abstand-atr` und `--dual-gate-qqq-ema50-5min-diff-pct`), kein Fehler in diesem Codepfad — der Exit 1 kam ausschließlich von der unabhängigen `level_register.json`-Frische-Guard (255 Min alt, Obergrenze 90 Min, Finalentscheidung #22), nicht von TODO 7. Damit ist der zuvor unverifizierte CDP-Lesevorgang jetzt bewiesen; alle drei vorherigen Versuche waren am alten Tool-Schema/gestopptem TradingView gescheitert.

## Fünfte Gegencheck-Teilrunde: TODO-7-Trockenlauf unabhängig nachgeprüft (Opus)

Opus hat den von Sonnet gemeldeten Trockenlauf nicht geglaubt, sondern nachgerechnet und nachgestellt — **alle Zahlenbehauptungen bestätigt**:

- **Der 15min/5min-Verdacht ist ausgeräumt.** Der identische letzte Bar (`time=1788985800`, Close 716,25) in beiden Dateien war KEIN Bug: die Zeitabstände sind messbar verschieden (`qqq_15m.json` 245× 900 s, `qqq_5m.json` 248× 300 s). 20:30 UTC ist gleichzeitig 15min- und 5min-Bucket-Start, beide Dateien wurden im selben Moment gelesen — die laufende Kerze war deshalb zwangsläufig identisch. Zusätzlich selbst live gegengeprüft: eigener Timeframe-Wechsel auf 5 → letzter Bar 20:40 UTC (kein 15min-Raster) → Wechsel zurück auf 15 → letzter Bar wieder 20:30 UTC. `chart_set_timeframe` wirkt also nachweislich, `save_path` überschreibt nachweislich (zweiter Live-Read desselben Slots lieferte Close 716,36 statt 716,25).
- **Zahlen unabhängig nachgerechnet** (eigenes Node-Snippet, ohne die Projektmodule): EMA50(5min) = 716,3067 (Skript: 716,307), ATR(14, Wilder, 15min) = 0,96635 (Skript: 0,97 gerundet), `--dual-gate-qqq-abstand-atr` = 3,7062 (Skript: 3,706), `--dual-gate-qqq-ema50-5min-diff-pct` = −0,00234 (Skript: −0,002). Deckungsgleich.
- **EMA-Mathematik gegen TradingView selbst validiert:** die "Moving Average Exponential" auf der QQQ-15min-Pane zeigt 716,58; dieselbe Formel auf `qqq_15m.json` mit Länge 50 liefert 716,577 — Übereinstimmung auf die angezeigte Genauigkeit. `computeEma` reproduziert also den TradingView-Standardindikator, ohne dass ein Indikator ans Layout muss.
- **Exit 1 wirklich nur vom Register-Guard:** Kontrolllauf ganz OHNE `--qqq-bars`/`--qqq-bars-5m` endet mit identischer Meldung und Exit 1 (`level_register.json` 261 Min alt) — der TODO-7-Codepfad ist an dem Exit unbeteiligt. Bestätigt.
- **Missbrauchstests bestanden:** `save_path` + `summary=true` → Fehler; `scripts/../scripts/qqq_5m.json` → Traversal abgewiesen; falscher `symbol`-Guard → abgewiesen; in allen drei Fällen wurde die Zieldatei nachweislich NICHT angefasst. Fehlende Datei → Hard-Exit 1 mit Klartext. Veraltete Datei (Zeitstempel −1 Tag) MIT Stichzeit → Frische-Guard feuert korrekt ("1395 Min, Schwelle 32 Min"). Falsch benannte Datei (15min-Bars im 5m-Slot) → Warnung "Kerzenlaenge 15 Min statt 5 Min" feuert.

**G8 [P2, von Opus gefixt]:** Ein unparsebares `--qqq-at` wurde im `--qqq-bars-5m`-Block STILL verschluckt (fehlender else-Zweig, `gate_check.cjs` ~Z. 2696) — die EMA rechnete dann über ALLE Bars inkl. laufender Kerze, der Frische-Guard war aus, und die Protokollzeile behauptete "kein --qqq-at", obwohl eine Stichzeit übergeben wurde (Unwahrheit im Protokoll). Der `--qqq-bars`-Block daneben macht daraus seit jeher einen Hard-Exit; ohne `--qqq-bars` — genau der TODO-7-Fall, in dem nur die 5min-EMA fehlt — griff die Absicherung nicht. Live reproduziert, symmetrisch gefixt (Hard-Exit 1 mit eigener Meldung), regressionsgeprüft (Happy Path bitgleich).

**G9 [P2, von Opus entschärft, Restentscheidung offen]:** Ohne `--qqq-at` sind BEIDE Schutzmechanismen gleichzeitig aus — die Abschneidung auf abgeschlossene Kerzen UND der G2-Frische-Guard (`frischeWarnung` gibt ohne `atSec` immer `null` zurück). Eine tagealte Datei lief so völlig warnungsfrei durch und lieferte eine plausible falsche Zahl (ATR 0,835 statt 0,966 = 13,5 % daneben, `--dual-gate-qqq-abstand-atr` 4,291 statt 3,706 = 15,8 % daneben) — direkt in ein A3-Pflichtfeld. Entschärft: die "kein --qqq-at"-Texte in `gate_check.cjs` und `atr_qqq.cjs` weisen jetzt explizit aus, dass der Frische-Guard aus ist und die Bar-Zeit selbst geprüft werden muss. **Kein Blocker**, weil `vollcheck.cjs` `atSec` immer aus `--jetzt` ableitet und das erzeugte Übergabekommando immer `--qqq-at` enthält — der Loop-Pfad ist also immer scharf. Offen als Regelfrage (nicht von Opus entschieden): soll `--qqq-at` bei `--qqq-bars`/`--qqq-bars-5m` zur Pflicht werden, statt nur gewarnt zu werden?

## Status: FREIGEGEBEN

Beide vor dem nächsten Testtag nötigen Punkte sind erledigt: G5 entschieden+verifiziert (Option d) und TODO-7-Trockenlauf erfolgreich — letzterer von Opus unabhängig nachgerechnet und live nachgestellt, alle Zahlen bestätigt. Die Hochstufung ist gerechtfertigt. Offen, aber ausdrücklich KEIN Blocker: K2 (P2, Deadline vs. nachgemeldete Bedingung) und G9-Restfrage (P2, `--qqq-at` verpflichtend machen?). G8 wurde von Opus gefunden und gefixt.

Analyse: [[project_testtag_analyse_2026-09-09]], vorige Runde: [[project_gegencheck_fable_umsetzung_2026-09-09_todos_nach_testtag]]
