---
name: project_testtag_analyse_2026-09-29
description: "Opus-Analyse fiktiver Testtag 29.09.2026 — EINGESCHRAENKT: 0 Trades, 3 Live-Gate-Laeufe (1 PASS, Q-ROT), nur 40 % Loop-Abdeckung (Start 16:42, Usage-Limit-Ausfall 18:32-20:41), P3 nie gegriffen, 1H-Override-Analyse: keine Aenderung"
metadata:
  node_type: memory
  type: project
  originSessionId: 06006c65-4ed0-4041-9028-70709ef145bd
  modified: 2026-09-30T09:02:53.359Z
---

# Opus-Analyse Testtag 29.09.2026 (fiktiv)

**Urteil: EINGESCHRAENKT.** Gemessenes Fenster 16:42-18:31 lief sauber (23 VC-Slots, 1 Hard-Exit VC#17 = fehlendes `--stale-n`), Regeln entschieden neutral bis richtig. Nur ~40 % des Fensters gemessen: Start erst 16:42 (nicht 15:30) und **Usage-Limit-Ausfall 18:32-20:41** (loop_stopp erst 20:41, Grund im Log). Die einzige echte Abwaertsbewegung (18:40, -54 Pkt) fiel in die Luecke. 0 Trades.

**3 Live-Gate-Laeufe (alle Short, fiktiv):** 18:03 FAIL RR 0,89 (alter Anker 30372,55; mit AUTO-Anker 30337,45 waere PASS gewesen, aber Q 2/4 ROT) | 18:11:26 FAIL RR 0,776 (TP1 30250 aus Vorpruefung uebernommen, Kurs war 11 Pkt gerutscht) | 18:11:44 PASS, Q 2/4 ROT (Q1 nein, Q4 0,06 wegen Rundzahl 30300) -> ausgelassen (Option D). Nachtraege: +0,44R offen / TP1 +0,78R / +0,84R offen (MFE ~1,07R); Kombi-fiktiv Stall-Exit 19:15 +0,27R (x0,5). Laut Live-Check ~20:50 haette SL 30377,35 gehalten -1R getroffen — kein Urteil ueber Q-ROT bei n=1.

**Prozesslehre (ohne Regelaenderung):** TP1/SL fuer den Live-Gate immer aus den Fenster-Zeilen des JETZT-Laufs nehmen, nicht aus der Vorpruefung von vor 1-3 Min uebernehmen (Laeufe 1+2). Sonnet-Korrekturen durch Opus: Screenshots waren 18 von 23 VCs LUECKE (nicht "3"), frische nur VC#5/#10/#15/#17/#20; X1 war nur 17:31 und 18:06 faellig (P2-Zaehler zaehlt nicht-faellige VCs mit = Anzeige-Mangel); kerzen-qqq=3 war richtig (Heuristik-Fehlalarm).

**Freeze-Stand:** Skript zaehlt 3/5 bewertbare Tage, AUTO 6/20 (Σ-1,67R). Opus-Empfehlung 29.09.: 29.09. als Teiltag NICHT zaehlen -> 2/5, AUTO 5/20 (am 30.09. von Opus selbst zurueckgenommen: inkonsequent, weil der 25.09. mit 50 % Abdeckung bereits zaehlt). Freeze-Ende: NEIN. Offizieller `tagesmomente.cjs --datum 2026-09-29`-Lauf erst am 30.09. 10:45 nachgeholt (Fable, Auftrag 0.1): 23 Momente, 13 qualifiziert, Bars ok 66/66, ALT -1,00 / AUTO -1,00 / FLOOR Σ -1,56; momente_log.jsonl 270 -> 293 Zeilen, nur der 29.09. kam dazu (sha1 3ee7bb00 -> 45bc6413). `--auswertung` danach woertlich: „Bewertbare Testtage: 3 (2026-09-25, 2026-09-28, 2026-09-29) | ALT n 2 Σ R -0,68 Ø R -0,34 | AUTO n 6 Trefferquote 17% Σ R -1,67 Ø R -0,28, ohne besten Tag (28.09.) -0,57 | FLOOR n 12 Σ R -5,18 Ø R -0,43 | FREEZE-ENDE ERREICHT: nein — fehlende Zahlen: bewertbare Testtage 3/5 (fehlen 2); AUTO unabhaengige Bewegungen 6/20 (fehlen 14; Abbruchregel: 3/10 bewertbare Tage)“.

## Levi-Entscheidungen 30.09.2026 (verbindlich; Opus-Pruefung in [[opus_vorschlag_2026-09-30]])
- **a) Der 29.09. zaehlt als Freeze-Tag** (3/5, AUTO 6/20). Teiltage zaehlen auch kuenftig. Abdeckung (5-Min-Slots 15:30-20:00 mit Voll-Check): **29.09. = 43 %**, **25.09. zaehlte bereits mit 50 %**, 28.09. 93 %. Ohne beide Teiltage (nur 28.09.): ALT n 1 +0,32 | AUTO n 3 Σ +0,03 | FLOOR n 8 Σ -2,95 (ohne nur den 29.09.: AUTO n 5 Σ -0,67, FLOOR n 10 Σ -3,62, ALT n 1 +0,32). Die Abdeckungsanzeige in `tagesmomente.cjs --auswertung` (A2, 30.09.) ist reine Anzeige; Freeze-Zeile und Bewertbarkeit unveraendert. Kosten: Teiltage verbrauchen die 10-Tage-Obergrenze (bei Vollzaehlung faellt Tag 10 auf Do 08.10.2026), liefern aber wenig AUTO-Bewegungen (Engpass AUTO 6/20).
- **b) „19:55er Schlusskerze“ = Kerze mit Label 19:55 (schliesst 20:00 DE)** = bisheriges Verhalten von tagesmomente/kombi_fiktiv (Setup 3: +0,84 R, nicht +0,69 R). Neu nur: `skipped_fiktiv.cjs --nachtrag --bars` hat jetzt denselben Default-Stichtag 20:00 DE (A1, 30.09.; vorher ohne `--bis` ganze Bars-Datei -> Setup 3 waere per Kerze 20:40 SL-HIT -1 R gewesen).
- **c) IBF (1H-Intrabar-Freigabe, halbe Position): NUR Schattenmessung** per `scripts/analyse/ibf_schatten.cjs`, Primaervariante AUTO, kein Live-Eingriff, Live-Entscheidung erst nach Freeze-Ende. Vorab-Kriterien + Stand in [[project_ibf_schatten_2026-09-30]]. 1H-Override bleibt live unveraendert.
- **d) Usage-Limit: keine Massnahme** („mein Fehler“). „Remote Control vorher an“ bleibt gueltig.
- **P3-Verlaengerung:** 3-Tage-Urteil nur mit >= 1 impuls-faelligem Fall, sonst bis +2 Tage verlaengern (dokumentiert in [[project_p1p3_anker_reset_pflicht_2026-09-29]]).
- **Faktenprotokoll 29.09.: NICHT nachholen.** Ab 30.09. gilt `scripts/loop_archiv/<datum>.txt` als Tagesprotokoll ([[feedback_tagesabschluss]]).
- **Push/Backup:** Code-Repo-Push freigegeben (macht Sonnet nach dem Opus-Gegencheck). Memory-Repo `LeviUmn/trading-memory` ist laut Pruefung 30.09. OEFFENTLICH (HTTP 200 ohne Auth) -> dort nur lokal committen, KEIN Push, bis Levi es auf privat stellt.

**P3 (erster echter Tag):** 1 Reset (18:11, 0,87xATR), 0 Kleinstschritt, 0 P3-Hard-Exits. Kriterium 1 formal erfuellt (0/13) aber ohne Aussagekraft (Short-Phase nur 60 Min gemessen), Kriterium 2 erfuellt. P3 griff nie (nur Distanz-Ausloeser, keine Pflicht). Empfehlung: `pflicht` unveraendert; 3-Tage-Urteil nur mit >=1 impuls-faelligem Fall, sonst bis +2 Tage verlaengern.

**1H-Override (Levis Frage):** Pflicht, ist Veto. 10 Episoden ueber alle Testtage, 9 bewertbar: 6x Verlust vermieden, 3x Gewinn gekostet, netto ~+3,4R gespart (nur an Trendtagen 15.09./23.09. teuer). Schnellere Varianten (P8-Ueberschuss, laufender Kurs) drehen ohne den 15.09. ins Minus. Heute waere auch ohne Override PASS+Q-ROT gewesen. Engpass-Reihenfolge Q4/Q1 > Anker-Geometrie > Override (Q4 in 11 von 16 PASS-Laeufen NEIN, Q1 in 12/16 NEIN/UNKLAR). **Empfehlung: keine Aenderung**; neu pruefen ab >=20 Episoden (heute 9), Kriterium dann Ø R >= +0,10, >=0 ohne besten Tag.

**Q3-auto / Tagesende-Regel (rueckwirkend 29.09.):** Q3 in allen 3 Laeufen yes, 0 Widersprueche/0 UNBEKANNT (Tag 1/3, trivial). Tagesende: 1 PASS, ausgelassen mit Grund (1/3); offene Definition: "19:55-Schluss" = Schluss der 19:50- oder 19:55-Kerze (Setup 3: +0,69R vs +0,84R). **Opus: Fable setzt beide um, zuerst Q3-auto, dann Tagesende, je mit Opus-Gegencheck; dazu Abdeckungsanzeige (>=80 % des Fensters mit VCs) in tagesmomente --auswertung.** Noch NICHT beauftragt (wartet auf Levi).

**Levi muss entscheiden:** (a) 29.09. als Freeze-Tag zaehlen? (Empf. nein) (b) welche Kerze = "19:55-Schluss"? (c) Override bis Freeze-Review unveraendert? (Empf. ja) (d) Usage-Limit-Vorsorge: Budget vor 15:10 pruefen.

**Why:** Ehrliche Datenlage: kleines n, grosse Messluecke; Regeln haben heute nichts Belegbares verschenkt, Engpass ist Q4/Q1, nicht der Override. **How to apply:** Vor Regelarbeit diese Datei + [[project_testtag_analyse_2026-09-28]] + [[project_p1p3_anker_reset_pflicht_2026-09-29]] lesen; Freeze einhalten. Voller Bericht im Scratchpad opus_analyse_2026-09-29.md (session-lokal). Tagesprotokoll: scripts/loop_archiv/2026-09-29.txt (aus VC-Ausgaben + Live-Gate-Laeufen zusammengesetzt, ungetrackt). 1H-Analyseliste: [[project_opus_analyseliste_2026-09-29_1h_override]] (erledigt).
