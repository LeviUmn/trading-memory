---
name: project_h1_q2_trendkontext_vorabkriterium_2026-10-01
description: "H1 festgeschrieben, K1-K10 verbindlich (Levi 01.10.2026) + Abschnitt 7a: Kombi-ROT (ii), Auswertungsplan R6, Skript-Zusatzregeln R7 (Opus-Gegencheck 2); R6-a ENTSCHIEDEN (Levi (a) 01.10.2026): Endauswertung fester Stichtag Fr 30.10.2026 Tagesabschluss, S5-S8/F5 umgesetzt"
metadata:
  node_type: memory
  type: project
  originSessionId: fable-auftrag-h1-2026-10-01
  modified: 2026-10-02T08:47:33.828Z
---

# H1 — Q2 ✗ im Trendkontext: pre-registriertes Vorab-Kriterium (festgeschrieben 01.10.2026)

## 1. Status / Beschluss

- **Festgeschrieben am 01.10.2026 auf Levis ausdruecklichen Auftrag**, Grundlage: Opus-Antwort zu Entscheidung (d) [[opus_antwort_d_qrot_2026-10-01]], Abschnitt 4 („Loesungsvorschlag: eine vorab festgelegte Schatten-Auswertung, keine Regelaenderung“) und Abschnitt 5 Punkt 2 („H1 wie in Abschnitt 4 heute festschreiben … Es kostet jetzt 0 Code“).
- **Reine Dokumentation.** Kein Code, keine Regel- oder Skriptaenderung, nichts im Repo beruehrt, kein Commit/Push. *(Stand 01.10.2026 abends: das reine Lese-Auswerteskript `scripts/analyse/h1_auswertung.cjs` liegt als untracked Entwurf vor, s. Abschnitt 5 Punkt 1, 7a und Restunklarheit R3; Gates/Schwellen/Q-Faktoren unveraendert.)*
- **Auswertung erst nach dem Freeze-Ende** (Definition in [[project_testtag_2026-09-23_besprechung_ausstehend]] Abschnitt 5: ≥ 5 bewertbare Testtage ab 25.09.2026 UND AUTO ≥ 20 unabhaengige Bewegungen, Abbruch nach 10 bewertbaren Tagen, Obergrenze ca. Do 08.10.2026). Das gilt fuer **beide** Auswertungen des Plans R6 (Abschnitt 7a): auch die **Zwischenauswertung** kommt fruehestens nach Freeze-Ende (Klarstellung Opus-Gegencheck 3, 01.10.2026); nur die Zaehlstand-Abfrage (`--echt --zaehlstand`, keine R-Werte) ist jederzeit zulaessig. Opus: ein Urteil zu H1 ist realistisch **Ende Oktober** moeglich, nicht schon zum Freeze-Ende (Abschnitt 4 Zeitplan: ~10–12 Testtage ab 01.10., also etwa 16.–23.10.; DST-Fenster 26.–30.10. beachten, siehe [[project_vollcheck_dst_fix_todo_2026-10]]).
- **Stichtag der Endauswertung: Fr 30.10.2026 (Tagesabschluss) — Levi-Entscheid (a) vom 01.10.2026**, fester Kalenderstichtag, NICHT das Freeze-Ende, kein zweiter Ausloeser, keine Verlaengerung (Details Abschnitt 7a, R6/R6-a). (geaendert 02.10.2026 → 08.10.2026, s. Abschnitt 8)
- **Waehrend des Freeze KEINE Regelaenderung an Q-Score/Q-ROT.** Q-ROT live bleibt **Veto**, auch fuer den Echtgeld-Start #44 (Opus Abschnitt 5 Punkt 1, Levi-Auftrag 01.10.).
- **Kombi/V2 laeuft fiktiv unveraendert weiter** (ROT = Groesse 0,25, GELB 0,5, getrennt geloggt in `kombi_fiktiv_log.jsonl`, Stand 57c711a).

## 2. Kernfrage und Definitionen (woertlich nach Opus, Abschnitt 4)

**H1 (einzige Haupthypothese):** „Momente mit Q2 ✗ (|Entry − EMA50 5m| > 1,5×ATR) sind **im Trendkontext** nicht schlechter, sondern mindestens kostendeckend.“

**Trendkontext** (live erkennbar, Schwelle jetzt fixiert, nicht getunt) — beide Bedingungen zugleich:
1. Der letzte **geschlossene 1H-Bar** liegt **seit Session-Start ununterbrochen** auf der **Trade-Seite der 1H-EMA50**, UND
2. der **1H-Abstand ist ≥ 2,0×ATR(5m)** (Quelle: `oneh_shadow_log.jsonl`, Feld `abstand_atr`).

**Q2 ✗** = |Entry − EMA50 (5m)| > 1,5×ATR (heutige Q2-Definition seit 14.09.2026, Feld `q2_wert` in `momente_log.jsonl`). Q2 ✓ = Abstand ≤ 1,5×ATR. Die alte Q2-Formel (14×ATR, 11.09.) zaehlt nicht.

**Die 4 Zellen (nur diese, keine weiteren Dimensionen):**

| | Trendkontext **ja** | Trendkontext **nein** |
|---|---|---|
| **Q2 ✗** | Zelle A (**Pruefzelle fuer H1**) | Zelle B |
| **Q2 ✓** | Zelle C | Zelle D |

**Unabhaengiger Moment:** Opus' Formulierung (Abschnitt 1): „Unabhaengig heisst: ein Cluster je Tag/Bewegung, Folgelaeufe in Klammern.“ Fuer die Tagesmomente gilt die Unabhaengigkeitsdefinition von `tagesmomente.cjs` („unabhaengige Bewegungen“, dieselbe wie im Freeze-Kriterium) — verbindlich festgelegt in K1 (Abschnitt 7: Feld `varianten.FLOOR.unabhaengig === true`). Es zaehlen **nur unabhaengige Momente**, keine Folgemomente derselben Bewegung.

**Datenquelle:** `tagesmomente`/`momente_log.jsonl` (je Moment schon geloggt: Datum/Zeit, Richtung, `q2_wert`, `q3_auto`, `sl_dist_atr`, Ergebnisse je Variante, Bias) plus `oneh_shadow_log.jsonl` (1H-Abstand per Join, nicht im Loop). Q1 bleibt bewusst draussen (Selbstauskunft, nicht nachtraeglich messbar).

**Zaehlbeginn: NUR Tage ab 01.10.2026 (Vorwaertstest).** Alles bis einschliesslich 30.09.2026 war Hypothesen-Bildung und ist **nicht Teil des Tests** — weder die 7 unabhaengigen Q-ROT-Altfaelle (Ø +0,23 R, ohne besten Tag −0,02 R) noch die momente_log-Vorab-Stichprobe (FLOOR Q2 ✗ n 20 Σ +6,00 gegen Q2 ✓ n 10 Σ −0,85; ohne 21.09. Q2 ✗ n 12 Σ −1,56 — „Extension-Effekt“ = Trendtag-Effekt, effektive Stichprobe ~6 Tage).

## 3. Auswertung (exakt, ohne Ermessensspielraum)

**3.1 Masse**
- Hauptmass: **r_rr1 der Variante FLOOR** (RR-1-Ausgang).
- Kontrolle: **r_rr1 der Variante AUTO**.
- Nur unabhaengige Momente. R ungewichtet.

**3.2 Tabellen statt Modell — pro Zelle ausweisen:** n, Anzahl Tage, Σ R, Ø R, Ø R ohne den besten Tag, Beta-Binomial-Posterior der Trefferquote mit **Prior Beta(5,5)** (= 50 % mit Gewicht 10).

**3.3 Mindest-n (Vorbedingung, vor jeder Bewertung):** Eine Zelle wird erst bewertet ab **≥ 15 unabhaengigen Momenten aus ≥ 6 verschiedenen Tagen**. Darunter: „zu selten, nicht belegt“, kein Vorschlag, keine Deutung.

**3.4 Checkliste fuer einen Regelaenderungs-Vorschlag** (gilt fuer Zelle A „Q2 ✗ × Trendkontext ja“, Hauptmass FLOOR; **alle Punkte gleichzeitig erfuellt**, sonst kein Vorschlag; Lesart „alle Punkte auf Zelle A“ durch Opus-Gegencheck 01.10.2026 bestaetigt, siehe Abschnitt 7):
1. Ø R ≥ **+0,10**.
2. Ø R ≥ **0 auch ohne den besten Tag** (Levis Freeze-Kriterium vom 28.09.2026).
3. **P(Trefferquote > 50 %) ≥ 0,8** (Beta-Binomial-Posterior, Prior Beta(5,5)).
4. **Stabilitaet:** Der Effekt hat in **beiden Haelften des Vorwaertszeitraums dasselbe Vorzeichen** (Opus Methode Punkt 3).
5. **Kombi-ROT-Faelle aus derselben Zeit nicht negativ:** Σ `r_primaer` ≥ 0 (aus `kombi_fiktiv_log.jsonl`).
6. **SL-Treffer ≤ 30 Min in ≤ 1/3 der Faelle** (Opus Schwellen-Punkt 4, letzter Spiegelstrich; im Auftragstext vom 01.10. nicht aufgezaehlt, steht aber in der Opus-Datei — uebernommen und per K4 verbindlich bestaetigt, Messvorschrift siehe Abschnitt 7).

Zusaetzlich auszuweisen (keine Schwelle, Pflichtanzeige): dieselbe Tabelle fuer AUTO als Kontrolle; weicht AUTO im Vorzeichen von FLOOR ab, ist das im Vorschlag ausdruecklich zu nennen.

**3.5 Was bei Erfuellung passiert**
- Es wird ein **Vorschlag** formuliert, eng und nicht pauschal (Opus Punkt 5 woertlich): „Q-ROT, das nur aus Q1/Q4 (+Q2) kommt, wird **im Trendkontext** zu Groesse 0,25 statt Veto“ — **zuerst wieder fiktiv**, nie direkt live. Das bisherige Veto bleibt ausserhalb des Trendkontexts bestehen. (Form des „fiktiv“: K7 in Abschnitt 7, bewusst offen bis H1 erfuellt ist.)
- Reihenfolge: Opus-Gegencheck vor jeder Aenderung → Levi entscheidet (Ja/Nein) → Umsetzung erst nach Freeze-Ende → **Commit/Push nur durch Levi** ([[feedback_commit_push_nur_levi]]).

**3.6 Was bei Nichterfuellung passiert**
- Kein Vorschlag. Q-ROT bleibt Veto. H1 gilt als **nicht belegt** (bei Mindest-n verfehlt: „zu selten, nicht belegt“). Keine Nachjustierung der Schwellen, keine neue Zelle, keine zweite Hypothese aus denselben Daten (Mehrfachtest-Warnung Opus Abschnitt 2: ~60 moegliche Zellen, bei n≈7 findet man immer ein „Muster“).

## 4. Nicht tun (verbindlich, Opus Abschnitt 5 Punkt 4 + Levi 01.10.)

- **Nicht** Q-ROT jetzt live lockern (auch nicht auf halbe Groesse, auch nicht fuer #44).
- **Nicht** Q1/Q4 im Freeze anfassen.
- **Nicht** die Schwellen anhand der 7 Altfaelle (oder sonst anhand bereits gesehener Daten) verschieben.
- **Nicht** die Kriterien dieser Datei nachtraeglich aendern. Eine Aenderung des Kriteriums ist nur mit Levi UND dokumentiertem Grund zulaessig, **nie nach Sicht der Ergebnisse**.
- Eine Aenderung mitten im Freeze wuerde die laufende Freeze-Zaehlung ungueltig machen (Gate-Basis aendert sich) und #44 verschieben (Opus Abschnitt 6).

## 5. Offene Folgepunkte (nur notiert, NICHT umgesetzt, Levi hat noch nicht entschieden)

1. **Reines Lese-Auswerteskript fuer H1** (read-only ueber `momente_log.jsonl` + `oneh_shadow_log.jsonl`, nicht im Loop). Opus-Empfehlung: ins **erste Paket nach Auftrag B**, einmalig beauftragen. Stand 01.10.2026 (Levi-Auftrag): vorgesehen als `scripts/analyse/h1_auswertung.cjs` — **Entwurf, noch nicht committet**; die H1-Auswertung laeuft ausschliesslich ueber dieses Skript und erst nach Freeze-Ende (siehe Abschnitt 7, Sichtungsverbot). Spaetere reine Messfelder (Mengenbremse): 1H-Abstand per Join aus oneh_shadow_log; ADX(5m) je VC; Q4 maschinell gegen den Register-Stand des VC (ob `register_touch_log` den Stand rekonstruierbar haelt, ist zu pruefen).
2. **Q4-Messproblem am 50er-Raster (Befund I1 14.09., 35/37 ✗; Q4 ✗ in 7/7 Altfaellen, ab ATR ≥ 33 rechnerisch unmoeglich)** gehoert ins **Freeze-Review** — Messproblem, keine Lernfrage. Kandidat Q4-NEU-A laeuft im Kombi-Schatten bereits mit.
3. **Datenluecke / Selektionsbias-Risiko:** Die zwei schnellen Verlierer (09.09. 17:25 Chop, −1,00 R nach ~14 Min; 17.09. 15:27 Eroeffnung, −1,00 R nach 3 Min) fehlen bzw. fallen in `skipped_setups_fiktiv.jsonl` kaum auf. Wer naiv aus dem Log zaehlt, ueberschaetzt das Bild. Stichtage waren bisher uneinheitlich (19:00 / `--bis` / Default 20:00 seit 30.09.).
4. Kombi ≥ 20 unabhaengige ROT-Cluster: realistisch erst **November** (Opus Zeitplan).

## 6. Verweise

[[opus_antwort_d_qrot_2026-10-01]] (Quelle, Abschnitte 4–6) · [[opus_gegencheck_auftrag_b_h1_2026-10-01]] (Quelle der K1–K10-Vorschlaege, Abschnitt „Memory (b)“, Tabelle K1–K8 + „Zusaetzlich vorher zu fixieren“ K9/K10) · [[opus_bericht_testtag_2026-09-30]] (Testtag 30.09., Fall #7) · [[project_testtag_2026-09-23_besprechung_ausstehend]] (Freeze-Ende-Regel Abschnitt 5) · [[project_ibf_schatten_2026-09-30]] (Stilvorlage festgeschriebenes Vorab-Kriterium, gleiche Logik „nur Schatten, Kriterien vorab“) · [[feedback_regeldisziplin]] (Verlust nach Vorab-Kriterium = „trotz Regeleinhaltung“, ohne = „selbstgemacht“).

**Why:** Q-ROT sperrt strukturell Momentum-/Fortsetzungs-Entries (Q1 verlangt Ablehnung, Q4 misst am 50er-Raster fast nichts). Ob das Geld kostet, ist offen: 7 Altfaelle, 95 %-KI [−0,83; +1,29], ohne besten Tag −0,02 R; die Plus-Faelle haengen an Trendtagen. Ohne vorab fixierte Kriterien waere jede Lockerung Overfitting auf 3 Trendtage in Folge — und ein Verlust daraus „selbstgemacht“. **How to apply:** Nach dem Freeze-Ende **zuerst diese Datei lesen**, dann die Auswertung **exakt nach Abschnitt 3** (Mindest-n → Tabellen → Checkliste 1–6) durchfuehren; nichts an den Schwellen anpassen; Ergebnis Opus zum Gegencheck, Entscheidung Levi.

## Klaerungsbedarf K1–K8 — ERLEDIGT (01.10.2026, siehe Abschnitt 7)

> **Status: ERLEDIGT.** Alle acht Punkte wurden von Opus im Gegencheck [[opus_gegencheck_auftrag_b_h1_2026-10-01]] beantwortet und von Levi am 01.10.2026 freigegeben („so umsetzen wie von Opus empfohlen“). Die verbindlichen Festlegungen stehen in **Abschnitt 7**; der folgende Text bleibt nur als Protokoll der urspruenglichen Fragen stehen und ist **nicht mehr offen** (Ausnahme K7, bewusst offen, s. Abschnitt 7).

- **K1 „unabhaengiger Moment“:** Opus definiert „unabhaengig“ in Abschnitt 1 nur fuer Gate-Cluster („ein Cluster je Tag/Bewegung“). Fuer Tagesmomente wird die Definition aus `tagesmomente.cjs` („unabhaengige Bewegungen“) vorausgesetzt, aber nicht ausgeschrieben. Zu bestaetigen, dass die Skript-Definition gilt.
- **K2 „Treffer“ fuer die Trefferquote (Checkliste 3):** nicht explizit definiert. Naheliegend bei r_rr1: TP1 (RR 1) erreicht = Treffer, SL = kein Treffer. Offen, wie „offen bis Stichtag“ zaehlt (Opus nennt in Abschnitt 2 zwei Quoten: „TP1“ und „positiv bis Stichtag“).
- **K3 „beide Haelften des Vorwaertszeitraums“:** Teilung nach Kalendertagen, nach Testtagen oder nach Momentenzahl? Nicht festgelegt.
- **K4 SL-Treffer ≤ 30 Min (Checkliste 6):** steht in der Opus-Datei, fehlte im Auftragstext vom 01.10. Ausserdem offen, ob `momente_log.jsonl` die Zeit bis zum SL-Treffer enthaelt (Opus zaehlt sie nicht unter „schon geloggt“), und fuer welche Variante (FLOOR?) gemessen wird.
- **K5 „Session-Start“** in der Trendkontext-Definition: 14:30 DE (Messfenster tagesmomente) oder 15:30 DE (Testtag-Start)? Nicht festgelegt.
- **K6 „1H-Abstand“:** ob Abstand des 1H-Close oder des aktuellen Kurses zur 1H-EMA50 gemeint ist — Opus verweist auf `abstand_atr` im oneh_shadow_log; die dortige Berechnung ist massgeblich, zu bestaetigen.
- **K7 „zuerst wieder fiktiv“ (3.5):** Kombi/V2 fuehrt ROT bereits fiktiv mit 0,25. Offen, ob die Trendkontext-Variante als eigene, getrennt geloggte Fiktiv-Zeile laufen soll oder ob der bestehende Kombi-Schatten als „fiktiv“ gilt.
- **K8 Stabilitaetspruefung (Checkliste 4):** Opus fuehrt sie als Methode-Punkt 3, nicht in der „alles gleichzeitig“-Schwellenliste (Punkt 4). Hier als harte Checklisten-Bedingung uebernommen (Auftrag Levi 01.10.) — zu bestaetigen.

## 7. Verbindliche Festlegungen K1–K10 (Levi, 01.10.2026)

**Status:** Von Levi am **01.10.2026** freigegeben („so umsetzen wie von Opus empfohlen“), und zwar **VOR Sichtung jeglicher H1-relevanter Momentdaten ab 01.10.2026**. Quelle: Opus-Gegencheck [[opus_gegencheck_auftrag_b_h1_2026-10-01]], Abschnitt „Memory (b)“, Tabelle K1–K8 und „Zusaetzlich vorher zu fixieren“ (K9, K10) — Wortlaut der Opus-Vorschlaege unveraendert uebernommen. **Ab jetzt unveraenderlich**, ausser mit Levi UND dokumentiertem Grund, **nie nach Sicht der Ergebnisse** (gleiche Regel wie Abschnitt 4).

Opus' Begruendung fuer die Vorab-Fixierung (woertlich): „K1–K8 und was VOR der ersten Sichtung von 01.10.-Daten fix sein muss. Sonst ist es kein Vorab-Kriterium mehr: Jede Opus-Tagesanalyse zeigt Momente. Verbindlich entscheidet Levi.“

**Lesart bestaetigt:** Die Checkliste 3.4 (**alle Punkte 1–6**) gilt fuer **Zelle A = Q2 ✗ im Trendkontext**. Opus: „Die Checkliste 1–6 auf Zelle A anzuwenden ist eine Lesart. Opus nennt die Zelle nur in Punkt 1, die Lesart ist aber plausibel.“ — von Levi mit der Freigabe 01.10. bestaetigt.

| K | Thema | Verbindliche Definition (Opus-Vorschlag, wortgetreu) |
|---|---|---|
| **K1** | unabhaengiger Moment | Feld `varianten.FLOOR.unabhaengig === true` aus tagesmomente (AUTO entsprechend). Die Zellenzuordnung erfolgt **nach** der Unabhaengigkeitsauswahl, keine Neuauswahl je Zelle. |
| **K2** | Treffer (Trefferquote, Checkliste 3) | Wie die Trefferquote von tagesmomente: `exit_art` TP (RR1) = Treffer. `exit_art` OFFEN, SL und Stall = kein Treffer. |
| **K3** | Haelften (Stabilitaet, Checkliste 4) | Gezaehlte Tage chronologisch, erste ⌈d/2⌉ gegen den Rest. Vorzeichen = Ø `r_rr1` FLOOR der Zelle A je Haelfte, 0 zaehlt als „kein gleiches Vorzeichen“. |
| **K4** | SL-Treffer ≤ 30 Min (Checkliste 6) | Daten liegen vor: `exit_ts − ts` bei `exit_art` SL, Variante FLOOR, Basis = unabhaengige Momente der Zelle A. Bedingung beibehalten. |
| **K5** | Session-Start (Trendkontext Bedingung 1) | Feld `session_start` je Zeile in momente_log (15:30, bzw. 14:30 im DST-Fenster). Beginnt das Shadow-Log spaeter als Session-Start + 15 Min: Trendkontext „nicht bestimmbar“, Moment ausschliessen und Anzahl ausweisen. |
| **K6** | 1H-Abstand (Trendkontext Bedingung 2 + 1) | `abstand_atr` des Shadow-Eintrags mit gleicher VC-Nummer und gleichem Datum (das ist **Kurs** − EMA50 1H, nicht der 1H-Close). Long ≥ +2,0, short ≤ −2,0. Bedingung 1 ueber `bias['1h']` aller Shadow-Eintraege von Session-Start bis zum Moment == dir. |
| **K7** | „zuerst fiktiv“ (3.5) | **Bewusst offen bis H1 erfuellt ist** (Opus: vorher zwingend „nein (erst bei Erfolg)“). Opus-Vorschlag, festgehalten fuer den Erfolgsfall: eigene getrennt geloggte Fiktiv-Zeile; Kombi/V2 bleibt unveraendert, sonst vermischen sich die Messungen. |
| **K8** | Stabilitaet hart (Checkliste 4) | Ja, hart. Opus schreibt „muss“. Checkliste 4 bleibt harte Bedingung. |
| **K9** | welche Tage zaehlen | tagesmomente-bewertbar (bars ok, ≥ 20 VC), fiktive und echte Testtage gleich. |
| **K10** | Q2 nicht bestimmbar | Momente mit `q.q2` null werden ausgeschlossen und ausgewiesen. |

Feldnamen zu K1–K6/K10 beziehen sich auf `momente_log.jsonl` (tagesmomente: `varianten.<FLOOR|AUTO>.{unabhaengig, exit_art, exit_ts, r_rr1}`, `session_start`, `q.q2`, `ts`) und `oneh_shadow_log.jsonl` (vollcheck: `abstand_atr`, `bias['1h']`, `vc`).

**Sichtungsverbot (Opus-Hinweis, verbindlich):** Bis zur Freigabe dieser Festlegungen waren keine H1-relevanten Momentdaten ab 01.10.2026 zu sichten („Bis dahin keine H1-relevanten Momentdaten ab 01.10. sichten“). Weiter gilt: **Tagesanalysen ab 01.10.2026 zeigen und bewerten die H1-Zellen nicht.** Die H1-Auswertung erfolgt **nur** nach Freeze-Ende per `scripts/analyse/h1_auswertung.cjs` (Entwurf, noch nicht committet), exakt nach Abschnitt 3 mit den Definitionen dieses Abschnitts und nach dem Auswertungsplan R6 (unten; Endauswertung zum festen Stichtag **30.10.2026**, Levi-Entscheid (a)). Vor der Zwischen-/Endauswertung ist nur die Zaehlstand-Abfrage `--echt --zaehlstand` zulaessig (keine R-Werte).

### 7a. Nachtraege aus Opus-Gegencheck 2 (01.10.2026) — von Levi am 01.10.2026 freigegeben („Opus' Empfehlungen 1:1 umsetzen“), VOR Sicht von H1-Daten ab 01.10.2026 (momente_log: 0 Zeilen ab 01.10., nur gezaehlt)

Quelle: [[opus_gegencheck_2_2026-10-01]] Abschnitt 5 (Empfehlungen (i)–(v), H8, H9) und Abschnitt 7 (Restauflagen F1–F4, S1–S4). Gleiche Unveraenderlichkeitsregel wie K1–K10. Das Skript `h1_auswertung.cjs` traegt dieselben Regeln (Kopfkommentar + Konstanten), Skript und diese Datei muessen identisch bleiben.

| Nr. | Thema | Verbindliche Festlegung (Levi 01.10.2026) |
|---|---|---|
| **(i) / R1** | K3 Haelften-Basis | **Alle nach K9 zaehlenden Tage** (wie im Skript), chronologisch, erste ⌈d/2⌉ gegen den Rest — nicht nur Tage mit Zelle-A-Moment. Hat eine Haelfte keinen Zelle-A-Moment, ist Punkt 4 **NICHT AUSWERTBAR = weiter sammeln**, ausdruecklich **NICHT** „nicht bestanden“. R1 damit **geschlossen**. |
| **(ii) / R5** | Kombi-ROT-Definition (Checkliste 5) | Es zaehlt **nur `ampel_kombi === 'ROT'`** (nicht `ampel_alt`; Altbeleg 29.09.: alt ROT, kombi GELB zaehlt nicht). **UNBEKANNT zaehlt nicht mit**, wird aber **getrennt ausgewiesen** (n, Σ r_primaer). **Fenster:** `datum` ≥ 01.10.2026 bis einschliesslich **letzter gezaehlter Tag** (K9); Kombi-Eintraege danach werden ignoriert und gezaehlt. **0 ROT-Faelle = „keine Faelle, kein Gegenbefund“ → Punkt 5 gilt als erfuellt** (leere Summe 0 ≥ 0), **mit PFLICHTVERMERK im Bericht** (sonst waere H1 im Echtgeld-Zeitraum ohne Fiktivtage dauerhaft unauswertbar, Opus F2). **ROT-Faelle ohne Nachtrag → Punkt 5 NICHT AUSWERTBAR**, bis der Nachtrag da ist (Nachtrag ist Tagesabschluss-Pflicht). Im Skript identisch festgeschrieben (Konstanten `KOMBI_ROT_WERT`, `KOMBI_UNBEKANNT_WERT`, `KOMBI_PFLICHTVERMERK`; Selbsttests 0 Faelle / UNBEKANNT / ohne Nachtrag / Obergrenze letzter gezaehlter Tag). |
| **(iii)** | K5 bei Teiltagen | **K5 unveraendert.** Die 12 im Probelauf verlorenen FLOOR-Momente stammen aus 3 Spaetstart-Tagen (Shadow ab 21.09. 16:00, 25.09. 17:47, 29.09. 16:42); Bedingung 1 („seit Session-Start ununterbrochen“) ist dort nicht pruefbar, eine Rekonstruktion waere erfunden. Betriebsfolge: startet der Loop spaeter als Session-Start + 15 Min (15:45, im DST-Fenster 14:45), faellt der Tag fuer H1 praktisch aus — puenktlicher Start ist das einzige Gegenmittel. |
| **(iv)** | K9 Fensterpruefung | **Beibehalten** (`K9_FENSTER_PRUEFEN = true`): „tagesmomente-bewertbar“ schliesst das Fenster laut `tagesmomente.cjs` selbst ein; das ist die woertliche K9-Definition, keine Zusatzregel. Den Schalter als Konstante nicht mehr anfassen. |
| **R6** | Auswertungsplan (Opus F3, Schutz gegen optional stopping / Nachjustieren nach Sicht der Daten) | **Hoechstens EINE Zwischenauswertung**, sobald Zelle A (Hauptmass FLOOR) **erstmals ≥ 15 unabhaengige Momente aus ≥ 6 Tagen** erreicht (`--echt --auswertung zwischen`; unter der Schwelle gibt das Skript nur den Zaehlstand aus, keine R-Werte; fruehestens nach Freeze-Ende, Abschnitt 1), **plus EINE Endauswertung zum festen Stichtag** (`--echt --auswertung end`). Was zuerst kommt, wird mit Datum notiert. Laeufe davor **nur als Zaehlstand-Abfrage** (`--echt --zaehlstand`: Tage, Momente je Zelle, Ausschluesse, Mindest-n-Status — keine R-Werte, Trefferquoten, Posterior, Checkliste, Urteil; jederzeit zulaessig). **Stichtag (R6-a, ENTSCHIEDEN — Levi-Entscheid (a) vom 01.10.2026, auf Empfehlung Opus-Gegencheck 3):** **fester Kalenderstichtag Fr 30.10.2026, ausgewertet beim Tagesabschluss**; letzter einbezogener Tag ist der 30.10.2026. **KEIN** Freeze-Ende als Stichtag, **KEIN** zweiter Ausloeser („12 Tage“), **KEINE** Verlaengerung (Variante (b) mit S6 wurde NICHT gewaehlt). Wird bis dahin das Mindest-n (Zelle A ≥ 15 Momente aus ≥ 6 Tagen) nicht erreicht, gilt H1 als **„nicht belegt“** (Q-ROT bleibt Veto); eine Fortsetzung waere nur als neue, neu pre-registrierte Hypothese auf Daten ab 31.10.2026 moeglich. **Hoechstens EINE Zwischenauswertung plus EINE Endauswertung am/nach dem Stichtag.** Im Skript hart (S5): Konstante `ENDSTICHTAG = '2026-10-30'`; `--echt --auswertung end` vor dem Tagesabschluss des Stichtags endet mit Exit 1 (Lesart: DE-Ortszeit des Laufs > 30.10.2026 ODER == 30.10.2026 ab 20:05 Uhr, d. h. nach dem 20:00-Stichtag des Tagesabschlusses; bewusst die Uhr und nicht „letzter gezaehlter Tag“, damit ein Loop-Ausfall am 30.10. die Endauswertung weder blockiert noch vorzieht), kein Parameter/keine Umgebungsvariable uebersteuert das; im Echtmodus werden nur Daten mit `datum` ≤ 30.10.2026 einbezogen (spaetere ignoriert und gezaehlt). `end` ist als Bericht **nur einmal** vorgesehen — technisch nicht erzwingbar (Skript schreibt nichts), deshalb traegt jeder end-Bericht den Vermerk „Endauswertung nur einmal; weitere Laeufe sind Wiederholungen und nicht entscheidungsrelevant“. Der Freeze-Stand wird vom Skript weiterhin nicht geprueft (der Stichtag liegt sicher nach dem Freeze-Ende). (geaendert 02.10.2026 → 08.10.2026, s. Abschnitt 8) |
| **S5–S8 / F5** | Nachbesserungen aus Opus-Gegencheck 3 ([[opus_gegencheck_3_2026-10-01]]; Levi-Auftrag 01.10.2026) | **F5:** die Interimsregel „Stichtag = Freeze-Ende laut Regel“ ist aus Datei und Skript entfernt und durch den Levi-Entscheid (a) ersetzt (s. R6). **S5:** Stichtag hart im Skript (s. R6). **S7 (N3):** `datum` wird nur im ISO-Format `YYYY-MM-DD` als Datum verglichen; andere Formate (z. B. „01.10.2026“) werden nicht verglichen, sondern ausgeschlossen und ausgewiesen („Datum nicht ISO: n Eintraege ausgeschlossen“), im Probelauf ohne jede Inhaltsausgabe. **S8 (N4):** Zaehlstand-Schutz per **Allowlist** der erlaubten Schluessel (Text und JSON), CLI-Selbsttest per `spawnSync` auf synthetischen Temp-Logs (`--echt --zaehlstand --json`, `--echt --auswertung zwischen --json` unter Schwelle, `--echt --auswertung end` vor Stichtag → Exit 1), Text-Regex deckt ASCII-Minus, Unicode-Minus, Plus, P-Werte (x,xxx) und Prozentquoten ab; alle 12 Opus-Mutanten (Ma–Ml, darunter die zuvor unentdeckten Mb/Mc/Me/Mf) werden vom Selbsttest gefangen. **Hinweise a–d umgesetzt:** H9-Testmeldung „nicht geprueft“ (statt „nicht bestanden“); Echt-Hinweis je Modus (Zaehlstand: „jederzeit zulaessig“); FLOOR/AUTO-Vorzeichenvergleich bei Ø = 0,00 → „nicht vergleichbar“ (Ø = 0 hat kein Vorzeichen); Haelften heissen „Haelfte 1/Haelfte 2“ (Schluessel `n_haelfte1`, `avg_haelfte2` …), nicht mehr „H1/H2“. |
| **R7** | Skript-Zusatzregeln (Opus F4; nicht in K1–K10, konservativ) | Die 4 Zuordnungsentscheidungen von `h1_auswertung.cjs` gelten verbindlich: **(1)** `bias['1h']` null in einem Shadow-Eintrag des Fensters Session-Start..Moment → Trendkontext **„nicht bestimmbar“** (Ausschluss + Ausweisung), **nicht** „Trend nein“. **(2)** `abstand_atr` fehlt/nicht numerisch → nicht bestimmbar (Ausschluss). **(3)** Moment vor Session-Start → ausgeschlossen und ausgewiesen. **(4)** Join Moment ↔ Shadow: zuerst **exakter `ts`**, sonst **`datum` + `vc`** mit dem **letzten Shadow-Eintrag ≤ Moment** (vc-Nummern koennen doppelt vorkommen). Keine der vier Regeln fuehrt zu einer Zellenzuordnung. |
| **R2** | K2 „Stall“ | **Geschlossen** (Opus (v)): `tagesmomente.cjs` kennt keinen Stall-Exit; `exit_art` ist TP1-HIT/SL-HIT/OFFEN/null, null ist ueber `unabhaengig` ausgeschlossen. „Alles ausser TP1-HIT = kein Treffer“ ist vollstaendig. |
| **R4** | K5/K6 1H-Bedingung 1 | Bleibt als **bekannte Messgrenze**, K6 ist massgeblich: `bias['1h']` im Shadow-Log ist `biasVon(--close-1h, --ema50-1h)` des Voll-Checks; ob `--close-1h` der letzte *geschlossene* 1H-Bar ist, legt vollcheck nicht fest (Operatorsache). **Nicht nachjustieren.** |
| **K4-Hinweis** | Dauer-Messung | `exit_ts` ist der **Schluss** der SL-Bar (5-Min-Raster): Dauer bis zu 5 Min ueberschaetzt, schnelle SL eher untergezaehlt (leicht nachsichtig). Entspricht dem Wortlaut von K4, bleibt so, ist bekannt. |
| **H8** | Prozesssatz „nie vom Schatten abschreiben“ (`loop_prompt.cjs` Z. 216, Q3-AUTO) | **Von Levi bestaetigt (01.10.2026).** Keine Gate-Wirkung; Voraussetzung dafuer, dass KONSISTENT/WIDERSPRUCH etwas misst. Keine Codeaenderung. |
| **H9** | Test liest echte Memory-Datei (`tests/trading_scripts.test.js`, B5/B2a-Test) | **Akzeptiert** (Praezedenzfall Z. 1685/1702). Umgesetzt 01.10.2026: im Skip-Fall (Memory-Datei nicht erreichbar, z. B. nach Mac-Umzug) gibt der Test eine sichtbare `t.diagnostic`-Meldung aus; kein Verhaltensunterschied, wenn die Datei erreichbar ist. Bekannt: eine Memory-Umformulierung an B2a/B5 kann den Repo-Test brechen (gewollte Kopplung). |

**Skript-Stand nach S1–S4 (01.10.2026):** `--echt` ist ein reines Flag (`--echt nein`/`--echt false`/jeder Wert ausser `true` = Exit 1; unbekannte Optionen = Exit 1), `--echt` verlangt `--zaehlstand` oder `--auswertung zwischen|end`; Probelauf (ohne `--echt`) liest nur Daten vor 01.10.2026 und gibt keinen Moment ab 01.10. aus. Selbsttest 62/62.

**Skript-Stand nach S5–S8/F5 (01.10.2026, Stand „S5-S8“):** zusaetzlich `ENDSTICHTAG = '2026-10-30'` (end vor Tagesabschluss 30.10. 20:05 DE = Exit 1, Pruefung vor dem Lesen der Logs), Echt-Fenster 01.10.–30.10.2026, ISO-Datumspruefung S7, Allowlist/CLI-Selbsttest S8, Hinweise a–d. Selbsttest 89/89 (der Selbsttest schreibt nur synthetische Logs in ein Temp-Verzeichnis und loescht sie wieder; echte Logs werden nie beschrieben). Probelauf-Zahlen unveraendert (FLOOR 17 gezaehlt; A 2 / B 7 / C 4 / D 4; AUTO 1 / 3 / 2 / 2; Kombi ROT 3 Σ +1,89; UNBEKANNT 1; Urteil ZU WENIG DATEN); einzige Textaenderung dort: Vorzeichenzeile „nicht vergleichbar (Ø FLOOR = 0,00 …)“ statt „JA“ (Hinweis c).

**Betriebs-Randbedingung fuer den Stichtag (Opus-Hinweis Gegencheck 3, NOCH NICHT ERLEDIGT):** In der DST-Woche **Mo 26.–Fr 30.10.2026** muss der Loop um **14:30 DE** starten (K9 verlangt `session_start 14:30`, K5 verlangt Shadow-Log ab ≤ 14:45); der **vollcheck.cjs-DST-Fix** ([[project_vollcheck_dst_fix_todo_2026-10]]) ist **vorher faellig**. Startet der Loop dort wie gewohnt 15:30, fallen per K5 **alle Momente dieser Woche** fuer H1 aus — ausgerechnet die letzte Woche vor dem Stichtag.

**Zaehlregel A3** (Opus-Auflage zum B1-Vorab-Kriterium „0 UNBEKANNT“, `q3_auto.quelle_ts === null`): betrifft den Q3-AUTO-Schatten, **nicht H1** — fuer H1 ist kein A3-Bezug noetig.

## Restunklarheit (Stand 01.10.2026 nach Opus-Gegencheck 3 — R1/R2 geschlossen, R3 aktualisiert, R4 bekannte Messgrenze, R6-a ENTSCHIEDEN; keine offene Restunklarheit zum Kriterium)

- **R1 (K3) „gezaehlte Tage“: GESCHLOSSEN (Levi 01.10.2026, Empfehlung (i)).** Basis = alle nach K9 zaehlenden Tage; leere Haelfte = NICHT AUSWERTBAR = weiter sammeln, nicht „nicht bestanden“ (Abschnitt 7a). *Urspruengliche Frage (Protokoll): ob „gezaehlte Tage“ alle K9-Tage meint oder nur Tage mit Zelle-A-Moment, und wie eine Haelfte ohne Zelle-A-Moment zaehlt.*
- **R2 (K2) „Stall“: GESCHLOSSEN (Opus (v)).** `tagesmomente.cjs` kennt keinen Stall-Exit (`exit_art` TP1-HIT/SL-HIT/OFFEN/null; null ueber `unabhaengig` ausgeschlossen). „Alles ausser TP1-HIT = kein Treffer“ ist vollstaendig; die Skript-Konstante `K2_TREFFER_EXIT_ARTEN = ['TP1-HIT']` ist damit exakt.
- **R3 (Skript): AKTUALISIERT (Opus F1).** `scripts/analyse/h1_auswertung.cjs` **existiert** im Arbeitsbaum als **Entwurf, untracked, nicht committet** *(ueberholt, B1 02.10.2026: committet als 7690d41 am 01.10.2026)* — Stand 01.10.2026 nach S5–S8/F5: **Zeilenzahl: siehe `wc -l` (die fruehere Angabe „651 Zeilen“ ist veraltet — B1 02.10.2026; sie veraltet bei jedem Edit)** (nach S1–S4: 511; vor S1–S4: 363 Zeilen laut Opus; die fruehere Angabe „nicht vorhanden“ ist ueberholt). Commit nur durch Levi ([[feedback_commit_push_nur_levi]]), vorgesehen als „Commit 2“ nach dem kurzen Opus-Gegencheck zu S5–S8/F5; der erste `--echt --auswertung`-Lauf erst danach (Opus-Gegencheck 3, Restauflage 7).
- **R4 (K5/K6) 1H-Bedingung 1: BEKANNTE MESSGRENZE, K6 massgeblich (Opus (v)), nicht nachjustieren.** `bias['1h']` im Shadow-Log ist `biasVon(--close-1h, --ema50-1h)` des Voll-Checks (`vollcheck.cjs` ~Z. 1306); ob `--close-1h` der letzte *geschlossene* 1H-Bar ist, legt vollcheck nicht fest (Operatorsache). Wird als Messgrenze im Ergebnisbericht genannt, nicht korrigiert.
- **R6-a: ENTSCHIEDEN (Levi-Entscheid (a), 01.10.2026, vor Sicht jeglicher H1-Daten ab 01.10.; momente_log ab 01.10.: 0 Zeilen, nur gezaehlt).** Stichtag der EINEN Endauswertung = **fester Kalenderstichtag Fr 30.10.2026 (Tagesabschluss)**. Kein Freeze-Ende als Stichtag, kein zweiter Ausloeser („12 Tage“), keine Verlaengerung (Variante (b) mit S6 nicht gewaehlt). Mindest-n bis dahin verfehlt → H1 „nicht belegt“, Q-ROT bleibt Veto. Hoechstens EINE Zwischenauswertung (ab Zelle A ≥ 15 Momente aus ≥ 6 Tagen) plus EINE Endauswertung am/nach dem Stichtag. Im Skript hart verankert (S5, `ENDSTICHTAG`), s. Abschnitt 7a. Die fruehere Fable-Interimsregel „Stichtag = Freeze-Ende laut Regel“ (Opus N2/F5) ist aus Datei und Skript **entfernt**. *Protokoll der urspruenglichen Frage: Opus' Bericht nannte nur Beispiele („z. B. 30.10.2026 oder nach 12 gezaehlten Tagen“), die Freeze-Ende-Regel ist eine Bedingung (Schaetzung ca. 08.10.2026), kein Datum; Opus-Gegencheck 3 empfahl den 30.10.2026 als datenunabhaengigen Stichtag, der sicher nach dem Freeze-Ende liegt.* (geaendert 02.10.2026 → 08.10.2026, s. Abschnitt 8)

## 8. Aenderung der Vorabregistrierung 02.10.2026 (Levi)

**Was:** Stichtag der EINEN Endauswertung **alt Fr 30.10.2026 (Tagesabschluss) → neu Do 08.10.2026 (Tagesabschluss, end-Lauf ab 20:05 DE)**. Levi-Entscheidung 02.10.2026 ([[project_endauswertung_08_10_vorgezogen_2026-10-02]]), Grund woertlich: „das reicht mir an Dauer“. Umgesetzt im Skript (`ENDSTICHTAG = '2026-10-08'`, `ENDSTICHTAG_URSPRUENGLICH = '2026-10-30'` nur Anzeige; Fable-Auftrag [[fable_auftrag_2026-10-02_endauswertung_08_10]] Teil A, Umsetzung [[fable_umsetzung_2026-10-02_endauswertung_08_10]]).

**Zulaessigkeit (Abschnitt 4 verlangt Levi UND dokumentierten Grund, nie nach Sicht der Ergebnisse):** (1) ausdrueckliche Levi-Entscheidung mit dokumentiertem Grund; (2) **nicht ergebnisgetrieben** — bis zur Entscheidung wurde nur der Zaehlstand gesehen (`--echt --zaehlstand` 02.10. vormittags: d = 1, Zelle A n = 0, Zelle B n = 4), **keine R-Werte von H1 gesichtet**.

**Unveraendert:** K1–K10, Checkliste 3.4, Mindest-n 3.3 (≥ 15 unabhaengige Momente aus ≥ 6 Tagen), Prior Beta(5,5), R1–R7, Z_START 01.10.2026, keine Uebersteuerung per CLI/Umgebungsvariable. Der Stichtag ist im Skript nur fuer Selbsttests als interner Funktionsparameter `auswerten({ endstichtag })` injizierbar (analog `jetztMs`).

**Pflichtzeile in jedem Echt-Lauf (A2):** `STICHPROBE KLEINER ALS BEI REGISTRIERUNG GEPLANT: Stichtag 2026-10-08 statt 2026-10-30 (Levi 02.10.2026); gezaehlte Tage d=<n> (moeglich max. 6 Handelstage 01.-08.10. statt 22 bis 30.10.), Zelle A FLOOR n=<n>, AUTO n=<n>; Zaehlung ab 2026-10-01.` — ohne R-Werte, Quoten, Posterior (JSON-Schluessel `stichprobe_hinweis`, S8-Allowlist).

**Kollisionen (Opus A5, nur dokumentiert, keine Kriterienaenderung):**

| Stelle | Kollision | Behandlung |
|---|---|---|
| 3.3 Mindest-n (≥ 15 unabh. Momente aus ≥ 6 Tagen, Zelle A) | Bis 08.10. gibt es hoechstens 6 Handelstage (01., 02., 05.–08.10.). Das Mindest-n ist nur erreichbar, wenn **jeder** Tag nach K9 zaehlt **und** Zelle A ≥ 15 Momente hat. Realistisch ist der Ausgang „zu selten, nicht belegt“ (3.6), Q-ROT bleibt dann Veto. | Unveraendert; in der STICHPROBE-Zeile und hier ausdruecklich vermerkt. |
| Abschnitt 1 „Auswertung erst nach Freeze-Ende“ | Bisher lag der 30.10. „sicher danach“. Am 08.10. greift die Freeze-Abbruchregel nur, wenn 02., 05., 06., 07. und 08.10. alle bewertbar sind (dann 10/10). Faellt ein Tag aus, liegt der 08.10. **vor** dem Freeze-Ende. | Lesart nach Levi-Entscheid 2: Der Stichtag ist ein **fester Kalendertag** (wie R6-a: „die Uhr ist datenunabhaengig“). Das Skript prueft den Freeze-Stand weiterhin nicht. **Endauswertung 08.10. auch bei < 10 bewertbaren Tagen (Levi 02.10.; Rueckfrage R-1, Default-Lesart — Levi bestaetigt).** |
| R6-a „Fortsetzung nur als neue Hypothese auf Daten ab 31.10.2026“ | Folgeaenderung des Stichtags | Wird zu „ab 09.10.2026“. Folgeaenderung, keine inhaltliche Aenderung. |
| K3 Haelften | Bei d ≤ 6 Tagen sind es 3 gegen 3 (bzw. ⌈d/2⌉) | Unveraendert. Hat eine Haelfte keinen Zelle-A-Moment, gilt „NICHT AUSWERTBAR“ (R1). |
| K9 „tagesmomente-bewertbar“ | Kopplung nur an Tagesdaten, nicht an den 30.10. | Unveraendert. Am 08.10. muessen Bars-Sicherung und `tagesmomente --datum 2026-10-08` **vor** dem end-Lauf stehen. |
| DST-Randbedingung (oben, „Betriebs-Randbedingung fuer den Stichtag“) | Die Woche 26.–30.10. liegt jetzt nach dem Stichtag | Fuer H1 obsolet. Der DST-Fix `vollcheck.cjs` bleibt aus anderen Gruenden noetig ([[project_vollcheck_dst_fix_todo_2026-10]]). |
| Zwischenauswertung (R6) | Praktisch gegenstandslos (Schwelle vor dem 08.10. nicht erreichbar) | Unveraendert. |

**Messgrenze aus dem Pruefauftrag C7 (01.10.2026, nur Zuordnungsgruende/Zaehlungen, keine R-Werte):** Alle 4 unabhaengigen FLOOR-Momente des 01.10. (16:01, 16:11, 16:21, 17:16, alle short, alle Q2 ✗) liegen in Zelle B mit dem Skript-Grund `1h-Bias nicht durchgehend == dir` — das Shadow-Log traegt fuer VC#1–#7 (15:26–15:56) `bias['1h'] = 'long'` (der letzte geschlossene 1H-Bar lag ueber der EMA50) und ab VC#8 (16:01) `short`; Trendkontext-Bedingung 1 (K6: `bias['1h']` **aller** Shadow-Eintraege von Session-Start bis zum Moment == dir) scheitert damit fuer **jeden** Short-Moment des Tages, definitionsgemaess korrekt (Bedingung 2 allein waere bei 2 der 4 Momente erfuellt gewesen). **Trendtage, die gegen den 1H-Stand der Eroeffnung laufen, fallen per Definition aus Zelle A.** Keine Kriterienaenderung — Material fuer die Besprechung nach dem 08.10.

**Ablauf am Do 08.10.2026 (Tagesabschluss, Reihenfolge verbindlich):** Loop-Stopp (`loop_stopp.cjs`) → 5m-Bars sichern (`data_get_ohlcv`, Datei muss bis ≥ 20:00 DE reichen; `protokoll_bilanz.cjs` HINWEIS C2 beachten) → `tagesmomente.cjs --datum 2026-10-08 --bars …` → skipped-/kombi-Nachtraege (`skipped_fiktiv.cjs --nachtrag … --bars`, `kombi_fiktiv.cjs --nachtrag … --bars`) → `tagesmomente.cjs --auswertung` (Block `ENDAUSWERTUNG` woertlich ins Protokoll) → **ab 20:05 DE genau einmal** `node scripts/analyse/h1_auswertung.cjs --echt --auswertung end` (vorher Exit 1; weitere Laeufe sind Wiederholungen, nicht entscheidungsrelevant) → Opus-Gegencheck → Besprechung mit Levi (Q-ROT/Regelwerk-Aenderungen erst danach; eine Q-ROT-Aenderung bei „zu selten, nicht belegt“ ist keine H1-Folge, sondern eine neue Levi-Entscheidung auf duenner Datenbasis — Opus R-2).

**Stand bei der Aenderung (02.10.2026 vormittags, nur Zaehlstand):** Freeze 5/5 bewertbare Tage (25., 28., 29., 30.09., 01.10.), AUTO 10/20, Abbruchregel 5/10; H1 d = 1, Zelle A n = 0. Commit/Push nur durch Levi ([[feedback_commit_push_nur_levi]]).
