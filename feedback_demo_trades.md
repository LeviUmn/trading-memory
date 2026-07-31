---
name: feedback-demo-trades
description: "Demo-Loop fuer Dritte (z.B. Freunde) — voller Regelsatz inkl. aktiver Blackouts, aber KEIN Eintrag in Trade-DB/trade_log"
metadata:
  node_type: memory
  type: feedback
  originSessionId: session-2026-07-29
  modified: 2026-07-29T16:49:53.031Z
---

Am 29.07.2026 (FOMC-Tag) wurde erstmals eine Live-Loop-Demo fuer Levis besten Freund durchgefuehrt — 4x 5-Min-Kerzen NAS100/QQQ beobachtet, mit 1-Min-Quick-Ticks + 5-Min-Voll-Checks per `CronCreate`, wie im echten Live-Trading.

**Regel:** Auch bei einer reinen Demo (kein echtes Geld, kein echter Trade) gilt:
1. Der volle 1-Min/5-Min-Loop mit allen Regeln (MTF-Wechsel 15min/1H, QQQ-Dual-Gate, Chasing-Check, Entscheidungsbaum mit Kerzenfarbe/Schlusstyp) laeuft unveraendert — User-Wunsch explizit "im richtigen Loop mit allen Regeln und allen Updates", nicht eine vereinfachte Schau-Version. Auch eine konkrete Long/Short-Einschaetzung mit Entry-Optionen gehoert dazu, wenn danach gefragt wird.
2. Aktive News-/Event-Blackout-Regeln (z.B. FOMC, siehe [[feedback_session_update]]) bleiben auch in der Demo in Kraft — selbst ein technisch sauber ausgeloestes Setup wird explizit als "kein Entry heute wegen Blackout" gekennzeichnet, nicht stillschweigend anders behandelt als an einem normalen Handelstag.
3. Ein waehrend einer Demo entstandenes (simuliertes) Setup wird NICHT in `trade_db.cjs`/`trade_log.md` eingetragen — das wuerde die echte Trade-Statistik ([[feedback_memory_pflege]]) verfaelschen.

**Why:** Levi wollte seinem Freund die reale Arbeitsweise zeigen, nicht eine geschoente Version — die Glaubwuerdigkeit der Demo haengt daran, dass exakt dieselbe Disziplin gilt wie im echten Live-Trading, inklusive der Momente, in denen das System sich trotz gutem Setup selbst zurueckhaelt (siehe [[project_fomc_29_07_2026]]).

**How to apply:** Bei kuenftigen "zeig mal/Demo"-Anfragen fuer Dritte denselben vollen Regelsatz + aktives Blackout-Bewusstsein anwenden wie im echten Trading, aber am Ende explizit sicherstellen, dass keine Demo-Daten in die reale Trade-Statistik einfliessen.
