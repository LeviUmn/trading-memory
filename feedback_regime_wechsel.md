---
name: feedback-regime-wechsel
description: 5 Verlust-Trades in Folge (02.07.2026) nach Makro-Schock-Häufung — Unterscheidung zwischen Prozessfehlern und Marktregime-Wechsel wichtig für Bewertung der Strategie. Korrigiert 04.07.2026 nach unabhängigem Fable-5-Review — ursprüngliche 2-von-5-Einstufung war zu großzügig.
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 028ce287-5d0c-400c-bf08-bdc1d4c4670c
---

Nach 7 profitablen/breakeven Trades in Folge (12.06.-30.06.2026, ruhiger trendender Markt) kam eine Serie von 5 Verlust-Trades in Folge (Trade #8-#12, 01.07.-02.07.2026), die zeitlich genau mit einer Häufung großer Makro-Überraschungen zusammenfiel (ADP-Miss, ISM-Miss am 01.07.; NFP-Miss + Zinserwartungs-Repricing am 02.07.).

**KORRIGIERT 04.07.2026 — unabhängiger Fable-5-Review widerspricht der ursprünglichen Einstufung:** Die ursprüngliche Analyse (nur 2 von 5 = Prozessfehler) war zu wohlwollend. Neubewertung pro Trade:

| # | Ursprüngliche Einstufung | Korrigierte Einstufung | Begründung |
|---|---|---|---|
| #8 | Prozessfehler | Prozessfehler ✓ (unverändert) | Entry bei RSI 48,64 statt >50 — Doppelkriterium gebrochen |
| #9 | "korrekter Prozess, Regime" | **Prozessfehler** | Chop wurde live erkannt und benannt, Trade trotzdem ausgelöst — [[feedback_chartanalyse]] Punkt 7 nennt das selbst explizit "Nichtbefolgung", nicht Regime |
| #10 | Prozessfehler | Prozessfehler ✓ + Sizing-Fehler | Double-Top übersehen — UND zusätzlich 1.400€ Position bei 6x Hebel am Schock-Tag (oberes Phasen-Limit genau an einem 8d-Schock-Tag) |
| #11 | "korrekter Prozess" | Grenzwertig | Sauber gemanagt (BE-Stop), aber User-Testposition außerhalb des Standard-Workflows, direkt am selben Schock-Tag nach #10 eröffnet |
| #12 | "korrekter Prozess, Regime" | **Prozessfehler — übersehener Cooldown-Bruch** | Nach #10 (-15,60€) und #11 (-2,68€, als Loss geführt) waren am 02.07. bereits 2 Verluste in Folge am selben Tag — die Cooldown-Regel ("2 Verluste in Folge am selben Tag → Trading-Stopp für den Rest des Tages", [[project_risikomanagement]]) hätte Trade #12 verhindern müssen. Das wurde weder am 02.07. noch in der Phasenübergang-Review noch im Backtest vom 03.07. bemerkt. Siehe trades/trading_2026-07-02.md. |

**Korrigierte Bilanz: 4 von 5 Verlusten waren disziplin-/prozessbedingt, nicht 2 von 5.** Das Regime-Narrativ war deutlich übergewichtet — das ist relevant, weil eine Fehlklassifizierung als "nur Regime" dazu verleitet, neue Chart-Regeln zu ergänzen statt bestehende durchzusetzen.

**Why:** User äußerte explizit Enttäuschung darüber, dass sich die Strategie "komplett gedreht" habe. Die erste Einzelanalyse der 5 Verluste war ein guter erster Schritt, aber zu wohlwollend — sie hat einen Cooldown-Verstoß (#12) übersehen und Trade #9 trotz eigener Dokumentation eines Regelbruchs als "Regime" durchgehen lassen. Eine externe, unabhängige Zweitmeinung (Fable-5-Review, 04.07.2026) hat das aufgedeckt. Reklassifizierung von Verlusten als "Regime statt Fehler" ist ein bekannter Trader-Bias (Selbstschutz) — bei jeder künftigen Nachbesprechung aktiv dagegen prüfen.

**How to apply:** Bei einer Verlustserie nicht vorschnell den gesamten Prozess in Frage stellen oder Panik-Anpassungen vornehmen. Stattdessen jeden Verlust-Trade einzeln klassifizieren: (a) echter Prozessfehler (Regel wurde nicht befolgt, inkl. übersehener Automatismen wie Cooldown) vs. (b) korrekter Prozess, aber ungünstiges Marktregime. Nur bei (a) ist eine Prozess-Anpassung nötig — bei (b) ist die richtige Reaktion, das aktuelle Marktregime zu erkennen und ggf. eine Pause einzulegen, nicht die Strategie selbst zu verwerfen. Bei der Einstufung aktiv gegen die eigene Tendenz prüfen, Verluste zu Regime umzudeuten — im Zweifel die strengere Einstufung (Prozessfehler) wählen und explizit nach übersehenen Regelbrüchen (Cooldown, Sizing, Chop-Nichtbefolgung) suchen, bevor "Regime" akzeptiert wird.

**Bestätigte Wirkung der Korrekturen (08.07.2026, User-Einschätzung vor dem Phase-1-Review):** Die nach dieser Analyse ergänzten Regeln (NQ1!-EMA50-Gate-Rekalibrierung, Teilverkauf-Standard TP1/TP2, Dual-Gate-Bestätigung, Voll-Check-Rhythmus) zeigten ab Trade #13 (+42,96€, TP1+TP2 komplett) sichtbar Wirkung. Trade #14 (08.07.) war trotz Netto-Verlust (-28,81€) prozessual sauber — der Verlust war extern verursacht (Broker-SL-Blockade bei Scalable Capital, siehe [[feedback_broker_sl_tp_exklusiv]]), nicht durch einen der hier korrigierten Fehlertypen, und wurde durch den improvisierten Short-Hedge aktiv begrenzt (von potenziell größerem Schaden auf -1,92%). Trade #15 lief wieder sauber nach Plan (+1,15%). User bewertet den Trend seit Trade #13 als klar positiv — die Lehren aus dieser Verlustserie werden als validiert eingestuft, bevor am 09.07.2026 das große Phase-1-Review stattfindet.

**Fortschreibung 13.07.2026 (User-Beobachtung nach Trade #17):** Die Erholung hält über den Phasenübergang hinweg an. Win-Rate-Vergleich der drei Abschnitte: vor der Verlustserie (#1-7) 100% (6W/1BE, BE ausgeklammert), während der Serie (#8-12) 0% (0W/1BE/4L), danach (#13-17) 80% (4W/1L). User wertet das explizit als Bestätigung, dass die Reviews/Regel-Korrekturen einen echten, messbaren Unterschied machen — nicht nur dokumentiert, sondern im Ergebnis sichtbar. Weiterhin kleine Stichprobe (5 Trades im "danach"-Fenster), Einordnung als starkes, aber noch nicht endgültig belastbares Signal bleibt bestehen.
