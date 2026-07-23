---
name: feedback-verify-dont-cave
description: "User bestätigt: bei Widerspruch zu eigenen Aussagen (z.B. 'ADP kam gerade') immer live nachchecken statt sofort zuzustimmen — auch wenn User selbst widerspricht"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 028ce287-5d0c-400c-bf08-bdc1d4c4670c
---

Wenn der User eine Aussage macht, die den zuvor recherchierten/verifizierten Fakten widerspricht (z.B. "Nein, die ADP-Zahlen kamen jetzt gerade, NAS100 geht doch hoch, schau"), nicht einfach zustimmen oder umschwenken — sondern aktiv mit Live-Daten (Chart, Tweets, News) nachprüfen was tatsächlich passiert ist, und dem User das verifizierte Ergebnis zurückmelden (auch wenn es weder die eigene noch die User-Aussage 1:1 bestätigt).

**Why:** Am 02.07.2026 behauptete der User, ADP-Zahlen seien gerade reingekommen (NAS100-Spike als Beleg). Claude hatte bereits korrekt identifiziert, dass ADP schon am Vortag kam. Statt der User-Aussage zu folgen, wurde der Chart + Twitter-Feed live gecheckt — Ergebnis: es war tatsächlich der NFP-Report (57K vs. 113K Erwartung, riesiger Miss), nicht ADP, der den Spike auslöste. User hat das explizit als gutes Systemverhalten gelobt ("Gut dass du bei deiner Meinung bleibst und selbst checkst, System funktioniert").

**How to apply:** Bei Trading-relevanten Fakten (Makrodaten, Kursbewegungen, Event-Timing) im Zweifel immer verifizieren statt dem Gedächtnis oder der User-Behauptung blind zu vertrauen — unabhängig davon, wer zuerst "Recht" zu haben scheint. Gilt besonders während laufender Marktsessions, wo Fehlinformation zu falschen Trading-Entscheidungen führen kann.

**Gegenbeispiel — Claude selbst lag falsch (02.07.2026):** Claude behauptete, VIX sei während eines heftigen NAS100-Sell-offs "flach geblieben" (kein Angst-Signal), basierend auf einem einzelnen `quote_get`-Snapshot. User widersprach ("VIX ist aber ordentlich mitgekommen, war +1..%"). Beim Nachchecken der tatsächlichen VIX-Chart-Bars (nicht nur eines Snapshots) stellte sich heraus: VIX stieg tatsächlich von ~15,79 auf 16,74 (+5-6%) während des Sell-offs — User hatte recht, Claudes Aussage war zu stark vereinfacht.

**Why:** Ein einzelner `quote_get`-Snapshot zeigt nur den aktuellen Stand, keine Bewegung über Zeit. Eine Aussage über "wie sich X während Y verhalten hat" braucht die tatsächliche Kurshistorie (OHLCV-Bars), nicht nur einen Momentaufnahme-Vergleich.

**How to apply:** Bevor eine Aussage über die Bewegung/das Verhalten eines Instruments über einen Zeitraum getroffen wird (nicht nur der aktuelle Stand), die tatsächlichen historischen Bars prüfen (z.B. `chart_set_symbol` auf das Zielsymbol + `data_get_ohlcv` mit passendem `count`, danach zurückwechseln) statt aus 1-2 Snapshot-Vergleichen zu extrapolieren. Gilt in beide Richtungen — sowohl wenn der User widerspricht, als auch wenn Claude selbst eine Verhaltens-Behauptung aufstellt, die geprüft werden könnte.
