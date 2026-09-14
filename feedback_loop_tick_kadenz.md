---
name: feedback-loop-tick-kadenz
description: "Im aktiven Live-/Testtag-Loop MUSS jede 1-Min-Kerze einen Quick-Tick und jede 5-Min-Kerze einen Voll-Check bekommen — Standardmechanik dafür ist ein minütlicher CronCreate-Job, NICHT ScheduleWakeup (Drift 89-93s gemessen)"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 56b9f40d-72c3-47d3-a120-9aa3871bcfb6
  modified: 2026-09-09T09:52:44.168Z
---

Im aktiven 1-Min-Loop (Live-Trading oder fiktiver Testtag, siehe [[feedback_live_trading]]) muss wirklich JEDE 1-Minuten-Kerze einen Quick-Tick bekommen und JEDE 5-Minuten-Kerze einen vollen Voll-Check — kein Ausdünnen auf 90s+-Intervalle "damit Zeit für die Verarbeitung bleibt".

**Why:** Levi hat das am 08.09.2026 zunächst korrigiert, nachdem `ScheduleWakeup` mit `delaySeconds: 90` statt 60 aufgerufen wurde. Die Korrektur auf `delaySeconds: 60` war aber selbst noch keine tragfähige Lösung: `ScheduleWakeup` produzierte real 89-93s Drift durch Dispatch-Overhead — das technische Minimum von 60s wird in der Praxis nicht eingehalten. Levi ist deshalb um 15:49 desselben Tages auf einen minütlichen `CronCreate`-Job (`*/1 * * * *`) umgestiegen, der danach von 15:51 bis 19:43 Uhr — fast vier Stunden, 46 Voll-Checks — wandzeitgenau ohne eine einzige ausgefallene Minute lief (siehe [[project_testtag_analyse_2026-09-08]], TODO 1 der dortigen Fable-Liste). `ScheduleWakeup` ist damit für diesen Zweck als **ungeeignet** erwiesen, nicht nur als vorher falsch konfiguriert.

**How to apply:** Für den Start eines Live-/Testtag-Loops IMMER `CronCreate` mit `*/1 * * * *` verwenden, nicht `ScheduleWakeup`. `ScheduleWakeup` bleibt für andere Zwecke (z.B. dynamische /loop-Wiederkehr außerhalb des Trading-Loops) geeignet, aber nicht für die minutengenaue Tick-Kadenz. Falls die eigene Verarbeitung eines Ticks länger als 60s dauert und dadurch eine Kerze de facto verpasst wird: NICHT versuchen, die verpasste Kerze rückwirkend nachzuholen (kein Zeitreise-Backfill) — einfach beim nächstliegenden, noch vor einem liegenden Tick regulär weitermachen, als wäre das die aktuelle Kerze. Voll-Checks bleiben an den 5-Min-Kerzenschluss gekoppelt (siehe bestehende Zählweise `(Min seit Start)/5+1` in den Testtag-Protokollen), Quick-Ticks an jede einzelne 1-Min-Kerze dazwischen. Beim Faktenprotokoll-Abschluss (siehe [[feedback_tagesabschluss]]) gehört `CronDelete` als Aufräumschritt dazu, damit der Job nicht über das Loop-Ende hinaus weiterläuft.
