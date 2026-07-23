---
name: feedback-order-bestaetigung
description: "Order-Ausführung beim Broker immer aktiv bestätigen lassen, nie als ausgeführt annehmen"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: trading-session-2026-06-22
---

Nie davon ausgehen, dass eine geplante Order so ausgeführt wurde wie besprochen — immer aktiv nach Richtung, Stückzahl und Preis fragen bzw. bestätigen lassen, bevor ich eine Position als "offen" tracke.

**Why:** Am 22.06.2026 hat der User beim Order-Klick bei Scalable Capital versehentlich Long statt Short gekauft. Ich bin auf Basis der Planung davon ausgegangen, der Short sei live, und habe SL/TP/P&L auf eine Position getrackt, die in Wirklichkeit nie existierte — bis der User das klarstellte. Kein Schaden entstanden (User hat den Fehlklick selbst bemerkt und mit +-0€ glattgestellt), aber das hätte bei größerem Kursausschlag zu falscher Einschätzung der echten Risikolage führen können.

**How to apply:** Nach jeder "Order ist durch"-Meldung des Users kurz Richtung + Stückzahl + Preis explizit zurückspiegeln und bei Unsicherheit nachfragen ("Long oder Short? Zu welchem Kurs?"), bevor ich anfange SL/TP/P&L live zu tracken. Siehe [[trade-2026-06-22]] für den konkreten Vorfall.
