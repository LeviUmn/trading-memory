# Opus: Prüfung der Levi-Entscheidungen a–d, Vorschlag 1H-Intrabar-Freigabe, Fable-Auftragspaket, Lückencheck

Stand: Mi 30.09.2026, 10:25 DE (`date` = Wed Sep 30 10:24:40 2026). Nur gelesen. Im Repo wurde nichts verändert. `momente_log.jsonl` hatte vor und nach meinen Läufen denselben Hash (sha1 3ee7bb00…). Alle Rechnungen liefen gegen Scratch-Kopien in diesem Ordner:
- `mom_scratch.jsonl`: Repo-Log plus 29.09., für die Freeze-Zählung.
- `halb_sim.cjs`, `episoden.cjs`: Intrabar-Simulation. Basis sind `mom_base`/`mom_live` aus Opus' 29.09.-Scratch, erzeugt von `p8sim.cjs`.
- `repo/`: `git archive HEAD` mit dem Testlauf.

---

## 1. Prüfung von Levis Entscheidungen

### a) „29.09. zählt als Freeze-Tag“

**Was das konkret heißt** (Nachweis: `tagesmomente.cjs --auswertung` auf der Scratch-Kopie mit dem 29.09.):
- **3/5 bewertbare Tage** (25.09., 28.09., 29.09.), **AUTO 6/20**, 3/10 der Obergrenze. FREEZE-ENDE: nein.
- AUTO seit 25.09.: n=6, Σ −1,67 R, Ø −0,28 R, Ø ohne besten Tag −0,57 R. Ohne den 29.09. wären es n=5, Σ −0,67, Ø −0,13, ohne besten Tag −0,35.
- FLOOR: n=12, Σ −5,18 R. Ohne den 29.09.: n=10, Σ −3,62 R.
- ALT: n=2, Σ −0,68 R. Ohne den 29.09.: n=1, +0,32 R.

**Wichtige Korrektur an meiner eigenen Empfehlung von gestern:** Der 25.09. war ebenfalls nur ein Teiltag, und er wird seit dem 28.09. als bewertbar gezählt. Abdeckung aus den Voll-Check-Zeiten in `momente_log` (Slots mit VC von 54 Slots zwischen 15:30 und 20:00):

| Tag | VC | erster–letzter VC | Abdeckung |
|---|---|---|---|
| 11.09. | 51 | 15:40–19:55 | 94 % |
| 15.09. | 48 | 15:25–19:57 | 87 % |
| 21.09. | 45 | 16:00–19:50 | 83 % |
| 23.09. | 47 | 15:39–20:03 | 85 % |
| **25.09.** | 27 | **17:47**–19:55 | **50 %** |
| 28.09. | 52 | 15:22–19:56 | 93 % |
| **29.09.** | 23 | **16:42–18:30** | **43 %** |

Meine Empfehlung „29.09. nicht zählen“ war also **inkonsequent**. Dieselbe Logik hätte auch den 25.09. streichen müssen, dann stünde der Freeze bei 1/5. Levis Entscheidung ist mit dem 25.09. **konsistent**. Das nehme ich ausdrücklich zurück.

**Was die Entscheidung tatsächlich kostet:**
- **Wenig beim Tageszähler.** Der eigentliche Engpass ist AUTO ≥ 20. Bisher kamen im Schnitt 2,0 AUTO-Bewegungen pro bewertbarem Tag (6/3). Für die fehlenden 14 braucht es also rund 7 weitere bewertbare Tage.
- **Das Risiko liegt bei der Obergrenze.** Nach 10 bewertbaren Tagen endet der Freeze auch ohne AUTO ≥ 20, und AUTO gilt dann als „zu selten, nicht belegt“. Teiltage verbrauchen Tage dieser Obergrenze, liefern aber wenig AUTO-Bewegungen.
  - Wenn ab heute jeder Handelstag bewertbar ist, fällt der 10. bewertbare Tag auf **Do 08.10.2026**. Ohne den 29.09. wäre es Fr 09.10.
  - Rechnerisch treffen Obergrenze und AUTO=20 ungefähr gleichzeitig ein. Ob AUTO bis dahin belegt ist, ist offen, eher knapp.
- **Mehrdeutig, bitte von Levi rückbestätigen:** Die Entscheidung gilt für den 29.09. Gilt sie auch als **Regel für künftige Teiltage**? Heute zählt jeder Tag mit ≥ 20 VC, Bars ok und dem Standardfenster, auch mit 40 %. Mein Vorschlag: so lassen (keine Definitionsänderung im Freeze), aber die Abdeckung **anzeigen und dokumentieren**.

**Bleibt die Abdeckungsanzeige sinnvoll? Ja**, als reine Transparenz:
- Die Zeile in `--auswertung` weist je Tag die Abdeckung aus und markiert Tage unter 80 % als „Teiltag (zählt, Levi 30.09.)“.
- Dazu kommt eine **Referenzzeile „ohne Teiltage“**, gekennzeichnet als „NICHT maßgeblich“, nach dem Muster der bestehenden `--seit`-Referenz.
- So sieht man beim Freeze-Review sofort, ob ein Ergebnis an den Teiltagen hängt.

**Dokumentationspflicht (Fable, reine Memory-Pflege):** In `project_testtag_2026-09-23_besprechung_ausstehend.md`, Abschnitt 5, und in `project_testtag_analyse_2026-09-29.md` festhalten:
- „29.09. zählt (Levi 30.09.), Abdeckung 43 %.“
- „25.09. zählte bereits mit 50 %.“
- Die Werte ohne diese Tage (siehe oben).

**Voraussetzung, damit die 3/5 überhaupt stimmt:** Der offizielle Lauf `tagesmomente.cjs --datum 2026-09-29` fehlt noch (siehe 4). Im Repo steht heute **2/5, AUTO 5/20**.

### b) „wir nehmen die 19:55er Schlusskerze“

**Was die Skripte heute tun** (verifiziert):
- `kombi_fiktiv.cjs`:
  - Z. 57: `LOOP_ENDE_DE = '20:00'`.
  - Z. 120: gewertet werden Bars mit `time < bisSec`.
  - Letzte gewertete Kerze ist also die mit **Label 19:55** (öffnet 19:55, schließt 20:00).
  - Der Nachtrag vom 29.09. lief mit `--bis 2026-09-29T18:00:00Z`, also 20:00 DE.
- `skipped_fiktiv.cjs`:
  - **hat keinen Default** (Z. 176: `if (bisSec !== null)`). Ohne `--bis` wertet es die ganze Bars-Datei.
  - Die Datei vom 29.09. reicht bis 20:40. **Ohne `--bis` wäre Setup 3 als SL-HIT −1 R gebucht worden** (Hoch 30385,15 in der Kerze 20:40).
  - Der Operator hat `--bis 18:00Z` übergeben. Der Eintrag zeigt `exit_ts 2026-09-29T18:00:00.000Z` und `terminalkurs 30246.45`.
- `tagesmomente.cjs` (Z. 113): ebenfalls `time < bisMs` mit Default 20:00, also dieselbe Kerze 19:55.

**Die Zahlen aus `nas100_5m_2026-09-29.json`:**
- Die Kerze 19:50 schließt bei 30256,95. Setup 3 steht damit bei (30306,05 − 30256,95) / 71,3 = **+0,69 R**.
- Die Kerze 19:55 schließt bei 30246,45, also **+0,84 R**.

**Wahrscheinlichste Lesart:** „19:55er Schlusskerze“ ist die Kerze mit **Label 19:55**, geschlossen um 20:00, also +0,84 R. Das ist **genau das heutige Verhalten** aller drei Skripte (bei `skipped_fiktiv` nur, wenn der Operator `--bis` richtig setzt).

**Was sich dadurch ändert:** am Stichtag **nichts**. Der Auftrag „Tagesende“ schrumpft auf drei Punkte:
1. `skipped_fiktiv.cjs` bekommt denselben Default wie `kombi_fiktiv` (20:00 DE des Eintragsdatums). Das ist die eigentliche Fehlerquelle.
2. Die Nachtrags-Kommandozeile in `skipped_nachtrag.cjs` wird korrigiert. Sie schlägt heute noch die veraltete Hoch/Tief-Form ohne `--bars`/`--bis` vor (Z. 28).
3. `protokoll_bilanz.cjs` meldet PASS-Freigaben, die ohne Grund ausgelassen wurden.

Die geplante Rückbau-Konstante „Stichtag 20:00“ ist damit überflüssig, weil ohnehin 20:00 gilt.

**Rückbestätigung von Levi nötig** (ein Satz): „19:55er Kerze = die Kerze, die 19:55 öffnet und 20:00 schließt (+0,84 R), richtig?“

Falls er die 19:50-Kerze meint (Schluss 19:55, +0,69 R), muss auch `tagesmomente.cjs` mitgezogen werden, sonst messen Nachträge und Freeze-Messung verschiedene Horizonte. Das würde die Freeze-Messdefinition mitten im Freeze ändern und alle Tage neu rechnen. **Davon rate ich ab.**

### c) 1H-Intrabar-Freigabe mit halber Position

Details siehe 2. Kurzfassung:
- Der Vorschlag ist logisch sauber und risikobegrenzt.
- Mit der **echten SL-Anker-Regel (ALT)** hätte er in 5 Testtagen **0 zusätzliche Trades** gebracht. Mit AUTO wäre es eine Freigabe gewesen, die Q-ROT gewesen wäre und in der Kette −0,5 R gekostet hätte.
- Der Nutzen zeigt sich nur in FLOOR (1,5×ATR ohne Anker). Das ist keine gültige Live-SL-Regel.
- **Empfehlung: nur als Schatten-Auswertung, Live-Entscheidung erst nach dem Freeze-Ende.**

**Mehrdeutig:**
- „vielleicht eine Lösung finden“ ist keine Freigabe für eine Live-Änderung.
- Die Agenda-Frage c) war „Override unverändert bis Freeze-Review?“. Ich lese Levis Antwort so: der Override bleibt live unverändert, die Idee wird gemessen. **Das bitte bestätigen lassen.**

### d) „mein Fehler, wir machen da weiter nichts“

Akzeptiert, keine Maßnahme.
- Hinweis: Mit a) kosten Teiltage wenig beim Tageszähler, verbrauchen aber die 10-Tage-Obergrenze.
- Die Abdeckungsanzeige aus Auftrag 3 macht solche Tage künftig automatisch sichtbar. Mehr braucht es nicht.
- Unabhängig davon bleibt „Remote Control vorher an“ laut `feedback_testtag_start_verlaesslichkeit.md` gültig. Die Ursache am 25.09. war eine andere.

---

## 2. Vorschlag zu c): „1H-Intrabar-Freigabe (IBF), halbe Position“

### 2.1 Definition (prüfbar)

**Anwendbar**, wenn alle Punkte gelten:
1. Das 1H-Override-Veto ist aktiv: Der letzte **geschlossene** 1H-Bar steht auf der falschen EMA50-Seite. Die Definition vom 04.09. bleibt unverändert.
2. **3/4-Konstellation**: 5m, 15m (NAS100) und QQQ-15m zeigen alle in die Traderichtung. Das ist genau `blockade_3v4 = true` im `oneh_shadow_log`, also das, was heute schon gemessen wird.
3. **Intrabar-Seite:** Der Kurs steht in Traderichtung jenseits der EMA50 des letzten geschlossenen 1H-Bars. Maßgeblich ist der Entry beim Gate bzw. `kurs` im Voll-Check. Long: Kurs > EMA50, Short: Kurs < EMA50.
   - Das ist **mathematisch identisch** mit „laufender 1H-Close gegen laufende 1H-EMA50“. Es gilt EMA_live = EMA_prev + α·(P − EMA_prev), also P − EMA_live = (1−α)·(P − EMA_prev). Das Vorzeichen ist gleich, eine Mehrdeutigkeit gibt es nicht.
   - Die nötigen Werte (`kurs`, `ema50_1h`) stehen schon heute im Shadow-Log.
4. **Keine Schwelle, keine Mindestdauer.** Begründung aus den Zahlen: Die Variante mit Schwelle (P8, 2 Schlüsse ≥ 1,0×ATR jenseits) war schlechter als die ohne Schwelle, ohne den 15.09. −2,0 R gegenüber −1,05 R. Eine Schwelle verschiebt den Einstieg nur ans Ende der Bewegung.
5. **Rückfall-Bedingung:** Die IBF erlischt, sobald der Kurs vor dem Entry wieder auf die falsche Seite fällt. Sie gilt jeweils nur für den aktuellen Gate-Lauf. Nach dem Entry gibt es keine Sonderregel, es gelten normaler SL, TP und Punkt 12. Kein Nachkauf auf volle Größe, wenn der 1H-Bar danach richtig schließt (Nachkauf Typ B bleibt verboten).
6. **„Alle anderen Indikatoren schlagen an“** heißt: Dual-Gate 15m NAS + QQQ, SL-Anker TAUGLICH inkl. P1/P3, GESAMTSTATUS PASS (RR-Gate, TP-Realismus, 8c2) und Q-Score **nicht ROT**.
   - Q3 bleibt auf dem geschlossenen 1H-Bar. Q3 ist bei IBF damit automatisch NICHT ERFÜLLT, das Maximum ist 3/4 = **GELB = halbe Position**.
   - So entsteht Levis „halbe Position“ **aus der bestehenden Q-Score-Logik**, ohne neue Sizing-Regel.
   - Q3 zusätzlich auf intrabar umzustellen, wäre eine doppelte Lockerung. Davon rate ich ab.

**Wie es sich zu den bestehenden Regeln verhält:**
- **1H-Override-Veto (04.09.):** Die IBF ist eine Ausnahme davon, also eine Regeländerung an einem Punkt der Freeze-Liste „nicht anfassen“ (`project_testtag_analyse_2026-09-28.md`, Abschnitt D). **Live im Freeze nicht erlaubt.**
- **Wo das Veto heute greift:** Nicht in `gate_check.cjs`. Dessen 1H-Zeile ist „[Anzeige, kein Gate]“ (Z. 3037). Das Veto wirkt in `vollcheck.cjs` über die Dual-Gate-Definition („technisch 2/2 wäre KEIN gültiges Dual-Gate“, Z. 1333) und damit darüber, dass Sonnet keinen 7b1-Ablauf startet. Eine spätere Live-Umsetzung betrifft deshalb vollcheck (Dual-Gate-Zeile), loop_prompt (7b1-Text) und das Regelwerk 7b1. Die Folgen gehen also über gate_check hinaus.
- **13.1 / k-Zähler (b1):** Eine IBF-Situation zählt **nicht** als vollständiges Dual-Gate. 13.1 bleibt unverändert, sonst entstünde eine Nebenwirkung auf den Chasing-Zähler.
- **Sizing, wenn mehrere Gründe zusammenkommen:** Es gilt die **Stacking-Regel** (`project_risikomanagement.md` Z. 196 ff., `feedback_live_trading.md` Z. 824/1233): Egal wie viele Halbierungsgründe zusammenkommen, es wird **nur einmal halbiert**.
  - IBF (GELB) im Halbierungsfenster 15:30–16:00: 0,5, nicht 0,25.
  - IBF mit Q-ROT, live: **kein Trade** (Q-ROT bleibt Veto).
  - Fiktiver Kombi-Modus: Größe = **min(Kombi-Größe, 0,5)**, **nicht das Produkt**. ROT bleibt also 0,25, GELB/GRÜN wird 0,5.
- **Order-Sperre / Blackout / Sperrfristen:** unberührt. Die IBF hebt keine Sperre auf.

### 2.2 Nachrechnung

Methode: exakt die von gestern. Im `oneh_shadow_log` wird bei `blockade_3v4` und Intrabar-Seite `bias['1h']` auf null gesetzt, dann läuft `tagesmomente.cjs` (unverändert) über fünf Tage mit Bars: 15./23./25./28./29.09. Für den 16.09. fehlen die Bars, am 17. und 21.09. gab es keine Blockade.

Neu ist, dass Momente, die die IBF freigibt, **mit Gewicht 0,5** in die Kette eingehen. Die Kette bleibt dabei „eine fiktive Position zur Zeit“: Eine freigegebene halbe Position kann also eine spätere volle Position verdrängen. Skript: `halb_sim.cjs`.

**Kettendelta gegenüber heute (Veto), Summe über die 5 Tage:**

| Szenario | ALT | AUTO | FLOOR | FLOOR ohne 15.09. | FLOOR ohne besten Δ-Tag |
|---|---|---|---|---|---|
| S1 Live-Seite, volle Größe (gestern) | ±0 | ±0 | **+1,95** | −1,05 | −1,05 |
| **S2 Live-Seite × 0,5 (Levis Idee, ohne Q-Filter)** | ±0 | **−0,50** | **+1,45** | **−1,05** | −1,05 |
| **S3 × 0,5 + Q-Score nicht ROT (Levis Idee komplett)** | ±0 | ±0 | **+2,50** | **±0,00** | ±0,00 |
| S4 volle Größe + nicht ROT (zum Vergleich) | ±0 | ±0 | +3,00 | ±0,00 | ±0,00 |

FLOOR S3 je Tag: 15.09. +2,50 | 23.09. ±0 | 25.09. ±0 | 28.09. +0,50 | 29.09. −0,50.

Achtung Kettenartefakt: Der größte Teil der +2,50 am 15.09. stammt **nicht** aus den freigegebenen Trades selbst (Σ +0,50 bei halber Gewichtung). Er kommt daher, dass zwei Verlierer der heutigen Kette (16:00 Long −1, 17:03 Short −1) verdrängt werden.

**Je Blockade-Episode** (erster IBF-fähiger Moment, RR-1-Ausgang, R ungewichtet; Skript `episoden.cjs`):

| Episode | IBF ab | FLOOR | ALT | AUTO | ampel_max | erster Moment mit ampel_max ≠ ROT |
|---|---|---|---|---|---|---|
| 15.09. Long | 15:45 | −1 | zu | zu | GELB | 15:45: FLOOR −1 |
| 15.09. Short | 16:10 | +1 | zu | zu | GELB | 16:10: FLOOR +1 |
| 23.09. Short (2 Phasen: 16:11–16:51, 17:31–17:51) | 16:11 | +1 | zu | 17:31: +1 | **ROT** | keiner, Q-ROT → kein Trade |
| 28.09. Short | 15:46 | −1 | zu | zu | ROT | 15:56: FLOOR +1 |
| 29.09. Long | 16:42 | −1 | zu | zu | GELB | 16:42: FLOOR −1 |
| 29.09. Short | 17:31 | +0,57 offen | +0,32 offen | zu | ROT | 17:36: FLOOR +1, ALT +1 |

In drei Episoden greift die IBF **nicht**, weil der Kurs nicht jenseits der EMA stand: 25.09. 17:52, 28.09. Long 18:31 und 28.09. Long 19:16. Das sind genau drei der sechs Fälle „Verlust vermieden“. Die Live-Seite behält sie blockiert, das ist ihr Vorteil gegenüber „Override ganz weg“.

Auswertung auf Episodenbasis (FLOOR, weil ALT/AUTO fast nie offen sind):
- **Ohne Q-Filter (S2):** 6 Episoden, 3 Gewinne, 3 Verluste. Σ −0,43 R ungewichtet, also −0,22 R bei halber Größe. **Ø −0,07 R** je Episode. Ohne den besten Tag (23.09.): Ø −0,29 R. **Kriterium verfehlt.**
- **Mit „alle anderen Indikatoren“ (S3, ampel_max ≠ ROT):** 5 Episoden (23.09. fällt als ROT raus), 3 Gewinne, 2 Verluste. Σ +1,0 R, **Ø +0,20 R** (halbe Größe +0,10). Ohne den besten Tag (28.09.): Ø **±0,00**.
- Formal: Ø ≥ +0,10 ✓, ohne besten Tag ≥ 0 ✓ (genau auf der Kante), Gewinne ≥ Verluste ✓. **Mindest-n verfehlt: n=5, Soll ≥ 10.**

**Filtern die „anderen Indikatoren“ die Fehlwenden?**
- **29.09. Long 16:42: nein.** ampel_max ist GELB, die Fehlwende wird freigegeben (−1 R, halb −0,5).
- **23.09. erster Moment 16:21 (SL): ja**, aber zum Preis des ganzen Trendtag-Gewinns. Q2 ist dort NICHT ERFÜLLT (Überdehnung > 1,5×ATR). Der Trendtag-Modus (Y4-b) greift nicht, weil er den 1H-Bias *in* Richtung verlangt. Also ROT über die ganze Episode.
- Der Q-Filter nimmt also den Trendtag heraus (Gewinn und Verlust zugleich) und lässt die Fehlwende vom 29.09. durch. Das ist **keine gezielte Filterwirkung, sondern Zufall bei n=5.**

**Die ehrliche Obergrenze:** `ampel_max` nimmt Q1 und Q4 als erfüllt an. Beide sind während einer Blockade nie gemessen, weil kein Gate-Lauf stattfindet. Historisch waren in PASS-Läufen **Q4 in 11 von 16 NEIN** und **Q1 in 12 von 16 NEIN/UNKLAR** (Bericht 29.09., Abschnitt F3). Da Q3 bei IBF immer NEIN ist, führt schon ein einziges NEIN bei Q1 oder Q4 zu **ROT**. Realistisch sind also die meisten der 5 „GELB“-Episoden in Wahrheit ROT, und die IBF würde live noch seltener greifen.

**Der entscheidende Befund:**
- Mit dem **ALT-Anker**, also der heute live gültigen SL-Regel, war das TP1-Fenster bei 5 von 6 IBF-Episoden **zu**. Die einzige offene (29.09. Short) liegt in der Kette hinter der Long-Position von 17:01.
- Ergebnis: **0 zusätzliche Trades in allen Szenarien**. AUTO: eine Freigabe (23.09. 17:31, ROT), die bei halber Größe einen ganzen Gewinner verdrängt: −0,5 R.
- Die IBF adressiert also **nicht den Engpass**. Die Reihenfolge von gestern bestätigt sich: Q4/Q1 vor Anker-Geometrie vor Override.
- Bei Payoff 0,94:1 und Ø −0,51 R (realisiert) sind „mehr Entries“ zudem kein Ziel an sich.

**Warnung wegen Überanpassung:** n = 5–6 Episoden. Ein einziger Trendtag (15.09. oder 23.09.) dreht jedes Vorzeichen. Die FLOOR-Gewinne hängen zu über 80 % am 15.09.

### 2.3 Freeze-Konformität und Empfehlung

- (ii) **live sofort: nein.** Das wäre eine Änderung am 1H-Override (Liste „nicht anfassen bis Freeze-Ende“) und die zweite Live-Änderung nach P3. Außerdem gibt es keinen Beleg.
- (iii) **Live-Entscheidung erst nach dem Freeze-Ende.**
- (i) **Bis dahin nur Schattenmessung, und zwar ohne Loop-Änderung.** Alle Eingaben stehen schon in den Logs (`oneh_shadow_log`: kurs, ema50_1h, blockade_3v4, bias; `momente_log`: Geometrie und ampel_min/max). Ein **reines Auswertungsskript** rechnet das nach, Sonnet hat im Loop keine Mehrarbeit.
- Q1/Q4 während der Blockade bleiben ungemessen und werden als Band ampel_min/ampel_max ausgewiesen. Das akzeptiere ich bewusst, statt Sonnet in Blockaden zu Schatten-Gate-Läufen zu verpflichten (das kostet Tempo).

**Vorab-Kriterien, jetzt festgelegt, geprüft beim Freeze-Review:**
1. Basis: je Blockade-Episode der **erste IBF-fähige Moment mit ampel_max ≠ ROT**. RR-1-Ausgang, R ungewichtet. **Primärvariante AUTO**, dazu ALT und FLOOR nur als Referenz. FLOOR allein belegt nichts, weil FLOOR keine Live-SL-Regel ist.
2. **Mindest-n: ≥ 10 Episoden mit offenem TP1-Fenster in der Primärvariante.** Ist das am Freeze-Ende nicht erreicht, gilt „zu selten, nicht belegt“ und es wird nicht umgesetzt.
3. **Ø R ≥ +0,10** UND **Ø R ohne den besten Tag ≥ 0** UND **Gewinn-Episoden ≥ Verlust-Episoden**.
4. Zusätzlich muss das **Kettendelta gegenüber heute bei Gewicht 0,5 ohne den besten Tag ≥ 0** sein. So wird die Verdrängung voller Positionen erfasst.
5. Getrennt ausgewiesen: das Ergebnis bei ampel_min (alle unbekannten Q als NEIN). Ist das negativ, wird ausdrücklich gewarnt.

**Heutiger Stand gegen diese Kriterien:** n(AUTO, offen, nicht ROT) = **1** (29.09. 17:51, +1). Bei rund 1,2 Episoden pro Tag und kaum offenen Fenstern wird n ≥ 10 bis zum Freeze-Ende (spätestens rund 08.10.) **sehr wahrscheinlich nicht erreicht**. Meine Prognose: „zu selten, nicht belegt“.

**Rückbau:** Da nichts live geht, gibt es nichts zurückzubauen. Das Auswertungsskript schreibt nichts. Falls die Regel nach dem Freeze doch live gehen soll: eine Konstante `IBF_MODUS = 'aus' | 'live'` in vollcheck.cjs, Default `'aus'`.

**Ist die Idee unsinnig? Nein.** Sie ist die sauberste der schnelleren Varianten: Sie hält Fehlwenden, die nie über die EMA kommen, weiter blockiert, und hat eine natürliche halbe Größe über GELB. Sie **löst nur nicht das Problem**, das Levi lösen will (zu wenige Trades), weil der Anker das Fenster fast immer schließt.

**Bessere Alternative für „mehr Trades“:** nichts Neues bauen. Die bereits laufenden Messungen bis zum Freeze-Ende durchziehen:
- AUTO-Anker: A2, P3, tagesmomente.
- Q-ROT als Größe statt Veto: Kombi-Schatten, bisher n=5, Σ +0,34 R gewichtet.

Diese beiden adressieren die belegten Engpässe (Anker, Q1/Q4). Beim Freeze-Review wird die IBF mitgeprüft. Greift dann eine AUTO-Anker-Regel, öffnen sich auch die IBF-Fenster, und die IBF bekommt überhaupt erst eine Chance.

---

## 3. Auftragspaket für Fable

**Mengenbremse:** Zwei Code-Aufträge plus ein reiner Ausführungsauftrag. Die Kleinigkeiten stecken in Auftrag B. Q3-auto, Tagesende, Abdeckung und IBF sind so auf zwei Pakete verteilt, die sich keine Dateien teilen (außer der Testdatei, siehe Warnung).

**Reihenfolge:**
- **Auftrag 0** (heute, sofort, kein Code) → **Auftrag A** (Auswertung, ohne Einfluss auf den Loop, heute möglich) → **Auftrag B** (Live-Anzeige und Prozesstext, erst **nach Loop-Stopp heute**, aktiv ab dem nächsten Testtag).
- Jeder Auftrag bekommt einen eigenen Opus-Gegencheck, bevorzugt durch einen frischen Opus ohne diesen Kontext.

**Überschneidungen in denselben Dateien:**
- `tests/trading_scripts.test.js` betrifft A (skipped_fiktiv/protokoll_bilanz) und B (gate_check/vollcheck). **A und B nacheinander umsetzen, nicht parallel**, B auf Stand A aufbauen.
- `gate_check.cjs` wird von `vollcheck.cjs` als Unterprozess aufgerufen: Die Vorprüfungszeilen werden per Regex zitiert und geparst (vollcheck Z. 360–430). Textänderungen in gate_check (kerzen-qqq) nur in **Live-Zeilen, die vollcheck nicht parst**, sonst das Parsing mitprüfen.
- `tagesmomente.cjs` wird nur von A angefasst.

### Auftrag 0: Ausführung, kein Code (Fable)

1. **Offizieller Tagesmomente-Lauf 29.09.:**
   ```
   node scripts/tagesmomente.cjs --datum 2026-09-29 --bars scripts/nas100_5m_2026-09-29.json
   ```
   Erwartet laut Scratch-Probe: 23 Momente, 13 qualifiziert, Bars ok 66/66; ALT −1,00 / AUTO −1,00 / FLOOR Σ −1,56. Der Lauf ist idempotent. Danach `--auswertung` wörtlich festhalten: 3/5, AUTO 6/20, FREEZE-ENDE nein.
2. **Memory-Dokumentation der Levi-Entscheidungen a–d**, mit Abdeckungszahlen 25.09. 50 % und 29.09. 43 %:
   - `project_testtag_2026-09-23_besprechung_ausstehend.md`, Abschnitt 5 (Stand-Zeile aktualisieren)
   - `project_testtag_analyse_2026-09-29.md`
   - Agenda-Datei als erledigt markieren, Wochentag korrigieren
   - MEMORY.md-Zeilen **ersetzen**, nicht anhängen (Index-Konvention)
   - IBF-Entscheidung mit den Vorab-Kriterien aus 2.3 als eigene Projekt-Datei
3. **Memory-Git-Backup:** Der letzte Commit im Memory-Repo ist `922fafe` vom **23.09. 11:16**. Seitdem sind 9 geänderte und 10 ungetrackte Dateien ungesichert, darunter die Freeze-Definition und alle Analysen vom 23. bis 29.09. Commit + Push wie bei früheren Memory-Backups. *[Nachtrag 30.09.: KEINE Handlungsanweisung an Sonnet/Fable — Commit und Push beauftragt nur Levi, Memory-Repo ist öffentlich → kein Push; siehe [[feedback_commit_push_nur_levi]].]*

### Auftrag A: Auswertung am Tagesende (Tagesende-Regel, Abdeckung, IBF-Schatten)

**Ziel:** Nachträge am Tagesende deterministisch machen, Teiltage sichtbar machen und die IBF messbar machen. Kein Einfluss auf den Loop, keine Änderung an Gates oder Schwellen, also freeze-konform.

**A1: Tagesende-Stichtag (Levi-Entscheidung b, Lesart „Kerze 19:55 bis 20:00“, vorbehaltlich Rückbestätigung)**
- `scripts/skipped_fiktiv.cjs`, Nachtrag mit `--bars`: Fehlt `--bis`, gilt standardmäßig **20:00 DE des Eintragsdatums** (`e.datum`). Die Berechnung ist dieselbe wie `loopEndeSec()` in `kombi_fiktiv.cjs` Z. 101–108, Sommerzeit über Intl, ohne Bibliothek. Nicht kopieren: am besten in ein gemeinsames Modul auslagern (Muster `skipped_nachtrag.cjs` / `register_constants.cjs`) und beide Skripte darauf umstellen.
- Neues Feld `bis_quelle` im Nachtrag: `'--bis'` oder `'Loop-Ende 20:00 DE (Default)'`, wie bei kombi (Z. 263).
- Konstante `TAGESENDE_DE = '20:00'` an **einer** Stelle.
- `--bis` bleibt als Override, z. B. für einen Loop-Stopp vor 20:00.
- `scripts/skipped_nachtrag.cjs` `nachtragCmd()` (Z. 27–29): Die vorgeschlagene Kommandozeile lautet künftig `node scripts/skipped_fiktiv.cjs --nachtrag <id> --entscheidung <…> --bars scripts/nas100_5m_<datum>.json` (Bars-Form, X4). Die Hoch/Tief-Form nur noch als Fallback-Hinweis.
- `scripts/protokoll_bilanz.cjs`: neue Abweichungszeile „PASS-FREIGABE OHNE ABLEHNUNGSGRUND AUSGELASSEN“. Sie greift bei Einträgen des Tages mit `grundVon(r) === 'freigegeben'`, `entscheidung === 'ausgelassen'` und ohne Begründungsfeld.
  - Falls `skipped_fiktiv --nachtrag` noch kein Begründungsfeld hat: optionales `--grund-auslassen "<Text>"` ergänzen, als Feld `auslass_grund`.
  - Das ist das Vorab-Kriterium „0 Auslassungen ohne Grund bei PASS“. Exit-Verhalten wie bei den bestehenden Abweichungen.
- **Tests:**
  - Setup 3 vom 29.09. mit `nas100_5m_2026-09-29.json` ohne `--bis` ergibt **OFFEN +0,84 R, exit_ts 18:00:00Z** (heute ergäbe das SL-HIT −1 R, Kerze 20:40).
  - Mit `--bis 2026-09-29T17:55:00Z` ergibt es +0,69 R.
  - Ein Nachtrag an einem Divergenztag (z. B. 26.10.2026) rechnet 20:00 DE korrekt in UTC um.
  - protokoll_bilanz meldet einen konstruierten GELB-Eintrag „ausgelassen ohne Grund“ und schweigt bei einem mit Grund.
- **Vorab-Kriterium:** 0 PASS-Auslassungen ohne Grund an 3 Testtagen. Stand: 29.09. = 1/3 (die Situation trat nicht ein).
- **Rückbau:** keiner nötig, der Default entspricht der bisherigen Praxis. Falls Levi doch die 19:50-Kerze meint, eine Konstante ändern **und** `tagesmomente` klären (siehe 1b; davon rate ich ab).

**A2: Abdeckungsanzeige in `tagesmomente.cjs --auswertung` (nur Anzeige)**
- In `tageInfo()` (Z. 245–254) je Tag berechnen: `abdeckung` = Anteil der 5-Min-Slots im Fenster [Session-Start; 20:00) mit mindestens einem Voll-Check-Moment (`de_hm`). Normaltag 54 Slots. An Divergenztagen gilt 14:30, also mehr Slots.
- Konstante `ABDECKUNG_SOLL = 0.80`.
- Ausgabe in der „Tage:“-Zeile: `… , Abdeckung 43 % (TEILTAG, zählt — Levi 30.09.2026)`.
- Zusätzlich **eine Referenzzeile** „Ohne Teiltage (< 80 %), NICHT maßgeblich:“ mit n / Σ R / Ø R je Variante, nach dem Muster der `--seit`-Referenz (Z. 289).
- Die **FREEZE-ENDE-Zeile und die Bewertbarkeit bleiben unverändert** (Entscheidung a).
- Tests: Die Scratch-Kopie mit 25.09., 28.09. und 29.09. liefert 50 % / 93 % / 43 %. Die Freeze-Zeile ist byte-identisch zu vorher. Ein Divergenztag rechnet mit 14:30.
- Vorab-Kriterium: Der 29.09. erscheint mit 43 % und der 25.09. mit 50 % als Teiltag.
- Rückbau: nicht nötig, es ist nur eine Anzeige.

**A3: IBF-Schattenauswertung, neues Skript `scripts/analyse/ibf_schatten.cjs` (nur lesend)**
- Liest `oneh_shadow_log.jsonl`, `gate_check_log.jsonl` und `nas100_5m_<datum>.json` je Tag.
- Kopiert die Shadow-Einträge **im Speicher**. Wo `blockade_3v4 === true` und `sign·(kurs − ema50_1h) > 0` gilt, wird `bias['1h'] = null` gesetzt. Danach `require('../tagesmomente.cjs').bewerteTag(...)` aufrufen, einmal mit Original und einmal mit modifiziertem Shadow.
- **Schreibt nichts**, weder in `momente_log.jsonl` noch sonstwo. Nur stdout, optional `--json`.
- Ausgabe:
  - (a) Episodentabelle: erster IBF-fähiger Moment je Blockade-Episode; eine Episode ist ein zusammenhängender Blockade-Lauf gleicher Richtung, Lücke ≤ 20 Min. Dazu ALT/AUTO/FLOOR-Ergebnis, ampel_min/ampel_max und der erste Moment mit ampel_max ≠ ROT.
  - (b) Kettendelta gegenüber heute bei Gewicht 0,5 für freigegebene Momente (Methode siehe `halb_sim.cjs` in diesem Scratch-Ordner), je Tag und Variante.
  - (c) Prüfung der Vorab-Kriterien 1–5 aus Abschnitt 2.3, Primärvariante AUTO.
  - (d) Hinweiszeile „Messung, kein Gate; Live-Entscheidung erst nach Freeze-Ende (Levi 30.09.)“.
- Standard: alle Tage mit Bars-Datei. `--seit` optional.
- **Abnahme:** reproduziert die Zahlen aus Abschnitt 2.2:
  - FLOOR S2 Δ +1,45 / S3 Δ +2,50, ohne 15.09. −1,05 / ±0,00
  - ALT ±0 in allen Szenarien, AUTO S2 −0,50
  - Episoden wie in der Tabelle, 23.09. ROT
- Tests: mindestens 1 Fixture-Test (Mini-Shadow-Log + Bars) plus der Nachweis, dass `momente_log.jsonl` unverändert bleibt (Hash vorher/nachher).

**Opus-Gegencheck-Punkte A:**
- Default-`--bis` wirkt nur bei `--bars`, nicht bei der Hoch/Tief-Form.
- Kein Einfluss auf vorhandene Nachträge (keine Neuberechnung beim Lesen).
- Sommerzeit-Umrechnung richtig.
- protokoll_bilanz ohne Fehlalarm bei ROT-Einträgen (die haben einen Grund).
- Abdeckung: FREEZE-Zeile byte-identisch zu vorher.
- ibf_schatten: kein Schreibzugriff; das Ergebnis stimmt mit meinen Zahlen überein.
- Lauf gegen Scratch-Kopien, nie gegen die Live-Logs schreibend.

### Auftrag B: Live-Schatten- und Anzeige-Paket (Q3-auto, Drift-Hinweis, Kleinigkeiten)

**Ziel:** Weniger Ermessen für Sonnet (Determinismus), keine Änderung an Gates, Schwellen oder Q-Faktoren. Umsetzen erst **nach dem Loop-Stopp heute**, dann ab dem nächsten Testtag aktiv.

**B1: Q3-auto als Schattenzeile in `scripts/gate_check.cjs`, nur im Live-Modus**
- Quelle der Beine:
  - **5m** aus den Gate-Argumenten: `--entry` gegen `--ema50-5min`, beide A3-Pflicht.
  - **1H** aus `--override-1h-close` gegen `--override-1h-ema50`, A3-Pflicht.
  - **15m NAS** und **QQQ** aus dem **jüngsten Eintrag in `oneh_shadow_log.jsonl`**: gleicher DE-Tag, ts ≤ `--jetzt` + 60 s, Alter ≤ 360 s.
  - Neues optionales Argument `--oneh-shadow-log <Pfad>` für Tests, Standard `scripts/oneh_shadow_log.jsonl`.
- Urteil:
  - `ERFUELLT`, wenn alle 4 Beine == `--dir`.
  - `NICHT ERFUELLT`, wenn ein Bein gerichtet dagegen steht oder neutral ist.
  - `UNBEKANNT`, wenn ein Bein fehlt, das Log veraltet ist oder die Argumente fehlen.
- Ausgabezeile (nach der KOMBI-SCHATTEN-Zeile):
  `Q3-AUTO (Schatten, kein Gate): <ERFUELLT|NICHT ERFUELLT|UNBEKANNT> — 5m <..> | 15m <..> | QQQ <..> | 1H <..> (Shadow-Log VC#<n> <hh:mm:ss>, Alter <s> s) | manuell --q3-coherence <yes|no|fehlt> -> KONSISTENT | WIDERSPRUCH`
- Log-Feld additiv in `gate_check_log.jsonl`: `q3_auto: {status, beine, quelle_ts, manuell, widerspruch}`. Keine bestehenden Feldnamen ändern (Kommentar Z. 4037).
- **Der Q-Score rechnet weiter mit dem manuellen `--q3-coherence`.** Q3-auto ändert nichts an Ampel, Größe oder Exit.
- Konstante `Q3_AUTO_MODUS = 'schatten' | 'aus'`. Bei `'aus'` erscheinen weder Zeile noch Feld (Rückbau).
- **Golden-Files:** Test B-1 (`tests/trading_scripts.test.js` Z. 4230) verlangt Byte-Gleichheit „bis auf die KOMBI-SCHATTEN-Zeile“. Die Q3-AUTO-Zeile **genauso zeilengenau ausnehmen**, die Goldens **nicht neu erzeugen**. Opus prüft, dass die Ausnahme nur exakt diese Zeile trifft.
- Tests: Nachstellung 29.09. 18:11 (alle short, ERFUELLT, konsistent) · Widerspruch (manuell yes, 1H dagegen) · Shadow-Log älter als 360 s ergibt UNBEKANNT · anderer Tag ergibt UNBEKANNT · `'aus'` ergibt eine Ausgabe identisch zu HEAD.
- **Vorab-Kriterium:** 0 unentdeckte Widersprüche und 0 UNBEKANNT an 3 Testtagen. Stand 1/3: 29.09. rückwirkend, trivialer Fall. Tag 2 und 3 sind die nächsten Testtage mit Live-Läufen.
- Rückbau: `Q3_AUTO_MODUS = 'aus'`.

**B2: Drift-Hinweis und Prozessregel „TP1/SL aus dem Live-Lauf“**
- Die Regel steht heute **nirgends**. Gesucht in `loop_prompt.cjs` (Z. 213 beschreibt den Live-Gate-Aufruf, ohne diese Regel), in `feedback_live_trading.md` (7b1 Schritt 5) und in `feedback_vollcheck_format.md`, jeweils ohne Treffer für „aus dem Live-Lauf“, „nicht aus der Vorprüfung“ oder eine vergleichbare Formulierung.
- (a) **Text** in `scripts/loop_prompt.cjs`, Block Z. 213, einen Satz ergänzen: „TP1 und SL für den Live-Gate IMMER aus den W3-/Fenster-Zeilen DIESES Laufs bzw. neu für den aktuellen Entry bestimmen, nie aus der Vorprüfung von vor 1–3 Min übernehmen (29.09.: zwei FAIL-Läufe aus diesem Grund).“ Denselben Satz in `feedback_live_trading.md`, 7b1 Schritt 5.
  - Die Selbsttest-Fragmente von loop_prompt beachten (Testdatei Z. 53 ff.).
- (b) **Determinismus** in `gate_check.cjs` Live-Modus, **nur als Hinweiszeile**:
  - Bei `--sl-vorpruefung-ref last` den Entry der referenzierten Vorprüfung mit `--entry` vergleichen.
  - Ist |Δ| ≥ 0,25×ATR, ausgeben: `DRIFT-HINWEIS (kein Gate): Entry seit Vorprüfung <Δ> Pkt gedriftet — SL-Floor für DIESEN Entry wäre <x> (übergeben <sl>); TP1-Kandidaten dieses Laufs siehe W3-Zeile`.
  - Der SL-Floor wird mit derselben Funktion gerechnet wie `--sl-auto` in der Vorprüfung (Z. 1636 ff.).
  - Kein Einfluss auf Exit oder Status.
  - Beleg 29.09.: Lauf 3 hätte SL 30366,35 statt 30377,35 gehabt, RR 1,76 statt 1,49.
  - Test mit genau diesem Fall.

**B3: kerzen-qqq-Heuristik als Hinweis statt Warnung, an zwei Stellen**
- `scripts/vollcheck.cjs` Z. 1473: `WARNUNG: QQQ-Zähler < NAS100/3 — vermutlich zu niedrig (A4)` wird zu `HINWEIS (kein Fehler, A4): QQQ-Zähler < NAS100/3 — plausibel, wenn QQQ später als NAS100 die Seite gewechselt hat; nur prüfen, falls beide gleichzeitig kreuzten`.
- `scripts/gate_check.cjs` Z. 2986: `PLAUSIBILITAETS-WARNUNG kerzen-qqq` wird entsprechend zu `PLAUSIBILITAETS-HINWEIS`.
- Vorher prüfen, ob ein Skript auf „WARNUNG“ matcht.
  - In Tests und Fixtures gibt es keinen Treffer auf „vermutlich zu niedrig“.
  - „⚠ Prozess:“ in `loop_archiv/2026-09-23.txt` Z. 2681 stammt aus **keinem** Skript (grep in `scripts/*.cjs` ohne Treffer). Das ist Sonnets eigene Hervorhebung im Chat. Genau das soll die Umformulierung dämpfen: Das Wort WARNUNG zieht Sonnets Aufmerksamkeit auf einen Fehlalarm.

**B4: A2-Zähler „X1 FAELLIG seit N“ nur echte Fällig-VCs**
- `scripts/vollcheck.cjs` `x1StatusAus()` Z. 395–409: `n` zählt heute alle Exit-0-Slots seit dem ersten Fälligwerden, auch nicht fällige. Am 29.09. hieß das „8 VCs“, echt fällig waren 2 (17:31, 18:06).
- Neu: `n` zählt nur Slots, in denen die Vorprüfung des Tages mit gleichem Anker und gleicher Richtung eine Zeile `ANKER-RESET FAELLIG (X1` im `output` von `gate_check_log.jsonl` trug (Muster wie bei der A8-Pflichtsuche, Z. 413–428), plus den laufenden Slot, wenn dieser fällig ist.
- Anzeige: `X1 FAELLIG in <n> Voll-Checks seit <hh:mm:ss> DE (davon zuletzt <k> in Folge)`.
- `nPflicht` (A8) bleibt unverändert.
- Tests in `trading_scripts.test.js` Z. 4527–4903 anpassen. Die Regex-Tests enthalten „X1 FAELLIG seit \d+ Voll-Checks?“, also Format und Semantik bewusst zusammen ändern.
- Nachstellung 29.09. ergibt n = 2, nicht 8.

**B5: Session-Extrema ins Register, nur Prozesstext**
- Das Werkzeug gibt es schon: `register_touch.cjs --session-hoch/--session-tief` (Z. 238–246).
- In `loop_prompt.cjs` ergänzen: „Neues US-Session-Hoch/-Tief (ab 15:30) → im selben Voll-Check `register_touch.cjs --session-hoch|--session-tief <Preis> --note "VC#n"`“.
- Anlass: 29.09. fehlte das Session-Tief 30256,55, am 28.09. das Tagestief 30081,85.
- Kein Code in vollcheck, weil vollcheck keine NAS-Session-Extrema als Eingabe kennt. Das wäre eine neue Pflichteingabe und damit Tempo-Kosten.

**Freeze-Konformität B:**
- B1, B2b, B3 und B4 sind reine Anzeige/Schatten.
- B2a und B5 sind Operator-Disziplin ohne Gate-Wirkung.
- Keine Änderung an Q-Faktoren, Schwellen oder Veto.
- Als Paket gilt B als **die eine Änderung des nächsten Testtags**.

**Opus-Gegencheck-Punkte B:**
- Golden B-1 ist nur um genau die Q3-AUTO-Zeile ausgenommen.
- Q-Score/Ampel/Exit ohne Unterschied mit und ohne Q3-auto, auf 300 gelogten Live-Läufen per Replay: nur die neue Zeile und das neue Feld unterscheiden sich.
- DRIFT-SL-Floor identisch zu `--sl-auto`.
- vollcheck-Parsing der Vorprüfungszeilen durch B3 unverändert.
- A2-Zähler am 29.09. ergibt 2.
- `npm test` grün (Referenz: HEAD 210/210, von mir am 30.09. in einer isolierten `git archive`-Kopie verifiziert).
- `test:e2e` NICHT ohne Vorwarnung an Levi.

**Nicht beauftragt (Backlog bis nach dem Freeze):** IBF live, AUTO-Anker-Nachführung, Code-Filter für veraltete Register-Level, `.gitattributes` für Golden-Fixtures (siehe 4, klein, kann Levi beim Commit mit erledigen).

---

## 4. Lückencheck: Was fehlt vom 28./29.09.

### FEHLT WIRKLICH (mit Beleg)

1. **Offizieller `tagesmomente`-Lauf 29.09.:** `scripts/momente_log.jsonl`, mtime 28.09. 20:05. Die Tageszählung enthält 11./15./21./23./25./28.09., **kein 2026-09-29**.
   - Folge: Das Repo zeigt heute **2/5, AUTO 5/20**. Erst nach dem Lauf gilt Levis a) mit 3/5, AUTO 6/20.
   - Nachholen: Auftrag 0.1, Fable. Die 5m-Bars liegen vor (`nas100_5m_2026-09-29.json`, 20:41, Pflichtfenster 66/66).
2. **Memory-Git-Backup:** Letzter Commit `922fafe` vom 2026-09-23 11:16. Seitdem 9 geänderte Dateien (u. a. MEMORY.md, feedback_live_trading.md, feedback_tagesabschluss.md) und 10 ungetrackte (u. a. `project_testtag_2026-09-23_besprechung_ausstehend.md` mit der **Freeze-Definition**, alle Testtag-Analysen vom 23. bis 29.09., der P1–P3-Stand, die Agenda).
   - Die Tagesabschluss-Routine („Git-Backup“) fehlt damit seit einer Woche. Das ist **der größte Risikopunkt**, auch mit Blick auf den Mac-Umzug.
3. **Repo nicht gepusht: 2 Commits** (`main...origin/main [ahead 2]`: `57c711a` Kombi/V2 **und** `ee27c70`). Die Agenda nennt nur ee27c70. `origin/main` steht auf 941a221.
4. **Q3-auto, Tagesende-Regel, Abdeckungsanzeige:** nicht umgesetzt. In `gate_check.cjs` gibt es keine Q3-auto-Logik, `q3_auto` existiert nur als Messfeld in tagesmomente. `skipped_fiktiv` hat keinen Default-`--bis`. `tageInfo()` kennt keine Abdeckung. Abgedeckt durch Auftrag A und B.
5. **Levi-Entscheidung P3-Verlängerung fehlt:** Mein Vorschlag vom 29.09. (Bericht D): Das 3-Tage-Urteil gilt nur mit ≥ 1 impuls-fälligem Fall, sonst wird bis +2 Tage verlängert. Das stand **nicht** in der Agenda a–d und ist deshalb nicht entschieden. Der 29.09. ist P3-Tag 1/3 mit 0 einschlägigen Fällen. **Bitte Levi vorlegen.**
6. **.gitignore-Lücken**, bei ungetrackten Dateien ist das der Hauptgrund:
   - `scripts/kombi_fiktiv_log.jsonl` (5 Einträge, 22,8 KB) ist **nicht** ignoriert, das Geschwister-Log `skipped_setups_fiktiv.jsonl` dagegen schon. Das ist ein Versehen aus 57c711a. **Ignorieren**, nicht committen (Log, ändert sich täglich).
   - `scripts/last_*_out.txt` (4 Dateien, 25.–28.09., veraltete Ein-Slot-Puffer wie das bereits ignorierte `last_vollcheck.txt`): **ignorieren oder löschen**.
   - `scripts/*.json.bak` (`nas100_15m/60m_2026-09-16.json.bak`): **nicht löschen**. Das ist die **einzige** Kopie der 1H/15m-Bars vom 16.09.; `nas100_60m_2026-09-16.json` gibt es nicht. Ignorieren und ins Daten-Backup aufnehmen.
   - `scripts/loop_archiv/` (2026-09-23.txt 723 KB, 2026-09-29.txt 263 KB): Tagesprotokolle, händisch zusammengesetzt, kein Produktivskript schreibt dorthin (GAP seit 25.09.). **Ignorieren** wie die anderen Logs und ins Daten-Backup aufnehmen. Alternativ, falls Levi die Protokolle versioniert haben will: ins Memory-Repo statt ins Code-Repo.
   - `scripts/analyse/out/` bleibt laut Memory bewusst uncommittet: **ignorieren**.
7. **Backup-würdig im Code-Repo (committen, gezielt `git add <datei>`, kein `-A`):**
   - `scripts/analyse/tagesmomente_alt_check.cjs`: wird in `feedback_tagesabschluss.md` Z. 192 als **Prüfwerkzeug** referenziert, ist aber ungetrackt.
   - Optional `scripts/analyse/backtest_2026-09-24.cjs` und `backtest_2026-09-25.cjs` (Einmal-Analysen, auf die Memory-Dateien verweisen; die übrigen analyse-Skripte sind bereits getrackt).
8. **Lokale Daten allgemein:** Alle Logs (`*.jsonl`), Bars (`nas100_*.json`, `qqq_*.json`), `trades.db` und `level_register.json` sind per `.gitignore` **nur lokal**. Die Memory-Notiz „trades.db vor Umzug sichern“ greift zu kurz: **vor dem Mac-Cutover ein vollständiges Daten-Backup** (Zip von `scripts/*.jsonl`, `scripts/*.json`, `scripts/*.bak`, `scripts/loop_archiv/`, `trades*.db`). Ohne die Logs ist die Freeze-Auswertung nicht reproduzierbar.
9. **`.gitattributes` für die Golden-Fixtures:** nicht angelegt (Datei fehlt; `core.autocrlf=true`). Das Risiko ist im P1–P3-Memory dokumentiert. Klein, aber vor einem Windows-Neuklon nötig. Am Mac unkritisch.

### ERLEDIGT (mit Beleg)

- **A1/A2 + P1–P3:** committet `ee27c70` (29.09. 16:28, 20 Dateien u. a. gate_check/vollcheck/tagesmomente/anker_auto). **Tests 210/210 grün**, von mir am 30.09. in einer isolierten `git archive HEAD`-Kopie verifiziert (`node --test tests/trading_scripts.test.js tests/tagesmomente.test.js`: 210 pass, 0 fail). `RESET_PFLICHT_MODUS = 'pflicht'` (gate_check Z. 956).
- **5m-Bars 29.09. gesichert:** `nas100_5m_2026-09-29.json` (20:41), Pflichtfenster lückenlos.
- **Skipped-Nachträge 29.09.:** 3 von 3 mit Ergebnis (+0,44 / +0,78 / +0,84 R, `nachtrag_ts` 18:42Z, mit `--bis 18:00Z`).
- **Kombi-Nachtrag 29.09.:** 1 von 1 (`r_primaer` +0,55, `bis_quelle --bis`, 20:00 DE).
- **loop_stopp** protokolliert (20:41).
- `add_trade.cjs`: **nicht einschlägig** (fiktiver Tag, 0 Trades).

### UNKLAR (nicht abschließend prüfbar)

- **Faktenprotokoll / 9 Pflicht-Abschlusszeilen 29.09.** (`feedback_tagesabschluss.md` Z. 190): kein Treffer für „Tagesmomente gerechnet:“ oder „Kombi-Nachtrag: JA“ in Memory oder `loop_archiv`. `memory/testtag/` endet am 15.09. Es ist unklar, ob es seit dem 23.09. überhaupt eine Protokolldatei gibt; laut 25.09.-GAP ist `protokoll_bilanz.cjs` ohne sie nicht lauffähig. Wahrscheinlich fehlt der formale Abschluss auch für 25./28.09. **Das sollte Levi bewusst entscheiden**: nachholen oder `loop_archiv` als Protokoll-Ersatz festlegen.
- **1H/QQQ-Bars 29.09.** (`nas100_60m_2026-09-29.json`, `qqq_15m_2026-09-29.json`) fehlen, am 24., 25. und 28.09. wurden sie gesichert. Pflicht sind laut Memory nur die 5m-Bars. Für spätere 1H-/IBF-Nachrechnungen wären sie nützlich, zurückholbar per `data_get_ohlcv` solange TradingView noch so weit zurück liefert.
- **„Sonstiges offen“ der Agenda:** Remote Control (Levi, Prozess), Mac-Cutover nach dem Testtag-Block, trades.db sichern (siehe 8, erweitert). Stand für mich nicht prüfbar.

### Fehler in der Agenda-Datei `project_morgen_agenda_2026-09-30.md`

1. Z. 11: „**Di** 30.09.2026“ ist falsch. Der 30.09.2026 ist ein **Mittwoch** (`date`: Wed Sep 30; der 29.09. war Dienstag).
2. Z. 32: „HEAD ee27c70 (nicht gepusht)“ ist unvollständig. **2 Commits** sind nicht gepusht (57c711a + ee27c70).
3. Z. 32: Ungetrackt ist nicht nur `loop_archiv/2026-09-29.txt`, sondern auch `loop_archiv/2026-09-23.txt`, dazu `scripts/analyse/tagesmomente_alt_check.cjs` (Prüfwerkzeug) und die beiden backtest-Skripte.
4. Z. 28 (Auftrag 2) unterstellt einen heutigen Stichtag „20:00“ mit Rückbau dorthin. Das gilt für kombi und tagesmomente, **aber nicht für `skipped_fiktiv`**, das keinen Default hat.
5. Die Agenda erwähnt das **fehlende Memory-Backup** (seit 23.09.) und den **fehlenden momente_log-Lauf 29.09.** nicht, obwohl Letzterer im Opus-Bericht (Z. 5) stand.
6. Die P3-Verlängerungsfrage (Bericht D) fehlt unter den Entscheidungen.
7. Kleinigkeit: `project_testtag_analyse_2026-09-29.md` Z. 19 („Skript zählt 3/5“) bezieht sich auf die Scratch-Kopie. Im Repo steht bis zum offiziellen Lauf 2/5.

---

## 5. Was Levi noch rückbestätigen oder entscheiden muss (kurz)

1. **b)** „19:55er Kerze = öffnet 19:55, schließt 20:00 (+0,84 R, heutiges Verhalten)“: ja oder nein.
2. **a)** Gilt „Teiltag zählt“ auch für künftige Tage, als Regel und nicht nur für den 29.09.? (Empfehlung: ja, mit Abdeckungsanzeige.)
3. **c)** Override bleibt live unverändert bis zum Freeze-Ende, die IBF wird nur schattengemessen, Vorab-Kriterien wie in 2.3: ja oder nein.
4. **P3-Verlängerung:** 3-Tage-Urteil nur mit ≥ 1 impuls-fälligem Fall, sonst bis +2 Tage.
5. **Protokoll-Ersatz:** Faktenprotokoll nachholen oder `loop_archiv` als Tagesprotokoll festlegen.
6. **Push** der 2 Commits, **Memory-Backup**, **Daten-Backup** vor dem Mac-Cutover. *[Nachtrag 30.09.: Push/Commit ausschließlich auf Levis Befehl, nicht durch Sonnet/Fable — [[feedback_commit_push_nur_levi]]; die 2 Commits wurden am 30.09. im Incident ohne Auftrag gepusht.]*
