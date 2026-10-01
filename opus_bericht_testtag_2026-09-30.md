# Opus-Analyse fiktiver Testtag Mi 30.09.2026 (NAS100) — nachgeholt am Do 01.10.2026

**Urteil: GÜLTIG, mit einem klaren Prozessmangel (Register).** Erstmals seit dem 28.09. ist ein Testtag voll gemessen: 54/54 Slots, Abdeckung 100 %, 56 Voll-Checks ohne Nummernlücke, frischer Screenshot in jedem VC. Der Markt lief einen Long-Trendtag (15:30–17:50 von 30410 auf 30635). Es gab 0 echte Trades und 1 fiktiven Kombi-Einstieg (+0,40 R). P3 hatte heute seine **ersten zwei echten Pflichtfälle** und hat beide sauber bestanden. Der Schwachpunkt war die Registerpflege: Das Session-Hoch stand von 16:01 bis Loop-Ende auf einem Zwischenwert (30584,85 statt 30620,35 bzw. 30635,25). Deshalb fehlte um 19:42 das TP1-Level, das aus dem FAIL einen PASS gemacht hätte. Geldwirkung: klein, aber ein Kombi-Datenpunkt ging verloren.

Quellen (Primär = Logs): vollcheck_log, gate_check_log (58 Vorprüfungen + 4 Live-Aufrufe), oneh_shadow_log, sl_anker_wechsel_log, skipped_setups_fiktiv, kombi_fiktiv_log, tweet_fetch_log, register_touch_log, quick_tick_log, loop_stopp_log, momente_log, loop_archiv/2026-09-30.txt (Faktenprotokoll Sonnet als Ergänzung), nas100_5m_2026-09-30.json, last_gate_check_1942(b)_exit1.txt.
- **Skriptläufe nur auf Kopien** in `%TEMP%\opus_2026-09-30\repo`: protokoll_bilanz, tagesmomente `--dry-run` und `--auswertung`, ibf_schatten, kombi_fiktiv `--auswertung`. Alle reproduzieren Sonnets Zahlen.
- `momente_log.jsonl` im Repo ist unverändert (sha1 9f870f4f… vorher und nachher).
- Live-Check nach 20:00 war **nicht möglich**: TradingView/CDP ist nicht erreichbar. Was nach 20:00 geschah, bleibt deshalb unbelegt.

---

## (A) Technik

| Punkt | Befund (aus Logs nachgerechnet) |
|---|---|
| Voll-Checks | #1–#56 lückenlos, 61 Logzeilen = 58 Exit-0-Läufe + 3 Hard-Exits. **Stimmt.** Die Läufe #10 und #34 stehen doppelt, weil die eingebettete SL-Vorprüfung mit „RESET-PFLICHT UNERFUELLT“ (P3) endete, siehe D. |
| Hard-Exits | VC#1 15:27 und VC#2 15:31: `--stale-n` fehlte. Das ist **derselbe Fehler wie am 29.09. (VC#17)**, jetzt 3× in 2 Tagen beim Loop-Start. VC#55 19:56: Register-Alter 61 Min. Dazu P3 2× in der Vorprüfung (16:11, 18:11) und 2× im Live-Gate (19:42: zuerst `--chasing`, dann `--grund-chasing` vergessen). Quote VC-Hard-Exits 3/56 = 5 %, P3 2/56 = 3,6 %. |
| „35 vollständig / 23 MIT LÜCKEN“ | Gezählt über die 58 Läufe stimmt das. Je VC (Endstand) sind es **35 / 21**. Alle 23 Läufe haben das Tweet-Artefakt. **2 davon (#10a, #34a) haben zusätzlich eine echte P3-Lücke.** „Alle durch das Tweet-Artefakt“ ist also leicht ungenau. |
| Tweet-Check | 30 Fetches (`set`/`polled`), **jeder 10-Min-Slot von 15:30 bis 20:00 genau einmal belegt, 0 echtes Über-Polling.** Die 23× „ÜBER-POLLING ✗“ sind ein reines Artefakt. Der Fetch läuft im Quick-Tick um :x0:4x. vollcheck.cjs startet ~1 Min später, bekommt `--tweet-fetch ja` und prüft die Fälligkeit gegen `--jetzt` (:x1), statt gegen den Zeitpunkt des Fetches (`last_poll`). Ergebnis: Die Lücken-Statistik ist heute zu 100 % Rauschen. 7× „Slot verpasst, nachgeholt“ ist dasselbe Muster mit umgekehrtem Vorzeichen. |
| Quick-Ticks | 185 Einträge, davon 184 im Fenster. **Die 13 Minuten ohne Eintrag stimmen**, allerdings nur nach Sonnets Konvention: Gezählt wird nach Schreibzeit, und die :x0-/:x5-Minuten gelten als VC-Startminuten (der VC-Log-Stempel liegt ~1 Min später bei :x1/:x6). Die 13 Lücken liegen fast alle in Gate-/VC-Spitzen (15:32/33, 19:42). Folgen hatte das nicht. |
| Screenshots | 57 Dateien (vc1–vc56 + session_update). In allen 58 Läufen steht der Y6-Zähler auf 0, alle frisch. 58 Behauptungen gegen 57 Dateien ist **keine echte Abweichung**: #10b und #34b verweisen auf dieselbe Datei. Großer Fortschritt gegenüber dem 29.09. (18/23 LÜCKE). |
| protokoll_bilanz | Auf der Kopie reproduziert: 2 Abweichungen (doppelte VC-Nummern 10/34; Screenshot 58 vs. 57). **Beide sind erklärbar, kein Befund.** Nebenbefund: Die Bilanz zählt die 4 Live-Aufrufe als „Exit-Code ?“ bzw. „nicht gefunden 2“, weil das Archiv im LIVE-GATE-Block keine `Exit-Code:`-Zeile hat (offener Punkt 6). |
| Register | 18 Touches, 15 davon mit Inhalt. **Session-Hoch:** zuletzt um 16:01 auf 30584,85 gesetzt („16:00-Kerze“), und zwar *während* der Kerze; die Kerze schloss mit Hoch 30600,65. Die echten Session-Hochs **30620,35 (16:05) und 30635,25 (17:50) kamen nie ins Register.** Sonnet kannte sie: Die Notiz von 17:16 nennt „Impuls-Hoch 30620.35“, VC#53 das Tageshoch 30629,95/30635,25. Danach gab es nur noch Zeitstempel-Touches (17:15, 18:55, 19:56, V11 „nur geprüft“). Inhaltlich war das Register ab 17:51 unverändert, beim Session-Hoch schon ab 16:01. **Das ist dasselbe Muster wie am 28./29.09.**, nur diesmal mit Wirkung (siehe B, 19:42). |
| Anker-Messung A2 | Die Zeile „AUTO-Anker: ohne --nas-bars-5m nicht berechnet“ steht in 58/58 Läufen, `anker_auto_log` hat 0 Einträge. Das ist **so gewollt** (Flag ist nicht im Template). AUTO wird nur nachträglich von tagesmomente gerechnet. Kein Befund. |

## (B) Momente, Gate-Läufe, Trades

**tagesmomente:** Reproduziert. 58 Momente, alle qualifiziert, Bars ok 66/66.

| Variante | Fenster offen | unabhängig | TP/SL/OFFEN | Σ R |
|---|---|---|---|---|
| ALT | 43 | 4 (15:27, 15:36, 16:46, 17:56) | 3/0/1 | **+3,01** |
| AUTO | 26 | 3 (16:25, 18:51, 19:41) | 1/1/1 | +0,80 |
| FLOOR | 58 | 5 | 3/1/1 | +2,04 |

Zum ersten Mal ist ALT die beste Variante. Am Trendtag passte der Vorprüfungs-Anker, sobald er nach P3 neu gesetzt war.

Ein Messdetail fürs Freeze-Review: Der AUTO-Moment 19:41 (+0,80 R) nutzt den Swing 30549,25 von 18:35. Die Kerzen 19:15 und 19:25 hatten ihn längst gebrochen (Tief 30492,35). Die AUTO-Definition („**erster** Swing nach dem Extrem, der unter dem Entry liegt“) lässt einen gebrochenen Swing wieder gelten, sobald der Kurs zurückkommt. Hier bindet ohnehin der Floor. Mit dem Swing 30492,35 wäre es +0,46 R statt +0,80 R gewesen. **Keine Änderung im Freeze**, nur notiert.

**Live-Gate 15:32:38: PASS, Q 1/4 ROT (Q1, Q2 2,01×, Q4 0,50 wegen Rundzahl 30500).**
- Long, Entry 30450,25, SL 30380, TP1 30550, Chasing yes (12 Kerzen, k 2/2). Option D: live ausgelassen.
- Kombi/V2: fiktiv mit Größe 0,25. TP1 in der Kerze 16:00, Stall-Exit 12.1a in der Kerze 16:20 bei 30576,65.
- Nachgerechnet: ½ × 1,42 + ½ × (126,4 / 70,25) = **+1,61 R × 0,25 = +0,40 R**. Ohne Stall: Rest bei 30586,95 → +1,68 R → +0,42 R. **Stimmt.**
- skipped_fiktiv (live ausgelassen, volle Größe): TP1-HIT 16:00, +1,42 R, mit Auslassgrund. Q-ROT hat heute also **+1,42 R (volle Größe) gekostet**. Bei n=1 ist das kein Urteil, aber der zweite Q-ROT-Gewinner in Folge (29.09. bis 20:00 +0,84 R).
- Die Retest-Zeitbox verfiel korrekt nach VC#4 (15:41).

**Live-Gate 19:42 (3 Aufrufe):**
- 17:42:01Z und 17:42:07Z: Hard-Exit (Chasing-Flag bzw. Grund fehlte). 17:42:15Z: **FAIL rrGate, RR 0,824** (Entry 30552,85, SL 30495,65 = 2,01×ATR am Anker 30509,85, TP1 30600). TP1-Fenster [30610,05 ; 30638,05]; laut W3 „kein Registerlevel“.
- **Eigene Nachrechnung Stichtag 20:00:** Gewertet werden die Kerzen 19:45, 19:50 und 19:55. Hoch 30594,25 (TP1 30600 verfehlt um 5,75), Tief 30547,45, SL nie berührt. Schluss der Kerze 19:55 bei 30586,95 → (30586,95 − 30552,85) / 57,2 = **+0,60 R OFFEN**, MFE 41,4 = 0,72 R. **Sonnets Nachtrag stimmt.** `bis_quelle` = Default 20:00, das heißt: **Der A1-Stichtag hat seinen ersten echten Einsatz bestanden.**
- **Was das Register gekostet hat:** Mit dem echten Session-Hoch 30635,25 im Register (auch 30620,35 lag im Fenster) wäre das Fenster nicht leer gewesen.
  - TP1 30635,25 → RR 82,4 / 57,2 = **1,44, Zone 2 (2,90×ATR) → PASS**.
  - Q-Score: Q1 nein, Q2 ja (0,4×), Q3 ja, Q4 nein (Gegenlevel 30600, Ratio 0,57) → **2/4 ROT, also live trotzdem ausgelassen.**
  - Kombi: Größe 0,25 (bei GELB über Q4-NEU-A 0,5; die Ampel lässt sich nicht exakt rekonstruieren). TP1 bis 20:00 nicht erreicht → +0,60 R × 0,25 = **+0,15 R** (bzw. +0,30 R).
  - **Geld 0, verloren ist 1 Kombi-Datenpunkt.**
  - Selbst ohne diesen Unterschied hätte der Lauf 6 Pkt tiefer im Entry-Fenster [30538,25 ; 30547,83] liegen müssen. Das Gate hat also richtig gerechnet.

**Trigger-Lücke am Trendtag (zur Kenntnis, keine Regelfrage im Freeze):** Zwischen 15:32 und 19:42 lief **kein einziges** Live-Gate, obwohl das Dual-Gate in 58 von 58 VCs auf 2/2 Long stand und ALT 43 offene Fenster hatte.
- Grund ist die Logik „Q-ROT geht vor, 13.1 beobachten, nur ein neuer Trigger zählt“ (VC#3–#37). Das ist regelkonform.
- Die (b)-Ausbrüche um 16:00 und 17:45 hat Sonnet erkannt und wegen der Geometrie verworfen: Fenster leer, SL 4,3× bzw. 3,3×ATR. **Beides stimmt**, ein Gate-Lauf wäre aussichtslos gewesen.
- Der ALT-Moment 16:46 (+1 R) entstand nur durch den P3-Reset, nicht durch einen Trigger. Ein Gate-Lauf dort wäre mit TP1 30650 PASS + Q-ROT gewesen. TP1 nicht erreicht, OFFEN +0,51 R × 0,25 ≈ **+0,13 R Kombi**.
- Diese „verpassten“ Messpunkte erfasst tagesmomente ohnehin. Der Kombi-Schatten wächst dagegen nur über echte Gate-Läufe, n=6 in 4 Tagen.

## (C) Trend 23./25./28./29./30.09.

| | 23.09. | 25.09. | 28.09. | 29.09. | **30.09.** |
|---|---|---|---|---|---|
| Abdeckung | 85 % | 50 % | 93 % | 43 % | **100 %** |
| Momente qualifiziert | 22 | 25 | 35 | 13 | **58** |
| Live-Gate gewertet / PASS | 0/0 | 0/0 | 6/4 | 3/1 | **2/1** (+2 Abbrüche) |
| Trades / Kombi fiktiv | 0/– | 0/– | 0/4 | 0/1 | **0/1 (+0,40)** |
| ALT unabh. (Σ R) | – | – | – | 1 (−1,00)* | **4 (+3,01)** |
| AUTO unabh. (Σ R) | 2 (+0,79) | 2 (−0,70) | 3 (+0,03) | 1 (−1,00) | **3 (+0,80)** |
| FLOOR unabh. (Σ R) | 3 (−0,40) | 2 (−0,67) | 8 (−2,95) | 2 (−1,56) | **5 (+2,04)** |

\*ALT-Werte vor dem 29.09. stehen nicht in meinen Berichten; seit 25.09. gilt: n 6, Σ +2,33.

- **Freeze (`--auswertung`, reproduziert):** 4/5 bewertbare Tage, **AUTO 9/20** (Σ −0,87, Ø −0,10, ohne besten Tag −0,28), 4/10 der Obergrenze → **FREEZE-ENDE nein.**
- Der 30.09. zählt **voll** (100 %, kein Teiltag).
- Kombi gesamt: n 6 (4 unabhängig), Σ gewichtet **+0,74 R** (29.09.: +0,34).
- Prognose: Bei 2,25 AUTO-Bewegungen je Tag fehlen 11, also rund 5 Tage. Tag 10 der Obergrenze fällt bei lückenloser Zählung auf **Do 08.10.** Das wird sehr knapp; „AUTO zu selten, nicht belegt“ ist ein realistischer Ausgang.

## (D) P3: Der 30.09. ist der erste Tag mit echten Pflichtfällen

**Zur Frage „19:51 der erste impuls-fällige Fall?“: Nein.**
- Von 19:51 bis 20:01 meldete X1 **nur den Distanz-Auslöser** (Impuls-Delta 1,86–1,96×ATR < 2×). Per Definition löst das keine Pflicht aus. Der Anker 30509,85 durfte also bleiben.
- Die echten Fälle lagen früher am Tag:

| Fall | fällig (X1) | Pflicht (Rücklauf) | Ablauf | Ergebnis |
|---|---|---|---|---|
| 1 | 15:51 (Distanz), ab 16:01 „beide“ | 16:11 | 16:01/16:06 „noch nicht einschlägig“ (am Extrem); **16:11:18 Hard-Exit** → 16:11:37 Begründung (kein bestätigtes HL nach Extrem 30620,35, **stimmt**); 16:41 zweite Begründung (HL 30509,85 unbestätigt, **stimmt**: das Fraktal-HL 30531,85 von 16:25 war um 16:30 schon gebrochen) | **Reset 16:46:16 → 30509,85** (Δ 110,45), genau im ersten VC nach der Bestätigung (16:45). Anschließend ALT +1 R. |
| 2 | 17:36 (Distanz) | 18:11 („beide“ seit ATR-Rückgang) | **18:11:34 Hard-Exit** | **Reset 18:12:04 → 30576,75** (Δ 66,9), im selben VC |

**Zählspalte 30.09.:**
- impuls-fällige Fälle **2**
- Resets gesamt **2**; dazu 2 Wechsel *gegen* die Richtung (18:31 auf 30556,25, 19:16 auf 30509,85, jeweils nach Bruch des Ankers)
- davon Kleinstschritt **0**
- P3-Hard-Exits **2** (3,6 % der Slots)

**Vorab-Kriterien (Rückbau, wenn eines verfehlt wird):**
- Kriterium 1 (Anker hinkt in ≤20 % der Momente >90 Min hinter AUTO): **0/58 = 0 %, erfüllt.** Die längste Abweichung waren 81 Min (16:25–17:46), mit ALT tiefer als AUTO.
- Kriterium 2: **erfüllt** (1× zweimal begründet abgelehnt, dann Reset bei Bestätigung; 1× Reset im selben VC).
- Hard-Exit-Quote ≤15 %: **erfüllt.**
- Ehrliche Einordnung: P3 hat hier genau das bewirkt, wofür es gebaut wurde. Beim 28.09.-Muster („X1 30× gemeldet, nie gesetzt“) wäre der Anker bei 30399,4 hängen geblieben und ALT hätte nach 16:01 kein offenes Fenster mehr gehabt.

**Operator-Abweichung (Geldwirkung 0):**
- Die Kerze 19:25 machte den Docht 30492,35 unter dem Anker 30509,85. Nach 7b1 3a („tiefstes LOW seit Impulshoch, inkl. Dochte“) wäre ab 19:31 30492,35 der Anker gewesen. Sonnet schrieb „Docht zurückgenommen, Anker bleibt“.
- Mit dem korrekten Anker hätte 19:42 SL 30478,15 (2,63×ATR) gehabt, also ebenfalls FAIL oder bestenfalls PASS-ROT. Ergebnis gleich.
- Dazu eine Definitionslücke: Unterschreitet der Kurs alle Tiefs seit dem Extrem (18:31), liefert 3a keinen gültigen Long-Anker mehr. Sonnet griff dann auf ältere Tiefs zurück, was vernünftig ist. **Gehört ins Freeze-Review** (Anker-Nachführung), jetzt nicht ändern.

**P3-Urteil gemäß Verlängerungsregel (Levi 30.09.):**
- Stand: 29.09. = Tag 1/3 (0 Fälle), **30.09. = Tag 2/3 mit 2 impuls-fälligen Fällen.**
- Die Bedingung „≥1 Fall im Zeitraum“ ist damit **erfüllt**. Eine Verlängerung ist nicht nötig, das 3-Tage-Urteil fällt nach dem **nächsten** Testtag.
- Zwischenstand: alle Rückbau-Kriterien eingehalten, Tendenz **`pflicht` behalten**.

## (E) Q3-auto / Auftrag B / Tagesende-Regel

**Q3-auto:**
- Auftrag B ist **nicht umgesetzt** (kein Commit nach 07b4ccb, in gate_check keine Q3-AUTO-Zeile). Die Prüfung erfolgt deshalb rückwirkend aus dem Shadow-Log.
- 15:32 (Shadow VC#2 15:31:51) und 19:42 (VC#52 19:41:29): 5m/15m/QQQ/1H alle long, `--q3-coherence yes`.
- **0 Widersprüche, 0 UNBEKANNT → Tag 2/3** (wieder der triviale Fall, alle Beine gleichgerichtet).

**Tagesende-Regel (A1, seit 30.09. aktiv):**
- Beide skipped-Nachträge und der Kombi-Nachtrag liefen mit dem Default `bis_quelle "Loop-Ende 20:00 DE"`, ohne manuelles `--bis`.
- 1 PASS (ROT) ausgelassen, **mit** `auslass_grund`. protokoll_bilanz: „PASS-Freigaben ohne Auslass-Grund: 0“ → **Tag 2/3.**
- Einschränkung: Der eigentliche Prüffall (GELB/GRÜN ausgelassen) kam wieder nicht vor.

**Abdeckungsanzeige (A2):** zeigt korrekt „Abdeckung 100 % (54/54)“, Freeze-Zeile unverändert. Erledigt.

**Archiv:** Das Tagesprotokoll wurde manuell zusammengesetzt, diesmal **mit** Hard-Exit-Blöcken (#55 Hard-Exit, _1b/_2b/_10b/_34b). Die vom 29.09. verlangte Vorlage ist erfüllt. Die automatische Erzeugung bleibt Teil von Auftrag B.

## (F) 1H-Override und IBF-Schatten

- **1H-Override:** Der letzte geschlossene 1H-Bar lag den ganzen Tag über der EMA50 (+86 bis +197 Pkt). **0 Blockaden, 0 Episoden.** Bestand unverändert: 10 Episoden (9 bewertbar); das Review-Kriterium ≥20 ist weit weg. Heute hat der Override nichts gekostet und nichts gespart.
- **IBF** (`ibf_schatten.cjs` auf der Kopie, schreibt nichts, Hash bestätigt):
  - Kriterium 1: n = **1** (29.09. 17:51 +1, Lesart A; streng gelesen 0)
  - Kriterium 2 (n ≥10): nicht erfüllt
  - Kriterium 3: nicht prüfbar
  - Kriterium 4: NEUTRAL (Δ 0)
  - Kriterium 5: n 0, kein negatives Ergebnis
  - Der 30.09. liefert Nullzeilen. **Urteil unverändert: „zu selten, nicht belegt“.** Die Prognose vom 30.09. bestätigt sich: Ohne Blockaden gibt es keine IBF-Daten, an Trendtagen mit 1H in Richtung erst recht nicht.

## (G) Empfehlung

**Freeze-Zählung:** Der 30.09. zählt **voll** → **4/5 Testtage, AUTO 9/20, Obergrenze 4/10.** Nach dem nächsten bewertbaren Tag sind es formal 5/5. Das Freeze-Ende hängt dann allein an AUTO ≥20 oder der Obergrenze (spätestens ~08.10.).

**Vorschläge (alle freeze-konform, nur Vorschlag, Levi entscheidet):**

| Prio | Vorschlag | Art | Begründung |
|---|---|---|---|
| **1** | **Auftrag B jetzt beauftragen, vor dem nächsten Testtag**, inkl. **B5 Session-Extrema ins Register** (Prozesstext: jedes neue US-Session-Hoch/-Tief sofort per `register_touch.cjs --session-hoch/-tief`, und ein V11-„nur geprüft“-Touch nur dann, wenn das Session-Extrem dem Chart entspricht) | Anzeige/Prozess | Heute **nachweislich** wirksam: Das fehlende 30635,25 machte aus PASS-ROT einen FAIL. Es ist das dritte Testtag in Folge mit dieser Lücke. |
| **2** | **Tweet-Artefakt in vollcheck.cjs** als B6: Fälligkeit bei `--tweet-fetch ja` gegen den Slot von `last_poll` prüfen, nicht gegen `--jetzt` | messverfälschender Bug (im Freeze erlaubt) | 23/23 „Über-Polling“ sind falsch. Die Format-/Lückenstatistik ist sonst wertlos, und Sonnet gewöhnt sich an rote ✗. |
| 3 | Loop-Start-Template: `--stale-n` (oder `--grund-stale-n`) fest in den ersten VC-Aufruf; im Live-Gate-Template `--chasing` + `--grund-chasing` immer paarweise | Operator-Disziplin | 3 Stale-Hard-Exits in 2 Tagen, 2 Chasing-Abbrüche am 19:42-Trigger (Sekunden im Trigger-Moment) |
| 4 | Archiv: im LIVE-GATE-Block eine Zeile `Exit-Code: <n>` je Lauf (bzw. automatisch über B) | Format | protokoll_bilanz liest sonst „Exit-Code ?“ |
| 5 | Ins Freeze-Review, **jetzt nicht ändern:** (i) 3a-Lücke „alle Tiefs seit Extrem gebrochen“; (ii) AUTO lässt gebrochene Swings wieder gelten; (iii) Trigger-Lücke an 2/2-Trendtagen (4 h ohne Gate-Lauf, Kombi-Schatten wächst zu langsam) | Backlog | Jede Änderung würde die laufende Messung mitten im Freeze umdefinieren |

**Mengenbremse:** Für den nächsten Testtag als die eine Änderung nur **B (+B6)**. Gegencheck durch einen frischen Opus, Commit/Push nur auf Levis Befehl.

---

## Auflösung der 7 offenen Punkte aus dem Abschluss-Memory

1. **„13:32-Setup: skipped ‚ausgelassen‘ vs. Kombi ‚Einstieg‘“ → kein Widerspruch, aufgelöst.**
   - „13:32“ ist **UTC** (= 15:32 DE). Derselbe Gate-Lauf schreibt designgemäß zwei Einträge: skipped_fiktiv (live geltende Regel, Option D: ausgelassen, volle Größe, +1,42 R) und kombi_fiktiv (V2-Schatten, 0,25, +0,40 R).
   - Beide sind über `skipped_id` verknüpft. `kombi_fiktiv --auswertung` zählt skipped ausdrücklich nicht mit (B-7, keine Doppelbuchung). Sonnets Nachtrag inkl. Auslassgrund ist korrekt.

2. **„19:45 Schluss über Register-Session-Hoch 30584,85 = (b)-Trigger?“ → formal ja, materiell nein, Geldwirkung 0.**
   - Nach 7b1 ist (b) „benanntes Level (Session-Extrem) bricht per Kerzenschluss“. Gemessen am Register war es ein (b)-Trigger-Moment, und die Pflichtzeilen (10)/(11)/(11a) fehlen.
   - Das Level war aber falsch: Das echte Session-Hoch 30635,25 wurde nicht gebrochen (Hoch bis 20:00 30594,25).
   - Ein Gate-Lauf um 19:51 wäre aussichtslos gewesen: X2-Fenster **LEER**, SL 89,65 = 3,15×ATR.
   - **Opus:** als Registerfehler werten, nicht als verpassten Trigger.

3. **Anker 30509,85 / X1 ab 19:51:** siehe D. Nur Distanz-Auslöser, also keine Pflicht. Nicht der erste P3-Fall (das waren 16:11 und 18:11). Zusätzlich: Der Docht 19:25 (30492,35) hätte nach 3a den Anker verschieben müssen, Wirkung 0.

4. **Handeingaben** (`--punkt11-signal`, ATR-D Stand 18:40, VIX, Makro-Text, `--zyklus-8a5 0` vs. maschinell 1, Label HH-HL/Range):
   - Alle sind **Anzeige-/Schattenfelder ohne Gate-Wirkung.** Der Regime-Check 8d blieb in jeder Variante „trend“ (1/3).
   - Das 1H-Label wechselt ab VC#51 auf „Range“ (6×, vorher 52× HH-HL). Plausibel nach dem Rutsch 19:15–19:25. Das Override hängt nur an der EMA-Seite.
   - `--zyklus-8a5 0` vs. maschinell 1: Die Pflichtzeile kommt erst ab dem 3. Zyklus, also folgenlos.
   - **Offen lassen**, nur bei einem Gate-relevanten Feld nachfassen.

5. **Register-Touch 19:56 „geprüft“ → aufgelöst, Befund.**
   - Es war ein reiner Zeitstempel-Touch (V11). Die Notiz „Session-Hoch 30584.85 wird getestet“ ist **inhaltlich falsch** (echt: 30635,25).
   - Damit wird die Frische-Regel formal erfüllt, ohne den Inhalt zu prüfen. Inhalt zuletzt geändert um 17:51, beim Session-Hoch um 16:01.
   - **Opus:** Prio 1 in G.

6. **Tweet-Artefakt / Archiv / 19:42-Hard-Exits → aufgelöst.**
   - Tweet: Artefakt bestätigt, 0 echtes Über-Polling, Fix siehe G2.
   - Archiv ohne Exit-Code-Zeile: G4.
   - 19:42: zwei vergessene Flags, Abbruch je 6–8 s, der dritte Lauf gültig. Kein Einfluss aufs Ergebnis (FAIL so oder so). Template siehe G3.

7. **Marktlage Loop-Ende → teilweise korrigiert.**
   - NAS100 bewegte sich 19:45–20:00 in 30577,85–30594,25 über der 5m-EMA50: belegt.
   - **„TP1-Fenster ab 19:56 nicht mehr leer“ ist falsch.** VC#55b um 19:56 meldet **LEER** (SL 3,1×ATR, max. RR 0,967). Offen war das Fenster erst in **VC#56 um 20:01**: [30647,2 ; 30653,05], 5,8 Pkt breit, 30650 mit RR 1,037. Das liegt nach dem Testtag-Ende und ist bedeutungslos.
   - „10J-Rendite >5,30 %, höchster Stand seit April 2002“ stammt aus einem Kobeissi-Tweet (VC#56) und ist **nicht unabhängig belegt.**
   - Was nach 20:00 kam: **nicht prüfbar** (TradingView aus).

## Entscheidungen für Levi (je mit Opus-Empfehlung)

- **(a) P3-Zählung:** 30.09. = Tag 2/3 mit 2 impuls-fälligen Fällen, keine Verlängerung nötig, Urteil nach dem nächsten Testtag. → **Empfehlung: bestätigen.**
- **(b) Auftrag B + B6 (Tweet-Fix) vor dem nächsten Testtag beauftragen**, mit B5 Session-Extrema als Pflicht-Prozesstext. → **Empfehlung: ja.** Es ist die eine Änderung des nächsten Testtags.
- **(c) (b)-Trigger bei laufendem 2/2:** Pflicht zum Gate-Lauf oder nur Pflichtzeilen? → **Empfehlung: im Freeze nichts ändern**, Thema ins Freeze-Review (Trigger-Lücke, G5-iii). Heute ohne Geldwirkung.
- **(d) Q-ROT:** 2 Tage in Folge hätte das Auslassen Geld gekostet (+1,42 R heute, +0,84 R bis 20:00 am 29.09.). → **Empfehlung: nicht reagieren** (Berichtsregel der Kombi-Auswertung: keine Regeländerung auf einzelne fiktive Ergebnisse). Kombi n=6 bleibt die Messung dafür.
