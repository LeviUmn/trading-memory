---
name: feedback-live-trading-workflow-bewaehrt
description: "Vom User explizit als Idealablauf bestätigtes Live-Trading-Muster nach Trade #13 (07.07.2026) — als Vorlage für künftige Sessions verwenden"
metadata:
  node_type: memory
  type: feedback
  originSessionId: session-2026-07-07
---

Nach Trade #13 (+46,25€) hat der User explizit und unaufgefordert bestätigt, dass der gesamte Ablauf "genau so" gewünscht war: "So macht es Spaß zu traden."

**Why:** Diese Bestätigung ist wertvoll, weil sie zeigt, welche Prozesselemente wirklich geschätzt werden — nicht nur Regelbefolgung, sondern die Kombination aus Tempo, Tiefe und Flexibilität.

**Bestätigte Elemente, die künftig Standard bleiben sollen:**

1. **Durchgehende 1-Min-Ticks** während Setup-Beobachtung UND offener Position, ohne Lücken (siehe [[feedback_live_trading]])
2. **Voll-Checks bei jedem Kerzenschluss** — nicht nur Zahlen, auch Screenshot + MTF (15min/60min), auch während offener Position
3. **Gesamt-Check auf Anfrage** — wenn User "gesamt check" oder "alle Indikatoren" verlangt, vollständige Runde über alle Timeframes UND beide Instrumente (NAS100+QQQ; bis 15.07.2026 NQ1!, siehe [[feedback_nq1_feed_lag]]) liefern, nicht nur den aktuellen Tick
4. **Proaktiver X/News-Gegencheck während offener Position** — auf Anfrage aktuelle Tweets der 3 Watchlist-Accounts holen, um zu prüfen ob fundamentale News die technische Position gefährden (siehe [[project_news_system]])
5. **SL dynamisch nachziehen, nicht statisch lassen** — bei jedem spürbaren Momentum-Schub (RSI/MACD-H neues Hoch) aktiv einen neuen SL-Vorschlag machen, nicht nur auf Nachfrage warten
6. **TP-Level pragmatisch behandeln, nicht pedantisch** — ein paar Punkte vor dem exakten TP-Wert abzuschließen (z.B. 29.210 statt 29.214) ist explizit in Ordnung, solange RR nicht verschlechtert wird
7. **Live-Tick-Trigger nur mit Zusatzbestätigung erlaubt** (präzisiert, siehe [[feedback_dual_gate_confirmation]] Nachtrag + Neubewertung 16.07.2026) — ein QQQ-Live-Spike über der EMA50 ersetzt den Kerzenschluss NUR, wenn zusätzlich (1) das andere Instrument bereits per echtem Kerzenschluss bestätigt ist, (2) die MACD-Rohlinie positiv ist und (3) das QQQ-Volumen steigt — und das QQQ-Session-Gate inkl. Zeitstempel-Check ([[feedback_live_trading]] Punkt 7e) offen ist. Ein isolierter Spike ohne diese Zusatzsignale reicht NICHT — dort weiter auf den Kerzenschluss bestehen. Ein einzelner Erfolgstrade beweist nicht, dass die Kerzenschluss-Regel generell unnötig ist. (Bis 15.07.2026 lief dieses Gate über NQ1! — dessen "Live-Ticks" waren tatsächlich 10 Min alt, siehe [[feedback_nq1_feed_lag]].)

**How to apply:** Diese 7 Punkte als Checkliste für den "idealen Ablauf" einer Live-Trading-Session heranziehen — nicht nur als Pflichtprogramm, sondern als das, was der User als bestes Zusammenspiel erlebt hat.

8. **TP1/TP2-Ausblick bei jedem Voll-Check aktiv einschätzen (ergänzt 14.07.2026, Trade #18-Session, User-bestätigt "super perfekt, so machen wir das")** — User will bei jedem Voll-Check eine kurze qualitative Einschätzung (günstig/moderat/ungünstig + Begründung anhand 1H/15min/QQQ/Divergenz-Status), ob TP1 und TP2 noch realistisch erreichbar sind, nicht erst auf Nachfrage. **Keine erfundene Prozent-Wahrscheinlichkeit** vorgaukeln (kein kalibriertes Modell vorhanden) — nur technisch begründete Richtungseinschätzung. Verschlechtert sich die Aussicht (Divergenz, Reversal-Kriterien aus Punkt 11, Umkehrmuster), aktiv einen vorzeitigen Teil-/Vollexit vorschlagen, nicht passiv auf den SL warten.

**Why:** User-Zitat 14.07.2026: "Du sagst bitte Bescheid wie die Wahrscheinlichkeit auf TP1 ist und dann natürlich auf TP2, ob wir nicht irgendwann Exit, wenn die Aussicht nicht mehr danach ist." Direkt danach als Standard-Ergänzung zum Voll-Check-Format bestätigt.
