---
name: project_testtag_analyse_2026-09-25
description: "Opus-Analyse Testtag 25.09.2026 (Loop-Start verspaetet, 0 Trades, Backtest 15:30-17:40 DE) — Besprechung mit Levi noch ausstehend (Montag)"
metadata:
  node_type: memory
  type: project
  originSessionId: 20681367-29a4-41ff-922e-2b23acdaf1a9
  modified: 2026-09-25T19:19:51.380Z
---

Testtag 25.09.2026 abgeschlossen (Loop-Stopp 20:00 DE, Exit 0 keine Position, CronDelete erledigt). Besprechung mit Levi steht noch aus (verschoben auf Montag).

## Ausgangslage
Loop startete WIEDER nicht planmaessig um 15:25 DE (Cron feuerte nicht, Session vermutlich durchgehend beschaeftigt) — zweiter Vorfall dieser Art nach dem ausgefallenen 24.09.-Testtag, siehe [[project_vollcheck_ausgabeformat_vereinfachung_todo_2026-09-16]] und die 24.09.-Vorgeschichte. Ab 17:43 DE nachgeholt, VC#1 live um 17:47 DE, dann VC#18-VC#27 (19:10-19:55 DE) + minuetlicher Cron bis 19:59 DE.

## Technische Auswertung (Regel-Freeze-Primaerfrage)
- Fiktiv-Modus-Guard griff korrekt (`testtag_modus` durchgehend "fiktiv" in vollcheck_state.json).
- **0 Eintraege** in kombi_fiktiv_log.jsonl UND skipped_setups_fiktiv.jsonl fuer 2026-09-25 — konsistent: Dual-Gate stand die GESAMTE Session 2/2 LONG bestaetigt, SL-Anker-Vorpruefung war jedes Mal TAUGLICH, aber die SL-Distanz-Diagnose (V5, Schwelle 4x ATR) lag durchgehend 4,6x-6,11x ATR — weit ueber TOT. Struktur-Anker (30452,65, gesetzt 18:01:03 DE) blieb hinter dem Impuls-Extrem (30672,85, 18:17:20 DE) zurueck — dasselbe "veraltete-Anker"-Muster wie in der 23.09.-FESTGEFAHREN-Analyse, siehe [[project_testtag_analyse_2026-09-23]].
- Q1/Q3-Uebergabe und BE-/Stall-Exit-Fiktivmechanik: nicht pruefbar heute, da kein einziger Live-Trigger stattfand.

## Zwei neue GAP-Befunde (Protokoll vs. Implementierung, keine Regeländerung)
1. **loop_archiv fehlt**: scripts/loop_archiv/ enthaelt nur 2026-09-23.txt, keine Datei fuer 2026-09-25. `grep -n "loop_archiv" scripts/vollcheck.cjs` = 0 Treffer — die installierte vollcheck.cjs schreibt entgegen der Cron-Prompt-Behauptung ("druckt automatisch ins Tagesarchiv") gar nicht dorthin. Gleiche Kategorie wie die bereits bekannten fehlenden `=== VC#N · Kurz ===`-Marker (siehe [[feedback_loop_tick_kadenz]] / Kurzblock-Konzept-Historie).
2. **protokoll_bilanz.cjs nicht lauffaehig ohne Protokolldatei**: Fuer 2026-09-25 existiert keine Markdown-Protokolldatei (kein trades/trading_2026-09-25.md o.ae.) — diese Session erzeugte nur Chat-Kurzfassungen. protokoll_bilanz.cjs verlangt zwingend `--protokoll <Pfad>` oder `--rechnung`. Der Tagesabschluss-Schritt "protokoll_bilanz.cjs ausfuehren" konnte damit strukturell nicht wie vorgesehen laufen.

**Warum wichtig:** beide Befunde betreffen die Beleg-/Auditierbarkeit des Loop-Betriebs selbst (nicht die Handelsregeln) — noch nicht besprochen, ob/wie behoben werden soll.

## Backtest 15:30-17:40 DE (Levis Zusatzauftrag, echte Kurse)
Adaptiert aus scripts/analyse/backtest_2026-09-24.cjs (jetzt scripts/analyse/backtest_2026-09-25.cjs). 18 von 27 Fuenf-Minuten-Slots mit Dual-Gate 2/2, 6 qualifizierte LONG-Momente (15:45-16:10 DE). Bei ALLEN 6 war der reguläre Struktur-Anker (V-ERST) nicht anwendbar — dasselbe Anker-Muster wie live. Nur 2 Momente (16:05, 16:10 DE) hatten ein loesbares TP1-Fenster; beide waeren SL-HIT (-1R) gegangen, sequenziell nur 1 hypothetischer Trade. Zusaetzlich: selbst im besten Fall Q-Score ROT (Q4 nicht erfuellt, Rundzahl-Ratio 0,16-0,19 gegen PDH 30536,25) — nach Regelwerk vermutlich ohnehin "ausgelassen" statt echter Trade.

Vorbehalt: verwendetes level_register.json trug Stand 18:06 DE (NACH dem untersuchten Fenster) — leichter Rueckblick-Bias, aber wie beauftragt "wie vorgefunden" verwendet.

## Einordnung
Kein neuer Befund gegenueber [[project_testtag_analyse_2026-09-23]] — bestaetigt das "veraltete-Anker"-Muster. Der verspaetete Loop-Start hat mit hoher Wahrscheinlichkeit KEINEN echten Trade verhindert. Regel-Freeze-Stand (0 fiktive Trades) unveraendert. Datenpunkt 1 von 5 fuer B6-Auswertung (30 fiktive Trades/5 Testtage) — bestaetigendes, kein neues Signal.

## Offene Punkte fuer die Montag-Besprechung
- Wiederholter Cron-Startausfall (jetzt 2x: 24.09. und 25.09.) — Ursache/Gegenmassnahme noch nicht besprochen, siehe [[feedback_testtag_start_verlaesslichkeit]].
- Die zwei neuen GAP-Befunde (loop_archiv, protokoll_bilanz.cjs) — offen, ob/wie behoben werden soll.
- Anker-Reset-Mechanik (X1-Diagnose ist reine Anzeige, kein automatischer Wechsel) — vierter Testtag in Folge mit demselben "veralteter Anker blockiert TP1-Fenster"-Muster (17.09., 21.09., 23.09., jetzt 25.09.) — moeglicher Kandidat fuer eine kuenftige Regeldiskussion NACH Ende des Regel-Freeze.
