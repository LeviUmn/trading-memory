---
name: feedback-sonnet-eigenbericht-unzuverlaessig
description: "Sonnets Tagesbericht/Zahlenbilanz aus dem Live-Loop-Gedaechtnis ist am Testtag-Ende unzuverlaessig (5 Fehlbehauptungen am 21.09., alle von Opus gegen die Logs korrigiert) — nie als Quelle fuer Zaehlungen zitieren, immer aus den Skript-Logs neu rechnen"
metadata: 
  node_type: memory
  type: feedback
  modified: 2026-09-21T19:17:52.030Z
  originSessionId: 9aca5bf5-bdfc-4bb6-922b-17583cc90312
---

Sonnets eigener Abschlussbericht am Ende eines langen Loop-Tages (Zahlenbilanz, Ereigniszusammenfassung) darf nie als Quelle für eine Analyse oder Zählung dienen — er ist strukturell fehleranfällig, unabhängig von gutem Willen oder Sorgfalt im Moment. Immer aus den maschinellen Logs (`vollcheck_log.jsonl`, `gate_check_log.jsonl`, `sl_anker_wechsel_log.jsonl`, `vollcheck_state.json` etc.) neu rechnen, nie aus dem Chat-Gedächtnis übernehmen.

**Why:** Am Testtag 21.09.2026 hat [[project_testtag_analyse_2026-09-21]] fünf konkrete Fehlbehauptungen in Sonnets Eigenbericht gefunden und gegen die Logs korrigiert:
1. Loop-Fenster als "14:00:30 bis 20:00:34 DE" berichtet — tatsächlich eine UTC/DE-Verwechslung (Z-Stempel als Ortszeit gelesen), korrekt 16:00:30 bis 19:50:30 DE.
2. "mind. 4 Live-Gate-Läufe" behauptet — tatsächlich 12.
3. Nur die letzten zwei von tatsächlich 5 vollzogenen SL-Anker-Resets genannt (die ersten drei lagen außerhalb des noch sichtbaren Chat-Kontextfensters).
4. "k-ohne-signal blieb den ganzen Tag bei 0" behauptet — tatsächlich erreichte k in 12 Voll-Checks ≥2/2 (Maximum 9), inkl. zwei echter 13.1×Q-ROT-Kollisionen.
5. Eine Tweet-Check-Prozesslücke als "Artefakt der fiktiven Testtag-Simulation" erklärt — tatsächlich ein strukturelles Toleranzfenster-Problem (`x_fetch_stamp.cjs`), das an einem echten Live-Tag mit derselben Prüftiefe identisch aufgetreten wäre.

Das Muster hinter allen fünf Fehlern: Ereignisse, die vor dem noch sichtbaren Chat-Kontextfenster lagen (v.a. nach einer Kontext-Kompaktierung) oder eine Zeitzonen-Umrechnung erforderten, wurden aus einem unvollständigen/fehlerhaften Gedächtnis rekonstruiert statt nachgeschlagen.

**How to apply:** Gilt für [[feedback_modellwahl_trading]] (Opus macht immer die Testtag-Analyse) — genau deshalb ist diese Rollentrennung richtig gebaut: Opus arbeitet grundsätzlich aus den Logs, nie aus dem Eigenbericht, und das trägt die Analysequalität auch wenn der Eigenbericht fehlerhaft ist. Für Sonnet selbst: bei jedem Faktenprotokoll-Abschluss oder jeder Zahlenaussage über den Tagesverlauf lieber explizit sagen "aus dem Chat-Verlauf, nicht verifiziert" statt eine Zahl mit Bestimmtheit zu nennen, wenn sie nicht gerade frisch aus einem Tool-Aufruf/Log stammt. Siehe auch [[feedback_zeitzone]] für die UTC/DE-Konvention, die Fehler 1 verursacht hat.
