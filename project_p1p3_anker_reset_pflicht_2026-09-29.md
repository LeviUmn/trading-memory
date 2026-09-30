---
name: project_p1p3_anker_reset_pflicht_2026-09-29
description: "P1-P3 (Anker-Seitenpruefung, A2-Schattenzeile, P3-Reset-Pflicht mit 30-Min-Begruendung) aus Opus-Analyse 28.09. UMGESETZT+COMMITTET 29.09. (ee27c70), 3 Opus-Gegencheck-Runden FREIGEGEBEN; 29.09. = erster Testtag mit P3"
metadata:
  node_type: memory
  type: project
  originSessionId: 06006c65-4ed0-4041-9028-70709ef145bd
  modified: 2026-09-30T10:02:49.252Z
---

# P1-P3 Anker-Seitenpruefung / Reset-Pflicht (Commit ee27c70, 29.09.2026; am 30.09. gepusht — im Incident ohne Levi-Auftrag, s. [[feedback_commit_push_nur_levi]])

**Anlass:** Levi fragte 29.09., ob der Anker "jetzt schon oefter das Problem" war. Opus-Analyse: Ja bei verpassten Entries (alle 5 Testtage 17./21./23./25./28.09.), aber nicht beim verpassten Gewinn (AUTO-Anker-Bewegungen zusammen +0,09R bei n=6). Ursache = Ausfuehrung (X1-Warnung stand in 30/30 Vorpruefungen am 28.09., Operator setzte den Reset nicht) + Design (X1 nur Anzeige) + Skript-Luecke (keine Richtungspruefung, 28.09. 19:46-19:56 `--dir long` mit Anker ueber Entry lief 3x als TAUGLICH). Entscheidung Levi: keine Anker-Regelaenderung im Freeze, aber P1+P2 (freeze-konform) und P3 als die eine Live-Aenderung.

**Was drin ist:**
- **P1:** Anker auf falscher Seite des Entrys (Long: muss < Entry, Short: > Entry, == Entry = Fehler) -> Hard-Exit 1 (Vorpruefung, Live-Gate, --vorschau, vollcheck). Bei offener Position in vollcheck nur Warnzeile + LUECKE (A1).
- **P2/A2:** Schattenzeile "AUTO-ANKER-RESET ... X1 FAELLIG seit N Voll-Checks" (nur Anzeige/Log), unabhaengig von `--nas-bars-5m` (nur die AUTO-Anker-Berechnung braucht das Flag, das NICHT ins Template A kommt: 15m-Bars wuerden still als 5m gerechnet).
- **P3:** Auslöser impuls/beide + Ruecklauf + Anker nicht zum Entry hin neu gesetzt -> Hard-Exit 1 "RESET-PFLICHT UNERFUELLT" ohne `--grund-kein-reset "<Text>"`. Begruendung gilt bis neues Impuls-Extrem oder max. 30 Min (DE-Uhr), auch im Live-Gate. Reiner Distanz-Ausloeser (Y1-b) = keine Pflicht. Ein Anker vom Entry weg zaehlt nicht als Reset (A4/B1, Faelligkeit wird nachgerechnet).
- **Operator-Flags:** `--grund-kein-reset`, `--position offen|keine` (wirkt nur mit heutigem Tick in Traderichtung, <=30 Min, 60 s Zukunftstoleranz; im Live-Gate `--position offen` = Hard-Exit), optional `--extrem-seit-anker <Preis> --extrem-seit-anker-ts <UTC-ISO Z>` im Live-Gate. Ruecklauf-Pflicht bei offener Position nur Anzeige (A6).
- **R1:** Reset mit |Δ| < `SL_ANKER_PUFFER_ATR` (0,5xATR) wird nur als "KLEINSTSCHRITT" gekennzeichnet (Zeile + Log-Felder `kleinstschritt`, `delta_pkt`), KEIN Mindestabstand (waere neue Schwelle).

**Rueckbau:** `RESET_PFLICHT_MODUS = 'anzeige'` in gate_check.cjs schaltet P3 auf reine Anzeige. Nach 3 Testtagen zurueck auf Anzeige, falls Vorab-Kriterium 1 verfehlt (Anker hinkt in <=20 % der Momente >90 Min hinter AUTO; 28.09.: 77 %) ODER Hard-Exit-Quote (P3-Exits pro VC-Slot, aus gate_check_log/vollcheck_log.luecken) > 15 %. Vorab-Kriterium 2: jeder Reset <=1 VC nach Faelligkeit oder begruendet abgelehnt. Regel und 30-Min-Wert duerfen waehrend der 3 Testtage nicht geaendert werden.
**Testtag-Analyse:** Zaehlspalte "Resets gesamt | davon Kleinstschritt" fuehren; tritt >=1 Kleinstschritt-Reset auf, legt die Analyse Levi "Reset unter Vorbehalt" als Entscheidung vor (kein Gate ohne Levi-Freigabe vor Beginn eines Testtag-Blocks).

**Pruefung:** 3 Opus-Gegencheck-Runden (FREIGEGEBEN, kein Blocker), Tests 210/210, Golden A2 7/7 gegen 57c711a, Normalpfad-Replay von 300 Vorpruefungen: nur 9 P1 + 127 P3 Abweichungen. Simulierte noetige Begruendungen: 23.09. 8 (17 %), 25.09. 2 (7 %), 28.09. 5 (10 %). Vorher nie im echten Loop gelaufen — **29.09. ist der erste echte Lauf.**

**Noch offen (bewusst NICHT vor 29.09. umgesetzt, Freeze: die eine Live-Aenderung ist mit P3 verbraucht):**
1. Q3-auto als Schattenzeile in gate_check.cjs (Kriterium: 0 unentdeckte Q3-Widersprueche + 0 UNBEKANNT in 3 Testtagen).
2. Tagesende-Regel fiktive Position (19:55 automatisch bewerten, nur als Auswertungsregel, nicht als Loop-Aenderung um 19:55; Kriterium: 0 Auslassungen ohne Ablehnungsgrund bei PASS in 3 Testtagen).
3. Kleinigkeiten (im Freeze erlaubt), Reihenfolge laut Opus: Register inhaltlich pflegen (betrifft Gates), Korrekturnotiz GELB 17:32Z (28.09.), loop_archiv-Luecke klaeren.
**Levi-Entscheid 29.09.:** Der Testtag 29.09. zaehlt fuer die 3-Tage-Kriterien von Q3-auto und Tagesende-Regel rueckwirkend mit (Auswertung aus Bars/Shadow-Log). Umsetzung erst nach Opus-Tagesanalyse 29.09., je mit Opus-Gegencheck. (Tagesende-Regel = Auftrag A1, umgesetzt 30.09.; Q3-auto = Auftrag B1, nach Loop-Stopp 30.09.)

**Levi-Entscheid 30.09. — P3-Verlaengerung (Opus-Vorschlag Bericht 29.09. Abschnitt D):** Das 3-Tage-Urteil ueber P3 (`pflicht` behalten oder Rueckbau auf `anzeige`) gilt NUR, wenn in den 3 Testtagen **>= 1 impuls-faelliger Fall** (Ausloeser impuls/beide + Ruecklauf, also eine echte Reset-Pflicht) aufgetreten ist. Sonst wird der Beobachtungszeitraum **um bis zu +2 Testtage verlaengert** (max. 5). Stand: 29.09. = P3-Tag 1/3 mit 0 einschlaegigen Faellen (1 Reset 18:11 nur Distanz-Ausloeser, 0 Kleinstschritt, 0 P3-Hard-Exits). Zaehlspalte je Testtag weiterfuehren: impuls-faellige Faelle | Resets gesamt | davon Kleinstschritt | P3-Hard-Exits. Regel und 30-Min-Wert bleiben waehrend der (ggf. verlaengerten) Beobachtung unveraendert.

**Randnotizen:** Kein `git add -A` (ungetrackte Analyse-/Log-Dateien nicht mit committen; `.gitignore`-Muster `scripts/*.bak_*` passt nicht auf `*.json.bak`). Risiko (nicht von Opus geprueft): Golden-Fixtures `tests/fixtures/vollcheck_a2_golden/` werden byte-verglichen, es gibt keine `.gitattributes`, `core.autocrlf=true` -> Windows-Neuclone koennte sie auf CRLF umschreiben; Absicherung waere `tests/fixtures/vollcheck_a2_golden/* -text` in `.gitattributes`. Mac-Umzug: LF, unkritisch.

**Why:** Die Anker-Kette war ueber 5 Testtage der wiederkehrende Einstiegsblocker, aber der Geldnutzen einer Regelaenderung (AUTO-Anker) ist bei n=6 nicht belegt (Overfitting) -> erst messen, dann entscheiden. **How to apply:** Vor jeder Anker-Regelarbeit diese Datei lesen; den Freeze ([[project_testtag_2026-09-23_besprechung_ausstehend]]) einhalten; Test-/Operator-Ablauf am Testtag siehe [[feedback_testtag_start_verlaesslichkeit]].

Verwandt: [[project_testtag_analyse_2026-09-28]], [[project_testtag_analyse_2026-09-25]], [[project_testtag_analyse_2026-09-23]].
