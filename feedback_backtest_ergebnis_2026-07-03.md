---
name: feedback-backtest-ergebnis-2026-07-03
description: "Backtest-Ergebnis 03.07.2026 — alle 12 Trades gegen die 5 neuen Regeln (9d Chartmuster, 7 Chop, 8d Regime-Gate, 8c Schock-SL, 8a1 Zweistufiger Trigger) geprüft"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: e444895c-b8eb-4ad0-b48b-0c4adf685f6b
---

**Kontext:** Durchgeführt wie in [[feedback_backtest_ablauf]] geplant, am 03.07.2026 (US-Feiertag). Basis: [[trades/trade_log]] + alle Detail-Dateien, geprüft gegen [[feedback_chartanalyse]].

## Ergebnis pro Regel

**9d Chartmuster-Check — KEEP, stark validiert.** Ursprungsfall Trade #10 (Double-Top 30.057/30.042, nur 15 Pkt Unterschied, damals nicht geprüft) wäre damit direkt erkannt worden. Bei Trade #12 korrekt als gültiges Double-Bottom bestätigt — kein False Positive. Kein Gewinn-Trade (#1,3,4,5,6,7) hätte fälschlich geblockt werden können, keiner zeigt ein Muster gegen die eigene Position.

**7 Chop-Erkennung — KEEP, stark validiert, aber Umsetzungsproblem.** Trade #9 (01.07., -12,96€) ist der Beweisfall: Chop wurde live korrekt erkannt und benannt ("weder Short noch Long sauber bestätigt"), der Short-Trigger kam trotzdem in exakt dieser Phase. Die Regel-Formulierung war schon richtig — das Problem war Nichtbefolgung, nicht fehlende Definition. **Konsequenz:** Sobald Chop nach der Definition erkannt wird, ist das eine Pflicht-Aktion (halbieren/auslassen), keine Beobachtung, die dann trotzdem ignoriert werden darf.

**8d Markt-Regime-Gate — KEEP, aber nur Dämpfer, kein Filter.** Korrekt inaktiv an allen 7 Gewinn-Tagen (12.-30.06., ruhiger Markt). Korrekt aktiv am 01./02.07. (Makro-Häufung ADP-Miss+ISM-Miss+NFP-Miss). Hätte aber keinen der 5 Verluste direkt verhindert — die Ursachen liegen anderswo (früher Entry #8, Chop #9 s.o., Double-Top #10 s.o., Nach-Entry-Chop #12 nicht vor Entry erkennbar). Wert liegt in kleinerer Positionsgröße = kleinerer €-Schaden, nicht in Trade-Verhinderung.

**8c Schock-Tag-SL-Multiplikator — INKONKLUSIV.** Ob ein 2,5-3x weiterer SL bei Trade #8/#10/#12 den jeweiligen Bounce überlebt hätte, lässt sich aus den gespeicherten Zusammenfassungen nicht sicher beantworten — dafür fehlen die genauen Minuten-Bars nach SL-Auslösung. Bei Bedarf gezielt mit `data_get_ohlcv` für die exakten Zeitfenster nachprüfbar, sobald TradingView läuft.

**8a1 Zweistufiger Trigger — Funktioniert wie gedacht, n=1.** Nur bei Trade #12 angewendet. Staging (naher Trigger 29.500 + struktureller Trigger 29.530) lief sauber, der Verlust entstand erst durch Chop NACH dem Trigger — kein Fehler des Trigger-Mechanismus selbst.

**Freitags-/Feiertags-Verstärkungsregel — Zu kleine Stichprobe.** Wurde erst aus dem 02.07. selbst abgeleitet (zirkulär), es gibt noch keinen zweiten Vergleichstag unter den 12 Trades. Weiter beobachten beim nächsten Handelstag vor einer Pause.

## Kernbefund

Kein False Positive gegen die 7 Gewinn-Trades bei irgendeiner der 5 Regeln — sie sind also nicht zu aggressiv kalibriert. Von den 5 Verlusten (#8-12) wären 2 (#9, #10) durch die neuen Regeln direkt verhindert/abgeschwächt worden, WENN sie konsequent angewendet worden wären. Der größte Hebel ist nicht mehr neue Regeln, sondern **Durchsetzung**: Trade #9 zeigt, dass eine korrekt erkannte Warnung trotzdem ignoriert wurde. Regime-Gate (8d) und SL-Multiplikator (8c) sind Positionsgrößen-Dämpfer, keine Trade-Filter — das war vorher nicht klar unterschieden.

**Why:** Nach der Verlustserie #8-12 (siehe [[feedback_regime_wechsel]]) wurden mehrere neue Regeln ergänzt, ohne sie gegen die Historie zu prüfen. Dieser Backtest bestätigt, dass die Regeln inhaltlich richtig sind, aber die eigentliche Schwachstelle Disziplin bei der Umsetzung war (Trade #9), nicht fehlende Checks.

**How to apply:** Bei künftigen Chop-Erkennungen (Punkt 7 in [[feedback_chartanalyse]]) die Halbierung/Auslassung als bindend behandeln, nicht als Beobachtung, die durch ein einzelnes erfülltes Kriterium überstimmt werden darf. 8d/8c weiter als Positionsgrößen-Modifikator nutzen, nicht als alleinigen Entry-Filter erwarten.

## Teil 2 (03.07.2026, später am Tag): Verifikation mit echten TradingView-Chart-Daten

Nachdem 9d1/4a (laufende Muster-/Kerzen-Überwachung während offener Positionen, siehe [[feedback_chartanalyse]]) ergänzt wurden, wurde TradingView gestartet (NAS100/FOREXCOM) und die tatsächlichen historischen Bars für 01.07. und 02.07.2026 gezogen (Auflösung 15min, da der 500-Bar-Cap auf 5min-Basis nicht bis zum 01.07. zurückreicht — bei Bedarf für Tick-genaue Analyse mit 5min nachprüfbar).

**Bug gefunden & gefixt:** `scrollToDate` in `src/core/chart.js` warf `evaluate is not defined` — der DI-Resolve (`const { evaluate } = _resolve(_deps)`) fehlte, im Gegensatz zu allen anderen Funktionen in der Datei. Gefixt (Zeile 161), Workaround für diese Session war Scrollen per `ui_scroll` + `chart_set_timeframe`, da `zoomToBarsRange` ohnehin nur bereits geladene Bars neu zentriert und keine neue Historie nachlädt.

### Trade #10 (02.07., -15,60€) — reale Bars bestätigen den Fall präzise
- 12:30 UTC Bar: Hoch **30057,4** — exakt der vorbörsliche Referenzpunkt aus dem Memory-Eintrag.
- 14:00 UTC Bar (16:00 CET): Open 30006,15 → Hoch **30042,15** (exakt der erinnerte Ablehnungspunkt, nur 15 Pkt unter 30057,4) → Close **29965,25**, Tief bereits **29882,05** — d.h. **Entry, Ablehnung UND SL-Bruch (SL 29970) fanden innerhalb derselben 15-Minuten-Kerze statt.** Der Umschwung war schneller als die bisherige Beschreibung vermuten ließ.
- 14:15 UTC Bar: Tief 29661 — der Kurs war schon massiv unter dem SL, bevor eine reguläre Reaktion überhaupt möglich gewesen wäre, wenn man nur alle 15 Minuten hinschaut.

**Konkrete Antwort auf die Ausgangsfrage ("hätten wir mit Gewinn aussteigen können"):** Ja, aber nur bei Beobachtung auf 5-Min- oder feinerer Basis — bei 15-Min-Auflösung war das Zeitfenster zwischen Ablehnung (30042) und SL-Bruch (29970) so kurz, dass es innerhalb einer einzigen Kerze lag. Das bestätigt: 9d1/4a müssen auf der tatsächlichen Trading-Auflösung (5min, siehe [[feedback_chart_layout]]) laufen, nicht nur bei größeren Kerzen-Updates — der reale Fall war schneller als die ursprüngliche Beschreibung nahelegte.

### Trade #9 (01.07., -12,96€) — Chop real bestätigt
Bars 16:00-17:15 UTC oszillieren durchgehend zwischen ~29.920 und ~29.980 (60 Pkt Range über 5 Kerzen) ohne Trendrichtung — deckt sich exakt mit der erinnerten "whippy"-Einschätzung und bestätigt nochmal die Chop-Diagnose unabhängig vom Gedächtnisprotokoll.

### Trade #11/#12 (02.07.) — Double-Bottom-Zone exakt bestätigt
15:00 UTC Bar Tief 29382,35 und 15:15 UTC Bar Tief 29364,35 — exakt die erinnerten Double-Bottom-Level (29.382/29.364). Zusätzlicher neuer Fund: Die 15:30-UTC-Kerze (vermutlich die Trigger-Kerze für Trade #12) zeigt selbst einen langen oberen Docht (Hoch 29569 vs. Close 29467,25) — ein Ablehnungssignal near dem 29.530-Level, das schon IM Trigger sichtbar war. Auf 15-Min-Basis nicht eindeutig von der eigentlichen Entry-Kerze zu trennen — für eine abschließende Bewertung wären 5-Min-Bars nötig.

**Fazit Teil 2:** Die Erinnerungsprotokolle waren inhaltlich korrekt, aber teils zeitlich ungenau (Trade #10 lief schneller ab als beschrieben). Wichtigste neue Erkenntnis: 9d1/4a-Überwachung darf nicht nur "bei jedem Update" pauschal gemeint sein, sondern muss auf 5-Min-Kerzenbasis laufen, weil ganze Entry-Ablehnung-SL-Zyklen an volatilen Tagen innerhalb einer einzigen 15-Min-Kerze ablaufen können.

## Teil 3 (03.07.2026): 8c (Schock-SL-Multiplikator) mit echten 5-Min-Bars final geklärt

Punkt 4 aus der offenen Liste — bisher "inkonklusiv". Mit den tatsächlichen 5-Min-Bars (per `ui_evaluate` direkt aus dem TradingView-Bar-Buffer gelesen, da `data_get_ohlcv` technisch bedingt immer nur die letzten 500 Bars ab "jetzt" liefert, siehe unten) lässt sich das jetzt trade-genau beantworten.

**Trade #8 (01.07., -20,01€) — 8c hätte sehr wahrscheinlich geholfen.** Entry-Retest ~14:35-14:40 UTC bei 30.001, SL 29.930 brach um 14:45 UTC (Tief 29.874,25). Aber: ab 15:00 UTC erholte sich der Kurs kräftig und erreichte um 15:30 UTC das **Tageshoch 30.091,95** — nur 45 Minuten nach dem SL-Bruch. Ein 2,5-3x weiterer Schock-SL (Regel 8c) hätte bei ca. 29.789-29.824 gelegen, deutlich unter dem tatsächlichen Tief von 29.874 — **der Trade hätte überlebt** und wäre mit hoher Wahrscheinlichkeit ein Gewinn geworden statt -20,01€.

**Trade #10 (02.07., -15,60€) — Korrektur der Teil-2-Einschätzung.** Die 5-Min-Bars zeigen: Entry ~14:00 UTC bei 30.018, Hoch **30.042,15 innerhalb derselben 5-Min-Kerze**, die Kerze schließt aber schon bei 30.008,65 (nahe Breakeven) — die eigentliche Krise (Tief 29.917, SL-Bruch) folgt erst in der NÄCHSTEN 5-Min-Kerze. Das heißt: Selbst 5-Min-Beobachtung hätte den Peak bei 30.042 wahrscheinlich nicht in Echtzeit einfangen können, da der Rückfall auf 30.008 bereits innerhalb derselben Kerze passierte. Realistisch wäre ein Ausstieg nahe Breakeven beim Kerzenschluss (30.008) gewesen, nicht mit vollem (~+24 Pkt) Gewinn wie in Teil 2 zunächst behauptet. Diese Aussage wird hiermit präzisiert.

**Trade #12 (02.07., -11,13€) — 8c hätte NICHT geholfen, 9d1/4a schon.** Neuer Fund: Die 3. Kerze nach Entry (14:20 UTC) macht ein Hoch bei 29.567,65 — nur 2 Punkte unter dem unmittelbar zuvor gemachten Hoch 29.569,05 (Mini-Double-Top) — und schließt scharf ab auf 29.467,25. Ein 9d1/4a-Exit an dieser Kerze hätte den Verlust von -11,13€ auf etwa -4€ reduziert (34 statt 91 Pkt Risiko realisiert). Ein weiterer SL (8c) hätte dagegen NICHT geholfen: der Kurs fiel danach ungebremst weiter bis 29.088 (17:45 UTC), keine Erholung in Sicht — ein wetierer SL hätte den Verlust nur vergrößert.

**Kernbefund Teil 3:** 8c ist trade-abhängig wirksam, kein Automatismus — bei Trade #8 hätte es den Verlust komplett verhindert (Markt erholte sich schnell), bei Trade #12 hätte es ihn nur vergrößert (Markt fiel weiter). Die zuverlässigere Regel bleibt 9d1/4a (Muster+Kerze-Kombination erkennen und aktiv aussteigen), weil sie unabhängig von der Richtung der Fortsetzung funktioniert. Bei Trade #10 war das Reaktionsfenster so kurz (Peak und Rückfall in derselben 5-Min-Kerze), dass keine der beiden Regeln realistisch den vollen Gewinn gesichert hätte — nur eine Verlustbegrenzung nahe Breakeven war real erreichbar.

**Technische Notiz:** `data_get_ohlcv` liest immer die letzten `count` (max. 500) Bars ab dem aktuellen Kerzen-Index — unabhängig davon, wohin der Chart gescrollt ist. Für Bar-Daten, die weiter als 500 Bars in der Vergangenheit liegen (hier: 07-01 auf 5-Min-Basis, da "heute" bereits der 03.07. ist), musste direkt per `ui_evaluate` auf den Bar-Buffer zugegriffen werden. Kein Code-Bug, sondern eine Design-Grenze des Tools — bei Bedarf für zukünftige Backtests relevant.

## Teil 4 (03.07.2026): Hypothetische Gesamtstatistik mit dem Wissen von heute

Rekonstruktion: wie hätten sich die 12 Trades entwickelt, wenn 9d1/4a (laufende Muster-/Kerzen-Überwachung), Regel 7 (bindende Chop-Vermeidung) und die 8c-Erkenntnisse (trade-abhängig) von Anfang an konsequent angewendet worden wären.

| # | Original | Mit heutigem Wissen | Begründung |
|---|---|---|---|
| 1-7 | +58,16€ | +58,16€ (unverändert) | Ruhiger Markt, keine der neuen Regeln hätte gegriffen |
| 8 | -20,01€ | ≈ +12€ | 8c überlebt den Whipsaw (Tief 29.874 > hypothetischer weiterer SL ~29.789-29.824), Exit realistisch beim ersten Pullback nach dem Tageshoch 30.092 (~30.040), nicht am exakten Top |
| 9 | -12,96€ | 0€ | Chop live erkannt — mit bindender Regel 7 wird der Trade konsequent ausgelassen statt trotzdem getriggert |
| 10 | -15,60€ | ≈ -3,25€ | 9d1/4a fängt den Kerzenschluss bei 30.008 ab statt den vollen SL zu reiten — Peak bei 30.042 war aber zu schnell (gleiche 5-Min-Kerze) für vollen Gewinn |
| 11 | -2,68€ | -2,68€ (unverändert) | Bereits sauber gemanagt (Breakeven-SL) |
| 12 | -11,13€ | ≈ -4,16€ | 9d1/4a erkennt die Mini-Double-Top-Kerze (29.567 vs. 29.569, 2 Pkt Unterschied) und steigt dort aus |

**Ergebnis: ≈ +60€ statt tatsächlich -4,22€ (Swing ≈ +64€).** Win-Rate 58% (7 Wins, 2 Breakeven inkl. vermiedenem Trade #9, 3 Verluste) statt 50% (6/1/5), gleiche Zählweise wie im Original-Log.

**Wichtige Einschränkung:** Das ist eine Rekonstruktion mit dem Wissen von heute auf echte Kursdaten angewandt, keine Garantie für tatsächliches Verhalten in Echtzeit — Trade #9 selbst ist der Beweis, dass eine korrekt erkannte Warnung trotzdem ignoriert werden kann (siehe Punkt 7 in [[feedback_chartanalyse]]). Der Wert liegt nicht in der Zahl +60€, sondern in den jetzt konkreten, datengestützten Trigger-Punkten (Kerzenschluss-Level, Double-Top-Distanzen), an denen tatsächlich reagiert werden muss.
