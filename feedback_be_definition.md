---
name: feedback-be-definition
description: "Verbindliche Breakeven-Klassifizierung für Trade-Statistiken — löst die alte #2-vs-#11-Inkonsistenz auf, festgelegt 09.07.2026 nach Fable-5-Review"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: e3468883-3dd5-4cd2-aa4f-c6979adc898f
---

Ein abgeschlossener Trade zählt als **BE (Breakeven)**, wenn eine dieser Bedingungen zutrifft:
1. Ergebnis ist ±0,00€
2. SL wurde auf Entry/Breakeven nachgezogen und der Trade wurde durch Slippage minimal negativ ausgelöst (Beispiel: [[trades/trade_log]] Trade #11, -0,27% durch 0,01€/Stück Slippage bei exakt auf Entry nachgezogenem SL)
3. Der Trade wurde bewusst manuell bei Breakeven geschlossen

Jeder Close im Minus, der NICHT über einen BE-nachgezogenen SL lief, ist ein **Loss** — unabhängig von der Höhe (auch ein kleiner Verlust bleibt Loss).
Jeder Close im Plus ist ein **Win** — auch wenn er klein ist (Beispiel: Trade #5, +0,28%, Close vor TP1 im Gewinn zählt als Win, nicht als BE).

**Why:** Am 04.07.2026 (Fable-5-Review) fiel auf, dass Trade #2 (~0€) als "Win/Kapitalschutz" und Trade #11 (-2,68€, ebenfalls durch BE-SL+Slippage entstanden) als "Loss" geführt wurden — inkonsistent trotz vergleichbarem Mechanismus. Am 09.07.2026 (zweites Fable-5-Review vor dem Phase-1→2-Übergang) wurde die Klärung explizit angefordert. Der User hat die Regel mechanismus-basiert definiert (Ergebnis-Vorzeichen + War-SL-auf-BE-nachgezogen), nicht über einen reinen %/€-Schwellenwert — ein rein prozentualer Schwellenwert (z.B. Fables Vorschlag "<0,3% = BE") hätte echte kleine Gewinne wie Trade #5 fälschlich zu BE gemacht.

**How to apply:** Bei jeder künftigen Trade-Klassifizierung in [[trades/trade_log]] diese drei Kriterien prüfen, bevor Win/Loss/BE eingetragen wird. Bei Statistik-Auswertungen (Win-Rate, Expectancy) BE-Trades aus dem Nenner der Win-Rate ausklammern, aber sekundär auch die inklusive Zahl nennen (siehe Kopfzeile in [[trades/trade_log]]: 61,5% exklusive BE / 53,3% inklusive BE bei Phase-1-Abschluss).
