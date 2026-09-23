---
name: project-testtag-2026-09-21-besprechung-ausstehend
description: "Besprechung 22.09.2026 ABGESCHLOSSEN: Levi will an Trendtagen wie dem 21.09. Trades bekommen (alle 7 ausgelassenen Setups hätten TP1 erreicht) → Entscheidung Y4-a + Y4-b (Trendtag-Modus: Q2 als Schattenfaktor ab ADX>=45 + Dual-Gate 2/2 + 1H-Bias + Q2 gemessen), Y4-c zurückgestellt. UMGESETZT+COMMITTET 22.09. (9819ce0) nach 3 Runden Opus-Gegencheck (FREIGEGEBEN MIT AUFLAGEN → FREIGEGEBEN → FREIGEGEBEN), A1-A4 + Backlog#1-4 erledigt. Ergebnisblock am Dateiende."
metadata: 
  node_type: memory
  type: project
  modified: 2026-09-22T10:14:41.701Z
  originSessionId: 9aca5bf5-bdfc-4bb6-922b-17583cc90312
---

Levi (21.09.2026, nach Vorlage der Opus-Analyse): "Bitte noch für morgen vermerken, wie wir das Setup hätten ja machen können, weil ja jeder TP erreichbar gewesen wäre. Ich will das wir trades machen, besonders an solchen Tagen. Das vermerken, dass wir das morgen vor der Umsetzung von Opus Empfehlungen noch besprechen."

**Zu besprechen, BEVOR irgendeine der Y1–Y8-Empfehlungen aus [[project_testtag_analyse_2026-09-21]] umgesetzt wird:**

Wie hätte das Regelwerk so kalibriert werden können, dass an einem Tag wie dem 21.09. tatsächlich Trades genommen worden wären — Levi will explizit, dass an solchen Tagen (klarer Trendtag, 2/2 Dual-Gate durchgehend, ADX 61, 1H-Bias nie dagegen) auch wirklich eingestiegen wird, nicht nur sauber analysiert/ausgelassen.

**Ausgangslage aus der Analyse:**
- 0 Trades am 21.09., obwohl alle 7 im Tagesverlauf ausgelassenen Setups bar-für-bar nachgerechnet TP1 erreicht haben — keines wurde per SL beendet (zwei mit Vorbehalt: 16:07 war ein regelkonformes RR-FAIL wegen Ankerfehler, 19:07 hätte nach Punkt 12/12.3 vorzeitig abgebaut werden müssen).
- Der limitierende Faktor war strukturell **Q2** (Abstand Entry↔EMA50(5min) relativ zu ATR, Schwelle ≤1,5x) — 9 von 9 Bewertungen NEIN bei 5,68–8,09x ATR. Auf einem echten Trendtag entfernt sich der Kurs per Definition von seiner EMA50 und kommt ihr nicht mehr nahe — Q2 ist auf Trendtagen strukturell nie erfüllbar.
- Das ist bereits als **Y4** in [[project_testtag_analyse_2026-09-21]] aufgeworfen ("darf ein Q-Faktor, der auf Trendtagen strukturell nie erfüllbar ist, den Score deckeln?") mit drei Diskussionsoptionen: (i) Q2-Schwelle regimeabhängig (lockerer bei bestätigtem Trendtag nach 8d/ADX), (ii) Q2 auf Trendtagen als reinen Schattenfaktor führen und Score nur aus Q1/Q3/Q4 bilden, (iii) unverändert lassen und Trendtage mit 0 Trades akzeptieren.
- Derselbe Grunddeckel-Effekt trat bereits am 17.09. auf, dort über Q4 statt Q2 (siehe [[project_testtag_analyse_2026-09-17]]) — zwei aufeinanderfolgende Testtage, an denen ein einzelner Q-Faktor den gesamten Tag strukturell auf ROT gedeckelt hat, unabhängig von der tatsächlichen Setup-Qualität.

**Warum das eine Besprechung braucht, bevor Fable etwas umsetzt:** Levis Ziel ("wir wollen Trades machen, besonders an solchen Tagen") ist eine Prioritätsentscheidung, keine reine Bugfix-Frage — sie berührt die Grundkonstruktion des Q-Score-Gates (Schwelle vs. Regimeabhängigkeit vs. Gewichtung) und sollte nicht nebenbei im selben Umsetzungspaket wie die eher mechanischen Y1/Y2/Y3/Y5–Y8-Punkte durchlaufen. Vorschlag für das Gespräch: erst Y4 im Detail durchgehen (inkl. der Frage, ob Option ii — Q2 als Schattenfaktor auf Trendtagen — das gewünschte Ergebnis am 21.09. erzeugt hätte, ohne an chop-artigen Tagen die Schutzfunktion von Q2 zu verlieren), dann erst den Rest der Y-Liste umsetzen lassen.

Siehe auch [[feedback_dont_change_running_system]] (Verbesserungsfunde vermerken statt sofort umsetzen) und [[feedback_regeldisziplin]] (Verlust durch Regelbruch vs. durch Marktlage) als Hintergrund für die Abwägung.

---

## Ergebnis (22.09.2026) — Besprechung abgeschlossen, Y4 umgesetzt

**Levi-Entscheidung:** Y4-a + Y4-b umsetzen (Option ii aus [[project_testtag_analyse_2026-09-21]]), Y4-c zurückgestellt. Y1–Y3, Y5–Y8 bleiben offen.

**Fable-Umsetzung (COMMITTET 22.09.2026, Hash 9819ce0):**
- **Y4-a:** `--q1-reject` / `--q3-coherence` sind A3-Pflicht mit `--grund-`-Ausweg (fehlender Wert → Q1/Q3 UNKLAR, sichtbar als MESSFELD-AUSNAHME statt still).
- **Y4-b Trendtag-Modus** (`gate_check.cjs`, Konstante `Q2_MODUS = 'trendtag'`, `TRENDTAG_ADX_MIN = 45`): Q2 wird zum Schattenfaktor (zählt nicht in die Ampel, bleibt voll gerechnet/sichtbar), wenn ALLE Bedingungen erfüllt sind — (1) ADX(14, 5min) ≥ 45, (2) Dual-Gate 2/2 (`--q3-coherence yes`), (3) 1H-Bias in Traderichtung (`--override-1h-close` vs. `--override-1h-ema50`), (4) **A1:** Q2 tatsächlich gemessen (kein `--grund-ema50-5min`). Fehlt eine → Fail-Closed, Q2 zählt regulär (n=4). Q-Score dann aus Q1/Q3/Q4 (3/3 GRÜN, 2/3 GELB). Pflicht-Ausgabeblock `TRENDTAG-MODUS AKTIV|NICHT AKTIV (Y4-b, ...)`, Schattenlog `scripts/q2_schatten_log.jsonl` (Schwelle 45 nach ~5 Trendtagen aus `trendmodus_bei` nachkalibrieren), `protokoll_bilanz.cjs` meldet Live-Läufe mit ADX ≥ 45 ohne Block. ADX bekommt damit erstmals eine Rechtsfolge (kein Gate/Veto — nur ob Q2 zählt). Am 21.09. hätte das die 9/9-Q2-NEIN-Deckelung aufgehoben.

**Opus-Gegencheck: FREIGEGEBEN MIT AUFLAGEN — alle drei umgesetzt (22.09.2026):**
- **A1 (Geldfolge):** entschuldigtes Q2 (UNKNOWN) durfte den Trendmodus nicht scharf schalten (3/4 GELB → 3/3 GRÜN). Jetzt vierte Bedingung `q2Gemessen`, im Ausgabeblock/`--json`/Schattenlog sichtbar.
- **A2 (Anzeige):** Nenner `/4` war an 6 Stellen hartcodiert (Skipped-Konsolenzeile, `skipped_fiktiv.cjs --list`, 3× `vollcheck.cjs`, Sizing-Grund "Q-Score 3/4 GELB") → jetzt aus `nenner`/`q_nenner` (Fallback 4 für alte Einträge); `q_nenner` neu im Skipped-Eintrag.
- **A4 (Typo-Lücke):** `--q1-reject yse` wurde still UNKLAR → jetzt Enum-Prüfung mit Hard-Exit 1 (Muster `--blackout`). **Nebenbefund dabei gefunden+gefixt:** wertloser Schalter `--q1-reject`/`--q3-coherence` (ohne Wert) kam als `true` an → `toBool(true)` = **JA** trotz MESSFELD-AUSNAHME "fehlt" (bei `--q3-coherence` hätte das den Trendmodus scharf geschaltet). Jetzt konsequent UNKLAR.

**Backlog #1-#4 (aus Gegencheck-Runde 2, alle umgesetzt+committet 22.09.):**
- **#1:** Retest-Zeitbox-Texte (`gate_check.cjs` Retest-Zeitbox-Block + Option-D-Solo-Default-Zeile, `vollcheck.cjs` 13.1-Konsequenz) von hartcodiert "Q-Score ≥ 3/4" auf nennerunabhängig "GELB oder besser (hier n-1/n)" umgestellt — dritte Kurz-Konsultation empfahl das (Begründung: `ampelAus()` definiert GELB seit jeher relativ zum Nenner, nie als feste 75%-Schwelle; eine strikte 3/3-Lesart hätte die Neu-Aufruf-Hürde über die Einstiegs-Hürde gehoben und den Q2-Deckel-Effekt durch die Hintertür zurückgebracht).
- **#2:** `toBool(true)`-Altlast (Nebenbefund aus A4) generalisiert auf alle 7 betroffenen Boolean-CLI-Felder (`q1-reject`, `q3-coherence`, `qqq-volume-below-avg`, `chasing`, `chop-flip`, `dual-gate-nas100`, `dual-gate-qqq`) via neue Konstante `BOOL_CLI_FELDER` — wertlose Schalter werden jetzt konsequent zu UNKLAR statt fälschlich JA/erfüllt.
- **#3/#4:** zwei veraltete/ungenaue Kommentare korrigiert.
- 3. Opus-Gegencheck (nach #1-#4): **FREIGEGEBEN**, 311/311 Tests grün, Kernlogik (Trendmodus/A1/A2/A4) unberührt außer semantisch identischer Verschiebung des Delete-Mechanismus.

**Y4-c (Q1 auf Trendtagen):** weiterhin bewusst zurückgestellt — keine Kalibrierungsbasis (0 von 6 Q1-Messungen bei ADX≥45 verwertbar), erst nach ~3-5 weiteren Trendtagen mit Schattenmessung entscheiden.

**Offen / Restpunkte (klein, nicht dringend, Backlog):**
1. Kein Regressionstest, der die Formulierung "GELB oder besser" fest pinnt — ein Rückfall auf harte Bruchzahl bliebe grün.
2. `--qqq-volume-below-avg` ist das einzige der 7 `BOOL_CLI_FELDER` ohne Testabdeckung (manuell verifiziert, korrekt).
3. Formulierungsasymmetrie: `vollcheck.cjs` fehlt der Zusatz "regulaer 3/4, Trendtag-Modus 2/3" gegenüber `gate_check.cjs` (nur Wording).
4. `BOOL_CLI_FELDER`-Konstante bricht die alphabetische Gruppierung der umgebenden Konstanten (kosmetisch).
5. `TRENDTAG_ADX_MIN = 45` ist in `protokoll_bilanz.cjs` als Kopie dupliziert — bei Nachkalibrierung müssen beide Stellen angefasst werden.
6. Erster Live-Trendtag mit aktivem Modus steht aus; Kalibrierung der 45er-Schwelle nach ~5 Trendtagen anhand `q2_schatten_log.jsonl`/`trendmodus_bei`.
