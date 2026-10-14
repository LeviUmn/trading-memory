---
name: fable_auftrag_2026-10-05_auflagen_a1_a4
description: "Fable-Auftrag 05.10.2026 (Opus): Auflagen A1-A4 aus dem Opus-Gegencheck 05.10. zu 20d21aa. HEUTE nur A1 (Memory-Nachtrag Gegencheck 03.10. K1-K4 + Hinweise 1/2, Quelle gefunden: Transkript 9f4b9f35) und A4 (neues Lese-Skript scripts/analyse/rk13_1_filter.cjs, Live-Gates nach VC mit richtungskonflikt_13_1 separat). A2/A3 (Wortlaut loop_prompt.cjs) erst nach Endauswertung 08.10. Keine Live-Datei, keine Regel; Fable-Fenster bis 14:00 DE; Commit/Push nur Levi."
metadata:
  type: project
  originSessionId: 627984e6-a710-48e6-9244-0efcc80854ad
  modified: 2026-10-05T08:42:39.475Z
---

# Fable-Auftrag 05.10.2026: Auflagen A1-A4 (Gegencheck 20d21aa)

**Herkunft:** [[opus_gegencheck_umsetzung_prio1_prio2_2026-10-05]] Abschnitt "Auflagen" (Z. 72-76). Geschrieben von Opus, Mo 05.10.2026, Start 10:39:03 DE (bare `date`). Stand: HEAD 20d21aa, main == origin/main, Worktree `scripts/` sauber (`git diff --quiet HEAD -- scripts`).

**Rahmen (hart):** Regel-Freeze bis Endauswertung Do 08.10.2026, **keine Regelaenderung** ([[project_endauswertung_08_10_vorgezogen_2026-10-02]]). Hoechstens **eine** Live-Aenderung pro Testtag ([[project_testtag_2026-09-23_besprechung_ausstehend]] Z. 56 Freeze-Ende-Absatz und Z. 60 "Eine Änderung pro Testtag"). Die Live-Aenderung des 05.10. ist das Paket 20d21aa. Sein Abnahmelauf von 10:30 gilt nur, solange `loop_prompt.cjs` und die beiden Quell-Memories unveraendert bleiben.

**Rolle:** Fable setzt nur um. **Kein `git add`/commit/push** ([[feedback_commit_push_nur_levi]]). Am Ende woertlich "bereit zum Commit" melden und warten.

**Start-Bedingung: ERFUELLT, Fable kann sofort starten.** Der Scope von heute (A1 + A4) haengt an keinem offenen Levi-Entscheid. L-1/L-2 (unten) betreffen nur A2/A3 bzw. sind mit Default vorbelegt.

**Zeitfenster:** Mo 05.10. **bis 14:00 DE** (Puffer vor dem Start-Update 15:00 und vor 15:15). Ist Fable bis 14:00 nicht fertig, wird abgebrochen und nach dem Loop-Stopp 05.10. (ab ~20:05) oder am 06.10. vor 14:00 fortgesetzt. **Waehrend des Loops (15:00 bis Loop-Stopp) arbeitet Fable nicht** und laesst auch `npm test` nicht laufen. Den Opus-Kurzcheck fuer A4 gibt es **spaetestens Do 08.10. 14:30**, also vor dem end-Lauf (R4: erst `tagesmomente --datum 2026-10-08`, danach `--auswertung`/h1-`end`).

## Termin je Auflage (Opus-Festlegung)

| Auflage | Art | Termin | Begruendung |
|---|---|---|---|
| A1 Doku K1-K4 | nur Memory | **heute bis 14:00** | Beruehrt kein Skript und keine der zwei Memories, die `loop_prompt.cjs` liest. Unkritisch. |
| A2 Wortlaut "demselben N" | `loop_prompt.cjs` (Live) | **nach 08.10.** (Folgeauftrag) | Ist live-wirksam und braucht einen neuen Abnahmelauf und einen Opus-Check. Mitten in 05.-08.10. wuerde sich das Messinstrument fuer das Vorab-Kriterium [O12] aendern ("0 Abweichungen Dateiname <-> Kopfzeile"). Der Fall ist selten (Hard-Exit plus Wiederholung erst im Folgeslot), und der Prompt verlangt bei Abweichung schon jetzt "im Chat vermerken, nicht umbenennen". Der Nutzen rechtfertigt den Aufwand nicht. |
| A3 doppeltes `--jetzt` | `loop_prompt.cjs` (Live) | **nach 08.10.**, zusammen mit A2 | Ist harmlos: `parseArgs` nimmt den letzten Wert, und der Prompt sagt seit K2 schon "dessen --jetzt ist derselbe Wert <J>" (`loop_prompt.cjs` Z. 218). Es geht nur um Vereinfachung. |
| A4 Filter 08.10. | neues Lese-Skript `scripts/analyse/` | **heute bis 14:00**, Opus-Kurzcheck bis 08.10. 14:30 | Kein Live-Skript liest es. Die Filterregel wird **vor** den Daten ab 05.10. festgelegt (Vorab-Charakter). Laeuft Fable heute frueh, gibt es noch keine Daten vom 05.10., also kann nichts vorab gesichtet werden. |

---

## A1 — Memory-Nachtrag: Opus-Gegencheck 03.10. (K1-K4 + Hinweise 1/2)

**Quelle: auffindbar, keine Levi-Rueckfrage noetig.** Der Commit-Text von 20d21aa nennt nur "(K1-K4 + zwei Zusatzhinweise eingearbeitet)". Der Inhalt steht im lokalen Session-Transkript `C:\Users\umnus\.claude\projects\C--Users-umnus-tradingview-mcp\9f4b9f35-da7a-456c-9a16-094c4f451adb.jsonl` (nur lesen, nur mit `grep -o`, die Datei hat 23.228 Zeilen):
- Z. 23062 (03.10. 16:51 DE): Opus-Befunde K1-K4 mit Korrekturtexten.
- Z. 23129 (17:12 DE): "K1-K4 stimmen mit meinen Soll-Texten ueberein", 3 Hunks (Z. 218, 260, 271).
- Z. 23174 (17:18 DE): Hinweis 1 und Hinweis 2 eingearbeitet.
- Z. 23202 (17:27 DE): Commit-Text.

**Soll:** In [[fable_auftrag_2026-10-03_prio1_prio2]] am Dateiende einen Abschnitt "## Nachtrag 05.10.: Opus-Gegencheck der Umsetzung 03.10. (K1-K4 + Hinweise 1/2)" anhaengen, hoechstens 12 Zeilen. Inhalt je Punkt: 1 Satz Befund, die aktuelle Fundstelle in `loop_prompt.cjs` und der Vermerk "Hilfe/Wortlaut, keine Pflicht, keine Regelwirkung".
- **K1** (Z. 218): Vor dem ersten Exit-0-VC wird `vc_nr.cjs` mit demselben `--erster-vollcheck <Wert>` aufgerufen. Grund: Ohne State liefert vc_nr immer 1, und eine Wiederholung im Folgeslot hiesse sonst `_1b` statt `_2`.
- **K2** (Z. 218): `--jetzt` steht doppelt. Der Prompt stellt klar: "dessen --jetzt ist derselbe Wert <J>".
- **K3** (Z. 271): "verspaetet" wurde ersetzt durch "Fehlte das Slot-Fire (Minute % 5 == 0) oder kam es spaeter".
- **K4** (Z. 260): "Im/ab Slot" wurde zu "Im Slot".
- **Hinweis 1** (Z. 218): die K1-Klammer "(beim allerersten Lauf = <J>; nach Hard-Exit von VC#1 unveraendert der <J> jenes Laufs)".
- **Hinweis 2** (Z. 271): P2.4 mit "(vor dem ersten Exit-0-Lauf mit --erster-vollcheck <Wert>, wie REDIRECT-PFLICHT)".

Den Wortlaut nimmt Fable aus dem Transkript, nicht aus dem Gedaechtnis. Weicht er vom heutigen Text in `loop_prompt.cjs` ab, wird das gemeldet und nicht angeglichen. Zusaetzlich bekommt die A1-Zeile in [[opus_gegencheck_umsetzung_prio1_prio2_2026-10-05]] (Z. 73) den Zusatz " — erledigt 05.10. (Nachtrag in fable_auftrag_2026-10-03_prio1_prio2)". **Nicht** in MEMORY.md eintragen (das macht Sonnet/der Aufrufer).

**Akzeptanz:** Es sind genau 2 Memory-Dateien geaendert, beide nur additiv. Jede K-/Hinweis-Aussage hat eine gepruefte Zeilenangabe. Kein Repo-File ist geaendert.

## A4 — Lese-Skript `scripts/analyse/rk13_1_filter.cjs` (neu)

**Zweck:** Am 08.10. werden Live-Gates, die auf einen VC mit `richtungskonflikt_13_1: true` folgen, getrennt ausgewiesen. Ihre `--chasing`-Entscheide fliessen nicht in die 13.1-Bewertung ein. Das Skript ist **kein Gate und kein Urteil** und schlaegt keine Regel vor.

**Datenbasis (geprueft 05.10.):**
- `scripts/gate_check_log.jsonl` (1088 Z.): Live-Gates haben `modus:"live"` (157 Stueck, sonst `sl-vorpruefung`). Relevante Felder: `ts`, `jetzt`, `exit`, `status`, `dir`, `gemessen.chasing`, `argv` (dort `--grund-chasing "…"`).
- `scripts/vollcheck_log.jsonl` (529 Z.): Felder `ts`, `vc`, `exit_code`, `konsequenz`, bedingt `dry_run` (`vollcheck.cjs` Z. 654) und bedingt `richtungskonflikt_13_1` (gibt es erst ab 20d21aa, also ab dem 05.10.).
- Optional `scripts/kombi_fiktiv_log.jsonl` (`jetzt`, `skipped_id`) und `scripts/skipped_setups_fiktiv.jsonl` (`id`, `ergebnis_r`).

**Filterregel (Default, siehe L-2):** Zu jedem Live-Gate gehoert der **letzte `vollcheck_log`-Record mit `exit_code` 0, ohne `dry_run: true`, mit `ts` <= Gate-`ts` und am selben DE-Kalendertag** (DE-Datum wie in den Nachbarskripten, ohne `TZ=`). Traegt dieser Record `richtungskonflikt_13_1: true`, kommt das Gate in Gruppe **"NACH 13.1-KONFLIKT"**, sonst in **"OHNE KONFLIKT"**. Gibt es keinen solchen VC, kommt es in "OHNE VC-BEZUG".

**Soll:**
- CLI: `node scripts/analyse/rk13_1_filter.cjs [--ab 2026-10-05] [--bis 2026-10-08] [--json] [--gate-log P] [--vc-log P] [--kombi-log P] [--skipped-log P] [--selbsttest]`. Default `--ab 2026-10-05`. Die Default-Pfade sind relativ zu `__dirname/..`.
- Ausgabe je Tag und Gruppe: Gate-Uhrzeit DE, `status`/`exit`, `dir`, `chasing`, `grund-chasing` (aus `argv`, auf 80 Zeichen gekuerzt), VC-Nr. des Bezugs-VC. Wenn eindeutig zuordenbar, zusaetzlich `ergebnis_r` ueber `kombi.jetzt == gate.jetzt` und `kombi.skipped_id`, sonst "nicht zugeordnet". Dazu eine Summenzeile je Gruppe (Anzahl, davon chasing yes/no).
- Eine Fusszeile woertlich: "Gates der Gruppe NACH 13.1-KONFLIKT nicht in die 13.1-/--chasing-Bewertung mischen (Opus-Gegencheck 05.10., A4). Reine Auswertung, kein Gate, keine Regelaenderung."
- **Schreibt nie** in `scripts/` oder sonst ein echtes Log. `--selbsttest` arbeitet wie in `scripts/analyse/h1_auswertung.cjs`: synthetische Logs in `os.tmpdir()`, Aufraeumen per try/finally, mindestens diese Faelle:
  - (i) Gate nach einem Konflikt-VC kommt in NACH 13.1-KONFLIKT.
  - (ii) Gate nach einem VC ohne Feld kommt in OHNE KONFLIKT.
  - (iii) Liegt ein Hard-Exit-VC (exit 1, nie ein Feld) zwischen Konflikt-VC und Gate, zaehlt der Konflikt-VC als Bezug.
  - (iv) Ein `dry_run`-Konflikt-Record wird ignoriert.
  - (v) Ein Konflikt-VC vom Vortag gilt nicht (OHNE VC-BEZUG).
  - (vi) Ein `sl-vorpruefung`-Record wird nicht gezaehlt.
  - Exit 0 bei Erfolg, sonst Exit 1.
- Kopfkommentar mit Herkunft (A4), "REINES LESE-SKRIPT", Filterregel und Freeze-Hinweis.
- **Kein** Eingriff in `tagesmomente.cjs`, `h1_auswertung.cjs`, `protokoll_bilanz.cjs`, `skipped_fiktiv.cjs` oder `package.json`/Testsuite. Die Mengenbremse gilt: ein neues File, sonst nichts.

**Akzeptanz:**
- `--selbsttest` liefert Exit 0, alle Faelle gruen.
- Echtlauf auf **Kopien** der Logs in `$TEMP` (ueber `--gate-log/--vc-log/...`): mit `--ab 2026-09-16 --bis 2026-10-02` Exit 0 und **0 Gates in NACH 13.1-KONFLIKT**. Das ist erwartet, weil das Feld vor dem 05.10. nicht existiert. Abweichungen werden gemeldet. Mit Default `--ab 2026-10-05` heute vor dem Loop ist die Ausgabe leer bzw. "keine Live-Gates". Die Inhalte werden nicht weiter gesichtet.
- `--json` liefert gueltiges JSON mit denselben Zahlen.
- `git status` zeigt nur `?? scripts/analyse/rk13_1_filter.cjs` als neue Datei, keine getrackte Datei ist geaendert.

---

## Reihenfolge, Verifikation, Rueckmeldung

**Reihenfolge:**
1. `date` notieren und die sha1 der Live-Dateien festhalten.
2. A1.
3. A4 (Skript, Selbsttest, Echtlauf auf Kopien).
4. Optional einmal `npm test`, nur vor 14:00 (erwartet 424/424, unveraendert).
5. sha1 nachher pruefen.
6. Meldung.

**Verifikation (sicher):**
- **NIE `npm run test:e2e`**, kein `node --test tests/` ohne Pfadliste ([[feedback_npm_test_destruktiv_incident_2026-09-17]]).
- Logs nur als Kopien in `$TEMP`.
- sha1 vorher und nachher, alles muss identisch sein:
  - Live-Skripte: `loop_prompt.cjs` 9165617f…, `vollcheck.cjs` 1279067c…, `gate_check.cjs` d22c580a…, `loop_stopp.cjs` ff5633b2…, `vc_nr.cjs` 49979c19… (Stand 05.10. 10:41).
  - Logs: `momente_log.jsonl`, `skipped_setups_fiktiv.jsonl`, `kombi_fiktiv_log.jsonl`, `gate_check_log.jsonl`, `vollcheck_log.jsonl`, `vollcheck_state.json`, `oneh_shadow_log.jsonl`, `scripts/loop_archiv/*.txt`.
  - Memories: `feedback_live_trading.md`, `feedback_vollcheck_format.md`.

**Rueckmeldeformat:**
1. Je Datei: was geaendert wurde (2 Memories, 1 neues Skript).
2. Selbsttest- und Echtlauf-Ergebnis (Befehl -> Exit/Zahlen).
3. sha1-Tabelle vorher/nachher.
4. Abweichungen vom Auftrag mit Grund (insbesondere Wortlaut-Abweichungen bei A1).
5. Offene Punkte.
6. Zum Schluss woertlich **"bereit zum Commit"**, ohne `git add`/commit/push.

**Nicht anfassen:**
- Live-Skripte `loop_prompt.cjs`, `vollcheck.cjs`, `gate_check.cjs`, `loop_stopp.cjs`, `vc_nr.cjs` (A2/A3 ausdruecklich **nicht** heute).
- Memories `feedback_live_trading.md` und `feedback_vollcheck_format.md` (sonst ist der Abnahmelauf von 10:30 ungueltig).
- `tagesmomente.cjs`, `h1_auswertung.cjs` (H1-Vorabregistrierung), `protokoll_bilanz.cjs`, `skipped_fiktiv.cjs`, `kombi_fiktiv.cjs`, `package.json`, `tests/`.
- Regelwerk-Logik und Schwellen, H1 K1-K10, Freeze-Definition.
- Laufende Logs und `*.bak`, `/tmp/vc_*`, `scripts/loop_archiv/*`, `scripts/vollcheck_state.json`, die untracked Altdateien, MEMORY.md.

## Gegencheck-Punkte (Opus-Kurzcheck bis 08.10. 14:30)
1. A1: Wortlaut K1-K4/H1-H2 gegen Transkript Z. 23062/23129/23174 und gegen `loop_prompt.cjs` Z. 218/260/271. Nur additiv, keine Pflicht-Sprache.
2. A4: Filterregel exakt wie oben (Bezug = letzter Exit-0-VC ohne dry_run, selber DE-Tag). Selbsttest (i)-(vi) gruen. Kein Schreibzugriff ausserhalb von `os.tmpdir()`. Fusszeile vorhanden. Kein Urteil, keine Regel.
3. Betrieb: sha1 unveraendert, kein e2e, kein Commit, nur 1 neue Repo-Datei.

## Entscheidungen
- **L-1 (Levi, nicht blockierend fuer heute): Termin A2+A3. Empfehlung Opus: nach der Endauswertung 08.10., als ein kleiner Folgeauftrag.** Der neue Wortlaut fuer A2 waere: "Dateiname = Ausgabe von vc_nr.cjs fuer das --jetzt DIESES Laufs; Suffix b nur bei Wiederholung im selben Slot". Er braucht einen Abnahmelauf und einen Opus-Check. Die Alternative waere nach dem Loop-Stopp 05.10. fuer den Testtag 06.10. (als die eine Live-Aenderung des 06.10.). Davon raet Opus ab, weil sich das [O12]-Messinstrument mitten im Fenster 05.-08.10. aendern wuerde. **Default ohne Antwort: nach 08.10.**
- **L-2 (Levi, Default vorbelegt): A4-Bezugsregel.** Default ist der letzte Exit-0-VC desselben DE-Tages vor dem Gate. Die Alternative waere "nur der unmittelbar vorangehende VC-Record, egal welcher Exit". Die Empfehlung ist der Default, weil ein Hard-Exit-Record das Feld nie traegt und sonst einen Konflikt verdecken wuerde. Fable startet mit dem Default.
- **L-3 (Levi, Empfehlung nein): heute einen muendlichen Interimshinweis zu A2 an die Loop-Session geben?** Opus raet ab: Das waere faktisch eine zweite Prompt-Aenderung am Testtag. Die bestehende Regel "Abweichung im Chat vermerken, nicht umbenennen" macht den seltenen Fall sichtbar, und `loop_archiv.cjs` meldet `_x`/Fremdformen als NICHT EINGELESEN.
