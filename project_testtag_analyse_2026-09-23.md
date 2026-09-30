---
name: project-testtag-analyse-2026-09-23
description: "Opus-Analyse Testtag 23.09.2026 (fiktiv, 0 Trades) mit Levis Kernfrage: festgefahren durch Regeln oder gut kalibriert (selten, aber treffsicher)? Urteil: FESTGEFAHREN, nicht belegt kalibriert. Auftakt einer mehrteiligen Messreihe, Fortsetzung in [[project_testtag_2026-09-23_besprechung_ausstehend]]."
metadata:
  type: project
  originSessionId: session_2026-09-24
  modified: 2026-09-24T09:38:48.787Z
---

## Auftrag und Kontext

Levi wollte am 24.09.2026 explizit wissen, ob sich das System durch die vielen Iterationen (X1-X8, Y1-Y8, Trendtag-Modus) mit Regeln festgefahren hat, sodass strukturell kaum noch Trades möglich sind — oder ob es inzwischen so gut kalibriert ist, dass es selten, aber mit hoher Trefferqualität tradet. Opus (frischer Subagent, eigenständig aus Rohdaten, vor jedem Narrativ) hat den fiktiven Testtag 23.09.2026 analysiert.

## Kernbefund: FESTGEFAHREN, nicht belegt kalibriert

**Fakten 23.09.:** 49 Voll-Checks, 15:39-20:03 DE, **0 Trades, 0 echte Live-Gate-Läufe** (nur 96 SL-Vorprüfungen). Dual-Gate stand in 46/47 Voll-Checks durchgehend 2/2 SHORT (Korrektur an einer ursprünglichen Fehlannahme "Whipsaw" — stimmte laut Rohlog nicht). SL-Anker (30715.35, Spike-Hoch) wurde in ALLEN 47 Voll-Checks nie nachgezogen, obwohl X1 durchgehend "Reset fällig" meldete → TP1-Fenster war 47/47 Mal LEER (max. RR 0,26-0,65).

**Selbst mit regelkonformem Anker-Reset:** weiterhin 0 Trades (Fenster nur ~1,1 Pkt breit) — der Anker-Bug war NICHT die Hauptursache.

**Strukturbefunde:**
- **Q4** ist bei ATR > ~36 mathematisch unerfüllbar (50er-Raster-Pflicht + RR≥1 + Mindest-SL) — reine Rechenmechanik, keine Setup-Bewertung
- **Q2** an Trendtagen praktisch nie erfüllbar (Y4-Schwelle ADX≥45 wurde am 23.09. nie erreicht, Max 42,1)
- **Muster über 6+ Testtage**, kein Einzeltag (01./03./04./10./15./16./17./21.09.)

**Gegenprobe "selten, aber treffsicher"?** Widerlegt. Vom Q-Score blockierte Setups hätten häufiger TP1 erreicht als die durchgelassenen. Seit 11.09. kein einziger voller TP1→TP2-Lauf mehr, netto ≈ −4R über 17 Testtage.

## Opus' ursprüngliche Vorschläge V1-V6 (Priorität hoch: V1-V3)

- **V1:** Q4 vom 50er-Raster entkoppeln
- **V2:** Q-ROT als Sizing statt Veto (Viertel-/Halbposition statt Auslassen)
- **V3:** Anker-Definition für Fortsetzungen erweitern (jüngster bestätigter LH/HL zusätzlich zum "ersten nach Extrem")
- **V4:** Y4-Schwelle (ADX≥45) prüfen
- **V5:** 1H-Override auswerten (`oneh_shadow_log.jsonl`)
- **V6:** Loop-Warnung bei X1 dreimal ohne Reset

Explizit NICHT vorgeschlagen: Lockerung von RR≥1, 3×ATR-Deckel oder Sperrfristen — diese haben historisch echte Verlierer gefiltert.

## Grenzen des Urteils (Opus selbst benannt)

Simulation auf 1-Min-Schlusskursen statt echten 5-Min-Bars (CDP-Verbindung am 23.09. ausgefallen). Q2/Q4 am 23.09. nachgerechnet, nicht live gemessen. Kleine, korrelierte Stichprobe (~19 Q-blockierte Setups in ~7 Clustern an 3 Tagen). Eigenes Overfitting-Risiko benannt: Vorschläge sind Nachschärfungen, deshalb max. ein Hebel pro Validierungstag, vorher mit Logs belegen.

**Fortsetzung/Umsetzungsstand:** siehe [[project_testtag_2026-09-23_besprechung_ausstehend]].
