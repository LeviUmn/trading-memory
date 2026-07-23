---
name: feedback-dual-gate-confirmation
description: "Bei Multi-Instrument-Bestätigungs-Setups (NAS100+QQQ EMA-Gate, bis 15.07. NQ1!) beide Seiten mit gleichem Bestätigungsgrad prüfen — Kerzenschluss vs. Live-Tick nicht mischen; Spike-Ausnahme 16.07.2026 auf QQQ neu bewertet"
metadata:
  node_type: memory
  type: feedback
  originSessionId: session-2026-07-07
---

Bei Trade #13 (07.07.2026) wurde ein Long-Setup mit zwei Bedingungen definiert: NAS100 muss Kerze über EMA50 **schließen**, NQ1! muss über eigener EMA50 **stehen**. Im entscheidenden Moment wurde NAS100 korrekt per bestätigtem Kerzenschluss geprüft, NQ1! aber nur per Live-Tick (`quote_get`) — der kurz über der EMA lag, aber auf der tatsächlich geschlossenen Kerze (`data_get_ohlcv`) klar darunter blieb. Trotzdem wurde "Go" gegeben.

**Why:** User bemerkte live: "du sagtest auf einmal go, obwohl du schon meintest, beides muss über EMA schließen" — die Regel wurde selbst formuliert und dann im entscheidenden Moment nicht symmetrisch angewendet. User hat trotzdem korrekt gehandelt (auf explizites Go vertraut, wie vereinbart) — der Fehler lag beim Trigger-Check, nicht bei der Ausführung.

**How to apply:** Bei jedem Multi-Instrument-Bestätigungs-Setup (z.B. [[feedback_live_trading]] Punkt 7b QQQ-Gegencheck, bis 15.07.2026 NQ1!) für ALLE beteiligten Instrumente denselben Bestätigungsgrad verlangen — entweder beide per Kerzenschluss (`data_get_ohlcv`, letzte abgeschlossene Bar) oder beide explizit als "Live-Tick, noch nicht kerzenbestätigt" kennzeichnen. Nie einen Live-Tick für ein Instrument als gleichwertig zu einem Kerzenschluss beim anderen behandeln, auch nicht unter Zeitdruck kurz vor dem eigentlichen Trigger-Moment.

**Nachtrag nach Trade-Ergebnis (07.07.2026, User-bestätigt nach erfolgreichem Trade #13):** Der Trade lief am Ende sehr profitabel (+46,25€), obwohl NQ1! beim Go-Signal nur per Live-Spike (nicht per Kerzenschluss) über der EMA50 stand. User-Rückmeldung explizit: "genau richtig".

**Wichtig — Nachtrag NICHT als "Spikes reichen immer" fehlinterpretieren (präzisiert nach Nachfrage 07.07.2026):** Ein einzelner erfolgreicher Trade beweist nicht, dass die Kerzenschluss-Anforderung unnötig ist — es ist Stichprobengröße 1. Der Spike war in diesem Fall kein isoliertes Signal, sondern von mehreren weiteren Faktoren gestützt: NAS100 war zu dem Zeitpunkt bereits per echtem Kerzenschluss über EMA50 bestätigt, MACD war auf der Rohlinie (nicht nur Histogram) positiv gecrosst, RSI kreuzte 50, UND NQ1! zeigte dabei steigendes Volumen. Der eigentliche Zweck der Kerzenschluss-Regel bleibt bestehen: einen Fehlausbruch (Spike kehrt sofort um, während NAS100 auf dünner CFD-Liquidität weiterläuft) auszufiltern — das ist hier nicht passiert, hätte aber passieren können.

**Präzisierte Regel (Symbole 16.07.2026 auf QQQ umgestellt, inhaltlich neu bewertet — siehe Abschnitt unten):** Ein QQQ-Live-Spike über der EMA50 ist NUR dann als Trigger akzeptabel, wenn er von mindestens diesen Zusatzsignalen gestützt wird: (1) NAS100 bereits per echtem Kerzenschluss bestätigt, (2) MACD-Rohlinie (nicht nur Histogram) positiv, (3) steigendes Volumen auf QQQ selbst (relativ zum eigenen QQQ-Durchschnitt, siehe [[feedback_live_trading]] Punkt 7b). Ein isolierter QQQ-Spike ohne diese Zusatzbestätigung reicht NICHT — dort weiter auf den QQQ-Kerzenschluss bestehen. Diese Bedingung ersetzt die vorherige, zu pauschale Formulierung "Live-Tick-Trigger sind erlaubt, solange transparent benannt".

---

## Neubewertung nach dem NQ1!→QQQ-Wechsel (16.07.2026, Fable-Review)

Diese Regel wurde ursprünglich auf NQ1! kalibriert — und zwar zu einem Zeitpunkt, als noch niemand wusste, dass der NQ1!-Feed lizenzbedingt konstant 10 Minuten verzögert war ([[feedback_nq1_feed_lag]]). Das ändert die Einordnung in zwei Richtungen:

1. **Rückblickend:** Was am 07.07.2026 als "NQ1!-Live-Spike" behandelt wurde, war in Wahrheit ein ~10 Minuten alter Datenpunkt. Das Go-Signal bei Trade #13 war also noch schwächer fundiert als damals angenommen — nicht "Live-Tick statt Kerzenschluss", sondern "veralteter Tick statt Kerzenschluss". Dass der Trade trotzdem gewann, sagt entsprechend noch weniger über die Tragfähigkeit von Spike-Triggern aus als die ohnehin dokumentierte Stichprobengröße-1-Warnung oben.

2. **Ab jetzt (QQQ, Echtzeit):** Die Regel bleibt inhaltlich in Kraft, denn ihr Kern ist vom Feed-Lag unabhängig: Die Kerzenschluss-Anforderung filtert Intrabar-Fehlausbrüche (Spike kehrt vor Kerzenschluss wieder um) — dieses Risiko besteht bei einem Echtzeit-Feed genauso wie vorher. Neu ist: Ein QQQ-Live-Tick bildet jetzt tatsächlich die Gegenwart ab, d.h. die drei Zusatzsignale prüfen jetzt wirklich das, wofür sie gedacht waren, statt zusätzlich unbemerkt 10 Minuten Vergangenheit zu kompensieren. **Zusätzliche Voraussetzung seit QQQ:** Vor jeder Anwendung der Spike-Ausnahme muss das Session-Gate inkl. Zeitstempel-Check ([[feedback_live_trading]] Punkt 7e) bestanden sein — ein "Live-Spike" auf einem Bar, der älter als 1 Kerze ist (QQQ-Session zu oder Feed-Problem), ist kein Live-Spike und die Ausnahme greift nicht.
