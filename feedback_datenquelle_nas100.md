---
name: feedback-datenquelle-nas100
description: "Beim Traden von NAS100 immer direkte Chart-Bars nutzen, nicht quote_get — quote_get kann stale Daten liefern"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 7c946990-591f-4e7e-acad-80f3259782b4
---

Für den aktuellen Trading-Chart (aktuell NAS100) immer die direkten Chart-Bars verwenden (`chart_get_state` zur Symbol-Bestätigung + `data_get_ohlcv` mit wenigen `count` für den letzten Kurs), NICHT `quote_get`.

**Why:** Am 01.07.2026 lieferte `quote_get(symbol: "FX:NAS100")` über mehrere Checks hinweg eine eingefrorene/stale Quote — Open/High blieben stundenlang unverändert (30.276/30.337), obwohl der Kurs längst gefallen war. Das führte zu einer falschen Kursmeldung (30.030 statt real ~29.978). Der tatsächliche Chart lief auf Feed `FOREXCOM:NAS100` und zeigte den korrekten Live-Kurs über die direkten Bars.

**How to apply:** Bei jedem Live-Check während des Tradings von NAS100 den Kurs aus `data_get_ohlcv` (letzte Bar, `count: 1-5`) oder `chart_get_state` + Screenshot ziehen, nicht aus `quote_get`. Falls künftig ein anderes Instrument getradet wird, muss dieselbe Prüfung (quote_get vs. direkte Chart-Bars vergleichen) für das neue Symbol wiederholt werden, bevor man sich auf eine Quelle verlässt — aktuell ist ausschließlich NAS100 im Fokus ([[feedback_instrumenten_fokus]]).

**Rückfall am 02.07.2026:** Trotz dieser Regel wurde während eines aktiven 1-Min-Live-Trading-Loops `quote_get("FX:NAS100")` erneut parallel mitgezogen und als Kursquelle im Status-Update verwendet — diesmal lieferte es einen zu HOHEN Wert (30.066-30.082 statt echte 30.022 laut Chart-Bars), nicht wie beim ersten Vorfall einen eingefrorenen zu niedrigen. User (Levi) hat den Fehler bemerkt und korrigiert. Der Fehler passiert also in beide Richtungen (zu hoch UND zu alt), nicht nur "stale nach unten". **Verschärfte Regel:** In JEDEM Tool-Call-Batch, der NAS100-Kurs ermitteln soll (auch bei parallelen Checks mit anderen Instrumenten wie VIX), `quote_get` für `FX:NAS100`/`FOREXCOM:NAS100` grundsätzlich weglassen — nicht nur "bevorzugt Chart-Bars nutzen", sondern quote_get für NAS100 komplett vermeiden, auch wenn es aus Gewohnheit/Bequemlichkeit parallel zu anderen quote_get-Calls mit angehängt werden könnte.
