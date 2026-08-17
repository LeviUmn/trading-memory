---
name: project-regelwerk-audit-2026-08-07
description: "Fable-Vollprüfung des Regelwerks (Fehler/Inkonsistenzen + Paid-Upgrade-Kandidaten), 07.08.2026 — 2 offene Risiken, TradingEconomics-API als Top-Kaufempfehlung"
metadata:
  type: project
  originSessionId: session-2026-08-07
  modified: 2026-08-07T11:11:38.403Z
---

Levi hat Fable am 07.08.2026 gebeten, das komplette Regelwerk (alle `feedback_*`/`project_*`-Memories, nicht die Tages-Trade-Logs) auf Fehler/Inkonsistenzen zu prüfen — Anlass: Levi ist jetzt explizit offen für bezahlte TradingView-Add-ons, wenn ein echter Vorteil entsteht ("Ich bin bereit Dinge per Add-on zu buchen, wenn wir daraus einen Vorteil haben").

## Offene Fehler (Stand 07.08., Punkt 1+2 seither behoben)

1. ✅ **BEHOBEN 07.08.2026:** `feedback_dax_trennung.md` referenzierte `feedback_live_trading_dax.md`, die nicht existierte — jetzt geschrieben (siehe [[project_dax_erweiterung]] To-do 11).
2. ✅ **BEHOBEN 07.08.2026, anders als ursprünglich vorgeschlagen:** Statt "start update dich #2" für NAS100 einzuführen, hat Levi eine eindeutigere Lösung gewählt — DAX-Trigger umbenannt zu **"start update dich dax"**, NAS100 bleibt unverändert schlicht **"start update dich"**. Kein "#1"/"#2"-Schema mehr, siehe [[feedback_session_update]] und [[feedback_session_update_dax]].
3. **Stall-Exit-Review (Punkt 12/12.3 in [[feedback_live_trading]]) überfällig, kein Zähler geführt** — mind. 5 relevante Trades seit 21.07. (#23/#25/#27/#28/#33), Review-Schwelle "3-5 Fälle" wahrscheinlich erreicht, aber nie explizit abgehakt. Fix: expliziten n-Zähler einführen wie bei [[feedback_realisiertes_rr]].
4. **15:30-16:00-Fenster-Review** ([[feedback_trading_zeitfenster]], seit 23.07., n=0) — Status seit Einführung nicht aktualisiert, gegen Trade-DB prüfen.
5. Kosmetisch: MEMORY.md-Index-Ungenauigkeit zum DAX-To-do-Stand (in dieser Session korrigiert), [[feedback_instrumenten_fokus]] übergeordneter Rahmen überholt durch [[project_dax_erweiterung]] (Korrelationsregel selbst bleibt gültig), [[project_tradingeconomics_api_idee]] stufte einen Vorfall fälschlich als "ersten Fall" ein.

## Paid-Upgrade-Bewertung

- **TradingEconomics-API ($149/Monat) — Top-Empfehlung, aktiv prüfen.** Einziger Kandidat mit echtem wiederkehrendem Schmerzpunkt (Kalender-/Datenfehler 28.07., 30.07., 03.08. zweimal). Vor Kauf: TradingView-eigenes News-/Kalenderpanel als günstigere Alternative testen.
- Essential-Plan-Indikator-Limit (5/Chart): nicht jetzt, relevant erst wieder bei DAX-To-do-Punkt 10 (Indikator-Neubewertung).
- DOM/Order-Flow: kein Kaufthema, architektonisch blockiert durch No-Auto-Order-Entscheidung ([[project_vision]]).
- UnusualWhales: Bedingung "Phase 3 komplett" noch nicht erfüllt, korrekt zurückgestellt.
- Nasdaq-Feed-Add-on (NASDAQ GIDS €5): bereits in [[project_nasdaq_official_feed_idee]] behandelt, keine neue Erkenntnis.
- Bloomberg: endgültig verworfen, kein Diskussionsbedarf.
- X-API-Guthaben ($25 vom 30.06.) dürfte bald aufgebraucht sein — proaktiv nachladen empfohlen, kein "Upgrade" im engeren Sinn.

**Why:** Levis Stimmungswechsel (bereit für bezahlte Verbesserungen, wenn Vorteil klar ist) macht eine bewusste, einmalige Bestandsaufnahme sinnvoll statt reaktivem Einzelfall-Prüfen.

**How to apply:** Noch keine der offenen Korrekturen umgesetzt, keine der Paid-Optionen gebucht — wartet auf Levis Priorisierung/Entscheidung.
