---
name: project_punkt11_sl_empfehlung_2026-09-14
description: "Empfehlungsvorlage (Opus 5, 14.09.2026) zu den drei Punkten, die aus der Textkorrektur-Serie an feedback_live_trading.md als echte REGELFRAGEN uebrig geblieben sind: W5 (Punkt-11-Schwelle 2 oder 3 von 4), N1 (Geltungsbereich von Punkt 11 — nur Dreh oder auch Exit) und N5 (Cron-Item (8) sagt weiter 'SL=MAX(Struktur,Floor)'). Kernbefund: Punkt 11 speist DREI Rechtsfolgen mit DREI verschiedenen impliziten Schwellen (Dreh 2-3/4, Vollexit-Deckelung Z1074 ≥1/4, Chasing-Zaehlerreset 13.1 ≥1/4) — und die sichere Richtung ist bei Dreh und 13.1 GEGENLAEUFIG, eine einzige Zahl kann beide nicht bedienen. Nachgerechnet an 44 echten Punkt-11-Auswertungen (28 am 08.09. mit offener Position, 16 am 10.09.) und an scripts/gate_check_log.jsonl (42 Laeufe am 10.09. mit --sl 29249.95 = Anker). Empfehlung: Option C (getrennte Schwellen je Rechtsfolge), N5 als Ein-Zeilen-Textvervollstaendigung vor dem naechsten Testtag. KEINE Regelaenderung, KEIN Commit — Levi entscheidet."
metadata:
  node_type: memory
  type: project
  status: "Opus-Gegencheck 14.09.2026: FREIGEGEBEN MIT AUFLAGEN — 1 Blocker gefunden und gefixt (position_tick.cjs rechnete weiter 2/4 und meldete bei 2/4 einen Dreh), 2 Nachbesserungen (Schattenzeile maschinell + [Voll-Check]-Marker, Maschinenseite in der Rechtsfolgen-Tabelle), Fables offener Punkt 1 (Deckelung (b)) als KEINE Regelluecke aufgeloest, chartanalyse Z584 freigegeben; Cron-Injektionsverdacht NICHT bestaetigt (loop_prompt.cjs trug N5 schon). Tests 73/73 -> 74/74, ungepusht. Schattenzaehler Vollexit 2/4: 0/~10. Offen fuer Levi: 2/4-Freigabe nach ~10 Momenten, DAX-Schwelle"
  originSessionId: session_01E4zVYdeDA5XiMXhwykDWcw
  modified: 2026-09-14T13:50:58.390Z
---

# Punkt 11 (W5/N1) und die SL-Kurzformel im Cron-Block (N5) — Empfehlungsvorlage

**Die Frage in einem Satz:** Reicht bei Punkt 11 die Erfüllung von 2 oder von 3 der 4 Reversal-Kriterien — und wofür genau gilt diese Schwelle dann, nur für den Dreh auf die Gegenrichtung oder auch für einen Exit?

**Woher das kommt:** [[project_pruefung_feedback_live_trading_2026-09-14]] — W1–W4 sind textlich korrigiert und gegengecheckt, W5 blieb bewusst offen (Fail-Safe 3/4 aktiv), N1 und N5 kamen im Opus-Gegencheck dazu. Dieser Bericht **ändert nichts** an `feedback_live_trading.md`, am Code oder an der DB. Kein Commit, kein Push ([[feedback_dont_change_running_system]], Punkt 14 Regeländerungs-Tempo-Bremse).

**Der Befund, der die Frage umstellt:** Punkt 11 ist nicht eine Regel mit einer unklaren Zahl, sondern **eine Kriterienliste, die drei verschiedene Rechtsfolgen speist — mit drei verschiedenen impliziten Schwellen**, und bei zweien davon zeigt die sichere Richtung **gegenläufig**. Das ist der Grund, warum die Datei die Zahl nie eindeutig hinschreiben konnte: es gibt keine einzelne richtige Zahl.

---

## 1. Rekonstruktion: was war ursprünglich gemeint?

### 1.1 Die drei Rechtsfolgen, die heute an derselben Kriterienliste hängen

| # | Rechtsfolge | Fundstelle | Dort genannte Schwelle | Sichere Richtung |
|---|---|---|---|---|
| R1 | **Dreh-Vorschlag** (Position schließen **und** Gegenposition eröffnen) | Z994, Z990, Z1002, Z1008 | „mindestens 2-3 dieser Kriterien" | **höher** = sicherer (frisches Risiko) |
| R2 | **Vollexit-Freigabe** über die Punkt-12-Deckelung (25–50 %, NIE Vollexit) | Z1074 | zwei Zahlen **im selben Absatz**: Deckelung fällt, sobald **≥1** Kriterium erfüllt ist — Vollexit aber nur „über Punkt 11 selbst (echtes Reversal, 2-3 von 4)" | **niedriger** = sicherer (Kapitalschutz) |
| R3 | **Chasing-Zählerreset 13.1** (`--punkt11-signal ja\|nein`, `k-ohne-signal`) | Z1158, Z1167, Cron-Block Z155 | wörtlich „**kein einziges Punkt-11-Kriterium**" = **≥1** ist ein Signal | **niedriger** = sicherer (weniger 50-%-Chasing-Entries) |

**Das ist der Kern.** Bei R1 ist 3/4 die vorsichtige Antwort, bei R3 ist 3/4 die **riskantere** — denn `--punkt11-signal ja` setzt `k-ohne-signal` auf 0 und **verhindert** den 13.1-50-%-Einstiegsvorschlag; `nein` lässt den Zähler laufen und **erzwingt** ihn ab k=2 (`vollcheck.cjs` Z408–417, Hard-Exit 1 ohne das Feld). Eine höhere Punkt-11-Schwelle heißt dort: seltener „ja", also **mehr** Chasing-Entries. Eine einzige Zahl für beide Rechtsfolgen ist damit nicht nur unpräzise, sie ist **strukturell unmöglich**.

### 1.2 Was die Datumsangaben sagen

| Datum | Stelle | Aussage |
|---|---|---|
| 13.07. | Z994 (Levi-Zitat), Z990 (Vorrang-Klärung) | Punkt 11 = „schließen **und** drehen"; Exit allein läuft über 4a/9d1; „volle 2-3-von-4-Bestätigung" |
| 22.07. | Z1010 (Belegpflicht, nach Trade #24) | „Punkt-11-**Exit**/Dreh-Vorschlag", Beispielverdikt „→ **2/4**, Exit gerechtfertigt" — Anlass war ein **50-%-Teilexit** |
| 23.07. | Z1074 (Deckelung) | Vollexit nur über Punkt 11 „(2-3 von 4)" **oder** 9d1/4a; Deckelung fällt schon bei ≥1 Kriterium |
| 30.07. | Z1158 (13.1-Präzisierung) | „kein einziges Punkt-11-Kriterium" über 2 Voll-Checks |
| **31.07.** | **Z992, Z862, Z1097** | **Punkt 11 = „zu hohe Schwelle, für Dreh gedacht", „bewusst hohe, träge Schwelle"; der Teilexit-/Verteidigungsfall wird ausdrücklich aus Punkt 11 heraus nach 7c verlagert; 12.3 bestätigt „EXKLUSIV Punkt 11" nur für den Dreh** |
| 21.08. | Z1014, Z1023 (Volumen-Dämpfer) | „selbst ein technisch erfülltes **2/4**-Kriterium … NICHT ausreichend"; „2/4 + schwaches Volumen → kein Dreh" |
| 04.09. | Z1167 (13.1 verankert) | Maschinenfeld `--punkt11-signal ja\|nein`, A3-pflichtig bei erfülltem Chasing-Kriterium |

**Daraus lese ich die wahrscheinlichste ursprüngliche Absicht:**

1. **„2-3 von 4" war nie eine Spanne, sondern eine Zusammenfassung zweier Fälle.** Die einzige Stelle der Datei, an der eine **2** in einem *Regelsatz* statt in einem *Beispiel* steht, ist die QQQ-Ausnahme in Kriterium 3 (Z999): *„Falls das QQQ-Gate laut Punkt 7e gerade nicht verfügbar ist, entfällt dieses Kriterium ersatzlos — dann müssen **2 der verbleibenden 3** Kriterien erfüllt sein."* Das ergibt als Ausnahme nur Sinn, wenn der Normalfall **3** ist. Läge der Normalfall bei 2, wäre „2 von 3" keine Ausnahme, sondern eine Verschärfung, die niemand so formuliert hätte. **„2-3 von 4" = „3 von 4, bei geschlossenem QQQ-Gate 2 von 3".**
2. **Die Kriterienliste selbst ist konjunktiv formuliert.** Z997–Z999 lauten „1. … / 2. **UND** … / 3. **UND** … (Pflicht-Teil, siehe Punkt 7b)"; nur Kriterium 4 (Z1000) steht ohne „UND". Wörtlich gelesen sind 1+2+3 alle Pflicht und 4 ist die Verstärkung — das ist eine **3-von-4-Konstruktion mit benanntem Pflicht-Kriterium**, keine freie Auszählung. Unter „2 von 4" könnte man das ausdrücklich als Pflicht-Teil markierte QQQ-Kriterium überspringen; das widerspricht seinem eigenen Wortlaut.
3. **Jede Querverweis-Stelle der Datei beschreibt Punkt 11 als die hohe, träge Schwelle** — Z854 („reagiert bewusst träge, um Fehlsignale zu vermeiden"), Z856 („nicht erst wenn 2-3 von 4 … vorliegen"), Z862 („eine bewusst hohe, träge Schwelle, weil ein verfrühter Dreh eine neue Position mit echtem frischem Risiko eröffnet"), Z1097 („die hohe 2-3-von-4-Schwelle"). Keine einzige Stelle beschreibt Punkt 11 als niedrigschwellig. Mit der Lesart „2 von 4" wäre Punkt 11 die **niedrigste** Reversal-Schwelle des ganzen Regelwerks — 7c (fünfte Erweiterung) und 12.3 verlangen mehr Bestätigungs-Checks als „MACD-H negativ + EMA50-Bruch".
4. **Die „2"-Belege stammen beide aus Vorgängen, die später umgewidmet wurden.** Z1010 dokumentiert einen **50-%-Teilexit** (Trade #24) — genau die Handlung, die am 31.07. per Z992/Z862 ausdrücklich aus Punkt 11 heraus nach 7c verschoben wurde. Das Beispielverdikt „2/4, Exit gerechtfertigt" ist damit inhaltlich überholt, ohne dass es je einen Marker bekommen hätte. Z1023 („2/4 + schwaches Volumen → kein Dreh") ist neuer (21.08.), nennt die 2 aber nur illustrativ am unteren Rand — unter Schwelle 3 bleibt der Satz wahr, nur trivial.

**Rekonstruktions-Fazit:** Gemeint war mit hoher Wahrscheinlichkeit **3 von 4 (bei geschlossenem QQQ-Gate 2 von 3), ausschließlich für den Dreh**. Die „2" ist ein Rückstand aus der Zeit vor dem 31.07., als Punkt 11 noch den Teilexit-Fall mitabdeckte. Die Fassung, die Fable als Fail-Safe gewählt hat, ist damit sehr wahrscheinlich **auch die ursprünglich gemeinte** — aber sie deckt nur R1 ab und lässt R2/R3 offen.

---

## 2. Nachrechnung an echten Daten

**Datenbasis (selbst ausgezählt, nicht aus einem Bericht übernommen):** 44 dokumentierte Punkt-11-Auswertungen an zwei Tagen.

| Tag | Lage | n | 0/4 | 1/4 | 2/4 | 3/4 | 4/4 |
|---|---|---|---|---|---|---|---|
| 08.09.2026 | **mit offener Position** (Long #VC8-fiktiv) | 28 | 9 | 8 | **10** | 1 | 0 |
| 10.09.2026 | ohne Position (Chasing-Gegencheck, `--punkt11-signal`) | 16 | 11 | 1 | **2** | 2 | 0 |
| **Summe** | | **44** | 20 | 9 | **12** | 3 | 0 |

**Das Entscheidungsband ist real und breit: 12 von 44 Auswertungen (27 %) liegen exakt auf 2/4** — genau dort, wo die Schwelle den Ausgang umdreht. Das ist kein theoretischer Randfall. 4/4 kam an keinem der beiden Tage vor.

### 2.1 Fall A — 08.09.2026, offene Long-Position: was 2/4 gekostet hätte

Während des zweiten (fiktiven) Trades lag die Auszählung über **rund sechs aufeinanderfolgende Voll-Checks** stabil auf 2/4, jeweils mit denselben zwei Kriterien (MACD-H 5min negativ + NAS100-EMA50-Bruch mit Folgekerze), während QQQ hielt:

- *„unverändert 2/4 … QQQ hält noch, keine klare Beschleunigung. Situation stabilisiert sich eher seitwärts"*
- *„jetzt mit zweiter bestätigender Kerze verstärkt … keine weitere Eskalation zu 3/4"*
- *„2/4 unverändert auf bestätigter Basis, aber Tendenz klar rückläufig (Entspannung)"*
- dann: *„**auf 1/4 zurückgefallen** — MACD-Hist(5min) ist wieder positiv"*

**Unter Schwelle 2/4 hätte Punkt 11 in dieser Episode sechsmal hintereinander eine aktive Dreh-/Exit-Handlungsaufforderung verlangt** („explizit und sofort als Handlungsaufforderung melden", Z1008) — bei einer Position, die sich anschließend wieder erholte und am Loop-Ende offen und nahe Entry stand. **Unter 3/4 wäre kein einziges Mal etwas ausgelöst worden.** Was tatsächlich passierte: Sonnet meldete keinen Dreh, sondern kommentierte die Entspannung — es hat in der Praxis also **bereits 3/4 angewendet**, ohne dass das irgendwo stand.

**Einschränkung, ehrlich:** Das ist **eine** Episode an **einem** Tag mit einem fiktiven Trade, dessen Ausgang nie feststand. Es beweist nicht, dass 2/4 falsch ist — es zeigt nur, dass 2/4 in der einzigen dokumentierten Anwendung mit offener Position ≈6 Fehlalarme produziert hätte und 3/4 null. Nach [[feedback_regime_wechsel]]/[[feedback_live_trading]] Punkt 14 ist n=1 kein Kalibrierungsbeweis.

**Zusatzbefund, der gegen die Sicherheitsnetz-Erzählung spricht:** Der Volumen-Dämpfer (Z1014/Z1023) soll ein 2/4 abfangen. Am 08.09. taucht `RVOL`/`Volumen-Check` in der **gesamten** Protokolldatei nur **6×** auf — während der 2/4-Episode **kein einziges Mal**. Ein Dämpfer, der in der Praxis nicht ausgefüllt wird, kann die niedrigere Schwelle nicht absichern. (Deckt sich mit dem 08.09.-Kernbefund „13 Pflichtzeilen in 46/46 Voll-Checks nicht angewendet", [[project_testtag_analyse_2026-09-08]].)

### 2.2 Fall B — 10.09.2026, `--punkt11-signal`: hier dreht die Wirkung um

Die Chasing-Kette VC#51–#53 (19:10:53 / 19:16:21 / 19:21:45 DE), Punkt-11-Kriterien gegen einen SHORT-Bias gewertet:

| VC | Zeit (DE) | Auszählung | Antwort damals (`2-3/4`-Lesart) | `k-ohne-signal` | 13.1-Konsequenz |
|---|---|---|---|---|---|
| #51 | 19:10:53 | **3/4** (Krit. 1, 2, 4) | ja | auf 0 zurückgesetzt | beobachten |
| #52 | 19:16:21 | **2/4** (Krit. 1, 2) | ja | 0 | beobachten |
| #53 | 19:21:45 | **2/4** (Krit. 1, 2) | ja | 0 | beobachten |

**Unter Schwelle 3/4 rechnet sich dieselbe Kette so:** #51 bleibt „ja" (k=0), #52 wird „nein" (k=1), #53 wird „nein" (**k=2**) → nach 13.1 Punkt 1 lautet die Konsequenz dann **zwingend „50 %-Einstieg AKTIV vorschlagen"** und löst den vollen 7b1-Ablauf mit `gate_check.cjs --chasing yes` aus. **Die strengere Zahl erzeugt hier also einen zusätzlichen Entry-Vorschlag, keinen weniger.**

Was daraus geworden wäre (SHORT, Kurs bei VC#53 ≈ 29.190):

- Die SL-Anker-Vorprüfung lief an diesem VC auf `SL-ANKER DERZEIT UNTAUGLICH` (`--entry 29197.05 --sl 29249.95`, verifiziert in `scripts/gate_check_log.jsonl`, Lauf 2026-09-10T17:21:58Z). **Der zusätzliche 13.1-Vorschlag wäre also am selben Tag am N5-Defekt gescheitert** — exakt wie der real dokumentierte 13.1-Override bei VC#58.
- Mit dem regelkonformen SL (29.264,9, siehe Abschnitt 3) wäre er nicht gescheitert. Kursverlauf danach: 19:26 = 29.181,75 → 19:39 = 29.160,75 → 19:59 = 29.159,25. TP1 29.100 **nicht** erreicht, SL 29.264,9 **nicht** getroffen — die Position wäre am Loop-Ende mit ca. **+30 Pkt** offen gewesen. **Kein Ergebnisbeweis in beide Richtungen** ([[feedback_live_trading]] 13.1 Hindsight-Warnung: 13.1 ist eine Prozess-, keine Ergebnisregel).

**Das wichtigste Ergebnis dieser Nachrechnung ist nicht der Kurs, sondern der Interlock:** Am 10.09. war der Unterschied zwischen 2/4 und 3/4 praktisch **unsichtbar**, weil der SL-Defekt (N5) ohnehin alles blockierte. **Sobald N5 behoben ist, wird die W5-Entscheidung erstmals wirksam.** Die beiden Punkte gehören deshalb zusammen entschieden.

---

## 3. W5/N1 — die Optionen

### Option A — „3 von 4, nur Dreh" (Fail-Safe festschreiben)

Z1002/Z1008 auf „3 von 4 (bei geschlossenem QQQ-Gate: 2 von 3)" setzen; Punkt 11 ist **ausschließlich** die Grundlage für den Dreh-Vorschlag; Exit/Teilgewinn/SL-Nachzug laufen unverändert über 4a/9d1, 7c (fünfte Erweiterung) und 12.

- **Pro:** kleinste Änderung; entspricht der rekonstruierten Absicht; entspricht dem, was Sonnet am 08.09. faktisch schon tat; der aktive Fail-Safe wird nur bestätigt, also **null Verhaltensänderung gegenüber heute**; löst W5 und den Z990-Teil von N1 in einem Satz.
- **Contra:** lässt **R2 und R3 ungelöst.** Z1074 behält seine zwei Zahlen im selben Absatz, und `--punkt11-signal` bleibt ein A3-Pflichtfeld mit Hard-Exit 1, dessen Antwort nicht definiert ist. Am 10.09. wurde es 16× beantwortet — mit einer Schwelle, die nirgends steht.
- **Duplizierungs-Check:** sauber. 7c deckt die Zwischenlage (Teilgewinn/SL-Nachzug), 12 den Stall, 4a/9d1 das Muster-Exit. Keine zwei Mechanismen für dasselbe Ereignis.

### Option B — „2 von 4, Punkt 11 gilt auch für den Exit"

Z1002/Z1008 auf 2 festschreiben, Z1010/Z1023 unverändert lassen, Punkt 11 ausdrücklich als Exit-**und**-Dreh-Grundlage benennen.

- **Pro:** passt zur Belegvorlage Z1010 und zum Volumen-Dämpfer, also zu den **zwei jüngsten** „2"-Stellen; senkt die Hürde für den schützenden Vollexit im schmalen Band „2/4 erfüllt, aber kein 4a-/9d1-Muster, kein 7c-Trigger, kein Stall".
- **Contra:** widerspricht dem 31.07.-Paket (Z992/Z862/Z1097) frontal, das den Teilexit-Fall ausdrücklich **aus** Punkt 11 herausgenommen hat; macht Punkt 11 zur niedrigsten Reversal-Schwelle des Regelwerks; hätte am 08.09. ≈6 Fehlalarme erzeugt; und das vorgesehene Sicherheitsnetz (Volumen-Dämpfer) wurde in der Praxis nicht ausgefüllt.
- **Duplizierungs-Check:** **schlecht.** 2/4 = „MACD-H negativ über 2 Checks + EMA50-Bruch mit Folgekerze" ist genau der Zustand, den 7c (fünfte Erweiterung, 5 bzw. 3 Checks) und 12.3 (2 Kerzen + RSI ≥5) bereits abdecken. Punkt 11 würde für dasselbe Ereignis **scharf mitschalten**, mit einer *härteren* Rechtsfolge (Vollexit/Dreh statt Teilgewinn) — und die Punkt-12-Deckelung (Z1074, „NIE Vollexit") liefe leer, weil ihr eigenes Ausnahmetor bei 2/4 offenstünde. Genau der Regel-Interlock, vor dem Z1097 warnt.

### Option C — getrennte Schwellen je Rechtsfolge (EMPFEHLUNG)

Punkt 11 behält **eine** Kriterienliste, bekommt aber eine **explizite Rechtsfolgen-Tabelle**. Drei Zeilen, keine neue Mechanik:

| Rechtsfolge | Schwelle | Begründung |
|---|---|---|
| **Dreh-Vorschlag** (Gegenposition eröffnen) | **3 von 4**; bei geschlossenem QQQ-Gate 2 von 3; Volumen-Dämpfer nachgelagert | frisches Risiko, Kosten-Asymmetrie wie in Z862 beschrieben |
| **Vollexit-Freigabe** über die 12-Deckelung (Z1074) | **2 von 4** — und die Deckelung selbst fällt erst bei **2 von 4** statt bei ≥1 (Z1074 (b) angleichen) | Kapitalschutz; hebt die heutige Lücke „Deckelung weg, aber kein Vollexit-Weg" auf |
| **`--punkt11-signal ja\|nein` (13.1)** | **≥1 Kriterium = „ja"** (der Wortlaut von 13.1: „kein einziges Punkt-11-Kriterium") | dort ist „ja" die **konservative** Antwort (Zähler-Reset = **kein** 50-%-Chasing-Entry) |

Dazu ein Satz, der N1 abschließt: *„Punkt 11 begründet keinen eigenständigen Exit. Der Exit nach Punkt 11 ist immer das Schließen-Bein eines Drehs oder die Vollexit-Freigabe nach Z1074. Ein reiner Schutz-Exit wird über 4a/9d1, 7c oder 12 begründet."*

- **Pro:** Das ist die einzige Option, die **alle drei** Fundstellen-Familien widerspruchsfrei bedient, und sie bildet ab, was die Datei **ohnehin schon tut** — sie benennt die Zahlen nur, statt sie unter einem Namen zu verstecken. Sie ist auch die einzige, die die gegenläufige Fail-Safe-Richtung von R1 und R3 berücksichtigt: unter Option A oder B würde eine der beiden Rechtsfolgen zwangsläufig in die riskante Richtung gestellt.
- **Contra:** drei Zahlen statt einer, mehr Text an einer ohnehin langen Stelle — Punkt 14 („eine Checkliste, die von 2 auf 9+ Pflichtzeilen wächst, wird unter Zeitdruck selbst zur Fehlerquelle") ist ein ernstzunehmender Einwand. Gegenargument: es sind keine neuen Prüfungen, sondern **eine Tabelle über schon existierende Rechtsfolgen**; die Auszählung im Loop bleibt identisch (die vier Kriterien werden ohnehin bei jedem Check-in ausgewiesen, Cron-Item (4)).
- **Duplizierungs-Check:** sauber, und besser als A. 7c/12 bleiben für Teilgewinn/SL-Nachzug zuständig (25–50 %, NIE Vollexit); Punkt 11 wird bei 2/4 **nur** zum Türöffner für den Vollexit, bleibt aber selbst kein Auslöser — der Auslöser bleibt 7c/12/9d1/4a. Damit schaltet an keinem Ereignis zweimal etwas scharf.

### Empfehlung und Verbindlichkeitsgrad

**Option C**, und zwar gestaffelt:

1. **Sofort verbindlich, weil es keine Regeländerung ist, sondern eine Disambiguierung:** Dreh = 3/4 (QQQ zu: 2/3), Punkt 11 begründet keinen eigenständigen Exit. Beides ist heute schon der Fail-Safe bzw. der Wortlaut von Z990/Z992/Z1097 — es ändert **kein** Verhalten gegenüber dem aktuellen Stand, es macht nur nachlesbar, was gilt. Punkt 14 (Tempo-Bremse) greift hier nicht, weil keine neue Regel scharf geschaltet wird.
2. **Sofort verbindlich, weil ein Hard-Exit-1-Feld eine definierte Antwort braucht:** `--punkt11-signal ja` ab dem ersten erfüllten Kriterium. Das ist eine **Wiederherstellung** des 13.1-Wortlauts, nicht eine neue Regel, und die Richtung ist konservativ (mehr „ja" → weniger 13.1-Entries). Am 10.09. hätte das an VC#52/#53 nichts geändert (dort wurde bereits „ja" geantwortet), an VC ohne Position mit 1/4 (eine Stelle, 18:55) wäre aus „nein" ein „ja" geworden.
3. **Schattenmodus statt Sofortentscheidung** für die eine Zahl, die tatsächlich Verhalten ändert: **die Vollexit-Freigabe bei 2/4 (Z1074)**. Sie wird erst relevant, wenn wieder eine Position offen ist, und die Datenlage dafür ist n=1 Episode. Vorschlag analog zu Q2/Q4: in der ohnehin bestehenden Punkt-11-Pflichtzeile mitführen („2/4 → Vollexit-Freigabe wäre offen, Deckelung 12 griffe nicht"), aber **noch keine Rechtsfolge**, bis ~10 Voll-Checks mit offener Position im 2/4-Band gesammelt sind. Bis dahin bleibt die heutige Deckelung (25–50 %) unverändert bindend.

**Falls du es kürzer willst:** Option A allein ist vertretbar und besser als der Status quo — aber dann muss `--punkt11-signal` trotzdem separat definiert werden, sonst bleibt das einzige maschinengelesene Punkt-11-Feld undefiniert. Das ist dann C, nur ohne die Vollexit-Zeile.

---

## 4. N5 — die SL-Kurzformel im Cron-Block

### 4.1 Was am 10.09.2026 tatsächlich passiert ist

Verifiziert an `scripts/gate_check_log.jsonl` (nicht aus dem Bericht übernommen; 210 Zeilen, davon 60 vom 10.09., 150 vom 11.09.):

- **42 von 60 Läufen am 10.09. trugen `--sl 29249.95`** — exakt den Struktur-Anker. Urteile dieser 42: **38× `SL-ANKER DERZEIT UNTAUGLICH`**, 2× `NICHT ABSCHLIESSEND PRUEFBAR`, 1× `TAUGLICH`, 1× `FAIL` (Live-Lauf). Das deckt sich mit [[project_testtag_bilanz_2026-09-10]] („ab VC#21 40×").
- `--sl-anker` wurde am 10.09. **kein einziges Mal** übergeben (0 von 60) — der Parameter wurde erst am 11.09. A3-pflichtig.
- **Am 11.09. (nach dem Fix):** 145 von 150 Läufen mit `--sl-anker`, davon **141× SL ≠ Anker** (Puffer angewendet, z. B. 29.264,9 vs. Anker 29.249,95 = 14,95 = 0,5 × ATR 29,8). Die **4** verbliebenen `SL == Anker`-Versuche endeten **alle mit Exit 1** (`EINGABEFEHLER SL/Anker`, `gate_check.cjs` Z838).

**Bewertung der Frage „Regelverstoß oder nur unglücklich formuliert?" — beides, in dieser Reihenfolge:**

- Der **berechnete SL war regelwidrig**: 7b1 Schritt 3a („Der Anker ist der Bezugspunkt, NIE der SL-Preis selbst") und [[feedback_chartanalyse]] 8c2 (Puffer ≥0,5 × ATR ab der getesteten Zonenkante) waren zum 10.09. beide schon in Kraft. 38 Urteile sagten das jedes Mal ausdrücklich.
- **Eine Order ist daraus nie geworden** — das System hat in jedem einzelnen Fall angeschlagen. Es gab also keinen Kapitalschaden und keinen unbemerkten Durchgriff.
- **Der Schaden war ein anderer:** 40+ Zyklen liefen gegen eine Wand, und der einzige real handelbare Moment des Tages (VC#54, 19:25, Short 29.185,15, regelkonformer SL 29.264,9, TP1 29.100, RR 1,068) erreichte deshalb nie das volle Gate. Das ist ein **Prozessfehler mit Opportunitätskosten**, kein stiller Regelbruch.
- **Ursache ist genau die Kurzformel:** Item (8) des Cron-Blocks ist der einzige Text, den Sonnet im Trigger-Moment garantiert liest. Er sagt „SL=MAX(Struktur,Floor)". Das Wort „Struktur" liest sich als der Strukturpunkt selbst — und genau das wurde eingesetzt. Formal ist es kein Widerspruch (Schritt 3 „technisches Level" ist laut 3a der gepufferte Wert), aber es ist eine **Unvollständigkeit an der gefährlichsten Stelle**, und die Datei dokumentiert an vier Stellen (16.07./27.07./26.08./09.09.), dass eine Regel, die nicht wörtlich im Cron-Block steht, strukturell ausfällt.

### 4.2 Textvorschlag

Die vollständige, dreigliedrige Formel **steht bereits im selben Codeblock**, 55 Zeilen weiter oben, in Item (6f) (Z195–196):

> `--sl <Anker ∓ 0,5×ATR>` — bewertet wird der REGELKONFORME SL = MAX(Anker ∓ 0,5×ATR, sichere Cluster-Kante ∓ 0,5×ATR, 8c-Floor)

Es ist also keine neue Formulierung nötig, nur die gleiche Zeile an der zweiten Stelle. **Ist (Z247–251, Auszug):**

```
(8) Voller 7b1-Ablauf, Schritt 0 + Schritte 1-5 (Schritt 0 = SL-Anker-Vorprüfung ZUERST,
    siehe Item (6f) — bindend seit 07.09.2026, Rechtsfolge E3c: URTEIL UNTAUGLICH → kein
    7b1-Ablauf, keine Order; erst bei TAUGLICH: ATR ablesen → SL-Floor rechnen →
    SL=MAX(Struktur,Floor) → erst danach TP/RR → `gate_check.cjs` aufrufen; W4-Korrektur
    14.09.2026) — Aufrufweg
```

**Vorschlag (nur die eine Zeile ersetzt, +1 Zeile im Block):**

```
(8) Voller 7b1-Ablauf, Schritt 0 + Schritte 1-5 (Schritt 0 = SL-Anker-Vorprüfung ZUERST,
    siehe Item (6f) — bindend seit 07.09.2026, Rechtsfolge E3c: URTEIL UNTAUGLICH → kein
    7b1-Ablauf, keine Order; erst bei TAUGLICH: ATR ablesen → SL-Floor rechnen →
    SL = MAX(Anker ∓ 0,5×ATR, sichere Cluster-Kante ∓ 0,5×ATR, 8c-Floor) — der Anker ist
    NIE der SL (7b1 Schritt 3a; 10.09.2026: 40× SL exakt auf dem Anker) → erst danach
    TP/RR → `gate_check.cjs` aufrufen; W4-Korrektur 14.09.2026, N5-Ergänzung 14.09.2026)
```

Eigenschaften: **kein neuer Begriff** (Wortlaut identisch zu Item (6f)), keine neue Pflichtzeile, keine neue Rechtsfolge, +74 Zeichen / +1 Zeile in einem Block von ~290 Zeilen. Die Struktur des geschützten Blocks bleibt unangetastet (gleiche Einrückung, gleiche Klammer-Konvention, Item-Nummerierung unverändert). Der kurze Vorfallsverweis „40× SL exakt auf dem Anker" ist bewusst drin — er ist das, was die Formel im Trigger-Moment merkfähig macht; wenn du den Block knapp halten willst, kann er entfallen, ohne dass Regelinhalt verloren geht.

### 4.3 Blocker oder nicht?

**Kein Blocker für den nächsten Testtag — aber die höchstpriorisierte Nicht-Blocker-Position.** Begründung:

- Der **Code fängt den Fall inzwischen ab**: `--sl-anker` ist seit 11.09. A3-pflichtig ohne `--grund-`-Ausweg, `gate_check.cjs` weist `SL == Anker` **und** „Puffer < 0,5 × ATR" als Eingabefehler mit Hard-Exit 1 zurück (Z838/Z843), `--sl-vorpruefung --sl-auto` rechnet den regelkonformen Kandidaten selbst und `vollcheck.cjs` ruft `--sl-auto` auf. Der 11.09.-Log belegt das live: 4 von 4 Versuchen abgebrochen.
- Was der Code **nicht** verhindert, ist der Umweg: Sonnet rechnet nach der Kurzformel, läuft in Exit 1 bzw. UNTAUGLICH, korrigiert oder auch nicht — am 10.09. wurde 40× nicht korrigiert. Der Code schützt das Kapital, nicht die Zyklen.
- Es ist eine **Ein-Zeilen-Textvervollständigung ohne Regelinhalt** (die Formel gilt seit 11.09., sie steht nur an dieser Stelle unvollständig). Punkt 14 (Tempo-Bremse) greift also nicht — es wird nichts scharf geschaltet.

**Empfehlung: vor dem nächsten Testtag erledigen**, zusammen mit der W5/N1-Entscheidung, weil erst der N5-Fix die W5-Entscheidung überhaupt wirksam macht (Abschnitt 2.2).

---

## 5. N2, N3, N4 — brauchen die eine eigene Entscheidung?

| | Inhalt | Einordnung |
|---|---|---|
| **N2** | Belegvorlage Z1010 endet mit „→ 2/4, Exit gerechtfertigt" | **Folgekorrektur, mit einer Formulierungsentscheidung.** Die Formatvorlage bleibt wörtlich (sie ist die Pflichtzeilen-Vorlage), aber das **Verdikt** am Ende muss zur Entscheidung passen. Unter Option C z. B.: „→ 2/4 → kein Dreh (Schwelle 3/4), Vollexit-Freigabe nach Z1074 offen". Fällt mit der W5/N1-Entscheidung automatisch an, braucht keine eigene Levi-Entscheidung. |
| **N3** | Volumen-Dämpfer ist nur für „2/4" formuliert (Z1014, Z1023) | **Reine Folgekorrektur.** Die Formulierung wird zahlunabhängig gemacht („dämpft eine gerade erfüllte Schwelle"), dann läuft der Dämpfer unter jeder gewählten Zahl. Keine eigene Entscheidung. *Separat davon* bleibt der praktische Befund aus 2.1 stehen: der Dämpfer wurde am 08.09. nicht ausgefüllt — das ist ein Disziplinthema, keine Regelfrage. |
| **N4** | Fünf weitere „2-3 von 4"-Stellen ohne Marker: Z854, Z856, Z862, Z1074, Z1097 | **Drei davon (Z854, Z856, Z862) sind reine Folgekorrekturen** — sie referenzieren Punkt 11 nur beschreibend als „die hohe, träge Schwelle" und werden mit der Zahl mitgezogen. **Z1097** ebenso (mechanisch). **Z1074 ist KEINE Folgekorrektur** — der Absatz trägt eine eigene, zweite Schwelle („keines der vier … erfüllt" = ≥1) und ist genau die R2-Stelle aus Abschnitt 1.1. **Z1074 gehört in die Entscheidung, nicht in die Nacharbeit.** |

**Zusammengefasst: N3 und vier der fünf N4-Stellen sind Nacharbeit und müssen nicht separat entschieden werden. N2 ist Nacharbeit mit einer Formulierungswahl. Z1074 (aus N4) ist Teil der W5/N1-Entscheidung selbst.** Dazu kommt eine im Bericht noch nicht erfasste Stelle: **13.1 / `--punkt11-signal`** (Z1158, Z1167, Cron-Block Z155, `vollcheck.cjs` Z408–417) — das ist die dritte Rechtsfolge und ebenfalls Teil der Entscheidung, nicht der Nacharbeit.

---

## 6. Gesamtempfehlung in vier Sätzen

1. **W5/N1 → Option C:** eine Kriterienliste, drei benannte Rechtsfolgen — **Dreh 3 von 4** (QQQ zu: 2 von 3), **`--punkt11-signal` ab ≥1 Kriterium „ja"**, **Vollexit-Freigabe bei 2 von 4 zunächst nur im Schattenmodus mitführen**; Punkt 11 begründet keinen eigenständigen Schutz-Exit (der läuft über 4a/9d1/7c/12).
2. **Punkte 1 und 2 sofort verbindlich** (beides Disambiguierung bzw. Wiederherstellung eines Wortlauts, null Verhaltensänderung gegenüber heute), **Punkt 3 erst nach ~10 dokumentierten 2/4-Momenten mit offener Position** — analog zum Vorgehen bei Q2/Q4.
3. **N5 → Cron-Item (8) auf die dreigliedrige Formel aus Item (6f) vervollständigen**, vor dem nächsten Testtag; kein Blocker (der Code fängt den Fall ab, 4/4 belegt), aber die Ein-Zeilen-Korrektur mit dem besten Verhältnis im ganzen Paket — und erst sie macht die W5-Entscheidung überhaupt wirksam.
4. **N2/N3/N4 brauchen keine eigene Entscheidung**, mit einer Ausnahme: **Z1074** gehört inhaltlich zu W5/N1 und darf nicht als Textangleichung mitlaufen.

**Ehrliche Einordnung der Datenlage:** 44 Auswertungen an zwei Tagen, davon 28 an einem einzigen fiktiven Trade, 0 echte Trades seit dem 21.08. — das ist ein **Plausibilitätsbeleg, kein Kalibrierungsbeweis**. Die Empfehlung zu Punkt 1/2 trägt trotzdem, weil sie den bestehenden Zustand nur festschreibt. Die Empfehlung zu Punkt 3 trägt ausdrücklich **nicht** ohne die Schattenmessung.

---

## 7. Was ich bewusst NICHT vorschlage

- **Die vier Kriterien selbst anzufassen** (Z997–Z1000). Die konjunktive Formulierung „1 UND 2 UND 3" neben einer Zählschwelle ist inkonsistent, aber sie umzuschreiben wäre eine echte Regeländerung an einer Stelle, die seit 13.07.2026 unverändert läuft. Falls dich das stört: als eigene Frage aufnehmen, nicht in dieses Paket.
- **Den Volumen-Dämpfer zu streichen**, obwohl er am 08.09. nie ausgefüllt wurde. Das ist ein Disziplin-, kein Regelproblem — und die Pflichtzeile steht seit 26.08. im Cron-Block (Item (3)).
- **Die Kürzungen K1–K30** aus [[project_pruefung_feedback_live_trading_2026-09-14]] anzustoßen. Unabhängige Frage, unverändert offen.

## ENTSCHIEDEN 14.09.2026 (Levi): Opus-Empfehlung angenommen

Levi: "Ja bitte so an Fable zur Umsetzung geben". Option C (getrennte Schwellen je Rechtsfolge) verbindlich: Dreh-Vorschlag 3/4, Punkt 11 begründet KEINEN eigenständigen Exit, `--punkt11-signal`/13.1-Zählerreset ab ≥1 Kriterium "ja", Vollexit-Freigabe bei 2/4 (Z1074) nur Schattenmodus (~10 Momente, kein bindendes Verhalten). N5 (Cron-Item (8) SL-Formel vervollständigen) zusammen mit W5 umsetzen, da N5 erst W5 wirksam macht. Fable-Umsetzung beauftragt 14.09.2026, danach Opus-Gegencheck (Autor≠Prüfer), Levi committet selbst.

## Punkt 11 + N5 umgesetzt 14.09.2026 (Fable)

**Status:** umgesetzt, **Opus-Gegencheck ausstehend** (Autor ≠ Prüfer). Kein Commit, kein Push — Levi committet selbst. Tests `node --test tests/trading_scripts.test.js`: 73/73 vor → 73/73 nach den Änderungen. Keine Codeänderung.

**Geänderte Dateien:** `feedback_live_trading.md` (1.204 → 1.219 Zeilen, 285.003 → 290.695 B; Wiki-Links 45 → 47 Ziele, neu: [[project_pruefung_feedback_live_trading_2026-09-14]] + [[project_punkt11_sl_empfehlung_2026-09-14]], kein Link gebrochen), `feedback_vollcheck_format.md` (Template A, `--punkt11-signal`-Zeile — wird von `loop_prompt.cjs` injiziert, Ausgabe geprüft), `feedback_chartanalyse.md` (Z584, Deckelungs-Spiegel von Punkt 12), `MEMORY.md` (Indexzeilen).

| Punkt | Umsetzung in `feedback_live_trading.md` (neue Zeilennummern) |
|---|---|
| **1 Dreh 3/4** | Z1009 („mindestens 2-3 dieser Kriterien" → „mindestens **3 dieser 4** … bei geschlossenem QQQ-Gate 2 der verbleibenden 3", alte Zahl als Kursiv-Klammer), Z1023 How to apply („Sobald 2-3 … Fail-Safe" → „Sobald 3 von 4 …"), Z997 Vorrang-Klärung („volle 2-3-von-4-Bestätigung" → „3-von-4"). Querverweise mitgezogen: Z854, Z856, Z862 (7c) und Z1112 (12.3 Scope) — alle beschreiben Punkt 11 weiter als „hohe, träge Schwelle", jetzt konsistent mit 3/4; wo eine alte Zahl stand, steht sie als „vorher „2-3 von 4"" daneben. Verbleibende „2-3 von 4"-Treffer in der Datei: 4, alle in „vorher/bis dahin stand hier"-Klammern (maschinell geprüft). |
| **2 Kein eigenständiger Exit** | Z997 (Vorrang-Klärung, fett: „Punkt 11 begründet KEINEN eigenständigen Exit"), Z1011–1019 (neue **Rechtsfolgen-Tabelle** anstelle des W5-Markers + eigener Satz „Punkt 11 begründet KEINEN eigenständigen Exit. Der Exit nach Punkt 11 ist immer das Schließen-Bein eines Drehs …"), Z1001 (User-Vorgabe: Schließen + Drehen = EIN Vorschlag), Z1025 (Belegpflicht: Titel „für jeden Punkt-11-Exit" → „für jeden Punkt-11-Dreh-Vorschlag", Verdikt-Beispiel N2, Schlusssatz), Z1073 (Punkt 12 Regel 3: „Punkt 11 als Dreh"), Z1089 (Deckelung: „nur über Punkt 11 selbst" → „nur als Schließen-Bein eines Punkt-11-Drehs (3 von 4)"), Cron-Item (4) Z130–133. |
| **3 `--punkt11-signal ja` ab ≥1** | **Code-Befund:** `scripts/vollcheck.cjs` Z408–421 rechnet keine Schwelle — `--punkt11-signal` ist eine manuelle Enum-Eingabe (`ja` → k=0, `nein` → k+1, fehlt → Hard-Exit 1); die Schwelle stand bisher **nirgends** explizit. Reine Regeltext-Ergänzung, kein Code-Fix: Z1173 (13.1 Präzisierung: „kein einziges Kriterium" = schon EIN Kriterium ist Signal, bewusst niedriger als 3/4, Begründung), Z1182 (13.1 verankert, Definition vor „Ab k=2"), Cron-Item (6b) Z160–161, `feedback_vollcheck_format.md` Z150 (Template A situativ). |
| **4 Vollexit 2/4 nur Schatten** | Z1089 (Deckelung): Opus-Vorschlag (Freigabe bei 2/4 + Bedingung (b) auf „weniger als 2") ausdrücklich als **BEOBACHTET, NICHT BINDEND** formuliert; Bedingung (b) bleibt „keines der vier". Rechtsfolgen-Tabelle Z1017 Zeile 3 gleichlautend. **Gewählte Schattenlösung:** feste Textzeile `Vollexit-Schatten (11): 2/4 — keine Rechtsfolge` als Teil der Belegpflicht-Zeile bei genau 2/4 (Cron-Item (4) Z132, Z1023, Belegvorlage Z1025), Zählung beim Tagesabschluss per Textsuche im Protokoll, Nachtrag hier. **Warum kein Log-Feld:** kein Skript verarbeitet die Kriterienzahl — `vollcheck.cjs` kennt nur ja/nein für 13.1 (ohne Position), `position_tick.cjs` hat kein Punkt-11-Feld; ein neues Feld + JSONL wäre neue Infrastruktur für ~10 Beobachtungsmomente. Der Cron-Block ist der Ort, den Sonnet sicher liest — deshalb die Zeile dort und nicht nur im Fließtext. **Zählerstand: 0/~10 Voll-Checks mit offener Position im 2/4-Band.** |
| **5 N5 Cron-Item (8)** | Z252–258 (N5-Zeilen Z255–258) wörtlich nach Opus-Vorschlag: „SL = MAX(Anker ∓ 0,5×ATR, sichere Cluster-Kante ∓ 0,5×ATR, 8c-Floor) — der Anker ist NIE der SL (7b1 Schritt 3a; 10.09.2026: 40× SL exakt auf dem Anker)", +2 Zeilen, Einrückung/Klammer-Konvention unverändert. Befund: `loop_prompt.cjs` injiziert den Cron-Block **nicht** (es liest nur Template A/B aus `feedback_vollcheck_format.md`) — der Block ab Z119 wird weiterhin manuell wörtlich in den CronCreate-Prompt kopiert; deshalb muss der nächste Loop-Start den aktualisierten Block neu übernehmen. |
| **6 N2–N4** | N2: Belegvorlage endet jetzt „→ 2/4 → kein Dreh (Schwelle 3/4) \| Vollexit-Schatten (11): 2/4 — keine Rechtsfolge", altes Verdikt als Historie mit Trade-#24-Einordnung. N3: Volumen-Dämpfer Z1029/Z1038 zahlunabhängig („gerade erfüllte Dreh-Schwelle + schwaches Volumen → kein Dreh"), AVWAP-Modifikator Z1040 mitgezogen. N4: Z854/856/862/1112 erledigt (s. Punkt 1), Z1089 gehört zu Punkt 4. |

**Cron-Block Bilanz:** +7 Zeilen (Item (4) +3, Item (6b) +2, Item (8) +2), keine Item-Nummer verändert, keine Pflichtzeile entfernt.

### Offene Punkte / Lücken für den Opus-Gegencheck (bewusst NICHT von Fable geschlossen)

1. **Lücke Punkt 12 Deckelung, Bedingung (b) (Z1089):** (b) lautet weiter „keines der vier Punkt-11-Kriterien erfüllt". Bei 1/4 oder 2/4 entfällt die Deckelung damit formal — aber keine Regel begründet dann einen Vollexit (Punkt 11 erst bei 3/4 und nur als Dreh; 12 und 7c fünfte Erweiterung sagen „NIE Vollexit"). Ob die 25-50-%-Deckelung in diesem Band fortgilt, ist im Text nicht bestimmt. Im Text als „Offen für den Opus-Gegencheck" markiert. Vor der Entscheidung war die Lücke schon da (Opus-Bericht Abschnitt 3, Option C „hebt die heutige Lücke auf"), durch Punkt 2 wird sie jetzt sichtbar statt implizit. Kandidaten-Antworten (Opus/Levi): (a) Deckelung gilt bis 3/4 fort (dann (b) auf „weniger als 3" ziehen), (b) Schattenmodus-Ergebnis abwarten.
2. **Schatten-Zeile im Quick-Tick:** Cron-Item (4) gilt bei JEDEM Check-in mit offener Position (Quick-Tick + Voll-Check); Opus' Zählgröße war „~10 Voll-Checks". Fable-Festlegung: gezählt werden nur Voll-Checks (Minute % 5 == 0), die Zeile erscheint aber auch im Quick-Tick. Opus möge bestätigen oder auf „nur Voll-Check" einengen.
3. **Konjunktive Kriterienliste** (Z1004–1007, „1. … 2. UND … 3. UND …") bleibt wie von Opus empfohlen unangetastet — neben „3 von 4" weiterhin sprachlich inkonsistent.
4. **Andere Dateien mit „2-3 von 4":** `feedback_live_trading_dax.md` Z58/Z80 („für DAX 2-3 von 3 statt 2-3 von 4") — DAX-Datei wegen [[feedback_dax_trennung]] nicht angefasst; die DAX-Schwelle ist eine eigene Entscheidung. `feedback_positionsfokus_bei_offener_position.md` Z12 ist Vorfallserzählung (18.08.), belassen.
5. **`vollcheck.cjs` Z417 Fehlermeldung** („Punkt-11-Kippsignal in diesem Voll-Check? ja -> k=0") nennt keine Schwelle — Kosmetik, kein Verhaltensunterschied; nicht geändert (kein Code-Mandat).
6. `feedback_chartanalyse.md` Z584 (Spiegel der Deckelung) wurde als Folgekorrektur mitgezogen — außerhalb des wörtlichen Auftrags (nur `feedback_live_trading.md`), aber dieselbe Zahl in einer zweiten Live-Datei; Opus möge das absegnen oder zurückdrehen.

---

## Opus-Gegencheck Punkt 11 + N5, 14.09.2026

**Gesamturteil: FREIGEGEBEN MIT AUFLAGEN.** Fables Textumsetzung entspricht Levis Entscheidung (Option C) und ist an allen selbst nachgeschlagenen Fundstellen korrekt zitiert. Aber: **ein Blocker im Code**, den Fable übersehen und in seinem Bericht sogar ausdrücklich falsch ausgeschlossen hat. Blocker ist gefixt, Nachbesserungen umgesetzt, Tests 73/73 → **74/74**. Kein Commit, kein Push, `trades.db` unberührt.

### BLOCKER (gefixt): `position_tick.cjs` rechnete weiter mit 2/4 und meldete bei 2/4 einen Dreh

Fables Bericht (Punkt 4, Begründung "Warum kein Log-Feld"): *"`position_tick.cjs` hat kein Punkt-11-Feld"*. Das ist **sachlich falsch**. `scripts/position_tick.cjs` ist genau das Skript, das bei JEDEM Check-in mit offener Position läuft (Template B, von `loop_prompt.cjs` injiziert, ~190×/Tag) und dessen Ausgabe wörtlich ins Protokoll übernommen wird — und es rechnet die komplette Punkt-11-Kriterienliste inklusive Schwelle selbst:

```
Z79  const REVERSAL_SCHWELLE = 2;   // Punkt 11: "mindestens 2-3 dieser Kriterien" — Untergrenze 2 (siehe Abschlussbericht: Auslegung offen)
Z376 const signal = erfuellt >= REVERSAL_SCHWELLE;
Z386 `${erfuellt}/${nenner} -> REVERSAL-SIGNAL (Schwelle ${REVERSAL_SCHWELLE}-3 von ${nenner}...) -> als Handlungsaufforderung melden: ${exitPreis}; ggf. Gegenrichtungs-Setup nennen, Levi entscheidet (Punkt 11)`
```

Wirkung ohne Fix: Genau die 12 von 44 nachgerechneten 2/4-Auswertungen (Abschnitt 2 oben) hätten die Maschine dazu gebracht, **wörtlich eine Dreh-Handlungsaufforderung mit konkretem Exit-Preis** auszugeben — also exakt die Rechtsfolge, die Levis Entscheidung abgeschafft hat, und zwar aus dem Text, der im Loop am zuverlässigsten gelesen wird. Der Regeltext hätte 3/4 gesagt, die Skriptausgabe daneben 2/4 = Dreh. Das ist schlimmer als der Zustand vor der Entscheidung.

**Fix (selbst umgesetzt, klein und eindeutig):** Konstante nach Nenner getrennt, Textbausteine angeglichen.

```
const REVERSAL_SCHWELLE_VON4 = 3; // Punkt 11 Dreh-Schwelle: 3 von 4 (Levi-Entscheidung 14.09.2026, Option C)
const REVERSAL_SCHWELLE_VON3 = 2; // bei geschlossenem QQQ-Session-Gate (7e): 2 der verbleibenden 3
const revSchwelle = k3Entfaellt ? REVERSAL_SCHWELLE_VON3 : REVERSAL_SCHWELLE_VON4;
```

Live gegengeprüft (vier Läufe): 2/4 → `KEIN Reversal-Signal (Dreh-Schwelle 3 von 4)`; 3/4 → `REVERSAL-SIGNAL (Dreh-Schwelle 3 von 4)`; QQQ-Gate zu + 2/3 → `REVERSAL-SIGNAL (Dreh-Schwelle 2 von 3)`. Dazu ein **Regressionstest** in `tests/trading_scripts.test.js` (alle vier Fälle + Schattenzeile), weil die Schwelle vorher durch **keinen einzigen** Test gedeckt war — das ist der Grund, warum sie zwei Regelrevisionen unbemerkt überlebt hat.

### Der Cron-Injektions-Verdacht hat sich NICHT bestätigt — N5 wirkt, sogar doppelt

Selbst verifiziert in `scripts/loop_prompt.cjs` (149 Zeilen, vollständig gelesen): Das Skript liest aus `feedback_vollcheck_format.md` **nur** Template A und Template B (`fencesNach(lines, /^\*Template A\b/, 2)`), alles andere sind hartkodierte `block.push(...)`-Strings. Den Cron-Block aus `feedback_live_trading.md` Z119 ff. liest es nicht — Fables Befund stimmt, der Block wird manuell kopiert.

Entscheidend ist aber, was daraus folgt, und das hat Fable nicht geprüft: Der **tatsächlich injizierte** Block trägt die dreigliedrige SL-Formel bereits wörtlich:

> `SL = MAX(Anker +/- 0,5 x ATR, sichere Cluster-Kante +/- 0,5 x ATR, 8c-Floor 1,5 x ATR) — nicht der naive Anker-Puffer` (`loop_prompt.cjs`, Zeile "SL-ANKER IST NIE DER SL (TODO 1, 10.09.: 40x SL exakt auf 29249.95)")

N5 war also im maschinell erzeugten Prompt seit dem 11.09. schon repariert; Fables Ergänzung in Cron-Item (8) schließt die Lücke im manuell kopierten Zweitexemplar. Beide Wege sagen jetzt dasselbe. **Kein Blocker, sondern Info/OK** — und die ursprüngliche N5-Diagnose ("die Kurzformel ist der einzige Text im Trigger-Moment") war insoweit zu pessimistisch.

Ebenfalls verifiziert: `feedback_vollcheck_format.md` Z150 (Template A, situativer Block) liegt innerhalb der zweiten Code-Fence nach der `*Template A`-Markerzeile und **wird injiziert**. Fables Änderung dort wirkt.

### Code-Befund `vollcheck.cjs` Z408–421 — bestätigt

`const p11 = enumArg(args, 'punkt11-signal', ENUM_JA_NEIN);` … `else if (toBool(p11)) kAbl = { wert: 0, ... }` — reine manuelle ja/nein-Eingabe, keine Schwellenrechnung, Hard-Exit 1 bei Fehlen. Eine Präzisierung zu Fables Formulierung: Der Hard-Exit greift nur, wenn `--state` gesetzt, das Chasing-Kriterium erfüllt UND `--k-ohne-signal` nicht manuell übergeben ist (Z417) — der manuelle Zähler bleibt der dokumentierte Ausweg. Ändert nichts an der Bewertung.

### Nachbesserungen (selbst umgesetzt)

1. **Schattenzeile jetzt maschinell + Takt-Marker** (löst Fables offenen Punkt 2). `position_tick.cjs` druckt bei genau 2/4 selbst `Vollexit-Schatten (11): 2/4 — keine Rechtsfolge [Voll-Check|Quick-Tick]`. Begründung: Eine handgeschriebene Pflichtzeile ist genau die Bauart, die am 08.09.2026 in 46/46 Voll-Checks ausgefallen ist ([[project_testtag_analyse_2026-09-08]]) — der Schattenzähler hätte auf 0 stehen bleiben können, ohne dass es jemandem auffällt. Der Marker löst zugleich das Zählproblem: `position_tick.cjs` läuft bei JEDEM Check-in, die Zählgröße sind aber ~10 **Voll-Checks**; ohne Marker wäre die Textsuche beim Tagesabschluss um Faktor ~5 verzerrt. Gezählt werden nur Treffer mit `[Voll-Check]`. Regeltext an vier Stellen nachgezogen (Cron-Item (4), Rechtsfolgen-Tabelle Zeile 3, "How to apply", Belegvorlage) inkl. ausdrücklichem "nicht zusätzlich von Hand schreiben (sonst Doppelzählung)".
2. **Rechtsfolgen-Tabelle Zeile 1** nennt jetzt, dass `position_tick.cjs` die Schwelle selbst rechnet — damit ist die Maschinenseite im Regelwerk sichtbar und der nächste Schwellenwechsel vergisst sie nicht wieder.

### Fables offener Punkt 1 (Deckelung, Bedingung (b)) — KEINE Regellücke, aufgelöst statt vorgelegt

Im Volltext von Punkt 12 nachgelesen: Der Deckelungs-Absatz (Z1089) ist **deklaratorisch, nicht konstitutiv** — er sagt in seinem eigenen "Why": *"obwohl Punkt 12 selbst schon immer nur 'Teilgewinn, KEIN Vollexit' verlangte. Diese Ergänzung macht das nur explizit, ändert die bestehende Logik nicht."* Die 25–50-%-Begrenzung steht unabhängig von (a)/(b) dreifach im eigenen Regeltext: Punkt 12 Regel 2 (Z1072, "NICHT automatisch den Vollexit"), Regel 3 (Z1073, "ersetzt keinen Vollexit-Mechanismus") und 12.3 Abgrenzung zu 7c (Z1117, "NIE Vollexit/Dreh"). Ein Wegfall von (b) bei 1/4 oder 2/4 öffnet also **kein** Vollexit-Tor; er nimmt nur dem Erinnerungssatz seine Bedingung. **Levi muss hier nichts entscheiden.** Im Text ist der "Offen"-Marker durch die Auflösung ersetzt. Bedingung (b) auf "weniger als 3" zu ziehen wäre reine Kosmetik und wurde bewusst NICHT gemacht — das wäre eine halbe Vorentscheidung zugunsten der 2/4-Freigabe, die im Schattenmodus noch offen ist.

### Vollständigkeits-Scan (gesamte Datei, selbst ausgezählt)

- `"2-3 von 4"`: 4 Treffer (Z997, Z1011, Z1089, Z1112) — alle in "bis dahin/vorher stand hier"-Klammern. Fables Angabe stimmt.
- Alle `X von 4` / `X/4`-Treffer einzeln geprüft: Z842 ("Kriterium 3 entfällt, 2 von 3 verbleibenden"), Z861/Z863/Z869 (7c), Z1009/1014–1019/1023/1025/1029/1038/1040, Z1089, Z1112, Z1173/Z1182 — durchgehend konsistent mit 3/4 bzw. 2/3. **Keine 9. übersehene Fundstelle im Fließtext.** Die übersehene Stelle lag im Code, nicht im Text.
- Durchgerechnetes Beispiel 2/4 nach der neuen Fassung: kein Dreh (Z1019/Z1023), `--punkt11-signal ja` (Tabelle Zeile 2 + Z1173/Z1182, ab ≥1), Schattenzeile mit Takt-Marker (jetzt maschinell). Alle drei Rechtsfolgen sind im Text an je zwei Stellen redundant belegt und widersprechen sich nicht. Verwirrungspotenzial für einen Erstleser: gering, seit die Schwelle auch in der Skriptausgabe mitläuft.

### Querprüfungen

- **4a/9d1, 7c, 12, 12.3, 13.1:** kein Widerspruch zur neuen Rechtsfolgen-Tabelle. 7c (fünfte Erweiterung) und 12 bleiben die Schutz-Exit-Wege (NIE Vollexit/Dreh), 12.3 Scope-Klarstellung (Z1112) trägt jetzt 3/4, 13.1 (Z1173/Z1182) trägt die ≥1-Definition. Die in Abschnitt 1.1 beschriebene gegenläufige Fail-Safe-Richtung ist an beiden Stellen richtig herum umgesetzt.
- **`feedback_chartanalyse.md` Z584 (Fables offener Punkt 6): FREIGEGEBEN.** Die Stelle ist der wörtliche Spiegel der Punkt-12-Deckelung für 8e und nennt dieselbe Zahl. Sie nicht mitzuziehen hätte eine **aktive Falschangabe in einer zweiten Live-Datei** stehen lassen — das wäre der übergriffigere Fehler gewesen. Formulierung korrekt, Schattenmodus korrekt als Verweis statt als eigene Regel.
- **`feedback_live_trading_dax.md` Z58/Z80 ("2-3 von 3"): zu Recht nicht angefasst** ([[feedback_dax_trennung]]) — aber als eigene, jetzt sichtbar gewordene Entscheidung vormerken, sonst trägt die DAX-Datei dauerhaft die alte Spanne.
- **Tests:** 73/73 vor dem Gegencheck (selbst ausgeführt), **74/74** nach Blocker-Fix + neuem Regressionstest.

### Offen für Levi (nicht von Opus entschieden)

1. **Vollexit-Freigabe bei 2/4** bleibt Schattenmodus, Zählerstand 0/~10 — unverändert, nur die Messung ist jetzt maschinell verlässlich.
2. **DAX-Schwelle** (`feedback_live_trading_dax.md`, "2-3 von 3"): eigene Entscheidung, bisher nirgends terminiert.
3. **Kosmetik, kein Handlungsbedarf:** `vollcheck.cjs` Z417-Fehlermeldung nennt weiterhin keine Schwelle; die konjunktive Kriterienliste (Z1004–1007, "1. … 2. UND … 3. UND …") bleibt neben "3 von 4" sprachlich inkonsistent — beide wie ursprünglich empfohlen unangetastet.

**Verlässlich für den Live-Loop?** Ja — jetzt. Vor dem Blocker-Fix war die Datei verlässlich und die Maschine daneben falsch, was in der Praxis schlechter ist als beides falsch. Einzige verbleibende Handlungsauflage vor dem nächsten Testtag: **den Cron-Block ab Z119 neu in den CronCreate-Prompt kopieren** (er wird nicht injiziert) und `loop_prompt.cjs --testtag …` frisch ausführen.
