---
name: project-fable-vollpruefung-phase1-3-2026-08-12
description: "Fable-Vollprüfung 12.08.2026: Phase-3-Review + Gesamt-Review Phase 1-3 (Trades #1-35), auf Levis Wunsch schonungslos ohne Ausnahmen. Kernbefunde: RR U-förmig (1,18→1,75→0,86), 2%-Risiko-Behauptung nie erreicht (Ø~0,5%), Regelbrüche systematisch untertrieben (real 2-5/10 in Phase 3 statt 0, 6/15 in Phase 1 statt 2), Kapitalziel bei aktuellem Phase-3-Edge NICHT erreichbar (852€/Monat bei 50k statt Ziel 2.100-3.900€). 10 priorisierte Handlungsempfehlungen, werden ab 12.08. Punkt für Punkt durchgesprochen."
metadata:
  node_type: memory
  type: project
  originSessionId: session-2026-08-12
  modified: 2026-08-12T11:02:50.269Z
---

## Auftrag und Kontext

Levi bat am 12.08.2026 explizit um eine schonungslose Fable-5-Vollprüfung ohne Ausnahmen: Phase-3-Abschluss-Review UND Gesamt-Review Phase 1-3, inklusive Regelwerk-Audit, RR-Prüfung, 2%-Risiko-Prüfung, Kapital/Ziel-Prüfung — als Grundlage für die Entscheidung Richtung Phase 4 / 50.000€-Kapitalstufe. Methodik: unabhängige Neuberechnung aus `trade_log.md` + 30 Tagesdateien + `trades.db`, gegengeprüft mit `trade_stats.cjs`.

**Arithmetik-Ergebnis:** Keine Rechenfehler gefunden — alle Summen/Mittelwerte/RR/EV stimmen zwischen trade_log.md, trades.db und trade_stats.cjs überein.

## Kernbefund 1: RR-Verlauf ist U-förmig, nicht monoton verbessernd

| Phase | RR | EV/Trade | Ergebnis € |
|---|---|---|---|
| 1 (#1-15) | 1,18:1 | 0,385% | 24,62€ |
| 2 (#16-25) | 1,75:1 | 0,611% | 114,24€ |
| 3 (#26-35) | 0,86:1 | 0,213% | 100,55€ |
| Gesamt | 1,07:1 | 0,4% | 239,41€ |

Phase 3 fällt unter Phase-1-Niveau zurück. Treiber: v.a. Trade #26 und #34 (größte Einzelverluste der Historie). Trade #34 explizit kein Marktrauschen — Schock-Tag war aktiv bestätigt (8d), SL trotzdem nur 1,7× ATR statt vorgeschriebener 2,5-3× (8c).

## Kernbefund 2: 2%-Risiko-Behauptung nie erreicht (Phase 3 nachgerechnet)

Tatsächliches Risiko pro Trade lag zwischen 0,21% und 1,26% des Kapitals (Ø ≈0,5%), nie bei den dokumentierten 2%/300€. Positions-Limit war durchgehend bindend, nie das Risiko-Limit — strukturell dieselbe Lücke, die für Phase 1 bereits eingeräumt war (dort 0,07-0,17%), gilt der Sache nach unverändert auch für Phase 3.

## Kernbefund 3: Regelbrüche systematisch untertrieben

Offiziell 6/35 (2 Phase 1, 4 Phase 2, 0 Phase 3). Real vermutlich höher:
- **Phase 1:** laut [[feedback_regime_wechsel]] wurden 4 von 5 Verlusten der Serie #8-12 als Prozessfehler reklassifiziert, nie in trade_log.md/trades.db nachgetragen → reale Zahl eher 6/15
- **Phase 3:** Trade #31 ("echter Prozessfehler" laut eigenem Review) und Trade #34 ("systematischer Fehler") stehen in trades.db weiterhin als `rule_violation=0`. Dazu #29/#30/#32 mit dokumentierten Abweichungen ohne Pflichtformat → reale Zahl eher 2-5/10, nicht 0
- Gleiches Muster wurde im Phase-2-Review (24.07.) bereits einmal festgestellt ("Transparenz wurde mit Regelkonformität verwechselt") — Korrekturkreislauf (Reklassifizierung → zurück in DB) ist nicht robust, fiel in Phase 3 erneut aus
- Survivorship-Bias sichtbar: die 6 offiziellen Regelbrüche sind 5 Gewinn-Trades/1 Verlust, Ø-Ergebnis 1,302%/Trade (3× höher als Gesamt-EV)

## Kernbefund 4: Strukturelle Regel-Lücke — Schock-Status nicht mit SL-Formel verknüpft

Trade #34: erkannte Schock-Tag-Klassifikation (8d) war nicht mechanisch mit der SL-Distanz-Formel (8c) verknüpft — dieselbe Lücke hat GLEICHZEITIG auch die Stacking-Halbierungsregel umgangen (beide hängen an "SL-Breite"). Seit 07.08. unbehoben, 4 Lösungsoptionen liegen unentschieden vor (siehe [[trades/trading_2026-08-07]]).

## Kernbefund 5: Kapitalziel bei aktuellem Edge NICHT erreichbar

Monatliche Run-Rate real: 239,41€/56 Tage ≈ **130€/Monat** = 4,3% der Zielmitte (3.000€). Trade-Frequenz ist NICHT das Problem (≈19/Monat, nah am Ziel-Modell). Lücke liegt bei Positionsgröße + realisiertem €-EV/Trade (6,84€ real vs. 150-195€ Zielmodell).

**Drei Szenarien für 50k-Stufe:**
- Aktuellster Edge (Phase 3, EV 0,213%): ≈852€/Monat = 41% der unteren Zielgrenze — **Ziel nicht erreichbar**
- Lifetime-Blended (EV 0,4%): ≈1.600€/Monat = 76% der unteren Grenze — weiterhin verfehlt
- Bester bisheriger Edge (Phase 2, EV 0,611%): ≈2.444€/Monat — läge im unteren Zielband

**Nur beim Phase-2-Niveau ist das Einkommensziel überhaupt erreichbar.** Aktuellstes Signal (Phase 3) zeigt Gegenrichtung. n=35 liegt noch unter der selbst gesetzten Monte-Carlo-Schwelle (~50, siehe [[project_robustheit_monte_carlo]]).

## Positiv-Liste (nicht anfassen)

Keine Revenge-Trading-Belege in 30 Tagesdateien. Echte Selbstkorrektur-Kultur (#19 BE→Loss, #21 doppelt korrigiert, #13-Preis korrigiert). Gestaffelter Phase-3-Einstieg diszipliniert. #34→#35-Korrektur binnen 49 Minuten unter echtem Druck. BE-Definition/Broker-Wert-Priorität sauber etabliert. Unattended-Monitoring live bestätigt. 50%-Sizing bei #33 korrekt gepreist.

## 10 Handlungsempfehlungen (priorisiert) — Bearbeitungsstatus

Levi arbeitet sich ab 12.08.2026 Punkt für Punkt durch, in dieser Reihenfolge:

1. **Sofort:** Schock-ATR-Tier/Regime-Gate-Lücke schließen — **ERLEDIGT 12.08.2026.** Umgesetzt als reine Prozessregel (kein Code) in [[feedback_chartanalyse]] Punkt 8c, neuer Block "Schock-Tier-RR-Gate": 5-Schritte-Go/No-Go-Ablauf, der die SL-ATR-Kopplung (07.08.-Fix) mit einer zwingenden RR-Rückkopplung gegen TP1 verknüpft (Präzedenz Trade #17: SL verbreitert ohne TP1-Neucheck → RR unbemerkt auf 0,32:1 gefallen). Risiko-Limit-Frage war bereits durch die bestehende Positionsgrößen-Formel (MIN-Logik) gelöst, keine neue Logik nötig. Option 4 (Skript-Automatisierung) bewusst zurückgestellt — Fable empfiehlt sie erst mit echter Testabdeckung, nicht wie `size.cjs` ungetestet auszurollen. **Offener Folgepunkt, separat zu prüfen:** Verdachtsfall Trade #35 — Stacking-Regel (SL-Breite-Trigger) evtl. nicht auf die Positionsgröße angewendet trotz korrekt verbreitertem Schock-Tier-SL, siehe [[trades/trading_2026-08-07]].
2. Regelbruch-Tracking rückwirkend korrigieren + Korrekturkreislauf strukturell reparieren — **ERLEDIGT 12.08.2026.** `trades.db` korrigiert: #8, #9, #10, #12, #31, #34 auf `rule_violation=1` gesetzt (Quellen als Notiz je Zeile vermerkt). Neue Gesamtzahl: **12/35** (Phase 1: 6/15, Phase 2: 4/10 unverändert, Phase 3: 2/10). #11 (Grenzwertig) und #33 (Kommunikationslücke, keine entscheidungsrelevante Abweichung) bewusst NICHT hinzugefügt. `trade_log.md` ist seit 27.07.2026 nicht mehr die führende Quelle (endet bei #30) — mit Freeze-Hinweis versehen, `trades.db` ist ab sofort die alleinige Quelle. Struktureller Fix: neue Pflicht-Abschlusszeile "DB-Sync: JA/NEIN" in [[feedback_tagesabschluss]], Abschnitt "Reklassifizierung bestehender Trades". Backstop-Idee `check_violation_sync.cjs` (automatischer Abgleich Tagesdateien↔DB) bewusst zurückgestellt, noch nicht gebaut.
3. Phase-4-Entscheidung nicht auf Basis von "0 Regelbrüche" treffen — **ERLEDIGT 12.08.2026.** Levis Vorschlag (Phase 3 um 5 Trades auf #40 verlängern, dann Phase 4 mit 10 Trades auf #50 = Monte-Carlo-Schwelle) wurde von Fable geprüft und mit Auflagen umgesetzt, siehe [[project_phase4_gates_2026-08-12]]: #36-40 nur bei Schock-Tier-Fällen auf 50%-Floor reduziert (Gate erst 0 Mal live bestätigt), sonst normale Phase-3-Größe. #41-50 bleiben bei Phase-3-Größe (4.000-5.000€) statt der ursprünglich für Phase 4 geplanten 10.000€ — Fable widersprach hier explizit Levis erstem Impuls (10k€ sofort), weil der aktuellste Edge (Phase 3) das Einkommensziel nicht trägt und ein unbewiesener Fix nicht mit doppelter Positionsgröße getestet werden sollte. Sechs numerische Gates fürs #50-Review jetzt vorab festgelegt (0 #21/#34-Typ-Regelbrüche, RR≥1,3:1, EV≥0,5%, WR≥55%, 100% DB-Sync, MC-Ergebnis nicht in unteren 20%) — nur wenn alle sechs erfüllt sind, ist der 10k€/50k-Übergang freigegeben. Vier blinde Flecken dokumentiert, u.a.: n=50 erreichen beweist nur Sequenzrobustheit, nicht dass der Edge groß genug ist.
4. Überfällige Reviews abschließen — **ERLEDIGT 12.08.2026 (beide Reviews durchgeführt, aber ehrlich nicht abschließend).** Zeitfenster-Regel (15:30-16:00): nur n=3 statt angenommener 5 (#28, #30, #31), leichter Datenlean gegen die Regel (Hochrechnung +15,50€ bei Vollgröße vs. real +7,75€), aber zu dünn für eine Entscheidung — Regel bleibt in Kraft, siehe [[feedback_trading_zeitfenster]]. Stall-Exit Punkt 12: n=7 Fälle (#23,#25,#27,#28,#32,#33,#35), nur #32 sauber regelkonform; wichtigster Fund — #35 (einziger Fall nach dem 12.4-Fix) wurde eigentlich vom Punkt-7c-Trigger ausgelöst, nicht von einem echten Punkt-12-Trigger, die 12.4-Erweiterung bleibt funktional ungetestet. Nicht als validiert geschlossen, siehe [[feedback_live_trading]] Punkt 12.5. Beide Regeln unverändert in Kraft, keine Regeländerung ausgelöst.
5. Kapitalskalierung auf aktuellsten statt besten Edge stützen, ehrlich kommunizieren — **ERLEDIGT 12.08.2026.** Kapital-Realitätscheck (130€/Monat real vs. 2.100-3.900€ Ziel, nur Phase-2-Edge erreicht das Ziel) als dauerhaftes Addendum in [[project_vision]] und [[project_performance_ziele]] verankert, nicht nur im Audit vergraben. Klargestellt: die bisher getrackte Win-Rate-Metrik ("erreicht") verdeckte die eigentliche Lücke (€-EV/Trade bei Zielkapital, "verfehlt"). 10k€/50k-Übergang bereits über [[project_phase4_gates_2026-08-12]] an Edge-Qualität gekoppelt, nicht nur an Trade-Anzahl.
6. TP1-Gate mit echtem Puffer statt exakt an der Kante — **ABGELEHNT 12.08.2026 (Levi-Entscheidung, bewusst kein Fehler).** TP1 bei exakt 1:1 ist gewolltes Design (garantierter risikofreier Teilverkauf sobald erreicht), TP2 trägt das offene Gewinnpotenzial ohne RR-Deckel (siehe 27.07.-Reform in [[feedback_chartanalyse]] Punkt 8b). Der beobachtete RR-Rückgang in Phase 3 geht damit primär auf die Verlustseite zurück (#26, #34), nicht auf das TP1-Gate selbst — keine Regeländerung.
7. Wochen-/Monatsabschluss-Infrastruktur — **TEILWEISE ERLEDIGT 12.08.2026 (Minimalversion).** `node scripts/trade_stats.cjs --group week/month` implementiert (UTC-sicheres Datums-Parsing, wiederverwendet die bereits bewährte `report()`-Funktion). Alle Validierungstests bestanden: Wochen-/Monatssummen exakt gegen die Gesamtstatistik (35 Trades, 239,41€) reconciled, Grenzfälle #28/#29/#30 korrekt zugeordnet, Ground-Truth-Abgleich #26-30 (169,79€) bestätigt. Volle Infrastruktur (narrative Abschlüsse, Drawdown-Tracking, MC-Anbindung) bewusst vertagt — Trigger: nach #40-Review oder wenn #50 absehbar 2-3 Wochen entfernt ist, siehe [[project_phase4_gates_2026-08-12]].
8. size.cjs-Testdisziplin auf alle Live-Skripte ausweiten — **ERLEDIGT 12.08.2026.** Alle 10 unversionierten Skripte geprüft (cooldown/trade_db/add_trade/trade_stats/fred_snapshot jeweils NAS100 vs. DAX, plus eu_snapshot_dax.py, migrate_trade_log.cjs). **Echter Bug gefunden und gefixt:** `migrate_trade_log.cjs` hätte bei Re-Run die Trades #31-35 unwiderruflich gelöscht (baut `trades.db` unbedingt aus `trade_log.md` neu auf, das seit 27.07. bei #30 eingefroren ist, siehe Punkt 2) — Guard eingebaut, live gegengetestet, `trades.db` unangetastet (35 Trades bestätigt). Alle anderen Skripte sauber, keine Vorzeichen-/Kopierfehler zwischen NAS100/DAX-Varianten. Standing-Rule (Reconciliation-Pflicht vor Live-Einsatz trade-kritischer Skripte) verschärft in [[feedback_size_script_short_bug]].
9. Survivorship-Bias bei Regelbrüchen aktiv adressieren — **TEILWEISE ERLEDIGT 12.08.2026 (Levi-Entscheidung).** Zahlenkorrektur: die 12 Verstöße sind jetzt 5W/7L (nicht mehr Mehrheit Gewinn) — ABER alle 6 heute neu ergänzten Fälle (#8,9,10,12,31,34) waren Verluste, kein neu gefundener Gewinn-Verstoß, das bestätigt den Bias auf einer anderen Ebene. Umgesetzt: (a) Pflichtzeile "Regelkonformität geprüft: JA — Verstoß: JA/NEIN" bei JEDEM Trade in [[feedback_tagesabschluss]], (b) Levis Vorschlag (taggleiches Fable-Review bei erkanntem Verstoß) formalisiert in [[feedback_regeldisziplin]] — Neuerung: jetzt auch bei Gewinn-Trades erzwungen, nicht nur informell bei Verlusten wie bisher. **Bewusst NICHT umgesetzt (Levi-Entscheidung):** automatische 50%-Sizing-Reduktion nach Verstoß, keine neue Cooldown-Regel-3 für Verstoß-Häufung. **Dringender Nebenfund gefixt:** `cooldown_check.cjs` las bisher aus dem seit 27.07. eingefrorenen `trade_log.md`, war damit >2 Wochen blind für Trades #31-35 (Tages-Cooldown + Rolling-Loss-Schutz liefen auf veralteten Daten) — umgestellt auf `trades.db`, getestet (liest jetzt korrekt 35 Trades, Status aktuell grün). `cooldown_check_dax.cjs` hatte denselben Bug, proaktiv mitgefixt (bisher folgenlos, da 0 DAX-Trades bisher).
10. Monte-Carlo-Schwelle vor Phase 4 im Auge behalten — **ERLEDIGT 12.08.2026.** [[project_robustheit_monte_carlo]] und [[project_phase4_gates_2026-08-12]] waren auseinandergelaufen (20%-Schwelle nur im Gates-Dokument). Vier Ambiguitäten aufgelöst: €-Endergebnis als alleinige Gate-Metrik (Drawdown nur diagnostisch), 20%-Schwelle in Ursprungsdatei zurückgespiegelt, Methode explizit als Permutationstest ohne Zurücklegen benannt (Sequenz- vs. Regime-Robustheit-Caveat verlinkt), Datenbasis auf `trades.db.result_eur` fixiert. Skriptbau bleibt beim bereits gesetzten Trigger (#40-Review oder #50 absehbar 2-3 Wochen entfernt).

## Status: Alle 10 Punkte abgearbeitet (12.08.2026)

1 Sofort-Fix, 2 Datenkorrektur+Prozess, 3 Strategie-Entscheidung, 4 Reviews (ehrlich unentscheidbar dokumentiert), 5 Zielrealität verankert, 6 abgelehnt (bewusstes Design, kein Fehler), 7 Minimalversion, 8 Skript-Audit (1 echter Bug gefunden+gefixt), 9 teilweise (Erkennungspflicht ja, Mechanik nein, dringender Cooldown-Datenbug nebenbei gefixt), 10 Textklärung. Nächster echter Meilenstein: #40-Review (Zwischenstand der Phase-3-Verlängerung), dann #50-Review gegen die sechs Gates in [[project_phase4_gates_2026-08-12]].

**Why:** Das ist die entscheidende Grundlage für die Phase-4-/50k-Entscheidung — ohne diese Prüfung hätte die Skalierung auf Basis der optimistischsten (Lifetime- oder Phase-2-)Zahl statt der aktuellsten (Phase-3-)Zahl stattgefunden.

**How to apply:** Bei jeder künftigen Diskussion über Phase 4 oder Kapitalskalierung diesen Report als Referenz heranziehen, nicht die einzelnen Tages-Reviews isoliert. Bearbeitungsstatus der 10 Punkte oben aktuell halten, sobald Levi und Claude einen Punkt abschließen.
