---
name: feedback-broker-wert-prioritaet
description: "Bei Trade-Ergebnissen gilt der vom User bei Scalable Capital abgelesene/bestätigte Wert als Quelle der Wahrheit, nicht eine aus notierten Fill-Preisen zurückgerechnete Zahl"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: e3468883-3dd5-4cd2-aa4f-c6979adc898f
---

Wenn ein vom User genannter, bei Scalable Capital abgelesener Trade-Ergebniswert von einer aus einzelnen Fill-Preisen zurückgerechneten Zahl abweicht, gilt **immer der vom User abgelesene Broker-Wert** als korrekt — nicht die Rückrechnung.

**Konkreter Fall (aufgedeckt 09.07.2026, Fable-5-Review):** Trade #13 (07.07.2026) wurde in [[trades/trade_log]] mit +42,96€ geführt (aus den notierten Tranchen-Fills 252 Stk @3,02€ + 253 Stk @3,09€ auf Entry 2,97€ zurückgerechnet), obwohl in der Tagesdatei direkt daneben "Gesamtergebnis (Broker-bestätigt): +46,25€" stand. Auf Nachfrage entschied der User explizit: der abgelesene Wert (+46,25€) ist korrekt, nicht die Rückrechnung. Grund: notierte Fill-Preise können vereinfacht/gerundet sein oder einzelne Teilausführungen zu leicht anderen Kursen nicht vollständig abbilden — der Broker-Kontostand ist die Ground Truth.

**Why:** User-Zitat: "Wir nehmen immer das, was ich dir geschrieben habe von den Werten, die ich bei Scalable abgelesen habe." Rückrechnungen aus einzelnen Fill-Preisen sind eine Plausibilitätsprüfung, kein Ersatz für den tatsächlichen Kontostand.

**How to apply:** Bei künftigen Trades den vom User genannten/abgelesenen €-Betrag direkt übernehmen. Wenn eine Rückrechnung aus Fill-Preisen zur Kontrolle abweicht, das als Hinweis auf einen Notiz-/Rundungsfehler in den Fill-Preisen behandeln (z.B. aktiv nachfragen, ob ein Fill anders lief als notiert) — aber nie eigenmächtig den Broker-Wert durch die Rückrechnung ersetzen.
