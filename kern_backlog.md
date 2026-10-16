---
name: kern_backlog
description: "Backlog Kern-Regelwerk 1.0 (Regel-Freeze seit Fiktivtag 1, 15.10.2026): Ideen mit Datum, NICHTS davon umgesetzt; Format Datum | Idee | Beobachtung | Fundstelle. Stand 16.10.2026: 9 Eintraege aus dem Opus-Bericht Fiktivtag 1 (Abschnitt 4 B)"
metadata:
  node_type: memory
  type: project
  originSessionId: d311f88c-498e-4854-84e9-d61f3c31362a
  modified: 2026-10-16T08:25:40.291Z
---

# Kern-Backlog (kern-1.0, Regel-Freeze)

Regel-Freeze seit Fiktivtag 1 (15.10.2026, [[regelwerk_kern_2026-10]] Abschnitt "Regel-Freeze"): keine Regel-, Schwellen- oder Konstantenaenderung; Ideen landen hier mit Datum. Eintraege sind NUR Ideen — nichts davon ist umgesetzt, entschieden oder freigegeben. Umsetzung erst nach Levi-Entscheid (fruehestens in der Opus-Zwischenanalyse nach 5 gueltigen Tagen). Angelegt 16.10.2026 (Fable, Levi-Auftrag) mit den 9 Eintraegen aus [[opus_bericht_kern_fiktivtag1_2026-10-15]] Abschnitt (4)(B), Wortlaut unveraendert.

Format: Datum | Idee | Beobachtung | Fundstelle

| Datum | Idee | Beobachtung | Fundstelle |
|---|---|---|---|
| 2026-10-15 | Kennzahl "entry_live - Entry-Open" je Trade in der Bilanz | Gate laeuft ca. 60 s nach Kerzen-Open, heute 6,5 bzw. 4,0 Pkt zu Lasten (0,08/0,04 R) | kern_gate_log 15:51:02, 16:16:00 |
| 2026-10-15 | 1m-Sicherung frueher/zweistufig oder Warnung bei zu kurzem 1m-Bestand | 300 1m-Bars reichen nur bis 15:03 zurueck; Sicherung nach ca. 20:35 verlore 15:35-Entries und damit (c) | nas100_1m_2026-10-15.json |
| 2026-10-15 | Positions-Tick, Nachtrag, Bilanz, Stopp ins kern_archiv mitschreiben | Archiv enthaelt nur Start/Check/Gate | kern_archiv/2026-10-15.txt |
| 2026-10-15 | Einheitliche Zeitlabels in der Bilanz | Trigger unter Kerzen-Open, Alt-Momente unter Entry-Slot | kern_bilanz.cjs Z. 73 vs 80 |
| 2026-10-15 | Alt-Momente zusaetzlich mit gate_check-Schatten bewerten | "alt haette" nur fuer Kern-Gates bekannt | Bilanz Alt-Zustandsmomente |
| 2026-10-15 | Exit-Erkennung im Positions-Tick auch aus --kurs stempeln | Exits 1m 16:01/16:42, erkannt erst 16:05:56/16:45:57 | kern_position_log |
| 2026-10-15 | Check-Zeile "TP1 erreicht -> 50 %" nach bestaetigtem Teilverkauf unterdruecken | Risiko doppelter Teilverkauf im Echtgeld | kern_position_log 14:00:54Z |
| 2026-10-15 | Pfad-Effizienz als Schatten neben Tagestyp | V-Tag 0,65 "Trend", Pfad-Effizienz 0,24 | nas100_5m_2026-10-15.json |
| 2026-10-15 | Schreibfehler beim kern_exit_log sichtbar machen | try/catch schluckt, (a) nicht belegbar | kern_common.cjs Z. 64 |

Vermerk zu Eintrag 2 (1m-Bestand; 16.10.2026, Opus-Kurzcheck A1, Eintrag selbst unveraendert): heute kein Fix. In der DST-Woche 26.-30.10. (Entry ab 14:35) reicht der 1m-Bestand von 300 Bars bei Sicherung 20:02 nur bis ca. 15:03 zurueck → Technik (c) fuer Trades 14:35-15:02 sonst zwingend NEIN. Test bis 23.10. (`chart_scroll_to_date` auf heute 14:30 + `data_get_ohlcv` summary 1m: Bestand > 330?); danach ggf. 6a-Ergaenzung "1m vor dem Save zurueckscrollen" als Fehlerkorrektur mit Opus-Kurzcheck vor 26.10. — das waere eine Fehlerkorrektur (Technik c messbar halten), keine Regelaenderung. Details: [[project_kern_fiktivtag1_2026-10-15_ablauf]] Punkt 4.

Nicht im Backlog (weil Fehlerkorrekturen, Opus-Bericht Abschnitt 4 A): A1 Prompt-Text save_path/count (behoben 16.10.2026, Fable, nur `kern_loop_prompt.cjs`-Text; Opus-Kurzcheck 16.10. freigegeben mit Auflage, Auflagen umgesetzt), A2 EMA-Pruefzeile laufende Kerze (offen), A3 kern_bilanz Kriterium (d) bei nach 20:00 offener Position (offen).
