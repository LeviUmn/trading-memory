---
name: opus_gegencheck_kern_regelwerk_2026-10-14
description: "Opus-Gegencheck 14.10.2026 (13:38-13:57 DE) zu Fables Kern-Umsetzung (K0-K9): MIT AUFLAGEN freigegeben, NICHT startreif. Rechenkern stimmt: 7 Fixtures nachgerechnet, 8 Crosses und Zeitstrahl 13.10. exakt, DST ok, 290/290 Tests, keine Alt-Datei veraendert, kein Echtgeld-Pfad, dry-run-Schreibbefund bestaetigt. 4 Start-Blocker: A1 ON_HOCH/ON_TIEF = US-Session-Extrema, A2 Bilanz (b) falsch NEIN bei Sperre/8c2/Re-Entry, A3 Exit erst nach dem Check erkannt, A4 keine Bid/Ask-Quelle. Abweichung 2 (19:31) bestaetigt. Replay 8 Tage: n 9, Oe -0,47 R."
metadata:
  type: project
  originSessionId: opus-gegencheck-kern-2026-10-14
  modified: 2026-10-14T15:44:39.714Z
---

# Opus-Gegencheck Kern-Regelwerk, 14.10.2026

**Urteil: MIT AUFLAGEN. Der erste Kern-Fiktivtag darf erst starten, wenn A1 bis A4 erledigt sind** (A1-A3 sind Code-Korrekturen von zusammen etwa 30-60 Zeilen, A4 ist eine Entscheidung von Levi). Danach braucht es einen Opus-Kurzcheck nur für diese Stellen.

**Zeitstempel:** Start 13:38:03 DE, Ende 13:57 DE.

**Hygiene:**
- sha1 von 300 Dateien (`scripts/`, `tests/`, `package.json`) wurde um 13:38 und um 13:54 genommen: **identisch**. Dasselbe gilt für alle 225 Memory-Dateien.
- Gerechnet wurde nur auf Kopien in `%TEMP%\...\scratchpad\{repo,rt,sp,dr}`.
- Git habe ich nur für `git diff` und `git status` benutzt. Kein test:e2e, kein CDP.
- Ich habe nichts geändert außer dieser Datei. MEMORY.md ist nicht angefasst.

---

## Ergebnistabelle Punkte 1-10

| # | Prüfpunkt | Ergebnis | Beleg |
|---|---|---|---|
| 1 | K0-Hashes | **erfüllt** | `sha1sum -c _freeze_alt_2026-10-14.sha1`: 79 OK, nur `package.json` und `vollcheck.cjs` FAILED. Laut mtime wurde unter `scripts/` heute nichts außer kern_* und diesen beiden Dateien verändert. Lücke: siehe A9 |
| 2 | DST | **erfüllt** | `kern_zeit.fenster`: 13.10. 15:30/15:35-19:00; 26./27./30.10. Divergenz 14:30/14:35-19:00; 02.11. normal; März 09.-27.03. Divergenz. Der `vollcheck.cjs`-Diff hat 9 Zeilen. An den Grenzen 15:00/15:29/15:30/15:59/16:00/16:04/16:05 ist das Verhalten auf Normaltagen identisch, am 27.10. eine Stunde früher. Konflikt zur Endurteil-Regel 4: siehe A5 |
| 3 | 12 Regeln im Code | **10 von 12 exakt, 2 mit Befund** | Regel 3: A1. Regel 4 im DST-Fenster: A5. Details unten |
| 4 | 7 K2-Fixtures (eigener Code) | **erfüllt** | +0,491 / +0,961 / +1,50 / −1,00 / −1,00 / −0,128 (Erwartung −0,11, Δ 0,018). Opus' Erwartung hatte beim Short-Zeit-Exit keinen Spread, Fables Zahl ist richtig. F7 ist Cap-FAIL: 5,71× (ATR nach der 15:30-Kerze) bzw. 7,20× (ATR davor) |
| 5 | Trigger und Zeitstrahl 13.10. | **erfüllt, exakt** | Eigene EMA-Berechnung ergibt 15:35↑ 16:50↓ 17:10↑ 17:15↓ 18:00↑ 19:15↓ 19:35↑ 19:45↓. Richtungswechsel 15:45/16:00/16:15/17:00/18:15/18:30/19:00/19:30/19:45/20:00. Der 1H ist den ganzen Tag bearisch |
| 6 | Replay 05./08./13.10. | **läuft durch, Zahlen weichen ab (erklärt)** | 05.10.: 0 Trades. 08.10.: 2 Trades (TP1+BE +0,5; TP1+TP2 +1,5). 13.10.: 1 Trade (ZEIT −0,15). Σ +1,85 R bei n 3. Details unten |
| 7 | dry-run schreibt (beide Richtungen) | **Fables Befund bestätigt, Isolation wirkt** | Direkter Aufruf `gate_check.cjs --dry-run` in einer Kopie, Status UNKNOWN: nur `trigger_kandidaten_log.jsonl` ändert sich (gate_check.cjs Z. 5213-5219). Drei kern_gate-Läufe mit Schatten im Mirror: alle 188 Alt-Dateien bleiben identisch, Temp-Verzeichnisse sind entfernt |
| 8 | Argumente, Prompt, Echtgeld | **erfüllt** | Gate ≤ 8 (zählt Schlüssel, 9 ergibt Exit 1). Check im Prompt ≤ 4, aber ohne Code-Grenze (siehe A12). Prompt hat 59 Zeilen, ist eingebettet, ohne Datei-Zeiger. `TESTTAG_MODI=['fiktiv']`, `--testtag echt` ergibt Exit 1. Kein Order- oder Broker-Code, kein automatischer Übergang. Der Bilanz-Text lautet "Levi entscheidet frei" |
| 9 | Tests | **erfüllt** | 290/290 grün (kern 44). `trading_scripts.test.js` und `tagesmomente.test.js` sind sha-gleich zum Freeze, also 246 alte Tests unverändert |
| 10 | Rollback-Probe | **erfüllt bis auf eine Stempelzeile** | `loop_prompt.cjs` läuft (Exit 0, 487 Zeilen). Gegen eine rekonstruierte Vorher-Fassung von `feedback_live_trading.md` (ohne Kopfvermerk) unterscheidet sich genau eine Zeile: "Stand 2026-10-14 13:32" statt "2026-10-01 11:45" (mtime-Stempel des Memos). Alle injizierten Bausteine sind byte-gleich. Siehe A10 |

---

## Auflagen (nach Schweregrad)

### HOCH (Start-Blocker)

**A1. Regel 3 verletzt: ON_HOCH und ON_TIEF sind US-Session-Extrema, keine Overnight-Level.**
- **Befund:** Die Zuordnung behandelt `Session-Hoch`/`Session-Tief` als Overnight-Level und nimmt den ersten Treffer in Register-Reihenfolge. Im Register sind diese Typen aber die US-Session-Extrema, die `register_touch.cjs` im alten Voll-Check nachgezogen hat. Laut Register-Text sind sie ausdrücklich "vor B5-Umstellung auf US-Session-Extrema". Die echten Overnight-Level heißen dort `Nacht-Hoch Pre-Market 08.10.` und `Nacht-Tief Pre-Market 08.10.` und treffen wegen des Exakt-Vergleichs nie.
- **Folgen:**
  - Am 05.10. ist ON_TIEF = 30794,35 = das Tief der 15:30-Kerze desselben Tages. Im Replay mit dem Tagesend-Register ist das sogar ein Lookahead.
  - Die K3-Akzeptanz "7 Felder" ist formal erfüllt, inhaltlich falsch. Der Auftrag schließt US-Session-Extrema ausdrücklich aus.
  - Level-Cross-Trigger feuern damit an falschen Leveln.
- **Beleg:** `kern_config.cjs:25`, `kern_start.cjs:33`, `kern_alt_schatten.cjs:33`; Register `level_register.json.bis2026-10-13.bak` (Einträge Session-Hoch/-Tief vs. Nacht-Hoch/-Tief).
- **Vorschlag:** ON_HOCH/ON_TIEF deterministisch in `kern_start` aus den 5m-Bars berechnen (Fenster Vortag 22:00 bis 15:30 DE, im DST-Fenster entsprechend), nie aus `Session-*`. Alternativ nur Typen mit Präfix `Nacht-`/`Overnight-` akzeptieren. Test mit dem 13.10.-Register.

**A2. Bilanz-Kriterium (b) meldet bei regelkonformem Verlauf NEIN (Sperrfrist, 8c2, Re-Entry).**
- **Befund:**
  - Bei einem Gate-FAIL (Sperre, 8c2, Re-Entry) schreibt `kern_check` "kein Trade: …". Der Prompt ruft `kern_gate` nur bei "GATE JETZT" auf.
  - `kern_bilanz` modelliert Sperre, 8c2 und Re-Entry nicht und erwartet für diese Trigger trotzdem einen Gate-Lauf.
  - Fables Kommentar "das Gate hat ihn dann als FAIL sperre protokolliert" trifft deshalb nicht zu.
- **Nachgewiesen:** Replay 08.10. mit `--event 17:40:CPI`. Um 17:35 meldet der Check "kein Trade: sperre", die Bilanz dann "(b) NEIN — … ohne Gate-Lauf 1 (17:30)".
- **Folge:** Jeder Sperrfrist-Tag und jeder 8c2- oder Re-Entry-Fall verdirbt einen sonst sauberen Testtag (Technik "NICHT erfüllt").
- **Beleg:** `kern_check.cjs:73-77`, `kern_loop_prompt.cjs:31-37`, `kern_bilanz.cjs:60-76` (Kommentar Z. 14-15).
- **Vorschlag:** Für (b) einen `kern_check`-Logeintrag mit `trigger` und `gates_vorschau.status=FAIL` im selben Slot als "verarbeitet" werten. Alternativ in der Bilanz dieselben `R.gates()` anwenden. Test mit Sperre und 8c2.

**A3. Exits werden erst nach dem Voll-Check erkannt, dadurch geht ein Trigger am Schluss der Exit-Kerze verloren.**
- **Befund:**
  - `kern_position_tick` erkennt SL/TP/BE nur aus den gespeicherten 5m-Bars. `--kurs` dient nur der Anzeige, und `nas100_1m.json` wird im Loop nicht gespeichert.
  - Die Bars werden nur zur 5-Min-Marke gespeichert. Der Prompt lässt den Tick in Check-Minuten **nach** dem Check laufen.
  - Folge: Ein Exit in Kerze X wird erst in der Minute X+5 erkannt, also nachdem `kern_check` mit "Position offen" schon jede Trigger-Prüfung übersprungen hat.
- **Folgen:**
  - Ein Trigger am Schluss der Exit-Kerze ist nach Regel 9 zulässig ("belegt bis Ende der Exit-Kerze"), wird aber nie gegatet. Die Bilanz zählt ihn als zulässig, (b) wird NEIN.
  - Im "Echtzeit-Replay" (Snapshot nur zur 5-Min-Marke, laufende Kerze O=H=L=C=Open) sieht man den Versatz: 08.10. TP1 erst 18:05 (Kerze 18:00), BE erst 18:20 (Kerze 18:15); 24.09. SL erst 18:20 (Kerze 18:15). In den 8 Replay-Tagen fiel zufällig kein Trigger auf diesen Schluss.
- **Beleg:** `kern_loop_prompt.cjs:39-40` ("auch in Voll-Check-Minuten nach dem Check"), `kern_check.cjs:72`, `kern_position_tick.cjs:41-51`.
- **Vorschlag:** (a) Im Prompt in Check-Minuten zuerst den Tick, dann den Check aufrufen. (b) Oder `kern_check` rechnet die Position selbst per `simuliere` und gibt sie frei. Zusätzlich sollte der Tick mit `--kurs` gegen SL/TP prüfen oder `nas100_1m.json` je Minute gespeichert werden (Live-Exit ohne 5-Min-Verzug).

**A4. Für Kriterium (e) gibt es keine Bid/Ask-Quelle. Bei jedem Gate-Tag droht (e) = NEIN oder ein erfundener Spread.**
- **Befund:**
  - `PRODUKT` ist leer. Der Prompt sagt "Bid/Ask ablesen … fehlt es: --grund-spread".
  - Die Bilanz wertet (e) nur bei numerischem `spread_pkt` als ja. Ein `--grund-spread`-Gate zählt als fehlend.
  - Der Loop-Agent hat keinen Zugang zu Scalable-Kursen. Es bleibt also (e) = NEIN an jedem Tag mit Gate, oder der Agent setzt eine erfundene Zahl in `--spread-pkt`. Das wäre schlimmer, weil es Daten fälscht.
- **Beleg:** `kern_config.cjs:42`, `kern_loop_prompt.cjs:35-37`, `kern_bilanz.cjs:85-86`.
- **Vorschlag (Levi entscheidet vor Tag 1, L3):**
  - Entweder liest Levi selbst Bid/Ask je Gate ab (Anwesenheit nötig),
  - oder ein fester, vorab dokumentierter Referenz-Spread gilt als Protokoll (`--spread-pkt` mit Quelle "Referenz Levi <Datum>"). (e) zählt dann als "Referenz", und die Messung des echten Spreads gehört ins Echtgeld,
  - oder (e) wird für den Fiktivtest ausdrücklich umdefiniert.
  - In jedem Fall einen Satz in den Prompt: "keine Spread-Zahl schätzen".

### MITTEL

**A5. Konflikt Endurteil-Regel 4 vs. Auftrag K1 im DST-Fenster.**
- Das Endurteil sagt "im DST-Divergenzfenster beide Zeiten eine Stunde früher", also 14:35-18:00. Der Auftrag (mein Entwurf) und der Code verwenden 14:35-19:00 bei Zeit-Exit 20:00.
- Fable hat korrekt nach Auftrag gebaut.
- 26.-30.10. liegt wahrscheinlich in den Testtagen 8-10.
- **Beleg:** `kern_zeit.cjs:29-30`.
- **Vorschlag:** Levi bestätigt vor dem 26.10. eine Fassung. Meine Empfehlung ist 14:35-19:00, weil Zeit-Exit 20:00 fest bleibt und der Runway dann so lang ist wie an Normaltagen. Das Endurteil wird dann im Wortlaut nachgezogen.

**A6. `kern_start` prüft die Frische des Registers nicht.**
- Ist der Start-Update nicht gelaufen, friert der Kern stillschweigend Level eines anderen Tages ein. Der 13.10.-Replay lief so mit Leveln vom 08.10.: `updated 2026-10-08`, und es wurde nichts gemeldet.
- **Beleg:** `kern_start.cjs:91-96`.
- **Vorschlag:** Exit 1, wenn PDH/PDL/Pivots nicht zur letzten abgeschlossenen Session passen oder `updated` nicht vom heutigen DE-Datum ist. Ausnahme `--register` (Tests).

**A7. Spread-Eingaben ≤ 0 werden akzeptiert.**
- `--spread-pkt -5` oder bid > ask liefert negativen Spread. Folgen: Schatten "wäre PASS", BE auf der falschen Seite, R zu gut gerechnet (Probe: TP1+BE mit −5 ergibt +0,75 R).
- **Beleg:** `kern_common.cjs:55-59`, `kern_gate.cjs:82-87`, `kern_regeln.cjs:301-304`.
- **Vorschlag:** Spread < 0 ergibt Exit 1. Bei 0 einen Hinweis ausgeben.

**A8. Prompt-Unklarheit beim Zeit-Exit.**
- Der Prompt sagt "Zeit-Exit fällig: Rest zum Kurs schließen (fiktiv)", nennt aber keinen Befehl. Tatsächlich schließt der Tick ab 20:00 automatisch.
- Risiko: Der Agent greift zu `--notfall`. Das ergibt Klasse NOTFALL, "manuell" wird > 0, und der Anteil manueller Exits (Soll 0) stimmt nicht mehr.
- **Beleg:** `kern_loop_prompt.cjs:42`.
- **Vorschlag:** "Nichts aufrufen, der Tick um 20:00 schließt automatisch (Zeit-Exit ausgeführt). --notfall nie für den Zeit-Exit."

### NIEDRIG

**A9. Die K0-Freeze-Liste deckt nicht alles ab.**
- Nicht enthalten sind `trades.db`, datierte Bars, `*.bak`, `last_*`, `setup_karte.json`, `loop_archiv/`.
- Heute ist laut mtime keine dieser Dateien verändert.
- **Vorschlag:** Für den Abgleich nach Tag 1 eine erweiterte Liste anlegen.

**A10. Rollback-Stempel und neue Abhängigkeit.**
- (a) Die Ausgabe von `loop_prompt.cjs` hängt an der mtime von `feedback_live_trading.md`. Der Kopfvermerk ändert eine Zeile, nicht den Inhalt.
- (b) `vollcheck.cjs:753` braucht jetzt `kern_zeit.cjs`. Ein Rollback darf `kern_zeit.cjs` nicht löschen.
- **Vorschlag:** Beides in `regelwerk_kern_2026-10.md` unter "Rollback" vermerken.

**A11. Simulation füllt bei Gap am SL-Preis.**
- Öffnet eine Kerze jenseits des SL, wird trotzdem zum SL-Preis gefüllt (Probe: Open 85, SL 90 ergibt −1,00 statt −1,5 R). Das ist leicht optimistisch.
- Außerdem wird BE in der TP1-Kerze nicht geprüft.
- **Beleg:** `kern_regeln.cjs:221-226`.
- **Vorschlag:** Für SL/BE gilt Fill = schlechterer Wert aus Open und Marke. Als Regel-Freeze-konforme Fehlerkorrektur dokumentieren.

**A12. Weitere Kleinpunkte.**
- `kern_check` hat keine 4-Argument-Grenze im Code.
- `kern_nachtrag` rechnet R bei korrigiertem Entry neu, prüft aber Cap und Floor nicht nach (`kern_nachtrag.cjs:28`).
- Die Nachtrag-Klasse beruht auf Spread 2, der Live-State auf dem gemessenen Spread. Bei großem Spread können die Klassen auseinanderlaufen.
- Doppelte Bars mit gleicher `time` werden nicht entfernt (`atr_qqq.loadBars`).
- Liegt ein 15m-Schluss-Check zu spät, geht die Gate-Bildung ohne Nachholen verloren (bewusst so, (b) misst es).

**A13. Der Schatten "alt hätte" hat wenig Aussagekraft.**
- In den Replays ergab er 1× "nicht bewertbar" (kein Level in Richtung) und 2× UNKNOWN (`tp2RealismGate`, Q 1/4 UNBEKANNT). PASS/FAIL kommt praktisch nur bei passenden Registerleveln heraus.
- Endurteil-Testplan Punkt 4, zweiter Teil (verpasste Alt-Gates zählen), fehlt. Fable meldet das ehrlich: Es war nicht im Auftrag, also kein Fehler Fables.
- **Vorschlag:** Levi entscheidet, ob diese Messung entfällt oder bis zur Zwischenanalyse nachgerüstet wird. Ohne sie gibt es keine Aussage "Kern vs. Alt".

**A14. Regel 8 und `size.cjs`.**
- Das temporäre Positions-Limit von 2.500 € in `size.cjs` bindet. Probe 08.10.: 453 Stück, Ist-Risiko 23 € statt 75 €.
- Im Fiktivtest spielt das keine Rolle (nur ausgewiesen, R-Messung).
- **Vor einem Echtgeld-GO zu klären.**

---

## Regel für Regel (Punkt 3)

1. **Richtung** (`kern_regeln.cjs:66-79`, EMA aus `atr_qqq.computeEma`): exakt. "gleich" ergibt keine Richtung, 1H-"gleich" gilt als "nicht unter". Die EMA-Quelle sind die Bars mit Prüfzeile (L2). Das weicht vom Endurteil-Wortlaut "Chartwerte" ab, ist aber per Auftrag so entschieden.
2. **Trigger** (`kern_regeln.cjs:83-120`, `kern_lage.cjs:25-27`):
   - Cross gilt mit Vorkerze ≤/≥ (gleich zählt als Gegenseite). Es gibt höchstens einen Trigger je Schluss, Priorität EMA vor Level vor Bildung.
   - **Abweichung 2 (19:31 statt 19:36): BESTÄTIGT.** Mein Endurteil-Wortlaut zu Regel 2b lautet "der 15m-Schluss, mit dem die Richtung 2/2 wird". Der Trigger ist also das Ereignis am 15m-Schluss 19:30, die Entry-Kerze ist 19:30.
   - Ein Check um 19:36 hat die 19:30-5m-Kerze hinter sich. Würde 2b dort erneut feuern, wäre das genau die Zustands-Logik, die der Cross-Trigger abschaffen sollte (bis zu drei Trigger je Bildung).
   - Die "19:36" in meinem Auftrag stammte aus dem alten Lauf (VC 19:36) und war mein Fehler.
   - Die Negativprobe 19:36 ("Trigger: nein") ist richtig. Ergebnis 19:31: Entry-Kerze 19:30 > 19:00, also FAIL sperre-zeit.
3. **Level-Liste:** **Befund A1.** Keine Rundzahlen oder Fib (richtig). Keine Nachträge im Loop (richtig).
4. **Entry** (`kern_gate.cjs:100-102`, `kern_regeln.gates` zeit inklusive Grenzen): Ein Trigger am 15:30-Schluss ergibt FAIL zeit (05.10. 15:30 belegt). Ein Trigger am 18:55-Schluss ergibt Entry 19:00, PASS (24.09. 19:00 belegt). DST: A5.
5. **SL** (`kern_regeln.cjs:125-143`): Der Swing ist nicht strikt (≤/≥), i+1 ist geschlossen, low < Entry. Puffer 0,5 ATR, Floor 1,5 ATR, ohne Swing gilt der Floor. Exakt.
6. **Kein Trade** (`kern_regeln.cjs:149-162`):
   - Cap 3,0 ist exakt.
   - Spread im Fiktivbetrieb nur als Schatten (`SPREAD_GATE_HART false`), ohne CLI-Schalter. Exakt.
   - 8c2 nutzt die Kern-SL-Liste. Der Zusatz "per Docht" ist vereinfacht, laut Auftrag zulässig. 10-06 19:50 greift 8c2 korrekt.
   - Ergänzend A7.
7. **Ausgänge** (`kern_regeln.cjs:200-249`, `kern_position_tick.cjs`):
   - SL wird vor TP in derselben Kerze geprüft. TP2 frühestens ab der Folgekerze. Zeit-Exit zum Schluss der 19:55-Kerze, Tick "fällig" ab 19:58 und ausgeführt ab 20:00.
   - BE ± Spread realisiert bei Long und Short 0. Das ist im Bid-Chart-Modell die richtige Lesart von N1.
   - Zum Merken für Echtgeld: Die Short-BE-Order auf der Ask-Seite liegt am Entry selbst.
   - Klassen und `--notfall` exakt. Ergänzend A8 und A11.
8. **Größe:** nur ausgewiesen. `size.cjs` mit bid/ask/hebel, sonst EUR/Pkt-Formel. **Fables Aussage stimmt** (`kern_gate.cjs:30-41`, 119). Ergänzend A14.
9. **Tageslimits:**
   - 1 Position, belegt bis Ende der Exit-Kerze (`gates` position; Bilanz `belegtBis`). Max. 3 Trades. 2 Verluste (≤ −0,5 R) oder −2 R stoppen (07.10. 18:15 korrekt "Stopp nach 2 Verlusten").
   - Re-Entry nur mit neuem Trigger UND neuem Anker. Das ist korrekt als ODER-Sperre umgesetzt (07.10. 16:30 neuer Anker, zulässig).
   - Ergänzend A3.
10. **Sperrfristen und Schock:**
    - Event −15/+30 ist exakt (17:40 ergibt 17:25-18:10).
    - Schock = ATR nach Schluss der Session-Start-Kerze ≥ 2× Median der letzten 10 datierten Tage, bei < 5 Tagen "nicht bestimmbar". Im Divergenzfenster gilt die 14:30-Kerze, konsistent.
    - Exakt bis auf die Wirkung in der Bilanz (A2).
11. **Protokoll:** Die Formel (ask−bid)/bid×Kurs/hebel stimmt. Pflicht `--spread-pkt` oder bid/ask/hebel oder Grund. Ergänzend A4 und A7.
12. **Schatten:** wirkt nicht (geprüft: Gate-Status unabhängig). Isolation hält. Ergänzend A13.

## Replay (Punkt 6) und Plausibilität

Zwei Replay-Varianten liefen nach Prompt-Reihenfolge, mit Spread 2:
- (I) mit vollständigen datierten Bars,
- (II) "echtzeitnah": Snapshot nur zur 5-Min-Marke, laufende Kerze nur mit Open.

Für die drei Pflichttage sind die Ergebnisse identisch:

| Tag | Trigger im Fenster (gegatet) | Trades | Klassen | Σ R | Opus ausführbar (Endurteil-Sim) |
|---|---|---|---|---|---|
| 05.10. | 0 (3 Level-Crosses außerhalb des Fensters) | 0 | – | 0 | +0,96 (Entry 16:55) |
| 08.10. | 2 (dazu 3 Cap-FAIL) | 2 | TP1+BE, TP1+TP2 | +2,00 | ≈ +1,2 |
| 13.10. | 1 (zwei weitere bei offener Position) | 1 | ZEIT | −0,15 | ≈ −1,1 (15:35-SL + 17:05 −0,11) |

Bilanz an allen drei Tagen gültig, (a), (b), (d), (e), (f) ja. **(c) ist bei Trades immer NEIN (keine 1m-Bars).** Die Bilanz bleibt bei Wiederholung identisch, der Nachtrag ebenfalls.

**Abweichungen erklärt.** Die Endurteil-Gegenrechnung setzte Entries an die **alten Gate-Momente** (Zustand). Der Kern feuert nur bei einem **Cross** oder bei der **Bildung** der Richtung. Daraus folgt:
- 05.10. (Trendtag, 5m durchgehend über der EMA) hat im Fenster keinen Cross, also keinen Trade.
- 13.10. 15:35 ist kein Cross, also kein Trade.
- 08.10. wird schon am 17:30-Cross gefasst statt am 18:00-Gate. Ergebnis +0,5 statt −1,0.

**Ehrliche Zusatzzahl.** Replay (II) über alle 8 Tage mit datierten 15m/60m/QQQ-Bars ergibt n 9, Σ −4,26 R, **Ø −0,47 R**. Einschränkung: 24./25./28.09. und 06.10. liefen ohne Register, also ohne Level-Cross. Nur die 4 Tage mit Register (05./07./08./13.10.) ergeben n 5, Σ −0,15, Ø −0,03.
- Das ist bei n 9 Rauschen (SE ≈ ±0,35). Es zeigt aber: **Der Cross-Trigger ist eine andere Trade-Menge als die, auf der "≈ 0 bis +0,1 R" gerechnet wurde. Er ist ungemessen.**
- Am Testplan ändert das nichts (Prozesstest, kein Kantentest). Levi sollte die "+0,1" nicht als Erwartung für den Kern lesen.
- Der Trendtag 05.10. ohne Trade ist das bekannte Risiko des Cross-Triggers in der anderen Richtung: Er verpasst ruhige Trends.

## Prüfung von Fables Selbstbericht (Abschnitt 6 und Einzelaussagen)

- **Punkt 1 (2b-Semantik):** bestätigt, siehe oben.
- **Punkt 2 (Spread-Modell, BE, Zeit-Exit, TP2, SL vor TP):** bestätigt durch eigene Nachrechnung. Ergänzend A11.
- **Punkt 3 (Swing, Floor):** bestätigt.
- **Punkt 4 (isolierter Schatten):** die Isolation ist korrekt. Der Schreibbefund ist **selbst verifiziert**: gate_check.cjs Z. 5211-5219, Probe-Diff nur `trigger_kandidaten_log.jsonl`. "Nicht bewertbar" ist als Ausgang akzeptabel, inhaltlich schwach (A13).
- **Punkt 5 (Bilanz-(b)):** die Definition ist **fehlerhaft, siehe A2**. Fables eigene Annahme zum Sperrfall (Kommentar) trifft nicht zu.
- **Punkt 6 (vollcheck-Diff):** bestätigt.
- **Punkt 7 (Rollback):** bestätigt mit der Stempel-Einschränkung (A10).
- **Punkt 8 (5,71× statt 7×):** bestätigt. Beide ATR-Lesarten ergeben FAIL. Die Lesart "ATR nach der Trigger-Kerze" ist die regelgerechte.
- **"Technik (c) bleibt NEIN bis 1m-Bars existieren":** zutreffend. Der Prompt enthält den 1m-Abruf (count 360 deckt 14:02-20:02 ab). Die 1m-Logik ist nur synthetisch getestet. Das kein Blocker, aber am Tag 1 prüfen.
- **"Testplan Punkt 4 nicht Teil der Pakete":** zutreffend (A13).
- **"size.cjs nur mit bid/ask/hebel":** zutreffend (A14).
- **"Keine Datei schreibt in alte Logs":** bestätigt (Mirror-Hash über 188 Alt-Dateien).
- **Was Fable nicht gesehen hat:** A1 (Level-Semantik), A2 (Bilanz vs. Check-Handlung), A3 (Exit-Erkennung nach dem Check), A4 (Spread-Quelle de facto nicht vorhanden).

## Gesamtwertung

Der Rechenkern ist sauber und gut nachvollziehbar:
- Fixtures, Crosses, Richtungs-Zeitstrahl und DST stimmen auf den Punkt.
- Die Isolation der Alt-Welt hält nachweislich.
- Es gibt keinen Echtgeld-Pfad und keinen automatischen Übergang.
- 290/290 Tests sind grün.
- Fables Selbstbericht ist in den geprüften Zahlen zutreffend. Er nennt seine Abweichungen offen, und die 19:31-Entscheidung ist richtig.

Die Fehler liegen an den Nahtstellen zwischen Skripten, Prompt und Daten, also dort, wo Unit-Tests nicht hinsehen:
- Ein Level-Typ bedeutet im Register etwas anderes als im Regelwerk (A1).
- Die Bilanz erwartet Gate-Läufe, die der Prompt korrekt unterlässt (A2).
- Der Tick sieht Exits erst nach dem Check (A3).
- Kriterium (e) setzt eine Bid/Ask-Quelle voraus, die der Loop nicht hat (A4).

Keiner dieser Punkte kostet Geld. Zwei verfälschen aber die Messung (A1 Trigger an falschen Leveln, A4 Gefahr erfundener Spreads), zwei verderben gültige Testtage (A2, A3). Bei maximal 10 Tagen ist das genau das, was wir uns nicht leisten können.

**Kann der erste Kern-Fiktivtag so starten? Nein. Mit diesen Bedingungen ja:**
1. A1, A2, A3 umgesetzt (Fable). Je ein Test dazu: Overnight-Level aus Bars, Sperrfall ergibt (b) ja, Exit in Kerze X mit Trigger am X-Schluss ergibt Gate-Lauf. Opus-Kurzcheck nur dieser Stellen.
2. A4 von Levi entschieden (wer Bid/Ask liefert bzw. Referenz-Spread) und ein Satz im Prompt "keine Spread-Zahl schätzen". Dazu L3 `PRODUKT` gesetzt oder ausdrücklich leer gelassen.
3. A5 von Levi bestätigt, spätestens vor dem 26.10.
4. A6 bis A8 möglichst vor Tag 1. Sie sind klein, und ab Tag 1 gilt der Regel-Freeze.
5. Wie gehabt: Cron und Remote Control vorher an, `kern_start` mit frischem Register, Commit nur durch Levi.

Erwartungsmanagement für Levi: Der Fiktivtest misst Technik und Trigger-Verhalten, nicht die Kante. Die eigenen Replays des Cross-Triggers liegen bei n 9 eher unter null. Das ist kein Grund, nicht zu testen, aber ein Grund, die "+0,1 R" aus dem Endurteil nicht als Kern-Erwartung zu zitieren.

---

## Nachtrag 14.10. (Levi-Rückfrage A4)

**Frage Levi:** "Bid/Ask brauchen wir doch nur beim Echtgeld, aktuell bei fiktiv nicht, oder?"

**Antwort: Im Kern ja.** Levis Annahme stimmt für das, was der Fiktivtest leisten soll.

### 1. Wozu der Messwert im Fiktivtest gedacht war und was ohne ihn fehlt

Laut Endurteil-Testplan ist der Fiktivtest ein **Prozesstest, kein Kantentest** (Punkt 1). Go/No-Go läuft nur über Technik und Abbruch. Der gemessene Spread hängt an drei Stellen:

1. **Kennzahl c und "R mit gemessenem Spread"** (Testplan Punkt 3).
   - Ohne Messwert rechnet der Nachtrag ohnehin `r_spread2` mit 2 Pkt (`kern_nachtrag.cjs:30`).
   - Verloren geht nur die Aussage "Kante nach echten Kosten". Die wäre bei n ≈ 12-19 (SE ≈ ±0,22 R) gegen eine Kante ≤ 0,1 R ohnehin nicht belastbar.
2. **Spread-Gate 10 % als Schatten** (L1). Es blockiert fiktiv nichts. Ohne Messwert entfällt nur die Zählung "wie oft wäre ein Trade am Produkt gescheitert".
3. **Abbruch-Kriterium "gemessener Spread Ø > 10 % der SL-Distanz".**
   - Das Endurteil nennt es selbst "Produktfrage, nicht Regelfrage".
   - Levis Entscheid 5 vom 14.10. sagt "Spread-/Produktvergleich nicht Teil des Fiktivtests".

**Fazit zu 1.** Der Zwischenanalyse fehlt ohne Messwert nichts Entscheidendes: p, q, Zeit-Exits, Trigger-Verhalten, Technik und Trend/Range bleiben vollständig. Der Verlust betrifft das **Echtgeld-GO**.
- Dort ist der Messwert unverzichtbar. Mit 9 Pkt KO-Spread lag die ausführbare Rechnung bei Ø ≈ 0 bis −0,29 R statt +0,11 (Endurteil Kurzfazit 2).
- Gemessen wird er dort sowieso im Produktvergleich (E9), zusammen mit dem `size.cjs`-Limit (A14).
- Echtgeld bleibt ohnehin an Levis ausdrückliches GO gebunden.

Eine ehrliche Einschränkung: Levis Entscheid 5 sagte zusätzlich "im Fiktivtest wird nur Bid/Ask protokolliert". Mit dieser Antwort ändert Levi seinen eigenen Entscheid. Das ist vor Tag 1 zulässig, weil der Regel-Freeze erst ab Fiktivtag 1 gilt. Es sollte aber als Entscheid festgehalten werden.

### 2. Empfehlung

- **(e) für den Fiktivtest auf "nicht anwendbar"** setzen und im Bilanz-Text so ausweisen. Nicht stillschweigend streichen, sonst sieht später niemand, dass es fehlte.
- **Referenz-Spread 2 Pkt** als Konstante in `kern_config`, im Log überall als `spread_quelle: "ANNAHME Referenz 2 Pkt (kein Messwert)"`. Begründung:
  - Die Endurteil-Gegenrechnung und der Nachtrag (`r_spread2`) arbeiten mit 2 Pkt. Ein anderer Wert würde die Vergleichbarkeit mit der eigenen Referenzrechnung brechen.
  - 2 Pkt liegt auch beim Ziel "≤ 3 Pkt" aus E9.
  - Er wirkt auf BE und Live-R gleich wie im Nachtrag, ist also konsistent.
- **Optionale Zusatzzeile** in der Bilanz: "Spread-Gate-Schatten bei 9 Pkt (KO-Schätzung): x von n Trades wären FAIL". Sie kostet nichts, ist klar als Schätzung markiert und bereitet die Produktfrage vor. Kein Muss.
- **Keine Pflicht-Stichproben durch Levi im Fiktivtest.** Gründe:
  - Sie verlangen Anwesenheit am Gate-Zeitpunkt.
  - Sie würden (e) als Kriterium wieder aufleben lassen.
  - 5-15 Ablesungen ersetzen den Produktvergleich nicht.
  - Freiwillige Ablesungen sind mit `--bid/--ask/--hebel` weiterhin möglich und werden dann als Messwert geloggt (Pfad besteht schon).
- **Bid/Ask-Messung, Spread-Gate hart und das Abbruch-Kriterium Spread** werden ausdrücklich auf "vor einem Echtgeld-GO" verschoben, zusammen mit A14.

### 3. Was sich an A4 und den Start-Bedingungen ändert

**A4 ist kein Levi-Entscheidungs-Blocker mehr** (Levi hat entschieden). Er bleibt aber ein **kleiner Code- und Prompt-Rest vor Tag 1**, weil der Code heute so arbeitet:
- Ohne Spread-Argument bricht `kern_gate` mit Exit 1 ab, oder es braucht `--grund-spread`.
- Damit wird (e) NEIN.
- Der Prompt sagt weiter "Bid/Ask ablesen". Das ist die Einladung, eine Zahl zu erfinden.

**Betroffene Stellen (nur Liste):**
- `kern_config.cjs:42` und `:52`: `PRODUKT`/`SPREAD_NACHTRAG_PKT`. Neue Konstante Referenz-Spread plus Kennzeichen "Annahme".
- `kern_gate.cjs:80-89`: Spread-Pflicht. Ohne Angabe gilt die Referenz mit `spread_quelle` ANNAHME statt Exit 1. `--bid/--ask/--hebel` bleiben als freiwilliger Messweg.
- `kern_gate.cjs:105, 126, 130, 142`: BE, Trade- und State-Spread, Kastenzeile (Annahme sichtbar).
- `kern_regeln.cjs:156-159`: Spread-Schatten-Text "Annahme" statt Messwert.
- `kern_check.cjs:76`: Handlungstext ohne `--spread-pkt <Pkt>`.
- `kern_bilanz.cjs:85-86`: (e) "n/a (fiktiv, Annahme)". Das hat ok-Wirkung, wird aber ausgewiesen.
- `kern_bilanz.cjs:93`: c-Kennzahl als Annahme markieren.
- `kern_bilanz.cjs:133-138`: Abbruch "Ø Spread > 10 %" nur bei Messwerten werten.
- `kern_nachtrag.cjs:29`: `spread_gemessen` nur aus echten Messwerten, sonst `null` und Kennzeichen Annahme.
- `kern_loop_prompt.cjs:35-37`: Prompt-Satz ersetzen durch "Kein Bid/Ask nötig, Spread = Referenz-Annahme. Nie eine Spread-Zahl schätzen oder eintragen."
- `tests/kern.test.js`: Gate ohne Spread ergibt PASS mit ANNAHME, (e) n/a, kein Abbruch über Annahme-Spread.
- Memo `regelwerk_kern_2026-10.md` (Regel 11 und Levi-Entscheid 5): Entscheid-Änderung datiert vermerken.

### Aktualisierte Start-Bedingungen für den ersten Kern-Fiktivtag

1. **A1, A2, A3** umgesetzt (Fable), je ein Test, Opus-Kurzcheck. **Unverändert Blocker.**
2. **A4 (neu):** Referenz-Spread 2 Pkt als ANNAHME und (e) "n/a" umgesetzt, Prompt-Satz "nie Spread schätzen" drin, Levis Entscheid-Änderung im Memo vermerkt. Kleiner Rest, im selben Opus-Kurzcheck. Keine Bid/Ask-Quelle mehr nötig.
3. **A5** von Levi bestätigt, spätestens vor dem 26.10.
4. **A6-A8** möglichst vor Tag 1 (Regel-Freeze).
5. Cron und Remote Control vorher an, `kern_start` mit frischem Register, Commit nur durch Levi.
6. **Vor einem späteren Echtgeld-GO (nicht für den Fiktivstart):** Produktvergleich mit gemessenem Bid/Ask, Spread-Gate 10 % hart, Abbruch-Kriterium Spread aktiv, `size.cjs`-Positions-Limit klären (A14).

---

## Kurzcheck Auflagen-Umsetzung 14.10.

**Urteil: FREIGEGEBEN MIT RESTAUFLAGEN.** Es gibt keine Start-Blocker mehr. Die Restauflagen R1 bis R4 sind klein und blockieren den Start nicht.

**Zeit und Hygiene:**
- Start 16:53:35 DE, Ende 17:02 DE.
- sha1 von 300+ Repo-Dateien (scripts/tests/package.json) und aller Memory-Dateien vorher und nachher **identisch**.
- Gerechnet wurde nur auf Kopien im Scratchpad (`v2`, `w1` bis `w4`).
- git nur `status` und `diff --stat`. Kein test:e2e. MEMORY.md ist nicht angefasst.
- Geändert habe ich nur diese Datei (Anhang).

### Punkt 7: Was hat Fable seit meinem ersten Check geändert?

- Laut sha1-Vergleich mit meinem Stand von 13:54 haben sich **nur** geändert: 12 `kern_*.cjs`, `tests/kern.test.js` und die neue Datei `_freeze_alt_2026-10-14_erweitert.sha1`.
- `vollcheck.cjs` und `package.json` sind seit 13:54 unverändert (`git diff --stat` weiterhin 9+6 Zeilen).
- K0-Liste: nur die zwei bekannten FAILED. Die erweiterte Liste stimmt 112/112.
- Die Testdateien `trading_scripts` und `tagesmomente` sind sha-gleich zum Freeze.
- **Tests: 298 von 298 grün** (kern 52).
- Außerhalb der Auflagen wurde nichts verändert.

### Tabelle A1 bis A13

| # | Ergebnis | Beleg / eigene Prüfung |
|---|---|---|
| A1 Overnight | **bestätigt, Rest R1** | `kern_start.cjs:54-66` und `:36-45`. Selbst aus den datierten Bars nachgerechnet (Vortag 22:00 bis heute 15:00 DE):<br>05.10. Tief **30709,2** (Hoch 30970,1, 192 Kerzen, Fr 22:00 bis Mo 15:00)<br>08.10. **31222,7 / 30877** (deckt sich mit den Register-Nachtwerten)<br>13.10. **30683,4 / 30386,6**<br>`vortag()`: Mo 26.10. ergibt Fr 23.10. (22:00, keine Divergenz); Mo 02.11. ergibt Fr 30.10. (21:00, Divergenz). Beides richtig.<br>`Session-*` werden nicht mehr verwendet.<br>Wochenende ist abgedeckt. Lücken führen zu einem HINWEIS, keine Bars zu `null`. |
| A2 Bilanz (b) | **bestätigt, Rest R2** | `kern_bilanz.cjs:60-92`: gleiche `gates()`-Logik wie der Check, Blackout aus dem Check-Log.<br>Replay 08.10. mit CPI 17:40: "(b) ja — regelkonform blockiert 5 (… 17:20 cap+sperre, 17:30 sperre …)". |
| A3 Exit vor Trigger | **bestätigt** | `kern_check.cjs:66-71` rechnet die Position vor der Trigger-Prüfung ab; der Prompt ruft den Tick vor dem Check auf.<br>**Selbst gebauter Fall:** 08.10. so verändert, dass BE in der 18:10-Kerze fällt und ein EMA-Cross am 18:15-Schluss liegt. Ergebnis: Trigger 18:15 wird um 18:20 gegatet (PASS).<br>Das gilt in **beiden** Aufrufreihenfolgen (Tick oder Check zuerst); (b) ja; kein doppelter Trigger. Tageslimit 3 greift danach korrekt (18:45 "limit"). |
| A4 Spread-Annahme | **bestätigt, Rest R3** | Ohne Spread kein Exit 1 mehr. Überall steht ANNAHME (Kasten, `spread_quelle`, Nachtrag `spread_basis`, Bilanz `c (ANNAHME 2 Pkt)`, (e) "n/a").<br>Das Abbruch-Kriterium Spread wird nur mit Messwerten gewertet.<br>Prompt-Satz "NIE eine Spread-Zahl schätzen" ist drin; der Selbsttest prüft ihn (`kern_loop_prompt.cjs:96`); `--spread-pkt` ist im Prompt nicht mehr erlaubt.<br>**Rest:** Im Code nimmt `--spread-pkt <x>` weiterhin einen Wert an und protokolliert ihn als **"Messwert (Handeingabe)"** (`kern_gate.cjs:68`). Ein geschätzter Wert kann also technisch als Messwert geloggt werden. |
| A5 DST | **bestätigt** | `kern_zeit.cjs:22-24`. 27.10.: Loop 14:20, Session-Start 14:30, Entry 14:35-19:00, Zeit-Exit 20:00, Terminal 20:02 – identisch in `fenster()`, Prompt (61 Zeilen, Exit 0) und Check (`imFenster` aus `fenster`). 13.10. und 02.11. sind Normaltage. |
| A6 Register-Frische | **bestätigt** | `kern_start.cjs:123-133`: Register nicht von heute führt zu Exit 1, mit `--register` nur zu einer Warnung (Test Z. 242). |
| A7 Spread ≤ 0 | **bestätigt** | `kern_gate.cjs:68-73`: Spread ≤ 0 und bid ≥ ask führen zu Exit 1. `spreadPkt` liefert bei ask ≤ bid `null` (`kern_regeln.cjs:305-309`). Im Nachtrag führt `--spread-gemessen` ≤ 0 zu Exit 1. |
| A8 Zeit-Exit-Text | **bestätigt** | Prompt Z. 60-62 ("NICHTS aufrufen … nie --notfall"), Tick-Handlungstext, Hinweis bei `--notfall`. Die Replays zeigen "19:58 fällig" und um 20:00 "Zeit-Exit ausgeführt". |
| A9 Freeze erweitert | **bestätigt** | 112/112 OK. |
| A10 Rollback-Doku | **bestätigt** | `regelwerk_kern_2026-10.md` "Rollback": `kern_zeit.cjs` darf nicht gelöscht werden. |
| A11 Gap-Fill | **bestätigt** | `kern_regeln.cjs:225-230`. Eigene konstruierte Fälle:<br>Long Open 85 / SL 90 ergibt **−1,5** (vorher −1,0); Short spiegelbildlich −1,5; ohne Gap −1,0.<br>TP1, danach Gap unter BE (Open 95) ergibt +0,25.<br>Die 6 Sim-Fixtures bleiben unverändert (+0,49 / +0,96 / +1,50 / −1,00 / −1,00 / −0,13), keiner hat ein Gap. |
| A12 Kleinpunkte | **bestätigt** | Argumentgrenze Check 4 und Gate 8 (`kern_common.argGrenze`), doppelte Bars werden entfernt, im Nachtrag wird `cap_nachtrag` ausgewiesen. |
| A13 Schatten/Alt-Momente | **bestätigt (Messung vereinfacht)** | UNKNOWN wird als "nicht bewertbar" ausgewiesen. Die Alt-Zustandsmomente werden gezählt (05.10.: 15, 08.10.: 7, 13.10.: 1). Das ist eine bewusst grobe Annäherung, nur Messung. |

### Prüfung von Fables Nachtrag (Gutachten in eigener Sache)

- **"3 Replays neu gelaufen: alle unverändert" stimmt nur für A11.** Mit den richtigen Overnight-Leveln aus A1 ändert sich das Replay vom 05.10.:
  - Level-Cross über ON_HOCH 30970,1 um 16:45, Long-Entry 16:50, Ergebnis **TP1+ZEIT +1,11 R** (vorher 0 Trades).
  - Neues Ergebnis für 05./08./13.10.: **n 4, Σ +2,96 R** (statt n 3, +1,85).
  - Fable hat beim Replay vermutlich kern_start ohne `--bars` (datiert) laufen lassen, also mit Overnight `null` oder Overnight-Werten aus einer anderen Datei.
- **Echtzeitnahes Replay über 8 Tage (Tick zuerst, neuer Code):** n 11, Σ −2,65 R, Ø −0,24 R. Neu ist +0,5 am 28.09. (Level-Cross), dazu der 05.10.
  - (b) ist an allen 8 Tagen "ja".
  - (c) ist an Tagen mit Trades wie erwartet NEIN, weil 1m-Bars fehlen.
- **Mein Befund vom ersten Check bleibt bestehen:** Der Cross-Trigger ist bisher ungemessen; die Zahlen liegen bei kleinem n um null. Am Testplan ändert das nichts.

### Punkt 6: BE in der TP1-Kerze – Entscheidung: **keine Code-Änderung, nur A15 (Doku, niedrig)**

Begründung:
1. Mit 5m-OHLC lässt sich die Reihenfolge innerhalb der TP1-Kerze nicht bestimmen.
   - Fällt TP1 in die Entry-Kerze, liegt das Open (= Entry) immer jenseits der BE-Marke.
   - Ein BE-Check dort würde **jeden** Treffer zu BE machen, also systematisch falsch-negativ werten (Beispiel 08.10. 18:45: +1,50 würde zu +0,50, obwohl das Hoch vor TP1 lag).
   - Das wäre nicht konservativ, sondern ein Artefakt.
2. Reale Ausführbarkeit nach Endurteil-Regel 7 und 11 ("TP1/TP2 per Alarm"):
   - Bei Scalable sind SL und TP exklusiv. Nach TP1 muss Levi teilverkaufen und den Stop neu setzen; das dauert Minuten.
   - Ein in derselben 5m-Kerze nachgezogener und dort getroffener BE-Stop ist praktisch nicht ausführbar.
   - Die Konvention "BE und TP2 erst ab der Kerze nach TP1" entspricht also eher der Realität.
3. Sie ist leicht optimistisch, und zwar nur für den Fall "TP1, dann Rücklauf auf BE in derselben Kerze". Genau das deckt der 1m-Nachtrag ab (`intrabar_detail.abweichung`), sobald die 1m-Bars existieren.

**A15 (niedrig, kein Start-Blocker):**
- Die Konvention steht bereits in `regelwerk_kern_2026-10.md` Regel 7.
- Ergänzen: In `kern_bilanz` die Zahl der Trades mit 1m-Abweichung (`intrabar_detail.abweichung`) als Kennzahl ausweisen.
- Für die Zwischenanalyse vormerken: Bei ≥ 2 Abweichungen in 10 Tagen wird die Konvention mit 1m neu bewertet.
- **Folgen für Fixtures und Replay-Summe: keine.** +1,85 bzw. jetzt +2,96 bleiben.

### Restauflagen (keine blockiert den Start)

- **R1 (niedrig):** `kern_start` prüft nicht, ob `scripts/nas100_5m.json` von heute ist.
  - Mit einer alten Datei steht Overnight nur mit HINWEIS auf `null` oder unvollständig, ohne Exit.
  - **Vorschlag:** Exit 1, wenn die letzte Kerze älter als 30 Min ist (ohne explizites `--bars`).
- **R2 (niedrig):** Die Bilanz akzeptiert für (b) zusätzlich "der Check sagte FAIL" (`checkFail`, `kern_bilanz.cjs` ~Z. 84), auch wenn ihre eigene Nachrechnung PASS ergibt.
  - Eine Abweichung zwischen Check und Bilanz bleibt dadurch unsichtbar.
  - **Vorschlag:** solche Fälle als eigene Zeile "Abweichung Check/Bilanz" ausweisen (nicht als NEIN).
- **R3 (niedrig-mittel):** `--spread-pkt` wird als "Messwert (Handeingabe)" geloggt (`kern_gate.cjs:68`).
  - **Vorschlag:** Im Fiktivtest `--spread-pkt` ablehnen und als Messweg nur `--bid/--ask/--hebel` zulassen, oder die Quelle "Handeingabe, ungeprüft" nennen und `spread_messwert` = false setzen.
  - Der Prompt verbietet `--spread-pkt` bereits; das Risiko ist also gering.
- **R4 (niedrig):** In Fables Bericht die Aussage "Replays unverändert" berichtigen (05.10. jetzt +1,11 R durch A1), damit die Zwischenanalyse nicht von falschen Ausgangszahlen ausgeht.
- **A15 (niedrig):** wie oben.
- **Unverändert offen, nicht für den Fiktivstart:** A14 und die Echtgeld-Themen (Bid/Ask, hartes Spread-Gate, Abbruch-Kriterium Spread).

### Kann der erste Kern-Fiktivtag starten?

**Ja.** R1 bis R4 und A15 können vor Tag 1 nachgezogen werden (Fehlerkorrektur, Opus-Kurzblick genügt). Sie dürfen aber auch als datierte Fehlerkorrekturen nach Tag 1 kommen; keine davon verfälscht Trigger oder R.

Was Levi vorher braucht:
1. **Commit** der Kern-Dateien, der erweiterten Freeze-Liste, von `vollcheck.cjs` und `package.json`. Commit macht nur Levi.
2. **Den ersten Fiktivtag festlegen (L4).** Cron und Remote Control vorher an (`feedback_testtag_start_verlaesslichkeit`).
3. **Start-Update am Tag:**
   - Register-Pflege für PDH/PDL/PP/R1/S1 mit `updated` von heute, sonst gibt `kern_start` Exit 1.
   - NAS100 5m count 300 nach `scripts/nas100_5m.json` speichern, **vor** `kern_start` (Overnight-Level).
   - Kalender-Events als `--event HH:MM:Typ`.
4. **Erwartung:**
   - (c) wird erst mit dem 1m-Abruf beim Tagesabschluss "ja"; am Tag 1 prüfen, dass `nas100_1m_<datum>.json` entsteht.
   - (e) steht im Fiktivtest auf "n/a".
   - Ergebnis-R ist Messgröße, keine Prognose.
   - Echtgeld gibt es nur nach Levis ausdrücklichem GO; der Code hat dafür keinen Pfad.

---

## Kurzcheck 2 Restauflagen R1-R4/A15 (14.10.)

**Urteil: FREIGEGEBEN.** Es bleiben zwei kleine Hinweise (R5, R6). Sie sind keine Start-Bedingung.

**Zeit:** Start 17:29:47 DE, Ende 17:35 DE.

**Hygiene:**
- sha1 von Repo und Memory vorher und nachher **identisch**.
- Gerechnet nur auf Kopien (`v3`, `r1`-`r3`). git nur `diff --stat`/`status`.
- MEMORY.md ist nicht angefasst. Geändert habe ich nur diesen Anhang.

**Punkt 5: Nichts außerhalb von R1-R4/A15 verändert.**
- Seit Kurzcheck 1 haben sich nur `kern_start`, `kern_config`, `kern_gate`, `kern_nachtrag`, `kern_bilanz` und `tests/kern.test.js` geändert. `kern_regeln.cjs` ist unverändert, also auch die Exit-Konvention (A15).
- K0: nur `vollcheck.cjs` und `package.json` FAILED, `git diff --stat` weiterhin 9+6 Zeilen.
- Erweiterte Freeze-Liste 112/112 OK.
- **Tests 302/302 grün.** Die alten Testdateien sind sha-gleich.
- Die 6 Sim-Fixtures bleiben bei +0,49 / +0,96 / +1,50 / −1,00 / −1,00 / −0,13.
- Replay 05./08./13.10.: +1,11 / +2,00 / −0,15, also Σ **+2,96**. Kriterium (b) ist an allen drei Tagen "ja".

| # | Ergebnis | Beleg / eigene Prüfung |
|---|---|---|
| R1 Bars-Frische `kern_start` | **bestätigt, Hinweis R6** | `kern_start.cjs:139-150`, `kern_config.cjs:20`. Eigene Fälle:<br>(a) Start 15:08 mit Bars bis 15:05: läuft, Overnight 30683,4/30386,6.<br>(b) Start 15:45 mit Bars, deren letzte Kerze 35 Min alt ist: Exit 1.<br>(c) Bars vom 08.10.: Exit 1.<br>(f) DST-Tag 27.10. (synthetisch), Start 14:08: läuft, Nachtfenster 26.10. 21:00 → 14:00.<br>Ein regulärer Start-Update (Bars um 15:00-15:20 gespeichert) wird **nicht** fälschlich blockiert.<br>**Lücke:** Eine **leere oder fehlende** Standard-Datei führt nur zu einem HINWEIS und Overnight = `null`, kein Exit (Fall d/e). |
| R2 Abweichung Check/Nachrechnung | **bestätigt** | `kern_bilanz.cjs:84-91, 130, 172`. Konstruierter Fall: 08.10. mit CPI, danach die Blackouts aus dem Check-Log entfernt.<br>Ergebnis: "Abweichungen Check/Nachrechnung 1 … Check 17:35:20 sagte FAIL sperre, Nachrechnung PASS" als eigene Zeile, auch in `--auswertung`.<br>Die drei Mengen (Gate vorhanden / Abweichung / ohne Gate) sind per Konstruktion disjunkt, es gibt **keine Doppelzählung**.<br>Ein Tag ohne Abweichung zeigt "0".<br>Wie vorgeschlagen bleibt (b) dabei "ja". Die Abweichung ist sichtbar, aber nicht durchfallrelevant. In der Zwischenanalyse muss die Zahl angesehen werden. |
| R3 Spread nur aus Bid/Ask | **bestätigt im Gate, Rest R5 im Nachtrag** | `kern_gate.cjs:65-78`: `--spread-pkt` (mit Wert, ohne Wert oder negativ) ergibt immer Exit 1. Bid ≥ Ask ergibt Exit 1. Ein echter Bid/Ask-Wert wird als `spread_quelle_typ: bid_ask` geloggt, ohne Angabe als `annahme`.<br>Der Prompt kennt `--spread-pkt` und `--spread-gemessen` nicht; der Selbsttest wehrt beide ab (`ERLAUBTE_ARGS`).<br>Kein Fixture, kein Replay und kein Test bricht. |
| R4 Replay-Aussage | **bestätigt** | `regelwerk_kern_2026-10.md` "Replay-Stand (R4)" und Fables Bericht: n 4, Σ +2,96. Dazu das echtzeitnahe Replay mit n 11, Ø −0,24.<br>Die Ursache ist richtig benannt (Overnight war `null`).<br>Der Satz "keine Erwartungswert-Zahl" steht drin. Eine Erwartungsaussage gibt es nicht mehr. Sachlich korrekt. |
| A15 Kennzahl 1m-Abweichung | **bestätigt** | `kern_bilanz.cjs:110-112, 140, 172`. Wird in Tages- und Gesamtsicht gezählt.<br>Die Konvention (BE/TP2 ab der Kerze nach TP1) ist unverändert (`kern_regeln.cjs` nicht berührt). |

### Neue Hinweise (keine Start-Bedingung)

**R5 (niedrig): `kern_nachtrag --spread-gemessen <x>` ist ein Schlupfloch für Messwerte.**
- Die Zahl wird ohne Beleg als "Messwert (… Quelle vom Operator zu belegen)" übernommen.
- Probe 13.10. mit `--spread-gemessen 9`: Aus dem Trade ZEIT −0,15 wird "gemessen" **SL −1,00**. Die Bilanz zählt das dann als Verlust 1, Tag −1 R, und c = 0,09 als **Messwert**. Damit fließt es auch in das Abbruch-Kriterium Spread und in den Tagesstopp.
- Der Prompt ruft das Argument nicht auf, das Risiko ist also gering. Fables Satz "ein Zahlenwert ohne Quelle landet nie im Log" stimmt deshalb nur für `kern_gate`.
- **Vorschlag:** Im Fiktivtest `--spread-gemessen` abweisen, oder den Wert als "Szenario" führen (`spread_messwert` false, nicht in c, Abbruch oder Tagesstopp).

**R6 (niedrig): Leere oder fehlende Standard-Bars in `kern_start` führen nur zu einem HINWEIS.**
- Overnight-Hoch und -Tief stehen dann auf `null`. Damit fallen zwei Level-Cross-Quellen für den ganzen Tag weg, und der Start läuft trotzdem durch.
- **Vorschlag:** Ohne `--bars` auch hier Exit 1, analog zu R1.

### Kann der erste Kern-Fiktivtag starten?

**Ja.** Levi braucht vorher nur noch:
1. den **Commit** (nur durch Levi): alle `kern_*.cjs`, `tests/kern.test.js`, beide Freeze-Listen, `vollcheck.cjs`, `package.json`;
2. den **Termin** des ersten Fiktivtags. Cron und Remote Control sind vorher an.

Am Tag selbst gilt der Ablauf aus dem Prompt:
- Register-Pflege mit Datum von heute.
- NAS100 5m count 300 nach `scripts/nas100_5m.json` speichern, **vor** `kern_start`. Spätestens 30 Min vor dem Aufruf, sonst gibt es Exit 1.
- Events als `--event`.
- Beim Tagesabschluss die 1m-Bars sichern, damit Kriterium (c) erfüllbar ist.

R5 und R6 können als dokumentierte Fehlerkorrektur auch nach Tag 1 kommen. Echtgeld gibt es nur nach Levis ausdrücklichem GO.

---

## Commit-/Push-Empfehlung 14.10.

Das ist nur eine Empfehlung. **Commit und Push macht ausschließlich Levi** (`feedback_commit_push_nur_levi`). Ich habe nur gelesen (`git status/log/remote/diff --stat/check-ignore`), nichts gestaget und kein fetch ausgeführt. Grundlage ist der Stand **nach Fables Fertigmeldung zu R5/R6**, deshalb sind die Dateien nach Mustern beschrieben.

### 1. Code-Repo `tradingview-mcp` (main, origin = github.com/LeviUmn/tradingview-mcp)

**Ausgangslage:**
- `git status -sb` meldet "ahead 1": `19b2581` ist laut letztem bekannten Remote-Stand **noch nicht gepusht**.
- Bisherige Praxis laut `git log`: ein Commit je abgenommenem Paket, deutsche Message mit Bezug auf den Opus-Gegencheck, Co-Authored-By-Zeile.
- `loop_archiv/`, `last_*`, `*.bak` und `analyse/abschluss_*` wurden **nie** committet. Laut `.gitignore`-Kommentar entscheidet Levi bei `loop_archiv/` je Datei.

**Commit 1 – DST-Fix.**
- Dateien: `scripts/kern_zeit.cjs`, `scripts/vollcheck.cjs`.
- `kern_zeit.cjs` gehört zwingend hierher: `vollcheck.cjs` lädt es (`require('./kern_zeit.cjs')`). Ohne die Datei wäre der Commit-Stand nicht lauffähig.
- Message-Entwurf:
  ```
  K1 DST-Fix (Kern-Auftrag 14.10.2026): vollcheck.cjs Order-Sperre/Halbierung aus kern_zeit.fenster() statt fester Stunden 15/16

  kern_zeit.cjs (ICU-Messung Europe/Berlin vs. America/New_York, Divergenzfenster 26.-30.10.2026 = Session-Start 14:30 DE).
  vollcheck.cjs: 9 Diff-Zeilen, Normaltage unveraendert (15:00-15:29 / 15:30-15:59 / 16:00-16:04). Frist 19.10. eingehalten.
  Opus-Gegencheck 14.10.2026: DST-Grenzen nachgerechnet, freigegeben.

  Co-Authored-By: Claude Sonnet 5.5 <noreply@anthropic.com>
  ```

**Commit 2 – Kern-Regelwerk 1.0.**
- Dateien:
  - alle übrigen `scripts/kern_*.cjs`, und zwar **nur Code-Dateien `*.cjs`**,
  - `tests/kern.test.js`,
  - `package.json` (Testliste; erst hier, weil sie auf `tests/kern.test.js` verweist),
  - `scripts/_freeze_alt_2026-10-14.sha1` und `scripts/_freeze_alt_2026-10-14_erweitert.sha1`,
  - die `.gitignore`-Ergänzung (siehe unten).
- Message-Entwurf:
  ```
  Kern-Regelwerk 1.0 (Opus-Endurteil 14.10.2026, Levi-Freigabe): kern_*-Skripte daneben, altes Regelwerk eingefroren (Variante B)

  12 Regeln als reine Funktionen (kern_regeln/kern_config), Start/Check/Gate/Positions-Tick/Nachtrag/Bilanz/Stopp,
  Cron-Prompt < 120 Zeilen eingebettet, Schatten "alt haette" in isolierter Temp-Kopie (gate_check --dry-run schreibt
  trigger_kandidaten_log), Overnight-Level aus 5m-Bars, Spread im Fiktivtest = ANNAHME 2 Pkt (nur bid/ask als Messwert).
  testtag_modus ausschliesslich fiktiv, kein Echtgeld-Pfad. Freeze-Listen K0 (81) + erweitert (112).
  Opus-Gegencheck + Kurzchecks 1-3 (14.10.2026): Auflagen A1-A15, R1-R6 umgesetzt, freigegeben. Tests 3 Dateien gruen.

  Co-Authored-By: Claude Sonnet 5.5 <noreply@anthropic.com>
  ```
  Die Testzahl nach R5/R6 setzt Levi aus dem letzten `node --test`-Lauf ein.

**`.gitignore`-Frage: ja, vor Tag 1 klären.** Ab morgen entstehen Kern-Laufzeitdateien, die **nicht** ignoriert sind (geprüft mit `git check-ignore`):
- `scripts/kern_state.json`
- `scripts/kern_*_log.jsonl` (check, gate, position, exit, loop_stopp, quick_tick)
- `scripts/kern_trades_fiktiv.jsonl`
- `scripts/kern_level_*.json`
- `scripts/kern_archiv/`
- `scripts/last_kern_position.json`

`nas100_1m_*.json` ist durch `scripts/nas100_*.json` bereits abgedeckt.

Empfehlung: diese Muster in Commit 2 aufnehmen. Die Muster `scripts/kern_*.json`/`.jsonl` treffen keine Code-Datei, weil die Skripte `.cjs` sind. Ausnahme wie bei `loop_archiv/`: Will Levi `kern_archiv/` als Tagesprotokoll versionieren, lässt er nur diesen Ordner draußen.

**NICHT committen** (Laufzeit- und Tagesartefakte, bisherige Praxis):
- `scripts/last_loop_prompt_*.txt`, `last_loop_prompt_err.txt`, `last_gate_check_*_exit*.txt`, `last_protokoll_bilanz_*.txt`
- `scripts/*.bis1900.bak`, `scripts/level_register.json.bis*.bak`
- `scripts/analyse/abschluss_2026-10-08/` und `abschluss_2026-10-13/` (Tagesabschluss-Ausgaben)
- `scripts/loop_archiv/` (nur auf ausdrücklichen Wunsch Levis je Datei)
- alle `kern_*`-Laufzeitdateien (siehe oben)

**Separat zu entscheiden, nicht Teil des Kerns:**
- `scripts/analyse/backtest_2026-09-24.cjs` und `backtest_2026-09-25.cjs` sind Lese-Skripte vom 24./25.09. Wenn Levi sie behalten will, als eigener dritter Commit "Analyse-Skripte Backtest 24./25.09.". Sonst untracked lassen.

**Hinweis:** `tests/kern.test.js` braucht lokale Daten, die nicht im Repo sind: `level_register.json.bis2026-10-13.bak`, datierte Bars vom 13.10. Ein frischer Clone kann diese Tests nicht laufen lassen. Für das Projekt ist das kein Blocker, sollte aber bekannt sein (eventuell später kleine Fixture-Dateien unter `tests/fixtures/`).

### 2. Memory-Repo (eigenes Git-Repo, origin = github.com/LeviUmn/trading-memory, **öffentlich**)

**Ausgangslage:** `main...origin/main` ist gleichauf, letzter Commit `0537860` (Memory 03.10.). Seitdem sind **34 Dateien** offen:
- 5 geändert: `MEMORY.md`, `fable_auftrag_2026-10-03…`, die 3 Kopfvermerk-Memos
- 29 neu: Testtage 05.-13.10., Opus-Berichte, Endurteil, Kern-Auftrag/-Umsetzung, `regelwerk_kern_2026-10`, dieser Bericht

**Inhaltsprüfung (grep auf alle 34 Dateien):**
- **Keine** E-Mail-Adressen, Passwörter, Tokens, API-Keys, IBAN, Depot- oder Kontonummern, Telefon oder Adresse.
- Enthalten sind: Kapitalbasis und Stufen (5.000/10.000/50.000 €), einzelne Trade-Beträge, das Risiko 75 €, der Windows-Benutzername in Pfaden (`C:\Users\umnus`), der Vorname Levi und die Strategie- und Regeldetails.
- **Diese Inhaltsklasse ist bereits öffentlich:** 27 Dateien mit dem Benutzernamen und 59 mit €-Beträgen liegen schon in HEAD. Es gibt also keine neue Sensitivitätsklasse, aber auch keine Entwarnung für die Grundsatzfrage.

**Empfehlung:** Ein Commit "Memory 05.-14.10.2026: Testtage 05./07./08./13.10., Opus-Berichte, Endurteil Regel-Lockerung, Kern-Regelwerk 1.0 (Auftrag, Umsetzung, Gegencheck + Kurzchecks), Levi-Entscheide" mit allen 34 Dateien. Zu committen ist der Stand **nach** Fables R5/R6-Memo-Update.

### 3. Push

- **Code-Repo: Push ja, nach dem Opus-Kurzcheck R5/R6 und vor dem Start von Tag 1 (15.10. vor 15:00).** Grund: Backup des freigegebenen Standes, der danach eingefroren ist. Das schiebt auch das noch offene `19b2581` mit hoch.
- **Memory-Repo: Push ja, gleiche Zeit, sofern Levi die Öffentlichkeit bewusst bestätigt.** Technisch spricht nichts dagegen; inhaltlich wird das komplette Kern-Regelwerk samt fiktiver Ergebnisse öffentlich. Wer das nicht will, stellt das Repo auf privat, bevor er pusht. Zurückhalten wäre nur dann sinnvoll.
- **Remote-Risiko:** ohne fetch nicht prüfbar. Der lokale Stand zeigt keine Divergenz (Code ahead 1/behind 0, Memory gleichauf), der Remote-Stand kann sich seit dem letzten fetch aber geändert haben.
  - Vor dem Push: `git fetch` und `git status -sb`.
  - Bei "behind" oder "diverged": **kein Force-Push**, erst klären.
- `git push` ohne `--force`, je Repo einzeln.

### 4. Rückweg (Vorschlag)

- **Vor Commit 1:** Tag `vor-kern-1.0` auf `19b2581`.
- **Nach Commit 2:** Tag `kern-1.0-freeze`. Das ist der Stand, mit dem Tag 1 läuft; jede spätere Fehlerkorrektur ist dagegen diffbar.
- **Betrieblicher Rückweg ohne git:** alter Prompt `node scripts/loop_prompt.cjs`. Die `kern_*`-Dateien bleiben liegen, `kern_zeit.cjs` darf **nicht** gelöscht werden (A10).
- **Rücknahme im Repo:** `git revert <Commit2> <Commit1>`. Das erzeugt neue Commits, ist push-sicher und braucht kein Force; `reset` und Force-Push sind nicht nötig.
- **Memory-Repo:** analog Tag `memory-2026-10-14`.

### Offene Fragen an Levi

1. Ist `LeviUmn/tradingview-mcp` öffentlich oder privat? Für den Code unkritisch (keine Secrets, `.env`/`.mcp.json` sind ignoriert), aber gut zu wissen.
2. Soll das Memory-Repo öffentlich bleiben, jetzt mit dem vollständigen Kern-Regelwerk und den Testergebnissen?
3. `kern_archiv/` ignorieren oder als Tagesprotokoll versionieren? Gleiche Frage wie bei `loop_archiv/`.
4. Die Backtest-Skripte 24./25.09. committen oder liegen lassen?

---

## Kurzblick 3 R5/R6 (14.10.)

**Urteil: FREIGEGEBEN.** Es gibt keine Restauflagen.

**Zeit und Hygiene:**
- 17:41-17:45 DE.
- sha1 von Repo und Memory vorher und nachher identisch.
- Gerechnet nur auf Kopien (`v4`, `r5`, `r6`).

**Was Fable geändert hat:** Seit Kurzcheck 2 nur `kern_nachtrag.cjs`, `kern_start.cjs` und `tests/kern.test.js`. Dazu:
- `git diff --stat` unverändert (9+6 Zeilen).
- K0: nur `vollcheck.cjs` und `package.json` FAILED.
- Erweiterte Freeze-Liste 112/112 OK.
- `kern_regeln`, `kern_gate` und `kern_bilanz` sind unberührt.
- **Tests selbst gelaufen: 304/304 grün.**

**R5 – bestätigt** (`kern_nachtrag.cjs:65-69, 52`)
- Probe 13.10. mit `--spread-gemessen 9`: Exit 1. Mit `--spread-gemessen` ohne Wert: ebenfalls Exit 1.
- `kern_trades_fiktiv.jsonl` ist danach sha-gleich. Der Trade behält `r_spread_gemessen: null` und "ANNAHME".
- Einen anderen Weg, wie ein Spread ohne Beleg als Messwert in die Bilanz kommt, gibt es nicht mehr:
  - `kern_gate` nimmt nur `--bid/--ask/--hebel` an (`--spread-pkt` ergibt Exit 1).
  - Die Erlaubt-Liste im Prompt-Selbsttest enthält keines der beiden Spread-Argumente.
  - Die Bilanz liest `r_spread_gemessen` nur aus dem Nachtrag.
- Randnotiz: Die Schreibweise `--spread-gemessen=9` wird vom Parser als fremdes Argument still ignoriert. Wirkung: keine, die Datei bleibt identisch, kein Messwert. Keine Auflage.

**R6 – bestätigt** (`kern_start.cjs:150-158`)
- Fehlende, leere oder kaputte Standard-Bars ergeben Exit 1. **Es entsteht weder `kern_state.json` noch eine Level-Datei noch ein Archiv-Eintrag**, ein erneuter Lauf braucht kein `--neu-grund`.
- Regulärer Start-Update läuft durch:
  - 13.10. um 15:28 mit Bars bis 15:25.
  - DST-Tag 27.10. (synthetisch) um 14:28 mit Bars bis 14:25: Nacht 26.10. 21:00 bis 14:00, Overnight 30683,4/30386,6.
  - Eine falsche Blockade habe ich nicht gefunden.
- Fehlende oder leere Datei bei explizitem `--bars`: Warnung, Overnight = `null`, Start läuft. Das ist richtig so: `--bars` ist nur für Tests und Trockenläufe gedacht, der Prompt verwendet es nicht.

**Der erste Kern-Fiktivtag (15.10.) kann starten.** Levi braucht dafür nur noch den Commit und Push laut Empfehlung (Abschnitt "Commit-/Push-Empfehlung 14.10.", Stand nach R5/R6) und die Cron-Einrichtung durch den Koordinator.
