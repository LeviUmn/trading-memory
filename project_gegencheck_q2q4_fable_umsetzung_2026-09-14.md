---
name: project_gegencheck_q2q4_fable_umsetzung_2026-09-14
description: "Opus-Gegencheck (Autor≠Prüfer) der Fable-Umsetzung von Levis Q2/Q4-Entscheidung vom 14.09.2026 (Option A: Q2 = |Entry − EMA50(5min)| / ATR skriptseitig aus --ema50-5min; Option Q4-a: Runway maschinell aus level_register.json; Nachtrag-Pflicht; Prüfkriterium vor Echtgeld). Alles live nachgerechnet und per CLI gegen die reale level_register.json getestet, nicht nur gelesen: Q2 stimmt (09.09. VC#1 = 2,13x NEIN; 11.09. VC#42 = 0,42x JA), die alte unerfüllbare Formel hat live KEINE Hintertür mehr (4 Missbrauchspfade geprüft, alle Exit 1 bzw. UNKNOWN), Q4 VC#42 = 0,35 gegen das echte Register bestätigt (3/4 GELB), Tests 70/70 selbst nachgefahren. KEIN Blocker. 5 Nachbesserungen: (N1) Q4 'kein Gegenlevel im Register' = ERFÜLLT wird auch dann vergeben, wenn die eigene Warnung belegt, dass das Register die Strecke gar nicht abdeckt — Datenlücke wird zum Persilschein und hebt 3/4 GELB auf 4/4 GRÜN (Levi-Entscheidung nötig, NICHT selbst geändert); (N2) --grund-runway-ratio-Zirkelprüfung ist durch Umschreiben umgehbar (tolerierbar, war so entschieden); (N3) --grund-ema50-5min koppelte stumm --dual-gate-q2-budget-pct zurück in die A3-Pflicht → Exit 1 am falschen Feld (VON OPUS GEFIXT: Hinweiszeile + Regeltext); (N4) Freigaben fielen ohne deklarierten testtag_modus STUMM aus dem Nenner des Prüfkriteriums (VON OPUS GEFIXT: Hinweiszeile + Regeltext); (N5) --batch-strikt rechnet Q4 historischer Trades gegen das HEUTIGE Register. Nach den beiden Opus-Fixes Tests 72/72 grün. Gesamturteil FREIGEGEBEN MIT AUFLAGEN (N1 vor dem nächsten Testtag entscheiden). Kein Commit/Push. RUNDE 2 (N1-Fix, 14.09.2026): Fables N1-Umsetzung live in 3 Faellen bestaetigt (UNKLAR bei Registerluecke, ERFUELLT bei belegter Abdeckung, Echtlauf-Zitat zeichengenau reproduziert), Testfaelle von Hand nachgerechnet, Tests 73/73 selbst gefahren. KEIN Blocker, aber N1b: Fable hatte N1 nur auf den Zweig "gar kein Gegenlevel" angewandt — ein einzelnes Level jenseits TP1 (Pivot R2/Fib, im realen Register immer vorhanden) hebelte es aus (identisches Setup: mit R2 4/4 GRUEN, ohne R2 3/4 GELB), N1 waere gegen ein normales Register totes Recht geblieben; die Runde-1-Empfehlung hatte diesen Fall woertlich mitgenannt. VON OPUS GEFIXT (gleiche Bedingung, keine neue Schwelle/kein neuer Faktor, "vor TP1" unberuehrt), dazu N1c (Override-Text) + Regeltext/loop_prompt nachgezogen, 4 neue Testfaelle, 2 Bestandstests auf register-konsistente 4/4-Geometrie umgestellt. Gesamturteil Runde 2: FREIGEGEBEN MIT AUFLAGEN (N1b bestaetigen; Register-Pflege ist jetzt sizing-relevant). Q2/Q4 testtagreif, fuer Echtgeld fehlen die Daten (0/15), nicht die Regeln."
metadata:
  node_type: memory
  type: project
  status: Runde 2 abgeschlossen 14.09.2026 — FREIGEGEBEN MIT AUFLAGEN, kein Blocker; Fables N1 korrekt, aber N1b von Opus nachgezogen (Zweig "jenseits TP1"); 4 Opus-Fixes gesamt, Tests 73/73, ungepusht (Levi committet)
  originSessionId: session_01E4zVYdeDA5XiMXhwykDWcw
  modified: 2026-09-14T10:51:06.046Z
---

## Kurzfazit

**FREIGEGEBEN MIT AUFLAGEN — kein Blocker.** Fables Umsetzung entspricht Levis Entscheidung vom 14.09.2026 ([[project_q2_kalibrierung_entscheidungsvorlage_2026-09-11]], Abschnitt „ENTSCHIEDEN"), ist rechnerisch korrekt und reproduziert den 11.09.-Kernfehler *für Q2 nicht mehr*: die alte, strukturell unerfüllbare Formel (Median 14,2× bei Schwelle 1,5×, [[project_testtag_analyse_2026-09-11]]) ist live über keinen Pfad mehr erreichbar. Der Bericht deckt sich in allen nachgeprüften Punkten mit dem Code — auch die von Fable selbst genannten offenen Punkte sind vollständig und ehrlich benannt.

Der 11.09.-**Muster**fehler („fehlende/unpassende Daten werden zum Persilschein") lebt allerdings an genau einer Stelle weiter, nur mit vertauschten Rollen: bei Q4 wird ein Register, das die Strecke zwischen Entry und TP1 nachweislich nicht abdeckt, mit **ERFÜLLT** belohnt statt mit UNKLAR (N1) — und das hebt den Score von 3/4 GELB auf 4/4 GRÜN, also von halber auf volle Position. Das ist keine Regelverletzung (Levis Entscheidung sagt zu diesem Fall nichts), sondern eine Designentscheidung, die Fable allein getroffen hat. Sie gehört vor dem nächsten Testtag entschieden.

Geprüft wurde **live, nicht nur gelesen**: Sandbox-Kopie aller `scripts/*.cjs` + Kopie der realen `scripts/level_register.json` (Zeitstempel aufgefrischt), echte CLI-Läufe inkl. des wörtlich kopierten 7b1-Schritt-5-Kommandotemplates, 4 Missbrauchsversuche gegen die Q2-Altformel, 5 Umgehungsversuche gegen die Q4-Zirkelprüfung, `protokoll_bilanz.cjs` gegen einen Tag MIT und einen Tag OHNE Fiktiv-Einträge. Kein Zugriff auf `trades.db`, kein Commit, kein Push, keine echten Logs beschrieben.

## 1. Q2 nachgerechnet (Punkt 1 der Prüfaufgabe) — OK

`q2Emaanker()` (`scripts/gate_check.cjs:1124`) rechnet `Math.abs(entry - ema) / atr`, `evaluateQScore()` hängt Q2 ausschließlich daran, Schwelle `Q2_ATR_MAX = 1.5` unverändert, Grenzfall inklusiv (`<= 1.5 + 1e-9`).

| Fall | Rechnung | Erwartet | Live-Lauf |
|---|---|---|---|
| 09.09. VC#1 (Short) | \|29.338,35 − 29.436,9\| = 98,55 / 46,2 | 2,13× → NEIN | **2,13× NEIN, Ampel bleibt ROT** ✓ |
| 11.09. VC#42 (Long) | \|29.423,65 − 29.412,2\| = 11,45 / 27,3 | 0,42× → JA | **0,42× JA** ✓ |

Damit ist Opus' Nachrechnung aus der Entscheidungsvorlage bestätigt: A hebt die korrekte 09.09.-Ablehnung nicht auf und öffnet den 11.09.-Trendtag. Die Pflicht-Ausgabezeile `Q2-Rohwert (Option A, 14.09.2026 — EMA50-Anker verbindlich)` erscheint auch bei UNKNOWN.

**Tests selbst gefahren:** `node --test tests/trading_scripts.test.js` → **70 tests / 70 pass / 0 fail** (vorher 60/60) — Fables Zahl bestätigt, nicht geglaubt.

## 2. Q2-Altparameter: keine Hintertür (Punkt 2) — OK

Vier Missbrauchspfade live durchgespielt:

| Aufruf | Ergebnis |
|---|---|
| `--impuls-reifegrad-atr-emaanker 0.42` **ohne** `--ema50-5min` | **Exit 1** mit Umstellungs-Hinweis |
| `--impuls-reifegrad-atr-emaanker 0.42` **mit** `--ema50-5min` (Kombination) | **Exit 1** (der Altparameter wird immer abgewiesen, nicht nur als Fallback) |
| `--ema50-5min` komplett weglassen | **Exit 1**, A3-Pflichtliste nennt `--ema50-5min` |
| `--grund-ema50-5min` + `--impuls-reifegrad-atr 16.3` (der 11.09.-Wert) | **Q2 UNKLAR**, die 16,3 erscheinen nur als `Impuls-Ursprung-Wert … (GEMESSEN, KEIN GATE)` |

Der Batch-Fallback ist **kein** Einfallstor für die kaputte Formel: `q2Emaanker()` greift im Batch nur auf `impulsReifegradAtrEmaanker` zurück — das ist der alte **EMA50**-Zweitanker (B3, seit 25.08.), also identische Formel. Das Feld `impulsReifegradAtr` (Impuls-Ursprung, der defekte Zähler) ist **nirgends** Q2-Quelle, auch nicht im Batch. Im Live-Zweig ist `opts.batch` nie `true` (`evaluateTrade(trade, { levelRegister })` an beiden Live-Aufrufstellen).

## 3. Q4 kritisch geprüft (Punkt 3)

### 3a. Echtlauf gegen das reale Register — bestätigt
VC#42 (Long 29.423,65 → TP1 29.500) gegen eine Kopie der echten `level_register.json`:
Kandidaten über dem Entry: **29.450** (Rundzahl 50er, 26,35 Pkt), 29.480,10 (Session-Hoch), 29.482,80 (PDH) — 29.500 als TP1-Level ±2 ausgenommen. TP1-Distanz 76,35 Pkt → **0,345 → 0,35, 3 Register-Level zwischen Entry und TP1** — exakt Fables Zahl, Ausgabezeile und `3/4 GELB (halbe Position)` reproduziert.

### 3b. „Kein Gegenlevel = ERFÜLLT" — **N1, Nachbesserung (wichtig), NICHT selbst geändert**
Der Fall ist live nachgestellt (Register auf Level unterhalb des Entrys gestutzt):

```
[Q-Faktor Q4 = JA] Runway n/a — KEIN Gegenlevel im Register in Trade-Richtung LONG … -> ERFUELLT mit Vermerk
WARNUNG Q4-Runway (kein Gate): arithmetische 50er-Rundzahl(en) 29450 zwischen Entry und TP1 stehen NICHT im Register …
```

Das Skript **weiß im selben Lauf**, dass das Register die Strecke nicht abdeckt — und vergibt trotzdem das bestmögliche Q4-Ergebnis. Wirkung: bei Q1/Q2/Q3 ✓ wird aus **3/4 GELB (halbe Position) ein 4/4 GRÜN (volle Position)** — eine Risikoerhöhung aus einer Datenlücke heraus. Genau dieses Muster (unvollständiges/unpassendes Datum wird zum Freibrief) war der 11.09.-Kernbefund, dort bei Q2.

Mildernd: (a) die Warnung ist laut und steht direkt daneben; (b) praktisch tritt „gar kein Gegenlevel" bei NAS100 selten auf, weil Pivots R2/S2 und die Fib-Extensions im Register weit außerhalb liegen; (c) `loadLevelRegister()` bricht ohnehin bei Register > 90 Min ab.

**Empfehlung (keine neue Schwelle, kein neuer Faktor):** Q4 = **UNKLAR** statt ERFÜLLT, wenn `lage === 'keins'` (oder das nächste Level erst jenseits TP1 liegt) **und** gleichzeitig die Warnung „arithmetische 50er … NICHT im Register" feuert. Ein-Zeilen-Änderung in `evaluateQScore()`. Nicht selbst gemacht: Levis Entscheidung sagt zum Fall „kein Level" nichts, Fables „ERFÜLLT mit Vermerk" ist eine eigene Setzung, und die Änderung wäre eine echte Regelsemantik-Entscheidung.

### 3c. Entry-Toleranz ±2 Pkt (Fable-Zusatz) — Info/OK
Die ±2 sind **keine neue Schwelle**, sondern die bestehende Projektkonstante `LEVEL_REGISTER_TOLERANCE = 2.0` (`scripts/register_constants.cjs:23`), dieselbe, mit der 8b1/8b2 TP-Level gegen das Register verifizieren, und dieselbe, die Levis Entscheidung für den TP1-Ausschluss bereits vorsieht.
**Effekt ohne die Toleranz:** Jeder Entry, der auf einem Registerlevel genommen wird (`--entry-source "Registerlevel …"` — der Regelfall), fände sein eigenes Basislevel 0–2 Pkt entfernt wieder → Ratio ≈ 0 → Q4 **immer** NEIN. Das wäre ein reines Rundungsartefakt. Die Toleranz ist fachlich richtig und symmetrisch zur TP1-Seite; ich lasse sie stehen.

### 3d. Rundzahlen nur aus dem Register — Info/OK, kein Bruch
`evaluateTrade()` führt beide Sichten nebeneinander: der **Register**-Abgleich (`matchLevelRegister`, 8b1/8b2 TP-Verifikation) und die **arithmetische** 50er-Liste als Diagnose (`Rundzahlen (50er, arithmetisch, unabhängig vom Register) im Fenster:`). Q4 hängt an der Register-Sicht — konsistent mit 8b1, wo ein TP1-Preis ohne Register-Eintrag ebenfalls nicht als Level zählt (→ UNKLAR). Die Lücke zwischen beiden Sichten wird durch die neue Warnung überbrückt. Kein Bruch.

### 3e. Was Q4-a faktisch bewirkt — Info (für die Kalibrierung)
Nachgerechnet über die 12 Fiktiv-Zeilen vom 11.09. gegen das reale Register: **10 von 12 Q4 NEIN, 2 ERFÜLLT** (18:36 und 18:51, beide mit TP1-Distanz < 50 Pkt). Über alle 51 Live-Gate-Aufrufe im Log mit Entry/TP1: 40 NEIN / 11 ERFÜLLT.
Grund: das Register führt regelkonform ein lückenloses **50er-Band** (Schritt-6-Regel Nr. 7). Sobald die TP1-Distanz > ~54 Pkt ist — bei 8c (SL ≥ 1,5× ATR) und RR ≥ 1 der Normalfall — liegt **zwangsläufig** eine 50er dazwischen. Q4 ist damit de facto eine Binärprüfung „TP1-Distanz klein genug, dass keine 50er dazwischenliegt" und die Ratio-Zahl selbst dekorativ. Das ist **nicht** der 11.09.-Fehler (Q4 bleibt mit ~17–22 % erfüllbar und differenziert), aber es heißt: **4/4 GRÜN wird zur Ausnahme, GELB/halbe Position zum Regelfall.** Das ist eine Folge, die Levi bewusst zur Kenntnis nehmen sollte — kein Einwand gegen die Umsetzung, aber ein Kalibrierungspunkt für die Auswertung nach 15 Momenten.

## 4. Zirkel-Check `--grund-runway-ratio` (Punkt 4) — N2, tolerierbar

`RUNWAY_GRUND_ZIRKEL_RE = /gereift|reifegrad/i`. Fünf Varianten live getestet:

| Begründung | Ergebnis |
|---|---|
| „Impuls bereits 16,5x ATR gereift" | Exit 1 ✓ |
| „REIFEGRAD zu hoch" (Großschreibung) | Exit 1 ✓ |
| „Gereiftheit" (Wortvariante) | Exit 1 ✓ |
| „der Impuls ist **weit fortgeschritten**, kein belastbares Runway-Ziel" | **läuft durch**, Q4 = JA per Override |
| „Bewegung schon sehr ausgedehnt" | **läuft durch** |

Die Textprüfung ist also durch bloßes Umschreiben umgehbar. **Kein Blocker:** genau diese Prüfung („Textprüfung auf ‚gereift'/‚Reifegrad' → Exit 1") steht wörtlich in Levis Entscheidung, Fable hat sie erfüllt; und der Override bleibt vollständig sichtbar (`OVERRIDE --runway-ratio, Grund: …` **plus** der gerechnete Wert daneben) und wandert mit `runway_override` / `runway_override_grund` ins Fiktiv-Log. Eine härtere Prüfung (z.B. „Grund muss einen Preis nennen") wäre eine neue Regel und verstößt gegen Levis Rahmenvorgabe „Regelwerk nicht aufblähen". **Empfehlung: so lassen, beim Tagesabschluss die `runway_override`-Zeilen mitlesen.**

## 5. Nachtrag-Pflicht (Punkt 5) — OK

- Tag **mit** offenen Einträgen (Datum 11.09., 12 offene): ABWEICHUNG + Exit 1, mit fertiger `--nachtrag`-Kommandozeile je Eintrag inkl. Richtung/Entry/SL/TP1 und Ablehnungsgrund. ✓
- Tag **ohne** Fiktiv-Einträge (09.09.): Zeile „0 Setup-Moment(e) … ohne Entscheidung/hypothetisches Ergebnis: 0", **keine** Fiktiv-Abweichung — kein Fehlalarm. ✓ (Exit 1 kam dort aus sechs anderen, älteren Protokoll-Abweichungen.)
- `--nur-zaehlung` schließt die Fiktiv-Abweichung korrekt aus (8 statt 9 Abweichungen). ✓
- `skipped_fiktiv.cjs --nachtrag --sl-hit-min` vorhanden, negativer Wert = Exit 1. `--list` zählt jetzt nach beiden Feldern.

Kleinigkeit (kosmetisch, nicht geändert): die „fertige" Kommandozeile enthält den Platzhalter `--entscheidung ausgelassen|eingestiegen` — in Bash ist das eine Pipe, also nicht 1:1 einfügbar. Harmlos (bricht sofort ab), aber „fertig" ist zu viel gesagt.

## 6. Prüfkriterium / `--auswertung-q2a` (Punkt 6) — OK mit Hinweis

- Momente-Dedupe `datum|dir|entry|sl|tp1` ist **konservativer** als die V2-Logger-Dedupe (die nur innerhalb von 5 Min zusammenfasst): dasselbe Setup um 18:21 und um 19:06 wird hier zu 1 Moment, im Log sind es 2 Zeilen. Richtung stimmt (lieber zu wenige als zu viele „unabhängige" Momente), und die Zeile mit Nachtrag gewinnt bei der Auswahl. Kein Doppelzählen.
- Kriterium (b) ist korrekt kodiert: `schnell * 3 <= frei.length` ⇔ höchstens 1 von 3.
- Fehlen Nachträge, liefert die Auswertung **„nicht auswertbar"** statt einer geschönten Zahl. Gut.
- **Nachrechnung mit den 09.09.-Daten nicht möglich** (Prüfaufgabe Punkt 6): `skipped_setups_fiktiv.jsonl` enthält **keine** 09.09.-Zeilen — der Auto-Logger (TODO 5) entstand erst am Abend des 09.09. Die vier bekannten Q-ROT-Ablehnungen von 09.09. sind nie ins Log gelaufen. Der Live-Lauf auf dem echten Log liefert folgerichtig `0 (aus 0 Zeile(n); 12 Zeile(n) vor Option A / ohne q2_anker nicht gezählt)` → „noch zu wenig Daten (0/15)". Korrektes Verhalten, aber praktisch heißt es: **die Datenbasis für das Echtgeld-Prüfkriterium beginnt bei null** und braucht realistisch zwei bis drei volle Testtage.
- Die SL-Hit-Erkennung hängt an `/SL-HIT/i` im Freitext bzw. `ergebnis_r === -1`. Funktioniert mit dem selbst gerechneten Text, ist bei manuell getipptem `--hypothetisches-ergebnis` aber fragil. Info, kein Befund.

## 7. Freigabe-Logging (Punkt 7) — **N4, von Opus gefixt**

Live nachgestellt (PASS + 3/4 GELB, drei State-Varianten):

| `vollcheck_state.json` | Verhalten **vorher** |
|---|---|
| `testtag_modus: fiktiv` (heutiges DE-Datum) | Eintrag geschrieben, `quelle: … FREIGEGEBEN`, `ablehnungsgrund: null` ✓ |
| `testtag_modus: echt` | kein Eintrag (gewollt — Freigaben sind dort Trades) ✓ |
| **State fehlt / Datum von gestern / kein Flag** | **kein Eintrag, keine Meldung, völlig stumm** ✗ |

Damit hing der Nenner des ganzen Prüfkriteriums an einem Flag, dessen Fehlen nirgends auffällt. Die V4-Logik vom 11.09. ist genau umgekehrt gebaut (fail-safe: undeklarierter Modus **blockt** in `add_skipped_setup.cjs`); hier war es fail-silent. Seit Runde 2 injiziert `loop_prompt.cjs` `--testtag` in jeden `vollcheck.cjs`-Aufruf, das Risiko ist also klein — aber ein Gate-Check außerhalb des Loops, ein über Mitternacht laufender State oder eine abweichende `--vollcheck-state`-Datei genügen.

**Opus-Fix (rein additiv, kein Gate, kein Sizing, kein Exit-Einfluss):** neue Zeile
`HINWEIS FREIGABE NICHT GELOGGT (Option A, 7b1a-Pruefkriterium): … traegt fuer heute KEINEN testtag_modus (fiktiv|echt) — die Zeile fehlt damit im Nenner.`
Sie erscheint **nur** bei PASS + GELB/GRÜN und **nur**, wenn der Modus gar nicht deklariert ist (bei `echt` bleibt es still, dort ist das Nichtloggen gewollt). Regeltext in [[feedback_live_trading]] 7b1a (Prüfkriterium-Absatz) um einen Satz ergänzt.

## 8. Kommandotemplate wörtlich ausgeführt (Punkt 8) — OK

Das 7b1-Schritt-5-Template aus [[feedback_live_trading]] wurde wörtlich kopiert, mit den VC#42-Zahlen befüllt und in der Sandbox ausgeführt (inkl. `> scripts/last_gate_check.txt 2>&1; echo "Exit-Code: $?"`):

```
Exit-Code: 0
--> GESAMTSTATUS: PASS  [Sizing-Flag: halbe Position]
--> Q-SCORE: 3/4 GELB (halbe Position)  [offen: Q4 (nicht erfuellt)]
```

Der 31.08.2026-Fehler (Template bricht wörtlich kopiert mit Exit 1 ab) wiederholt sich **nicht**. Zwei Randbeobachtungen: `--sl-vorpruefung-ref last` setzt voraus, dass Schritt 0 vorher gelaufen ist (sonst Exit 1 — korrekt und gewollt), und ein von Hand eingetragener `--dual-gate-q2-budget-pct` erzeugt jetzt eine Konsistenzwarnung, weil die Formel auf dem neuen Q2-Wert steht (ebenfalls gewollt; der Wert wird bei Weglassen selbst gerechnet). Beide Templates (Schritt 5 und die `Gate-Check ausgeführt:`-Zeile) tragen identische Parametersätze — 38 gemeinsame Parameter, keine Abweichung, kein Altparameter im Kommandoteil.

### N3 — von Opus gefixt: `--grund-ema50-5min` sprengte den Lauf am **falschen** Feld
Der dokumentierte Ausweg allein genügt nicht: weil `--dual-gate-q2-budget-pct` sich aus `(1,5 − Q2)/1,5` selbst rechnet, fällt es ohne Q2-Wert zurück in die A3-Pflichtliste. Live:

```
node … --grund-ema50-5min "EMA nicht ablesbar"   →   Exit 1
Fehlende Pflicht-Messfelder (ohne begleitendes --grund-<feld>): --dual-gate-q2-budget-pct
```

Der Loop hätte an einem Tag mit nicht ablesbarer EMA50 den dokumentierten Ausweg genommen und wäre trotzdem ohne zitierbaren GESAMTSTATUS dagestanden — dieselbe Klasse wie der 31.08.-Templatefehler. **Opus-Fix:** Hinweiszeile `HINWEIS Q2-BUDGET-KOPPLUNG (A3, Option A 14.09.2026): … --dual-gate-q2-budget-pct … ist wieder A3-Pflichtfeld: Wert uebergeben oder --grund-dual-gate-q2-budget-pct "<Begruendung>" mitgeben, sonst Hard-Exit 1.` **Bewusst kein Auto-Ausweg** — A3 heißt: eine Ausnahme wird *deklariert*, nie vergeben. Regeltext in [[feedback_live_trading]] 7b1a (Q2-Bullet) um einen Satz ergänzt.

## 9. Regelwerk-Kohärenz (Punkt 9) — OK

Keine doppelte oder widersprüchliche Q2-Definition gefunden:
- [[feedback_live_trading]] 7b1a: neue Q2-/Q4-Bullets, alte Fassung als eingeklammerte Historie **ersetzt**, nicht danebengestellt. `--runway-ratio` in der Herkunfts-Liste durchgestrichen und ersetzt.
- [[feedback_chartanalyse]]: der Absatz „Impuls-Reifegrad in ATR" beschreibt weiter das **Protokollfeld** (was korrekt ist) und trägt direkt darunter den Satz „Stand 14.09.2026 … der Q2-Faktor hängt seitdem NICHT mehr an diesem Feld". Sauber.
- `gate_check.cjs`-Kopfkommentar, `--ema50-5min`-Hilfetext, `MEASUREMENT_REQUIRED`, Ausgabezeilen und Fehlertexte nennen durchgehend dieselbe Formel und dasselbe Datum.
- `vollcheck.cjs` reicht (wie behauptet) keine Q-Parameter an `gate_check.cjs` durch — nur `--sl-vorpruefung`; sein `--impuls-pkt` dient Fib/Chartmuster und ist jetzt vom Q-Score entkoppelt (genau die Verbindung, über die am 11.09. die Tagesamplitude in Q2 geriet). Kein anderes Skript im Repo kennt `--impuls-reifegrad-atr-emaanker` mehr.

## 10. Levis Rahmenvorgabe „nicht aufblähen" (Punkt 10) — eingehalten

Ersetzt statt angehängt: ein Pflichtparameter raus (`--impuls-reifegrad-atr` aus `MEASUREMENT_REQUIRED`), einer rein (`--ema50-5min`) — Nettozahl der A3-Pflichtfelder unverändert. `--runway-ratio` verliert seine Pflichtrolle und wird Override. Keine neue Schwelle, kein neuer Faktor, Ampel-Logik unangetastet. Einzige echte Vermehrung: die siebte Pflicht-Abschlusszeile im Tagesabschluss (`Fiktiv-Log nachgetragen`) — das war aber Punkt 3 von Levis Entscheidung, und sie wird maschinell erzwungen statt per Merkregel.

## 11. Weitere Befunde (Info)

- **N5 — `--batch-strikt` und Q4:** Der Strikt-Batch lädt das **heutige** `level_register.json` und rechnet damit Q4 für *historische* Trades. Für die Machbarkeitsmessung (#8) ist das ein Anachronismus; im Banner steht nur „STRIKT (Live-Logik: Register + …)" ohne Vorbehalt. Nicht handlungskritisch, aber jede künftige Q4-Auswertung aus `--batch-strikt` ist nicht historisch belastbar.
- **Register wird live immer geladen:** Bestätigt (vorher nur bei `--tp1/--tp2-level-price`). Ein Live-Aufruf mit fehlendem/älter-als-90-Min-Register bricht jetzt auch ohne TP-Level-Parameter mit Exit 1 ab. Konservativ und konsistent (ohne verifiziertes TP1-Level war PASS ohnehin unerreichbar), akzeptiert.
- **Wick-Zonen (8c2)** haben kein `price` und fallen damit aus der Q4-Gegenlevel-Suche — eine per Wick getestete Zone ist aber ein starkes Gegenlevel. Macht Q4 tendenziell optimistischer. Nur zur Kenntnis, keine Empfehlung (die Zonen sind Intervalle, keine Punkte; eine Umstellung wäre eine eigene Entscheidung).
- **GATE_BASE in den Tests** setzt `--ema50-5min` per Default auf den Entry (Q2 = 0,00× → immer ERFÜLLT). Für die Bestandstests unkritisch (sie prüfen andere Gates), die Q2-Tests setzen eigene Werte. Nur zur Kenntnis.
- Fables Selbstauskunft deckte sich in **allen** stichprobenartig nachgeprüften Punkten mit dem Code; die sechs von ihm selbst als offen benannten Punkte sind vollständig — das macht N1/N4 nicht weniger relevant, spricht aber für die Verlässlichkeit des Berichts.

## Befundliste

| # | Klasse | Befund | Status |
|---|---|---|---|
| — | **Blocker** | keiner | — |
| **N1** | Nachbesserung (wichtig) | Q4 „kein Gegenlevel im Register" = **ERFÜLLT**, obwohl die eigene Warnung im selben Lauf belegt, dass das Register die Strecke nicht abdeckt → Datenlücke hebt 3/4 GELB auf 4/4 GRÜN (volle Position). 11.09.-Muster mit vertauschten Rollen. | **offen — Levi-Entscheidung** (Empfehlung: UNKLAR statt ERFÜLLT, wenn die 50er-Warnung feuert) |
| **N2** | Nachbesserung (tolerierbar) | Zirkelprüfung `--grund-runway-ratio` per Umschreibung umgehbar („weit fortgeschritten") | **so lassen** (war genau so entschieden; Override bleibt sichtbar + geloggt) |
| **N3** | Nachbesserung | `--grund-ema50-5min` koppelte stumm `--dual-gate-q2-budget-pct` zurück in die A3-Pflicht → Exit 1 am falschen Feld | **von Opus gefixt** (Hinweiszeile + Regeltext, kein Auto-Ausweg) |
| **N4** | Nachbesserung | Freigaben fielen ohne deklarierten `testtag_modus` **stumm** aus dem Nenner des Prüfkriteriums | **von Opus gefixt** (Hinweiszeile + Regeltext) |
| **N5** | Info | `--batch-strikt` rechnet Q4 historischer Trades gegen das heutige Register | benannt |
| **I1** | Info | Q4-a ist am 50er-Band faktisch binär → 4/4 GRÜN wird selten (11.09.: 2/12; Gate-Log gesamt: 11/51) | Kalibrierungspunkt für die Auswertung |
| **I2** | Info | Datenbasis des Prüfkriteriums startet bei 0/15 (keine 09.09.-Zeilen im Fiktiv-Log) | 2–3 Testtage einplanen |
| **I3** | Info | `--entscheidung ausgelassen\|eingestiegen` in der „fertigen" Nachtrag-Kommandozeile ist in Bash eine Pipe | kosmetisch |

## Änderungen durch Opus (klein, additiv, rückbaubar)

1. `scripts/gate_check.cjs` — `HINWEIS Q2-BUDGET-KOPPLUNG (A3, Option A 14.09.2026)` im Q2-Budget-Block (N3).
2. `scripts/gate_check.cjs` — `HINWEIS FREIGABE NICHT GELOGGT (Option A, 7b1a-Pruefkriterium)` vor dem Fiktiv-Log-Block (N4).
3. `tests/trading_scripts.test.js` — zwei neue Tests („Opus-Gegencheck: …") mit je vier Fällen.
4. [[feedback_live_trading]] 7b1a — je ein Satz im Q2-Bullet (Budget-Kopplung) und im Prüfkriterium-Absatz (`testtag_modus` als Voraussetzung).

Beide Codeänderungen sind **reine Ausgabezeilen**: kein Gate, kein Sizing, kein Exit-Code, kein Log-Feld, keine Schwelle. Nichts an `trades.db`, kein Commit, kein Push.

**Tests: vorher 70/70 (Fables Stand, selbst nachgefahren) → nachher 72/72 grün.**

## Gesamturteil

**FREIGEGEBEN MIT AUFLAGEN.** Committierbar wie es ist (inkl. der beiden Opus-Zeilen). Auflage: **N1 vor dem nächsten Testtag entscheiden** — soll ein Register, das die Strecke Entry→TP1 nachweislich nicht abdeckt, Q4 auf ERFÜLLT (volle Position) heben, oder auf UNKLAR (halbe Position) fallen? Zweite, weichere Auflage: I1 im Blick behalten — wenn nach 15 Momenten praktisch nie ein 4/4 GRÜN entsteht, ist Q4-a am 50er-Band zu eng geeicht und gehört zusammen mit der Q2-Schwelle in dieselbe Auswertung.

Vorgänger: [[project_q2_kalibrierung_entscheidungsvorlage_2026-09-11]] (Entscheidung), [[project_testtag_analyse_2026-09-11]] (Befund). Formatvorbild: [[project_gegencheck_fable_umsetzung_2026-09-09_f1_b1_b2]].

## Levi-Entscheidung 14.09.2026 zu N1

Levi: "Ja bitte so freigeben und an Fable geben zum umsetzen. Danach von Opus wieder gegenprüfen lassen." N1 wird wie vom Prüfer vorgeschlagen umgesetzt: Q4 "kein Gegenlevel im Register" liefert UNKLAR (nicht ERFÜLLT), wenn die Registerlücken-Warnung (fehlende arithmetische 50er-Abdeckung Entry→TP1) feuert. Kein neuer Faktor, keine neue Schwelle. Fable-Umsetzung beauftragt, danach erneuter unabhängiger Opus-Gegencheck (frischer Agent).

## N1 umgesetzt 14.09.2026 (Fable, nach Levi-Entscheidung; erneuter Opus-Gegencheck ausstehend)

**Datei/Funktion:** `scripts/gate_check.cjs` — `runwayFromRegister()` (Zweig `!naechst`) liefert jetzt `registerLuecke: true` + `rzFehlend: [...]`, wenn kein Gegenlevel gefunden wurde UND die bestehende 50er-Warnung (arithmetische 50er zwischen Entry und TP1 ohne Registereintrag) feuert; `evaluateQScore()` setzt Q4 in diesem Fall auf **UNKNOWN** statt ERFUELLT (Ampel-Logik unverändert, UNKNOWN zählt in die bestehende UNBEKANNT-/`[offen: Q4 (unklar)]`-Mechanik). Belegte Abwesenheit (kein 50er auf der Strecke fehlt) bleibt ERFUELLT mit Vermerk, Detailtext ergänzt um „50er-Abdeckung Entry->TP1 im Register geprueft". Die Zweige „Level vor TP1" (NICHT ERFUELLT, VC#9 0,48 / VC#42 0,35) und „nächstes jenseits TP1" (ERFUELLT + Warnung) sind unverändert — N1 wörtlich nur auf den „kein Gegenlevel"-Zweig angewandt. `evaluateTrade()` reicht beide Felder im `runway`-Objekt durch (Override-Pfad unberührt: mit gültigem Override greift weiterhin `effektiv`).

**Refactor vs. Duplikat:** weder noch — die 50er-Abdeckungsprüfung lebt seit der Q4-a-Umsetzung bereits *in* `runwayFromRegister()` (`rzFehlend`-Schleife, dieselbe, die die WARNUNG-Zeile speist). Die Rundzahl-Zeile im TP1-Fenster-Block von `evaluateTrade()` prüft ein anderes Intervall (zulässiges TP1-Fenster [RR≥1-Grenze; Zone-3-Grenze], nicht Entry→TP1) und ist keine Doppelimplementierung. Damit ist der UNKNOWN-Fall garantiert an dieselbe Bedingung gekoppelt wie die Warnung (keine Divergenz möglich).

**Ausgabe/Log:** `[Q-Faktor Q4 = UNKLAR] Runway (>=1,0): Runway n/a — KEIN Gegenlevel … UND keine 50er-Rundzahl-Abdeckung Entry->TP1 (29750 / 29800 fehlen im Register) — Register deckt die Strecke Entry->TP1 nicht ab, Runway nicht belastbar beurteilbar -> UNKLAR statt ERFUELLT (N1, 14.09.2026)` (Format wie Q1/Q3-UNKLAR: `[Q-Faktor X = UNKLAR] Label: … nicht pruefbar/beurteilbar`). Fiktiv-Log: `runway_gegenlevel` bleibt bei belegter Abwesenheit `"keins"`, im N1-Fall `"keins (Register-Luecke)"`; neues Feld `runway_register_luecke` (Liste der fehlenden 50er, sonst null); `q_faktoren.Q4.status` = UNKNOWN. Kein bestehender Feldname geändert (`--auswertung-q2a` liest `q2_anker`/`q_ampel`/`ergebnis_r`/`sl_hit_min`, nicht `runway_*`). Kopfkommentar (Konstantenblock Q4) und [[feedback_live_trading]] 7b1a Q4-Bullet (ein Satz) nachgezogen; die beiden Opus-Hinweiszeilen/-Tests unangetastet.

**Tests:** bestehender Q4-a-Test: Fall (c) von „gar keins → ERFUELLT" auf „gar keins + 50er-Lücke → UNKLAR, 3/4 GELB" umgestellt (Verhalten hat sich per Levi-Entscheidung geändert), neuer Fall (d) belegte Abwesenheit (Long 29.755 → TP1 = Rundzahl 29.800 im Register, ATR 25: kein 50er fehlt → ERFUELLT, 4/4 GRÜN), Fall (e) Regression „jenseits TP1 + Warnung bleibt ERFUELLT" (VC#42 gegen Test-Register 1,54); Fall (a) VC#9 0,48 NICHT ERFUELLT unverändert. Neuer Test „N1 im Fiktiv-Log" prüft die Logfelder beider Fälle. **72/72 → 73/73 grün.** Echtlauf in Sandbox-Kopie (Register nur PP/PDH/29.600/Measured Move 29.820, Long 29.740 → 29.820, Q1-Q3 JA): vorher 4/4 GRÜN, jetzt `--> Q-SCORE: 3/4 GELB (halbe Position)  [offen: Q4 (unklar)]`.

**Offen für den Opus-Gegencheck:** (1) Zweig „nächstes Level erst jenseits TP1" + 50er-Warnung bleibt ERFUELLT (Opus-Empfehlung nannte diesen Fall in Klammern, Levis Auftrag beschränkt N1 wörtlich auf „kein Gegenlevel gefunden"); (2) ein Override `--runway-ratio` + gültiger Grund übersteuert auch den N1-UNKNOWN (wie bisher jede Rechnung); (3) Batch-strikt erbt N1 automatisch (gleiche Funktion), lenient-Batch ohne Register unverändert.

## Opus-Gegencheck Runde 2 (N1-Fix), 14.09.2026

**Kurzfazit: FREIGEGEBEN MIT AUFLAGEN — kein Blocker, aber eine wichtige Nachbesserung (N1b), die Opus selbst umgesetzt hat.** Fables N1-Umsetzung ist korrekt, sein Bericht deckt sich in jedem nachgeprüften Punkt mit dem Code, und alle drei geforderten Fälle laufen live so, wie er sie beschreibt. **Aber:** N1 wurde nur auf den Zweig „gar kein Gegenlevel" angewandt — und dieser Zweig ist gegen ein realistisch gefülltes Register praktisch unerreichbar. Der häufige Fall („nächstes Gegenlevel erst jenseits TP1") blieb ERFÜLLT und hätte N1 zu weitgehend totem Code gemacht. Das ist live nachgewiesen und behoben.

Geprüft wurde wieder **live, nicht nur gelesen**: Sandbox-Kopie aller `scripts/*.cjs`, sechs selbst gebaute Register-/Geometrie-Fälle, dazu ein Lauf gegen eine Kopie der **echten** `level_register.json` (Zeitstempel aufgefrischt). Kein Zugriff auf `trades.db`, keine echten Logs beschrieben, kein Commit, kein Push.

### 1. N1-Logik live nachgerechnet (Prüfaufgabe 1) — bestätigt

| Fall | Register | Geometrie | Ergebnis |
|---|---|---|---|
| (a) kein Gegenlevel **+ Lücke** | PP 29205,83 / PDH 29482,8 / 29600 / MM 29820 | Long 29.740 → TP1 29.820, ATR 40 | **Q4 UNKLAR**, Text wörtlich wie von Fable zitiert (29750 / 29800 fehlen), `3/4 GELB (halbe Position) [offen: Q4 (unklar)]` ✓ |
| (b) kein Gegenlevel **+ Abdeckung belegt** | nur 29800 (= TP1) | Long 29.755 → TP1 29.800, ATR 25 | **Q4 JA**, „ERFUELLT mit Vermerk … 50er-Abdeckung Entry->TP1 im Register geprueft", keine Warnung, **4/4 GRÜN** ✓ |
| (c) Fables Echtlauf-Zitat | wie (a) | wie (a) | **zeichengenau reproduziert** (inkl. `-> UNKLAR statt ERFUELLT (N1, 14.09.2026)`) ✓ |

Die Rechnung stimmt auch von Hand: bei (a) ist der erste 50er-Kandidat 29.750, danach 29.800, beide mehr als 2 Pkt von Entry/TP1 entfernt und nicht im Register → fehlende 50er = [29750, 29800]; der einzige Level-Kandidat 29.820 fällt als TP1-Level ±2 heraus → „kein Gegenlevel" + Registerlücke → UNKNOWN. Bei (b) ist der einzige 50er auf der Strecke die TP1 selbst und wird übersprungen → keine Lücke → ERFÜLLT. Beides deckt sich mit den Testfällen (c)/(d).

**Regression gegen das reale Register:** VC#42 (Long 29.423,65 → TP1 29.500) liefert unverändert `Runway-Ratio 0.35 … Rundzahl 50er @ 29450 (26.35 Pkt, vor TP1, 3 Register-Level zwischen Entry und TP1)` **ohne** Lücken-Warnung. Bei gepflegtem 50er-Band feuert N1 also nie — genau richtig.

### 2. Ist die Lücken-Erkennung dieselbe Bedingung wie die TP1-Fenster-Warnung? (Prüfaufgabe 2) — Fable hat recht, Info/OK

Nein, und das ist korrekt so. Die 50er-Schleife in `runwayFromRegister()` läuft über **Entry→TP1** und meldet *jede* fehlende 50er. Die Rundzahl-Zeile im TP1-Fenster-Block (`gate_check.cjs` ~Z. 2130) läuft über das **zulässige TP1-Fenster** [RR≥1-Grenze; Zone-3-Grenze], filtert zusätzlich auf generierte Level und meldet nur, wenn **keine einzige** davon im Register steht. Zwei Intervalle, zwei Prädikate, ein Zweck je Zeile — **keine Doppelimplementierung**, Fables Darstellung stimmt.

Divergenz ist möglich und im Testfall (b)/VC#42 sogar sichtbar: die TP1-Fenster-Zeile meldet 29.500 als im Register vorhanden (keine Warnung), während die Q4-Zeile das fehlende 29.450 anmahnt. Das ist **tolerierbar, kein Blocker**: N1 hängt ausschließlich an derselben Liste, die die direkt danebenstehende `WARNUNG Q4-Runway`-Zeile speist — Urteil und Begründung können innerhalb von Q4 nicht auseinanderlaufen. Die TP1-Fenster-Zeile beantwortet eine andere Frage (TP1-Kandidatensuche) und hat keine Sizing-Wirkung.

### 3. Fables offener Punkt (a): „jenseits TP1" bleibt ERFÜLLT — **N1b, Nachbesserung (wichtig), VON OPUS GEFIXT**

**Befund (live erzwungen):** Identische Geometrie wie Fall (a) oben, identische Registerlücke (29.750/29.800 fehlen) — nur **ein einziges weit entferntes Level zusätzlich** (Pivot R2 @ 29.900):

```
vorher:  [Q-Faktor Q4 = JA] Runway-Ratio 2 = … Pivot R2 @ 29900 (160 Pkt, jenseits TP1, kein Register-Level zwischen Entry und TP1)
         --> Q-SCORE: 4/4 GRÜN (volle Position)
```

Dasselbe Setup war ohne R2 `3/4 GELB`. Ein beliebiges Level weit jenseits von TP1 hebelt N1 also vollständig aus. Das ist kein Randfall, sondern der **Regelfall**: die reale `level_register.json` führt Pivot R2 29.669,93 und Fib-Extensions 29.916/30.075 über und S2 28.741,73 unter dem Kursband — für praktisch jeden NAS100-Trade existiert ein Level jenseits von TP1. Der Zweig „gar kein Gegenlevel", auf den Fable N1 beschränkt hat, ist damit gegen ein normal gefülltes Register **kaum je erreichbar** — N1 wäre weitgehend totes Recht geblieben.

Verschärfend: bei „jenseits TP1" entsteht die Ratio ≥ 1 **genau deshalb**, weil das Level dazwischen im Register fehlt. Der Testfall (b) beweist das an echten Zahlen: dieselbe VC#42-Geometrie ergibt gegen das Test-Register (ohne 29.450) `1,54 → ERFÜLLT`, gegen das **reale** Register (mit 29.450) `0,35 → NICHT ERFÜLLT`. Die Datenlücke dreht das Ergebnis um zwei Stufen — vom Ausschlusskriterium zur vollen Position. Das ist dasselbe 11.09.-Muster („fehlende Daten = Persilschein"), gegen das N1 gerichtet war.

**Einschätzung:** Die Beschränkung auf „kein Gegenlevel" folgt dem Wortlaut des Zusammenfassungsabsatzes, aber nicht dem Beschluss: Levis Freigabe („Ja bitte so freigeben und an Fable geben zum umsetzen") bezog sich auf die Runde-1-Empfehlung, und die lautete wörtlich *„wenn `lage === 'keins'` **(oder das nächste Level erst jenseits TP1 liegt)** und gleichzeitig die Warnung feuert"*. Der Fall war also von Anfang an mitgemeint. Fachlich ist er der wichtigere der beiden.

**Opus-Fix (klein, dieselbe Bedingung, keine neue Schwelle, kein neuer Faktor, rückbaubar):**
- `runwayFromRegister()`, Fund-Zweig: Registerlücke gilt jetzt auch dort, aber **nur** wenn kein Level vor TP1 liegt („vor TP1" bleibt unberührt — dort ist das Ergebnis ohnehin NICHT ERFÜLLT und ein weiteres fehlendes Level ändert daran nichts). Textzusatz: `— ABER keine 50er-Rundzahl-Abdeckung Entry->TP1 (… fehlen im Register): die Ratio >=1 entsteht nur mangels Registereintrag auf der Strecke … -> UNKLAR statt ERFUELLT (N1, 14.09.2026)`.
- `evaluateQScore()`: ein vorgezogener Zweig (Registerlücke UND kein Override) → UNKNOWN. Damit hängen beide N1-Fälle (Runde 1 + Runde 2) an **einer** Stelle; der alte Ternär im „keins"-Zweig ist entfallen (nicht mehr erreichbar).
- Die gerechnete Zahl bleibt sichtbar und geloggt (`runway_ratio_gerechnet`, `runway_register_luecke`), nur der Status ist UNKNOWN.

Nachher, derselbe Lauf: `[Q-Faktor Q4 = UNKLAR] … Runway-Ratio 2 … — ABER keine 50er-Rundzahl-Abdeckung Entry->TP1 (29750 / 29800 fehlen im Register)` → `3/4 GELB (halbe Position) [offen: Q4 (unklar)]`.

**Nebenwirkung, bewusst in Kauf genommen:** Q4 4/4 GRÜN wird noch seltener (verstärkt I1 aus Runde 1). Der Ausweg ist aber kein Regelbruch, sondern Pflege: sobald das 50er-Band regelkonform geführt ist (`register_touch.cjs --rundzahlen`, Schritt-6-Regel Nr. 7), feuert die Warnung nicht und Q4 wird wieder normal entschieden. N1b macht die bestehende Warnung damit erstmals verbindlich statt dekorativ. Falls Levi das nicht will, ist es ein Einzeiler zurück (Registerlücke im Fund-Zweig hart auf `false`).

### 4. Testabdeckung (Prüfaufgabe 4) — Fables Fälle stimmen, von Opus erweitert

Nachgerechnet von Hand aus dem Testcode:
- **(a) VC#9:** Short 29.388,55 → TP1 29.308,3 (80,25 Pkt). Kandidaten unter Entry: 29.350 (38,55 Pkt), Pivot S1 29.349,83 (38,72), Fib 29.332,33 (56,22) → 3 Level zwischen Entry und TP1 ✓, nächstes 29.350 → 38,55/80,25 = **0,4803 → 0,48 NICHT ERFÜLLT** ✓. Einzige 50er auf der Strecke ist 29.350 und steht im Register → keine Warnung ✓ (der Test prüft das negativ mit).
- **(c)** und **(d)** wie oben unter Punkt 1 nachgerechnet — beide behaupten, was sie tun.
- **(e)** behauptete „N1 betrifft NUR den kein-Gegenlevel-Zweig" — das war die Stelle, an der der Befund aus Punkt 3 saß. Ersetzt.

Opus-Erweiterungen im selben Testblock (keine neuen `it`-Blöcke, daher weiter 73 Tests):
- **(b)** VC#42 gegen das Test-Register: jetzt **UNKLAR** statt ERFÜLLT, mit dem vollständigen N1-Text.
- **(e) neu:** Gegenprobe „jenseits TP1 **ohne** Lücke" (Long 29.755 → 29.800, Pivot R2 29.900) → **3,22 ERFÜLLT, 4/4 GRÜN**, keine Warnung. Verhindert, dass der Fix zu breit wirkt.
- **(f) neu:** genau der Aushebelungsfall aus Punkt 3 → UNKLAR / 3-4 GELB.
- **(g) neu:** „vor TP1" + Lücke bleibt **NICHT ERFÜLLT** (keine Aufwertung zu UNKLAR).
- Im Override-Test: gültiger Override sticht auch den N1-UNKNOWN, jetzt sichtbar markiert.
- Zwei Bestandstests (Freigabe-Logging N4 und der Opus-N4-Test aus Runde 1) bauten ihr „4/4 GRÜN"-Szenario auf VC#42 gegen das lückenhafte Test-Register — also auf genau dem Artefakt, das N1b beseitigt. Sie stehen jetzt auf einer register-konsistenten 4/4-Geometrie (Long 29.655 → 29.700 bzw. 29.755 → 29.800 mit belegter 50er-Abdeckung). Der geprüfte Gegenstand (Freigabe-Logging, `testtag_modus`) ist unverändert.

### 5. Regressionscheck (Prüfaufgabe 5) — sauber

Q1, Q2, Q3 unverändert; Ampel-Logik (mehr UNKNOWN als ERFÜLLT → UNBEKANNT, 4/4 GRÜN, 3/4 GELB, sonst ROT) Zeichen für Zeichen unangetastet — Q4 UNKNOWN läuft in die bestehende UNBEKANNT-/`[offen: Q4 (unklar)]`-Mechanik. Hard-Gates (RR 8b, SL-Floor 8c, 8b1/8b1a, 8b2, 8c2, GC-2/GC-5) nicht berührt, Exit-Codes nicht berührt. Die beiden Opus-Fixes aus Runde 1 (**N3** Q2-Budget-Kopplung, **N4** stumme Freigabe) sind unverändert vorhanden und live bestätigt — die N4-Hinweiszeile erschien in jedem Sandbox-Lauf ohne `testtag_modus`. `--auswertung-q2a` liest nach wie vor kein `runway_*`-Feld (nachgeprüft: `skipped_fiktiv.cjs` schreibt `runway_ratio` nur in `--add`, liest es nirgends).

### 6. Der unbeauftragte Regelwerk-Satz in 7b1a (Prüfaufgabe 6) — sachlich korrekt, von Opus nachgezogen

Fables Ergänzung war **keine** Doppelaussage: sie steht als „außer"-Klausel **im** bestehenden Q4-Satz, nicht als neuer Absatz daneben, nennt Bedingung, Wirkung und die beiden Log-Felder, und deckte sich exakt mit dem damaligen Code. Kein Aufblähen, kein Widerspruch. Durch N1b musste sie erweitert werden („in beiden Fällen aber nur, wenn das Register die Strecke Entry→TP1 nachweislich abdeckt …; ein Level **vor** TP1 bleibt unberührt") — gleiche Länge, gleiche Stelle. Zusätzlich eine halbe Zeile in der `loop_prompt.cjs`-Triggerzeile, weil die Konsequenz operativ ist: fehlt eine 50er, kostet das im Zweifel die halbe Position, und das Gegenmittel (`register_touch.cjs --rundzahlen`) gehört an die Stelle, an der der Loop es liest.

### 7. Tests (Prüfaufgabe 7)

Fables **73/73** vor der Prüfung selbst nachgefahren und bestätigt. Nach den Opus-Änderungen: **73/73 grün** (unverändert 73, weil die neuen Fälle in bestehende `it`-Blöcke gehängt wurden statt neue anzulegen).

### 8. Fables offene Punkte (b) und (c) (Prüfaufgabe 8) — beide unkritisch

- **(b) Override sticht N1** — live bestätigt: `--runway-ratio 1.3 --grund-runway-ratio "Box-Oberkante 29870 …"` liefert Q4 = JA, 4/4 GRÜN. **Richtig so**: ein begründeter Override ist die dokumentierte Ausnahme für genau diesen Fall („Level existiert, steht nur noch nicht im Register"), er ist begründungspflichtig, die Zirkelprüfung greift, und beide Werte stehen nebeneinander im Log (`runway_override`, `runway_override_grund`, `runway_ratio_gerechnet`, jetzt zusätzlich `runway_register_luecke`). Damit ist Missbrauch beim Tagesabschluss sichtbar. **Opus-Kleinigkeit umgesetzt:** in diesem Fall sah der Leser „Q4 = JA" direkt neben „-> UNKLAR statt ERFUELLT"; der Detailtext trägt jetzt den Zusatz `[der Override sticht diese Register-Luecke (N1) — begruendungspflichtig und als runway_override/runway_register_luecke geloggt]`.
- **(c) `--batch-strikt` erbt N1** — live bestätigt (Batch-Lauf liefert `status: "UNKNOWN"` mit demselben Text). Konsistent und gewollt; es verschärft allerdings **N5** aus Runde 1: der Strikt-Batch rechnet historische Trades gegen das *heutige* Register, und ein heute unvollständiges 50er-Band erzeugt jetzt zusätzlich UNKNOWNs statt nur schiefer Ratios. Q4-Auswertungen aus `--batch-strikt` bleiben damit erst recht nicht historisch belastbar — N5 unverändert als Info stehen lassen.

### Befundliste Runde 2

| # | Klasse | Befund | Status |
|---|---|---|---|
| — | **Blocker** | keiner | — |
| **N1** | Auflage aus Runde 1 | „kein Gegenlevel" + Registerlücke → UNKLAR | **von Fable korrekt umgesetzt**, live in 3 Fällen bestätigt |
| **N1b** | Nachbesserung (wichtig) | Zweig „nächstes Level erst jenseits TP1" blieb ERFÜLLT — ein einzelnes weit entferntes Level (Pivot R2/Fib, im realen Register immer vorhanden) hebelte N1 aus und machte es gegen ein normales Register zu totem Code | **von Opus gefixt** (gleiche Bedingung, kein neuer Faktor/keine neue Schwelle; 4 Testfälle) |
| **N1c** | Info/kosmetisch | Bei gültigem Override stand „Q4 = JA" neben „-> UNKLAR statt ERFUELLT" im gerechneten Teil | **von Opus gefixt** (Klammerzusatz) |
| **N2/N5, I1-I3** | Info | unverändert aus Runde 1; N5 durch N1b leicht verschärft, I1 (4/4 GRÜN selten) ebenfalls | benannt |
| **I4** | Info | `runway_ratio` kann im N1b-Fall eine Zahl ≥ 1 tragen, während Q4 UNKNOWN ist (Zahl = Doku, Status = Urteil) — beim Übertragen in `add_trade.cjs` nicht verwechseln | benannt |

### Änderungen durch Opus in Runde 2

1. `scripts/gate_check.cjs` — `runwayFromRegister()` Fund-Zweig: Registerlücke + N1-Text für „jenseits TP1" (N1b).
2. `scripts/gate_check.cjs` — `evaluateQScore()`: ein vorgezogener Lücken-Zweig für beide N1-Fälle; toter Ternär im „keins"-Zweig entfernt.
3. `scripts/gate_check.cjs` — Override-Detailtext: Klammerzusatz bei gestochener Registerlücke (N1c); Kopfkommentar Q4-Block nachgezogen.
4. `tests/trading_scripts.test.js` — Fälle (b)/(e) ersetzt, (f)/(g) + Override-N1-Fall neu; zwei Bestandstests auf eine register-konsistente 4/4-Geometrie umgestellt.
5. [[feedback_live_trading]] 7b1a Q4-Bullet — „in beiden Fällen"-Formulierung + „vor TP1 unberührt".
6. `scripts/loop_prompt.cjs` — halbe Zeile: fehlende 50er → Q4 UNKLAR → Rundzahl-Band pflegen.

Nichts an `trades.db`, kein Commit, kein Push. **Tests: 73/73 grün.**

### Gesamturteil Runde 2

**FREIGEGEBEN MIT AUFLAGEN.** Committierbar wie es ist (Fables N1 + die drei Opus-Änderungen). Auflagen, beide ohne Codeänderung:
1. **N1b zur Kenntnis nehmen und bestätigen** — Opus hat Levis Grundsatzentscheidung auf den zweiten, praktisch wichtigeren Zweig angewandt, weil sie in der Runde-1-Empfehlung wörtlich mitgenannt war. Wenn Levi das enger will, ist es ein Einzeiler zurück.
2. **Register-Pflege ist ab jetzt sizing-relevant**: ein nicht nachgeführtes 50er-Band kostet einen Q-Punkt (halbe statt volle Position). Das ist gewollt, sollte aber am nächsten Testtag bewusst beobachtet werden — zusammen mit I1 (wie oft entsteht überhaupt noch ein 4/4 GRÜN?) und dem Prüfkriterium (Datenbasis startet bei 0/15, I2).

Nach beiden Runden gilt: Q2/Q4 sind für den nächsten **Testtag** reif. Für Echtgeld fehlen weiter die Daten, nicht die Regeln — ≥15 Q-bewertete Momente mit vollständigem Nachtrag, dann die Auswertung nach 7b1a.

## N1b — Wirkung auf Trade-Qualität, Nachrechnung 14.09.2026 (Levi-Rückfrage)

**Levis Frage:** „Warum macht es grün spürbar seltener? Sind die Trades dadurch erfolgreicher oder sind wir jetzt zu ängstlich? Nicht dass wir dadurch so viele gute Trades liegen lassen."

**Methode:** `runwayFromRegister()` aus einer Sandbox-Kopie von `gate_check.cjs` (echte Funktion, nicht nachgebaut) gegen alle 51 Live-Gate-Aufrufe aus `scripts/gate_check_log.jsonl` (= 37 unabhängige Setup-Momente, 10.09. + 11.09.; ältere Tage sind nicht geloggt). Register pro Tag rekonstruiert: 11.09. ab 14:21 UTC = die reale `level_register.json`, davor ohne 29.550; 10.09./11.09.-Vormittag aus den Log-Ausgaben rekonstruiert (Rundzahl-Band 28.950–29.200, Pivot S2 29.149,97, Session-Tief 29.125,8, 2 Fib-Extensions). Kontrolle: VC#42 liefert reproduzierbar 0,35 — identisch zum Runde-1-Echtlauf. Zusätzlich `skipped_setups_fiktiv.jsonl` (12 Zeilen), 09.09.-Protokoll (4 Q4-Bewertungen) und die 11.09.-Kursverläufe.

### Befund 1 — N1b hätte an keinem einzigen realen Moment etwas geändert: **0 von 37**

| Zweig | Momente | Q4 |
|---|---|---|
| Gegenlevel **vor** TP1 (Register deckt ab) | **35** | NICHT ERFÜLLT (Ratio 0,07–0,78) |
| Gegenlevel **jenseits** TP1, Abdeckung belegt | **2** (11.09. 18:36 / 18:51 DE) | ERFÜLLT → bleibt ERFÜLLT |
| „kein Gegenlevel" / „jenseits TP1" **mit** 50er-Lücke (N1/N1b-Fall) | **0** | — |

Grund: das 50er-Band war an beiden Tagen tatsächlich gepflegt (`register_touch.cjs --rundzahlen`: 10.09. Band [28.950;29.200], 11.09. Band [29.300;29.550]), und jedes TP1 lag innerhalb des Bandes. Auch die vier Q4-Bewertungen vom 09.09. (VC#1 0,75, VC#9 0,48, VC#10 0,47, VC#12 0,53 — die Ablehnungen, die ~2,9R vermieden haben) liegen sämtlich im Zweig „Rundzahl **vor** TP1", den N1b nicht anfasst. **Der bewiesene Nutzen von Q4 stammt komplett aus einem Zweig, den N1b nicht berührt.**

### Befund 2 — wenn N1b feuert, ersetzt es ein FALSCHES ERFÜLLT, kein richtiges

Simulation an denselben 33 Momenten des 11.09. mit künstlich veraltetem Rundzahl-Band (alle Rundzahl-Einträge entfernt = Worst case):

| | ohne N1b | mit N1b | bei gepflegtem Register (Wahrheit) |
|---|---|---|---|
| ERFÜLLT | **7** | 2 | **2** |
| UNKLAR | 0 | 5 | 0 |
| NICHT ERFÜLLT | 26 | 26 | **31** |

Die **5** Momente, die N1b auf UNKLAR zieht, sind **exakt** die 5, die ein korrekt gepflegtes Register mit NICHT ERFÜLLT bewertet hätte — Trefferquote 5/5, keine einzige Fehl-Abwertung. Die 2 echten ERFÜLLT (18:36 / 18:51) überstehen N1b unangetastet.

Das ist kein Zufall, sondern strukturell beweisbar: N1b feuert nur, wenn (i) **kein** Registerlevel zwischen Entry und TP1 liegt **und** (ii) dort eine arithmetische 50er **fehlt**. Trägt man diese 50er nach, wird sie zum Level vor TP1 → Ratio < 1 → NICHT ERFÜLLT. **Es gibt keinen Fall, in dem Registerpflege ein ERFÜLLT ergäbe, das N1b auf UNKLAR zieht.** N1b ist damit sogar *milder* als die korrekte Antwort.

### Befund 3 — Levis Fall (b) („Struktur da, nur keine 50er") kann N1b gar nicht auslösen

Ein Pivot / Fib / Session-Extremum zwischen Entry und TP1 steht im Register, erzeugt `vorTp1.length > 0` und schaltet die Lücken-Prüfung ab (Code: `registerLuecke = !vorTp1.length && rzFehlend.length > 0`). N1b prüft ausschließlich, ob das Register die Strecke abdeckt — nicht, ob der Markt dort strukturlos ist. Ein reines Erfassungsproblem wird also nicht als Marktrisiko bestraft; bestraft wird ein **unvollständiges Register**, und das Gegenmittel steht daneben (`register_touch.cjs --rundzahlen`).

### Befund 4 — das eigentliche „zu ängstlich"-Risiko sitzt nicht bei N1b, sondern bei Q4-a selbst (I1)

Über die 37 Momente: **2 ERFÜLLT / 35 NICHT ERFÜLLT (5,4 %)** — und zwar *ohne* jede N1b-Wirkung. Ursache ist die Kombination 8c (SL ≥ 1,5× ATR) + 8b (RR ≥ 1) + 50er-Raster: ab **ATR ≥ 33,3 Pkt** ist die TP1-Distanz zwangsläufig ≥ 50 Pkt, also liegt immer eine 50er dazwischen → Q4 = ERFÜLLT ist **arithmetisch unmöglich**. Das traf **15 der 37 Momente** (41 %); ATR-Spanne 16,6–47,0, Median 32,2 — die Schwelle liegt mitten in der Verteilung. Dass 4/4 GRÜN selten ist, ist zu ~100 % Q4-a (I1) und zu 0 % N1b.
Gegenbefund zur Qualität der 50er als Hindernis: am 11.09. (Trendtag +460 Pkt) lief der Kurs durch 29.400 / 29.450 glatt hindurch (29.450,85 um 19:25) — die 50er, die vier Setups das Q4 kosteten, waren real keine Hürde.

### Was wäre an den betroffenen Momenten passiert

Die zwei Q4-ERFÜLLT-Momente (11.09. 18:36 Long 29.400,95→29.450 und 18:51 Long 29.401,35→29.450) hätten TP1 um 19:25 erreicht (29.450,85) und laut 11.09.-Analyse nie den SL getroffen. Beide bleiben mit N1b **4/4 GRÜN (volle Position)** — genau die zwei Gewinner behält die Regel. Der einzige real entstandene Trade (VC#35, 18:31, ~+0,6R) war schon vor N1b GELB/halbe Position (Q4 = 0,12, Rundzahl 29.400 vor TP1) und ist von N1b nicht betroffen.

### Einordnung: **(a) gut kalibriert** — keine Nachbesserung an N1b empfohlen

N1b trifft in 0 von 37 realen Momenten, und in der Simulation 5/5 mal genau das, was ein gepflegtes Register ergeben hätte. Es ist keine zusätzliche Ängstlichkeit, sondern das Schließen einer Hintertür, durch die eine Datenlücke volle Positionsgröße gerechtfertigt hätte. **Datenlage ehrlich:** n = 2 geloggte Tage, 0 tatsächliche Auslösungen — die Aussage stützt sich nicht auf die Stichprobe, sondern auf die Bedingung selbst (Beweis in Befund 2), deshalb (a) und nicht (c).

**Offen bleibt I1 als echte Kalibrierungsfrage** (Q4-a, nicht N1b): wenn nach ≥15 Q-bewerteten Momenten weiter <10 % 4/4 GRÜN entstehen, sind zwei Stellschrauben zu prüfen — (1) nur *bestätigte* Level (per Wick/Kerzenschluss getestet, `register_touch.cjs` Touch-Historie) als Q4-Gegenlevel zählen statt jeder generierten 50er, oder (2) Q4-Schwelle von 1,0 auf ~0,8 senken. Beides ist eine Levi-Entscheidung, nichts davon umgesetzt.
