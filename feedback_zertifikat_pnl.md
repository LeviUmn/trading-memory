---
name: feedback-zertifikat-pnl
description: "P&L immer mit echtem Zertifikat-Kurs vom Broker rechnen, nie aus NQ-Punkten schätzen — €/Punkt-Ratio immer live kalibrieren UND per Formel gegenchecken"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 220d4224-24b6-42aa-87ad-8a89ca782b91
  modified: 2026-08-04T14:49:02.487Z
---

P&L **nie** aus NQ-Punkten hochrechnen ohne echten Zertifikat-Kurs. Immer:
1. User nach aktuellem Stückpreis fragen
2. Verhältnis ableiten: (Zert-Differenz) / (NQ-Differenz) = €/Punkt
3. **Gegenrechnen mit der Formel:** €/Punkt = Zertifikatspreis × Faktor ÷ Indexstand. Weicht die live kalibrierte Ratio um mehr als ~10% von der Formel ab → Alarm, Werte nochmal prüfen bevor P&L kommuniziert wird.
4. Damit TP/SL in Zertifikat-Preisen ausrechnen

**Korrigiert 04.07.2026 (Fable-5-Review):** Die zuvor hier gespeicherte "Näherung 0,01€ pro NQ-Punkt" (30.06.2026, GS 5x Long, Zert ~40€, NQ ~30.140) war selbst falsch — dieselbe Session hat den Wert später auf **0,0069€/Punkt** korrigiert (siehe [[trading_session_nasdaq]] und trades/trading_2026-06-30.md). Formel-Gegencheck: 40€ × 5 ÷ 30.140 ≈ 0,0066€/Punkt — deckt sich mit 0,0069, nicht mit 0,01. Die alte falsche Zahl darf nicht mehr als Referenz verwendet werden — Ratio IMMER neu aus dem aktuellen Zertifikat/NQ-Paar ableiten, nie aus dieser Datei kopieren.

**Why:** User hat am 30.06. einen ersten Fehler korrigiert (+11€ ausgewiesen statt tatsächlich +2,94€). Der Ratio-Fehler wiederholte sich am selben Tag in der Merkregel selbst — Beweis, dass eine einzelne gespeicherte Zahl ohne Formel-Gegencheck nicht robust genug ist. Jedes neue Produkt (Trades liefen bereits über Zertifikate von 1,33€ bis 40,46€) ist eine neue Fehlerchance.

**How to apply:** Bei jeder Trade-Eröffnung sofort echten Zertifikat-Kurs erfragen, Ratio live kalibrieren UND per Formel gegenrechnen, nie eine alte Zahl aus dieser oder einer anderen Datei wiederverwenden.

**Rückfall 04.08.2026 (Trade #30):** Bei Entry (7x, NAS100 29.340/25,96€) wurde TP1/TP2 zunächst rein aus der theoretischen Hebelformel berechnet (0,0062€/Punkt), ohne sofortigen Live-Abgleich. Erst ~20 Min später fiel dem User selbst auf, dass der tatsächliche Broker-Kurs abwich (26,325€ statt errechneter 26,394€ bei NAS100 29.410) — reale Ratio war 0,00521€/Punkt, effektiver Hebel nur ~5,9x statt nominell 7x (Spread/Emittenten-Kosten). Nach Rekalibrierung stimmten TP1/TP2 deutlich genauer. **Bestätigt: der Formel-Check allein reicht nicht, wenn er nicht AM Entry-Moment mit einem echten Kurspaar erfolgt** — die Regel oben (Schritt 1-3 sofort bei Trade-Eröffnung) muss aktiv ausgeführt werden, nicht nur als Fallback bereitstehen, falls der User eine Abweichung bemerkt.
