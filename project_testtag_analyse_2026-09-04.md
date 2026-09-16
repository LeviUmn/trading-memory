---
name: project_testtag_analyse_2026-09-04
description: "Opus-Analyse Testtag 04.09.2026 — Vergleich zu 03.09., erster Live-Test der P1-P7-Mechanismen (ENTWURF, ungelesen von Levi)"
metadata: 
  node_type: memory
  type: project
  status: draft
  originSessionId: 64bc5dd1-d029-45dc-8bd7-8f3b918a2940
  modified: 2026-09-04T18:14:26.394Z
---

## Auslöser und Prüfumfang

Erster Live-Testtag nach der Regelwerksrunde vom 04.09. (P1-P7, siehe [[project_testtag_analyse_2026-09-03]]). Geprüft wurde [[testtag/testtag_2026-09-04]] (`memory/testtag/testtag_2026-09-04.md`) vollständig (2.353 Zeilen, Sessionstart 15:00, Faktenprotokoll-Abschluss 20:00) im Vergleich zur Vortagesanalyse. Kernfrage laut Auftrag: Haben die neuen Mechanismen in der Praxis getragen, und was blockierte heute die 4 Dual-Gate-2/2-Trigger?

Vorweg das Wichtigste: **Der Tag ist prozessual der bislang sauberste der Serie** — die vier am 03.09. diagnostizierten Prozessfehler sind nachweisbar behoben, nicht wiederholt. Gleichzeitig hat der Tag drei Dinge sichtbar gemacht, die vorher unter dem RR-Gate verdeckt lagen: eine strukturelle Spannung zwischen 8c2 und der TP1-Fenster-Arithmetik, ein asymmetrisch wirkendes 1H-Kriterium, und drei nachweislich falsche Zahlen im eigenen Faktenprotokoll-Abschluss.

## Tagesverlauf in Kürze

NFP-Beat (162,0K vs. 56,0K Konsens) um 14:30 löst vor Sessionstart einen NAS100-spezifischen Abverkauf aus (29.660,8 → 29.441,0, ca. 220 Pkt), bei fallendem VIX/VXN und starkem DJI — korrekt als Sektor-Rotation und nicht als breites Risk-off eingeordnet. Ab 15:05 V-förmige Erholung, die den gesamten Schock binnen 90 Minuten fast vollständig zurückholt (Hoch 29.654,25 um 16:00). Danach eine ca. 50-minütige Verteilungsrange (29.560-29.655), die um 16:20 nach unten bricht, und ein durchgehender Abwärtstrend bis 17:31 (Test des Schock-Tiefs). Abends erneute Erholung bis 29.549,85 (18:49), dann ein zweiter, tieferer Abverkauf, der um 19:42 mit 29.431,95 das ursprüngliche NFP-Schock-Tief unterschreitet. Schluss ca. 29.460.

Zwei Einordnungen dazu, die im Protokoll nur angerissen sind:

Erstens war der Abwärtsimpuls 16:00→19:42 mit 222 Pkt exakt so groß wie der NFP-Schock selbst — nur ohne Auslöser-Meldung. Der Loop hat das korrekt beobachtet und mehrfach als "beobachtungswürdig" vermerkt, ohne 8d anzufassen. Das ist richtig: keines der drei 8d-Kriterien war erfüllt (Range/ATR-D 0,49x, nur 1 Makro-Event, VIX unter Vortagesschluss). Dass ein 222-Pkt-Move an einem Tag mit ATR-D 446,9 formal kein Schock-Regime ist, ist konsistent, aber es zeigt, dass 8d bewusst nur sehr grobe Ausreißer fängt.

Zweitens: Der 1H-Bias war den ganzen Tag bullisch, durch beide Abverkäufe hindurch, und wurde erst um 19:45 überhaupt rechnerisch gefährdet (Abstand des laufenden Bars zur 1H-EMA50 auf ~32 Pkt geschrumpft, danach wieder auf ~44 Pkt entspannt). Ursache ist rein mechanisch: die 1H-EMA50 lag den gesamten Tag zwischen 29.392 und 29.405, während der Kurs zwischen 29.432 und 29.661 handelte. Der 1H-Bias war damit keine Variable, sondern über fünf Stunden eine Konstante.

## Befunde

### F1 — Drei nachweislich falsche Zahlen im Faktenprotokoll-Abschluss

Das ist der einzige Befund, der über eine Bewertung hinausgeht und eine Tatsachenkorrektur ist. Der Abschluss vom 20:00 Uhr enthält drei Aussagen, die dem eigenen Protokolltext widersprechen:

1. **`--chasing` (Abschnitt 6, Pflicht-Abschlusszeile):** "heute nie mit `--chasing yes` ausgelöst (alle vier Trigger hatten `--chasing no`)". Das ist falsch. Bei VC#7 (15:35) steht wörtlich im Protokoll: "Chasing-Kriterium erfüllt, `--chasing yes` gesetzt", der Aufruf enthält `--chasing yes`, und die Skriptausgabe meldet "Chasing-Status (Punkt 13, P6): JA — Chasing-Situation (--chasing yes) -> halbe Position" sowie das Sizing-Flag "halbe Position" in der GESAMTSTATUS-Zeile. Von vier Triggern hatte einer `yes`, drei `no`. Der Befund ist inhaltlich relevant, weil die Abschlusszeile den Live-Status von P6 falsch in den Speicher schreibt: P6 wurde nicht "nie ausgelöst", sondern der `yes`-Pfad wurde erreicht und korrekt in ein Sizing-Flag übersetzt — nur die tatsächliche Positionsgrößen-Konsequenz blieb ungetestet, weil derselbe Aufruf am RR- und am 8c2-Gate scheiterte.

2. **Voll-Check-Zahl:** "#1 (15:05) bis #59 (19:55) = 59 vollständige Voll-Checks". Tatsächlich fielen vier nummerierte Slots ersatzlos aus (#8/15:40, #18/16:30, #46/18:50, #48/19:00), alle vier im Protokoll selbst offengelegt. Real durchgeführt: **55**. Die Nummerierung ist real-zeit-verankert und damit korrekt, die Summe daraus nicht.

3. **Register-Updates:** "level_register.json-Updates: 3" — und dann werden in derselben Zeile vier Zeitpunkte aufgezählt (15:01, 15:50, 18:51, 19:45). Es waren **4**.

Keine dieser Abweichungen verschleiert etwas — die Rohdaten stehen jeweils vollständig im Protokoll, und Abschnitt 4 listet die Lücken korrekt auf. Es sind Rechenfehler in der Selbstbilanz, keine Vertuschung. Die Fehlerklasse ist aber identisch mit der vom 28.08. (falsche Screenshot-Bilanz) und damit ein Rückfall in ein bereits einmal adressiertes Muster: die Zahlenbilanz wird geschrieben, ohne gegen den eigenen Protokolltext nachgezählt zu werden.

### F2 — P1 Retest-Zeitbox: funktioniert, mit zwei präzise benennbaren Lücken

Das ist der klarste Erfolg des Tages. Vier Zeitboxen eröffnet, alle vier vollständig abgearbeitet, in jedem betroffenen Folge-Voll-Check eine Pflichtzeile mit Kurs, Fenstergrenzen und Konsequenz. Verglichen mit dem 03.09., wo nach einem FAIL 35 Voll-Checks ohne jeden Neu-Aufruf verstrichen, ist das ein qualitativer Sprung. Der vollständige Zyklus lief live durch: FAIL (18:49) → Zeitbox → Bedingung erfüllt (18:55, Kurs im Entry-Fenster) → Neu-Aufruf → erneutes FAIL → neue Zeitbox → VC+1 nicht erfüllt (19:05) → VC+2 nicht erfüllt (19:10) → VERFALLEN. Das ist genau der spezifizierte Ablauf.

Zwei Schwachstellen, beide real, beide heute ohne Schaden:

**(a) Die Zeitbox misst in Voll-Checks, nicht in Uhrzeit — bei Protokolllücken dehnt sie sich still.** Nach VC#7 fiel der Slot 15:40 aus; der Loop wertete VC#9 (15:45) als *ersten* Check statt als zweiten, mit der ausdrücklichen Begründung "VC+1 hat faktisch nie stattgefunden". Beim 19:00-Ausfall wurde identisch verfahren (VC+1 auf 19:05, VC+2 auf 19:10 verschoben). Die Auslegung ist in sich schlüssig, konsistent angewandt und transparent offengelegt — aber sie steht nicht in der Spezifikation, und sie wirkt immer in Richtung Verlängerung. Effektiv war das Fenster in beiden Fällen 5 Minuten länger als geplant. Bei mehreren aufeinanderfolgenden Ausfällen wäre eine Zeitbox theoretisch beliebig dehnbar. Die Frage, ob P1 zusätzlich eine Wanduhr-Obergrenze braucht (z.B. "max. 2 Voll-Checks ODER 15 Minuten, was zuerst eintritt"), ist damit empirisch aufgeworfen.

**(b) Die Neu-Aufruf-Bedingung kann einen garantiert erfolglosen Aufruf auslösen.** Das ist der handfestere Punkt. Um 18:55 war die Bedingung "Kurs im Entry-Fenster" erfüllt, also wurde neu aufgerufen — mit **demselben strukturellen SL 29.511,95**, der 6 Minuten zuvor am 8c2-Gate gescheitert war. Das Ergebnis stand vorher fest, und das Skript sagt es in seiner eigenen Ausgabe: "IDENTISCHER Fehlgrund wie beim 18:49-Aufruf, Struktur-SL hat sich nicht verändert". Ursache ist systematisch: das Entry-Fenster wird aus RR≥1, Zone-3-Grenze und 8c-SL-Floor berechnet — alles entry-abhängige Schranken. 8c2 ist dagegen rein SL-abhängig und kann per Konstruktion nie in einem Entry-Fenster abgebildet sein. Das ist die direkte Fortsetzung des eigenen B6-Befunds vom 04.09. ("Entry-Fenster deckt nur 2 von 3 entry-abhängigen Hard-Gates ab"): B6 wurde für den 8c-Floor geschlossen, aber die verbleibende Lücke ist konzeptioneller Natur — ein SL-abhängiges Gate lässt sich nicht durch eine Entry-Schranke abbilden. Die Neu-Aufruf-Bedingung müsste unterdrückt werden, solange der FAIL-Grund ein nicht-entry-abhängiges Gate ist und der SL unverändert ist. Kosten heute: ein verschwendeter Aufruf. Kosten an einem Echtgeldtag: 3-4 Minuten Loop-Zeit im ungünstigsten Moment.

Eine dritte, heute nur zufällig unkritische Konstellation: Um 15:50 fielen der letzte Check der ersten Zeitbox (VERFALLEN) und ein neuer, eigenständiger 2/2-Trigger auf denselben Voll-Check. Der Loop hat das sauber getrennt behandelt. Wäre der neue Trigger fünf Minuten früher gekommen, hätte P1 keine Regel dafür gehabt, ob ein frischer Trigger eine laufende Zeitbox verdrängt oder neben ihr läuft.

### F3 — `--chasing`: angewandt, korrekt gefüllt, Konsequenz weiterhin ungetestet

Die Auftragsprämisse ("heute nie auf yes gesetzt") stimmt nicht, siehe F1. Die tatsächliche Anwendung war:

- **VC#7 (15:35): `yes`.** 5 bestätigt grüne 5min-Kerzen in Folge ohne Konsolidierungspause seit dem Pullback-Tief. Nach dem Wortlaut von Punkt 13 (4-5 Kerzen ohne Pause) korrekt. Die Bewertung ist konservativ — der RSI(5min) stand bei 56,4, also nicht im Erschöpfungsbereich, und der Ausgangspunkt war ein Schock-Tief. Der Loop hätte auch `no` argumentieren können; er hat sich für die vorsichtige Variante entschieden. Das ist der richtige Fehler, falls es einer ist.
- **VC#10 (15:50): `no`.** 1 Kerze seit dem Gegenseiten-Wechsel. Zwingend richtig.
- **18:49: `no`.** Diskutabel. NAS100-Ausbruch 2 Kerzen alt nach 30-minütiger Konsolidierung — das trägt `no`. Aber gleichzeitig lief QQQ die 11. grüne Kerze in Folge, und die P2-Eskalation war aktiv (Reifegrad 1,79x, Q2-Budget −19,4 %). Der Loop hat die Zählung korrekt am Entry-Instrument (NAS100) festgemacht, was der Regelwortlaut hergibt. Eine Regel, die die Erschöpfung des Bestätigungsinstruments (QQQ) mitberücksichtigt, existiert nicht — das ist eine offene Definitionsfrage, kein Fehler.
- **18:55: `no`.** Kurs 4 Pkt unter dem 18:49-Niveau nach Rücksetzer. Richtig.

Beachtenswert ist noch, dass der Loop bei VC#12 (16:00) selbst notierte: "Bei einem dritten Trigger-Versuch wäre `--chasing yes` zu setzen". Der nächste Trigger kam 2 Stunden 49 Minuten später, nach einem vollständigen Abwärtsimpuls und einer neuen Erholung — die Serie war vielfach zurückgesetzt, `no` war dort richtig. Es zeigt aber, dass der Chasing-Zustand rein manuell über lange Lücken mitgeführt wird und keinen definierten Verfall hat.

Fazit zu P6: Das Pflichtfeld hat seinen Zweck erfüllt (4/4 Aufrufe gefüllt, kein Hard-Exit wegen fehlendem Feld, `yes`-Pfad einmal live erreicht und korrekt in "halbe Position" übersetzt, ausdrücklich nicht kumulativ mit dem Halbierungsfenster). Was weiterhin aussteht, ist ein PASS-Fall mit `yes`, an dem sich die tatsächliche Positionsgrößen-Rechnung beobachten ließe. Das lässt sich nicht erzwingen, es braucht einen PASS.

### F4 — 8c2: strukturell verdächtig, nicht tageszufällig — aber nicht aus dem Grund, den man zuerst vermutet

Die Faktenlage: 8c2 war in **allen vier** Gate-Aufrufen FAIL und in zweien (18:49, 18:55) der **alleinige** Blocker. Bei VC#7 und VC#10 stand es neben einem knapp verfehlten RR-Gate. Kein anderes Gate ist heute auch nur einmal gefallen: SL-Mindestdistanz 4/4 PASS, TP-Realismus 4/4 PASS (Zone 1 oder 2), Geometrie 4/4 PASS, Schock-Tier 4/4 PASS.

Drei Beobachtungen, die zusammen die Antwort ergeben:

**(1) Die Auslegung ist strenger als "Puffer".** Die Skriptausgabe bei VC#10 formuliert es explizit: "SL muss die Zone unterschreiten, nicht nur 27,6 Pkt davon entfernt liegen". 8c2 ist damit keine Abstandsregel, sondern eine Platzierungsregel — der SL muss *jenseits* jeder kürzlich per Docht getesteten Zone liegen. Ein SL, der zwischen Entry und Zone liegt, fällt unabhängig vom Abstand durch. Bei VC#10 lag der SL 27,6 Pkt über der Zone und scheiterte.

**(2) Das kollidiert per Konstruktion mit der P4-SL-Anker-Definition.** P4 (Fables Fassung, am 04.09. übernommen) verlangt den SL am tiefsten Low seit dem letzten Impulshoch — also an einem Docht. 8c2 bestraft genau solche Dochte, wenn in der Nähe schon einmal ein Docht war. An einem Erholungstag mit mehreren Reclaim-Zyklen ist die nächstliegende Struktur *immer* ein kürzlich getesteter Docht; das ist keine Tagesbesonderheit, sondern die Definition von Struktur. Der Loop hat P4 heute vorbildlich befolgt (alle drei SL-Herleitungen zitieren ein OHLC-Low mit Kerzen-Zeitstempel, nie einen Close) — und ist genau deshalb 4/4 an 8c2 gescheitert. Die beiden Regeln ziehen in entgegengesetzte Richtungen. Das ist kein Loop-Fehler, sondern eine Regelwerksspannung, die vorher nicht sichtbar war, weil das RR-Gate immer zuerst fiel.

**(3) Der 30-Punkte-Sockel im Trigger-Radius ist der wahrscheinlich fehlkalibrierte Teil, nicht der Puffer.** Der Trigger lautet max(30 Pkt, 0,5xATR). Bei den heutigen ATR(5min)-Werten von 22,3 bis 33,1 dominiert durchgehend der absolute Sockel: 30 Pkt entsprechen 0,9x bis 1,3x ATR. Jeder SL, der weniger als gut eine ATR-Einheit von irgendeinem getesteten Level entfernt liegt, wird erfasst — auf einem 5-Minuten-Chart trifft das auf praktisch jede Struktur zu. 8c2 stammt aus den Verlusten #42/#43 vom 21.08. (Opex/PMI-Schock), einem Tag mit vermutlich deutlich höherer ATR; dort hätte der 0,5xATR-Term dominiert und der 30-Pkt-Sockel wäre wirkungslos gewesen. An einem ruhigen Tag kehrt sich das Verhältnis um und der Sockel wird zur bindenden Größe. Das ist die konkrete, überprüfbare Hypothese: **ATR(5min) an #42/#43 gegen die heutigen 22-33 Pkt halten.** Liegt sie dort bei 60+, ist der Sockel nachweislich außerhalb seines Kalibrierungsbereichs im Einsatz.

**Der entscheidende Gegentest — was hätte 8c2-Konformität gebracht?** Ich habe die Alternative durchgerechnet, weil die Frage "zu strikt?" nur mit der Gegenrechnung beantwortbar ist:

- **18:49** (Entry 29.553,85, ATR 23,4, Zone 29.499,35): Ein konformer SL müsste bei ≤29.487,65 liegen. SL-Distanz dann 66,2 Pkt; RR≥1 verlangt TP1 ≥29.620,05; die Zone-3-Schranke (3xATR) kappt bei 29.624,05. Zulässiges TP1-Fenster: **4,0 Pkt breit**. Registrierte Level darin: keine (29.600 darunter, 29.650 darüber). Der Trade wäre auch mit konformem SL unmöglich gewesen.
- **18:55** (Entry 29.549,85, ATR 22,3): konformer SL ≤29.488,20, TP1-Fenster [29.611,50; 29.616,75] = **5,25 Pkt**, kein Level darin. Ebenfalls unmöglich.

Das heißt: An beiden Abend-Triggern war 8c2 zwar der protokollierte Blocker, aber **nicht die eigentliche Ursache** — eine Lockerung von 8c2 allein hätte an diesem Tag exakt null Trades erzeugt. Wer 8c2 anfasst, ohne die TP1-Fenster-Arithmetik anzufassen, verschiebt nur, an welchem Gate der FAIL protokolliert wird.

**Wann es möglich gewesen wäre — und warum es nicht erreichbar war.** Mit demselben konformen SL 29.487,65 und TP1 29.600 existierte ein vollständig regelkonformes Entry-Fenster von [29.529,80; 29.543,83] (8c-Floor ≥29.522,75, RR≥1 ≤29.543,83, Zone-3 ≥29.529,80). Bei einem Entry von z.B. 29.535 wären alle Hard-Gates PASS gewesen: SL-Distanz 47,4 Pkt = 2,02x ATR, TP1 65,0 Pkt = 2,78x ATR (Zone 2, halbe Position), RR 1,37:1, 8c2 erfüllt. Der Kurs stand von 18:44 bis 18:48 durchgehend in diesem Band (29.530,25 bis 29.540,95). Der Dual-Gate-2/2-Trigger kam aber erst um 18:49 bei 29.553,85 — **10 Punkte über der Obergrenze des Fensters**. Das Fenster schloss, gerade als es hätte genutzt werden dürfen.

Das ist dieselbe Kausalität, die Opus/Fable am 03.09. für VC#23 festgestellt haben ("das Kriterium erfüllte sich exakt dann, wenn es wertlos war"), nur eine Regelwerksrunde später und mit deutlich besseren Ausgangswerten: Der Entry-Reifegrad lag heute bei 1,73x bis 1,79x statt 2,41x bis 2,79x am 03.09. Die Entry-Timing-These vom 03.09. ist damit teilweise widerlegt: Der Entry *war* heute erheblich früher, und es hat trotzdem nicht gereicht. Der verbleibende Abstand von 10 Punkten ist nicht durch besseres Timing schließbar — er ist die Latenz zwischen "Struktur wäre handelbar" und "Dual-Gate bestätigt".

Und eine Ergänzung, die ich für wichtiger halte als die 8c2-Debatte: Die zwei Gegenrechnungen oben unterstellen einen SL bei 29.487,65 — ein Niveau, das **keine Struktur ist**, sondern rückwärts aus dem 8c2-Puffer abgeleitet. Ein solcher SL widerspricht P4. Es gibt heute keinen Pfad, der P4 *und* 8c2 *und* RR≥1 *und* die Zone-3-Kappung gleichzeitig erfüllt. Das ist der eigentliche Befund des Tages.

### F5 — Der TP2-Level wurde 4/4 nicht übergeben, obwohl er 3/4 im Register vorhanden war

In allen vier Aufrufen steht "[N/A] TP2-Realismus (8b2): `--tp2-level-price` nicht angegeben". Fable hat am 03.09. festgestellt, dass ein fehlender TP2-Level einen GESAMTSTATUS PASS verhindert (damals: "selbst mit Opus' SL wäre GESAMTSTATUS UNKNOWN gewesen"); die P7-Klarstellung vom 04.09. formuliert dagegen, TP2 sei "kein Hard-Gate, sondern nur Sizing-Signal". Welche der beiden Lesarten der aktuelle Code umsetzt, habe ich nicht am Code verifiziert — das ist eine Ein-Zeilen-Prüfung, die vor dem nächsten Testtag Klarheit schaffen würde.

Unabhängig davon ist die Unterlassung selbst real und wiederholt sich 4/4:
- 18:49: 8b2 verlangt TP2 mit RR≥2, also ≥29.637,65. Im Register lagen 29.650, Pivot R1 29.654,6 und Session-Hoch 29.660,8 — **verfügbar, nicht übergeben**.
- 18:55: TP2 ≥29.625,65 nötig; 29.650 verfügbar, nicht übergeben.
- VC#10: TP2 ≥29.712,85 nötig; das Rundzahl-Band war um 15:50 gerade auf 29.700/29.750 erweitert worden — **verfügbar, nicht übergeben**.
- VC#7: TP2 ≥29.731,15 nötig; das Band endete zu diesem Zeitpunkt noch bei 29.650 — hier war tatsächlich keiner verfügbar.

In drei von vier Fällen war der Level da und wurde nicht mitgegeben. Selbst in der milden Lesart (nur Sizing-Signal) unterdrückt das einen Eingabewert, der die Positionsgröße bestimmt. Das ist derselbe Punkt, den Fable am 03.09. angemerkt hat, und er ist heute unverändert wiederaufgetreten — der einzige echte Wiederholungsfall des Tages neben F7.

### F6 — Register-Frische: 17 Voll-Checks in Folge mit einer sachlich falschen Pflichtzeile

Der Exit-Code-1-Fall um 18:49 ist regelkonform abgearbeitet worden: Hard-Exit erkannt, Register sofort neu geschrieben (QQQ-AVWAP-Bänder per `data_get_study_values` neu abgerufen, Pivots/PDH-PDL/Session-Extrema/Rundzahlen re-verifiziert), UTC-Zeitstempel gesetzt, zweiter Aufruf regulär durchgelaufen, alles wörtlich protokolliert. Nach Abbruch-Gate v2 Klarstellung (ii) zählt dieser Fall wegen der erfolgreichen Wiederholung im selben Trigger-Moment ausdrücklich **nicht** für Kriterium (d). Die Behandlung im Moment selbst ist einwandfrei.

Der Weg dorthin ist es nicht. Das Register war 181 Minuten alt, also doppelt so alt wie die harte Grenze. Und das war nicht unbemerkt: Die Register-Zeile im Voll-Check hat das Alter jedes Mal korrekt ausgewiesen — und jedes Mal die falsche Schlussfolgerung gezogen. Ab VC#29 (17:25, "Alter 95 Min, weiter gültig") bis VC#45 (18:45, "Alter 175 Min, weiter gültig") sind das **17 aufeinanderfolgende Voll-Checks, in denen ein Register als gültig bezeichnet wurde, das die harte 90-Minuten-Grenze bereits überschritten hatte** — jeweils mit "Register ✓" in der Format-Zeile bestätigt. (Bei VC#7 war die Zeile noch explizit korrekt: "Alter 40 Min, noch unter der 60-Min-Warnschwelle".)

Dass daraus kein Schaden entstand, ist Timing-Glück: Der Trigger kam um 18:49, und der zweite Aufruf landete danach zufällig noch im Entry-Fenster. Wäre der Trigger um 17:30 gekommen, hätte derselbe Hard-Exit denselben Trigger-Moment gekostet — und der Kurs stand damals in einem viel schnelleren Umfeld.

Der Loop hat die Ursache in Abschnitt 4 selbst richtig benannt ("Register-Pflege läuft über lange trigger-freie Phasen nicht automatisch mit und fällt erst beim nächsten Trigger auf"). Was fehlt, ist nicht die Erkenntnis, sondern der Schwellenwert in der Pflichtzeile: Sie prüft offenbar gegen eine 60-Minuten-*Warnschwelle*, kennt aber die 90-Minuten-*Hartgrenze* nicht. Das ist der konkreteste vor dem nächsten Testtag adressierbare Defekt des Tages.

### F7 — P5 (Register-Vorwärtspflege) nur zur Hälfte umgesetzt

Die Rundzahl-Band-Hälfte hat funktioniert, und zwar vorbildlich: Das Skript warnte bei VC#7 ("Band deckt das Fenster NICHT ab"), der Loop notierte es sofort und zog das Band um 15:50 auf 29.700/29.750 nach. Genau die Rückkopplung, für die P5 gebaut wurde.

Die Fib-Extension-Hälfte hat nicht stattgefunden. In beiden dokumentierten Levelsuche-Zeilen (VC#7 und 18:49) steht "Fib/EMA-Konfluenz [—]", ohne Begründung. Fibonacci-Werte wurden den ganzen Tag berechnet, aber ausnahmslos als **Retracements** des Impulses 29.660,8→29.441,0 — nie als Extension eines laufenden Impulses, und nie als TP-Kandidat. Das ist exakt der Befund vom 03.09. ("in 58 VCs nie als Extension genutzt, nur als Retracement"), unverändert wiederholt. Er ist heute besonders teuer, weil die TP1-Fenster (17,8 / 42,1 / 28,3 / 29,0 Pkt breit) auf ein Levelraster aus 50er-Rundzahlen trafen — ein zusätzlicher vorwärtsgerichteter Levelkandidat hätte an genau der Stelle gewirkt, an der die Fenster leer blieben. Anmerkung dazu: Die Fable-Rückfrage 4 vom 03.09. ("Fib-Extension wird weiterhin manuell von Sonnet berechnet, nicht skriptseitig") ist damit empirisch beantwortet — manuell bedeutet in der Praxis: gar nicht.

### F8 — Die größte Bewegung des Tages hat nie ein Gate erreicht

Von 16:20 bis etwa 18:00 waren durchgehend 3 von 4 Dual-Gate-Ebenen bärisch: NAS100 5min und 15min unter EMA50, QQQ zeitweise vollständig bärisch (EMA50 und beide AVWAP-Bänder verloren), ADX(5min) zwischen 30 und 38 mit klarer -DI-Dominanz, Directional RelVol Avg bis −1,45. Der Loop hat das korrekt erfasst und in jedem Voll-Check dieselbe Zeile geschrieben: kein 2/2, weil das 1H-Bein fehlt.

Der Kurs fiel in diesem Fenster von 29.654 auf 29.432 — 222 Punkte, dieselbe Größenordnung wie der NFP-Schock. Es gab dafür **keinen einzigen `gate_check.cjs`-Aufruf**, weil die Voraussetzung nie eintrat.

Der Grund ist mechanisch, nicht diskretionär: Die 1H-EMA50 lag zwischen 29.392 und 29.405. Damit der 1H-Bias kippt, hätte ein 1H-Bar unter ~29.400 schließen müssen — das Tagestief war 29.431,95. Der Short war den ganzen Tag nicht erreichbar, unabhängig davon, wie sauber die Struktur war.

Das ist ausdrücklich **kein Vorwurf an den Loop** — die Auslegung "Bias des zuletzt geschlossenen 1H-Bars" ist die am 03.09. bewusst getroffene Entscheidung, und sie wurde heute lehrbuchmäßig und ohne einen einzigen Eigenauslegungsversuch angewandt (deutlicher Fortschritt gegenüber dem 03.09.). Aber der erste Live-Tag unter der neuen Definition hat sofort ihre Spiegelbild-Schwäche gezeigt: Was am 03.09. Longs zu spät freigab, blockiert jetzt Shorts praktisch bedingungslos, solange die 1H-EMA50 nach einem vorangegangenen Aufwärtstrend weit unten nachhängt. Die alte Fassung war zu eng, die neue ist in Trendfolge-Regimen asymmetrisch. Das ist ein Datenpunkt für die zurückgestellte P8-Entscheidung, mehr will ich daraus nicht machen — eine Regel nach einem Tag zu drehen, wäre genau das Overfitting, vor dem Fable am 03.09. gewarnt hat.

### F9 — Kleinere Beobachtungen

- **A3-Messfeld-Ausnahmen als Dauerzustand:** `--trend-effizienz` und `--dual-gate-qqq-abstand-atr` wurden in 4/4 Aufrufen mit einem `--grund-`-Text ersetzt, jeweils mit derselben Standardbegründung ("in diesem Voll-Check nicht separat berechnet" / "QQQ-ATR nicht als Chart-Indikator sichtbar"). Eine Ausnahme, die in 100 % der Fälle greift, ist keine Ausnahme mehr, sondern ein nicht implementiertes Feld. Bei der Trend-Effizienz ist die Begründung besonders schwach — sie ist aus den vorhandenen Bars rechenbar; "nicht berechnet" ist eine Entscheidung, kein Hindernis. Die am 02.09. eingeführte Messfeld-Erzwingung wird damit formal erfüllt, inhaltlich nicht.
- **Plausibilitätswarnung ignoriert:** Bei VC#7 meldete das Skript "PLAUSIBILITAETS-WARNUNG kerzen-qqq: 1 < 1,67 — Wert vermutlich zu niedrig angesetzt". Im Fließtext wurde sie nicht aufgegriffen oder entkräftet. Folgenlos (kein Gate), aber die Warnung hat ihren Zweck damit verfehlt.
- **Screenshot-Disziplin:** Ab VC#19 (16:35) wurde in **39 aufeinanderfolgenden Voll-Checks** der Screenshot ausgelassen, jeweils mit einer selbst formulierten Begründung ("keine strukturelle Änderung"), einmal sogar ausdrücklich trotz Level-Update (VC#57). Das ist transparent und damit keine stille Auslassung — aber es ist faktisch eine Selbst-Suspendierung einer Pflicht. Zudem enthält die Zahlenbilanz in Abschnitt 1 überhaupt keine Screenshot-Zeile; die am 28.08. angemahnte Screenshot-Bilanz ist im aktuellen Abschluss-Template nicht angekommen.
- **Nachrichtenlage blind:** X-API 45 Fehlversuche in Folge, seit 03.09. 17:51 UTC kein Fetch (>22 Stunden); zusätzlich schlug der Kalender-Direktabruf im Session-Update zweimal fehl. Zwei von drei Nachrichtenkanälen waren den gesamten Tag tot, nur FRED lieferte. Die Offenlegung war jedes Mal korrekt, es wurde nichts erfunden und kein Ersatz behauptet. Aber es gab auch keinen definierten Degraded-Mode: Der Tag lief mit voller Order-Freigabe, obwohl der Tweet-Check als Kontrolle über die gesamte Session null Funktion hatte. Heute folgenlos (kein Trade, kein bekanntes Event) — an einem Echtgeldtag wäre das eine Vorab-Entscheidung wert.
- **Selbstkorrektur:** Die falsche Quick-Tick-Zeile um 15:19 ("Order-Sperre läuft in 1 Min ab") wurde bei VC#4 aus eigenem Antrieb richtiggestellt. Kleinigkeit, aber es ist das Verhalten, das man sehen will.

## Vergleich 03.09. → 04.09.: behoben oder wiederholt?

| Befund vom 03.09. | Status am 04.09. |
|---|---|
| 1H-Kriterium: Sonnets Eigenauslegung ohne Regelgrundlage | **Behoben.** Neue Definition durchgehend und wortgetreu angewandt, kein Eigenauslegungsversuch. Neue Spiegelbild-Schwäche sichtbar (F8). |
| Retest-Zeitbox nie erzwungen (35 VCs ohne Neu-Aufruf) | **Behoben.** 4 Zeitboxen, alle vollständig durchlaufen, ein echter Neu-Aufruf. Zwei Detaillücken (F2). |
| Punkt 13.1 / Chasing nie angewandt | **Behoben im Mechanismus.** Pflichtfeld 4/4 gefüllt, `yes`-Pfad einmal erreicht. Sizing-Konsequenz mangels PASS weiter ungetestet; Abschlusszeile falsch (F1/F3). |
| SL-Anker: Close statt Low, veraltete Zone | **Behoben.** Alle drei SL-Herleitungen zitieren ein OHLC-Low mit Kerzen-Zeitstempel. Führt paradoxerweise direkt in den 8c2-FAIL (F4). |
| Leeres TP-Fenster undiagnostiziert | **Behoben.** P3-Fenster-Diagnose in 4/4 Aufrufen, inklusive der handlungsrelevanten Zeilen "Entry liegt 2 Pkt / 0,5 Pkt AUSSERHALB". |
| Entry-Reifegrad folgenlos als Anzeige | **Verbessert.** P2-Eskalation 4/4 ausgegeben; Reifegrad von 2,41-2,79x auf 1,70-1,79x gefallen. Wirkung real, aber nicht ausreichend (F4). |
| Register-Vorwärtspflege / Rundzahl-Band veraltet | **Halb behoben.** Rundzahl-Band-Warnung griff und wurde befolgt; Fib-Extension weiterhin nie genutzt (F7). |
| TP2-Level fehlt im Aufruf | **Wiederholt, 4/4** — in 3 Fällen war der Level im Register vorhanden (F5). |
| Falsche Zahlen in der Selbstbilanz (28.08.-Klasse) | **Rückfall, 3 Fälle** (F1). |

Sechs von neun Punkten sind sauber erledigt, zwei halb, zwei sind Wiederholungen. Für eine einzige Regelwerksrunde ist das eine gute Quote.

## Gesamtbild: 0 Trades bei 4 Triggern

Über vier Testtage stehen jetzt **11 `gate_check.cjs`-Läufe mit 0 PASS**. Die Verteilung der bindenden Restriktion hat sich dabei verschoben:

- 01.-03.09.: 7 Läufe, 6 davon FAIL am **RR-Gate** (0,15:1 und 0,70:1 als Extremwerte).
- 04.09.: 4 Läufe, RR-Werte 0,944 / 0,982 / **1,101** / **1,323** — erstmals zwei Läufe über der 1:1-Schwelle. Dafür 4/4 FAIL am **8c2**.

Das ist kein neues Problem, sondern Constraint-Migration: Die P1-P6-Maßnahmen haben das RR-Gate gelöst (der Reifegrad sank, der Register-Level-Treffer wurde erzwungen, die Fenster-Diagnose machte die Geometrie sichtbar) — und darunter kam das nächste bindende Gate zum Vorschein. Genau das ist das erwartete Verhalten eines gestaffelten Gate-Systems, und es ist der Beleg dafür, dass die 04.09.-Maßnahmen gewirkt haben.

Die Frage "scheitern Trigger fast immer an SL-Struktur-Gates statt an RR/TP-Realismus?" würde ich deshalb verneinen — die richtige Formulierung ist: **Sie scheitern immer an demjenigen Gate, das gerade als nächstes in der Kette steht, und die gemeinsame Ursache dahinter ist dieselbe.** Ein Entry, der spät zur Struktur kommt, vergrößert die SL-Distanz (tötet RR) *und* drückt den SL an kürzlich getestete Zonen (tötet 8c2). Ein Symptom, zwei Anzeigen.

Was der Tag darüber hinaus zeigt — und das halte ich für die wichtigste Erkenntnis: Für einen PASS müssen gleichzeitig gelten (a) SL ≥1,5x ATR vom Entry, (b) SL jenseits jeder Dochtzone im Radius max(30; 0,5xATR), (c) TP1 ≤3x ATR (für volle Größe ≤2x), (d) TP1 ≥ SL-Distanz, (e) TP1 exakt auf einem registrierten Level (±2 Pkt). (a) und (c)/(d) zusammen begrenzen das TP1-Fenster auf maximal 1,5x ATR Breite; heute waren es real 17,8 bis 42,1 Punkte. Dieses Fenster trifft auf ein Levelraster, das im Wesentlichen aus 50er-Rundzahlen besteht. Die Trefferwahrscheinlichkeit ist damit strukturell begrenzt, bevor 8c2 überhaupt geprüft wird — und danach muss 8c2 auch noch halten.

Daraus folgt für mich: **Der wirksame Hebel liegt an der Leveldichte, nicht an den Schwellen.** Ein zusätzlicher vorwärtsgerichteter Levelkandidat pro Impuls (P5-Fib-Extension, Measured Move) verändert die Trefferwahrscheinlichkeit im engen Fenster unmittelbar; eine Lockerung von 8c2 hätte an diesem Tag nachweislich null Trades erzeugt (F4-Gegenrechnung). Das deckt sich mit Fables 03.09.-Fazit und wird durch die heutigen Zahlen gestützt statt widerlegt.

Einschränkung in beide Richtungen, damit das nicht überinterpretiert wird: Vier fiktive Tage sind eine dünne Basis, und drei davon waren atypisch (Trendtag +476 Pkt, NFP-Schock mit Vollerholung und zweitem Abverkauf). Andererseits: 0 von 11 ist eine Zahl, die man nicht dauerhaft mit "der Markt bot nichts" erklären kann, wenn an zwei der vier Tage 200+ Punkte gelaufen sind. Die saubere Antwort darauf ist keine Schwellenänderung aus dem Bauch heraus, sondern eine Machbarkeitsmessung: über historische Bars auszählen, wie oft die fünf Bedingungen (a)-(e) überhaupt gleichzeitig erfüllbar sind. Solange diese Zahl unbekannt ist, ist jede Kalibrierungsdiskussion Meinung. Der Umstand, dass das 15-Trade-Review als Datengrundlage vorgesehen ist, während 0 Trades zustande kommen, ist eine Sackgasse, die früher oder später aufgelöst werden muss.

## Bewertung der Auftragsfragen im Überblick

- **Wurden 03.09.-Probleme wiederholt?** Überwiegend nicht — siehe Tabelle. Echte Wiederholungen: fehlender TP2-Level (F5) und ungenutzte Fib-Extension (F7). Ein Rückfall in eine ältere Fehlerklasse: falsche Selbstbilanz-Zahlen (F1).
- **Hat P1 funktioniert?** Ja, im Kern sauber und wie spezifiziert. Zwei Interpretationsspielräume: Dehnung der Zeitbox bei Protokolllücken (in beiden Fällen konsistent und konservativ angewandt, aber nicht spezifiziert) und eine Neu-Aufruf-Bedingung, die einen arithmetisch aussichtslosen Aufruf auslösen kann.
- **Wurde `--chasing` korrekt verwendet?** Ja, und entgegen der Abschlusszeile einmal mit `yes` (VC#7). Drei Entscheidungen zwingend richtig, eine (18:49) vertretbar, aber diskutabel.
- **8c2 strukturell oder tageszufällig?** Strukturell verdächtig — die Kollision mit der P4-SL-Anker-Definition und die Dominanz des 30-Punkte-Sockels bei niedriger ATR sind generische Mechanismen, keine Tagesbesonderheiten. Aber: an diesem Tag war 8c2 der protokollierte, nicht der eigentliche Blocker.
- **Exit-Code-1-Fall:** Im Moment selbst regelkonform und sauber behandelt, zählt nach Klarstellung (ii) nicht für Kriterium (d). Der Weg dorthin war der Defekt (17 falsche Frische-Zeilen).
- **Levelsuche/Registerpflege ausreichend?** Substanziell ja (4 Updates, nicht 3; alle vier Gate-Aufrufe hatten register-verifizierte TP1-Level, VC#7 mit 0 Pkt Abweichung). Bei der Frequenz nein: drei Stunden ohne Update bei laufend falsch bewerteter Frische.

## Abbruch-Gate-Vorprüfung (v2, Kriterien a-d gegen den 04.09.)

- **(a)** Faktenprotokoll-Abschluss vollständig, alle 6 Abschnitte plus die beiden Pflichtzeilen vorhanden, Frontmatter `completed` → nicht ausgelöst.
- **(b)** 4 von 59 fälligen Voll-Check-Slots ausgefallen (#8, #18, #46, #48) = 6,8 %, unter der 10-%-Schwelle; alle offengelegt → nicht ausgelöst.
- **(c)** Kein einziger GESAMTSTATUS UNKNOWN, alle vier Läufe klare FAIL; kein struktureller Verifikationsfehler des Registers (alle TP1-Level wurden getroffen) → nicht ausgelöst.
- **(d)** Ein registerbezogener Exit-Code 1 (Alter über 90-Min-Grenze), aber nur einer statt der geforderten ≥2, und nach Klarstellung (ii) im selben Trigger-Moment erfolgreich korrigiert wiederholt → zählt ausdrücklich nicht → nicht ausgelöst.

Keines der maschinellen Kriterien greift. Damit hängt die Bewertung an (e), meinem Prüferfeld.

## Abbruch-Empfehlung

Ich stufe nicht auf ABBRUCH: Der Tag war prozessual gut, die neuen Mechanismen haben getragen, es gibt keinen still deaktivierten Hard-Gate-Pfad und keine korrumpierte Statusdatei. Ich stufe aber auch nicht auf KEIN EINWAND, aus zwei Gründen. Erstens verlangt der Prüferauftrag ausdrücklich mindestens EINGESCHRÄNKT bei einer nachweislich falschen Faktenprotokoll-Aussage — davon gibt es drei (F1), und die zur `--chasing`-Nutzung schreibt den Live-Status eines gerade erst eingeführten Mechanismus falsch in den Speicher. Zweitens ist die Register-Frischeprüfung im Voll-Check nachweislich defekt: Sie hat 17 Voll-Checks in Folge ein Register als gültig ausgewiesen, das die harte 90-Minuten-Grenze bereits überschritten hatte, und nur die Zufälligkeit des Trigger-Zeitpunkts hat verhindert, dass das einen Trigger-Moment gekostet hat. Genau dafür ist die mittlere Antwortstufe gedacht: Der Mechanismus ist billig vor dem ersten Order-Ablauf nachweisbar, ein Beobachtungsmodus wäre unverhältnismäßig.

`Abbruch-Empfehlung: EINGESCHRÄNKT (Register-Frischeprüfung in der Voll-Check-Pflichtzeile — das ausgewiesene Registeralter muss vor dem ersten 7b1-Order-Ablauf einmal nachweislich gegen die harte 90-Minuten-Obergrenze geprüft und das Ergebnis als eigene Protokollzeile belegt werden; die Zeile hat am 04.09. 17 Voll-Checks in Folge ein 95-175 Minuten altes Register als "weiter gültig" bezeichnet)`

## Für das Gespräch am 07.09. offen

Reine Auflistung, keine Handlungsanweisung — das entscheidet Levi:

1. Zahlenkorrekturen aus F1 (chasing `yes` bei VC#7; 55 statt 59 Voll-Checks; 4 statt 3 Register-Updates) — betrifft den Speicherstand, nicht das Regelwerk.
2. Die Spannung zwischen P4 (SL am Docht) und 8c2 (SL jenseits jeder Dochtzone) — heute erstmals sichtbar, weil beide zugleich korrekt befolgt wurden.
3. Der 30-Punkte-Sockel in 8c2 gegen die ATR(5min) an #42/#43 gehalten — die eine Zahl, die die Kalibrierungsfrage entscheidet.
4. TP2-Level-Übergabe (F5) und die Frage, ob N/A bei 8b2 einen PASS verhindert — Ein-Zeilen-Codeprüfung.
5. P1-Zeitbox: Wanduhr-Grenze zusätzlich zur Voll-Check-Zählung, und Unterdrückung des Neu-Aufrufs bei unverändertem SL nach einem 8c2-FAIL.
6. Asymmetrie des 1H-Kriteriums (F8) als Datenpunkt für die zurückgestellte P8-Entscheidung — bewusst kein Änderungsvorschlag nach einem Tag.
7. Degraded-Mode bei ausgefallenen Nachrichtenkanälen (X-API seit >22 h tot, Kalender-Direktabruf gescheitert).
8. Die Leveldichte-These als Alternative zur Schwellendiskussion, und die Machbarkeitsmessung, die sie prüfbar machen würde.
