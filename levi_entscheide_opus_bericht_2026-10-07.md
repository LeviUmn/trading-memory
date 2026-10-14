---
name: levi_entscheide_opus_bericht_2026-10-07
description: "Levi-Entscheide 08.10.2026 zum Opus-Bericht Testtag 07.10.: alle 10 wie von Opus empfohlen, AUSSER Nr. 6: Endauswertung verschoben auf Mo 12.10.2026 (09.10. kein Testtag)"
metadata:
  node_type: memory
  type: project
  originSessionId: 3a4acdfa-7e3f-4ee0-b05a-9e82cf94fc45
  modified: 2026-10-08T09:34:55.393Z
---

# Levi-Entscheide zum Opus-Bericht Testtag 07.10. (besprochen Do 08.10.2026)

Quelle: [[opus_bericht_testtag_2026-10-07]]. Levi: "Ich stimme allem zu, mit 6. ..."

**Entscheide 1-5 und 7-10: wie von Opus empfohlen.**
1. 07.10. bewertbar (8/10, AUTO 16/20).
2. Gate 19:17 nicht aendern, in Endauswertung kennzeichnen; **heute kein Gate bei 1H-Override dagegen**.
3. Zeitbox-Neuaufrufe nach PASS-ROT: heute nichts aendern, dedupliziert ausweisen.
4. B5: US-Session-Hoch/-Tief ab 15:30 sofort ins Register (bestehender Wortlaut) — im Startprompt erwaehnen.
5. Punkt-11-Bezug fuer 13.1: heute konsistent **Gate-Richtungs-Lesart** (wie ab VC#21 am 07.10.), in `--grund-chasing` benennen; Grundsatzentscheid nach der Endauswertung.
7. Q-ROT: keine Lockerung aus diesen Daten.
8. H1: zur Kenntnis, end-Lauf genau einmal, Registrierung nicht aendern.
9. Prompt-Zeiger statt Einbettung: fuer heute akzeptiert, jeder VC-Fire liest die Datei voll.
10. Abschluss-Memo 07.10. nicht ueberschreiben, Korrekturliste im Opus-Bericht genuegt.

**Entscheid 6 ABWEICHEND (Levi, 08.10.):** Nicht Option A "Endauswertung am 08.10.", sondern
- **Do 08.10. heute: noch Testtag** (normal 15:20-20:00).
- **Fr 09.10.: Levi nicht da, kein Testtag.**
- **Mo 12.10.2026: Endauswertung gemeinsam** (Besprechung mit Levi).
- Max. Freeze-Tage damit 9/10 (06.10. ausgefallen, 09.10. kein Testtag) — die 10-Tage-Regel greift nicht; AUTO nur ueber >= 4 AUTO heute erreichbar.

**Why:** Levi ist am 09.10. nicht verfuegbar und will die Auswertung mit ihm zusammen machen.
**How to apply:** Startprompt 08.10. mit Entscheiden 2, 4, 5 + datierte Bars-Kopien sofort nach Loop-Stopp. Tagesabschluss heute normal (tagesmomente --datum 2026-10-08 zuerst). **H1-end-Lauf (Levi bestaetigt 08.10.): laeuft heute Abend nach 20:05 wie registriert, genau einmal; Registrierung NICHT verschieben.** Alles uebrige (tagesmomente --auswertung, kombi_fiktiv --auswertung, rk13_1_filter, ibf_schatten, Besprechung) am Mo 12.10.; heute nur Tagesabschluss + tagesmomente --datum 2026-10-08 (R4). Cron-Frage fuer 09.10. (keine Crons feuern lassen) war offen. Nichts committen ([[feedback_commit_push_nur_levi]]).
