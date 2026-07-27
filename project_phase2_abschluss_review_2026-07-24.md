---
name: project-phase2-abschluss-review-2026-07-24
description: "Fable-Review Phase 2 (Trades #16-25), 24.07.2026 — Go-Entscheidung für Phase 3 mit Bedingungen, korrigierte Kennzahlen"
metadata:
  type: project
  originSessionId: session-2026-07-24
  modified: 2026-07-24T08:42:04.704Z
---

Vollständiges Phasenübergang-Review (Phase 2 → 3), durchgeführt von Fable am 24.07.2026, analog zum Phase-1→2-Review vom 09.07.2026. Siehe [[project_risikomanagement]] und [[project_performance_ziele]] für die laufenden Zahlen.

## Korrigierte Kennzahlen (Fable deckte zwei Rechenfehler auf)
- Phase-2-Gesamtgewinn (#16-25): **+114,24€** (nicht +113,84€, kleiner Rundungsfehler in einer Zwischenrechnung)
- Ø-Rendite Phase 2 (#16-25, n=10): **0,61%** (nicht 0,58%)
- Beide Korrekturen ändern die Kernaussage nicht (Ziel weiterhin verfehlt), wurden aber in [[project_performance_ziele]] nachgezogen.

## Zentraler neuer Befund: Phase-2-eigenes RR deutlich besser als die Lifetime-Zahl zeigt
Blended über alle 25 Trades liegt RR bei 1,27:1. Getrennt nach Phase gerechnet:
- **Phase 1 (isoliert): RR ≈1,18:1**
- **Phase 2 (isoliert): RR ≈1,75:1** — Ø-Gewinn 1,15%/Win, Ø-Verlust 0,66%/Loss

Die RR-Disziplin hat sich innerhalb von Phase 2 real verbessert (kleinere, disziplinierter geführte Verluste), was in der bisherigen Blended-Zahl unsichtbar blieb. Ab Phase 3 wird eine eigene, separat getrackte RR-Zahl geführt (siehe [[feedback_realisiertes_rr]]), nicht nur die laufende Lifetime-Zahl.

## Positiv (Phase 2)
- Win-Rate 70% (7/10) trifft [[project_performance_ziele]] exakt
- Erwartungswert klar positiv, Drawdown minimal (größter Einzelverlust 0,10% des Kapitals)
- RR-Verbesserung real (siehe oben)
- Funktionierender Lernzyklus: 8c (ATR-SL-Pflicht-Gate), 8e (Teilgewinn-Cap 50%), Punkt 13 (Chasing→Halbierung), Stacking-Regel — alles direkte Konsequenzen aus Phase-2-Fehlern (#21, #24)
- Trade #25 als sauberster Trade der Phase (volles Dual-Gate, kein Regelbruch)
- Ehrliche Selbstkorrektur-Kultur (#19 BE→Loss, #21 zweifach korrigiert)

## Negativ / verbesserungswürdig (Phase 2)
- Ø-Rendite 0,61% klar unter Ziel 1-1,5%
- Win-Rate fragil: ohne Trade #25 (+1,96%) wären es nur 66,7% gewesen — n=10 ist keine robuste Basis
- **Regelbruch-Rate real höher als offiziell gezählt:** Fable wertet zusätzlich zu den 4 offiziell gezählten Fällen (#16, #17, #20, #21) auch #22 (Entry in selbst erkannter Chop-Zone), #23 (TP2-RR 1,75:1 statt Pflicht-1:2, nur "offen deklariert" statt korrigiert) und #24 (Chasing) als Grenzfälle — realistisch 5-7 von 10 Trades mit Prozess-Reibung, nicht 4. **Kernkritik:** Offenheit beim Begehen eines Verstoßes (#23) macht den Verstoß nicht ungeschehen — Transparenz wurde bisher mit Regelkonformität verwechselt.
- **Survivorship-Bias unadressiert:** Kein Regelbruch bei einem Gewinn-Trade (#16, #17, #20) hatte je eine Konsequenz jenseits einer Notiz — Risiko, dass Regeln bei größerer Positionsgröße in Phase 3 als "nicht so wichtig" unterlaufen werden.
- 3 von 10 Phase-2-Trades liefen als User-Eigenentries außerhalb des aktiven Loops (#20, #22, #24) — ausgerechnet #24 (Chasing) war einer davon, kein Zufall laut Fable.

## Ehrliche Zieleinschätzung
Win-Rate-Ziel (70%): grenzwertig erreicht, nicht robust bestätigt (hängt an einem einzelnen Trade). Rendite-Ziel (1-1,5%): klar verfehlt (0,61%). Realisiertes RR: gemischt — blended schwach, Phase-2-eigen echte Verbesserung. **Gesamtfazit: auf dem Weg, aber noch nicht da** — echte Fortschritte (RR, Prozessreife), aber zwei ungelöste Strukturprobleme (Rendite-Lücke, folgenlose Regelbrüche bei Gewinnern).

## Go/No-Go-Entscheidung: GO mit Bedingungen (kein uneingeschränktes Go)
Formale Hochstufen-Kriterien komfortabel erfüllt, aber kein voller Sprung auf die maximale Phase-3-Bandbreite (5.000€/2%) ohne Bewährungsfenster. Sechs Bedingungen:
1. **Gestaffelter Einstieg:** erste 3-5 Phase-3-Trades (#26-#30) am unteren Ende der Phase-3-Bandbreite (~4.000€, nicht sofort 5.000€/volles 2%-Risiko), erst danach volle Größe — und nur wenn in diesem Fenster kein schwerer Regelbruch vom #21-Typ auftritt
2. **RR-Minima als echte Hard-Gates:** das #23-Muster (TP2 unter 1:2, nur "offen deklariert") darf in Phase 3 nicht mehr als regelkonform durchgehen — entweder TP2 anpassen oder Position reduzieren, nicht wissentlich unterschreiten
3. **Symmetrische Konsequenz-Regel:** jeder Regelbruch (Gewinn oder Verlust) löst eine verpflichtende Mini-Review vor dem nächsten Trade aus, nicht nur bei Verlust-Trades
4. **User-Eigenentries:** mindestens sofortiges Retro-Check im Loop direkt nach dem Entry, nicht erst beim nächsten geplanten Voll-Check
5. **Phase-3-RR separat tracken**, nicht nur blended (siehe Befund oben)
6. **Besonders beobachten:** ob das 8c-ATR-Mindestdistanz-Gate (erst Mitte Phase 2 eingeführt) unter realem Zeitdruck bei größerer Positionsgröße tatsächlich vor dem RR-Check greift — bislang nur theoretisch gefixt, noch nicht über mehrere Trades bei höherem Risiko live bestätigt

**Why:** Levi wollte vor dem Phase-3-Übergang ein volles, unabhängiges Fable-Review analog zum Phase-1→2-Übergang — explizit mit Positiv/Negativ-Liste, vollständiger Trade-Tabelle und ehrlicher Zieleinschätzung, Fable soll das Go geben.

**How to apply:** Ab Trade #26 gilt der gestaffelte Phase-3-Start (Bedingung 1-6 oben). Bei jeder Positionsgrößen-Entscheidung in den ersten 3-5 Phase-3-Trades die untere Bandbreite (~4.000€) verwenden, nicht automatisch die volle Phase-3-Spanne. Nach diesem Bewährungsfenster erneutes kurzes Review, ob auf volle Phase-3-Größe hochgefahren wird.
