---
name: project-studie-volumenprofil-opus-2026-08-31
description: "Opus-Studie 31.08.2026 zu drei Levi-Fragen nach dem TradingView-Hilfeartikel 'Volumenprofil-Indikatoren': (1) Volume Profile integrieren? NEIN, vierte Ablehnung, entscheidend ist die QQQ→NAS100-Level-Transfersperre, nicht die Tick-Volumen-Frage. (2) Indikatoren auf Indikatoren? NEIN für jetzt, Schattenmess-Kanal ist voll — eine einzige Ausnahme (Flip-Count) benannt. (3) Renko/Kagi/Line-Break/P&F intraday? NEIN, ATR(14) degeneriert auf preisgetriebenen Charts zur Konstante und reißt 8b1/8c/8d/GC-5 gleichzeitig"
metadata: 
  node_type: memory
  type: project
  modified: 2026-08-31T12:56:03.610Z
  originSessionId: e40d2c87-d383-4377-a725-334d9fa865f3
---

# Studie: Volumenprofil, Meta-Indikatoren und preisgetriebene Charttypen

**Auftrag:** Levi hat den TradingView-Hilfeartikel „Volumenprofil-Indikatoren: Grundkonzepte" gelesen und drei Fragen gestellt. Diese Studie ist reine Entscheidungsvorlage — nichts wurde umgesetzt, kein Chart angefasst, keine andere Memory-Datei geändert.

**Datenstand-Vorbehalt:** TradingView lief zum Zeitpunkt dieser Studie nicht (`tv_health_check` → „CDP connection failed"). Alle Chart-Aussagen unten stützen sich auf die dokumentierten, mehrfach live verifizierten Befunde in `feedback_chart_layout.md` und `feedback_chartanalyse.md`, nicht auf eine frische Live-Messung. An den zwei Stellen, wo das für die Entscheidung relevant ist, steht ein expliziter Verifikationsauftrag.

---

## Kurzfazit

**Volumenprofil: Nein** — und zwar nicht wegen der Tick-Volumen-Frage aus dem Artikel, sondern aus einem härteren, systemeigenen Grund: Das einzige Instrument mit echtem Volumen ist QQQ, und für QQQ gilt seit dem 15.07.2026 die feste Regel, dass dessen Preisniveaus **nie** als 1:1-Level auf den NAS100-Chart übertragen werden dürfen. Ein POC/VAH/VAL ist aber ausschließlich ein exaktes Preisniveau — das Produkt dieses Werkzeugs ist genau das, was die bestehende Regel verbietet zu transferieren. Dazu kommt: Das ist bereits die vierte unabhängige Ablehnung desselben Vorschlags (31.07., 22.08., 24.08.), und der Artikel liefert kein neues Gegenargument, sondern bestätigt das alte.

**Indikatoren auf Indikatoren: Nein für jetzt, mit einer benannten Ausnahme.** Das System enthält bereits vier Meta-Konstrukte (MACD-Histogramm, ADX, Bollinger Bands, VWAP-σ-Bänder) und nutzt sie korrekt — die Kategorie ist also nicht falsch, sondern schon bedient. Ein *weiteres* wäre methodisch vertretbar, scheitert aber am Zustand des Systems: Der „GEMESSEN, KEIN GATE"-Schattenmess-Kanal in `gate_check.cjs` führt aktuell acht unausgewertete Rohwerte, alle warten auf dieselbe Trade-15-Auswertung, und n liegt weit darunter. Ein neunter Wert verdünnt diese Auswertung, statt Information hinzuzufügen.

**Renko/Kagi/Line-Break/P&F: Nein, auch nicht als Zusatzansicht.** Der Konflikt ist nicht „passt schlecht", sondern mechanisch: Auf einem preisgetriebenen Chart hat jede Kerze per Konstruktion (nahezu) dieselbe Größe — **ATR(14) degeneriert damit zu einer Konstante**. ATR ist aber die gemeinsame Recheneinheit von 8b1 (Drei-Zonen-TP-Filter), 8c (SL-Floor), 8c2 (Cluster-Zuschlag), 8d (Regime-Gate) und dem GC-5-Konsistenz-Gate. Diese fünf Regeln würden nicht „ungenauer", sie würden **still gegen eine Zahl ohne Informationsgehalt rechnen und trotzdem PASS melden**. Dazu der zweite Bruch: Ein Renko-Ziegel hat keinen Kerzenschluss-Zeitpunkt — das zeitgetaktete Dual-Gate hätte nichts zu prüfen.

**Verbindende Beobachtung über alle drei Fragen:** Alle drei Vorschläge liefern zusätzliche *Level-* oder *Bestätigungs-Information* auf der Entry-Seite. Das ist nachweislich nicht die schwache Stelle dieses Systems. Die dokumentierte Diagnose vom 24.08.2026 lautet: Setup-Erkennung stark, Exit-/Risikoseite strukturell schwach, Phase-3-EV nach dem C-1-Fix negativ. Keiner der drei Vorschläge adressiert das.

---

## Frage 1 — Volumenprofil: ja/nein/mit Einschränkung

### 1.0 Vorbemerkung: Das ist die vierte Prüfung desselben Vorschlags

Bevor inhaltlich argumentiert wird, gehört der Vorgeschichte-Befund auf den Tisch, weil er die Beweislast verschiebt:

| Datum | Wer | Ergebnis |
|---|---|---|
| 31.07.2026 | Fable (`project_review_4h_und_indikator_ranking_2026-07-31`) | Abgelehnt. Begründung: „Volume Profile/Order Flow=10 unterstellt echten Transaktionsdatenzugang — NAS100 ist CFD ohne echtes Volumen." |
| 22.08.2026 | Fable (`project_indikator_upgrade_diskussion_22_08_2026`) | Explizit unter „Nicht empfohlen": „Volume Profile/VPOC (planabhängig + hohe Interpretationslast)". |
| 24.08.2026 | Opus (`project_studie_bessere_trades_2026-08-24`, Abschn. 208) | „Explizit nicht umgesetzt (Opus geprüft und abgelehnt, unverändert gültig): … Volume Profile/VPOC." |
| 31.08.2026 | diese Studie | siehe unten |

Die 31.07.-Entscheidung enthält zusätzlich eine ausdrückliche Wiederaufroll-Sperre: *„Diese Entscheidung nicht bei jeder neuen Quelle von vorne aufrollen, außer es gibt neue konkrete Evidenz (z.B. echten Order-Flow-Datenzugang oder einen belegten Fall, in dem RSI im System versagt hat)."*

**Ist der TradingView-Hilfeartikel neue Evidenz?** Nein. Er ist eine Herstellerbeschreibung des Features, kein Wirksamkeitsnachweis, und er enthält keinen Trade aus der eigenen Historie. Inhaltlich liefert er sogar das Gegenteil: Er sagt selbst, dass Index-CFDs nur Tick-Volumen bekommen, und er stuft das Werkzeug selbst als „reaktiv, nicht prädiktiv" ein. Der Artikel ist damit ein Argument **für** die bestehende Ablehnung, nicht dagegen.

Das ist kein Grund, die Frage nicht ernsthaft zu beantworten — Levi hat sie gestellt, sie wird beantwortet. Aber es ist ein Grund, danach eine Sperre zu setzen (siehe Abschnitt 1.7).

### 1.1 Die Tick-Volumen-Frage — und warum sie hier nicht die entscheidende ist

Der Artikel sagt: Für Indizes/Forex/Krypto-CFDs liefert TradingView „Tick-Volumen" (Anzahl Kursaktualisierungen), nicht echtes Handelsvolumen. Recherche zum Stand der Praxis:

- Tick-Volumen korreliert in der Praxis **überraschend gut** mit echtem Volumen. Für Major-FX-Paare werden Korrelationen von ca. 0,85–0,90 auf H1-Bars gegen echtes CME-Futures-Volumen berichtet, eine ältere Untersuchung (Marney 2011) kam auf ~90% Übereinstimmung gegen aggregiertes ECN-Volumen.
- Die praktische Folge daraus, in der Literatur konsistent formuliert: Die **Form** eines Tick-Volumen-Profils (Lage des POC, Ausdehnung der Value Area, wo HVN/LVN sitzen) ähnelt meist dem echten Profil. Die **absoluten Zahlen** sind ein Proxy und variieren je Broker/Feed — ein Tick kann ein 1-Lot- oder ein 1000-Lot-Trade sein, und zwei Broker erzeugen für dasselbe Instrument nicht dasselbe Profil.

**Faires Zwischenfazit:** Wer Tick-Volumen pauschal als „wertlos" abtut, argumentiert schwächer als die Datenlage hergibt. Für ein reines Formsignal (wo hat der Preis lange verweilt) ist Tick-Volumen ein akzeptierter Proxy. Ich stufe dieses Argument allein also **nicht** als ausreichend für eine Ablehnung ein.

**Aber:** In diesem konkreten Setup ist die Frage eine Ebene darunter bereits entschieden. `FOREXCOM:NAS100` liefert in dieser TradingView-Instanz **überhaupt kein Volumen** — nicht „nur Tick-Volumen", sondern gar keines. Das ist in `feedback_chart_layout.md` seit dem 23.07.2026 dokumentiert und mehrfach nach Neustarts reproduziert: *„`Volume` und `VWAP` liefern deshalb keine Werte (roter Fehler-Indikator im Vol-Pane, VWAP zeigt keine Zahl/Linie) — kein temporärer Bug, tritt nach jedem Neustart wieder auf."* Genau deshalb wurde Bollinger Bands als Volume-Ersatz eingesetzt und QQQ überhaupt erst als zweite Pane eingeführt.

Ein Volumenprofil auf NAS100 wäre also nicht „etwas ungenau" — es hätte auf dem Hauptinstrument keine Datenbasis. Der Artikel beschreibt eine Kategorie (Index-CFD mit Tick-Volumen), in die dieser spezifische Feed offenbar nicht einmal fällt.

> **Verifikationsauftrag, falls diese Entscheidung je neu aufgerollt wird:** Es ist theoretisch möglich, dass TradingViews Volumenprofil-Studien intern auf einen Tick-Zähler zurückfallen, auch wenn die reguläre `Volume`-Studie auf demselben Symbol scheitert. Das ist **nicht** verifiziert und war mit ausgeschalteter TradingView-Instanz auch nicht prüfbar. Wer das prüfen will: eine Volumenprofil-Studie testweise auf NAS100 legen und schauen, ob überhaupt ein Histogramm erscheint. Solange das nicht geprüft ist, gilt die Beweislast bei der Idee, nicht bei der Ablehnung.

### 1.2 Der eigentliche Killer: QQQ liefert Level, die nicht übertragen werden dürfen

Selbst wenn man NAS100 aufgibt und das Profil dort baut, wo es Daten hat — auf QQQ —, kollidiert es mit einer Regel, die seit 15.07.2026 unverändert gilt und in zwei Dateien steht:

> *„QQQ-Preise (~$-Bereich) und VWAP-Level gelten nur als Richtungs-Referenz (Kurs über/unter VWAP), nie als 1:1-Level für den NAS100-Chart — wie schon bei NQ1!."* (`feedback_chart_layout.md`)

Das ist die entscheidende Kollision, und sie ist strukturell, nicht graduell:

- QQQ-**Richtungs**information ist übertragbar: „Kurs über/unter EMA50", „Kurs über/unter VWAP", „Volumen-Spike ja/nein". Das ist genau, wofür QQQ im Dual-Gate benutzt wird — als binäres Ja/Nein, nie als Zahl.
- Ein Volumenprofil produziert aber **ausschließlich** Preisniveaus: POC, VAH, VAL, HVN-Zonen, LVN-Zonen. Deren gesamter Nutzen liegt in ihrer exakten Lage. Ein POC, den man nur als „irgendwo da oben" verwenden darf, ist kein POC.

Man kann das nicht durch Umrechnung reparieren: QQQ und NAS100 laufen zwar synchron in der Richtung, aber ihr Verhältnis ist kein fixer Faktor (unterschiedliche Basket-Zusammensetzung — QQQ = Nasdaq-100 Trust mit eigenem Tracking, NAS100-CFD = Broker-Preis mit eigenem Spread und Roll-Verhalten). Ein QQQ-POC bei $713,40 lässt sich nicht in „NAS100 29.412" umrechnen, ohne eine Präzision zu erfinden, die die Daten nicht hergeben. Genau diese Art erfundener Präzision hat das Regelwerk an anderer Stelle bereits als Fehlermuster identifiziert (Kopfrechnung statt `gate_check.cjs`, 25.08.2026).

**Damit ist die Frage im Kern entschieden.** Alles Folgende sind zusätzliche, unabhängige Gründe — jeder einzelne würde für sich schon reichen.

### 1.3 Zusatzproblem: Das QQQ-Volumen ist für ein Profil schlechter geeignet als für einen Spike-Check

QQQ läuft über den Cboe-One-Feed, der laut eigener Dokumentation nur **~10-20% des konsolidierten Volumens** zeigt. Für die bisherige Nutzung ist das ausdrücklich als ausreichend eingestuft: *„für die relative Spike-Bewertung ausreichend, absolute Zahlen nicht als Marktgröße interpretieren."*

Für ein Volumenprofil ist derselbe Datenmangel aber qualitativ schwerwiegender, nicht nur quantitativ:

- Ein **Spike-Check** ist eine Aussage über die *Zeitachse*: „ist jetzt mehr los als vorhin". Dafür genügt eine konsistente Teilstichprobe, solange sie über den Tag ähnlich groß bleibt.
- Ein **Profil** ist eine Aussage über die *Preisachse*: „auf welchem Preisniveau wurde am meisten gehandelt". Das ist eine Verteilungsaussage, und dafür muss die Stichprobe über die Preisniveaus hinweg unverzerrt sein — nicht nur konstant über die Zeit.

Genau diese Unverzerrtheit ist bei Cboe One strukturell nicht gegeben. QQQ ist Nasdaq-gelistet; die **Eröffnungs- und Schlussauktion** — die beiden mit Abstand größten Einzelpreis-Volumenkonzentrationen des Handelstages — werden auf der Nasdaq-Primary ausgeführt, nicht auf den vier Cboe-Börsen (BZX, BYX, EDGA, EDGX), die den Cboe-One-Feed bilden. Ein aus diesem Feed gebautes Tagesprofil hätte also ausgerechnet die volumenstärksten Preisknoten des Tages systematisch **nicht** drin. Das verschiebt nicht nur die Höhe der Balken, es kann den POC auf ein anderes Preisniveau legen.

(Diese Feed-Abdeckungs-Aussage ist eine strukturelle Ableitung aus der Marktmechanik, keine Live-Messung — sie ist aber leicht prüfbar, indem man das QQQ-Volumen um 15:30 MESZ mit dem konsolidierten Tagesvolumen aus einer zweiten Quelle vergleicht.)

### 1.4 Redundanz: Die Funktion ist bereits fünffach belegt

8b1/8b2 verlangen TP-Kandidaten aus „echten technischen Levels", 8b2 zählt die zulässigen Quellen sogar auf: *„Pivot (7a1a), Vortageshoch/-tief (PDH/PDL), VWAP-Band, Box-Kante o.ä."*. 8c2 braucht getestete Cluster-Zonen für den SL. Was das System dafür bereits hat:

| Vorhandene Quelle | Was sie liefert | Verhältnis zum Volumenprofil |
|---|---|---|
| Pivots 7a1a (PP/R1/R2/S1/S2, täglich berechnet + gezeichnet) | Statische Tages-S/R | Gleiche Funktion, mit Puffer-Regel (7a1) und Nachzieh-Automatik |
| Fibonacci 0,382/0,5/0,618/0,786 + Ext. 1,272/1,618 | Retracement- und Zielzonen | Gleiche Funktion |
| PDH/PDL, Session-Extreme, mehrfach getestete Zonen (Checkliste Pkt. 2) | Historisch bestätigte Zonen | **Das ist die HVN-Idee, nur preis- statt volumenbasiert** |
| Bollinger Bands + EMA50 als dynamischer S/R | Volatilitäts-/Trendzonen | Gleiche Funktion |
| **VWAP + σ-Band 1 + σ-Band 2 auf QQQ** | Volumengewichteter Mittelwert + Streuung | **Das ist funktional bereits eine Value Area** |

Die letzte Zeile ist der wichtige Punkt und wird meist übersehen: Eine Value Area ist definiert als der Preisbereich, in dem ~70% des Volumens gehandelt wurde — also ein volumengewichtetes Zentrum plus ein Streuungsmaß. Ein VWAP mit ±1σ ist genau das, auf demselben Instrument, mit echtem (wenn auch partiellem) Volumen, bereits live gesetzt und bereits in zwei Regeln verankert (8a4 Bedingung 4, Punkt 11 „Sechster Kontextfaktor", 8e seit 24.08. als Widerstandszonen-Ausprägung). Ein Volumenprofil würde die Value-Area-Idee nicht einführen — es würde sie ein zweites Mal einführen, mit einer anderen Rechenmethode und ohne Regel, welche der beiden im Konfliktfall gewinnt.

**Und der Zusatznutzen zeigt in die falsche Richtung.** Das durch Trade #38 dokumentierte Problem war nicht *zu wenige* TP-Kandidaten — es war, dass ein zu weit entfernter Kandidat akzeptiert wurde, weil die RR-Zahl formal passte. Die Antwort darauf (8b1, Drei-Zonen-Filter) war, die Kandidatenliste **härter zu filtern**. Mehr Kandidatenquellen hinzuzufügen läuft dieser Korrektur direkt entgegen — und es gibt keine Regel, die einen HVN gegen einen Pivot rangiert. Man würde also Auswahlfreiheit vergrößern in einem System, dessen letzte drei Regelwerksänderungen genau diese Auswahlfreiheit eingeschränkt haben.

### 1.5 Chart-Realestate und Ablaufkosten

Die Slot-Frage ist **nicht** das Problem: Nach dem Plan-Upgrade vom 24.08.2026 stehen 10 Slots pro Chart zur Verfügung, NAS100 nutzt 6, QQQ 4. Es wäre Platz.

Die realen Kosten liegen woanders:

1. **Horizontaler Platz.** Ein Volumenprofil-Histogramm liegt auf der rechten Preisachse — exakt dort, wo im 2v-Split die aktuelle Kursaktion und die per `draw_shape` gezeichneten Pivot-Linien (7a1a Schritt 3) sichtbar sein müssen. Das QQQ-Pane hat bei 2v ohnehin nur die halbe Höhe. Das Problem ist damit dasselbe wie beim VIX-Pane am 02.07.2026, das aus genau diesem Grund entfernt wurde.
2. **Punkt 14 (Pflichtzeilen-Wachstum).** Fables Vorbehalt vom 22.08. gilt unverändert: *„Jeder neue Indikator ist auch eine neue Zahl unter Zeitdruck."* Bei Trade #42 wurde ein Hinweis nachweislich zu spät gelesen. Ein Profil liefert nicht eine Zahl, sondern mindestens fünf (POC, VAH, VAL, plus HVN/LVN-Zonen) — und im Gegensatz zu ADX oder RVOL sind das keine ablesbaren Skalare, sondern eine Grafik, die interpretiert werden muss. Das ist die „hohe Interpretationslast", die Fable am 22.08. genannt hat, konkret gemacht.
3. **Persistenz-Aufwand.** Zwei der bisherigen Indikatoren überleben Neustarts nicht zuverlässig (RVOL gar nicht, VWAP-Band-2 einmal nicht). Jede weitere Studie ist ein weiterer Session-Start-Prüfschritt in einer Checkliste, die dafür bereits eine eigene Regel gebraucht hat.

### 1.6 Strategie-Fit: Die Beispielstrategie ist ein konkurrierendes Setup

Die im Artikel beschriebene Beispielstrategie — Tageseröffnung gegen Vortagesprofil, Rückkehr zum POC als Einstieg — ist eine **Mean-Reversion-Logik**. Das bestehende System ist ein Trendfolge-System: EMA50-Reclaim/-Break, bestätigt durch zwei Instrumente, mit expliziter Regel „Instrumente/Indikatoren widersprechen sich → neutral, kein Trade".

Ein POC-Reversion-Signal wäre damit kein Zusatzfilter, sondern eine **zweite, unabhängige Richtungsmeinung**. Für genau diese Kategorie gibt es bereits einen Präzedenzfall mit klarer Entscheidung: Beim ADX/DMI-Einbau am 24.08.2026 wurde ausdrücklich festgelegt, nur die ADX-Linie zu verwerten und **+DI/−DI nicht als Richtungssignal** zu nutzen — Begründung wörtlich: *„würde eine dritte, konkurrierende Richtungsmeinung neben Dual-Gate/MACD schaffen."* Ein POC-Reversionssignal fällt in exakt dieselbe Kategorie und müsste konsequenterweise dieselbe Antwort bekommen.

Ergänzend: Der Artikel stuft das Werkzeug selbst als **reaktiv, nicht prädiktiv** ein. Das System hat reaktive Kontextgrößen bereits (ATR, Box-Breite, Trend-Effizienz, ADX). Der Grenznutzen einer weiteren reaktiven Größe ist niedrig.

### 1.7 Empfehlung Frage 1

**Nein — keine Integration des Volumenprofils, in keiner Form, weder auf NAS100 noch auf QQQ.**

Begründungskette, in absteigender Härte:
1. Das Produkt eines Volumenprofils (exakte Preisniveaus) ist auf dem einzigen Instrument mit Volumen (QQQ) durch eine seit 15.07.2026 bestehende Regel vom Transfer auf NAS100 ausgeschlossen. **Das allein entscheidet die Frage.**
2. NAS100 hat kein Volumen — dort existiert die Datenbasis gar nicht erst.
3. Die Value-Area-Funktion ist durch VWAP ± σ-Bänder auf demselben Instrument bereits belegt, die HVN-Funktion durch mehrfach getestete Zonen/Pivots/Fibs.
4. Mehr TP-Kandidatenquellen laufen der 8b1-Korrekturrichtung (Kandidaten härter filtern) entgegen.
5. Die POC-Reversionslogik wäre eine konkurrierende Richtungsmeinung — dieselbe Kategorie, die bei +DI/−DI bereits abgelehnt wurde.
6. Der Cboe-One-Teilfeed ist für Verteilungsaussagen über die Preisachse strukturell verzerrt (fehlende Auktionsprints), auch wenn er für Spike-Checks taugt.

**Formale Empfehlung zum Vorgang:** Diese Frage ist jetzt viermal unabhängig beantwortet worden, jedes Mal negativ, jedes Mal mit im Kern denselben Gründen. Ich schlage vor, sie als **abgeschlossen** zu markieren mit einer expliziten Wiederaufroll-Bedingung, damit nicht bei der nächsten gelesenen Quelle Runde fünf beginnt. Die Bedingung sollte lauten: *Neu geprüft wird nur, wenn (a) NAS100 selbst nachweislich echtes Handelsvolumen liefert, ODER (b) das Hauptinstrument auf ein Instrument mit echtem Börsenvolumen wechselt (z.B. MNQ-Futures im Zug des IBKR-Themas).* Beides sind konkrete, überprüfbare Ereignisse — kein weiterer gelesener Artikel qualifiziert.

**Was stattdessen zuerst getan werden sollte (die ehrliche Alternative):** Das System hat bereits ein volumenbasiertes Werkzeug installiert, das die Frage „wo wurde Wert akzeptiert" auf echtem Volumen beantwortet — den **Anchored VWAP (Remote)** auf QQQ. Es läuft seit dem 26.08.2026 durchgehend im Fallback (`in_0 = 0`, Anker = Session-Start), und ein echter „tagesentscheidender" Anker ist **noch nie gesetzt worden**, obwohl die Nutzungsregel dafür seit dem 24.08. geschrieben ist. Bevor ein neues Volumen-Werkzeug hinzukommt, sollte das bereits installierte einmal tatsächlich benutzt werden. Das kostet null Slots, null neue Regeln und null Interpretationslast — nur einen bewussten Klick beim Session-Start.

---

## Frage 2 — „Indikatoren auf Indikatoren"

### 2.1 Was damit gemeint ist, und welche Varianten es gibt

Gemeint ist ein Indikator, dessen Eingabeserie nicht der Preis ist, sondern die Ausgabeserie eines anderen Indikators. Die gängigen Varianten, sortiert nach dem, was sie methodisch tun:

| Variante | Beispiel | Was sie tut |
|---|---|---|
| **A — Glättung** | Signallinie auf MACD; EMA auf RSI; MA auf ADX | Rauschen aus einer Indikatorserie nehmen, Kreuzung als Signal |
| **B — Normalisierung** | StochRSI (Stochastik auf RSI) | Einen Oszillator in seine eigene jüngste Spanne skalieren |
| **C — Streuung/Bänder** | Bollinger Bands auf RSI; Keltner auf einem Oszillator | Statistische Ausreißer *innerhalb* der Indikatorserie markieren |
| **D — Ableitung** | Steigung/ROC von RSI oder MACD-H; Flip-Count über N Kerzen | Die *Veränderungsrate* statt des Niveaus messen |
| **E — Anderer Input** | MFI (RSI-Logik auf Preis×Volumen); CVD-basierte Oszillatoren | Kein Meta-Indikator im engeren Sinn — anderer Rohdatenstrom |

### 2.2 Methodische Bewertung: welche Kategorie echte Information liefert

**A (Glättung): meist nur eine verzögerte Kopie.** Ein gleitender Durchschnitt auf ADX enthält per Konstruktion keine Information, die nicht schon in ADX steckt — er entfernt Information (hochfrequente Anteile) und fügt Verzögerung hinzu. Das ist ein Trade: weniger Fehlsignale gegen späteres Erkennen. Der Sonderfall ist die *Kreuzung* zwischen geglätteter und ungeglätteter Linie — die ist tatsächlich ein neues Ereignis (sie markiert einen Vorzeichenwechsel der Steigung), aber sie ist damit nur eine schlecht kalibrierte Variante von Kategorie D.

**B (Normalisierung): schlechte Passung für dieses System.** StochRSI ist der Klassiker dieser Kategorie und **verstärkt** Signale statt sie zu filtern — es ist bekannt dafür, in Seitwärtsphasen deutlich mehr Kreuzungen zu erzeugen als RSI selbst. Das dokumentierte Hauptproblem dieses Systems ist Chop (~32% aller Tage laut der 121-Tage-Statistik vom 20.08.), und das dokumentierte Fehlermuster ist „Chop erkannt, aber ignoriert". Ein Indikator, der in Chop *mehr* Signale erzeugt, arbeitet gegen die einzige Regelgruppe, die dieses System am dringendsten braucht. Fable hat Stochastic/Williams %R am 22.08. mit genau diesem Argument abgelehnt („könnte Chop-Fehlsignale sogar verschlimmern") — StochRSI ist dieselbe Familie, eine Ebene höher gestapelt.

**C (Bänder auf Oszillator): statistisch fragwürdig.** Bollinger Bänder setzen implizit eine annähernd normalverteilte, unbeschränkte Serie voraus. RSI ist auf [0,100] beschränkt und in der Nähe der Grenzen systematisch schief verteilt. Bänder um RSI komprimieren dort mechanisch, ohne dass das etwas über den Markt aussagt — man misst eine Eigenschaft der Formel, nicht des Preises.

**D (Ableitung): die einzige Kategorie mit echtem Zusatzgehalt.** Die Steigung einer Serie ist mathematisch nicht in ihrem Niveau enthalten — „RSI bei 55" und „RSI bei 55 und seit drei Kerzen fallend" sind unterschiedliche Zustände. Wichtig: **Dieses System nutzt diese Kategorie bereits und stützt sich stark darauf.** Das MACD-**Histogramm** ist MACD minus Signallinie, also die Differenz zweier abgeleiteter Serien — ein zweistufiges Konstrukt — und das Regelwerk beobachtet ausdrücklich seinen *Vorzeichenwechsel*, nicht seinen Wert. Genauso ist ADX selbst bereits ein Meta-Indikator (geglättetes DX, das aus geglätteten DM/TR gebildet wird), und Bollinger Bands sind Kategorie C auf dem Preis.

**Zwischenfazit zur Methodik:** „Indikatoren auf Indikatoren" ist keine per se unsaubere Idee — dieses System *besteht* bereits zu großen Teilen daraus (MACD-H, ADX, BB, VWAP-σ) und wendet sie korrekt an. Die Kategorie ist also nicht das Problem. Die Frage ist nur, ob eine **weitere** Schicht Information hinzufügt oder nur Parameter.

### 2.3 Passung zur Rohwert-Pflicht-Architektur von `gate_check.cjs`

Hier liegt der eigentliche Entscheidungsgrund, und er ist nicht methodisch, sondern zustandsbezogen.

`gate_check.cjs` ist sauber zweigeteilt (verifiziert am Skript, 950 Zeilen, Stand 31.08.2026):
- **Acht harte Gates** mit Dreizustandslogik `PASS | FAIL | UNKNOWN` (`geometryGate`, `rrGate`, `slFloorGate`, `tpRealismGate`, `zoneRrConsistencyGate`, `tp2RealismGate`, `clusterGate`, `schockTierGate`) — jeder mit einer klar benannten Regel im Memory als Quelle.
- **Ein Schattenmess-Kanal**, dessen Werte im Output explizit mit `(roh, kein Gate)` markiert werden und die nur mitgeschrieben, nie ausgewertet werden.

Diese Trennung ist die architektonische Stärke des Systems. Ein Meta-Indikator müsste in einen der beiden Kanäle:

- **Als Gate:** ausgeschlossen. Kein Wert darf zum Gate werden, bevor er über die vorgeschriebenen 10-15 Fälle validiert ist — das ist an vier Stellen als Review-Pflicht verankert (7a1, 7a1b, 8b1, 8b2). Ein neuer Meta-Indikator hat n=0.
- **Als Schattenmessung:** methodisch korrekt, aber der Kanal ist **voll**. Aktuell mitgeführt und alle noch unausgewertet: Trend-Effizienz, ADX(14), Dual-Gate-QQQ-EMA50(15min)-Abstand, Dual-Gate-QQQ-EMA50(5min)-Abstand, Dual-Gate-NAS100-Q2-Budget, QQQ-VWAP-Band1-Abstand, Kerzen-seit-Gegenseite (NAS100/QQQ), Pivot/Rundzahl-Distanz, Impuls-Reifegrad-ATR, dazu Q-Score und die 8b1-Schritt-5-Vorschau.

Das sind rund zehn Rohwerte, die **alle auf dieselbe Auswertung nach 15 Trades warten** — und der Zähler steht bei ~#43 mit den jüngsten Schattenmessungen erst seit 24.-27.08.2026 aktiv, also bei praktisch n≈0-2 echten Trades. Ein elfter Wert verbessert diese Auswertung nicht; er verschlechtert sie, weil bei fester Stichprobengröße mehr geprüfte Kandidaten die Wahrscheinlichkeit erhöhen, dass irgendeiner davon zufällig signifikant aussieht. Das ist ein statistisches Problem, kein Geschmacksproblem.

Dazu kommt der Punkt-14-Aspekt: Ein Meta-Indikator hat eine **zweistufige Herkunftskette**. Wenn er der Basisgröße widerspricht (geglätteter ADX steigt, roher ADX fällt), gibt es keine Regel, welche gewinnt — und diese Regel müsste unter Zeitdruck im Kopf existieren. Genau dieses Muster hat das Regelwerk bei +DI/−DI bereits abgelehnt.

### 2.4 Empfehlung Frage 2

**Nein — keine zusätzliche Meta-Indikator-Schicht, weder auf dem Chart noch in `gate_check.cjs`, bis die bestehende Schattenmessung nach 15 Trades ausgewertet ist.**

Begründung in einem Satz: Die Kategorie ist methodisch in Ordnung und im System bereits vierfach vertreten (MACD-H, ADX, BB, VWAP-σ) — aber der Kanal, in den ein neuer Wert gehören würde, führt bereits zehn unausgewertete Rohwerte, und ein elfter verdünnt die anstehende Auswertung, statt Information hinzuzufügen. Das ist exakt das Muster, gegen das die Tempo-Bremse aus Punkt 14 geschrieben wurde.

**Die eine Ausnahme, für später vorgemerkt — falls Levi nach der Trade-15-Auswertung genau einen haben will:** Ein **Flip-Count** (Kategorie D) über die letzten 4 abgeschlossenen 5-Min-Kerzen: wie oft haben RSI-50-Linie und MACD-Histogramm-Vorzeichen in diesem Fenster gewechselt. Warum ausgerechnet der:

- Er übersetzt die Chop-Definition aus Punkt 7 (*„RSI und/oder MACD-Histogram kippen innerhalb weniger (2-4) 5-Min-Kerzen mehrfach zwischen bullisch/bärisch hin und her"*) **wörtlich** in eine Zahl. Aktuell wird genau dieses Kriterium per Augenmaß gezählt — dieselbe Lücke, die ATR bei der „Rausch-Kerze" (23.07.) und ADX bei der Trendstärke (24.08.) geschlossen haben.
- ADX schließt diese Lücke **nicht**: ADX misst Trendstärke, nicht Kippfrequenz. Ein Markt kann bei ADX 18 sowohl ruhig-seitwärts als auch hektisch-flippend sein — für die Chop-Regel sind das zwei völlig verschiedene Zustände mit derselben ADX-Zahl.
- Er kostet **null Indikator-Slots und null Chart-Fläche**: Er wird aus RSI- und MACD-H-Werten berechnet, die der Loop ohnehin bei jedem 5-Min-Kerzenschluss abliest, und würde als reiner Rohwert in `gate_check.cjs` mitlaufen — dieselbe Bauart wie `--trend-effizienz` und `--adx`.
- Er hat eine klare Falsifikationsbedingung: Wenn er nicht mit dem korreliert, was das Regelwerk am selben Tag als Chop klassifiziert hat, ist er nutzlos und wird wieder gestrichen.

**Aber nicht jetzt.** Zuerst die bestehenden zehn Rohwerte auswerten. Erst wenn diese Auswertung steht und der Kanal leer ist, ist das ein sinnvoller nächster Kandidat — und dann als Einzelwert, nicht als Paket.

---

## Frage 3 — Renko, Kagi, Line-Break, Point & Figure für Intraday

Hier ist die Antwort am eindeutigsten, und sie hängt nicht an Geschmack.

### 3.1 Der Trigger-Konflikt: Es gibt keinen Kerzenschluss

Die gesamte Entry-Mechanik ist zeitgetaktet und an bestätigte Kerzenschlüsse gebunden:
- Loop auf `CronCreate * * * * *` (jede Minute), Voll-Check alle 5 Minuten.
- Dual-Gate (Punkt 7b): bestätigter Kerzenschluss auf NAS100-**5min** und QQQ-**15min** über/unter der jeweiligen EMA50.
- Pflichtzeile „Kerze geschlossen" (Punkt 7d0), Chop-Auflösung explizit über *„3+ tatsächlich abgeschlossene Kerzenschlüsse"* mit der ausdrücklichen Klarstellung, dass *„die aktuell offene, noch laufende Kerze nicht mitzählt"*.
- Der Kerzenraster-Fix (28.08.2026) und die Trigger-Moment-Bindung sind beide Präzisierungen genau dieser Zeitachse.

Auf einem Renko-/Kagi-/Line-Break-/P&F-Chart entsteht ein neues Element nach einer **Preisbewegung**, nicht nach Zeitablauf. Daraus folgen zwei konkrete Bruchstellen:

1. **Ruhige Phase:** In einer engen Range bildet sich über eine Stunde kein einziger neuer Ziegel. Der Loop läuft weiter im Minutentakt, hat aber nichts zu prüfen — die Frage „hat die letzte Kerze über der EMA50 geschlossen?" ist nicht mit „ja" oder „nein" beantwortbar, sondern mit „es gibt keine letzte Kerze seit dem letzten Mal". Das Dual-Gate hätte keinen definierten Zustand.
2. **Impulsphase:** In einer scharfen Bewegung entstehen fünf Ziegel innerhalb einer Minute. Der Trigger würde innerhalb eines einzigen 1-Min-Ticks mehrfach feuern — und der Kerzenraster-Fix, der genau das Zuordnungsproblem lösen sollte, hätte kein Raster mehr, an dem er ausrichten kann.

Beide Fälle sind nicht durch eine Regelanpassung heilbar. Die Dual-Gate-Logik ist auf zwei *unterschiedlichen* Zeitebenen (5min/15min) definiert, und diese Asymmetrie ist bewusst gesetzt (*„QQQ ist die langsamere, unterstützende Bestätigungsebene"*, 28.07.2026). Auf preisgetriebenen Charts gibt es keine Zeitebenen, also auch kein „langsamer" — die Asymmetrie, die das ganze Dual-Gate-Konzept trägt, wäre nicht mehr definierbar.

### 3.2 Der härtere Bruch: ATR(14) degeneriert zur Konstante

Das ist der Punkt, den ich für entscheidend halte, und er ist nicht offensichtlich.

Ein Renko-Chart besteht per Definition aus Ziegeln **identischer Höhe** (die Box-Größe). Kagi- und Line-Break-Elemente sind ebenfalls durch Preisschwellen definiert, P&F-Kästchen ebenso. Ein ATR(14), der über 14 solcher Elemente rechnet, ergibt damit **näherungsweise die Box-Größe selbst** — eine Konstante, die nichts über die aktuelle Marktvolatilität aussagt, sondern nur die eigene Chart-Einstellung zurückliest.

Was in diesem System an ATR(14) hängt:

| Regel | Nutzung von ATR(14) | Folge bei konstantem ATR |
|---|---|---|
| 8c SL-Mindestdistanz | SL ≥ 1,5× ATR (normal) / 2,5-3× ATR (Schock) | Floor wird zur fixen Punktzahl, unabhängig von der Marktlage |
| 8b1 Drei-Zonen-TP-Filter | Zone 1 ≤2× ATR, Zone 2 bis 3× ATR, Zone 3 darüber | Zonengrenzen werden statisch — der gesamte Realismus-Filter misst nichts mehr |
| 8c2 SL-Cluster-Zuschlag | +0,5× ATR Puffer | Puffer wird beliebig |
| 8d Regime-Gate, Kriterium 1 | Tagesrange / ATR(14)D, Schwelle >2× | Schock-Tag-Erkennung bricht — und darüber auch 8b1a (Schock-Tier = No-Trade) |
| GC-5 `zoneRrConsistencyGate` | Prüft die Identität RR ≤ 2/Floor-Multiplikator | Prüft eine Identität zwischen zwei Konstanten — meldet immer PASS |
| 7a1a Pivot-Nachziehen | Annäherung ≈1-2× ATR | Nachzieh-Automatik verliert ihren Auslöser |

**Das ist der eigentliche Grund für ein klares Nein.** Nicht dass diese Regeln fehlschlagen würden — sie würden **weiterlaufen und PASS melden**, weil `gate_check.cjs` nicht prüfen kann, ob der übergebene `--atr`-Wert von einem zeit- oder einem preisbasierten Chart stammt. Es gibt keine Fehlermeldung, keinen roten Indikator, kein UNKNOWN. Genau diese Klasse von Fehler — eine Zahl, die formal vorliegt, aber inhaltlich nichts misst — hat das System in den letzten zwei Wochen zweimal getroffen (die zwei unterschiedlichen ATR-Ablesungen 5min/15min am 25.08., der 0/3-Zähler beim Regime-Gate am 28.08.). Beide Male war das Ergebnis eine Kette von Regeln, die scheinbar geprüft wurde und tatsächlich nicht.

Dasselbe gilt in abgeschwächter Form für **alle anderen Indikatoren**: RSI(14) auf Renko ist RSI über 14 Ziegel, nicht über 14 Zeiteinheiten — je nach Marktphase können das 20 Minuten oder 4 Stunden sein. Dieselbe RSI-Zahl bedeutet an zwei Punkten des Tages Unterschiedliches. Die gesamte Chop-Definition aus Punkt 7 (RSI/MACD-Kippen „innerhalb weniger 5-Min-Kerzen") verliert ihren Bezugsrahmen.

### 3.3 Volumenverzerrung — was der Artikel selbst sagt

Der Artikel warnt ausdrücklich vor „atypischen Charts" (Heikin Ashi, Renko, Line Break, Kagi, P&F, Range): Die Bar-Daten sind synthetisch, kein echter Preis, was die Volumendaten verzerrt. Das konkrete Beispiel: Eine Tageskerze wird zu N Renko-Ziegeln, das Tagesvolumen wird durch N geteilt und jedem Ziegel zugerechnet — was laut Artikel „zu ungenauen Prognosen führen kann".

Für dieses System heißt das konkret: Auf einem Renko-QQQ-Chart wären **Volume, VWAP, Anchored VWAP inklusive beider σ-Bänder und RVOL** allesamt auf umverteiltem Pseudo-Volumen gerechnet. Das trifft die QQQ-Pane an ihrer einzigen Existenzberechtigung — sie ist ausschließlich deshalb da, weil NAS100 kein Volumen hat. Ein Renko-QQQ wäre eine Pane, die den einen Job, für den sie eingerichtet wurde, nicht mehr erfüllt.

Bemerkenswert: Der Artikel verbindet damit Frage 1 und Frage 3 — die Kombination „Volumenprofil auf Renko", die auf den ersten Blick wie die Summe zweier Verbesserungen aussieht, ist laut der Quelle selbst die schlechteste Variante von beidem.

### 3.4 Ein weiteres, praktisches Problem: Reproduzierbarkeit

TradingViews Renko kann mit ATR-basierter Box-Größe betrieben werden. In dieser Variante ändert sich die Box-Größe mit dem ATR — und damit wird die **gesamte historische Ziegelfolge neu berechnet**, wenn sich der ATR ändert. Der Chart von gestern sieht heute anders aus.

Für dieses System ist das ein eigenständiger Ausschlussgrund, unabhängig von allem oben: Das komplette Faktenprotokoll-, Tagesabschluss- und Testtag-Analyse-Verfahren beruht darauf, dass ein am Vortag abgelesener Wert später reproduzierbar ist. Die Zitierpflicht aus `feedback_memory_pflege.md` und die Rekonstruktionen in den Opus-Testtag-Reviews (z.B. die Kerzen-Zeitstempel-Prüfung bei Trade #21) wären auf einem repaintenden Chart nicht durchführbar. Bei fixer Box-Größe entfällt dieses spezielle Problem — die Punkte 3.1 bis 3.3 bleiben davon unberührt.

### 3.5 Wäre es als reine Zusatzansicht neben dem Kerzenchart sinnvoll?

Das ist die fairste Version der Frage, und sie verdient eine eigene Antwort statt eines pauschalen Nein. Der Gedanke ist legitim: Renko als visueller Rauschfilter, um Trend gegen Chop zu unterscheiden, ohne den Gate-Trigger anzufassen.

**Trotzdem nein, aus drei Gründen:**

1. **Der Job ist vor sieben Tagen bereits vergeben worden.** „Objektive Chop-/Trend-Unterscheidung" ist exakt die Lücke, für die am 24.08.2026 ADX/DMI eingebaut wurde — mit der ausdrücklichen Begründung, die bisher informelle Chop-Erkennung *„in eine objektive, live mitlaufende Zahl"* zu übersetzen. Dazu kommt die Trend-Effizienz als Rohwert (die den Regimewechsel am 20.08. nachweislich korrekt verifiziert hat). Renko würde dieselbe Frage ein drittes Mal beantworten, diesmal per Augenmaß statt per Zahl — also einen Rückschritt gegenüber dem, was gerade erst objektiviert wurde. Und wenn Renko-Optik und ADX widersprechen, gibt es keine Regel, welche gewinnt.

2. **Es gibt keinen sauberen Implementierungsweg.** Der 2v-Split ist voll (NAS100 + QQQ), und beide Panes sind gate-relevant. `chart_set_type` würde den Typ einer *bestehenden* Pane ändern — also ausgerechnet den Chart mutieren, an dem das Dual-Gate hängt. Ein drittes Pane quetscht die Sub-Panes (RSI/MACD/ATR/ADX) zusammen; genau daran ist das VIX-Pane am 02.07.2026 gescheitert. `tab_new` funktioniert in der Desktop-App nicht zuverlässig (meldet Erfolg, `tab_count` bleibt 1). Es bleibt also nur „umschalten und zurückschalten" — ein Vorgang, der während der heißen Phase durch Punkt 3a/7b ohnehin verboten ist, weil er Zeit kostet.

3. **Punkt 14.** Eine Zusatzansicht ist eine weitere Sache, die unter Zeitdruck angeschaut werden muss — mit dem Unterschied zu einer Zahl, dass sie interpretiert statt abgelesen wird. Bei Trade #42 wurde ein einzeiliger Hinweis zu spät gelesen.

**Die einzige Form, in der ich das mittragen würde** — und ausdrücklich als niedrigwertig, nicht als Empfehlung: ein gelegentlicher Blick auf einen Renko-Chart mit **fixer** (nicht ATR-basierter) Box-Größe im **Wochenreview am Montagmorgen** oder beim Tagesabschluss, rein zur nachträglichen Regime-Einordnung, auf einem separaten Layout, das das Live-Layout nicht berührt. Kein Live-Loop, kein Gate, keine Pflichtzeile, keine Änderung am 2v-Split. Der Nutzen wäre gering (die Trend-Effizienz-Zahl leistet dasselbe präziser), das Risiko wäre null. Wenn Levi die Neugier befriedigen will, ist das der einzige unschädliche Ort dafür.

### 3.6 Empfehlung Frage 3

**Nein — nicht als Ersatz, nicht als paralleler Trigger, und im Live-Loop auch nicht als Zusatzansicht.**

Die Rangfolge der Gründe:
1. **ATR(14) wird auf preisgetriebenen Charts zur Konstante** und reißt damit 8c, 8b1, 8c2, 8d, 8b1a, GC-5 und 7a1a gleichzeitig — ohne dass irgendein Gate das bemerken könnte. Das ist der Ausschlussgrund.
2. Es gibt keinen definierten Kerzenschluss, an den das Dual-Gate und die zeitgetaktete Loop-Architektur anknüpfen könnten — weder in ruhigen noch in Impulsphasen.
3. Alle Volumen-Werte auf QQQ (Volume, VWAP, σ-Bänder, RVOL) würden auf umverteiltem Pseudo-Volumen rechnen — die QQQ-Pane verlöre ihre einzige Existenzberechtigung.
4. ATR-basiertes Renko repaintet und macht das Faktenprotokoll nicht reproduzierbar.
5. Als Zusatzansicht: Der Job (objektive Chop-Erkennung) ist seit 24.08. an ADX + Trend-Effizienz vergeben, es gibt keinen kollisionsfreien Implementierungsweg, und es würde Punkt 14 belasten.

---

## Zusammenfassende Entscheidungsvorlage

| Frage | Empfehlung | Härtester Einzelgrund |
|---|---|---|
| 1 — Volumenprofil | **Nein**, plus formale Wiederaufroll-Sperre | POC/VAH/VAL sind exakte Preisniveaus; die einzige Volumenquelle (QQQ) darf laut Regel vom 15.07.2026 nie 1:1 auf NAS100 übertragen werden |
| 2 — Indikatoren auf Indikatoren | **Nein für jetzt**; Flip-Count als einziger Kandidat nach der Trade-15-Auswertung vorgemerkt | Schattenmess-Kanal führt bereits zehn unausgewertete Rohwerte bei n≈0-2 |
| 3 — Renko/Kagi/Line-Break/P&F | **Nein**, auch nicht als Live-Zusatzansicht | ATR(14) degeneriert zur Konstante und reißt sieben Regeln gleichzeitig, ohne dass ein Gate es merkt |

**Was ich stattdessen empfehle, in dieser Reihenfolge:**

1. **Den Anchored VWAP endlich einmal wirklich verankern.** Das volumenbasierte Werkzeug ist installiert, die Nutzungsregel steht seit 24.08., der Anker steht seit 26.08. durchgehend auf Fallback (`in_0 = 0`) und wurde noch nie bewusst gesetzt. Kostenpunkt: ein Klick beim Session-Start. Das ist die Frage-1-Idee (wo wurde Wert akzeptiert), auf echtem Volumen, mit bereits geschriebener Regel und null neuen Zahlen.
2. **Die zehn bestehenden Schattenmess-Rohwerte auswerten**, bevor irgendein elfter dazukommt. Diese Auswertung ist bereits vierfach als Review-Pflicht terminiert und noch nicht fällig — aber sie ist der Engpass für jede weitere Werkzeugentscheidung.
3. **Den Fokus auf die Exit-/Risikoseite verschieben.** Alle drei geprüften Vorschläge liefern Entry-seitige Level- und Bestätigungsinformation. Die Diagnose vom 24.08.2026 lautet: Setup-Erkennung stark, Exit-/Risikoseite strukturell schwach. Der bereits explizit als eigener nächster Schritt vorgemerkte Früh-Exit-Stack-Rückbau (Opus' Option A, zurückgestellt bis nach der Trade-15-Auswertung) adressiert das direkt — die drei hier geprüften Ideen tun es nicht.

---

## Quellen (Recherche zu Tick-Volumen, Frage 1.1/1.3)

- [Tick Volume vs Real Volume: The Difference Almost Nobody Explains — Trading Wyckoff](https://tradingwyckoff.com/en/tick-volume-vs-real-volume/)
- [Why Is Tick Volume Important To Monitor In The Forex Market? — Global Prime](https://globalprime.medium.com/why-is-tick-volume-important-to-monitor-56a936eea70d)
- [Volume Profile, Footprint Charts and CVD: Is Your Data Real — or a Proxy? — MQL5 Blogs](https://www.mql5.com/en/blogs/post/774007)
- [Volume profile indicators: basic concepts — TradingView (der von Levi gelesene Artikel)](https://www.tradingview.com/support/solutions/43000502040-volume-profile-indicators-basic-concepts/)

Interne Quellen: `feedback_chart_layout.md`, `feedback_chartanalyse.md` (Punkte 5-10), `feedback_live_trading.md` (7b/7b1/7b1a/14), `project_risikomanagement.md`, `project_review_4h_und_indikator_ranking_2026-07-31.md`, `project_indikator_upgrade_diskussion_22_08_2026.md`, `project_studie_bessere_trades_2026-08-24.md`, `project_opus_vollpruefung_2026-08-24.md`, `project_testtag_analyse_2026-08-25.md`, `project_testtag_analyse_2026-08-28.md`, sowie `scripts/gate_check.cjs` (direkt am Code geprüft, 950 Zeilen, Stand 31.08.2026).
