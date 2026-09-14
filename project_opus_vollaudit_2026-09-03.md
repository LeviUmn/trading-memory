---
name: project-opus-vollaudit-2026-09-03
description: "Unabhängiges Opus-Vollaudit über den gesamten Umsetzungsprozess der 25-Punkte-Auftragsliste 03.09.2026 plus der drei Nachtrags-Punkte (28 Punkte gesamt), über alle drei Fable-Runden hinweg: Code-Gegenprobe an scripts/gate_check.cjs, x_fetch_stamp.cjs, quote_check.cjs (alle Pfade real ausgeführt), Textstellen-Gegenprobe an 9 memory-Dateien, Konsistenzprüfung des Gesamtsystems nach allen Runden, Plausibilitätsprüfung der 20-Tage-Auszählung und der VIX-vs-VXN-Entscheidung gegen live geholte CBOE-Daten. Ergebnis: 25 BESTÄTIGT, 3 ABWEICHEND, 0 NICHT GEFUNDEN. Drei Konsistenzbefunde mittleren Gewichts (Tweet-Fälligkeits-Widerspruch im operativen Template, Terminalbedingung fehlt in der Format-Zeile, veraltetes Kommandobeispiel im Skript-Kopfkommentar), ein Zahlenfehler in der 20-Tage-Auszählung (K3-neu 1/20 statt 0/20, intraday gerechnet). Freigabeempfehlung: committet-fähig mit einer Vorab-Entscheidung und zwei Nachzügen vor dem nächsten Testtag."
metadata:
  node_type: memory
  type: project
  status: draft
  originSessionId: e1a9edc1-4a4d-4c86-b03e-98d36d93627e
  modified: 2026-09-03T11:19:00.128Z
---

# Opus-Vollaudit des Umsetzungsprozesses 03.09.2026 (alle drei Runden)

**Prüfauftrag (Levi, wörtlich):** „alle Änderungen von Opus prüfen ob alles so passt und umgesetzt wurde" — vollständiger Audit über den gesamten Prozess, nicht nur über die zuletzt offenen Punkte, vor dem Commit.

**Prüfer:** unabhängige Opus-Instanz ohne Vorkontext zur laufenden Session.

**Gelesen:** [[project_testtag_analyse_2026-09-01]], [[project_testtag_analyse_2026-09-02]], [[project_fable_auftragsliste_2026-09-03]], [[project_fable_umsetzung_2026-09-03]], [[project_opus_gegencheck_fable_empfehlungen_2026-09-03]], [[project_fable_finalentscheidung_2026-09-03]] (inkl. Nachtrag).

**Gegengeprüft am realen Artefakt:** `scripts/gate_check.cjs` (voller `git diff`, +272/−6 Zeilen, plus gezielte Codelektüre), `scripts/x_fetch_stamp.cjs`, `scripts/quote_check.cjs`, `scripts/level_register.json`, `.gitignore`, Git-Status beider Repos — und **13 real ausgeführte Skriptläufe** über alle in den Berichten behaupteten Pfade. Regelwerk gegengelesen: `feedback_chartanalyse.md`, `feedback_live_trading.md`, `feedback_vollcheck_format.md`, `feedback_loop_ablauf_uebersicht.md`, `feedback_tagesabschluss.md`, `feedback_session_update.md`, `feedback_zeitzone.md`, `feedback_trading_zeitfenster.md`, `project_testtag_plan_01_02_09_2026.md`.

**Zusätzlich extern verifiziert:** VIX- und VXN-Tagesbars live über das TradingView-MCP (`CBOE_DLY:VIX`, `CBOE_DLY:VXN`, Timeframe D) gegen die Zahlen der 20-Tage-Auszählung. Der Chart wurde danach auf `FOREXCOM:NAS100` / 5min zurückgestellt (Abruf um 13:09 Uhr, vor Session-Start).

**Keine Regeländerung, kein Code, kein Commit, kein MEMORY.md-Eintrag durch diese Prüfung.**

**Bilanz vorab: 25 BESTÄTIGT | 3 ABWEICHEND | 0 NICHT GEFUNDEN.** Kein Punkt ist zwischen den drei Runden verloren gegangen. Freigabeempfehlung: **committet-fähig**, mit einer Entscheidung vor dem Commit und zwei Einzeiler-Nachzügen vor dem nächsten Testtag.

---

## Ebene 1 — Wurde umgesetzt, was behauptet wird?

### SKRIPT-FIX (7 Punkte) — alle BESTÄTIGT

**#1 `--cluster-level` (BESTÄTIGT, abweichend wie dokumentiert).** `{ key: 'cluster-level' }` steht auf der A3-Pflichtliste (`gate_check.cjs:1551`). Die vier Zweige liegen in `evaluateTrade()`: `clusterLevelNone` (Z. 736, Auswertung Z. 1215–1221), Normalpfad (Z. 1221 ff.), „Zone da, ATR fehlt" → `pass:null` (Z. 1229–1236, der von Fable zusätzlich gefundene Fall), Batch-Altsemantik (Z. 1237–1238), UNKNOWN mit Grund (Z. 1241–1247). Der `none`-Weg ist die einzige Abweichung von der Auftragsliste und in [[project_fable_umsetzung_2026-09-03]] begründet — die Begründung trägt: ohne ihn wäre PASS an jedem Tag ohne wick-getestete Zone strukturell unerreichbar.
**Real ausgeführt, alle fünf Pfade grün:** `none` → `[PASS ✓] … GEPRUEFT, Regel nicht einschlaegig`; fehlend ohne Grund → `Fehlende Pflicht-Messfelder … --cluster-level`, Exit 1; fehlend mit `--grund-cluster-level` → `[N/A]` + `MESSFELD-AUSNAHME (A3)`-Zeile + GESAMTSTATUS UNKNOWN; `--cluster-level abc` → eigene Ungültig-Meldung, UNKNOWN; Zone ohne ATR → `pass:null` mit korrekter Detailzeile. Batch-Lauf liefert unverändert `Keine Cluster-Zone angegeben — Regel nicht ausgeloest (Batch-Modus: alte Semantik…)`, Exit 3.

**#2 Frische-Guard Levelregister (BESTÄTIGT).** `LEVEL_REGISTER_HARD_MAX_AGE_MINUTES = 90` (Z. 471, mit 27-zeiligem Begründungskommentar ab Z. 444). Negativ-Alter-Exit `ageMinutes < -1` (Z. 1684–1688), Hard-Exit über der Obergrenze (Z. 1689–1693), 60-Min-Warnung unverändert (Z. 1694). Beide Exits liegen hinter der frühen `--batch`-Rückkehr (Z. 1518) und zusätzlich hinter `wantsRegister` (Z. 1630) — also live-only und nur bei behauptetem TP-Level, konsistent mit dem GC-5-Präzedenzfall.
**Real ausgeführt:** Aufruf mit `--tp1-level-price` gegen den echten Registerstand → `Register ist 910 Min alt und ueberschreitet die HARTE Obergrenze von 90 Min …`, Exit 1. Das 1440-Min-Provisorium aus Runde 1 ist restlos ersetzt, kein Rest im Code.

**#7 `x_fetch_stamp.cjs` (BESTÄTIGT).** Neue Datei, 143 Zeilen, sauberer Kopfkommentar mit Fehlerhistorie. **Real ausgeführt, alle Guards greifen mit Exit 1:** Zukunfts-Stempel (`> jetzt+2 Min`), fehlendes/falsches Z-Format, Rückwärtslauf; `--force` schreibt die bewusste Korrektur; `--check` liefert Delta + `faellig (>= 10 Min): JA/NEIN`. Schreibweg im Regelwerk umgestellt ([[feedback_session_update]] Schritt 2, Z. 38: „AUSSCHLIESSLICH über den Guard-Helper, nie mehr von Hand per echo/Write").

**#8 Rundzahl-Entzirkularisierung (BESTÄTIGT, bewusst nur Option B).** `isGeneratedLevel()` (Z. 491–493) matcht `/rundzahl/i` gegen `levelart`/`type`; die Registereinträge tragen `"levelart": "Rundzahlen"` — greift also. Wirksam an allen drei Ausgabestellen: TP1-Herkunftszeile (`printResult`, Z. 1298 ff.), 8b1-Gate-Detail (Z. 890 ff.), TP2-Detail (Z. 1032 ff.).
**Real ausgeführt** mit `--tp1-level-price 29200`: `TP1-Level-Herkunft: Rundzahl 29200 @ 29200 — REGISTER-TREFFER, GENERIERTE KATEGORIE — KEINE unabhaengige Verifikation …`. Die Begründung für Option B statt eines künstlichen „unabhängigen Abrufwegs" ist inhaltlich richtig — Rundzahlen sind Arithmetik, jede Konstruktion wäre Scheinverifikation.

**#9 Q2-Budget-Formel (BESTÄTIGT).** Block Z. 1569–1589, vor der A3-Schleife, hinter der Batch-Rückkehr. Formel im Kommentar verbindlich dokumentiert und gegen VC#40 vom 01.09. verifiziert.
**Real ausgeführt:** ohne Wert → `Q2-BUDGET SELBST BERECHNET … (1,5 - 1.1) / 1,5 * 100 = 26.667%`, und das A3-Feld gilt danach als befüllt (die Selbstberechnung schreibt `args['dual-gate-q2-budget-pct']`, Z. 1577). Mit widersprüchlichem Wert → Konsistenzwarnung.

**#10 `quote_check.cjs` (BESTÄTIGT).** Neue Datei, 108 Zeilen. **Real ausgeführt:** vollständiger Block im Protokoll → `WOERTLICH: JA … ab Zeile 6`, Exit 0; nach Entfernen der Q-Score-Zeile → `WOERTLICH: NEIN`, längster Treffer `24/25`, erste Abweichung zeilengenau benannt, Liste der nirgends vorkommenden Zeilen, Exit 1. Genau das geforderte Verhalten.

**#20 `--entry-source` (BESTÄTIGT).** A3-Pflichtliste Z. 1555, Durchreichung Z. 1721, Ausgabezeile mit drei Zweigen (Wert / Batch-Sonderfall / fehlend) Z. 1338–1346. **Real ausgeführt:** `Entry-Herkunft: Live-Kurs bei Gate-Aufruf (--entry-source; erwartete Kategorien: …)`; im Batch `… im Batch-Modus ohne Pflicht`.

### REGELWERK-FRAGE (5 Punkte) — alle BESTÄTIGT

**#3 8d-Kriterium 2 (BESTÄTIGT, Opus-Option D).** [[feedback_chartanalyse]] Z. 492 ist vollständig neu gefasst: „(a) auf der Standard-Blackout-Liste UND (b) messbar vom Konsens abgewichen", Ist/Konsens/Marktreaktion dokumentationspflichtig, keine Prozentschwelle, Latch bis Handelsschluss, 1–2-Tage-Rückblick ausdrücklich gestrichen, alter Wortlaut als ~~durchgestrichen~~ zitiert. Genau die Fassung, die der Gegencheck empfohlen hat.

**#11 8d-Kriterium 3 (BESTÄTIGT, gekoppelt).** Z. 493: Richtungsfilter, **nur** die erste Klausel (VIX ≥ Vortagesschluss), zweite Klausel ausdrücklich verworfen mit Flacker-Begründung. Neuer Block „8d-Ratschen-Fix" Z. 507 mit der Monotonie-Rechnung und der Kopplungsbegründung, K3-Schattenmessung Z. 509, Entscheidungsregel („~15 Handelstage oder nächstes Opus-Meilenstein-Review"), erweiterte Pflichtzeile Z. 515. Fable hat die eigene widerlegte Begründung („K1 bildet nachlassende Wirkung ab") im Dokument selbst als falsch markiert — das ist die richtige Behandlung.

**#12 Abbruch-Gate (BESTÄTIGT, vollständig).** [[project_testtag_plan_01_02_09_2026]] Z. 127–148, Abschnitt „ABBRUCH-GATE v2". Enthält alle vier vom Gegencheck geforderten Bestandteile: dreiwertiges Feld (Z. 137–140), Fail-Closed (Z. 141), Geltungsbereich nach Opus-Option D (Z. 142), geänderter Prüfer-Auftrag im Wortlaut (Z. 144). (c) neu gefasst mit Klarstellung (i) (Z. 133), (d) auf „registerbezogene Exit-1-Ursachen" verallgemeinert mit Klarstellungen (ii)/(iii) (Z. 135). Alte Tag-2-Fassung als historisch markiert (Z. 129). Register-UTC-Konvention für fiktive Testtage ergänzt (Z. 148).

**#21 Register-Tragfähigkeit (BESTÄTIGT, Option A mit beiden Präzisierungen).** Code: `tagHoch`/`tagTief` (Z. 1755–1756), Anzeige-Block Z. 1166–1192, Ausgabe Z. 1348–1352 — `!isBatch`-geschützt, fließt nicht in `status`/`overallPass`/`exitCode` ein. **Real ausgeführt, alle vier Pfade:** innerhalb der Range → Doppelrollen-Formulierung („als ZIELMARKE erreichbarkeitsbelegt … als HALTEZONE geschwächt … gemischte Information, kein Mangel per se"), außerhalb, nur ein Wert übergeben, vertauscht (`EINGABEFEHLER`), gar nicht übergeben („nicht geprueft"). Regelwerk: [[feedback_chartanalyse]] Z. 486, [[feedback_live_trading]] Z. 609. Die optionale statt A3-pflichtige Variante ist die vom Gegencheck als risikoärmer benannte — sauber begründet.

**#22 Frischefenster + Registeralter (BESTÄTIGT, mit den korrigierten Zahlen).** Registeralter 90 Min im Code (s. #2). MTF-Frische: Pflicht-Templatezeile [[feedback_vollcheck_format]] Z. 28 mit **Bezugspunkt Kerzen-SCHLUSS** und den korrigierten Schwellen **12/32/122**, Format-Zeilen-✓ Z. 40, voller Abschnitt Z. 79 inkl. Spreizungspflicht, Anschlussregel an die #15-Timestamps in [[feedback_loop_ablauf_uebersicht]] Z. 36. `updated`-Konvention in [[feedback_session_update]] Z. 163. Fables ursprüngliche 7/17/62-Tabelle ist restlos ersetzt, kein Rest.

### PROMPT/PROZESS (13 Punkte) — 11 BESTÄTIGT, 2 ABWEICHEND

| # | Status | Fundstelle |
|---|---|---|
| **#4** Zeitanker | BESTÄTIGT | [[feedback_live_trading]] Z. 871–875 (vier Pflichten, wörtlich wie beauftragt); [[feedback_vollcheck_format]] Z. 20 (Kopfzeile `HH:MM:SS`) + Z. 71 |
| **#5** „Wörtlich" | BESTÄTIGT | [[feedback_live_trading]] Z. 619–622 (Ein-Fassungs-Regel, Kurzform-Kennzeichnung, quote_check-Pflicht) |
| **#6** Halbierungsfenster | BESTÄTIGT | [[feedback_vollcheck_format]] Z. 40 (`[\| Halbierungsfenster 15:30–16:00 aktiv]`) + Z. 75; Querverweis [[feedback_trading_zeitfenster]] Z. 37 |
| **#13** Auszählen | BESTÄTIGT | [[feedback_tagesabschluss]] Z. 152 („lückenlos" nur mit Minutenraster-Gegenrechnung) |
| **#14** VC-Nummer | BESTÄTIGT | [[feedback_vollcheck_format]] Z. 40 + Z. 73 („ohne Nummer gilt die Pflichtzeile als fehlend") |
| **#15** Timestamp-Nachweis | BESTÄTIGT | [[feedback_loop_ablauf_uebersicht]] Z. 36 inkl. 20:55-Unzulässigkeitsregel |
| **#16** Terminalbedingung | **ABWEICHEND** | [[feedback_live_trading]] Z. 1092 vorhanden und korrekt — **aber die dort verlangte Format-Zeilen-Komponente existiert im Format-Zeilen-Template nicht**, s. Befund K-2 |
| **#17** Tweet-Fälligkeit | **ABWEICHEND** | [[feedback_vollcheck_format]] Z. 77 vorhanden — **steht aber im Widerspruch zur Template-Zeile Z. 38 derselben Datei**, s. Befund K-1 |
| **#18** 2/2 Zustand/Ereignis | BESTÄTIGT | [[feedback_live_trading]] Z. 561–565 (beide Definitionen, VC#7-Muster); getrennte Zählung [[feedback_tagesabschluss]] Z. 152 |
| **#19** Zählstände | BESTÄTIGT | [[feedback_vollcheck_format]] Z. 34 (Template) + Z. 83 + Format-Zeilen-✓ |
| **#23** Screenshot-Namen | BESTÄTIGT | [[feedback_live_trading]] Z. 839 (Default-Name unverändert, Verbot simulierter Zeiten + Doppelendungen) |
| **#24** Zeitbasen | BESTÄTIGT | [[feedback_zeitzone]] Z. 25–29 |
| **#25** Zählweise 2-beinig | BESTÄTIGT | [[feedback_live_trading]] Z. 562 („x von 3" ausdrücklich unzulässig) |

### Nachtrags-Punkte (3) — 2 BESTÄTIGT, 1 ABWEICHEND

**N1 20-Tage-Auszählung: ABWEICHEND.** Die Auszählung existiert, die Daten sind echt und die Hauptzahlen stimmen — aber eine der vier Ergebniszahlen ist auf der falschen Zeitebene gerechnet (Befund B-3) und eine Unsicherheit ist nicht benannt (B-4). Details in Ebene 2b.

**N2 VIX vs. VXN: BESTÄTIGT** (mit einer Korrektur an einer Stützzahl, Befund B-6). Entscheidung, Begründung und Revisit-Trigger stehen in [[feedback_chartanalyse]] Z. 511.

**N3 Zwei kosmetische Codefixes: BESTÄTIGT.** Q2-Toleranz auf 0,1 pp verschärft (Z. 1584, mit Toleranzangabe in der Warnung); `clusterLevelInvalid` als eigene Erkennungsklasse (Z. 1112–1113) mit eigener Meldung (Z. 1244–1245). **Beide real getestet:** `--impuls-reifegrad-atr 1.1 --dual-gate-q2-budget-pct 12.3` → Warnung mit „14.37 Prozentpunkte … (Toleranz 0,1 pp)"; `--cluster-level abc` → `… ist UNGUELTIG — weder Zahl noch "none"/"keine" (Tippfehler?)` statt der irreführenden „fehlt"-Meldung.

### Zählung

| | BESTÄTIGT | ABWEICHEND | NICHT GEFUNDEN |
|---|---|---|---|
| SKRIPT-FIX (7) | 7 | 0 | 0 |
| REGELWERK-FRAGE (5) | 5 | 0 | 0 |
| PROMPT/PROZESS (13) | 11 | 2 (#16, #17) | 0 |
| Nachtrag (3) | 2 | 1 (N1) | 0 |
| **Summe (28)** | **25** | **3** | **0** |

**Keine einzige Behauptung „umgesetzt in Datei X" war unbelegt.** Das ist bemerkenswert — bei drei Runden und 28 Punkten hätte ich mit mindestens einem NICHT-GEFUNDEN gerechnet. Die Berichte sind ehrlich, auch dort, wo sie eine Abweichung von der Beauftragung einräumen (#1 `none`-Pfad, #8 nur Option B, #22 pro-Level-`updated` abgelehnt).

---

## Ebene 2 — Ist das Gesamtsystem nach allen drei Runden intern konsistent?

Das ist der Teil, den keine der drei Runden geprüft hat, weil jede nur die eigenen Punkte im Blick hatte. Drei Befunde mittleren Gewichts, drei kleine, zwei explizite Entwarnungen.

### K-1 (mittel) — Die Tweet-Fälligkeit widerspricht sich innerhalb einer Datei, und zwar genau im operativ kopierten Template

[[feedback_vollcheck_format]] enthält jetzt zwei unvereinbare Aussagen:

- **Z. 38 (die Template-Zeile, die Sonnet bei jedem Voll-Check kopiert):** *„(Fälligkeit = festes Kerzenraster, echte Minute % 10 == 0 — **NICHT Delta seit dem letzten Ist-Abruf**; Kerzenraster-Fix 28.08.2026 …)"*
- **Z. 77 (die neue #17-Regel):** *„‚fällig' darf … nur stehen, wenn BEIDE Bedingungen erfüllt sind: (a) Raster-Slot … UND **(b) das Minuten-Delta aus `x_last_fetch.json` ist tatsächlich ≥10**"*

Und der Block bei Z. 61 wiederholt die alte Fassung („Die Fälligkeit hängt nicht mehr am Minuten-Delta …") ohne Historie-Markierung.

Fable hat den **identischen** Widerspruch in [[feedback_loop_ablauf_uebersicht]] Z. 40 mustergültig behandelt — Klammer „Fälligkeitsmechanik erneut abgelöst — Stand 03.09.2026 gilt: … UND Delta ≥10 Min", alter Absatz durchgestrichen als Historie. In der Datei mit dem tatsächlich kopierten Template ist genau das unterblieben. Damit steht der 24.08.-Befund („zwei widersprüchliche Fälligkeitsmechaniken nebeneinander"), den Fable in Runde 1 ausdrücklich beheben wollte, jetzt in der wichtigeren der beiden Dateien.

Zweitens: Die Template-Zeile hat **keinen Platz für das `x_fetch_stamp --check`-Ergebnis**. Eine Pflicht, deren Ausgabe im Template nicht vorgesehen ist, fällt erfahrungsgemäß binnen eines Handelstags weg — das ist die Fehlerklasse, gegen die #14 und #19 gebaut wurden.

**Nachtrag ist ein Einzeiler:** Klammertext in Z. 38 auf die Doppelbedingung umstellen, Z. 61 als Historie markieren, im Template hinter „letzter Fetch vor Xmin" ein `(x_fetch_stamp --check: JA/NEIN)` ergänzen.

### K-2 (mittel) — #16 verlangt eine Format-Zeilen-Komponente, die es im Format-Zeilen-Template nicht gibt

[[feedback_live_trading]] Z. 1092 (Punkt 15d) lautet: *„… trägt … eine Pflichtzeile im Fließtext **UND in der Format-Zeile**"*. Die kanonische Format-Zeile in [[feedback_vollcheck_format]] Z. 40 lautet:

```
Format: Fließtext ✓ | Voll-Check #<N> vollständig (MTF ✓ | MTF-Frische ✓ | 11b ✓ | 8d ✓ | ADX ✓ | Zählstände ✓ | Tweet-Check ✓/n.a. | Register ✓ | Screenshot ✓) [| Order-Sperre 15:00–15:30 aktiv] [| Halbierungsfenster 15:30–16:00 aktiv]
```

Das Wort „Terminalbedingung" kommt in [[feedback_vollcheck_format]] **an keiner Stelle** vor (einziger Treffer ist ein Nebensatz in der Why-Begründung zu Punkt 4). Order-Sperre und Halbierungsfenster haben in Runde 1 ihren eckigen Klammer-Slot bekommen — die Terminalbedingung nicht. Am 02.09. ist genau dieser Punkt ausgefallen, weil er nur im Fließtext einer anderen Datei stand; die Reparatur reproduziert die Ursache.

**Nachtrag ist ein Einzeiler:** `[| Terminalbedingung 21:45 geprüft ✓]` in die Template-Zeile, plus zwei Sätze im zugehörigen Abschnitt.

### K-3 (mittel) — Das „vollständige Beispiel" im Kopfkommentar von `gate_check.cjs` ist nicht mehr lauffähig

`scripts/gate_check.cjs` Z. 58–66 führt unter der Überschrift *„Vollständiges Beispiel:"* einen Aufruf, der `--cluster-level` und `--entry-source` nicht enthält. **Real ausgeführt:**

```
Fehlende Pflicht-Messfelder (ohne begleitendes --grund-<feld>): --cluster-level, --entry-source
EXIT=1
```

Zusätzlich erzeugt dasselbe Beispiel jetzt eine unnötige Warnung, weil seine Zahlen `--impuls-reifegrad-atr 1.1` und `--dual-gate-q2-budget-pct 12.3` einander nach der neu festgeschriebenen Formel widersprechen (Formelwert 26,667 %).

Fable hat beide 7b1-Templates in [[feedback_live_trading]] (Z. 600 und Z. 614) korrekt und vollständig ergänzt — inklusive `[--tag-hoch <X> --tag-tief <X>]`. Das Beispiel im Skript selbst wurde übersehen. Der Kopfkommentar warnt zwar an anderer Stelle „NICHT als Live-Vorlage kopieren", nennt dieses Beispiel aber trotzdem „vollständig" — und genau diese Fehlerklasse (Template im Skript hinkt der A3-Liste hinterher) ist am 27.08. und am 31.08. bereits zweimal aufgetreten und in den Kommentaren beider Male vermerkt. Dritter Auftritt.

**Explizit geprüft und in Ordnung:** Außerhalb dieser einen Stelle existiert **kein** weiteres aktives Kommandobeispiel ohne die neuen Pflichtfelder. Ich habe alle memory-Dateien nach `gate_check.cjs --entry` und nach `dual-gate-qqq-abstand-pct` durchsucht; die einzigen weiteren Treffer stehen in [[project_regelwerk_ueberarbeitung_2026-08-26]] (historisches Umsetzungsprotokoll, kein Template) und in den Testtag-Rohprotokollen (Vergangenheitsbelege). Die A3-Erweiterung erzeugt also genau **einen** Template-Bruch, nicht mehrere.

### K-4 (niedrig) — Abbruch-Gate v2, Klarstellung (i) erfasst nur eine von drei nicht-strukturellen UNKNOWN-Klassen

Die neue (c)-Fassung schließt UNKNOWN aus „fehlender oder per `--grund-<feld>` ersetzter Messfeld-Angabe" aus. Der Code kennt seit dieser Runde aber drei nicht-strukturelle UNKNOWN-Quellen im 8c2-Gate: fehlend (erfasst), **ungültiger Wert** (`clusterLevelInvalid`, Runde 3 — nicht erfasst) und **Zone angegeben, aber ATR fehlt** (Runde 1 — nicht erfasst).

**Warum das nicht kritisch ist:** (c) verlangt zusätzlich *positiv*, dass das UNKNOWN „aus einem strukturellen Verifikationsfehler (ein korrekt registriertes Level wurde vom Registerabgleich nicht getroffen)" stammt. Beide Lücken scheitern schon an dieser Hauptbedingung, das Gate löst also nicht falsch aus. Die Klarstellung ist unvollständig, nicht falsch. **Empfehlung, falls (c) ohnehin nochmal angefasst wird:** (i) auf „jedes UNKNOWN, das nicht aus dem Registerabgleich stammt" verallgemeinern statt drei Fälle aufzuzählen.

**Vollständigkeitsprüfung der Gegenrichtung — das war die eigentliche Ebene-2-Frage:** Ich habe alle acht Live-Exit-1-Pfade des Skripts gegen die neue (d)-Fassung gestellt: fehlende Basisparameter (Z. 1526), A3-Messfeld (Z. 1615), Preisformat-Schutz (Z. 585/597), Register fehlt (Z. 1637), Register unparsebar (Z. 1644), Register-Schema ungültig (Z. 1648), `updated` in der Zukunft (Z. 1687), Alter > 90 Min (Z. 1692). Die drei registerbezogenen Ursachen sind in (d) namentlich genannt, „Schema ungültig" fällt unter „unlesbar", die drei CLI-Ursachen sind über Klarstellung (iii) generell ausgenommen. **Der Code öffnet keinen Exit-1-Pfad, den die neue Kriterienfassung nicht erfasst.** Das war der dringlichste Punkt des Gegenchecks und er ist sauber geschlossen.

### K-5 (niedrig, aber vor dem Commit zu entscheiden) — `level_register.json` ist weder getrackt noch ignoriert

`scripts/level_register.json` steht **nicht** in `.gitignore` und ist **nicht** getrackt (`git ls-files --others --exclude-standard` listet es). Alle funktional vergleichbaren Statusdateien sind ignoriert: `x_last_fetch.json`, `last_gate_check.txt`, `last_cooldown_check.txt`, `trades.db`.

Ein `git add -A` würde das Register mit dem Stand `"updated": "2026-09-02T19:55:00.000Z"` mitcommitten — ein Stand, der unter der ab heute geltenden 90-Min-Regel **per Definition defekt** ist (910 Min alt, jeder Live-Aufruf bricht ab). Das ist kein Fehler, aber eine unbeabsichtigte Entscheidung: Entweder ins `.gitignore` (konsistent mit den anderen Statusdateien) oder bewusst tracken. Bisher hat das in drei Runden niemand entschieden.

### K-6 (Hinweis, kein Defekt) — die 90-Min-Grenze ist ab sofort scharf und der aktuelle Registerstand reißt sie

Real ausgeführt: 910 Min → Exit 1. Das ist gewollt und in [[feedback_session_update]] Z. 163 sowie [[project_testtag_plan_01_02_09_2026]] Z. 148 angekündigt. Kein Handlungsbedarf, nur Bewusstsein: Am nächsten Handelstag muss Schritt 6 das Register schreiben, **bevor** der erste Gate-Aufruf mit TP-Level kommt — sonst ist der erste Trigger-Moment ein Exit-1-Zwischenschritt.

### K-7 (mittel, eigener inhaltlicher Befund) — die neue K2-Fassung macht konsenslose Blackout-Termine strukturell K2-unfähig

Die neue K2-Definition verlangt kumulativ (a) Blackout-Liste **und** (b) messbare Konsens-Abweichung mit dokumentiertem Ist/Konsens. Auf der Standard-Blackout-Liste ([[feedback_session_update]] Z. 57–66) steht aber unter anderem **„FOMC-Protokolle / Fed-Entscheidungen / Fed-Reden"** — Ereignisse, für die es keinen veröffentlichten Konsenswert gibt und die deshalb Bedingung (b) **niemals** erfüllen können.

Konkrete Folge am eigenen Material: Der 28.08.2026 war ein Tag, an dem eine Fed-Rede den Markt nachweislich in eine Zinserhöhungs-Repricing schickte (Protokoll `testtag/testtag_2026-08-28.md`, Voll-Checks 16:00 und 16:20, wörtlich: *„Der Markt preist jetzt aktiv Zinserhöhungen ein — das ist der fundamentale Auslöser der gesamten Abwärtsbewegung seit 16:00"*). Unter der neuen K2-Fassung wäre dieses Ereignis für K2 **nicht qualifizierend**, egal wie heftig die Reaktion — genau der Tagestyp, für den 8d gebaut wurde.

Das ist keine Kritik an Option D (der Kern — Überraschung statt Termin — ist richtig, und ich habe ihn selbst empfohlen). Es ist eine Lücke im Wortlaut, die weder die Gegencheck-Vorlage noch die Umsetzung benannt hat. **Empfehlung: ein Satz in [[feedback_chartanalyse]] Z. 492**, wie Blackout-Termine ohne veröffentlichten Konsens behandelt werden — mein Vorschlag: *„Ereignisse ohne veröffentlichten Konsenswert (Fed-Reden, FOMC-Protokolle) qualifizieren für K2 nur bei einer im Voll-Check ausdrücklich benannten, dem Ereignis zugeordneten Marktreaktion; die Reaktion ersetzt dort die Konsens-Abweichung."* Das hält die Auditierbarkeit (die Zuordnung muss protokolliert sein) und schließt die Lücke.

### Entwarnung 1 — Die K3-Schattenmessung ist sauber verdrahtet, nirgends scharf

Explizit geprüft, wie beauftragt. Drei Ebenen:

1. **Code:** `gate_check.cjs` prüft 8d konstruktionsbedingt gar nicht (N-15-Klarstellung, [[feedback_chartanalyse]] Z. 349 — `--tier` ist ein manuell übernommener Parameter). Ich habe das Skript nach `vix`, `8d` und `schatten` durchsucht: keine Fundstelle. Es gibt keinen Codepfad, über den K3-neu wirken könnte.
2. **Pflichtzeile:** Der `n/3`-Zähler und die eckige Kriterienklammer enthalten ausschließlich die gate-wirksame K3-Fassung; K3-neu steht in einer **zweiten, separaten** Klammer, ausdrücklich mit „(GEMESSEN, KEIN GATE)" beschriftet ([[feedback_chartanalyse]] Z. 515). Es gibt keine Formulierung, die K3-neu in den Zähler einrechnet.
3. **Wortlaut-Gleichheit:** Die 8d-Pflichtzeile in [[feedback_chartanalyse]] Z. 515 und die Template-Zeile in [[feedback_vollcheck_format]] Z. 30 sind **zeichenweise deckungsgleich** (bis auf die Markdown-Fettung und den Schlusspunkt). Das war die zweite konkret gestellte Ebene-2-Frage — hier gibt es keinen Bruch.

### Entwarnung 2 — die A3-/Optional-Parameter sind in beiden 7b1-Templates konsistent dokumentiert

`--cluster-level <Preis|none>`, `--entry-source "…"` und `[--tag-hoch <X> --tag-tief <X>]` stehen in **beiden** Kommandotemplates ([[feedback_live_trading]] Z. 600 Schritt 5 und Z. 614 Pflichtzeile) in identischer Form und identischer Reihenfolge, plus je einem erklärenden Bullet (Z. 607, 608, 609) mit korrekter Pflicht/Optional-Kennzeichnung. Der einzige Template-Bruch im ganzen System ist K-3.

### Kleinere Beobachtungen ohne Handlungsdruck

- **Namenskollision:** `quote_check.cjs` heißt nach „Zitat", aber „quote" bedeutet in diesem Projekt sonst durchgehend „Kurs" (`quote_get`, `quote`-Tools). `zitat_check.cjs` wäre eindeutiger. Kosmetik, kein Fehler.
- **`--set`-Randfall:** [[feedback_session_update]] Z. 38 regelt „leerer Fetch → kein Set", aber nicht den Fall „gesehene Tweets sind alle älter als der Wasserstand" — dort würde `--set` mit Exit 1 abbrechen (Rückwärtslauf-Guard). Praktisch löst der Loop das durch Weglassen; eine halbe Zeile würde es explizit machen.
- **Vorbestehende, nicht aus dieser Runde stammende Inkonsistenz:** [[feedback_chartanalyse]] Z. 497 („Ergebnis: … entweder Positionsgröße spürbar reduzieren … ODER pausieren") ist seit der 8b1a-Entscheidung vom 24.08. („Schock-Tier = No-Trade") überholt, steht aber unverändert direkt über den frisch überarbeiteten Kriterien. Wer 8d von oben nach unten liest und bei Z. 497 aufhört, bekommt die alte Rechtsfolge. Nicht durch diese Runde verursacht — aber die Runde hat den Abschnitt angefasst und die Gelegenheit nicht genutzt.

---

## Ebene 2b — Die drei Runde-3-Ergänzungen im Einzelnen

### Die 20-Tage-Auszählung

**Handelstag-Fenster: plausibel und exakt.** Ich habe die Börsentage vom 06.08. bis 02.09.2026 nachgezählt: **exakt 20 Werktage**, kein US-Feiertag im Fenster (Labor Day 2026 = 07.09., liegt außerhalb). Die Wochentagsbezeichnungen aller 20 Tabellenzeilen stimmen mit dem realen Kalender überein. Vom Stichtag 03.09. aus sind das genau die letzten 20 abgeschlossenen Handelstage. ✓

**Datenbasis: real, nicht rekonstruiert.** Ich habe die VIX-Tagesbars live über dasselbe Tool erneut geholt (`CBOE_DLY:VIX`, D, 24 Bars). **Alle 20 Hoch/Tief-Wertepaare der Tabelle stimmen zeichengenau mit dem Feed überein.** Alle 20 Prozentwerte habe ich nachgerechnet — 20/20 korrekt auf zwei Nachkommastellen. Die vier Ergebniszahlen sind konsistent: (a) 2/20 = 10 %, (b) 9/20 = 45 %, Produkt 4,5 %, direkte Koinzidenz 0/20 (am 13.08. Range 2,85 %, am 26.08. 3,48 % — beide unter 5 %). Auch die beiden ≥2-Termine-Tage sind memory-belegt: 13.08. PPI + Claims ([[project_makrodaten_2026-08-13_bis_14]]), 26.08. GDP 2nd Est + PCE (`testtag/testtag_2026-08-24` und `_2026-08-25`, beide vorab notiert). **Nichts an dieser Auszählung ist erfunden.**

**B-3 — der eine echte Fehler: K3-neu ist auf der falschen Zeitebene gerechnet.** Der Nachtrag schreibt: *„K3-neu … hätte an **0/20** Tagen gefeuert (VIX-Max 16,82; größtes Δ **+9,5 %** am 01.09., knapp unter der Schwelle …)"*.

+9,5 % ist der **Schluss-zu-Schluss**-Wert (real: 15,20 → 16,34 = +9,52 %, exakt getroffen). K3 ist aber ein **Intraday**-Kriterium: es wird bei jedem Voll-Check gegen den *aktuellen* VIX-Stand geprüft, nicht einmal am Feierabend. Die intraday erreichbare Maximalausprägung ist Tageshoch vs. Vortagesschluss:

- **01.09.2026: Hoch 16,80 vs. Vortagesschluss 14,92 = +12,60 %** → über der +10-%-Schwelle.

Damit gilt: **K3-neu hätte im Fenster an 1/20 Tagen gefeuert, nicht an 0/20**, und die Schwelle wurde nicht „knapp verfehlt", sondern intraday gerissen. (Zur Einordnung, außerhalb des Fensters: 05.08. hätte mit +11,70 % ebenfalls ausgelöst.) Die VIX-≥-20-Klausel bleibt bei 0/20 — Tageshoch-Maximum 16,82.

**Bewertung:** Der Fehler ändert die Entscheidung nicht, er **stärkt** K3-neu sogar — die Formel feuert genau am nervösesten Tag des Fensters und an keinem anderen, was besser ist als „gar nie". Aber die Zahl 0/20 geht so als Baseline in die Schattenphase ein und würde später falsch verglichen. Gehört korrigiert, bevor die Messreihe startet.

**B-4 — eine nicht benannte Unsicherheit: Fed-Reden sind untergezählt.** Die Blackout-Liste enthält „FOMC-Protokolle / Fed-Entscheidungen / **Fed-Reden**" — das mit Abstand häufigste Listenelement. Die Rekonstruktion setzt es an genau 2 von 20 Tagen an (19.08. Minutes, 28.08. Warsh). Realistisch lagen im Fenster mehr Fed-Auftritte; das Protokoll des 28.08. nennt sogar selbst einen zweiten möglichen Redner („Bessent … Jackson Hole"). **(a) = 2/20 ist damit eine Untergrenze, keine belastbare Punktschätzung.** Die Unsicherheitenliste des Nachtrags ist sonst überdurchschnittlich ehrlich — sie benennt die Kalenderrekonstruktion als solche, n=20 als dünn, die Produktformel-Unabhängigkeitsannahme, die erweiterten VIX-Handelszeiten als Obergrenze für (b) und die Release-Bündel-Konvention. Die Fed-Reden fehlen. Unter der neuen K2-Fassung relativiert sich das (siehe K-7), aber die Auszählung war gegen die *Termin*-Fassung gerechnet, in der sie zählen.

**Gesamturteil zur Auszählung:** Methodisch sauber gebaut, Datenbasis real und exakt, Unsicherheiten zu 5 von 6 Punkten ehrlich benannt, eine Ergebniszahl auf der falschen Zeitebene, eine Unsicherheit ausgelassen. Die Kernaussage („deutlich unter Opus' 30-%-Tragbarkeitsgrenze") hält auch nach beiden Korrekturen.

### Die VIX-vs-VXN-Entscheidung

**Ich habe auch die VXN-Tagesbars live geholt** (`CBOE_DLY:VXN`, D, 22 Bars) und über dasselbe Fenster gegengerechnet:

| Behauptung im Nachtrag | Real gemessen | Urteil |
|---|---|---|
| VXN-Tageshoch an 20/20 Tagen ≥ 20 | 20/20 | ✓ exakt |
| VXN-Schluss ≥ 20 an 18/20 Tagen | **19/20** (nur 28.08. schloss mit 19,92 darunter) | **Zahl falsch, Richtung stützt die Aussage** |
| VXN/VIX-Schlussverhältnis 1,34–1,58, Mittel 1,44 | 1,344 / 1,581 / 1,437 | ✓ exakt |
| 01.09.: VXN +8,8 % vs. VIX +9,5 % | +8,82 % vs. +9,52 % | ✓ exakt |

**B-6:** Die „18/20" ist um eins daneben. Der Fehler geht in die Richtung, die die Entscheidung stützt (VXN reißt die 20er-Marke sogar noch häufiger als behauptet) — die Schlussfolgerung bleibt unberührt.

**Ist das Argument intern schlüssig? Ja.** Die Kernaussage — eine „VXN ≥ 20"-Klausel hätte exakt den Degenerationsfehler, den Option C beheben soll — ist durch 20/20 Tageshochs zweifelsfrei belegt.

**Wurde die Alternative fair abgewogen? Ja, ausdrücklich.** Der Nachtrag betrachtet genau die Alternative, nach der zu fragen war (höhere VXN-Schwelle statt Rückkehr zu VIX, ~28 = 1,4 × 20) und verwirft sie mit zwei prüfbaren Gründen: (1) kein externer konventioneller Anker, (2) das Verhältnis ist nicht konstant — und beziffert die Spreizung empirisch mit 1,34–1,58, was ich exakt bestätigen kann. Das ist keine Abkürzung, sondern genau das Argument, das gegen eine vierte unkalibrierte Zahl spricht — dasselbe Argument, das im Gegencheck für Option C sprach, hier konsequent gegen eine VXN-Schwelle angewandt. Auch 18 vs. 20 ist behandelt (im Fenster diskriminiert keine der beiden Zahlen, 18 wäre unverankert). Und es ist ein **Revisit-Trigger dokumentiert** ([[feedback_chartanalyse]] Z. 511: NAS100-Vola-Ereignis, das VXN zeigt, während VIX < 20 bleibt und die Δ-Klausel schweigt). Ich habe hier nichts zu ergänzen.

Ein einschränkender Hinweis, der in keinem der Dokumente steht: Alle diese Zahlen stammen aus einem **einzigen, außergewöhnlich ruhigen Fenster** (VIX-Maximum 16,82, kein einziger echter Vola-Schock in 20 Tagen). Sie belegen zuverlässig, dass VXN dauerhaft über 20 liegt; sie belegen **nicht**, wie sich VIX-20 und VXN-28 in einem Stressregime unterscheiden. Für die Wahl der Referenzgröße genügt das, weil das Degenerations-Argument regimeunabhängig ist. Für die spätere Schwellenkalibrierung genügt es nicht — die Schattenphase muss mindestens einen Vola-Tag enthalten, sonst ist sie nach 15 Handelstagen genauso aussagelos wie das Fenster.

### Die zwei kosmetischen Codefixes

Beide direkt im Code gelesen und real ausgeführt — siehe N3 in Ebene 1. Beide korrekt, minimal-invasiv, Gate-Semantik unberührt, Batch unberührt. Die Toleranzangabe „(Toleranz 0,1 pp)" wurde zusätzlich in den Warntext aufgenommen, was in der Beauftragung nicht verlangt war und die Meldung selbsterklärend macht. Nichts zu beanstanden.

---

## Ebene 3 — Was fehlt, gemessen an der ursprünglichen 25-Punkte-Liste?

Ich bin die Übersichtstabelle aus [[project_fable_auftragsliste_2026-09-03]] (7 SKRIPT-FIX / 5 REGELWERK-FRAGE / 13 PROMPT-PROZESS, 6 Hoch / 13 Mittel / 6 Niedrig) Punkt für Punkt durchgegangen.

**Kein Punkt ist zwischen den drei Runden verloren gegangen.** Alle 25 plus die 3 Nachtragspunkte sind behandelt: 22 hart umgesetzt, 3 bewusst abweichend mit dokumentierter Begründung, 1 als Schattenmessung statt Hart-Umsetzung, 2 als Anzeige statt Gate.

Die vier bewussten Abweichungen und ihre Begründungslage:

| Punkt | Abweichung | Begründung tragfähig? |
|---|---|---|
| **#1** | zusätzlicher `none`-Wert, den die Liste nicht vorsah | **Ja.** Ohne ihn wäre PASS an jedem Tag ohne wick-getestete Zone strukturell unerreichbar — die Liste hätte das Gate faktisch in eine Dauersperre verwandelt. |
| **#8** | nur Option B (ehrlich labeln), kein unabhängiger Abrufweg | **Ja.** Für eine aus dem Kurs generierte Rundzahl existiert kein unabhängiger Abrufweg; Option A wäre Scheinverifikation gewesen. |
| **#11** | Schattenmessung statt sofortiger Umstellung auf K3-neu | **Ja.** Das ist ausdrücklich der zweite Zweig meiner eigenen Gegencheck-Empfehlung, nicht ein Kompromiss dagegen — und mit n=2 Datentagen der richtige. Der nachgewiesene Fehlmodus (Feuern auf Entspannung) ist über den Richtungsfilter sofort raus. |
| **#22** | pro-Level-`updated` (meine Gegencheck-Idee) **abgelehnt** | **Ja, mit Einschränkung.** Das Argument („ein Zeitstempel ist nur so ehrlich wie sein Schreiber — der Tag-1-Fehler war ein nicht nachgeführter WERT") trägt. Der Verweis auf `--tag-hoch`/`--tag-tief` als Ersatz-Frischquelle deckt allerdings nur die Tages-Extrema, nicht die AVWAP-Bänder, die genauso driften. Der dokumentierte Revisit-Trigger fängt das ab. Ich trage die Ablehnung mit. |

Ein Punkt verdient eine ausdrückliche Anerkennung: Bei **#3, #11, #12 und #22** hat Fable nach dem Gegencheck die eigene Position revidiert und die Revision jeweils **mit der eigenen widerlegten Begründung im Dokument** markiert, statt sie stillschweigend zu ersetzen. Bei #12 ist sogar die falsch verortete eigene Begründung („Ermessens-, kein Fähigkeitsproblem") ausdrücklich im Regelwerktext korrigiert ([[project_testtag_plan_01_02_09_2026]] Z. 146). Das ist selten und macht den Prozess nachprüfbar — es ist der Grund, warum dieses Audit überhaupt so schnell verifizierbar war.

**Was nicht Teil der 28 Punkte war, aber zum Commit gehört (Umfangshinweis):** Der Commit wird mehr enthalten als diese Runde. Im memory-Repo stehen 12 modifizierte und 13 unversionierte Dateien; davon stammen `MEMORY.md`, `feedback_chart_layout.md`, `project_risikomanagement.md` sowie sechs Projektdokumente vom 31.08. und die beiden Testtag-Protokolle samt Analysen vom 01./02.09. aus früheren Arbeitsständen (Dateizeitstempel geprüft). Im Code-Repo sind es die drei Skripte plus das Register. „Alle Änderungen committen" heißt hier: rund vier Tage aufgelaufene Arbeit in einem Zug. Kein Fehler — aber Levi sollte wissen, dass er nicht nur die 28 Punkte dieser Runde festschreibt. Bestätigt: **`MEMORY.md` wurde durch diese Runde nicht angefasst** (Dateizeitstempel 31.08., 23:49) — die Leitplanke „kein MEMORY.md-Eintrag" ist über alle drei Runden eingehalten.

---

## Ebene 4 — Gesamturteil und Freigabeempfehlung

### Committet-fähig: **Ja.**

Der Code ist an jedem behaupteten Pfad real ausgeführt und verhält sich wie beschrieben; die Batch-Semantik ist an zwei unabhängigen Läufen als unverändert nachgewiesen; die Regelwerk-Änderungen sind vollständig, an der richtigen Stelle und in sich schlüssig; die empirische Kalibrierung stützt sich auf real geholte, von mir unabhängig nachgeprüfte Marktdaten. Keiner der gefundenen Punkte macht das System schlechter als den Zustand vor dieser Runde — jeder Befund betrifft eine Verbesserung, die noch nicht ganz zu Ende geführt ist. **Es gibt keinen Grund, den Commit zurückzuhalten.**

### Vor dem Commit: eine Entscheidung, 30 Sekunden

**K-5:** `scripts/level_register.json` gehört entweder in `.gitignore` (konsistent mit `x_last_fetch.json`, `last_gate_check.txt`, `trades.db`) oder bewusst ins Repo. Ohne Entscheidung wandert bei `git add -A` ein 910 Minuten alter, unter der neuen Regel per Definition defekter Registerstand in die Historie. Meine Empfehlung: **ignorieren** — es ist eine Statusdatei, die bei jedem Voll-Check neu geschrieben wird, und ein eingecheckter Stand kann nur irreführen. Das Schema ist ohnehin im Kopfkommentar von `gate_check.cjs` dokumentiert.

### Vor dem nächsten Testtag (nicht vor dem Commit): zwei Einzeiler

Beide gehören in dieselbe Fehlerklasse, gegen die diese Runde angetreten ist — „die Regel steht im Fließtext, aber nicht im Template, also fällt sie im Loop weg". An beiden Testtagen ist genau das nachweislich passiert.

1. **K-1:** [[feedback_vollcheck_format]] Z. 38 auf die Doppelbedingung umstellen (der Klammertext sagt derzeit wörtlich das Gegenteil der neuen #17-Regel), Z. 61 als Historie markieren, Platz für das `x_fetch_stamp --check`-Ergebnis im Template schaffen. Fable hat den identischen Widerspruch in [[feedback_loop_ablauf_uebersicht]] bereits mustergültig gelöst — dieselbe Behandlung reicht.
2. **K-2:** `[| Terminalbedingung 21:45 geprüft ✓]` in die Format-Zeile aufnehmen, analog zu Order-Sperre und Halbierungsfenster.

### Bekannt, aber nicht blockierend — Levi kann das bewusst mitnehmen

- **K-3:** Kommandobeispiel im Kopfkommentar von `gate_check.cjs` (Z. 58–66) ist nicht mehr lauffähig. Betrifft nur, wer das Skript liest statt das Regelwerk; die beiden verbindlichen 7b1-Templates sind korrekt. Beim nächsten Anfassen der Datei mitnehmen.
- **B-3:** Die Baseline-Zahl der K3-Schattenmessung sollte von „0/20, knapp verfehlt" auf „1/20 (01.09., +12,6 % intraday)" korrigiert werden, bevor die Messreihe startet — sonst wird nach 15 Handelstagen gegen eine falsche Referenz verglichen. Zwei Sätze in [[project_fable_finalentscheidung_2026-09-03]] und in [[feedback_chartanalyse]] Z. 509.
- **B-6:** „VXN-Schluss 18/20" → 19/20. Reine Zahlenkorrektur, Entscheidung unberührt.
- **K-4:** Klarstellung (i) in Abbruch-Gate v2 (c) verallgemeinern. Kein Auslöserisiko, nur Vollständigkeit.
- **K-7:** Ein Satz zu konsenslosen Blackout-Terminen in der neuen K2-Fassung. Der inhaltlich gewichtigste der Restpunkte — er entscheidet, ob 8d an einem Fed-Tag überhaupt noch greifen kann. Kein Blocker, weil die Fehlerrichtung konservativ ist (8d greift seltener, nicht öfter), aber vor dem Echtgeld-Start zu klären.
- **B-4:** Fed-Reden als Unsicherheit in der 20-Tage-Auszählung nachtragen — (a) = 2/20 ist eine Untergrenze.
- **Vorbestehend:** [[feedback_chartanalyse]] Z. 497 gibt die durch 8b1a überholte Rechtsfolge wieder. Nicht aus dieser Runde, aber beim nächsten 8d-Anfassen mitnehmen.

### Zum Prozess, ohne Diplomatie

Drei Runden, 28 Punkte, null unbelegte Umsetzungsbehauptungen, jede Abweichung von der Beauftragung ausgewiesen und begründet, jede revidierte Position mit der eigenen widerlegten Begründung im Text markiert, und Marktdaten, die einer unabhängigen Live-Gegenprobe an 20 von 20 Datenpunkten standhalten. Das ist deutlich besser als der Zustand, den die beiden Testtagsanalysen beschreiben.

Die verbleibende Schwäche ist über alle drei Runden dieselbe und dieselbe wie im Gegencheck vom Vormittag: **Isolation.** Jede Runde hat ihre Punkte sauber abgearbeitet und dabei nicht geprüft, ob die Nachbarstelle noch dazu passt. Alle drei mittelschweren Befunde dieses Audits (K-1, K-2, K-3) sind genau das — eine neue Regel wurde an einer Stelle korrekt eingeführt und an der zweiten, operativ wichtigeren Stelle nicht nachgezogen. Alle drei sind Einzeiler. Und alle drei betreffen ausgerechnet Templates: die Artefakte, die im Loop tatsächlich kopiert werden. Wenn aus diesem Audit eine einzige Prozesslehre mitgenommen wird, dann diese: **Wer eine Pflichtzeile ergänzt, ändert im selben Zug das Template, in dem sie erscheinen soll — nicht nur den Abschnitt, in dem sie erklärt wird.**

*Entwurf. Keine Regeländerung, kein Code, kein Commit, kein MEMORY.md-Eintrag durch diese Prüfung. Entscheidung liegt bei Levi.*
