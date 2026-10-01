# Opus-Antwort zu Entscheidung (d): Blockt Q-ROT Gewinner? (Do 01.10.2026)

**Kurzfazit:**
1. Dein Gefühl hat einen wahren Kern. Q-ROT blockt aber nicht zufällig Gewinner, sondern **eine ganze Klasse von Einstiegen**: Ausbruchs- und Fortsetzungs-Entries. Q4 ist am 50er-Raster praktisch nie erfüllbar (5 % der Momente, ab ATR ≥ 33 rechnerisch unmöglich). Q1 verlangt eine Ablehnung, die ein Ausbruch nicht hat. Damit ist jedes Momentum-Setup automatisch ROT, egal wie gut es ist.
2. **Belegt ist, dass diese Klasse Geld verliert, nicht. Belegt ist das Gegenteil aber auch nicht.** Es gibt 7 unabhängige Q-ROT-Fälle, 5 davon seit der heutigen Q2-Definition. Sie bringen Ø +0,23 R (95 %-KI −0,83 bis +1,29), seit 14.09. Ø +0,65 R (KI −0,70 bis +2,00). TP1 haben nur 2 von 7 erreicht. **Ohne den besten Tag liegt Ø bei −0,02 R.** Damit fällt das Ergebnis durch das Kriterium, das du selbst am 28.09. festgelegt hast.
3. Dass Q-ROT „Gewinner blockt“, siehst du an 3 Trendtagen in Folge (21., 29., 30.09.). Die zwei schnellen Verlierer (09.09. Chop, 17.09. Eröffnung) stehen nicht oder kaum im jsonl. Die 12 „ROT-Gewinner“ vom 11.09. beruhen auf der alten, kaputten Q2-Formel und zählen nicht.
4. **Empfehlung:** Q-ROT live **jetzt nicht** lockern, auch nicht für den Echtgeld-Start #44. Stattdessen genau **eine** vorab festgelegte Schatten-Hypothese (unten, H1) auf neuen Tagen ab 01.10. testen, mit klarer Schwelle für eine spätere Regeländerung. Kombi/V2 läuft unverändert weiter.
5. Zum adaptiven Lernen: ja, aber auf Ebene der **Tagesmomente** (2–5 unabhängige pro Tag), nicht über Gate-Läufe (≈1 pro Tag). Mit einfachen Tabellen und Beta-Binomial, ohne Machine Learning. Ein Urteil ist realistisch **Ende Oktober** möglich, nicht schon zum Freeze-Ende.

---

## 1. Fall-Tabelle (aus skipped_setups_fiktiv / kombi_fiktiv / q2_schatten_log, 09.09. aus project_testtag_analyse_2026-09-09)

**Q-ROT, PASS ausgelassen. Unabhängig heißt: ein Cluster je Tag/Bewegung, Folgeläufe in Klammern.**

| # | Datum/Zeit DE | Dir | Q1 | Q2 (EMA50-Abst.) | Q3 | Q4 (Runway) | Score | Ergebnis (Stichtag) | Struktur |
|---|---|---|---|---|---|---|---|---|---|
| 1 | 09.09. 17:25 | short | ✓ (falsch-pos.) | ✗ (A: 2,13×) | ? | ✗ manuell | 2/4 | **−1,00 R** SL nach ~14 Min | Chop, ADX 12–28, nicht im jsonl |
| 2 | 09.09. 18:10 (+18:15, 18:25) | short | ✗ | ✓ (A: ~0,95×) | ? | ✗ 0,48 | 1/4 | **−0,63 R** offen 19:00 | Chop, gleicher Anker/TP1 |
| 3 | 17.09. 15:27 (+15:31) | long | ✗ | ✗ 4,08× | ✓ | ✗ 0,53 | 1/4 | **−1,00 R** SL nach 3 Min (beide) | Eröffnungsminuten, SL 1,74×ATR, RR 1,60 |
| 4 | 21.09. 16:41 (+17:16) | long | ✗ | ✗ 7,47× | ✓ | ✗ 0,28 | 1/4 | **+1,76 R** TP1 55 Min (+1,80) | Trendtag long, SL 1,5×ATR, Chasing |
| 5 | 28.09. 19:36 (+19:41) | short | ✗ | ✓ 1,21× | ✗ | ✗ 0,12 | 1/4 | **+0,23 R** offen 20:00 (+0,05) | ADX 28,7, SL 1,66×ATR, RR 1,54 |
| 6 | 29.09. 18:11 | short | ✗ | ✓ 0,88× | ✓ | ✗ 0,06 (NEU-A ✓) | 2/4 | **+0,84 R** offen 20:00 (Stall-Exit +0,55) | ADX 16, SL 1,77×ATR, RR 1,49 |
| 7 | 30.09. 15:32 | long | ✗ | ✗ 2,01× | ✓ | ✗ 0,50 | 1/4 | **+1,42 R** TP1 25 Min | Trendtag long, ADX 31, SL 1,81×ATR |

**Vergleichsgruppen** (fiktiv, gleiche Messmethode):
- **UNBEKANNT** (nach Option D wie ROT behandelt):
  - 17.09. 16:07: +1,00 (TP1)
  - 21.09. 18:03 / 18:22 / 19:07 / 19:28: +1,29 / +1,06 / +0,42 / +1,04. Das ist derselbe Trendtag wie #4, also kein neuer unabhängiger Fall.
  - 28.09. 18:55: −0,09
- **GELB** (fiktiv eingestiegen bzw. Kombi 0,5):
  - 16.09. 20:26 und 20:47: −1 und −1
  - 28.09. 19:32: +0,05
  - **Ø je Cluster −0,48 R**, also schlechter als ROT.
- **GRÜN:** 0 Fälle, nie aufgetreten.
- **Echte Trades in trades.db:** Q-Score nur bei #36/#39/#40/#41, alle 0/4 „unklar“. Für diese Frage **unbrauchbar**.
- **Ausgeschlossen:** 11.09. (12 Einträge, ROT allein durch die alte Q2-Formel 14×ATR). Mit der heutigen Definition wären sie GELB gewesen.

**n:** 9 ROT-Gate-Einträge seit 14.09. ergeben **5 unabhängige** Fälle. Mit dem 09.09. (auch unter Option A ROT, so nachgerechnet am 14.09.) sind es **7**. Kombi/V2: n 6, davon 4 unabhängig. Σ gewichtet +0,74 R, ungewichtet (r_primaer) +2,39 R.

**Welcher Faktor trägt das ROT?**
- Q4 ✗ und Q1 ✗ in **6/6 Fällen seit 17.09.**, Q4 ✗ in 7/7.
- Q2 ✗ nur in 3 Fällen, Q3 ✗ nur in 1 Fall.
- Der Trendmodus (Q2 als Schattenfaktor) hätte **keinen** Fall gedreht: q2_schatten_log zeigt „ohne Q2“ 0/3 bzw. 1/3, also ROT.
- **Q4 ist der systematische Faktor, aber nicht im Sinne von „blockt falsch“. Er misst fast nichts** und drückt so jede Ampel um eine Stufe (Befund I1 vom 14.09., 35/37 ✗). Q1 ist eine Selbstauskunft und beim Ausbruch per Definition ✗.
- Ergebnis: **ROT = „kein Retest-Entry“.** Das ist Absicht des Designs (Studie 24.08.: Q-Score als Retest-Qualitätsmaß), wird aber im Trend zur Pauschalsperre.

## 2. Statistik ehrlich

- **Mittelwert R** (je Cluster, erster Lauf):
  - n=7: Σ +1,62, Ø +0,23, SD 1,15, 95 %-KI (t) **[−0,83 ; +1,29]**
  - n=5 (seit 14.09.): Ø +0,65, KI **[−0,70 ; +2,00]**
  - Beide Intervalle schließen 0 und auch −0,5 R ein.
- **Quoten** (Wilson):
  - TP1 2/7 = 29 %, KI [8 % ; 64 %]
  - „positiv bis Stichtag“ 5/7, KI [36 % ; 92 %]
  - Die reale Gewinnschwelle bei deinem Payoff (0,94:1) liegt bei ~52–56 %. Die Quote ist also mit „klar profitabel“ **und** mit „Verlustbringer“ vereinbar.
- **Freeze-Kriterium** (Ø ≥ +0,10 UND ≥ 0 ohne besten Tag):
  - n=7: +0,23 / **−0,02 → nicht erfüllt**
  - n=5: +0,65 / +0,37 → erfüllt, aber nur mit 5 Werten, davon 2 „offen bis Stichtag“
- **Bayes grob:** Mit einem skeptischen Prior (Ø −0,1 R, SD 0,3; ein ungeprüfter Filterbruch ist eher schlecht als gut) schrumpft der Schätzer auf **≈ 0 bis +0,1 R**. P(Ø > 0) liegt flach bei ~0,7 (n=7) bzw. ~0,87 (n=5). Das heißt „eher positiv als negativ“, mehr nicht.
- **Power:** Um +0,3 R von 0 zu trennen (SD ~1,1, 80 % Power), braucht es **~50–80 unabhängige Fälle**. Mit ~1 ROT-Gate-Cluster pro Testtag dauert das Monate.
- **Verzerrungen:**
  - **Regime:** 4 von 5 Plus-Fällen fallen auf Trendtage seit dem 21.09. Die Verlierer lagen an Chop- bzw. Eröffnungstagen. Das ist **ein** Faktor (Tagestyp), keine 5 unabhängigen Belege.
  - **Selektion:** Die Logeinträge entstehen automatisch zur Gate-Zeit (V3) und werden mechanisch bar-für-bar nachgetragen, das ist gut. Die Stichtage sind aber uneinheitlich: 19:00 (09.09.), `--bis` (28./29.09.), Default 20:00 (seit 30.09.). 2 der 5 Plus-Fälle sind „offen“, nicht realisiert. Mit Stall-Exit wäre 29.09. +0,55 statt +0,84.
  - **Wahrnehmung:** Die beiden −1-R-Fälle vom 09.09. fehlen im jsonl. Die 11.09.-Einträge sehen wie ROT-Gewinner aus, sind aber Altdefinition. Wer naiv aus dem Log zählt, überschätzt das Bild deutlich.
  - **Mehrfachtests:** 4 Q-Faktoren × ~5 Strukturmerkmale × 3 Varianten ergeben ~60 Zellen. Bei n≈7 findet man darin sicher irgendein „Muster“.
- **Größere Vorab-Stichprobe aus momente_log** (tagesmomente, RR-1-Ausgang, unabhängige Momente, nur Q2 rekonstruierbar, Q1/Q4 = null):
  - FLOOR: Q2 ✗ n 20, Σ +6,00 (13 positiv) gegen Q2 ✓ n 10, Σ −0,85
  - AUTO: Q2 ✗ 7, Σ +3,00 gegen Q2 ✓ 6, Σ −1,08
  - **Aber:** 21.09. allein liefert +7,56 aus 8 FLOOR-Momenten. **Ohne 21.09. ist kein Unterschied da:** Q2 ✗ n 12, Σ −1,56 gegen Q2 ✓ n 10, Σ −0,85.
  - Der „Extension-Effekt“ ist also ein **Trendtag-Effekt**. Die effektive Stichprobe sind ~6 Tage, nicht 30 Momente.

**Antwort auf (1):** Q-ROT sperrt strukturell Momentum-Entries. Ob das Geld kostet, ist **offen**. Die Daten sprechen leicht dafür, dass diese Entries an **Trendtagen** gut sind und an anderen Tagen schlecht. Belastbar ist davon nichts.

## 3. Ist „adaptiv lernen“ über Kombi/V2 schon abgedeckt?

**Teilweise.** Kombi/V2 setzt deine Idee bereits um: ROT als Größe 0,25 statt Veto, GELB 0,5, getrennt geloggt, ohne Doppelbuchung (57c711a). Es fehlen drei Dinge:
- (a) Es wächst nur über echte Gate-Läufe. Das sind n 6 in 3 Tagen. Die Trigger-Lücke am Trendtag (30.09.: 4 h ohne Gate-Lauf) hält es klein.
- (b) Es gibt keine Struktur-Dimension (Tagestyp, Tageszeit).
- (c) Q1 ist Selbstauskunft und an Momenten nicht rekonstruierbar.

Die schnellere Datenquelle ist **tagesmomente** (58 Momente am 30.09., 2–5 unabhängige pro Tag und Variante). Q2-Wert, SL-Abstand (sl_dist_atr), Uhrzeit, Richtung und Bias stehen dort schon drin. Der 1H-Abstand zur EMA50 steht pro VC in oneh_shadow_log.

## 4. Lösungsvorschlag: eine vorab festgelegte Schatten-Auswertung (keine Regeländerung)

**Diese Formulierung heute festschreiben, bevor neue Daten da sind.** Sie gilt dann für Tage **ab 01.10.** als Vorwärtstest. Alles bis 30.09. war Hypothesen-Bildung und zählt nicht.

**H1 (einzige Haupthypothese):** Momente mit Q2 ✗ (|Entry − EMA50 5m| > 1,5×ATR) sind **im Trendkontext** nicht schlechter, sondern mindestens kostendeckend.
- **Trendkontext** (live erkennbar, Schwelle jetzt fixiert, nicht getunt): Der letzte geschlossene 1H-Bar liegt seit Session-Start ununterbrochen auf der Trade-Seite der 1H-EMA50, **und** der 1H-Abstand ist ≥ 2,0×ATR(5m) (oneh_shadow_log `abstand_atr`).
- **Zellen (nur 4):** Q2 ✗ / Q2 ✓ × Trendkontext ja / nein.
- **Maß:** r_rr1 der Variante FLOOR (Hauptmaß) und AUTO (Kontrolle), nur unabhängige Momente.
- **Was pro Moment schon geloggt ist:** Datum/Zeit, Richtung, q2_wert, q3_auto, sl_dist_atr, Ergebnisse je Variante, Bias.
- **Was als reine Messung später dazukommt** (Mengenbremse, nach Auftrag B):
  - 1H-Abstand per Join aus oneh_shadow_log statt im Loop
  - ADX(5m) je VC als Messfeld
  - Q4 maschinell gegen den Register-Stand des VC. Ob register_touch_log den Stand rekonstruierbar hält, muss geprüft werden.
  - Q1 bleibt bewusst draußen. Eine Selbstauskunft lässt sich nicht nachträglich messen.

**Methode, Schutz gegen Overfitting:**
1. **Tabellen statt Modell.** Pro Zelle: n, Tage, Σ R, Ø R, Ø R ohne besten Tag, Beta-Binomial-Posterior der Trefferquote (Prior Beta(5,5) = 50 % mit Gewicht 10).
2. **Mindest-n:** eine Zelle wird erst bewertet ab **≥ 15 unabhängigen Momenten aus ≥ 6 verschiedenen Tagen.**
3. **Stabilität:** Der Effekt muss in **beiden Hälften** des Vorwärtszeitraums dasselbe Vorzeichen haben.
4. **Schwelle für einen Regeländerungs-Vorschlag** (erst nach dem Freeze, Levi entscheidet), alles gleichzeitig:
   - Zelle „Q2 ✗ × Trend“ Ø-R ≥ +0,10 und ≥ 0 ohne besten Tag (dein 28.09.-Kriterium)
   - P(Quote > 50 %) ≥ 0,8
   - Kombi-ROT-Fälle aus derselben Zeit nicht negativ (Σ r_primaer ≥ 0)
   - SL-Treffer ≤ 30 Min in ≤ 1/3 der Fälle
5. **Was dann vorgeschlagen würde (eng, nicht pauschal):** „Q-ROT, das nur aus Q1/Q4 (+Q2) kommt, wird **im Trendkontext** zu Größe 0,25 statt Veto“, zuerst wieder fiktiv. Das bisherige Veto bleibt außerhalb des Trendkontexts.
6. Getrennt davon gehört **Q4 (50er-Raster, I1)** ins Freeze-Review. Das ist ein Messproblem und keine Lernfrage. Der Kandidat Q4-NEU-A läuft im Kombi-Schatten schon mit (er hob den 29.09. auf GELB).

**Zeitplan:**
- **Bis Freeze-Ende** (5/5 Tage ist nach dem nächsten Testtag erreicht, AUTO 9/20, Obergrenze ~Do 08.10.): nichts ändern, Testtage normal laufen lassen, tagesmomente und Kombi-Nachtrag wie bisher.
- **Hochrechnung:** ~2,5 unabhängige Q2-✗-Momente pro Tag (FLOOR). Für ≥ 15 je Zelle, aufgeteilt in Trend/Nicht-Trend, braucht es **~10–12 Testtage ab 01.10., also etwa 16.–23.10.** Achtung DST-Fenster 26.–30.10. (vollcheck-Zeiten falsch, siehe TODO).
- **Kombi ≥ 20 unabhängige ROT-Cluster:** realistisch erst **November**.
- **Auswerte-Skript** (read-only über momente_log + oneh_shadow_log): einmalig nach Auftrag B beauftragen, nicht im Loop.

## 5. Entscheidung für Levi

**(d-neu) Empfehlung, bitte mit Ja/Nein entscheiden:**
1. **Q-ROT live bleibt Veto**, auch beim Echtgeld-Start #44. Kombi/V2 läuft fiktiv unverändert weiter. → **Ja.**
2. **H1 wie in Abschnitt 4 heute festschreiben** (Trendkontext-Definition, 4 Zellen, Mindest-n 15 / 6 Tage, Schwellen). Zählung ab 01.10. Bis 30.09. gilt nur als Hypothesen-Quelle. → **Ja.** Es kostet jetzt 0 Code.
3. **Auswerte-Skript H1** ins erste Paket **nach** Auftrag B. Q4 (50er-Raster) ins Freeze-Review. → **Ja.**
4. **Nicht tun:** Q-ROT jetzt auf halbe Größe live setzen, Q1/Q4 im Freeze anfassen, Schwellen auf Basis der 7 Fälle verschieben.

## 6. Risiken, wenn du jetzt lockerst und es Zufall war

- **Geld:** Phase 3 riskiert 75 € pro Trade. Ein Ø von −0,2 R liegt voll im KI. Bei 10 ROT-Trades sind das −150 € Erwartungswert, Streuung ±260 €, also realistisch bis −400 €. Bei Größe 0,25 nur ~19 € pro Trade, deshalb ist Kombi der richtige Ort zum Lernen.
- **Muster der Verlierer:** Beide ROT-Verlierer stoppten **schnell** aus (3 bzw. 14 Min, Eröffnung bzw. Chop). Das ist genau der Fall, gegen den Q1/Q4 schützen sollen. Trendtage kommen nicht jeden Tag. Eine Lockerung nach 3 Trendtagen in Folge ist das klassische Rezept, um am nächsten Range-Tag zu bezahlen.
- **Messung:** Eine Änderung mitten im Freeze macht die laufende Freeze-Zählung ungültig (die Gate-Basis ändert sich). Dann beginnt die Zählung von vorn, und #44 verschiebt sich.
- **Disziplin** (feedback_regeldisziplin): Ein Verlust aus einer Lockerung ohne Vorab-Kriterium wäre **selbstgemacht**. Ein Verlust nach H1-Freigabe wäre „Verlust trotz Regeleinhaltung“. Dieser Unterschied ist der eigentliche Wert der Vorab-Kriterien.
- **Umgekehrtes Risiko** (ehrlich): Wenn H1 stimmt, kostet das Veto an Trendtagen ~+1 bis +1,5 R pro Tag. Das ist real, wird aber vom fiktiven Kombi-Schatten mitgemessen, ohne Echtgeld. Ein paar Wochen Geduld kosten dich also kein Geld, nur Ungeduld.

---
Quellen: scripts/skipped_setups_fiktiv.jsonl (40 Zeilen), kombi_fiktiv_log.jsonl (6), q2_schatten_log.jsonl, momente_log.jsonl (351 Zeilen, 277 qualifiziert), trades.db. Alles auf Kopien in %TEMP%\opus_qrot gerechnet. Memory: opus_bericht_testtag_2026-09-30, project_testtag_analyse_2026-09-09/-17/-21, project_q2_kalibrierung_entscheidungsvorlage_2026-09-11, project_gegencheck_q2q4_fable_umsetzung_2026-09-14 (I1), project_studie_bessere_trades_2026-08-24, project_testtag_2026-09-23_besprechung_ausstehend (Freeze §5), feedback_live_trading (Option D). Repo und Memory unverändert, nur diese Datei ist neu.
