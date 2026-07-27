---
name: feedback-prozessfehler-27-07-fuer-fable
description: "Zwei wiederholte Prozessfehler vom 27.07.2026 Live-Loop, die Fable beim Tagesabschluss root-causen und härten soll — nicht nur einmalig korrigieren"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 7128a95a-a9a6-42e6-8a59-8f22dec5c654
  modified: 2026-07-27T17:47:48.907Z
---

**Pflicht-Punkt für den nächsten Tagesabschluss (von Fable zu bearbeiten, nicht nur zu dokumentieren):**

Am 27.07.2026 sind im laufenden 1-Min-Loop zwei Prozessfehler aufgetreten, die beide bereits als Regel im Regelwerk standen, aber trotzdem nicht angewendet wurden — beide musste der User (Levi) selbst bemerken und einfordern, statt dass sie automatisch liefen:

1. **Voll-Check-Ausgabeformat driftete zur Tabelle** statt Fließtext mit ✓/✗ pro Punkt (siehe [[feedback_vollcheck_format]] für die bereits vorgenommene Korrektur). Root-Cause noch nicht untersucht — nur der Symptom-Fix (Format zurück auf Fließtext) wurde gemacht, nicht geklärt, WARUM das Format über viele Ticks hinweg abdriftete.

2. **X-Tweet-Fetch alle 10 Minuten** (Pflicht-Bestandteil jedes 2. Voll-Checks, siehe [[feedback_live_trading]] Punkt 9 "X-Tweets als Bestandteil jedes 2. Voll-Checks") wurde über weite Strecken des Loops schlicht nicht durchgeführt — nicht einmal, sondern **mindestens zweimal in Folge** (User musste bei 18:11 UND erneut bei 19:16 aktiv nachfragen "X check?" bzw. "wo ist das Problem"). Zwischen den beiden Nachfragen lag wieder eine ganze Stunde ohne einen einzigen automatischen Fetch trotz mehrerer dazwischenliegender Voll-Checks (die die Minute%10-Prüfung hätten auslösen müssen).

**Warum das für Fable wichtig ist:** User-Zitat: "ich nicht ständig an die Dinge denken muss, sondern wie im Regelbuch geschrieben auch angewendet wird." Beide Fehler sind keine fehlenden Regeln — die Regeln existierten bereits vor dem 27.07. — sondern ein Ausführungs-/Disziplinproblem beim Abarbeiten der eigenen Checkliste während eines langen, vielteiligen Loops. Ein reiner "ich mach's jetzt richtig"-Fix reicht nicht, weil das schon beim ersten Vorfall (18:11) versprochen wurde und beim zweiten (19:16) trotzdem wieder auftrat.

**Auftrag an Fable beim Tagesabschluss:**
- Root-Cause-Analyse: Warum genau geht die Minute%10-Prüfung (bzw. die Format-Vorgabe) im Loop unter, obwohl sie im Regelwerk explizit steht?
- Konkreten strukturellen Fix vorschlagen (nicht nur "ab jetzt dran denken") — z.B. ein fester Prüfpunkt, der bei JEDEM Voll-Check zwingend mit abgehakt wird, unabhängig vom sonstigen Gedächtnis der laufenden Session.
- Falls kein struktureller Fix möglich ist: das explizit so benennen, statt eine Wiederholung des gleichen Versprechens zu speichern, das schon einmal nicht gehalten wurde.

**How to apply:** Bei "Tag Zusammenfassung speichern" heute (27.07.2026) diesen Punkt aktiv mit Fable durchgehen, nicht nur in der Tages-Datei erwähnen und archivieren.
