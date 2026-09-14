---
name: project-pruefung-feedback-live-trading-2026-09-14
description: "Einzelprüfung von feedback_live_trading.md (280.057 B, 1.199 Zeilen, 31 Abschnitte) auf Kürzbarkeit — der eigene Durchgang, den [[project_memory_aufraeumen_2026-09-14]] E7 gefordert hat. 30 nummerierte Kürzungsvorschläge, 5 innere Widersprüche, klare Nicht-anfassen-Liste. Bericht, KEINE Umsetzung der Kürzungen. Nachtrag 14.09.2026: W1-W5 von Fable korrigiert (W3 gegen gate_check.cjs verifiziert, W5 nur markiert mit Fail-Safe 3/4). Opus-Gegencheck 14.09.2026 (letzter Abschnitt): FREIGEGEBEN MIT AUFLAGEN, kein Blocker; W3-Code selbst gelesen + sandboxed CLI-Lauf bestätigt Exit 1 trotz --grund-cluster-level; eine übersehene W1-Stelle (Z444) und eine unvollständige (Z442) selbst nachgezogen (F1/F2, reines Markup); fünf Nachbesserungen offen, darunter N1 (Punkt-11-Scope Exit vs. Dreh — 6. Uneindeutigkeit, neu gefunden) und N5 (Cron-Item 8 'SL=MAX(Struktur,Floor)'). Datei jetzt 1.204 Zeilen / 285.003 B, Wiki-Links 45/45 unverändert, Tests 73/73."
metadata:
  node_type: memory
  type: project
  status: "W1-W4 korrigiert + Opus-gegengecheckt 14.09.2026: FREIGEGEBEN MIT AUFLAGEN (kein Blocker, 2 Opus-Fixes F1/F2, 5 Nachbesserungen N1-N5 offen); W5 weiter offen (Levi-Entscheidung, Fail-Safe 3/4 aktiv); Kürzungen K1-K30 offen (Levi-Entscheidung)"
  originSessionId: session_current
  modified: 2026-09-14T12:23:54.204Z
---

# `feedback_live_trading.md` — Einzelprüfung auf Kürzbarkeit (14.09.2026, Opus 5)

**Auftrag:** Der allgemeine Aufräum-Bericht ([[project_memory_aufraeumen_2026-09-14]], Empfehlung E7) hat diese Datei als größten Kontext-Hebel *und* als einziges echtes Live-Risiko benannt und einen eigenen Durchgang mit Gegencheck verlangt. Das ist dieser Durchgang. **An der Datei selbst wurde nichts geändert** — kein Zeichen, kein Commit, kein Push. Alle Zahlen sind am 14.09.2026 am Dateisystem gemessen, nicht geschätzt ([[feedback_memory_pflege]], Zitierpflicht).

**Methode:** Die Datei wurde vollständig gelesen (1.199 von 1.199 Zeilen, in 14 Leseabschnitten), nicht stichprobenhaft. Danach Byte-Messung je Überschrift und je Kürzungskandidat.

---

## 1. Kurzfazit

### 1.1 Ist-Zustand

| Kennzahl | Wert |
|---|---|
| Größe | **280.057 Bytes** (273,5 KB) |
| Zeilen | 1.199 |
| Abschnitte (`##`/`###`) | 31 |
| `description`-Feld (Zeile 3) | 2.337 B |
| Zeilen > 2.000 Zeichen | 11 (längste: Z681 mit 5.014 B = das `gate_check.cjs`-Schritt-5-Template) |
| Wiki-Links | 129 Vorkommen auf 45 verschiedene Ziele |
| Durchgestrichene (`~~`) Passagen | 8 Zeilen |

### 1.2 Anteile nach Klassifikation

Die Einteilung ist **bewusst defensiv**: alles, wo ich nicht sicher sagen konnte „das braucht Sonnet im Loop heute nicht mehr", steht als AKTIV-BINDEND.

| Klasse | geschätzte Bytes | Anteil |
|---|---|---|
| **AKTIV-BINDEND** (Regel gilt heute, Volltext bleibt) | ~238.000 | **85 %** |
| **HISTORISCHER ÄNDERUNGSVERMERK** (Regel bleibt, Herleitung kürzbar) | ~33.000 | 12 % |
| **VERALTET/ÜBERHOLT** (durch spätere Passage derselben Datei ersetzt) | ~9.000 | 3 % |

### 1.3 Kürzungspotenzial

| | Bytes | danach | Reduktion |
|---|---|---|---|
| Ist | 280.057 | — | — |
| **Nur „sicher" (K1–K22)** | −33,1 KB | **~247 KB** | **−12 %** |
| **sicher + „mit Vorbehalt" (K1–K30)** | −42,0 KB | **~238 KB** | **−15 %** |

### 1.4 Die wichtigste Aussage dieses Berichts — und sie korrigiert eine Erwartung

Der allgemeine Bericht schätzte für E7 einen **„Geltender Stand"-Kopf von 40–60 KB, also −75 %**. Diese Zahl ist mit reinem Kürzen **nicht erreichbar**. Gemessen komme ich auf **−12 bis −15 %**.

Der Grund ist strukturell und wichtig für jede Folgeentscheidung: Diese Datei mischt Regel und Begründung **innerhalb einzelner Absätze**, nicht zwischen ihnen. Zeile 672 (Schritt 0, 4.675 B) ist ein einziger Absatz, in dem geltendes Kommandotemplate, Rechtsfolge, Auszählung vom 10.09., Opus-Auflagen-Historie und Why lückenlos ineinander verwoben sind. Einen solchen Absatz sauber in „Regel" und „Herleitung" zu trennen ist **Umschreiben, nicht Kürzen** — und Umschreiben ist genau die Operation, vor der [[feedback_dont_change_running_system]] warnt.

**−75 % gibt es nur mit Umstrukturierung, und Umstrukturierung heißt Neuformulierung geltender Gate-Regeln.** Ich rate davon weiterhin ab (Details in Abschnitt 5).

### 1.5 Der zweite, unabhängige Befund: es gibt echte Widersprüche

Unabhängig von der Größe habe ich **5 Stellen gefunden, an denen zwei verschiedene Fassungen derselben Regel im Text stehen** und ein Leser im Zweifel sein kann, welche gilt. Zwei davon (W1, W2) betreffen Schwellenwerte, die Sonnet im Loop direkt anwendet. **Das ist der dringendere Teil dieses Berichts** — er rechtfertigt eine Bearbeitung auch dann, wenn du sonst nichts kürzen willst (Abschnitt 3).

### 1.6 Empfehlung zur Form der Umsetzung, falls du kürzt

**Nicht löschen, sondern auslagern.** Alle unten als kürzbar markierten Passagen wandern wörtlich in eine neue Datei `feedback_live_trading_historie.md`, und an der Fundstelle bleibt ein Ein-Zeiler mit Verweis. Dann gilt:

- Session-Start lädt nur noch die Regeldatei (−33 bis −42 KB).
- Kein Byte Herleitung geht verloren; jeder künftige Gegencheck kann weiter zitieren.
- Kein Wiki-Link bricht (die Historie-Datei nimmt sie mit, siehe Abschnitt 6).
- Das Risiko sinkt drastisch gegenüber dem Löschen: ein Fehlurteil von mir ist reversibel.

---

## 2. Abschnittsweise Klassifikation

### 2.1 Überblick je Abschnitt (gemessen)

| Abschnitt | Zeilen | Bytes | Dominante Klasse |
|---|---|---|---|
| Frontmatter + Kopf | 1–18 | 2.788 | `description` überholt (K1) |
| 1. Fast Mode | 19–22 | 143 | AKTIV |
| 2. Kompaktes Signal-Format | 23–39 | 2.167 | AKTIV |
| 2a. Loop-Intervall 1 Min | 40–50 | 1.678 | AKTIV (ScheduleWakeup-Falle historisch) |
| **2b. CronCreate-Cadence** | **51–464** | **51.046** | AKTIV mit großem historischem Anteil |
| 2c. Positionsgrößen-Bestätigung | 465–472 | 2.001 | AKTIV |
| 3. Tool-Sequenz | 473–482 | 850 | AKTIV |
| 3a. MTF-Bias | 483–493 | 3.088 | AKTIV |
| 3b. Screenshot-Kadenz | 494–500 | 697 | AKTIV |
| 4. Permission-Prompts | 501–505 | 309 | AKTIV |
| 4a. Kommunikation im Loop | 506–513 | 664 | AKTIV |
| 5. Memory in Session | 514–516 | 200 | AKTIV |
| 6. Format ohne /fast | 517–523 | 810 | AKTIV |
| 7. Entscheidungsbaum (+7d0/7d/7f) | 524–596 | 10.499 | AKTIV, Vorfallshistorie kürzbar |
| 7a. Indikator-Freigabe | 597–607 | 2.115 | AKTIV |
| 7b. Zwei-Pane-Setup | 608–628 | 5.874 | AKTIV |
| **7b1. Entry-Freigabe / 5-Schritte-Ablauf** | **629–728** | **54.276** | AKTIV — **Herzstück, höchste Vorsicht** |
| 7b1a. Q-Score | 729–761 | 16.482 | AKTIV, AVWAP-Historie kürzbar |
| 7b1b. Dual-Gate-Schattenmessung | 762–788 | 6.179 | AKTIV (laufende Messung) |
| 7b1c. Stale-Klasse | 789–820 | 8.639 | AKTIV, Paket-6a-Vermerke kürzbar |
| 7e. QQQ-Session-Gate | 821–833 | 2.694 | AKTIV |
| 7c. MACD-Momentum (5 Erweiterungen) | 834–876 | 14.562 | AKTIV, Vorfallshistorie kürzbar |
| 8. Positions-Kasten | 877–897 | 2.801 | AKTIV |
| **9. Voll-Check-Rhythmus** | **898–957** | **21.296** | AKTIV, Anti-Drift-Historie kürzbar |
| 9a. Systemzeit-Pflicht | 958–973 | 4.039 | AKTIV, **enthält W1** |
| 10. Permission-Modus Auto | 974–982 | 1.262 | AKTIV |
| 11. Reversal-Management | 983–1038 | 19.687 | AKTIV, RVOL-Herleitung kürzbar |
| 12. Stall-Exit (+12.2/12.3/12.4) | 1039–1112 | 17.947 | AKTIV, **enthält W2** |
| 12.5 Review-Abschluss 12.08. | 1113–1129 | 5.029 | überwiegend historisch |
| 13. Chasing (+13.1) | 1130–1166 | 10.834 | AKTIV, Vorgeschichte kürzbar |
| 14. Regeländerungs-Tempo-Bremse | 1167–1182 | 3.942 | AKTIV |
| 15. Solo-Mandat | 1183–1199 | 5.459 | AKTIV |

**Drei Abschnitte tragen 45 % der Datei:** 7b1 (54,3 KB), 2b (51,0 KB), 9 (21,3 KB). Dort liegt praktisch das gesamte Potenzial — und dort ist auch das gesamte Risiko.

---

### 2.2 Die 30 Kürzungsvorschläge, einzeln zustimmbar

Format: **Nummer · Ort · Klasse · Ist → Vorschlag · Ersparnis**. Jeder Vorschlag ist unabhängig von den anderen.

#### Gruppe A — Frontmatter und Abschnitt 2b (CronCreate)

**K1 · Zeile 3, `description`-Feld · VERALTET · 2.337 B → ~350 B · −2,0 KB**
Zitat (Anfang): *„Live-Trading-Protokoll für maximale Geschwindigkeit — Signal-Format, Check-in-Sequenz, Fast-Mode. Seit 31.08.2026: gate_check/cooldown_check per >-Redirect … 31.08.2026 zusätzlich: … Opus-Zweitprüfung … Opus-Drittfix 31.08.2026 Abend … Opus-Analyse Testtag 10.09. umgesetzt 11.09.2026 … Opus-Gegencheck zu Commit 5adb7d6 …"*
Das ist ein Änderungsprotokoll im Metadatenfeld — genau das Muster, das der allgemeine Bericht in 1.6 als Lehrstück benannt hat. **Vorschlag, wörtlich:** „Live-Trading-Protokoll: Loop-Kadenz, Signal-/Ausgabeformate, Entscheidungsbaum, Dual-Gate + 7b1-Entry-Freigabe (gate_check.cjs), Q-Score, Reversal-/Stall-/Chasing-Regeln, Solo-Mandat. Fortschreibungshistorie steht im Dokument selbst."
*Risiko: keins — die `description` enthält keine Regel, die nicht im Body steht. Habe das stichprobenweise für alle fünf genannten Änderungen geprüft.*

**K2 · Z53–59, „Vorgeschichte kurz" / „Was CronCreate ist" / „Gemessene Werte" · HISTORISCH · 2.199 B → ~450 B · −1,75 KB**
Zitat: *„Ein erster Test mit `ScheduleWakeup` (flaches `delaySeconds: 60`) zeigte, dass diese Methode mechanisch NIE schneller als alle 2 echten Minuten feuern kann …"* plus die Laientexterklärung „Was CronCreate ist (einfach erklärt)" und das Messprotokoll vom 10.07. (12:14–12:23, Abweichung +11 bis +24 s).
Das operative Vorgehen steht ab Z61. **Vorschlag:** Drei Absätze auf einen Satz: „`CronCreate` (`* * * * *`, `recurring: true`) ersetzt `ScheduleWakeup` seit 10.07.2026 — Letzteres konnte mechanisch nie schneller als alle 2 Minuten feuern. Messreihe und Erklärung: siehe Historie."
*Die Kernaussage „~10–25 s Anlauf, kein Drift, kein Skip" sollte als halber Satz erhalten bleiben — sie ist ein Betriebsparameter, kein Geschichtsdatum. Ist im Vorschlag enthalten.*

**K3 · Z421, „Bezug zu Punkt 2a" · HISTORISCH · 469 B → ~120 B · −0,35 KB**
Der operative Rest („`ScheduleWakeup` nur noch als Fallback") muss bleiben, die Erzählung („das bringt die Praxis zurück zu dem, was 2a von Anfang an vorsah, zwischenzeitlich aber ersetzt wurde") nicht.

**K4 · Z431–455, X-Tweets-Block · VERALTET + HISTORISCH · 8.251 B → ~2.600 B · −5,5 KB · GRÖSSTER EINZELVORSCHLAG**
Das ist die schlimmste Stelle der Datei und zugleich Widerspruch **W1** (siehe 3.1). Der Block enthält **vier** Fälligkeitsmechaniken übereinander:
- Z431 Überschrift mit `~~alle 60 Minuten~~ alle 10 Minuten`
- Z433 Why 60→10 (25.08., gilt)
- Z435 Why 10→60 (24.08., ausdrücklich „historisch — überholt")
- Z437 Why ursprünglich (10.07.)
- Z439 „How to apply" — 1.442 B mit **sieben** Durchstreichungspaaren, deren nicht-gestrichener Rest die Delta-Prüfung beschreibt, obwohl eine vorangestellte Klammer sagt, sie sei nicht mehr die Fälligkeitsquelle
- Z441 Why Modulo-Ersatz (25.08., „durch den Kerzenraster-Fix unten überholt")
- **Z443–449 Kerzenraster-Fix — die ALLEIN geltende Regel**
- Z451 „Abgrenzung zum Modulo-Einwand vom 25.08." — erklärt, warum ein überholter Einwand gegenstandslos ist

**Vorschlag:** Z433, Z435, Z437, Z439, Z441, Z451 vollständig in die Historie auslagern. Es bleiben: Z431 (Überschrift ohne Durchstreichungen, neu formuliert), Z443–449 (Kerzenraster-Fix + Nachhol-Regel, **unverändert**), Z453 (Anlass-Trigger), Z455 (How to apply, unverändert). Ergebnis: ein Block, in dem genau **eine** Fälligkeitsmechanik steht.
*Das ist die einzige Kürzung, die ich auch ohne Kontextgewinn empfehlen würde — hier kostet der Altbestand nicht nur Platz, sondern Klarheit.*

**K5 · Z949, Anti-Drift-Fix Punkt (b) · VERALTET · 2.234 B → ~600 B · −1,6 KB**
Dieselbe Delta-/Kerzenraster-Kollision ein zweites Mal, hier in Abschnitt 9. Der nicht durchgestrichene Text endet mit **„zurückkorrigiert 25.08.2026: ≥10 Minuten → Fetch fällig"** — fett, ungestrichen, obwohl seit 28.08. das Kerzenraster gilt. **Vorschlag:** Absatz (b) ersetzen durch: „b) Tweet-Fetch-Fälligkeit läuft seit 28.08.2026 über das feste Kerzenraster (Minute % 10 == 0, siehe Punkt 9 ‚Kerzenraster-Fix'); `x_last_fetch.json` nur noch Verpasst-Erkennung. Die Pflicht-Ausgabezeile ‚Tweet-Check: …' bleibt unverändert Pflicht in JEDEM Voll-Check, auch bei ‚nichts Neues'." Die drei Formatbeispiele der Pflichtzeile **bleiben wörtlich stehen**.

**K6 · Z966, „Why (%10-Streichung, 25.08.2026)" · VERALTET · 774 B → 0 · −0,76 KB**
Zitat: *„Die Delta-Variante ist als einzige zonenunabhängig UND unabhängig davon, ob ein Voll-Check exakt auf eine runde Minute fällt, und wird hiermit als alleinige verbindliche Fälligkeitsprüfung festgeschrieben."*
Dieser Satz **behauptet Verbindlichkeit** und trägt keinen Überholungs-Marker, obwohl er am 28.08. vollständig umgekehrt wurde. Ein Leser, der in Abschnitt 9a einsteigt, liest hier die falsche Regel als geltend. **Vorschlag: ersatzlos streichen** (der Inhalt steht in der Historie über K4).

**K7 · Z962, „How to apply" in 9a · HISTORISCH · 1.063 B → ~700 B · −0,35 KB**
Die Durchstreichungskette und der Klammer-Rückblick auf „die Delta-Ära 25.–27.08." raus; die geltende Anweisung (Minute aus echter Systemzeit, % 5 und % 10) bleibt wörtlich.

**K8 · Z117, Kopfabsatz „Vollständiger Soll/Ist-Abgleich 26.08.2026" · HISTORISCH · 1.195 B → ~300 B · −0,9 KB**
Der Absatz erklärt, warum die folgenden Pflichtzeilen in den Cron-Prompt aufgenommen wurden. **Der Codeblock danach (Z119–406) bleibt zu 100 % unangetastet** — das ist der Text, den Sonnet wörtlich in den CronCreate-Prompt kopiert. Nur die Einleitung wird zum Einzeiler: „Ab hier zusätzlich wörtlich in den CronCreate-Prompt übernehmen — ersetzt nichts Bestehendes (Soll/Ist-Abgleich 26.08.2026)."

#### Gruppe B — Abschnitt 7b1 (Entry-Freigabe)

> **Vorbemerkung:** Dies ist der gefährlichste Abschnitt der Datei. Alle Vorschläge hier betreffen **ausschließlich Klammer- und Why-Text**, nie eine Parameterliste, nie eine Rechtsfolge, nie eine Pflichtzeile.

**K9 · Z667–670, „Verschärfung 26.08.2026" + „Was am 25.08.2026 passiert ist" · HISTORISCH · 1.314 B → ~300 B · −1,0 KB**
Die geltende Regel ist ein Satz: „`gate_check.cjs` ist PFLICHT-Werkzeug, Kopfrechnung ist kein zulässiger Ersatz." Die Rekonstruktion der beiden #T1/#T3-Kopfrechnungen vom 25.08. (zwei ATR-Ablesungen, `slFloorGate: FAIL`) ist Herleitung.

**K10 · Z681, Schlussklammer des Schritt-5-Templates · HISTORISCH · 5.014 B → ~3.200 B · −1,8 KB**
Die Zeile ist die längste der Datei. Das **Kommandotemplate selbst (die ersten ~3.200 B) ist AKTIV-BINDEND und darf kein Zeichen verlieren.** Kürzbar ist ausschließlich die abschließende Kursiv-Klammer:
*„(Option A 14.09.2026 … Historie: Template-Korrektur 27.08.2026, Paket 6a … Template vervollständigt 31.08.2026 nach Opus-Gegencheck … Dritte Vervollständigung 31.08.2026, Opus-Drittprüfung Restbefunde 2+3 … Vierte Ergänzung 31.08.2026 Abend …)"*
Vier Vervollständigungsrunden desselben Templates. **Vorschlag:** ersetzen durch die einzige heute noch operative Aussage: „*Option A 14.09.2026: `--ema50-5min` ersetzt `--impuls-reifegrad-atr-emaanker` (Altparameter = Exit 1); `--runway-ratio` nur noch Override mit Pflichtgrund. `[--level-register]` ist optional (Default `scripts/level_register.json`), `--tp1-/--tp2-level-price` werden gegen das Register verifiziert (±2 Pkt), fehlende Registerdatei = Hard-Exit 1. Entstehungsgeschichte des Templates: siehe Historie.*"

**K11 · Z695, Klammer der `Gate-Check ausgeführt:`-Zeile · HISTORISCH · 2.898 B → ~1.100 B · −1,8 KB**
Identisches Bild: Das Template ist aktiv, die Klammer erzählt dieselben vier Vervollständigungsrunden **ein zweites Mal** („Template angeglichen 27.08.2026, Paket 6b/K2 … Erneut vervollständigt … Dritte Vervollständigung … Vierte Ergänzung … Fünfte Ergänzung"). **Erhalten bleiben muss** die Fünfte Ergänzung (Exit-Code-1-Klarstellung: „Exit-Code: 0/2 meint die Ausgänge der BEWERTUNG; Exit 1 ist ein realer dritter Ausgang und liefert keinen zitierbaren GESAMTSTATUS") — das ist eine geltende Betriebsregel, keine Historie.

**K12 · Z708 + Z710, „Strukturbefund" + „Historie P7" · HISTORISCH · 1.355 B → ~700 B · −0,65 KB**
Z710 zitiert: *„Am 04.09.2026 vormittags stand eine `Risiko-Ausnutzung (P7)`-Zeile im Skript … nach Opus' Gegencheck und Levis Klarstellung am selben Tag vollständig entfernt …"* plus die Korrektur der 50k-Annahme. Die Kapitalbasis-Korrektur gehört inhaltlich nach [[project_risikomanagement]] (steht dort bereits) — hier reicht ein Verweis. Der Strukturbefund (Z708: 1,5 %-Ziel nur über realisierte TP2 erreichbar) ist **eine geltende Einordnung** und bleibt.

**K13 · Z721–727, vier aufeinanderfolgende „Why"-Absätze · HISTORISCH · 3.113 B → ~900 B · −2,15 KB · ACHTUNG LINKS**
Vier `**Why:**`-Absätze hintereinander, die drei verschiedene Vorfälle (24./25.08., Spike-Ausnahme, #36/#39) erzählen, plus „Bewusst NICHT Teil dieser Regel" (kein Broker-Eingriff — **das bleibt**, es ist eine Abgrenzung, keine Historie).
**Zwingende Auflage:** Z725 ist die **einzige** Stelle der gesamten Datei, die [[trades/trading_2026-08-17]] und [[trades/trading_2026-08-19]] verlinkt. Die Kurzfassung muss beide Links tragen, z. B.: „**Why:** n=2, gleiches Muster — Trade #36 ([[trades/trading_2026-08-17]], Entry vor abgeschlossener RR-Prüfung, LOSS) und Trade #39 ([[trades/trading_2026-08-19]], identischer Ablauf, WIN — nach [[feedback_regeldisziplin]] kein Freispruch)."

#### Gruppe C — Abschnitt 7b1a (Q-Score)

**K14 · Z740–742, AVWAP-Statusnotiz + zwei Updates · VERALTET · 2.743 B → ~550 B · −2,15 KB**
Z740 ist **vollständig durchgestrichen** (`~~Statusnotiz 25.08.2026 … noch nicht live verifiziert~~`), gefolgt vom Update, das sie aufhebt, gefolgt von Z742 (Update 26.08.: doppelte VWAP entfernt) — ein dreistufiger Verlauf über eine Frage, die seit dem 26.08. beantwortet ist. **Vorschlag:** ein Satz — „Der Q1-Beleg über `Anchored VWAP (Remote)` ist seit 25.08.2026 live verifiziert und nutzbar (Anker per `indicator_set_inputs`, `in_0` = Unix-Sekunden); Anker-Limit max. 1×/Session + 1×/Position, siehe [[feedback_chart_layout]]." Der offene Punkt „Persistenz über `tv_launch`-Neustart noch nicht getestet" muss als Halbsatz mit — **das ist noch offen**, nicht Historie.

**K15 · Z743, durchgestrichener Q3-Tie-Breaker + Begründung · VERALTET · 1.912 B → ~900 B · −1,0 KB**
Der durchgestrichene Tie-Breaker-Text plus die RVOL-Umstellung von 24.08. plus die Rückstufung von 25.08. Geltend ist nur: „Q3 bestimmt sich ausschließlich aus dem MTF-Dual-Gate-Ergebnis (5min/15min/1H NAS100 + QQQ). `--qqq-volume-below-avg` wird entgegengenommen, rein informativ angezeigt, fließt NICHT in `factors.q3.status` ein (GEMESSEN, KEIN GATE seit 25.08.2026)." Die 0,84–1,06-Spannen-Begründung und der #T1-Fall gehen in die Historie.

#### Gruppe D — Abschnitte 9, 11, 12, 12.5, 13, 14, 15

**K16 · Z940–945, Root-Cause-Analyse des Anti-Drift-Fixes · HISTORISCH · 1.901 B → ~400 B · −1,5 KB**
Drei nummerierte Punkte über die 100+ Ticks vom 27.07. und warum ein „ab jetzt dran denken" kein Fix ist. Der **Fix selbst (a/b/c ab Z946) bleibt vollständig.**

**K17 · Z952, „Ehrliche Einordnung" · HISTORISCH · ~1.100 B → ~300 B · −0,8 KB**
*Unsicher — defensiv zu behandeln.* Der Absatz sagt, dass eine Pflichtzeile mechanisch mitgeschrieben werden kann, ohne dass die Prüfung stattfand, und dass ein Fortbestehen explizit zu benennen ist. Das ist **halb Meta-Regel, halb Reflexion**. Ich würde den letzten Satz („Falls sich zeigt, dass die Pflicht-Zeilen mechanisch ohne echte Prüfung mitlaufen, ist das explizit als Fortbestehen des Problems zu benennen") **wörtlich behalten** und nur die Herleitung davor kürzen.

**K18 · Z1013, „RVOL statt Kopf-Vergleich" · HISTORISCH · 2.264 B → ~800 B · −1,45 KB**
Die Herleitung (Levis Handelsfenster liegt nach dem US-Open-Peak, ein flacher Durchschnitt unterschätzt späte Bewegungen systematisch) ist lehrreich, aber für die Anwendung reicht: „Volumen-Bestätigung wird am Indikator **Relative Volume at Time** (RVOL, QQQ-Pane) abgelesen, nicht am geschätzten Durchschnitt — zeitnormalisiert. 50 %-Schwelle unverändert. Gilt identisch für [[feedback_chartanalyse]] 8a3, Punkt 7b und den Q3-Tiebreak. GEMESSEN, KEIN GATE (25.08.2026)."

**K19 · Z1005 + Z1011, Belegpflicht-Vorfall + Why zum Volumen-Dämpfer · HISTORISCH · 1.935 B → ~1.000 B · −0,9 KB**
**Wichtig:** Das Belegformat-Beispiel in Z1005 (*„MACD-H 5min: -2,1, seit 2 Checks negativ ✓ | EMA50: … | QQQ: … ✗ | 15min: … ✗ → 2/4, Exit gerechtfertigt"*) ist eine **Formatvorlage und muss wörtlich bleiben**. Kürzbar ist nur die Trade-#24-Vorgeschichte davor und die 99–154K-vs-195–204K-Messung in Z1011.

**K20 · Z1041–1043 + Z1055–1058 + Z1065 (Punkt 12, Begründungsabsätze) · HISTORISCH · 1.454 B → ~500 B · −0,95 KB**
„Fable-Review-Ergebnis (21.07.2026)", „Warum 3 Kerzen", „Warum Teilgewinn statt Vollexit", „Bewährt bei Trade #23". **Achtung:** „Warum 3 Kerzen" begründet eine Zahl, die durch 12.3 auf 2 geändert wurde — der Absatz ist damit **nicht nur historisch, sondern irreführend** (siehe W2). Er sollte in jedem Fall weg oder auf 12.3 umgeschrieben werden.

**K21 · Z1079, „Vorgeschichte / Weg dorthin" in 12.3 · HISTORISCH · 1.313 B → ~300 B · −1,0 KB**
Erzählt Trade #28 und die zwei verworfenen Alternativen (a)/(b). Die Aussage, die bleiben muss: „Kerzenschluss-Pflicht + RSI-Magnitudenschwelle bleiben als Rauschfilter erhalten, nur beide Zahlen werden halbiert" — das steht bereits in Z1084/Z1099.

**K22 · Z1118–1127, 12.5 Fall-für-Fall-Liste · HISTORISCH · 3.468 B → ~900 B · −2,5 KB**
Sieben Trades einzeln nacherzählt (#23/#25/#27/#28/#32/#33/#35). **Das Verdikt (Z1128–1129) ist die operative Substanz und bleibt wörtlich** — insbesondere der offene Rest-Gap: „12.4 ist durch keinen einzigen Fall bestätigt; beim nächsten echten kerzenschluss-basierten Punkt-12-Trigger explizit gegenchecken, ob die ‚Stall-Check: …'-Zeile bei jedem Check-in erscheint." **Vorschlag:** Falltabelle auf sieben Zeilen à einem Satz eindampfen oder komplett in die Historie, Kernbefund (1 von 7 regelkonform) als Satz behalten.

**K23 · Z1147–1150, 13.1 Vorgeschichte 30.07. · HISTORISCH · 1.169 B → ~350 B · −0,8 KB**
Der 14:30-Post-PCE-Breakout und das Levi-Zitat. **Die Hindsight-Warnung (Z1155) und die Präzisierung (Z1153, 2-Voll-Check-Schwelle) bleiben — beide sind geltende Regel.**

**K24 · Z1179, „Why" zu Punkt 14 · HISTORISCH · 1.296 B → ~420 B · −0,9 KB · ACHTUNG LINK**
**Z1179 ist die einzige Stelle der Datei, die [[trades/trading_2026-08-21]] verlinkt.** Die Kurzfassung muss den Link tragen. Die operative Substanz („eine Checkliste, die in einer Woche von 2 auf 9+ Pflichtzeilen wächst, wird unter Zeitdruck selbst zur Fehlerquelle") in einem Satz behalten, die Abgrenzung zu [[feedback_dont_change_running_system]] am Ende **wörtlich behalten**.

**K25 · Z1185, „Auslöser" zu Punkt 15 · HISTORISCH · 966 B → ~280 B · −0,7 KB**
Der 25.08.-Ablauf (19:36 „stop" → „Ah okay du bist im Trade" → 20:00 unresolved). Die Pflichten (a)–(d) darunter bleiben vollständig.

#### Gruppe E — Kürzungen „mit Vorbehalt" (K26–K30)

> Hier bin ich mir **weniger sicher**. Der Text ist historisch, aber er trägt jeweils ein Beispiel oder eine Schwellenbegründung, die im Zweifel mehr wert ist als der Platz. Ich schlage sie vor, empfehle sie aber nicht aktiv.

**K26 · Z541–556, 7d0-Rückfallgeschichte · 4.175 B → ~2.100 B · −2,05 KB**
Kürzbar: der Trade-#29-Vorfall, die „3. Rückfall in Folge"-Erzählung (03./04./05.08.), das Fable-Zitat. **Unbedingt behalten:** das Format `Kerze geschlossen: JA (Bar-Start HH:MM + <Timeframe>min = HH:MM:00 ≤ jetzt HH:MM:SS)` **samt dem #T2-Gegenbeispiel** (*„Kerze geschlossen: NEIN (Bar-Start 14:40 + 5min = 14:45:00 > jetzt 14:44:53)"*) — das Beispiel ist der Grund, warum das Format überhaupt funktioniert.

**K27 · Z859–862, 7c fünfte Erweiterung, Vorfall Trade #28 · 1.959 B → ~600 B · −1,35 KB**
Die Kosten-Asymmetrie-Begründung ist gut und knapp formulierbar; die Rekonstruktion der 16:38–16:49-Sequenz und der zwei Levi-Nachfragen ist Herleitung. *Vorbehalt: Die Regel (5 bzw. 3 Checks) steht unter offener Review-Pflicht (Z875) — solange die läuft, ist der Ursprungsfall der einzige Datenpunkt.*

**K28 · Z793 + Z801 + Z804–807 + Z810 + Z815, Paket-6a-Korrekturvermerke in 7b1c · ~3.400 B → ~1.500 B · −1,9 KB**
Fünf Kursiv-Klammern der Bauart *„(Korrektur 27.08.2026, Paket 6a: QQQ stand hier fälschlich auf 5min — das lockerte die Bedingung um Faktor 3)"*. **Vorbehalt, und ein ernster:** Diese Vermerke sagen nicht nur, *dass* korrigiert wurde, sondern *warum die Zahl so ist*. Bedingung 4 trägt zusätzlich den Hinweis „Feuert die Klasse in Flaggen, ist diese Bedingung die Stelle zum Nachschärfen" — **das ist eine Wartungsanweisung für die noch ausstehende Kalibrierung (Z817, 10–15 Anwendungsfälle) und muss bleiben.** Ich würde hier nur die drei Vermerke kürzen, die reine Umsetzungsfehler dokumentieren (Z801, Z810, Z815).

**K29 · Z916–919, Punkt-9-Verschärfungshistorie · 1.372 B → ~600 B · −0,75 KB**
Trade #16 (TP1-Erreichen ändert nichts an der Voll-Check-Pflicht) — **die Regel bleibt**, die Nacherzählung des Audits vom 21.07. („Fibonacci nur opportunistisch, 9d-Liste nur reaktiv") kann weg. *Vorbehalt: der Satz „lieber ein ehrlich reduzierter, realistisch einhaltbarer Standard als eine Vollständigkeit, die nur auf dem Papier steht" ist die Designphilosophie des ganzen Abschnitts.*

**K30 · Z934 + Z936, Why-Teile der Offenlegungspflicht und Screenshot-Konvention · 2.212 B → ~1.400 B · −0,8 KB**
Kürzbar sind die Vorfallsbeschreibungen (27.08. 19:11–19:17, „Tag 1 kodierte zwischen #31 und #59 die Simulationszeit", „63 von 84 Protokollreferenzen"). **Die Verbote selbst bleiben wörtlich** — keine simulierten Zeiten im Dateinamen, keine doppelten `.png`-Endungen.

---

## 3. Gefundene Widersprüche und Doppeldefinitionen

**Das ist der Teil, der unabhängig von jeder Kürzungsentscheidung eine Antwort braucht.**

### W1 — Tweet-Fetch-Fälligkeit: drei Mechaniken, vier Fundstellen, zwei davon ohne Überholungsmarker · SCHWERE: HOCH

| Fundstelle | Was dort steht | Status |
|---|---|---|
| Z439 („How to apply") | Delta ≥10 Min gegen `x_last_fetch.json` | vorangestellte Klammer sagt „nur noch Verpasst-Erkennung" — der Fließtext liest sich aber weiter wie eine Regel |
| **Z443–449 (Kerzenraster-Fix)** | **Minute % 10 == 0, „ab jetzt die ALLEINIGE verbindliche Fälligkeitsmechanik"** | **gilt** |
| Z949 (b) | „**zurückkorrigiert 25.08.2026: ≥10 Minuten → Fetch fällig**" — fett, **nicht** durchgestrichen | überholt, ohne Marker |
| Z966 („Why %10-Streichung") | „Die Delta-Variante … **wird hiermit als alleinige verbindliche Fälligkeitsprüfung festgeschrieben**" | überholt, **ohne jeden Marker** |

Zwei Stellen erklären die Delta-Prüfung mit dem Wort „alleinige verbindliche" für gültig, eine dritte erklärt mit denselben Worten das Kerzenraster für gültig. Praktisch führen beide Mechaniken meistens zum selben Ergebnis — aber nicht bei einem verzögerten Fire, und genau dort hat der 27.08. fünf versäumte Fetches produziert.
**Unabhängig von jeder Kürzung: Z966 streichen, Z949 (b) und Z439 auf die Kerzenraster-Fassung bereinigen.** Umgesetzt über K4/K5/K6/K7.

### W2 — Punkt 12: alte Schwelle (3 Kerzen / RSI 8–10) steht 25 Zeilen vor der neuen (2 Kerzen / RSI ≥5) · SCHWERE: HOCH

- Z1046–1048 (Regel Punkt 1 + 1a): „ab der **4.** aufeinanderfolgenden Kerze … RSI **≥8-10** Punkte"
- Z1052 (Regel Punkt 2): „**3** aufeinanderfolgende 5-Min-Kerzenschlüsse … RSI **mindestens 8-10** Punkte"
- Z1055: „**Warum 3 Kerzen:** Lang genug, um normales kurzes Verschnaufen nicht fälschlich als Stillstand zu werten"
- Z1067 (How to apply): „Nach der **3.** Kerze … ab der **4.** Kerze"
- **Z1081–1085 (12.3): „ersetzt ‚3 Kerzen / 8-10 Punkte' aus Punkt 2 oben ab jetzt" → 2 Kerzen / RSI ≥5, 1a-Override ab der 3. Kerze**

An **keiner** der vier alten Stellen steht ein Hinweis auf 12.3. Wer Punkt 12 von oben liest und bei „How to apply" (Z1067) aufhört — was im Loop das Wahrscheinliche ist —, wendet die falsche Schwelle an. Die Pflichtzeile in 12.4 (Z1107) zählt dagegen gegen 12.3.
**Vorschlag, minimal und ohne Kürzung:** In Z1046, Z1048, Z1052 und Z1067 jeweils die Zahlen durch die 12.3-Werte ersetzen und dahinter `(verschärft 31.07.2026, Punkt 12.3)` setzen; Z1055 („Warum 3 Kerzen") streichen oder auf 2 umschreiben. **Das ist der einzige Vorschlag dieses Berichts, den ich unabhängig von deiner Kürzungsentscheidung empfehle.**

### W3 — `--cluster-level`: `--grund-`-Ausweg gleichzeitig erlaubt und verboten · SCHWERE: MITTEL

- Z672 (Schritt 0) und Z698: „`--sl-anker` und `--cluster-level` sind im Live-Aufruf A3-pflichtig und — wie `--override-1h-close` — **ohne `--grund-`-Ausweg**" (Opus-Gegencheck 11.09., Auflage 3)
- **Z688 (Befüllungspflicht):** „Fehlt der Parameter, läuft 8c2 auf UNKNOWN statt stillem PASS (**mit `--grund-cluster-level "..."`**, ohne Grund Exit 1)"

Z688 beschreibt noch den Stand vom 03.09. Da genau dieser Ausweg laut Z672 am 10.09. live ein falsches PASS erzeugt hat, ist das kein Schönheitsfehler.
Zusätzlich stehen drei verschiedene Wertelisten nebeneinander: `<register|Preis|tief-hoch|none>` (Z672/681/698), `<register|Preis|none>` (Z190) und `<Preis|none>` (Z688/695).
**Vorschlag:** Z688 auf die 11.09.-Fassung angleichen, alle vier Wertelisten vereinheitlichen. Keine Kürzung, reine Korrektur.

### W4 — Der „5-Schritte-Ablauf" hat acht Schritte, und die Kurzfassung im Cron-Prompt lässt den bindenden Schritt 0 aus · SCHWERE: MITTEL

Z247 (Item (8) im CronCreate-Block) beschreibt 7b1 als: „ATR ablesen → SL-Floor rechnen → SL=MAX(Struktur,Floor) → erst danach TP/RR → `gate_check.cjs` aufrufen". **Schritt 0 (SL-Anker-Vorprüfung) fehlt** — obwohl er seit 07.09.2026 von Levi bestätigt bindend ist und *vor* allen anderen läuft. Er steht im selben Codeblock separat als Item (6f), aber nur für Voll-Checks mit beobachtetem Setup, nicht in der Order-Sequenz.
Der Abschnitt heißt außerdem „Ablauf aus fünf Schritten" und führt 0, 1, 2, 3, 3a, 4, 4b, 5.
**Vorschlag:** Item (8) um „Schritt 0 (SL-Anker-Vorprüfung, Rechtsfolge E3c) → " am Anfang ergänzen; Überschrift auf „Ablauf" ohne Zahl. Keine Kürzung.

### W5 — Punkt 11: „2-3 von 4" ist keine entscheidbare Schwelle · SCHWERE: NIEDRIG (Altbestand, defensiv nicht anfassen)

Z999 sagt „mindestens 2-3 dieser Kriterien", Z1005 zeigt ein Beispiel mit Verdikt „→ 2/4, Exit gerechtfertigt", Z1009 spricht von „selbst ein technisch erfülltes 2/4-Kriterium". Faktisch gilt 2 als Schwelle und 3 als Komfort — ausgesprochen ist es nirgends. **Ich schlage hier keine Änderung vor** (das wäre eine Regeländerung, nicht eine Bereinigung), notiere es aber als offene Frage für dich (Abschnitt 7, Frage 5).

---

## 4. Was ich NICHT anfassen würde — und warum

Diese Liste ist bewusst lang. Wo ich zwischen „Platz sparen" und „Regelverlust-Risiko" abwägen musste, habe ich durchgehend gegen das Kürzen entschieden.

**N1 · Der komplette CronCreate-Codeblock, Z119–406 (~14 KB).**
Das ist der Text, der wörtlich in den Cron-Prompt kopiert wird — Items (1) bis (13), inklusive der SL-Anker-Vorprüfung (6f), der Retest-Zeitbox (6c), des Register-Checks (6d) und der beiden Aufrufwege mit Redirect und Exit-Code. Er ist **lang, weil er vollständig sein muss**: Die gesamte Datei dokumentiert an vier Stellen (16.07., 27.07., 26.08., 09.09.), dass jede Regel, die *nicht* wörtlich in diesem Block steht, strukturell ausfällt. Ein hier gekürzter Satz ist eine Regel, die im Loop nicht mehr ankommt. **Kein Zeichen.**

**N2 · Z672, Schritt 0 (SL-Anker-Vorprüfung), 4.675 B.**
Enthält Kommandotemplate, `--sl-auto`-Formel `MAX(Anker ∓ 0,5×ATR, sichere Cluster-Kante ∓ 0,5×ATR, 8c-Floor)`, die Einmal-pro-Session-Zonenfrage, die Rechtsfolge („kein 7b1-Ablauf"), die N2-Lockerung mit `--repeat` und die drei Urteile. Der historische Anteil (Auszählung 10.09., 38×/3×/2×) ist hier **Beleg für die Kalibrierung einer noch jungen Regel**, nicht Erzählung. Defensiv: Volltext.

**N3 · Z676, Schritt 3a (SL-Anker-Definition), 2.843 B.**
„Anker ist Bezugspunkt, NIE der SL" ist die frischeste harte Regel der Datei (11.09.) und adressiert das #42/#43-Muster. Die VC#54-Rechnung (Cluster-Kante 29.250 vs. Anker 29.249,95, 0,05 Pkt Unterschied) ist der einzige durchgerechnete Beleg, dass die Kante und nicht der Anker misst. **Volltext.**

**N4 · Z736–737, Q2- und Q4-Definition, 3.226 B.**
Beide **seit gestern/heute** verbindlich (Option A / Q4-a, Levi-Entscheidung 14.09.). Die eingeklammerte Historie (alte Formel, Median 14,2×) ist erst Tage alt und gehört zum laufenden Prüfkriterium in Z756. **Nicht anfassen, bis das 15-Momente-Fenster ausgewertet ist.**

**N5 · Z756, Prüfkriterium Option A vor dem nächsten Echtgeld-Trade.**
Offener Auswertungsmaßstab mit Zählerstand 0/15. Aktiv.

**N6 · 7b1b vollständig (Z762–788, 6.179 B).**
Sieht wie Historie aus („Auslöser dieser Regel: Testtag 24.08."), ist aber eine **laufende Datensammlung** mit noch nicht festgelegtem N und drei bzw. zwei DB-Spalten, die nur hier dokumentiert sind. Der Auswertungszweck (Verhältnis `dual_gate_qqq_abstand_pct` zu `dual_gate_q2_budget_pct`) steht nirgends sonst. *Unsicher, ob die Messung noch läuft — defensiv beibehalten.*

**N7 · 7b1c Bedingungen 1–5 (Z803–808) + Review-Pflicht (Z817).**
Alle Schwellen sind ausdrücklich **unkalibriert** und warten auf 10–15 Anwendungsfälle. Bedingung 4 trägt die Nachschärf-Anweisung. Volltext, bis die Kalibrierung gelaufen ist.

**N8 · Alle Pflichtzeilen-Formatvorlagen, überall.**
`Entry-Freigabe: …`, `Gate-Check ausgeführt: …`, `Stall-Check: …`, `Volumen-Check: …`, `Chasing-Status (13): …`, `Retest-Zeitbox: …`, `Kerze geschlossen: …`, `Tweet-Check: …`, `Levelsuche TP1/TP2: …`, `Gate-Eingaben: …`, `Ereignis-AVWAP-Reset: …`, `Spike-Ausnahme: …`, `9b-Divergenz-Check: …`, `Terminalbedingung …`. Zusammen mehrere KB — und jede einzelne wird im Output gegengeprüft. **Kein Zeichen.**

**N9 · Z1022–1037, Ausführungsschritt Entry-AVWAP-Anker (7.815 B).**
Enthält die Millisekunden-vs.-Sekunden-Falle, die Pflicht-Rücklese-Verifikation (weil die Studie **kommentarlos** in den Fallback fällt) und die Reset-Pflicht 4a. Drei dokumentierte Fehlerquellen, alle mit realem Vorfall. **Volltext.**

**N10 · Z1034 + Z1037, die offene Kollision zweiter Ordnung.**
„Entry-Anker und Q1-/Katalysator-Anker teilen sich dieselbe Ereignis-Instanz, es gibt KEINE Priorisierungsregel — Levi entscheidet im Moment." Das ist ein **offener Punkt**, kein Verlauf.

**N11 · Z1128–1129, das Verdikt von 12.5.**
„12.4 ist durch keinen einzigen Fall bestätigt … beim nächsten echten Trigger explizit gegenchecken." Offene Auflage.

**N12 · Punkt 14 (Regeländerungs-Tempo-Bremse) und Punkt 15 (Solo-Mandat) als Regeltexte.**
Beide kurz, beide aktiv, beide mit prüfbaren Kriterien. Nur die Auslöser-Erzählungen (K24/K25) sind Kandidaten.

**N13 · Z704 (P7) und Z706 (RR-Definition).**
„RR heißt am Entry immer die TP1-Zahl", „Renditeziel ist KEIN Entry-Kriterium". Zwei Sätze, beide von Levi abschließend entschieden, beide werden regelmäßig falsch erinnert. **Volltext.**

**N14 · Z639–644, Begriffs- und Zählkonvention Dual-Gate.**
„2-beinig", „2/2-ZUSTAND" vs. „2/2-TRIGGER-EREIGNIS", 1H-Override-Definition. Wird in der Tagesbilanz getrennt gezählt. Sieht wie Terminologie-Pflege aus, ist aber Zählgrundlage.

**N15 · Z714–717, Ein-Fassungs-Regel + `quote_check.cjs`.**
Dritte Verschärfung derselben Pflicht nach drei Rückfällen. Aktiv, maschinell geprüft.

---

## 5. Warum ich weiterhin von der Zweiteilung (E7) abrate

Der allgemeine Bericht hat E7 („Geltender Stand"-Kopf + „Herleitung"-Teil) als größten Hebel mit hohem Risiko eingestuft und nicht empfohlen. Nach der vollständigen Lektüre **bestätige ich das und kann jetzt sagen, warum konkret:**

1. **Regel und Begründung stehen im selben Satz, nicht in getrennten Absätzen.** Beispiel Z688: „`--dual-gate-q2-budget-pct` seit 03.09.2026 nie mehr per Ausnahme weglassen: Die Formel ist im Skript festgeschrieben — `(1,5 − Q2) / 1,5 × 100` — und fehlt der Wert, berechnet `gate_check.cjs` ihn SELBST." Regel, Formel, Historie und Code-Verhalten in 25 Wörtern. Eine Trennung heißt hier: neu schreiben.
2. **Die Reihenfolge trägt Bedeutung.** In 7b1 stehen Schritt 0 → 1 → 2 → 3 → 3a → 4 → 4b → 5 in Ausführungsreihenfolge. Ein „Kopf", der nur die Endfassungen sammelt, verliert genau die Sequenz, die die Regel ausmacht.
3. **Die Datei hat 129 interne Querverweise der Form „siehe Punkt 7b1 Schritt 4b", „Details in Punkt 2b Item (7)/(8)".** Eine Umstrukturierung bricht jeden einzelnen davon, und keiner davon ist maschinell prüfbar wie ein Wiki-Link.
4. **Der Gewinn ist kleiner als gedacht.** Mein gemessenes Maximum an reiner Herleitung sind 42 KB von 280 KB. Selbst eine perfekte Trennung käme nicht auf −75 %, sondern eher auf −25 bis −35 % — weil der Rest tatsächlich geltende Regel ist.

**Der Ertrag rechtfertigt das Risiko nicht.** Wenn du Session-Start-Kontext sparen willst, ist die Auslagerung der 42 KB Historie in eine zweite Datei (Abschnitt 1.6) das bessere Geschäft: dasselbe Risiko wie ein Textumzug, nicht wie eine Neuformulierung.

---

## 6. Cross-Referenz-Check

**Geprüft:** alle 129 Wiki-Link-Vorkommen (45 Ziele) gegen die 30 Kürzungsvorschläge.

**Drei Links stehen als einziges Vorkommen der Datei in einem Kürzungsvorschlag** — sie müssen in der jeweiligen Kurzfassung erhalten bleiben:

| Link | Zeile | betroffener Vorschlag | Auflage |
|---|---|---|---|
| [[trades/trading_2026-08-17]] | 725 | K13 | Link in die Why-Kurzfassung übernehmen |
| [[trades/trading_2026-08-19]] | 725 | K13 | Link in die Why-Kurzfassung übernehmen |
| [[trades/trading_2026-08-21]] | 1179 | K24 | Link in die Why-Kurzfassung übernehmen |

Diese drei sind besonders heikel: Laut [[project_memory_aufraeumen_2026-09-14]] (1.5) sind 17 `trades/`-Dateien über keinen Link erreichbar. Fallen diese drei Links weg, werden aus 17 unerreichbaren Dateien 20.

**Alle übrigen in Kürzungsvorschlägen enthaltenen Links haben mindestens ein zweites Vorkommen außerhalb:**
- [[project_opus_vollpruefung_2026-08-24]]: Z431 (K4) **und** Z750 (bleibt) ✓
- [[feedback_prozessfehler_27_07_fuer_fable]]: Z435 (K4) **und** Z938 (bleibt — K16 setzt erst bei Z940 an) ✓
- [[feedback_dual_gate_confirmation]]: Z723 (K13) **und** Z663 (bleibt) ✓
- [[feedback_broker_wert_prioritaet]]: Z727 (K13, bleibt ohnehin) **und** Z892 ✓
- [[project_regelwerk_audit_2026-08-07]]: Z1113 (Überschrift, bleibt) **und** Z1063 ✓
- [[project_testtag_analyse_2026-08-24]] (15×), [[project_testtag_analyse_2026-08-25]] (6×), [[project_testtag_analyse_2026-08-27]] (9×), [[feedback_chart_layout]] (19×), [[feedback_chartanalyse]] (49×), [[feedback_vollcheck_format]] (22×): jeweils reichlich abgedeckt ✓

**Einbahn-Beobachtung (kein Kürzungsthema, aber passend zum allgemeinen Bericht):** Diese Datei verlinkt [[testtag/testtag_2026-08-28]] (Z1032) — das ist die einzige Verankerung dieses Rohprotokolls im Memory. Sie liegt ausgerechnet in der Reset-Pflicht 4a, die ich zum Volltext-Erhalt empfehle (N9). Passt gut zusammen: nicht anfassen.

**Rückwärtsrichtung:** [[feedback_live_trading]] wird von `MEMORY.md` und mindestens 12 weiteren Dateien referenziert, immer als Ganzes („[[feedback_live_trading]] Punkt 7b1", „Punkt 12", „Punkt 14"). **Solange die Punkt-Nummern erhalten bleiben, bricht nichts.** Alle 30 Vorschläge respektieren die Nummerierung — keiner löscht oder verschiebt eine Überschrift.

---

## 7. Offene Fragen für Levi

1. **Bereinigung der Widersprüche — auch ohne Kürzung?** W1 (Tweet-Fälligkeit) und W2 (Punkt-12-Schwelle) sind Stellen, an denen Sonnet im Loop die falsche Fassung lesen kann. Beide sind in ~20 Minuten bereinigt, ohne dass ein Byte Inhalt verloren geht. **Meine Empfehlung: ja, unabhängig von allem anderen.** Zustimmung?
2. **Auslagern statt löschen?** Sollen die als kürzbar markierten Passagen wörtlich in eine neue `feedback_live_trading_historie.md` wandern (mein Vorschlag) oder tatsächlich entfallen? Auslagern kostet nichts außer einer Datei, die nicht automatisch geladen wird.
3. **Welche Gruppen?** A (2b/Frontmatter, −12,4 KB) / B (7b1, −7,4 KB) / C (7b1a, −3,15 KB) / D (9/11/12/13/14/15, −10,1 KB) / E (mit Vorbehalt, −8,9 KB). Du kannst einzeln zustimmen; die Gruppen sind unabhängig.
4. **12.5 (Review-Abschluss 12.08.) — abgeschlossen oder offen?** Das Verdikt sagt „NICHT als vollständig validiert geschlossen, vertagt auf den nächsten Fall mit echtem kerzenschluss-basiertem Punkt-12-Trigger". Ist der inzwischen aufgetreten? Wenn ja, ist K22 unkritisch; wenn nein, bleibt der Abschnitt aktiv und K22 sollte nur die Falltabelle betreffen.
5. **W5 — Punkt-11-Schwelle „2-3 von 4":** Soll das auf eine Zahl festgelegt werden? Das wäre eine **Regeländerung**, nicht eine Bereinigung, und fällt damit unter Punkt 14 (Tempo-Bremse). Ich habe bewusst nichts vorgeschlagen.
6. **Läuft die 7b1b-Schattenmessung noch?** Falls die Auswertung („nach N Sessions, noch kein festes N") gar nicht mehr vorgesehen ist, wären hier weitere ~4 KB frei. Ich habe defensiv auf „läuft" entschieden.
7. **E7 endgültig zu den Akten?** Ich empfehle, die Zweiteilung von `feedback_live_trading.md` nicht als „später" zu führen, sondern als **abgelehnt** zu markieren, mit diesem Bericht als Begründung — sonst taucht sie in jedem künftigen Aufräum-Durchgang erneut als „größter Hebel" auf.

---

## 8. Ehrliche Einordnung dieses Berichts

Was dieser Bericht **nicht** leisten kann: Ich habe die Regeln gegen den Text der Datei geprüft, nicht gegen den laufenden Code. Wenn `gate_check.cjs` sich heute anders verhält, als Z672/681/688 beschreiben, würde ich das hier nicht sehen. Für W3 (`--grund-cluster-level`) wäre ein Blick in `scripts/gate_check.cjs` der saubere Gegencheck — den habe ich bewusst nicht gemacht, weil der Auftrag „nur diese eine Datei" lautete.

Was ich mit Sicherheit sagen kann: **Die Datei ist nicht aufgebläht, sie ist vollständig protokolliert.** 85 % sind geltende Regel. Der Rest ist der Preis für die Zitierpflicht und für Autor≠Prüfer — beides Vorgaben, die ich für richtig halte ([[project_memory_aufraeumen_2026-09-14]], Abschnitt 5). Die 42 KB, die ich anfassen würde, sind kein Versäumnis, sondern der normale Bodensatz eines Dokuments, das 14 Wochen lang jeden Tag fortgeschrieben wurde.

**Wenn du nur eine Sache aus diesem Bericht umsetzt, dann W1 und W2.** Die kosten keinen Kontext, sie sparen auch keinen — sie verhindern, dass der Loop eine überholte Zahl anwendet.

---

## W1-W5 korrigiert 14.09.2026 (Fable) — Opus-Gegencheck ausstehend

**Auftrag:** Nur die fünf Widersprüche aus Abschnitt 3, minimal-invasiv, keine Kürzung (K1-K30 unberührt, Levi-Entscheidung weiter offen). Datei danach **1.204 Zeilen / 284.579 B** (vorher 1.199 / 280.057 — +4,5 KB, weil nichts gelöscht, nur markiert/ergänzt). Alle **45 Wiki-Link-Ziele unverändert** (gegen HEAD `efcac0e` gemessen). `node --test tests/trading_scripts.test.js`: **73/73 PASS** — der einzige Test, der die Datei liest (Z1592: beide Kommandotemplates müssen `--ema50-5min` und `[--runway-ratio` tragen), ist von W3 nicht berührt. Kein Commit, kein Push. Zeilenangaben unten = Stand VOR der Korrektur (wie im Bericht oben); nach W4 (Item 8) verschieben sich Zeilen ab 247 um +3, nach W5 ab 999 um +2.

### W1 — Tweet-Fetch-Fälligkeit

> **ÜBERHOLT 14.09.2026 (N1-Klärung, Opus-Gegencheck) — vor dem Weiterverwenden lesen:** Die unten dokumentierte W1-Fassung („verbindlich ist ausschließlich das Kerzenraster“, „das Delta entscheidet NICHT mehr über die Fälligkeit“) war in ihrem Kern richtig (das Delta ALLEIN löst seit 28.08.2026 nicht mehr aus), in ihrer Absolutheit aber falsch: Seit 03.09.2026 („Punkt 17“) gilt die **Doppelbedingung** Raster-Slot % 10 == 0 **UND** Delta ≥ 10 Min, maschinell erzwungen seit 09.09.2026 (`x_fetch_stamp.cjs`: `faellig = rasterSlot && deltaOk`). `feedback_live_trading.md` wurde am 14.09.2026 an neun Stellen darauf nachgezogen. **Dieser Abschnitt ist ab hier Historie — nicht erneut als Korrekturvorlage in Richtung „Raster allein“ verwenden.** Herleitung und Belege: [[project_n1_tweet_faelligkeit_klaerung_2026-09-14]].

(a) Vorgefunden wie beschrieben: Z949 (b) endete fett/ungestrichen mit „zurückkorrigiert 25.08.2026: ≥10 Minuten → Fetch fällig … robuster als reine Modulo-Prüfung"; Z966 „wird hiermit als alleinige verbindliche Fälligkeitsprüfung festgeschrieben" ohne jeden Marker. **Zusatzfund (Abweichung vom Bericht):** Z439 trug zwar die Vorab-Klammer „nur noch Verpasst-Erkennung", enthielt aber weiter „NICHT eine Minute-Modulo-Prüfung" — das widerspricht dem Kerzenraster direkt (das IST eine %10-Prüfung). Ebenfalls korrigiert.
(b) Änderungen:
- Z966: Überschrift → „Why (%10-Streichung, 25.08.2026 — HISTORISCH, am 28.08.2026 durch den Kerzenraster-Fix vollständig umgekehrt; W1-Korrektur 14.09.2026)", Vorab-Klammer „KEINE geltende Regel mehr … verbindlich ist ausschließlich das Kerzenraster"; Satzende „~~und wird hiermit als alleinige verbindliche Fälligkeitsprüfung festgeschrieben~~ *(Festschreibung vom 25.08.2026 — AUFGEHOBEN 28.08.2026 …)*". Nicht ersatzlos gestrichen (das wäre K6 = Kürzung), sondern markiert.
- Z949 (b): der ganze Teil „— **zurückkorrigiert 25.08.2026: ≥10 Minuten → Fetch fällig** (…) — robuster als reine Modulo-Prüfung …" durchgestrichen; neu fett: „**seit 28.08.2026 (Kerzenraster-Fix): Fällig ist der Fetch AUSSCHLIESSLICH bei echter Minute % 10 == 0 … Das Delta entscheidet NICHT mehr über die Fälligkeit, sondern nur noch über die Verpasst-Erkennung**". Die drei Formatbeispiele stehen wörtlich weiter; davor der Hinweis, dass die Kerzenraster-Fassung in [[feedback_vollcheck_format]] (Raster-Slot HH:M0) maßgeblich ist.
- Z439: „(…; ~~NICHT eine Minute-Modulo-Prüfung~~ — seit 28.08.2026 IST die Fälligkeit eine Minute-%10-Prüfung an der echten Systemzeit, siehe Kerzenraster-Fix unten)".
Ergebnis: „alleinige verbindliche" steht ungestrichen nur noch an Z443 (Kerzenraster-Fix).

### W2 — Punkt 12, alte Schwelle 3 Kerzen / RSI 8-10
(a) Vorgefunden wie beschrieben an Z1048 (1a) / Z1052 (Regel 2) / Z1055 („Warum 3 Kerzen") / Z1067 (How to apply). **Zwei Zusatzfunde, im Bericht nicht genannt, beide ohne Marker:** Z1059 Timing-Klarstellung „frühestens BEIM tatsächlichen 3. Kerzenschluss" und Z1073 (12.2 How to apply) „Punkt-12-Trigger (3+ Kerzenschlüsse …)". Beide ebenfalls auf 12.3 gezogen. Datierte Erzählungen (Z1065 Trade #23 „noch nicht nach neuer Fassung geprüft", Z1071 Trade-#27-Vorfall, Z1079 Vorgeschichte 12.3, 12.5 Fall #27) bewusst unverändert — sie beschreiben den damaligen Stand.
(b) Änderungen — jeweils Zahl ersetzt, alte Zahl in Klammer sichtbar:
- Z1048 (1a): „ersten 3 Kerzen" → „ersten 2 Kerzen (seit 31.07.2026, Punkt 12.3 — ursprünglich 3)"; „Ab der 4. … Kerze" → „Ab der 3. … (12.3 — ursprünglich ab der 4.)"; „RSI ≥8-10" → „RSI ≥5 (12.3 — ursprünglich 8-10)"; Schluss „ersten 2 Kerzen (12.3)".
- Z1050 (Why 1a): nur Header-Marker „Zahlen in diesem Absatz = Stand 27.07.2026 VOR der Halbierung durch 12.3 — heute gilt: Override ab der 3. Kerze, RSI ≥5"; Absatz selbst unverändert (Lebensdauer-Begründung).
- Z1052 (Regel 2): „3 aufeinanderfolgende" → „2 aufeinanderfolgende (verschärft 31.07.2026, Punkt 12.3 — ursprünglich 3)"; „mindestens 8-10 Punkte" → „mindestens 5 Punkte (12.3 — ursprünglich 8-10)".
- Z1055: „Warum 3 Kerzen:" → „Warum ursprünglich 3 Kerzen (HISTORISCH — ERSETZT durch 12.3 am 31.07.2026: heute 2 Kerzen / RSI ≥5, Begründung der Halbierung dort):" — Text behalten (erklärt, warum es überhaupt eine Kerzen-Untergrenze gibt).
- Z1059: „3. Kerzenschluss" → „2. Kerzenschluss (der Trigger-Kerze nach 12.3 — ursprünglich der 3.)".
- Z1067: „Nach der 3. Kerze … erkennbarer RSI-Abkühlung" → „Nach der 2. Kerze (12.3 — ursprünglich 3.) … RSI ≥5 Punkte (12.3)"; Ergänzung 27.07.: „bei der 3. Kerze … ab der 4." → „bei der 2. … ab der 3. (weiterhin RSI ≥5; Zahlen seit 12.3, ursprünglich 3./4.)".
- Z1073 (12.2): „(3+ Kerzenschlüsse ohne neues Extrem + RSI-Abstand)" → „(seit 12.3: 2 Kerzenschlüsse … + RSI ≥5 — zur Zeit von Trade #27 noch 3+)".
Ergebnis: Kein Regelsatz in Punkt 12 nennt 3 Kerzen / 8-10 mehr als geltend; 12.3 bleibt die kanonische Stelle.

### W3 — `--cluster-level` / `--grund-cluster-level`
(c) **Code-Stand `scripts/gate_check.cjs` (nur gelesen, nichts geändert):** `MEASUREMENT_REQUIRED` führt `{ key: 'cluster-level', keinGrund: true }` (Z3036) und `{ key: 'sl-anker', keinGrund: true }` (Z3040); fehlt der Wert, wird ein `--grund-cluster-level` ignoriert und der Lauf bricht mit Exit 1 ab, Meldung „OHNE --grund-Ausweg … --cluster-level = register|<p>|<tief-hoch>|none" (Z3222). Kopfkommentar Z427-430 und Z1825-1829 sagen dasselbe („seit 11.09.2026 (Auflage 3) live NICHT mehr erreichbar"). `--sl-vorpruefung` (Z2554) und `--vorschau` (Z2686) verlangen `--cluster-level` ebenfalls ohne Default. Batch: `lenient` (Z1338) = fehlendes `--cluster-level` = PASS, `--batch-strikt` = UNKNOWN. **Fazit: Z672/Z698 waren richtig, Z688 beschrieb den Stand vom 03.09.** Kanonische Werteliste laut Code: `register|<p>|<tief-hoch>|none`.
(a)/(b) Vorgefunden wie beschrieben. Z688 neu: Werteliste `<register|Preis|tief-hoch|none>`, „seit 11.09.2026 OHNE `--grund-`-Ausweg", „Fehlt der Parameter im Live-Aufruf → **Hard-Exit 1**", alte Fassung durchgestrichen mit Vermerk „live nicht mehr erreichbar; `--grund-cluster-level` wird NICHT mehr akzeptiert; nur der Batch-Modus behält die alte Semantik", Verweis auf Schritt 0 als kanonische Stelle. Wertelisten vereinheitlicht: Z192 (Cron 6f) `<register|Preis|none>` → `<register|Preis|tief-hoch|none>`; Z681 (Schritt-5-Template) `<Preis|none>` → `<register|Preis|tief-hoch|none>`; Z695 (`Gate-Check ausgeführt:`) `X|none` → `register|X|tief-hoch|none`. Z672/Z698 unverändert.

### W4 — Cron-Item (8) ohne Schritt 0
(a) Vorgefunden wie beschrieben (Z247: „Voller 5-Schritte-Ablauf aus Punkt 7b1 (ATR ablesen → …)"); Schritt 0 stand nur in (6f) für Voll-Checks mit beobachtetem Setup, nicht in der Order-Sequenz.
(b) Z247 neu: „Voller 7b1-Ablauf, Schritt 0 + Schritte 1-5 (Schritt 0 = SL-Anker-Vorprüfung ZUERST, siehe Item (6f) — bindend seit 07.09.2026, Rechtsfolge E3c: URTEIL UNTAUGLICH → kein 7b1-Ablauf, keine Order; erst bei TAUGLICH: ATR ablesen → …)". Z671 Überschrift: „der Ablauf aus fünf Schritten, seit 07.09.2026 mit vorgeschaltetem Schritt 0, also 0 → 1 → 2 → 3 → 3a → 4 → 4b → 5" — der Suchstring „Ablauf aus fünf Schritten" bleibt absichtlich erhalten, weil (6f) ihn wörtlich referenziert (Berichtsvorschlag „Überschrift ohne Zahl" deshalb nicht übernommen).

### W5 — Punkt 11 „2-3 von 4" (NICHT gelöst, nur sichtbar gemacht)
(a) Vorgefunden wie beschrieben: Z999 und Z1003 „2-3", Z1005 Belegvorlage „→ 2/4, Exit gerechtfertigt", Z1009 „technisch erfülltes 2/4-Kriterium … NICHT ausreichend", Z1018 „2/4 + schwaches Volumen → kein Dreh"; dazu Z996 die explizit definierte Ausnahme „2 der verbleibenden 3" bei geschlossenem QQQ-Gate. Uneindeutigkeit real.
(b) Keine Zahl festgelegt. Nach Z999 ein ⚠️-Markierungsabsatz „UNEINDEUTIG (W5 …) — Levi-Entscheidung ausstehend, KEINE Regeländerung durch Fable" eingefügt; Z1003 mit Kurzverweis „(⚠️ W5: bis zur Levi-Entscheidung Fail-Safe 3 von 4 für den Dreh, siehe Markierung oben)".
(d) **Gewählte Fail-Safe-Richtung: 3 von 4, nur für den Dreh-Vorschlag auf die Gegenrichtung.** Begründung: Punkt 11 ist laut eigener Vorrang-Klärung (Z987) ausschließlich für „aktiv auf die Gegenrichtung drehen" zuständig — ein Dreh eröffnet eine neue Position mit frischem Risiko, die höhere Schwelle ist dort die sichere Seite. Der Positionsschutz wird nicht gelockert, weil Exit/Teilgewinn/SL-Nachzug über 4a/9d1 (ein Signal reicht), 7c fünfte Erweiterung und Punkt 12 unabhängig weiterlaufen. Ein 2/4-Befund wird weiter per Belegpflicht ausgewiesen, aber als „Beobachtung, kein Dreh (Fail-Safe W5)". Die „2 von 3"-Ausnahme bei geschlossenem QQQ-Gate ist ausdrücklich unberührt und in Levis Entscheidung mit aufgenommen. Belegvorlage Z1005 wörtlich unverändert (Formatvorlage) — sie zeigt jetzt bewusst einen Fall, den der Fail-Safe anders bewerten würde; die Markierung sagt das.

### Offen für den Opus-Gegencheck (nichts davon von Fable geändert)
1. **W3, Code-Politur:** `args['grund-cluster-level']` wird in Z3354 weiter eingelesen und als `clusterLevelGrund` an `evaluateTrade` gereicht — live toter Pfad (A3-Abbruch kommt vorher), im Batch-strikt-Modus noch in der UNKNOWN-Detailzeile (Z1956) sichtbar. Die `--vorschau`-Fehlermeldung Z2688 nennt `register|<p>|none` ohne `tief-hoch`, obwohl `parseClusterZones` das Band akzeptiert. Kein Widerspruch, kein Blocker.
2. **W4, außerhalb des Auftrags:** Item (8) fasst den SL weiter als „SL=MAX(Struktur,Floor)" zusammen — nach 7b1 Schritt 3a (11.09.) ist der SL aber Anker ∓ 0,5×ATR, nie der Anker selbst. Die Kurzformel im Cron-Block (dem Text, den Sonnet tatsächlich liest) kann das #42/#43-Muster nahelegen; Vorschlag: „SL=MAX(Anker ∓ 0,5×ATR, Floor; 3a)". Levi-/Opus-Entscheidung.
3. **W1, Zitierpräzision:** Z113 und Z962 verweisen auf „Punkt 9 Kerzenraster-Fix", der Absatz steht aber im X-Tweets-Abschnitt von Punkt 2b. Kein Regelwiderspruch; die neuen W1-Texte referenzieren den Abschnitt beim Namen.
4. **W5:** Levi-Entscheidung 2 oder 3 von 4 (inkl. QQQ-geschlossen-Ausnahme „2 von 3") — Frage 5 in Abschnitt 7; bis dahin Fail-Safe 3/4 für den Dreh.

---

## Opus-Gegencheck W1-W5, 14.09.2026

**Auftrag:** Unabhängig prüfen, ob Fables W1-W5-Korrekturen vollständig, korrekt und ohne neue Fehler sind. Autor ≠ Prüfer ([[feedback_modellwahl_trading]]). Kein Commit, kein Push.

**Methode:** `feedback_live_trading.md` vollständig gelesen (1.204/1.204 Zeilen in 8 Abschnitten, nicht nur die geänderten Stellen); jede der 14 von Fable genannten Fundstellen im Ist-Text aufgesucht; anschließend Volltext-Scans über alle betroffenen Begriffe/Zahlen; `scripts/gate_check.cjs` selbst gelesen UND per sandboxed CLI-Lauf getestet (Kopie unter `%TEMP%/.../gcsandbox`, damit `gate_check_log.jsonl` des Repos unberührt bleibt — verifiziert über `git status --porcelain scripts`); Tests vor und nach den eigenen Edits.

### Gesamturteil: **FREIGEGEBEN MIT AUFLAGEN**

Kein Blocker. W1-W4 sind sachlich korrekt umgesetzt, W3 ist gegen den Code belegt (nicht nur behauptet), W5 ist regelkonform als offen markiert statt heimlich entschieden. Zwei Markup-Inkonsistenzen habe ich selbst nachgezogen (unten F1/F2, reine Durchstreichungen, kein Regelinhalt). Fünf Nachbesserungen bleiben für Levi.

### 1. Fundstellen-Verifikation (14 Stellen, alle selbst gelesen)

| W | Stelle (Ist-Zeile) | Fables Zitat trifft zu? | Im Kontext verständlich? |
|---|---|---|---|
| W1 | Z442 (How to apply 2b) | ja | **teilweise — siehe F1** |
| W1 | Z952 (Anti-Drift (b)) | ja | ja, saubere Kaskade: alles Alte gestrichen, die geltende Fassung fett |
| W1 | Z969 (Why %10-Streichung 9a) | ja | ja, Überschrift + Vorab-Klammer + Durchstreichung greifen ineinander |
| W2 | Z1053 (1a) | ja | ja — alte Zahl in Klammer, neue fett |
| W2 | Z1055 (Why 1a, Header-Marker) | ja | ja |
| W2 | Z1057 (Regel 2) | ja | ja |
| W2 | Z1060 ("Warum ursprünglich 3 Kerzen") | ja | ja |
| W2 | Z1064 (Timing-Klarstellung) | ja | ja |
| W2 | Z1072 / Z1078 (How to apply / 12.2) | ja | ja |
| W3 | Z691 (Befüllungspflicht) | ja | ja — der Hard-Exit-1-Satz steht VOR der gestrichenen Altfassung, das ist die richtige Reihenfolge |
| W3 | Z192 / Z684 / Z698 (Wertelisten) | ja | ja, alle vier jetzt `register|Preis|tief-hoch|none` |
| W3 | Z675 / Z701 (unverändert) | ja | ja |
| W4 | Z247 (Cron-Item 8) | ja | ja — der Verweis auf Item (6f) **existiert** (Z190), Rechtsfolge E3c korrekt zitiert |
| W4 | Z674 (Überschrift 7b1) | ja | ja — Suchstring "Ablauf aus fünf Schritten" erhalten, Item (6f) in Z199 referenziert ihn wörtlich, der Verweis hält |
| W5 | Z1004 (Warn-Marker) / Z1008 (Kurzverweis) | ja | ja |

Fables Zeilen-Buchhaltung ist nachgerechnet korrekt (1.199 + 3 aus W4 + 2 aus W5 = 1.204; alle Alt-/Neu-Zeilennummern gehen auf). Dateigröße vor meinen Edits 284.579 B wie angegeben. **Wiki-Links: 45 Ziele, gegen HEAD `efcac0e` diff-identisch** — selbst nachgemessen, nicht übernommen.

### 2. Vollständigkeits-Scan (eigene Suche über die GESAMTE Datei)

- **W1** (`alleinige verbindliche`, `Fälligkeit`, Delta-Erwähnungen): **eine von Fable übersehene Stelle — Z444** ("Why Modulo-Ersatz, 25.08.2026"). Dort stand der Satz *"...wird hiermit als alleinige verbindliche Fälligkeitsprüfung festgeschrieben"* **ungestrichen**. Fables Bilanzsatz *"'alleinige verbindliche' steht jetzt ungestrichen nur noch beim Kerzenraster-Fix"* war damit **faktisch falsch**. Entschärfend: Die Absatzüberschrift trug seit jeher den Marker "durch den Kerzenraster-Fix unten überholt" (deshalb hatte auch der Ursprungsbericht sie nicht als W1-Fundstelle geführt). **Selbst gefixt, F2.**
- **W2** (`3 aufeinanderfolgende`, `3 Kerzen`, `8-10`, `3. Kerzenschluss`, `3+ Kerzenschlüsse`, `4. Kerze`): **keine weitere Stelle.** Die verbliebenen Treffer sind alle korrekt: Z874 und Z1106 nennen "3/8-10" ausdrücklich als *alte* Fassung, Z1055/Z1084/Z1124-1126 sind datierte Vorfallserzählungen, Z820 gehört zur 7b1c-Stale-Klasse (6/3 Kerzen, anderer Gegenstand). **Kein Regelsatz in Punkt 12 nennt die alte Schwelle mehr als geltend.** Fables zwei Zusatzfunde (Timing-Klarstellung, 12.2) waren echte Funde, die der Ursprungsbericht nicht hatte.
- **W3** (alle `--cluster-level`/`--grund-cluster-level`): 14 Fundstellen, alle konsistent. Die einzige verbliebene `--grund-cluster-level`-Erwähnung ist die durchgestrichene Altfassung in Z691 — korrekt so. Das `description`-Frontmatter (Z3) nennt die Regel ebenfalls korrekt ("live Pflicht OHNE --grund-Ausweg").
- **W4** (alle Cron-Items, die 7b1 beschreiben): Item (8) ist das einzige. Items (6f), (7), (9) beschreiben andere Schritte, keine Doppelung.

**Ergebnis: keine siebte oder achte Fundstelle derselben Klasse.** Der einzige Nachtrag ist Z444 (W1).

### 3. W3 — eigene Code-Verifikation (Lesen + Ausführen)

Gelesen in `scripts/gate_check.cjs`:
- `MEASUREMENT_REQUIRED` führt `{ key: 'cluster-level', keinGrund: true }` und `{ key: 'sl-anker', keinGrund: true }`.
- Die Auswerteschleife lautet sinngemäß: `if (grundKey && !f.keinGrund) grundNotes.push(...) else { missingMeasurements.push(...); if (f.keinGrund) ohneAusweg.push(...) }` → bei `keinGrund: true` landet ein vorhandenes `--grund-*` **nicht** im Ausweg-Zweig, sondern im Fehlzweig. **Exit 1, der Grund wird ignoriert.**
- Die Fehlermeldung nennt die kanonische Liste `register|<p>|<tief-hoch>|none`.
- Batch: `lenient = isBatch && opts.strikt !== true` → fehlendes `--cluster-level` bleibt dort PASS; `--batch-strikt` läuft wie live.

**Live-Gegenprobe (Sandbox-Kopie, echte CLI-Läufe):**
- Vollständiger Aufruf **ohne** `--cluster-level`, **mit** `--grund-cluster-level "keine Zone messbar"` → `Exit-Code: 1`, Ausgabe: *"Fehlende Pflicht-Messfelder ... --cluster-level"* plus *"OHNE --grund-Ausweg ... ein --grund-<feld> wird NICHT akzeptiert"*.
- Derselbe Aufruf ganz ohne beides → identisches Verhalten.

**Fables Code-Lesart ist unabhängig bestätigt.** Die Werteliste ist korrekt, aber nicht erschöpfend: `parseClusterZones()` akzeptiert zusätzlich `keine` als Synonym für `none` und **Mehrfachzonen** (durch `/`, `;` oder Leerzeichen getrennt). Das ist kein Fehler der Regeldatei (die Liste ist eine gültige Teilmenge), aber die Mehrfachzonen-Fähigkeit steht nirgends im Regelwerk — siehe Punkt 7, Fables offener Punkt (1).

### 4. W5 — fachliche Bewertung der Fail-Safe-Richtung

**Die Mechanik stimmt.** Auslösemengen für den Dreh-Vorschlag: Schwelle 2 → {2/4, 3/4, 4/4}; Schwelle 3 → {3/4, 4/4}. Letztere ist echte Teilmenge, also löst "3 von 4" bei einem 2/4-Befund **nachweislich nicht** aus, wo "2 von 4" auslösen würde. Seltener, nicht häufiger — Fables Aussage ist korrekt.

**Die Deckung im Text ist überwiegend gegeben, aber nicht lückenlos:**
- Z990 (Vorrang-Klärung) trägt die Aussage wörtlich: *"Punkt 4a/9d1 bleiben das primäre, schnellere Signal für reine Gewinnsicherung/Exit (eine erkannte Umkehrkerze + Muster reicht dort allein). Punkt 11 hier ist eine zusätzliche, striktere Schwelle NUR für die Entscheidung, aktiv auf die Gegenrichtung zu drehen."*
- Z992 verweist für die Zwischenlage ausdrücklich auf 7c fünfte Erweiterung; Z1097 (12.3 Scope-Klarstellung) bestätigt die Arbeitsteilung ein zweites Mal.
- **Aber Z1010 (Belegpflicht) widerspricht Z990:** Die Überschrift lautet *"Belegpflicht für jeden Punkt-11-Exit"*, der Text spricht von *"Punkt-11-Exit/Dreh-Vorschlag"* und schließt mit *"Ein Exit ohne diese Auflistung ... ist stattdessen über 9d1/4a ... oder Punkt 12 zu begründen, nicht über Punkt 11"* — das setzt voraus, dass ein Exit sehr wohl über Punkt 11 begründbar ist. Auch Z994 fasst Punkt 11 als "schließen UND drehen". **Punkt 11 hat damit zwei Lesarten im selben Dokument (nur Dreh / Exit + Dreh) — ein eigenständiger, von Fable und vom Ursprungsbericht nicht erfasster Befund (unten N1).** Unter der engen Lesart ist der Fail-Safe unbedenklich; unter der weiten verzögert er im schmalen Band "2/4 erfüllt, aber kein 4a-/9d1-Muster, keine 5 Gegenbewegungs-Ticks, kein Stall" auch den *schützenden* Exit.

**Fazit W5:** Richtung korrekt, Begründung im Kern gedeckt, aber der Satz "Positionsschutz bleibt ungelockert" ruht auf genau der Lesart, die Z1010 bestreitet. Für Levis Entscheidung gehört N1 mit auf den Tisch.

### 5. Neue Widersprüche durch die Korrekturen?

**Keine harten.** Geprüft: alle neu eingefügten Verweise ("Item (6f)", "Schritt 0 oben", "Punkt 12.3", "siehe Markierung oben", "Ablauf aus fünf Schritten") zeigen auf existierende, unveränderte Anker. Keine Zahl doppelt oder neu widersprüchlich. Zwei **weiche** Neu-Effekte des W5-Fail-Safes, siehe N2/N3.

### 6. Eigene Fixes (F1/F2) — reine Markup-Angleichung, kein Regelinhalt

**F1 · Z442 (W1, unvollständig korrigiert):** Fable hat in Z442 nur *"NICHT eine Minute-Modulo-Prüfung"* gestrichen, den Auslöser selbst — *"bei **≥10 Min Delta** Tweet-Fetch einbauen"* — aber **fett und ungestrichen** stehen lassen. Genau diese Formulierung hat Fable in Punkt 9 (Z952) durchgestrichen. Damit stand dieselbe aufgehobene Fälligkeitsregel an einer Stelle gestrichen, an der anderen aktiv. Nachgezogen: Auslöser durchgestrichen, geltende Fassung fett in derselben Klammer ("Fällig ... AUSSCHLIESSLICH bei echter Minute % 10 == 0 ... Delta nur noch Verpasst-Erkennung"). Der Anlass-Trigger (±15 Min Kalender-Release) bleibt unverändert gültig, er steht ohnehin eigenständig in Z456.

**F2 · Z444 (W1, übersehene Fundstelle):** *"...und wird hiermit als alleinige verbindliche Fälligkeitsprüfung festgeschrieben"* durchgestrichen und mit derselben Klammer markiert wie in Z969. **Ergebnis jetzt tatsächlich: keine einzige ungestrichene "alleinige verbindliche Fälligkeitsprüfung" mehr in der Datei** (3/3 Vorkommen markiert, maschinell nachgezählt).

Datei danach **1.204 Zeilen / 285.003 B** (+424 B). Wiki-Links weiter **45/45 diff-identisch** gegen `efcac0e`. Keine Punkt-Nummer, keine Pflichtzeile, keine Formatvorlage, kein Parameter angefasst.

### 7. Offene Nachbesserungen für Levi (nichts davon selbst geändert)

**N1 · Punkt-11-Scope: "nur Dreh" oder "Exit + Dreh"? — NEU GEFUNDEN, Klasse wie W1-W5, Schwere MITTEL.** Z990/Z1097 sagen "ausschließlich Dreh", Z1010/Z994 behandeln Punkt 11 als Exit-Grundlage. Der Widerspruch ist älter als die W-Korrekturen, aber er entscheidet mit, ob Fables W5-Fail-Safe wirklich nur den Dreh verschärft. **Nicht selbst korrigiert — das verlangt eine inhaltliche Regel-Entscheidung, keine Textangleichung.** Gehört zusammen mit W5 beantwortet.

**N2 · Der W5-Fail-Safe und die Belegvorlage Z1010 kollidieren sichtbar.** Die Vorlage endet wörtlich mit *"→ 2/4, Exit gerechtfertigt"*, der Marker in Z1004 sagt, ein 2/4-Befund sei "Beobachtung, kein Dreh ... statt Handlungsaufforderung". Wer im Loop Z1010 liest, sieht den Fail-Safe nicht. Fable hat das bewusst so gelassen (Formatvorlage bleibt wörtlich) und im Marker angesprochen — **ein Halbsatz an Z1010 ("der Fail-Safe W5 betrifft nur den Dreh; für den reinen Exit bleibt diese Vorlage gültig") würde die Stelle schließen.** Erst nach Levis W5-Entscheidung sinnvoll.

**N3 · Der Volumen-Dämpfer ist nur für "2/4" formuliert.** Z1014/Z1023 lauten *"selbst ein technisch erfülltes 2/4-Kriterium ... NICHT ausreichend"* bzw. *"2/4 + schwaches Volumen → kein Dreh"*. Unter dem Fail-Safe 3/4 ist 2/4 ohnehin kein Dreh — die Dämpferregel läuft für ihren einzigen genannten Fall leer und sagt nichts über "3/4 + schwaches Volumen". Mit Levis W5-Entscheidung mitziehen.

**N4 · "2-3 von 4" steht an fünf weiteren Stellen ohne W5-Hinweis:** Z854, Z856, Z862 (7c), Z1074 (12 Deckelung), Z1097 (12.3 Scope). Besonders Z1074 und Z1097 sind loop-relevant. Solange W5 offen ist, wäre je ein Warn-Verweis konsequent — Fable hat nur die zwei Stellen in Punkt 11 selbst markiert.

**N5 · Cron-Item (8) sagt weiter "SL=MAX(Struktur,Floor)" — Fables eigener offener Punkt (2), von mir eigenständig geprüft: KEIN Widerspruch, aber eine echte Unvollständigkeit am gefährlichsten Ort.** "Anker ≠ SL" (3a) und "SL = MAX(technisches Level, Floor)" (Schritt 3) passen zusammen: der Anker ist nicht der SL, sondern geht als `Anker ∓ 0,5×ATR` in das MAX ein. Item (8) verkürzt das auf "Struktur" — im **einzigen** Text, den Sonnet im Trigger-Moment garantiert liest, und dessen Unvollständigkeit die Datei viermal (16.07./27.07./26.08./09.09.) als strukturellen Ausfallgrund dokumentiert. Am 10.09. lag der SL 40× exakt auf dem Anker. **Vorschlag wörtlich:** `SL = MAX(Anker ∓ 0,5×ATR, sichere Cluster-Kante ∓ 0,5×ATR, 8c-Floor)` (7b1 Schritt 3/3a). **Bewusst nicht selbst umgesetzt:** Das ist eine Ergänzung im geschützten Cron-Codeblock (N1 des Ursprungsberichts) und liegt außerhalb des W1-W5-Mandats — Levi-Freigabe.

**Zu Fables übrigen offenen Punkten:**
- (1) `clusterLevelGrund` wird weiter eingelesen: **unkritisch/OK.** Live toter Pfad, der A3-Abbruch kommt vorher — im Sandbox-Lauf bestätigt. Im Batch-strikt-Modus ist er die UNKNOWN-Detailzeile, dort gewollt. Die `--vorschau`-Hilfe nennt `register|<p>|none` ohne `tief-hoch`, obwohl `parseClusterZones()` das Band akzeptiert: **Info, Code-Kosmetik, kein Verhaltensunterschied.** Ergänzend: dieselbe Funktion akzeptiert auch Mehrfachzonen und `keine` — beides steht in keiner Regelstelle.
- (3) "Punkt 9 Kerzenraster-Fix" in Z93 (Cron-Block) und Z965 (9a), obwohl der Absatz in 2b steht: **Info, kein toter Verweis.** Punkt 9 enthält mit Z952 selbst einen Kerzenraster-Absatz, der korrekt weiterleitet. Z93 liegt im geschützten Cron-Block, deshalb nicht angefasst.
- (4) W5-Entscheidung offen: **erwartet, kein Fehler** — mit N1/N2/N3 zusammen vorlegen.

### 8. Regelwerk-Kohärenz nach den Korrekturen (Qualitätsfrage, kein Blocker)

Die Datei ist an den 16 Stellen **eindeutiger, aber sperriger** geworden. Sauber gelöst sind W2 (neue Zahl vorn, alte klein in Klammer — liest sich flüssig) und W4 (eine eingeschobene Klausel, kein Durchstreichen). Schwer lesbar bleiben die beiden Tweet-Fälligkeits-Absätze Z442 und Z952: Z952 trägt nach meiner Zählung sieben Durchstreichungspaare in einem Satz, in dem drei aufgehobene Schwellen (60 Min / 10 Min / Delta) hintereinander abgeräumt werden, bevor die geltende Regel kommt. **Inhaltlich ist das jetzt richtig, aber es bestätigt K4/K5 des Ursprungsberichts:** An dieser einen Stelle kostet der Altbestand nicht nur Platz, sondern Lesbarkeit im Trigger-Moment. Wenn Levi überhaupt etwas kürzt, ist K4/K5 der Kandidat mit dem besten Verhältnis — und er fällt jetzt mit W1 zusammen, also ohne zusätzliches Risiko.

### 9. Tests

`node --test tests/trading_scripts.test.js` — **73/73 PASS, 0 fail** (vor meinen Edits und nach F1/F2 erneut gelaufen, identisches Ergebnis). Die einzige Prüfung, die diese Datei liest (beide Kommandotemplates müssen `--ema50-5min` und `[--runway-ratio` tragen), ist von W1-W5 und von F1/F2 nicht berührt. **Kein Commit, kein Push** — der Arbeitsstand im Code-Repo bleibt wie vorgefunden.

### 10. Ist die Datei jetzt verlässlich für den Live-Loop?

**Ja, für W1-W4.** Die vier Stellen, an denen der Loop eine überholte Zahl oder Mechanik lesen konnte, sind geschlossen und — bei W3 — gegen das ausführende Skript belegt. **Mit einer ehrlichen Einschränkung:** W5 ist weiterhin offen (per Auftrag), N1 legt eine zweite, bisher unbemerkte Uneindeutigkeit derselben Regel frei, und N5 lässt im Cron-Block eine SL-Kurzformel stehen, die das #42/#43-Muster nahelegt. Keiner dieser drei Punkte ist durch die W1-W5-Arbeit entstanden — aber N5 ist der Punkt, den ich vor dem nächsten Echtgeld-Ablauf am dringendsten beantwortet sähe.
