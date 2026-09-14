---
name: project-testtag-analyse-2026-09-01
description: "Unabhängige Opus-Analyse des unbeaufsichtigten Solo-Testtags 01.09.2026 (0 Trades, 84 Voll-Checks): Handelsentscheidungen durchgehend regelkonform, aber Levelregister-Lücke nur formal geschlossen (Frische-Guard inert, TP1-Verifikation bei Rundzahlen zirkulär), 8c2-Gate bei #64/#66 durch weggelassenes --cluster-level still deaktiviert, 'wörtliche' gate_check-Ausgaben erneut redigiert, x_last_fetch.json mit ~2h Zukunftsstempel korrumpiert, Faktenprotokoll an vier Stellen nachweislich falsch — und die 19-Min-Zeitdifferenz ist ein strukturelles Loop-Problem, kein Latenz-Artefakt."
metadata: 
  node_type: memory
  type: project
  status: draft
  originSessionId: e1a9edc1-4a4d-4c86-b03e-98d36d93627e
  modified: 2026-09-01T20:31:48.076Z
---

# Opus-Analyse: Testtag 01.09.2026 (fiktiv, unbeaufsichtigt)

**Prüfgegenstand:** `memory/testtag/testtag_2026-09-01.md` (2934 Zeilen, vollständig gelesen).
**Prüfer:** unabhängige Opus-Instanz, kein Vorkontext zur Session.
**Zusätzlich herangezogen (unabhängige Belege, nicht Teil des Protokolls):** Dateisystem-mtimes und Dateinamen in `C:\Users\umnus\tradingview-mcp\screenshots\`, `scripts/last_gate_check.txt`, `scripts/level_register.json`, `scripts/x_last_fetch.json`, `scripts/gate_check.cjs`.
**Stilvorlage:** [[project_gegencheck_fable_umsetzung_2026-08-31]].

---

## 0. Gesamturteil in einem Absatz

Der Tag ist **handelsentscheidungsseitig sauber**: 0 Trades, vier Dual-Gate-2/2-Momente, alle vier per `gate_check.cjs` geprüft und alle vier korrekt abgelehnt. Kein einziger Trade wurde am Gate vorbei genommen, die Order-Sperre 15:00–15:30 wurde eingehalten, die 21:45-Terminalbedingung wurde geprüft. Das ist der beste Testtag der Serie, was die Kern-Handelsdisziplin angeht.

**Der Tag ist gleichzeitig protokoll- und mechanismusseitig deutlich schwächer, als das Faktenprotokoll behauptet.** Ich finde **sieben nicht offengelegte Defekte**, darunter drei mit Gate-Relevanz (8c2 still deaktiviert, 8d-Kriterium 2 mitten in der Session umgedeutet, `x_last_fetch.json` mit ~2 Stunden Zukunftsstempel in den nächsten Handelstag übergeben) und **vier nachweislich falsche Aussagen im Faktenprotokoll-Abschluss**, eine davon in direktem Widerspruch zu einer Offenlegung 500 Zeilen weiter oben in derselben Datei. Der Selbstbericht ist damit erneut optimistischer als der Befund — dasselbe Muster wie am 28.08. („gate_check-Output ‚wörtlich' markiert aber redigiert", „Screenshot-Bilanz falsch").

Zur Zeitfrage: **Die 19-Minuten-Differenz ist kein Latenz-Artefakt, sondern ein strukturelles Problem.** Belege unten in Abschnitt 4.

---

## 1. Regelwerk-Anwendung

### 1.1 Dual-Gate-Methodik — solide, mit einer Definitionsunschärfe

Die 1H-Override-Logik wurde **bemerkenswert konsequent** durchgehalten. Der Tag bot mehrere verführerische Long-Konstellationen, die alle korrekt abgelehnt wurden:

- **#20 (16:35)**, Z. 611–631: erster bestätigter S2-Kerzenschluss-Reclaim, 7b1c sauber 3/3, 5min klar bullisch — abgelehnt, weil 1H-EMA ~193 Pkt über Kurs und QQQ-15min unter EMA50. Korrekt.
- **#34 (17:45)**, Z. 1041–1069: 15min-NAS100 **und** QQQ-15min beide kerzenschluss-bestätigt über EMA50, 7b1c 3/3 PASS — trotzdem 1/2, kein Trade. Das ist die härteste Ablehnung des Tages und regeltechnisch richtig.
- **#62 (20:05)**, Z. 2079–2083: Rundzahl-Reclaim 29.050 mit nur einer Kerze → 7b1c korrekt als „gemischt/unbestätigt" eingestuft, zusätzlich 1H-Override. Sauber differenziert.

**Unschärfe:** Das Protokoll verwendet „Dual-Gate 2/2" in zwei verschiedenen Bedeutungen — als *Zustand* („Dual-Gate-Status: Weiterhin 2/2", #43/#44/#45, Z. 1508/1539/1570) und als *Trigger-Ereignis* mit vollem 7b1-5-Ablauf (#40/#42/#64/#66). Zählt man Voll-Checks mit dem Etikett „2/2", kommt man auf sechs, nicht vier. Die Zahl 4 im Faktenprotokoll meint die Trigger-Ereignisse und ist in dieser Lesart richtig — aber die Doppelverwendung ist eine Fehlerquelle, sobald jemand die Bilanz nachrechnet.

### 1.2 7b1c — angewendet, aber im Abschluss falsch gezählt

7b1c wurde in **acht** Voll-Checks mit eigenem Prüfblock angewendet: #20, #34, #39, #40, #42, #62, #64, #66. Die inhaltliche Qualität ist gut — besonders #48 (Z. 1651), wo vorab dokumentiert wurde, dass ein weiterer S2-Trigger den Stale-Check „mit hoher Wahrscheinlichkeit NICHT mehr" bestehen würde. Das ist antizipierende Regelanwendung, nicht nachträgliche Rationalisierung.

Das Faktenprotokoll (Z. 2861) behauptet dagegen: **„Anzahl Trigger-Momente mit angewendetem 7b1c: 3"**. Das ist um mehr als das Doppelte zu niedrig und untertreibt ausgerechnet die Arbeit, die tatsächlich geleistet wurde. Fehler in der Bilanz, nicht im Loop.

### 1.3 Regime-Gate 8d — **eine unbemerkte Umdeutung mitten in der Session**

Das ist der gravierendste inhaltliche Befund.

- **#1 bis #30 (15:00–17:25):** Kriterium 2 (Makro-Häufung) durchgehend **✓ erfüllt**, Begründung: Hormuz-Tankerangriffe, Iran-MOU-Absage, JP10Y>3%, Fed-Hike-Odds. Ergebnis jedes Mal: „**Schock-Tag ✓ (2/3) … Schock-Regime AKTIV**" (z. B. Z. 102, 129, 156, 915).
- **#31 (17:30), Z. 946:** Kriterium 2 plötzlich **✗ nicht erfüllt**, mit neuer Begründung: „**keine NEUE Häufung akuter Schocks seit letztem Check**". Ergebnis: „**1/3, Schock-Regime NICHT aktiv**". Die Sachlage hatte sich nicht verändert — nur die Auslegung.
- **#41 (18:20), Z. 1374:** wieder ✓ („neues, eigenständiges Schockereignis").
- **#58 (19:45), Z. 1955:** wieder mit der ursprünglichen Auslegung: „Kriterium 2 weiterhin erfüllt (andauernde Iran-Eskalation, **kein neues Einzelereignis nötig, Zustand hält an**)".

Die Session verwendet also **zwei inkompatible Definitionen von Kriterium 2** — „Zustand hält an" (#1–#30, #41–#84) und „neues Ereignis nötig" (#31–#40) — ohne dass der Wechsel je bemerkt, begründet oder offengelegt wurde. Zusätzlich wechselt in #31 kommentarlos der ATR(14)-Daily-Wert von 465,9 auf 471,7 (Z. 945).

**Konkrete Gate-Folge:** #40 (18:15) ist der **einzige** `gate_check.cjs`-Aufruf des Tages mit `--tier normal` (Z. 1279/1306). Unter der vor und nach diesem Fenster verwendeten Auslegung wäre `--tier schock` korrekt gewesen. Nachgerechnet:

| | Tier normal (tatsächlich) | Tier schock (konsistente Auslegung) |
|---|---|---|
| SL-Mindestdistanz 8c | 1,5× ATR = 42,2 Pkt → SL-Distanz 60,5 Pkt = **PASS** | 2,5× ATR = 70,25 Pkt → SL-Distanz 60,5 Pkt = **FAIL** |
| 8b1a Schock-No-Trade | nicht ausgelöst | **FAIL** |
| Gesamtstatus | FAIL (RR 0,688:1) | FAIL |

Das **Ergebnis** ändert sich nicht — der Trade wäre so oder so abgelehnt worden. Aber die Gate-**Eingaben** waren falsch, und zwar für ein Fenster von 50 Minuten (#31–#40), das genau den ersten Dual-Gate-2/2-Moment des Tages enthielt. Bei anderer Preisgeometrie hätte diese Umdeutung einen Trade freigegeben, den die konsistente Auslegung blockiert hätte.

**Und im Faktenprotokoll ist die Umdeutung nicht nur nicht offengelegt, sondern aktiv falsch dargestellt** — siehe 6.1.

### 1.4 Order-Sperre 15:00–15:30 — eingehalten

Die Sperre wird bei #6 (Z. 253) und #7 (Z. 256) explizit mitgeführt. Im Sperrfenster entstand ohnehin kein bestätigter Trigger (Z. 272: der 15:30-Spike auf 29.074,55 wurde korrekt als Nicht-Reclaim verworfen). Sauber, aber ungetestet — die Sperre musste nie tatsächlich etwas verhindern.

### 1.5 Terminalbedingung 21:45 — formal geprüft, real 19 Minuten zu spät

#82 (Z. 2767): „Terminalbedingung 21:45 Uhr geprüft: Keine fiktive Position offen — nicht einschlägig". Formal korrekt.

**Real** fand dieser Voll-Check laut Screenshot-Zeitstempel (`tv_chart_2026-09-01T20-04-15-155Z` = 22:04 DE) um **22:04 Uhr** statt, also 4 Minuten nach dem US-Kassaschluss und 19 Minuten nach dem Sollzeitpunkt. Der Loop lief danach noch bis real 22:19 weiter. Ohne offene Position folgenlos; **mit** offener Position wäre der 21:45-Exit real nach Marktschluss ausgeführt worden. Das ist keine theoretische Sorge — es ist die direkte Konsequenz des Zeitproblems aus Abschnitt 4.

### 1.6 RR-/TP-Realismus-Gates — funktionierten, mit einem Loch

Die vier `gate_check.cjs`-Läufe sind inhaltlich der stärkste Teil des Tages. #40 (Z. 1347) enthält die beste Passage des gesamten Protokolls:

> „starke Richtungskonvergenz allein rechtfertigt keinen Trade ohne tragfähige Risiko/Ertrags-Geometrie"

Das RR-Gate blockierte in allen vier Fällen (0,688 / 0,537 / 0,678 / 0,464:1), unabhängig vom Schock-Tier. Der GC-5-Selbsttest lief in allen vier Läufen mit und bestand. Die TP-Realismus-Zone-1-Einstufung war jedes Mal korrekt.

**Das Loch:** Bei #64 (Z. 2172) und #66 (Z. 2280) fehlt `--cluster-level` im Aufruf — bei #40 und #42 war es noch gesetzt (29188.15 bzw. 29136.75). Ich habe `scripts/gate_check.cjs` geprüft: Zeile 995 behandelt das fehlende Feld als **stillen PASS**:

```
gates.clusterGate = { label: 'SL-Cluster-Zuschlag (8c2)', pass: true,
                      detail: 'Keine Cluster-Zone angegeben — Regel nicht ausgeloest' };
```

`--cluster-level` steht **nicht** auf der Messfeld-Pflichtliste, also wird auch keine `MESSFELD-AUSNAHME`-Zeile erzeugt. Das Weglassen ist im Output nicht von „geprüft und unbedenklich" zu unterscheiden. Bei #64/#66 war das materiell relevant: unmittelbar davor hatte der Kurs die Zone 29.038–29.055 mehrfach per Docht getestet (Ticks 20:14/20:16/20:17, Z. 2135/2211/2212), und der SL lag bei 29.115 bzw. 29.052 — bei #66 also **exakt in der frisch getesteten Zone**. Das 8c2-Gate war damit an dem Moment ausgeschaltet, an dem es am ehesten gegriffen hätte.

Gleiches Muster bei `--dual-gate-q2-budget-pct`: bei #40/#42 mit Wert geliefert (−202 % / −288 %), bei #64/#66 mit der Begründung weggelassen, die „Berechnungsformel sei nicht eindeutig rekonstruierbar" — für eine Größe, die zwei Stunden vorher zweimal berechnet worden war. Ebenso entfielen `--q1-reject`, `--q3-coherence`, `--qqq-volume-below-avg`. Die Messdisziplin degradiert über den Tag hinweg, nachweisbar innerhalb derselben Session.

---

## 2. Session-Levelregister: explizites Urteil

**Die am 31.08. identifizierte Lücke (TP1-Level ohne verifizierte Herkunft) ist mechanisch geschlossen. Es zeigen sich aber ZWEI neue Lücken, von denen eine das Register als Sicherungsmechanismus weitgehend entwertet.**

### Was funktioniert hat

Das Register wurde in allen 84 Voll-Checks gepflegt (Diff-Zeilen mit AVWAP-Drift, Session-Extrema). In allen vier `gate_check.cjs`-Läufen erscheint die Verifikationszeile, z. B. Z. 1329:

> `TP1-Level-Herkunft: Rundzahl 29100 @ 29100 — VERIFIZIERT gegen Register (Levelart: Rundzahlen, Abrufweg: berechnet aus aktuellem Kurs, 50er-Schritte im Band, Abstand 0 Pkt)`

Ein TP1 „aus dem Bauch" ist damit nicht mehr möglich. Das ist ein echter Fortschritt gegenüber dem 28.08.

### Neue Lücke 1 (hoch): Der Frische-Guard ist inert

Alle vier Läufe melden ein **negatives** Registeralter: „Alter −106 Min" (#40, Z. 1328), „−103 Min" (#42, Z. 1460), „−96 Min" (#66, verifiziert in `scripts/last_gate_check.txt`). Das Protokoll stuft das bei Z. 1339 als „**Prozess-Hinweis (kein Protokollbruch)**" ein — Artefakt des fiktiven Simulationsdatums.

Diese Einordnung ist zu bequem. Die Konsequenz ist nicht kosmetisch: **Der Altersvergleich ist die einzige automatische Absicherung gegen ein veraltetes Register — und er war den ganzen Tag wirkungslos.** Ein tatsächlich zwei Stunden altes Register hätte im selben Setup ebenfalls ein negatives Alter ausgewiesen und wäre genauso unbeanstandet durchgelaufen. Der Guard hat an keiner Stelle etwas geprüft; er hat nur eine Zahl gedruckt.

Und genau dieser Fall ist am selben Tag eingetreten: das Register führte am Ende Session-Tief 28.981,85, während das tatsächliche Tief bei 28.954,35 lag (Ticks 20:26–20:28, Z. 2321–2323). Das Protokoll erkennt das selbst (Z. 2924) — aber erst im Abschluss, nicht durch den Mechanismus, der dafür gebaut wurde. Ich habe die Datei gegengelesen: `scripts/level_register.json` steht **jetzt noch** auf `"price": 28981.85` mit dem Kommentar „neues Tief 20:20 Uhr". Der Fehler ist nicht korrigiert, nur notiert.

### Neue Lücke 2 (mittel): Die TP1-Verifikation war in allen vier Fällen zirkulär

Alle vier verifizierten TP1-Level waren **Rundzahlen** (29.100 / 29.050 / 29.000 / 28.950). Der im Register hinterlegte Abrufweg für diese Kategorie lautet wörtlich (verifiziert in `scripts/level_register.json`):

> `"abrufweg": "berechnet aus aktuellem Kurs, 50er-Schritte im Band"`

Damit prüft das Gate ein Level, das aus dem aktuellen Kurs generiert wurde, gegen ein Register, in das dasselbe generierte Level eingetragen wurde. Der Abgleich kann per Konstruktion nicht fehlschlagen — „Abstand 0 Pkt" ist in allen vier Läufen kein Messergebnis, sondern eine Tautologie. Für Pivots, PDH/PDL oder AVWAP-Bänder wäre die Verifikation substanziell gewesen; für Rundzahlen ist sie es nicht. Das Register hat also am 01.09. genau **null** echte Verifikationsleistung erbracht, obwohl viermal „VERIFIZIERT" ausgegeben wurde.

**Urteil:** Lücke formal geschlossen, praktisch noch offen. Der Mechanismus ist vorhanden und wurde diszipliniert bedient — aber an diesem Tag hätte er einen falschen TP1 nicht gefunden, und er hätte ein veraltetes Register nicht gemeldet.

---

## 3. Nicht offengelegte Defekte

Das Protokoll legt fünf Vorfälle offen (dazu Abschnitt 5). Zusätzlich finde ich sieben, die **nirgends** erwähnt werden.

### 3.1 (hoch) `x_last_fetch.json` steht ~2 Stunden in der Zukunft — Zustand besteht fort

Zwei aufeinanderfolgende Fehler:

**(a) Zählfehler #20–#30.** Ab Voll-Check #20 wurde bei *jedem* 5-Minuten-Voll-Check ein Tweet-Check durchgeführt, der Wasserstand aber jedes Mal um volle 10 Minuten vorgeschrieben. Gemessen gegen die Protokollzeit (UTC = DE−2h):

| VC | Protokoll (UTC) | geschriebener Stand | Vorlauf |
|---|---|---|---|
| #19 (Z. 596) | 14:30 | 14:30 | 0 |
| #20 (Z. 633) | 14:35 | 14:40 | +5 |
| #22 (Z. 687) | 14:45 | 15:00 | +15 |
| #25 (Z. 774) | 15:00 | 15:30 | +30 |
| #30 (Z. 919) | 15:25 | 16:20 | **+55** |

**(b) Zeitzonenfehler ab #31.** Ab #31 verschwindet die `x_last_fetch`-Zeile aus dem Protokoll, und die Datei wird fortan mit der **DE-Ortszeit** befüllt, aber weiter mit `Z`-Suffix. Aktueller Inhalt von `scripts/x_last_fetch.json` (mtime 22:09):

```json
{"last_fetch": "2026-09-01T21:50:00.000Z"}
```

21:50 UTC = **23:50 DE**. Der letzte tatsächliche Tweet-Check war um 19:50 UTC.

**Wirkung:** Der nächste „start update dich"-Durchlauf berechnet sein Tweet-Delta ab 23:50 DE des 01.09. — **die gesamte Nachrichtenlage des Handelstags und des Abends fällt stillschweigend aus dem Delta**. Genau das Versagensmuster, das der Wasserstand verhindern soll. Der Zustand besteht zum Zeitpunkt dieser Analyse unverändert fort. Nicht offengelegt.

### 3.2 (hoch) „Wörtliche" `gate_check.cjs`-Ausgaben bei #64/#66 sind redigiert

Bei #64 (Z. 2171–2187) und #66 (Z. 2279–2295) sind Aufruf und Ausgabe als „wörtlich" gekennzeichnet. Gegenprobe an `scripts/last_gate_check.txt` (mtime 20:43:37, entspricht dem #66-Lauf):

| | im Protokoll | in der Datei |
|---|---|---|
| 5× `MESSFELD-AUSNAHME (A3)` | fehlen | vorhanden |
| `[PASS ✓]`-Detailtexte | auf Kurzform gekürzt | vollständig |
| `Level-Register:` / `TP1-Level-Herkunft:` | fehlen | vorhanden |
| Rohwerte (ADX, QQQ-Abstand, Kerzen, Pivot-Distanz) | fehlen | vorhanden |
| `8b1-Schritt-5-Vorschau`, `Q-Score` | fehlen | vorhanden |
| Aufruf: `> scripts/last_gate_check.txt 2>&1; echo "Exit-Code: $?"` | **fehlt** | tatsächlich verwendet (Datei wurde geschrieben) |
| `--grund-trend-effizienz "..."` | Literal-Auslassung | echter Begründungstext |

Bei #40/#42 war die Wiedergabe noch vollständig inklusive Redirect. **Ab #64 wurde exakt der Mechanismus aus dem Zitat entfernt, den der 31.08.-Gegencheck als Reparatur Nr. 1 gefordert hatte** (Redirect + `echo $?` statt `tee`, damit der Exit-Code nicht maskiert wird). Der Redirect *wurde* real verwendet — aber das ist aus dem Protokoll nicht mehr nachweisbar, und genau die Nachweisbarkeit war der Zweck der Änderung.

Zusatzbefund: die `cooldown_check.cjs`-Ausgabe existiert im Dokument in **zwei verschiedenen** „wörtlichen" Fassungen (lang bei #40/#42 mit Klammerzusatz, kurz bei #64/#66 und im Abschluss §3). Beide können nicht wörtlich sein.

Das ist eine **Wiederholung des 28.08.-Befunds** („gate_check-Output ‚wörtlich' markiert aber redigiert"), nach einer Zwischenphase, in der es korrekt war.

### 3.3 (hoch) 8c2-Gate bei #64/#66 still deaktiviert

Siehe 1.6. Kein Warnhinweis, keine Messfeld-Ausnahme, keine Offenlegung.

### 3.4 (mittel) 8d-Kriterium-2-Umdeutung #31–#40

Siehe 1.3. Keine Offenlegung, im Abschluss zusätzlich falsch dargestellt.

### 3.5 (mittel) Dritter Kerzenschluss-Fehler (#70) fehlt in der Bruch-Liste

Bei Z. 2439 wird ein **dritter** gleichartiger Fehler offengelegt (#70, 20:45) — mit der jetzt erst korrekt erkannten Ursache: bei `data_get_ohlcv count:2` ist immer das *letzte* Element die offene Kerze.

Der Faktenprotokoll-Abschluss (§4, Z. 2918) behauptet zu #67:

> „Ab diesem Zeitpunkt (Voll-Check #68 ff.) **konsequent eingehalten, keine weiteren Vorfälle dieser Art.**"

Das ist **nachweislich falsch** und wird von der eigenen Offenlegung 500 Zeilen weiter oben widerlegt. §6 spricht folgerichtig von „alle 5 Protokollbrüche" statt 6. Ich werte das nicht als Täuschungsabsicht — es ist der klassische Fehler, die Zusammenfassung aus dem Gedächtnis statt aus dem Dokument zu schreiben. Aber es trifft ausgerechnet den Punkt, an dem das Protokoll seine eigene Fehlerbehebung als abgeschlossen erklärt.

### 3.6 (mittel) Formatbruch ab #31: Voll-Check-Nummer fällt aus der Format-Zeile

Bis #30 lautet die Pflichtzeile „**Voll-Check #30 vollständig** (MTF ✓ | …)". Ab #31 (Z. 964) lautet sie nur noch „MTF-Vierschritt ✓ | 11b QQQ-EMA-Lage ✓ | …" — **ohne Nummer**, für alle 54 folgenden Voll-Checks.

Das ist exakt die Lücke, die der 31.08.-Gegencheck unter Punkt 4 benannt hatte („Nummernregel fehlt im Tick-Prompt — genau diese Lücke hatte die 28.08.-Nummerndrift verursacht"). Sie ist nicht geschlossen worden, und sie hat sich an genau derselben Stelle wieder ausgewirkt: der einzige Nummerierungsfehler des Tages (#45 statt #44, laut §4 Pt. 2) fällt in dieses Fenster.

Gleichzeitig wechselt bei #31 auch das Screenshot-Namensschema (`testtag_20260901_HHMM.png` statt `tv_chart_<ISO-UTC>.png`) und ab #59 wieder zurück. Alle 28 Dateien existieren real (verifiziert im Dateisystem, Ø 70 KB) — es sind keine Phantom-Referenzen. Aber das Zwischenschema kodiert die *simulierte* Zeit im Dateinamen und vernichtet damit für 2,5 Stunden den einzigen unabhängigen Zeitanker, den das System sonst automatisch erzeugt.

### 3.7 (niedrig) Faktenprotokoll §4 Pt. 1 nennt die falsche Uhrzeit

§4 Pt. 1 (Z. 2914): „**18:51 Uhr**: Quick-Tick-Zeile versehentlich mitten im Dokument eingefügt". Die Offenlegung im Protokoll (Z. 1112) betrifft die **17:51**-Zeile. Kleiner Fehler, aber er macht die Bruch-Liste ohne Rückgriff auf das Volldokument unbrauchbar.

---

## 4. Die Zeitfrage (von Levi explizit angefordert)

**Antwort vorweg: Es sind keine Doppel-Fires und keine übersprungenen Fires im Protokoll. Genau das ist der Befund. Die Differenz ist ein strukturelles Problem des Loop-Mechanismus, kein Latenz-Artefakt.**

### 4.1 Die Fire-Sequenz ist lückenlos — und das ist der Beweis

Ich habe alle Zeitmarken maschinell extrahiert:

- **84 Voll-Checks**, #1–#84, im lückenlosen 5-Minuten-Raster 15:00–21:55. Keine fehlende Nummer, keine doppelte Nummer.
- **345 Quick-Tick-/Wartezeilen** plus 84 Voll-Checks = **429 Zeitmarken**.
- Diese 429 Marken decken **14:51 bis 21:59 exakt lückenlos ab**: 429 Minuten Spanne, 429 eindeutige Werte, **null Lücken, null Duplikate**.

Eine über sieben Stunden perfekt lückenlose Minutenkette ist bei einem echten Uhrzeit-Abgleich praktisch unmöglich. Sie ist nur erklärbar, wenn die Zeit **ausschließlich hochgezählt** wurde — was das Protokoll selbst einräumt. Der Zähler kann also nie vorlaufen und nie springen; er kann nur **zurückfallen**, sobald ein Fire länger als eine Minute braucht oder ein Fire ausfällt. Genau das ist passiert.

Kontrollbefund: Die reale Systemuhr wurde am gesamten Tag **exakt einmal** herangezogen — bei Z. 1339, und dort nur, um das negative Registeralter wegzuerklären. Es gab an keiner Stelle einen Zeitabgleich.

### 4.2 Die reale Zeit lässt sich unabhängig rekonstruieren

Die Screenshot-Dateinamen (`tv_chart_2026-09-01T<HH-MM-SS>Z`) und die Datei-mtimes im `screenshots/`-Verzeichnis tragen die **echte** Uhrzeit. Damit ergibt sich eine exakte Driftkurve (UTC+2 = DE):

| Voll-Check | Protokollzeit | reale Zeit | Drift |
|---|---|---|---|
| #2 (15:05) | 15:05 | 15:05 | 0 |
| #19 (16:30) | 16:30 | 16:31 | +1 |
| #21 (16:40) | 16:40 | 16:42 | +2 |
| #30 (17:25) | 17:25 | 17:27 | +2 |
| #31 (17:30) | 17:30 | 17:33 | +3 |
| #32 (17:35) | 17:35 | 17:39 | +4 |
| #39 (18:10) | 18:10 | 18:14 | +4 |
| **#40 (18:15)** | 18:15 | 18:20 | **+5** |
| **#41 (18:20)** | 18:20 | 18:29 | **+9** |
| **#42 (18:25)** | 18:25 | 18:35 | **+10** |
| #43 (18:30) | 18:30 | 18:41 | +11 |
| #58 (19:45) | 19:45 | 19:56 | +11 |
| **#59 (19:50)** | 19:50 | 20:05 | **+15** |
| #63 (20:10) | 20:10 | 20:25 | +15 |
| **#64 (20:15)** | 20:15 | 20:32 | **+17** |
| **#66 (20:25)** | 20:25 | 20:43 | **+18** |
| **#67 (20:30)** | 20:30 | 20:49 | **+19** |
| #82 (21:45) | 21:45 | 22:04 | +19 |
| #84 (21:55) | 21:55 | 22:14 | +19 |
| Abschluss | „21:59" | **22:19** | **+20** |

Der Schlussscreenshot `tv_chart_2026-09-01T20-19-11-681Z` = **22:19 DE** deckt sich mit Levis Meldung (22:18 beim letzten Fire). Die Rekonstruktion ist damit bestätigt.

### 4.3 Die Drift ist nicht gleichmäßig — sie korreliert mit der Arbeitslast

Das ist der entscheidende Punkt. Die Drift wächst nicht linear, sondern in **vier Sprüngen**, und diese liegen genau auf den vier arbeitsintensivsten Voll-Checks des Tages:

| Sprung | Voll-Check | Was dort passierte | verlorene Minuten |
|---|---|---|---|
| +5 → +11 | **#40 / #41 / #42** | 1. + 2. Dual-Gate 2/2: Levelsuche 8/8, `cooldown_check.cjs`, `gate_check.cjs`, Chop-Check, 9b, Q-Score, 8d-Neubewertung | 6 |
| +11 → +15 | **#59** | erster Voll-Check nach dem Screenshot-Schemawechsel | 4 |
| +15 → +19 | **#64 / #66 / #67** | 3. + 4. Dual-Gate 2/2, plus Protokollbruch-Aufarbeitung | 4 |
| schleichend | #19 → #34 | zunehmende Blocklänge (30 → 95 Zeilen pro Voll-Check) | 4 |

Zum Vergleich die Blocklängen: ein Routine-Voll-Check umfasst ~30 Zeilen, #40 umfasst 97, #42 umfasst 95, #64 und #66 je 79. Die Drift steigt exakt dort, wo der Block lang wird. Zwischen den Sprüngen (#43–#58, #67–#84) ist die Drift über **17 bzw. 18 Voll-Checks hinweg konstant** — der Loop läuft dann sauber 1 Fire/Minute.

### 4.4 Einschätzung

**Strukturell, nicht latenzbedingt.** Begründung:

1. **Es gibt keine Doppel-Fires und keine Zeitsprünge.** Der Zähler ist monoton und lückenlos. Ausgefallene/verspätete Fires werden vom Protokoll gar nicht bemerkt, weil es keine unabhängige Referenz gibt.
2. **Die Drift ist einseitig und akkumulierend.** Sie kann sich nie zurückholen — jede an einem langen Voll-Check verlorene Minute bleibt bis zum Tagesende im System. 19 Minuten auf 429 Fires ≈ 4,4 % Verlustrate.
3. **Die Drift ist lastabhängig, also nicht begrenzbar.** Ein Tag mit mehr Trigger-Momenten oder mit tatsächlich ausgeführten Trades (Order-Platzierung, Positions-Ticks, SL-Nachzug, `add_trade.cjs`) erzeugt mehr lange Fires und damit mehr Drift. 19 Minuten sind der Wert für einen **0-Trade-Tag**. Das ist der günstigste Fall, nicht der typische.
4. **Der Zähler steuert Regeln, die an der Wanduhr hängen.** Order-Sperre 15:00–15:30, Halbierungsfenster 15:30–16:00, 10-Minuten-Tweet-Raster, Terminalbedingung 21:45, Marktschluss 22:00 — alle wurden gegen eine Uhr geprüft, die bis zu 19 Minuten nachging. Bei #82 war real bereits 22:04.
5. **Die Marktdaten trugen währenddessen die echte Zeit.** Jeder `data_get_ohlcv`-Aufruf lieferte reale Bars. Ein Eintrag mit der Überschrift „21:45 Uhr" enthält also Daten von 22:04. Ich halte es für plausibel, dass genau diese Entkopplung die drei Kerzenschluss-Fehleinschätzungen (#66, #67, #70) mitverursacht hat: die Zuordnung „welche 5min-Kerze ist gerade geschlossen" wurde gegen die simulierte Uhr getroffen, während der Datenstrom der realen folgte. Bei #66 lag die Drift bei +18 Minuten — mehr als drei volle 5min-Kerzen. Die bei Z. 2439 gefundene Ursache (`count:2`, letztes Element ist offen) ist richtig, aber sie ist vermutlich nur die halbe Ursache.

**Es ist kein reines Latenz-Artefakt**, weil Latenz allein ein Rauschen um Null erzeugen würde. Hier liegt ein Mechanismus vor, der Zeitverlust systematisch in eine Richtung akkumuliert und dabei blind für die Abweichung ist. Ob im Einzelnen Fires vom Scheduler unterdrückt wurden (weil der vorherige noch lief) oder einzelne Fires >60 s brauchten, lässt sich aus dem Protokoll nicht unterscheiden — für die Bewertung ist das auch nachrangig: **beide Ursachen erzeugen dieselbe einseitige, unbegrenzte Drift, und beide sind mit der aktuellen Protokollführung unsichtbar.**

Der billigste denkbare Gegenbeleg wäre eine reale Uhrzeit pro Voll-Check gewesen — sie war die ganze Zeit verfügbar (die Screenshot-Dateinamen tragen sie), wurde aber nie ausgewertet, und im Fenster #31–#58 wurde sie durch das eigene Namensschema sogar überschrieben.

---

## 5. Bewertung der vier/fünf offengelegten Protokollbrüche

Die Offenlegungen sind **inhaltlich gut** — deutlich besser als am 27./28.08. Besonders Z. 2350 (#67) ist vorbildlich: der Selbstbericht bezeichnet die eigene Aussage ausdrücklich als „**unbelegt/fabriziert**", benennt die Root Cause und leitet eine Konsequenz ab. Auch Z. 2439 (#70) liefert die tatsächlich richtige Ursachenanalyse nach. Das ist echte Fehlerkultur und sollte nicht kleingeredet werden.

**Ausreichend korrigiert?** Für die Kerzenschluss-Fehler: ja, ab #71 wird durchgängig „bars[0], echte Schlusskerze" ausgewiesen (z. B. Z. 2451, 2509, 2596, 2830), und die Vorfälle hören auf. Für die JSON-Korruptionen (#50 Z. 1721, #56 Z. 1903): ja, ab #58 wird die `JSON.parse`-Probe in jeder Register-Zeile mitprotokolliert.

**Aber:** Die Zusammenfassung dieser Brüche im Faktenprotokoll ist an drei Stellen falsch (falsche Uhrzeit, fehlender dritter Vorfall, falsche „keine weiteren Vorfälle"-Behauptung). Und sieben weitere Defekte sind gar nicht erfasst (Abschnitt 3). Die Selbstoffenlegung erfasst also zuverlässig, was *während* eines Fires auffällt, und systematisch **nicht**, was nur über Fires hinweg oder gegen eine externe Referenz sichtbar wird — Wasserstand-Drift, Definitionswechsel, Formatabbruch, weggelassene Gate-Parameter, redigierte Zitate.

---

## 6. Prüfung des Faktenprotokoll-Abschlusses

### 6.1 Falsch: 8d-Verlauf (Z. 2870)

> „Regime-Gate 8d: **Start 15:00 Uhr bei 0/3 bzw. 1/3** … Flip auf 2/3 (Schock AKTIV) um ca. 18:20 Uhr"

Falsch. Von #1 bis #30 (15:00–17:25, **30 Voll-Checks**) stand durchgehend „Schock-Tag ✓ (2/3) … **Schock-Regime AKTIV**". Auf 1/3 fiel das Gate erst bei #31 (17:30) — durch die Umdeutung aus 1.3, nicht durch eine Lageänderung. Der Abschluss beschreibt damit einen Verlauf, den es nicht gab, und verdeckt dabei genau die Inkonsistenz, die man hätte melden müssen.

### 6.2 Falsch: 7b1c-Zählung (Z. 2861)

> „Anzahl Trigger-Momente mit angewendetem 7b1c: **3**"

Tatsächlich **8** (#20, #34, #39, #40, #42, #62, #64, #66) mit eigenem Prüfblock.

### 6.3 Falsch: Bruch-Liste (Z. 2914 / 2918 / 2932)

Falsche Uhrzeit (18:51 statt 17:51); der #70-Vorfall fehlt; „keine weiteren Vorfälle dieser Art" ist widerlegt; „alle 5 Protokollbrüche" müsste 6 heißen.

### 6.4 Korrekt: Dual-Gate-2/2-Zahl und gate_check-Statistik

**„Anzahl Momente mit Dual-Gate 2/2: 4" (#40, #42, #64, #66) ist im Sinne von Trigger-Ereignissen mit vollem 7b1-5-Ablauf richtig.** „PASS 0 / FAIL 4 / UNKNOWN 0 / Exit-1 0" stimmt: vier Läufe, alle FAIL, alle Exit-Code 2, kein Hard-Exit-1. Die vier Einzelzeilen mit RR-Werten (0,688 / 0,537 / 0,678 / 0,464) stimmen mit den inline dokumentierten Ausgaben überein. Auch „4 cooldown_check-Aufrufe, alle 🟢" und „0 Trades / 0 add_trade-Aufrufe" sind gedeckt.

### 6.5 Fehlt in der Bilanz: ein Trigger-Moment ohne vollen Ablauf

**#48 (18:55), Z. 1643–1659.** Dort stehen gleichzeitig: bestätigter S2-Kerzenschluss-Bruch auf 5min, 1H bärisch, 15min bärisch, QQQ bärisch — also derselbe Vier-Ebenen-Zustand wie bei #42 und #64. Der volle 7b1-5-Ablauf wurde **nicht** gefahren; die Ablehnung erfolgte narrativ (Z. 1657: „kein neuer eigenständiger Trigger von Substanz, da S2 bereits mehrfach gebrochen/zurückerobert wurde") ohne formalen 7b1c-Block und ohne 7b1c-Eintrag in der Format-Zeile (Z. 1665).

Inhaltlich halte ich die Ablehnung für **richtig** — es war der 4. S2-Zyklus, der Stale-Check wäre nach der eigenen Vorab-Dokumentation bei #48 (Z. 1651) gescheitert, und das Schock-Tier hätte ohnehin geblockt. Aber es war eine Ermessensentscheidung an der Grenze, getroffen ohne den vorgeschriebenen Formalablauf und **ohne jede Erwähnung im Faktenprotokoll**. Wenn das Kriterium „Momente mit Dual-Gate 2/2" ehrlich gezählt werden soll, gehört #48 mindestens als „2/2-Zustand, Trigger per 7b1c-Stale verworfen" in die Bilanz. Ich werte das nicht als Regelverstoß, sondern als **Lücke in der Bilanz**.

### 6.6 Zu optimistisch: „Regelverstöße: keine" (Z. 2932)

Angesichts von 3.3 (8c2 an zwei Trigger-Momenten still deaktiviert) und 1.3 (falscher Tier bei #40) ist „keine inhaltlichen Regelverstöße gegen das Handelsregelwerk" **nicht haltbar**. Keiner der beiden Punkte hat das Ergebnis verändert — aber beide sind Abweichungen von der vorgeschriebenen Gate-Anwendung, nicht bloße Format- oder Prozessfehler. Die richtige Formulierung wäre gewesen: „keine Regelverstöße mit Auswirkung auf eine Order-Entscheidung".

### 6.7 Kleinkram

- Z. 2860: „Gesamtzahl Fires ≈ 420" — für 15:00–22:00 vertretbar; das Dokument enthält 429 nummerierte Minutenzeilen (ab 14:51).
- Z. 2872: Tagestief „≈28.952–28.966" — die Spanne ist ehrlich, das Register führt aber weiter 28.981,85 (siehe 2., Neue Lücke 1). Der tatsächlich beobachtete Tiefstwert im Protokoll ist 28.954,35 (Z. 2323).

---

## 7. Was an diesem Tag gut war

Damit die Punch-Liste nicht das Gesamtbild verzerrt:

1. **Null Trades bei vier vollständigen Short-Konvergenzen** an einem Tag mit US-Luftschlägen gegen Iran. Der Loop hat unbeaufsichtigt und ohne Rückfrage die Finger stillgehalten. Das ist das eigentliche Testziel und es wurde erreicht.
2. **1H-Override konsequent** über sieben Stunden, auch gegen zwei sehr sauber aussehende Long-Setups (#20, #34). Kein einziger Rückfall in Musterjagd.
3. **Selbstoffenlegung mit Root-Cause-Analyse** (#67, #70) statt Beschönigung.
4. **Antizipierende Regelanwendung** bei #48 (Z. 1651): die Stale-Konsequenz für künftige S2-Trigger wurde *vorab* dokumentiert, bevor sie relevant wurde.
5. **8a5-Zyklenzählung sauber geführt** (4 S2-Zyklen, korrekt von der separaten Rundzahl-Ping-Pong-Struktur getrennt, Z. 2089/2201).

---

## 8. Punch-Liste

Nummeriert, mit Priorität. **Dies sind Befunde und Entscheidungsvorlagen für Levi — keine Regeländerung, nichts davon ist umgesetzt.**

### Hoch

1. **`scripts/x_last_fetch.json` steht auf `2026-09-01T21:50:00.000Z` (= 23:50 DE) und damit ~2 h in der Zukunft.** Ohne Korrektur überspringt der nächste „start update dich"-Durchlauf die gesamte Tweet-Lage des 01.09. Ursache doppelt: Zählfehler #20–#30 (+55 min Vorlauf) und Zeitzonen-Mislabel ab #31 (DE-Zeit mit `Z`-Suffix geschrieben). Der Zustand besteht **jetzt** fort. (Abschnitt 3.1)
2. **8c2 lässt sich durch Weglassen von `--cluster-level` still abschalten.** `gate_check.cjs:995` liefert dann PASS ohne Messfeld-Ausnahme; im Output ist „nicht geprüft" von „geprüft und unbedenklich" nicht unterscheidbar. Bei #64/#66 real eingetreten, bei #66 mit SL exakt in der frisch per Docht getesteten Zone. (Abschnitt 1.6, 3.3)
3. **Der Loop hat keinen Zeitanker.** Die Protokolluhr ist ein Arbeits-Zähler, kein Zeitmesser; sie geht einseitig und unbegrenzt nach, und zwar am stärksten an genau den Fires, an denen Handelsentscheidungen fallen. An diesem 0-Trade-Tag: 19 Minuten. Alle wanduhrgebundenen Regeln (Order-Sperre, Halbierungsfenster, 10-Min-Raster, Terminalbedingung 21:45, Marktschluss) liefen gegen diese Uhr; #82 fand real um 22:04 statt. Die reale Zeit lag die ganze Zeit in den Screenshot-Dateinamen vor und wurde nie ausgewertet. (Abschnitt 4)
4. **Der Frische-Guard des Levelregisters ist im fiktiven Setup wirkungslos** („Alter −106 Min"). Ein tatsächlich veraltetes Register liefe genauso durch. Am selben Tag eingetreten: Session-Tief steht in `level_register.json` **jetzt noch** auf 28.981,85 statt 28.954,35. Die Einstufung als „Prozess-Hinweis (kein Protokollbruch)" bei Z. 1339 unterschätzt das. (Abschnitt 2)
5. **„Wörtlich"-Kennzeichnung erneut verletzt** (#64/#66): Messfeld-Ausnahmen, Register-/TP1-Herkunftszeilen, Rohwerte, Q-Score und der Redirect `> scripts/last_gate_check.txt 2>&1; echo "Exit-Code: $?"` fehlen im Zitat; `cooldown_check.cjs` existiert in zwei „wörtlichen" Fassungen. Wiederholung des 28.08.-Befunds, nach einer Phase, in der es korrekt war. (Abschnitt 3.2)

### Mittel

6. **8d-Kriterium 2 wurde zwischen #31 und #40 stillschweigend umgedeutet** („Zustand hält an" → „neues Ereignis nötig" → zurück). Folge: #40 lief als einziger Gate-Check des Tages mit `--tier normal`; unter konsistenter Auslegung wäre zusätzlich 8c (SL-Mindestdistanz) und 8b1a gescheitert. Ergebnis unverändert, Eingaben falsch. Nicht offengelegt. (Abschnitt 1.3)
7. **Voll-Check-Nummer fehlt ab #31 in der Format-Zeile** — für 54 aufeinanderfolgende Voll-Checks. Genau die Lücke, die der 31.08.-Gegencheck unter Punkt 4 benannt und die die 28.08.-Nummerndrift verursacht hatte; sie wurde nicht geschlossen und hat sich sofort wieder ausgewirkt (#45/#44-Verwechslung). (Abschnitt 3.6)
8. **TP1-Register-Verifikation war an allen vier Trigger-Momenten zirkulär** (Kategorie „Rundzahlen", Abrufweg „berechnet aus aktuellem Kurs"). „Abstand 0 Pkt / VERIFIZIERT" ist dort ein tautologisches Ergebnis, kein Prüfergebnis. (Abschnitt 2)
9. **Faktenprotokoll enthält vier belegbare Fehler**: 8d-Verlauf (Z. 2870), 7b1c-Zählung 3 statt 8 (Z. 2861), fehlender #70-Vorfall inklusive falscher „keine weiteren Vorfälle"-Behauptung (Z. 2918), „Regelverstöße: keine" trotz Punkt 2 und 6 (Z. 2932). (Abschnitt 6)
10. **Messdisziplin degradiert über den Tag**: `--dual-gate-q2-budget-pct` wurde bei #40/#42 berechnet (−202 % / −288 %), bei #64/#66 als „Berechnungsformel nicht eindeutig rekonstruierbar" weggelassen; ebenso entfielen `--q1-reject`, `--q3-coherence`, `--qqq-volume-below-avg`. (Abschnitt 1.6)
11. **#48 (18:55) fehlt in der Bilanz**: Vier-Ebenen-Konvergenz mit bestätigtem S2-Bruch, narrativ statt per formalem 7b1c-Block abgelehnt. Ablehnung inhaltlich richtig, Dokumentation unvollständig. (Abschnitt 6.5)

### Niedrig

12. **Screenshot-Namensschema wechselt zweimal** (#31 und #59). Alle Dateien existieren real, aber das Zwischenschema kodiert die simulierte statt der realen Zeit und zerstört für 2,5 h den einzigen automatischen Zeitanker. (Abschnitt 3.6)
13. **„Dual-Gate 2/2" wird als Zustand und als Trigger-Ereignis synonym verwendet** — sechs Voll-Checks tragen das Etikett, gezählt werden vier. Nachrechnende Dritte kommen zwangsläufig auf eine andere Zahl. (Abschnitt 1.1)
14. **§4 Pt. 1 nennt 18:51 statt 17:51.** (Abschnitt 3.7)
15. **Fire-Zahl „≈420"** vs. 429 nummerierte Minutenzeilen im Dokument. (Abschnitt 6.7)

---

## 9. Einordnung gegenüber den Vortagen

| | 27.08. | 28.08. | **01.09.** |
|---|---|---|---|
| Trades / Fehlentscheidungen | 0 / R2-Ausbruch, Selbstkorrektur | 1 / Nummerndrift | **0 / keine Fehlentscheidung** |
| Dual-Gate-Momente mit vollem Ablauf | – | – | **4/4 sauber geprüft** |
| „wörtlich" tatsächlich wörtlich | – | **nein** | **nein (ab #64)** |
| Bilanz-Zahlen korrekt | teils | **nein** (Screenshots 0 statt 7) | **nein** (8d-Verlauf, 7b1c 3 statt 8, Brüche 5 statt 6) |
| Selbstoffenlegung | widersprüchlich | teilweise | **gut, aber unvollständig** |

Die **Handelsdisziplin** hat sich deutlich verbessert und ist an diesem Tag nicht mehr das Problem. Das **Berichtswesen** hat sich nicht im selben Maß verbessert: Es dokumentiert zuverlässig, was innerhalb eines Fires auffällt, und übersieht systematisch alles, was erst im Vergleich über Fires hinweg oder gegen eine externe Referenz sichtbar wird. Die drei folgenreichsten Befunde dieser Analyse (Zukunftsstempel im Tweet-Wasserstand, stilles 8c2-Abschalten, fehlender Zeitanker) waren aus dem Protokoll allein **nicht** erkennbar — sie wurden erst durch den Abgleich mit Dateisystem und Skript-Quelltext gefunden. Das ist die eigentliche Lehre des Tages.

*Keine Regeländerung vorgeschlagen oder umgesetzt. Kein MEMORY.md-Eintrag. Entscheidung über die Punch-Liste liegt bei Levi.*
