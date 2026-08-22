---
name: feedback-chartanalyse
description: Vollständige Chartanalyse bei jeder Trade-Entscheidung — alle technischen Tools anwenden
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 607aa8f6-9958-4c1c-9c75-4afabcffb717
  modified: 2026-08-22T09:22:33.082Z
---

Bei JEDER Trade-Entscheidung immer eine vollständige Chartanalyse durchführen — nie nur auf einen Faktor schauen.

**Why:** Technische Analyse erhöht die Trefferquote erheblich. Heute (12.06.2026) haben wir den Support bei 29.400 korrekt identifiziert — mit mehr Tools wäre der Entry noch präziser gewesen.

**How to apply:** Immer ALLE relevanten Tools checken bevor Entry/Exit empfohlen wird.

**Korrigiert 04.07.2026 (Fable-5-Review) — der bisherige Satz unten war ein Widerspruch zu [[feedback_live_trading]] und ist ersetzt:** Die volle Checkliste (Chartmuster 9d, Candlestick 4, Fibonacci 3, echter MTF-Wechsel 8) kann in der heißen Phase NICHT bei jedem 1-Min-Tick automatisch vollständig laufen — [[feedback_live_trading]] Punkt 3a/3b/7b verbieten dort genau die Tools (Screenshot, Timeframe-Wechsel, Pane-Wechsel), die für Muster-/Candlestick-/MTF-Prüfung nötig sind. Beides gleichzeitig zu behaupten war Scheincompliance. Die gültige Regel ist jetzt **[[feedback_live_trading]] Punkt 9 (Voll-Check-Rhythmus)**: die volle Tiefe bleibt Pflicht, läuft aber nicht im 1-Min-Takt, sondern an definierten Voll-Check-Zeitpunkten (Kerzenschluss, Übergang ruhig→heiß, unmittelbar vor Entry, bei extremer Zahlen-Bewegung) und gilt bis zum nächsten dieser Zeitpunkte als aktuell. Der 1-Min-Loop dazwischen prüft nur die Zahlen-Sequenz gegen diesen zuletzt eingefrorenen Kontext.

~~Alter Text (ersetzt):~~ Bei JEDEM Check automatisch die VOLLSTÄNDIGE Checkliste anwenden, auch im 1-Min-Loop — das ist technisch nicht durchführbar, ohne Screenshot/Timeframe-Wechsel/Pane-Wechsel in der heißen Phase zu verletzen, siehe Korrektur oben.

---

## Checkliste Chartanalyse

### 1. Trend (Übergeordnet)
- **Zeitrahmen:** Immer erst 1H/4H anschauen, dann auf 5min für Entry zoomen
- **Higher Highs / Higher Lows** → Uptrend (Long bias)
- **Lower Highs / Lower Lows** → Downtrend (Short bias)
- **Seitwärts** → Kein Trade oder Range-Trading

### 2. Support & Resistance
- Horizontale Levels aus Vortagshochs/-tiefs (PDH/PDL)
- Wochenhoch/-tief
- Runde Zahlen (29.000, 29.500, 30.000 etc.)
- Mehrfach getestete Zonen (mind. 2x berührt = starkes Level)

### 3. Fibonacci Retracements & Extensions (erweitert 02.07.2026)
**Retracements** (Einstiegszonen nach einem starken Move, Fib von Tief zu Hoch ziehen):
- **0.236** — schwache/flache Korrektur, nur bei sehr starkem Trend relevant
- **0.382** — moderate Korrektur
- **0.5** — psychologisch wichtig (kein echtes Fib-Level, aber stark beachtet)
- **0.618** (goldener Schnitt) — stärkste Unterstützung bei Rücksetzer, wichtigstes Level
- **0.786** — tiefe Korrektur, letzte Verteidigungslinie bevor Trend als gebrochen gilt

**Extensions** (Ziel-/TP-Zonen bei Trendfortsetzung, siehe auch Punkt 8b):
- **1.272** — erstes Extension-Ziel, oft TP1
- **1.618** (goldener Schnitt) — Hauptziel, am häufigsten erreicht
- **2.0 / 2.618** — bei sehr starken/parabolischen Moves, seltener erreicht

Entry wenn Kurs Fib-Level + Bounce-/Bestätigungskerze zeigt (siehe Punkt 4).

### 4. Candlestick-Muster (erweitert 02.07.2026 — praxisrelevante Vollständigkeit)

**Einzelkerzen-Muster:**
- **Hammer / Pin Bar** am Support → Long-Signal (langer unterer Docht, kleiner Body oben)
- **Inverted Hammer** → bullische Umkehr, braucht Bestätigung durch Folgekerze
- **Hanging Man** an Resistance → bärische Umkehr (gleiche Form wie Hammer, andere Kontextzone)
- **Shooting Star** an Resistance → Warnung / Exit (langer oberer Docht)
- **Doji** an Key-Level → Unentschlossenheit, warten. Varianten: **Dragonfly Doji** (bullisch, langer unterer Docht), **Gravestone Doji** (bärisch, langer oberer Docht)
- **Marubozu** (kein/kaum Docht, voller Body) → starkes Momentum in Kerzenrichtung
- **Spinning Top** → Unentschlossenheit (kleiner Body, Dochte beidseitig)

**Zwei-Kerzen-Muster:**
- **Bullish/Bearish Engulfing** (große Kerze verschluckt vorherige) → starkes Signal
- **Bullish/Bearish Harami** (kleine Kerze innerhalb der vorherigen, "Inside Bar") → möglicher Trendwechsel, schwächer als Engulfing
- **Piercing Line** → bullische Umkehr (grüne Kerze schließt über Mitte der vorherigen roten)
- **Dark Cloud Cover** → bärische Umkehr (rote Kerze schließt unter Mitte der vorherigen grünen)
- **Tweezer Top / Tweezer Bottom** → zwei Kerzen mit fast identischem Hoch/Tief, Umkehr-Signal

**Drei-Kerzen-Muster:**
- **Morning Star** → starke bullische Umkehr (fallend, Doji/kleine Kerze, dann starke grüne Kerze)
- **Evening Star** → starke bärische Umkehr (steigend, Doji/kleine Kerze, dann starke rote Kerze)
- **Three White Soldiers** → starkes bullisches Momentum (3 aufeinanderfolgende grüne Kerzen mit höheren Closes)
- **Three Black Crows** → starkes bärisches Momentum (3 aufeinanderfolgende rote Kerzen mit tieferen Closes)
- **3 grüne Kerzen in Folge** → Momentum, aber Vorsicht vor Überkauf (schwächere Variante von Three White Soldiers)

**Why (Vollständigkeits-Hinweis):** Es gibt in der klassischen Candlestick-Lehre über 40 benannte Muster — die obige Liste deckt die praxisrelevanten ab, seltene/exotische Restmuster bringen keinen zusätzlichen Trading-Mehrwert und werden bewusst ausgelassen.

**4a. Gilt auch WÄHREND einer offenen Position, in Kombination mit den Chartmustern aus 9d1 (ergänzt 03.07.2026).** Genau wie bei den Chartmustern (siehe [[feedback_chartanalyse]] Punkt 9d1) ist die Candlestick-Prüfung keine reine Entry-Prüfung, sondern läuft weiter, solange eine Position offen ist.

**Kombinationslogik für den Exit-Entscheid (das eigentliche "wann verkaufen"):**
- **Einzelne Umkehrkerze gegen die Position OHNE erkennbares Chartmuster** (z.B. eine einzelne Shooting Star ohne Kontext) → Beobachten, noch kein automatischer Exit, aber explizit als Warnsignal benennen
- **Umkehrkerze gegen die Position UND passendes Chartmuster aus 9d1 gleichzeitig** (z.B. Shooting Star / Bearish Engulfing / Evening Star an einer bereits früher am Tag getesteten Resistance-Zone bei Long, oder Hammer / Bullish Engulfing / Morning Star an bereits getesteter Support-Zone bei Short) → das ist das konkrete, aktionable Exit-Signal — hier aktiv Teil-/Vollgewinnmitnahme oder SL-Nachzug auf Breakeven vorschlagen, nicht erst auf TP/SL warten
- **Fortsetzungs-Kerzenmuster in Positionsrichtung** (z.B. Three White Soldiers bei Long) zusätzlich zu einem Fortsetzungs-Chartmuster (Flag/Pennant) → verstärkt das Halten-Signal aus 9d1, kein Exit-Grund

**Why:** Bei Trade #10 (02.07.2026) fehlte nicht nur die Chartmuster-Prüfung während der offenen Position (siehe 9d1), sondern auch die explizite Verbindung zur Ablehnungskerze bei 30.042 — beides zusammen (Kerze + Zonen-Retest) ist ein stärkeres Signal als jedes für sich allein. User hat am 03.07.2026 bestätigt, dass Candlestick-Signale live und in Verbindung mit den Chartmustern die Grundlage für die Exit-Entscheidung sein sollen.

**How to apply:** Bei jedem Kurs-Update einer offenen Position sowohl auf Kerzenmuster (Punkt 4) als auch auf Chartmuster (Punkt 9d1) achten und explizit sagen, ob beide zusammen ein Signal ergeben oder nur eines von beiden — nur die Kombination (Kerze + Muster/Zone) ist ein hartes Exit-Signal, eine einzelne Kerze allein reicht nicht.

### 5. Volumen
- Hohe Volumen bei Ausbruch = echter Breakout
- Niedriges Volumen bei Ausbruch = Fakeout-Risiko
- Volumen-Spike ohne Preisbewegung = Akkumulation (Vorbereitung auf Move)

**Datenquelle (seit 15.07.2026):** Alle Volumen-Kriterien dieser Checkliste werden auf der QQQ-Pane (Pane 1, `BATS:QQQ`) abgelesen, nicht auf NAS100 (CFD ohne Volumen). Bewertung immer relativ (Spike vs. eigener Durchschnitt auf demselben Chart) — der Cboe-One-Feed zeigt nur einen Teil des Gesamtvolumens, absolute Zahlen sind nicht marktrepräsentativ. Verfügbarkeit nur zur US-Session, siehe [[feedback_live_trading]] Punkt 7e.

### 6. Indikatoren (auf TradingView) — zwei Instrumente statt einem (präzisiert 16.07.2026)

**Seit 03.07./15.07.2026 strukturell auf zwei Panes verteilt** (siehe [[feedback_chart_layout]]): NAS100 (Pane 0, CFD) hat kein echtes Volumen und kein VWAP mehr, QQQ (Pane 1, seit 15.07.2026, vorher NQ1!) trägt beides. Die alte Vorstellung "5 Indikatoren auf einem Chart" ist überholt — die Kombinations-Regel unten prüft jetzt bewusst beide Instrumente, statt VWAP ersatzlos zu streichen.

**NAS100 (Pane 0) — 3 Indikatoren:**
- **RSI (14):** Überkauft >70, Überverkauft <30 → Divergenz ist stärkstes Signal
- **MACD:** Histogram-Divergenz als frühestes Trendwechsel-Signal, Crossover als Bestätigung
- **EMA 50:** Trendfilter — Kurs über EMA50 = Long-Bias, darunter = Short-Bias. EMA50 selbst als dynamischer S/R.

**QQQ (Pane 1) — 3 Indikatoren, siehe [[feedback_live_trading]] Punkt 7b:**
- **Volume** — Volumenbestätigung bei Ausbrüchen (Spike = echtes Signal, niedrig = Fakeout), immer relativ zum eigenen Durchschnitt bewertet, nie absolute Zahlen
- **VWAP** (Volumengewichteter Durchschnittspreis, resettet täglich): Institutioneller Tagesanker — Kurs über VWAP = bullisch, darunter = bearisch. Nur hier verfügbar, NAS100 hat kein VWAP.
- **EMA 50:** identische Rolle wie auf NAS100, Basis für das EMA50-vs-EMA50-Pflicht-Gate

**Kombinations-Regel (stärkstes Signal, jetzt zwei Instrumente — löst die alte Ein-Chart-Regel ab):**
- NAS100 unter eigener EMA50 UND QQQ unter eigener EMA50 UND QQQ unter VWAP → starker Short-Bias, nur Short-Setups suchen
- NAS100 über eigener EMA50 UND QQQ über eigener EMA50 UND QQQ über VWAP → starker Long-Bias, nur Long-Setups suchen
- Instrumente/Indikatoren widersprechen sich → neutral, kein Trade

**Why:** Die alte Regel prüfte EMA50 und VWAP auf demselben (NAS100-)Chart — das geht seit dem VWAP-Wegfall auf NAS100 nicht mehr. Statt VWAP ersatzlos zu streichen, übernimmt QQQ diese Rolle, konsistent mit dem ohnehin bestehenden Dual-Gate-Prinzip aus [[feedback_live_trading]] Punkt 7b (EMA50-vs-EMA50 = Pflicht, VWAP+Volumen = unterstützend, kein harter Blocker).

### 7. Marktstruktur
- **Breakout:** Kurs bricht über Resistance mit Volumen → Entry
- **Retest:** Nach Breakout kommt Kurs zurück auf altes Resistance (jetzt Support) → Entry
- **Fakeout:** Kurzer Spike über Level, dann sofort zurück → kein Entry

**Chop-Erkennung (ergänzt 01.07.2026):** RSI und/oder MACD-Histogram kippen innerhalb weniger (2-4) 5-Min-Kerzen mehrfach zwischen bullisch/bärisch hin und her, ohne dass sich eine Seite durchsetzt, UND VIX bleibt dabei auffällig flach (keine Angst, aber auch keine Überzeugung). Das ist echtes Chop, kein Trend.

**Why:** Am 01.07.2026 wurde diese Chop-Phase korrekt live erkannt und benannt ("sehr whippy, weder Short noch Long sauber bestätigt"), aber kurz danach trotzdem ein Trade ausgelöst, weil ein einzelnes technisches Kriterium zufällig erfüllt wurde. Der Trade endete im Verlust durch genau das erwartete Hin-und-Her.

**How to apply:** Sobald Chop nach obiger Definition erkannt wird, siehe [[project_risikomanagement]] Regel "Chop-Erkennung" — Position halbieren oder Trade auslassen. Diese Einschätzung gilt so lange fort, bis sich RSI/MACD über mehrere (3+) Kerzen konsistent in eine Richtung bewegen, nicht nur für den einen Moment, in dem ein Kriterium technisch erfüllt ist.

**Stacking-Hinweis (ergänzt 23.07.2026):** Trifft gleichzeitig ein weiterer Halbierungs-Grund zu (Chasing, siehe [[feedback_live_trading]] Punkt 13, oder das 15:30-16:00-Zeit-Gate, siehe [[feedback_trading_zeitfenster]]), wird trotzdem nur einmal halbiert — kein Multiplizieren. Zentrale Regel dazu in [[project_risikomanagement]], Abschnitt "Stacking-Regel für gleichzeitige Halbierungs-Gründe".

**Präzisierung "3+ Kerzen" (ergänzt 16.07.2026, teilweise korrigiert nach unabhängiger Fable-Prüfung):** Gemeint sind 3+ tatsächlich **abgeschlossene Kerzenschlüsse** in dieselbe Richtung — NICHT mehrere Zahlen-Ticks innerhalb einer einzigen, noch offenen Kerze. **Wichtig — Korrektur:** Bei Trade #21 wurde ursprünglich (fälschlich) behauptet, es habe bei Entry nur 1 abgeschlossene gerichtete Kerze gegeben — eine Kerzen-Fehlzuordnung (das Intrabar-Tief 29.143 gehörte zur Entry-Kerze selbst, nicht zur vorherigen). Fable hat gegengecheckt: tatsächlich lagen bei Entry bereits **3 konsistent tiefere Kerzenschlüsse in Folge** vor (15:00 → 29.216,95, 15:05 → 29.198,95, 15:10/Entry → 29.165,85) — Chop war nach dieser Regel zum Entry-Zeitpunkt bereits regulär beendet. Die allgemeine Lehre bleibt trotzdem gültig (Ticks innerhalb einer offenen Kerze sind kein Ersatz für abgeschlossene Kerzenschlüsse, siehe Why unten), nur die konkrete Anwendung auf Trade #21 war falsch — der eigentliche Fehler bei Trade #21 lag woanders (siehe [[trades/trading_2026-07-16]] und [[trades/trade_log]] Trade #21: SL verletzte die BASIS-8c-Regel, unabhängig von Chop). **How to apply:** Bei der Frage "ist Chop vorbei" ausschließlich auf `data_get_ohlcv`-Kerzen mit abgeschlossenem `time`-Stempel zählen (die aktuell offene, noch laufende Kerze zählt nicht mit) — UND bei der Kerzen-Zuordnung Zeitstempel exakt gegenprüfen, nicht aus dem Gedächtnis rekonstruieren, wie es bei Trade #21 fälschlich passierte.

**Bindend, nicht nur Beobachtung (bestätigt im Backtest 03.07.2026, siehe [[feedback_backtest_ergebnis_2026-07-03]]):** Bei Trade #9 (01.07.2026) wurde Chop live korrekt erkannt und benannt, der Trade wurde trotzdem ausgelöst, weil ein einzelnes Kriterium zufällig erfüllt war — Verlust -12,96€. Die Regel-Definition war schon richtig, das Problem war Nichtbefolgung. Sobald Chop erkannt ist, ist Halbieren/Auslassen eine Pflicht-Aktion, kein optionaler Hinweis, der von einem einzelnen erfüllten Doppelkriterium überstimmt werden darf.

**7a. SL-Platzierung an Chop-Tagen: unter das tatsächlich getestete Level, nicht unter das nächste kleine Swing-Low (ergänzt 13.07.2026, Trade #17 belegt; korrigiert nach Fable-Review 13.07.2026 — integriert in Punkt 8c statt eigenständige Parallel-Regel)**

**Einordnung nach Fable-Review:** Diese Regel ist kein neues Prinzip, sondern eine konkrete Methode für das bereits bestehende Punkt 8c (Schock-Tag-Multiplikator: SL-Abstand an Schock-/Chop-Tagen auf das 2,5-3-fache der Rausch-Kerze erhöhen). 7a beantwortet nur die Frage "wohin genau", wenn 8c bereits "weiter" fordert: nicht zum nächsten kleinen Swing-Low, sondern zum letzten mehrfach bestätigten strukturellen Level (z.B. ein Tagestief, das schon 2-3x angelaufen und verteidigt wurde).

**Zwingende Ergänzung (Fable-Review, nachdem Trade #17 selbst dagegen verstieß):** Ein weiterer SL nach dieser Methode ist NUR zulässig, wenn TP1 die RR-Minima aus Punkt 8b weiterhin erfüllt (TP1 ≥1:1 zum neuen, weiteren SL-Abstand). Bei Trade #17 wurde der SL auf das Tagestief (29.291) gelegt, ohne die TP-Level entsprechend anzupassen — das ergab nur RR 0,32:1 (TP1) bzw. 0,56:1 (TP2), ein klarer Verstoß gegen die damals gültige 8b-Fassung, der zunächst unerkannt blieb. **Wenn ein weiterer Chop-SL die TP1-RR-Minima reißt: entweder ein ferneres TP1-Level suchen, auf einen engeren Entry warten, oder den Trade auslassen — nicht den SL verbreitern ohne TP1 mitzuziehen.**

**Update 27.07.2026:** Die TP2≥1:2-Pflicht aus der historischen Trade-#17-Analyse oben ist überholt — seit dem Fable-Review vom 27.07.2026 gilt für TP2 keine RR-Mindestpflicht mehr, siehe aktualisiertes Punkt 8b. Die Kernlehre aus Trade #17 (SL und TP nicht isoliert voneinander verschieben, feste Reihenfolge Struktur→SL→RR-Check) bleibt unverändert gültig, nur bezogen auf TP1 statt TP1+TP2.

**Why:** Bei Trade #17 (13.07.2026) fiel der Kurs direkt nach dem Entry in einer typischen Chop-Sequenz (4 Kerzen hin und her) bis auf ein Tief, das ein enger SL am nächsten kleinen Swing-Low mit hoher Wahrscheinlichkeit ausgelöst hätte — der tatsächlich gesetzte SL lag deutlich weiter unten, am mehrfach getesteten Tagestief, und überlebte die Chop-Phase. Der anschließende echte Move kam erst danach. Ein enger SL hätte den Trade in reinem Rauschen beendet, bevor die eigentliche Bewegung überhaupt begann — aber der weitere SL selbst erzeugte den RR-Bruch oben, weil die TP-Seite nicht mitgedacht wurde.

**Zusätzliches Muster-Wissen:** Ein frisch bestätigter Ausbruch/Reclaim (alle Kriterien erfüllt, Dual-Gate bestätigt) nach einem scharfen V-Reversal wird an einem erkannten Chop-Tag häufig noch einmal zurückgetestet, bevor die eigentliche Bewegung einsetzt — mehrere Kerzen Hin-und-Her direkt nach dem Entry sind an solchen Tagen eher normal als ein Warnsignal, solange dabei nicht mehrere Reversal-Kriterien gleichzeitig kippen (siehe [[feedback_live_trading]] Punkt 11).

**How to apply:** Sobald die Chop-Definition (RSI/MACD kippen mehrfach, VIX bleibt flach) für den Tag erfüllt ist, bei der SL-Festlegung aktiv das letzte mehrfach getestete strukturelle Level statt des nächsten kleinen Swing-Lows wählen — UND direkt im selben Schritt gegen Punkt 8b gegenrechnen, ob TP1/TP2 damit noch die RR-Minima erfüllen. Erfüllen sie das nicht mehr, Trade auslassen oder TP-Level anpassen, nicht den SL isoliert verbreitern. Direkt nach dem Entry auftretende Gegenbewegungen an einem erkannten Chop-Tag nicht vorschnell als Reversal werten, sondern gegen die vollständigen 4 Kriterien aus [[feedback_live_trading]] Punkt 11 prüfen, bevor reagiert wird.

**7a1. SL-Nachzug: Puffer hinter berechneten Pivot-/Round-Number-Levels (ergänzt 16.07.2026, Fable-Empfehlung nach Trade #19 — Soft-Heuristik, kein hartes Muss; Quelle geändert 23.07.2026)**

Bei Trade #19 (15.07.2026) wurde der nachgezogene SL (29.300) durch einen Spike auf 29.335,95 ausgelöst — der Kurs wurde dabei fast exakt an einer sichtbar eingezeichneten Pivot-Widerstandslinie (Pivots Traditional Auto) abgelehnt, bevor er wieder fiel. Sichtbare Pivot-Level und runde Hunderter-Marken wirken als Liquiditätsmagneten (bekannte Marktlogik, nicht nur aus diesem einen Trade abgeleitet) — ein SL exakt darauf ist strukturell stop-hunt-gefährdet.

**Quellenwechsel (23.07.2026, Fable-Review):** Pivot Points Standard wurde als Chart-Indikator entfernt (Essential-Plan-Limit, ersetzt durch ATR, siehe [[feedback_chart_layout]]). Die Level kommen jetzt nicht mehr aus einer sichtbaren Chart-Studie, sondern aus der einmaligen Tages-Berechnung — siehe Punkt 7a1a. Die Regel-Logik selbst (≤10 Punkte Nähe → 10-15 Punkte Puffer) bleibt unverändert.

**Regel (bewusst als Heuristik, nicht als Blocker):** Liegt das geplante SL-Nachzugs-Level ≤10 Punkte an einem berechneten Pivot-Level oder einer runden Hunderter-Marke, den SL stattdessen 10-15 Punkte dahinter setzen — **außer** das zusätzliche Risiko würde die 1,5%-Kapitalgrenze (siehe [[project_risikomanagement]]) verletzen, dann bleibt das ursprüngliche Level bestehen.

**Einordnung:** Ergänzt die bestehende SL-Trailing-Praxis (7a/8c), ersetzt sie nicht — gilt zusätzlich zur RR-Gegenrechnung aus 8b, genau wie 7a selbst.

**Review-Pflicht (Bedingung für den Fortbestand dieser Regel) — Zählung am 23.07.2026 neu gestartet:** Das alte n=1 (16.07., indikatorbasiert/visuell) zählt nicht mehr, da sich die Levels-Quelle geändert hat. Nach den nächsten 5 Anwendungsfällen MIT der neuen Berechnungsmethode explizit prüfen, ob der Puffer tatsächlich einen Unterschied gemacht hat (SL hätte ohne Puffer aber mit Puffer nicht ausgelöst, oder umgekehrt). Zeigt sich kein klarer Nutzen, diese Regel wieder streichen statt sie als ungenutzte Karteileiche stehen zu lassen.

**Why:** Levi hat Fable am 16.07.2026 direkt nach dessen Einschätzung gefragt (nicht selbst vorentschieden) — Fables Empfehlung war explizit "weder bindende Regel noch bloße Watch-Note", sondern eine konkrete, im 1-Min-Loop anwendbare Heuristik mit eingebautem Verfallstest, um Regel-Wildwuchs (ein wiederkehrend benanntes Problem in diesem System, siehe [[project_fable_tiefendiagnose_2026-07-15]]) zu vermeiden.

**7a1a. Pivot-Berechnung ohne Chart-Indikator (ergänzt 23.07.2026, Fable-Review)**

Einmal pro Handelstag, beim Session-Start (nicht im 1-Min-Loop, kein Tempo-Konflikt mit Punkt 9):
1. `data_get_ohlcv` auf Tages-Timeframe → High/Low/Close des letzten abgeschlossenen Handelstags
2. Standard-Pivot-Formel: PP = (H+L+C)/3, R1 = 2×PP−L, S1 = 2×PP−H, R2 = PP+(H−L), S2 = PP−(H−L)
3. Die 1-2 kursnächsten Level zusätzlich per `draw_shape` (horizontal_line) auf den Chart zeichnen — macht die Zone in Voll-Check-Screenshots wieder sichtbar für 7a1/8e, belegt aber KEINEN der 5 Indikator-Slots (Zeichnungen fallen nicht unter das Plan-Limit)
4. Gültig bis zum nächsten Session-Start, dann neu berechnen

**Nachziehen bei Annäherung (ergänzt 23.07.2026, User-Anforderung):** Nur die 1-2 kursnächsten Level werden initial gezeichnet, R1/R2/S2 bleiben zunächst nur berechnet, nicht gezeichnet (siehe Punkt 3). Sobald sich der Kurs im laufenden Voll-Check-Rhythmus (Punkt 9) einem noch nicht gezeichneten Level (R1, R2 oder S2) auf eine Distanz nähert, die der aktuellen Rausch-Kerzen-Größe (≈1-2× ATR(14), siehe 8c) entspricht, dieses Level SELBSTSTÄNDIG per `draw_shape` nachziehen — ohne dass der User danach fragen muss. Grund: Der User kennt die berechneten Zahlenwerte nicht auswendig und kann selbst nicht erkennen, wann eine Annäherung bevorsteht — das ist explizit Aufgabe des Loops, nicht des Users (gleiches Prinzip wie Punkt 7a in [[feedback_live_trading]]: proaktiv melden statt auf Nachfrage warten). Nicht mehr benötigte Level (Kurs hat sie klar hinter sich gelassen, keine SL-Relevanz mehr) können unauffällig stehen bleiben oder bei Gelegenheit per `draw_remove_one` entfernt werden — kein Pflichtschritt, nur Aufräumen bei Bedarf.

**7a1b. Spike über Pivot-R-Level ohne bestätigten Schlusskurs — Beobachtungsnotiz (ergänzt 17.08.2026, Fable-Review nach Trade #36, n=1)**

**Beobachtung (ausdrücklich noch KEIN eigenständiges Exit- oder Entry-Verbotssignal, kein Blocker):** Spikt der Kurs intrabar über ein berechnetes Pivot-R-Level (siehe 7a1a) hinaus, OHNE dass ein 5-Min-Kerzenschluss über diesem Level bestätigt, und folgt danach ein Seitwärtslauf von mehr als 15 Minuten direkt an/unter der Linie ohne neues Bewegungs-Hoch, wird das als **reduzierte Fortsetzungs-Konfidenz** markiert — nicht mehr automatisch als reine Bestätigung der Trendfortsetzung gewertet.

**Why:** Bei Trade #36 (17.08.2026) durchbrach der Kurs R1 (30.183,15) kurzzeitig mit einem Hoch von 30.192,55, ohne dass sich ein Schlusskurs darüber bestätigte. Es folgte eine 20-25-minütige enge Konsolidierung direkt an/unter R1 mit 6-7 gescheiterten Rückeroberungsversuchen, bevor der Kurs sichtbar abdrehte und kontinuierlich fiel — ein klassisches False-Break-/Absorptionsmuster an einem Pivot-Widerstand, das die bestehende Logik (7a1a behandelt Pivot-Level primär als statische Ziel-/Widerstandsmarken, 9d1 prüft nicht spezifisch auf diese Konstellation) bisher nicht von einem echten Ausbruch unterschied. n=1 — dies ist eine Beobachtungsnotiz, keine belastbare Regel.

**How to apply:** Tritt dieses Muster (Spike über Pivot-R-Level ohne bestätigten 5-Min-Schlusskurs + anschließend >15 Min Seitwärtslauf ohne neues Hoch direkt an der Linie) auf: kein Nachladen der Position auf Basis dieses Spikes, erhöhte Wachsamkeit für den Stall-Exit-Trigger (Punkt 12.3 in [[feedback_live_trading]]), aber weiterhin normale Bestätigungs-/Exit-Logik ansonsten unverändert anwenden — die Beobachtung schärft die Interpretation, ersetzt aber kein bestehendes Kriterium.

**Review-Pflicht (Bedingung für den Fortbestand dieser Notiz):** Nach den nächsten 2-3 vergleichbaren Fällen (Spike über ein Pivot-R/S-Level ohne bestätigten Schlusskurs, gefolgt von längerem Seitwärtslauf direkt an der Linie) explizit prüfen, ob sich das Muster wiederholt bestätigt. Bestätigt es sich, kann daraus eine feste Regel werden (z.B. mit konkreten Handlungskonsequenzen für Entry/Exit); bestätigt es sich nicht, diese Notiz wieder streichen statt sie als ungenutzte Karteileiche stehen zu lassen (gleiches Prinzip wie bei 7a1, siehe Review-Pflicht dort).

### 8. Multi-Timeframe — feste Reihenfolge (eingeführt 26.06.2026, auf zwei Instrumente erweitert 16.07.2026)
Top-Down immer in dieser Reihenfolge, nie überspringen. **Seit 16.07.2026 läuft dieser Check auf BEIDEN Panes** (NAS100 + QQQ), nicht mehr nur NAS100 — löst die Asymmetrie auf, dass der finale Entry-Trigger (Dual-Gate, [[feedback_live_trading]] Punkt 7b) längst zwei Instrumente prüft, der übergeordnete MTF-Bias bisher aber nur eines. Nur in der ruhigen Phase durchführen (siehe [[feedback_live_trading]] Punkt 3a); in der heißen Phase entfällt der QQQ-Teil ersatzlos, Zeitdruck hat Vorrang.

**Schritt 1 — 1H Chart: Bias festlegen**
- **NAS100 (`chart_set_timeframe` 60):** Trend: Higher Highs/Lows (bullisch) oder Lower Highs/Lows (bearisch)? Große S/R-Zonen (Vortagshoch/-tief, Wochenhoch/-tief)? EMA50-Verhältnis?
- **QQQ (`pane_focus` auf Pane 1, dort ebenfalls `chart_set_timeframe` 60):** VWAP-Verhältnis (Kurs darüber/darunter), Volumen-Kontext, eigene EMA50 — siehe [[feedback_live_trading]] Punkt 7b für die Gewichtung (EMA50 = Pflicht-Gate, VWAP+Volumen = unterstützend, kein harter Blocker). Entfällt ersatzlos, wenn das Session-Gate aus Punkt 7e gerade geschlossen ist.
- Ergebnis: klares "Long-Bias" / "Short-Bias" / "Neutral", getragen von BEIDEN Instrumenten — dieser Bias ÜBERSCHREIBT das 5min-Bild

**Schritt 2 — 15min Chart: Entry-Zone eingrenzen**
- **NAS100:** Welches Level ist als Entry geeignet? (S/R-Nähe) Gibt es ein erkennbares Muster (Konsolidierung, Keil, Range)? MACD und RSI auf 15min: Stützen sie den 1H-Bias?
- **QQQ:** VWAP-Nähe/-Richtung als zusätzlicher Kontext für die Entry-Zone — unterstützend, kein eigenständiges Muss

**Schritt 3 — 5min Chart: Trigger bestätigen**
- Bestätigungskerze (Hammer, Engulfing, Schlusskurs über/unter Level)
- Volumen: steigt beim Ausbruch?
- RSI nicht extrem überkauft/überverkauft beim Entry
- KEIN Entry gegen den 1H-Bias, egal wie gut die 5min-Kerze aussieht

### 8a1. Zweistufiges Trigger-System bei weit entferntem Bestätigungslevel (ergänzt 02.07.2026)
Wenn das identifizierte Bestätigungslevel für ein Setup weit vom aktuellen Kurs entfernt ist (z.B. 100+ Punkte NAS100), nicht stur auf das eine ferne Level warten — stattdessen in zwei Stufen denken:

1. **Naher taktischer Trigger:** ein näheres, bereits mehrfach getestetes Level (z.B. lokaler Widerstand der letzten paar Kerzen) + Doppelkriterium (RSI-Schwelle, MACD) → rechtfertigt einen kleineren, taktischen Einstieg mit engerem SL
2. **Fernerer struktureller Trigger:** das ursprüngliche, weiter entfernte Level (z.B. alter Bounce-Fail-Punkt, EMA50-Reclaim) → bleibt Ziel/Bestätigung für die volle Positionsgröße bzw. den übergeordneten Trendwechsel

**Why:** Am 02.07.2026 wurde ein Long-Bestätigungslevel bei 29.530 vorgeschlagen, während der Kurs bei ~29.430 stand (100 Punkte entfernt). User hat zurecht hinterfragt, ob das nicht zu weit weg ist, um darauf zu warten. Lösung: näheres Level (29.500, bereits zweimal angetestet) als taktischen Trigger definiert, das ferne Level (29.530/EMA50) blieb für die volle Bestätigung bestehen — User bestätigte diesen Ansatz als sinnvoll.

**How to apply:** Bei jedem Setup mit einem fernen Bestätigungslevel aktiv prüfen, ob ein näheres, technisch begründbares Level als Zwischenschritt sinnvoll ist, statt den User pauschal auf das eine ferne Level zu vertrösten.

### 8a2. Zwei-Kerzen-Bestätigung für den EMA50-Reclaim-Trigger (Stufe-1, ergänzt 22.08.2026, Fable-Kernstrategie-Review)

Der Stufe-1-Trigger aus 8a1 basiert bei einem EMA50-Reclaim bisher auf einem einzelnen 5-Min-Kerzenschluss jenseits EMA50 plus einer unpräzise definierten "Folgekerze" (z.B. Trade #38: "5min-EMA50-Schlusskurs-Bruch + Folgekerze") — unklar blieb dabei, ob die Folgekerze selbst ebenfalls jenseits EMA50 schließen musste oder nur "keine Gegenbewegung" zeigen durfte. Ab jetzt gilt präzise: Der Stufe-1-EMA50-Reclaim-Trigger braucht **2 aufeinanderfolgende 5-Min-Kerzenschlüsse auf derselben Seite von EMA50** (beide Schlusskurse bestätigen den Bruch, nicht nur der erste), bevor die Stufe-1-Position ausgelöst wird.

**Why:** Trade #38 (18.08.2026) — Entry nach Bruch-Kerzenschluss + Folgekerze, RR-Gate 2,56:1 ✓, Stufe-1-Größe korrekt angewendet. Kurs fiel direkt danach in einen Fehlausbruch/Retest zurück, testete die SL-Zone zweimal intrabar, SL wurde ausgelöst (-17,75€). Ob die damalige "Folgekerze" selbst über EMA50 schloss, ist aus der Tagesnotiz nicht eindeutig rekonstruierbar — genau diese Unschärfe soll die Neufassung beseitigen. Ausgelöst durch Levis Frage nach der Kernstrategie (22.08.2026, nach dem verworfenen Reversal-Tactical-Trigger-Thema desselben Tages).

**Trade-off (explizit, mit Levi besprochen):** Verzögert den Stufe-1-Trigger um mindestens eine weitere 5-Min-Kerze, verschiebt Entry-/SL-/TP-Level und senkt tendenziell das RR (Entry weiter vom ursprünglichen SL-Level entfernt, TP oft nicht proportional weiter). Gilt **nur** für den Stufe-1/taktischen EMA-Reclaim-Trigger (8a1), NICHT für den primären NAS100-Dual-Gate-Kerzenschluss (Punkt 7b in [[feedback_live_trading]]) — dort bleibt vorerst 1 bestätigter Kerzenschluss Standard. Eine Ausweitung auf 2 Kerzen für das gesamte Dual-Gate ist eine offene, hier bewusst nicht getroffene Entscheidung.

**How to apply:** Bei jedem Stufe-1-Setup nach 8a1 mit EMA50-Reclaim als Trigger-Bedingung erst nach dem 2. aufeinanderfolgenden 5-Min-Kerzenschluss auf der neuen Seite von EMA50 auslösen, nicht nach dem 1. Im Output explizit kennzeichnen (z.B. "EMA50-Reclaim: Kerze 1/2 bestätigt, warte auf 2. Schluss").

**Review-Pflicht:** Analog zu anderen frisch eingeführten Schwellen (7a1, Punkt 12) nach den nächsten 5 Anwendungsfällen prüfen, ob die zusätzliche Verzögerung den Whipsaw-Schutz wert war oder zu oft echte, schnelle Fortsetzungen abschneidet.

### 8a3. Volumen-Soft-Gate während der Reclaim-Bestätigung (8a2, ergänzt 22.08.2026, Fable-Kernstrategie-Review)

Ergänzt 8a2 um eine Volumen-Prüfung während desselben Bestätigungsfensters (jetzt 2 Kerzen), bewusst NICHT als durchgehendes, permanentes Gate über die gesamte Beobachtungsphase.

**Regel:** QQQ-Volumen über die beiden Reclaim-Bestätigungskerzen (relativ zum eigenen Durchschnitt der unmittelbar vorausgehenden Bewegung) wird mitgeführt. Liegt es deutlich darunter (Faustregel <50%, dieselbe Schwelle wie beim bereits bestehenden Volumen-Dämpfer in [[feedback_live_trading]] Punkt 11), gilt das als Vertrauens-Dämpfer: der Trigger löst trotzdem aus (kein hartes Veto), aber explizit mit dem Vermerk "schwach getragen" statt "bestätigt" — bei ohnehin grenzwertigen RR-/TP-Realismus-Werten (8b/8b1) sollte ein schwach getragener Reclaim eher zum Auslassen führen als ein volumenbestätigter.

**Pflicht-Ausgabezeile beim Stufe-1-Trigger:** `Reclaim-Volumen-Check: QQQ X K (Reclaim-Fenster, 2 Kerzen) vs. Y K (Ø vorausgehende Bewegung) → bestätigt/schwach getragen`

**Warum nur im Reclaim-Fenster, nicht durchgehend (Levis Nachfrage 22.08.2026 beantwortet):** Ein permanentes, tick-für-tick-Volumen-Gate wurde bewusst geprüft und verworfen, aus zwei Gründen: (1) Volumen ist ein Ereignis-Signal, kein Zustands-Signal — die TA-Aussage "Ausbruch braucht Volumen" bezieht sich auf den Moment des Levelbruchs selbst, nicht auf jede beliebige Minute davor/danach. Volumen schwankt außerhalb dieses Moments aus vielen harmlosen Gründen (Tageszeit, Nachrichten-Pause), ohne dass das etwas über die Validität der Bewegung aussagt — ein Dauer-Gate würde ein sauberes Signal mit Rauschen verdünnen, nicht schärfen. (2) Ein durchgehendes Gate würde gegen die bestehende Phasen-Architektur aus [[feedback_live_trading]] Punkt 7b verstoßen ("heiße Phase: KEIN Pane-Wechsel zu QQQ mehr pro Loop-Durchlauf") — dieser Tempo-Schutz existiert genau deshalb, weil wiederholte QQQ-Checks reale Sekunden kosten und den eigentlichen Preis-Trigger verzögern können. Die Bindung an das 2-Kerzen-Reclaim-Fenster (statt an jeden 1-Min-Tick) ist der bewusste Mittelweg: breiter als eine einzelne Momentaufnahme (deckt jetzt beide Bestätigungskerzen aus 8a2 ab), aber zeitlich begrenzt statt unbegrenzt fortlaufend. Der bereits bestehende Volumen-Dämpfer in Punkt 11 ([[feedback_live_trading]]) bleibt davon unberührt — der ist für eine andere Frage (Position verteidigen/drehen) gedacht und läuft dort bewusst im Voll-Check-Rhythmus, nicht kontinuierlich.

**Review-Pflicht:** Läuft mit derselben 5-Anwendungsfälle-Prüfung wie 8a2 (beide Regeln entstehen zusammen, aus demselben Anlass, gemeinsam zu bewerten).

### 8b. TP-Festlegung — Teilverkauf-Standard (verbindlich, überarbeitet 04.07.2026 nach Fable-5-Review, siehe [[feedback_realisiertes_rr]])
TP wird IMMER technisch bestimmt, nie als Wunsch-%:
- Nächste Resistance/Support (mehrfach getestete Levels)
- Vorheriges Hoch/Tief (Tag/Woche)
- Runde Zahlen
- Fibonacci-Extension (1.272 / 1.618)

**Warum überarbeitet:** Nach 12 Trades hat KEIN einziger Trade je ein TP erreicht — jeder Gewinn war ein vorzeitiger manueller Exit durch ein legitimes Schutzsignal (Divergenz, 3h-Regel, Breakeven nach RSI-Extrem, Chop/Muster-Exit), das fast immer vor dem fernen TP auslöste. Realisiertes RR lag bei ~0,78:1 statt geplanter 1:2. Die Schutzsignale selbst waren nicht das Problem (sie haben mehrfach Round-Trips verhindert, z.B. Trade #2) — das Problem war ein einzelnes, zu fernes TP-Ziel ohne Zwischenstufe.

**Neuer Ablauf — IMMER zwei TP-Stufen definieren, nicht nur bei Gelegenheit:**
1. Struktur/Levels bestimmen → SL am Level festlegen → **erst danach** RR-Check (feste Reihenfolge, siehe Update 27.07.2026 unten — nicht iterativ hin- und herschieben)
2. **TP1** = das nächste technische Level zwischen Entry und dem alten Fernziel (z.B. erste Fibonacci-Extension 1.272, nächste kleinere Zwischenzone, naher runder Preis) — MUSS mindestens RR 1:1 zum SL bieten (hart, unverändert)
3. **TP2** = das strukturelle Fernziel wie bisher (nächste große Resistance/Support, Fib-Extension 1.618/2.0, Vortages-/Wochenhoch/-tief) — **keine RR-Mindestpflicht mehr seit 27.07.2026** (die frühere "≥1:2"-Pflicht entfällt, siehe Update unten), offen nach oben, kein Deckel
4. Wenn schon TP1 kein RR ≥1:1 erreicht → **entweder** Trade auslassen, **oder** auf einen besseren/engeren Entry näher an der echten Struktur warten, bei dem ein regelkonformer (ATR-basierter), aber engerer SL möglich wird (siehe Update 27.07.2026) — NICHT das TP-Ziel künstlich weiter hinausschieben, um RR≥1:1 formal zu erzwingen

**Update 27.07.2026 (Fable-Review, Levi-Entscheidung — RR als Auswahlfilter statt Ziel-Diktat):** Die alte starre Pflicht "TP1≥1:1 UND TP2≥1:2" führte praktisch dazu, dass bei notwendig breitem SL (Chop-/Schock-Tag, siehe 7a/8c) TP2 auf ein technisch nicht vorhandenes, unrealistisch fernes Level gezogen werden musste (Levi-Beobachtung, 1-2 konkrete Praxisfälle), oder der Trade ausgelassen wurde, obwohl ein valider, näherer TP1 durchaus erreichbar gewesen wäre. Nach gemeinsamer Analyse mit Fable: **RR ist ein Auswahlfilter (TP1-Untergrenze), keine Ziel-Vorschrift.** TP kommt IMMER aus echter Marktstruktur (Fib-Extension, Measured Move, S/R) — RR ist das Ergebnis dieser Rechnung, kein Eingabewert, der die TP-Position bestimmt. Deshalb: TP1 bleibt hart bei RR≥1:1 (niedrige, meist erreichbare Hürde — anders als die alte TP2-1:2-Pflicht, die oft strukturell gar nicht vorhanden war), TP2 wird komplett von jeder RR-Pflicht befreit und läuft offen nach oben unter den bestehenden Schutzsignalen (Divergenz 9b, Reversal-Kriterien Punkt 11, Stall-Regel Punkt 12, Widerstandstest+RSI 8e). Feste Reihenfolge bei der Trade-Vorbereitung: **Struktur/Levels bestimmen → SL festlegen → RR-Check (nur TP1 gegen 1:1) → Go/No-Go** — nicht iterativ hin- und herschieben (das war der Kernfehler bei Trade #17, siehe 7a). Ist selbst TP1 bei notwendig breitem SL nicht bei RR≥1:1 erreichbar: NICHT das TP künstlich weiter hinausschieben, sondern entweder auslassen oder auf einen besseren/engeren Entry näher an der echten Struktur warten, an dem ein regelkonformer, aber engerer ATR-SL möglich wird.

**Ausführung bei TP1-Erreichen:** 50% der Position schließen, SL auf dem Rest sofort auf Breakeven nachziehen. Die verbleibenden 50% laufen weiter Richtung TP2 — dabei gelten alle bisherigen Schutzsignale unverändert und uneingeschränkt (Divergenz 9b, Chartmuster 9d1/4a, 3h-Regel, Chop-Erkennung) für diese Restposition. Kein neuer Mechanismus für den Rest, nur die bisherige Logik auf eine kleinere Position angewendet.

**Zählung:** Ein TP1-Teilverkauf + Rest-Exit ist weiterhin EIN Trade für die Phasen-Zählung (siehe Definition "1 Trade" oben), keine zwei.

**Erwarteter Effekt:** Der bisherige Fehlmodus — Exit durch Schutzsignal bevor das ferne TP überhaupt in Reichweite kommt — trifft jetzt nur noch die zweite Hälfte der Position, nachdem die erste Hälfte bereits zuverlässiger bei TP1 realisiert wurde. Das hebt das realisierte RR an, ohne die Schutzsignale abzuschwächen, die bisher Verluste verhindert haben.

### 8c. Mindestschwelle für Setups — ATR-basiert (Methode auf echten ATR(14) umgestellt 23.07.2026, Fable-Review)
Kein Trade wird genommen, wenn SL zu nah am aktuellen Kurs liegt (würde durch normales Markt-Rauschen ausgestoppt statt durch echte Trendwende).

**Methode (aktualisiert):**
1. Aktuellen ATR(14)-Wert vom Chart ablesen (`data_get_study_values`, seit 23.07.2026 fester Bestandteil des NAS100-Indikatoren-Sets, siehe [[feedback_chart_layout]]) — ersetzt die vorherige manuelle Schätzung der "größten Rausch-Kerze". Grund: Diese Schätzung fiel bei Trade #21 zu eng aus (55 statt nötiger 109-145 Pkt) und machte den anschließenden RR-Check ungültig.
2. Mindest-SL-Abstand = 1,5-2× ATR(14)
3. Mindest-TP1 (bei RR 1:1) ergibt sich daraus automatisch — TP2 folgt eigenständig aus der Marktstruktur, keine RR-Kopplung an den SL mehr (siehe Punkt 8b, Update 27.07.2026)

**Alte Methode (bis 22.07.2026, nur noch als Fallback falls ATR ausnahmsweise nicht verfügbar ist):** Größte "Rausch-Kerze" der letzten Kerzen im 5min-Chart manuell ermitteln (z.B. 12.06.2026: bis zu 120 Punkte / 0,41% Nasdaq ohne Trendwechsel), dann wie oben 1,5-2× als Mindest-SL-Abstand.

**Faustregel bei normaler Volatilität (VIX ~18, Stand 12.06.2026):**
- Mindest-SL auf Zertifikat (5x Hebel): ~3%
- Mindest-TP auf Zertifikat: ~6-8%
- Trades mit kleinerem TP-Ziel als diese Schwelle werden ausgelassen — zu hohes Rauschen-Risiko

**Dynamisch anpassen:** Bei VIX > 25 → größere Abstände nötig. Bei VIX < 15 → engere Ziele evtl. möglich. Schwelle bei jedem Setup neu anhand aktueller Kerzen berechnen, nicht statisch verwenden.

**Schock-Tag-Multiplikator (ergänzt 02.07.2026):** An erkannten News-Schock-Tagen (siehe 9e) ist die "Rausch-Kerze" selbst oft deutlich größer als an ruhigen Tagen (z.B. 02.07.2026: 100-150 Pkt statt der üblichen 30-60 Pkt). An solchen Tagen den Multiplikator aus Schritt 2 auf **2,5-3× statt 1,5-2×** erhöhen, damit der SL nicht formal regelkonform, aber im Tageskontext trotzdem zu eng ist (siehe Trade #12, 02.07.2026 — SL bei 90 Pkt war rechnerisch regelkonform, aber im Kontext des Schock-Tags knapp).

**Positionsgrößen-Kopplung (ergänzt 27.07.2026, Fable-Review):** Sobald der Mindest-SL-Abstand den Schock-/Chop-Tier (2,5-3× ATR) statt des Normal-Tiers (1,5-2×) zieht, gilt automatisch ein 50%-Floor der Positionsgröße — reiht sich als vierter Trigger in die bestehende Stacking-Regel ein, siehe [[project_risikomanagement]] Abschnitt "Stacking-Regel für gleichzeitige Halbierungs-Gründe". **Why:** Ein weiter SL hält das €-Risiko über die Positionsgrößen-Formel zwar bereits automatisch konstant (solange der Risiko-Limit-Term bindet) — die zusätzliche Halbierung ist trotzdem sinnvoll, weil ein SL im Schock-/Chop-Tier primär ein Konfidenz-Abschlag ist (unschärfere Level, mehr Fehlausbrüche, siehe #17/#19/#21), kein reines Risiko-Zahlen-Problem. **Nicht doppelt zählen:** Stammen Chop-Erkennung UND der Schock-ATR-Tier aus demselben Marktregime, gilt trotzdem nur EINMAL der 50%-Floor, kein Multiplizieren.

**Pflicht-Ausgabezeile bei Schock-Tag-Status (ergänzt 07.08.2026, Fable-Review nach Trade #34 — schnellster Loss der Historie):** Am 07.08.2026 war der Tag im selben Loop-Durchlauf bereits durchgehend als Schock-Tag klassifiziert (Regime-Gate 8d), der ATR-Wert war bei jedem Voll-Check sichtbar — trotzdem wurde beim SL-Setzen die normale Tier (1,5-2x) statt der vorgeschriebenen Schock-Tier (2,5-3x) verwendet. Das Problem war keine fehlende Information, sondern eine fehlende Verbindung zwischen bereits erkannter Klassifikation und der SL-Formel (von Fable im Review bestätigt: "eine korrekt erkannte Klassifikation muss aktiv mit der abhängigen Formel verknüpft werden, sonst bleibt sie wirkungslose Information"). **Fix:** Sobald Regime-Gate 8d "Schock-Tag ✓" zeigt, gehört ab jetzt bei JEDER SL-Festlegung eine Pflichtzeile in den Output, bevor der Trade final bestätigt wird: `SL/ATR-Ratio: X,Xx — Schock-Tier (2,5-3x) erfüllt ✓/✗`. Fehlt diese Zeile bei einem Trade an einem erkannten Schock-Tag, gilt die Entry-Prüfung als nicht vollständig — exakt dieselbe Behandlung wie die bereits bestehenden Pflichtzeilen (Tweet-Check, Kerze-geschlossen, Format). Bewusst NICHT übernommen: ein hartes Veto (Trade-Blockade bei Verstoß) oder ein separates Cross-Check-Skript — beides wurde nach gemeinsamer Prüfung (Sonnet 5 + Fable 5) verworfen, weil die Lücke die Verbindung war, nicht die Datenbeschaffung oder fehlende Durchsetzung in diesem manuellen System.

**Schock-Tier-RR-Gate — SL/ATR-Kopplung UND RR-Rückkopplung gegen TP1 (ergänzt 12.08.2026, Fable-Review im Rahmen der Phase-1-3-Vollprüfung):** Die Pflichtzeile oben verbindet die Schock-Klassifikation nur mit der SL-Distanz. Sie sagt noch nichts darüber, ob der dadurch verbreiterte SL die RR-Pflicht aus Punkt 7a/8b weiterhin erfüllt (Trade-#17-Präzedenzfall: dort wurde der SL nach demselben Prinzip verbreitert, TP1 aber nicht neu gegengerechnet, RR fiel unbemerkt auf 0,32:1). Sobald Regime-Gate 8d "Schock-Tag ✓" zeigt, gilt deshalb zusätzlich zur Positionsgrößen-Kopplung oben dieser zwingende 5-Schritte-Ablauf:

1. SL-Basisniveau technisch bestimmen (Methode aus Punkt 7a, unverändert).
2. Schock-Tier-Floor anwenden: SL-Distanz = MAX(technisches SL-Level, 2,5-3× ATR(14)).
3. RR gegen das bereits identifizierte, echte TP1-Level zwingend neu berechnen — TP1 darf dabei NICHT verschoben werden (bestehendes Verbot aus 8b bleibt unverändert in Kraft).
4. Go/No-Go-Gate:
   - **RR(TP1) ≥ 1:1 →** Trade gültig. Stacking-Regel prüfen (SL-Breite bleibt der 4. Trigger für den 50%-Floor, die "nicht doppelt zählen"-Klausel oben bleibt gültig). Erst danach `size.cjs` mit dem verbreiterten SL aufrufen. Zwei Pflicht-Ausgabezeilen (ergänzt die Zeile aus dem 07.08.2026-Fix oben um eine zweite, ersetzt sie nicht): `SL/ATR-Ratio: X,Xx — Schock-Tier erfüllt ✓` UND `RR-Check TP1 bei Schock-Tier-SL: X,X:1 ✓`.
   - **RR(TP1) < 1:1 →** Trade INVALIDE. Nur zwei erlaubte Reaktionen: Trade auslassen, oder auf einen näheren/engeren Entry an der echten Struktur warten, an dem ein Schock-Tier-SL RR≥1:1 gegen ein echtes technisches TP1-Level noch erfüllt. Keine dritte Option — insbesondere NICHT TP1 verschieben, um die Rechnung zu erzwingen, und NICHT den SL nachträglich wieder verengen.
5. Risiko-Limit selbst braucht keine zusätzliche Prüfung: Die bestehende Positionsgrößen-Formel (Schritt 6-8 in [[project_risikomanagement]], MIN(Risiko-Limit, Positions-Limit)) garantiert bereits automatisch, dass ein breiterer SL nur die Stückzahl senkt, nie das €-Risiko über das Limit hebt — das ist bereits gelöst, nicht neu zu bauen.

**Why:** Verbindet zwei Lücken, die bislang getrennt bestanden, obwohl sie immer zusammen hätten gelten müssen: Trade #34 (07.08.2026, siehe Pflichtzeile oben) zeigte die fehlende Verknüpfung von erkannter Schock-Klassifikation zur SL-Formel; der strukturelle Präzedenzfall aus Trade #17 (13.07.2026, siehe 7a) zeigte das Gegenstück — ein verbreiterter SL wurde gesetzt, ohne TP1 gegen die neue RR neu zu prüfen. Beide Regeln bestanden bereits einzeln, wurden aber nie als ein gemeinsamer Ablauf durchgesetzt. Ergänzt am 12.08.2026 im Rahmen der Fable-Phase-1-3-Vollprüfung. **Offener Folgepunkt (hier bewusst NICHT gelöst, wird separat geprüft):** eine noch unbestätigte Frage zur Stacking-Regel bei Trade #35 (07.08.2026) — Positionsgrößen-Diskrepanz 276 vs. 376 Stk., siehe [[trades/trading_2026-08-07]].

**Kein Automatismus — trade-abhängig, mit echten 5-Min-Bars verifiziert (ergänzt 03.07.2026, siehe [[feedback_backtest_ergebnis_2026-07-03]] Teil 3).** Der Multiplikator ist kein Freifahrtschein, der jeden Verlust verhindert: Bei Trade #8 hätte ein 2,5-3x weiterer SL den Whipsaw überlebt (Kurs erholte sich 45 Min. später bis zum Tageshoch) — bei Trade #12 hätte er NICHT geholfen, weil der Kurs danach ungebremst weiterfiel und der Verlust nur größer geworden wäre. **How to apply:** 8c reduziert Rauschen-Stopps, ersetzt aber nicht die aktive Beobachtung — 9d1/4a (Muster+Kerze live erkennen und aussteigen) bleibt die zuverlässigere Regel, weil sie unabhängig davon funktioniert, ob sich der Markt danach erholt oder weiter fällt.

**Verstoß gegen die BASIS-Regel (nicht nur den Chop-Multiplikator) bei Trade #21 (16.07.2026, Fable-verifiziert):** Größte Rausch-Kerze der 45 Min vor Entry: 72,7 Pkt (14:55-Kerze). Mindest-SL nach Schritt 2 (1,5-2×, schon OHNE Chop-/Schock-Aufschlag): 109-145 Pkt. Tatsächlicher SL: nur 55 Pkt — unterschritt damit bereits die einfache Basis-Regel, nicht erst den Chop-/Schock-Multiplikator. **Direkte Folge:** Mit einem regelkonformen SL (109+ Pkt) hätte TP1 (75 Pkt entfernt) nur RR ≈0,69:1 erreicht — klar unter der Pflicht-Schwelle ≥1:1 aus Punkt 8b. Der zum Entry-Zeitpunkt geloggte "RR-Check bestanden" (1,4:1) war also ein Artefakt des zu engen, nicht-regelkonformen SL — keine echte Bestätigung. **How to apply (verschärft):** Schritt 1-2 dieser Regel (Rausch-Kerze ermitteln, SL ≥1,5-2× davon) ist ein Pflicht-Gate VOR dem RR-Check aus 8b, nicht danach — wird die SL-Mindestdistanz verletzt, ist die anschließende RR-Rechnung automatisch ungültig, selbst wenn sie rechnerisch "besteht". Bei sehr nahen TP-Zielen (wie hier 75/115 Pkt gegenüber 109+ Pkt Mindest-SL) bedeutet das oft: Trade auslassen, weil kein regelkonformer SL+TP-Kombination die RR-Minima gleichzeitig erfüllt — nicht den SL nachträglich verbreitern und hoffen, dass die TPs noch passen.

### 8c2. SL-Cluster-Regel — wiederholter Wick-Stop in derselben Zone erzwingt zusätzlichen SL-Puffer (ergänzt 21.08.2026, Fable-Review nach Trade #42/#43, umgesetzt nach Levi-Entscheidung)

Wurde ein SL innerhalb derselben Handelssession bereits durch einen Wick/Spike (nicht durch einen bestätigten Kerzenschluss) ausgelöst, muss ein neuer Trade in dieselbe Richtung, dessen SL in der bereits getesteten Zone (±20-30 Punkte um das alte SL-Level) läge, seinen SL um mindestens **0,5× ATR(14)** zusätzlich von dieser Zone entfernt platzieren — oben auf die reguläre Methode aus Punkt 8c (1,5-2× ATR bzw. 2,5-3× im Schock-Tier).

**Why:** Trade #42 (21.08.2026) wurde bei SL 29.255 durch einen Wick ausgestoppt (-54,35€), Trade #43 direkt danach in derselben Richtung mit SL 29.248 — nur 7 Punkte tiefer, praktisch in derselben Zone — erneut durch einen Wick ausgestoppt (-29,18€). Beide SLs lagen faktisch im selben mehrfach angelaufenen Level (29.240-254), das insgesamt mindestens viermal getestet wurde, bevor es brach. Ein SL, der bereits einmal in dieser Session durch Rauschen (Wick, kein Schlusskurs) gefallen ist, hat sich als Liquiditätsmagnet erwiesen — ein zweiter Trade mit SL an praktisch derselben Stelle wiederholt exakt dasselbe Risiko, ohne dass sich an der Marktstruktur etwas geändert hätte.

**How to apply:** Vor jeder SL-Festlegung aktiv prüfen, ob in der laufenden Session bereits ein SL (eigener oder erkennbar fremder, z.B. eine sichtbare Wick-Rejection an genau diesem Level) in einer Distanz von ≤20-30 Punkten zum geplanten neuen SL durch einen Wick ausgelöst wurde. Wenn ja: neuer SL = altes SL-Level ± zusätzlich 0,5× ATR(14) in die sichere Richtung, oben auf die reguläre 8c-Distanz. Der TP-Realismus-Check (8b1) läuft danach ganz normal gegen den neuen, weiteren SL-Abstand — führt das zu RR < 1:1, gilt Trade auslassen oder engerer Entry (identische Eskalationslogik wie in 8c/8b1 bereits etabliert), nicht TP künstlich verschieben.

**Abgrenzung zu 7a1 (Pivot-Magnet-Puffer):** 7a1 behandelt berechnete Pivot-/Rundzahlen-Level als potenzielle Magneten unabhängig davon, ob sie schon getestet wurden. Diese Regel hier ist enger und schärfer: Sie greift nur, wenn ein SL in dieser Zone bereits NACHWEISLICH in dieser Session gefallen ist (nicht nur theoretisch nah an einem Level liegt) — beide Regeln können gleichzeitig greifen, addieren sich aber nicht (max. eine der beiden Puffer-Ergänzungen zählt, nicht kumulativ).

### 8b1. TP-Realismus-Filter — Hard-Gate vor der TP1-Auswahl, nicht nur Warnzeile danach (ergänzt 18.08.2026, Fable-Review nach Trade #38)

**Kernprinzip (bestätigt die bestehende Kausalkette aus 8b, ergänzt sie um einen fehlenden Filterschritt):** SL und TP kommen bewusst aus zwei unterschiedlichen Quellen — SL beantwortet "wo beweist der Markt, dass die These falsch ist" (Struktur/Rauschpuffer, siehe 7a/8c), TP beantwortet "wohin bewegt sich der Markt plausibel, wenn die These stimmt" (echtes technisches Level: Fib-Extension, EMA-Konfluenz, Pivot, Struktur-Hoch). Diese Trennung ist **kein** Konstruktionsfehler und wird NICHT aufgehoben — ein TP, das stattdessen mechanisch aus der SL-Distanz abgeleitet würde (z.B. immer "SL × Faktor"), wäre der eigentliche Zufallstreffer, weil er keinen Bezug zu echter Struktur/Liquidität hätte. Der tatsächliche Fehler lag bislang darin, dass ein TP-Kandidat, der zwar auf einem echten Level lag, aber weit außerhalb der real beobachtbaren Marktbewegung des Tages, trotzdem als gültiges TP1 akzeptiert wurde, solange die RR-Zahl rechnerisch ≥1:1 ergab.

**Neuer, verbindlicher Ablauf (ersetzt Schritt 2 aus dem 8b-Ablauf, Rest von 8b bleibt unverändert):**
1. Setup/Trigger bestätigt (wie bisher).
2. SL aus Struktur/Rauschkerzen bestimmen (7a/8c inkl. Schock-Tier) → definiert die Risikoeinheit R.
3. TP-Kandidatenliste aus echten technischen Levels bilden (Fib-Extension, EMA-Konfluenz, Pivot, Struktur-Hoch/-Tief), aufsteigend nach Distanz sortiert.
4. **Realismus-Filter (neu, VOR der TP1-Auswahl):** Kandidaten verwerfen, die weiter als **2× ATR(14)** ODER weiter als **2× die Breite der unmittelbar vorausgehenden Konsolidierungsbox** (soweit erkennbar) entfernt liegen — diese gelten als statistisch kaum erreichbar für die Marktbewegung des Moments, unabhängig davon, wie das RR rechnerisch aussieht.
5. TP1 = der nächste verbleibende (realistische) Kandidat aus Schritt 4, der zusätzlich RR≥1:1 zu R erfüllt (Schritt aus 8b unverändert). RR ergibt sich aus der echten Distanz — schwankendes RR (mal 1,3:1, mal 3:1, je nach Lage des nächsten echten Levels) ist gesund und erwartbar, kein Mangel, der ausgeglichen werden müsste.
6. Erfüllt kein Kandidat innerhalb des Realismus-Fensters (Schritt 4) gleichzeitig RR≥1:1 → **kein Trade.** Explizit als "kein valides RR trotz bestätigtem Setup" loggen — nicht das ferne, unrealistische Level trotzdem als TP1 nehmen, nur weil die RR-Zahl formal passt.

**Pflicht-Ausgabezeile vor jeder Entry-Bestätigung (gleiche Behandlung wie die bestehenden Pflichtzeilen aus 8c/07.08. und 12.08.):**
`TP-Realismus: TP1 X Pkt = Y,Y× ATR / Z,Z× letzte Box-Breite — ✓/✗`
Fehlt diese Zeile, gilt die Entry-Prüfung als nicht vollständig.

**Why:** Bei Trade #38 (18.08.2026) lag TP1 (15min-EMA50, 100 Punkte entfernt) bei ca. 2,7-3,1× ATR(14) und über dem 2-fachen der vorausgehenden 45-Punkte-Konsolidierungsbox, die 40-50 Minuten gehalten hatte — objektiv außerhalb dessen, was die unmittelbare Marktbewegung plausibel hergab. Das RR-Gate (2,56:1) bestätigte den Trade trotzdem, weil TP und SL bis dahin nur als unabhängiger Quotient geprüft wurden, nie gegen die tatsächliche Bewegungsgröße des Tages. Der Kurs kam nach Entry nie näher an TP1 heran als die Ausgangsbasis, sondern fiel direkt zurück in die alte Box und wurde dort ausgestoppt — ein Totalausfall der erwarteten Bewegung, kein normaler Rücksetzer. Zusatzbefund aus derselben Analyse: TP1 und TP2 lagen bei #38 nur 22 Punkte auseinander, was die Zweistufen-Teilgewinn-Logik aus 8b praktisch entwertete — auch das ist ein Symptom fehlender Realismus-Prüfung bei der TP-Auswahl.

**Bewusst NICHT übernommen:** Ein fester, kalibrierter Zahlenwert für die 2×-Schwelle über den aktuellen Vorschlag hinaus — die Datenbasis (n=13 in Phase 3) ist noch zu klein, um den exakten Schwellenwert empirisch zu validieren. Nach den nächsten 10-15 Trades mit dieser Pflichtzeile im Log prüfen, ob TP/ATR- bzw. TP/Box-Verhältnis tatsächlich mit Trefferquote/MFE korreliert, und die Schwelle bei Bedarf nachjustieren.

**Review-Pflicht:** Wie bei anderen frisch eingeführten Heuristiken (siehe 7a1) nach den nächsten 10-15 Anwendungsfällen prüfen, ob der Filter tatsächlich schlechte Trades wie #38 verhindert hätte, ohne dabei valide Trades unnötig auszusortieren. Zeigt sich kein klarer Nutzen oder eine zu hohe Fehlausschluss-Rate, Schwelle anpassen statt Regel unverändert als Karteileiche stehen zu lassen.

**Zeitliche Kopplung an den Entry (ergänzt 19.08.2026, siehe [[feedback_live_trading]] Punkt 7b1):** Diese Prüfung (wie auch der RR-Check aus 8b) muss als abgeschlossenes PASS VOR jeder Order-Ausführung vorliegen, nicht nur rechnerisch irgendwann davor oder danach nachgewiesen sein — auch wenn Levi die Order eigenständig ohne Claudes Bestätigung auslöst. Volle Regel, Ablauf und Pflicht-Ausgabezeile dazu in [[feedback_live_trading]] Punkt 7b1.

**Log-Präzisierung — exakte Zahlen statt nur ✓/✗ (ergänzt 20.08.2026, Fable-Review nach Trade #40/#41):** Bei Trade #40 (20.08.2026) wurde die Pflichtzeile korrekt mit ✓ ausgegeben, aber der exakte ATR(14)-Wert zum Entry-Zeitpunkt nicht separat dokumentiert — beim Tagesabschluss musste er aus einer Tagesspanne (44-104) grob geschätzt werden, eine zu breite Range für eine spätere Kalibrierung. Ab sofort gehört zur Pflichtzeile aus Schritt 4 zusätzlich der **exakte ATR(14)-Wert und die exakte Box-Breite als Zahl** (nicht nur das daraus abgeleitete Verhältnis), sowie nach Trade-Abschluss zusätzlich **wie nah TP1 tatsächlich erreicht wurde (MFE in Punkten)** in die Trade-DB-Notiz (`add_trade.cjs --notes`). Format der Pflichtzeile unverändert (`TP-Realismus: TP1 X Pkt = Y,Y× ATR(=Z Pkt) / W,W× Box(=V Pkt) — ✓/✗`), nur die Rohwerte werden jetzt zusätzlich mitgeführt statt nur des Verhältnisses.

**Why:** Fable-Review (20.08.2026) bestätigte anhand von Trade #40/#41: Bei n=2 ist keine inhaltliche Aussage zur 2×-Schwelle möglich (Review-Pflicht liegt weiterhin bei 10-15 Fällen, unverändert), aber schon jetzt zeigte sich, dass Trade #40 wegen der fehlenden Rohwert-Dokumentation nicht als sauberer Kalibrierungsdatenpunkt taugt, nur als Prozess-Nachweis (Pflichtzeile korrekt ausgegeben). Ohne diese Ergänzung würde die eigentliche Review nach 10-15 Fällen auf denselben Schätzungs-Fallstrick treffen wie hier. Zusatzbeobachtung aus dem Review (noch keine Regel, nur Hypothese bei n=2): Trade #41 lag mit TP1/ATR ≈1,4-1,5× klar konservativ innerhalb der Schwelle und verfehlte TP1 trotzdem nur um ~7 Punkte (MFE), während Trade #40 als Grenzfall (~1,8-2,0×) den SL traf, ohne sich TP1 überhaupt anzunähern — spricht eher dafür, dass nicht die TP-Distanz der limitierende Faktor war, sondern das separat diagnostizierte Chop-Regime (siehe [[trades/trading_2026-08-20]]), das gerichtete Bewegungen unabhängig von der Zieldistanz abwürgt. Die MFE-Protokollierung ab jetzt soll genau diese Unterscheidung (Distanz-Problem vs. Regime-Problem) bei der Review nach 10-15 Fällen möglich machen.

### 8d. Markt-Regime-Gate — VOR der Setup-Suche prüfen (ergänzt 02.07.2026)
Bevor überhaupt aktiv nach Setups gesucht wird (nicht erst wenn schon ein Setup entsteht), einen groben Regime-Check durchführen:

1. **Heutige Tagesrange vs. Referenz:** Ist die bisherige Tagesrange (Hoch-Tief) ungewöhnlich groß (z.B. Faustregel: >2× der durchschnittlichen Tagesrange der letzten Tage)?
2. **Häufung von Makro-Überraschungen:** Gab es heute oder in den letzten 1-2 Tagen mehrere große Datenüberraschungen (Konsens-Abweichungen), nicht nur eine?
3. **VIX-Bewegung:** Hat VIX heute bereits eine deutliche Bewegung gezeigt (>5% Intraday-Range), auch wenn er aktuell wieder ruhig aussieht?

**Ergebnis:** Wenn 2 von 3 Kriterien zutreffen → "Schock-Regime" aktiv. Konsequenz: entweder Positionsgröße spürbar reduzieren (Richtung unteres Phasen-Limit) UND SL-Multiplikator erhöhen (siehe 8c Schock-Tag-Multiplikator), ODER an besonders extremen Tagen (alle 3 Kriterien erfüllt) das Trading für den Rest des Tages ganz pausieren, auch ohne dass die Cooldown-Regel (2 Verluste) technisch schon ausgelöst wurde.

**Why:** Am 02.07.2026 zeigte sich, dass selbst technisch saubere Setups (Trade #12: Double-Bottom, zweistufiger Trigger, MACD-Crossover) in einem durch mehrere Makro-Schocks geprägten Marktregime scheiterten. Die Lehre: das Regime selbst VOR der Setup-Suche einschätzen, nicht nur reaktiv auf Chop innerhalb eines einzelnen Setups reagieren.

**Symmetrisches Gegenstück — Lehrbuch-Chop-Regime, Ergebnis klar als "kein Setup möglich" benennen (ergänzt 16.07.2026, User-Vorschlag nach Trade #21, präzisiert nach User-Korrektur: NICHT "Suche pausieren"):** Die bestehende Chop-Erkennung (siehe oben) sagt "bei erkanntem Chop Position halbieren oder Trade auslassen" — bezogen auf einen einzelnen Setup-Versuch. Zeigt aber die GESAMTE Session das Lehrbuch-Muster (Netto-Bewegung ~0% über mehrere Stunden trotz großer Zwischen-Range — Beispiel 16.07.2026: 13:00-16:00 Uhr Netto +0,01% bei 269 Pkt Range), ist das eine Regime-Aussage, keine Einzeltrade-Aussage: **Loop, Voll-Checks und Quick-Ticks laufen unverändert weiter wie in ruhiger Phase** (kein Beobachtungs-Abbau) — nur das Ergebnis jeder Prüfung wird klar und aktiv als "unter diesen Bedingungen ist kein valides Setup möglich" benannt, statt weiter auf Kerzenebene so zu tun, als würde nach einem Trigger gesucht. Es gibt in einem echten Chop-Regime schlicht kein Setup, das die Basis-Regeln (8b RR, 8c Rauschen) gleichzeitig erfüllen kann.

**Ausnahme — Makro-/Geopolitischer Katalysator:** Beim Voll-Check (Punkt 9, inkl. Tweet-Fetch alle 10 Min) weiterhin aktiv auf Schlagzeilen prüfen, die das Chop-Regime brechen könnten (Fed-Kommentar, Geopolitik-Eskalation/-Deeskalation, überraschende Makrodaten außerhalb des Kalenders) — taucht so ein Katalysator auf, das Regime sofort neu bewerten.

**How to apply:** Bei jedem Voll-Check die Netto-Bewegung der letzten 2-3 Stunden gegen die Zwischen-Range gegenrechnen (frisch per `data_get_ohlcv`, nicht aus dem Gedächtnis — Zitierpflicht). Fällt die Netto-Bewegung deutlich kleiner aus als die Range (Faustregel: Netto <10% der Range), das Ergebnis aktiv als "Lehrbuch-Chop, kein Setup möglich" aussprechen — Beobachtung und Loop-Cadence bleiben voll aktiv, nur die Erwartungshaltung ist ehrlich auf "aktuell kein Trigger zu erwarten" statt einer aktiven Trigger-Jagd auf jeder Kerze.

**Klarstellung (User-Formulierung, 16.07.2026):** Die Suche nach einem Trade läuft die GANZE Zeit ununterbrochen — Chop ist eine Wahrnehmung/Schlussfolgerung, die WÄHREND dieser laufenden Suche entsteht, kein Grund, die Beobachtung zu unterbrechen ("die Augen zuzumachen"). Die Lage kann sich jederzeit sofort ändern (News, Struktur-Bruch) — deshalb bleibt jeder Check so gründlich wie in der ruhigen Phase, auch wenn das wahrscheinliche Ergebnis "kein Setup" ist.

**Rangfolge Auslassen vs. Halbieren bei bestätigtem Regime-Chop (ergänzt 20.08.2026, Fable-Review, Levi-Entscheidung):** Punkt 7 stellt "Position halbieren oder Trade auslassen" bislang gleichrangig nebeneinander. Sobald aber das Regime SELBST als Lehrbuch-Chop bestätigt ist (diese Session-Ebene, nicht nur ein einzelnes wackelndes Setup nach Punkt 7), gilt ab jetzt eine Rangfolge: **"Trade auslassen" ist die Standard-Reaktion.** "Halbieren" bleibt ausschließlich für den Fall reserviert, dass das Tages-Regime insgesamt noch NICHT als Chop bestätigt ist, aber ein einzelnes Setup unsicher wirkt.

**Ausnahme — volle Gate-Kette bleibt handelbar:** Diese Standard-Reaktion gilt NICHT, wenn ein konkretes Setup trotz bestätigtem Regime-Chop Dual-Gate (7b) + RR-Gate (8b) + TP-Realismus (8b1) alle drei ohne jeden Kompromiss als PASS erfüllt. Ein Setup, das diese volle Kette sauber besteht, zeigt in der Praxis meist auch keine aktive Chop-Symptomatik aus Punkt 7 (RSI/MACD-Flip + flacher VIX) mehr — das ist aber keine automatische Garantie, da Dual-Gate (7b) EMA50-Ausrichtung + Kerzenschluss/Muster prüft, nicht RSI/MACD direkt. Zeigt sich trotz PASS an allen drei Gates gleichzeitig ein aktives RSI/MACD-Flip nach Punkt 7, zählt das als Kompromiss — die Ausnahme greift dann NICHT, Auslassen bleibt Standard. Regime-Chop (Session-Ebene) und ein einzelnes, vollständig bestätigtes Setup schließen sich sonst nicht zwangsläufig aus; ein pauschales Auslassen würde in diesem Fall echte, regelkonforme Trades blockieren statt vor Rauschen zu schützen.

**Pflicht-Ausgabezeile bei bestätigtem Regime-Chop, sobald die Ausnahme geprüft wird (ergänzt 20.08.2026, gleiche Behandlung wie die bestehenden Pflichtzeilen aus 7b1/8b1/8c):**
`Regime-Chop-Ausnahme: Dual-Gate ✓ + RR-Gate ✓ + TP-Realismus ✓ + kein RSI/MACD-Flip ✓ — ✓/✗ (nur bei bestätigtem Regime-Chop relevant)`
Liegt bei bestätigtem Regime-Chop (Session-Ebene) für ein konkretes Setup KEINE vollständige Zeile mit allen vier ✓ vor, gilt automatisch "Auslassen" — kein Trade ohne diese Zeile, unabhängig davon, wie überzeugend das Setup sonst wirkt.

**Why:** Levi beobachtete nach einer schwachen Woche (siehe [[trades/trading_2026-08-20]]), dass die FOMO-Falle nicht in der Positionsgröße liegt, sondern im Einstieg selbst, solange der Markt strukturell keine sauberen Signale liefert — eine halbierte Position in einem strukturell chop-artigen Regime verliert seltener groß, aber öfter, der Erwartungswert bleibt schlecht. Fables Gegenargument (bestätigt an Trade #40/#41 vom selben Tag, beide regelkonform trotz diagnostiziertem Chop-Tag) verhinderte eine zu pauschale Fassung: Die Ausnahme für die volle Gate-Kette stellt sicher, dass echte Ausbrüche wie #40/#41 weiterhin zulässig bleiben. Fable stufte dies als risikoreduzierende Priorisierung bestehender Reaktionen ein (kein neues Analyse-Signal), deshalb reicht lockere Beobachtung über die nächsten Anwendungsfälle, kein hartes n=10-Validierungsgate wie bei neu eingeführten Indikatoren/Schwellenwerten.

**Pflicht-Ausgabezeile bei bestätigtem Lehrbuch-Chop (ergänzt 30.07.2026, Fable-Tagesabschluss-Review nach 30.07.2026):** Am 30.07.2026 wurde "Lehrbuch-Chop" beim ersten Erkennen (~17:08) einmalig explizit benannt, danach über ~45 Minuten nur noch mit dem generischen "kein Setup, weiter Beobachtung" wiederholt — strukturell dasselbe Muster wie der bereits diagnostizierte Prozessfehler vom 27.07. (stille Wiederholung ohne sichtbaren Beleg, siehe [[feedback_prozessfehler_27_07_fuer_fable]] und [[feedback_live_trading]] Punkt 9 "Struktureller Anti-Drift-Fix"), nur diesmal beim Regime-Status statt beim Tweet-Check. **Fix (derselbe Mechanismus, auf einen dritten Fall angewendet):** Solange ein Voll-Check innerhalb einer bereits erkannten Lehrbuch-Chop-Phase läuft, gehört eine Pflicht-Zeile in den Output: `Regime: Lehrbuch-Chop bestätigt (Netto X% bei Y Pkt Range)` — bei jedem einzelnen Voll-Check, nicht nur beim ersten Erkennen. Fehlt diese Zeile während einer laufenden Chop-Phase, gilt der Voll-Check als nicht vollständig, genau wie bei den bereits bestehenden Pflicht-Zeilen (1H-MTF-Schritt, Tweet-Check, Format).

**Zusätzlicher Faktor Nr. 4 — letzter Handelstag vor Wochenende/Feiertag, bidirektional (ergänzt 02.07.2026, präzisiert nach User-Feedback):** Der letzte Handelstag vor einer mehrtägigen Schließzeit (normal Freitag, oder Donnerstag falls Freitag Feiertag ist — dann gilt der Risk-Modus schon am Donnerstag, weil am Freitag nicht mehr reagiert werden kann) zeigt verstärktes Positions-Management-Verhalten. **Wichtig: das ist NICHT automatisch bärisch — die Richtung folgt der vorherrschenden Nachrichtenlage:**

- **Bei schlechten/unsicheren News (geopolitische Spannungen, negative Makro-Überraschung):** Risk-Off verstärkt sich Richtung Wochenende — Trader wollen Risiko nicht über die Pause halten → zusätzlicher Verkaufsdruck
- **Bei guten News/positivem Momentum:** Gegenteiliger Effekt möglich — Trader wollen Gewinnpositionen nicht vorzeitig verkaufen und nehmen das Risiko übers Wochenende bewusst in Kauf (FOMO/Halten) → Rally kann sich ebenfalls verstärken statt abzuflachen

**Praktische Konsequenz:** Nicht pauschal "vorsichtiger am letzten Handelstag" — sondern **die aktuelle Nachrichten-/Sentiment-Richtung als Verstärkungsfaktor einpreisen**. Ist der Tag schon bärisch (wie am 02.07. durch Zins-Repricing) → erwarten, dass sich das Richtung Handelsschluss noch verstärkt, nicht abschwächt. Ist der Tag bullisch → ebenfalls mit Verstärkung statt Abflachung rechnen. Bewegungen am letzten Handelstag vor einer Pause tendenziell größer/impulsiver einschätzen als an einem normalen Tag, unabhängig von der Richtung — Positionsgrößen und SL-Abstände entsprechend anpassen (siehe Schock-Tag-Multiplikator in 8c).

**Why:** User hat klargestellt, dass eine reine "Freitags-Vorsicht"-Regel zu einseitig wäre — an guten News-Tagen wollen Trader Positionen im Gewinn eher NICHT vor dem Wochenende schließen, was Long-Momentum sogar verstärken kann statt zu bremsen. Die Regel muss die Richtung der Verstärkung von der aktuellen Marktlage ableiten, nicht pauschal Risk-Off annehmen.

### 8e. Widerstandstest + überkauft = zusätzliches Teilgewinn-Signal (ergänzt 14.07.2026, Trade #18)
Unabhängig vom regulären TP1/TP2-Teilverkauf-Standard (Punkt 8b): Wenn der Kurs während einer offenen Position eine bekannte Widerstandszone testet (BB-Oberband, altes Session-Hoch, berechnetes Pivot-Level siehe 7a1a) **UND gleichzeitig der 1H-RSI nahe überkauft ist (>65-70)**, aktiv einen Teilgewinn (25-50% der Restposition) vorschlagen — auch wenn kein TP-Level erreicht ist und kein hartes Reversal-/Exit-Kriterium (Double-Top, EMA50-Bruch, Divergenz) formal ausgelöst hat.

**Why:** Bei Trade #18 (14.07.2026) testete der Kurs bei 29.692 exakt die Widerstandszone, während der 1H-RSI bei 69,6 nahe überkauft stand — beides war zum Zeitpunkt bereits bekannt/beobachtet, wurde aber nur als Kontext vermerkt, nicht in eine Handlungsregel übersetzt. Der Kurs fiel danach über ~20 Minuten kontinuierlich zurück bis zum nachgezogenen SL, wodurch ein Großteil des Peak-Gewinns (~2%) wieder abgegeben wurde, bevor der reguläre SL griff. Keine der bestehenden Exit-Regeln (9d1/4a Umkehrmuster, Punkt 11 Reversal-Kriterien) hatte am Peak selbst schon ausgelöst — diese Lücke schließt die neue Regel.

**How to apply:** Bei jedem Voll-Check während einer offenen Position explizit prüfen: (1) testet der Kurs gerade eine bekannte Widerstandszone? UND (2) ist der 1H-RSI >65-70? Wenn beides zutrifft, das als eigenständiges Teilgewinn-Signal aussprechen (z.B. "Widerstandstest + überkauft — Vorschlag: 30% Teilgewinn sichern"), unabhängig davon ob TP1 technisch schon erreicht ist. Ergänzt, ersetzt aber nicht die bestehenden TP1/TP2- und Reversal-Regeln.

**Deckelung — Rest bewusst weiterlaufen lassen, wenn der Trend intakt bleibt (ergänzt 23.07.2026, Fable-Review + Levi-Entscheidung):** Ein Teilexit über diese Regel bleibt **auf 25-50% begrenzt, NIE Vollexit**, solange (a) der übergeordnete 1H/15min-Bias (Punkt 8) weiterhin klar in Positionsrichtung zeigt UND (b) keines der vier Punkt-11-Reversal-Kriterien ([[feedback_live_trading]]) erfüllt ist. Ein Vollexit vor TP2 ist unter diesen Bedingungen nur über Punkt 11 selbst (echtes Reversal, 2-3 von 4 Kriterien) oder ein hartes 9d1/4a-Muster+Kerze-Signal möglich, nicht über 8e allein. **Why:** Fable-Audit 23.07.2026 stellte fest, dass alle proaktiven Exit-Regeln (8e, 9d1/4a, 11, Punkt 12) einseitig Richtung "früher raus" ziehen, ohne dass irgendwo aktiv ausgesprochen wurde, wann der Rest bewusst weiterlaufen soll — das bremst das realisierte RR (siehe [[feedback_realisiertes_rr]]), obwohl 8e selbst schon immer nur "Teilgewinn" verlangte, nie Vollexit. Diese Ergänzung macht das nur explizit, ändert die bestehende Logik nicht.

### 9. Risiko-Reward vor jedem Trade
- Minimum **1:2** (riskiere 1, gewinne 2)
- Besser **1:3** (30 Punkte SL → 90 Punkte TP)
- Niemals Trade ohne klares SL und TP vor dem Entry

### 9b. Divergenz proaktiv zu Trendwechsel nutzen (Lernpunkt 23.06.2026)
Wenn nach einem erfolgreichen Long-Trade eine bärische MACD-Divergenz erkannt wird (Kurs hält Hochs, Histogram fällt kontinuierlich), nicht nur als "noch nicht nachkaufen"-Warnung behandeln, sondern aktiv vorschlagen auf Short-Beobachtung umzuschalten — nicht erst warten bis User explizit danach fragt.

**Why:** Am 23.06.2026 wurde die Divergenz live erkannt und korrekt als Grund genannt, einen Long-Reentry nicht zu bestätigen — aber die Konsequenz ("das könnte sich zu einem Short-Setup entwickeln") wurde nicht proaktiv gezogen, erst auf Nachfrage des Users.

**How to apply:** Bei erkannter Divergenz nach einem Move explizit beide Richtungen ansprechen: "Long hier nicht bestätigt wegen Divergenz X — falls sich das zu einem Bruch entwickelt, beobachte ich auch Short-Levels mit."

**Explizit bestätigt 23.06.2026:** User will aktiv und unaufgefordert informiert werden, sobald sich die Grundlagen für ein Setup ändern (z.B. RR kippt von <1:2 auf >1:2 weil Kerzen-Range sich beruhigt und engerer SL möglich wird) — nicht erst auf Nachfrage. Gilt für beide Richtungen: Setup wird möglich UND Setup wird invalidiert, jeweils mit Begründung warum (welche Kennzahl sich geändert hat).

### 9c. Mehrfach-Kriterien immer explizit als "noch nicht erfüllt" benennen (Lernpunkt 01.07.2026)
Wenn ein Setup mehrere gleichzeitige Bedingungen hat (z.B. "Kerze schließt in Zone X UND RSI über 50"), reicht es nicht, nur die Preiszone zu nennen — der User kann das als alleiniges Kriterium missverstehen und zu früh selbst traden.

**Why:** Am 01.07.2026 wurde eine Preiszone (29.990-30.000) als Reclaim-Ziel genannt, RSI war aber noch bei 48,64 (nicht über 50). User hat beim Erreichen der Zone sofort gekauft, ohne auf die RSI-Bestätigung zu warten — Entry kam einen Tick zu früh, direkt bevor das Momentum kurz wieder abkühlte.

**How to apply:** Bei jedem Setup mit mehreren UND-Bedingungen bei jedem Update explizit sagen, welche Teile schon erfüllt sind und welche noch fehlen (z.B. "Preis ist in der Zone ✓, RSI noch bei 48 — noch nicht auslösen"), statt nur den erreichten Teilaspekt zu bestätigen.

### 9d. Chartmuster (Patterns) — aktiver Pflicht-Check bei JEDEM Setup, nicht nur an News-Tagen (ergänzt 02.07.2026, präzisiert nach User-Feedback)
Bei JEDEM Setup und JEDEM Trigger aktiv auf klassische Chartmuster prüfen, nicht nur auf Doppelkriterium (Preiszone + RSI/MACD) verlassen — siehe Trade #10. Gilt immer, unabhängig davon ob News-Schock-Tag oder normaler Tag (siehe 9e).

**Zwei Funktionen der Muster-Checks — nicht nur Risiko-Filter, auch Chancen-Erkennung:**
1. **Warnung/Risiko:** Bärisches Muster (Double Top, Head & Shoulders, Rising Wedge etc.) gegen die eigene Long-These → Vorsicht/Auslassen
2. **Aktives Entry-Signal:** Bullisches Muster (Double Bottom, Inverse H&S, Falling Wedge, Bullish Flag etc.) das gerade abschließt → das selbst ist ein Grund, einen Long aktiv zu suchen/vorzuschlagen, nicht nur passiv auf Doppelkriterium zu warten

**Why:** User hat klargestellt, dass die Muster-Checks nicht nur zum Bremsen da sind, sondern genauso zum proaktiven Erkennen von Einstiegspunkten (z.B. ein sich formender Double Bottom als eigenständiges Long-Signal).

**Umkehrmuster (Reversal):**
- Double Top / Double Bottom — zwei Hochs/Tiefs auf nahezu gleichem Level (±0,1-0,2%)
- Head and Shoulders / Inverse Head and Shoulders

**Fortsetzungsmuster (Continuation):**
- Bullish/Bearish Flag, Bullish/Bearish Pennant
- Bullish/Bearish Rectangle
- Ascending/Descending Channel

**Dreiecke & Keile:**
- Ascending/Descending/Symmetrical Triangle
- Rising Wedge (bärisch) / Falling Wedge (bullisch)

**Konkreter Check vor jedem Trigger:** Wurde die aktuelle Ziel-/Trigger-Zone bereits **im selben Trading-Tag** getestet? Wenn ja: auf welchem Level genau (±20-30 Punkte NAS100), und wie wurde sie damals abgelehnt/gehalten? Ein zweiter Test derselben Zone am selben Tag = potenzielles Double-Top/-Bottom, auch wenn RSI/MACD für eine Fortsetzung sprechen.

**Why:** Am 02.07.2026 (Trade #10) war das Doppelkriterium technisch sauber erfüllt (RSI>50, MACD-Crossover), trotzdem kam ein scharfer Reversal — im Nachhinein zeigte sich ein Double-Top bei fast identischem Level (30.057 vorbörslich, 30.042 beim Trigger, nur ~15 Pkt Unterschied), das vor dem Entry nicht aktiv geprüft wurde.

**How to apply:** Bei jedem Setup-Update zusätzlich zum Doppelkriterium-Status explizit sagen, ob die Zone bereits früher am Tag getestet wurde und welches Muster ggf. vorliegt (z.B. "Preis+RSI+MACD erfüllt ✓, ABER: zweiter Test derselben Zone (30.040-30.060) heute — Double-Top-Risiko, Vorsicht").

**9d1. Gilt auch WÄHREND einer offenen Position, nicht nur vor dem Entry — die GESAMTE Musterliste, nicht nur Double-Top/-Bottom (ergänzt 03.07.2026, präzisiert nach User-Bestätigung).** Der Muster-Check ist keine reine Entry-Gate-Prüfung, sondern eine laufende Beobachtung, solange eine Position offen ist. Das gilt für alle drei Kategorien aus 9d, nicht nur für den Double-Top-Fall:
- **Umkehrmuster** (Double Top/Bottom, H&S/Inverse H&S) gegen die eigene Position, oft an einer bereits früher am Tag/vorbörslich getesteten Zone mit Ablehnungskerze → aktives Exit-Signal
- **Fortsetzungsmuster** (Flag, Pennant, Rectangle, Channel) in Positionsrichtung, das gerade sauber abschließt → spricht für Halten/Nachziehen des SL statt vorzeitigem Exit
- **Dreiecke & Keile** — z.B. ein sich gegen die Position bildender Keil (Rising Wedge bei Long, Falling Wedge bei Short) → Warnsignal, auch ohne dass ein exaktes altes Level erneut getestet wird

Sobald eines dieser Muster erkennbar wird, unabhängig davon ob das TP schon erreicht ist, aktiv melden — nicht erst wenn SL oder TP technisch ausgelöst wird.

**Why:** Bei Trade #10 (02.07.2026) lief der Kurs nach Entry bei 30.018 bis auf 30.042 — nur 15 Pkt unter dem vorbörslichen Hoch 30.057, mit einer sichtbaren Ablehnungskerze, während die Position bereits im Plus war (~+24 Pkt). Es wurde auf das ferne TP bei 30.112 gewartet statt bei der Ablehnung zu sichern — der Kurs drehte danach vollständig und der Trade endete mit SL bei -15,60€ statt mit einem kleinen Gewinn. Der 9d-Check wurde damals nur vor dem Entry gedacht, nicht als laufende Beobachtung einer offenen Position angewendet. User hat am 03.07.2026 explizit bestätigt, dass das für ALLE Chartmuster aus 9d gelten soll, nicht nur für den Double-Top-Spezialfall.

**How to apply:** Bei jedem Kurs-Update einer offenen Position aktiv die gesamte 9d-Musterliste im Kopf behalten — nicht nur auf Preiszone/RSI/MACD schauen. Erkennbares Muster gegen die Position (auch ohne exakten alten Level-Retest, z.B. ein sich neu bildender Keil) → proaktiv Teil-/Vollgewinnmitnahme oder SL-Nachzug vorschlagen, statt stur auf das ursprüngliche TP zu warten. Erkennbares Fortsetzungsmuster in Positionsrichtung → spricht für Halten. Gilt in beide Richtungen (Long UND Short).

**Muss auf 5-Min-Basis laufen, nicht nur "bei jedem größeren Update" (verifiziert 03.07.2026 mit echten Chart-Bars, siehe [[feedback_backtest_ergebnis_2026-07-03]]).** Bei Trade #10 zeigen die echten 15-Min-Bars, dass Entry (30.018), Ablehnung am alten Hoch (30.042) UND SL-Bruch (unter 29.970) innerhalb EINER einzigen 15-Minuten-Kerze abliefen. Auf 15-Min-Auflösung beobachtet, wäre keine Reaktionszeit zwischen Ablehnung und SL-Bruch geblieben. **Konsequenz:** 9d1/4a-Überwachung einer offenen Position muss auf der tatsächlichen Trading-Auflösung laufen (5min, siehe [[feedback_chart_layout]]), nicht nur bei gröberen Kerzen-Updates — an volatilen Tagen kann sich ein kompletter Entry-Ablehnung-SL-Zyklus schneller abspielen, als eine grobe Beobachtung mitbekommt.

### 9e. News-Schock-Tage — bestehende Checks verschärfen, nicht pauschal bremsen (ergänzt 02.07.2026)
An Tagen mit großen Makro-Überraschungen (z.B. NFP/ADP/ISM-Miss oder -Beat weit vom Konsens entfernt) KEINE pauschale Extra-Vorsicht-Regel, die alle Trades verlangsamt — das würde auch saubere, eindeutige Trends blockieren.

**Stattdessen:** Chop-Erkennung (Punkt 7) und Double-Top/Bottom-Check (Punkt 9d) an News-Schock-Tagen besonders konsequent und explizit anwenden, bevor getriggert wird — das sind die tatsächlichen Mechanismen, nicht der News-Tag selbst.

**Why:** Alle 3 Verlust-Trades der Phase 1 (T8, T9, T10) fielen auf Makro-Überraschungs-Tage (ADP+ISM-Miss 01.07., NFP-Miss 02.07.) — aber die eigentliche Ursache war jeweils Chop bzw. ein übersehenes Double-Top, nicht der News-Tag an sich. User hat explizit korrigiert: eine pauschale Bremse würde auch eindeutige Long/Short-Trends an guten News-Tagen unnötig verhindern.

**How to apply:** Ist die Struktur an einem News-Schock-Tag sauber (kein Double-Top/-Bottom, kein Chop-Muster) → normal traden. Zeigt sich eines der beiden Muster → auslassen oder Position halbieren, genau wie an jedem anderen Tag auch — nur die Wahrscheinlichkeit, dass diese Muster auftreten, ist an News-Schock-Tagen höher, daher besonders genau hinschauen.

**Vorrang-Klausel (ergänzt 04.07.2026, Fable-5-Review — löst Widerspruch zu 8d auf):** 9e und 8d ([[project_risikomanagement]]) widersprachen sich scheinbar: 9e sagt "keine pauschale Extra-Vorsicht" bei sauberer Struktur, 8d sagt "Schock-Regime → Position reduzieren/pausieren". Klarstellung: **8d ist eine Kapital-/Sizing-Regel und gilt IMMER, sobald ihre Kriterien (Tagesrange, Makro-Häufung, VIX-Bewegung) erfüllt sind — unabhängig davon, wie sauber die Chartstruktur aussieht.** 9e bezieht sich NUR auf die Muster-Wachsamkeit (Chop-/Double-Top-Check), nicht auf Positionsgröße — sie erlaubt, bei sauberer Struktur trotzdem zu traden, aber NICHT in normaler Positionsgröße, wenn 8d bereits "Schock-Regime" ausgelöst hat. Bei Konflikt gewinnt 8d (Sizing/Pause), 9e entscheidet nur noch über "traden oder auslassen", nicht über die Größe.

### 10. Marktkontext
- **VIX > 20 → kleinere Position** (Positionsgröße Richtung unteres Phasen-Limit, siehe [[project_risikomanagement]] Hebel-Staffelung)
- **SL-Abstand bei VIX > 20 NICHT automatisch enger** — korrigiert nach Fable-Review 13.07.2026, löst alten Widerspruch zu Punkt 8c auf: Ein engerer SL bei erhöhter Vola erhöht das Risiko, durch normales Rauschen ausgestoppt zu werden (siehe Trade #17-Lehre in Punkt 7a). Für den SL-Abstand gilt stattdessen Punkt 8c (Schock-Tag-Multiplikator, VIX/Rauschen-basiert — je höher die Rausch-Kerzen, desto weiter der SL, nicht enger). "VIX hoch" senkt also die Positionsgröße, nicht den SL-Abstand.
- News-Events (Fed, CPI) → kein Entry kurz davor
- ~~Freitag nach 17:00 → kein neuer Trade~~ **Gestrichen (18.07.2026, User-Korrektur nach hypothetischem Backtest des 17.07.2026-Fensters).** Ein fixer Uhrzeit-Cutoff ist nicht zielführend — der US-Markt hat um 17:00 DE-Zeit erst 1,5 Stunden offen (Open 15:30, siehe [[feedback_trading_zeitfenster]]), eine pauschale Uhrzeitgrenze blendet das aus und widersprach ohnehin schon der differenzierteren Regel in Punkt 8d ("letzter Handelstag vor Wochenende", die explizit NICHT pauschal, sondern nach Sentiment/Regime bewertet). **Ersetzt durch:** Freitagnachmittags-/abends-Trades individuell bewerten — Setup-Qualität, Markt-Regime-Gate (8d), RR-Check (8b/8c) — keine automatische Zeit-Blockade mehr. Bei aktivem Schock-Regime (8d, 2-3 von 3 Kriterien) greift ohnehin schon die dortige Sizing-Reduktion/Pause-Option, das deckt den ursprünglichen Schutzzweck ab, ohne saubere Setups an ruhigen Freitagen pauschal zu verhindern.
- Montag vor 10:00 → abwarten wie Markt eröffnet
