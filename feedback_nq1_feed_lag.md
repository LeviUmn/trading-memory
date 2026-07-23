---
name: feedback-nq1-feed-lag
description: "GELÖST 15.07.2026 (Wechsel auf QQQ) — NQ1!-Pane (Futures) lief gegenüber NAS100-Pane (CFD) konstant ~10 Min hinterher (Delayed-Feed-Lizenz), Dual-Gate-Checks liefen mit veralteten Daten"
metadata:
  type: feedback
  originSessionId: session-2026-07-15
---

**STATUS: GELÖST durch Symbolwechsel, 15.07.2026.** Pane 1 wurde von `CME_MINI_DL:NQ1!` auf `BATS:QQQ` umgestellt (Cboe-One-Echtzeitfeed, Zeitstempel-Gleichlauf mit NAS100 am 15.07.2026 verifiziert). Ursache des Lags war die Delayed-Feed-Lizenz (`_DL`-Suffix, strukturell 10 Min) — CME-Echtzeitdaten wären nur über den Ultimate-Plan (€71,99/Mon.) verfügbar gewesen, wirtschaftlich nicht sinnvoll. **Diese Datei bleibt als Referenz erhalten für den Fall einer Rückkehr zu NQ1!: ohne Echtzeit-Lizenz gilt der 10-Min-Lag unverändert.** Der unten beschriebene Zeitstempel-Gegencheck lebt verallgemeinert in [[feedback_live_trading]] Punkt 7e weiter — er erkennt jetzt sowohl Feed-Lag als auch geschlossene QQQ-Session (identisches Symptom: alter Bar-Zeitstempel).

---

## Ursprüngliches Problem (Original-Eintrag, 15.07.2026)

Im 2-Pane-Layout (NAS100 = Pane 0, NQ1! = Pane 1, siehe [[feedback_chart_layout]]) läuft der NQ1!-Datenfeed nicht immer synchron zu NAS100. Am 15.07.2026 (~16:58 UTC) fiel auf: während NAS100 (Pane 0) bereits beim Balken mit Zeitstempel 1784134500 stand, zeigte NQ1! (Pane 1) den Balken mit Zeitstempel 1784133900 noch als aktuellsten/live — eine Verzögerung von **2 Kerzen (~10 Minuten)**. User hat das live erkannt ("du siehst das jetzt erst die Spike Kerze kam, wir sind in Nas100 schon 2 Kerzen weiter").

**Why:** Der komplette Dual-Gate-Mechanismus ([[feedback_live_trading]] Punkt 7b) setzt voraus, dass die NQ1!-Werte (VWAP/Volume) den AKTUELLEN Marktzustand zeitgleich mit NAS100 widerspiegeln — als finales Gate kurz vor jedem Entry/Exit. Wenn NQ1! stattdessen ~10 Min alte Daten zeigt, ist die "Bestätigung" in Wahrheit eine Bestätigung gegen die Vergangenheit, nicht gegen die Gegenwart — besonders kritisch in schnellen Spike-Phasen wie beim SL-Auslöser von Trade #19, wo sich der Kurs innerhalb von 1-2 Kerzen um 60+ Punkte bewegte.

**Korrektur (Fable-5-Review, 15.07.2026):** Der Lag ist **konstant exakt 10 Minuten**, nicht variabel — der ursprüngliche Verdacht unten ("scheint nicht konstant zu sein") war falsch. Beweis: Das Symbol `CME_MINI_DL:NQ1!` trägt das `_DL`-Suffix, TradingViews explizite Kennzeichnung für Delayed-Feeds — eine lizenzbedingte Börsenregel (CME-Echtzeitdaten ohne kostenpflichtiges Abo = 10 Min delayed), kein Netzwerk-/Liquiditätsproblem. In ruhigen Phasen fiel der Lag nur nicht auf, weil niemand die Timestamps verglich und 10 Min alte Richtungsdaten in Trendphasen zufällig mit der Gegenwart übereinstimmten.

**How to apply (historisch, nur bei Rückkehr zu NQ1! ohne Echtzeit-Abo relevant):** Bei jedem NQ1!-Check (in Voll-Checks oder vor einem Trigger) den Zeitstempel des zurückgegebenen Balkens (`data_get_ohlcv`/`quote_get` → `time`-Feld) gegen den aktuellen NAS100-Zeitstempel gegenprüfen. Bei Abweichung von mehr als 1 Kerze (5 Min) die NQ1!-Bestätigung explizit als "verzögert, mit Vorsicht zu genießen" kennzeichnen statt sie wie eine Echtzeit-Bestätigung zu behandeln.
