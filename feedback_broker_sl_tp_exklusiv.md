---
name: feedback-broker-sl-tp-exklusiv
description: Scalable Capital erlaubt keine gleichzeitige SL- und TP-Order auf derselben Position — bei jedem Trade-Setup berücksichtigen
metadata:
  node_type: memory
  type: feedback
  originSessionId: session-2026-07-08
---

Bei Scalable Capital können SL und TP nicht gleichzeitig als aktive Orders auf derselben Position liegen — es ist immer nur eine der beiden Ordertypen aktiv.

**Why:** Bei Trade #14 (08.07.2026) wurde ein SL bei 28.800 gesetzt. Als der Kurs später TP1/TP2-Niveau ansteuerte, stellte sich heraus, dass keine TP-Order parallel hinterlegt werden konnte — außerdem blockierte die aktive SL-Order zusätzlich sogar den manuellen Verkauf, was in Kombination mit einem separaten Broker-Bug (Stornierung der SL ging zeitweise gar nicht) zu einer Situation führte, in der der User bei Erreichen eines TP-Levels nicht hätte verkaufen können, ohne vorher aktiv die SL zu stornieren.

**How to apply:** Bei jedem künftigen Trade-Setup mit SL+TP1+TP2-Plan (siehe [[feedback_chartanalyse]] Punkt 8b) von vornherein einplanen, dass TP1/TP2 NICHT automatisch ausgeführt werden, solange die SL-Order aktiv ist — der User muss beim Erreichen eines TP-Levels manuell die SL stornieren und dann verkaufen (zwei Schritte, nicht einer). Deshalb bei Kursannäherung an TP1/TP2 besonders frühzeitig und deutlich warnen (nicht erst exakt beim Erreichen), damit genug Zeit für den zweistufigen manuellen Prozess bleibt. Alternative für ruhigere Trades: bewusst nur SL ODER nur TP aktiv setzen, je nachdem was gerade wichtiger ist, statt beides parallel zu erwarten.
