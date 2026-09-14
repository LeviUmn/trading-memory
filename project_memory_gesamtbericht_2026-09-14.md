---
name: project-memory-gesamtbericht-2026-09-14
description: "Zusammenführung der beiden Aufräum-Berichte vom 14.09.2026 zu einer priorisierten Entscheidungsvorlage: was gefahrlos aufgeräumt werden kann und was nicht. Vier Risikokategorien, 7 freigebbare Umsetzungsschritte (3 davon mit Gegencheck-Pflicht), realistische Gesamtzahl Session-Ladelast 511 KB → ~470 KB. Korrigiert die E7-Erwartung (−75 % ist nicht erreichbar). Bericht, KEINE Umsetzung."
metadata:
  node_type: memory
  type: project
  status: "Gesamtbericht, Levi-Entscheidung ausstehend"
  originSessionId: session_current
  modified: 2026-09-14T11:58:52.292Z
---

# Memory-Aufräumen: Gesamtbild und Entscheidungsvorlage (14.09.2026, Opus 5)

**Auftrag (Levi, wörtlich):** *„Wenn wir den haben, bitte ich dann Opus noch einmal ein Gesamtbild / Gesamtbericht zu erstellen, damit wir wissen, was wir wirklich aufräumen können, ohne dass wir wichtige Daten verlieren, die uns Performance im Trading kosten."*

Dieser Bericht führt [[project_memory_aufraeumen_2026-09-14]] (ganzer Memory-Ordner) und [[project_pruefung_feedback_live_trading_2026-09-14]] (die eine große Regeldatei) zusammen. **Details werden nicht wiederholt — hier steht nur, was sich aus beiden zusammen ergibt.** Nichts geändert, kein Commit, kein Push.

---

## 1. Zusammengeführte Bestandsaufnahme

| Kennzahl | Wert | Quelle |
|---|---|---|
| Memory gesamt | 179 Dateien / 4,62 MB | Bericht 1, 1.1 |
| davon Testtag-Material (Rohprotokolle + Analysen) | 2,62 MB = **55 %** | Bericht 1, Diagnose (c) |
| **Session-Start-Pflichtlektüre** | **522.856 B ≈ 511 KB** | Bericht 1, 1.8 |
| davon `feedback_live_trading.md` | 280.057 B = **54 %** | Bericht 2, 1.1 |
| davon `feedback_chartanalyse.md` | 158.090 B = 30 % | Bericht 1, 1.8 |
| davon `MEMORY.md` | 22.766 B (heute gemessen) = 4,4 % | frisch |
| Anteil geltende Regel in `feedback_live_trading.md` | **85 %** (defensiv klassifiziert) | Bericht 2, 1.2 |
| Tote Wiki-Links | 5 Ziele / 7 Vorkommen, alle Tippfehler | Bericht 1, 1.5 |
| Git-Sicherung Memory-Repo | **erledigt** — `efcac0e`, 63 Dateien, 22.352 Zeilen | frisch verifiziert |

**Die eine Zahl, die alles ordnet:** `MEMORY.md` ist 4,4 % der Ladelast. Wer nur den Index kürzt, hat das Problem nicht angefasst — aber die große Regeldatei lässt sich nur zu 12–15 % kürzen, ohne geltende Regeln umzuschreiben. **Daraus folgt: es gibt keinen großen Hebel. Es gibt viele kleine, und eine dringende Korrektur, die mit Platz gar nichts zu tun hat.**

---

## 2. Priorisierte Gesamtliste — nach Risiko/Nutzen, nicht nach Berichtsreihenfolge

### Kategorie 0 — SOFORT, kein Aufräumen, sondern Fehlerkorrektur

**Das ist kein Platzsparen. Hier steht im geltenden Regelwerk an mehreren Stellen eine überholte Zahl ohne Überholungsmarker — der Loop kann die falsche Fassung lesen.** Diese Kategorie gehört vor alles andere, auch vor jede Kürzungsentscheidung, und sie spart null Bytes.

| # | Befund | Schwere | Trading-Risiko | Aufwand |
|---|---|---|---|---|
| **W2** | Punkt 12 Stall-Exit: „3 Kerzen / RSI 8–10" steht an **vier** Stellen (Z1046/1048/1052/1067), 12.3 hat auf **2 Kerzen / RSI ≥5** verschärft — kein einziger Verweis an den alten Stellen. Wer im Loop bei „How to apply" aufhört, wendet die falsche Schwelle an. | HOCH | **Stall-Exit feuert eine Kerze zu spät** | ~10 Min |
| **W1** | Tweet-Fetch-Fälligkeit: drei Mechaniken, vier Fundstellen. Z966 erklärt die Delta-Prüfung „als alleinige verbindliche" — ohne Marker, obwohl am 28.08. umgekehrt. Z949(b) fett + ungestrichen ebenso. | HOCH | verpasste News-Fetches (27.08.: 5 Stück) | ~10 Min |
| **W3** | `--cluster-level`: `--grund-`-Ausweg an einer Stelle verboten (Z672/698, Auflage 11.09.), an anderer erlaubt (Z688). Genau dieser Ausweg hat am 10.09. live ein falsches PASS erzeugt. Dazu drei verschiedene Wertelisten. | MITTEL | falsches Gate-PASS | ~10 Min |
| **W4** | Der Cron-Prompt-Block (Item 8) beschreibt 7b1 **ohne Schritt 0** (SL-Anker-Vorprüfung) — obwohl der seit 07.09. bindend ist und *zuerst* läuft. Was nicht im Cron-Block steht, fällt im Loop strukturell aus (viermal dokumentiert). | MITTEL | Vorprüfung wird übersprungen | ~5 Min |

> Opus' eigene Formulierung dazu: *„Wenn du nur eine Sache aus diesem Bericht umsetzt, dann W1 und W2."* Ich schließe mich an und ergänze W3/W4 — beide sind ebenfalls reine Angleichung, kein Byte Inhalt geht verloren.

**Einschränkung, ehrlich:** W3 wurde gegen den *Text* geprüft, nicht gegen `scripts/gate_check.cjs`. Vor der Angleichung muss einmal im Skript nachgesehen werden, welche Fassung dort tatsächlich implementiert ist (Bericht 2, Abschnitt 8).

---

### Kategorie 1 — Aufräumen mit vernachlässigbarem Risiko

Hier kann keine live gebrauchte Information verloren gehen, weil entweder nur Metadaten betroffen sind oder der Text wörtlich in eine Historie-Datei umzieht.

| Maßnahme | Herkunft | Ersparnis | Risiko |
|---|---|---|---|
| `description`-Feld von `feedback_live_trading.md` (Änderungsprotokoll im Metadatenfeld) ersetzen | K1 | −2,0 KB | keins — enthält keine Regel, die nicht im Body steht (stichprobenweise geprüft) |
| 5 tote Wiki-Links reparieren (reine Tippfehler) | E4 | 0 KB, Zähler auf 0 | keins |
| 2 verwaiste Testtag-Rohprotokolle + 2 Volumenprofil-Studien wieder anhängen | E4 | 0 KB, +Auffindbarkeit | keins |
| **Index-Zeilenkonvention** ≤200 Zeichen, **ersetzen statt anhängen** | E2 | `MEMORY.md` −30 % | gering |
| **`## Archiv`-Abschnitt** am Ende von `MEMORY.md`, 22 Abschluss-Zeilen auf ≤120 Zeichen | E3 | zusammen mit E2: 22,8 → **~11–12 KB** | gering |
| Kerzenraster-Block bereinigen (fällt mit W1 zusammen) | K4/K5/K6/K7 | −8,2 KB | keins, wenn ausgelagert |
| Einleitungsabsatz vor dem Cron-Codeblock auf einen Satz | K8 | −0,9 KB | keins — der Codeblock selbst bleibt zu 100 % unangetastet |

**Formprinzip für alles ab hier: auslagern, nicht löschen.** Jede gekürzte Passage wandert wörtlich in eine neue `feedback_live_trading_historie.md`, an der Fundstelle bleibt ein Verweis. Kein Byte Herleitung geht verloren, jeder künftige Gegencheck kann weiter zitieren, und ein Fehlurteil ist reversibel (Bericht 2, 1.6).

---

### Kategorie 2 — Aufräumen mit Ermessen (Levi entscheidet, Gegencheck danach Pflicht)

Alles hier ist Herleitungstext in der aktiven Regeldatei. Das Risiko ist nicht „die Information ist weg" (sie zieht um), sondern **„beim Kürzen wird versehentlich ein halber Satz geltende Regel mitgenommen"**. Genau dafür gilt Autor ≠ Prüfer.

| Gruppe | Inhalt | Ersparnis | Auflagen |
|---|---|---|---|
| **A** (Rest) | CronCreate-Vorgeschichte, ScheduleWakeup-Messreihe | −2,1 KB | Betriebsparameter „10–25 s Anlauf, kein Drift" als Halbsatz erhalten |
| **B** | 7b1 Entry-Freigabe: nur Klammer- und Why-Text, nie Parameterliste/Rechtsfolge/Pflichtzeile | −7,4 KB | **K13 muss [[trades/trading_2026-08-17]] + [[trades/trading_2026-08-19]] in die Kurzfassung übernehmen** |
| **C** | 7b1a Q-Score: AVWAP-Statusnotiz, Q3-Tie-Breaker | −3,15 KB | offener Punkt „Persistenz über `tv_launch`-Neustart ungetestet" muss mit |
| **D** | Abschnitte 9/11/12/13/14/15: Vorfallsbeschreibungen | −10,1 KB | **K24 muss [[trades/trading_2026-08-21]] tragen**; Formatvorlagen (K19, K17-Schlusssatz) wörtlich behalten |
| E5 | 09.09.-Kette, Entscheidungsvorlagen ins Archiv (Index-Umzug, keine Löschung) | ~0 KB, +Übersicht | kein Link bricht |
| E6 | August-Analysen + `project_opus_vollpruefung_2026-08-24.md` kürzen | bis −100 KB Plattenplatz | **Vorfrage offen:** der Abschnitt „Entscheidungen für Levi — NICHT umgesetzt, nur vorbereitet" muss vorher geklärt sein |

**Diese drei Links sind die einzige Verankerung ihrer Zieldateien** — fallen sie weg, werden aus 17 unerreichbaren `trades/`-Dateien 20. Das ist die konkreteste Verlustgefahr im ganzen Vorhaben und zugleich die am leichtesten zu vermeidende.

---

### Kategorie 3 — Echter Vorbehalt (nur auf ausdrückliche Freigabe)

Der Prüfer hat hier selbst Unsicherheit signalisiert: *„Ich schlage sie vor, empfehle sie aber nicht aktiv."*

| # | Warum unsicher |
|---|---|
| K26 (7d0-Rückfallgeschichte, −2,05 KB) | das #T2-Gegenbeispiel ist der Grund, warum das Kerzenschluss-Format funktioniert — muss zwingend bleiben |
| K27 (7c fünfte Erweiterung, −1,35 KB) | Regel steht unter **offener Review-Pflicht** — solange die läuft, ist der Ursprungsfall der einzige Datenpunkt |
| K28 (Paket-6a-Vermerke, −1,9 KB) | die Vermerke sagen *warum die Zahl so ist*; Bedingung 4 trägt eine Wartungsanweisung für die noch ausstehende Kalibrierung |
| K29 (Punkt-9-Historie, −0,75 KB) | enthält die Designphilosophie des Abschnitts |
| K30 (Offenlegungspflicht-Whys, −0,8 KB) | die Verbote selbst müssen wörtlich bleiben |
| 7b1b-Schattenmessung (~4 KB) | **unklar, ob die Messung noch läuft** — defensiv auf „läuft" entschieden |

**Meine Empfehlung: Kategorie 3 zurückstellen.** 8,9 KB von 511 KB = 1,7 % der Ladelast. Der Ertrag rechtfertigt es nicht, gegen eine ausdrückliche Unsicherheit des Prüfers zu entscheiden.

---

### Kategorie 4 — NICHT anfassen (Trading-Risiko > Nutzen)

| Was | Warum |
|---|---|
| **E7 — Zweiteilung von `feedback_live_trading.md`** | Regel und Begründung stehen **im selben Satz**, nicht in getrennten Absätzen. Trennen = Neuformulieren geltender Gate-Regeln. Dazu 129 interne Querverweise („siehe Punkt 7b1 Schritt 4b"), maschinell nicht prüfbar. Beide Berichte raten ab. **Vorschlag: als ABGELEHNT markieren, nicht als „später"** — sonst taucht es in jedem künftigen Durchgang wieder als „größter Hebel" auf. |
| **N1 — Cron-Codeblock Z119–406 (~14 KB)** | Viermal dokumentiert: Was nicht wörtlich in diesem Block steht, fällt im Loop strukturell aus. **Kein Zeichen.** |
| **N8 — alle Pflichtzeilen-Formatvorlagen** | 14 Vorlagen, jede wird im Output gegengeprüft. **Kein Zeichen.** |
| N2–N7, N9–N15 | Schritt 0 / SL-Anker-Definition / Q2+Q4 (seit heute verbindlich) / 7b1c-Schwellen (unkalibriert, warten auf 10–15 Fälle) / AVWAP-Ausführungsschritt / offene Kollision zweiter Ordnung / 12.5-Verdikt |
| **Alle `testtag/*.md` Rohprotokolle (2,16 MB = 46 %)** | Primärquelle jeder nachträglichen Auszählung. Wer sie kürzt, macht künftige Gegenchecks unmöglich. Sie werden **nicht automatisch geladen** — sie kosten nur Plattenplatz, und der ist kein Problem. |
| `project_studie_bessere_trades_2026-08-24.md` | Begründungsquelle für den produktiv laufenden Q-Score in `gate_check.cjs` |
| **W5 (Punkt-11-Schwelle „2–3 von 4")** | Das Festlegen wäre eine **Regeländerung**, keine Bereinigung → fällt unter Punkt 14 Tempo-Bremse. Nur als offene Frage notieren. |
| `feedback_chartanalyse.md` (158 KB) | **Noch nie einzeln geprüft.** Nicht blind kürzen — eigener Durchgang analog zu Bericht 2, oder gar nicht. |

---

## 3. Realistische Gesamtzahl

**Die E7-Erwartung ist korrigiert.** Bericht 1 schätzte −75 % Session-Kontext. Bericht 2 hat gemessen: **mit reinem Kürzen sind −12 bis −15 % erreichbar**, mit perfekter Umstrukturierung −25 bis −35 % — und Umstrukturierung ist der Vorgang, von dem beide Berichte abraten. Die 42 KB sind *„kein Versäumnis, sondern der normale Bodensatz eines Dokuments, das 14 Wochen lang jeden Tag fortgeschrieben wurde."*

| Posten | heute | nach Kat. 0+1+2 | nach zusätzlich Kat. 3 |
|---|---|---|---|
| `MEMORY.md` | 22,8 KB | **~11,5 KB** | ~11,5 KB |
| `feedback_live_trading.md` | 280,1 KB | **~247 KB** | ~238 KB |
| `feedback_chartanalyse.md` | 158,1 KB | 158,1 KB (ungeprüft) | 158,1 KB |
| `feedback_chart_layout.md` + 3× `trades/` | 62,3 KB | 62,3 KB | 62,3 KB |
| **Session-Ladelast** | **511 KB** | **~468 KB (−8,4 %)** | **~459 KB (−10,2 %)** |
| Memory gesamt (Platte) | 4,62 MB | ~4,62 MB (Auslagerung = Umzug) | ~4,62 MB |

**Realistische Zielgröße: ~470 KB Session-Start statt 511 KB.** Das ist ehrlich wenig. Wer deutlich darunter will, muss entweder `feedback_chartanalyse.md` einem eigenen Prüfdurchgang unterziehen (dort sind bei gleichem Verhältnis grob −20 KB denkbar, ungeprüft) oder E7 doch machen — und das ist die Entscheidung, von der beide Berichte abraten.

**Der eigentliche Ertrag dieses Vorhabens liegt nicht in KB, sondern in Kategorie 0:** vier Stellen, an denen der Loop heute eine überholte Zahl lesen kann. Das ist der Punkt, an dem Aufräumen tatsächlich Trading-Performance schützt statt sie zu gefährden.

---

## 4. Umsetzungsplan — 7 einzeln freigebbare Schritte

| Schritt | Inhalt | Zuordnung | Aufwand | Gegencheck? |
|---|---|---|---|---|
| **S1** | **Widersprüche bereinigen.** W2 (Zahlen in Z1046/1048/1052/1067 auf 12.3-Werte + Marker), W1 (Z966 streichen, Z949b + Z439 auf Kerzenraster), W3 (Z688 angleichen, 4 Wertelisten vereinheitlichen — **vorher `gate_check.cjs` gegenlesen**), W4 (Cron-Item 8 um Schritt 0 ergänzen) | W1–W4, K6 | ~45 Min | **JA — Pflicht** |
| **S2** | **Metadaten + Verlinkung.** `description` K1 ersetzen; 5 tote Links reparieren; 4 verwaiste Dateien anhängen; Git-Backup-Punkt in [[feedback_tagesabschluss]] auf **beide** Repos ausweiten | K1, E4, E1-Nachzug | ~30 Min | nein |
| **S3** | **`MEMORY.md` neu ordnen.** Index-Konvention ≤200 Zeichen + „ersetzen statt anhängen"; `## Archiv` am Dateiende mit den 22 Abschluss-Zeilen ≤120 Zeichen | E2, E3 | ~75 Min | nein (Selbstcheck: Linkziele unverändert) |
| **S4** | **Historie-Datei anlegen + Gruppe A.** `feedback_live_trading_historie.md` erzeugen, Kerzenraster-Altbestand (K4/K5/K7) und K2/K3/K8 wörtlich umziehen | K2–K5, K7, K8 | ~60 Min | **JA — Pflicht** |
| **S5** | **Gruppen B, C, D.** K9–K25 auslagern. Auflagen: drei `trades/`-Links erhalten, alle Formatvorlagen wörtlich, Punkt-Nummern unverändert | K9–K25 | ~2,5 h | **JA — Pflicht, härtester Schritt** |
| **S6** | **Gruppe E** — nur nach ausdrücklicher Levi-Freigabe, meine Empfehlung: zurückstellen | K26–K30 | ~60 Min | **JA** |
| **S7** | **Verankern + abschließen.** Wartungsroutine (Bericht 1, Abschnitt 4) in [[feedback_memory_pflege]]; E5-Index-Umzüge; E6 **erst nach Klärung der Vorfrage**; E7 als ABGELEHNT vermerken | E5, E6, E7, Routine | ~90 Min | nein |

**Warum S1/S4/S5/S6 einen unabhängigen Gegencheck brauchen:** Sie verändern `feedback_live_trading.md` — die operative Grundlage jedes Live-Loops. Eine fehlerhaft gekürzte Live-Regel ist genauso riskant wie ein fehlerhafter Code-Fix, und sie fällt erst im Live-Betrieb auf. [[feedback_modellwahl_trading]] (Autor ≠ Prüfer) gilt hier unverändert: wer kürzt, prüft nicht selbst nach. Der Gegencheck hat genau eine Frage zu beantworten: **Ist in der gekürzten Fassung eine geltende Regel, Pflichtzeile, Formatvorlage oder Zahl verschwunden?**

**S2/S3/S7 brauchen keinen Gegencheck** — sie fassen Metadaten, Index und Verlinkung an, nie Regeltext. Ein gebrochener Wiki-Link ist maschinell zählbar und in Sekunden repariert.

**Vor S5 zu klären (aus Bericht 2, Abschnitt 7):** Ist 12.5 inzwischen durch einen echten kerzenschluss-basierten Punkt-12-Trigger geschlossen? Läuft die 7b1b-Schattenmessung noch? Beides entscheidet, ob K22 und N6 überhaupt Kandidaten sind.

---

## 5. Was NICHT verloren geht, wenn wir aufräumen

Levis Sorge wörtlich beantwortet: **Kürzen heißt hier „Herleitungstext straffen und an eine andere Stelle legen", nicht „Fakten löschen".** Jede Zahl, die heute belegbar ist, bleibt es.

| Quelle | Inhalt | Status |
|---|---|---|
| **Git-Historie Memory-Repo** | jeder Stand **jeder** Memory-Datei vor jeder Kürzung | **jetzt gesichert** — `efcac0e`, 63 Dateien, 22.352 Zeilen (E1 erledigt) |
| `feedback_live_trading_historie.md` (neu, S4) | **jede ausgelagerte Passage wörtlich**, kein Byte verkürzt | wird nicht automatisch geladen → kostet keinen Session-Kontext |
| `testtag/*.md` (12 Dateien, 2,16 MB) | 84 Voll-Checks im Wortlaut je Testtag, Session-Updates | **bleiben ungekürzt** (Kategorie 4) |
| `scripts/gate_check_log.jsonl` (846 KB) | jeder Gate-Aufruf mit Ein- und Ausgaben | unberührt |
| `scripts/trades.db` (73 KB) | alle Trades mit Zahlen — die harte Quelle für jede Statistik | unberührt |
| `scripts/skipped_setups_fiktiv.jsonl` (16 KB), `trigger_kandidaten_log.jsonl` (18 KB), `oneh_shadow_log.jsonl` (27 KB), `register_touch_log.jsonl` (13 KB), `loop_stopp_log.jsonl` | abgelehnte Setups, Trigger-Kandidaten, 1H-Schattenmessung, Register-Berührungen | unberührt |
| Git-Historie Code-Repo | jede Regeländerung als Commit mit Begründung | unberührt |

**Die eine Bedingung, unter der doch etwas verloren ginge:** wenn eine Zahl **ausschließlich** in einer gekürzten Passage stand und nie in ein Log, eine DB oder eine Regeldatei gewandert ist. Deshalb die Auslagerungs-Regel: nichts wird gelöscht, alles zieht um. Damit ist dieser Fall konstruktiv ausgeschlossen — und selbst ein Fehler bleibt über die Git-Historie rückholbar.

**Was tatsächlich anders wird:** In der Regeldatei steht künftig das Ergebnis mit Verweis, statt Ergebnis plus Rechenweg im selben Absatz. Wer den Rechenweg braucht, macht einen Klick mehr. Wer im Loop die Regel braucht, findet sie schneller und ohne überholte Nebenfassung daneben.

---

## 6. Entscheidung für Levi — eine Zeile pro Schritt

1. **S1 (Widersprüche W1–W4)** — meine klare Empfehlung: **ja, unabhängig von allem anderen**. Spart nichts, verhindert eine falsch angewandte Regel.
2. **S2 (Metadaten, tote Links)** — risikolos, freigeben.
3. **S3 (`MEMORY.md`)** — freigeben; offen bleibt nur: Kappung bei **200** oder 150 Zeichen?
4. **S4 + S5 (Auslagerung Historie)** — willst du die ~33 KB überhaupt? Ertrag ist −6,5 % Session-Ladelast. Wenn ja: mit Gegencheck.
5. **S6 (Gruppe E)** — meine Empfehlung: **zurückstellen**.
6. **S7 (Wartungsroutine + E7 als abgelehnt markieren)** — freigeben; E6 erst nach Klärung der Vorfrage.
7. **`feedback_chartanalyse.md`** — eigener Prüfdurchgang wie Bericht 2, oder bewusst nicht anfassen?

**Meine Gesamtempfehlung in einem Satz:** S1 sofort, S2 und S3 als saubere Routine hinterher, S4/S5 nur wenn du den Kontextgewinn wirklich willst — und S6 sowie E7 bleiben liegen, weil 1,7 % Ladelast kein Grund sind, gegen die eigene Regel [[feedback_dont_change_running_system]] zu arbeiten.
