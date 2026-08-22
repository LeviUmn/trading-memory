---
name: project-indikator-upgrade-diskussion-22-08-2026
description: "Offene Diskussion 22.08.2026: TradingView-Plan-Upgrade für mehr Indikator-Slots, Fables priorisierte Wunschliste, CVD/Volume-Delta-Frage über Unusual-Whales-Orderflow-Zugang neu aufgeworfen — Fortsetzung am 23.08. geplant"
metadata:
  node_type: memory
  type: project
  originSessionId: session-2026-08-22
  modified: 2026-08-22T09:41:25.614Z
---

Im Rahmen der heutigen Fable-Diskussion (siehe [[feedback_chartanalyse]] Punkte 8a2/8a3, [[feedback_chart_layout]] VWAP-Bänder live gesetzt) fragte Levi, was er bei einem TradingView-Plan-Upgrade (mehr Indikator-Slots als aktuell 5/5 auf NAS100, 3/5 auf QQQ) zusätzlich hinzufügen sollte. Levi prüft den Preis selbst und will am **23.08.2026 weitermachen** — dieser Eintrag ist der Ausgangspunkt für die Fortsetzung.

## Fables priorisierte Antwort (22.08.2026)

**Zuerst (klare Lücke, keine Redundanz):**
- **ADX/DMI (NAS100):** macht die bisher informelle Chop-Erkennung (Punkt 7, `feedback_chartanalyse.md`) zu einer objektiven Live-Zahl — direkter Bezug zum wiederholten Muster "Chop erkannt, aber ignoriert" (#9, #12).
- **Anchored VWAP von einem Schlüsselpunkt statt nur Session-Open (QQQ):** zeigt auf Volumenbasis, ob eine umkämpfte Zone (z.B. die 29.240-254-Clusterzone aus #42/#43) wirklich akzeptiert wird oder nur durchwickt wird.

**Nice-to-have, zweite Welle:** Money Flow Index (QQQ, echtes Volumen statt reinem Preis wie RSI, aber bisher unbewiesen/n=0), Keltner-Channels (NAS100, schärft die bestehende BB-Squeeze-Vorwarnung, kein eigenständiges neues Signal).

**Nicht empfohlen:** Volume Profile/VPOC (planabhängig + hohe Interpretationslast), Supertrend (redundant mit der bestehenden ATR-SL-Logik aus Punkt 8c), Stochastic/Williams %R (redundant mit RSI, könnte Chop-Fehlsignale sogar verschlimmern), Ichimoku (hohe Interpretationslast ohne belegte Edge, gleiches Muster wie bereits abgelehnte ICT/SMC-Konzepte).

**CVD/Volume Delta:** Zum Zeitpunkt dieser ersten Antwort als "kein Slot-Problem, ein Datenproblem" eingestuft — echter Orderflow (Bid/Ask-differenzierte Tick-Daten) ist über die vorhandenen Feeds strukturell nicht verfügbar (NAS100-CFD ohne Volumen, QQQ-Cboe-One nur Teilvolumen, kein DOM/Broker-Anbindung). Ein Plan-Upgrade mit mehr Slots löst das laut Fable NICHT.

**Ehrlicher Vorbehalt, den Fable mitgegeben hat:** ~32% aller Tage sind laut der 121-Tage-Regime-Statistik vom 20.08. Chop, unabhängig von der Indikatorenzahl. Die 31.07.-Review kam schon einmal zum selben Schluss: Profitabilität hängt an Regeldisziplin, nicht an mehr Werkzeugen. Jeder neue Indikator ist auch eine neue Zahl unter Zeitdruck (siehe [[feedback_live_trading]] Punkt 14, Trade #42 "Hinweis zu spät gelesen"). Falls etwas hinzukommt: an den 5-Minuten-Voll-Check koppeln, nie an den 1-Minuten-Quick-Tick.

Preise selbst: Fable kennt die aktuellen TradingView-Tarife nicht zuverlässig, nur grobe Tier-Struktur — Levi prüft das selbst auf tradingview.com.

## Offener Faden: CVD/Volume Delta über Unusual Whales neu aufgeworfen

Levi hat direkt im Anschluss vorgeschlagen, die CVD-Datenlücke nicht über TradingView selbst, sondern über einen **Unusual-Whales-Orderflow-Datenzugang** zu schließen — das würde die Datenbeschränkung, die Fable als Blocker genannt hat, potenziell auflösen. Das greift die bereits bestehende, bewusst zurückgestellte **konditionale UnusualWhales-Integration** aus [[project_vision]] wieder auf (dort: "Phase 4 (Trade 36-40): UnusualWhales konditional" — Bedingung war ursprünglich "erst Phase 3 komplett + Gesamt-Review, dann ggf. eigene Testphase").

**Wichtig für die Fortsetzung:** Der Gesamt-Review über Phasen 1-3 hat inzwischen bereits stattgefunden ([[project_fable_vollpruefung_phase1-3_2026-08-12]], 12.08.2026) — die ursprüngliche Vorbedingung für "UnusualWhales weiter prüfen" könnte damit bereits erfüllt sein. Das muss beim nächsten Fable-Gespräch explizit gegen den aktuellen Phasenstand (siehe [[project_phase4_gates_2026-08-12]], Stand nach Trade #43) geprüft werden, nicht neu verhandelt werden, als hätte der Review noch nicht stattgefunden.

**Fables Antwort (nachgetragen, 22.08.2026):**

1. **Vorbedingung formal erfüllt, aber Beschluss steht dagegen:** Der 12.08.-Gesamt-Review hat stattgefunden, aber `project_phase4_gates_2026-08-12.md` hat UW-Integration bewusst vom 10k€-Test getrennt und zu einem eigenen Schritt erst NACH bestandenem #50-Review gemacht ("Getrennte Zwecke nicht vermischen"). Stand #43 = erst 3 von 15 Trades im #41-50-Fenster, die sechs Gates (RR≥1,3, EV≥0,5%, WR≥55% etc.) noch nicht auswertbar. UW jetzt anzugehen liefe dem bereits getroffenen Beschluss zuwider.
2. **Wichtigerer Punkt — Kategorie-Verwechslung:** Unusual Whales liefert KEIN CVD/Bid-Ask-Delta. UW ist ein Options-Flow/Dark-Pool-Tool (GEX, Max Pain, Options-Sweeps, Dark-Pool-Prints) — aus Dealer-Hedging abgeleitete statische Preiszonen, kein Tick-für-Tick-Kauf/Verkaufsdruck im Spotmarkt. CVD und GEX/Max Pain sind zwei verschiedene Orderflow-Konzepte. UW schließt die ursprüngliche CVD-Datenlücke NICHT — die bleibt strukturell offen (kein DOM/Tick-Feed für NAS100/QQQ).
3. **Priorität bleibt unverändert:** ADX/DMI + Anchored VWAP zuerst. GEX/Max Pain bliebe als separater, eigenständiger Layer sinnvoll (nicht als CVD-Ersatz) — aber laut bestehendem Plan erst nach #50.
4. Kosten: alte Notiz nennt ~50$/Monat, laut Fable evtl. veraltet, selbst prüfen.

**Konsequenz für morgen:** UnusualWhales bleibt zurückgestellt bis #50-Gates, das ist kein neuer Beschluss, sondern Bestätigung des bestehenden. Die CVD-Frage bleibt offen/ungelöst (Datenproblem, kein Slot-Problem) — Fokus für den Plan-Upgrade sollte auf ADX/Anchored VWAP liegen, nicht auf Orderflow-Ersatz.

**Why:** Levi will das Plan-Upgrade nicht isoliert von der größeren, schon länger im Raum stehenden Order-Flow-Frage entscheiden — wenn Unusual Whales ohnehin eine reale Option ist, ändert das, wie wichtig CVD als TradingView-interner Indikator überhaupt noch wäre.

**How to apply:** Bei Fortsetzung am 23.08. (oder später) zuerst diesen Eintrag lesen, dann: (1) Levis Preis-Check zum Plan-Upgrade abfragen, (2) Fables nachgereichte CVD/UnusualWhales-Neubewertung einbeziehen, (3) den aktuellen Phasenstand gegen die Vorbedingung aus [[project_vision]] abgleichen, bevor über UnusualWhales neu entschieden wird.

## Pflicht-Folgeschritt, sobald alle Indikatoren final geprüft/umgesetzt sind (ergänzt 22.08.2026, Levi-Auftrag)

Levi wörtlich: "Wenn wir morgen final alle Indikatoren geprüft und umgesetzt haben, dann mit den Regeln noch einmal durchgehen, ob die Regeln auch die Indikatoren verarbeiten, der Ablauf passt, damit die Indikatoren auch richtige Daten liefern für die Setup-Suche UND viel wichtiger, wenn ein Setup besteht, dass das Setup aktiv geschützt wird und kurzfristig bei Marktänderung auch manueller Teilexit oder Vollexit vorgeschlagen wird."

**Why:** Ein neuer Indikator ist wertlos, wenn das bestehende Regelwerk ihn nicht aktiv verarbeitet (nur als zusätzliche Zahl ohne Verankerung in Punkt 7b/8/9-Trigger-Logik) — und Levis eigentliche Priorität ist explizit NICHT nur bessere Setup-Erkennung, sondern vor allem aktiver Schutz einer bereits laufenden Position bei kurzfristiger Marktänderung (Teil-/Vollexit-Vorschlag), also eine Stärkung von [[feedback_live_trading]] Punkt 11/12, nicht nur von der Entry-Seite.

**How to apply:** Nach Abschluss der Indikator-Entscheidung (welche neu dazukommen, ggf. inkl. CVD/Unusual Whales) einen eigenen Regelwerk-Durchgang machen — nicht nur neue Indikatoren in die bestehenden Punkte einfügen, sondern explizit prüfen: (1) Liefern sie im Live-Betrieb die erwarteten Daten für die Setup-Suche zuverlässig (Datenqualität/Verfügbarkeit, analog zur bisherigen Prüfpraxis bei neuen Tools), (2) sind sie in den Positionsschutz-Mechanismus (Punkt 11 Reversal-Management, Punkt 12 Stall-Exit) eingebaut, sodass sie aktiv einen Teil-/Vollexit-Vorschlag auslösen können, nicht nur passiv am Entry mitlaufen.
