---
name: feedback-testtag-start-verlaesslichkeit
description: "Fiktiver-Testtag-Start scheitert wiederholt (24.09. + 25.09.2026) trotz geplantem Tagesablauf — zwei unabhängige Ursachen, beide erst spät bemerkt"
metadata:
  type: feedback
  originSessionId: session_2026-09-25
  modified: 2026-09-28T09:14:25.078Z
---

Der geplante Start eines fiktiven Testtags ist zweimal in Folge nicht wie vorgesehen passiert, obwohl der Tagesablauf explizit festgehalten war ([[feedback_trading_zeitfenster]] "Tagesablauf-Fixpunkte für Sonnet").

**24.09.2026:** Levi war 15:10-18:51 DE nicht am Rechner, Sonnet hat nur erinnert (Trigger-Regel: kein automatischer Start ohne explizites "start update dich"/Bestätigung), keine Bestätigung kam → Testtag fiel komplett aus, nachträglich nur ein Backtest möglich (siehe [[project_testtag_2026-09-23_besprechung_ausstehend]] Abschnitt 4).

**25.09.2026:** Diesmal wurden für 15:10/15:25/20:00 DE explizit CronCreate-Jobs gesetzt (inkl. minütlichem Tick-Job ab 15:25 nach [[feedback_loop_tick_kadenz]]). Der 15:25-Job feuerte, der Loop-Start-Pflichtblock wurde korrekt als Cron-Prompt gesetzt — aber der **minütliche Tick-Job lief trotzdem nicht**: zwischen 15:25 und 17:43 DE wurde die Session durchgehend mit anderen Tool-Aufrufen beschäftigt (u.a. das Setzen des riesigen Cron-Prompts selbst), CronCreate-Jobs feuern laut Tool-Beschreibung aber nur, wenn die REPL idle ist. Levi kam um 17:43 DE zurück und fand keinerlei Aktivität seit 15:25 vor — `vollcheck_state.json` stand noch auf dem 23.09.-Stand.

**Korrektur 28.09. (Levi):** Ursache am 25.09. war NICHT (nur) eine beschäftigte Session, sondern eine offene Entscheidung/Rückfrage, die Levi nicht beantworten konnte, weil er nicht am PC war — die wartende Session ist nicht idle, also feuert der Cron nicht. Gegenmaßnahme: Levi schaltet vor dem Weggehen Remote Control ein, um Rückfragen am Handy zu beantworten. Bei ausgeschaltetem/fehlendem Remote Control ist der Cron-Start ohne Levi nicht verlässlich.

**Why:** Zwei völlig unabhängige Fehlerquellen (fehlende Bestätigung vs. Cron feuert nicht während einer durchgehend beschäftigten Session) erzeugen dasselbe Symptom: Levi merkt den Ausfall erst Stunden später, der Testtag ist für die B6-Auswertung (30 fiktive Trades aus 5 Testtagen, [[project_testtag_2026-09-23_besprechung_ausstehend]] Abschnitt 5) dann nur noch per Backtest teilweise rekonstruierbar, nicht live gemessen.

**How to apply:** Nach dem Setzen eines Testtag-Start-Crons NICHT davon ausgehen, dass er zuverlässig unbeaufsichtigt durchläuft, solange die Session selbst in diesem Zeitraum mit langen eigenen Tool-Ketten beschäftigt sein könnte. Konkret: (a) kurz nach der geplanten Startzeit aktiv gegenchecken, ob `vollcheck_state.json`/`quick_tick_log.jsonl` tatsächlich frische Einträge bekommen (nicht nur `CronList` prüfen — ein gelisteter Job sagt nichts darüber, ob er auch feuert), (b) falls die Session absehbar mit einer längeren eigenen Aufgabe beschäftigt sein wird, Levi vorher aktiv darauf hinweisen statt sich auf den Cron zu verlassen, (c) bei einer Lücke wie am 25.09. den fehlenden Zeitraum (hier 15:30-17:40 DE) im Opus-Tagesbericht explizit rückwirkend nachrechnen lassen (Backtest-Vorlage [[project_testtag_2026-09-23_besprechung_ausstehend]] Abschnitt 4), statt die Lücke stillschweigend zu lassen.
