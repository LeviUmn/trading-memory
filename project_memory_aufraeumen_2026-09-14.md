---
name: project-memory-aufraeumen-2026-09-14
description: "Bestandsaufnahme und Aufräum-Vorschlag für das Memory-System (Stand 14.09.2026, Opus 5). Kernzahlen: 179 Dateien / 4,62 MB, MEMORY.md 22.372 B mit 110 Indexzeilen (Ø 193, längste 1.416 Zeichen), 5 tote Wiki-Links (alle Schreibfehler, keine Vormerkungen), 24 über keinen Link erreichbare Dateien, memory/-Git-Repo seit 31.08. nicht committet (62 offene Änderungen). Diagnose: nicht MEMORY.md ist der teuerste Posten, sondern die Session-Start-Pflichtlektüre mit ~511 KB. Bericht, KEINE Umsetzung — Levi entscheidet."
metadata:
  node_type: memory
  type: project
  status: "Bericht, Levi-Entscheidung ausstehend"
  originSessionId: session_current
  modified: 2026-09-14T11:39:31.136Z
---

# Memory-System: Bestandsaufnahme, Diagnose, Aufräum-Vorschlag (14.09.2026)

**Auftrag:** Zustand des persistenten Memory-Systems prüfen, nachdem `MEMORY.md` an ein Leselimit herankam. Reine Analyse — nichts gelöscht, nichts umgeschrieben, nichts zusammengefasst. Alle Zahlen unten sind am 14.09.2026 frisch am Dateisystem gemessen, nicht aus dem Gedächtnis ([[feedback_memory_pflege]], Zitierpflicht).

**Wichtigste Korrektur vorweg:** Der Hinweis lautete „~20,8 KB". Gemessen sind es **22.372 Bytes**. Die Datei ist seit dem Hinweis also weiter gewachsen — heute kamen die Zeilen zu Q2/Q4 dazu.

---

## 1. Bestandsaufnahme

### 1.1 Gesamtumfang

| Bereich | Dateien | Bytes | Anteil |
|---|---|---|---|
| Wurzel (`*.md`, inkl. `MEMORY.md`) | 127 | 2.178.182 | 46,0 % |
| `testtag/` | 12 | 2.163.688 | 45,7 % |
| `trades/` | 37 | 384.407 | 8,1 % |
| `dax_beobachtung/` | 2 | 2.973 | 0,1 % |
| `dax_trades/` | 1 | 797 | 0,0 % |
| **Summe** | **179** | **4.730.047** (4,62 MB) | 100 % |

Dazu kommt ein `.git/` von 5,1 MB im Memory-Ordner selbst (siehe 1.7).

### 1.2 MEMORY.md selbst

- **22.372 Bytes**, 115 Zeilen, davon **110 Indexzeilen**
- Indexzeilen zusammen: 21.320 Bytes = **95,3 % der Datei**
- Zeilenlänge: **Ø 193 Zeichen**, längste **1.416 Zeichen** (Zeile 3, Q2/Q4-Gegencheck 14.09.)
- **17 Zeilen > 200 Zeichen**, davon **10 Zeilen > 400 Zeichen**
- Diese 17 langen Zeilen = 10.207 Bytes = **47,9 % des gesamten Index** bei 15,5 % der Zeilen. Ihr Schnitt: 600 Zeichen.

Die Top-10 nach Länge (Zeichen): 1416 · 1188 · 1129 · 931 · 844 · 702 · 609 · 602 · 536 · 452 — alle zehn sind Einträge aus dem Zeitraum 07.–14.09.2026.

**Der aussagekräftigste Schnitt:**

| Zeilentyp | Anzahl | Bytes | Ø/Zeile |
|---|---|---|---|
| Zeigt auf **datierte** Datei (Episode: Testtag, Gegencheck, Analyse) | 33 | 11.552 | ~350 |
| Zeigt auf **undatierte** Datei (Dauerregel, Referenz) | 77 | 9.878 | ~128 |

**30 % der Zeilen tragen 54 % der Bytes.** Die Dauerregel-Zeilen sind diszipliniert kurz; die Episoden-Zeilen sind es nicht.

22 Indexzeilen tragen bereits einen Abschluss-Marker im Titel (ABGESCHLOSSEN / FREIGEGEBEN / GELÖST / ENTSCHIEDEN / UMGESETZT / verworfen / kein GO) und belegen zusammen **9.875 Bytes**.

### 1.3 Typen und Namensmuster

| `type` im Frontmatter | Dateien |
|---|---|
| `project` | 128 |
| `feedback` | 47 |
| `reference` | 2 |
| `user` | 1 |

`type: project` dominiert deutlich — auch alle `trades/trading_*.md` (37) und `testtag/testtag_*.md` (12) tragen `type: project`, obwohl sie inhaltlich Protokolle sind, keine Projekte. Der Typ trennt also praktisch nichts mehr.

Nach Dateinamen-Präfix: 73 `project_*`, 49 `feedback_*`, 37 `trading_*`, 12 `testtag_*`, 2 `trade_*`, 2 `reference_*`, 1 `user_identity`, 1 `uebersicht`, 1 `notiz_*`.

**Frontmatter-Disziplin:** 178 von 179 Dateien haben Frontmatter (nur `MEMORY.md` nicht — korrekt so). 48 Dateien haben **kein** `modified`-Feld, fast alle davon aus Juni/Juli 2026 plus die kleinen `feedback_*`-Einzelregeln. Das Feld wurde erst später eingeführt und nie nachgezogen — kein Schaden, aber eine Altersbestimmung per Frontmatter ist für diese 48 nicht möglich.

`modified`-Verteilung der übrigen 131: 18× Juli, 58× August, 55× September.

### 1.4 Wachstumstempo

Neue Dateien nach Datum im Dateinamen:

| Monat | neue Dateien |
|---|---|
| 2026-06 | 5 |
| 2026-07 | 25 |
| 2026-08 | 32 |
| 2026-09 (bis zum 14.) | **40** |

In 14 Tagen September sind mehr Dateien entstanden als im gesamten August. Das ist der eigentliche Treiber, nicht die Länge einzelner Dateien.

### 1.5 Verlinkung

- **149** verschiedene Wiki-Link-Ziele werden im gesamten Memory referenziert.
- **98** Dateien sind direkt aus `MEMORY.md` verlinkt; **80** nicht.
- Von diesen 80 sind **56** immerhin über einen Wiki-Link aus einer anderen Datei erreichbar.
- **24 Dateien sind über gar keinen Link erreichbar** — weder Index noch Wiki-Link:

  `feedback_bloomberg_x.md`, `feedback_trading_tempo.md`, `project_spacex_ipo.md`,
  `project_studie_volumenprofil_fable_2026-08-31.md`, `project_studie_volumenprofil_opus_2026-08-31.md`,
  `testtag/testtag_2026-08-31.md`, `testtag/testtag_2026-09-04.md`,
  sowie 17 `trades/trading_*.md` (12.06., 22.06., 23.06., 25.06., 30.06., 01.07., 02.07., 06.07., 07.07., 10.07., 18.07., 27.07., 28.07., 30.07., 05.08., 06.08., 18.08.).

  Bei den `trades/`-Dateien ist das unkritisch: die Sammelzeile „Ältere Trade-Sessions" am Ende von `MEMORY.md` nennt sie im Fließtext, und die harten Zahlen stehen ohnehin in `scripts/trades.db`. Kritisch ist es bei **`testtag/testtag_2026-08-31.md` (164.962 B)** und **`testtag/testtag_2026-09-04.md` (249.352 B)** — zwei Rohprotokolle von zusammen 405 KB, auf die nichts zeigt, obwohl die zugehörigen Analysen (`project_testtag_analyse_2026-08-25.md` bzw. `_2026-09-04.md`) existieren. Und bei den beiden Volumenprofil-Studien von Fable und Opus (61 KB), von denen nur die Sonnet-Variante im Index steht.

**Tote Links: nur 5 verschiedene Ziele, 7 Vorkommen** — deutlich weniger als vermutet, und **keines davon ist eine bewusste Vormerkung**. Alle fünf sind Schreibfehler gegen die eigene Namenskonvention:

| Totes Linkziel | Vorkommen | Gemeint ist offensichtlich |
|---|---|---|
| `trade-2026-06-23` | 2 | `trades/trading_2026-06-23` |
| `trade-2026-06-22` | 2 | `trades/trading_2026-06-22` |
| `trade-2026-07-01` | 1 | `trades/trading_2026-07-01` |
| `trade_log` | 1 | `trades/trade_log` (Ordner-Präfix fehlt) |
| `feedback-instrumenten-fokus` | 1 | `feedback_instrumenten_fokus` (Bindestrich statt Unterstrich) |

Das ist ein gutes Zeugnis: die Verlinkung ist inhaltlich intakt, es sind Tippfehler aus der Frühzeit, kein Berg unerledigter Vormerkungen.

### 1.6 Das description-Feld wächst nach demselben Muster wie der Index

Summe aller `description`-Zeilen: **65.708 Bytes** über 179 Dateien, Ø 367 Zeichen. Die längsten:

| Bytes | Datei |
|---|---|
| 2.668 | `project_gegencheck_q2q4_fable_umsetzung_2026-09-14.md` |
| 2.336 | `feedback_live_trading.md` |
| 1.593 | `project_testtag_analyse_2026-09-11.md` |
| 1.451 | `feedback_chartanalyse.md` |
| 1.100 | `project_opus_vollpruefung_2026-08-24.md` |

Die `description` von `feedback_live_trading.md` ist ein Lehrstück: sie beginnt mit einem sauberen Satz und hängt danach in einer einzigen Zeile die Änderungen vom 31.08., 31.08. (zusätzlich), 31.08. (Drittfix), 11.09. und den 11.09.-Gegencheck aneinander. Das ist **exakt dasselbe Muster wie in `MEMORY.md`**: angehängt statt ersetzt.

### 1.7 Die Git-Sicherung des Memory-Ordners steht seit zwei Wochen

Der Memory-Ordner ist ein **eigenes Git-Repo** (50 Commits, 5,1 MB `.git`, nicht Teil von `tradingview-mcp` — `git ls-files memory` im Projekt-Repo liefert „did not match any files").

- Letzter Commit: **31.08.2026, 23:50** — „Testtag-Plan 01.+02.09.2026"
- Offene Änderungen heute: **62** (46 neue Dateien, 16 geänderte)

Der Tagesabschluss ([[feedback_tagesabschluss]]) führt „Git-Backup" als Pflichtpunkt. Für das Code-Repo läuft das; für das Memory-Repo ist es seit **zwei Wochen** ausgefallen — genau über den Zeitraum der intensivsten Testtag- und Gegencheck-Arbeit. Das gesamte September-Memory (40 Dateien) hat aktuell **keine Versionssicherung**. Das ist der akuteste Befund dieses Berichts und hat mit Größe gar nichts zu tun.

### 1.8 Die tatsächlichen Kontext-Kostenträger

`MEMORY.md` mit 22 KB ist **nicht** der teuerste Posten. Die Pflichtlektüre beim Session-Start nach [[feedback_memory_pflege]] ist:

| Datei | Bytes |
|---|---|
| `MEMORY.md` | 22.372 |
| `feedback_live_trading.md` | 280.057 |
| `feedback_chartanalyse.md` | 158.090 |
| `feedback_chart_layout.md` | 33.456 |
| letzte 3 `trades/`-Dateien | 28.881 |
| **Summe** | **522.856** (≈ 511 KB, grob 130–150k Tokens) |

`MEMORY.md` ist davon **4,3 %**. Wer nur den Index kürzt, hat das Problem nicht angefasst.

### 1.9 Die zehn größten Dateien

| Bytes | Datei |
|---|---|
| 340.684 | `testtag/testtag_2026-09-02.md` |
| 313.731 | `testtag/testtag_2026-09-10.md` |
| 280.057 | `feedback_live_trading.md` |
| 275.199 | `testtag/testtag_2026-09-01.md` |
| 249.352 | `testtag/testtag_2026-09-04.md` |
| 221.570 | `testtag/testtag_2026-08-28.md` |
| 220.792 | `testtag/testtag_2026-09-03.md` |
| 164.962 | `testtag/testtag_2026-08-31.md` |
| 158.090 | `feedback_chartanalyse.md` |
| 149.261 | `testtag/testtag_2026-09-08.md` |

29 der 127 Wurzel-Dateien sind dagegen kleiner als 3 KB — die kleinen `feedback_*`-Einzelregeln. Die sind kein Problem; sie sind das Format, das funktioniert.

### 1.10 Thematische Cluster (Redundanz-Kandidaten)

| Cluster | Dateien | Bytes |
|---|---|---|
| `project_testtag_analyse_*` (24.08.–11.09.) | 11 | 460.288 |
| `project_gegencheck_*` (31.08.–14.09.) | 10 | 121.759 |
| — davon allein die Kette vom **09.09.** | 6 | 49.437 |
| Audits/Vollprüfungen (`vollpruefung`, `vollaudit`, `finalcheck`, `meilensteincheck`, `regelwerk_audit`, `fable_vollpruefung`) | 6 | 160.845 |
| Volumenprofil-Studie (Sonnet/Fable/Opus) | 3 | 72.346 |
| Geschwindigkeit/Effektivität-Studie (Fable/Opus + Todo-Liste + Gegencheck) | 4 | 22.111 |
| Entscheidungs-/Vorlagen-Dateien | 7 | 105.131 |

Die **09.09.-Kette** ist der Extremfall: sechs Dateien beschreiben einen einzigen Tag —
`project_gegencheck_fable_umsetzung_2026-09-09.md` (Runde 1),
`..._runde2.md`, `..._runde3.md`, `..._runde4.md`,
`..._todos_nach_testtag.md`, `..._f1_b1_b2.md`.
Fünf davon sind aus `MEMORY.md` gar nicht direkt verlinkt, nur über Wiki-Links untereinander. Der Index kennt dafür zwei Zeilen (609 + 536 Zeichen), die die Kette in Prosa nacherzählen.

### 1.11 Inhaltliche Stichprobe (5 große Dateien, nicht nur Größe)

- **`project_opus_vollpruefung_2026-08-24.md`** (74.658 B, 361 Zeilen): 30 Überschriften, davon **sechs** mit „Nachtrag 24.08.2026" — Nachmittag, Abend, Nacht, Nacht spät, Nacht spät (2). Ein einziger Tag, sechsfach fortgeschrieben. Enthält einen Abschnitt „Entscheidungen für Levi — NICHT umgesetzt, nur vorbereitet". Ob diese Entscheidungen inzwischen gefallen sind, steht nicht in der Datei. → **Kürzungskandidat**, aber erst nach Klärung dieses Punktes.
- **`project_testtag_analyse_2026-08-28.md`** (75.731 B, 605 Zeilen, 25 Abschnitte): sehr detaillierte Nachrechnungen („die Trigger-Ebene war in 25 von 58 Voll-Checks unbefüllt", „der Zähler steht 58 von 58 Mal da und hat nur ein Drittel gemessen"). Die *Schlussfolgerungen* sind längst ins Regelwerk gewandert; die *Rechenwege* stehen nur hier. → Kürzbar, aber siehe Zielkonflikt in Abschnitt 5.
- **`project_opus_vollaudit_2026-09-03.md`** (42.298 B, 297 Zeilen): vier Prüfebenen, Befunde K-1 bis K-7, Freigabeurteil. Der Freigabe-Teil ist dauerhaft relevant, die Ebene-1-Verifikation („Wurde umgesetzt, was behauptet wird?") ist nach dem Commit historisch. → Klarer Kürzungskandidat.
- **`project_studie_bessere_trades_2026-08-24.md`** (42.345 B): Studie v1.0 plus drei Nachträge zu Phase 0/1 plus vier neue Indikatoren. Der Q-Score aus dieser Studie läuft **produktiv** in `gate_check.cjs`. → **Volltext behalten**, das ist die Begründungsquelle für scharf geschaltete Logik.
- **`testtag/testtag_2026-09-02.md`** (340.684 B, 3.764 Zeilen, 207 Erwähnungen von „Voll-Check"): 84 Voll-Checks im Wortlaut plus vollständiges Session-Update. Reines Rohprotokoll. → Nicht kürzen, aber auch nicht im aktiven Index führen (siehe E3).

---

## 2. Diagnose

Die vier Vermutungen aus dem Auftrag, geprüft:

**(a) MEMORY.md-Zeilen werden angehängt statt ersetzt — BESTÄTIGT, klarster Einzelbefund.**
Beweis: Episoden-Zeilen 350 Byte im Schnitt gegen 128 Byte bei Dauerregel-Zeilen; 17 Zeilen tragen 48 % des Index; die längste (1.416 Zeichen) beschreibt einen Vorgang von *heute*. Die Zeilen lesen sich als Verlaufsprotokoll („Runde 1: … Runde 2: … Offen: …"), nicht als Index-Eintrag. Ein Index soll sagen, *wo* etwas steht; diese Zeilen sagen, *was* drinsteht — und machen damit das Nachlesen der Zieldatei überflüssig, was genau der Grund ist, warum sie weiterwachsen. Dasselbe Muster greift auf das `description`-Feld über (1.6).

**(b) Abgeschlossene Ketten bleiben als Einzeldateien bestehen — BESTÄTIGT, aber weniger schlimm als vermutet.**
Die 09.09.-Kette (6 Dateien / 49 KB) und die Entscheidungsvorlagen sind echte Fälle. Der Umfang ist aber überschaubar: alle 10 `project_gegencheck_*` zusammen sind 122 KB = 2,6 % des Memory. Die Konsolidierung lohnt für die **Übersichtlichkeit**, kaum für die Größe.

**(c) Testtag-Analysen bleiben unbegrenzt im Volltext — BESTÄTIGT und der zweitgrößte Posten.**
11 Analysen = 460 KB (9,7 %), dazu 12 Rohprotokolle = 2,16 MB (45,7 %). Zusammen **55 % des gesamten Memory sind Testtag-Material** aus 19 Kalendertagen.

**(d) Überholte Alt-Stände werden nicht archiviert — TEILWEISE WIDERLEGT.**
`project_risikomanagement.md` ist sauber fortgeschrieben, die 50k-Annahme wurde korrigiert und die Korrektur ist im Index vermerkt. Es gibt aber Fälle: 17 unerreichbare `trades/`-Dateien, `trading_session_nasdaq.md` (seit 16.07. in [[feedback_memory_pflege]] selbst als „nur noch Archiv" markiert, steht weiter unsortiert im Wurzelverzeichnis), zwei unerreichbare Testtagsprotokolle von zusammen 405 KB.

**Zwei Befunde, die im Auftrag nicht vermutet waren:**

**(e) Die Git-Sicherung des Memory-Repos steht seit 31.08. — 62 offene Änderungen, 40 September-Dateien ohne Backup.** Das ist ein Datenverlust-Risiko, kein Größenproblem, und gehört meiner Ansicht nach vor alles andere erledigt.

**(f) Der wahre Kontext-Kostentreiber ist die Session-Start-Pflichtlektüre mit 511 KB, nicht der 22-KB-Index.** `feedback_live_trading.md` hat 1.199 Zeilen, 31 Abschnitte, **123 Zeilen mit „ergänzt"/„Nachtrag"/„korrigiert"/„aktualisiert"** und 11 Einzelzeilen über 2.000 Zeichen (längste: 5.013). Die Datei ist nicht falsch — sie ist ein vollständig protokolliertes Regelwerk. Aber sie wird bei **jedem** Session-Start komplett gelesen, während der weit überwiegende Teil Herleitung ist und nur ein kleiner Teil die aktuell geltende Regel.

**Zusammengefasst:** Das Memory-System ist inhaltlich in einem besseren Zustand, als die Größe vermuten lässt — Verlinkung intakt, Frontmatter fast vollständig, keine Halde verwaister Vormerkungen. Das Problem ist ein **Formproblem** (Verlaufs-Erzählung an Stellen, die Nachschlage-Stellen sein sollten: Index, `description`, Regelwerks-Kopf) und ein **Risikoproblem** (Git).

---

## 3. Empfehlungen — priorisiert, mit Aufwand/Nutzen

### E1 — Memory-Repo committen (SOFORT)
**Aufwand:** 5 Minuten. **Nutzen:** sehr hoch. **Risiko:** keins.

62 offene Änderungen sichern, danach den Git-Backup-Schritt in [[feedback_tagesabschluss]] explizit auf **beide** Repos ausweiten (Code *und* Memory), weil die aktuelle Formulierung offenbar nur das Code-Repo trifft. Das ist die einzige Empfehlung, die ich unabhängig von allem anderen als dringend bezeichne.

### E2 — Index-Zeilenkonvention einführen (HOCH)
**Aufwand:** ~45 Min einmalig für 17 Zeilen. **Nutzen:** hoch. **Risiko:** gering.

Vorschlag als Regel: **Eine Indexzeile ist maximal ~200 Zeichen und enthält Titel + Status + ein bis zwei Kennzahlen + Verweis. Sie erzählt nicht, was in der Datei steht.** Jede weitere Zahl gehört in die Zieldatei.

Gemessener Effekt:
- Kappung auf 200 Zeichen: Index 21.320 → 14.513 B, `MEMORY.md` **22,4 KB → ~15,6 KB (−30 %)**
- Kappung auf 150 Zeichen: Index → 13.490 B, `MEMORY.md` **→ ~14,5 KB (−35 %)**

Zweite Hälfte der Regel, die das Nachwachsen verhindert: **Bei einer Fortsetzung desselben Vorgangs wird die bestehende Zeile ersetzt, nie erweitert.** Der Verlauf („Runde 1 … Runde 2 …") gehört in die Zieldatei; im Index steht nur der aktuelle Stand. Genau dieses Prinzip fehlt heute und erklärt Befund (a).

*Als gutes Beispiel habe ich die Indexzeile für diesen Bericht bewusst kurz gehalten.*

### E3 — Abgeschlossenes in einen Archiv-Abschnitt am Dateiende (HOCH)
**Aufwand:** ~30 Min. **Nutzen:** hoch. **Risiko:** gering.

Ich empfehle **`## Archiv` als Abschnitt am Ende von `MEMORY.md`, NICHT eine separate `MEMORY_ARCHIV.md`.** Begründung: Eine separate, nicht automatisch geladene Datei wird nie gelesen, weil niemand weiß, dass sie existiert — und Auffindbarkeit ist der einzige Zweck des Index. Ein Abschnitt am Ende bleibt auffindbar und kostet bei Einzeilern kaum etwas.

Ins Archiv gehören die **22 Zeilen mit Abschluss-Marker** (heute 9.875 B), dort auf **eine Zeile ≤ 120 Zeichen** eingedampft: Titel, Datum, Ergebnis in drei Worten, Link. Effekt zusammen mit E2: **`MEMORY.md` ≈ 11–12 KB, also etwa die Hälfte von heute.**

Kriterium für „abgeschlossen", damit das entscheidbar bleibt: Der Vorgang hat eine Levi-Entscheidung bekommen **und** die Umsetzung ist committet **und** kein Gegencheck steht aus. Alles mit „offen", „ausstehend", „ENTWURF", „ungepusht" bleibt oben.

**Nicht ins Archiv** gehören trotz Abschluss-Marker die Zeilen, die auf täglich gültige Regeln zeigen — [[project_risikomanagement]], [[feedback_chartanalyse]]-Regeln, [[feedback_dax_trennung]]. „Entschieden" heißt dort „gilt", nicht „vorbei".

### E4 — Verwaiste Dateien wieder anhängen, tote Links reparieren (MITTEL)
**Aufwand:** 20 Min. **Nutzen:** mittel. **Risiko:** keins.

Die 24 unerreichbaren Dateien brauchen keinen Index-Eintrag, aber die beiden großen Testtagsprotokolle (`testtag/testtag_2026-08-31.md`, `testtag/testtag_2026-09-04.md`) sollten aus ihren jeweiligen Analyse-Dateien per Wiki-Link erreichbar sein, wie es bei 09.09. und 10.09. bereits vorbildlich gemacht ist. Gleiches für die zwei Volumenprofil-Studien von Fable und Opus, die im Index fehlen, obwohl die Sonnet-Variante drinsteht.

Dazu die **5 toten Links reparieren** (reine Tippfehler, siehe 1.5) — zwei Minuten, und der Tote-Link-Zähler steht danach sauber auf 0. Erst dann ist er als Indikator für echte, bewusst gesetzte Vormerkungen brauchbar.

### E5 — Konsolidierungskandidaten (MITTEL, Levi-Entscheidung nötig)
**Aufwand:** je Cluster 30–60 Min. **Nutzen:** mittel (Übersicht, kaum Größe).

**Kandidat 1 — die 09.09.-Kette (6 Dateien, 49.437 B).** `project_gegencheck_fable_umsetzung_2026-09-09_f1_b1_b2.md` ist bereits die Gesamtkette-Datei und im Index als „09.09.2026 Gesamtkette (FREIGEGEBEN)" geführt. Sie könnte die Kurzfassung der vier Runden aufnehmen; die vier Runden-Dateien bleiben als Detailbelege liegen, verschwinden aber aus dem Index.
**Link-Folgen, falls doch etwas entfernt würde:** `_runde4` verlinkt `_runde3` → `_runde2` → `_runde1`; die Gesamtkette-Datei verlinkt `_todos_nach_testtag` und `project_testtag_analyse_2026-09-09`; die Index-Zeile „vollcheck.cjs/position_tick.cjs Bau + Gegencheck-Runden 1-4" zeigt auf `_runde4`. Solange nichts gelöscht wird, bricht kein einziger Link.

**Kandidat 2 — die Entscheidungsvorlagen.** `project_rr_decke_entscheidungsvorlage_2026-09-10.md` (ENTSCHIEDEN 10.09., Option A) und `project_q2_kalibrierung_entscheidungsvorlage_2026-09-11.md` (ENTSCHIEDEN 14.09., Option A + Q4-a) haben ihren Zweck erfüllt. Die *getroffene* Entscheidung steht ohnehin in der Regel- bzw. Skriptdatei; die Vorlage dokumentiert nur die verworfenen Alternativen. Beide gehören ins Archiv (E3), nicht in den aktiven Index.
**Link-Folgen:** `project_opus_meilensteincheck_2026-09-10.md` und `project_testtag_analyse_2026-09-11.md` verlinken beide per Wiki-Link — beim reinen Index-Umzug bricht nichts.

**Kandidat 3 — die drei Volumenprofil-Studien (72.346 B).** Drei Modelle, ein Ergebnis („VP bedingtes Ja, Meta-Indikatoren nein, Renko/Kagi/P&F nein"). Der Konvergenz- und Dissens-Teil ist wertvoll, die dreifache Herleitung nicht. Konsolidierbar zu einer Datei mit drei kurzen Positionen — aber nur, wenn du das Thema als erledigt ansiehst.

### E6 — Kürzen vs. Volltext-Pflicht (MITTEL)
**Aufwand:** je Datei 20–40 Min. **Nutzen:** mittel.

**Kürzbar (Rechenweg raus, Ergebnis + Einordnung bleibt):**
- `project_testtag_analyse_2026-08-24/-25/-27/-28.md` (zusammen 238 KB) — alle vier sind älter als zwei Wochen, ihre TODOs sind nachweislich umgesetzt und committet.
- `project_opus_vollpruefung_2026-08-24.md` (74,7 KB) — die sechs „Nachtrag"-Ebenen eines einzigen Tages auf einen Ergebnisabschnitt eindampfen. **Vorher klären:** der Abschnitt „Entscheidungen für Levi — NICHT umgesetzt, nur vorbereitet". Sind die inzwischen gefallen? Wenn nein, ist das ein offener Punkt, der eher nach oben gehört als gekürzt.
- `project_opus_vollaudit_2026-09-03.md` (42,3 KB) — Ebene 1 (Umsetzungs-Verifikation) ist nach dem Commit historisch.

**Volltext behalten, nicht anfassen:**
- [[feedback_live_trading]], [[feedback_chartanalyse]], [[feedback_vollcheck_format]], [[feedback_session_update]], [[feedback_tagesabschluss]] — werden aktiv befolgt.
- [[project_risikomanagement]], [[project_vision]], [[user_identity]], [[feedback_coaching_ehrlichkeit]], [[feedback_regeldisziplin]], [[feedback_memory_pflege]], [[feedback_regime_wechsel]].
- `project_studie_bessere_trades_2026-08-24.md` — Begründungsquelle für den produktiv laufenden Q-Score in `gate_check.cjs`.
- **Alle `testtag/*.md` Rohprotokolle.** Sie sind groß (2,16 MB = 46 % des Memory), aber sie sind die *Primärquelle* für jede nachträgliche Auszählung. Wer sie kürzt, macht künftige Gegenchecks unmöglich. Sie werden ohnehin nicht automatisch geladen — sie kosten nur Plattenplatz, und der ist kein Problem.

### E7 — Der größte Hebel, den ich trotzdem NICHT jetzt empfehle (HOCH, aber heikel)
**Aufwand:** hoch. **Nutzen:** sehr hoch. **Risiko:** hoch.

`feedback_live_trading.md` (280 KB) auf zwei Ebenen trennen: ein **„Geltender Stand"**-Kopf mit den nummerierten Regeln in ihrer heutigen Fassung, und ein **„Herleitung / Änderungshistorie"**-Teil darunter. Beim Session-Start würde nur der Kopf gelesen. Grobe Schätzung: Kopf ~40–60 KB, also **rund −75 % Session-Start-Kontext** bei voller Auditierbarkeit im selben Dokument.

**Warum ich trotzdem abrate, solange du nicht ausdrücklich zustimmst:** Genau diese Datei ist die operative Grundlage jedes Live-Loops. Eine Umstrukturierung von 1.199 Zeilen mit 123 Änderungsmarkern hat ein realistisches Risiko, unterwegs eine geltende Klausel zu verlieren — und das würde erst im Live-Betrieb auffallen. Das ist der Fall, für den [[feedback_dont_change_running_system]] geschrieben wurde. Wenn überhaupt, dann als eigener Vorgang mit Autor-≠-Prüfer-Gegencheck ([[feedback_modellwahl_trading]]), nicht als Nebenprodukt eines Aufräumens.

---

## 4. Wartungsroutine-Vorschlag

Im Stil von [[feedback_tagesabschluss]] und [[feedback_session_update]] formuliert, damit es sich ins bestehende Routinen-System einfügt.

### 4.1 Nach jedem abgeschlossenen Vorgang (Gegencheck / Analyse / Entscheidung)

**Why:** Ohne diesen Schritt wächst der Index bei jeder Fortsetzung um eine weitere Verlaufsschicht, statt den Stand zu ersetzen — nachgewiesen an 17 Zeilen, die 48 % des Index tragen.

**How to apply — drei Handgriffe, zusammen unter 5 Minuten:**
1. **Index-Zeile ersetzen, nicht erweitern.** Der neue Stand ersetzt den alten Text derselben Zeile. Maximal ~200 Zeichen. Verlauf gehört in die Zieldatei.
2. **Status-Wort in den Titel.** Eines von: `OFFEN` / `ENTWURF` / `UMGESETZT, Gegencheck offen` / `ABGESCHLOSSEN`. Nur `ABGESCHLOSSEN` qualifiziert später fürs Archiv.
3. **Memory-Repo committen.** Gleiche Pflicht wie beim Code-Repo.

### 4.2 Monatlich (erster Handelstag des Monats, vor dem Session-Update)

**Why:** Im September sind in 14 Tagen 40 Dateien entstanden. Bei diesem Tempo ist ein Quartalsrhythmus zu langsam — ein Quartal wären nach heutigem Tempo grob 250 neue Dateien.

**How to apply — sechs Punkte:**
1. Alle Indexzeilen mit `ABGESCHLOSSEN` in den `## Archiv`-Abschnitt am Ende von `MEMORY.md` verschieben, dabei auf ≤ 120 Zeichen eindampfen.
2. Index auf Zeilen > 200 Zeichen prüfen und kürzen.
3. Tote Wiki-Links zählen. Erwartung nach E4: 0 durch Tippfehler. Was übrig bleibt, ist eine echte Vormerkung → auf die Liste offener Punkte.
4. Unerreichbare Dateien prüfen (weder Index noch Wiki-Link) → entweder anhängen oder bewusst als Archiv markieren.
5. Analyse- und Gegencheck-Dateien, die älter als **6 Wochen** sind und deren TODOs alle committet sind, auf Ergebnis + Einordnung kürzen — **Rohprotokolle in `testtag/` ausgenommen**.
6. `git log -1` im Memory-Ordner: Liegt der letzte Commit mehr als 3 Tage zurück, ist Punkt 4.1.3 ausgefallen.

### 4.3 Sollwerte zur Selbstkontrolle
- `MEMORY.md` ≤ **15 KB**. Wird der Wert überschritten, ist das kein Anlass zu löschen, sondern der Auslöser für einen Archiv-Durchlauf nach 4.2.
- Längste Indexzeile ≤ 200 Zeichen.
- `description`-Feld ≤ 300 Zeichen — dieselbe Ersetzen-statt-Anhängen-Regel wie beim Index.

---

## 5. Der Zielkonflikt, offen benannt

Ein erheblicher Teil des Wachstums ist **kein Fehler, sondern der Preis für deine eigenen Vorgaben** — und ich halte diese Vorgaben für richtig:

- **Zitierpflicht** ([[feedback_memory_pflege]]): Jede Zahl braucht einen frischen Tool-Call. Das erzeugt zwangsläufig lange, zahlengesättigte Texte. Die Alternative wäre, aus dem Gedächtnis zu schreiben — genau der Fehler, der bei Trade #21 aufflog.
- **Ungefilterte Ehrlichkeit** ([[feedback_coaching_ehrlichkeit]]): Eine beschönigte Kurzfassung wäre kürzer und wertlos.
- **Autor ≠ Prüfer** ([[feedback_modellwahl_trading]]): Fable setzt um, Opus prüft gegen. Dieses Verfahren *produziert* mehrere Dateien pro Vorgang — das ist seine Funktionsweise, nicht sein Nebenprodukt. Die 09.09.-Kette mit ihren sechs Dateien ist entstanden, weil vier Runden nötig waren, um eine Endlosschleife zu finden. Ohne die Runden wäre der Defekt im Live-Loop gelandet.

**Wo ich die Grenze ziehen würde:** Vollständigkeit ist in der **Zieldatei** Pflicht. Im **Index** ist sie ein Fehler — dort kostet sie bei *jeder* Session Kontext, ohne einen Gegencheck zu ermöglichen. Dieselbe Logik für das `description`-Feld. Das ist die eigentliche Empfehlung dieses Berichts: nicht weniger dokumentieren, sondern an der richtigen Stelle dokumentieren.

**Was durch Kürzung verloren ginge — ehrlich:**
- Werden Analysen nach E6 gekürzt, sind die *Rechenwege* nicht mehr zitierfähig, nur noch die Ergebnisse und deren Einordnung.
- Die Originaldaten bleiben aber auffindbar in: `testtag/*.md` (Rohprotokolle, ungekürzt), `scripts/gate_check_log.jsonl` (846 KB), `scripts/trades.db`, `scripts/skipped_setups_fiktiv.jsonl`, `scripts/oneh_shadow_log.jsonl`, `scripts/trigger_kandidaten_log.jsonl`, `scripts/register_touch_log.jsonl`, `scripts/vollcheck_state.json` sowie im Git-Verlauf beider Repos.
- **Nicht mehr rekonstruierbar wäre eine Zahl nur dann, wenn sie ausschließlich in einer gekürzten Analyse stand und nie in ein Log, eine DB oder eine Regeldatei gewandert ist.** Deshalb: Vor jeder Kürzung einmal prüfen, ob die tragenden Zahlen anderswo belegt sind. Wenn nicht, bleibt die Passage stehen. Im Zweifel Volltext — 4,6 MB Plattenplatz sind kein Problem, ein nicht mehr belegbarer Befund schon.

---

## 6. Offene Entscheidungen für Levi

Nichts davon ist umgesetzt — analog zur Tempo-Bremse bei Regeländerungen ([[feedback_live_trading]] Punkt 14) entscheidest du, was davon läuft.

1. **E1 Git-Commit des Memory-Repos** — 62 offene Änderungen seit 31.08. Meine Empfehlung: sofort, unabhängig vom Rest.
2. **E2 Index-Zeilenkonvention** (≤ 200 Zeichen, ersetzen statt anhängen) — willst du das als Regel, und wenn ja: 200 oder 150 Zeichen?
3. **E3 Archiv-Abschnitt** — `## Archiv` am Ende von `MEMORY.md` (meine Empfehlung) oder doch eine separate `MEMORY_ARCHIV.md`? Und: Ist mein Abschluss-Kriterium (Entscheidung gefallen + committet + kein offener Gegencheck) richtig?
4. **E5 Konsolidierungen** — 09.09.-Kette, Entscheidungsvorlagen, Volumenprofil-Studien: Welche davon gelten dir als erledigt?
5. **E6 Kürzungen** — August-Analysen und `project_opus_vollpruefung_2026-08-24.md` kürzen? Vorher zu klären: Sind die dort als „NICHT umgesetzt, nur vorbereitet" geführten Levi-Entscheidungen inzwischen gefallen?
6. **E7 Zweiteilung von `feedback_live_trading.md`** — der mit Abstand größte Kontext-Hebel (511 KB → geschätzt ~130 KB Session-Start), aber auch das einzige echte Risiko in diesem Bericht. Meine Empfehlung: **nicht jetzt**, und wenn, dann als eigener Vorgang mit Gegencheck.
7. **Verankerung** — soll die Wartungsroutine aus Abschnitt 4 als fester Abschnitt in [[feedback_memory_pflege]] aufgenommen werden, analog zum Tagesabschluss?

**Geschätzter Gesamteffekt bei E2 + E3:** `MEMORY.md` von **22,4 KB auf ~11–12 KB**, also etwa halbiert, ohne dass eine einzige Datei verschwindet oder ein Wiki-Link bricht. Das Gesamt-Memory bliebe praktisch unverändert bei ~4,6 MB — das ist auch in Ordnung, denn es wird nicht automatisch geladen.
