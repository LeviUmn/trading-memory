---
name: project_testtag_analyse_2026-09-09
description: "Opus-Analyse Testtag 09.09.2026: inhaltlich stärkster Tag der Serie (4/4 Q-Score-Ablehnungen nachgerechnet korrekt, ca. 2,9R Verlust vermieden, SL-Anker-Vorprüfung erstmals maschinell belegt 11/11), aber dritter Prozess-Rückfall in Folge — vollcheck.cjs/position_tick.cjs trotz Freigabe 0/18 genutzt, Tweet-Fälligkeit lief 18/18 auf falscher Rasterphase, MTF-Frische-✓ 18/18 ohne existierende Zeile. Status EINGESCHRÄNKT, 11 TODOs für Fable, 2 Selbstkorrekturen an Opus' eigenem Runde-4-Urteil."
metadata:
  node_type: memory
  type: project
  status: entwurf
  originSessionId: f86d6469-f4c5-46be-9c92-a1637d9a3ef6
  modified: 2026-09-09T17:22:41.416Z
---

## Kurzfazit

Der 09.09. ist inhaltlich der bisher stärkste Tag der Testtag-Serie und prozessual der dritte Rückfall in Folge — beides gleichzeitig belegt.

Inhaltlich: 4 technische Gate-PASS-Trigger (VC#1, #9, #10, #12), alle 4 mangels Q-Score abgelehnt, alle 4 in der Rückrechnung gegen den späteren Kursverlauf korrekt. Erstmals lief die Kette SL-Vorprüfung → Gate → Q-Score → bewusstes Auslassen vollständig und maschinell belegbar durch (11 UNTAUGLICH-Urteile mit Zeitstempel/Preis in `trigger_kandidaten_log.jsonl`, deckungsgleich mit dem Fließtext).

Prozessual: Die am selben Tag in 4 Härtungsrunden freigegebenen Skripte (`vollcheck.cjs`/`position_tick.cjs`) wurden im Loop kein einziges Mal benutzt. Schwerer als das (und von Sonnets Faktenprotokoll nicht gefunden): die Tweet-Check-Fälligkeit lief den ganzen Tag auf der falschen Rasterphase (18/18 Voll-Checks ohne Rohwert-Nachweis `x_fetch_stamp.cjs`), und in 18/18 Format-Zeilen steht `MTF-Frische ✓` ohne dass die zugehörige Pflichtzeile je existiert hätte.

## Die vier Ausgelassen-Entscheidungen — nachgerechnet

| | VC#1 17:25 | VC#9 18:10 | VC#10 18:15 | VC#12 18:25 |
|---|---|---|---|---|
| Entry/SL/Risiko | 29.338,35 / 29.428,35 / 90,0 Pkt | 29.388,55 / 29.455,55 / 67,0 | 29.388,85 / 29.455,55 / 66,7 | 29.389,85 / 29.455,55 / 65,7 |
| TP1-Weg | 118,4 Pkt | 80,25 | 80,55 | 81,55 |
| Q-Score | 2/4 ROT | 1/4 ROT | 1/4 ROT | 1/4 ROT |
| MFE tatsächlich | 5,80 Pkt (0,06 R) | 26,60 (0,40 R) | 26,90 (0,40 R) | 27,90 (0,42 R) |
| Ergebnis 19:00 | SL-Hit ≈17:39, **−1,00 R** | offen −0,63 R | offen −0,63 R | offen −0,63 R |

VC#1: klarster Fall der Serie, SL wäre ~14 Min nach Entry gerissen, TP1 nie in Reichweite. VC#9/10/12: statistisch **eine** Beobachtung (identischer Anker/TP1, 15 Min Abstand), nicht drei — Tief 29.361,95 (18:29), danach Erholung bis 8,70 Pkt unter dem SL bei Terminalzeit; Kerzen-Hochs fehlen im Protokoll, Stop-Out nicht auszuschließen.

Bemerkenswert: Q4-Runway-Gegenlevel (Rundzahl 29.350) traf mit 11,95 Pkt Abstand zum tatsächlichen Tief — bisher bester Q4-Kalibrierungspunkt, aber nirgends maschinenlesbar erfasst (→ TODO 5). Q1 war bei VC#1 der einzige falsch-positive Faktor (das benannte "saubere Ablehnungshoch" wurde 15 Min später gebrochen).

**Urteil:** Auslassen war in allen 4 Fällen richtig, Q-Score wirkte erstmals als echtes Risikofilter statt Formalie. Einschränkung: Chop-Tag (ADX 12-28) — beweist Chop-Erkennung, nicht Trendtag-Verhalten; entscheidet erst die 15-Trade-Auswertung nach 7b1a (fehlt mangels Skipped-Setup-Erfassung, siehe TODO 5).

## Prozessbruch — mit Korrekturen an der Selbstanklage

Zwei Punkte aus Sonnets Faktenprotokoll sind falsch/zu hart:
- `position_tick.cjs` hatte 0 Anwendungsfälle (keine offene Position den ganzen Tag) — kein Verstoß, kein Anlass.
- VC#14/#16 tragen `Tweet-Check ✓` MIT frisch abgerufenen Meldungen — kein ausgefallener Check, nur fehlende Pflichtzeilenform.

Dafür ein selbst nicht gefundener, schwererer Befund: Tweet-Fälligkeit lief auf "jeder 2. Voll-Check" statt der seit 03.09. verbindlichen Doppelbedingung (Minute % 10 == 0 UND Delta ≥10 aus `x_fetch_stamp.cjs --check`). Ergebnis: alle 7 Fetches auf ungültigen Slots (% 10 == 5), alle 8 echten Fälligkeits-Slots als "nicht fällig" abgehakt, `x_fetch_stamp` 0× im Protokoll. Dritter Rückfall derselben Fehlerklasse (27.08., 02.09., 09.09.). Schaden heute gering (Wasserstand real gepflegt, keine verpasste Kursbewegung).

Weitere Ausgezählt-Befunde (08.09. vs. 09.09.): SL-Anker-Vorprüfung 0/2 → 17/17 (behoben, belegt) | MTF-Frische-Zeile 0/46 → 0/18 bei 18/18 gesetztem ✓ (schlechter: falsches ✓) | 1H-Schatten 0/46 → 0/18, Logdatei existiert nicht (unverändert tot) | Register-Check wörtlich 0/46 → 0/18 (Form weiter verletzt) | Gate-Check-CLI wörtlich 0/2 → 0/4 (unverändert) | Retest-Zeitbox zweimal ohne "VERFALLEN"-Erklärung fallengelassen (nach VC#1, nach VC#12) — wörtlich der 03.09.-Befund.

**22 fehlende Quick-Tick-Minuten aufgelöst:** 21 von 22 liegen direkt hinter einem Voll-Check — die Handkomposition frisst systematisch 1-2 Minuten in den Folge-Slot (VC#1 mit Gate-Aufruf: 6 Min, VC#9: 2 Min). Das entspricht 28% der Tick-Kadenz und ist eine Regression gegenüber dem 08.09. (4 Std. ohne eine ausgefallene Minute).

**Einordnung:** Kein Hard-Gate umgangen, keine Order, kein Kapital berührt. Die eine bindende 08.09.-Auflage (SL-Vorprüfung vor jedem Gate) ist erfüllt und maschinell belegt — größter Einzelfortschritt der Serie. Die andere (vollständiger Voll-Check im gültigen Format) ist zum dritten Mal in Folge offen. Diagnose: der Fehler ist nicht wiederholt, sondern eine Ebene höher gewandert — die Anweisung "benutze vollcheck.cjs" steht nur als Fließtext in `feedback_vollcheck_format.md`, das im 5-Minuten-Loop niemand liest.

**Opus-Selbstkritik:** Das eigene Runde-4-Urteil ("freigegeben, keine Blocker") war technisch richtig, praktisch unzureichend — ein Trockenlauf beweist Lauffähigkeit, nie Anwendung. Die D1-Einschätzung "Randfälle praktisch selten" ist am ersten Einsatztag durch die 11-fache Fehlzählung in `protokoll_bilanz.cjs` (siehe TODO 3) falsifiziert.

## TODOs für Fable (priorisiert, P1 vor nächstem Testtag)

1. **[P1] Kommandotemplate in den CronCreate-Loop-Prompt, nicht nur ins Regelwerk.** Kernmaßnahme — `vollcheck.cjs` ist freigegeben und wurde 0/18 genutzt, weil die Anweisung an einer im Trigger-Moment ungelesenen Stelle steht.
2. **[P1] `vollcheck.cjs` braucht `--state <Datei>`.** ~15 der 52 A3-Felder sind fortzuschreibende Zähler (kerzen-nas100/qqq, k-ohne-signal, Register-Alter etc.), nicht ablesbar — heute Ursache der `--kerzen-nas100 5`-Schätzung und 4/4 `--grund-`-Ausfällen bei `--kerzen-qqq`.
3. **[P1] `protokoll_bilanz.cjs`: 2 Regex-Defekte.** Kopfzeilen-Regex verlangt `(Nr.` direkt hinter Uhrzeit (heutiges Format hat `DE-Zeit (UTC) (Nr.` dazwischen → 0/18 Treffer); `is22`-Regex verlangt `2/2` direkt hinter Doppelpunkt (17/18 Zeilen haben "weiterhin 2/2" dazwischen). Reale Zahl: 11 2/2-Zustände ohne 7b1, gezählt: 1 (Faktor 11 daneben). Zusätzlich: bei `heads.length===0` keine blockbasierte Zahl mehr drucken, sondern "nicht ermittelbar".
4. **[P1] Tweet-Fälligkeit maschinell erzwingen.** `vollcheck.cjs` ruft `x_fetch_stamp.cjs --check` selbst auf, druckt Rasterstatus+Delta wörtlich, kippt Format-Zeile bei Widerspruch.
5. **[P1] PASS-aber-ausgelassen maschinenlesbar erfassen**, auch fiktiv (`skipped_setups_fiktiv.jsonl`, nicht `trades.db`): Entry/SL/TP/Q-Flags/Terminalkurs. Ohne das bleibt die 7b1a-Auswertung nach 15 Trades mangels Daten unmöglich — bisher 7 von 9 Testtagen ohne Trade sind sonst Nulltage.
6. **[P2] Format-Zeilen-Verifikation gegen Zeileninhalt** (✓ nur wenn Pflichtzeile tatsächlich im Block existiert) — genereller Fänger für die dominante Fehlerklasse beider Tage.
7. **[P2] QQQ-ATR im Skript rechnen** (unverändert TODO seit 08.09., 3. Tag in Folge 4/4 `--grund-`-Ausfall).
8. **[P2] 1H-Schatten-Zeile implementieren oder formell aussetzen** (Logdatei existiert nicht, 0/64 über 2 Tage).
9. **[P2] Retest-Zeitbox: Geltungsbereich auf "PASS+Q-ROT ausgelassen" erweitern, Verfall erzwingen** (`gate_check.cjs` schreibt Deadline bereits nach `last_gate_fail.json`).
10. **[P3] Terminalbedingung in Format-Zeile** (Levi-Vorgabe 19:00 kam im Protokoll 0× vor).
11. **[P3] Gate-Check wörtlich zitieren** — `last_gate_check.txt` ist rollierender Ein-Slot-Puffer (nur VC#12 erhalten), Protokoll-Zitat ist der einzige dauerhafte Nachweis.

Bewusst nicht auf der Liste: keine neue Handelsregel, keine Schwellenanpassung — das System hat kein Regeldefizit, sondern ein Ausführungsdefizit.

## Zahlenkorrekturen am Faktenprotokoll

- SL-Vorprüfungs-Urteile: **17, nicht 16** (6× TAUGLICH: VC#1,9,10,11,12,13 — Faktenprotokoll übersieht VC#1 und VC#11 und nennt einen unverständlichen Klammersatz mit nicht existenter "VC#20"). Vierter Vorfall der Klasse "Bilanzzahl ohne Nachrechnen" (28.08., 04.09., 08.09., heute).
- Loop-relevante `register_touch.cjs`-Aufrufe: **2, nicht 3** (der dritte lag um 15:20:22 UTC vor Loop-Start, beim Session-Update).
- Getrennte Zählung 2/2-Zustände vs. 2/2-Trigger-Ereignisse (seit 03.09. Pflicht, im Faktenprotokoll gefehlt): 1 echtes Trigger-Ereignis (VC#1), 12 fortbestehende Zustände.

## Status-Einordnung

**EINGESCHRÄNKT** — Auflage für den nächsten Testtag: wörtlicher `vollcheck.cjs`-Aufruf muss im Loop-Prompt stehen, mindestens die ersten 3 Voll-Checks müssen unveränderte Skript-Ausgabe sein. Kein ABBRUCH (kein Hard-Gate umgangen, keine Order, alle 4 risikotragenden Entscheidungen korrekt, SL-Vorprüfungs-Auflage aus 08.09. erstmals maschinell erfüllt). Kein KEIN-EINWAND (dritte Serie in Folge mit offener Vollcheck-Format-Auflage).

Empfohlene Reihenfolge: TODO 1+2 zuerst und allein (Prompt-Template + Zustandsspeicher), dann TODO 3+4 (Ein-Datei-Fixes), dann ein Testtag mit einzigem Erfolgskriterium "läuft der Voll-Check aus dem Skript?". TODO 5 kann parallel laufen (fasst den Loop nicht an, höchster Langfristwert für die 7b1a-Auswertung).

Analyse: [[project_testtag_analyse_2026-09-08]], Gegencheck-Runden: [[project_gegencheck_fable_umsetzung_2026-09-09_runde4]]
