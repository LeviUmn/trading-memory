---
name: feedback-trading-tempo
description: "Beim Trading immer maximales Tempo — kurze Ausgaben, und bei Positionsempfehlung erst Stückpreis erfragen, dann Gesamtbetrag nennen"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 220d4224-24b6-42aa-87ad-8a89ca782b91
---

Beim Live-Trading gilt:
1. **Maximales Tempo** — nur kompakte Ausgaben, keine langen Erklärungen während des Tradings
2. **Positionsgröße:** Erst fragen "Was kostet 1 Stk aktuell?" → dann Gesamtbetrag in € nennen, den der User direkt beim Broker eingibt (Broker rechnet Stückzahl automatisch aus)
3. **Tabelle (Entry/SL/TP) nur zeigen wenn Trade tatsächlich eröffnet wird** — nie vorher als "Vorschau" oder Referenz, das wird als Signal missverstanden

**Why:** User gibt beim Broker einen Gesamtbetrag ein, nicht eine Stückzahl. Stückpreis variiert und ist nur der User im Broker-Interface sichtbar.

**How to apply:** Bei jeder Positionsempfehlung: zuerst nach aktuellem Stückpreis fragen, dann Gesamtbetrag empfehlen. Nie Stückzahl empfehlen ohne den aktuellen Preis zu kennen.
