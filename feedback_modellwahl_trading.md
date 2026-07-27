---
name: feedback-modellwahl-trading
description: "Sonnet 5 für aktives Trading (1-Min-Loop, Quick-Tick + 5-Min-Voll-Check), Fable 5 immer für Analyse/Reviews/Tagesabschlüsse — User-Entscheidung 27.07.2026 nach Fable-Konsultation zu Opus 5"
metadata:
  type: feedback
  originSessionId: session-2026-07-27
  modified: 2026-07-27T10:32:16.136Z
---

## Modellwahl-Regel: aktives Trading vs. Analyse/Review

**Regel:** Sonnet 5 bleibt Engine für alles zeitkritische im aktiven Trading (1-Minuten-Loop: sowohl Quick-Tick als auch der 5-Minuten-Voll-Check, siehe [[feedback_loop_ablauf_uebersicht]]). Fable wird **immer** für Analyse, Reviews und Tagesabschlüsse hinzugezogen — unabhängig davon, ob es zeitkritisch wäre oder nicht.

**Why:** Am 27.07.2026 kam die Frage auf, ob Opus 5 (neu released 24.07.2026, laut Anthropic nahe an Fable-Performance bei halbem Preis) oder Fable 5 besser als Trading-Engine geeignet wären. Ein per Agent-Tool hinzugezogenes Fable-Review empfahl ursprünglich eine Aufteilung nach Zeitkritikalität (Sonnet/Opus für den Loop inkl. Voll-Check, Fable nur für nicht-zeitkritische Reviews) — u.a. weil Fable als Flaggschiff-Modell vermutlich mehr interne Denkzeit pro Tool-Call-Kette braucht (unbestätigt, keine echten Benchmarks verfügbar) und das bei zeitkritischen Entry-/Exit-Entscheidungen zum Verpassen von Gelegenheiten führen könnte. Levi hat sich bewusst für eine einfachere, klare Trennung entschieden: Sonnet für ALLES aktive Trading (auch den 5-Min-Voll-Check), Fable ausschließlich für nachträgliche Analyse/Reviews/Tagesabschluss — das entspricht ohnehin dem bisherigen Muster (alle bisherigen Fable-Reviews liefen nachträglich, nie mitten im Loop).

**How to apply:** Bei jedem Tool-Aufruf des Agent-Tools für Trading-Analyse/Reviews/Tagesabschluss `model: "fable"` verwenden (wie in diesem Chat demonstriert). Der aktive 1-Min-Loop (egal ob Quick-Tick oder 5-Min-Voll-Check mit MTF/QQQ/Chartmuster) läuft immer auf dem Standardmodell der laufenden Session (aktuell Sonnet 5), nie mit Fable-Subagent-Delegation.

**Offener Punkt — Fable-Verfügbarkeit:** Fable 5 hat Stand 27.07.2026 kein angekündigtes Deprecation-Datum (anders als z.B. Opus 4.1, Retirement 05.08.2026; Sonnet 4/Opus 4 bereits am 15.06.2026 retiriert). Sollte Anthropic eine Fable-5-Deprecation ankündigen, muss diese Regel neu bewertet werden — bei jedem größeren Modell-Review (z.B. nächster Phasenübergang) kurz gegenchecken, ob eine Deprecation-Ankündigung für Fable vorliegt.
