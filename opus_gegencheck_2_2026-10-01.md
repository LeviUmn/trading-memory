---
name: opus_gegencheck_2_2026-10-01
description: "Zweiter Opus-Gegencheck 01.10.2026: Nachbesserungen A1-A7, H1-Festschreibung K1-K10, Entwurf h1_auswertung.cjs"
metadata:
  node_type: memory
  type: project
  originSessionId: 9f4b9f35-da7a-456c-9a16-094c4f451adb
  modified: 2026-10-01T10:11:53.475Z
---

# Opus-Gegencheck 2 (01.10.2026): Nachbesserungen zu Auftrag B, H1-Festschreibung, h1_auswertung.cjs

## Gesamturteile

1. **Code-Commit (Auftrag B + A1–A7): FREIGEGEBEN.** Commitfähig. Offen bleibt nur A6, und das betrifft MEMORY.md, nicht den Code.
   - 3 eigene Vollläufe ergaben je 227/227 grün.
   - Die Mutante M9 und 3 Varianten davon werden vom neuen A2-Test gefangen.
   - Seit dem ersten Gegencheck haben sich `gate_check.cjs` und `vollcheck.cjs` nicht verändert (CR-bereinigter Vergleich mit dem Stand von damals). Geändert sind nur `loop_prompt.cjs` Z. 207/216 (A3/A4) und die Testdatei (A1/A2). Damit gibt es keine stille Verhaltensänderung, die Freeze-Regel ist eingehalten.
2. **H1-Festschreibung: FREIGEGEBEN MIT AUFLAGEN.**
   - K1–K10 sind wortgetreu übernommen, nichts ist abgeschwächt.
   - Offen: R3 ist veraltet. Außerdem fehlen 2 Vorab-Festlegungen: die Kombi-ROT-Definition (R5) und der Auswertungszeitpunkt bzw. die Zahl der Auswertungen (R6). Beide müssen vor dem ersten `--echt`-Lauf fixiert sein.
3. **h1_auswertung.cjs (Entwurf): commitfähig JA, mit Auflagen S1–S4.**
   - Die Statistik ist korrekt (unabhängig nachgerechnet).
   - Die Zellenzahlen des Probelaufs habe ich mit eigenem Code reproduziert.
   - Das Skript schreibt nichts, der Integritätsschutz greift.
   - Kleine Mängel: `--echt nein` schaltet den Echtmodus ein, und 2 Kommentare sind veraltet.

Nichts davon ist blockierend.

---

## 1. Auflagen A1–A7 aus Gegencheck 1

| | Status | Beleg |
|---|---|---|
| **A1** | **erfüllt** | `tests/trading_scripts.test.js:5415-5418`: `norm()` ersetzt `\d{2}:\d{2}:\d{2} DE` → `HH:MM:SS DE`. Details unter der Tabelle. |
| **A2** | **erfüllt** (Abweichung akzeptabel) | `tests/trading_scripts.test.js:5436-5492`, Details unten. |
| **A3** | **erfüllt** | `scripts/loop_prompt.cjs:216` („Zaehlregel A3 … q3_auto.quelle_ts null … ein Bein in q3_auto.beine null … unabhaengig vom Status … aus gate_check_log.jsonl“). Ebenso `opus_vorschlag_2026-09-30.md`, Nachtrag am Ende von Auftrag B (wortgleich, „verbindlich“). |
| **A4** | **erfüllt** | `scripts/loop_prompt.cjs:207`: „Chart-Bars data_get_ohlcv — NICHT quote_get … (feedback_datenquelle_nas100.md)“ und „im DST-Divergenzfenster 26.-30.10.2026: ab 14:30 DE“. Stimmt mit [[feedback_zeitzone]] überein (Tabelle: Herbst 2026 Mo 26.10.–Fr 30.10., US-Open 14:30 DE) und mit [[feedback_datenquelle_nas100]]. |
| **A5** | **erfüllt** | `feedback_live_trading.md` ~Z. 757 (memory-git diff): „Steht die Zeile im Output **und weicht der dort genannte SL-Floor vom übergebenen SL ab**, ist … ein Prozessfehler“ + Klammer zum Gleichheitsfall. |
| **A6** | **teilweise, NICHT erfüllt** | `MEMORY.md` Z. 7 (H1-Zeile) hat jetzt **233 Zeichen** (nachgezählt mit Unicode-Codepoints), vorher 304. Die Grenze ist 200. Die Leerzeilen sind entfernt ✓, die Opus-Bericht-Zeile (Z. 8) hat 177 ✓. Eine eigene Indexzeile für `opus_antwort_d_qrot_2026-10-01.md` gibt es nicht (nur als Textverweis in der H1-Zeile), ebenso keine für `opus_gegencheck_auftrag_b_h1_2026-10-01.md`. Weiterhin über 200: Z. 9 (Testtag 30.09.) 216, Z. 19 (Freeze) 208. |
| **A7** | **erfüllt** | `opus_vorschlag_2026-09-30.md`, Nachtrag „zur Spezifikation B“: alle 3 Abweichungen inhaltlich korrekt wiedergegeben, je „akzeptabel“, Restrisiko B6 genannt. |

**A1 im Detail:**
- Die Ursache ist richtig getroffen: `gate_check.cjs:3235` `zeitboxDeadlineMs = Date.now() + …`.
- Die übrigen Wanduhr-Stellen sind im Test nicht ausgabewirksam:
  - Z. 4721 nur bei Blackout, im Test `none`.
  - Z. 4787 „Alter N Min“ ist bereits normalisiert.
- Ist die Normalisierung zu breit? Nein, vertretbar. Beide Läufe bekommen dasselbe `--jetzt`. Eine Uhrzeit kann sich also nur durch die Wanduhr unterscheiden. Eine echte Regression, die *ausschließlich* eine Uhrzeit verschiebt, ist für den Rückbau-Schalter praktisch ausgeschlossen. Der Vergleich bleibt zeilengenau.

**A2 im Detail:**
- Fables Begründung stimmt. In `tests/fixtures/kombi_v2_golden/szenarien.cjs` Z. 32/36/45 hat `b_pass_gelb` `--ema50-5min` = Entry (5m-Bein neutral) und `--grund-override-1h-*` (1H-Bein fehlt). ERFUELLT ist damit unerreichbar.
- `e_pass_trendmodus` (Z. 51) hat 5m short (29389,85 < 29462,45) und 1H short (29350 < 29400) und ist das einzige PASS-Golden mit beiden Beinen. Die Wahl ist richtig und **besser** als die Auflage.
- Der Test spiegelt **nicht** die eigene Implementierung. Er vergleicht gegen Golden-Dateien aus HEAD 941a221, also gegen Exit, stdout ohne Q3-Zeile, stderr und die alten Logfelder.
- Zusätzlich gibt es eine b_pass_gelb-Variante ERFUELLT vs. NICHT ERFUELLT, die byte-gleich zueinander sein muss, plus GESAMTSTATUS/Q-Score-Zeile wie Golden.

## 2. Testergebnis und Mutationsproben (eigene Läufe)

- **Vollläufe** auf einer Kopie in `%TEMP%\opus_gegencheck2\wt` mit `node --test tests/trading_scripts.test.js tests/tagesmomente.test.js`:
  - Lauf 1: **227/227**
  - Lauf 2: **227/227**
  - Lauf 3: **227/227**
  - Lauf 2 und 3 liefen parallel zu den Mutationsläufen, also unter Last.
- **Mutationsproben** (je eine Kopie, A2-Test per `--test-name-pattern`):

| Mutante | Eingriff (nach `evaluateTrade`, vor B2b) | gefangen? |
|---|---|---|
| M9a | Q3-AUTO ERFUELLT + PASS → `status = 'FAIL'` (Original-M9) | **ja** („e_pass_trendmodus/short: Exit 2 statt 0“) |
| M9b | Q3-AUTO ERFUELLT → `sizingFlag` gesetzt (Größen-Leck) | **ja** (stdout ≠ Golden) |
| M9c | NICHT ERFUELLT + PASS → FAIL | **ja** („…/long: Exit 2 statt 0“) |
| M9d | ERFUELLT → qScore.score +1 | **ja** (stdout ≠ Golden) |

- **Echte Logs:** 16 Dateien (`scripts/*.jsonl`, `trades.db`) vorher und nachher sha1-identisch. Fixtures sind seit Gegencheck 1 unverändert.

## 3. H1-Festschreibung (`project_h1_q2_trendkontext_vorabkriterium_2026-10-01.md`)

**Wortgetreue Übernahme:** Abschnitt 7 stimmt mit der K-Tabelle aus Gegencheck 1 überein. Abweichungen gibt es nur in Form zulässiger Klarstellungen:
- K2 ergänzt „`exit_art`“.
- K8 ergänzt „bleibt harte Bedingung“.
- K7 ist richtig als „bewusst offen“ geführt.
- Nichts ist abgeschwächt. Keine Stelle bezeichnet K1–K8 noch als offen (die Überschrift „Klaerungsbedarf … ERLEDIGT“ ist korrekt).
- Das ergänzte Sichtungsverbot für Tagesanalysen ist eine Verschärfung und mit Opus' Hinweis konsistent.

**Befunde:**
- **Auflage F1 (R3 veraltet):** R3 sagt, `h1_auswertung.cjs` sei „nicht vorhanden“. Das Skript existiert inzwischen (Entwurf, 363 Zeilen, nicht 351). Den Absatz aktualisieren.
- **Auflage F2 (R5, neu: Kombi-ROT ist nicht eindeutig).** Checkliste 5 sagt „Kombi-ROT-Fälle“, das Log führt aber `ampel_alt` **und** `ampel_kombi`. Altbeleg 29.09.: alt = ROT, kombi = GELB. Außerdem undefiniert:
  - UNBEKANNT: wird beim Sizing wie ROT behandelt, siehe `gate_check.cjs:1234`.
  - 0 Fälle.
  - ROT-Fälle ohne Nachtrag.

  **Gewichtig:** Kombi-Einträge entstehen **nur an fiktiven Testtagen**. Wird nach dem Freeze echt gehandelt (#44), gibt es keine Kombi-Fälle mehr. Mit der Skriptregel „0 Fälle = NICHT AUSWERTBAR“ wäre H1 dann **dauerhaft unauswertbar**. Festlegung vorab nötig, Empfehlung unter (ii).
- **Auflage F3 (R6, neu: Auswertungszeitpunkt).** Die Datei legt keinen Stichtag und keine Zahl der Auswertungen fest. Wer `--echt` nach jedem Testtag laufen lässt, bis „ZULAESSIG“ erscheint, betreibt optional stopping. Das wäre genau die Nachjustierung nach Sicht, die Abschnitt 4 verbietet.

  **Empfehlung:** höchstens **eine** Zwischenauswertung, sobald Zelle A erstmals ≥ 15/6 erreicht, und **eine** Endauswertung zu einem festen Stichtag, z. B. 30.10.2026 oder nach 12 gezählten Tagen. Was zuerst kommt, notieren. Läufe davor nur als Zählstand-Abfrage (n/Tage der Zelle A), ohne R-Werte. Das muss das Skript dann können, siehe S4.
- **Auflage F4 (R7, neu: Skript-Zusatzregeln dokumentieren).** Das Skript trifft 4 Zuordnungsentscheidungen, die in K1–K10 nicht stehen:
  - `bias['1h']` null im Fenster → „nicht bestimmbar“ statt „Trend nein“
  - `abstand_atr` fehlt → nicht bestimmbar
  - Moment vor Session-Start → ausgeschlossen
  - Join: exakter ts zuerst, sonst datum + vc mit letztem Eintrag ≤ Moment

  Alle 4 sind konservativ und vertretbar. Sie müssen aber **vor** Sicht der 01.10.-Daten in Abschnitt 7 stehen, sonst sind sie später angreifbar.
- **Hinweis:** K4 misst `exit_ts − ts`, und `exit_ts` ist der **Schluss** der SL-Bar (`tagesmomente.cjs:126`). Die Dauer wird also bis zu 5 Min überschätzt, schnelle SL werden eher untergezählt (leicht nachsichtig). Das entspricht dem Wortlaut von K4, deshalb bleibt es so, ist aber bekannt.

**Eindeutigkeit:** Mit F2–F4 ist die Auswertung ohne Ermessen durchführbar. Ohne F2/F3 nicht.

## 4. h1_auswertung.cjs (Entwurf)

**(a) Statistik korrekt.**
- `betaPosterior` habe ich gegen 2 unabhängige Verfahren geprüft:
  - exakte Binomialsumme: P(p > 0,5) = P(Bin(a+b−1; 0,5) ≤ a−1)
  - eigene Mittelpunkt-Integration mit 200.000 Stützstellen
- Bei 9 Werten stimmen alle 6 Nachkommastellen überein:

| Treffer/n | Posterior | P(Quote > 50 %) |
|---|---|---|
| 0/0 | Beta(5,5) | 0,500000 |
| 10/15 | Beta(15,10) | 0,846272 |
| 8/15 | Beta(13,12) | 0,580590 |
| 12/15 | Beta(17,8) | 0,968043 |
| 11/18 | Beta(16,12) | 0,778966 |
| 20/30 | Beta(25,15) | 0,945935 |
| 3/15 | Beta(8,17) | 0,031957 |
| 15/15 | Beta(20,5) | 0,999228 |
| 9/16 | Beta(14,12) | 0,654981 |

- Nicht-ganzzahlig: I₀,₃(2,5; 3,5) = 0,296753 in beiden Verfahren.
- Selbsttest: 36/36.

**(b) Treue zu K1–K10 und Checkliste:**
- Alle 6 Punkte plus Mindest-n sind umgesetzt, alle gleichzeitig nötig. Ø R ohne besten Tag zieht den Tag mit der höchsten Tagessumme komplett ab.
- K1: `unabhaengig` stammt aus tagesmomente (eine fiktive Position zur Zeit, `tagesmomente.cjs:202-209`), die Zellen werden erst danach zugeordnet ✓.
- K9 inklusive Fensterprüfung ist identisch zu `tagesmomente.tageInfo` (`tagesmomente.cjs:242/261-264`).

**(c) Integritätsschutz greift.**
- Probelauf: Momente, Shadow und Kombi werden per `datum < Z_START` gefiltert. Ab 01.10. gibt das Skript nur die *Anzahl* ignorierter Zeilen aus.
- `--json` und `--details` arbeiten nur auf gefilterten Daten. Der Selbsttest prüft, dass das Probe-JSON keinen 01.10.+-Datumsstring enthält.
- `--echt` gibt den Hinweis immer aus (stderr + stdout bzw. JSON-Feld).
- Umgehbar ist der Schutz nur absichtlich:
  - mit einer Logkopie, deren Daten umdatiert sind (`--momente-log`)
  - über den Export `auswerten({modus:'echt'})`
  - durch Ändern von Z_START

  Das ist technisch nicht vermeidbar und Disziplinsache.
- **Mangel S1:** `parseArgs` macht aus `--echt nein` bzw. `--echt false` → `args.echt = 'nein'` → **Echtmodus**. Selbst probiert, nur Altdaten.
- **Hinweis:** Probelauf-Zeile „kombi N“ zeigt die Zahl der Kombi-Einträge ab 01.10. (harmlos).

**(d) Join, Zeit und Randfälle:**
- Exakter ts-Join traf im Probelauf alle Momente, kein Rückfall nötig.
- DE-Zeit läuft über Intl, `TZ=` kommt nicht vor. Selbsttest 27.10. → Soll 14:30 = 13:30Z ✓.
- Fehlendes Pflichtlog → Exit 1. Fehlendes Kombi-Log → leer.
- Tag mit < 20 VC wird nicht gezählt (getestet).
- Mitternacht ist irrelevant (Fenster bis 20:00).
- Ungetestet: Join-Rückfall datum + vc, bias null, Kombi ohne Nachtrag, End-to-End-Lauf an einem Divergenztag (s. S3/S4).

**(e) Schreibt nichts.** `grep` nach writeFile/append/rename/unlink/spawn/exec findet 0 Treffer. Die sha1 der echten Logs ist identisch (s. o.).

**(f) Probelauf auf Kopien:**
- 351 Momente, 8 gezählte Tage, 277 qualifiziert.
- FLOOR gezählt 17, Zellen A 2 / B 7 / C 4 / D 4.
- Σ R A +0,00 / B −1,00 / C +2,17 / D −2,35.
- AUTO A 1 / B 3 / C 2 / D 2.
- Ausschlüsse FLOOR: q2 null 10, K5 12, vor Session-Start 1.
- **Mit eigenem, unabhängig geschriebenem Code exakt reproduziert.**
- Die Kombi-Zahl (n 3 ROT, Σ +1,89) passt zum Log: 0,23 + 0,05 + 1,61.
- Urteil „ZU WENIG DATEN“, korrekt.

**Weitere Befunde:**
- **Auflage S2:** Die Kommentare in Z. 33 (K1) und Z. 41 (K4) sagen noch „Levi-Entscheid ausstehend“. Nur Text, aber widersprüchlich zu Z. 26 und zur H1-Datei.
- **Auflage S3:** Die Kombi-Regel (Z. 63-65) an die Entscheidung zu (ii) anpassen.
- **Auflage S4:** Umsetzung von F3. Entweder ein Modus `--echt --nur-zaehlstand` (n/Tage der Zelle A, keine R-Werte), oder die Regel steht rein organisatorisch in der H1-Datei.
- **Hinweis:** DIVERGENZ-Tabelle ist aus tagesmomente dupliziert (bei Verlängerung beide Stellen nachziehen).

## 5. Empfehlungen zu (i)–(v), H8, H9

Alle Empfehlungen sind **heute** vor Sicht von H1-Daten ab 01.10. zu entscheiden. Im momente_log gibt es 0 Zeilen ab 01.10. (nur gezählt, nicht gesichtet). Damit respektieren sie die Vorab-Regel.

- **(i) Hälften-Basis K3: Skript beibehalten (alle K9-Tage).**
  - Das ist der Wortlaut („gezählte Tage“ = K9 „welche Tage zählen“), und es ist der echtere Stabilitätstest: Liegen alle Zelle-A-Momente in der ersten Zeithälfte, ist die Stabilität eben **nicht** gezeigt.
  - Eine leere Hälfte ist NICHT AUSWERTBAR = weiter sammeln, **nicht** NICHT BESTANDEN. Konservativ, ohne Fehlpositiv-Risiko.
  - Die Alternative (nur Tage mit Zelle-A-Moment) würde Trend-Cluster künstlich auf beide Hälften verteilen.
  - R1 damit schließen.
- **(ii) Kombi-ROT: `ampel_kombi === 'ROT'` (Wortlaut „Kombi-ROT“). UNBEKANNT nicht mitzählen, aber getrennt ausweisen. Fenster: datum ≥ 01.10. bis letzter gezählter Tag.**
  - **0 Fälle = „keine Fälle, kein Gegenbefund“ → Punkt 5 gilt als erfüllt (leere Summe 0 ≥ 0), mit Pflichtvermerk im Bericht.** Sonst blockiert ein Echtgeld-Zeitraum ohne Fiktivtage H1 dauerhaft (s. F2).
  - ROT-Fälle ohne Nachtrag → Punkt 5 NICHT AUSWERTBAR, bis der Nachtrag da ist. Der Nachtrag ist ohnehin Pflicht beim Tagesabschluss.
  - Levi entscheidet.
- **(iii) K5 bei Teiltagen: K5 unverändert lassen.**
  - Die 12 verlorenen FLOOR-Momente stammen aus 3 Spätstart-Tagen (Shadow ab 21.09. 16:00, 25.09. 17:47, 29.09. 16:42). Bedingung 1 („seit Session-Start ununterbrochen“) ist dort nicht prüfbar, eine Rekonstruktion wäre erfunden.
  - Folge für den Betrieb: Startet der Loop später als 15:45 (im DST-Fenster 14:45), fällt der Tag für H1 praktisch aus. Pünktlicher Start ist das einzige Gegenmittel.
- **(iv) K9-Fensterprüfung: beibehalten (`K9_FENSTER_PRUEFEN = true`).**
  - „tagesmomente-bewertbar“ schließt das Fenster laut tagesmomente selbst ein (`tagesmomente.cjs:224-264`). Das ist also keine Zusatzregel, sondern die wörtliche K9-Definition.
  - Den Schalter als Konstante nicht mehr anfassen.
- **(v) R1–R4:**
  - R1: geschlossen durch (i).
  - **R2: geschlossen.** tagesmomente kennt keinen Stall-Exit (`tagesmomente.cjs:25`). Die exit_art-Werte sind TP1-HIT/SL-HIT/OFFEN/null, und null ist durch `unabhaengig` ausgeschlossen (Z. 207). „Alles außer TP1-HIT = kein Treffer“ ist vollständig.
  - **R3: aktualisieren** (F1).
  - **R4: bleibt als bekannte Messgrenze, K6 ist maßgeblich.** `bias['1h']` im Shadow-Log ist `biasVon(--close-1h, --ema50-1h)` des VC (`vollcheck.cjs:1306`). Ob `--close-1h` der letzte *geschlossene* 1H-Bar ist, legt vollcheck nicht fest, das ist Operatorsache. Nicht nachjustieren.
- **H8 („nie vom Schatten abschreiben“, `loop_prompt.cjs:216`):** Levi sollte bestätigen.
  - Der Satz hat keine Gate-Wirkung.
  - Er ist die Voraussetzung dafür, dass KONSISTENT/WIDERSPRUCH überhaupt etwas misst. Ein abgeschriebener manueller Wert macht das B1-Vorab-Kriterium wertlos.
  - Keine Änderung nötig, nur Bestätigung.
- **H9 (Test liest echte Memory-Datei, `tests/trading_scripts.test.js:5575-5580`, Muster wie bei Z. 1685/1702):**
  - Akzeptieren, es gibt einen Präzedenzfall.
  - Empfehlung für später (kein Commit-Hindernis): im Skip-Fall eine `t.diagnostic`-Meldung ausgeben. Nach dem Mac-Umzug ändert sich der Projektpfad-Schlüssel, dann wird still übersprungen.
  - Eine Memory-Umformulierung kann den Repo-Test brechen. Das ist gewollt (Kopplung Prozesstext ↔ Memory), aber bekannt.

## 6. Neue Befunde nach Schwere

- **Blockierend:** keine.
- **Auflagen:** A6 (rest), F1–F4, S1–S4 (siehe Liste unten).
- **Hinweise:**
  - B5-Satz in `feedback_live_trading.md` sagt nur „ab 15:30“, ohne DST-Zusatz. loop_prompt ist korrigiert, die Memory-Datei nicht. Konsistenz.
  - A2-Test fällt zwischen 00:00:00 und 00:00:45 DE um (Shadow-ts Vortag). Irrelevant.
  - K4-Messung überschätzt die Dauer um ≤ 5 Min.

## 7. Restauflagen (Fable-Nachbesserungsauftrag, keine Gate-Wirkung)

1. **A6:** `MEMORY.md` Z. 7 (H1) auf ≤ 200 Zeichen kürzen (Codepoints zählen, ganze Zeile inklusive Link). Vorschlag: den Text „Quelle opus_antwort_d…“ streichen, die Quelle steht in der Datei. Optional Z. 9 (216) und Z. 19 (208) kürzen. Ob `opus_antwort_d_qrot_2026-10-01.md`, `opus_gegencheck_auftrag_b_h1_2026-10-01.md` und dieser Bericht eigene Indexzeilen bekommen oder ins Archiv gehen, entscheidet Levi.
2. **F1:** H1-Datei, R3 auf den aktuellen Stand bringen: Skript existiert als Entwurf, 363 Zeilen, nicht committet.
3. **F2/F3/F4:** H1-Datei Abschnitt 7 ergänzen, **nach Levi-Entscheid zu (i)/(ii) und zum Auswertungsplan**, vor dem ersten `--echt`-Lauf:
   - R1 = alle K9-Tage, leere Hälfte = NICHT AUSWERTBAR.
   - R2 geschlossen.
   - R5 Kombi-ROT-Definition (ii).
   - R6 Auswertungsplan (max. 1 Zwischen- + 1 Endauswertung, Stichtag).
   - R7 die 4 Skript-Zusatzregeln.
4. **S1:** `h1_auswertung.cjs` `main()`: `--echt` nur als reines Flag akzeptieren (`args.echt === true`), sonst Exit 1. Ebenso unbekannte Optionen → Exit 1.
5. **S2:** Kommentare Z. 33 und Z. 41: „Levi-Entscheid ausstehend“ → „Levi 01.10.2026 freigegeben“.
6. **S3:** Kombi-Regel (Z. 63-65, `auswerten` Z. 196-198, Checkliste Punkt 5) gemäß (ii) umsetzen und Selbsttests ergänzen: 0 Fälle, ohne Nachtrag, UNBEKANNT getrennt, Obergrenze letzter gezählter Tag.
7. **S4:** Auswertungsplan (F3) technisch stützen (`--nur-zaehlstand` ohne R-Werte) oder ausdrücklich nur organisatorisch regeln. Zusätzliche Selbsttests: Join-Rückfall datum + vc, bias null, Divergenztag End-to-End.
8. Danach ein kurzer Opus-Gegencheck nur für S1–S4/F2–F4.

## 8. Commit-Empfehlung (Ausführung nur durch Levi)

- **Repo, Commit 1 (jetzt möglich):**
  - `scripts/gate_check.cjs`
  - `scripts/vollcheck.cjs`
  - `scripts/loop_prompt.cjs`
  - `tests/trading_scripts.test.js`

  Die Commit-Message sollte die Spezifikationsabweichungen B1/B6 (A7) und H8 nennen.
- **Repo, Commit 2 (nach S1–S3, möglichst S4):** `scripts/analyse/h1_auswertung.cjs`. Erst nach Levis Entscheid zu (ii)/F3, damit der Code die fixierte Regel trägt.
- **Nicht committen:**
  - `scripts/loop_archiv/`
  - `scripts/last_gate_check_1942*_exit1.txt`
  - `scripts/analyse/backtest_2026-09-2{4,5}.cjs`

  Die backtest-Dateien sind nicht Teil dieses Pakets und nicht geprüft.
- **Memory-Repo (nur lokaler Commit, KEIN Push, Repo ist öffentlich):**
  - `MEMORY.md` (nach A6)
  - `feedback_live_trading.md`
  - `opus_vorschlag_2026-09-30.md`
  - `opus_antwort_d_qrot_2026-10-01.md`
  - `opus_bericht_testtag_2026-09-30.md`
  - `opus_gegencheck_auftrag_b_h1_2026-10-01.md`
  - `project_h1_q2_trendkontext_vorabkriterium_2026-10-01.md` (nach F1–F4)
  - `project_testtag_2026-09-30_abschluss.md`
  - `opus_gegencheck_2_2026-10-01.md`

**Nicht geprüft:** `feedback_live_trading.md` außerhalb B2a/B5, die backtest-Skripte, Fables „5 Vollläufe“ (`%TEMP%\fable_a1_runs.txt` nicht gelesen; meine 3 Läufe sind grün).
