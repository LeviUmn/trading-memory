# Opus-Analyse fiktiver Testtag Di 29.09.2026 (NAS100)

**Urteil: EINGESCHRÄNKT.** Im gemessenen Fenster 16:42–18:31 lief der Loop technisch sauber, und die Regeln haben neutral bis richtig entschieden. 60 % der Session waren aber nicht gemessen: 15:30–16:42 (später Start) und 18:32–20:00 (Usage-Limit). Die einzige echte Abwärtsbewegung des Tages (18:40, −54 Pkt in einer Kerze) fiel genau in diese Lücke. Es gab 0 Trades. Ein PASS wurde wegen Q-ROT ausgelassen, und der fiktive Kombi-Einstieg brachte +0,27 R.

Quellen: vollcheck_log, gate_check_log (23 Vorprüfungen + 3 Live-Läufe), oneh_shadow_log, anker_auto_log, sl_anker_wechsel_log, skipped_setups_fiktiv, kombi_fiktiv_log, tweet_fetch_log, register_touch_log, loop_stopp_log, loop_archiv/2026-09-29.txt, nas100_5m_2026-09-29.json sowie ein Live-Check von TradingView um 20:50 (read-only). `tagesmomente.cjs` lief nur gegen **Scratchpad-Kopien**: `momente_log.jsonl` im Repo ist unverändert, der offizielle Tagesabschluss-Lauf fehlt noch.

---

## (A) Technik

| Punkt | Befund aus den Logs |
|---|---|
| Voll-Checks | 23 Slots von 16:42 bis 18:30 ohne Lücke (Abstand 4–6 Min), dazu 1 Hard-Exit (VC#17 18:01:22, fehlendes `--stale-n`, 36 s später wiederholt). Hard-Exit-Quote 1/23 = 4,3 % |
| Quick-Ticks | 81 Ticks von 16:43 bis 18:32. Die Minuten ohne Tick sind fast nur :x0/:x5, weil der VC dort erst um :x1 lief. Echte Lücke praktisch keine |
| Lücke 18:32–20:00 | Kein VC, kein Tick. `loop_stopp` 20:41 hat den Grund korrekt geloggt (Usage-Limit) und Exit „keine Position“ gemeldet. Verloren ging der Abverkauf 18:40–18:45 (Tief 30229,45) und alles danach. Das ist eine **Messlücke, kein Regelbefund** |
| Register | Touches um 16:38, 16:45, 16:56, 17:06, 17:25 und 18:30. Zwischen 17:25 und 18:30 lagen **65 Min ohne Pflege**, also die ganze aktive Phase. Inhaltlich kamen nur Rundzahl-Bänder und Fib dazu, das US-Session-Tief 30256,55 (17:25) wurde nicht eingetragen. Das Gate lief mit einem 37–46 Min alten Register. Folgen hatte das heute keine, weil Q4 an der 30300 scheiterte |
| Tweet-Check | 12 Fetches, alle ~10 Min, also kein echtes Über-Polling. 12× „Slot verpasst, nachgeholt“ ist ein reines Timing-Artefakt (VC feuert :x1 nach dem :x0-Slot). Einmal Über-Polling-Lücke bei VC#17 (Artefakt) |
| Screenshots | **Sonnets Angabe ist falsch.** Nicht „3 Auslassungen (Y6 max 3/6)“, sondern **18 von 23 VCs mit Screenshot-LUECKE** und Y6 bis **4/6**. Frische Screenshots gab es nur bei VC#5, #10, #15, #17 und #20 (5 Dateien plus Start-Update) |
| kerzen-qqq-Warnung | Der Wert 3 ist **richtig**. QQQ schloss erst 17:30 unter der 15m-EMA50, bis 18:11 folgten 17:30/17:45/18:00. Die Warnung ist ein Fehlalarm der Heuristik (sie nimmt an, dass beide Instrumente gleichzeitig kreuzen). Kein Operator-Fehler |
| `--jetzt` vorausgeraten | Im tweet_fetch_log belegt sind 3 Fälle mit 12–22 s Vorlauf (18:01, 18:11, 18:21). Die genannten 70 s finde ich im Log nicht. Wirkung hatte es keine |
| SL im Live-Gate (2)/(3) | Bestätigt. Übernommen wurde 30377,35, also der Floor für den Vorprüfungs-Entry 30317,05. Für den Entry 30306,05 wäre der Floor 30366,35 gewesen (Anker 30337,45 + 0,5×ATR = 30357,55 bindet nicht). Lauf 3 hätte dann RR 1,76 statt 1,49 gehabt. Am PASS/Q-ROT ändert das nichts |
| X1 „fällig seit 17:31 = 8 VCs“ | **So nicht richtig.** X1 war nur bei VC#11 (17:31, 2,72×ATR) und VC#18 (18:06, 2,56×ATR) fällig. Bei VC#12–#17 hieß es „kein Reset fällig“ (1,76–2,33×ATR, weil der Kurs zurücklief). Die P2-Zeile zählt „seit dem ersten Fällig ohne Reset“ und schließt die nicht fälligen VCs mit ein. **Kleiner Anzeige-Mangel der A2-Zeile** |
| Ebenfalls übersehen | Von 16:42 bis 17:00 blockierte der 1H-Override **Longs**: der 15:00-Bar schloss 30333,35 unter der EMA50 von 30349,9, während 15m und QQQ long standen. Siehe (F) |

Die übrigen Sonnet-Angaben stimmen alle mit den Logs überein: Loop-Start, Gate-Zahlen, Nachträge (Setup 1: +0,44 R / MFE 61,6 / MAE 26,9; Setup 2: TP1 in der Kerze 18:40–18:45 = +0,78 R; Setup 3: +0,84 R / MFE 76,6 = 1,07 R / MAE 9,0; Kombi: Stall-Exit 19:15 bei 30267,15 = +0,55 R × 0,5 = +0,27 R) und 1H-Werte. Einzige Abweichung: Das TP1-Fenster „[30196; 30256,75]“ aus Lauf 2 stammt aus der **Vorprüfung** (Entry 30317,05). Für den Live-Entry 30305,65 lag es bei [30185,05; 30233,95].

## (B) Momente und die 3 Live-Gate-Läufe

**tagesmomente (gemessenes Fenster):** 23 Momente, 13 qualifiziert. Unabhängige Bewegungen: ALT 1 (−1,00 R), AUTO 1 (−1,00 R), FLOOR 2 (−1,56 R).
Den Tag prägt ein Messartefakt. Die gezählte Bewegung ist der **Long-Moment 17:01** (Dual-Gate long, 1H nach dem 17:00-Schluss long). Er wurde nie gehandelt (Fenster leer, SL-Anker 3,9×ATR). Er läuft bis zum SL um 18:45 und sperrt nach der Regel „nur eine fiktive Position“ alle Short-Momente von 18:01 bis 18:30 für die Zählung. Einzeln betrachtet haben diese Short-Momente in allen Varianten TP1 erreicht oder standen bei 20:00 mit +0,4 bis +0,7 R offen. Dasselbe Muster wird an jedem Tag mit Richtungswechsel auftreten, deshalb zur Kenntnis. Eine Änderung schlage ich nicht vor (die Definition steht fest).

**Hypothetisch 18:35–19:55** (Short-Bias unterstellt, QQQ ungemessen, RR-1-Ausgang, eigene Nachrechnung):
- 18:35/18:40: alle Varianten TP1 bei 18:50. Diese Bewegung ist aber mit der gezählten 18:25/18:30-Bewegung identisch und damit **nicht unabhängig**.
- 18:45 (nach der Sturzkerze): mit dem ALT-Anker ist das Fenster zu (3,05×ATR). AUTO/FLOOR hätten den SL um 19:30 getroffen (−1 R).
- 19:05/19:10: AUTO/FLOOR mit SL um 19:40 (−1 R).
- 19:25–19:45: AUTO/FLOOR TP1 knapp um 19:55/20:00.
- Als Gate-Lauf wahrscheinlich: 18:35 mit Anker 30337,45 → SL 30356, TP1 30200, RR ≈ 1,5 PASS. Q4 scheitert aber an der 30250 davor (Ratio ≈ 0,47), also **wieder PASS + Q-ROT**. Außerdem galt das 18:11-Setup seit 18:21 als verfallen, es hätte also einen neuen Trigger gebraucht.
- **Schluss:** Die Lücke hat mit hoher Wahrscheinlichkeit keinen Trade gekostet. Gekostet hat sie Messpunkte (vermutlich auch den ersten P3-Fall, siehe D).

**Lauf 1 (18:03, FAIL RR 0,89).** Das Gate hat richtig mit dem Live-Kurs gerechnet. Einen stale Entry gab es nicht: Nicht der Gate-Aufruf war veraltet, **das Setup aus der Vorprüfung war es.** Kurs 30299,65 um 18:02:06 ergibt RR 1,063, Kurs 30291,05 um 18:03:13 ergibt 0,89. Die W3-Zeile sagte korrekt „RETEST ABWARTEN“ und es wurde danach gehandelt. Die eigentliche Ursache ist der Anker: Mit dem AUTO-Anker 30337,45 (bestätigt 17:50) wäre Lauf 1 **PASS** gewesen (SL 30358,3, RR 1,35). Q-Score dann aber 2/4 ROT (Q1 nein, Q4 = 0,45 wegen 30250), also **auch dann ausgelassen, Geldwirkung 0**. X1 hat zwischen 17:36 und 18:02 zu Recht nichts verlangt, die Distanz blieb unter 2,5×ATR.

**Lauf 2 (18:11:26, FAIL RR 0,776).** Das war ein Fehler bei der TP1-Wahl: 30250 passte zum Vorprüfungs-Entry 30317,05, nach 11,4 Pkt Kursrutsch aber nicht mehr. Die W3-Zeile nannte 30200 als PASS-fähig, und 18 s später wurde korrigiert. **Folge: keine.** Lauf 1 und Lauf 2 haben dasselbe Muster: Zwischen Vorprüfung und Gate driftet der Kurs, und TP1/SL werden aus der Vorprüfung übernommen. Ohne Regeländerung wäre das so zu lösen: TP1 und SL immer aus den W3-/Fenster-Zeilen des Live-Laufs übernehmen, nicht aus der Vorprüfung. Das ist eine reine Operator-Disziplin, das Skript liefert die Zahlen bereits.

**Lauf 3 (18:11:44, PASS, Q 2/4 ROT, ausgelassen).** Nach Option D war das regelkonform. Der Q4-Wert von 0,06 entsteht, weil die generierte Rundzahl 30300 6 Pkt vor dem Entry liegt. Der Kurs hat sie sofort durchbrochen. Danach hing er aber 75 Min zwischen 30230 und 30270 fest, genau um die 30250, und **TP1 30200 wurde nie erreicht** (Tief 30229,45, 29 Pkt davor).
Einordnung des Ergebnisses:
- Mit Stall-Exit (Kombi): +0,55 R (+0,27 R bei 0,5 Größe).
- Stichtag 20:00: +0,84 R.
- **Offen gehalten: SL 30377,35 wird in der Kerze 20:40 getroffen** (Hoch 30385,15, Live-Check 20:50), also −1 R.

Bei n=1 ist das kein Urteil über Q-ROT. Das Auslassen war vertretbar, das Ergebnis hing allein am Exit-Management.

## (C) Trend 23./25./28./29.09.

| | 23.09. | 25.09. | 28.09. | 29.09. (Teiltag 16:42–18:31) |
|---|---|---|---|---|
| Momente qualifiziert | 22 | 25 | 35 | 13 |
| Live-Gate-Läufe (gewertet) | 0 | 0 | 6 | 3 |
| PASS | 0 | 0 | 4 | 1 |
| Trades | 0 | 0 | 0 | 0 (Kombi fiktiv 1: +0,27 R) |
| AUTO unabhängig (Σ R) | 2 (+0,79) | 2 (−0,70) | 3 (+0,03) | 1 (−1,00) |
| FLOOR unabhängig (Σ R) | 3 (−0,40) | 2 (−0,67) | 8 (−2,95) | 2 (−1,56) |

**Freeze-Stand** (`--auswertung` auf einer Scratch-Kopie mit 29.09.):
- Formal **3/5 bewertbare Tage, AUTO 6/20**.
- AUTO seit 25.09.: Σ −1,67 R, Ø −0,28 R. FLOOR: n=12, Σ −5,18 R.
- **Freeze-Ende: NEIN.**

**Einwand:** Das Skript zählt den 29.09. als bewertbar, weil es nur „≥20 VCs + Bars ok + Default-Fenster“ prüft. Die tatsächliche Loop-Abdeckung war 1 h 49 von 4 h 30 (40 %). Mit 20 VCs lassen sich schon 1 h 40 als „voller Tag“ werten, und das ist eine Lücke in der Definition. Meine Empfehlung: den 29.09. **als Teiltag nicht mitzählen**, dann 2/5 und AUTO 5/20. Das entscheidet Levi.

## (D) P3-Auswertung (erster echter Tag)

- **Resets gesamt 1 | davon Kleinstschritt 0.** Um 18:11 wechselte der Anker von 30372,55 auf 30337,45, Δ 35,1 Pkt = 0,87×ATR ≥ 0,5. Dazu kam 1 Richtungswechsel um 17:31 (long → short, kein Reset).
- **P3-Hard-Exits: 0/23 = 0 %.** Hard-Exits gesamt 1/23 = 4,3 %, Rückbau-Schwelle 15 % nicht berührt.
- **Vorab-Kriterium 1** (Anker hinkt in ≤20 % der Momente >90 Min hinter AUTO): **0/13 = 0 %, erfüllt.** Aussagekraft fast keine. Die Short-Phase war nur 60 Min gemessen, ein Hinterherhinken >90 Min war also gar nicht möglich. In der Long-Phase waren ALT und AUTO identisch (30267,35).
- **Vorab-Kriterium 2** (Reset ≤1 VC nach Fälligkeit oder begründet abgelehnt): **erfüllt.**
  - Fällig um 17:31: AUTO war n/a (kein bestätigter Swing), es gab also kein Reset-Ziel. Ab 17:36 war es nicht mehr fällig.
  - Fällig um 18:06: Reset um 18:11 = VC+1.
  - Die „8 VCs Verzug“ sind ein Artefakt des P2-Zählers.
- **Warum P3 nicht griff:** Es gab nur Distanz-Auslöser (Y1-b), und die lösen per Definition keine Pflicht aus. Das Impuls-Delta blieb 0, weil der Anker um 17:31 *nach* dem Impuls gesetzt wurde. Ein neues Extrem jenseits von 30256,55 kam erst um 18:40, im ungemessenen Fenster. Dort hätte das Impuls-Delta ab dem Kurs beim Reset (30317) bei ≈87 Pkt ≈ 2,2–2,4×ATR gelegen. **Wahrscheinlich lag dort der erste echte P3-Fall.** Getestet ist P3 damit **nicht**.
- **War „Distanz-Auslöser ohne Pflicht“ der Engpass?** Heute nein. Das Problem war ein anderes: Der bessere AUTO-Anker war von 17:50 bis 18:06 bestätigt und sichtbar, aber **kein Auslöser** (Distanz 1,95–2,45×ATR < 2,5) verlangte den Wechsel. Eine Pflicht für den Distanz-Auslöser hätte deshalb um 18:03 auch nicht geholfen. Hätte der AUTO-Anker gegolten, wäre aus FAIL nur PASS + Q-ROT geworden, also Geldwirkung 0. Der Fall „AUTO bestätigt ≠ ALT, aber X1 schweigt“ ist Anker-Nachführung und damit **Freeze-Thema**.
- **Empfehlung:** P3 unverändert im Modus `pflicht` lassen. Das heißt konkret: Regel, Auslöser-Liste und der 30-Min-Wert bleiben auch dann gleich, wenn der 29.09. eine Lücke andeutet. Der 29.09. zählt als Tag 1/3, hat aber **0 einschlägige P3-Fälle**. Vorschlag an Levi (Auswertungsregel, keine P3-Änderung): Das 3-Tage-Urteil gilt nur, wenn mindestens 1 impuls-fälliger Fall darin vorkommt. Sonst wird um bis zu 2 Tage verlängert.

## (E) Q3-auto und Tagesende-Regel (29.09. rückwirkend)

**Q3-auto:**
- Alle 3 Live-Läufe hatten `--q3-coherence yes`.
- Gegenprobe gegen die VC-Beine: um 18:01:58 und 18:11:04 lagen 1H (30312,15 < 30345,7/30346,5), 15m, 5m (Entry < EMA50-5m 30344,2/30341,3) und QQQ (737,32 < 738,05/738,07) alle short. `q3_auto` aus tagesmomente = ERFÜLLT.
- **0 Widersprüche, 0 UNBEKANNT → Tag 1/3 erfüllt.** Einschränkung: Alle 4 Beine gleichgerichtet ist der triviale Fall.

**Tagesende-Regel:**
- 1 PASS (18:11:44), ausgelassen mit dokumentiertem Grund (Q-ROT, Option D).
- Die Kombi-Position war um 19:15 per Stall-Exit geschlossen.
- **0 Auslassungen ohne Ablehnungsgrund → formal 1/3.** Die 19:55-Situation trat aber nicht ein und wurde auch nicht beobachtet (Loop aus). Der Tag belegt die Regel eigentlich nicht, er zählt nur nach Levis Entscheidung.
- Wie wichtig die Regel ist, zeigt Setup 3: +0,84 R bei 20:00, und danach wäre um 20:40 der SL getroffen worden (−1 R).
- **Offene Definition:** „19:55“ kann den Schluss der 19:50-Kerze meinen (30256,95 → Setup 3 = +0,69 R) oder den Schluss der 19:55-Kerze (30246,45 → +0,84 R, das aktuelle Verhalten der Nachtrag-Skripte). Das muss vor der Umsetzung festgelegt werden.

**Fable: ja, beide umsetzen, in dieser Reihenfolge** (Details siehe G):
1. Q3-auto
2. Tagesende

Beides ist Messung bzw. Auswertung, kein Gate und keine Loop-Änderung, also freeze-konform. Jeweils mit Opus-Gegencheck.

## (F) 1H-Override (Pflichtpunkt, Levis Wunsch)

Zu Levis Frage „Ist 1H Pflicht für Short?“: Ja. Seit 04.09. ist der 1H-Override ein **Veto**. Der letzte *geschlossene* 1H-Bar darf nicht gegen die Richtung stehen (EMA50-Seite). Eine Zusatzbestätigung verlangt er nicht.

**(1) Blockaden über alle Testtage** (`oneh_shadow_log`, blockade_3v4). Für 17.09. und 21.09. gibt es 0 Blockaden. Für 16.09. fehlen die Bars, das Ergebnis ist unbekannt. Bewertet ist jeweils der erste Moment jeder Episode, RR-1-Ausgang, tagesmomente-Logik mit gelöschtem 1H-Veto.

| Episode | Dauer (≥) | Bewegung während Blockade | FLOOR | ALT/AUTO | Override hat … |
|---|---|---|---|---|---|
| 15.09. Long 15:25–15:55 | 30 Min | 8,7 Pkt | −1 | Fenster zu | Verlust vermieden |
| 15.09. Short 16:10–16:58 | 49 Min | 96 Pkt (E7 ✓) | +1 | zu | Gewinn gekostet |
| 16.09. Short 20:35–20:51 | 16 Min | 0 | ? | ? | – |
| 23.09. Short 15:45–17:51 | 126 Min | 210 Pkt (E7 ✓) | +1 | AUTO +1 (ab 17:16, 6×) | Gewinn gekostet |
| 25.09. Short 17:52–17:56 | 4 Min | 0 | −1 | AUTO −1 | Verlust vermieden |
| 28.09. Short 15:46–15:56 | 10 Min | 9,6 | −1 | zu | Verlust vermieden |
| 28.09. Long 18:31–18:41 | 10 Min | 0 | −1 | AUTO −1 | Verlust vermieden |
| 28.09. Long 19:16–19:26 | 10 Min | 0 | −1 | ALT −0,37 offen | Verlust vermieden |
| 29.09. Long 16:42–16:56 | 18 Min (bis 17:00) | 11 Pkt | −1 | zu | Verlust vermieden |
| 29.09. Short 17:31–17:56 | 29 Min (bis 18:00) | 0 | +0,57 offen | ALT +0,32 offen (17:36: +1) | wenig gekostet |

- **10 Episoden, 9 bewertbar:** 6 × Verlust vermieden, 3 × Gewinn gekostet. **FLOOR netto: der Override hat ≈ +3,4 R gespart.**
- Andere Zählweise (tagesmomente-Kette, alle unabhängigen Bewegungen, 5 Tage) zum Vergleich:
  - Ohne Override: FLOOR +3,0 R besser, getragen von 23.09. (+4,0) und 15.09. (+2,0), während 28.09. −2,0 und 29.09. −1,0 zurückgehen.
  - AUTO: −0,49 R schlechter.
  - ALT: −1,0 R schlechter.
- Die 28.09.-Zahl „~−2 R ohne Override“ bestätige ich (FLOOR Δ −2,0 R).
- E7 (≥15 Min UND ≥1,5×ATR) erfüllen nur 2/10 Episoden, nämlich die beiden echten Trendtage (15.09., 23.09.). Das sind zugleich die einzigen, in denen der Override richtig Geld gekostet hat.

**(2) Trägheit, Varianten nachgerechnet** (aus den 5m-Bars und der EMA50-1H im Shadow-Log):
- Der Verzug ist real, bis 60 Min. Am 23.09. dauerte die Blockade 126 Min, weil zwei 1H-Bars in Folge über der EMA schlossen.
- **P8-Überschuss** (2 letzte 5m-Schlüsse ≥1,0×ATR jenseits der 1H-EMA50) hätte freigegeben:
  - 15.09. Short sofort (+1).
  - 23.09. erst um 16:21: dieser erste Moment endet im **SL**, und die Kette bringt −1,0 R gegenüber heute.
  - 28.09. Short sofort (−1).
  - 29.09. **Long um 16:42/16:45** (−1, in den Abverkauf) und Short um 17:31.
  - Ketten-Summe gegenüber der aktuellen Regel: FLOOR +1,0 R über 5 Tage, AUTO ±0, ALT ±0. **Ohne den 15.09.: FLOOR −2,0 R.**
- **Live-Seite** (laufender Kurs gegen 1H-EMA50, ohne Schwelle): FLOOR +1,95 R, AUTO/ALT ±0. Ohne den 15.09.: −1,05 R.
- **Fallstudie 29.09.:** Beide schnelleren Varianten hätten *zuerst den Long um 16:42* freigegeben, mitten in den späteren Fall von 17:00–17:25 (−1 R). Den Short hätten sie um 17:31 freigegeben. Um 17:31 war die Geometrie aber tot (TP1-Fenster 11,6 Pkt, kein Level). Um 17:36 wäre sie gegangen (SL 30393,6, TP1 30200, RR 1,61), mit Q-Score ≤ 2/4 ROT (Q3 nein wegen geschlossenem 1H, Q4 = 0,16 wegen 30300). Also **auch ohne Override kein Trade**. Nach dem Fall des Overrides um 18:00: 3 Gate-Läufe, 0 Trades aus anderen Gründen.
- Das Muster: Jede schnellere Variante gibt beide Richtungen schneller frei. Was sie an echten Wenden gewinnt, verliert sie an Fehlwenden.

**(3) Wo der Engpass wirklich liegt:**
- 29.09.: Um 18:03 und 18:11 scheiterte **kein** Lauf am Override. Die Gründe waren RR/Geometrie (stale Anker 1×, TP1-Wahl 1×) und Q-ROT (Q1 + Q4) 1×.
- Über alle PASS-Läufe mit Q-Faktoren seit 16.09. (n=16): **Q4 NEIN in 11/16** (oft wegen einer generierten 50er-Rundzahl direkt vor dem Entry), **Q1 NEIN/UNKLAR in 12/16**. Der Override blockt nur auf VC-Ebene: 10 Episoden in 11 Testtagen.
- Reihenfolge der Engpässe: **Q4/Q1 > Anker-Geometrie > 1H-Override.**

**(4) Empfehlung:**
- **Keine Änderung am Override.** Das gilt unabhängig vom Freeze: Die Daten sprechen eher *für* ihn (≈ +3,4 R gespart auf Episodenbasis), und keine schnellere Variante ist ohne den 15.09. positiv.
- „Mehr Entries“ würden bei Ø −0,51 R und Payoff 0,94:1 mehr Verlust bedeuten, nicht mehr Gewinn.
- Beim Freeze-Review neu ansehen, frühestens bei **≥20 bewertbaren Blockade-Episoden** (heute 9, ≈1,5 pro Tag, also ≈7 weitere Testtage).
- Kriterium für eine Variante dann: Ø R je freigegebener Episode ≥ +0,10 UND ≥ 0 ohne den besten Tag UND nicht mehr freigegebene Verlust- als Gewinnepisoden.
- Rückbau entfällt, weil nichts geändert wird. Die Messung braucht keinen neuen Code: `oneh_shadow_log` (ema50_1h, kurs) plus die Bars reichen, die Nachrechnung ist reproduzierbar.
- Overfitting-Warnung: n=9. Ein einziger Trendtag (23.09.) dreht das Vorzeichen.

## (G) Empfehlung

**Aufträge an Fable (max. 3, freeze-konform, jeweils mit Opus-Gegencheck):**

1. **Q3-auto als Schattenzeile in gate_check.cjs.** Aus vollcheck_state bzw. `--override-1h-*` und den VC-Beinen ableiten und bei Widerspruch zu `--q3-coherence` oder fehlendem Bein (UNBEKANNT) warnen.
   - Vorab-Kriterium: 0 unentdeckte Widersprüche + 0 UNBEKANNT in 3 Testtagen (29.09. = 1/3).
   - Rückbau: eine Konstante auf „aus“.
2. **Tagesende-Auswertungsregel.** skipped_fiktiv/kombi_fiktiv-Nachtrag mit Default-Stichtag „Schluss der 19:50-Kerze = 19:55 DE“ (oder 19:55-Kerze, Levi entscheidet). protokoll_bilanz meldet PASS-Auslassungen ohne Ablehnungsgrund.
   - Vorab-Kriterium: 0 Auslassungen ohne Grund bei PASS in 3 Tagen (29.09. = 1/3, Situation nicht eingetreten).
   - Rückbau: Default-`--bis` zurück auf 20:00.
3. **Abdeckungsprüfung für „bewertbarer Testtag“ in tagesmomente `--auswertung`.** Ausgewiesen wird der Anteil des Fensters, der mit VCs abgedeckt ist (Soll ≥ 80 %). Das ist reine Messung, die Freeze-*Definition* ändert Levi.
   - Vorab-Kriterium: Der 29.09. wird mit 40 % als Teiltag ausgewiesen.
   - Rückbau: Die Zeile ist nur Anzeige.

Kleinigkeiten, im Freeze erlaubt und ohne Auftrag: den A2-Zähler „X1 fällig seit N“ auf echte Fällig-VCs zählen lassen, die kerzen-qqq-Heuristik als Hinweis statt Warnung, das Register mit Session-Extrema pflegen.

**Levi entscheidet:**
- (a) Zählt der 29.09. als Freeze-Tag? (Empfehlung: nein → 2/5, AUTO 5/20)
- (b) Welche Kerze gilt als „19:55-Schluss“?
- (c) Bleibt der 1H-Override unverändert bis Freeze-Review? (Empfehlung: ja)
- (d) Wie wird ein Usage-Limit am Testtag vermieden (Budget vor 15:10 prüfen)? Zum zweiten Mal in einer Woche war ein Testtag nur teilweise gemessen.

**Levis Frage: Hätten andere Regeln heute mehr Trades gebracht, oder war alles richtig?**
- Mehr Trades: **ja, genau einen**, wenn Q-ROT kein Veto wäre (Kombi-Regel). Ergebnis +0,27 R mit Stall-Exit, +0,84 R mit Stichtag 20:00, −1 R bei Halten über 20:40.
- Ohne 1H-Override: **kein** zusätzlicher Trade (17:36 wäre PASS + Q-ROT gewesen). Über 5 Tage hat der Override eher Geld gespart.
- Der sofortige AUTO-Anker hätte aus einem FAIL einen PASS gemacht, der ebenfalls an Q-ROT hängen geblieben wäre.
- Die Regeln haben heute also nichts Belegbares verschenkt.
- Der eigentliche Engpass für mehr Trades ist Q4 (Rundzahlen direkt vor dem Entry) zusammen mit Q1. Genau das misst der Kombi-Schatten bereits: n=5, Σ +0,34 R gewichtet. Ob das „mehr Geld“ bedeutet, zeigt erst das Freeze-Ende, nicht ein einzelner Tag.
