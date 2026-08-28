---
name: project_testtag_analyse_2026-08-27
description: "Unabhängiges Opus-Review des fiktiven Testtags 27.08.2026 (Sonnet solo im Loop, 17:00-20:00 Uhr, 0 Trades). Kernbefund: Die Tweet-Check-Zeile — am 25.08. noch die einzige Pflichtzeile, die hielt — ist an diesem Tag nachweislich gebrochen (5 versäumte Pflicht-Fetches, per Artefakt belegt), und der durchgehend verwendete Begriff 'stale' widerspricht sowohl dem eigenen Protokoll (NAS100 brach die EMA50 um 17:43/17:50) als auch der seit 27.08. formal definierten Stale-Klasse 7b1c (19:10 wäre 2/5, nicht stale). Die 19:10-Selbstkorrektur ist inhaltlich echt und vorab belegt. Nichts umgesetzt, alles Entscheidungsvorlage für Levi."
metadata:
  node_type: memory
  type: project
  originSessionId: opus-review-2026-08-27
  modified: 2026-08-28T08:10:09.730Z
---

# Testtag-Analyse 27.08.2026 — Opus-Review (unabhängig)

Quelle: [[testtag/testtag_2026-08-27]] (Rohprotokoll, 470 Zeilen). Gegengeprüft gegen [[feedback_live_trading]] (2b/7b/7b1/7b1a/7b1b/7b1c/7c/7d0/7e/8/9/11/12.4/13/14/15), [[feedback_chartanalyse]] (8a1-8a4, 8b/8b1/8b2, 8c/8c2, 8d, 9b/9d), [[feedback_vollcheck_format]], [[project_regelwerk_ueberarbeitung_2026-08-26]] (Pakete 1-6c), [[project_risikomanagement]], [[feedback_modellwahl_trading]] sowie gegen die Artefakte im Repo: `scripts/x_last_fetch.json`, `scripts/trades.db`, `scripts/gate_check.cjs`, `scripts/cooldown_check.cjs`, `scripts/add_skipped_setup.cjs`, `screenshots/`. Vergleichsmaßstab für Tiefe/Format: [[project_testtag_analyse_2026-08-24]] und [[project_testtag_analyse_2026-08-25]].

**Der Tag war fiktiv, es gab 0 Trades. Keine Datei wurde von Opus geändert, keine Regel angepasst, kein Chart angefasst, kein Skript ausgeführt außer lesenden DB-Abfragen. Alles unten ist Entscheidungsvorlage, nichts ist umgesetzt.**

---

## 0. Vorbemerkung — was ich prüfen konnte und was nicht

**(a) Die Datenlage ist die beste der drei Testtage.** 470 Zeilen gegenüber 335 (24.08.) und 101 (25.08.). Alle 35 protokollierten Voll-Checks stehen im Wortlaut, die Quick-Ticks großenteils ebenfalls. Vorschlag 6 aus dem 25.08.-Review ("Testtag-Protokollpflicht auf das 24.08.-Niveau zurückholen") ist damit erfüllt und sogar übertroffen. Das ist die wichtigste Verbesserung des Tages und der Grund, warum dieses Review überhaupt etwas belegen kann.

**(b) Es gibt an diesem Tag zum ersten Mal eine unabhängige Artefakt-Spur.** Drei Quellen außerhalb des Protokolls, die ich gegenlesen konnte:

| Artefakt | Befund |
|---|---|
| `scripts/x_last_fetch.json` | Inhalt `2026-08-27T17:49:04.000Z` = **19:49:04 Ortszeit**; Datei-Änderungszeit **19:56 Ortszeit** |
| `screenshots/` | 26 Screenshots vom 27.08., von 16:39 bis **19:11:44** — danach kein einziger mehr |
| `scripts/trades.db` | `trades` 43 Zeilen (letzter echter Trade #43 vom 21.08.), `skipped_setups` **0 Zeilen** |

Diese drei Artefakte tragen einen erheblichen Teil der Befunde unten und machen sie unabhängig vom Protokolltext prüfbar. Dass die DB unangetastet blieb, ist korrekt: kein fiktiver Trade wurde eingetragen.

**(c) Was ich ausdrücklich nicht feststellen kann.** Ob `cooldown_check.cjs` um 19:10 tatsächlich lief (das Skript hinterlässt kein Artefakt, und der Output wurde nicht ins Protokoll übernommen — dazu Abschnitt 4); ob zwischen den nicht einzeln protokollierten Minuten Prüfungen stattfanden; und was zwischen 19:11 und 19:17 geschah (Abschnitt 7). Wo ich unten trotzdem eine Einschätzung abgebe, ist sie als Einschätzung gekennzeichnet.

**(d) Der wichtigste Befund ist wieder arithmetisch, nicht interpretativ** — und er trifft ausgerechnet die eine Pflichtzeile, die das 25.08.-Review als funktionierend gelobt hat. Er steht deshalb vorn.

---

## 1. Bilanz in Zahlen

- **Loop-Fenster:** 16:4x Uhr CronCreate, erster protokollierter Tick 17:02, letzter Check 20:00 Uhr (Levi-Stop, festgelegt 18:14 Uhr)
- **Levi im Chat:** punktuell (17:03/17:04 Nachfrage, 17:08/17:09 Kontingent-Vorgaben, 18:14 Stopp-Zeitpunkt) — im Übrigen Solo-Mandat wie 24./25.08.
- **Fiktive Trades:** 0
- **Voll-Checks: protokolliert 35, im Protokoll durchgezählt bis „36." — die Zählung enthält einen Phantom-Eintrag** (nach „(3.)" um 17:15 folgt „(5.)" um 17:20, die Nummer 4 existiert nicht) **und eine echte Lücke** (um **19:15 fand kein Voll-Check statt**, die einzige ausgelassene 5-Minuten-Marke des gesamten Tages)
- **Quick-Ticks:** 65 protokollierte Einträge/Blöcke
- **Screenshots:** 26, letzter um 19:11:44 — für die letzten 49 Minuten des Tages keiner
- **Tweet-Fetches:** 17 im Protokoll ausgewiesen; **mindestens 5 fällige Fetches nicht durchgeführt** (Abschnitt 2)
- **Neue Einträge in `skipped_setups`:** 0 — **dritter Testtag in Folge mit n=0**
- **Von Sonnet selbst offengelegte Prozessfehler:** 0 (die 19:10-Szene ist als Selbstkorrektur, nicht als Fehler protokolliert)
- **Von Opus gefundene Regel-/Formatverstöße:** 5 versäumte Tweet-Fetches, 0 von ~35 fälligen `Spike-Ausnahme`-Zeilen, 9 Regime-Gate-Zeilen ohne Rohwert, 3 Voll-Checks mit geschätzten statt gelesenen 5min-Werten, 1 sachlich falsche Dual-Gate-Zustandsbeschreibung (ab 19:10 wiederholt)

---

## 2. Kernbefund: Die Tweet-Check-Zeile ist gebrochen — und zwar die eine, die am 25.08. gehalten hat

### 2.1 Warum das ausgerechnet diese Zeile ist

Das 25.08.-Review kam zu diesem Schluss ([[project_testtag_analyse_2026-08-25]] Abschnitt 6):

> „**Die Tweet-Check-Zeile funktioniert**, weil sie seit dem 25.08. das rohe Minuten-Delta mitführen muss statt nur ‚fällig/nicht fällig'. Ein Delta ist gegenprüfbar, ein Urteil nicht. … der einzige Pflichtzeilen-Fix in diesem Projekt, der aus einem Urteil einen Rohwert gemacht hat — und der einzige, der heute nicht gebrochen wurde."

Auf dieser Beobachtung ruht Vorschlag 1 des 25.08.-Reviews, ruht die Format-Verschärfung vom 26.08. ([[feedback_vollcheck_format]], Absatz „Prinzip auf zwei weitere Pflichtzeilen ausgedehnt"), und ruht damit ein guter Teil der These „Rohwerte statt Urteile lösen das Problem". Wenn diese Zeile bricht, ist das kein Nebenbefund.

### 2.2 Die Rechnung

Das Protokoll nennt bei jedem durchgeführten Fetch den neuen `x_last_fetch.json`-Zeitstempel (UTC). Umgerechnet in Ortszeit (MESZ = UTC+2) ergibt das eine lückenlose Kette von 17 Stempeln. Aus ihr lässt sich jedes behauptete Delta nachrechnen. Ergebnis für die Voll-Checks, die „**nicht fällig**" meldeten:

| Voll-Check | letzter Stempel (Ortszeit) | Delta behauptet | Delta tatsächlich | Schwelle ≥10 Min | Urteil |
|---|---|---|---|---|---|
| 17:05 | ~16:28 (Sessionstart-Fetch) | „Delta 37 Min" | **37** | fällig | **✗ nicht durchgeführt** |
| 17:35 | 17:25:00 | „10 Min, Grenzfall" | **10,0** | fällig | **✗ nicht durchgeführt** |
| 18:15 | 18:06:06 | „5 Min" | 8,9 | nicht fällig | ✓ (Delta falsch) |
| 18:35 | 18:22:38 | „5 Min" | **12,4** | fällig | **✗ nicht durchgeführt** |
| 18:45 | 18:34:00 | „5 Min" | **11,0** | fällig | **✗ nicht durchgeführt** |
| 19:50 | 19:40:08 | „5 Min" | 9,9 | Grenzfall | ✗ (Bezug: Check-Zeit statt Stempel) |
| 20:00 | 19:49:04 | „~11 Min" | **10,9** | fällig | **✗ nicht durchgeführt** |

**Fünf klar fällige Pflicht-Fetches wurden nicht durchgeführt, ein sechster liegt auf der Schwelle.** Der letzte davon ist unabhängig vom Protokoll bewiesen: `scripts/x_last_fetch.json` steht bis heute auf `2026-08-27T17:49:04.000Z`. Nach dem 19:55-Voll-Check hat es keinen weiteren Fetch gegeben — der 20:00-Check hat mit einem selbst genannten Delta von 11 Minuten „nicht fällig" gemeldet.

Der erste Fall ist der bemerkenswerteste: Der **allererste Voll-Check des Tages** (17:05) nennt selbst „Delta 37 Min zum Sessionstart-Fetch" und schließt daraus „noch nicht fällig". Bei einer Schwelle von 10 Minuten ist das ein Faktor 3,7 über der Fälligkeit. Fünf Minuten später wurde derselbe Fetch dann als „fällig, Delta ~41 Min" nachgeholt.

### 2.3 Auch die Deltas selbst stimmen nicht

Unabhängig von der Fälligkeitsfrage weichen mehrere genannte Deltas von den protokolleigenen Zeitstempeln ab:

| Voll-Check | Delta behauptet | aus den eigenen Stempeln | Abweichung |
|---|---|---|---|
| 17:50 | 12 Min | 9,9 | +2,1 |
| 18:10 | „fällig, Delta 5 Min" | 8,9 | in sich widersprüchlich (5 < 10 = „fällig"?) |
| 18:40 | 12 Min | 17,4 | −5,4 |
| 19:00 | 16 Min | 9,3 | +6,7 |
| 18:35 / 18:45 | je 5 Min | 12,4 / 11,0 | −7,4 / −6,0 |

Und in **vier** Voll-Checks (19:10, 19:25, 19:45, 19:55) wird überhaupt kein Delta genannt, sondern nur die Ordinalzahl („2. Voll-Check seit 17:12-Fetch"). [[feedback_vollcheck_format]] (Zeitzonen-Fix, 25.08.2026) verlangt ausdrücklich: „die Zeile nennt in **JEDEM** der drei Zweige das reine Minuten-Delta seit dem letzten Fetch, nicht nur einen Uhrzeit-Vergleich." Ausgerechnet der Voll-Check mit dem zentralen Ereignis des Tages (19:10) ist einer der vier ohne Rohwert.

### 2.4 Die Ursache — und sie ist keine Schlamperei allein

Zwei Dinge greifen ineinander.

**Erstens: Der Zeitstempel ist nicht der Fetch-Zeitpunkt.** `x_last_fetch.json` wurde laut Datei-Änderungszeit um **19:56** geschrieben, trägt aber den Wert **19:49:04**. Sieben Minuten Differenz. Dasselbe Muster zeigt das Protokoll durchgehend: 7 der 17 Stempel liegen **vor** dem Voll-Check, der sie angeblich geschrieben hat (17:30 → 17:25:00; 18:10 → 18:06:06; 18:30 → 18:22:38; 18:40 → 18:34:00; 19:35 → 19:31:34; 19:45 → 19:40:08; 19:55 → 19:49:04), drei liegen ~2 Minuten danach. **Damit ist der Rohwert, auf dem die einzige funktionierende Pflichtzeile des Projekts beruht, selbst falsch gemessen** — und alle daraus gerechneten Deltas erben den Fehler. Das ist die exakte Wiederholung des 25.08.-Musters „die Eingaben waren das Problem, nicht die Arithmetik", nur eine Ebene tiefer.

**Zweitens: Das Regelwerk enthält weiterhin zwei konkurrierende Fälligkeitsmechaniken.** [[feedback_live_trading]] Punkt 2b (2) und Punkt 9 (b) schreiben die Delta-Prüfung als alleinige Quelle fest. Punkt 9, Abschnitt „X-Tweets als Bestandteil des Voll-Checks", enthält aber in **lebendem, nicht durchgestrichenem** Text weiterhin: „**bleibt bei jedem 2. Voll-Check (alle 10 Min)**". Genau diese Ordinal-Variante hat Sonnet in den vier Zeilen ohne Delta benutzt — und genau in ihr entstehen die Aussetzer, weil „jeder 2. Voll-Check" bei stempel-verschobenen Fetches auf 11-12 Minuten driftet.

**Das war bereits ein Befund vom 24.08.** Opus-Vorschlag 3 von damals lautete wörtlich: „Widersprüchliche Fälligkeitsmechanik in `feedback_live_trading.md` bereinigen — Delta als alleinige Quelle, alte Fassungen durchstreichen." An drei Stellen (Punkt 9 „How to apply", Punkt 9 (b), Punkt 12-Bereich) ist das geschehen. An einer Stelle nicht. **Die fünf versäumten Fetches sind die messbaren Kosten dieser einen nicht bereinigten Stelle.**

### 2.5 Was das inhaltlich bedeutete

Ehrlich eingeordnet: **Es ist an diesem Tag nichts passiert.** Die Nachrichtenlage war ruhig (NVDA-Nachwirkung, Iran/Hormuz-Rhetorik ohne neue Aktion, Wiederholungen), kein Blackout, kein Kalender-Release im Fenster. Kein einziger der fünf versäumten Fetches hätte an einer Entscheidung etwas geändert — es gab keine Entscheidung. Der Schaden ist null; der Befund ist trotzdem hart, weil er die Grundlage einer Reform-These angreift, die auf genau dieser Zeile aufgebaut wurde.

---

## 3. Zweiter Kernbefund: „stale" bedeutet an diesem Tag zwei verschiedene Dinge — und beide Verwendungen halten der Prüfung nicht stand

Das Wort „stale" trägt das gesamte Tagesergebnis. Es steht in praktisch jedem Voll-Check und im Tagesabschluss. Es hat an diesem Tag zwei Probleme.

### 3.1 Die Zustandsbeschreibung ist ab 17:55 sachlich falsch

Ab dem 19:10-Voll-Check steht wiederholt (19:10 „Fakt-Check", dann 19:20, 19:25 wörtlich):

> „Beide Instrumente laufen bereits seit dem Vormittag (QQQ-Cross ca. 16:00-16:15, NAS100 ähnlich früh) **ununterbrochen** über ihrer EMA50"

Das widerspricht dem eigenen Protokoll. Um **17:44** steht dort:

> „NAS100 5min-Kerze 17:38-17:43 geschlossen bei 29.493,15, unter EMA50 (~29.505) — **erster echter Kerzenschluss unter EMA50** seit dem morgendlichen Impuls"

und um **17:49**:

> „NAS100 5min-Kerze 17:45-17:50 … zweite Kerze in Folge unter EMA50 — NAS100-Seite jetzt ‚2/2' bestätigt"

NAS100 lief also **nicht** ununterbrochen über seiner EMA50. Es gab zwei bestätigte Schlusskurse darunter. Und daraus folgt die eigentlich interessante Konsequenz, die im Protokoll nirgends gezogen wird:

> **NAS100 hat um 17:55 und 18:00 einen frischen 8a2-Long-Reclaim vollzogen (1/2 und 2/2).**

Die Zahlen dafür stehen im Protokoll: die 17:50-17:55-Kerze schloss bei 29.545 über EMA50 29.505,8 (1/2), die 17:55-18:00-Kerze bei 29.533,85 über EMA50 ~29.507 (2/2). Ab 18:00 war die NAS100-Hälfte des Long-Dual-Gates also **frisch**, nicht stale. Das Protokoll schreibt stattdessen um 18:05: „beide Instrumente bullisch stabil, **Dual-Gate bleibt stale**" — und wiederholt das bis 20:00.

**Am Ergebnis ändert das nichts** (QQQ war nie unter seiner EMA50 und konnte deshalb keinen frischen Cross liefern — dazu Abschnitt 6). Aber es ändert die Beschreibung: Der Tag hatte einen frischen NAS100-Trigger, an dem die B2-Schattenmessung nach [[feedback_live_trading]] 7b1b (QQQ-Abstand zur eigenen 5min-EMA50 und zum VWAP-σ-Band 1 im Trigger-Moment) hätte erhoben werden müssen. Sie wurde nicht erhoben. Das ist der dritte Testtag in Folge, an dem die Schattenmessung an ihrem einzigen möglichen Einsatzpunkt schweigt.

### 3.2 Die formale Stale-Klasse (7b1c) hätte am 19:10 „nicht stale" gesagt

Seit dem 27.08. — dem Vormittag desselben Tages — ist „Stale" ein **definierter Begriff** mit fünf Bedingungen ([[feedback_live_trading]] 7b1c). Ich habe sie für den 19:10-Moment gegen die Protokollwerte durchgerechnet:

| Bedingung 7b1c (Stale-Long) | Wert um 19:10 | erfüllt? |
|---|---|---|
| 1. Gate-Alter: NAS100 ≥6 5min-Kerzen / QQQ ≥3 15min-Kerzen ohne Gegenseiten-Schluss | NAS100 ~15 Kerzen seit 17:50; QQQ nie darunter | ✓ |
| 2. Gate-Reserve: QQQ-Abstand zur eigenen 15min-EMA50 ≥40 % der QQQ-Session-Range | 720,43 − 717,40 = 3,03 Pkt bei Session-Range ~3,5 Pkt ≈ **85 %** | ✓ |
| 3. Momentum-Verfall: ADX(5min) ≥25 % unter Bewegungs-Hoch UND fällt ≥3 Kerzen | ADX **17,85 = Tageshoch**, steigend | **✗** |
| 4. Struktur gegen Richtung: jüngstes Swing-Hoch ≥0,5× ATR unter dem Session-Hoch | Kurs steht **auf** dem Session-Hoch (29.612,35) | **✗** |
| 5. Indikator gegen Richtung: MACD-H(5min) gegen Gate-Richtung | MACD-H **+5,7**, mit der Richtung | **✗** |

**Ergebnis: 2/5 — nach der eigenen, tags zuvor beschlossenen Definition war die Lage um 19:10 ausdrücklich NICHT „stale".** Sie war das genaue Gegenteil dessen, was 7b1c beschreibt: ein altes Gate, hinter dem der Markt mit maximalem Momentum in Gate-Richtung lief.

Das ist kein Regelverstoß — die Zeile war formal nicht geschuldet, weil sie laut Punkt 2b/7b1c nur „vor jeder tatsächlichen Order" erscheint und keine Order zustande kam. Es ist ein **Begriffsproblem mit Folgen**: Das Protokoll benutzt „stale" für „kein frischer Cross als Trigger verfügbar", das Regelwerk seit 27.08. für „Gate ist alt UND der Markt läuft dagegen". Beide Bedeutungen stehen jetzt nebeneinander unter demselben Wort, und der Tagesabschluss verwendet es in der ersten Bedeutung mit ausdrücklichem Verweis auf 7b1c („durchgehend ‚stale' **im Sinne von 7b1c**"). Das ist der einzige Satz im Protokoll, den ich als schlicht unzutreffend bezeichnen würde.

---

## 4. Die 19:10-Szene — echte Selbstkorrektur oder nachträgliche Rationalisierung?

**Mein Urteil: die Regelanwendung ist echt und vorab belegt. Die Dramaturgie („war kurz vor der Order") ist es nicht — sie ist weder belegbar noch widerlegbar, und der Tagesabschluss überhöht sie.**

### 4.1 Was dafür spricht, und zwar stark

Der entscheidende Punkt ist nicht der 19:10-Eintrag selbst, sondern das, was **davor** steht. Bereits um **18:45**, 25 Minuten vor dem Ereignis:

> „Chartmuster: direkter R2-Test mit Momentum-Beschleunigung — stärkstes bullisches Signal des Tages, aber technisch **Widerstandsdurchbruch, kein frischer 8a2-EMA50-Cross**. … **Widerstandsdurchbruch ist kein automatischer Trigger ohne frischen EMA50-Cross**"

und um **19:00**:

> „Bei Bestätigung über R2 wäre dies der bisher sauberste Ansatzpunkt des Tages, weiterhin Chasing-Check (Punkt 13) und **Dual-Gate-Status (QQQ-Seite unverändert stale, kein frischer EMA-Cross)** zu prüfen."

Beide Sätze stehen in Voll-Checks, die durch Screenshots um 18:46:17 und 19:01:37 zeitlich verankert sind. **Die Regel war also 25 Minuten vor der Versuchung schriftlich festgelegt, mit dem richtigen Inhalt und in der richtigen Richtung.** Das ist eine sehr viel bessere Beleglage als eine nachträgliche Begründung, und es ist genau die Form von Vorab-Festlegung, die eine Rationalisierung praktisch ausschließt: Wer sich nachträglich rechtfertigt, schreibt die Begründung nicht zweimal vorher hin, wo sie ihm später im Weg stehen könnte.

Ebenfalls dafür: der 18:50-Voll-Check akzeptierte eine unerwünschte Auflösung („R2 wurde nicht überzeugend gebrochen — Kerzenschluss unter Level trotz Intrabar-Durchbruch"), obwohl der Text 5 Minuten zuvor deutlich zum Ausbruch tendierte. Und der 19:05-Check wartete auf den echten Kerzenschluss der Rücktest-Kerze, statt den bereits bestätigten Ausbruch als Freibrief zu behandeln.

### 4.2 Was nicht belegbar ist

- **„cooldown_check.cjs bereits gelaufen, 🟢"** — das Skript schreibt nichts, der Output wurde nicht zitiert. [[feedback_live_trading]] Punkt 2b (7) verlangt „Kernzeile der Ausgabe ins Protokoll". Ein Ampelzeichen ist keine Kernzeile. **Damit ist die einzige nachprüfbare Behauptung der ganzen Szene unprüfbar gemacht worden** — und zwar durch dieselbe Formschwäche, für die das 25.08.-Review Vorschlag 2 geschrieben hat („`gate_check.cjs`-Aufruf im Protokoll belegen. … Solange ‚im Kopf gerechnet' und ‚Skript gelaufen' im Output identisch aussehen, ist 7b1s Klarstellung nicht durchsetzbar"). Derselbe Mechanismus, anderes Skript.
- **Wie weit die Vorbereitung tatsächlich gediehen war.** „Beim Zusammenstellen der Gate-Parameter fiel auf …" — es existiert kein Zwischenprodukt, kein `gate_check.cjs`-Aufruf, keine SL/TP-Skizze im Protokoll.
- **Die Zeitleiste unmittelbar danach.** Zwischen 19:10 und 19:17 gibt es **keinen einzigen Eintrag** — keine Quick-Ticks für 19:11-19:16, keinen 19:15-Voll-Check. Es ist die einzige solche Lücke des Tages, und sie liegt direkt hinter dem Schlüsselereignis. Dazu Abschnitt 7.

### 4.3 Eine innere Spannung, die ich benennen will

Wenn die Regel um 18:45 und um 19:00 zweimal schriftlich festgehalten wurde, warum begann dann um 19:10 überhaupt eine Entry-Vorbereitung? Zwei Lesarten, beide unbequem, beide aus dem Material nicht entscheidbar:

- Die Vorab-Festlegung war da und wurde 25 Minuten später trotzdem kurz vergessen — dann ist die Szene ein **Drift-Beleg** mit anschließendem Fang, nicht eine Standhaftigkeitsprobe. Das passt zum 25.08.-Befund „die Regel hält im Leerlauf und bricht unter Handlungsdruck", nur mit besserem Ausgang.
- Die „Vorbereitung" war weniger weit gediehen, als die Formulierung nahelegt — dann ist die Szene weniger dramatisch als beschrieben.

**In beiden Lesarten bleibt der Kern positiv:** kein Entry ohne Trigger, richtige Regel, richtiger Präzedenzfall (Punkt 7b1, Trade #39). Aber sie trägt nicht das Gewicht, das der Tagesabschluss ihr gibt.

### 4.4 Der Tagesabschluss bewertet den Tag — das war ausdrücklich nicht Sonnets Rolle

Levis Vorgabe um 18:14 Uhr, im Protokoll selbst festgehalten: „Übergabe an Opus für die Tagesanalyse … **Sonnet bewertet den Tag NICHT selbst, liefert nur das vollständige Protokoll**." [[feedback_modellwahl_trading]] weist Tagesabschluss und Einordnung Fable zu, Sonnet „NUR noch Live-Trading-Ausführung".

Der Abschnitt „Tagesabschluss 27.08.2026" enthält gleichwohl:

> „**Wichtigster Vorfall des Tages** …" · „ist **der zentrale Datenpunkt für das Opus-Review** …" · „beide Situationen wurden **regelkonform** ohne Entry aufgelöst"

Das sind drei Bewertungen: eine Gewichtung, eine Vorgabe an den Prüfer, ein Regelkonformitäts-Urteil über den eigenen Tag. Eine faktische Zusammenfassung wäre unbedenklich gewesen; die Selbstbescheinigung „regelkonform" ist genau die Aussage, die die Rollentrennung seit 24.08. verhindern soll. **Sie ist zudem in dieser Absolutheit nicht haltbar** — siehe Abschnitt 2 (fünf versäumte Pflicht-Fetches) und Abschnitt 8.

Das ist der milde, aber echte Wiederkehrer des 25.08.-Musters „die protokollierte Lehre ist eine Ergebnis-Schlussfolgerung, die dem Autor nicht zusteht".

---

## 5. Die 19:45-20:00-Szene — sauber gehandhabt, mit einer Format-Einschränkung

**Urteil: korrekt, ohne Interpretationsspielraum-Ausnutzung.** Der Ablauf ist über neun Ticks und drei Voll-Checks lückenlos dokumentiert:

| Zeit | Kurs | EMA50(5min) | Abstand |
|---|---|---|---|
| 19:45 | 29.561,15 | 29.545,1 | 16,1 |
| 19:50 | 29.549,45 | 29.545,0 | 4,45 |
| 19:58 | 29.546,05 | 29.545,5 | **0,55** |
| 19:59 | 29.549,15 | 29.545,6 | 3,55 |
| 20:00 (Schluss 19:55-20:00) | **29.548,05** | ~29.545,5-29.546,0 | ~2,1-2,6 |

Was hier richtig war: Der bärische DI-Cross um 19:46 (−DI 22,27 vs. +DI 20,57) wurde **nicht** als Trigger behandelt — korrekt, denn [[feedback_chartanalyse]] Punkt 6 schließt +DI/−DI als Richtungssignal ausdrücklich aus, und das 24.08.-Review hat das noch einmal bekräftigt („Ausdrücklich NICHT empfohlen: +DI/−DI als Kriterium"). Der Intrabar-Unterschnitt um 19:58 wurde nicht zum Bruch erklärt (7d0 sauber angewandt). Und die Entscheidung fiel am tatsächlichen Kerzenschluss, nicht vorher.

**Die eine Einschränkung:** Die wichtigste Boundary-Entscheidung des Tages („schloss die Kerze unter der EMA50?") steht im Protokoll mit einem EMA50-Wert als **Spanne**: „oberhalb der EMA50 zum damaligen Zeitpunkt (~29.545,5-29.546,0)". Der Abstand betrug ~2,1-2,6 Punkte, die Unsicherheit ~0,5 Punkte — das Ergebnis trägt, aber die Form ist exakt die, die Vorschlag 1 vom 25.08. abschaffen sollte: ein Urteil mit ungefährer Eingabe statt zweier abgelesener Zahlen. Hätte der Kurs 1 Punkt tiefer geschlossen, wäre die Entscheidung aus dem Protokoll heraus nicht mehr nachvollziehbar gewesen.

Ergänzend: Diese ganze Strecke wurde **ohne einen einzigen Screenshot** beurteilt (letzter Screenshot 19:11:44). Keine Regelverletzung — Screenshots sind keine Pflichtzeile — aber es ist die visuell unbelegteste Phase des Tages und zugleich die entscheidungsnaheste.

---

## 6. War das Dual-Gate strukturell blockiert? — getrennt nach Richtung, mit Zahlen

Die Frage aus dem Auftrag lautet, ob hier der 24.08.-Satz gilt: *„Ein Gate, das nie öffnen kann, testet keine Disziplin."* **Antwort: für die Long-Seite ja, für die Short-Seite nein — und die Short-Seite ist der belastbarere Befund.**

### 6.1 Short-Seite: das Gate war lebendig, es fehlten 0,24 Punkte

Um 17:45 stand alles bis auf einen Wert:

| | Wert | fehlend |
|---|---|---|
| NAS100 5min | 2 Schlüsse unter EMA50 (17:43: 29.493,15; 17:50: 29.460,15) | erfüllt (2/2) |
| NAS100 15min | Schluss 29.470,95 unter EMA50 29.479,6 | erfüllt |
| **QQQ 15min (Primärtrigger)** | Schluss **717,21** vs. EMA50 **716,84** | **0,37 Pkt / 0,052 %** |
| QQQ 15min Intrabar-Tief | 717,08 | **0,24 Pkt / 0,033 %** |

Zum Vergleich der 24.08.-Fall, den Opus damals als strukturell blockiert eingestuft hat: dort stand QQQ **2,14 Punkte / 0,302 %** von seiner eigenen EMA50 entfernt. **Am 27.08. war das Gate rund sechsmal näher am Umschlagen.** Ein Gate, das um 0,24 Punkte verfehlt wird, ist kein totes Gate — es ist ein knapp entschiedenes.

Und es wurde **richtig** entschieden: Zehn Minuten später (17:55) lief NAS100 in einer einzigen 5min-Kerze von 29.460 auf 29.545 zurück, voller Reclaim, die 1H-Kerze schloss mit 122 Punkten Spanne praktisch unverändert. **Das QQQ-Veto war ex post korrekt** — dieselbe Konstellation wie am 25.08. um 17:00, und dieselbe wie beim 24.08.-Retest-Long. Das ist der eine Punkt des Tages, an dem sich das Dual-Gate tatsächlich bewährt hat, und er ist mit Zahlen belegbar.

Die zweite Annäherung um 19:58 (NAS100 0,55 Pkt, QQQ ~1,5 Pkt) war ebenfalls real, blieb aber unter zwei fehlenden Kerzenschlüssen stehen.

### 6.2 Long-Seite: strukturell tot, aber aus einem präziseren Grund als das Protokoll angibt

Für einen frischen Long-Trigger hätte QQQ **zwei** Schritte gebraucht: erst einen 15min-Schluss unter seine EMA50, dann einen darüber. QQQ hat den ersten Schritt an seinem tiefsten Punkt des Tages um 0,24 Punkte verfehlt. Damit war der Long-Dual-Gate an diesem Tag praktisch nicht erreichbar — und zwar unabhängig davon, wie stark NAS100 stieg.

Das ist **Befund A des 24.08.-Reviews in gespiegelter Form**: Die NAS100-Referenz ist eine 5min-EMA50 (~4 Std. Kursgedächtnis), die QQQ-Referenz eine 15min-EMA50 (~12,5 Std.). Die schnelle Linie wurde am 27.08. zweimal gekreuzt, die langsame kein einziges Mal berührt. An einem Tag ohne größeren Rücksetzer kann die langsame Hälfte des Gates gar keinen frischen Cross erzeugen — nicht weil der Markt divergiert, sondern weil ihre Zeitebene das nicht hergibt.

**Konsequenz für die Auftragsfrage:** Der 19:10-R2-Ausbruch — das stärkste bullische Bild des Tages (ADX-Tageshoch 17,85, +DI 30,1 vs. −DI 15,6, zweifach bestätigter Kerzenschluss über R2, QQQ mit Session-Hoch) — traf auf ein Gate, das an diesem Tag nicht öffnen konnte. Der 24.08.-Satz gilt hier also. **Aber die 19:10-Szene widerlegt ihn trotzdem in einem Punkt:** Am 24.08. wurde ein *Setup* blockiert, das das Gate nie freigegeben hätte — es gab nichts zu widerstehen. Am 27.08. hat jemand begonnen, **ohne jeden Trigger** eine Order vorzubereiten, und hat abgebrochen. Das ist keine Geduldsprobe am Gate, sondern eine Disziplinprobe gegen den Eigenentry — genau der Fehlertyp von Trade #39. In dieser, engeren Lesart ist der Datenpunkt echt.

### 6.3 Ist „0 Trades" damit Disziplin oder Verweigerung?

**Weder noch — es ist der korrekte Ausgang eines Tages mit genau einer echten Gate-Entscheidung.** Diese eine Entscheidung (17:45, QQQ-Veto) war regelkonform und ex post richtig. Alles andere am Tag war entweder kein Setup oder ein Setup ohne existierenden Trigger-Pfad.

Was ich **nicht** sagen kann und ausdrücklich nicht sage: dass der Tag Disziplin über drei Stunden bewiesen hat. Dafür bräuchte es mehr als eine Entscheidungssituation. Was der Tag stattdessen belegt, ist etwas Nüchterneres und für die Testtag-Konstruktion Wichtigeres: **Bei einer Trigger-Definition, die auf zwei Instrumenten frische Kerzenschluss-Crosses verlangt, ist ein ruhiger Trendtag ohne Rücksetzer strukturell ein Null-Trade-Tag.** Das ist keine Aussage über die Ausführung, sondern über die Testtag-Auswahl: Wer Regeldisziplin testen will, braucht einen Tag, an dem das Gate mindestens einmal öffnet. Der 27.08. war 0,24 Punkte davon entfernt, ein solcher Tag zu sein.

---

## 7. Der Dokumentations-Bruch um 19:11-19:17 — ein Befund, der im Protokoll nicht vorkommt

Um 19:11 ändert sich die Protokollführung sprunghaft. Alle folgenden Beobachtungen sind unabhängig belegbar:

| Merkmal | vor 19:11 | ab 19:17 |
|---|---|---|
| Screenshots | 26 Stück, ~alle 5 Min, letzter **19:11:44** | **keiner** |
| Quick-Ticks | Sammelblöcke über 2-4 Minuten, Fließtext | Einzeleinträge pro Minute, Pipe-Format |
| Voll-Check-Länge | 5-12 Zeilen | 20-25 Zeilen mit eigenem QQQ-Block (AVWAP, Sigma-Bänder, Directional RelVol) |
| Regime-Gate-Zeile | „Regime-Gate 8d: ✗ (0/3)" | „Regime-Gate (8d): Trend intakt …" — **ohne n/3** |
| Tweet-Check-Zeile | Minuten-Delta | teils nur Ordinalzahl |
| 7c / 8a1 / 13 | nur bei Anlass erwähnt | in jedem Voll-Check explizit abgehakt |
| Lücke | — | **19:11-19:16 keine Einträge, 19:15-Voll-Check fehlt** |

Die naheliegendste Erklärung — und ich kennzeichne sie ausdrücklich als **Einschätzung, nicht als Befund** — ist ein Kontext-/Session-Wechsel um 19:11-19:17, möglicherweise im Zusammenhang mit Levis Kontingent-Hinweis von 17:08/17:09. Dafür spricht die Kombination aus Lücke, verändertem Ausgabetemplate und dem abrupten Ende der Screenshot-Kadenz. Dagegen spricht nichts, was ich sehen könnte.

**Was daran unabhängig von der Ursache ein echter Befund ist:** Die Umstellung ist im Protokoll **nirgends offengelegt**. Weder das Aussetzen der Screenshots noch der fehlende 19:15-Voll-Check noch der Formatwechsel wird erwähnt. Am 24.08. wurden drei Prozessfehler selbst gefunden und offengelegt; am 25.08. zwei von drei (auf Levis Nachfrage). Am 27.08. wurde **keiner** offengelegt — und dieser hier war der sichtbarste.

**Fairnesshalber:** Das neue Template ist in mehreren Punkten **besser** als das alte. Es hakt 7c, 8a1 und den Chasing-Check (13) explizit ab, es führt einen eigenständigen QQQ-Block mit AVWAP-Sigma-Bändern und Directional RelVol, und es rechnet die 1H-Strukturreserve in jedem Check nach. Der Bruch ist kein Qualitätsabfall — er ist ein unbemerkter, undokumentierter Wechsel, der zwei Rohwerte (8d-Zähler, Tweet-Delta) mitgenommen hat.

---

## 8. Pflichtzeilen-Bilanz — vollständig, mit fairer Abgrenzung

Zuerst die Abgrenzung, weil sie einen großen Teil der 25.08.-Kritikliste hier **entfallen** lässt: Vier Pflichtzeilen (Stall-Check 12.4, Positions-Kasten Punkt 8, Volumen-Check Punkt 11, Punkt-11-Reversal-Kriterien) gelten laut [[feedback_live_trading]] Punkt 2b nur „FALLS eine Position offen ist". Es war keine offen. Ebenso `Chop-Check` (nur bei Vollentry), `SL/ATR-Ratio` und `TP-Realismus` (nur an Entry-Momenten). **Diese sieben zu Recht nicht vorhandenen Zeilen sind kein Befund.**

Was tatsächlich geschuldet war:

| Pflichtzeile | Geltung | Soll | Ist | Urteil |
|---|---|---|---|---|
| MTF-Vierschritt (1H/15min/5min/QQQ) | jeder Voll-Check | 35 | 35 | ✓ |
| `Tweet-Check: …` (Zeile vorhanden) | jeder Voll-Check | 35 | 35 | ✓ |
| `Tweet-Check` mit Minuten-Delta | jeder Voll-Check | 35 | 31 | **✗ 4 ohne Rohwert** |
| Tweet-**Fetch** bei Delta ≥10 Min | anlassabhängig | 22 | 17 | **✗ 5 versäumt** (Abschnitt 2) |
| `Format: Fließtext ✓` | jeder Voll-Check | 35 | 35 | ✓ |
| `Regime-Gate (8d): Schock-Tag ✓/✗ (n/3)` | jeder Voll-Check | 35 | 35 vorhanden, **26 im Format** | **✗ 9 ohne n/3, andere Frage beantwortet** |
| `ADX(14, NAS100): X (GEMESSEN, KEIN GATE)` | jeder Voll-Check | 35 | 35 vorhanden, **32 mit Rohwert** | **✗ 3× „unverändert niedrig" / „~10,5" / „~14"** |
| `Spike-Ausnahme: …` bei **jeder** Dual-Gate-Prüfung | ~35 | ~35 | **0** | **✗ vollständig ausgefallen** |
| `Basis-Reclaim-Klasse: …` sobald 8a4 Bed. 1 erfüllt | ab 19:00 | ≥1 | **0** | **✗** (Scope strittig, s.u.) |
| `Stale-Check: …` (7b1c) | nur vor Order | 0 | 0 | ✓ entfällt zu Recht |
| `9b-Divergenz-Check: …` | nur vor Order | 0 | 0 | ✓ entfällt zu Recht |
| `Kerze geschlossen: …` im Rohwert-Format (7d0) | vor Order | 0 | 0 | ✓ entfällt; inhaltlich 4× korrekt gewartet |

Drei Punkte daraus verdienen Erläuterung.

**(1) Die `Spike-Ausnahme`-Zeile ist zum zweiten Mal in Folge komplett ausgefallen.** Punkt 2b (10) verlangt sie „bei JEDER Dual-Gate-Prüfung …, auch wenn daraus kein Entry wird — nicht nur beim finalen Trigger." Das Wort kommt im 470-Zeilen-Protokoll **null Mal** vor, obwohl der Dual-Gate-Status in praktisch jedem der 35 Voll-Checks bewertet wurde. Die Zeile wurde am 25.08. eingeführt, am 26.08. in Paket 2 ausdrücklich in den Tick-Prompt geschrieben, damit sie „nicht dasselbe Schicksal erleidet wie die übrigen Fließtext-only-Regeln" — und ist am ersten vollen Tag danach zu 0 % erschienen. **Das ist der klarste Beleg dieses Testtags dafür, dass die Verankerung im Tick-Prompt allein das Problem nicht löst.** Zur Fairness: inhaltlich wäre die Ausnahme an keiner Stelle einschlägig gewesen (kein Spike-Muster), der Ausfall ist folgenlos — aber genau das ist das Muster, das Fable am 05.08. vorhergesagt hat.

**(2) Die `Basis-Reclaim-Klasse` (8a4) hat am 27.08. an der falschen Stelle gefeuert und an der richtigen geschwiegen.** Um **17:55** vermerkt das Protokoll selbstkritisch: „Möglicher Basis-Reclaim-Kandidat (8a4) … volle 5-Bedingungs-Prüfung nicht mechanisch durchgerechnet, nur als Beobachtung vermerkt." Ich habe es nachgerechnet: **Bedingung 1 war dort NICHT erfüllt** (RSI am Tief 40,4, nicht ≤25; Kurs innerhalb S2/R2; kein News-Schock) — die Zeile war also gar nicht geschuldet, das schlechte Gewissen war unbegründet. Umgekehrt war Bedingung 1 ab **19:00** erfüllt: das Session-Extrem (29.612,85) entstand **außerhalb der Pivot-Range oberhalb R2**, was 8a4 Bedingung 1 wörtlich als Auslöser nennt. Dort erscheint keine Zeile.

Einschränkung, die ich ausdrücklich mache: Der **Geltungsbereich ist widersprüchlich**. 8a4 selbst formuliert die Pflichtzeile unabhängig von einer Order; Punkt 2b (11) stellt sie unter die Überschrift „Vor JEDER tatsächlichen Order/jedem Entry". Nach 2b wäre die Zeile also nicht geschuldet gewesen, nach 8a4 schon. Das ist eine Regelwerkslücke, kein eindeutiger Verstoß — aber sie sorgt dafür, dass die Klasse an einem Tag ohne Order nie feuern kann, egal wie gut das Muster passt.

**(3) Die 8d-Zeile hat ab 19:20 die Frage gewechselt.** [[feedback_vollcheck_format]] schreibt vor: `Regime-Gate (8d): Schock-Tag ✓/✗ (<Kriterien-Zahl>/3)`. Die drei Kriterien nach [[feedback_chartanalyse]] 8d sind Tagesrange vs. ATR(14)D, Häufung von Makro-Überraschungen und VIX-Intraday-Range. Ab 19:20 lautet die Zeile stattdessen „Regime-Gate (8d): Trend intakt (HH-HL 1H, Kurs >EMA50 …) — Regime bleibt ‚Trend', kein Chop-Signal". Das ist eine **andere Prüfung** (Trend vs. Chop statt Schock-Tag ja/nein) ohne den Zähler. Neun Voll-Checks in Folge. Folgenlos, weil der Tag unstreitig kein Schock-Tag war — aber es ist derselbe Zerfallspfad „Rohwert → Urteil → anderes Urteil", den das 25.08.-Review als Strukturbefund benannt hat.

---

## 9. Wurden die Pakete 1-6c tatsächlich exerciert?

Das Protokoll führt in seinem Kopf aus, der Tag laufe „mit dem kompletten seit 26./27.08. verschärften Regelwerk (Pakete 1-6c: zoneRrConsistencyGate, Stale-Klasse 7b1c, Bias-Synthese, Pivot-Pflichtschritt, 9b-Divergenz-Pflichtzeile, Befüllungspflicht Schattenmessfelder)". Tatsächlich exerciert wurden davon zwei von sechs Gruppen:

| Paket | Inhalt | am 27.08. exerciert? |
|---|---|---|
| 1 | `zoneRrConsistencyGate`, 5-Schritte-Ablauf, SL/ATR-Rohwertzeile, ATR-Zeitebene 5min | **nein** — feuert nur an einem Entry |
| 2 | Tick-Prompt-Vollständigkeit, ADX-Pflichtzeile, Session-Start-Indikator-Check | **teilweise** — ADX ✓ (32/35 mit Rohwert), Indikator-Check ✓ (auch nach dem CDP-Neustart), `Spike-Ausnahme` **0/35** |
| 3 | Pivot-Pflichtschritt, Bias-Synthese | **ja, und korrekt** — siehe unten |
| 5 | Entry-AVWAP-Anker | **nein** — nur nach Order |
| 6/6a-6c | Stale-Klasse 7b1c, 9b-Pflichtzeile, Befüllungspflicht, `dual_gate_*`/`kerzen_seit_gegenseite_*`/`pivot_distanz_punkte` | **nein** — alles order-gebunden; DB-Spalten bleiben leer |

**Paket 3 ist der klare Erfolg des Tages.** Ich habe die Pivots aus den genannten 26.08.-Werten (H 29.366,9 / L 29.028,7 / C 29.365,1) nachgerechnet: PP 29.253,567 · R1 29.478,433 · S1 29.140,233 · R2 **29.591,767** · S2 28.915,367 — **alle fünf exakt wie im Protokoll**. Auch die Fibonacci-Rechnung um 17:05 stimmt (29.558 − 0,618 × 492 = 29.253,9, Konfluenz mit PP korrekt erkannt). Und die Bias-Synthese ist genau so formuliert, wie Paket 3 sie verlangt: „**Kontext, kein Gate**". Der Pivot-Schritt hat den ganzen Tag getragen — R2 war das strukturierende Level von 17:15 bis 19:35.

**Der strukturelle Befund daraus:** Fünf der sechs Pakete — und insbesondere alle, die den 25.08.-Kernbefund (SL-Floor, Zonen-Konsistenz, Befüllungspflicht) reparieren sollen — sind **order-gebunden**. Ein Testtag ohne Order kann sie definitionsgemäß nicht prüfen. Nach drei Testtagen gilt deshalb weiterhin:

> **`skipped_setups` hat 0 Zeilen. `adx_at_entry`/`trend_effizienz` stehen bei 0 von 43 Trades. Die neuen Paket-6-Spalten haben n=0. Die gesamte seit dem 24.08. beschlossene Schattenmessung hat bis heute keinen einzigen Datenpunkt produziert.**

Vorschlag 10 des 25.08.-Reviews („B1/B2/B3-Schattenmessung an einem Testtag mindestens trocken durchlaufen") ist damit zum zweiten Mal nicht umgesetzt worden. Und Abschnitt 3.1 zeigt, dass es an diesem Tag sogar einen konkreten Anlass gab: den frischen NAS100-Reclaim um 17:55/18:00.

---

## 10. Technischer Vorfall 18:05 (CDP-Abbruch) — sauber gehandhabt

Der einzige Vorgang des Tages, den ich uneingeschränkt loben kann.

- **Keine offene Position** zum Zeitpunkt — korrekt festgestellt und explizit vermerkt.
- **`tv_launch`** erfolgreich, neue PID, TV 3.4.0 dokumentiert.
- **`pane_list`-Verifikation danach**: 2-Pane-Layout (NAS100 5min / QQQ 15min) bestätigt, Indikatoren vollständig, RVOL 0,81 plausibel, AVWAP-Fallback `in_0=0` benannt. Das ist exakt die Session-Start-Indikator-Checkliste aus Paket 2/3 ([[feedback_session_update]] Schritt 6), korrekt auf einen Neuverbindungsfall angewandt — sie stand dort für den Sessionstart, nicht für einen Reconnect, und wurde trotzdem gezogen.
- **Keine Datenlücke**: Quick-Ticks 18:02-18:04 und Voll-Check 18:05 schließen nahtlos an. Die Screenshot-Spur bestätigt das unabhängig (18:02:xx, dann 18:07:xx — Abstand konsistent mit dem Neustart).

Ein Restpunkt ohne Vorwurf: Der AVWAP-Anker blieb den ganzen Tag im Fallback (`in_0=0`). Das ist die seit dem 24.08. bekannte, offene Lücke, kein neuer Befund — es bedeutet aber, dass die 8a4-Bedingung 4 (VWAP-σ-Band 1) an diesem Tag gegen Session-Bänder gemessen worden wäre, was gemäß der in Paket 4 dokumentierten offenen Frage der derzeit einzig definierte Zustand ist.

---

## 11. Struktur-Reserve (1H HH-HL) — nachgerechnet, im Wesentlichen korrekt

Referenz ist das Tief der abgeschlossenen 1H-Kerze 18:00-19:00: **29.506,65**. Gegen dieses Tief wurde die laufende 1H-Kerze geprüft — methodisch richtig.

| Zeit | 1H-Low laut Protokoll | Reserve protokolliert | Reserve nachgerechnet |
|---|---|---|---|
| 19:35 | 29.570,95 | „ca. 64" | 64,30 ✓ |
| 19:40 | 29.567,85 | „ca. 61" | 61,20 ✓ |
| 19:45 | 29.563,05 (ggf. 29.561,15) | „ca. 56-58" | 56,40 / 54,50 — **Obergrenze 58 unbelegt** |
| 19:50 | 29.549,15 | „ca. 43" | 42,50 ✓ |
| 19:55 | 29.547,05 | „knapp 40" | 40,40 ✓ |
| 20:00 (Schluss) | 29.544,05 | „~37" | **37,40 ✓** |

Sechs von sechs im Rahmen, eine Spanne (19:45) zu großzügig nach oben gerundet. Auch die Gegenprobe stimmt: Die 19:00-20:00-Kerze schloss mit High 29.612,85 über dem vorherigen High (~29.594) und Low 29.544,05 über dem vorherigen Low 29.506,65 — **HH-HL formal intakt, wie behauptet**. Die Beobachtung ist korrekt und konsistent nachgerechnet; sie gehört zu den besseren Passagen des Protokolls.

---

## 12. Wiederkehrer-Check gegen 24.08. und 25.08.

| Befund | Herkunft | Status am 27.08. |
|---|---|---|
| Kerzenschluss-Boundary-Fehler | 24.08. Fehler 1 | **behoben** — 4× korrekt auf echten Schluss gewartet (17:38-17:43, 18:45-18:50, 19:00-19:05, 19:55-20:00), kein einziger Rückfall |
| Pane-Symbol-Verwechslung | 24.08. Fehler 4, 25.08. wieder | **kein Hinweis** — nach dem CDP-Neustart Pane-Zuordnung explizit verifiziert |
| UTC/Ortszeit-Verwechslung | 24.08. Fehler 3 | **behoben** — Stempel durchgehend als UTC gekennzeichnet, Deltas in Minuten |
| Dünnes Protokoll | 25.08. Abschnitt 9 / Vorschlag 6 | **behoben, deutlich** — 470 Zeilen, alle Voll-Checks im Wortlaut |
| `skipped_setups` leer | 24.08. + 25.08. | **unverändert n=0**, dritter Testtag |
| `Spike-Ausnahme` fehlt | 25.08. Abschnitt 8 | **erneut, diesmal vollständig (0/35)** |
| Pflichtzeile trägt Urteil statt Rohwert | 25.08. Abschnitt 6 (Strukturbefund) | **erneut** — 8d 9×, ADX 3×, 5min-RSI 3× („implizit"), EMA50 als Spanne um 20:00 |
| Skript-Aufruf nicht belegt | 25.08. Vorschlag 2 | **erneut** — `cooldown_check.cjs` nur als „🟢" |
| ATR-Nenner-Uneinheitlichkeit (B3) | 25.08. Abschnitt 12 | **nicht prüfbar** — ATR taucht nach 17:20 gar nicht mehr im Protokoll auf, mangels Trade auch nicht nötig |
| Widersprüchliche Tweet-Fälligkeit | 24.08. Vorschlag 3 | **nicht bereinigt → jetzt mit Folgen** (Abschnitt 2.4) |
| Selbstbewertung des eigenen Tages | 25.08. Abschnitt 3 („Lehre") | **erneut, milder** — Tagesabschluss vergibt „regelkonform" (Abschnitt 4.4) |
| Prozessfehler selbst offengelegt | 24.08.: 3 · 25.08.: 3 | **27.08.: 0** — und der sichtbarste (Doku-Bruch 19:11) blieb unerwähnt |

Die obere Hälfte dieser Tabelle ist echter Fortschritt, und zwar nicht wenig: Vier dokumentierte Fehlertypen sind an diesem Tag nicht wieder aufgetreten, darunter der 24.08.-Kerzenschluss-Bug an vier unabhängigen Gelegenheiten. Die untere Hälfte ist derselbe Strukturbefund wie am 25.08., in neuen Kleidern.

---

## 13. Was heute wirklich gut war

Nicht als Ausgleich, sondern weil es unabhängig belegbar ist:

- **Das Protokoll selbst.** 470 Zeilen, jeder Voll-Check im Wortlaut, Screenshot-Spur, Zeitstempel. Erst dadurch ist dieses Review möglich — und erst dadurch sind die Befunde in Abschnitt 2 überhaupt auffindbar. Ein dünneres Protokoll hätte einen „sauberen Tag" gemeldet.
- **Die 17:45-Entscheidung.** NAS100 2/2, 15min gebrochen, QQQ 0,37 Punkte darüber → kein Entry. Zehn Minuten später löste sich das Setup vollständig auf. Das ist die einzige echte Gate-Entscheidung des Tages, und sie war richtig.
- **Die Vorab-Festlegung um 18:45 und 19:00.** Zweimal schriftlich, 25 bzw. 10 Minuten vor der Versuchung, mit der korrekten Regel. Das ist die belastbarste Form von Disziplin-Beleg, die ein Protokoll überhaupt liefern kann.
- **Kein Trigger-Ersatz aus dem DI-Cross gemacht** (19:46), obwohl er verlockend war und die Bewegung ihm recht gab — [[feedback_chartanalyse]] Punkt 6 sauber eingehalten.
- **Paket 3 vollständig und arithmetisch korrekt exerciert** (Pivots auf drei Nachkommastellen, Bias-Synthese als „Kontext, kein Gate", Fibonacci-Konfluenz korrekt).
- **Der CDP-Vorfall** (Abschnitt 10).
- **Kein fiktiver Trade in `trades.db`** — die Trennung fiktiv/real wurde sauber eingehalten, `trades` steht unverändert bei 43.
- **Die Struktur-Reserve-Nachrechnung** stimmt in sechs von sechs Fällen.

---

## 14. Priorisierte Vorschlagsliste

Kennzeichnung wie an den Vortagen: **(a)** = Prozess/Tooling, keine Regeländerung · **(b)** = echte Regeländerung, braucht Levis Zustimmung. **Nichts davon ist umgesetzt.**

**1. (a, höchste Priorität) Den `x_last_fetch.json`-Zeitstempel maschinell setzen, nicht von Hand.**
Der Stempel liegt nachweislich bis zu 7 Minuten neben dem tatsächlichen Schreibzeitpunkt (Datei-Änderungszeit 19:56 vs. Inhalt 19:49:04). Solange der Rohwert selbst falsch ist, hilft keine Formatvorschrift. Konkret: ein Einzeiler-Skript `scripts/x_touch.cjs`, das `new Date().toISOString()` schreibt und das Delta gegen den vorherigen Wert direkt als fertige Pflichtzeile ausgibt — dann steht in der Zeile ein gemessener statt eines erinnerten Werts, und der Fehler aus Abschnitt 2 kann nicht mehr entstehen.

**2. (a) Die verbliebene Ordinal-Fälligkeit aus [[feedback_live_trading]] Punkt 9 streichen.**
An drei Stellen ist die alte Mechanik durchgestrichen, an einer nicht: „bleibt bei jedem 2. Voll-Check (alle 10 Min)". Genau sie wurde am 27.08. fünfmal statt der Delta-Prüfung verwendet. Das ist die exakte Wiedervorlage von Opus-Vorschlag 3 vom 24.08. — er wurde zu 75 % umgesetzt, und die fehlenden 25 % haben den Ausfall verursacht. Reine Textbereinigung, keine inhaltliche Änderung.

**3. (a) Skript-Aufrufe im Protokoll belegen — jetzt auch `cooldown_check.cjs`.**
Punkt 2b (7) verlangt bereits die „Kernzeile der Ausgabe". Am 27.08. stand „🟢". Empfehlung: dieselbe Behandlung wie bei `gate_check.cjs` in Vorschlag 2 vom 25.08. — die tatsächliche Ausgabezeile wörtlich. Es geht nicht um Misstrauen, sondern darum, dass die einzige prüfbare Aussage der 19:10-Szene derzeit unprüfbar ist.

**4. (a) Die 8d-Zeile auf ihr vorgeschriebenes Format zurückholen — und die Trend/Chop-Aussage daneben stellen, nicht darin.**
Die ab 19:20 verwendete Formulierung beantwortet eine sinnvolle, aber andere Frage. Vorschlag: `Regime-Gate (8d): Schock-Tag ✗ (0/3) | Regime: Trend/Chop` — der Zähler bleibt Pflicht, die Zusatzaussage ist willkommen. Dasselbe für die drei ADX-Zeilen ohne Zahl und die drei „RSI ~51 (implizit)"-Angaben: ein abgelesener Wert oder ein ausdrückliches „nicht gelesen", nichts dazwischen.

**5. (a/b, Levi entscheidet) Den Geltungsbereich der order-gebundenen Anzeige-Zeilen klären.**
Fünf der sechs Pakete feuern nur „vor einer Order". An einem Null-Trade-Tag messen sie deshalb nichts — und `skipped_setups` steht nach drei Testtagen bei 0. Die Widersprüche sind konkret benennbar: 8a4 formuliert seine Pflichtzeile order-unabhängig, Punkt 2b (11) stellt sie unter „Vor JEDER Order"; die `Spike-Ausnahme` ist ausdrücklich order-unabhängig und fiel trotzdem zu 100 % aus; die Stale-Klasse 7b1c kann den Fall, für den sie gebaut wurde (altes Gate, kein Trade), strukturell nie erfassen. Zwei saubere Wege: **(i)** die drei Anzeige-Klassen (8a4, 7b1c, Spike) an **Trigger-Momente** statt an Orders binden — also immer dann, wenn eine Dual-Gate-Hälfte frisch schließt oder ein benanntes Level bricht **(a)**; oder **(ii)** bewusst akzeptieren, dass die Schattenmessung erst mit echten Trades anläuft, und die Erwartung an Testtage entsprechend zurückschrauben **(b)**. Meine Empfehlung: **(i)** — sie kostet nichts, kontaminiert kein Fenster und hätte am 27.08. mindestens drei Datenpunkte erzeugt (17:45 Short-Trigger-Kandidat, 17:55/18:00 NAS100-Reclaim, 19:00-19:10 R2-Ausbruch).

**6. (a) Doppeldeutigkeit von „stale" auflösen.**
Seit dem 27.08. ist „Stale" ein definierter Begriff (7b1c: altes Gate **plus** Markt läuft dagegen). Das Protokoll benutzt dasselbe Wort für „kein frischer Cross als Trigger verfügbar" — am 19:10 waren beide Bedeutungen gleichzeitig im Text und widersprachen sich (2/5 nach 7b1c). Vorschlag: für den Trigger-Zustand ein anderes Wort („kein frischer Cross" / „Gate nicht neu ausgelöst") und „Stale" ausschließlich für 7b1c reservieren. Reine Terminologie, kein Regelinhalt — aber sonst wird die erste Auswertung der Klasse nach 10-15 Fällen auf vermischtem Material laufen.

**7. (a) Zustandsänderungen im Protokoll offenlegen — auch technische.**
Der Wechsel um 19:11 (Screenshots aus, Template neu, ein Voll-Check ausgelassen) ist nirgends erwähnt. Vorschlag: eine einzeilige Notiz bei jedem Bruch in der Beobachtungskette („ab 19:17: keine Screenshots mehr, Grund X"), analog zur bereits vorbildlich dokumentierten CDP-Meldung um 18:05. Ohne das ist im Nachhinein nicht unterscheidbar, ob eine Beobachtung ausblieb oder nur nicht aufgeschrieben wurde.

**8. (a) Voll-Check-Nummerierung aus der Uhrzeit ableiten, nicht mitzählen.**
Die Zählung sprang von (3.) auf (5.) und meldete am Ende „36." bei 35 tatsächlichen Checks; der einzige echte Ausfall (19:15) blieb dadurch unsichtbar. Wenn die Nummer aus der Startzeit berechnet wird, fällt jede Lücke sofort auf.

**9. (b, braucht Zustimmung) Testtag-Auswahl: mindestens eine Gate-Öffnung als Zielkriterium.**
Drei Testtage, drei sehr unterschiedliche Erkenntnisdichten. Am 27.08. gab es genau eine echte Dual-Gate-Entscheidung; die Long-Seite war strukturell gesperrt (Abschnitt 6.2). Ein Tag, an dem das Gate nie öffnen kann, testet die Entry-Kette nicht — und die Entry-Kette ist genau das, was die Pakete 1 und 6 repariert haben. Zwei Optionen: entweder Testtage bewusst an Tagen mit Rücksetzer-Struktur ansetzen, oder die Entry-Kette einmal offline gegen historische Bars durchspielen (Replay), statt auf einen passenden Live-Tag zu warten. **Ausdrücklich keine Regeländerung am Gate** — nur an der Testtag-Auswahl.

**10. (a) Rollentrennung im Tagesabschluss einhalten.**
Der Protokoll-Abschluss sollte Fakten enthalten (was geschah, welche Zahlen, welche Vorfälle) und keine Bewertung („wichtigster Vorfall", „regelkonform", „zentraler Datenpunkt für das Review"). Levis eigene Vorgabe von 18:14 sagt das bereits; es fehlt nur die Umsetzung im Ausgabeformat. Konkret: der letzte Protokollabschnitt bekommt eine feste Überschrift „Faktenprotokoll-Abschluss" statt „Tagesabschluss", und der Wertungsteil entfällt.

---

## 15. Was ich ausdrücklich NICHT vorschlage

- **Keine Änderung am Dual-Gate.** Die 17:45-Entscheidung war richtig und wurde vom Markt zehn Minuten später bestätigt. Die Long-Seiten-Blockade ist real (Abschnitt 6.2), aber sie ist ein bekanntes, seit dem 24.08. dokumentiertes Timeframe-Thema, dessen Entscheidung ausdrücklich am #50-Review hängt (B4). Phase 3 läuft bis #50, das 15-Trade-Sizing-Fenster läuft, [[feedback_dont_change_running_system]] und Punkt 14 gelten unverändert.
- **Keine neue Pflichtzeile.** Der Tag hat gezeigt, dass eine bestehende Zeile zu 100 % ausfallen kann (`Spike-Ausnahme`), obwohl sie tags zuvor eigens in den Tick-Prompt geschrieben wurde. Vorschläge 1-8 ändern Form, Quelle oder Geltungsbereich bestehender Zeilen; keiner fügt eine hinzu.
- **Keine Umdeutung von „0 Trades" zur bestandenen Disziplinprobe.** Es gab genau eine echte Gate-Entscheidung. Ein Tag mit einer Entscheidung belegt keine Disziplin über drei Stunden — Abschnitt 6.3.
- **Keine Abwertung der 19:10-Szene.** Die Vorab-Festlegung um 18:45 und 19:00 ist echt und stark. Was ich zurückweise, ist nur die Selbstbescheinigung im Tagesabschluss, nicht der Vorgang.
- **Keine Lockerung der 10-Minuten-Tweet-Schwelle**, um die fünf Aussetzer nachträglich regelkonform zu machen. Die Schwelle hat Levi am 25.08. bewusst gesetzt; der Befund ist ein Ausführungs-/Textbefund, kein Kalibrierungsbefund.
- **Kein Vorwurf aus den nicht vorhandenen positionsgebundenen Zeilen** (Stall-Check, Positions-Kasten, Volumen-Check, Chop-Check). Sie waren korrekt nicht geschuldet.

---

## 16. Ehrliche Gesamteinordnung

Der 27.08. ist der bestdokumentierte und in der Ausführung disziplinierteste der drei Testtage — und trotzdem nicht der sauberste.

Was gut war, ist echt und lässt sich mit Zahlen belegen: die 17:45-Entscheidung gegen ein Setup, dem nur 0,37 Punkte auf der QQQ-Seite fehlten und das sich zehn Minuten später selbst widerlegte; die zweifache schriftliche Vorab-Festlegung um 18:45 und 19:00, die die 19:10-Szene trägt; der vollständig und arithmetisch korrekt exercierte Pivot-/Bias-Schritt aus Paket 3; die saubere Behandlung des CDP-Ausfalls; und ein Protokoll, das erstmals seit dem 24.08. wieder ein echtes Review erlaubt.

Aber der Tag hat auch etwas Unbequemes gezeigt, und es ist derselbe Strukturbefund wie am 25.08., nur an einer neuen Stelle: **Die Pflichtzeile, die das letzte Review als einzige funktionierende gelobt und zum Vorbild der ganzen Reform gemacht hat, ist an diesem Tag gebrochen — fünf versäumte Pflicht-Fetches, mehrere Deltas, die mit den eigenen Zeitstempeln nicht zusammengehen, und ein Rohwert, der nachweislich bis zu sieben Minuten neben seinem eigenen Messzeitpunkt liegt.** Der Grund ist nicht Nachlässigkeit allein: eine Regelwerksstelle, die am 24.08. zur Bereinigung vorgeschlagen und zu drei Vierteln bereinigt wurde, steht noch da und wurde befolgt. Das ist die schärfste Lehre des Tages: **Eine zu 75 % bereinigte Widersprüchlichkeit wirkt wie eine gar nicht bereinigte.**

Zur Gewichtung, damit das Bild stimmt: Es ist ein fiktiver Tag, es gab null Trades, null Risiko, und keiner der Befunde hatte eine Marktfolge. Kein versäumter Tweet-Fetch hätte etwas geändert; keine fehlende `Spike-Ausnahme`-Zeile war inhaltlich einschlägig; die 8d-Zeile hätte in jeder Fassung „kein Schock-Tag" gesagt. Der Wert liegt ausschließlich darin, dass ein sehr ausführliches Protokoll drei Dinge sichtbar gemacht hat, die sonst unsichtbar geblieben wären: dass die Rohwert-These an ihrem eigenen Vorzeigebeispiel scheitern kann, dass „stale" inzwischen zwei Bedeutungen hat und beide am selben Tag falsch verwendet wurden, und dass fünf von sechs Reform-Paketen an einem Null-Trade-Tag konstruktionsbedingt nichts messen.

Und ein letzter Satz zur Testtag-Konstruktion, weil die Auftragsfrage danach ausdrücklich fragte: Der Tag hat die **Beobachtungs**-Disziplin gut geprüft und bestanden. Die **Entry**-Disziplin hat er nur an einer einzigen Stelle geprüft (19:10) und dort bestanden. Die **Rechen**-Disziplin — den Bereich, in dem am 25.08. alle Verstöße lagen und für den seither fünf Pakete gebaut wurden — hat er überhaupt nicht geprüft. Nach drei Testtagen ist die Entry-Rechenkette weiterhin genau einmal live gelaufen, nämlich am 25.08., und dort war sie fehlerhaft. Das ist die Lücke, die vor einem Echtgeld-Start geschlossen sein sollte, und sie schließt sich nicht dadurch, dass ein Tag ohne Trade gut verläuft.

---

*Levi entscheidet über alle Vorschläge. Bis dahin ist nichts umgesetzt, keine Regel geändert, keine Datei außer dieser angelegt.*
