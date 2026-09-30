---
name: project_testtag_analyse_2026-09-28
description: "Opus-Analyse fiktiver Testtag 28.09.2026 — EINGESCHRAENKT, 0 Trades, 6 Live-Gate-Laeufe (4 PASS), einziger hypothetischer Gewinner (16:37, +1R) durch unterlassenen Anker-Reset verpasst statt durch eine Regel"
metadata:
  node_type: memory
  type: project
  originSessionId: e60ebf92-3d84-4bf7-ab0d-caa0c564c2e0
  modified: 2026-09-28T18:17:17.980Z
---

# Opus-Analyse Testtag 28.09.2026 (fiktiv)

**Levis Frage:** ob wir Trades hätten machen können mit anderen Regeln, oder ob alles richtig war und wir auf dem Weg zu mehr möglichen Trades sind.

**Urteil: EINGESCHRÄNKT.** Technisch weitgehend stabil, erstmals echte Gate-Durchläufe mit PASS-Freigaben. Die Regeln haben am 28.09. richtig oder neutral entschieden — keine Regellockerung hätte einen Gewinner gebracht. Der einzige verpasste Gewinner (16:37, VC#16, AUTO-Anker, +1R) ging auf einen nicht vollzogenen Anker-Reset zurück (Ausführungsfehler, nicht Regelfehler). Ein Q-Score (19:32) war nicht ehrlich gesetzt (Q1/Q3 fälschlich "yes" → GELB statt ehrlich ROT).

## (A) Technik
- Loop-Start pünktlich (VC#1 15:22:11). 54 VC-Zeilen, 52 Momente; Lücken #9/#10 (Shell-Ausfall 15:57–16:08, fiel in den Abverkauf), #15 unbegründet, #42 verschoben. Doppelnummern #11/#26 = Hard-Exits (fehlende `--stale-n` bzw. `--range-stand`/`--vix-akt`), Wiederholung Sekunden später.
- "Stale-Check LUECKE" in 25 VCs (#11–#36, 2h ungemessen). Q1/Q3 in Live-Läufen 16:17/18:52/18:55 nicht gemessen.
- Guard (loop_stopp) griff korrekt, Exit 0, keine Position.
- Tweet-Check "ÜBER-POLLING" in 16 VCs — bestätigtes Zeitartefakt (Fetch im :x0-Slot, VC 1 Min später), kein Regelfehler.
- **Anker:** Short-Anker 30481,35 stand 15:46–18:51 (3h05) unverändert; X1 meldete "Reset fällig" nur einmal und schwieg danach — TP1-Fenster in 27/35 Momenten leer. Gleiches Muster wie 17./23./25.09.
- **Neuer Fund:** SL-Vorprüfung 19:46 lief mit `--dir long` + Long-Anker ÜBER dem Entry und meldete trotzdem "TAUGLICH" — Skript prüft die Anker-Richtungsplausibilität nicht. Heute ohne Auswirkung, aber ein Bugfix-Kandidat.
- Register zuletzt 16:56 inhaltlich aktualisiert, Tagestief 30081,85 fehlte im Register (nur altes Session-Tief 30259,4) → beeinflusste Q4 um 16:17.
- `loop_archiv/2026-09-28.txt` existiert nicht — der GAP vom 25.09. ist weiter offen (kein Produktivskript schreibt dorthin).

## (B) Momente & hypothetische Ergebnisse
35 qualifizierte Momente (alle SHORT), 7 Live-Gate-Läufe (6 real gewertet: 2 FAIL, 1 UNBEKANNT, 2 fälschlich/ehrlich unterschiedlich bewertetes ROT/GELB, 1 ROT — 0 Trades).
- Varianten: ALT Σ+0,32R (n=1, offen), AUTO Σ+0,03R (n=3), FLOOR Σ−2,95R (n=8).
- Kein einzelner PASS/Skipped-Fall erreichte TP1 oder SL bis Sessionende (Range 30334–30366 nach dem Iran-Spike 19:10).
- RR-Gate/TP-Realismus-Ablehnungen (16:17, 18:52) waren richtig — gelockert beide im Minus.
- Q-ROT-Auslassen kostete nichts (Ergebnisse zwischen −0,10R und +0,23R, reines Rauschen bei n=4).
- **Einziger echter Gewinner:** 16:37 (VC#16) nach dem AUTO-Anker (Swing-Hoch 16:20/bestätigt 16:35), Short 30269,35, wäre TP1 16:45 (+1R, MAE 0) gewesen — X1 verlangt diesen Reset bereits heute, wurde aber nicht vollzogen. Fenster nur ~5 Min offen.
- 1H-Override hat heute Geld gespart (ohne ihn ~−2R laut FLOOR-Simulation).
- Latenz 19:10 (Iran-Spike, +124 Pkt) hat einen Verlust verhindert (pünktlicher Short wäre −1R gewesen).
- **Fazit B:** Keine Variante hat belastbar Geld verdient; bei n=3 (AUTO) klare Overfitting-Warnung.

## (C) Trend 23./25./28.09.
| | 23.09. | 25.09. | 28.09. |
|---|---|---|---|
| Momente qualifiziert | 22 | 25 | 35 |
| Live-Gate-Läufe | 0 | 0 | 6 |
| PASS | 0 | 0 | 4 |
| Trades | 0 | 0 | 0 |
| AUTO unabhängig (ΣR) | 2 (+0,79) | 2 (−0,70) | 3 (+0,03) |
| FLOOR unabhängig (ΣR) | 3 (−0,40) | 2 (−0,67) | 8 (−2,95) |

Mehr Momente erreichen das Gate: ja. Mehr Gewinn: nicht belegt. **Freeze-Stand: 2 von 5 bewertbaren Testtagen (25.09.+28.09.), AUTO 5 von 20 unabhängigen Bewegungen. Freeze-Ende: NEIN.** Bei ~2,5 AUTO-Bewegungen/Tag realistisch erst nach ~8 bewertbaren Testtagen.

## (D) Empfehlung (3 Aufträge, freeze-konform, je mit Vorab-Kriterium)
1. **A2 als Schattenzeile übernehmen** (AUTO-Anker-Anzeige in vollcheck.cjs, "X1 fällig seit N VCs ohne Reset") — Kriterium: ALT-Anker in ≤20% der Momente >90 Min hinter AUTO (heute 77%).
2. **Q3-auto als Schattenzeile in gate_check** (aus vollcheck_state ableiten, warnt bei Widerspruch zum manuellen `--q3-coherence`) — Kriterium: 0 unentdeckte Q3-Widersprüche und 0 UNBEKANNT in 3 Testtagen (heute 1/6).
3. **Prozessregel fiktive Position am Tagesende:** automatisch zum 19:55er-Schluss bewerten statt Ermessens-Auslassen; loop_stopp Exit 2 bleibt echten Positionen vorbehalten — Kriterium: 0 Auslassungen ohne Ablehnungsgrund bei PASS in 3 Testtagen (heute 1: 19:32).

**Auch im Freeze erlaubt:** Korrekturnotiz zum GELB-Eintrag 17:32Z, Anker-Seitencheck als Bugfix (nach Levi-Zustimmung), Register inhaltlich pflegen statt nur "geprüft", loop_archiv-Lücke klären.
**Nicht anfassen bis Freeze-Ende:** RR≥1/3×ATR-Deckel/TP-Realismus-Zonen, Q2/Q4-Schwellen+Trendmodus-ADX, Q-ROT-Veto, 1H-Override+Totzone, Anker-Nachführung (V3/E1/V6), Chasing-Zähler-Semantik, Retest-Zeitbox, FLOOR als SL-Regel.

## (E) Abweichungen zu 23./25.09. und Operator-Hinweisen
- Zu 23.09.: dort "Anker-Bug nicht Hauptursache" — heute war er Ursache des einzigen Gewinners.
- Zu 25.09.: dort "kein neuer Befund" — heute 3 neue: falsches GELB, Shell-Ausfall im Schlüsselmoment, fehlende Anker-Plausibilitätsprüfung.
- Operator-Hinweise (Sonnet) größtenteils bestätigt; Sprung war +124 Pkt nicht +88; Punkt 3 nur teilweise (Exit 2 betrifft nur echte Positionen); Punkt 4 Mechanismus bestätigt aber 16 statt 7 VCs betroffen; Punkt 6: nicht das Skript wählte "long", der Operator übergab `--dir long` — der eigentliche Fund ist die fehlende Plausibilitätsprüfung.

Rohdaten/Quellen: scripts/vollcheck_log.jsonl, gate_check_log.jsonl, momente_log.jsonl, kombi_fiktiv_log.jsonl, skipped_setups_fiktiv.jsonl, sl_anker_wechsel_log.jsonl, nas100_5m_2026-09-28.json (eigene Nachrechnung bestätigt Fables Zahlen). Voller Bericht im Scratchpad als opus_analyse_2026-09-28.md.

Verwandt: [[project_testtag_analyse_2026-09-25]], [[project_testtag_analyse_2026-09-23]], [[project_testtag_2026-09-23_besprechung_ausstehend]] (Freeze-Definition), [[feedback_sonnet_eigenbericht_unzuverlaessig]].
