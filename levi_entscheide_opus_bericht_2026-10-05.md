---
name: levi_entscheide_opus_bericht_2026-10-05
description: "Levi hat am 06.10.2026 alle 7 Entscheide des Opus-Berichts Testtag 05.10. wie empfohlen angenommen; fuer Sonnet am 06.-08.10. gelten: Gate nach Buchstabe bei leerem TP1-Fenster, Q3 manuell nach EMA-Seiten-Definition; kein Fable-Auftrag im Freeze"
metadata:
  node_type: memory
  type: project
  originSessionId: ff1e3978-30b2-4753-b0cc-63142350c564
  modified: 2026-10-06T08:58:29.879Z
---

# Levi-Entscheide zum Opus-Bericht Testtag 05.10. (zugestimmt Di 06.10.2026 morgens)

Levi hat alle 7 Entscheide aus [[opus_bericht_testtag_2026-10-05]] Abschnitt 9 **so wie von Opus empfohlen** angenommen.

1. **05.10. bewertbar**, ohne Vorbehalt → Freeze 7/10, AUTO 12/20.
2. **Gate bei leerem TP1-Fenster: nach Buchstabe** (06.-08.10.): Trigger bzw. 13.1 AKTIV → Live-gate_check, solange die AUSSICHTSLOS-Sperre (SL-Anker UND TP1-Level unveraendert) nicht greift. Keine eigene Fenster-Heuristik. Keine Regelaenderung, nur Gleichbehandlung; Logs nicht aendern. Ob ein leeres TP1-Fenster kuenftig wie E3c wirken soll → Besprechung nach 08.10.
3. **Anker 18:35-20:00 akzeptiert** (formal regelkonform). Klaerung nach 08.10. zusammen mit Backlog (v): (a) X1-Impuls-Referenz aus Bar-Dochten, (b) HL vor neuem Extrem als 3a-Anker?, (c) P3-Begruendung verfaellt, wenn Praemisse wegfaellt.
4. **Q3 manuell vs. AUTO (18:46):** als 1 Widerspruchsfall "AUTO definitionsgemaess" zaehlen; bis 08.10. Q3 manuell nach EMA-Seiten-Definition (alle vier Ebenen kohaerent per EMA50-Seite) setzen. MACD-H/Divergenz gehoeren zu Punkt 11/9b, nicht zu Q3.
5. **H1:** Mindest-n (Zelle A aus >= 6 Tagen) bis 08.10. nicht mehr erreichbar; Registrierung nicht aendern, end-Lauf genau einmal, Ergebnis "Mindest-n nicht erreicht" so berichten.
6. **Q-ROT:** nicht reagieren (Entscheid 02.10. bleibt).
7. **Abschluss-Memo 05.10. nicht ueberschreiben**, Abweichungsliste im Opus-Bericht genuegt.

**Why:** Freeze bis Endauswertung 08.10.2026 — keine Skript-/Regelaenderungen; Entscheide 2 und 4 sind reine Verhaltensanweisungen fuer den Loop, damit die FAIL-Statistik bis 08.10. vollstaendig und vergleichbar wird.

**How to apply:**
- **Kein Fable-Auftrag** aus diesen Entscheiden; Backlog-Punkte (Screenshot-Pfad aus vc_nr, Live-gate_check immer mit `--jetzt`, Quick-Tick-`--jetzt` nie schaetzen, vollcheck vor Tweet-Lesung starten bei nahender Slotgrenze, EMA-Handwerte vom geschlossenen Bar, Entry-Etikett, Session-Hoch nur aus geschlossenen Kerzen) erst nach 08.10. (Mengenbremse).
- Entscheide 2 und 4 muessen **Sonnet vor Testtag-Start (ca. 15:20) im Startprompt** erreichen — nicht ueber Aenderung von `loop_prompt.cjs` (Skriptaenderung im Freeze).
- Nach 08.10.: A2/A3 (loop_prompt Z. 218), Punkte 2-4, DST-Fix bis 19.10.
- 06.-08.10. alle drei Tage bis 20:00 inkl. Bars fahren (nur dann ist 08.10. Tag 10).
