---
name: project-studie-geschwindigkeit-effektivitaet-opus-2026-09-07
description: "Opus-Studie 07.09.2026: wie wird das Trading-System schneller (1-Min/5-Min-Loop + offene Position) und effektiver (mehr valide Trades ohne Gates zu lockern) — 15 Vorschläge (A1-A8 Geschwindigkeit, B1-B7 Effektivität), priorisiert. Parallel läuft eine unabhängige Fable-Studie zur selben Frage. Entwurf, nichts umgesetzt."
metadata: 
  node_type: memory
  type: project
  status: draft
  modified: 2026-09-07T17:05:22.341Z
  originSessionId: b594a3be-34a1-46da-8616-0f50ceb56daa
---

# Opus-Studie: Geschwindigkeit & Effektivität (07.09.2026)

**Status: Vorschlag, nichts umgesetzt, nichts beschlossen.** Unabhängig parallel: Fable macht dieselbe Studie ohne Kenntnis dieser Antwort. Levi liest beide, bevor irgendetwas an Fable zur Umsetzung geht.

## Zwei tragende Vorbemerkungen

**Geschwindigkeit:** Ein Voll-Check kostet 18-22 Tool-/Skript-Aufrufe, davon 6 reine Chart-Navigation (`chart_set_timeframe` ×4, `pane_focus` ×2) ohne eigenen Datenwert — `data_get_study_values` nimmt keine Parameter, liest immer den fokussierten Pane. Das Umschalten ist heute alternativlos, `batch_run` hilft nicht (kann keine Indikatorwerte). Beleg: die 18:49-Trigger-Verarbeitung am 04.09. dauerte bis 18:53 — drei Fires verloren.

**Effektivität — die Zahl, die den Rahmen setzt:** TP1-Fenster am 04.09. waren 17,8/42,1/28,3/29,0 Pkt breit. Ein 50er-Rundzahlraster trifft ein Fenster der Breite *w* mit Wahrscheinlichkeit *w*/50 → für diese vier Fenster 36%/84%/57%/58%, im Mittel ~59%. **Zusätzliche Level, die selbst auf dem 50er-Raster liegen, bringen exakt null** — nur off-grid und bewegliche Level erhöhen die Trefferwahrscheinlichkeit. Realistische Obergrenze des ganzen Hebels: **~25-30% mehr PASS-fähige Trigger**, kein Vielfaches. Wer mehr verspricht, weicht Gates auf.

---

## Teil A — Geschwindigkeit

### Hoch
**A1 (SKRIPT) — `vollcheck.cjs`: ein Aufruf erzeugt den kompletten Pflichtzeilen-Block.** Statt 12-15 Pflichtzeilen von Hand + 3 Einzelskripte: ein Skript, dem die abgelesenen Rohwerte übergeben werden und das daraus MTF-Frische, Zählstände, 8d+K3-Schatten, Chasing-Status, Register-Check, Tweet-Check, 1H-Schatten, Format-Zeile erzeugt. Größter Einzelhebel: beseitigt strukturell die häufigste Fehlerklasse des Projekts (17× falsche Register-Frische, 9× zählerlose 8d-Zeile, 54× fehlende Nummer, Nummerndrift). **Harte Auflage: darf nichts ableiten, was es nicht bekommen hat — fehlender Parameter = sichtbare Lücke, nie ein Default.**

**A2 (LAYOUT+PROZESS) — 4-Pane-Layout mit festen Zeitrahmen, MTF ohne Umschalten.** NAS100 5m/15m/1H + QQQ 15m als feste Panes → MTF-Block wird reine Leseoperation, kein `chart_set_timeframe` mehr. Warnung: Layout-Werkzeuge sind historisch unzuverlässig (`feedback_layout_tools_unzuverlaessig.md`). **Als separates, neu benanntes Layout an einem Nicht-Handelstag testen, produktives Layout unangetastet lassen.**

**A5 (SKRIPT) — `position_tick.cjs` für offene Position.** Ein Aufruf: Abstand SL/TP1/TP2 in Punkten+ATR, Zertifikatspreise über `size.cjs`-Formel, laufendes R-Multiple, MFE, Stall-Zähler, Farbcode. Ersetzt die fehleranfälligste Handrechnung (der dokumentierte Zertifikatspreis-Fehler passierte genau hier). Wirkt ~60×/Stunde bei offener Position.

**A6 (PROZESS) — Setup-Suche-Pflichtzeilen bei offener Position aussetzen.** Chasing-Status, SL-Vorprüfung, Retest-Zeitbox, TP2-Kandidatenzeile, Levelsuche für neue Setups sind bei offener Position gegenstandslos. Deckt sich mit `feedback_positionsfokus_bei_offener_position.md`. **Ausdrücklich NICHT aussetzen:** MTF, QQQ-Dual-Gate, Punkt 11, 9d1, Stall, 8d (Risikoschutz).

### Mittel
**A3 (REGELWERKSFRAGE) — Screenshot-Pflicht an Realität anpassen.** 39 aufeinanderfolgende Auslassungen am 04.09. — keine Disziplinfrage mehr, sondern Regelfrage. Vorschlag: verbindlich bei Strukturänderung/Zonentest/Musterkandidat/8d-Statuswechsel/vor jedem Trigger, ohne Ausnahme; sonst regulär entfallen statt Selbstbefreiung. **Risikoverschiebung: weniger visuelle Kontrolle zwischen zwei Screenshots — Trigger-/Zonentest-Pflicht ist der nicht verhandelbare Teil.**

**A4 (SKRIPT) — A3-Dauerausnahmen abschaffen.** `--trend-effizienz`/`--dual-gate-qqq-abstand-atr` liefen 4/4 am 04.09. per Standard-Ausnahme. Trend-Effizienz selbst berechnen (wie Q2-Budget), QQQ-ATR: entweder Pane-Indikator ergänzen oder Feld streichen.

**A7 (SKRIPT) — Exit-Leiter beim Entry vorberechnen (`exit_plan.cjs`).** BE-Trigger, TP1-Teilverkauf, Trailing-Stufen, Terminalzeit einmal berechnet in Statusdatei; Tick meldet nur noch "Stufe 2 erreicht → SL auf X".

### Niedrig
**A8 — Quick-Tick NICHT anfassen.** 3 Aufrufe (`date`+`data_get_ohlcv summary`+`data_get_study_values`), bereits minimal.

---

## Teil B — Effektivität

### Hoch
**B1 (LAYOUT+SKRIPT) — Anchored VWAP (Session) mit Bändern auf NAS100-Pane.** Auffälligster Befund: **NAS100 hat aktuell KEINE VWAP-Level** — die 5 VWAP-Registereinträge liegen im QQQ-Preisraum (~718) und können nie ein NAS100-TP belegen. Session-AVWAP+1σ/2σ liefert bis zu 5 bewegliche, nicht-rasterkonforme Level genau im gehandelten Bereich. Registerkategorie + Pflegeweg existieren bereits. Sauberster Dichte-Zugewinn: kein Gate, keine Schwelle, nur fehlende Datenquelle.

**B2 (SKRIPT+PROZESS) — Setup-Karte: Entry-Fenster vorbereiten statt im Trigger-Moment berechnen.** Am 04.09. stand der Kurs 18:44-18:48 durchgehend im regelkonformen Entry-Fenster [29529,80; 29543,83]; der Trigger kam 18:49 bei 29553,85 — 10 Pkt zu spät. Latenz zwischen "Struktur wäre handelbar" und "nachgerechnet". Vorschlag: sobald ein Dual-Gate-Bein bestätigt ist, `setup_karte.json` mit Entry-Fenster/TP-Kandidaten erzeugen; Quick-Tick zeigt "Setup-Karte aktiv... fehlt: [Bein]". **Kein Gate wird aufgeweicht** — nur die Rechenzeit verschwindet aus dem Trigger-Moment. Bestes Wirkung-zu-Risiko-Verhältnis der ganzen Liste, wirkt auf A und B zugleich.

### Mittel
**B3 (SKRIPT) — Fib-Retracement (0,618/0,786) des laufenden Impulses als TP-Kandidat.** Extension ist gebaut, Retracement fehlt — klassische Fortsetzungs-TP-Zone, off-grid, gleicher Pflegeweg.

**B4 (REGELWERKSFRAGE+MESSUNG) — Bollinger-Band-Kanten als registerfähige Levelart.** Kostet null Zusatzabrufe (schon in `data_get_study_values`). Erst Schattenmessung im #8-Rahmen: (a) wie oft hätte BB-Kante ein leeres Fenster gefüllt, (b) wie oft wäre das TP historisch erreicht worden. **Risikoverschiebung: hebt PASS-Quote, kann Trefferquote senken — messen vor scharfschalten, wie beim 8c2-Sockel.** Gilt sinngemäß auch für B1/B3.

**B5 (SKRIPT) — Levelvollständigkeits-Zeile in `register_check.cjs`.** Am 04.09. nur 4 von 8 kanonischen Levelarten besetzt (Pivots, PDH/PDL, Session-Extrema, Rundzahlen; Fib/Boxen/eigene Level/NAS100-VWAP = 0). Tägliche Sichtbarkeit statt einmaliger Opus-Entdeckung.

**B7 (PROZESS) — Hypothetisches Ergebnis zu jedem `skipped_setup` nachtragen.** Erfassung ist Pflicht, Gegenrechnung beim Tagesabschluss fehlt (hätte TP1/SL erreicht?). Ohne sie misst man nur Häufigkeit des Nicht-Handelns, nie ob das gut war.

### Niedrig — geprüft und NICHT empfohlen
**B6 — Mittel-Pivots (M1-M4) bringen keine Dichte.** M(PP,R1) am 04.09. = 29.498,83, nur 1,2 Pkt von der ohnehin registrierten Rundzahl 29.500 (innerhalb ±2-Toleranz) — Konfluenz-Beleg, aber keine neue Dichte. Als Konfluenz-Markierung ja, als neue Levelart nein.

---

## Übersicht

| Prio | SKRIPT | PROZESS | LAYOUT | REGELWERKSFRAGE |
|---|---|---|---|---|
| Hoch | A1, A5, B2 | A6, B2 | A2, B1 | – |
| Mittel | A4, A7, B3, B5 | B7 | – | A3, B4 |
| Niedrig | – | A8 (nichts tun) | – | B6 (abgelehnt) |

**Empfohlene Reihenfolge:** A1 → B2 → A5/A6 → B1 → A2 (Testprojekt, Nicht-Handelstag) → Rest.

**Risiko-Ehrlichkeit:**
- Reine Gewinne ohne Risikoverschiebung: A1, A2, A4, A5, A7, B2, B5.
- Echte Verschiebung, Levi entscheidet: A3 (weniger Screenshots), A6 (weniger Prüfzeilen bei offener Position — Opus hält A6 für richtig, weil Fokus statt Auslassung, aber es ist eine Entscheidung).
- Effektivität mit Risikoverschiebung: B1, B3, B4 heben PASS-Quote, können Trefferquote senken — alle drei erst durch #8-Messung, bevor TP1-fähig.
