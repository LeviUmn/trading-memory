---
name: fable_auftrag_2026-10-03_prio1_prio2
description: "Fable-Auftrag 03.10.2026 (Entwurf Sonnet, Opus-Gegencheck 03.10. = freigegeben mit Auflagen, 14 Befunde eingearbeitet): Prio 1 vollcheck.cjs Diagnose-Warnzeile '13.1-RICHTUNGSKONFLIKT' + bedingtes Log-Feld richtungskonflikt_13_1 (Konsequenz-Zeile byte-identisch, KEIN Gate, Warnzeile ohne Handlungsvorgabe); Prio 2 loop_prompt.cjs-Paket (C4/C5 + Dateiname=Skript-Nummer via vc_nr.cjs, --terminal-geprueft im Terminal-Slot, --grund-chasing immer, P2.4 Fire im Slot -> VC nur bei Levi-Ja); Aktivierung Testtag 05.10.; KEINE Regelaenderung; Fable setzt nur um, Commit/Push nur Levi; Start erst nach Levi-Entscheid R-2"
metadata:
  type: project
  originSessionId: 9f4b9f35-da7a-456c-9a16-094c4f451adb
  modified: 2026-10-03T14:19:05.940Z
---

# Fable-Auftrag 03.10.2026: Prio 1 (Diagnose-Warnzeile 13.1) + Prio 2 (Loop-Prompt-Paket)

**Herkunft:** [[opus_bericht_testtag_2026-10-02]] Abschnitt 4 (F1-F3) und 8 (Fable-Auftraege-Tabelle). **Entwurf:** von Sonnet nach der Opus-Tabelle geschrieben (Levi 03.10.: "Fable Auftrag fuer Prio 1 und 2 schreiben"). **Opus-Gegencheck 03.10.2026: freigegeben mit Auflagen** — alle Zeilenanker stimmen bei HEAD f92a4e0 (`vollcheck.cjs` 496, 650-685 (`nr_luecke` 682), 1104-1121, 1196, 1286-1291, 1306, 1359, 1585/86, 1603, 1604-1612 (Vorschlags-Zweig 1611), 1613, 2340, 2414, 2422/2424; `loop_prompt.cjs` 199, 234, 236, 247); die 14 Auflagen sind unten eingearbeitet (Kennung `[O1]`...`[O14]`). Die Golden-Tests sind sicher: alle 7 Szenarien in `tests/fixtures/vollcheck_a2_golden` haben die Konsequenz "keine".

**Rahmen (unveraendert, Levi 02.10.):** Freeze laeuft, **keine Regelaenderung** (Q-ROT, Q-Score, Gates, 13.1, Punkt 11, Schwellen), Endauswertung fest 08.10.2026 ([[project_endauswertung_08_10_vorgezogen_2026-10-02]]). Beide Prio-Punkte sind **Diagnose bzw. Betriebs-/Prompttext ohne Gate-Wirkung**.

**Rolle:** Fable setzt nur um. **Kein `git add`/commit/push** ([[feedback_commit_push_nur_levi]]); am Ende melden: "bereit zum Commit" und warten.

**Start-Bedingung ERFUELLT (Levi 03.10.2026: "Ja so kann das umgesetzt werden"): R-2 = JA, P2.4 wird eingebaut; Umsetzung freigegeben.** **R-1 ist beantwortet** (Opus: Prio 1 + Prompt-Paket = EINE Betriebsaenderung, unter der Bedingung in [O4]).

**Zeitfenster:** 03./04.10. ist Wochenende, kein Cron-Testtag. `vollcheck.cjs` und `loop_prompt.cjs` sind Live-Skripte: Umsetzung **fertig und von Opus gegengecheckt bis Mo 05.10. 14:30 DE** (vor dem Start-Update 15:00; an Testtagen nicht zwischen 15:15 DE und Loop-Stopp aendern). Aktivierung = Testtag **05.10.** (Levi R-3, [[fable_vorschlag_c4_c5_loop_prompt_2026-10-02]]). **Fehlt der Opus-Gegencheck fuer Prio 1 bis 05.10. 14:30, geht nur das Prompt-Paket live; Prio 1 folgt dann am 06.10.** [O: R-1]

---

## PRIO 1 (Code, reine Diagnose): `scripts/vollcheck.cjs` — "13.1-RICHTUNGSKONFLIKT"

**Befund (Opus, am 02.10. belegt):** Die 13.1-Chasing-Zaehlung (`kerzen-nas100`, State-Feld `bias_5m`) zaehlt Kerzen auf der **5m-EMA50-Seite**, egal in welche Richtung das Dual-Gate steht. Stand das Dual-Gate LONG und die 5m-Kerzen SHORT-seitig (Pullback), meldete das Skript "50 %-Einstieg AKTIV vorschlagen" fuer LONG. Das ist das Gegenteil von Chasing in Setup-Richtung. Folge am 02.10.: VC#27 -> Live-Gate 17:36 LONG `--chasing yes` -> ein Artefakt-Datenpunkt (skipped -0,25 R, Kombi -0,01 R, Q-ROT-Zaehlung +1). Kein Geldschaden. Das Skript sagt im Grund-Text sogar "ACHTUNG Richtungs-Konflikt", aber nichts im Voll-Check-Output warnt.

**Fundstellen (Opus bestaetigt, `vollcheck.cjs`):**
- Z. 1306: `bias5 = biasVon(c5, e5)` (aktuelle 5m-Seite); Z. 1359: `dgRichtung` (Dual-Gate-/Setup-Richtung).
- Z. 889/921: Ableitung der `kerzen-nas100`-Zaehlung (Seitenwechsel = Reset); Z. 2340: `bias_5m: bias5` im State.
- Z. 1583-1613: 13.1-Block: `dualGateVoll` (1585), `kOhne` (1586), `vorschlag13_1` (1603), Konsequenz-Kette (1604-1612, der Vorschlags-Zweig ist Z. 1611), Ausgabezeile `**Chasing-Status (13):**` (1613), danach `if (kollision13_1)` (1614-1619).
- Log: `process.on('exit')`, Record `rec` Z. 650-685 (bedingtes Spread-Muster wie `nr_luecke` Z. 682); `v4.konsequenz` wird Z. 2414 gesetzt.

**Soll:**
1. **Bedingung per Flag, nicht per Textvergleich [O3]:** Im Vorschlags-Zweig Z. 1611 (mit und ohne Option-D-Ausnahmezweig) ein boolesches `vorschlagAktiv13_1 = true` setzen. Konflikt = `vorschlagAktiv13_1` **UND** `bias5 ∈ {long, short}` **UND** `dgRichtung ∈ {long, short}` **UND** `bias5 !== dgRichtung`. In allen anderen Zweigen (keine/beobachten/Q-ROT geht vor/nicht bestimmbar) keine Warnzeile.
   - **Randfaelle (dokumentieren und testen) [O3]:** `bias5 === 'neutral'` (5m-Close = EMA50): die Serie laeuft unveraendert auf der State-Seite weiter -> **keine** Warnung (konservativ) und **kein** Fix am State-Schreiben. **Ohne `--state`** ist `bias5` die Seite der Serie ("Kerzen seit Gegenseite" zaehlt per Definition auf der aktuellen Seite). Bei vollem Dual-Gate gilt immer `beinNas === dgRichtung`; ein Fallback auf das QQQ-Bein ist ohne Belang.
2. **Warnzeile**, direkt nach der `**Chasing-Status (13):**`-Zeile (und nach der Kollisionszeile, falls vorhanden) — **ohne neue Handlungsvorgabe [O4]** (Voraussetzung dafuer, dass Prio 1 als "reine Messung/Schatten-Zeile" gilt, siehe R-1):
   `**13.1-RICHTUNGSKONFLIKT (Diagnose, kein Gate):** Kerzenserie auf der <SHORT|LONG>-Seite der 5m-EMA50, Dual-Gate-Richtung <LONG|SHORT> — der 13.1-Vorschlag bezieht sich damit auf eine Serie GEGEN die Setup-Richtung (Pullback, kein Chasing im Sinne von Punkt 13). Konsequenz-Zeile und 7b1-Ablauf unveraendert (Freeze, Auslegung nach 08.10.); den Konflikt im Fliesstext und ggf. im --grund-chasing benennen.`
   Sprache/Stil wie die Nachbarzeilen; keine neue Pflicht, kein Exit-Effekt.
3. **Log-Feld [O2]:** `richtungskonflikt_13_1: true` im `vollcheck_log.jsonl`-Record, **nur wenn zutreffend** (bedingter Spread wie `nr_luecke`; fehlt sonst). `v4.richtungskonflikt_13_1` wird **zusammen mit `v4.konsequenz` an Z. 2414** gesetzt, nicht im 13.1-Block; ein Hard-Exit-Record traegt das Feld nie. Bestehende Zeilen/Golden-Artefakte bleiben unveraendert.
4. **Nicht aendern:** die Konsequenz-Kette/`vorschlag13_1`-Text, `kollision13_1`, `gate_check.cjs` (Live, tabu), Zaehlerlogik/State-Schema, Pflichtfelder.

**Akzeptanz Prio 1:**
- `npm test` gruen (nur Offline-Suiten; die Golden-Tests in `tests/trading_scripts.test.js` ab Z. ~4561 duerfen sich **nicht** aendern — aendert sich ein Golden, melden statt Golden anpassen).
- Neue Tests (Sandbox wie bestehende, nie die echten Logs): (i) Konflikt: State `bias_5m` short, `kerzen_nas100` >= 5, `k_ohne_signal` 1, dieser Lauf ohne Punkt-11-Signal, Dual-Gate 2/2 long -> k = 2 -> Warnzeile **genau 1x**, Konsequenz-Zeile **byte-identisch**, Log-Record mit `richtungskonflikt_13_1: true`; (ii) gleiche Richtung (5m long, Gate long, k = 2) -> keine Zeile, kein Feld; (iii) k = 1 -> nichts; (iv) Dual-Gate nicht voll -> nichts; (v) Option-D-Zweig `qrotVor` ("beobachten") -> nichts; (vi) `bias5 === 'neutral'` -> nichts. **Im Unit-Test die `Chasing-Status (13)`-Zeile als Literal pruefen [O5].**
- **Byte-Identitaet manuell nachweisen (nicht im Test) [O5]:** `git show HEAD:scripts/vollcheck.cjs` in eine Sandbox-Kopie legen und Konflikt-Szenario (i) mit HEAD und mit Patch fahren. Der stdout-Diff muss **genau die eine eingefuegte Zeile** sein, der `vollcheck_log`-Diff **genau das eine Feld**.
- **Sandbox-Pflicht [O6]:** Die Logs liegen relativ zu `__dirname` (`vollcheck.cjs` Z. 637). Replay und Tests **nur aus einer Kopie des ganzen `scripts`-Ordners in `%TEMP%`** starten (wie die SZ-Sandbox der Tests), **nie aus `scripts/`** (in Git-Bash `$TEMP` statt `%TEMP%`).
- **Replay 02.10. (Sandbox, nur Kopien):** `vollcheck_state.json` des 02.10. kopieren, `historie` bis vor VC#27 kuerzen, Argumente aus `/tmp/vc_2026-10-02_27.txt` rekonstruieren (die Datei zeigt "Dual-Gate 2/2 LONG", "6 Kerzen", "k 2/2", "50 %-Einstieg AKTIV") -> Warnzeile 1x bei VC#27; bei VC#26 (k = 1) 0x. Ist die Rekonstruktion nicht moeglich, den synthetischen Test (i) als Ersatz melden.
- **Rueckrechnung ueber alle Tage (lesend, in `%TEMP%`) [O1]:** `vollcheck_log.jsonl` (Feld `konsequenz` beginnt mit "50 %-Einstieg AKTIV") ueber `ts`+`vc` mit `oneh_shadow_log.jsonl` (Bias je Ebene) verknuepfen; die Gate-Richtung kommt aus dem 15m-Bein (`richtung` im Log ist durchgehend `none`). **Erwartung (Opus): 02.10. 1 VC (VC#27); 01.10. und 30.09. 0; zusaetzlich 16.09. VC#26 (2 Records, einer davon Re-Run) und VC#27; 28.09. VC#52.** Gezaehlt werden **verschiedene VC-Nummern je Tag, `dry_run`-Records ausgenommen.** Abweichung melden, nicht "passend machen".

---

## PRIO 2 (Template-Paket): `scripts/loop_prompt.cjs` (259 Zeilen, HEAD f92a4e0)

**Zustand:** C4/C5 (VC#1-Block, Redirect-Pflicht, `loop_archiv.cjs` im LOOP-STOPP) sind **noch nicht eingebaut**; die exakten Texte liegen in [[fable_vorschlag_c4_c5_loop_prompt_2026-10-02]] (Einfuegung 1 nach Z. 199, Aenderung 2 in der LOOP-STOPP-Zeile Z. 236, Aenderung 3 im Selbsttest Z. 247, inkl. der Opus-Korrekturen (a)-(d)). `loop_archiv.cjs` ist am 02.10. produktiv benutzt worden (Exit 0, 59 Dateien, Luecke gemeldet).

**P2.0 C4/C5 aus der Vorschlagsdatei 1:1 einbauen** (Anker vorher gegen die aktuelle Datei pruefen). `loop_stopp.cjs` **nicht** mitaendern (Korrektur (d) bleibt nach 08.10.). **[O7] P2.1 ersetzt in Einfuegung 1 genau die Klammer des C5-Textes "(N = Voll-Check-Nummer …; bei mehreren Loop-Starts am Tag fortlaufend nummerieren)" durch die Slot-Regel; alles andere bleibt 1:1.**

**Zusaetze aus dem Opus-Bericht 02.10. (nur Prompttext/Hilfsskript, keine Gate-Wirkung):**

**P2.1 Redirect-Dateiname = Skript-Nummer (F2) [O7, O8].** Problem 02.10.: Der Operator zaehlte selbst; nach dem ausgefallenen Slot 19:20 hiess die Datei `_48`, das Skript nummerierte aber #49 (`_48` = VC#49; `loop_archiv` meldete "Luecke #49"). Soll: N ist die **Slot-Nummer nach der Skript-Formel** (`vollcheck.cjs`: Funktion `vcNummer(jetztMs, ersterMs)` Z. 496, Aufruf Z. 1286-1291), nicht mitgezaehlt; faellt ein Slot aus, springt N mit (V12), Datei nie umbenennen/loeschen. Weicht die Kopfzeile "Nr. N" vom Dateinamen ab: im Chat vermerken, nicht umbenennen.
- **Umsetzung: neues Lese-Hilfsskript `scripts/vc_nr.cjs`** (nicht live-kritisch, schreibt nichts):
  - `vcNummer` **duplizieren statt exportieren** (damit Prio 2 nicht an der Abnahme von Prio 1 haengt); Paritaet per Test sichern.
  - Aufruf `node scripts/vc_nr.cjs --jetzt <UTC-ISO Z>`; **stdout nur die Zahl**, Fehler auf stderr mit Exit 1 (Aufruf ohne `--jetzt` -> klare Meldung, Exit 1).
  - **State vom Vortag:** `vollcheck_state.json` nur lesen, wenn `datum_de` = DE-Datum von `--jetzt` (wie `vollcheck.cjs` Z. 821); sonst Ausgabe **1** (am 05.10. liegt noch der State vom 02.10. mit `erster_vollcheck` 13:27:09Z, sonst kaeme eine Nummer um 870 heraus). Optional `--erster-vollcheck <ISO>` hat Vorrang vor dem State (wie Z. 824-829).
  - **Paritaetstest mit Fixture** `erster = 2026-10-02T13:27:09Z` (nicht mit dem echten State). Faelle (`--jetzt` in **UTC**; die Uhrzeiten im Log sind DE): 13:27:09Z -> 1; 17:26:16Z -> 49; 17:31:11Z -> 50; Slotgrenzen :x4:59/:x5:00; Vortags-State -> 1. (Opus hat die drei Werte am `vollcheck_log.jsonl` des 02.10. geprueft.)
  - **Aufrufmuster im Prompt:** dasselbe `--jetzt` fuer `vc_nr` und `vollcheck`, in **einem** Bash-Aufruf: `N=$(node scripts/vc_nr.cjs --jetzt <J>) || N=x; node scripts/vollcheck.cjs … > /tmp/vc_<D>_$N.txt 2>&1; echo "EXIT=$?"; cat /tmp/vc_<D>_$N.txt` (Wiederholung nach Hard-Exit als `_${N}b`). Bei einer Datei `_x` meldet `loop_archiv` "NICHT EINGELESEN", nichts geht still verloren.
- **Fallback**, falls Opus das Hilfsskript ablehnt: reiner Prompttext mit der Formel.
- ~~Optionale `loop_archiv`-Warnzeile~~ **gestrichen [O9]** (mit `vc_nr.cjs` ueberfluessig, wuerde die Mengenbremse sprengen).

**P2.2 `--terminal-geprueft` im Terminal-Slot (F3) [O10].** Hard-Exit VC#56 20:01: `--terminal-geprueft` fehlte, weil `--terminal-zeit 20:02` im Slot lag (`vollcheck.cjs` Z. 1104-1121: `terminalSlot = tMin >= slot && tMin <= slot + 5`; Pflicht Z. 1196). Die Pflicht steht im Prompt schon allgemein (Z. 234), der Hard-Exit kam trotzdem. Soll: `loop_prompt.cjs` **berechnet aus `terminal` die konkreten VC-Slots S mit S ≤ T ≤ S+5 und druckt sie** (T = 20:02 -> Slot 20:00; T = 20:00 -> Slots 19:55 **und** 20:00); bei `terminal = 'keine'`/Platzhalter entfaellt die Zeile. Satzbau: "Im/ab Slot <S>: `--terminal-geprueft ja|nein` (ja = Terminalbedingung/offene Position geprueft)."

**P2.3 `--grund-chasing` bei jedem Live-`gate_check.cjs` [O11].** Zwei Exit-1-Abbrueche am 02.10. (16:06:26 und 18:06:16): `--chasing no` ohne `--grund-chasing` (16:06 zudem inhaltlich falsch). **Als eigener, zusaetzlicher `block.push` hinter Z. 216** (nicht als Aenderung an Z. 216; Diff bleibt additiv). Text: "`--chasing yes|no` IMMER mit `--grund-chasing "<n gerichtete Kerzen, Seite, k-ohne-signal k>"` (Pflicht seit 11.09.2026, sonst Exit 1); yes/no wie bisher nach `feedback_live_trading.md` (Zeile `--chasing`), keine neue Auslegung. Steht im letzten Voll-Check '13.1-RICHTUNGSKONFLIKT', im Grund benennen." **Nicht** mit dem frueheren Entwurfssatz "`no` nur, wenn die Kriterien … nicht erfuellt sind": der drueckt im Konfliktfall genau zum 17:36-Fehler (yes uebernehmen). Hinweis Opus: `--grund-chasing` als Pflicht ist an Kommentaren belegt (`gate_check.cjs` Z. 3557/4352); Fable verfolgt den Codepfad einmal und meldet.

**P2.4 Fire im laufenden VC-Slot -> Voll-Check [O14] (Levi-Entscheid 4 = R-2).** 19:21-Fall: Das Fire kam 1 Min nach Slot-Beginn 19:20, Sonnet machte Quick-Tick statt VC -> Slot ausgefallen (#48). **Opus empfiehlt ja, ab 05.10. aktivieren** (nur Abdeckung, keine Regelwirkung). **Nicht schlafend einbauen:** Levi entscheidet **vor** dem Start von Fable; bei **Ja** wird es eingebaut, bei **Nein** gar nicht (kein Schalter, kein toter Text). Wortlaut: "Kommt ein Cron-Fire verspaetet, liegt aber noch im laufenden 5-Min-Slot, und fuer diesen Slot gibt es noch **keine** Datei `/tmp/vc_<D>_<N>.txt` (N aus `vc_nr.cjs`): Voll-Check ausfuehren, kein Quick-Tick. Ein Doppel-Fire im selben Slot bleibt Quick-Tick."

**P2.5 Selbsttest/Tests:** Fragmente aus P2.0 (Z. 247) um je ein Fragment pro neuer Pflicht erweitern (`--terminal-geprueft`, `--grund-chasing`, `vc_nr.cjs`, ggf. `Fire im laufenden`); `tests/trading_scripts.test.js` P1-Tests auf Konflikt pruefen und ein Fragment-Test ergaenzen; Test fuer die Slot-Berechnung aus P2.2 (T = 20:02 -> {20:00}; T = 20:00 -> {19:55, 20:00}; 'keine' -> keine Zeile).

**Akzeptanz Prio 2:**
- `node scripts/loop_prompt.cjs --testtag fiktiv --terminal-zeit 20:02` -> Exit 0, Selbsttest gruen, sichtbar: "ERSTER VOLL-CHECK DES TAGES", "REDIRECT-PFLICHT" (mit `vc_nr.cjs`-Aufrufmuster), LOOP-STOPP nennt `loop_archiv.cjs --datum`, `--terminal-geprueft` mit konkretem Slot, `--grund-chasing`, ggf. "Fire im laufenden".
- **[O13] Schutz fuer den Loop-Start:** Scheitert der Selbsttest, endet `loop_prompt.cjs` mit Exit 1 und der Testtag startet nicht. Der Abnahmelauf `node scripts/loop_prompt.cjs --testtag fiktiv --terminal-zeit 20:02` wird **mit den ECHTEN Memory-Dateien am 05.10. vor 14:30 wiederholt** (Sonnet/Opus); **zwischen Abnahme und Start keine Edits an `feedback_live_trading.md` / `feedback_vollcheck_format.md`**.
- `node scripts/loop_archiv.cjs --datum 2026-10-02 --out-dir $TEMP/la_test` laeuft Exit 0 und bleibt sonst unveraendert (Original-Archiv sha1 vor/nach identisch).
- `vc_nr.cjs`-Paritaet (Fixture-Faelle s. o.).
- Diff auf `loop_prompt.cjs` additiv (Ausnahme: die LOOP-STOPP-Zeile aus P2.0 und die Klammer aus [O7]); `loop_stopp.cjs`, `gate_check.cjs` und (ausser Prio 1) `vollcheck.cjs` per `git diff --quiet HEAD` unveraendert.
- **Vorab-Erfolgskriterium (Opus-Tabelle) [O12]:** 05.-08.10.: 0 Hard-Exits wegen `--terminal-geprueft`/`--grund-chasing`, 0 Abweichungen Dateiname <-> Kopfzeile "Nr. N".

---

## Reihenfolge, Verifikation, Rueckmeldung

**Reihenfolge:** (0) R-2 entschieden (JA, Levi 03.10.) -> (1) Prio 1 (isoliert, eigene Tests) -> (2) P2.0 -> P2.1-P2.5 -> (3) Gesamt-`npm test` -> (4) Meldung "bereit zum Commit". Opus-Gegencheck beider Pakete **vor Mo 05.10. 14:30**; Aktivierung automatisch, weil der Cron-Prompt am 05.10. aus `loop_prompt.cjs` entsteht.

**Verifikation (sicher):** `npm test` (nur Offline-Suiten, Tests laufen in einer Sandbox unter `%TEMP%`). **NIE `npm run test:e2e`**, kein `node --test tests/` ohne Pfadliste, kein `tests/e2e.test.js` ([[feedback_npm_test_destruktiv_incident_2026-09-17]]). Alles, was Logs schreibt, nur auf Kopien in `%TEMP%`/`$TEMP`. sha1 vorher/nachher von `momente_log.jsonl`, `skipped_setups_fiktiv.jsonl`, `kombi_fiktiv_log.jsonl`, `gate_check_log.jsonl`, `vollcheck_log.jsonl`, `vollcheck_state.json`, `loop_archiv/*.txt` vergleichen und melden (muss identisch sein).

**Rueckmeldeformat:** (1) je Datei: was geaendert, Diff-Zusammenfassung; (2) Testergebnisse (Befehl -> Exit/Anzahl), sha1-Vergleiche; (3) Replay-/Rueckrechnungs-Ergebnis Prio 1 (Zaehlung je Tag); (4) Abweichungen vom Auftrag mit Grund; (5) offene Punkte; (6) zum Schluss woertlich **"bereit zum Commit"**, ohne `git add`/commit/push.

**Nicht anfassen:** Regelwerk-Logik (Q-ROT, Q-Score, Gates, `gate_check.cjs`, 7b1, 8b/8c/8c2, Dual-Gate, 1H-Override, 13.1-Konsequenz-Logik, Punkt 11, Schwellen/Konstanten), H1-Kriterien K1-K10, Freeze-Schwellen, `loop_stopp.cjs`, laufende Logs und `*.bak`, Tagesdaten (`/tmp/vc_*`, `scripts/loop_archiv/*`, `scripts/nas100_5m_2026-1*.json`), `scripts/vollcheck_state.json` (Live-State fuer 05.10.).

## Gegencheck-Punkte (Opus nimmt ab)
1. Warnzeile: nur im Vorschlags-Zweig (Flag `vorschlagAktiv13_1`), nur bei Seiten-Konflikt, Text **ohne Handlungsvorgabe**; Konsequenz-Zeile byte-identisch (stdout-Diff = genau 1 Zeile, Log-Diff = genau 1 Feld); Golden-Tests unveraendert; Log-Feld bedingt und nur im Exit-0-Record.
2. Rueckrechnung Prio 1 gegen die Erwartung in [O1] (02.10.: 1; 01.10./30.09.: 0; 16.09.: VC#26/#27; 28.09.: VC#52).
3. `loop_prompt.cjs`-Diff: C4/C5 wie Vorschlagsdatei, [O7]-Klammer ersetzt, P2.1-P2.3 nur Prompttext (+ `vc_nr.cjs`), P2.4 nur bei Levi-Ja, additive `block.push`; keine Gate-/Regeltexte geaendert.
4. `vc_nr.cjs`: Paritaet inkl. Vortags-State -> 1 und UTC-Fixture; kein Schreibzugriff; stdout nur die Zahl.
5. Betrieb: `npm test` gruen, kein e2e, sha1 der Live-Dateien unveraendert, Abnahmelauf `loop_prompt.cjs` mit echten Memory-Dateien am 05.10. vor 14:30, kein Commit/Push.

## Entscheidungen
- **R-1 (beantwortet, Opus 03.10.): nein — Prio 1 und das Prompt-Paket zaehlen zusammen als EINE Betriebsaenderung am 05.10.** Massgeblich: `project_testtag_2026-09-23_besprechung_ausstehend.md` Z. 56 ("hoechstens eine Live-Aenderung pro Testtag; reine Messungen und Schatten-Zeilen sind erlaubt"); Prio 1 ist eine Diagnosezeile mit bedingtem Log-Feld ohne Gate- und Exit-Wirkung, Konsequenz byte-identisch -> "reine Messung/Schatten-Zeile". **Bedingung:** die Warnzeile enthaelt keine neue Handlungsvorgabe [O4]. Praezedenz: C4+C5 als eine Aenderung ([[fable_auftrag_2026-10-02_endauswertung_08_10]] Abschnitt D, Reihenfolge 5). Fehlt der Gegencheck bis 05.10. 14:30 -> Prio 1 erst am 06.10.
- **R-2 (ENTSCHIEDEN, Levi 03.10.2026: JA — P2.4 "Fire im Slot -> Voll-Check" wird eingebaut, ab 05.10. aktiv).** Opus empfahl **ja** (nur Abdeckung, schliesst die Luecke #48; die Bedingung "fuer den Slot lief noch kein VC" ist mit `vc_nr.cjs` ueber die fehlende Datei eindeutig pruefbar).- Opus-Entscheide 1-3, 5, 6 (Opus-Bericht Abschnitt 8) sind fuer diesen Auftrag nicht blockierend.

## Nicht in diesem Auftrag (Backlog, Opus-Tabelle)
F4 `protokoll_bilanz` (Screenshot-Soll gegen verschiedene VC-Nummern, b-Laeufe als "Wiederholung"), F5 Screenshot-Dateiname aus echter Aufnahmezeit, VIX-Range-Handwert, Entry-Quelle "5m-Close" gegen Bar-Close pruefen (Q3-AUTO-5m-Bein), QQQ-Session-Tief ab 15:30 im Session-Update, Impuls-Extrem aus Bar-Dochten, DST-Fix `vollcheck.cjs` (eigener Opus-Auftrag bis 19.10., [[project_vollcheck_dst_fix_todo_2026-10]]), `loop_stopp.cjs`-Hinweistext `--protokoll` -> `--datum` (nach 08.10.).
