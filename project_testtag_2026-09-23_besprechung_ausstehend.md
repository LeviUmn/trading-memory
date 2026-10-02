---
name: project-testtag-2026-09-23-besprechung-ausstehend
description: "Umsetzungskette aus der 23.09.-Analyse ABGESCHLOSSEN + COMMITTET (57c711a, 24.09.2026): V1/N2 gemessen+relativiert, E1+Fiktiv-Modus V2 (FIKTIV_ROT_GROESSE=0,25) freigegeben nach 2 Opus-Gegencheck-Runden. 24.09.-Testtag selbst ausgefallen (Loop nie gestartet), stattdessen rückwirkender Backtest mit echten Kursen (kein Trade belegbar, gegengeprüft). REGEL-FREEZE aktiv; Freeze-Ende seit 28.09.2026 neu definiert über tagesmomente.cjs --auswertung (≥5 bewertbare Testtage ab 25.09.2026 UND AUTO ≥20 unabhängige Bewegungen, Abbruch nach 10 Tagen) — B6 (≥30 fiktive Trades) nur noch Berichtsregel."
metadata:
  type: project
  originSessionId: session_2026-09-24
  modified: 2026-10-02T08:47:33.856Z
---

## STAND 24.09.2026 Abend — WICHTIG FÜR JEDE KÜNFTIGE SESSION: hier lesen, bevor am Regelwerk weitergearbeitet wird

Diese Datei ist der Zwischenstand der Umsetzungskette aus [[project_testtag_analyse_2026-09-23]]. Ablauf durchgehend: Opus schreibt Auftrag → Fable setzt um → Opus prüft gegen → Levi entscheidet. Sonnet (Hauptchat) koordiniert nur, bewertet nicht selbst (siehe [[feedback_modellwahl_trading]]).

## PLAN FÜR MORGEN (25.09.2026), explizit von Levi zum Festhalten beauftragt (24.09. abends)

**Kein weiteres Umsetzen — zuerst der fiktive Testtag zur Verifikation.** Der Regel-Freeze (Abschnitt 5) verbietet ausdrücklich weitere Regeländerungen, bevor mindestens 30 fiktive Trades aus mindestens 5 Testtagen vorliegen *(überholt seit 28.09.2026: Freeze-Ende laut Abschnitt 5, B6 nur noch Berichtsregel — Plan-Text vom 24.09. unverändert belassen)*. Reihenfolge für morgen früh:

1. **Betriebs-Voraussetzung zuerst:** Loop mit `loop_prompt.cjs --testtag fiktiv` starten — ohne dieses Flag bleibt der neue Fiktiv-Modus-Guard stumm inaktiv (erster Voll-Check schreibt `testtag_modus fiktiv` mit heutigem Datum in `vollcheck_state.json`, erst danach ist der Guard scharf).
2. **Registerpflege beim Session-Update:** veraltete Altzeichnungen aus `level_register.json` entfernen statt nur zu annotieren (Opus' konkrete Empfehlung aus dem 24.09.-Backtest — reine Datenpflege, kein Regelbruch, fällt nicht unter den Freeze).
3. **Dann den fiktiven Testtag laufen lassen** mit aktivem Fiktiv-Modus. Auswertung danach ist laut Opus' eigener Vorgabe **rein technisch** (griff der Guard, landeten Einstiege korrekt in `kombi_fiktiv_log.jsonl`, wurden Q1/Q3 übergeben — Y5, funktionierte BE/Stall-Exit fiktiv) — **nicht** ob die Trades gewannen. Das ist erst Datenpunkt 1 von mindestens 5 für die B6-Auswertung *(seit 28.09.2026: Datenpunkt für das Freeze-Ende laut Abschnitt 5; B6 nur noch Berichtsregel)*.
4. **Darf parallel laufen, ohne den Freeze zu berühren:** V5 (1H-Override-Auswertung über `oneh_shadow_log.jsonl`) kann als reiner Messschritt beauftragt werden — Ergebnis wird aber erst NACH dem Freeze umgesetzt.

**Ausdrücklich NICHT für morgen:** Anker-Nachführungsregel (aus E1), V6 (Loop-Warnung X1), Code-Filter für veraltete Register-Level — alle drei sind "erstes Paket nach dem Freeze", also erst nach dem Freeze-Ende laut Abschnitt 5 *(bis 28.09.2026: „5 Testtage/30 Trades“)*.

**Nicht vergessen (offener Punkt, nicht blockierend):** Levi könnte über X/News nachschauen, was am 24.09. um 18:15 DE den Kurs-Spike (+203 Pkt/5min) ausgelöst hat — reine Neugier/Einordnung, keine Regelfrage.

## 1. V1 (Q4 ohne 50er-Rundzahlen) + N2 (Q2/ADX-Trendmodus) — ABGESCHLOSSEN, RELATIVIERT

V1: Q4 war an live gemessenen Tagen (17./21.09.) in 0/10 Fällen entscheidend für ROT — NICHT live/gating umsetzen. N2: Die seit 22.09. laufende Y4-b-Logik neutralisiert Q2 bereits in allen 4 gemessenen ROT-Fällen (ADX 48,5-54,9) — Opus korrigierte die eigene Priorisierung. Übrig bleibt eine Verbundblockade aus **Q1+Q4 gemeinsam** an Trendtagen. ADX-Schwellenstaffel 30/35/40 führte zu 0 Ampelwechseln (keine PASS-Fälle im Band 30-45).

## 2. Levis EV-Frage + Overfitting-Warnung → Entscheidung für E1+V2

Levi: bei TP1 1:1/TP2 min. 1:2 reicht doch <50% WR — warum so restriktiv? Opus mit echten `trades.db`-Zahlen (43 Trades): realer Payoff nur 0,84:1, TP2 nur 3/43, reale Gewinnschwelle ~56% TP1-Quote, Phase 3 bisher −57,77€. Levi hat architektonisch recht (Veto verhindert auch Lernen), Rechnung "50% reicht" stimmt praktisch nicht. Eine nachträglich gebildete Kombi-Hypothese (n=2 Beobachtungen) wurde als Overfitting-Risiko erkannt und zu vollem **V2** (Q-ROT als Sizing, nicht nur enge Kombi) erweitert. **FIKTIV_ROT_GROESSE = 0,25** freigegeben.

## 3. E1 + Fiktiv-Modus V2 — FERTIG, 2x GEGENGEPRÜFT, COMMITTET (57c711a)

**E1-Kernbefund:** Von 258 qualifizierten Momenten (Dual-Gate 2/2 + 1H nicht dagegen, 6 Tage) erreichten nur 75 (29%) einen bewerteten Live-Gate-Lauf, davon 26 PASS (kein GRUEN). **8c2 UNTAUGLICH kam an KEINEM Tag als Blocker vor (0/258)** — strukturell erklärt: seit `--sl-auto` (ab 11.09.) ist 8c2 in allen 578 Vorprüfungen PASS. Hauptblocker ist **TP1-Fenster LEER** (107 Fälle, 41%) — und davon binden **100% am Struktur-Anker** (0× Cluster-Kante, 0× 8c-Floor). **60% dieser Anker sind älter als 90 Minuten** (Median 111 Min, Max 264). → Das Problem ist überwiegend **veraltete Anker**, nicht die Anker-Definition. Anker-Varianten: V-ERST (erster Swing nach Extrem) 0 SL-Treffer bei 9 TP1, aber viele noch offen am Horizont; V-JUENG (jüngster Swing) mehr Fenster (81 vs. 68) aber auch mehr SL (11), dünner Rand (+0,29R).

**Fiktiv-Modus V2 (`scripts/kombi_fiktiv.cjs` + Erweiterungen in `gate_check.cjs`):** Kombi-Ampel (Y4-b-Trendmodus + Q4-NEU-A) steuert an fiktiven Testtagen die Positionsgröße (GRUEN 1,0/GELB 0,5/ROT 0,25) statt zu blockieren. Hard-Guard fail-closed, byte-identische Ausgabe an Echtgeld-Tagen (Golden-Files aus 941a221, von Opus selbst unabhängig aus dem Commit neu erzeugt und byte-identisch bestätigt), niemals Schreibzugriff auf `trades.db`/Order-Pfad. Zwei Gegencheck-Runden: Runde 1 mit 8 Auflagen (B-1 bis B-8, u.a. Golden-Files statt Live-HEAD-Vergleich, Stall-Exit-Konvention `r_primaer`/`r_ohne_stall`, Doppelbuchungs-Fix über `skipped_id`), Runde 2 **volle Freigabe ohne offene Mängel**. `npm test`: 345/345 grün.

**COMMITTET 24.09.2026 abends** als `57c711a` (28 Dateien, 4148 Zeilen) — `scripts/gate_check.cjs`, `loop_prompt.cjs`, `protokoll_bilanz.cjs`, `tests/trading_scripts.test.js`, `scripts/kombi_fiktiv.cjs`, `tests/fixtures/kombi_v2_golden/`, `scripts/analyse/*.cjs`. **NICHT** committet: `scripts/analyse/out/` (Auswertungsartefakte).

## 4. 24.09.-Testtag: AUSGEFALLEN (Loop nie gestartet) — stattdessen Backtest mit echten Kursen

Levi war zwischen 15:10 und 18:51 DE nicht am Rechner, Sonnet hat nur erinnert (Trigger-Regel: kein automatischer Start ohne explizites "start update dich"), keine Bestätigung kam. Der Testtag ist damit für den 24.09. entfallen — kein Live-Loop, keine vollcheck_log/gate_check_log/oneh_shadow_log-Einträge.

**Stattdessen:** Rückwirkender Backtest 15:30-20:00 DE mit echten TradingView-Kursen (NAS100 5m/15m/60m + QQQ 15m, von Sonnet frisch geholt und als `..._2026-09-24.json` gesichert). **Ergebnis, von Opus gegengeprüft (eigene Nachrechnung, nicht nur Lektüre):**
- **Kein vollständiger, regelkonformer Trade belegbar.** V-ERST: 0 offene Fenster vor 20:00 (Definitionsartefakt, Tagesextrem lag vormittags). V-JUENG: 3 Momente erreichten die TP1-Stufe — 15:40 Short TP1-HIT +1R, aber NUR wegen fehlender aktueller Tagespivots im Register (R-IST = 23.09.-Stand, 1388 Min alt — **Backtest-Artefakt**, live hätte die Register-Frische-Sperre ab 90 Min gegriffen). Mit realistischem Register (R-REKON): Q4 NEIN, Fall wird ROT. 18:20/18:25 Short beide SL-HIT −1R, klar ROT.
- **Spike-Kerze 18:15-18:20 (+203 Pkt/5min) ist real** (von Opus gegengerechnet), Ursache nicht einordenbar (keine News-Logs, X-Feed in der Session nicht verbunden) — **offener Punkt für Levi**, ggf. selbst nachschauen was um 18:15 DE lief.
- Mit R-REKON zusätzlich 8 weitere Short-Fenster 17:35-18:15, alle vom Spike ausgestoppt: sequentiell **−2R**.
- **Opus' Kernsatz:** "Heute hätte das Blockieren Geld gespart" — Gegenstück zum 21.09., wo die Blocker Gewinner verhindert haben. Dieses Wechselspiel bestätigt, dass der V2-Weg mit kleinen fiktiven Größen richtig ist, kein pauschales Lockern.
- **Neuer kleiner Befund (im Freeze ohne Codeänderung lösbar):** `eligibleLevels()` filtert als "veraltet" markierte Register-Einträge NICHT heraus — sie zählen weiter als Q4-Gegenlevel/TP1-Kandidat. **Opus' operative Empfehlung: beim nächsten Session-Update solche Altzeichnungen aus dem Register ENTFERNEN statt nur zu annotieren** (reine Registerpflege, kein Regelbruch). Ein Code-Filter kommt ins erste Paket nach dem Freeze.

## 5. REGEL-FREEZE — aktiv, gilt bis Freeze-Ende laut Definition unten (seit 28.09.2026; B6 ist nur noch Berichtsregel)

- **Freeze-Ende (Levi-Entscheidung 28.09.2026, Opus-Empfehlung — ersetzt das bisherige B6-Ende):** „Der Regel-Freeze endet, sobald `tagesmomente.cjs --auswertung` mindestens 5 bewertbare Testtage zählt (Loop mit ≥ 20 Voll-Checks, 5m-Bars lückenlos ab 14:30 DE bis 20:00 DE) UND die Variante AUTO mindestens 20 unabhängige Bewegungen aufweist. Ist Letzteres nach 10 bewertbaren Testtagen nicht erreicht, endet der Freeze trotzdem. Die Variante gilt dann als ‚zu selten, nicht belegt‘. Danach gilt eine Variante oder Regeländerung nur als belegt bei Ø-R (RR-1-Ausgang, unabhängige Bewegungen) ≥ +0,10 UND Ø-R ≥ 0 auch ohne den besten Tag. Bis dahin unverändert: keine Änderung an Gates, Schwellen oder Q-Faktoren; Ausnahme nur messverfälschende Bugs; höchstens eine Live-Änderung pro Testtag; reine Messungen und Schatten-Zeilen sind erlaubt.“ **Gezählt werden Testtage ab 25.09.2026** (Levi 28.09.2026; die 10-Tage-Obergrenze zählt ebenfalls ab 25.09.; die Tage 11./15./21./23.09. bleiben in `scripts/momente_log.jsonl` als Referenz sichtbar, zählen aber nicht — `--auswertung` hat deshalb Default `--seit 2026-09-25`). „Lückenlos“ heißt im Code: höchstens 1 fehlende 5m-Bar im Fenster 14:30–20:00 DE (Details [[feedback_tagesabschluss]], Abschnitt Tagesmomente). **Stand 30.09.2026 vormittags (nach dem offiziellen tagesmomente-Lauf für den 29.09., siehe [[project_testtag_analyse_2026-09-29]]):** 3 bewertbare Testtage (25.09. + 28.09. + 29.09.), AUTO 6/20 unabhängige Bewegungen (Σ −1,67 R), 3/10 der Tage-Obergrenze → FREEZE-ENDE ERREICHT: nein. **Levi-Entscheidung a) 30.09.2026: Teiltage zählen** — 29.09. zählt mit 43 % Abdeckung (Slots 15:30–20:00 mit Voll-Check), 25.09. zählte bereits mit 50 %; die Abdeckung wird seit A2 (30.09.) in `--auswertung` je Tag angezeigt („TEILTAG < 80 %, zaehlt“) plus Referenzzeile „Ohne Teiltage … NICHT massgeblich“ (ohne 25.+29.09.: AUTO n 3 Σ +0,03, FLOOR n 8 Σ −2,95, ALT n 1 +0,32). Bei bisher 2,0 AUTO-Bewegungen je bewertbarem Tag fehlen für AUTO ≥ 20 noch ~7 Tage; bei Vollzählung aller Handelstage fällt der 10. bewertbare Tag (Abbruchregel) auf Do 08.10.2026 — Obergrenze und AUTO = 20 treffen ungefähr gleichzeitig ein, eher knapp. (Vorheriger Stand 28.09. abends: 2/5, AUTO 5/20.)
- Die bisherige B6-Regel (≥30 fiktive Trades aus ≥5 Testtagen) bleibt als **Berichtsregel** für `kombi_fiktiv.cjs --auswertung` bestehen, beendet den Freeze aber nicht mehr.
- **A1/A2 (Fable-Auftrag 28.09.2026, Opus-Vorlage):** A1 = `scripts/tagesmomente.cjs` + `scripts/anker_auto.cjs` (Tagesabschluss-Pflicht, reiner Nachlauf-Report, Varianten ALT/AUTO/FLOOR). A2 = AUTO-Anker als Schatten-Zeile in `vollcheck.cjs --nas-bars-5m` (Branch `a2-auto-anker-schatten`, Übernahme ins Haupt-WD erst nach Loop-Stopp + Opus-Nachprüfung; Übernahme-Runbook liegt bei Sonnet). Abnahme A1 (a): 98,24 % formal (sl_dist_atr 167/170; fenster_offen 168/170 = 98,82 %), 167/167 ohne die 3 Cluster-Fälle (11.09. 17:54/18:00/18:10, Log „sichere Cluster-Kante bindet“).
- **Auswertung nach jedem Testtag ist rein technisch** (griff der Guard, Log-Einträge korrekt, Q1/Q3 übergeben, BE/Stall-Exit funktioniert) — keine Ergebnisbewertung, bis das Freeze-Ende laut Definition oben erreicht ist (`tagesmomente.cjs --auswertung` druckt „belegt“ je Variante bis dahin als „vorlaeufig“).
- **Eine Änderung pro Testtag.**
- **Darf parallel laufen (reine Messung):** V5 (1H-Override über `oneh_shadow_log.jsonl`) — Umsetzung erst nach dem Freeze.
- **Wartet bis nach dem Freeze:** V6 (Loop-Warnung X1), Anker-Nachführungsregel aus E1 (Kandidat: max. Anker-Lebensdauer / Pflichterneuerung nach neuem Impuls-Extrem, NICHT einfach V-JUENG — dünner Rand), Code-Filter für veraltete Register-Level.
- **Pflicht an jedem Testtag-Abend:** 5-Min-Bars sichern (dated Datei).
- **02.10.2026 (Levi): Endauswertung auf 08.10.2026 vorgezogen; Schwellen unveraendert; Ausgabe-Block ENDAUSWERTUNG in `tagesmomente --auswertung`** (fester Kalendertag — gilt auch bei < 10 bewertbaren Tagen, Lesart R-1; Variantenurteile dann „unter Vorbehalt“; Stand 02.10. vormittags: 5/5 bewertbare Tage, AUTO 10/20, Abbruchregel 5/10; Details [[project_endauswertung_08_10_vorgezogen_2026-10-02]], [[fable_umsetzung_2026-10-02_endauswertung_08_10]]).

## 6. Offene To-dos für die nächste Session

1. **Betriebs-Voraussetzung für den nächsten fiktiven Testtag:** Loop mit `loop_prompt.cjs --testtag fiktiv` starten (erster Voll-Check schreibt `testtag_modus fiktiv` in den State, erst danach ist der Guard scharf). Tagesabschluss um `protokoll_bilanz.cjs --kombi-log` + `node scripts/kombi_fiktiv.cjs --nachtrag <id> --bars <Datei>` je offenem Eintrag ergänzen — seit 28.09.2026 in `feedback_tagesabschluss.md` dokumentiert (Abschnitt „Tagesmomente-Messung + Kombi-Fiktiv-Nachtrag“), zusammen mit dem neuen Pflichtschritt `tagesmomente.cjs`.
2. **Vor dem nächsten Session-Update:** veraltete Altzeichnungen aus `level_register.json` entfernen (Opus' Empfehlung aus dem 24.09.-Backtest).
3. **Offener Punkt:** Levi könnte über X/News nachschauen, was um 18:15 DE (24.09.) den Spike ausgelöst hat — nicht blockierend, nur zur Einordnung.
4. V5 (1H-Override) kann als Messschritt parallel beauftragt werden, Ergebnis erst nach dem Freeze nutzen.
5. Cron-Erinnerungen sind session-gebunden — bei neuer Session die Tagesablauf-Fixpunkte aus [[feedback_trading_zeitfenster]] im Blick behalten bzw. neu setzen.
