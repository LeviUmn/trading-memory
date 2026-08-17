---
name: feedback-trading-zeitfenster
description: "Täglicher Ablauf — Briefing vs. Backtesting getrennt, beste Entry-Zeitfenster für Nasdaq"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 607aa8f6-9958-4c1c-9c75-4afabcffb717
  modified: 2026-08-12T09:58:57.734Z
---

Festgelegter Tagesablauf und Zeitfenster-Strategie, besprochen am 22.06.2026.

**Why:** User wollte wissen ob tägliche Strategie-Prüfung zu risikoavers ist und wann am besten gehandelt wird. Wichtig: Tages-Briefing und echtes Backtesting sind zwei verschiedene Dinge mit unterschiedlicher Häufigkeit.

**How to apply:** Diesen Ablauf jeden Trading-Tag einhalten.

### Briefing vs. Backtesting (nicht verwechseln!)
- **Tages-Briefing:** Jeden Trading-Tag, 10-15 Min — Bias, Marktregime, Levels, Positionsgröße. Verzögert KEINE Einstiege.
- **Echtes Backtesting:** Wöchentlich/monatlich, separater Termin — Strategie-Regeln anhand Trade-Historie verfeinern.
- Trade-Trigger bleibt immer Echtzeit (Support-Bounce + Bestätigungskerze) — nicht auf "perfektes Backtest-Ergebnis" warten.

### Trading-Zeitfenster (US-Börse öffnet 15:30 Uhr deutscher Sommerzeit, NICHT 17:00!) — Open-Fenster überarbeitet 23.07.2026, Fable-Review
| Zeit (DE) | Aktivität |
|---|---|
| 09:00-10:00 | Morgen-Briefing (Macro/News), kein Trade |
| 15:00-15:30 | Vorbereitung — Loop/Session-Update, Kontext aufbauen (vorher 14:30-15:30, siehe Korrektur unten) |
| 15:30-16:00 | **Aktive Setup-Suche erlaubt, Entry möglich — aber Pflicht-Halbierung der Position** (vorher: nur beobachten, kein Entry) |
| 16:00-18:00 | **Beste Entry-Zone** — volle Positionsgröße bei bestätigtem Setup |
| 20:00-22:00 | Zweites Fenster, aber enger werden (Tagesende) |

### Korrektur 23.07.2026 (Fable-Review, nach Levis Late-Entry-Beobachtung/Trade #24)

**Herkunft der alten 30-Minuten-Regel geprüft:** Der harte "15:30-16:00 nur beobachten"-Stop wurde am 22.06.2026 ohne eigene Kalibrierung eingeführt — wörtlich als Analogie übernommen ("Gleiche Logik wie SpaceX-Strategie", ein Einzelaktien-/IPO-Kontext), nie an echten NAS100/QQQ-Daten getestet. Gleichzeitig ist das Dual-Gate laut [[feedback_live_trading]] Punkt 7e schon ab 15:30 technisch voll gültig — der reine Uhrzeit-Puffer bis 16:00 hat also keine Datenqualitäts-Grundlage. Bei Trade #24 (22.07.2026) lief der Kurs genau in diesem 15:30-16:00-Fenster bereits ~200 Punkte, bevor überhaupt gehandelt werden durfte — reines Uhrzeit-Artefakt, kein Setup-Problem.

**Neue Regel (Kompromiss, nicht volle Übernahme von Levis Vorschlag):** Die Grundsorge (Open-Volatilität = mehr Fakeouts) ist real und wird nicht ignoriert, aber nicht mehr mit einem starren Handelsverbot behandelt, sondern mit demselben Sizing-Mechanismus wie Chop (Punkt 8d) und Chasing (Punkt 13):
1. Ab 15:00: Vorbereitung/Loop-Start, Kontext aufbauen (Levi ist ohnehin ab ~14:30 vorbörslich am Rechner)
2. Ab 15:30: aktive Setup-Suche, Entry bei voller Regelbestätigung erlaubt — **Position dabei zwingend halbiert**, unabhängig davon ob Chop/Chasing zusätzlich vorliegen (Stacking-Hinweis: liegt zusätzlich Chop ODER Chasing vor, bleibt es bei der einen Halbierung, kein Multiplizieren — zentrale Regel siehe [[project_risikomanagement]], Abschnitt "Stacking-Regel für gleichzeitige Halbierungs-Gründe")
3. Ab 16:00: volle Positionsgröße bei bestätigtem Setup, wie bisher

**Review-Pflicht (analog 7a1/Punkt 12):** Nach den nächsten 5 Entries im 15:30-16:00-Fenster explizit auswerten, ob diese öfter/stärker ausgestoppt wurden als 16:00+-Entries. Bestätigt sich die Fakeout-Sorge nicht, Halbierungs-Pflicht für dieses Fenster wieder streichen und rein regelbasiert ohne Uhrzeit-Faktor behandeln — Basis: n=0 bei Einführung, bewusst noch nicht abgesichert.

**Terminierung (07.08.2026, Levi-Entscheidung nach Fable-Regelwerk-Audit, siehe [[project_regelwerk_audit_2026-08-07]]):** Status seit Einführung (23.07.) nicht aktualisiert/gegen die Trade-DB geprüft. Levi terminiert diese Review explizit auf **nach Abschluss von Phase 3** (siehe [[project_risikomanagement]]), analog zur Stall-Exit-Review in [[feedback_live_trading]] Punkt 12.

### Review-Ergebnis 12.08.2026 (Fable, Punkt 4 des Phase-1-3-Audits, siehe [[project_regelwerk_audit_2026-08-07]] — überfällige Review jetzt tatsächlich durchgeführt statt weiter aufgeschoben)

**Datenbasis:** `scripts/trades.db` (35 Trades) per direkter SQLite-Abfrage + alle Tages-Dateien seit Regel-Einführung (23.07.2026) auf Entry-Zeitstempel geprüft (Trade_log liefert keine Uhrzeiten, deshalb Tages-Dateien als Primärquelle genutzt). Ergebnis: tatsächlich im 15:30-16:00-Fenster eröffnete UND halbierte Trades: **n=3** (#28, #30, #31) — deutlich unter den in der ursprünglichen Review-Pflicht vorausgesetzten 5. Mehrere Entries lagen knapp außerhalb des Fensters und zählen nicht mit: #24 (22.07., Entry exakt 16:00:00 — der Fall, der die Regel überhaupt erst auslöste, selbst aber am Rand liegt, nicht im Fenster), #29 (03.08., Entry 16:05:36), #32 (05.08., 16:34:58), #33 (06.08., 16:15:08), #34 (07.08., 16:15:58), #35 (07.08., 17:04:58) sowie #25/#26/#27, alle nach 16:00.

**Die 3 gefundenen Fälle:**
- **#28** (31.07.) Entry 15:50:08, halbiert (229 Stk statt ~458 bei Vollgröße), **WIN +13,80€**
- **#30** (04.08.) Entry 15:50:05, halbiert (77 Stk / 1.998,92€ statt ~4.000€), **WIN +33,75€**
- **#31** (05.08.) Entry vor SL-Zeit 16:01:41, halbiert (401 Stk / 2.501,65€ statt ~5.000€), **LOSS -39,80€**

**Befund:** 2 von 3 Fenster-Trades waren Gewinne — dort hat die Halbierung nur Gewinn gekostet (grobe lineare Skalierung: +27,60€ statt +13,80€ bzw. +67,50€ statt +33,75€ bei Vollgröße). Der eine Verlust (#31) ist exakt der Fall, den die Regel absichern soll: bei Vollgröße wäre der Verlust ungefähr doppelt so hoch ausgefallen (≈-79,60€ statt -39,80€). Rechnet man alle drei Trades bei angenommener linearer Vollgrößen-Skalierung zusammen, ergäbe sich bei Vollgröße ein Netto von **+15,50€** (27,60 + 67,50 - 79,60) gegenüber dem tatsächlichen halbierten Netto von **+7,75€** — bei dieser kleinen Stichprobe wäre Vollgröße also sogar besser gefahren, weil zwei Gewinne den einen Verlust überwiegen. Diese Rechnung ist nur eine grobe Näherung (ignoriert Slippage/Fill-Nichtlinearität bei größerer Stückzahl) und bei n=3 statistisches Rauschen, kein belastbares Muster.

**Fakeout-Grundsorge der Regel:** Win-Rate im Fenster (2/3 ≈ 66,7%) liegt NICHT unter der Gesamt-Win-Rate aller 35 DB-Trades (21 Win / 33 ohne BE ≈ 63,6%) — kein Hinweis, dass Fenster-Entries öfter ausgestoppt werden als der Rest. Auch das ist bei n=3 nicht belastbar.

**Verdikt: Sample zu klein für eine belastbare Entscheidung — weder Beibehaltung noch Streichung ist durch die Daten gedeckt.** n=3 verfehlt die selbst gesetzte Schwelle (5) klar; die eigentlich fällige Review kann inhaltlich nicht abschließend beantwortet werden, nur ehrlich dokumentiert. Die vorhandene Evidenz zeigt in dieser kleinen Stichprobe eine leichte Tendenz GEGEN den Netto-Nutzen der Regel (mehr Gewinn beschnitten als Verlust vermieden), aber das kippt bei einem einzigen weiteren Fall in die andere Richtung — kein Grund für eine Änderung. **Entscheidung:** Regel bleibt unverändert in Kraft (kein Eingriff in ein laufendes System ohne robuste Evidenz, siehe [[feedback_dont_change_running_system]]). Diese Review gilt NICHT als abgeschlossen, sondern wird vertagt, bis 2 weitere echte Fenster-Entries vorliegen (dann n=5, ursprüngliche Schwelle erreicht) — erst dann erneut prüfen.

### Wichtig
- US-Börsenöffnung = 15:30 Uhr deutscher Sommerzeit (vorher fälschlich 17:00 angenommen — korrigiert am 22.06.2026)
- ~~Gleiche Logik wie SpaceX-Strategie: erste 30 Min nach Open nur beobachten~~ — ersetzt 23.07.2026, siehe Korrektur oben. Der SpaceX-Vergleich war eine unkalibrierte Analogie aus einem anderen Instrumenten-Kontext (Einzelaktie/IPO), nicht aus eigenen NAS100/QQQ-Daten abgeleitet.
