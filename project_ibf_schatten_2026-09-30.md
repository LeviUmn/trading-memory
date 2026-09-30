---
name: project_ibf_schatten_2026-09-30
description: "IBF = 1H-Intrabar-Freigabe mit halber Position (Levi-Idee 30.09.2026): Levi-Entscheidung c) NUR Schattenmessung per scripts/analyse/ibf_schatten.cjs, Primaervariante AUTO, kein Live-Eingriff; Vorab-Kriterien 1-5 fuer das Freeze-Review; Stand 30.09.: n=1 (AUTO), Prognose 'zu selten, nicht belegt'"
metadata:
  node_type: memory
  type: project
  originSessionId: 3c4562d6-d0b9-4432-8f7a-392557ffe83f
  modified: 2026-09-30T10:06:29.156Z
---

# IBF-Schattenmessung: 1H-Intrabar-Freigabe, halbe Position (Levi-Idee 30.09.2026)

**Levi-Entscheidung c) 30.09.2026 (verbindlich):** Der 1H-Override (Veto, Definition 04.09.: EMA50-Seite des letzten GESCHLOSSENEN 1H-Bars) bleibt **live unveraendert** bis zum Freeze-Ende. Die Idee „Freigabe, wenn der Kurs intrabar schon auf der richtigen Seite steht, mit halber Position“ wird **nur gemessen**: `node scripts/analyse/ibf_schatten.cjs` (Fable, Auftrag A3 nach Opus-Spezifikation [[opus_vorschlag_2026-09-30]] Abschnitt 2; reines Auswertungsskript, schreibt nichts, kein Loop-Schritt). **Live-Entscheidung erst nach dem Freeze-Ende**, Primaervariante AUTO; FLOOR/ALT nur Referenz (FLOOR = 1,5xATR ohne Anker ist keine Live-SL-Regel und belegt allein nichts).

## Definition (Opus 2.1, prueffbar)
1. 1H-Override-Veto aktiv (letzter geschlossener 1H-Bar auf der falschen EMA50-Seite).
2. 3/4-Konstellation: 5m, 15m NAS100 und QQQ-15m in Traderichtung = `blockade_3v4 = true` im `oneh_shadow_log.jsonl`.
3. Intrabar-Seite: Kurs in Traderichtung jenseits der 1H-EMA50 des letzten geschlossenen Bars (`sign*(kurs - ema50_1h) > 0`, Werte aus dem Shadow-Log; mathematisch identisch mit „laufender 1H-Close gegen laufende 1H-EMA50“).
4. Keine Schwelle, keine Mindestdauer (Variante mit Schwelle P8 war schlechter: ohne 15.09. −2,0 R statt −1,05 R).
5. Rueckfall: gilt je Gate-Lauf; nach dem Entry normaler SL/TP/Punkt 12, kein Nachkauf auf volle Groesse.
6. „Alle anderen Indikatoren“ = Dual-Gate 15m NAS+QQQ, SL-Anker TAUGLICH inkl. P1/P3, GESAMTSTATUS PASS, Q-Score nicht ROT. **Q3 bleibt auf dem geschlossenen 1H-Bar** → bei IBF automatisch NICHT ERFUELLT → Maximum 3/4 = GELB = **halbe Position aus der bestehenden Q-Score-Logik** (keine neue Sizing-Regel; Q3 zusaetzlich intrabar = doppelte Lockerung, abgelehnt). Stacking-Regel: nur einmal halbiert (IBF im 15:30-16:00-Fenster 0,5, nicht 0,25); IBF + Q-ROT live = kein Trade; Kombi-Modus Groesse = min(Kombi-Groesse, 0,5). 13.1/k-Zaehler: IBF zaehlt NICHT als vollstaendiges Dual-Gate. Sperren/Blackouts unberuehrt.

## Vorab-Kriterien (festgelegt 30.09.2026, geprueft beim Freeze-Review — nicht nachtraeglich aendern)
1. Basis je Blockade-Episode (zusammenhaengender Blockade-Lauf gleicher Richtung, Luecke ≤ 20 Min): der erste freigegebene Moment (IBF-faehig UND nur mit IBF qualifiziert) mit ampel_max ≠ ROT, RR-1-Ausgang, R ungewichtet; zaehlend fuer AUTO ist der erste solche Moment mit offenem AUTO-TP1-Fenster. **Lesart (Opus-Gegencheck C4, von Levi bestaetigt 30.09.2026, gilt so bis zum Freeze-Review):** je Episode zaehlt der ERSTE Moment, der nicht ROT ist UND ein offenes AUTO-Fenster hat — auch wenn davor ein nicht-ROT-Moment mit geschlossenem Fenster lag (29.09. Short: 17:36 GELB/Fenster zu → zaehlend 17:51 +1). Begruendung: „Fenster offen“ ist Geometrie beim Einstieg (`fenster_offen` ex ante, slDist ≤ 3×ATR), kein Blick in die Zukunft; ein Live-Operator wartet ebenso auf den ersten handelbaren Moment. Woertlich/streng gelesen (nur der allererste nicht-ROT-Moment) waere Stand 30.09.2026 **n = 0** statt n = 1 — die Skript-Ausgabezeile (c) 1 bleibt unveraendert.
2. **Mindest-n ≥ 10** zaehlende Episoden in AUTO; sonst „zu selten, nicht belegt“ → keine Umsetzung.
3. Ø R ≥ +0,10 UND Ø R ohne den besten Tag ≥ 0 UND Gewinn-Episoden ≥ Verlust-Episoden.
4. Kettendelta gegenueber heute (Veto) bei Gewicht 0,5 ohne den besten Δ-Tag ≥ 0 (erfasst die Verdraengung voller Positionen; eine fiktive Position zur Zeit).
5. Getrennt ausgewiesen: Ergebnis bei ampel_min (alle unbekannten Q als NEIN); negativ → ausdrueckliche Warnung. (Bei IBF ist Q3 immer NEIN; mit Q1/Q4 unbekannt=NEIN sind alle IBF-Momente ROT → n 0 — Q1/Q4 werden waehrend Blockaden nie gemessen, bewusst kein Schatten-Gate-Lauf fuer Sonnet.)

## Stand 30.09.2026 (Fable-Lauf, reproduziert Opus 2.2 exakt; Datenbasis 6 Tage 11./15./23./25./28./29.09. = alle Shadow-Log-Tage mit `nas100_5m_<datum>.json` — der 11.09. hat 0 Blockaden und traegt nur Nullzeilen bei, Episoden/Kriterien stammen aus den 5 Tagen 15./23./25./28./29.09.; nicht in der Datenbasis: 16.09. ohne 5m-Bars, 17.09. ohne eigene Bars-Datei, 21.09. nur in nas100_5m_2026-09-18_21.json — beide laut Opus ohne Blockade; Skript-Ausgabe seit 30.09. (C-3) nennt keinen „besten Δ-Tag“ mehr, wenn kein Tag Δ > 0 hat, z. B. AUTO S2)
- Kettendelta FLOOR: S1 (volle Groesse) +1,95 | **S2 (x0,5) +1,45** | **S3 (x0,5 + nicht ROT) +2,50** | S4 +3,00; **ohne 15.09.: S2 −1,05 / S3 ±0,00**. ALT ±0 in allen Szenarien. **AUTO S2 −0,50** (23.09. 17:31, ROT, verdraengt bei halber Groesse einen ganzen Gewinner), AUTO S3 ±0.
- Episoden (erster IBF-Moment, FLOOR/ALT/AUTO): 15.09. Long 15:45 −1/zu/zu GELB · 15.09. Short 16:10 +1/zu/zu GELB · 23.09. Short 16:11 +1/zu/zu **ROT ueber die ganze Episode** (Trendtag, Q2 ueberdehnt) · 28.09. Short 15:46 −1/zu/zu ROT (15:56 GELB: FLOOR +1) · 29.09. Long 16:42 −1/zu/zu GELB (Fehlwende wird freigegeben) · 29.09. Short 17:31 +0,57 offen / ALT +0,32 offen / AUTO zu, ROT (17:36 GELB: FLOOR +1, ALT +1; **AUTO zaehlend 17:51 +1**). Drei Blockaden bleiben blockiert (Kurs nie jenseits EMA): 25.09. 17:52, 28.09. 18:31, 28.09. 19:16 = drei der sechs „Verlust vermieden“-Faelle.
- **Kriterien:** n(AUTO) = **1** (29.09. 17:51 +1; Lesart A, s. Kriterium 1 — streng n = 0) → Kriterium 2 verfehlt → **„ZU SELTEN, NICHT BELEGT“**. Kriterium 4: Σ ±0 ohne jeden Tages-Δ = **NEUTRAL** (keine Wirkung; formal erfuellt, belegt nichts — Skript weist das seit 30.09. so aus, C2). Kriterium 5: n 0, kein negatives Ergebnis.
- Befund (Opus): Mit dem live gueltigen ALT-Anker war das TP1-Fenster bei 5 von 6 IBF-Episoden zu → **0 zusaetzliche Trades**; die IBF adressiert nicht den Engpass (Reihenfolge bleibt Q4/Q1 > Anker-Geometrie > Override). FLOOR-Gewinne haengen zu > 80 % am 15.09. (n 5-6, Ueberanpassung). Prognose: bei ~1,2 Episoden/Tag wird n ≥ 10 bis zum Freeze-Ende (spaetestens ~08.10.) sehr wahrscheinlich nicht erreicht.

**Rueckbau:** nichts noetig (nichts live). Falls nach dem Freeze doch live: Konstante `IBF_MODUS = 'aus' | 'live'` in vollcheck.cjs (Default `'aus'`); betroffen waeren vollcheck (Dual-Gate-Zeile), loop_prompt (7b1-Text) und Regelwerk 7b1 — nicht gate_check (dessen 1H-Zeile ist nur Anzeige).

**Why:** Die Idee ist die sauberste der schnelleren Override-Varianten (haelt Fehlwenden, die nie ueber die EMA kommen, blockiert; halbe Groesse ergibt sich aus GELB), loest aber nicht „zu wenige Trades“, weil der Anker das Fenster fast immer schliesst. **How to apply:** Beim Freeze-Review `node scripts/analyse/ibf_schatten.cjs` laufen lassen und gegen die Kriterien 1-5 pruefen; bis dahin keine Live-Aenderung am 1H-Override. Verwandt: [[project_opus_analyseliste_2026-09-29_1h_override]], [[project_testtag_analyse_2026-09-29]].
