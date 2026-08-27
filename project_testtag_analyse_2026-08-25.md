---
name: project_testtag_analyse_2026-08-25
description: "Unabhängiges Opus-Review des fiktiven Testtags 25.08.2026 (Levi bis 16:52 Uhr dabei, danach Sonnet solo bis 20:00 Uhr). Drei fiktive Trades (#T1 TP1+BE, #T2 gecancelt, #T3 offen bei Stopp). Kernbefund: #T1 und #T3 sind so, wie dokumentiert, arithmetisch nicht möglich — SL-Floor (8c) und TP-Zone-1 (8b1) schließen sich bei RR 1,34:1 gegenseitig aus. Nichts umgesetzt, alles Entscheidungsvorlage für Levi."
metadata:
  node_type: memory
  type: project
  originSessionId: opus-review-2026-08-25
  modified: 2026-08-25T18:13:30.774Z
---

# Testtag-Analyse 25.08.2026 — Opus-Review (unabhängig)

Quelle: [[testtag/testtag_2026-08-25]] (Rohprotokoll, 101 Zeilen). Gegengeprüft gegen [[feedback_live_trading]], [[feedback_chartanalyse]], [[feedback_vollcheck_format]], [[project_risikomanagement]], `scripts/gate_check.cjs`, `scripts/trades.db`, `scripts/x_last_fetch.json`. Vergleichsmaßstab für Tiefe/Format: [[project_testtag_analyse_2026-08-24]].

**Alle Trades dieses Tages sind fiktiv. Keine Datei wurde von Opus geändert, keine Regel angepasst, kein Chart angefasst. Alles unten ist Entscheidungsvorlage, nichts ist umgesetzt.**

---

## 0. Vorbemerkung — was ich prüfen konnte und was nicht

Zwei Dinge vorweg, weil sie die Belastbarkeit alles Folgenden bestimmen.

**(a) Die Datenlage ist deutlich dünner als am 24.08.** Das Rohprotokoll vom 24.08. hat 67 KB / 335 Zeilen und enthält jeden einzelnen der 37 Voll-Checks im Wortlaut. Das Protokoll vom 25.08. hat 11,7 KB / 101 Zeilen — bei **mehr** Trades und ähnlicher Laufzeit. Die komplette Solo-Strecke 16:52–20:00 Uhr (über drei Stunden, rund 190 Loop-Fires, davon ~37 Voll-Checks) ist in zwei Absätzen zusammengefasst. **Es gibt für diese drei Stunden keinen einzigen protokollierten Voll-Check, keine einzige Pflichtzeile, keinen Positions-Kasten, keinen Stall-Check.** Das ist kein Nebenaspekt: Der gesamte Pflichtzeilen-Apparat dieses Projekts existiert ausdrücklich zu dem Zweck, dass Levi im Nachhinein gegenprüfen kann ([[feedback_live_trading]] Punkt 9, "Struktureller Anti-Drift-Fix", Absatz "Ehrliche Einordnung"). Ein Tag ohne protokollierte Pflichtzeilen ist nicht auditierbar — unabhängig davon, wie sauber er tatsächlich gelaufen ist.

Ich kann daher Frage 6 (Prozessdisziplin über die Solo-Strecke) **nicht** so beantworten, wie sie gestellt wurde. Siehe Abschnitt 9 für das, was sich objektiv belegen lässt.

**(b) Der wichtigste Befund des Tages ist arithmetisch, nicht interpretativ.** Er hängt nicht daran, wie man den Tag liest, sondern nur an vier im Protokoll selbst genannten Zahlen. Er steht deshalb ganz vorn, vor der Trade-für-Trade-Besprechung.

---

## 1. Bilanz in Zahlen

- **Geplanter Loop-Start:** 15:30 Uhr · **tatsächlicher Start:** 16:10 Uhr (CronCreate-Backlog, 40 Min Verlust)
- **Levi aktiv im Loop:** 16:10–16:52 Uhr (42 Min) · **Sonnet solo:** 16:52–20:00 Uhr (3 Std 8 Min)
- **Fiktive Trades:** 3 — #T1 (Short, TP1 +63,05 Pkt + BE 0 Pkt, halbe Position), #T2 (Short, gecancelt, kein Ergebnis), #T3 (Short, volle Position, **bei Stopp offen/unresolved**)
- **Erkannte Prozessfehler laut Protokoll:** 3 (CronCreate-Backlog, TP1-RR-Schwellenwert bei #T1, verfrühte Kerzenabfrage bei #T2)
- **Von Opus zusätzlich gefundene Regelverstöße:** 2 harte (8c-SL-Mindestdistanz bei #T1 UND #T3), plus mindestens 6 fehlende Pflichtzeilen (Abschnitt 8)
- **Von Levi selbst aufgedeckt:** 2 von 3 protokollierten Fehlern (RR-Schwelle, Kerzenabfrage) — beide erst auf seine Nachfrage
- **`skipped_setups`-Einträge in `trades.db`:** 0 (Tabelle ist seit ihrer Anlage vollständig leer)

---

## 2. Kernbefund: #T1 und #T3 sind so, wie dokumentiert, arithmetisch unmöglich

### 2.1 Die Rechnung

Am 24.08.2026 nachts fanden Opus und Fable gemeinsam den Befund **N-1** (dokumentiert in [[feedback_chartanalyse]] Punkt 8b1, Änderungsvermerk "D-1"):

> SL-Floor (8c, min. 1,5× ATR) + Zone-1-Cap (8b1 Schritt 4, max. 2× ATR) ergeben ein maximal erreichbares RR in Zone 1 von **2 / 1,5 = 1,3333:1**.

Das ist keine Regel, sondern eine Identität. Sie gilt für jeden Trade, jeden ATR-Wert, jede Richtung. Daraus folgt ein Ein-Zeilen-Test:

> **Ein Trade, der gleichzeitig „TP-Realismus Zone 1 ✓" und „SL regelkonform nach 8c" meldet, kann kein RR(TP1) über 1,333:1 haben. Meldet er eines, ist mindestens eine der drei Angaben falsch.**

Beide Trades des Tages melden **RR 1,34:1**:

| | #T1 | #T3 |
|---|---|---|
| Entry | 29.172,80 | 29.171,25 |
| SL | 29.220,00 → **47,20 Pkt** | 29.217,00 → **45,75 Pkt** |
| TP1 (PP-Pivot) | 29.109,75 → **63,05 Pkt** | 29.109,75 → **61,50 Pkt** |
| RR(TP1) nachgerechnet | **1,3358:1** | **1,3443:1** |
| Zone-1-Decke | 1,3333:1 | 1,3333:1 |
| gemeldet | „Zone 1 ✓, 1,69×ATR" | „Zone 1 ✓, 1,35×ATR-15min" |

Beide liegen über der Decke. Beide melden trotzdem Zone 1 **und** einen unbeanstandeten SL. Das kann nicht sein.

### 2.2 Auflösung — der ATR, den das Protokoll nie ausschreibt

Das Protokoll nennt an keiner Stelle den absoluten ATR-Wert, nur abgeleitete Vielfache. Rückgerechnet aus den eigenen Angaben:

- **#T1:** SL 47,20 Pkt = „1,26× ATR" → ATR ≈ 37,46. TP1 63,05 Pkt = „1,69× ATR" → ATR ≈ 37,31. Beide Zahlen stammen also aus **einem** konsistenten ATR von rund **37,4 Pkt**.
- **#T3:** SL 45,75 Pkt = „~1,0× ATR-15min" → ATR ≈ 45,75. TP1 61,50 Pkt = „1,35× ATR-15min" → ATR ≈ 45,56. Ebenfalls **ein** konsistenter ATR von rund **45,6 Pkt**.

Damit ist der Widerspruch aufgelöst, und zwar eindeutig in eine Richtung:

- **#T1: SL/ATR = 1,26×** — Pflicht nach 8c ist ≥1,5× (Normal-Tier). **Unterschritten.**
- **#T3: SL/ATR = 1,00×** — Pflicht ≥1,5×. **Unterschritten, um ein Drittel.**

Die Zone-1-Einstufung war korrekt (1,69× bzw. 1,35× ATR, beide ≤2×). Falsch war die SL-Seite. Und genau dafür gibt es einen ausbuchstabierten Präzedenzfall im Regelwerk — Trade #21, [[feedback_chartanalyse]] Punkt 8c:

> „Schritt 1-2 dieser Regel (Rausch-Kerze ermitteln, SL ≥1,5-2× davon) ist ein Pflicht-Gate VOR dem RR-Check aus 8b, nicht danach — wird die SL-Mindestdistanz verletzt, ist die anschließende RR-Rechnung automatisch ungültig, selbst wenn sie rechnerisch ‚besteht'."

Das ist wörtlich der heutige Fall, zweimal.

### 2.3 Gegenprobe: die zweite mögliche Lesart — und warum sie #T3 auch nicht rettet

Man könnte einwenden: Vielleicht ist der im Protokoll verwendete ATR gar nicht der regelkonforme. `gate_check.cjs` definiert im Kopfkommentar ausdrücklich **ATR(14, 5min)**; das Protokoll rechnet bei #T3 explizit gegen „ATR-15min" (bei #T1 wird der Timeframe gar nicht genannt). Ein 15-Minuten-ATR ist naturgemäß deutlich größer als ein 5-Minuten-ATR — ein zu großer Nenner lässt jeden SL zu eng aussehen.

Also die Gegenprobe mit plausiblen 5min-ATR-Werten:

| angenommener 5min-ATR | #T3 SL/ATR | #T3 TP1/ATR | Folge für #T3 |
|---|---|---|---|
| 26 | 1,76× ✓ | 2,37× | **Zone 2 → halbe Position** |
| 28 | 1,63× ✓ | 2,20× | **Zone 2 → halbe Position** |
| 30 | 1,52× ✓ | 2,05× | **Zone 2 → halbe Position** |
| 31 | 1,48× ✗ | 1,98× | Zone 1, aber SL-Floor gerissen |

Das ist kein Zufall, sondern wieder N-1: Damit #T3 den SL-Floor besteht, muss ATR ≤ 30,5 sein; damit TP1 in Zone 1 fällt, muss ATR ≥ 30,75 sein. **Es existiert kein ATR-Wert, bei dem #T3 beides gleichzeitig erfüllt.**

**Ergebnis, das nicht von der ATR-Frage abhängt:**

- **#T3 war unter KEINER Lesart eine volle Position.** Entweder SL zu eng (→ nach 8c/#21-Präzedenz gar kein gültiger Trade), oder TP1 in Zone 2 (→ 8b1 Schritt 4: **halbe Position**). Die gemeldete Kombination „Zone 1 + volle Position + 4/4 GRÜN" ist in beiden Zweigen falsch.
- **#T1 war ebenfalls betroffen**, aber folgenlos: Er wurde ohnehin wegen der Chasing-Regel (Punkt 13) halbiert — das richtige Sizing kam über den falschen Weg zustande.

### 2.4 Was der 8c-konforme #T3 gewesen wäre

Rechnet man #T3 mit einem regelkonformen SL nach (1,5 × 45,6 = 68,4 Pkt → SL 29.239,65), ergibt sich:

> RR(TP1) = 61,50 / 68,40 = **0,90:1**

Unter 1:1. Nach [[feedback_chartanalyse]] Punkt 8b Schritt 4 und 8b1 Schritt 6 ist die Konsequenz nicht verhandelbar: **kein Trade** — oder auf einen engeren Entry näher an der Struktur warten. **Der Vorzeige-Trade des Tages, der einzige mit 4/4 GRÜN und voller Größe, hätte nach dem eigenen Regelwerk nicht stattfinden dürfen.**

Zur Fairness die Gegenrechnung für #T1: 1,5 × 37,4 = 56,1 Pkt → SL 29.228,90 → RR = 63,05/56,1 = **1,12:1**, weiterhin ≥1:1. **#T1 wäre also mit korrektem SL ein gültiger Trade geblieben** — und da das Hoch nach Entry nur bei 29.178,55 lag, hätte der weitere SL am Ergebnis nichts geändert. #T1s Verstoß ist echt, aber ohne Ergebnisfolge. #T3s ist beides: echt und folgenreich.

### 2.5 Der eigentliche Schluss daraus: `gate_check.cjs` wurde nicht ausgeführt

`gate_check.cjs` nimmt **einen einzigen** `--atr`-Parameter entgegen und speist damit sowohl `slFloorGate` (8c) als auch `tpRealismGate` (8b1). Genau deshalb **kann** das Skript die Kombination „Zone 1 + SL-Floor bestanden + RR 1,34" gar nicht ausgeben — sie ist bei einem gemeinsamen ATR unmöglich. Dass sie im Protokoll steht, ist der Beleg: **Für #T1 und #T3 wurde das Skript nicht gelaufen, die Gates wurden im Kopf gerechnet.**

Das ist bemerkenswert, weil [[feedback_live_trading]] Punkt 7b1 seit dem 24.08. explizit klarstellt, dass sich die Entry-Freigabe auf `gate_check.cjs` stützt, und Punkt 8c dasselbe für die SL/ATR-Zeile festhält („stützt sich ab sofort auf `gate_check.cjs` statt auf eine manuelle Kopfrechnung"). Und es erklärt den bereits protokollierten Fehler von 16:20 Uhr mit: Der veraltete 1,5:1-Schwellenwert wurde aus dem Gedächtnis geholt, obwohl `TP1_SELECTION_RR_THRESHOLD = 1.0` seit D-1 deterministisch im Skript steht. **Es war nie ein Wissensproblem — es war ein Ausführungsproblem.** Dazu Abschnitt 6.

---

## 3. Trade #T1 im Detail

**Was korrekt war:** Dual-Gate sauber (NAS100 5min + QQQ 15min beide per echtem Kerzenschluss unter eigener EMA50, 16:15 und 16:20 als 1/2 und 2/2). RR-Arithmetik stimmt exakt (1,3358 → 1,34, TP2 7,5742 → 7,57). Chasing-Erkennung nach Punkt 13 korrekt (4 gerichtete Kerzenschlüsse) und korrekt in eine Halbierung übersetzt. TP1-Teilverkauf 50 % + sofortiger BE-Nachzug entspricht 8b wörtlich. Die Unterscheidung „Entry-Signal braucht geschlossene Kerze / ein aktiver Stop löst sofort aus" wurde beim BE-Hit richtig angewendet — dieselbe saubere Trennung wie am 24.08.

**Was nicht korrekt war:**

1. **SL-Mindestdistanz 8c gerissen** (1,26× statt ≥1,5× ATR) — siehe Abschnitt 2. Keine `SL/ATR-Ratio:`-Pflichtzeile im Protokoll; das Verhältnis steht nur beiläufig in der SL-Beschreibung, ohne Abgleich gegen den Floor.
2. **RR-Schwellenwert-Fehler** (1,5:1 statt 1:1) — protokolliert, von Levi aufgedeckt, korrigiert. Siehe Abschnitt 6 zur Ursache.
3. **Keine `Spike-Ausnahme:`-Pflichtzeile.** Diese Zeile wurde am Morgen desselben Tages eingeführt ([[feedback_live_trading]] Punkt 7b1, „Spike-Ausnahme-Pflichtzeile", 25.08.2026) und gilt bei **jeder** Dual-Gate-Prüfung. Bei #T3 taucht sie auf, bei #T1 und #T2 nicht.
4. **Kein `Chop-Check:`** — Pflichtzeile bei jedem Vollentry nach [[project_risikomanagement]] („Explizite Chop-Offenlegungspflicht am Entry-Moment"). Bei #T1 formal entschärft, weil ohnehin halbiert wurde; bei #T3 nicht (Abschnitt 5).
5. **Kein Regime-Gate 8d dokumentiert** — weder hier noch irgendwo sonst am Tag. Das Voll-Check-Format ([[feedback_vollcheck_format]]) verlangt die Zeile in jedem Voll-Check. Dazu gleich mehr, denn heute war sie inhaltlich relevant.
6. **Kein `cooldown_check.cjs`** dokumentiert (Workflow Schritt 0, [[project_risikomanagement]]).

**Zur Volatilitätsfrage, die niemand gestellt hat:** TP1 (Tief 29.069,35) und BE-SL (Hoch 29.178,55) wurden laut Protokoll in **derselben** 5-Minuten-Kerze getroffen. Das ist eine Kerze mit **über 109 Punkten Spanne**. Bei einem ATR von ~37,4 ist das eine Kerze von fast 3× ATR. Zwei Konsequenzen:

- Es untermauert Befund 2.2 unabhängig: ein 47-Punkte-SL in einem Markt, der 109-Punkte-Kerzen produziert, ist genau der Fall, für den 8c existiert.
- Es hätte das Regime-Gate 8d (Kriterium Tagesrange) auf den Tisch gebracht. Fällt 8d auf „Schock-Tag ✓", greift **8b1a: No-Trade** — und dann wäre #T3 zwei Stunden später ohnehin ausgeschlossen gewesen. Das ist keine Behauptung, dass es ein Schock-Tag war (die drei 8d-Kriterien sind nirgends geprüft) — sondern die Feststellung, dass ein Gate mit potenzieller Komplett-Ausschluss-Wirkung an einem Tag mit 109-Punkte-Kerzen kein einziges Mal aufgerufen wurde.

**Zur „Lehre" im Protokoll.** Dort steht, der Q-Score ROT (2/4) sei „kein Beleg gegen den Q-Score, eher ein Beleg dass die Sizing-Reduktion bei ROT-Situationen die richtige Antwort ist". Das ist eine Ergebnis-Schlussfolgerung aus n=1 mit günstigem Ausgang — genau der Schluss, vor dem [[feedback_regeldisziplin]] warnt und den Fable am 30.07. unter „Hindsight-Warnung" ausdrücklich als unzulässig markiert hat. Zumal der Ausgang selbst nicht eindeutig ist: Aus einer 5-Minuten-Kerze lässt sich nicht ablesen, ob das Tief vor dem Hoch kam. Das Protokoll hat hier die für sich ungünstigere Reihenfolge unterstellt (erst TP1, dann BE) — das ist redlich und soll nicht kritisiert werden. Aber die Intrabar-Reihenfolge gehört benannt, nicht stillschweigend gesetzt, weil sie im allgemeinen Fall die Statistik schönt.

Nebenbei: Der Q3-Faktor wurde mit „1H-Kohärenz grenzwertig" auf ✗ gesetzt. Seit der Rückstufung des RVOL-Tie-Breakers am 25.08. bestimmt sich Q3 **ausschließlich** aus dem MTF-Dual-Gate-Ergebnis (5min/15min/1H NAS100 + QQQ) — das ist ein Boolean aus vier ablesbaren Zuständen, es gibt keinen Zustand „grenzwertig". Hier wurde ein mechanischer Faktor nach Gefühl gesetzt. Folgenlos (ROT wirkt im laufenden Fenster ohnehin nur als Anzeige), aber Teil eines Musters — siehe Q2/Q4 bei #T3.

---

## 4. Trade #T2 — der 7d0-Verstoß und warum die protokollierte Lehre zu weich ist

**Der Vorgang selbst ist vorbildlich behandelt.** Der Fehler wurde offengelegt, der Trade gecancelt, die Zeichnungen entfernt, und er zählt ausdrücklich nicht als Ergebnis (weder Win noch Loss noch BE). Das ist die richtige Behandlung und deckt sich mit [[feedback_regeldisziplin]]. Levis Begründung („Ich hätte den Trade nicht ausgelöst, da du mir einen Kursrange gegeben hast, was wir nicht erfüllt haben") ist die exakt richtige Kategorie: nicht das Ergebnis, sondern das gebrochene eigene Kriterium.

**Die Lehre im Protokoll ist es nicht.** Dort steht:

> „Künftig: bei einer Abfrage <15-20 Sekunden vor Kerzenende lieber kurz erneut abfragen statt den Zwischenstand als Schluss zu behandeln."

Zwei Probleme damit.

**Erstens: Die Regel war nicht lückenhaft, sie wurde nicht ausgeführt.** Punkt 7d0 schreibt vor: „Bar-Startzeit + Timeframe-Dauer muss ≤ aktuelle Zeit sein". Für die 14:40-Kerze heißt das 14:45:00 ≤ jetzt. Zum Abfragezeitpunkt war es 14:44:53. Der Test wäre also **eindeutig fehlgeschlagen**, wenn er tatsächlich durchgeführt worden wäre. Es gibt hier nichts zu präzisieren — keine „15-20-Sekunden-Zone" wäre nötig gewesen, die bestehende Sekundengenauigkeit hätte gereicht. Was fehlte, war die Ausführung.

**Zweitens: Damit ist der Pflichtzeilen-Mechanismus selbst nachweislich gebrochen.** Die 7d0-Pflichtzeile („Kerze geschlossen: JA (Ende HH:MM)") wurde am 05.08. eingeführt, nachdem derselbe Fehler drei Tage in Folge aufgetreten war. Fable hat den Restrisiko-Fall damals wörtlich vorhergesagt ([[feedback_live_trading]] Punkt 9, „Ehrliche Einordnung"):

> „Eine Pflicht-Zeile kann im Prinzip mechanisch mitgeschrieben werden, ohne dass die Prüfung dahinter ehrlich stattfand. … Falls sich in den nächsten Sessions zeigt, dass die Pflicht-Zeilen trotzdem mechanisch ohne echte Prüfung mitlaufen, ist das explizit als Fortbestehen des Problems zu benennen, nicht erneut nur als ‚ab jetzt besser aufpassen' zu verbuchen."

**Hiermit benannt: Der Fall ist eingetreten.** Und die protokollierte Lehre („lieber kurz erneut abfragen") ist exakt das, was Fable ausgeschlossen hat — ein „ab jetzt besser aufpassen" mit anderen Worten.

**Der Fix, der funktionieren würde, steht bereits im selben Protokoll.** Bei #T3 lautet die 7d0-Zeile nämlich:

> „NAS100 5min ✓ (3/3 echte Schlüsse, letzter 17:10-17:15 **verifiziert 17s nach Grenze**)"

Das ist die richtige Form: Die Zeile trägt ihre eigene Prüfgröße mit. „Verifiziert 17s nach Grenze" ist falsifizierbar; „JA" ist es nicht. Der Vorschlag in Abschnitt 14 ist deshalb kein neuer Mechanismus, sondern: **die Formulierung, die Sonnet heute bei #T3 selbst gefunden hat, verbindlich machen.**

**Und ein Muster, das ich nicht übergehen will:** Derselbe Kerzenschluss-Check schlug um **16:45 fehl** (#T2) und funktionierte um **17:00 korrekt** (erster Short-Anlauf, Abschnitt 7) — 15 Minuten auseinander, gleiches Instrument, gleiches Werkzeug, gleiche Person. Der Unterschied: Um 16:45 lag eine offene Handlungsfreigabe von Levi vor („gehst fiktiv short wenn es erfüllt ist"), um 17:00 nicht. **Der Fehler korreliert mit Handlungsdruck, nicht mit Wissensstand.** Das ist ein starkes Argument dafür, dass die Absicherung mechanisch belastbar sein muss statt disziplinabhängig — genau der Punkt, an dem eine Zeile mit Rohwerten einer Zeile mit Urteil überlegen ist.

---

## 5. Trade #T3 im Detail — was zu gut aussieht

Die Frage im Auftrag lautete: „Gibt es etwas, das zu gut aussieht, um wahr zu sein?" Ja, fünf Dinge.

**(1) Volle Position bei 4/4 GRÜN — nicht haltbar.** Siehe Abschnitt 2. Unter jeder ATR-Lesart ist entweder der SL zu eng (kein Trade) oder TP1 in Zone 2 (halbe Position). Das ist der harte Befund.

**(2) TP2 mit RR 7,78:1 ist eine Zahl ohne Bedeutung.** TP2 liegt 355,95 Punkte entfernt — das ist **7,8× ATR**. Zum Vergleich: 8b1 schließt einen TP1-Kandidaten jenseits von 3× ATR komplett aus (Zone 3), weil er außerhalb der realen Tagesbewegung liegt. Die gesamte bisherige Tagesspanne betrug rund 275 Punkte (Hoch 29.344,1 / Tief ~29.069). TP2 verlangt also mehr als die volle Tagesrange — in den 2 Std 45 Min, die bis zum US-Schluss blieben, und mit einem Overnight-Verbot ([[project_risikomanagement]], „Overnight-Politik") sowie einem Blackout-Tag am 26.08. (GDP + PCE + NVDA-Earnings) dahinter.

Das ist eine echte Lücke in **8b2**: Die Regel prüft für TP2 „echtes Chart-Level" und „RR≥2:1" — sie hat die RR-Prüfung von 8b1 geerbt, aber **nicht** dessen eigentliche Innovation, die Distanz-Plausibilität. 8b2 wurde ausdrücklich geschaffen, damit ein Trade nicht „billig bei TP1" durchgeht ohne realistischen Weg zu TP2. Ein TP2, das rechnerisch 7,78:1 liefert und praktisch unerreichbar ist, erfüllt diesen Zweck nicht — es erfüllt nur seinen Wortlaut. Dieselbe Konstruktion war schon bei #T1 um 16:20 im Einsatz (identisches S1-Level, 357,5 Punkte): **Das Level bleibt fest, während der verbleibende Handelstag schrumpft — die RR-Zahl bleibt schmeichelhaft, die Erreichbarkeit kollabiert.**

**(3) Q2 und Q4 sind nicht überprüfbar.** Q2 steht als **Spanne** im Protokoll: „0,74-1,26×ATR von Impulsursprung". Q2 ist per Definition eine einzelne Zahl bei gegebenem Anker — eine Spanne bedeutet, dass zwei Anker nebeneinander stehengelassen wurden, ohne sich zu entscheiden. Der verwendete Ankerkurs wird nicht genannt, obwohl [[feedback_chartanalyse]] 8a4 Bedingung 5 und die B3-Offenlegung vom Morgen desselben Tages genau das verlangen („der verwendete Anker muss in der Pflichtzeile stehen"). Zusätzlich wurde offenbar wieder gegen den 15min-ATR gerechnet, während `gate_check.cjs` Q2 verbindlich als `(Entry − Impuls-Ursprung) / ATR(14, **5min**)` definiert. Das ist B3 — die gestern offengelegte Anker-Uneinheitlichkeit — **einen Tag nach ihrer Offenlegung erneut aufgetreten**, in leicht verschobener Form (nicht Anker, sondern Nenner).

Q4 („Runway frei bis TP1 ✓") verlangt `runway_ratio = Distanz(Entry→erstes Gegenlevel) / Distanz(Entry→TP1) ≥1,0`, wobei das Gegenlevel manuell zu benennen ist. Es wird nicht benannt.

**Damit ruhen 2 der 4 grünen Q-Faktoren auf nicht dokumentierten Eingaben.** „4/4 GRÜN" ist keine geprüfte Aussage, sondern eine plausible.

**(4) Kein Chop-Check, obwohl der Tag ihn geradezu aufdrängt.** Entry #T1 um 16:20 bei 29.172,80. Entry #T3 um 19:15 bei 29.171,25. **Differenz nach drei Stunden: 1,55 Punkte.** Dazwischen ein Ausflug auf 29.069 und zurück. Das ist die Definition eines Round-Trips. Ob die formalen Chop-Kriterien (RSI/MACD-Flip-Flop bei flachem VIX) erfüllt waren, kann ich aus dem Protokoll nicht feststellen — aber die Pflichtzeile aus [[project_risikomanagement]] („Explizite Chop-Offenlegungspflicht am Entry-Moment", bei **jedem** Vollentry, eingeführt nach Trade #29 für exakt diese Konstellation) fehlt vollständig. Bei einem Vollentry ist das die Zeile mit der unmittelbarsten Sizing-Konsequenz: Chop ✓ → Halbierung nach Stacking-Regel.

**(5) Der Entry sitzt auf einem Level, das an diesem Tag schon einmal einen Stop gefressen hat.** Der BE-SL von #T1 lag bei 29.172,80 und wurde um 16:35 per Wick (29.178,55) ausgelöst. #T3s Entry liegt 1,55 Punkte davon entfernt. 8c2 (SL-Cluster-Regel) greift formal nicht — sie betrifft die SL-Platzierung, und #T3s SL (29.217) liegt 44 Punkte außerhalb der ±20-30-Punkte-Zone. Aber der Geist von 8c2 und [[feedback_wiederholte_zonentests]] ist unmittelbar einschlägig, und eine 8c2-Prüfzeile ist im Protokoll nirgends zu sehen.

**Was an #T3 dagegen wirklich gut war** (und das soll nicht untergehen): Das Dual-Gate war zum ersten Mal an diesem Tag über **alle** Ebenen erfüllt — NAS100 5min (drei echte Schlüsse), NAS100 15min, NAS100 1H, QQQ 15min (Primärtrigger, per echtem Kerzenschluss 709,93 unter EMA50 710,35), QQQ 1H. Das ist genau der Baustein, der beim ersten Anlauf fehlte, und er wurde nicht herbeigeredet, sondern abgewartet. Die 7d0-Verifikation ist in der vorbildlichen Form dokumentiert („17s nach Grenze", dreifach). Die Spike-Ausnahme-Pflichtzeile taucht hier zum ersten Mal überhaupt live auf und ist korrekt befüllt („nicht einschlägig — mehrstufige, mehrfach bestätigte Bewegung"). Die Chasing-Prüfung ist ehrlich negativ beantwortet („nur 2 konsekutive Bearish-Closes, kein 4-5er-Lauf") statt zur Halbierung hin gebogen. Die RR-Arithmetik stimmt auf vier Nachkommastellen.

**Ehrliche Gesamteinordnung von #T3:** Die *Signal*-Seite (Dual-Gate, MTF-Kohärenz, Timing, Geduld) war die beste des Tages und deutlich besser als bei #T1. Die *Risiko*-Seite (SL-Distanz, Zone, Sizing, TP2-Realismus) war die schwächste. Genau diese Trennung ist das Bemerkenswerte: Der Trade sieht gut aus, weil alles, was **beobachtet** wurde, gut war — und weil alles, was **gerechnet** werden musste, nicht gerechnet, sondern geschätzt wurde.

---

## 6. Die beiden Prozessfehler haben eine gemeinsame Ursache — struktureller Befund

Das Protokoll führt drei Einzelfehler auf. Meine Beurteilung: **Es sind nicht drei Fehler, sondern einer, dreimal.**

| Fehler | Es existierte ein deterministisches Verfahren | Was stattdessen passierte | Warum es unsichtbar blieb |
|---|---|---|---|
| RR-Schwelle 1,5:1 statt 1:1 (#T1, 16:20) | `gate_check.cjs`, `TP1_SELECTION_RR_THRESHOLD = 1.0` | Schwellenwert aus dem Gedächtnis | Die `Entry-Freigabe:`-Zeile zeigt nur das Urteil, nicht die Quelle |
| SL-Floor 8c gerissen (#T1 **und** #T3) | `gate_check.cjs`, `slFloorGate` | Verhältnis im Kopf, gegen den falschen ATR, ohne Abgleich mit dem Floor | Die `SL/ATR-Ratio:`-Pflichtzeile fehlt; der ATR-Rohwert steht nirgends |
| Kerze 7 s vor Schluss abgefragt (#T2, 16:45) | Punkt 7d0: Bar-Start + Dauer ≤ `Bash date` | Zustand angenommen | „Kerze geschlossen: JA" ist nicht falsifizierbar |

**Die gemeinsame Struktur: Jede dieser Pflichtzeilen transportiert ein Ergebnis, aber nicht ihre Eingaben. Deshalb sieht ein gerechneter Wert im Output exakt aus wie ein geratener.** Für Levi ist im Protokoll nicht unterscheidbar, ob eine Prüfung stattfand.

Und dieses Projekt hat den Gegenbeweis bereits selbst geliefert: **Die Tweet-Check-Zeile funktioniert**, weil sie seit dem 25.08. das rohe Minuten-Delta mitführen muss statt nur „fällig/nicht fällig". Ein Delta ist gegenprüfbar, ein Urteil nicht. Genau diese Umstellung war Opus-Vorschlag 2 vom 24.08., und sie ist der einzige Pflichtzeilen-Fix in diesem Projekt, der aus einem Urteil einen Rohwert gemacht hat — und der einzige, der heute nicht gebrochen wurde.

**Das ist der strukturelle Befund des 25.08.:** Der Pflichtzeilen-Apparat ist über Monate gewachsen, aber nur eine seiner Zeilen ist falsifizierbar gebaut. Die anderen sind Behauptungen. Solange das so ist, skaliert jede weitere Pflichtzeile das Problem, statt es zu lösen — und [[feedback_live_trading]] Punkt 14 hat bereits belegt, dass die reine Zeilenzahl selbst zur Fehlerquelle wird (Trade #42, „Hinweis zu spät gelesen").

Die Konsequenz ist deshalb ausdrücklich **nicht** „mehr Pflichtzeilen", sondern „dieselben Pflichtzeilen, mit Rohwerten statt Urteilen". Vorschläge 1-3 in Abschnitt 14.

---

## 7. Der nicht ausgelöste erste Short-Anlauf um 17:00 — echte Bewährungsprobe?

**Nein — jedenfalls nicht in dem starken Sinn, in dem die Frage gestellt ist. Aber wertvoller, als die Frage vermuten lässt.**

Der Ablauf: Eine bestätigte Bearish-Kerze unter EMA50, die zweite Bestätigungskerze (16:55-17:00) widerlegt das Signal (V-förmige Erholung, Schluss klar über EMA50, 15min und 1H ebenfalls zurück darüber). Kein 2/2, kein Trigger.

Das ist **keine Disziplinprobe**, weil das Setup sich selbst aufgelöst hat. Die Regel musste keiner Versuchung standhalten — nach der zweiten Kerze gab es nichts mehr zu widerstehen. Das ist strukturell dieselbe Einordnung wie beim 24.08.-Fall (B) („das Setup invalidierte sich von selbst") und bei Opus' Satz von gestern: *„Ein Gate, das nie öffnen kann, testet keine Disziplin."* Hier gilt die Variante: **Ein Signal, das sich selbst widerlegt, testet keine Geduld.**

**Aber es ist ein echter Beleg für etwas anderes, und zwar ein besserer als eine Disziplinprobe:** Am 24.08. ist an genau dieser Stelle ein Fehler passiert — eine noch offene Kerze wurde als „erste Ablehnungskerze" gewertet (Kerzenschluss-Boundary-Bug, 16:45–16:56). Heute wurde dieselbe Konstellation um 17:00 sauber gehandhabt: auf den echten Schluss gewartet, das Ergebnis gegen die eigene Erwartung akzeptiert. **Der 24.08.-Fehler hat sich an derselben Stelle nicht wiederholt.**

Und das macht den Kontrast zu 16:45 (#T2) erst richtig aussagekräftig, siehe Abschnitt 4: **derselbe Check, 15 Minuten früher, gebrochen — unter offener Handlungsfreigabe.** Ein Tag, der beide Fälle nebeneinander enthält, sagt mehr über die Fehlermechanik aus als zehn saubere Tage. Der Befund ist nicht „die Regel hält", sondern: **die Regel hält im Leerlauf und bricht unter Handlungsdruck.**

**Versteckte Schwäche, die ich zusätzlich sehe:** Der erste Anlauf hätte laut [[feedback_live_trading]] Punkt 7b1b bzw. [[feedback_chartanalyse]] 8a2 möglicherweise eine Schattenmessung ausgelöst — und ganz sicher hätte die Situationsklasse 8a4 („Basis-Reclaim nach Session-Extrem", seit heute morgen im Anzeige-Modus) geprüft werden müssen, sobald Bedingung 1 (Session-Extrem mit benennbarem Auslöser) vorlag. Der Tag hatte ein solches Extrem: die R1-Ablehnung um 14:00-14:06 UTC unter einem klar benennbaren Makro-Cluster (CB Consumer Confidence Miss, New Home Sales, Collins hawkish). **Weder die 8a4-Pflichtzeile noch ein `add_skipped_setup.cjs`-Aufruf sind im Protokoll zu finden — und `skipped_setups` in `trades.db` ist bis heute vollständig leer (0 Zeilen).** Die gesamte gestern beschlossene B1/B2/B3-Schattenmessung hat an ihrem ersten möglichen Einsatztag keine einzige Zeile Daten produziert.

Für einen fiktiven Testtag ist es vertretbar, nicht in die DB zu schreiben. Aber dann fehlt der Klasse auch der Trockenlauf — und die Frage, wie viele Sessions bis zu einer auswertbaren Stichprobe nötig sind, verschiebt sich weiter nach hinten, ohne dass es jemandem auffällt.

---

## 8. Fehlende Pflichtzeilen — vollständige Liste

Alle folgenden Zeilen sind im geltenden Regelwerk als „fehlt sie, gilt der Check als nicht durchgeführt" markiert. Keine davon findet sich im Protokoll des 25.08.:

| Pflichtzeile | Quelle | eingeführt | betrifft |
|---|---|---|---|
| `SL/ATR-Ratio: X,Xx — Tier erfüllt ✓/✗` | [[feedback_chartanalyse]] 8c | 07.08.2026 | #T1, #T3 |
| `TP-Realismus: … Zone 1/2/3 …` im vorgeschriebenen Format inkl. ATR-Rohwert und Box-Breite | [[feedback_chartanalyse]] 8b1 | 18./24.08.2026 | #T1, #T3 (nur verkürzt vorhanden) |
| `Chop-Check: …` bei jedem Vollentry | [[project_risikomanagement]] | 03.08.2026 | **#T3 (Vollentry!)** |
| `Spike-Ausnahme: …` bei jeder Dual-Gate-Prüfung | [[feedback_live_trading]] 7b1 | **25.08.2026** | #T1, #T2 (bei #T3 korrekt vorhanden) |
| `Regime-Gate (8d): Schock-Tag ✓/✗ (n/3)` in jedem Voll-Check | [[feedback_vollcheck_format]] | 27.07.2026 | gesamter Tag |
| `Stall-Check: X Kerzen ohne neues Extrem, RSI Y Punkte …` bei **jedem** Check-in mit offener Position | [[feedback_live_trading]] 12.4 | 06.08.2026 | gesamte #T3-Laufzeit (45 Min) und #T1-Laufzeit |
| `Tweet-Check: …` und `Format: Fließtext ✓` in jedem Voll-Check | [[feedback_vollcheck_format]] | 27.07.2026 | gesamter Tag (siehe Abschnitt 9) |
| `Basis-Reclaim-Klasse: Bed. 1-5 …` sobald Bedingung 1 erfüllt | [[feedback_chartanalyse]] 8a4 | **25.08.2026** | erster Short-Anlauf 17:00 |
| Positions-Kasten (SL/TP1/TP2 + Zert.-Preise) bei jedem Status-Update | [[feedback_live_trading]] Punkt 8 | 01./04./16.07.2026 | gesamte #T3-Laufzeit |

**Wichtige Einschränkung, die ich ausdrücklich mache:** Das heißt **nicht** zwingend, dass diese Zeilen live nicht ausgegeben wurden. Es heißt, dass sie im Protokoll nicht stehen. Bei einer Zeile wie 12.4 (Stall-Check, bei jedem Check-in) wäre eine vollständige Protokollierung über 190 Fires auch gar nicht sinnvoll. Aber die Konsequenz bleibt: **Für den gesamten Tag lässt sich keine einzige dieser Prüfungen nachträglich belegen.** Und drei der neun Zeilen (8c, 8b1, Chop-Check) sind nachweislich nicht nur unprotokolliert, sondern inhaltlich falsch bzw. gar nicht durchgeführt — das zeigt Abschnitt 2.

Bemerkenswert positiv im Gegenzug: **Die zwei am 25.08. morgens neu eingeführten Zeilen wurden bei #T3 tatsächlich angewendet** (Spike-Ausnahme; und die Tweet-Check-Delta-Logik lief, siehe Abschnitt 9). Frisch eingeführte Regeln greifen — es sind die alten, die verblassen. Das ist ein bekanntes Muster in diesem Projekt (7d0: drei Rückfälle nach Einführung) und ein Argument gegen „noch eine Zeile" als Standardantwort.

---

## 9. Prozessdisziplin über die Solo-Strecke (16:52–20:00) — was objektiv belegbar ist

Die Frage im Auftrag lautete, ob MTF-Schritte, Tweet-Checks und Positions-Kasten lückenlos durchgehalten wurden, und bat um Stichproben zu verschiedenen Zeitpunkten. **Das Protokoll enthält für diese drei Stunden keine Zeitpunkte, zwischen denen sich stichproben ließe.** Es gibt zwei zusammenfassende Absätze, keine Voll-Check-Einträge.

Was ich unabhängig verifizieren konnte:

**Belegt (positiv):** `scripts/x_last_fetch.json` steht auf `2026-08-25T17:55:17.000Z` = **19:55:17 Ortszeit** — also 5 Minuten vor dem Loop-Stopp um 20:00:17 Uhr. Der Tweet-Fetch lief mindestens bis unmittelbar vor Schluss und ist mit der seit heute wieder geltenden 10-Minuten-Schwelle vereinbar. Das ist ein echter, harter Datenpunkt und spricht dafür, dass der Tweet-Mechanismus über die Solo-Strecke lief.

**Belegt (negativ):** `skipped_setups` in `trades.db` hat 0 Zeilen. Für einen fiktiven Tag vertretbar, aber die Schattenmessung hat damit weiterhin n=0.

**Nicht verifizierbar:** alles andere. Ob der 1H-Schritt bei jedem Voll-Check lief, ob der QQQ-Durchlauf nie ausgelassen wurde, ob die Stall-Check-Zeile bei jedem Tick erschien, ob der Positions-Kasten mit Zertifikatspreisen ausgegeben wurde, ob die Format-Zeile stand. Die Behauptung „kein einziges Auslassen über die gesamte Laufzeit" mag zutreffen — **sie ist aus dem vorliegenden Material weder belegbar noch widerlegbar.**

**Meine ehrliche Einschätzung dazu, mit der gebotenen Vorsicht:** Es gibt einen indirekten Hinweis in Richtung *tatsächlicher* Disziplin — der Fall um 17:00, in dem eine Kerze regelkonform abgewartet und ein unerwünschtes Ergebnis akzeptiert wurde, und die vorbildliche 7d0-Formulierung bei #T3 („verifiziert 17s nach Grenze"). Das sind zwei Stellen, an denen jemand, der abkürzt, nicht so schreiben würde. Es gibt aber auch einen Hinweis in die Gegenrichtung: Die drei nachgewiesenen Rechenfehler (Abschnitt 2/6) betreffen alle die *rechnerischen* Prüfungen, während die *beobachtenden* Prüfungen (MTF, Dual-Gate, Kerzenschlüsse) durchweg sauber wirken. Es spricht also einiges dafür, dass die Beobachtungsdisziplin gehalten hat und die Rechendisziplin nicht — was zum Befund aus Abschnitt 6 passt.

**Was daraus folgt, ist unabhängig von der Antwort:** Ein Testtag, dessen Zweck ausdrücklich ein anschließendes Prozess-Review ist, muss ein Protokoll erzeugen, das dieses Review trägt. Der 24.08. tat das (335 Zeilen), der 25.08. nicht (101 Zeilen). Das ist die einzige Regression des Tages, die ich als Regression bezeichnen würde. Vorschlag 6 in Abschnitt 14.

---

## 10. Der offene Trade bei Stopp — Zensierung, Bias und das eigentliche Loch im Mandat

### 10.1 Ja, „bis 20 Uhr ODER SL/TP" erzeugt einen Bias — und zwar einen benennbaren

Das ist **Rechtszensierung** (right censoring): Trades, die schnell auflösen, gehen mit Ergebnis in die Statistik; ein Trade, der beim Abschneiden noch läuft, hat keines. Wenn langsam auflösende Trades systematisch anders sind als schnelle — und das sind sie, langsame Auflösung korreliert mit Range/Chop/schwachem Setup —, ist die verbleibende Stichprobe verzerrt, nicht bloß kleiner.

Und die Verzerrung ist heute im Protokoll bereits sprachlich sichtbar. Dort steht: „fiktiv +18,4 Pkt unrealisiert im Plus", „durchgehend im Plan", „SL nie ernsthaft angetestet". Alle drei Aussagen stimmen. Zusammen ergeben sie trotzdem eine positive Bilanzierung eines Trades, der kein Ergebnis hat. **Ein Mark-to-Market an einem willkürlich gesetzten Zeitpunkt ist kein Resultat — es ist der Kursstand um 20:00 Uhr.** Bei einem TP1 in 43,1 Punkten Entfernung und einem SL in 64,15 Punkten Entfernung war der Trade zum Stoppzeitpunkt in jeder Hinsicht offen.

### 10.2 Die schärfere Version: #T3s Gates wurden ohne Zeithorizont gerechnet

Entry 19:15 Uhr. Der Loop-Stopp um 20:00 Uhr wurde von Levi erst um 19:36 Uhr festgelegt — beim Entry war der Horizont also unbekannt, das ist **kein** Verstoß. Aber ab 19:36 Uhr war er bekannt, und ab da hatte die Position eine harte Restlaufzeit von 24 Minuten. In diesem Fenster:

- TP1 lag 61,5 Punkte entfernt,
- TP2 lag 355,95 Punkte entfernt,
- das Regelwerk verbietet Overnight-Positionen,
- der Folgetag ist ein Blackout-Tag (GDP + PCE + NVDA).

Es gibt im gesamten Regelwerk **keinen** Mechanismus, der einen Zeithorizont in die Entry-Gates einspeist. 8b1 fragt „wie weit ist TP1 in ATR?", nie „wie viel Zeit bleibt für diese Distanz?". Die einzige zeitbezogene Regel ist die 3-Stunden-Haltedauer-Grenze, und die wirkt in die andere Richtung. Das ist die eigentliche Antwort auf die Frage nach dem Bias: **Nicht nur die Auswertung ist zensiert — die Entscheidung selbst wurde ohne Kenntnis ihrer eigenen Frist getroffen und danach nicht nachgeführt.**

### 10.3 Wie ein offener Trade am Tagesende behandelt werden sollte

Drei Optionen, meine Empfehlung dabei:

- **A) Zensiert führen (empfohlen).** Eigener Status `OFFEN/ZENSIERT` in `trades.db` bzw. im Testtag-Protokoll. Zählt in den Nenner für Prozess-Kennzahlen (Regelkonformität, Gate-Vollständigkeit), **nicht** in Win-Rate, RR oder EV. Berichtsformat: „n=15, davon 1 zensiert". Kein Mark-to-Market, keine Formulierung wie „im Plus".
- **B) Vorwärts auflösen und als hypothetisch markieren.** Am nächsten Handelstag mit echten Daten gegen den ursprünglichen SL/TP prüfen, was tatsächlich passiert wäre, und das als hypothetisches Ergebnis führen. Die Infrastruktur dafür existiert bereits (`--hypothetisch` im Tagesabschluss-Prozess, [[feedback_tagesabschluss]]). Sauber, aber Mehraufwand.
- **C) Time-Boxed-Exit als Teil des Trade-Plans.** Sobald ein Stopp-Zeitpunkt bekannt ist, wird er Teil der Position: „flat bis 19:55" wird ausgesprochen und der Exit als `ZEIT-EXIT` geloggt, mit dem tatsächlich erzielten Ergebnis. Kein Zensierungsproblem mehr, dafür ein anderes Ergebnis als das ungestörte.

**Meine Empfehlung: A als Buchungsregel, C als operative Regel.** A löst das Statistikproblem, C löst das Entscheidungsproblem aus 10.2. B nur, wenn Levi den Erkenntniswert des kontrafaktischen Verlaufs will.

---

## 11. Das „stop"/„weiter"-Zwischenspiel um 19:36 Uhr

**Ja, hier lag ein Transparenzproblem — ein mildes, aber ein echtes, und es ist dieselbe Lücke wie in Abschnitt 10.**

Der Ablauf spricht für sich: Levi sagt „stop", stellt dann fest „Ah okay du bist im Trade", und weist „weiter" an. Das bedeutet: **Im Moment seiner Stopp-Anweisung war ihm nicht präsent, dass 21 Minuten zuvor eine volle Position eröffnet worden war.** Nach [[feedback_live_trading]] Punkt 8 (Positions-Kasten bei jedem Status-Update) und 12.4 (Stall-Check-Zeile bei jedem Check-in mit offener Position) hätte eine offene Position schwer zu übersehen sein müssen — sie wäre in jedem einzelnen Tick sichtbar gewesen.

Zwei Deutungen, beide unangenehm:

- Die Zeilen liefen, und Levi hat sie beim Wiedereinstieg in einen drei Stunden alten Nachrichtenstrom schlicht nicht gelesen — dann ist die Zeilendichte selbst das Problem (das Argument aus Punkt 14, Trade #42: „Hinweis zu spät gelesen").
- Die Zeilen liefen nicht — dann ist es ein Regelverstoß, den ich mangels Protokoll nicht feststellen kann.

**In beiden Fällen ist die Lehre dieselbe:** Der Übergang „keine Position" → „Position offen" ist eine Zustandsänderung, und Zustandsänderungen brauchen eine andere Darstellungsform als laufender Status. Ein Kasten, der in Tick 1 und in Tick 180 identisch aussieht, transportiert keine Neuigkeit.

**Und das Grundproblem ist das Mandat selbst.** Levis Übergabe um 16:52 lautete sinngemäß: „Ich muss los, ich sage stop, wenn du aufhören sollst." Damit war ein Start und ein Ende-Signal definiert — aber nie, **was beim Stopp mit einer offenen Position geschieht**. Genau diese undefinierte Terminalbedingung ist um 19:36 zum ersten Mal sichtbar geworden (Levi musste sie ad hoc auflösen) und um 20:00 zum zweiten Mal (unresolved Trade). **Das „stop"/„weiter"-Zwischenspiel und der offene Trade am Tagesende sind nicht zwei Themen, sondern zweimal dasselbe Loch.** Vorschlag 5 in Abschnitt 14 schließt beide zusammen.

---

## 12. Wiederkehrer vom 24.08. — was sich innerhalb von 24 Stunden wiederholt hat

Drei Befunde von gestern sind heute erneut aufgetreten, zwei davon **nach** ihrer Dokumentation:

1. **Pane-Symbol-Verwechslung.** Gestern Fehler 4 („`data_get_ohlcv`/`quote_get` ohne `pane_focus`-Reset"), Opus-Vorschlag 8 („Symbol-Absicherung"). Heute: „Chart-Pane war zwischenzeitlich fälschlich auf QQQ hängengeblieben". Erkannt und korrigiert, aber der Vorfalltyp reproduziert sich innerhalb eines Tages nach seiner Identifikation.
2. **ATR-/Anker-Uneinheitlichkeit (B3).** Gestern als Q2-Ankerproblem offengelegt und mit einem zweiten informativen Parameter versehen. Heute in verschobener Form wieder da: nicht der Anker, sondern der **Nenner** (15min-ATR statt 5min-ATR), und diesmal mit harter Sizing-Konsequenz statt nur informativ (Abschnitt 2). Das ist der Hinweis, dass B3 zu eng gefasst wurde: Offengelegt wurde der Anker, nicht die zugrundeliegende Krankheit — **Kennzahlen werden ohne ihre Bezugsgröße notiert.**
3. **Loop-Kaltstart frisst das erste Zeitfenster.** Gestern begann der Loop um 15:52 statt zum Sessionstart, heute um 16:10 statt 15:30 (CronCreate-Backlog: Memory-Vollstudium + Tweet-Fetch **vor** dem Anlegen des Cron-Jobs). Folge heute: Das 15:30-16:00-Halbierungsfenster verstrich ungeprüft. Ohne Marktfolge, weil keine Position offen war — aber es ist der zweite Tag in Folge, an dem die erste halbe Stunde strukturell verloren geht. Die Reihenfolge ist umkehrbar: `CronCreate` ist billig und idempotent und könnte **zuerst** laufen, während Memory und Tweets bereits im tickenden Loop abgearbeitet werden.

---

## 13. Was heute wirklich gut war

Nicht als Ausgleich, sondern weil es unabhängig belegbar ist:

- **Der zweite Anlauf wurde abgewartet.** Zwischen dem invalidierten Signal um 17:00 und dem tatsächlichen Setup um 19:15 liegen über zwei Stunden Solo-Loop ohne Trade. Dass in dieser Zeit kein halbgares Setup genommen wurde, obwohl niemand zusah, ist der ehrlichste Befund des Tages zu Levis Frage nach der Disziplin.
- **#T2 wurde konsequent gecancelt und aus der Wertung genommen.** Kein Ergebnis, keine Rechtfertigung, kein „war ja fast erfüllt". Das ist die Kategorie-Trennung, die [[feedback_regeldisziplin]] verlangt.
- **Der 24.08.-Kerzenschluss-Bug hat sich um 17:00 nicht wiederholt** (Abschnitt 7).
- **Die 7d0-Zeile bei #T3 ist die beste Formulierung dieser Pflichtzeile, die es in diesem Projekt bisher gibt** („verifiziert 17s nach Grenze") — falsifizierbar statt behauptend. Sie ist der Prototyp für Vorschlag 1.
- **Die Spike-Ausnahme-Pflichtzeile lief bei ihrer ersten Live-Anwendung korrekt.** Der gestrige Opus-Vorschlag 4 hat funktioniert.
- **Die Tweet-Check-Delta-Umstellung hat gehalten** (Abschnitt 9) — der einzige Pflichtzeilen-Fix, der heute nicht gebrochen wurde, und zugleich der einzige, der Rohwerte statt Urteile verlangt. Das ist kein Zufall.
- **Alle RR-Rechnungen im Protokoll stimmen auf vier Nachkommastellen** (1,3358 / 7,5742 / 1,3443 / 7,7803). Die Arithmetik war nie das Problem — die Eingaben waren es.

---

## 14. Priorisierte Vorschlagsliste

Kennzeichnung wie am 24.08.: **(a)** = Prozess/Tooling, keine Regeländerung · **(b)** = echte Regeländerung, braucht Levis Zustimmung. **Nichts davon ist umgesetzt.**

**1. (a, höchste Priorität) Pflichtzeilen tragen ab jetzt ihre Rohwerte, nicht nur ihr Urteil.**
Keine neue Zeile, keine neue Regel — nur eine Formatvorgabe für drei bestehende Zeilen, nach dem Vorbild der bereits funktionierenden Tweet-Check-Zeile und der #T3-eigenen 7d0-Formulierung:
- `Kerze geschlossen: JA (Bar-Start 17:10 + 5min = 17:15:00 ≤ jetzt 17:15:17)` statt `JA (Ende 17:15)`
- `SL/ATR-Ratio: 45,75 Pkt / ATR(14,5min)=X Pkt = Y,YYx — Floor 1,5x = Z,Z Pkt — ✓/✗` — **ATR-Rohwert und Timeframe zwingend im Text**
- `TP-Realismus: TP1 61,5 Pkt / ATR(14,5min)=X Pkt = Y,YYx → Zone n` — derselbe ATR-Rohwert wie in der SL-Zeile, sichtbar identisch
Begründung: Abschnitt 6. Hätte alle drei Fehler des Tages sichtbar gemacht, ohne die Checklistenlänge zu erhöhen.

**2. (a) `gate_check.cjs`-Aufruf im Protokoll belegen.**
Die `Entry-Freigabe:`-Zeile führt den tatsächlich abgesetzten CLI-Aufruf oder die `--json`-Kernzeile mit. Solange „im Kopf gerechnet" und „Skript gelaufen" im Output identisch aussehen, ist 7b1s Klarstellung vom 24.08. nicht durchsetzbar. Belegt durch Abschnitt 2.5: Wäre das Skript gelaufen, wären #T1 und #T3 gar nicht in dieser Form entstanden.

**3. (a) Konsistenz-Assertion in `gate_check.cjs`: der 1,333-Test.**
Wenn `slFloorGate` besteht **und** `tpRealismGate` Zone 1 meldet, muss RR(TP1) ≤ 1,3333 sein. Ergibt sich etwas anderes, ist eine Eingabe falsch → harter Warnhinweis statt PASS. Das Skript kann diesen Widerspruch heute intern gar nicht erzeugen (ein gemeinsamer `--atr`), aber die Assertion macht ihn auch bei **manuell** befüllten Werten sofort sichtbar und gibt Levi einen Ein-Zeilen-Audit für alle Altfälle: *RR über 1,333 bei gemeldeter Zone 1 = Widerspruch.*

**4. (a/b, Levi entscheidet) ATR-Timeframe für 8c/8b1/Q2 einmal verbindlich festschreiben.**
`gate_check.cjs` sagt ATR(14, 5min). Das Protokoll rechnet 15min. Das ist keine Kosmetik — es entscheidet bei #T3 zwischen „kein Trade", „halbe Position" und „volle Position" (Abschnitt 2.3). Zwei saubere Wege: (i) 5min als einzig gültiger Nenner bestätigen und die Praxis anpassen **(a)**, oder (ii) bewusst auf 15min umstellen und dann **beide** Schwellen (1,5×-Floor und 2×/3×-Zonen) neu kalibrieren, weil sie für den 5min-Wert gesetzt wurden **(b)**. Was nicht geht, ist der Ist-Zustand: 5min-kalibrierte Schwellen gegen 15min-Werte zu prüfen. Meine Empfehlung: **(i)**, weil (ii) mitten im 15-Trade-Sizing-Fenster eine Schwellen-Neukalibrierung bedeutet und das Fenster kontaminiert.

**5. (a) Terminalbedingung für das Solo-Mandat definieren.**
Drei Zeilen, die beide Löcher aus Abschnitt 10/11 schließen:
- Beim Eröffnen einer Position im Solo-Betrieb ein einmaliger, deutlich markierter Zustandswechsel-Hinweis (nicht derselbe Kasten wie im laufenden Status).
- Auf „stop" bei offener Position wird **zuerst** der Positionsstand geantwortet und die Entscheidung eingeholt (halten / zeitlich schließen / weiterlaufen), nicht kommentarlos gestoppt.
- Sobald ein Stopp-Zeitpunkt bekannt ist, wird die verbleibende Zeit gegen die TP1-Distanz gestellt und ein Time-Boxed-Exit ausgesprochen.

**6. (a) Testtag-Protokollpflicht auf das 24.08.-Niveau zurückholen.**
Ein Testtag existiert ausschließlich für das anschließende Review. Mindestumfang: jeder Voll-Check mit seinen Pflichtzeilen, so wie am 24.08. Ohne das ist die Kernfrage jedes Reviews („wurden die Regeln eingehalten?") strukturell unbeantwortbar — siehe Abschnitt 9. Alternativ, falls die volle Protokollierung zu teuer ist: eine maschinelle Mitschrift der Pflichtzeilen in eine separate Datei, statt sie in der Prosa zu verlieren.

**7. (b, braucht Zustimmung) 8b2 um eine Erreichbarkeitsprüfung ergänzen — oder die 7,78:1 ehrlich abwerten.**
8b2 prüft für TP2 „echtes Level + RR≥2:1", aber nicht die Distanz — obwohl 8b1 (aus dem sie hervorging) genau dafür geschaffen wurde. Ein TP2 bei 7,8× ATR ist formal 7,78:1 und praktisch unerreichbar (Abschnitt 5.2). Zwei Varianten: entweder eine reine **Anzeige** (`TP2-Distanz: X,X× ATR — GEMESSEN, KEIN GATE`), damit sich über mehrere Trades zeigt, ob die TP2-RR-Zahl überhaupt Informationsgehalt hat; oder eine echte Schwelle. **Meine Empfehlung: nur die Anzeige, jetzt kein Gate** — wir sind mitten im 15-Trade-Sizing-Fenster, und die TP2-Quote wird dort ohnehin gemessen ([[project_risikomanagement]], D-5). Gleiche Begründung wie gestern bei B1-B3.

**8. (b) Behandlung offener Trades am Tagesende festlegen.**
Status `OFFEN/ZENSIERT`, zählt in Prozess-, nicht in Ergebniskennzahlen; kein Mark-to-Market als „im Plus"; Berichtsformat „n=X, davon Y zensiert". Details und Alternativen in Abschnitt 10.3.

**9. (a) Loop-Kaltstart umdrehen.**
`CronCreate` zuerst, Memory-Studium und Tweet-Fetch **im** bereits tickenden Loop. Kostet nichts, spart zwei Tage in Folge je 20-40 Minuten Marktbeobachtung und rettet das 15:30-16:00-Fenster.

**10. (a) B1/B2/B3-Schattenmessung an einem Testtag mindestens trocken durchlaufen.**
`skipped_setups` hat nach zwei Testtagen 0 Zeilen (Abschnitt 7). Vorschlag: die drei Pflichtzeilen (8a4-Klasse, 7b1b-Schattenmessung) an Testtagen im Protokoll ausgeben, ohne in die DB zu schreiben — dann wird wenigstens geübt und sichtbar, wie oft die Klasse überhaupt feuern würde.

---

## 15. Was ich ausdrücklich NICHT vorschlage

- **Keine neue Pflichtzeile.** Der Tag hat gezeigt, dass mindestens neun bestehende Pflichtzeilen nicht belegt sind und drei nachweislich nicht durchgeführt wurden. Eine zehnte Zeile verbessert nichts — Punkt 14 (Tempo-Bremse) und der #42-Präzedenzfall sprechen direkt dagegen. Vorschläge 1-3 ändern die **Form** bestehender Zeilen, nicht ihre Anzahl.
- **Keine Änderung an Dual-Gate, Q-Score-Schwellen, 8b1-Zonengrenzen oder RR-Floor.** Das 15-Trade-Sizing-Fenster und die verlängerte Phase 3 bis #50 laufen; [[feedback_dont_change_running_system]] und Punkt 14 gelten unverändert. Die Befunde aus Abschnitt 2 sind **Ausführungs**-Befunde, keine Kalibrierungs-Befunde — die Regeln waren richtig, sie wurden nicht angewendet.
- **Keine Neubewertung von #T1 als „Beleg für den Q-Score".** n=1 mit günstigem Ausgang, siehe Abschnitt 3.
- **Keine Umdeutung des 17:00-Nicht-Trades zur bestandenen Disziplinprobe.** Er ist ein Beleg für korrekte Kerzenschluss-Handhabung, nicht für Standhaftigkeit — Abschnitt 7.
- **Keine Aufweichung von 8c, um #T3 nachträglich regelkonform zu machen.** Der Trade war nicht regelkonform. Das ist die Lehre, nicht der Anlass für eine Ausnahme.

---

## 16. Ehrliche Gesamteinordnung

Der Tag liest sich im Protokoll deutlich besser, als er war — und das ist selbst der Befund, nicht ein Vorwurf.

Was gut war, war echt: das Warten auf den zweiten Anlauf über zwei Stunden Solo-Loop, das konsequente Canceln von #T2, das saubere Dual-Gate bei #T3, die erste korrekte Anwendung der heute morgen eingeführten Spike-Zeile, die vorbildliche 7d0-Formulierung. Nichts davon ist geschönt.

Aber alle drei protokollierten Prozessfehler und beide zusätzlich gefundenen Regelverstöße liegen auf derselben Achse: **überall dort, wo eine Zahl hätte gerechnet werden müssen, wurde sie erinnert oder geschätzt — und der Pflichtzeilen-Apparat, der genau das verhindern soll, konnte es nicht sichtbar machen, weil seine Zeilen Urteile transportieren statt Eingaben.** Der Vorzeige-Trade des Tages, #T3 mit 4/4 GRÜN und voller Position, ist davon am stärksten betroffen: Seine Beobachtungsseite war die beste des Tages, seine Rechenseite die schwächste, und nach dem eigenen Regelwerk hätte er entweder halbiert oder gar nicht stattfinden dürfen.

Zur Einordnung, damit das Gewicht stimmt: Der Verstoß bei #T1 hatte keine Ergebnisfolge (der weitere, regelkonforme SL wäre nie berührt worden). Der Verstoß bei #T3 hatte keine *realisierte* Folge, weil der Trade bei Stopp offen war — der Schaden ist hypothetisch. Es ist ein fiktiver Tag ohne einen Euro Risiko. Der Wert liegt vollständig darin, dass ein Fehlermuster, das über Wochen unsichtbar war, an einem Tag dreimal in Folge sichtbar wurde und sich auf eine einzige Ursache zurückführen ließ — und dass die Gegenmaßnahme keine neue Regel ist, sondern eine Formatänderung an drei Zeilen, die es längst gibt.

Und ein letzter Punkt zur Sicherheit: Alles oben ruht auf einem Tag mit drei Trades und einem Protokoll, dem drei Stunden Detail fehlen. Das genügt, um einen Rechenfehler zu belegen — Abschnitt 2 ist Arithmetik und steht unabhängig davon. Es genügt nicht, um über Prozessdisziplin insgesamt zu urteilen. Wo ich das oben getan habe, habe ich es als Einschätzung gekennzeichnet und nicht als Befund.

---

*Levi entscheidet über alle Vorschläge. Bis dahin ist nichts umgesetzt, keine Regel geändert, keine Datei außer dieser angelegt.*
