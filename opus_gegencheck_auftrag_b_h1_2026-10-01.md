# Opus-Gegencheck Auftrag B (B1–B6) + Memory-Änderungen 01.10.2026 (H1, feedback_live_trading, Index, Berichte)

**Gesamturteil: FREIGEGEBEN MIT AUFLAGEN.** Nichts davon blockiert. Der Code hält die Freeze-Regel ein: Gates, Schwellen, Q-Faktoren, Ampel, Größen und Exits sind unverändert. Belegt ist das durch Code-Inspektion, 14 echte Live-Läufe im HEAD-vs.-Arbeitsbaum-Replay und 9 Mutationsproben. Fables „226/226 grün“ war aber **nicht reproduzierbar**. Mein Lauf ergab **225/226**: Der neue Test „B1 Rückbau 'aus'“ ist zeitabhängig flaky (A1). Ein Commit ist möglich, sobald A1 behoben ist. A2–A5 können im selben Nachbesserungsauftrag mitlaufen. Commit/Push beauftragt nur Levi.

Prüfgrundlage: `git diff` gegen HEAD 07b4ccb (4 Dateien, +502/−47). Alles lief read-only. Experimente liefen nur auf Kopien in `%TEMP%\opus_gegencheck` (wt, head, rp_*, mut_*). Die echten `scripts/*.jsonl` sind vorher/nachher sha1-identisch (15 Dateien). `tests/fixtures` ist unverändert, `x_fetch_stamp.cjs` ebenfalls.

---

## Testergebnis und Mutationsproben

- **Arbeitsbaum** (Kopie, `node --test tests/trading_scripts.test.js tests/tagesmomente.test.js`): **226 Tests, 225 pass, 1 fail.**
  - Fehlschlag: `trading_scripts.test.js:5409` „B1 Rückbau Q3_AUTO_MODUS = 'aus'“. Ursache: Die beiden verglichenen Läufe drucken die RETEST-ZEITBOX-Zeile mit `laeuft ab um 11:34:12` bzw. `11:34:13 DE-Ortszeit`. Das ist Date.now()+12 Min und nicht `--jetzt`.
  - `norm()` in Z. 5415 normalisiert das nicht. Der Test fällt also immer dann um, wenn zwischen den zwei Spawns eine Sekundengrenze liegt.
  - 3 Einzel-Reruns liefen grün. In den 10 Vollläufen (wt + 9 Mutanten) schlug er 2× allein aus diesem Grund fehl. **Klar flaky.**
- **HEAD-Kopie** (`git archive`): 218/218 grün.
- **Mutationsproben** (jeweils eine Kopie, voller trading_scripts-Lauf):

| Mutante | Eingriff | erkannt? |
|---|---|---|
| M1 | Q3-auto NICHT ERFUELLT senkt qScore um 1 (Freeze-Leck) | ja (8 Tests, u. a. Golden B-1) |
| M2 | Q3-AUTO-Zeile doppelt | ja |
| M3 | B4 zurück auf alte Zählung `slotsSeit(seitMs)` | ja (B4-Unit) |
| M4 | B6 Hälfte-Regel aus (`<10 Min`) | ja (4 Tests) |
| M5 | DRIFT-Schwelle 0,30 statt 0,25 | ja |
| M6 | Modus 'aus' schreibt trotzdem `q3_auto` | ja |
| M7 | B6 Doppel-Polling Off-by-one (`>` statt `>=`) | ja |
| **M9** | **Q3-auto ERFUELLT kippt PASS → FAIL (Freeze-Leck)** | **NEIN** (nur der flaky Zeittest schlug an) |
| M10 | zusätzliche „Q3-AUTO …“-Zeile bei PASS | ja (Golden-Zählung = 1) |

  - Lücke (A2): Es gibt keinen Test mit einem **PASS-Szenario und Q3-AUTO ERFUELLT/NICHT ERFUELLT**. Die Golden-Szenarien liefern UNBEKANNT bzw. NICHT ERFUELLT. Der B1-Test ist ein FAIL-Szenario, er vergleicht deshalb nur „Q-Score: NICHT RELEVANT … Rohwert“ und GESAMTSTATUS FAIL.
- **Replay Live-Läufe (Gegencheck-Punkt „300 Läufe“):** `gate_check_log.jsonl` hat nur 146 Live-Einträge. Davon sind mit den heutigen A3-Pflichtfeldern 14 lauffähig, die übrigen 132 scheitern in HEAD **und** Arbeitsbaum identisch an Exit 1 (Altaufrufe).
  - Die 14: Exit-Codes identisch (12× 2, 2× 0), stdout/stderr identisch bis auf Sandbox-Pfad und die Zeilen Q3-AUTO, DRIFT-HINWEIS, PLAUSIBILITAETS-HINWEIS.
  - Q3-AUTO-Verteilung: ERFUELLT/KONSISTENT 4 · NICHT ERFUELLT/KONSISTENT 2 · **NICHT ERFUELLT/WIDERSPRUCH 1** · NICHT PRUEFBAR 7.
  - Der Widerspruch ist echt: 28.09. 19:32:14, short. Entry 30350,25 > EMA50 5m 30307,5, also lag das 5m-Bein **gegen** die Richtung, und manuell stand `--q3-coherence yes`. Q3-auto hätte das gefangen.
  - Am 29.09. 16:11:34Z (der „18:11“-Fall) ergibt der Replay ERFUELLT/KONSISTENT, wie spezifiziert.

---

## B1 Q3-AUTO-Schattenzeile — Urteil: OK, Abweichungen akzeptabel (mit A3)

Belege: `gate_check.cjs` Z. 1003–1044 (Konstanten, `q3AutoBerechnen`), 3659–3660 (Zeile nach KOMBI-SCHATTEN), 4117 (Log-Feld nur live + 'schatten'), 5085–5096 (Aufruf **nach** `evaluateTrade`, Z. 5083).

- **Checkliste (2):** `result.q3Auto` wird nur in printResult (Z. 3660) und in der JSON-Ausgabe gelesen, `gateLogQ3Auto` nur im Log-Append. Es gibt keinen Zugriff in evaluateTrade, Q-Score, Sizing oder Exit. ✓
- **Checkliste (1):** Die Golden-Ausnahme in `trading_scripts.test.js` Z. 4246–4250 trifft nur Zeilen `^\s*Q3-AUTO \(Schatten, kein Gate\): `. Pro Szenario wird genau 1 verlangt und das Format per Regex geprüft. Der Log-Strip umfasst nur `register_snapshot` und `q3_auto`. Fixtures sind byte-unverändert. ✓
- **Checkliste (3):** Der 'aus'-Test vergleicht „Schatten minus Zeile“ mit 'aus'. Das ist inhaltlich richtig, wegen A1 aber flaky. Die A2/Kombi-Goldens laufen grün. ✓
- **Randfälle:** Die getestete Abdeckung ist gut.
  - Getestet: >360 s, Vortag, Datei fehlt, nur Zukunftseintrag, +30 s, Grund statt Wert.
  - Nicht getestet (Hinweis H1): unsortiertes Log. Q3-auto nimmt die *letzte Zeile* ≤ jetzt+60 s, nicht die jüngste. Bei Re-Runs mit älterem `--jetzt` wäre das falsch, praktisch ist es selten.
- **Hinweis H2:** Die Spezifikation verlangte als Test die „Nachstellung 29.09. 18:11 (alle short)“. Der Test nutzt synthetische Long-Werte. Ich habe den echten 29.09.-Lauf per Replay nachgeholt: ERFUELLT/KONSISTENT.

## B2 Drift-Hinweis + Prozesstext — Urteil: OK

Belege: `gate_check.cjs` Z. 5097–5108 (gleiche Funktion `slAutoKandidat` wie `--sl-auto` Z. 3773), 3588 (Zeile direkt nach der Vorprüfungszeile), `loop_prompt.cjs` Z. 216, `feedback_live_trading.md` direkt nach 7b1 Schritt 5.

- **Zahlen aus dem Primärlog nachgerechnet** (29.09.):
  - Vorprüfung 16:11:04Z: Entry 30317,05, Anker 30337,45, ATR 40,2.
  - Live 16:11:34Z: Entry 30306,05, SL 30377,35, TP1 30200.
  - Floor 30306,05 + 1,5×40,2 = **30366,35** ✓
  - RR 106,05/60,3 = **1,76** statt 106,05/71,3 = **1,49** ✓
- **Checkliste (4):** Es wirkt nicht auf das Gate. `driftHinweis` wird nur gedruckt bzw. als JSON ausgegeben. ✓
- **Hinweis H3:** Die Vorprüfung holt die Zonen aus einem Probelauf mit Struktur-SL, DRIFT nimmt `result.clusterZones` des Live-Laufs. Die Funktion ist dieselbe, die Zoneneingabe kann sich unterscheiden, wenn `--cluster-level` live anders gesetzt wird. Das ist vertretbar („für DIESEN Lauf“), sollte aber bekannt sein.
- **Hinweis H4:** Im Replay vom 30.09. erscheint die Zeile auch dann, wenn der Floor dem übergebenen SL entspricht (`waere 30495.65 … uebergeben 30495.65`). Das ist laut Spezifikation korrekt, aber Rauschen. Dazu gehört A5: der Memory-Satz „Steht die Zeile im Output, ist der Live-Aufruf mit dem Vorprüfungs-SL ein Prozessfehler“ ist in diesem Fall zu scharf.

## B3 kerzen-qqq HINWEIS — Urteil: OK

Belege: `vollcheck.cjs` Z. 1504, `gate_check.cjs` Z. 3044–3049 und printResult (`PLAUSIBILITAETS-HINWEIS kerzen-qqq (kein Fehler, A4)`).

- **Checkliste (5):** `grep` nach „vermutlich zu niedrig | PLAUSIBILITAETS-WARNUNG kerzen-qqq | WARNUNG: QQQ“ in `scripts/*.cjs`, `scripts/analyse/*.cjs` und `tests/fixtures` findet 0 Treffer (nur das Negativ-Regex im Test).
  - Die beiden anderen `PLAUSIBILITAETS-WARNUNG`-Zeilen (chasing, k-ohne-signal) bleiben bewusst unverändert.
  - Kein Konsument parst die Zeile. `protokoll_bilanz.cjs` Z. 382 matcht nur `^\**Tweet-Check`.
  - Der JSON-Feldname `kerzenQqqPlausibilityWarning` ist unverändert. ✓

## B4 X1-Zähler — Urteil: OK (Spezifikation erfüllt), 2 Hinweise

Belege: `vollcheck.cjs` Z. 387 ff. (gc-Liste einmal gelesen, `faelligSlots`, `nFolge`), 2091 (`X1 FAELLIG in <n> Voll-Check(s) seit <hh:mm:ss> DE (davon zuletzt <k> in Folge)`), 2424 (Export).

- **Checkliste (6), real nachgestellt:** Ich habe `x1StatusAus` auf Kopien von gate_check_log und vollcheck_log laufen lassen, Lauf 29.09. 18:05:50 DE, Anker 30372,55, short.
  - Ergebnis: **n = 2, nFolge = 1, seit 17:31:24 DE.** Dieselbe Funktion aus HEAD liefert **n = 8**.
  - Im Log tragen genau 17:31:24 und 18:05:50 „ANKER-RESET FAELLIG (X1“ ✓
- **nPflicht/A8:** Die Logik ist identisch. Es ist ein reiner Refactor (gleiches Filter- und Pflicht-Regex), der Unterschied ist nur, dass das Log jetzt einmal statt zweimal gelesen wird. ✓
- **Hinweis H5:** Im Nicht-machbar-Zweig ist `logHinweis` („vollcheck_log.jsonl fehlt — N nur aus diesem Lauf“) still entfallen, im machbar-Zweig steht er jetzt beim Pflichtteil. Für n ist das unerheblich (n kommt nicht mehr aus vollcheck_log), für `nFolge` ist es eine kleine Informationslücke. Das Wort „ohne Reset“ fehlt ebenfalls. Beides ist laut Spezifikationsformat zulässig, wurde aber nicht gemeldet.
- **Hinweis H6:** `anker_auto_log.x1_vcs_ohne_reset` behält den Namen, ändert aber ab 01.10. die Bedeutung. Wer das Log später über den 30.09./01.10. hinweg auswertet, muss das wissen. Bisher gibt es keinen Konsumenten (grep).
- Der Regex-Test Z. 4908 ist auf `>= 3` gelockert (geteilte Sandbox). Die exakte Zählung deckt der neue Unit-Test ab, das ist akzeptabel.

## B5 Session-Extrema (Prozesstext) — Urteil: OK mit A4

Belege: `loop_prompt.cjs` Z. 207, `feedback_live_trading.md` (Punkt Level-Register, Satz „Session-Extrema SOFORT ins Register (B5 …)“).

- Der Inhalt deckt sich mit Opus-Bericht G1 (inkl. V11-Touch-Regel). Es gibt kein Selbsttest-Fragment (Test Z. 303). ✓
- **A4:** Die loop_prompt-Fassung empfiehlt „Preis aus data_get_ohlcv/**quote_get**“. Das widerspricht [[feedback_datenquelle_nas100]] („quote_get kann stale sein, immer Chart-Bars“). Außerdem stimmt „ab 15:30 DE“ im DST-Divergenzfenster 26.–30.10. nicht (US-Open 14:30 DE, vgl. [[project_vollcheck_dst_fix_todo_2026-10]]).

## B6 Tweet-Slot-Fälligkeit — Urteil: OK, Abweichung akzeptabel

Belege: `vollcheck.cjs` Z. 301–303 (Konstanten), 1736–1790 (Parsing der `last_poll (`-Zeile, Slot-Urteil, drei neue Zweige vor den alten). Die Zweige greifen nur bei `--tweet-fetch ja && !doppelOk && !anlassOk`.

- **Eigenes Replay 30.09.** (loop_archiv + tweet_fetch_log, B6-Logik nachgebaut):
  - 23× „NICHT fällig … ÜBER-POLLING ✗“ → **23× regulärer Slot-Fetch ✓**
  - 6× „verpasst, nachgeholt“ (Artefakt) → regulär ✓
  - 15:27 (Loop-Start) bleibt „nachgeholt, Slot 15:10 verpasst“ (der echte Fall)
  - 28× `--tweet-fetch nein` unberührt
  - Fables Angabe stimmt.
- **Replay 29.09.:** 10× Artefakt-„nachgeholt“ → regulär, 1× Doppelbedingung unberührt.
- **Checkliste (7):** Echte Lücken (`verpasst`-Liste), echtes Doppel-Polling (gleicher Slot → ✗) und `--tweet-fetch nein` sind getestet (Test-Fälle C, D, E). `x_fetch_stamp.cjs` ist unverändert.
  - Die Slot-Rechnung erfolgt in UTC-ms modulo 10 Min, korrekt, weil DE-Offsets volle Stunden sind.
  - Die Anzeige rechnet mit Europe/Berlin, `TZ=` kommt nicht vor. ✓
- **Hinweis H7:** Fehlt `last_poll`, steht aber `prev_poll` da, liest `b6Isos[0]` prev_poll als last_poll (praktisch unmöglich). Ein Stempel des vorigen VC < 3 Min vor `--jetzt` gilt als „dieser Abruf“ (nur bei Re-Runs, dort richtig).

---

## Bewertung der 3 Abweichungen Fables

1. **Vorrang „gerichteter Gegenbefund → NICHT ERFUELLT, auch wenn ein Bein fehlt“: akzeptabel, mit Präzisierung (A3).**
   - Das ist logisch zwingend: Ein Bein gegen die Richtung macht ERFUELLT unmöglich, egal was das fehlende Bein sagt.
   - Aber: Das Vorab-Kriterium „0 UNBEKANNT an 3 Testtagen“ misst die *Datenverfügbarkeit*. Durch den Vorrang verschwindet ein veraltetes Shadow-Log hinter NICHT ERFUELLT.
   - Deshalb muss die Zählung über `q3_auto.quelle_ts === null` bzw. ein Bein `null` laufen, nicht über `status`. Die Daten dafür stehen im Log.
2. **Dritter Konsistenzwert NICHT PRUEFBAR: akzeptabel.** Die Spezifikation sieht `manuell … fehlt` selbst vor. Bei UNBEKANNT oder fehlendem manuellen Wert ist KONSISTENT/WIDERSPRUCH nicht definiert, ein dritter Zustand ist nötig.
3. **B6 Fall „Stempel folgt nach vollcheck“ + Regel erste/zweite Slot-Hälfte: akzeptabel.**
   - Strikt „gegen last_poll“ wäre im 29.09.-Muster falsch (dort ist last_poll noch der vorige Abruf).
   - Die Hälfte-Regel bildet die Loop-Kadenz ab: Der :x0-VC ruft regulär ab, ein Abruf im :x5-VC ist ein Nachhol-Fetch.
   - Beide Replays bestätigen die Zahlen.
   - Restrisiko: `--tweet-fetch ja` ohne Stempel wird geglaubt. Der nächste VC sieht den fehlenden Stempel über die unveränderte alte Logik aber weiterhin.
   - Bitte als Spezifikationsnachtrag in [[opus_vorschlag_2026-09-30]] bzw. im Freeze-Review vermerken.

## Mengenbremse / Freeze

- **Checkliste (8):** Alle `-`-Zeilen im Diff sind Kommentar- und Textzeilen, ein Refactor (A8-Schleife) oder Rückgabe-Objekte, die um Felder erweitert wurden. Es gibt keine Schwellen-, Gate- oder Q-Konstante. ✓
- **Über die Spezifikation hinaus (Hinweis H8):**
  - `loop_prompt.cjs` Z. 216 führt eine neue Operator-Pflicht ein: „WIDERSPRUCH/UNBEKANNT im Fließtext benennen … nie vom Schatten abschreiben“. Das ist sinnvoll, weil es „unentdeckt“ erst messbar macht, und es hat keine Gate-Wirkung. Trotzdem ist es eine stille Prozesserweiterung, Levi sollte sie bestätigen.
  - Ebenso ist der Memory-Satz „… ist ein Prozessfehler, kein Pech“ (B2a) eine normative Ergänzung (A5).
- **CRLF:** Skripte im Arbeitsbaum CRLF, Testdatei LF (daher die git-Warnung), Index LF. Beim Commit normalisiert, unkritisch.
- **Hinweis H9:** Der B5/B2a-Test liest die echte Memory-Datei (USERPROFILE-Pfad). Auf dem Mac wird das still übersprungen, und eine Memory-Änderung kann einen Repo-Test brechen.

---

## Memory

**(a) feedback_live_trading.md — OK, mit A5.**
- `git -C memory diff`: +4/−2, davon 1 Metadaten-Zeile.
- Der B2a-Absatz steht direkt nach 7b1 Schritt 5 (Z. ~755) und enthält den Spezifikationssatz wörtlich, die Belegzahlen sind korrekt (18:11:34, 30.366,35, 1,76/1,49).
- Der B5-Satz ist im Punkt „Level-Register aktualisieren“ ergänzt und deckt sich mit loop_prompt, ohne quote_get.
- **A5:** Den Prozessfehler-Satz präzisieren: nur, wenn der genannte Floor ≠ dem übergebenen SL ist.

**(b) project_h1_q2_trendkontext_vorabkriterium_2026-10-01.md — treu, nichts erfunden.**
- Alle Zahlen und Schwellen stimmen 1:1 mit [[opus_antwort_d_qrot_2026-10-01]] Abschnitt 4–5 überein: H1-Wortlaut, Trendkontext (Bedingung 1 + ≥ 2,0×ATR(5m)), 4 Zellen, FLOOR/AUTO, Beta(5,5), Mindest-n 15/6, Ø ≥ +0,10, ≥ 0 ohne besten Tag, P ≥ 0,8, Σ r_primaer ≥ 0, Zählung ab 01.10.
- Die Bedingung „SL-Treffer ≤ 30 Min in ≤ 1/3“ steht wörtlich bei Opus (Punkt 4, letzter Spiegelstrich). Sie zu übernehmen ist richtig, sie wegzulassen wäre die eigentliche Verfälschung gewesen.
- Ergänzungen ohne Widerspruch: Auswertung bzw. Umsetzung erst nach dem Freeze, Opus-Gegencheck vor jeder Änderung.
- Die Checkliste 1–6 auf Zelle A anzuwenden ist eine Lesart. Opus nennt die Zelle nur in Punkt 1, die Lesart ist aber plausibel.
- **K1–K8 und was VOR der ersten Sichtung von 01.10.-Daten fix sein muss.** Sonst ist es kein Vorab-Kriterium mehr: Jede Opus-Tagesanalyse zeigt Momente. Verbindlich entscheidet Levi.

| K | vorher zwingend? | Vorschlag |
|---|---|---|
| K1 unabhängig | **ja** | Feld `varianten.FLOOR.unabhaengig === true` aus tagesmomente (AUTO entsprechend). Die Zellenzuordnung erfolgt **nach** der Unabhängigkeitsauswahl, keine Neuauswahl je Zelle. |
| K2 Treffer | **ja** | Wie die Trefferquote von tagesmomente: `exit_art` TP (RR1) = Treffer. OFFEN, SL und Stall = kein Treffer. |
| K3 Hälften | **ja** | Gezählte Tage chronologisch, erste ⌈d/2⌉ gegen den Rest. Vorzeichen = Ø r_rr1 FLOOR der Zelle A je Hälfte, 0 zählt als „kein gleiches Vorzeichen“. |
| K4 SL ≤ 30 Min | **ja** | Daten liegen vor: `exit_ts − ts` bei `exit_art` SL, Variante FLOOR, Basis = unabhängige Momente der Zelle A. Bedingung beibehalten. |
| K5 Session-Start | **ja** | Feld `session_start` je Zeile in momente_log (15:30, bzw. 14:30 im DST-Fenster). Beginnt das Shadow-Log später als Session-Start + 15 Min: Trendkontext „nicht bestimmbar“, Moment ausschließen und Anzahl ausweisen. |
| K6 1H-Abstand | **ja** | `abstand_atr` des Shadow-Eintrags mit gleicher VC-Nummer und gleichem Datum (das ist **Kurs** − EMA50 1H, nicht der 1H-Close). Long ≥ +2,0, short ≤ −2,0. Bedingung 1 über `bias['1h']` aller Shadow-Einträge von Session-Start bis zum Moment == dir. |
| K7 „zuerst fiktiv“ | nein (erst bei Erfolg) | Eigene getrennt geloggte Fiktiv-Zeile. Kombi/V2 bleibt unverändert, sonst vermischen sich die Messungen. |
| K8 Stabilität hart | **ja** | Ja, hart. Opus schreibt „muss“. |

**Zusätzlich vorher zu fixieren (neu):**
- K9: welche Tage zählen. Vorschlag: tagesmomente-bewertbar (bars ok, ≥ 20 VC), fiktive und echte Testtage gleich.
- K10: Momente mit `q.q2` null werden ausgeschlossen und ausgewiesen.

**(c) MEMORY.md-Index — Auflage A6.**
- Die neue H1-Zeile hat **304 Zeichen** (> 200).
- Die neue Opus-Bericht-Zeile mit 177 Zeichen ist ok.
- Eingefügt wurden 2 Leerzeilen zwischen den Einträgen. Das verstößt nicht gegen die Konvention, ist aber uneinheitlich.
- Nicht von heute, aber ebenfalls über der Grenze: die Testtag-30.09.-Zeile (216, Datei vom 30.09. 21:48) und die geänderte Freeze-Zeile (208).

**(d) Berichte gegen die Logs (Stichproben, alle aus Primärlogs nachgerechnet):**
- Opus-Bericht 30.09.:
  - vollcheck_log 61 Zeilen = 58 Exit 0 + 3 Exit 1, VC max 56 ✓
  - 30 Fetches set/polled ✓
  - 18 Register-Touches ✓
  - 185 Quick-Ticks ✓
  - 58 Vorprüfungen + 4 Live ✓
  - 17:42:15Z FAIL, RR 0,824 ✓
- Opus-Antwort (d):
  - skipped 40 Zeilen, kombi 6, momente 351/277 ✓
  - Kombi Σ gewichtet +0,74 / r_primaer +2,39 ✓
  - Ø +0,23, SD 1,15, KI [−0,83; +1,29], ohne besten Tag −0,02, n=5 Ø +0,65 / +0,37 ✓
- **Fehler in (d), Hinweis H10:**
  - „positiv bis Stichtag **5/7**“ ist falsch. Laut eigener Tabelle sind es **4/7** (#4–#7), Wilson ca. [25 %; 84 %] statt [36 %; 92 %].
  - Folgefehler: „4 von **5** Plus-Fällen“ müsste 4 von 4 heißen.
  - „Q4 ✗ und Q1 ✗ in **6/6** Fällen seit 17.09.“: Seit 17.09. sind es 5 Fälle (#3–#7).
  - Keiner der Fehler ändert die Schlussfolgerung oder die H1-Datei, die diese Zahlen nicht übernimmt.
- **Widerspruch zwischen den Berichten (H11):** Der Bericht schreibt „Kombi n=6 in **4** Tagen“, die Antwort „in **3** Tagen“. Laut Log sind es 3 Tage (28., 29., 30.09.).

---

## Auflagen (Nachbesserungsauftrag Fable, alle ohne Gate-Wirkung)

- **A1 (vor Commit):** In `tests/trading_scripts.test.js` Z. 5415 `norm()` zusätzlich `\d{2}:\d{2}:\d{2} DE-Ortszeit` → `<T> DE-Ortszeit` normalisieren, ebenso andere Date.now()-abhängige Uhrzeiten der RETEST-ZEITBOX. Danach 5 Volläufe hintereinander grün belegen. Kein Skriptcode ändern.
- **A2:** Einen Test ergänzen, der ein **PASS-Szenario** (z. B. die Argumente von Golden `b_pass_gelb`) einmal mit Shadow-Log ERFUELLT und einmal mit NICHT ERFUELLT laufen lässt. Assert: stdout minus Q3-AUTO-Zeile ist byte-gleich zur Golden-Datei, Exit-Code gleich. Muss die Mutante M9 (PASS → FAIL bei ERFUELLT) fangen.
- **A3:** Zählregel für das B1-Vorab-Kriterium „0 UNBEKANNT“ schriftlich festlegen (loop_prompt-Satz und Auswertungsnotiz): gezählt wird jeder Live-Lauf mit `q3_auto.quelle_ts === null` oder einem Bein `null`, unabhängig vom Status.
- **A4:** `loop_prompt.cjs` B5-Block: „/quote_get“ streichen (Chart-Bars). Dazu „ab 15:30 DE (im DST-Divergenzfenster 26.–30.10.: 14:30 DE)“.
- **A5:** `feedback_live_trading.md` B2a: Den letzten Satz auf „… wenn der dort genannte SL-Floor vom übergebenen SL abweicht“ einschränken.
- **A6:** MEMORY.md: Die H1-Zeile auf ≤ 200 Zeichen kürzen (Link auf opus_antwort_d in die Datei verlagern), Leerzeilen entfernen. Optional die 30.09.-Zeilen (216/208) kürzen.
- **A7 (nur Doku):** Die Abweichung B6 (Stempel-folgt-Fall, Hälfte-Regel) und den B1-Vorrang als Spezifikationsnachtrag in opus_vorschlag_2026-09-30 bzw. in die Commit-Message.
- **Für Levi, vor dem ersten Zähltag H1:** K1–K6, K8, K9 und K10 entscheiden (Vorschläge siehe Tabelle). Bis dahin keine H1-relevanten Momentdaten ab 01.10. sichten.

**Hinweise ohne Auflage:** H1–H11 oben.

**Commit-Empfehlung:** Nach A1 ist der Commit der 4 Dateien technisch möglich, A2–A5 sollten möglichst im selben Paket mitkommen. Auftrag und Ausführung nur durch Levi. Die ungetrackten Dateien (`loop_archiv/`, `last_gate_check_1942*`, `backtest_*`) gehören nicht in diesen Commit.
