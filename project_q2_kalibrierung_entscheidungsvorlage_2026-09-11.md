---
name: project_q2_kalibrierung_entscheidungsvorlage_2026-09-11
description: "OFFEN (Levi-Entscheidung): Q2-Impuls-Reifegrad und Q4-Runway neu verankern? Aus der Opus-Analyse Testtag 11.09.2026 (V1a-d, V10): die verbindliche Q2-Formel impuls_pkt/Live-ATR war am Trendtag ab Loop-Sekunde 1 unerfuellbar (Median 14,2x bei Schwelle 1,5x) und bestraft fallende ATR; Q4 war 12/12 null und zirkulaer aus Q2 begruendet. Drei Optionen fuer Q2 (Rollentausch auf EMA50-Anker / Formel reparieren / Schattenmodus zuerst) und zwei fuer Q4 (maschinell rechnen / streichen). Keine Regelaenderung ohne Levi (feedback_dont_change_running_system, Regelaenderungs-Tempo-Bremse)."
metadata:
  node_type: memory
  type: project
  status: UMGESETZT 14.09.2026 (Fable) + Opus-Gegencheck FREIGEGEBEN MIT AUFLAGEN (kein Blocker, 2 Opus-Fixes, ungepusht — Levi committet); Auflage N1 umgesetzt 14.09. (Q4 UNKLAR bei Register-Luecke) + Opus-Gegencheck Runde 2 ABGESCHLOSSEN (kein Blocker; N1b nachgezogen: Luecke gilt auch bei "naechstes Level jenseits TP1", Tests 73/73)
  originSessionId: session_01QEYrdXFdPSQJXVMQJHSUB4
  modified: 2026-09-14T10:52:04.851Z
---

# Q2/Q4-Kalibrierung: Entscheidungsvorlage (offen seit 11.09.2026)

**Die Frage in einem Satz:** Soll der Q2-Faktor „Impuls-Reifegrad" (Schwelle ≤1,5× ATR, `gate_check.cjs --impuls-reifegrad-atr`, Regel in [[feedback_chartanalyse]] / [[feedback_live_trading]] 7b1a) einen anderen Anker bekommen — und was passiert mit Q4?

**Warum das entschieden werden muss:** Am 11.09.2026 (Trendtag +460 Pkt) war Q2 der alleinige Blocker aller 10 Setup-Momente (12/12: Q1 ✓, Q3 ✓, Q4 UNKNOWN, Q2 ✗ → 2/4 ROT; mit Q2 ✓ wäre es GELB = halbe Position). Die Formel misst nicht, was ihr Name sagt: Zähler = volle Impuls-Amplitude (entry-unabhängig, den ganzen Tag konstant 429,75/450,35 Pkt), Nenner = Live-ATR (fiel von 47,0 auf 20,6). Ergebnis 9,1x–16,5x, Median 14,2x — die Schwelle 1,5x wäre erst bei ATR ≥ 300 erfüllbar. Der informative Zweitanker (EMA50-Abstand) lag in denselben Fällen bei −0,17x bis 0,55x (2× 1,53x), also differenzierend um die Schwelle. Gegenbefund 09.09.: 4 Q-ROT-Ablehnungen nachträglich korrekt (~2,9R vermieden) — dort war Q2 aber nicht der entscheidende Faktor (Q1/Q4 kippten). Details: [[project_testtag_analyse_2026-09-11]].

Was NICHT zur Debatte steht: die Ampel-Logik (4/4 GRÜN, 3/4 GELB, ≤2 ROT), die Hard-Gates (RR, 8c, 8b1, 8c2) und die Rolle des Q-Scores als Sizing-Signal ohne Veto-Charakter über die Hard-Gates hinaus.

## Q2 — Option A: Rollentausch (Opus-Vorschlag V1c)

EMA50-Abstand (heute `--impuls-reifegrad-atr-emaanker`, GEMESSEN KEIN GATE) wird verbindlicher Q2-Anker, Schwelle 1,5× ATR bleibt; der Impuls-Ursprung-Anker wird informativer Zweitanker.
- **Für:** Werte seit 25.08. mitgeloggt (echte Verteilung vorhanden), entry-abhängig (misst den Abstand des Entrys zur dynamischen Struktur), reagiert richtig auf Konsolidierung (Abstand schrumpft, wenn der Kurs zur EMA50 zurückkommt). Kleinster Eingriff: zwei Parameter tauschen die Rolle, keine neue Messung im Loop.
- **Gegen:** EMA50-Abstand ist ein anderes Konzept als „Reifegrad" (Trendausdehnung seit Ursprung) — die Regel würde umbenannt, nicht kalibriert. Bei Retests direkt an der EMA50 wäre Q2 fast immer erfüllt; die Filterwirkung gegen „spät im Impuls" verschiebt sich auf Q1/Chasing (V3, jetzt berechnet).

## Q2 — Option B: Formel reparieren (Opus V1a+V1b)

Zähler = |Entry − Impuls-Ursprung| (statt voller Amplitude), Nenner = ATR zum Impuls-Start eingefroren (`atr_bei_impuls_start`, neues State-Feld in `vollcheck.cjs`), Schwelle neu zu eichen.
- **Für:** behält das Konzept „wie weit ist die Bewegung gelaufen, wenn ich einsteige"; beseitigt beide Artefakte.
- **Gegen:** Am 11.09. ergäbe auch das 11,85x (VC#35) — die Schwelle 1,5x bleibt um eine Größenordnung falsch; eine neue Schwelle müsste aus Daten kommen, die es (mit dieser Formel) noch nicht gibt. Bedeutet mindestens ~10–15 Handelstage Messung vor Verbindlichkeit. `vollcheck.cjs` müsste den Impuls-Ursprung zuverlässig führen (F1/G1-Fixes vom 09.09. zeigen, wie fehleranfällig das ist).

## Q2 — Option C: Schattenmodus zuerst (Opus V1d), Entscheidung A/B vertagt

`gate_check.cjs` rechnet beide Varianten (alt bindend, neu = Option A oder B), loggt beide Q2-Status + resultierende Ampeln in `gate_check_log.jsonl`/Fiktiv-Log; nach ~10 Handelstagen Kreuztabelle (wie [[project_nasdaq_official_feed_idee]]).
- **Für:** entspricht der Regeländerungs-Tempo-Bremse ([[feedback_live_trading]]) und „ein Datenpunkt kippt keine Regel". Kein Risiko für laufende Tage.
- **Gegen:** setzt voraus, dass Levi *vorher* festlegt, welche neue Variante geschattet wird (A oder B) — sonst wird nur die alte weiter geloggt. Und: Solange alt bindend bleibt, blockt Q2 an jedem Trendtag weiter alle Setups (der 11.09.-Zustand bleibt).
- **Hinweis Fable:** Technisch ist der Schattenmodus eine App-Anpassung (~40 Zeilen in `evaluateQScore` + Log-Felder). Nicht vorab gebaut, weil die Wahl des Schatten-Ankers die eigentliche Regelentscheidung ist.

## Q4 — Runway (Opus V10)

12/12 `runway_ratio: null`, Begründung im argv zirkulär („Impuls bereits 13,56x ATR gereift, kein Runway-Ziel") — Q4 ist faktisch abgeschaltet, der Score ein 3-Faktoren-Score mit Deckel 3/4.
- **Option Q4-a (APP-ANPASSUNG):** `gate_check.cjs` rechnet runway_ratio maschinell = Abstand Entry → nächstes Gegenlevel aus dem Level-Register jenseits TP1, geteilt durch TP1-Distanz; `--runway-ratio` nur noch Override. Am 09.09. traf das Register-Gegenlevel (Rundzahl 29350) das Tief auf 11,95 Pkt — bester Q4-Kalibrierungspunkt bisher.
- **Option Q4-b (REGELÄNDERUNG):** Q4 streichen, Ampel auf drei Faktoren eichen (3/3 GRÜN, 2/3 GELB, ≤1 ROT).
- In beiden Fällen: `--grund-runway-ratio` darf das Q2-Ergebnis nicht als Begründung akzeptieren (Textprüfung auf „gereift"/„Reifegrad" → Exit 1) — das ist eine reine App-Anpassung und kann unabhängig umgesetzt werden.

## Empfehlung Fable (unverbindlich, Levi entscheidet)

Q2: **Option C mit Option A als Schatten**, verbindlich nach 10 Handelstagen nur, wenn die Kreuztabelle zeigt, dass A die 09.09.-Ablehnungen nicht aufhebt. Q4: **Q4-a** (maschinell rechnen), weil die Datenbasis (Level-Register) bereits existiert und die Frage „gibt es Platz bis zum nächsten Gegenlevel" die einzige ist, die Q2 nicht ohnehin schon stellt. Aufwand nach Entscheidung: ~1 Fable-Session, kein Loop-Prompt-Eingriff.

**Bis zur Entscheidung gilt unverändert:** Q2 = Impuls-Ursprung-Anker, Schwelle 1,5× ATR, verbindlich; Zweitanker informativ. Keine Änderung in `gate_check.cjs` an der Q-Logik vorgenommen (11.09.2026).

## Opus-Stellungnahme 14.09.2026 (auf Levis Anfrage, Levi entscheidet weiterhin)

**Nachrechnung beider Tage mit Option A (Q2 = |Entry − EMA50 5min| / ATR, Schwelle 1,5):**
- 09.09. VC#1 (Short, SL-Hit −1R): |29.338,35 − 29.436,9| / 46,2 = **2,13x → Q2 NEIN → bleibt 2/4 ROT** (Verlust weiter vermieden). VC#9/10/12: EMA-Abstand ≈0,95x → Q2 JA, aber Q1 NEIN + Q4 NEIN (Runway 0,48) → **bleibt 2/4 ROT**. A hebt keine der 4 korrekten Ablehnungen auf.
- 11.09.: EMA-Anker 10/12 ≤1,5x (2× 1,53x) → Q2 JA → mindestens 3/4 GELB; VC#35 (einziger realistischer Trade, ~+0,6R) wäre freigegeben worden.

**Zusatzbefund Opus:** Q2 hatte an beiden Tagen verschiedene Zähler — 09.09. |Entry − lokaler Schwung-Ursprung| (4,96x/1,65–1,81x), 11.09. volle Tagesamplitude `impuls-pkt` aus `vollcheck.cjs` (429,75/450,35 Pkt → 9–16x). Der Wert ist eine frei eingetippte Zahl mit interpretierbarem „Ursprung" — Kerndefekt ist die Eingabe, nicht nur die Schwelle. Option B erbt genau das. Außerdem: Q1/Q3 sind Selbstauskunft (yes/no); am 09.09. hat Q4 (manuell gerechnet) zusammen mit Q1 die richtigen Ablehnungen getragen, am 11.09. war Q4 tot.

**Empfehlung Opus (weicht in einem Punkt von Fable ab):** A **verbindlich auf Testtagen** statt 10 Tage Schatten mit altem Anker bindend — Testtage sind fiktiv (0 € Risiko), der Testtag mit A bindend IST der Schattenmodus; alte Formel hätte als Schattenwert keine saubere Definition. Aber **A nur zusammen mit Q4-a** (sonst wird fast jedes Trend-Setup GELB, weil Q1/Q3 selbst deklariert sind). Umsetzungsliste: (1) gate_check rechnet Q2 selbst aus `--ema50-5min` + Entry + ATR (keine freie Zahl), Impuls-Ursprung-Wert nur noch Info; (2) Q4 maschinell aus Level-Register (nächstes Level in Trade-Richtung zwischen Entry und TP1, TP1-Level ausgenommen), `--runway-ratio` nur Override mit Grund, Zirkel-Textprüfung; (3) Nachtrag `skipped_fiktiv.cjs --nachtrag` im Tagesabschluss als harte Pflicht (12/12 null am 11.09. — ohne Nachtrag keine Auswertung); (4) Vorab festgelegtes Prüfkriterium vor Echtgeld: nach ≥15 unabhängigen Q-bewerteten Setup-Momenten Summe R der A-Freigaben > 0 und A-Freigaben mit SL-Hit ≤30 Min nicht häufiger als 1 von 3 — sonst Schwelle 1,0x oder zurück. Kein neuer Faktor, Ampel unverändert.

## ENTSCHIEDEN 14.09.2026 (Levi): Opus-Empfehlung angenommen

Levi: „Ja bitte so an Fable geben wie du gesagt hast Opus". Q2 = Option A verbindlich (auf Testtagen, skriptseitig aus `--ema50-5min`), Q4 = Q4-a maschinell aus Level-Register, Nachtrag-Pflicht im Tagesabschluss, Prüfkriterium vor Echtgeld wie oben. Fable-Umsetzung beauftragt 14.09.2026, danach Opus-Gegencheck (Autor≠Prüfer), Levi committet selbst.

## Umsetzung Fable 14.09.2026 (Levi-Entscheidung, 4 Punkte; Opus-Gegencheck offen, Levi committet)

**Punkt 1 — Q2 = EMA50-Anker (`scripts/gate_check.cjs`):** neuer A3-Pflichtparameter `--ema50-5min <Preis>` (Ausweg `--grund-ema50-5min` → Q2 UNKNOWN; keine Zahl/Komma → Exit 1). `q2Emaanker()` rechnet `|Entry − EMA50| / ATR`, `evaluateQScore()` hängt Q2 nur noch daran (Label „Reifegrad EMA50-Anker (<=1,5x ATR)", Detail mit Rechnung + `Impuls-Ursprung-Wert X (informativ)`), Schwelle `Q2_ATR_MAX = 1.5` unverändert. `--impuls-reifegrad-atr` ist aus `MEASUREMENT_REQUIRED` gestrichen (nur Info). `--impuls-reifegrad-atr-emaanker` live = Hard-Exit 1 mit Hinweis; im Batch bleibt das JSON-Feld `impulsReifegradAtrEmaanker` Fallback für Q2, wenn `ema50_5min` fehlt (gleiche Formel, GC-5 „nicht rückwirkend"), beides fehlend = UNKNOWN. ENTRY-REIFEGRAD-ESKALATION und die Punkt-9-Selbstberechnung von `--dual-gate-q2-budget-pct` laufen auf dem neuen Wert. Neue Pflicht-Ausgabezeile `Q2-Rohwert (Option A …)` auch bei FAIL/UNKNOWN. Fiktiv-Log-Felder `q2_anker`, `ema50_5min`, `q2_emaanker_atr`, `impuls_reifegrad_atr` (Info), `testtag_modus`.

**Punkt 2 — Q4 aus dem Register (`runwayFromRegister()`):** live wird das Register jetzt IMMER geladen (bestehender Hard-Exit-Pfad bei fehlend/unlesbar/>90 Min, Frische-Warnungen unverändert). Gegenlevel = alle `eligibleLevels` (NAS100-Preisraum, mit `price`: Pivots, PDH/PDL, Session-Extrema, Rundzahlen, Fib, Box-Kanten, eigene Zeichnungen; Wick-Zonen fallen ohne `price` heraus, QQQ per symbol-Filter), strikt jenseits Entry (±2 Pkt um den Entry ausgenommen — Basislevel eines „Registerlevel"-Entrys ist kein Gegenlevel), TP1-Level ±2 Pkt ausgenommen; kein Level vor TP1 → nächstes jenseits TP1 (≥1 → ERFÜLLT); gar keins → ERFÜLLT mit Vermerk. Rundzahlen zählen nur aus dem Register (arithmetische 50er zwischen Entry und TP1 ohne Registereintrag → WARNUNG, kein Gate). `--runway-ratio` nur Override: ohne `--grund-runway-ratio` Exit 1, Grund mit „gereift"/„Reifegrad" (case-insensitive) Exit 1, gültiger Grund → Override gilt, gerechneter Wert bleibt sichtbar/geloggt (`runway_ratio`, `runway_ratio_gerechnet`, `runway_gegenlevel`, `runway_override`, `runway_override_grund`); `--grund-runway-ratio` ohne Wert → Hinweis „wirkungslos". Batch lenient unverändert (JSON-Feld), `--batch-strikt` = Live-Rechnung, Override-Verstoß dort nur Warnung. Kalibrierung 09.09. VC#9 gegen die reale Registerkopie: Rundzahl 29.350 → **0,48 → NICHT ERFÜLLT** ✓.

**Punkt 3 — Nachtrag-Pflicht (`scripts/protokoll_bilanz.cjs`, `skipped_fiktiv.cjs`):** Tagesabschluss-Prüfskript ist `protokoll_bilanz.cjs` (Faktenprotokoll-Abschluss), nicht `abschluss.cjs` (Wochen/Monat). Offen = ohne `entscheidung` ODER `hypothetisches_ergebnis`; Abweichung (Exit 1, nicht bei `--nur-zaehlung`) mit fertigen Kommandozeilen je Eintrag (`--json`: `skippedFiktiv.offeneIds`/`nachtragKommandos`). `--nachtrag` kennt `--sl-hit-min`. Regeltext: [[feedback_tagesabschluss]] (Fiktiv-Log-Absatz, Abschlusszeile `Fiktiv-Log nachgetragen`).

**Punkt 4 — Prüfkriterium + Auswertung:** Regeltext in [[feedback_live_trading]] 7b1a („Prüfkriterium Option A vor dem nächsten Echtgeld-Trade"). `skipped_fiktiv.cjs --auswertung-q2a [--seit]`: zählt nur Einträge mit `q2_anker` EMA50 und Ampel GRÜN/GELB/ROT, Wiederholung desselben (datum, dir, entry, sl, tp1) = 1 Moment, druckt n/15, Freigaben mit Summe R, SL-Hits/schnelle SL-Hits (≤30 Min, aus `sl_hit_min`), Urteil erfüllt / nicht erfüllt / zu wenig Daten / nicht auswertbar (Nachträge fehlen). V2-Logger erweitert: bei `vollcheck_state.json` `testtag_modus: fiktiv` (heutiges DE-Datum; Testpfad `--vollcheck-state`) werden auch PASS + GELB/GRÜN geloggt (`ablehnungsgrund: null`, quelle „FREIGEGEBEN"), Dedupe wiederverwendet; an echten Tagen unverändert (Freigaben = Trades in trades.db).

**Templates/Regelwerk:** [[feedback_live_trading]] 7b1 Schritt 5 + `Gate-Check ausgeführt:`-Zeile (`--ema50-5min` rein, Altparameter raus, `--runway-ratio` optional als Override), Befüllungspflicht-Listen (Punkt 2b, Schritt 5, A3-Absatz), Q2-/Q4-Bullets in 7b1a ersetzt (B3-Absätze zusammengefasst als Historie), Herkunft der Q-Werte, `dual_gate_q2_budget_pct`-Definition, P2-Eskalation; [[feedback_chartanalyse]] Absatz „Impuls-Reifegrad in ATR" (Stand 14.09.); `scripts/loop_prompt.cjs` Trigger-Zeile nennt Q2/Q4/Nachtrag-Pflicht. `vollcheck.cjs` reicht keine Q-Parameter durch (nur `--sl-vorpruefung`) — unverändert.

**Tests:** `tests/trading_scripts.test.js` neuer describe-Block „Q2/Q4-Kalibrierung 14.09.2026" (10 Tests), `GATE_BASE` auf `--ema50-5min` (Default = Entry) umgestellt; **70/70 grün** (vorher 60/60). Echtlauf VC#42 (Long 29.423,65, SL 29.358,7, TP1 29.500, ATR 27,3, EMA50 29.412,2) in einer Sandbox-Kopie gegen die reale `level_register.json` (Zeitstempel aufgefrischt, keine echten Logs berührt): `Q2 = JA … 0.42x ATR (|Entry 29423.65 - EMA50 29412.2| = 11.45 Pkt / ATR 27.3)`, `Q4 = NEIN … Runway-Ratio 0.35 = Distanz Entry->Gegenlevel Rundzahl 50er @ 29450 (26.35 Pkt, vor TP1, 3 Register-Level …) / 76.35 Pkt` → **3/4 GELB (halbe Position)** — Opus' Nachrechnung „mindestens GELB" bestätigt, Q4 fällt am realen Register durch (29.450 vor TP1 29.500).

## Opus-Gegencheck 14.09.2026 — FREIGEGEBEN MIT AUFLAGEN

Vollbericht: [[project_gegencheck_q2q4_fable_umsetzung_2026-09-14]]. Kein Blocker; Q2 (2,13x / 0,42x), Q4 (0,35 gegen das reale Register) und das 7b1-Kommandotemplate live nachgerechnet/ausgeführt, alle vier Missbrauchspfade zur alten Q2-Formel enden in Exit 1 bzw. UNKNOWN. **Offene Auflage N1 (Levi entscheidet vor dem nächsten Testtag):** Q4 „kein Gegenlevel im Register" = ERFÜLLT, obwohl die eigene Warnung im selben Lauf belegt, dass das Register die Strecke nicht abdeckt — Datenlücke hebt 3/4 GELB auf 4/4 GRÜN (Empfehlung: UNKLAR statt ERFÜLLT, wenn die 50er-Warnung feuert). Von Opus selbst gefixt (reine Hinweiszeilen, kein Gate): Kopplung `--grund-ema50-5min` → `--dual-gate-q2-budget-pct` (Exit 1 am falschen Feld) und stummer Verlust von Freigaben ohne deklarierten `testtag_modus`. Tests 72/72.

**Offen für den Opus-Gegencheck (Fables eigene Liste, alle geprüft):** (1) Entry-Toleranz ±2 Pkt bei der Gegenlevel-Suche (Fable-Zusatz zur Vorgabe „strikt jenseits Entry"); (2) Register wird live immer geladen — Aufruf ohne Register bricht jetzt auch ohne `--tp1/--tp2-level-price` ab; (3) Batch-Fallback auf `impulsReifegradAtrEmaanker` für Q2; (4) Freigabe-Logging nur an Testtag-Modus fiktiv, Auswertung liest nur das Fiktiv-Log; (5) Q4 ERFÜLLT ohne Zahl bei „kein Gegenlevel" (DB/Log `runway_ratio` = null); (6) `--vollcheck-state` als neuer optionaler gate_check-Parameter (nur Testpfad).
