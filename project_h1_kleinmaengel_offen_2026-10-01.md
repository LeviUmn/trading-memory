---
name: project-h1-kleinmaengel-offen-2026-10-01
description: "Offene Kleinmaengel aus Opus-Gegencheck 4 (01.10.2026) zu h1_auswertung.cjs und H1-Datei — fuer Fable am 02.10. (Limit), nicht blockierend"
metadata:
  node_type: memory
  type: project
  originSessionId: 9f4b9f35-da7a-456c-9a16-094c4f451adb
  modified: 2026-10-01T13:11:24.380Z
---

Quelle: [[opus_gegencheck_4_2026-10-01]] (Fable-Auftrag VOLLSTAENDIG umgesetzt; Selbsttest 89/89, 2x 227/227, 27/27 Mutanten gefangen). Fable konnte die Kleinigkeiten wegen Usage-Limit nicht mehr umsetzen -> **am 02.10. von Fable nachholen lassen** (Opus: kein neuer Auftrag noetig, optional beim naechsten Edit; keine Aenderung an K1-K10, Statistik oder Gate-Code).

**Zu beheben (Kleinigkeiten, nicht blockierend):**
1. **B1:** `memory/project_h1_q2_trendkontext_vorabkriterium_2026-10-01.md` Z. ~163 (R3) sagt "651 Zeilen", das Skript hat 664 (Stand 01.10.) — Zahl aktualisieren (oder Zeilenzahl-Angabe streichen, sie veraltet bei jedem Edit).
2. **H-a:** `scripts/analyse/h1_auswertung.cjs` Kopf Z. 4-5 behauptet "SCHREIBT NICHTS"; seit S8 schreibt `--selbsttest` synthetische Logs in ein Temp-Verzeichnis und startet Kindprozesse (Z. 16-17 dokumentieren das korrekt) -> Kopf-Satz praezisieren ("nur --selbsttest schreibt synthetische Daten ins Temp-Verzeichnis, nie echte Logs"). Dazu `try/finally`, damit das Temp-Verzeichnis bei Abbruch des Selbsttests aufgeraeumt wird.

**Weitere Hinweise (Opus, ohne Auflage):**
- Stichtagssperre (`end` erst ab 30.10.2026 20:05 DE) nur absichtlich umgehbar (Systemuhr, `require(...).auswerten` im Echtmodus); "`zwischen` nur einmal" bleibt Disziplinsache.
- CLI-Test "Sperre vor dem Lesen der Logs" ist an die Systemuhr gebunden und laeuft nach dem 30.10. nicht mehr; die Tests mit injizierter Uhr decken die Logik weiter ab.
- Text-Selbsttest blendet Zeilen mit "HINWEIS:" aus — ein Leck genau dort wuerde er nicht fangen.
- DST-Woche 26.-30.10.2026: Loop muss um 14:30 DE starten, vollcheck-DST-Fix ([[project_vollcheck_dst_fix_todo_2026-10]]) vorher erledigen, sonst fallen per K5 alle Momente der Woche fuer H1 aus.

**Why:** Optionale Nacharbeit nicht verlieren; Fable-Limit am 01.10. erreicht. **How to apply:** Am 02.10. Fable mit B1 + H-a beauftragen (kleine Aenderung, danach Selbsttest + Volllauf, kein Commit/Push ohne Levi, siehe [[feedback_commit_push_nur_levi]]); die Hinweise bei Gelegenheit; DST-Fix als eigener Auftrag vor dem 26.10.
