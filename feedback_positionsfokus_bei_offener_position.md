---
name: feedback-positionsfokus-bei-offener-position
description: "Bei offener Position: Reversal-/Musteranalyse muss auf Risikoschutz der laufenden Position ausgerichtet sein, nicht auf Suche nach einem neuen Gegen-Setup"
metadata:
  type: feedback
  originSessionId: session-2026-08-18
  modified: 2026-08-18T14:34:55.969Z
---

Bei einer offenen Position darf die Reversal-/Chartmuster-Prüfung (Punkt 11/9d1 in [[feedback_live_trading]]) nicht wie eine Suche nach einem neuen Gegen-Entry wirken (z.B. "ist das ein Long-Setup?"), sondern muss konsequent als Risikoschutz für die LAUFENDE Position gerahmt sein.

**Konkreter Vorfall (18.08.2026, Trade während NAS100-Short):** Bei einem MACD-H-Cross gegen die Position (5min+15min positiv gedreht, Kurs näherte sich der EMA50) lief die Analyse noch im "wird das ein Reversal, erfüllt es 2-3 von 4 Kriterien"-Modus — technisch korrekt nach der bestehenden Regel, aber vom User als "ihr sucht nach einem Long-Setup statt zu schauen wie unsere Position läuft" empfunden. User verwies auf einen früheren, ähnlichen Fehler, bei dem dadurch der Ausstieg zu spät kam.

**Why:** Bei einer offenen Position zählt in erster Linie: Wie nah ist der Kurs am SL? Wie hoch ist der aktuelle unrealisierte Verlust/Gewinn? Baut sich Gegenmomentum auf? Das reine Abhaken von Reversal-Kriterien wie bei einer neuen Setup-Suche verzögert die Reaktion, weil es auf vollständige Bestätigung wartet statt auf frühzeitigen Risikoschutz zu optimieren.

**How to apply:** Bei jedem Voll-Check/Quick-Tick mit offener Position die Ausgabe explizit an der Position orientieren (Distanz zum SL, aktueller €/%-Stand, Momentumrichtung), nicht an "ist hier ein Trade-Signal entstanden". Die 4 Reversal-Kriterien aus Punkt 11 bleiben als Rahmen gültig, aber die Kommunikation und die Handlungsschwelle sollen proaktiver auf Risikoreduktion (SL-Nachzug enger, Teilausstieg) zielen, sobald sich Gegenmomentum bestätigt zeigt — nicht erst bei vollständiger Musterbestätigung wie bei einem neuen Entry. Ergänzt/präzisiert [[feedback_live_trading]] Punkt 11 für den Fall einer bereits offenen Position.
