---
name: feedback-positions-farbcode
description: "Positions-Ticks: SL immer mit 🔴, TP1/TP2 immer mit 🟢 markieren, zusätzlich Zertifikatspreis (€) neben Punkte-Abstand anzeigen"
metadata:
  node_type: memory
  type: feedback
  originSessionId: session-2026-07-14
  modified: 2026-07-23T15:59:19.269Z
---

Im laufenden Positions-Monitoring (1-Min-Loop bei offener Position) beim Anzeigen von SL/TP:

- SL immer mit 🔴 einleiten (echte Textfarben sind im Chat nicht möglich, siehe [[feedback_live_trading]] Punkt 7 — Emoji sind der etablierte Farb-Ersatz)
- TP1 und TP2 immer mit 🟢 einleiten
- **Absolutes NAS100-Kurslevel zeigen, NICHT die Restdistanz in Punkten** (korrigiert 14.07.2026 — User will wissen WO SL/TP liegen, nicht wie viele Punkte noch fehlen)
- Zusätzlich den Zertifikatspreis in € ergänzen

**Format:**
```
🔴 SL <NAS100-Level> (<Zertifikatspreis>€) | 🟢 TP1 <NAS100-Level> (<Zertifikatspreis>€) | 🟢 TP2 <NAS100-Level> (<Zertifikatspreis>€)
```

Beispiel: `🔴 SL 29.380 (70,88€) | 🟢 TP1 29.760 (75,58€) | 🟢 TP2 29.921 (77,57€)`

**Why:** User-Wunsch 14.07.2026 (Trade #18-Session) — SL/TP sollen auf einen Blick farblich wie Risiko/Chance unterscheidbar sein. Erste Version zeigte die Restdistanz in Punkten, User korrigierte das noch am selben Tag: die absoluten Kurslevel sind die eigentlich relevante Information (wo genau SL/TP liegen), nicht wie weit der aktuelle Kurs noch entfernt ist.

**How to apply:** Gilt ab sofort für jeden Quick-Tick und Voll-Check während einer offenen Position. Die Restdistanz kann bei Bedarf zusätzlich erwähnt werden, aber das absolute Level ist die Pflichtangabe, nicht optional.

**Wiederkehrender Vorfall (verschärft 23.07.2026, Trade #25 — laut User "wie die letzten alle Male"):** Der Fehler passiert konsistent an derselben Stelle: der allererste Positions-Kasten direkt nach einer neuen Order-Bestätigung (SL-Berechnung unmittelbar nach "Bin short/long, X Stk...") wird ohne 🔴🟢-Farbcode und/oder ohne Zertifikatspreis ausgegeben — die Regel wird erst angewendet, nachdem der User es erneut einfordert. Grund vermutlich: Diese Situation wird als "spontane Berechnung" statt als "Positions-Tick" wahrgenommen, obwohl sie es strukturell ist. **How to apply (verschärft):** Die Regel gilt ab dem ALLERERSTEN SL/TP-Kasten nach einer Order-Bestätigung, nicht erst ab dem ersten Loop-Tick danach — keine Übergangsphase ohne Farbcode/Zertifikatspreis, auch nicht bei der initialen Order-Berechnung selbst.
