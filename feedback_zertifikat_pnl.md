---
name: feedback-zertifikat-pnl
description: "P&L immer mit echtem Zertifikat-Kurs vom Broker rechnen, nie aus NQ-Punkten schätzen — €/Punkt-Ratio immer live kalibrieren UND per Formel gegenchecken"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 220d4224-24b6-42aa-87ad-8a89ca782b91
---

P&L **nie** aus NQ-Punkten hochrechnen ohne echten Zertifikat-Kurs. Immer:
1. User nach aktuellem Stückpreis fragen
2. Verhältnis ableiten: (Zert-Differenz) / (NQ-Differenz) = €/Punkt
3. **Gegenrechnen mit der Formel:** €/Punkt = Zertifikatspreis × Faktor ÷ Indexstand. Weicht die live kalibrierte Ratio um mehr als ~10% von der Formel ab → Alarm, Werte nochmal prüfen bevor P&L kommuniziert wird.
4. Damit TP/SL in Zertifikat-Preisen ausrechnen

**Korrigiert 04.07.2026 (Fable-5-Review):** Die zuvor hier gespeicherte "Näherung 0,01€ pro NQ-Punkt" (30.06.2026, GS 5x Long, Zert ~40€, NQ ~30.140) war selbst falsch — dieselbe Session hat den Wert später auf **0,0069€/Punkt** korrigiert (siehe [[trading_session_nasdaq]] und trades/trading_2026-06-30.md). Formel-Gegencheck: 40€ × 5 ÷ 30.140 ≈ 0,0066€/Punkt — deckt sich mit 0,0069, nicht mit 0,01. Die alte falsche Zahl darf nicht mehr als Referenz verwendet werden — Ratio IMMER neu aus dem aktuellen Zertifikat/NQ-Paar ableiten, nie aus dieser Datei kopieren.

**Why:** User hat am 30.06. einen ersten Fehler korrigiert (+11€ ausgewiesen statt tatsächlich +2,94€). Der Ratio-Fehler wiederholte sich am selben Tag in der Merkregel selbst — Beweis, dass eine einzelne gespeicherte Zahl ohne Formel-Gegencheck nicht robust genug ist. Jedes neue Produkt (Trades liefen bereits über Zertifikate von 1,33€ bis 40,46€) ist eine neue Fehlerchance.

**How to apply:** Bei jeder Trade-Eröffnung sofort echten Zertifikat-Kurs erfragen, Ratio live kalibrieren UND per Formel gegenrechnen, nie eine alte Zahl aus dieser oder einer anderen Datei wiederverwenden.
