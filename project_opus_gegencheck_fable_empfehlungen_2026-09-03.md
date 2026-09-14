---
name: project-opus-gegencheck-fable-empfehlungen-2026-09-03
description: "Unabhängige Opus-Zweitmeinung zu Fables fünf Entscheidungsvorlagen aus der Auftragsliste 03.09.2026 (#3 8d-Kriterium 2, #11 8d-Kriterium 3/VIX, #12 Abbruch-Gate, #21 Register Herkunft vs. Tragfähigkeit, #22 Frischefenster + Registeralter): 2× Zustimmung mit Vorbehalt, 2× Zustimmung mit Korrektur am Zahlenwert bzw. an der Framing, 1× Widerspruch (#11: Option C statt A). Zentraler eigener Befund: unter Fables Empfehlungen für #3 und #11 sind ALLE DREI 8d-Kriterien intraday monoton nicht-fallend — 8d wird zur Einbahn-Sperre, die faktisch nur noch der Makro-Kalender entscheidet. Zusätzlich Konsistenzprüfung der bereits umgesetzten Fixes #1/#2/#9/#20 gegen den realen Code."
metadata:
  node_type: memory
  type: project
  status: draft
  originSessionId: e1a9edc1-4a4d-4c86-b03e-98d36d93627e
  modified: 2026-09-03T10:22:08.629Z
---

# Opus-Gegencheck: Fables 5 Empfehlungen zu den Regelwerk-Fragen (03.09.2026)

**Prüfgegenstand:** [[project_fable_umsetzung_2026-09-03]], Abschnitt „REGELWERK-FRAGE (5 Entscheidungsvorlagen)" — Fables Optionen und Empfehlungen zu #3, #11, #12, #21, #22.
**Prüfer:** unabhängige Opus-Instanz, kein Vorkontext zur laufenden Session.
**Gegengelesen:** [[project_fable_auftragsliste_2026-09-03]] (Ursprungsrahmung der 5 Fragen), [[project_testtag_analyse_2026-09-01]] und [[project_testtag_analyse_2026-09-02]] (Sachverhalte hinter den Fragen), [[feedback_chartanalyse]] Punkt 8b1a/8c/8d (der geltende Regeltext), [[project_testtag_plan_01_02_09_2026]] (Wortlaut der vier Abbruch-Kriterien), `scripts/gate_check.cjs` und `scripts/level_register.json` im aktuellen, von Fable geänderten Zustand.
**Keine Regeländerung, kein Code, kein MEMORY.md-Eintrag durch diese Prüfung.**

**Bilanz vorab: 1× volle Zustimmung (#21, mit Präzisierung), 2× Zustimmung mit Vorbehalt (#3, #12), 1× Zustimmung im Prinzip aber mit falschem Zahlenwert (#22), 1× Widerspruch (#11).**

---

## 0. Ein Befund, der über alle fünf Fragen liegt: 8d wird unter Fables Empfehlungen zur Einbahnstraße

Fable hat #3 und #11 als zwei unabhängige Fragen behandelt. Sie sind es nicht — beide betreffen dasselbe Gate, und ihre Kombination hat eine Eigenschaft, die in keiner der beiden Vorlagen auftaucht. Ich habe sie nachgerechnet.

Der geltende 8d-Text ([[feedback_chartanalyse]], Punkt 8d) definiert:

- **K1** = heutige Tagesrange (Hoch−Tief) / ATR(14)D > 2×
- **K2** = Häufung von Makro-Überraschungen
- **K3** = VIX-Intraday-Range (Hoch−Tief)/Tief > 5 %

**Monotonie-Rechnung über einen Handelstag:**

| Kriterium | Zähler | Nenner | Verlauf intraday |
|---|---|---|---|
| K1 | Tageshoch − Tagestief: **nicht-fallend** | ATR(14)D: bei Sessionstart einmal gelesen, **konstant** | **monoton nicht-fallend** |
| K3 | VIX-Hoch − VIX-Tief: **nicht-fallend** | VIX-Tief: **nicht-steigend** | **monoton nicht-fallend** |
| K2 unter Fables Empfehlung C („Zustand, fortbestehend bis Tagesende") | — | — | **gelatcht, nicht-fallend** |

Damit gilt: **unter Fables Empfehlungen für #3 (C) und #11 (A, Formel bleibt) sind alle drei 8d-Kriterien intraday monoton nicht-fallend.** 8d kann über einen Handelstag nur hochratschen, nie zurückfallen. Sobald einmal 2/3 erreicht ist, gilt über [[feedback_chartanalyse]] 8b1a „Schock-Tier = No-Trade" bis Handelsschluss — unwiderruflich, unabhängig davon, was der Markt danach tut.

Fables ausdrückliches Gegenargument in der #3-Vorlage lautet: *„8d-K1 (Tagesrange) bildet nachlassende Wirkung ohnehin ab."* **Das ist sachlich falsch.** K1 ist eine kumulative Tagesgröße und kann im Tagesverlauf nicht sinken; sie bildet nachlassende Wirkung nicht ab, sondern das genaue Gegenteil. Der einzige tragende Grund gegen Option B (Verfallsfenster) in Fables #3-Vorlage hält damit nicht.

**Und der Ratschen-Effekt hat eine zweite Stufe.** Weil K3 in seiner jetzigen Form praktisch dauerhaft erfüllt ist (Begründung unter #11), degeneriert 8d zu:

> „≥2 Termine der Standard-Blackout-Liste heute → K2 ✓ → zusammen mit dem ohnehin stehenden K3 = 2/3 → Schock-Regime → 8b1a No-Trade für den ganzen Tag."

Das ist **kein Markt-Regime-Gate mehr, sondern ein Makro-Kalenderfilter.** Er wird beim ersten Voll-Check des Tages entschieden und lässt sich danach durch keinerlei Marktverhalten mehr korrigieren. Ein Tag mit ADP + ISM (oder Claims + PPI, oder Claims + Philly Fed) — nichts Seltenes — wäre unter Fables Empfehlungspaar ein Nicht-Handelstag, egal wie ruhig der Markt läuft.

**Beleg am eigenen Material:** Am 02.09. war K1 = 0,48× bis 0,62× (Schwelle 2×), also weit von erfüllt entfernt, während K3 den ganzen Tag von 5,52 % auf 11,24 % *anstieg* — bei fallendem VIX. Es fehlte nur ein zweiter Blackout-Termin (der Tag hatte genau einen, ADP), und der als „bester Testtag der Serie" und in Abschnitt 9 als bullische Erholungsphase beschriebene 02.09. wäre ein durchgehender No-Trade-Tag gewesen — aus dem Kalender, nicht aus dem Markt. Für Tag 1 gilt dasselbe Muster mit umgekehrtem Vorzeichen: 8d stand dort über weite Strecken auf 2/3, obwohl K1 nicht erfüllt war — rechnerisch geht das nur über K2+K3.

**Konsequenz für die Entscheidung:** #3 und #11 dürfen nicht getrennt entschieden werden. Wer #3 = C wählt (Latch), braucht bei #11 zwingend ein Kriterium, das zurückfallen kann — sonst hat 8d keinen einzigen rückstellbaren Freiheitsgrad mehr. Genau das leistet Fables #11-Empfehlung A nur scheinbar (siehe dort).

**Empirische Vorabprüfung, die ich Levi vor beiden Entscheidungen empfehle** (kostet eine halbe Stunde, entscheidet die Frage härter als jede Argumentation): Über die letzten 20 Handelstage auszählen, (a) an wie vielen ≥2 Termine der Standard-Blackout-Liste lagen und (b) an wie vielen die VIX-Intraday-Range ≥5 % betrug. Das Produkt beider Quoten ist der Anteil der Tage, an denen 8d unter Fables Empfehlungspaar den Handel komplett sperren würde. Liegt der über ~30 %, ist das Paar für ein System mit Renditeziel EV ≥ 0,5 %/Trade nicht tragbar.

---

## 1. #3 — Definition von 8d-Kriterium 2 (Makro-Häufung)

**Fables Empfehlung: Option C** (= Option A „Zustand, fortbestehend" plus die Tag-2-Eventdefinition „≥2 Events von der Standard-Blackout-Liste am selben Tag; bloße Marktreaktionen/News/Nicht-US-Events zählen nicht").

**Mein Urteil: Zustimmung mit Vorbehalt — bei der Zustandsfrage ja, bei der Eventdefinition nein.**

### Was ich unabhängig bestätige

Die Zustandslogik (A/C) gegenüber dem Verfallsfenster (B) trage ich mit, aber mit einer anderen Begründung als Fable. Fables Begründung („K1 bildet die nachlassende Wirkung ab") ist, wie oben gezeigt, falsch. Meine Begründung ist eine andere: **Ein Verfallsfenster würde einen zweiten frei kalibrierbaren Parameter (60? 90? 120 Minuten?) in ein Gate einführen, dessen bestehende Parameter nachweislich nie kalibriert wurden** — die 2×-Schwelle in K1 ist als „z.B. Faustregel" im Regeltext von Juli 2026 eingeführt, die 5 %-Schwelle in K3 ebenso. Ein neuer, ebenfalls ungeprüfter Parameter in einem Gate mit drei ungeprüften Parametern macht das Ergebnis nicht robuster, sondern nur schwerer nachzuvollziehen. Der Latch ist die einfachere Regel — **aber nur, solange mindestens ein anderes Kriterium zurückfallen kann.** Deshalb ist meine Zustimmung an #11 gekoppelt (Abschnitt 2).

Zusätzlich unabhängig bestätigt: Der Tag-1-Fehler war tatsächlich die Umdeutung *mitten in der Session*, nicht die Wahl der Auslegung. Zwischen #31 und #40 galt „neues Ereignis nötig", davor und danach „Zustand hält an" — und der einzige `--tier normal`-Aufruf des Tages lag exakt in diesem Fenster. Dass das Ergebnis (FAIL) unverändert blieb, ist Glück, nicht Struktur. Eine schriftlich fixierte Definition ist überfällig, unabhängig davon, welche.

### Wo ich widerspreche: Option C ist keine Festschreibung, sondern eine stille Regeländerung

Fable rahmt C als „Tag-2-Praxis festschreiben" und die Fragestellung insgesamt als „es gab keine Definition". **Beides stimmt nicht.** Der geltende Regeltext ([[feedback_chartanalyse]] 8d, Punkt 2) lautet wörtlich:

> „**Häufung von Makro-Überraschungen:** Gab es heute **oder in den letzten 1–2 Tagen** mehrere große **Datenüberraschungen (Konsens-Abweichungen)**, nicht nur eine?"

Die Tag-2-Praxis, die C festschreiben will, weicht davon in zwei materiellen Punkten ab:

1. **Sie streicht den 1–2-Tage-Rückblick** ersatzlos („am selben Tag").
2. **Sie ersetzt „Datenüberraschung (Konsens-Abweichung)" durch „Termin auf der Blackout-Liste".** Das sind zwei verschiedene Begriffe: Ein ADP-Wert exakt auf Konsens ist ein Blackout-Termin, aber keine Überraschung. Eine CPI-Zahl, die den Konsens deutlich verfehlt, ist eine Überraschung — und stünde auf der Blackout-Liste, andere Überraschungen (Zentralbank-Zitate, Revisionen, geopolitische Datenpunkte) nicht.

Damit ist C **inhaltlich näher an Tag 1 als an der geschriebenen Regel** — nur mit anderem Vorzeichen: Tag 1 zählte Marktereignisse (Hormuz, Iran-MOU, JP10Y, Fed-Odds) als „Überraschungen", was dem Wortlaut („Konsens-Abweichungen") ebenfalls nicht entsprach, aber dem Sinn (unerwartete Information) näher kam. Tag 2 zählte Termine statt Überraschungen, was dem Sinn ferner, aber der Auditierbarkeit näher steht.

Fables Optionsliste benennt diesen Zielkonflikt nirgends und enthält die geltende Regel nicht als Option. **Das ist der Rahmungsfehler in dieser Vorlage:** Levi bekommt drei Varianten zur Auswahl, von denen keine die Regel ist, die formal gerade gilt — und die empfohlene Variante ändert die Semantik von „Überraschung" auf „Termin", ohne dass die Vorlage das als Änderung ausweist.

### Meine Empfehlung: Option D (in Fables Liste nicht enthalten)

> **K2 ist erfüllt, wenn am selben Handelstag ≥2 Ereignisse eintreten, die (a) auf der Standard-Blackout-Liste stehen UND (b) messbar vom Konsens abgewichen sind (Ist- und Konsenswert werden im Voll-Check dokumentiert). K2 ist ein Zustand: einmal erfüllt, gilt es bis Handelsschluss. Der 1–2-Tage-Rückblick des bisherigen Wortlauts entfällt ausdrücklich (Begründung: 8d ist ein Intraday-Gate; ein Rückblick über Kalendertage ist über PDH/PDL und ATR bereits in den Preisdaten enthalten).**

**Warum D besser ist als C:**

- Es hält die Auditierbarkeit, die Tag 2 über 84 Checks bewiesen hat (die Blackout-Liste ist eine geschlossene, nachprüfbare Menge), und rettet gleichzeitig den Kern der geschriebenen Regel (Überraschung, nicht Termin).
- Der Zusatzaufwand ist null: Der Loop holt die Konsenswerte über den Tweet-/News-Check ohnehin — auf Tag 1 sind Konsens-Abweichungen im Protokoll dokumentiert (Chicago PMI 47,1 vs. Konsens 57,9 am 28.08. ist der Präzedenzfall, der 8d überhaupt erst in die Analyse gebracht hat).
- Es entschärft den Kalenderfilter-Effekt aus Abschnitt 0 erheblich: Zwei *Termine* sind häufig, zwei echte *Konsens-Abweichungen* deutlich seltener. Damit sinkt der Anteil struktureller No-Trade-Tage, ohne dass die Schutzfunktion verloren geht — im Gegenteil: Ein Tag mit zwei echten Datenüberraschungen ist genau der Tag, für den 8d gebaut wurde.
- Die Streichung des Rückblicks wird zur bewussten Entscheidung statt zum Nebeneffekt einer Formulierung.

**Kosten/Risiken meiner Empfehlung:** (i) „Messbar vom Konsens abgewichen" braucht eine Untergrenze, sonst zählt jede Nachkommastelle — Vorschlag: die Abweichung muss im Protokoll mit Ist/Konsens genannt sein und die Marktreaktion muss benannt werden; keine Prozentschwelle, um keine vierte unkalibrierte Zahl einzuführen. (ii) Konsenswerte sind nicht immer sofort verfügbar → dann gilt „nicht gemessen" nach der bestehenden 8d-Frische-Pflicht vom 31.08., nicht ✓. (iii) Wie C ist auch D ein Latch und braucht deshalb ein rückstellbares K3 (siehe #11).

---

## 2. #11 — Formel von 8d-Kriterium 3 (VIX)

**Fables Empfehlung: Option A** (Formel (Hoch−Tief)/Tief bleibt, plus Richtungsfilter: K3 zählt nur, wenn der aktuelle VIX ≥ Vortagesschluss ODER näher am Tageshoch als am Tagestief liegt).

**Mein Urteil: Widerspruch. Ich empfehle Option C (Niveau ODER Tagesanstieg).**

### Der Befund ist größer, als die Vorlage ihn darstellt

Fable beschreibt den Fehlmodus als „misst Entspannung als Schock". Das ist richtig, aber es ist nur die halbe Diagnose. Der eigentliche Defekt ist: **die Formel hat keinen absoluten Anker und ist deshalb nahezu dauerhaft erfüllt.**

Rechnung: Bei VIX-Niveau 15 entsprechen 5 % Intraday-Range **0,75 VIX-Punkten**. Bei VIX 20 sind es 1,0 Punkte. Eine Tagesspanne dieser Größe ist am VIX kein Ausnahmezustand, sondern das Normalverhalten — der VIX schwankt intraday routinemäßig um 0,8 bis 1,5 Punkte. Die Schwelle ist damit auf niedrigem VIX-Niveau faktisch immer gerissen, und zwar **umso leichter, je ruhiger der Markt ist** (kleinerer Nenner). Das ist die Umkehrung dessen, was das Kriterium messen soll.

Zwei Datenpunkte aus dem eigenen Material stützen das: An beiden Testtagen war K3 erfüllt, an beiden lag der VIX unter 17, an Tag 2 fiel er monoton. **In zwei von zwei beobachteten Fällen hat das Kriterium gefeuert, ohne dass ein Vola-Schock vorlag.** Ein Kriterium mit dieser Trefferquote trägt null Information ins Gate und macht 8d faktisch zu einem 2-Kriterien-Gate mit Schwelle 1.

### Warum Option A den Defekt nicht behebt

Ich habe Fables Richtungsfilter gegen den 02.09. durchgerechnet — er behebt genau den einen protokollierten Fall und erzeugt dabei zwei neue Probleme:

**(a) Der Filter lässt normale Tage weiter durch.** Zweite Klausel: „näher am Tageshoch als am Tagestief". Bei VIX-Hoch 16,82 / Tief 15,12 liegt die Mitte bei 15,97. Steht der VIX bei 16,0 — also mitten in einer völlig unauffälligen Seitwärtsspanne zwischen 15 und 17 —, ist er näher am Hoch, und K3 feuert mit 11,24 %. Der 02.09. wäre also nicht sauber ausgefiltert, sondern nur zeitweise: nachmittags, als der VIX gefallen war, aus; vormittags, als er noch oben stand, an.

**(b) Genau daraus folgt das schlimmere Problem: K3 flackert.** Der aktuelle VIX pendelt im Tagesverlauf um die Hoch/Tief-Mitte, und mit ihm kippt K3 hin und her — an einem Tag mit gelatchtem K2 also 8d zwischen 1/3 und 2/3, also der `--tier`-Parameter zwischen `normal` und `schock`, also 8b1a zwischen „Trade erlaubt" und „No-Trade". **Ob ein Trade zulässig ist, hinge dann davon ab, in welcher Minute der Trigger fällt.** Das ist derselbe Nicht-Reproduzierbarkeits-Fehler, den #3 gerade beseitigen soll — nur diesmal nicht durch menschliche Umdeutung, sondern durch die Formel selbst. Die Vergleichsgröße („näher am Hoch als am Tief") ist zudem ein reiner Mittelwert-Vergleich ohne ökonomische Bedeutung; ein Flackern an dieser Kante trägt keine Information.

Fables Begründung für A lautet: „behebt exakt den belegten Fehlmodus ohne neue Schwellenkalibrierung; die bestehende %-Schwelle bleibt vergleichbar mit der bisherigen Messreihe." Der Vergleichbarkeitsvorteil ist real, aber wertlos: **Die „bisherige Messreihe" besteht aus einem Kriterium, das in 100 % der beobachteten Fälle erfüllt war.** Eine Reihe konstanter Einsen ist keine Messreihe, die man erhalten müsste.

### Warum ich Option C empfehle — und warum sie billiger ist, als Fable annimmt

> **K3 = VIX ≥ 20 (Absolutschwelle) ODER VIX-Tagesveränderung gegenüber Vortagesschluss ≥ +10 %.**

**Argumente:**

1. **Der VIX ist bereits ein Niveau, kein Preis.** Ihn zusätzlich zu normieren (durch das Tagestief zu teilen) zerstört die Information, die er trägt. Ein VIX von 28→31 ist Panik bei 10,7 % Range; ein VIX von 15,1→16,8 ist Ruhe bei 11,2 % Range. Die aktuelle Formel bewertet den Ruhe-Tag höher. Das ist kein Kalibrierungsproblem, sondern ein Dimensionsfehler.
2. **Fables „größter Kalibrierungsaufwand" trifft nicht zu.** Die Anker existieren extern und sind nicht selbst zu erfinden: VIX 20 ist die konventionelle Grenze zwischen ruhig und erhöht, und die Tagesveränderung gegenüber Vortagesschluss ist die Größe, in der Vola-Schocks überall berichtet werden. Demgegenüber ist die geltende 5 %-Schwelle **noch nie kalibriert worden** — sie steht seit 02.07.2026 als Faustregel im Text. C ersetzt keine kalibrierte Schwelle durch eine unkalibrierte, sondern eine unkalibrierte durch eine konventionell verankerte. Das ist die *erste* Kalibrierung, nicht Mehraufwand.
3. **C ist rückstellbar und löst damit das Ratschen-Problem aus Abschnitt 0.** Sowohl das Niveau als auch die Tagesveränderung können im Tagesverlauf fallen. Damit behält 8d unter dem K2-Latch aus #3 genau einen Freiheitsgrad, der Entspannung abbilden kann — das ist der strukturelle Mindestbedarf.
4. **C flackert nicht willkürlich, sondern an einer bedeutungstragenden Kante** — und falls Flackern um VIX 20 stört, ist die Abhilfe hier sauber möglich (Hysterese: einmal ≥20 erreicht, bleibt K3 bis 19,0 an), während bei A ein Latch den Fehlmodus nur wieder einbauen würde.

**Kosten/Risiken meiner Empfehlung — ehrlich benannt:**

- **C ist deutlich schwerer zu erfüllen als die heutige Formel.** Ein Tag mit VIX 17 und +6 % wäre kein Schock-Tag mehr. Das ist beabsichtigt (die Schutzfunktion soll auf echte Vola-Ereignisse zielen), verschiebt aber die Fehlerrichtung von „zu oft gesperrt" auf „seltener gesperrt". Bei einem System mit Echtgeld-Ambition ist das die riskantere Richtung, und Levi muss sie bewusst wollen.
- **Die 20er-Schwelle passt zum US-Aktienregime, nicht automatisch zu NAS100-Eigenheiten.** Der NAS100 ist volatiler als der S&P; wer will, nimmt den VXN statt des VIX oder senkt auf 18. Das ist eine Feinjustierung, keine Grundsatzfrage.
- **Zwei Schwellen statt einer** (20 und +10 %) sind zwei Zahlen, die falsch sein können.

**Wie ich es einführen würde — und das ist ein Punkt, den Fables Vorlage komplett auslässt:** Nicht per Stichtag umschalten, sondern **als Schattenmessung parallel führen**, exakt nach dem hauseigenen Präzedenzfall aus Paket 6 („Dual-Gate-Schattenmessung auf trades, GEMESSEN, KEIN GATE", 31.08.). Die 8d-Pflichtzeile trägt ab sofort beide Werte: die geltende Range-Formel (gate-wirksam) und die C-Formel (Anzeige). Nach ~15 Handelstagen ist die Entscheidung datengestützt statt argumentativ. Weil der Fehler der alten Formel in Richtung „zu konservativ" zeigt, ist die Schattenphase risikofrei — sie kostet nur potenzielle Trades, keine potenziellen Verluste.

**Falls Levi jetzt sofort etwas entscheiden will:** Dann A als **Zwischenlösung mit Verfallsdatum** (bis die Schattenmessung für C vorliegt), ausdrücklich nicht als Endzustand — und in dem Fall bitte nur die erste Klausel („aktueller VIX ≥ Vortagesschluss"), nicht die zweite („näher am Hoch als am Tief"), weil die zweite die Flacker-Ursache ist und ohne sie die Regel eindeutig bleibt.

---

## 3. #12 — Abbruch-Gate um stille Defekte erweitern

**Fables Empfehlung: Option B** (Prüfer-Feld: die Opus-Vortagesanalyse bekommt ein Pflichtfeld „Abbruch-Empfehlung: JA/NEIN (+Kriterium)", das Abbruch-Gate übernimmt es 1:1).

**Mein Urteil: Zustimmung mit Vorbehalt — B ist im Ergebnis richtig, aber Fables Begründung zielt daneben, und drei notwendige Bestandteile fehlen.**

### Wo Fables Argument nicht trägt

Fables tragendes Argument gegen Option A lautet: *„Stille Defekte sind per Definition vom Ausführenden nicht erkennbar … Option A ließe den Loop die Analyse selbst interpretieren."* Der erste Halbsatz ist richtig, aber er betrifft den **eigenen** Tag des Loops. Beim Abbruch-Gate liest der Loop eine **fremde, fertige Analyse**. Das ist keine Detektions-, sondern eine Leseaufgabe — und Lesen kann der Loop. Die Behauptung, A verlange vom Loop das Unmögliche, ist damit nicht haltbar; A scheitert an etwas anderem, nämlich an der Auslegungsbreite von „stiller Defekt" (was in der Analyse ist gravierend genug?). Das ist ein Ermessens-, kein Fähigkeitsproblem, und es ist das gute Argument für B: Ein binäres Feld beseitigt das Ermessen, indem der Prüfer es ausübt, der die Sachlage kennt.

### Was in Fables Vorlage fehlt — drei Bestandteile, ohne die B nicht funktioniert

**(1) Fail-Closed-Default.** Fables B sagt nicht, was gilt, wenn das Feld fehlt oder die Analyse gar nicht existiert. Das ist der wichtigste Fall überhaupt: „Die Analyse wurde nicht geschrieben" ist selbst der schwerwiegendste stille Defekt, und ein Gate, das dann nichts findet, wäre wieder genau so blind wie heute. Der Präzedenzfall steht bereits im Testtag-Plan: *„Existiert die Datei nicht: Tag 1 ist komplett ausgefallen — das zählt selbst bereits als Abbruch-Kriterium."* Die Regel muss diese Logik übernehmen: **Analyse fehlt ODER Feld fehlt ODER Feld unklar → wie JA behandeln (Beobachtungsmodus).**

**(2) Der Prüfer-Auftrag muss mit geändert werden — sonst widerspricht das Feld dem Auftrag.** Beide bisherigen Analysen enthalten wörtlich das Gegenteil einer Empfehlung: *„Entscheidung liegt bei Levi; ich schlage nichts vor"* (Tag-2-Analyse, Abschnitt 7.3) und *„eine Frage ans Regelwerk (nicht meine Zuständigkeit)"* (Abschnitt 1.3). Auch die Auftragsliste betont, die Quellanalysen enthielten *„bewusst keine Lösungsvorschläge"*. Wird jetzt ein Pflichtfeld „Abbruch-Empfehlung JA/NEIN" eingeführt, ohne den Analyse-Auftrag zu ändern, produziert der nächste Prüfer entweder das Feld nicht (→ Fail-Closed greift bei jedem Lauf) oder er verstößt gegen seine Rolleninstruktion. Das ist eine Ein-Zeilen-Änderung, aber sie muss im selben Zug erfolgen.

**(3) Das Feld sollte dreiwertig sein, nicht binär.** Die Rechtsfolge des Abbruch-Gates ist heute binär und hart: Beobachtungsmodus = kein 7b1-Order-Ablauf, kein `gate_check.cjs`-Aufruf, den ganzen Tag. Stille Defekte sind aber sehr unterschiedlich schwer — „x_last_fetch.json steht in der Zukunft" (inzwischen durch #7 ohnehin ein lauter Fehler) ist etwas anderes als „ein Hard-Gate war still abgeschaltet". Ein Feld mit den Werten **KEIN / EINGESCHRÄNKT / ABBRUCH** erlaubt die mittlere Antwort („Tag läuft normal, aber der konkret defekte Mechanismus muss vor dem ersten Trade einmal nachweislich funktionieren"). Sonst entsteht der bekannte Alarmanlagen-Effekt: Ein Gate, dessen einzige Reaktion maximal ist, wird zurückhaltend ausgelöst und schützt dadurch weniger.

### Die Option, die in Fables Liste fehlt

**Option D — der Mensch als Gate für den ersten Tag, die Maschine nur für Folgetage.** Der Start einer Testtag-Serie hängt heute ohnehin an Levis Go („Go Testtag 01.09"), und ein Mensch, der die Vortagesanalyse gelesen hat, ist der beste denkbare Detektor für stille Defekte. Das Maschinen-Kriterium wird nur dort gebraucht, wo **kein** frisches Go dazwischenliegt — also bei mehrtägigen Cron-Serien, wo Tag 2 automatisch auf Tag 1 folgt. Die saubere Regelung ist deshalb: *Kriterium (e) gilt nur für Testtage ohne frisches Levi-Go; bei frischem Go ersetzt Levis Kenntnisnahme der Analyse das Kriterium.* Das spart Mechanik und verortet die Verantwortung richtig. Fables B bleibt für den Cron-Fall daneben bestehen — die beiden schließen sich nicht aus.

### Und der wichtigste Punkt: #12 kann nicht entschieden werden, ohne (c) und (d) neu zu fassen

Das ist ein Befund aus der Codeprüfung, den Fables Vorlage nicht enthält, obwohl er direkt aus Fables eigenen, bereits umgesetzten Fixes folgt. Der Wortlaut der geltenden Kriterien ([[project_testtag_plan_01_02_09_2026]]):

- **(c)** „Wurde ein 2/2-Dual-Gate-Moment erneut mit GESAMTSTATUS UNKNOWN abgelehnt, OBWOHL das Session-Levelregister nachweislich korrekt geschrieben … wurde?"
- **(d)** „Trat an Tag 1 wiederholt (≥2×) Exit-Code 1 Ursache 3 auf (Register fehlte/unlesbar im Trigger-Moment)?"

Durch die bereits umgesetzten Fixes gibt es ab sofort **neue Wege in genau diese beiden Zustände**:

- **#1** erzeugt bei fehlendem `--cluster-level` mit Grund ein `pass:null` → GESAMTSTATUS UNKNOWN. Passiert das an einem 2/2-Moment bei korrekt geschriebenem Register, ist Kriterium (c) nach seinem Wortlaut erfüllt — und der nächste Testtag läuft im Beobachtungsmodus, wegen eines vergessenen Kommandozeilenparameters.
- **#1 und #20** setzen `--cluster-level` und `--entry-source` auf die A3-Pflichtliste: fehlen sie ohne Grund, ist es Exit 1. Fable schreibt dazu selbst, ein Aufruf nach altem Template breche „ab jetzt mit Exit 1 (gewollt)". Formal ist das nicht „Ursache 3", also greift (d) nicht wörtlich — aber die Bilanzzeile „Exit-1-Abbrüche" im Faktenprotokoll zählt sie, und jede Neufassung des Abbruch-Gates muss sagen, ob ein sofort korrigierter und wiederholter Aufruf mitzählt.
- **#2** schafft mit dem Registeralter-Hard-Exit eine dritte, neue Exit-1-Ursache, die inhaltlich sehr wohl „Register nicht verwendbar im Trigger-Moment" ist — also (d)-verwandt, aber im Wortlaut nicht erfasst. Mit der von Fable in #22 vorgeschlagenen Verschärfung auf 90 Minuten wird dieser Pfad realistisch erreichbar (mit dem Provisorium 1440 praktisch nie).

**Empfehlung:** Wenn #12 entschieden wird, im selben Zug (c) und (d) neu formulieren, mit drei Klarstellungen: (i) UNKNOWN aus einer *fehlenden Messfeldangabe* ist ein Dokumentationsmangel, kein struktureller Fehler — er zählt für (c) nicht; (ii) ein Exit 1, der nach der EXIT-CODE-1-KLAUSEL korrigiert und im selben Trigger-Moment erfolgreich wiederholt wurde, zählt für (d) nicht; (iii) (d) wird von „Ursache 3" auf „registerbezogene Exit-1-Ursachen" verallgemeinert, damit die neue Alters-Ursache miterfasst ist. Ohne diese drei Sätze macht die Härtung aus #1/#2/#20 das Abbruch-Gate zu einem Haarauslöser — und das wäre eine unbeabsichtigte Nebenwirkung von Fixes, die für sich genommen richtig sind.

---

## 4. #21 — Register: Herkunft oder auch Tragfähigkeit?

**Fables Empfehlung: Option A** (Kennzeichnen: `gate_check.cjs` prüft, ob das TP-Level in der heutigen Session-Range liegt bzw. durchhandelt wurde, und ergänzt die Herkunftszeile — Anzeige, kein Gate).

**Mein Urteil: Zustimmung — mit zwei Präzisierungen, ohne die die Umsetzung falsch würde.**

### Warum ich A eigenständig für richtig halte

Fables Argument („n=1, ein Hard-Gate wäre unkalibriert") trage ich mit. Ich habe zwei zusätzliche, unabhängige Gründe:

**(1) Der auslösende Fall beweist, dass ein Gate hier nichts gebracht hätte.** Am 02.09. wurde der Trade an 8c2 abgelehnt, und die Tag-2-Analyse rechnet vor, dass es überhaupt keinen zulässigen Parametersatz gab (SL ≥ 29.066,3 für 8c2 → RR 0,935:1 → FAIL an 8b; SL enger unmöglich wegen 1,5×-ATR-Floor). Ein zusätzliches Hard-Gate auf „durchhandelt" hätte an diesem einzigen Datenpunkt **nichts** verändert. Ein Gate, das an seinem eigenen Anlassfall folgenlos bleibt, hat keine empirische Grundlage.

**(2) Die Frage ist leicht falsch gerahmt — „durchhandelt" ist für ein TP-Level kein Mangel, sondern ein Vorzug.** Ein TP ist ein Ziel, das der Kurs erreichen soll. Dass der Markt heute schon dort war, ist der beste verfügbare Beleg für Erreichbarkeit — und Erreichbarkeit ist exakt das, was 8b1 (TP-Realismus) misst. Der Einwand aus der Tag-2-Analyse („als Levelqualität ist es dünner") trifft eine andere Rolle: die als **Halt/Unterstützung**, also den Fall, dass der Kurs an diesem Level stoppen und einen Fill liefern soll. Ein durchhandeltes Level ist ein gutes *Ziel* und ein schwacher *Halt* — und ein TP1, an dem man Teilgewinn mitnimmt, ist beides zugleich.

Daraus folgt meine erste Präzisierung: **Die Kennzeichnung soll nicht bloß „intraday bereits durchhandelt" ausgeben, sondern die Doppelrolle benennen** — etwa: „Level lag heute bereits im gehandelten Bereich (Session-Tief 72 Pkt darunter) → als Zielmarke erreichbarkeitsbelegt, als Haltezone geschwächt". Sonst liest die Zeile wie eine Warnung, wo sie in der Sache eine gemischte Information ist, und erzeugt in der Praxis eine Vermeidungshaltung gegenüber genau den Retest-/Magnet-Setups, die Fable zu Recht nicht blockieren will.

### Zweite Präzisierung, und die ist wichtiger: die Datenquelle taugt so nicht

Fable schreibt, der Umsetzungsaufwand sei klein, weil „Session-Hoch/-Tief im Register stehen". Das stimmt, aber ich habe `scripts/level_register.json` gegengelesen, und die beiden Einträge sind **nicht vergleichbar erhoben**:

```
{ "price": 29166.15, "type": "Session-Hoch", "abrufweg": "data_get_ohlcv 5min-Bars, neues Hoch beobachtet bis VC#20 16:35 Uhr" }
{ "price": 28882,    "type": "Session-Tief", "abrufweg": "data_get_ohlcv Tages-Timeframe, forming bar 02.09.2026 (Stand Sessionstart)" }
```

Das Hoch stammt aus der eigenen Beobachtung des Loops ab 15:00 (schließt Vorbörse aus), das Tief aus der Tages-Bar (schließt Vorbörse ein). **Eine „durchhandelt"-Prüfung gegen dieses Paar hätte nach unten ein anderes Fenster als nach oben** — TP-Level unter dem Vorbörsen-Tief würden als durchhandelt markiert, TP-Level über einem Vorbörsen-Hoch nicht. Dazu kommt der Tag-1-Präzedenzfall: Dort stand das Session-Tief mit 28.981,85 im Register, während der reale Tiefpunkt bei 28.954,35 lag — der Wert war **falsch bei formal frischem `updated`-Zeitstempel**. Der Alters-Guard aus #2 kann so etwas prinzipiell nicht fangen (er prüft den Zeitstempel, nicht den Inhalt).

**Meine Empfehlung zur Datenquelle:** Nicht die Registerfelder verwenden, sondern **Tageshoch und Tagestief als zwei neue A3-Messfelder an `gate_check.cjs` übergeben** (`--tag-hoch` / `--tag-tief`). Begründung: Beide Werte werden für 8d-Kriterium 1 (Tagesrange/ATR) **ohnehin bei jedem Voll-Check frisch aus `quote_get` erhoben** — es entsteht null zusätzlicher Messaufwand, die Werte kommen aus einer einzigen konsistenten Quelle mit einem einzigen Zeitfenster, und sie sind über die A3-Pflicht erzwungen statt von der Register-Pflege abhängig. Als Nebeneffekt wird die 8d-K1-Eingabe zum ersten Mal maschinell mitprotokolliert, was die von Punkt #19 adressierte Rückverfolgbarkeit der laufenden Messgrößen verbessert.

**Kosten/Risiken:** Zwei weitere Pflichtfelder verlängern das ohnehin lange Aufrufkommando und erzeugen — siehe #12 — einen weiteren Exit-1-Pfad bei Vergessen. Wenn Levi die A3-Liste nicht weiter wachsen lassen will, ist die Alternative, `--tag-hoch`/`--tag-tief` als **optionale** Felder einzuführen (Kennzeichnung entfällt bei Fehlen, mit sichtbarer Zeile „Tragfähigkeit nicht geprüft — Tages-Extrema nicht übergeben"). Das ist der schwächere, aber risikoärmere Weg und passt zum Anzeige-Charakter der Option A.

---

## 5. #22 — Frischefenster für Gate-Eingaben + Registeralter-Obergrenze

**Fables Empfehlung: Option A + Registeralter hart 90 Minuten.**

**Mein Urteil: Bei der Struktur (A statt B) Zustimmung. Bei Fables konkreter Toleranztabelle Widerspruch — sie ist rechnerisch um eine volle Kerzenlänge zu eng. Bei den 90 Minuten Zustimmung mit dem Hinweis, dass die Zahl weniger leistet, als sie verspricht.**

### Teil 1: Kerzenfrische — A statt B ist richtig, aber die Zahlen stimmen nicht

**Zur Struktur:** Ich stimme zu, und zwar mit einem Argument, das über Fables „Feed-Rollover-Pech" hinausgeht: **Ein hartes Gate wäre unwirksam, weil es keine Abhilfe gibt.** Wenn `element[0]` auf 1H die vorletzte Stundenkerze liefert, kann der Loop keine frischere *bestätigte* 1H-Kerze herbeiführen — erneutes Lesen liefert dieselbe. Die einzige „Abhilfe" wäre Warten, und das heißt: den Trigger-Moment verstreichen lassen. Ein Gate, dessen einzige mögliche Reaktion das Aufgeben des Setups ist, ist kein Qualitätsfilter, sondern ein Zufallsgenerator. Ausweisen ist hier die einzig sinnvolle Behandlung.

**Zu den Zahlen — und das ist ein konkreter Fehler in Fables Vorlage.** Fable schlägt vor: „zulässige Obergrenze je Ebene = 1 Kerzenlänge + 2 Min Toleranz (5min ≤ 7, 15min ≤ 17, 1H ≤ 62 Min)". Ich habe die `count:2`-Semantik durchgerechnet:

Sei L die Kerzenlänge, t die aktuelle Zeit, b die letzte Kerzengrenze ≤ t.
- **Feed hat umgeschaltet:** `element[1]` = formende Kerze [b, b+L), `element[0]` = [b−L, b], Schlusszeit b → **Alter = t − b ∈ [0, L)**.
- **Feed hat noch nicht umgeschaltet:** `element[1]` = [b−L, b], `element[0]` = [b−2L, b−L], Schlusszeit b−L → **Alter = t − b + L ∈ [L, 2L)**.

**Das Maximum liegt also knapp unter 2L, nicht bei L + 2 Min.** Konkret auf 1H: Um 15:30 ohne Rollover ist die Referenz die 13:00-Kerze (Schluss 14:00) → **90 Minuten alt**. Um 15:59 ohne Rollover → **119 Minuten**. Fables Grenze von 62 Minuten würde in beiden Fällen „Referenz veraltet, neu lesen" melden, obwohl nichts defekt ist und erneutes Lesen nichts ändert.

Die Tag-2-Analyse liefert den Beleg selbst: *„bei VC#1 (15:00:20) ist die 1H-Referenz die 13:00-Kerze (also zwei Stunden alt)"*. Das ist der **erste Voll-Check des Tages, den Fable als vorbildlich zitiert** — und er reißt Fables Grenze, je nachdem ob man das Alter ab Kerzen-Open (120 Min) oder ab Kerzen-Schluss (60 Min) misst. **Genau diesen Bezugspunkt legt Fables Vorlage nicht fest**, was für sich schon ein Umsetzungsmangel ist.

**Meine Korrektur:**
1. **Bezugspunkt festschreiben:** Alter = jetzt − *Schlusszeit* der Referenzkerze (= Open + Kerzenlänge). Der Open-Timestamp wird nach #15 ohnehin mitgeführt, die Schlusszeit ist daraus trivial.
2. **Schwelle auf 2 Kerzenlängen + Toleranz setzen:** 5min ≤ 12, 15min ≤ 32, 1H ≤ 122 Minuten. Diese Grenzen sind im Normalbetrieb **konstruktiv nicht erreichbar** — genau deshalb sind sie brauchbar: Eine Überschreitung bedeutet dann nicht „Rollover-Pech", sondern **einen echten Feed-Defekt** (hängender Datenstrom, falsches Symbol, eingefrorene Session). Das ist die einzige Aussage, die ein Frische-Schwellenwert hier überhaupt tragen kann.
3. **Der eigentliche Wert liegt in der Nicht-Gleichzeitigkeit, nicht im Alter der einzelnen Ebene.** Der bei VC#13 beobachtete Fall (1H/15min sekundenaktuell, 5min fünf Minuten älter) wird von keiner Einzel-Ebenen-Schwelle erfasst — jede Ebene für sich war unauffällig. Die Pflichtangabe sollte deshalb zusätzlich die **Spreizung** ausweisen: „MTF-Snapshot-Spreizung: 5 Min (5min-Referenz älteste)". Das ist die Zahl, die am Trigger-Moment tatsächlich etwas über die Gleichzeitigkeit der Gate-Eingaben sagt.

### Teil 2: Registeralter — 90 Minuten ist vertretbar, aber die Zahl ist fast folgenlos

**Codelage geprüft (Auftragspunkt 3):** Fables Angabe stimmt. `scripts/gate_check.cjs:417` enthält `const LEVEL_REGISTER_HARD_MAX_AGE_MINUTES = 1440;`, ausgewertet in `main()` bei Zeile 1603–1607 als Hard-Exit 1; der Negativ-Alters-Exit steht bei 1598–1602 mit einer Toleranz von einer Minute; die 60-Minuten-Warnung ist unverändert. Beide Pfade liegen hinter der frühen `--batch`-Rückkehr, gelten also nur live — konsistent mit dem GC-5-Präzedenzfall „nicht rückwirkend". Der Kommentarblock ab Zeile 401 dokumentiert das Provisorium ausdrücklich als offene Regelfrage #22. **Fables Bericht deckt sich hier vollständig mit dem Code.**

**Zur Höhe:** Fables Rechnung (90 Min = 18 verpasste Voll-Checks = eindeutig defekte Session) ist korrekt. Ich ergänze zwei Beobachtungen:

- **Die Kosten eines Hard-Exits im Trigger-Moment sind nahe null**, und das ist das eigentliche Argument für eine enge Grenze: Die Abhilfe ist das Neuschreiben des Registers — ein Vorgang von Sekunden, den der Loop bei jedem Voll-Check ohnehin ausführt. Der „Abbruch" ist faktisch eine erzwungene Aktualisierung, kein Verlust des Setups. Wer das so sieht, kann auch enger gehen als 90.
- **Trotzdem ist die konkrete Zahl fast folgenlos.** In einer gesunden Session wird `updated` alle 5 Minuten erneuert; am 02.09. betrug der längste Abstand zwischen zwei Voll-Checks 6,5 Minuten. **Jeder Wert zwischen etwa 30 und 120 Minuten verhält sich in der Praxis identisch:** in einer gesunden Session bindet er nie, in einer kaputten immer. Die Wahl zwischen 90 und 1440 ist deshalb keine Feinjustierung, sondern nur die Entscheidung, ob auch „anderthalb Stunden ohne Registerpflege" als defekt gelten soll — und das sollte es. **Ich unterstütze 90.**
- **Was die Zahl nicht leistet, und das gehört in die Entscheidung:** Der Alters-Guard prüft den Zeitstempel, nicht den Inhalt. Der Tag-1-Schaden (Session-Tief 28.981,85 im Register, real 28.954,35) war ein **inhaltlich falscher Wert bei formal gültigem Zeitstempel** — genau dagegen hilft weder 90 noch 1440. Der Kopfkommentar im Code benennt diese Grenze bereits selbst („Der Alterscheck prueft NUR den globalen `updated`-Zeitstempel, keine pro-Level-…"). **Der wirksamere Schritt wäre ein eigenes `updated`-Feld pro Level-Eintrag**, mindestens für die driftenden Kategorien Session-Extrema und AVWAP-Bänder: Pivots und PDH/PDL sind den ganzen Tag konstant und dürfen 8 Stunden alt sein, ein Session-Tief von vor 40 Minuten ist wertlos. Eine einzige globale Schwelle ist für die statischen Level zu streng und für die dynamischen zu lax. Das ist eine echte, in Fables Vorlage fehlende Option — sie kostet ein Feld im Schema und einen Vergleich im Skript.
- **Ein praktischer Nebenhinweis zur Toleranz von einer Minute beim Negativ-Alter:** Die Register-`updated`-Werte werden auf volle Voll-Check-Minuten gerundet geschrieben (`13:55:00.000Z`, `19:55:00.000Z`). Solange der Loop den *abgeschlossenen* Voll-Check stempelt, ist eine Minute Toleranz ausreichend. Würde er je den *anstehenden* Slot stempeln, wäre jeder Gate-Aufruf ein Hard-Exit. Die Konvention „Zeitstempel = Zeit des gerade abgeschlossenen Voll-Checks" sollte deshalb im Regelwerk explizit stehen, wenn #22 entschieden wird. Ebenso gehört in den Testtag-Prompt der Hinweis, dass fiktive Testtage das Register ab jetzt zwingend mit **echter** UTC schreiben müssen — mit simuliertem Datum bricht jeder Gate-Aufruf ab (was gewollt, aber unangekündigt teuer ist).

---

## 6. Konsistenzprüfung der bereits umgesetzten Fixes (Auftragspunkt 3)

Geprüft wurde `scripts/gate_check.cjs` im Arbeitsverzeichnis (uncommitted, `git diff`: +181/−6 Zeilen) gegen Fables Beschreibung, beschränkt auf die Stellen mit Bezug zu den fünf offenen Fragen.

| Punkt | Fables Behauptung | Codelage | Urteil |
|---|---|---|---|
| **#2** | Registeralter-Obergrenze als Provisorium **1440 Min**, negatives Alter → Exit 1 | `LEVEL_REGISTER_HARD_MAX_AGE_MINUTES = 1440` (Z. 417); Negativ-Exit `ageMinutes < -1` (Z. 1598); Hard-Exit über Obergrenze (Z. 1603); beide live-only | **bestätigt** |
| **#1** | `--cluster-level` auf A3-Pflichtliste; `none`/`keine` = PASS mit Kennzeichnung; fehlend+Grund = UNKNOWN; fehlend ohne Grund = Exit 1; Batch unverändert | `{ key: 'cluster-level' }` in `MEASUREMENT_REQUIRED` (Z. 1468); `clusterLevelNone`-Zweig (Z. 715/1042); UNKNOWN-Zweig mit Grund (Z. 1080 ff.); `isBatch`-Zweig behält alte Semantik | **bestätigt**, inkl. des von Fable zusätzlich gefundenen Falls „Zone da, ATR fehlt" (jetzt `pass:null` statt stillem PASS) |
| **#9** | Q2-Budget wird bei Fehlen aus `--impuls-reifegrad-atr` selbst berechnet, Formel `(1,5−r)/1,5×100` | Block vor der A3-Schleife, setzt `args['dual-gate-q2-budget-pct']` und protokolliert „Q2-BUDGET SELBST BERECHNET"; `impuls-reifegrad-atr` steht selbst auf der Pflichtliste (Z. 1452), die Eingabe ist also praktisch immer vorhanden | **bestätigt**; die Ausnahme-Begründung „Formel nicht rekonstruierbar" ist damit tatsächlich gegenstandslos |
| **#20** | `--entry-source` A3-pflichtig, reine Doku-Zeile | `{ key: 'entry-source' }` (Z. 1472); Ausgabezeile „Entry-Herkunft:" mit Batch-Sonderfall | **bestätigt** |
| **#8** | Rundzahlen nicht mehr „VERIFIZIERT" | `isGeneratedLevel()` (Z. 486) greift an allen drei Ausgabestellen (TP1-Herkunftszeile, 8b1-Detail, TP2-Detail) | **bestätigt** |

**Drei Anmerkungen zur Konsistenz, keine Fehler:**

1. **Die Konsistenz-Warnung beim Q2-Budget hat eine Toleranz von 1 Prozentpunkt.** Die Formel ist deterministisch — jede Abweichung über Rundung hinaus ist ein Widerspruch. Eine Toleranz von 1 pp lässt echte Inkonsistenzen durchlaufen (bei Reifegrad-Werten um 4,5 entspricht 1 pp einer Reifegrad-Abweichung von 0,015 — knapp, aber die Größe wird ohnehin auf zwei Nachkommastellen übergeben). Kosmetisch: 0,1 pp wäre die passendere Grenze. Kein Handlungsdruck.
2. **Ein nicht-numerisches `--cluster-level`** (Tippfehler, „n/a") fällt in denselben Zweig wie „Parameter fehlt" und erzeugt die Meldung „`--cluster-level` fehlt". Das Verhalten ist sicher (UNKNOWN, kein stiller PASS), die Meldung aber irreführend. Kleinigkeit.
3. **Die wichtigste Konsistenzfrage steht nicht im Skript, sondern zwischen Skript und Testtag-Plan** — die Wechselwirkung von #1/#2/#20 mit den Abbruch-Kriterien (c) und (d), ausführlich unter #12 oben. Das ist der einzige Punkt dieser Prüfung, bei dem ich echten Handlungsbedarf sehe, bevor der nächste Testtag startet.

---

## 7. Gesamturteil

**Deckungsgrad mit Fables Empfehlungen: 1 von 5 voll, 2 von 5 mit Vorbehalt, 1 von 5 nur im Prinzip (falscher Zahlenwert), 1 von 5 Widerspruch.**

| Frage | Fable | Opus (unabhängig) | Deckung |
|---|---|---|---|
| **#3** 8d-K2 | C (Zustand + Blackout-Event-Definition) | Zustandslogik ja, Eventdefinition nein → **Option D** (Blackout-Termin **UND** dokumentierte Konsens-Abweichung; Rückblick bewusst streichen) | **teilweise** |
| **#11** 8d-K3 / VIX | A (Richtungsfilter) | **Widerspruch → Option C** (VIX ≥ 20 ODER Tagesanstieg ≥ +10 %), eingeführt als Schattenmessung; A allenfalls als befristete Zwischenlösung und nur mit der ersten Klausel | **nein** |
| **#12** Abbruch-Gate | B (Prüfer-Feld) | B im Ergebnis ja — aber Fables Begründung trägt nicht, und es fehlen: Fail-Closed-Default, Änderung des Prüfer-Auftrags, dreiwertiges Feld. Zwingend: (c)/(d) im selben Zug neu fassen | **ja, mit Vorbehalt** |
| **#21** Register-Tragfähigkeit | A (Kennzeichnen) | **ja** — mit Doppelrollen-Formulierung und `--tag-hoch`/`--tag-tief` statt der inkonsistenten Registerfelder | **ja** |
| **#22** Frischefenster + 90 Min | A + 90 Min | A ja; **Toleranztabelle falsch** (2 Kerzenlängen, nicht 1 → 12/32/122 Min, Bezugspunkt Kerzen-Schluss); 90 Min ja, aber pro-Level-Zeitstempel bringt mehr | **teilweise** |

### Die zwei wichtigsten Abweichungen, wenn Levi nur zwei Dinge mitnimmt

**1. #3 und #11 nicht getrennt entscheiden.** Unter Fables Empfehlungspaar sind alle drei 8d-Kriterien intraday monoton nicht-fallend — 8d wird eine Einbahn-Sperre, die faktisch der Makro-Kalender entscheidet, und Fables Gegenargument („K1 bildet nachlassende Wirkung ab") ist rechnerisch falsch, weil K1 als kumulative Tagesgröße ebenfalls nur steigen kann. Konkret: Hätte der 02.09. einen zweiten Blackout-Termin gehabt, wäre der als bester Testtag der Serie bewertete Tag unter diesem Paar ein durchgehender No-Trade-Tag gewesen — bei K1 = 0,62× und fallendem VIX. Mein Vorschlag: #3 = D (Latch, aber echte Überraschungen), #11 = C (rückstellbar, mit absolutem Anker), eingeführt über die hauseigene Schattenmessung. Vorab und billiger als jedes Argument: 20 Handelstage auszählen, an wie vielen ≥2 Blackout-Termine **und** VIX-Intraday-Range ≥5 % zusammenfielen.

**2. Vor dem nächsten Testtag müssen die Abbruch-Kriterien (c) und (d) nachgezogen werden.** Fables bereits umgesetzte Fixes #1/#2/#20 schaffen neue UNKNOWN- und Exit-1-Pfade, die im Wortlaut der geltenden Kriterien landen. Ein vergessener `--cluster-level` an einem 2/2-Moment erfüllt (c) buchstabengetreu und schickt den Folgetag in den Beobachtungsmodus — wegen eines fehlenden Kommandozeilenparameters. Das ist eine unbeabsichtigte Nebenwirkung richtiger Fixes und mit drei Klarstellungssätzen zu beheben.

**Zum Gesamteindruck, ohne Diplomatie:** Fables Vorlagen sind handwerklich gut gebaut — die Optionen sind sauber getrennt, die Befunde wurden vor der Umsetzung real gegen Code und Protokolle verifiziert (die Q2-Formel gegen VC#40 nachzurechnen war die richtige Arbeit), und die Empfehlungen sind durchweg konservativ und umsetzungsnah. Die Schwäche liegt nicht in der Sorgfalt, sondern in der **Isolation**: Jede der fünf Fragen wurde für sich beantwortet, ohne die Wechselwirkung zu den anderen vier und zu den gerade selbst umgesetzten Skript-Fixes. Genau dort liegen beide Abweichungen oben — und in einem Fall (#22-Toleranztabelle) eine Zahl, die durch das Beispiel widerlegt wird, das in derselben Analyse steht.

*Entwurf. Keine Regeländerung, kein Code, kein MEMORY.md-Eintrag durch diese Prüfung. Entscheidung liegt bei Levi.*
