---
name: project-phase4-gates-2026-08-12
description: "Phase 4 neu gestaffelt (12.08.2026, Fable-Empfehlung Punkt 3 der Vollprüfung, von Levi genehmigt): Phase 3 verlängert auf #36-40, Phase 4 (#41-50) bleibt bei Phase-3-Positionsgröße statt 10.000€. Sechs numerische Gates fürs #50-Review vorab festgelegt, vier blinde Flecken dokumentiert."
metadata:
  node_type: memory
  type: project
  originSessionId: session-2026-08-12
  modified: 2026-08-12T11:15:40.059Z
---

## Auftrag und Kontext

Teil der Fable-Vollprüfung Phase 1-3 vom 12.08.2026 ([[project_fable_vollpruefung_phase1-3_2026-08-12]]), Handlungsempfehlung Punkt 3 ("Phase-4-Entscheidung nicht auf Basis von '0 Regelbrüche' treffen"). Levi hat diese Empfehlung exakt wie vorgeschlagen genehmigt — diese Datei ist die verbindliche Umsetzung, keine Neuentwicklung.

**Warum überhaupt neu gestaffelt:** Die Vollprüfung zeigte, dass Phase 3 (#26-35) unter Phase-1-Niveau zurückfiel (RR 0,86:1 statt 1,18:1), dass die offiziellen Regelbrüche (0/10) real bei 2-5/10 lagen, und dass Trade #34 einen strukturellen Fehler offenlegte (Schock-Tier-Gate 8c nicht mechanisch mit der SL-Formel verknüpft — am 12.08.2026 behoben). Ein Sprung auf 10.000€/Trade direkt im Anschluss an einen frisch (nur prozessual, nicht mit echten Trades) reparierten Fehlertyp wäre eine Verdopplung des Einsatzes auf einer unbewiesenen, gerade erst geschlossenen Lücke.

## 1. Phase 3 verlängert um 5 Trades (#36-40)

Ersetzt/ergänzt den bisherigen Skalierungs-Fahrplan in [[project_risikomanagement]] (dort Phase 3 ursprünglich #26-35).

- Bei Trades, wo das Schock-Tier-Gate ([[feedback_chartanalyse]] Punkt 8c, ergänzt 12.08.2026) tatsächlich greift (Schock-/Chop-Tag erkannt UND Regime-Gate 8d ✓): Position läuft bei reduzierter Größe (~2.000-2.500€, etablierter 50%-Stacking-Floor aus [[project_risikomanagement]] Abschnitt "Stacking-Regel für gleichzeitige Halbierungs-Gründe").
- Trades ohne Schock-Klassifikation: normale Phase-3-Vollgröße (4.000-5.000€).
- **Falls in #36-40 kein einziger Schock-Fall auftritt:** Das #40-Review muss das explizit als "Gate weiterhin unbestätigt" vermerken — NICHT stillschweigend als bestanden werten. Fehlende Gegenbeispiele sind kein Beleg für Funktionsfähigkeit.

## 2. Phase 4 (#41-50) bleibt bei Phase-3-Größe, NICHT bei 10.000€

Ersetzt die bisherige Phase-4-Definition in [[project_risikomanagement]] ("Phase 4 neu definiert", ergänzt 31.07.2026: #36-40 bei ~10.000€/Trade).

- Phase 4 (#41-50) läuft bei derselben Positionsgröße wie Phase 3 (4.000-5.000€), nicht bei 10.000€.
- Der ursprünglich für Phase 4 vorgesehene 10.000€-Positionstest (plus konditionale UnusualWhales-Integration) wird zu einem **eigenen, späteren Bewährungsschritt** — erst NACH einem bestandenen #50-Review, nicht in denselben 10 Trades vermischt.

**Begründung:** Monte Carlo (siehe [[project_robustheit_monte_carlo]], Schwelle n≈50) braucht nur die Ergebnis-Verteilung, unabhängig von der Positionsgröße. Der aktuellste demonstrierte Edge (Phase 3, EV 0,213%/Trade) trägt das Einkommensziel nicht einmal bei voller 50k-Skalierung (≈852€/Monat statt Ziel 2.100-3.900€, siehe Vollprüfung Kernbefund 5). Auf einen unbewiesenen, frisch (nur prozessual) reparierten Fehlertyp direkt das Doppelte an Positionsgröße zu setzen wäre im Zweifelsfall doppelt so großer Verlust bei derselben unbestätigten Fehlerrate — nicht bessere Daten.

## 3. Sechs numerische Gates für das #50-Review (vorab festgelegt gegen Post-hoc-Rationalisierung)

| Kriterium | Schwelle | Begründung |
|---|---|---|
| #21/#34-Typ-Regelbrüche (SL-Distanz/RR-Invalidierung), gemessen über #36-50 (n=15) | 0 von 15 | Bright-Line, exakt die am 12.08.2026 geschlossene Lücke |
| Realisiertes RR (blended #36-50) | ≥ 1,3:1 | Klar über Phase 1 (1,18) und deutlich über Phase 3 (0,86) |
| EV/Trade (blended #36-50) | ≥ 0,5% | Nahe am einzigen bisher demonstrierten Niveau (Phase 2: 0,611%), das laut Vollprüfung das Einkommensziel überhaupt erreichbar macht |
| Win-Rate | ≥ 55% | Zwischen dem bisherigen Hochstufen-Minimum (45-50%) und Phase 2 (70%) |
| DB-Sync / Regelbruch-Tracking | 100%, `trades.db` einzige Quelle | Fortsetzung des am 12.08.2026 erledigten Punkt-2-Fixes (siehe [[project_fable_vollpruefung_phase1-3_2026-08-12]]) |
| Monte-Carlo-Ergebnis (n=50) | Reales Endergebnis nicht in den unteren 20% der simulierten Verteilung | Aus [[project_robustheit_monte_carlo]], jetzt als konkretes Ergebnis-Kriterium statt nur "MC durchgeführt" |

**Explizite Kopplung — alle sechs Kriterien sind UND-verknüpft, nicht optional:** Nur wenn ALLE sechs Kriterien erfüllt sind, ist der Übergang zu 10.000€/Trade bzw. 50k-Realkapital freigegeben.

**Unterschiedliche Konsequenzen je nach Ausfallmuster:**
- Werden nur RR/EV/WR verfehlt, aber Regelbrüche=0 und MC bestanden → Konsequenz ist Strategie-Überarbeitung (Edge-Problem).
- Treten Regelbrüche erneut auf → Konsequenz ist Prozess-Problem, unabhängig vom Ergebnis der anderen fünf Kriterien.
Diese beiden Fälle dürfen im #50-Review nicht zur selben Handlungsempfehlung zusammengefasst werden.

## 4. Vier blinde Flecken — im #50-Review explizit zu dokumentieren, nicht nur als gelöst abzuhaken

Diese vier Punkte sind bewusst offene Einschränkungen des #50-Review-Rahmens selbst, nicht Dinge, die durch das Bestehen der sechs Gates automatisch mitgelöst werden. Das #50-Review muss zu jedem einzelnen explizit Stellung nehmen (nicht stillschweigend übergehen):

1. **Sequenzrobustheit ≠ ausreichender Edge.** n=50/Monte-Carlo-Bestehen beweist NUR, dass das Ergebnis robust gegenüber der eigenen historischen Reihenfolge ist — NICHT, dass der Edge groß genug für das Einkommensziel ist. Beide Fragen müssen im #50-Review getrennt beantwortet werden. Ein bestandener MC-Test bei gleichzeitig verfehlten EV/RR-Gates bedeutet NICHT "GO".
2. **Getrennte Zwecke nicht vermischen.** Die ursprünglich getrennten Zwecke (10k€-Positionstest, UnusualWhales-Integration, n=50-Erreichen) dürfen nicht in denselben Trades vermischt werden — sonst lässt sich ein Fehlschlag hinterher nicht kausal einem einzelnen Faktor zuordnen.
3. **Regimebeschränkung.** Alle 15 Trades (#36-50) laufen im selben Symbol/Zeitraum/vermutlich ähnlichem Marktregime. Ein bestandener MC-Test ist keine Aussage über zukünftige Regimewechsel (Vola-Schübe, Zinsentscheid-Cluster im Herbst 2026).
4. **Asymmetrisches Fehlerrisiko in diesem Fenster.** Genau in diesem sensiblen Vertrauens-Aufbau-Fenster (#36-50) ist ein erneuter #34-Typ-Fehler bei größerer Positionsgröße besonders teuer — das ist der Grund für Konservativität (Phase-3-Größe statt 10k€) in diesem Fenster, nicht Aggressivität.

**Why (gesamt):** Diese Datei setzt die Gate-Kriterien VOR den 15 Trades fest, nicht danach — genau um die Post-hoc-Rationalisierung zu verhindern, die die Vollprüfung bei den "0 Regelbrüche"-Behauptungen in Phase 1-3 aufgedeckt hat.

**How to apply:** Bei jedem Tagesabschluss ab Trade #36 prüfen, ob ein Schock-Tier-Fall vorlag (→ Positionsgröße gemäß Abschnitt 1). Ab Trade #41 automatisch auf Phase-3-Größe bleiben, nicht auf 10.000€ hochstufen. Beim #40-Review Abschnitt 1 (Gate-Bestätigung) prüfen. Beim #50-Review alle sechs Gates aus Abschnitt 3 einzeln durchgehen UND alle vier blinden Flecken aus Abschnitt 4 explizit beantworten, bevor über 10.000€/50k-Realkapital entschieden wird.

## 5. Nachtrag 12.08.2026 — Minimal-Grouping implementiert, Voll-Infrastruktur weiter aufgeschoben

Umsetzung von Punkt 7 der Fable-Vollprüfung ([[project_fable_vollpruefung_phase1-3_2026-08-12]]), von Levi exakt wie empfohlen genehmigt: Das minimale Wochen-/Monats-Grouping (`trade_stats.cjs --group week` / `--group month`, UTC-sicheres `isoWeekKey()`/`monthKey()`) wurde heute implementiert und live gegen die 35-Trade-Baseline (239,41€) validiert (Rekonziliation exakt, Grenzfälle #28/#29/#30 korrekt Juli/August zugeordnet, keine Phantom-Buckets). Zweck: frühere Sichtbarkeit auf die drei metrikbasierten Gates (RR/EV/WR aus Abschnitt 3) während des #36-50-Fensters, ohne auf das #40- oder #50-Review warten zu müssen.

Die VOLLE Infrastruktur (narrative Wochen-/Monatsabschluss-Dokumente, Drawdown-Tracking, Monte-Carlo-Integration) bleibt weiterhin aufgeschoben. Build-Trigger dafür bleibt fest: nach dem #40-Review, ODER sobald #50 absehbar 2-3 Wochen entfernt ist — je nachdem, was zuerst eintritt.

## 6. Nachtrag 12.08.2026 — Monte-Carlo-Methodik vollständig spezifiziert

Die vier zuvor offenen Ambiguitäten der Monte-Carlo-Methode (Gate-Metrik, 20%-Schwelle, Permutationstest-vs-Bootstrap-Benennung, Datenbasis `trades.db`/`result_eur`) sind jetzt in [[project_robustheit_monte_carlo]] vollständig aufgelöst (Nachtrag 12.08.2026 dort) — reine Text-Klarstellung, kein Skript gebaut, Build-Trigger für Abschnitt 5 oben unverändert.

## 7. Nachtrag 12.08.2026 — Risiko-Limit-Prüfung

Fable-Gegencheck des bestehenden 2%-Kapitaleinsatz-Limits ([[project_risikomanagement]], dort die Basisregel), von Levi angefragt und die folgende Einschätzung genehmigt. Diese Ergänzung ist eine Verfeinerung/Interim-Einschränkung dieser Basisregel für den konkreten 10k€/50k-Übergangsschritt — KEINE Änderung der Phase-1-3-Historie und KEINE Änderung für das laufende #36-50-Fenster.

**1. Kelly-Gegencheck-Ergebnis:** Bei WR 60%/RR 1,07:1 (Lifetime-blended) läge Voll-Kelly bei ~22,6% Kapitaleinsatz, bei den aktuelleren Phase-3-Zahlen (WR 60%, RR 0,86:1) bei ~13,5%. Zehntel-Kelly (konservative Standardpraxis) landet bei 1,3-2,3% — die bestehenden 2% liegen also innerhalb einer vernünftigen fraktionalen-Kelly-Bandbreite für den demonstrierten Edge, sind nicht willkürlich gewählt.

**2. Der eigentliche Befund:** Das reale Risiko-Limit (2%/300€) hat in 35 Trades noch nie gebunden — das Positions-Limit (4.000-5.000€) bindet immer zuerst. Das ist KEIN Beweis, dass 2% richtig kalibriert ist, nur ein Beweis, dass es nie getestet wurde. Der größte Einzelverlust der Historie (#34, -117,50€) lag bei nur ~0,78% des 15k-Kapitals. Sollte das 2%-Limit je real greifen (Schock-Tier-SL + größere Position, z.B. beim 10k€-Test oder 50k-Kapitalbasis), wäre das ein Sprung auf mehr als das Doppelte des je real erlebten Risikos — ein Regimewechsel, kein Randfall.

**3. Fünfter blinder Fleck (Ergänzung zu den vier in Abschnitt 4):** Die obere Risiko-Schicht (2%/Trade, 4%/Tag) ist strukturell ungetestet in der Praxis — nur der rollierende 4-von-5-Verlustschutz hat bisher real gegriffen. Die Schichtung selbst ist logisch konsistent (2%→4%→20%, proportional gestaffelt), aber die Beweislage für die oberen zwei Schichten fehlt.

**4. Konkrete Empfehlung, als neue Regel:**
- **Für #41-50 (aktuelles Fenster, Phase-3-Größe 4.000-5.000€): keine Änderung** — das Risiko-Limit bindet hier ohnehin nicht, eine Anpassung wäre Kosmetik.
- **Für den nächsten echten Skalierungsschritt (10.000€-Positionstest bzw. 50k-Realkapital, frühestens nach bestandenem #50-Review): interim 1,5% statt 2%** als Risiko-Limit — abgeleitet vom bisher schlechtesten realen Fall (#34, hochgerechnet auf Schock-Tier-Maximum plus Sicherheitspuffer).
- **Volle 2% erst freigeben, wenn BEIDE Bedingungen erfüllt sind:** (a) die sechs #50-Gates (Abschnitt 3) bestanden, UND (b) das Risiko-Limit hat in der Praxis mindestens einmal real gebunden (nicht nur die Positionsformel im Kopf/auf dem Papier durchgerechnet) — damit belegt ist, dass die MIN-Logik unter echtem Marktdruck tut, was sie auf dem Papier tut.
