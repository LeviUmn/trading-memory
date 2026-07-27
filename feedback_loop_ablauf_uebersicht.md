---
name: feedback-loop-ablauf-uebersicht
description: "Konsolidierte Referenz: was im 1-Min-Loop bei jeder 1-Min-Kerze vs. jeder 5-Min-Kerze passiert — getrennt für 'ohne Position (Setup-Suche)' und 'mit offener Position (Sicherung/Exit)'. Fasst Punkt 3/8/9/9a/11/12 aus feedback_live_trading.md und 8e/9d1 aus feedback_chartanalyse.md zusammen."
metadata:
  node_type: memory
  type: feedback
  originSessionId: session-2026-07-21
  modified: 2026-07-27T18:07:05.455Z
---

Auf User-Wunsch (21.07.2026, nach zwei Fable-Audit-Runden zu Trade #23) als eine zusammenhängende Referenz gespeichert, damit sich künftige Sessions konsistent daran halten — bündelt Regeln, die sonst über mehrere Dateien verteilt sind.

**Why:** Der Fable-Audit vom 21.07.2026 deckte auf, dass Teile des Voll-Check-Pflichtumfangs (Fibonacci, volle 9d-Musterliste) real nicht eingehalten wurden, ohne das offenzulegen. Diese Datei soll als schneller, vollständiger Gegencheck dienen — "steht das hier drin, muss es auch passieren, sonst explizit kennzeichnen, dass es ausgelassen wurde."

**How to apply:** Bei JEDEM 1-Min-Loop (ob Setup-Suche oder offene Position) diese Übersicht als Soll-Zustand heranziehen. Bei Abweichung (ausgelassener Schritt) das im Output aktiv kennzeichnen (siehe [[feedback_live_trading]] Punkt 9, Offenlegungspflicht) — stillschweigendes Auslassen gilt als Regelbruch, unabhängig vom Trade-Ergebnis (siehe [[trades/trade_log]] Regelbruch-Klassifizierung).

---

## Szenario 1: 1-Min-Loop OHNE Position (Setup-Suche)

### Einmalig bei Session-Start (vor dem ersten Loop-Tick, ergänzt 23.07.2026)
1. **Pivot-Level berechnen** (kein Chart-Indikator mehr seit 23.07.2026, siehe [[feedback_chartanalyse]] Punkt 7a1a): `data_get_ohlcv` Tages-Timeframe (letzter abgeschlossener Handelstag) → PP=(H+L+C)/3, R1=2PP-L, S1=2PP-H, R2=PP+(H-L), S2=PP-(H-L). Die 1-2 kursnächsten Level per `draw_shape(horizontal_line)` einzeichnen. Gültig bis zum nächsten Session-Start, kein erneutes Berechnen im Loop selbst.
2. **Zeit-Gate prüfen** (siehe [[feedback_trading_zeitfenster]]): Vorbereitung ab 15:00, aktive Setup-Suche + Entry-Erlaubnis ab 15:30 **mit Pflicht-Halbierung** der Position, volle Größe erst ab 16:00. Bis dahin läuft der Loop unten wie beschrieben, aber jede Positionsgrößen-Berechnung im 15:30-16:00-Fenster nimmt automatisch die halbierte Phasen-Größe. Review-Pflicht nach 5 Anwendungsfällen in diesem Fenster (siehe dortige Notiz in [[feedback_trading_zeitfenster]]).

### Jede Minute (1-Min-Kerze) — Quick-Tick, [[feedback_live_trading]] Punkt 3
1. `Bash date` → echte Minute bestimmen (nie selbst hochzählen, Punkt 9a)
2. Parallel abrufen: `data_get_ohlcv(summary:true)` + `data_get_study_values` auf NAS100 (Pane 0)
3. Gegen den zuletzt "eingefrorenen" Voll-Check-Kontext (Muster/MTF/QQQ) bewerten — keine neue Bilderkennung, kein Timeframe-/Pane-Wechsel
4. **Bei Zonentest (Kurs nähert sich einem bekannten S/R-Level):** taktische Vorwarnkriterien aus [[feedback_live_trading]] Punkt 7c prüfen — MACD-H-Beschleunigung Richtung erwarteter Ausbruchsrichtung, RSI-Annäherung an 50/weg von Extremzone, Bollinger-Band-Squeeze (richtungsneutral, nur "Ausbruch wahrscheinlicher"). Alle drei sind reine Zahlenwerte aus Schritt 2, kein Screenshot/Timeframe-Wechsel nötig — passt in den Quick-Tick-Rahmen. Bei erfüllter Vorwarnung: taktischer Mini-Entry NUR über das Zweistufen-System aus Punkt 8a1 (naher taktischer Trigger, reduzierte Positionsgröße), ersetzt NICHT die volle Bestätigungspflicht für die reguläre Position.
5. **8a1 (Zweistufiges Trigger-System) laufend mitführen, wenn das strukturelle Ziellevel weit entfernt liegt:** naher taktischer Trigger (kleinere Position, engerer SL) vs. fernerer struktureller Trigger (volle Größe) — beide Stufen im Entscheidungsbaum-Kasten (Punkt 7) getrennt ausweisen, nicht nur den fernen Trigger nennen.
6. **Vor jedem tatsächlichen Vollentry-Go-Signal:** Chasing-Check nach Punkt 13 — 4-5 klar gerichtete Kerzenschlüsse ohne frische Bestätigung genau am aktuellen Kurs? Wenn ja: kein Vollentry, nur die bereits halbierte Position (Chasing-Kennzeichnung im Entry-Signal explizit machen), kein zusätzliches separates Konsolidierungs-Add.
7. Ausgabe: kompakter Entscheidungsbaum-Kasten (🟢/🔴/→warten) oder kurzer Status bei "unverändert" — kein Fließtext, keine Tick-Zähler

### Jede 5. Minute (5-Min-Kerze) — Voll-Check, Punkt 9 (korrigiert 21.07.2026)
1. Echte Minute erneut bestätigen
2. **MTF-Wechsel (uneingeschränkt Pflicht):** `chart_set_timeframe(15)` NAS100 → Werte lesen → `chart_set_timeframe(60)` → Werte + Struktur (HH-HL/LH-LL) letzter 5 Bars → zurück auf 5min
3. **QQQ-Dual-Gate:** Zeitstempel-Vergleich NAS100/QQQ-Bar → falls Session-Gate offen (Punkt 7e): `pane_focus(1)`, dort 15min+60min → EMA50/VWAP/Volumen lesen → zurück auf Pane 0
4. **Chartmuster (reduziert, NICHT mehr die volle [[feedback_chartanalyse]] 9d-Liste):** Screenshot + Prüfung auf die 3-4 zum Kontext wahrscheinlichsten Muster (z.B. Double-Top bei Zonentest, Flag/Pennant bei Konsolidierung, Keil bei Trendabschwächung)
5. **Fibonacci:** nur wenn ein erkennbarer Impuls/Trend vorliegt, sonst ausgelassen
6. **Tweet-Fetch-Fälligkeit aus `x_last_fetch.json` berechnen, nicht mental Minute%10 mitzählen** (ergänzt 27.07.2026, Anti-Drift-Fix nach zweimaligem Ausfall — siehe [[feedback_live_trading]] Punkt 9): Zeitstempel lesen, Differenz zur echten Systemzeit bilden. ≥10 Min → Fetch der 3 Accounts (DeItaone/KobeissiLetter/zerohedge) durchführen, Timestamp aktualisieren. Ergebnis IMMER als Pflicht-Zeile im Output (auch "noch nicht fällig" oder "nichts Neues") — nie stillschweigend weglassen, siehe Schritt 9
7. **Offenlegungspflicht:** im Output kurz nennen, was geprüft wurde und was ausgelassen wurde (z.B. "Fibonacci nicht berechnet, kein Impuls")
8. 8d-Regime-Gate (Schock/Chop) neu bewerten
9. **Ausgabe: Vorlage aus [[feedback_vollcheck_format]] Zeile für Zeile kopieren und befüllen** (Entscheidungsbaum mit Dual-Gate-Status, Invalidierungskriterium, was für ein Go-Signal noch fehlt) — zwingend inklusive der beiden Pflicht-Zeilen `Tweet-Check: ...` und `Format: Fließtext ✓` am Ende. Fehlen diese zwei Zeilen, gilt der Voll-Check als NICHT durchgeführt (gleiche Behandlung wie ein fehlender 1H-/QQQ-Schritt, ergänzt 27.07.2026 nach den zwei Prozessfehlern dieses Tages).

**Zusätzliche außerplanmäßige Voll-Check-Trigger** (unabhängig vom 5-Min-Takt): Übergang ruhig→heiß, unmittelbar vor Entry, extreme Zahlen-Bewegung zwischen zwei Kerzen (Punkt 9, Trigger 2-4).

---

## Szenario 2: 1-Min-Loop MIT offener Position (Sicherung + kurzfristiger Exit)

### Jede Minute — Quick-Tick, Punkt 3 + Positions-Kasten
1. Gleiche Zahlen-Sequenz wie oben (Kurs/RSI/MACD-H/EMA50 auf NAS100)
2. **Positions-Kasten bei jedem Update** (Punkt 8): 🔴 SL (mit Zertifikatspreis) / 🟢 TP1/TP2 (mit Zertifikatspreis) — Farbcode siehe [[feedback_positions_farbcode]]
3. Stall-Zähler (Punkt 12) mitzählen — aber: **Exit-Vorschlag erst BEIM tatsächlichen 3. Kerzenschluss selbst**, nie vorgezogen auf Basis der Zwischen-Tick-Zahlen, auch wenn RSI/Kurs schon "reif" aussehen (Timing-Klarstellung nach zweitem Fable-Review)
4. Punkt-11-Kriterien, soweit ohne Screenshot/Timeframe-Wechsel möglich (MACD-H-Vorzeichen, EMA-Bruch), grob im Blick behalten — volle Bestätigung folgt erst beim Voll-Check

### Jede 5. Minute — Voll-Check, gleicher reduzierter Standard wie Szenario 1, PLUS positionsspezifisch:
1. MTF + QQQ-Dual-Gate + Chartmuster (reduziert) + Fibonacci (nur bei Impuls) + Offenlegung — **läuft unverändert weiter bis zum kompletten Exit**, auch nach TP1-Teilverkauf (Verschärfung nach Trade #16)
2. **Punkt 11 (Reversal):** alle 4 Kriterien gegenchecken (MACD-H-Cross+Bestätigung über 2+ Checks, EMA-Bruch+Folgekerze, QQQ-EMA-Bruch, 15min/1H-Beschleunigung) — bei 2-3 erfüllt: aktiver Dreh-/Exit-Vorschlag
3. **[[feedback_chartanalyse]] Punkt 9d1/4a:** Umkehrmuster gegen die Position aktiv SUCHEN (nicht nur bei explizitem User-Hinweis benennen — genau das war der Audit-Befund vom 21.07.2026)
4. **Punkt 8e:** Widerstandstest + 1H-RSI>65-70 → Teilgewinn-Vorschlag (25-50%), auch ohne erreichtes TP1
5. **Punkt 12 (Stall):** zuerst prüfen, ob 9d1 ein Fortsetzungsmuster zeigt (dann halten, Punkt 12 greift NICHT) — sonst bei 3. Kerzenschluss ohne neues Hoch/Tief UND RSI ≥8-10 Punkte vom lokalen Extrem gefallen → Teilgewinn (25-50%) vorschlagen, KEIN Vollexit
6. SL-Nachzug: strukturell begründet (bestätigte höhere Tiefs/Support mit Rausch-Puffer nach Punkt 8c/7a), nicht auf jeden kleinen Rücksetzer

---

**Kernprinzip:** Die 1-Minute-Ebene ist in beiden Szenarien reine Arithmetik (Kurs/RSI/MACD-H/EMA50, plus Positions-Kasten wenn offen) — alles Schwere (Muster, MTF, QQQ, Fibonacci, Reversal-Kriterien) läuft ausschließlich im 5-Minuten-Takt (oder den außerplanmäßigen Triggern), mit ehrlicher Kennzeichnung statt stillschweigender Kürzung.
