---
name: project_testtag_analyse_2026-09-08
description: "Opus-Analyse Testtag 08.09.2026 — erster Testtag mit zwei echten (fiktiven) Trades. Zwei von Levi live bemerkte Prozessfehler (1H-Override-Regression, AVWAP-Anker vergessen) plus ein dritter, bisher unbemerkter (AVWAP-Reset nach SL fehlt). Kernbefund darüber hinaus: der komplette Pflichtzeilen-Apparat vom 03.-07.09. wurde in 46/46 Voll-Checks NICHT angewendet, inkl. der bindenden SL-Anker-Vorprüfung. ENTWURF, ungelesen von Levi."
metadata:
  node_type: memory
  type: project
  status: draft
  originSessionId: 56b9f40d-72c3-47d3-a120-9aa3871bcfb6
  modified: 2026-09-08T17:56:55.422Z
---

## Kurzfazit

Der 08.09. ist der erste Testtag der Serie, an dem überhaupt Trades zustande kamen — zwei Stück, und beide durch ein sauberes `gate_check.cjs`-PASS gedeckt. Das ist nach 11 Läufen mit 0 PASS an vier Vortagen ein echter Durchbruch, und die Richtungsanalyse war an beiden Setups korrekt. Gleichzeitig ist es der prozessual schwächste Tag seit dem 27.08.: die beiden von Levi live bemerkten Fehler sind real, aber sie sind nicht die schlimmsten des Tages — der schwerste Befund ist, dass praktisch der gesamte Pflichtzeilen-Apparat, den Fable zwischen dem 03. und 07.09. gebaut hat (Register-Check per Skript, MTF-Frische, Zählstände, Chasing-Status, Volumen-Check, Spike-Ausnahme, 1H-Schatten-Zeile, Levelsuche-Zeile, Gate-Check-Zeile, SL-Anker-Vorprüfung), in **46 von 46 Voll-Checks und an beiden Entries nicht ein einziges Mal** verwendet wurde. Der Tag lief im Format von Ende August.

Bilanz fiktiv: Trade #1 (Short) SL, ca. −17,26 €. Trade #2 (Long) bei Loop-Stopp offen, ~6,5 Pkt unter Entry, SL/TP intakt.

## Was lief gut

**Der Long-Trade ist handwerklich der beste Entry der ganzen Serie.** Nach der Regelkorrektur um 16:03 wurde die Zwei-Beine-Definition ab 17:15 wörtlich richtig angewendet: NAS100-15min-Schluss 29.570,95 > EMA50 29.552,9, QQQ-15min bereits seit dem 17:00-Schluss bestätigt, 1H-Override ausdrücklich am zuletzt geschlossenen NAS100-1H-Bar geprüft (nicht an QQQ, nicht intrabar). Der Entry kam 3 Kerzen nach dem Trigger nach einem Pullback, nicht am Spike-Hoch 29.594,65 — genau das Entry-Timing, das Opus und Fable am 07.09. unabhängig voneinander als den eigentlichen Hebel identifiziert hatten. Q2-Reifegrad 1,01× ATR statt 1,72× beim Short. Zone 1, volle Position, RR 1,051:1 mit register-verifiziertem TP1 (Pivot R1, Abweichung 0 Pkt) und TP2 (Pivot R2). Das ist die Umsetzung, die vier Testtage lang gefehlt hat.

**Die Lehre aus Fehler #2 wurde innerhalb desselben Tages befolgt.** Beim zweiten Entry stand der AVWAP-Ereignis-Anker (`GelNA6`, `in_0=1788881100`) nachweislich VOR dem Positions-Kasten, mit Rücklese-Verifikation und ausdrücklicher Gegenprüfung, dass die Session-Instanz `fQM3K5` unangetastet blieb. Das ist die Reaktionsgeschwindigkeit, die man sehen will.

**Der Wechsel auf den Cron-Job hat funktioniert.** Nach Levis Korrektur um 15:49 lief die Kadenz von 15:51 bis 19:43 minutengenau durch, über fast vier Stunden, ohne eine einzige ausgefallene Minute im Protokoll. 46 Voll-Checks, alle nummeriert und alle mit vollständigem MTF-Block. Verglichen mit dem 04.09. (4 ausgefallene Slots) und dem 01.09. (Nummerndrift ab #31) ist die reine Loop-Disziplin heute tadellos.

**Das Festhalten am Short bis zum SL war regelkonform und ehrlich dokumentiert.** Die Einordnung im Protokoll — "der SL griff technisch im Blindflug bezüglich der Reversal-Kriterien, was regelkonform ist" — ist genau die richtige Klassifizierung nach [[feedback_regime_wechsel]]: Verlust trotz Regeleinhaltung, kein Prozessfehler im Exit.

**Die Eskalation bei 2/4 um 18:30 kam aktiv und von selbst.** Sonnet hat nicht abgewartet, sondern die Verschärfung explizit an Levi gemeldet, mit belegten Einzelkriterien (MACD-H-Wert, EMA50-Bezug, QQQ-Stand, Beschleunigungs-Kriterium) — die Belegpflicht aus Punkt 11 ist damit erfüllt, anders als bei Trade #24 am 22.07.

## Was lief schlecht — Prozessfehler

### P1 — Der 1H-Override wurde erneut falsch angewendet (Rückfall auf eine am 04.09. abgeschlossene Entscheidung)

Der Sachverhalt ist unstrittig: Von VC#1 (15:32) bis VC#6 (16:00) hat das Protokoll durchgehend **fünf** Bestätigungsebenen als Dual-Gate-Kriterien geführt (5min/15min/1H NAS100 + 15min/1H QQQ) und den Trigger explizit daran scheitern lassen, dass "QQQ 1H der letzte Holdout" sei. Die Formulierung in VC#6 — "4 von 5 beobachteten Ebenen bestätigt bärisch" — ist genau die "x von 3/x von 5"-Zählweise, die [[feedback_live_trading]] 7b1 seit dem 03.09. wörtlich für **unzulässig** erklärt. Beide 15min-Beine standen seit dem 15:45-Schluss bestätigt bärisch; der einzige legitime Blocker war der 1H-Override auf Basis des 14-15-Uhr-NAS100-Bars, und der fiel um 16:00.

**Warum ist das trotz dokumentierter Vorentscheidung erneut passiert?** Drei Kandidaten, und ich halte nur einen für tragfähig:

*Kein Wissensproblem.* Die Regel steht an drei Stellen: [[feedback_live_trading]] 7b1 (zwei Spiegelstriche, wörtlich), [[project_1h_kriterium_offene_frage_2026-09-03]] (Status `resolved`), [[project_testtag_analyse_2026-09-04]] F8. Sie ist außerdem am 04.09. lehrbuchmäßig angewendet worden — die Fähigkeit war da.

*Kein Formulierungsproblem.* Der Wortlaut ist eindeutig und nennt sogar den Fehler beim Namen. Wer den Absatz liest, kann ihn nicht falsch verstehen.

*Es ist ein Prüfschritt-Problem — genauer: ein Nicht-Lesen-Problem.* Die Regel greift nur, wenn der Absatz im Trigger-Moment aktiv gelesen wird. Genau das ist heute nicht passiert; gelesen wurde er erst um 16:03 nach Levis Hinweis, und dann sofort richtig. Das deckt sich exakt mit dem Muster, das dieses Projekt schon dreimal diagnostiziert hat und das im Regelwerk selbst benannt ist: **"Fließtext-only-Regeln brechen unter Loop-Bedingungen"** (Punkt 11 Schritt 6, wörtlich). Die 1H-Definition ist bis heute reiner Fließtext — es gibt keine Zeile, kein Feld und keinen Skriptausgang, an dem eine falsche Anwendung auffallen würde. Zum Vergleich: der Entry-Reifegrad, die Register-Frische und die Chasing-Bewertung wurden nach demselben Befund jeweils in ein Skript-Pflichtfeld überführt und sind seitdem nicht mehr ausgefallen.

Es kommt erschwerend hinzu, dass die am 07.09. beschlossene **1H-Schatten-Pflichtzeile** (Abstand Kurs↔1H-EMA50 in Pkt und ATR, 3/4-Blockade-Flag, JSONL-Anhang nach `scripts/oneh_shadow_log.jsonl`) genau diese Ebene bei jedem Voll-Check sichtbar gemacht hätte. Sie wurde heute **null Mal** ausgegeben. Die Maßnahme, die den Fehler mit hoher Wahrscheinlichkeit verhindert hätte, existierte seit 24 Stunden auf dem Papier und war im Loop nicht präsent.

**Kosten:** 8 Minuten Verzug, ein um 36,2 Pkt schlechterer Entry, Zone 2 statt Zone 1 (halbe statt volle Position) — Details und meine Gegenrechnung unten unter "Nachgerechnete Analyse".

### P2 — AVWAP-Ereignis-Anker beim ersten Entry vergessen (18 Minuten)

Unstrittig, im Protokoll offen dokumentiert (Nachtrag 16:24 Uhr), von Levi bemerkt, sauber nachgeholt inkl. Rücklese-Verifikation und Prüfung der Session-Instanz. Für sich genommen die harmloseste der drei AVWAP-Verfehlungen des Tages: der Anker ist ein Konfidenz-Modifikator ohne Gate-Wirkung, und in den 18 Minuten wurde keine Reversal-Entscheidung getroffen, die er beeinflusst hätte.

Die Ursache ist dieselbe wie bei P1 und wird im Regelwerk an derselben Stelle vorweggenommen: Punkt 11 Schritt 6 ordnet ausdrücklich an, dass dieser Schritt zusätzlich als Punkt (12) im Tick-Prompt-Baustein (Punkt 2b) stehen muss, "damit diese Regeln nicht dasselbe Schicksal erleiden wie die übrigen diese Woche gefundenen Fließtext-only-Regeln". Er steht dort — und ist trotzdem ausgefallen, weil der Tick-Prompt-Baustein heute insgesamt nicht in der vorgesehenen Fassung gefahren wurde (siehe P4).

### P3 — Der AVWAP-Reset nach dem SL-Hit fehlt vollständig (bisher von niemandem bemerkt)

Das ist der Fehler, der weder Levi noch Sonnet aufgefallen ist, und er ist gravierender als P2.

Punkt 11 Schritt 4a verlangt seit dem 31.08.: nach JEDEM Positionsschluss — SL-Hit ausdrücklich eingeschlossen — im selben Abarbeitungsschritt wie die Exit-Dokumentation `in_0 = 0` auf der Ereignis-Instanz, plus Rücklese und Pflichtzeile `Ereignis-AVWAP-Reset: in_0=0 ✓ (nach <Exit-Art> <HH:MM>)`. Im Protokoll vom 08.09. kommt das Wort "Reset" **kein einziges Mal** vor. Der Ereignis-Anker des Shorts (`in_0=1788876300`, Entry-Kerze 16:05) blieb nach dem SL-Hit um 16:52 stehen und wurde erst um 17:26 vom neuen Long-Anker überschrieben.

Konsequenz: Die gesamte Setup-Suche zwischen 16:52 und 17:26 — 34 Minuten, sieben Voll-Checks (#16 bis #19 plus Ticks), also genau die Phase, in der das Long-Setup entstand und bewertet wurde — lief gegen eine veraltete, nicht als veraltet markierte AVWAP-Linie. Das ist wörtlich derselbe Vorfall wie am 28.08.2026, für den die Reset-Regel überhaupt erst geschrieben wurde. Ein Rückfall in einen bereits einmal geschlossenen Fehler, zehn Tage später, unentdeckt.

Schadenshöhe heute: gering, weil der Ereignis-AVWAP in der Setup-Suche nicht als Kriterium herangezogen wurde (er ist nur Konfidenz-Modifikator bei offener Position). Aber die Fehlermechanik ist identisch, und sie ist diesmal durch keinen Menschen im Raum abgefangen worden.

### P4 — Der Kernbefund: der Pflichtzeilen-Apparat vom 03.-07.09. war den ganzen Tag abwesend

Ich habe das Protokoll maschinell gegen die aktuell gültigen Pflichtzeilen aus [[feedback_vollcheck_format]] und [[feedback_live_trading]] 7b1 gezählt. Ergebnis (46 Voll-Checks, 2 Entries):

| Pflichtbestandteil | verankert seit | Treffer im Protokoll |
|---|---|---|
| `Register-Check:` mit Skript-Urteil (`register_check.cjs`) | 07.09. (bindende Auflage der 04.09.-Abbruch-Empfehlung) | **0 von 46** (1× Skript erwähnt in VC#4, sonst "Alter X Min, OK" aus dem Gedächtnis) |
| `1H-Schatten:`-Zeile + `oneh_shadow_log.jsonl` | 07.09. | **0** |
| `MTF-Frische:` inkl. Spreizung | 03.09. | **0** |
| `Zählstände:` (Reclaim-Zyklen, Kerzenzähler) | 03.09. | **0** |
| `Chasing-Status (13):` inkl. k/2-Zähler | 04.09. | **0 von 46** (nur einmal als Prosa im 17:26-Gate) |
| `Volumen-Check:` (RVOL, bei JEDEM Check-in mit offener Position) | 26.08. | **0** — bei ca. 190 Ticks mit offener Position |
| `Spike-Ausnahme:` | 25.08. | **0 von 46** |
| `Stale-Check:` (mindestens Negativbestätigung mit Grund) | 31.08. | **0** |
| Format-Zeile mit `Voll-Check #<N> vollständig` | 03.09. | **0 von 46** (Format-Zeile ohne Nummer = laut Regel "Voll-Check nicht durchgeführt") |
| `Levelsuche TP1/TP2:` mit allen 8 Levelarten | 31.08. | **0 von 2 Entries** |
| `Gate-Check ausgeführt: <voller CLI-Aufruf>` | 25.08./27.08. | **0 von 2 Entries** |
| SL-Anker-Vorprüfung (7b1 Schritt 0, `--sl-vorpruefung`) | 07.09., von Levi bestätigt, **bindend** | **0 von 2 Entries** |
| Chartmuster/Fibonacci-Zeile mit Extension-Pflicht (P5) | 04.09. | **0** |

Das ist keine Ansammlung von Kleinigkeiten, sondern ein Formatrückfall auf den Stand von Ende August. Drei Punkte daraus wiegen besonders schwer:

**(a) Die SL-Anker-Vorprüfung wurde an beiden Entries übersprungen.** Sie ist seit dem 07.09. — also seit dem Vortag, Commit `c72a836` — bindender Schritt 0 vor jedem 7b1-Ablauf, ausdrücklich von Levi bestätigt. Beide Trades des Tages wurden ohne sie eröffnet. Beide `--cluster-level none`-Angaben ("keine Wick-Zone vorhanden") sind damit unbelegte Behauptungen statt Skript-Urteile. Das ist die einzige Maßnahme des ganzen 07.09.-Pakets, die überhaupt hätte greifen können — und sie ist bei ihrem ersten Anwendungsfall komplett ausgefallen. Der im 07.09.-Gegencheck bereits als "künftiges Arbeitspaket, keine Eile" vorgemerkte A3-Pflichtfeld-Zwang für Schritt 0 hat damit ab heute Eile.

**(b) Der `Gate-Check ausgeführt:`-Nachweis fehlt an beiden Entries, und beim zweiten fehlt der Aufruf ganz.** Beim Short (16:08) steht der CLI-Aufruf zwar da, aber mit dem Platzhalter `[+ alle A3-Pflichtfelder]` statt der tatsächlichen Werte — die Wörtlich-Zitierpflicht aus 7b1 ist damit verletzt, und keine der elf A3-Messgrößen ist rekonstruierbar. Beim Long (17:26) steht überhaupt kein Aufruf im Protokoll, nur die zusammengefasste Ausgabe. Nach dem Wortlaut der Regel ("Fehlt sie, gilt Schritt 5 als nicht durchgeführt und die Entry-Freigabe als NICHT PASS, unabhängig davon, was die Entry-Freigabe-Zeile behauptet") sind formal **beide Entries nicht freigegeben** gewesen. Ich halte das materiell nicht für einen Regelbruch — das Skript ist erkennbar gelaufen, die Ausgabewerte sind plausibel und konsistent —, aber es ist genau die Lücke, die am 25.08. zu den beiden falschen RR-Werten geführt hat, und sie ist heute wieder offen.

**(c) Der `Volumen-Check:` fehlt bei ca. 190 Ticks mit offener Position.** Das ist nicht nur Formalie: der RVOL-Dämpfer ist das Instrument, das bei der 2/4-Lage um 18:30 hätte belegen müssen, ob die Gegenbewegung volumenmäßig getragen war. Ohne ihn ist die Entscheidung "halten" zwar im Ergebnis richtig gewesen, aber ohne die vom Regelwerk verlangte Beweisgrundlage getroffen worden.

Die Ursache ist mit hoher Wahrscheinlichkeit dieselbe wie bei P1/P2 und liegt im Format selbst: Der Voll-Check besteht inzwischen aus rund einem Dutzend Pflichtzeilen, die per Hand aus mehreren Regelwerksdateien zusammengesetzt werden müssen. Unter Loop-Druck fällt nicht eine Zeile aus, sondern es setzt sich das ältere, eingeübte Kurzformat durch. Das ist exakt die Diagnose, aus der am 07.09. der Vorschlag **S1 `vollcheck.cjs`** entstanden ist — und der ist bis heute nicht umgesetzt.

### P5 — Zwei falsche Zahlen im Protokoll (Fehlerklasse 28.08./04.09.)

1. **SL-Hit-Einordnung, 16:52:** "Tief 29.392,95 nur ~13 Pkt über TP1 29.350". Tatsächlich sind das **42,95 Pkt**. Der Trade war zu keinem Zeitpunkt nahe an TP1 — die MFE von 41,8 Pkt entsprach knapp der Hälfte der benötigten 84,8 Pkt. Die Aussage lässt den Short deutlich knapper aussehen, als er war, und färbt damit die Bewertung "hätte fast funktioniert".
2. **Tagesbilanz beim Loop-Stopp:** "VC8-fiktiv (Long) → offen, aktuell ca. +Buchgewinn nahe Entry". Bei Kurs 29.579,45 und Entry 29.585,95 ist das ein **Buchverlust von 6,50 Pkt**, kein Buchgewinn. Der Klammerzusatz ("Kurs marginal unter Entry") widerspricht der Aussage im selben Satz.

Beide sind Rechen-/Formulierungsfehler ohne Vertuschungsabsicht, die Rohdaten stehen jeweils daneben. Aber es ist dieselbe Klasse wie F1 vom 04.09. und die Screenshot-Bilanz vom 28.08.: Bilanzzahlen werden geschrieben, ohne gegen den eigenen Protokolltext nachgerechnet zu werden. Das ist jetzt der dritte Vorfall.

### P6 — Kein Faktenprotokoll-Abschluss

Der Tag endet mit "Loop gestoppt — 19:44 Uhr" plus einer vierzeiligen Bilanz. Der nach [[feedback_tagesabschluss]] vorgeschriebene Faktenprotokoll-Abschluss (6 Pflichtpunkte) fehlt, `protokoll_bilanz.cjs` wurde nicht aufgerufen, das Frontmatter steht weiter auf `status: in_progress`. Das ist teilweise entschuldbar — Levi hat den Loop abrupt beendet und die Position blieb offen —, aber genau dafür gibt es keine Regel, siehe P7.

### P7 — Loop-Stopp mit offener Position, ohne definierten Prozess

Die Position VC8-fiktiv bleibt "offen und unbeaufsichtigt bis zur nächsten Session". Es gibt im Regelwerk keinen definierten Ablauf dafür: kein Übergabekasten, keine Entscheidung, ob die Position beim Stopp glattgestellt, auf Broker-SL/TP übergeben oder bewusst offen gelassen wird, keine Regel für die Wiederaufnahme. Das ist **kein neuer Fall** — am 25.08.2026 endete der Testtag mit "#T3 solo bei Loop-Stopp offen", identische Konstellation, ohne dass daraus je eine Regel wurde. An einem Echtgeldtag ist das die gefährlichste offene Stelle des Systems: eine Position, für die niemand definiert hat, wer sie überwacht.

## Nachgerechnete Analyse: Was wäre bei korrektem 16:00-Entry passiert?

Sonnets Vorabrechnung im Notizzettel-Abschnitt habe ich vollständig gegengeprüft. Vorbemerkung: Levi hatte Sonnet ausdrücklich angewiesen, diese Bewertung **nicht** selbst zu finalisieren ("Du nicht, das macht Opus später"). Der Abschnitt enthält trotzdem ein fertiges "Fazit für Opus" mit Schlussurteil. Das ist kein schwerer Verstoß — die Rohdaten sind vollständig und korrekt aufbereitet, was den eigentlichen Auftrag erfüllt —, aber die Schlussfolgerung war nicht bestellt.

**Was rechnerisch stimmt:**

- Trigger-Zeitpunkt 16:00:00 ✓. Beide 15min-Beine seit dem 15:45-Schluss bestätigt bärisch, der 1H-Override fiel mit dem 16:00-Schluss (29.470,95 unter EMA50 ~29.530). Ein 2/2-Trigger-Ereignis lag damit vor.
- SL 29.533 ✓. Struktur-Anker nach P4/3a wäre das Bounce-Hoch 29.505,95 (15:59) gewesen → 39,05 Pkt Abstand, unter dem 8c-Floor von 1,5×ATR = 62,25 Pkt. `SL = MAX(Struktur, Floor)` ergibt korrekt 29.533,2.
- TP1 = Pivot S2 29.405,85, Distanz 65,1 Pkt, **RR 1,046:1** ✓ (Sonnet: 1,05:1), **1,569× ATR → Zone 1, volle Position** ✓.
- TP2 Fib-Ext 1,272 @ 29.260,18 → RR 3,386:1 ✓, 8b2 (RR≥2) erfüllt.
- **Zusatzbefund, den Sonnet nicht erhoben hat und der die These stützt:** Der Impuls-Reifegrad hätte bei 16:00 nur (29.505,95−29.470,95)/41,5 = **0,84× ATR** betragen statt der 1,72× beim tatsächlichen Entry. Die Reifegrad-Eskalation, die um 16:08 ausgelöst wurde, wäre gar nicht erst angefallen. Der frühere Entry war nicht nur rechnerisch besser, er war auch qualitativ ein anderer Setup-Typ.
- **TP1-Treffer: ja, aber nicht aus dem angegebenen Grund.** Sonnet stützt sich auf das 16:02-Tief 29.405,05 — das liegt **0,8 Punkte** unter TP1 29.405,85. Ein Fill mit 0,8 Pkt Marge ist bei Zertifikatsspread nicht sicher; die Aussage "TP1-Treffer innerhalb von ~2 Minuten" ist so nicht belastbar. Belastbar ist dagegen das 16:11-Tief **29.392,95**, das TP1 mit 12,9 Pkt Marge klar durchschlägt. TP1 wäre also mit hoher Sicherheit erreicht worden, nur eher um 16:11 als um 16:02.
- Der SL 29.533 wurde in der gesamten Folgebewegung nie bedroht (höchster Bounce bis zum SL-Hit des echten Trades: 29.517,65 um 16:52, und da wäre der Rest der Position längst auf Breakeven gestanden). ✓

**Wo ich Sonnets Darstellung deutlich einschränke — der entscheidende Punkt:**

Die Rechnung unterstellt einen Entry **exakt zum 16:00-Schlusskurs 29.470,95**. Das RR-Gate ist hier aber extrem entry-empfindlich. Bei fixem SL-Floor (62,25 Pkt) und TP1 = S2 29.405,85 verlangt RR≥1 einen Short-Entry von mindestens:

`E ≥ 29.405,85 + 62,25 = 29.468,10`

Der 16:00-Schluss lag bei 29.470,95 — **2,85 Punkte über der Gate-Grenze**. Und um 16:02 stand der Kurs bereits bei 29.406,65, mit einem Tief von 29.405,05. Das brauchbare Entry-Fenster war also real **etwa 60 bis 90 Sekunden breit**. Ein Entry bei 29.460 hätte RR 0,87 ergeben → FAIL. Ein Entry ab 16:02 hätte für TP1 ein tieferes Level gebraucht, das im Register nicht vorhanden war (das Rundzahl-Band endete bei 29.350, was dann Zone 2 bedeutet — also exakt die Situation, in der der echte Trade um 16:08 landete).

**Meine Einschätzung:** Der frühere Entry wäre besser gewesen, ja — und zwar auf allen vier Dimensionen (Preis, Sizing, Reifegrad, Ergebnis). Aber die Aussage "der 8-Minuten-Verzug hat den Verlust verursacht" ist zu stark. Präziser ist: **Der 8-Minuten-Verzug hat ein etwa einminütiges Zeitfenster verpasst, in dem als einzigem an diesem Nachmittag ein Zone-1-Short mit RR≥1 möglich war.** Ob dieses Fenster ohne den Regelfehler tatsächlich genutzt worden wäre, ist offen: der vollständige 7b1-Ablauf (Schritt 0 SL-Vorprüfung, Levelsuche-Zeile, `gate_check.cjs`, Entry) dauert im Loop mehrere Minuten. Ohne vorberechnete Zahlen wäre das Fenster mit hoher Wahrscheinlichkeit auch bei korrekter 1H-Anwendung zugefallen.

Genau das macht diesen Tag zum stärksten empirischen Argument für **S8 (Setup-Karte / `gate_check --vorschau`)** aus der 07.09.-Liste: SL-Anker, TP1-Kandidat, Entry-Fenster und Zone hätten ab 15:45 — als beide 15min-Beine standen — vollständig vorberechnet bereitliegen können. Der Trigger um 16:00 hätte dann nur noch "Kurs im Fenster? ja → Order" bedeutet. Das ist die einzige Maßnahme, die ein 60-Sekunden-Fenster überhaupt handhabbar macht, und sie verschiebt kein Risiko: beide Beine bleiben Pflicht, es werden nur Zahlen früher sichtbar.

**€-Größenordnung (grobe Schätzung, gleiche Annahmen wie im Protokoll: ~6× Hebel, keine Gebühren/Spread):** hypothetisch volle Position (~2.250 €), TP1-Teilverkauf 50 % bei +65,1 Pkt ≈ **+15 €**, Rest auf Breakeven → Gesamtergebnis leicht positiv. Tatsächlich: halbe Position, −75,25 Pkt = **−17,26 €**. Differenz rund **32 €** bzw. gut 2 R an einem Tag mit 15.000 € Kapital. Als Einzelzahl ist das klein; als wiederkehrender Effekt (Zone 2 statt Zone 1 bedeutet dauerhaft halbe Positionsgröße) ist es der relevanteste Hebel, den dieser Tag zeigt.

## Bewertung des offenen Long-Trades (VC8-fiktiv) und des Whipsaw-Managements

**Das Ergebnis war richtig.** Wer um 18:30 oder um 18:59 (Wick 29.521,25, 4 Pkt über SL) ausgestiegen wäre, hätte mit rund −60 Pkt geschlossen und die vollständige Erholung ab 19:01 (bis 29.610) verpasst. Die Position stand bei Loop-Stopp mit 2/2 Dual-Gate, 0/4 Reversal-Kriterien und 54 Pkt SL-Abstand da. Das Aushalten war die richtige Entscheidung.

**Der Weg dorthin war regelseitig dünner, als das Protokoll darstellt.** Drei Punkte:

*Erstens ist die Behauptung "kein automatischer Exit-Trigger nach Regelwerk (Schwelle bisher nicht klar definiert für 2/4)" (VC#32) nicht korrekt.* Punkt 11 sagt wörtlich: "Erst wenn mindestens 2-3 dieser Kriterien gleichzeitig erfüllt sind, gilt das als echtes Reversal-Signal" und in der How-to-apply-Zeile: "Sobald 2-3 gleichzeitig kippen, das explizit und sofort als Handlungsaufforderung melden (nicht nur als normalen Status), **inklusive Vorschlag für Exit-Preis**". Die Schwelle 2/4 ist also definiert — sie löst keinen automatischen Exit aus (richtig erkannt), verlangt aber einen Exit-Preis-Vorschlag an Levi. Der ist nicht erfolgt; eskaliert wurde die Lage, ohne die vom Regelwerk verlangte Handlungsoption zu formulieren. Die Unsicherheit im Protokoll ("neue, ernstere Stufe ohne direkten Präzedenzfall") entstand, weil auf einen Präzedenzfall statt auf den Regeltext geschaut wurde.

*Zweitens fehlte der Dämpfer, der die Entscheidung getragen hätte.* Der RVOL-Volumen-Dämpfer aus Punkt 11 sagt: 2/4 + Gegenbewegung unter 50 % des Volumens der Hauptbewegung → **kein Dreh**, Beobachtung. Genau das war die Lage: eine volumenschwache Whipsaw-Konsolidierung. Mit der Pflichtzeile `Volumen-Check:` hätte die Entscheidung "halten" nicht auf "kein Präzedenzfall" gestützt werden müssen, sondern auf einen gemessenen Wert. Die Zeile fehlte den ganzen Tag (P4c). Das richtige Ergebnis wurde ohne den vorgesehenen Beweis erreicht.

*Drittens wurden die beiden Zwischenmechanismen nie geprüft.* Zwischen "alles gut" und "Punkt-11-Dreh" liegen im Regelwerk zwei Stufen, die heute an keiner Stelle des Tages auftauchen: **Punkt 7c** (Warnsignal → aktive Positions-Verteidigung, zahlenbasierte niedrigere Schwelle für Teilgewinn/SL-Nachzug) und **Punkt 12** (Stall-Exit, ausdrücklich für "dem Kurs geht die Puste aus", ohne Reversal und ohne Umkehrmuster). Die Formulierung "kein Regelkriterium für Nachziehen erfüllt" (VC#12) ist deshalb nicht belegt — sie wäre erst belegt, wenn 7c und 12 geprüft und verneint worden wären. Das betrifft **beide** Trades: Der Short hatte um 16:11 eine MFE von 41,8 Pkt und ging danach in eine 40-minütige Konsolidierung mit fünfmal fallendem MACD-Hist — ein Lehrbuchfall für die Stall-Prüfung nach Punkt 12, die nie stattfand, bevor der Trade den vollen SL nahm.

**Meine Antwort auf die Auftragsfrage:** Nein, es braucht keinen neuen Schwellenwert für "2/4". Der existiert, und die Auslegung "kein automatischer Exit bei 2/4" ist richtig. Was fehlt, ist die **sichtbare Prüfung der drei bestehenden Stufen** (7c → 12 → 11) bei jedem Check-in mit offener Position, mit belegten Werten statt Prosa. Eine neue Regel wäre hier Overfitting an einem einzigen Whipsaw; die vorhandenen drei Regeln überhaupt anzuwenden ist die eigentliche Aufgabe.

## Technische Lücken / TODO-Liste für Fable

Priorisiert. P1 = vor dem nächsten Testtag, P2 = vor dem ersten Echtgeld-Trade, P3 = wenn Kapazität da ist. Nichts davon ist von mir umgesetzt — Umsetzung entscheidet Levi.

**1. [P1] `feedback_loop_tick_kadenz.md` auf `CronCreate` korrigieren.** Die Datei empfiehlt aktuell `ScheduleWakeup` mit `delaySeconds: 60` als Lösung. Das ist nachweislich falsch: real kamen 89-93 s Drift zustande (Dispatch-Overhead), Levi hat um 15:49 auf den bewährten minütlichen Cron-Job (`*/1 * * * *`) umgestellt, und der lief danach von 15:51 bis 19:43 wandzeitgenau. Die Datei muss den Cron-Job als Standard-Startmechanik für den Live-/Testtag-Loop festschreiben, `ScheduleWakeup` als nicht geeignet benennen (mit der gemessenen Drift als Begründung) und den Aufräumschritt `CronDelete` beim Faktenprotokoll-Abschluss enthalten. Solange die Datei unkorrigiert ist, produziert sie den Fehler beim nächsten Loop-Start erneut.

**2. [P1] `vollcheck.cjs` (= S1 aus [[project_todo_sicher_geschwindigkeit_effektivitaet_opus_2026-09-07]]) endlich bauen.** Nach dem heutigen Befund ist das keine Geschwindigkeitsoptimierung mehr, sondern die einzige belastbare Antwort auf P4: 13 Pflichtbestandteile in 46 Voll-Checks komplett ausgefallen, weil sie von Hand aus vier Regelwerksdateien komponiert werden müssen. Ein Aufruf muss den kompletten Block erzeugen — Format-Zeile mit Nummer, MTF-Frische inkl. Spreizung, `Register-Check:` mit dem wörtlichen `register_check.cjs`-Urteil, Zählstände, Chasing-Status mit k/2, 1H-Schatten-Zeile inkl. JSONL-Anhang, Stale-Check-Negativbestätigung, Spike-Ausnahme, bei offener Position zusätzlich `Volumen-Check:`. Auflage unverändert wie in S1 formuliert: **kein Parameter darf einen Default haben** — ein fehlender Wert muss als sichtbare Lücke erscheinen, nie als stiller Platzhalter.

**3. [P1] SL-Anker-Vorprüfung (7b1 Schritt 0) technisch erzwingen.** Sie ist seit dem 07.09. bindend und wurde bei ihrem allerersten Anwendungsfall 0 von 2 Mal ausgeführt. Konkret: `gate_check.cjs` im Live-Modus mit Exit 1 abbrechen lassen, wenn kein `--sl-vorpruefung-urteil "<wörtliche URTEIL-Zeile>"` (oder ein Referenz-Zeitstempel auf den Vorprüfungslauf) übergeben wurde — dieselbe A3-Mechanik wie bei den Messfeldern, inklusive `--grund-`-Ausweg mit Pflichtausgabe. Das war im 07.09.-Gegencheck bereits als "künftiges Arbeitspaket, keine Eile" vermerkt; der heutige Tag hebt die Eile-Einstufung auf.

**4. [P1] 1H-Override maschinell prüfbar machen statt nur als Fließtext.** Zwei Teile: (a) Die am 07.09. beschlossene 1H-Schatten-Pflichtzeile in `vollcheck.cjs` (Punkt 2) tatsächlich implementieren — sie hätte P1 heute mit hoher Wahrscheinlichkeit verhindert. (b) `gate_check.cjs` um zwei Pflichtfelder erweitern: `--override-1h-close <Schlusskurs des zuletzt GESCHLOSSENEN NAS100-1H-Bars>` und `--override-1h-ema50 <Wert>`, aus denen das Skript eine eigene Ausgabezeile erzeugt (`1H-Override (NAS100, letzter geschlossener Bar HH:MM): Close X vs. EMA50 Y → steht der Richtung entgegen: ja/nein`). Wichtig: **kein neues Gate**, nur Anzeige und Zwang zur Angabe der richtigen Bezugsgrößen. Ein Feld, das ausdrücklich "NAS100" und "letzter geschlossener Bar" im Namen trägt, macht die QQQ-1H-/Intrabar-Verwechslung strukturell schwer.

**5. [P1] `position_tick.cjs` (= S2) bauen, erweitert um die Exit-Stufen-Prüfung.** Der ursprüngliche S2-Umfang (SL/TP-Abstände, €/Punkt, R-Multiple, MFE, Stall-Zähler) bleibt; heute kommt begründet dazu: automatische `Volumen-Check:`-Zeile aus dem RVOL-Read, laufender Reversal-Kriterien-Zähler n/4 mit Einzelbelegen, und je eine Zeile für **Punkt 7c** und **Punkt 12** (Stall), damit "kein Kriterium für Nachziehen erfüllt" ein geprüftes Ergebnis wird statt einer Behauptung. Rund 190 Positions-Ticks an einem Tag von Hand zu schreiben ist der größte Einzelposten des Arbeitsaufwands und gleichzeitig die Stelle, an der heute die meisten Pflichtzeilen ausgefallen sind.

**6. [P2] AVWAP-Anker und -Reset an die Trade-Skripte koppeln.** Drei AVWAP-Verfehlungen an einem Tag (P2: Anker vergessen; P3: Reset nach SL komplett ausgefallen; beide Male dieselbe Fehlerklasse wie am 28.08.). Vorschlag: `add_trade.cjs` (auch im `--dry-run`) verlangt beim Trade-Abschluss ein Feld `--avwap-reset-bestaetigt <in_0-Rücklesewert>` und bricht ohne es ab; analog beim Entry ein Feld für den gesetzten Anker-Timestamp. Damit hängt der Anker-Lebenszyklus an einem Skript, das ohnehin bei jedem Entry und jedem Exit läuft, statt an einer Fließtext-Regel in Punkt 11.

**7. [P2] Prozess für "Loop-Stopp bei offener Position" definieren.** Zum zweiten Mal (25.08., 08.09.) endet ein Testtag mit einer offenen, unbeaufsichtigten Position und ohne Regel dafür. Nötig ist ein kurzer, verbindlicher Übergabeblock: Position glattstellen / an Broker-SL-TP übergeben / bewusst offen lassen (mit wem-überwacht-Angabe), aktueller Stand als Kasten, AVWAP-Ereignis-Anker-Status, und eine Wiederaufnahme-Checkliste für die Folgesession. Das ist die riskanteste offene Stelle für den Echtgeld-Start.

**8. [P2] Setup-Karte / `gate_check --vorschau` (= S8).** Begründung ist jetzt nicht mehr theoretisch: das einzige RR≥1-Zone-1-Fenster des Nachmittags war rund 60-90 Sekunden breit (siehe Nachrechnung). Ohne vorberechneten SL-Anker, TP1-Kandidat und Entry-Fenster ist ein solches Fenster im Loop nicht bedienbar — auch nicht ohne Regelfehler. Auflagen wie in S8 formuliert: beide Beine bleiben Pflicht, keine Order früher, Ausgabe nach `setup_karte.json`.

**9. [P2] `--dual-gate-qqq-abstand-atr`: ATR nicht auf dem QQQ-Pane, sondern rechnen.** Der Ausweichweg über `--grund-` wurde heute erneut genutzt und ist damit — wie schon am 04.09. in 4/4 Aufrufen — zum Dauerzustand geworden; eine Ausnahme, die immer greift, ist ein nicht implementiertes Feld. Ich empfehle **nicht** den Weg über einen zusätzlichen Chart-Indikator (das war Z9 und ist aus gutem Grund zurückgestellt: `layout_switch` hat schon einmal fast das NAS100-Layout überschrieben, RVOL überlebt Neustarts ohnehin nicht, und jede Layout-Änderung erzeugt eine neue Sessionstart-Routine). Der risikofreie Weg ist, ATR(14) auf QQQ-15min **aus der ohnehin geholten `data_get_ohlcv`-Ausgabe zu berechnen** — im Skript, deterministisch, ohne einen einzigen Klick am Chart. Damit ist Z9 ohne Layout-Risiko erledigt. Der QQQ-Pane hat zwar Slots frei (5/10), aber es gibt keinen Grund, das Risiko einzugehen, wenn die Zahl rechenbar ist.

**10. [P2] Zahlenbilanz maschinell gegenrechnen.** Dritter Vorfall derselben Klasse (28.08. Screenshot-Bilanz, 04.09. drei falsche Zahlen, heute zwei). `protokoll_bilanz.cjs` existiert bereits — nötig ist, dass sein Aufruf im Faktenprotokoll-Abschluss verpflichtend ist und dass die Bilanzzahlen ausschließlich aus seiner Ausgabe übernommen werden, nie aus dem Gedächtnis. Zusätzlich sollte er die zwei heute falschen Größen mit abdecken: Abstand MFE↔TP1 und Buchgewinn/-verlust einer offenen Position (Vorzeichen).

**11. [P3] Faktenprotokoll-Abschluss auch bei abruptem Loop-Stopp erzwingen.** Heute fehlt er komplett, Frontmatter steht auf `in_progress`. Sinnvoll gekoppelt an Punkt 7: der Übergabeblock IST der Abschluss, wenn der Tag mit offener Position endet.

**12. [P3] Trigger-Kandidaten-Zähler (= S9).** Unverändert offen und unverändert der fehlende Nenner: Auch heute lässt sich nicht sagen, wie viele Beinahe-Trigger es gab. Bei jetzt zwei tatsächlichen Trades wird die Frage "wie viele hätten es sein können" beantwortbar und damit erstmals nützlich.

## Abschluss: Ist das System bereit für den nächsten Schritt?

**Nein — aber aus einem anderen Grund als an den Vortagen, und der Grund ist behebbar.**

Das inhaltliche Bild hat sich klar verbessert. Nach 11 Gate-Läufen mit 0 PASS an vier Tagen gab es heute zwei PASS und zwei Trades, beide mit register-verifizierten Leveln, der zweite mit einwandfreiem Entry-Timing, korrekter Regelanwendung und Zone 1. Die am 07.09. formulierte These — der Hebel liegt am Entry-Timing und an der Leveldichte, nicht an den Schwellen — hat sich heute bestätigt, ohne dass eine einzige Schwelle angefasst wurde. Der Long-Trade ist der Beleg, dass das System handelbare Setups produziert.

Der Blocker liegt woanders: **Die Regeln, die zwischen dem 03. und 07.09. gebaut wurden, sind im Live-Loop nicht angekommen.** 13 Pflichtbestandteile, 0 Treffer in 46 Voll-Checks, darunter die am Vortag als bindend bestätigte SL-Anker-Vorprüfung und die als bindende Auflage der letzten Abbruch-Empfehlung eingeführte Register-Check-Zeile. Das ist kein Nachlässigkeitsproblem eines einzelnen Tages, sondern ein strukturelles Kapazitätsproblem: der Voll-Check ist per Handkomposition nicht mehr vollständig ausführbar. Solange das so ist, ist jede weitere Regel, die Fable schreibt, ein Papier-Fortschritt — und die drei Fehler des Tages (1H-Regression, AVWAP-Anker, AVWAP-Reset) sind alle drei genau an dieser Stelle entstanden.

Zwei Dinge sind mir dabei wichtig festzuhalten, damit die Bewertung nicht in die falsche Richtung kippt:

Erstens: Die beiden Fehler, die Levi live bemerkt hat, waren **nicht** die schwersten. Der ausgefallene AVWAP-Reset und der komplett abwesende Pflichtzeilen-Apparat sind gravierender und wären ohne diese Analyse unentdeckt geblieben. Das ist ein Argument für den Meilenstein-Check als eigenständige Rolle — und gleichzeitig eine Warnung: an einem Solo-Tag ohne Levi im Raum wären auch die beiden bemerkten Fehler durchgelaufen.

Zweitens: Ich sehe keinen Hinweis auf Vertuschung oder stilles Abschalten. Beide Fehler wurden nach Levis Hinweis sofort, vollständig und selbstkritisch protokolliert, der Notizzettel für diese Analyse ist inhaltlich korrekt aufbereitet (bis auf die zu starke Schlussfolgerung), und die zwei falschen Zahlen stehen jeweils direkt neben den korrekten Rohdaten. Das ist die Fehlerklasse "Rechnen ohne Nachrechnen", nicht "Beschönigen".

`Abbruch-Empfehlung: EINGESCHRÄNKT (Pflichtzeilen-Vollständigkeit — vor dem ersten 7b1-Order-Ablauf des nächsten Testtags muss EIN vollständiger Voll-Check im aktuell gültigen Format nachweislich im Protokoll stehen, inklusive Register-Check-Skripturteil, MTF-Frische, Zählstände, Chasing-Status, 1H-Schatten-Zeile und Format-Zeile mit Voll-Check-Nummer; und die SL-Anker-Vorprüfung nach 7b1 Schritt 0 muss vor beiden Gate-Aufrufen wörtlich protokolliert sein. Am 08.09. waren 13 Pflichtbestandteile in 46 von 46 Voll-Checks und an 2 von 2 Entries abwesend.)`

Kein ABBRUCH: es gibt keinen still deaktivierten Hard-Gate-Pfad, keine korrumpierte Statusdatei, kein Regelverstoß bei der Order-Ausführung selbst, und die inhaltliche Trade-Qualität war die beste der Serie. Kein KEIN EINWAND: die zuletzt erteilte Auflage (Register-Frischeprüfung als eigene, gerechnete Protokollzeile vor dem ersten Order-Ablauf) ist an ihrem ersten realen Anwendungstag nicht erfüllt worden, und die am Vortag als bindend bestätigte SL-Anker-Vorprüfung ist bei beiden Entries ausgefallen. Zwei bindende Auflagen in Folge nicht erfüllt ist genau der Fall, für den die mittlere Stufe da ist.

**Reihenfolge, die ich Levi empfehle:** Erst Punkte 1-5 der TODO-Liste (die vier P1-Punkte plus `position_tick.cjs`), dann ein Testtag, der ausschließlich prüft, ob der Pflichtzeilen-Apparat jetzt durchläuft. Keine neuen Regeln in dieser Runde — das System hat kein Regeldefizit, es hat ein Ausführungsdefizit.

## Offene Punkte für das Gespräch (Auflistung, keine Handlungsanweisung)

1. Die zwei Zahlenkorrekturen aus P5 (MFE-Abstand 42,95 statt ~13 Pkt; offener Long = Buchverlust 6,50 Pkt, kein Buchgewinn) — betrifft den Speicherstand, nicht das Regelwerk.
2. Ob der Notizzettel-Abschnitt in [[testtag/testtag_2026-09-08]] um meine Einschränkung ergänzt oder als Sonnets Rohdaten-Fassung stehen gelassen wird.
3. Ob die Punkt-11-Formulierung "Vorschlag für Exit-Preis" bei 2/4 verschärft, präzisiert oder unverändert bleiben soll — heute wurde eskaliert, aber ohne Preisvorschlag.
4. Ob Punkt 7c und Punkt 12 (Stall) künftig als eigene Pflichtzeilen bei offener Position geführt werden (Teil von TODO 5) oder ob das als Überregulierung gilt.
5. Der Status der Position VC8-fiktiv — bleibt sie für die nächste Session als fortlaufender Trade bestehen, oder wird der Tag rechnerisch mit dem Kurs beim Loop-Stopp abgeschlossen?
