---
name: fable_vorschlag_c4_c5_loop_prompt_2026-10-02
description: "VORSCHLAG (nicht aktiv, Aktivierung ab Testtag 05.10.2026 laut Levi R-3): C4 VC#1-Block + Redirect-Pflicht und C5 loop_archiv.cjs im LOOP-STOPP-Text fuer scripts/loop_prompt.cjs — NICHT in den Cron-Prompt eingebaut (Don't change a running system); Korrekturen (a)-(d) aus dem Opus-Gegencheck 02.10. eingearbeitet (festes Bash-Aufrufmuster mit echo EXIT/cat, --sl-anker nur ab >= 1 Bein (0-Bein-Dummy belegt unschaedlich, trotzdem weglassen), kein --ueberschreiben im LOOP-STOPP, loop_stopp.cjs erst nach 08.10.); Aktivierungsablauf nach R1 (02.10.): Prompt-Aenderung ausserhalb Loop-Zeit -> --testtag fiktiv -> npm test -> Opus-Kurzcheck Diff -> ab 05.10.; enthaelt die exakten Codezeilen + Testbefehl"
metadata:
  type: project
  originSessionId: fable-umsetzung-endauswertung-2026-10-02
  modified: 2026-10-02T09:29:12.234Z
---

# Vorschlag C4 + C5: Aenderungen an `scripts/loop_prompt.cjs` (Fable, 02.10.2026) — NICHT EINGEBAUT

**Warum nicht eingebaut:** Rueckfrage R-3 des Opus-Auftrags ([[fable_auftrag_2026-10-02_endauswertung_08_10]]) hat Levi am 02.10.2026 entschieden: **Aktivierung ab Testtag 05.10.2026**, nicht am 02.10. (Stand R1, Opus-Final-Gegencheck [[opus_gegencheck_final_auflagen_2026-10-02]]); damit bleibt es dabei: Aenderungen an der Loop-Prompt-Vorlage **nicht** in Dateien, die der heutige Cron-Testtag (Start 15:30 DE) liest ([[feedback_dont_change_running_system]]; Freeze: hoechstens eine Live-Aenderung pro Testtag — C4+C5 gelten zusammen als EINE Betriebsaenderung ohne Gate-Wirkung). `scripts/loop_archiv.cjs` selbst ist fertig und getestet (C5-Skript, siehe [[fable_umsetzung_2026-10-02_endauswertung_08_10]]); nur der Prompt-Text fehlt.

**Vorab-Prüfung an `vollcheck.cjs` (C4-Auflage, Befund):** `--sl-anker` und `--cluster-level` stehen in der A3-Spec mit `wenn: () => slVorpruefungFaellig` (Z. 1184/1191) — bei 0 Beinen keine Pflicht, aber **angenommen** (die Test-Basis `VC_BASE` uebergibt beide bei jedem Lauf, auch mit `--richtung none`); `--stale-n` ist ein optionales Zahlenfeld (nur Typ-Guard, Z. 1209-1211), `--grund-stale-n` wird nur bei erfuellter Stale-Vorbedingung gelesen (Z. 1376-1384). **Nichts wird bei 0 Beinen abgelehnt**, kein Code muss gelockert werden — bedingungslos verlangt der Block aber nur `--cluster-level` und `--stale-n`/`--grund-stale-n`; `--sl-anker` gilt **nur ab >= 1 Bein** (bei 0 Beinen weglassen, siehe Korrektur (b) — Stand nach Auflage 5, 02.10.2026).

**Korrektur (b), Opus-Gegencheck 02.10.2026 — `--sl-anker` bei 0 Beinen (Befund Fable, Sandbox-Dry-Run + Codelesen, keine Repo-Logs beruehrt):**
- Code: `slVorpruefungFaellig = richtung long|short ODER beinePre >= 1` (vollcheck.cjs Z. 726). Daran haengen **alle** Anker-Verbraucher: A3-Pflicht (Z. 1184/1191), P1-Seitenpruefung (Z. 1223 `if (slVorpruefungFaellig && dirP1 && …)`), der gesamte SL-Block mit Vorpruefungs-Unterprozess, P2/P3-Zaehlung (x1StatusAus) **und** der A2-Block mit `anker_live` → `anker_auto_log.jsonl` (Z. 2002 `if (slVorpruefungFaellig && slDir) { … }` schliesst erst Z. 2151, also NACH dem Log-Append Z. 2137). Bei 0 Beinen ohne Setup-Richtung ist der Block **unerreichbar**; `vollcheck_log.jsonl` traegt kein Anker-Feld.
- Beleg (Sandbox-Kopien aller scripts/*.cjs in %TEMP%, Golden-Szenario `c_nicht_faellig_live`: `--richtung none`, 15m-Close = EMA50 fuer NAS100 und QQQ = 0/2 Beine, `--nas-bars-5m` gesetzt, `--stale-n 0`): mit Anker 29480 (Golden), mit Dummy **99999** (falsche Seite fuer beide Richtungen) und Dummy **1**, je als `--dry-run` und als Live-Lauf in der Sandbox → Exit 0, Zeile "**Dual-Gate:** keine Richtung bestimmbar", **0** SL-Anker-/P1-/AUTO-ANKER-Zeilen, `anker_auto_log.jsonl` und `gate_check_log.jsonl` **nicht angelegt**, der Anker-Wert taucht in keinem Artefakt (vollcheck_log/oneh_shadow_log/tweet_fetch_log) auf. Gleiches sagt der bestehende Test (tests/trading_scripts.test.js, A2-Block: "nicht faellig (0/2 Beine) -> keine Zeile, kein Log", Golden-Artefakte byte-identisch).
- **Ergebnis: Ein Anker bei 0 Beinen verfaelscht nichts** — weder P1/P2/P3-Zaehlung noch `anker_auto_log.jsonl`; er wird schlicht nicht gelesen. Sobald aber >= 1 Bein gerichtet ist (15m-Close ≠ EMA50 — der Normalfall, auch bei `--richtung none`), wird der Anker Pflicht, P1 prueft die Seite gegen die **Bein-Richtung** und `anker_live` wird geloggt. Deshalb ist "IMMER --sl-anker, unabhaengig von der Beinzahl" **nur mit praezisiertem Wert** sinnvoll:
- **Empfehlung (im VC#1-Block so formulieren):** bei >= 1 Bein: Struktur-Anker (7b1 3a) auf der SL-Seite der **Bein-Richtung** (P1; das ist auch der Hard-Exit-Fall vom 01.10.). Bei 0 Beinen (beide 15m-Beine neutral, keine Setup-Richtung): `--sl-anker` **weglassen** — ohne Richtung gibt es keine SL-Seite, ein Dummy ist belegt unschaedlich, aber ein echter Strukturwert ohne Richtung ist nicht definierbar, und bei einem spaeteren Lauf mit Bein wuerde ein mitgeschleppter "Dummy" sonst ungeprueft als `anker_live` in die Messreihe wandern. `--cluster-level` und `--stale-n`/`--grund-stale-n` bleiben bedingungslos im VC#1. Die Zeile im Einfuegung-1-Block ist unten entsprechend geaendert (Anker-Text), der Rest unveraendert.

## Einfuegung 1 — nach Z. 199 (`block.push(\`--testtag ${testtag} ist PFLICHT ...\`)`), vor dem AUTO-ANKER-SCHATTEN-Absatz

```js
  // C4 (Fable-Auftrag 02.10.2026, Opus-Spezifikation): ERSTER VOLL-CHECK DES TAGES — 4 Loop-Start-Hard-Exits in 3 Tagen (29.09. VC#17 --stale-n,
  // 30.09. VC#1+#2 --stale-n, 01.10. VC#1 --sl-anker/--cluster-level). vollcheck.cjs nimmt die Werte auch bei 0 Beinen an (geprueft 02.10.).
  block.push('ERSTER VOLL-CHECK DES TAGES (C4, 02.10.2026 — 4 Loop-Start-Hard-Exits in 3 Tagen: 29.09. VC#17 --stale-n, 30.09. VC#1+#2 --stale-n, 01.10. VC#1 --sl-anker/--cluster-level): VC#1 traegt IMMER --cluster-level <register|none> (none NUR mit Begruendungssatz im Fliesstext) UND --stale-n <0-5> ODER --grund-stale-n "<Begruendung>" — unabhaengig von der Beinzahl (vollcheck.cjs nimmt beides auch bei 0 Beinen an; Pflicht werden sie erst bei >= 1 Bein bzw. erfuellter Stale-Vorbedingung, dann aber OHNE --grund-Ausweg — deshalb von Anfang an mitgeben, nicht erst nach dem Hard-Exit). --sl-anker: sobald >= 1 Bein gerichtet ist (15m-Close ungleich EMA50 bei NAS100 oder QQQ — der Normalfall, auch mit --richtung none) IMMER mitgeben: Struktur-Anker nach 7b1 Schritt 3a auf der SL-Seite der BEIN-Richtung (P1 prueft die Seite gegen das Bein; 01.10. VC#1 scheiterte genau daran); bei 0 Beinen (beide 15m-Beine neutral, keine Setup-Richtung) --sl-anker WEGLASSEN — ohne Richtung gibt es keine SL-Seite, der Wert wuerde nicht gelesen (geprueft 02.10., keine Wirkung auf P1/P2/P3/anker_auto_log), ein Dummy darf aber nie als anker_live in die Messreihe mitgeschleppt werden. Dasselbe gilt fuer VC#2. Vorab-Erfolgskriterium (Opus): 0 Hard-Exits in VC#1/#2 an den naechsten 3 Testtagen (Basis 4 in 3 Tagen).');
  // C5-Voraussetzung (Levi-Zusatzsatz seit 30.09., feedback_tagesabschluss.md 1a): Redirect-Pflicht je VC-Lauf — Quelle fuer scripts/loop_archiv.cjs.
  block.push(`REDIRECT-PFLICHT (C5, 02.10.2026): JEDER vollcheck.cjs-Aufruf laeuft im Bash-Tool (NICHT PowerShell: dort landet /tmp in C:\\tmp und loop_archiv.cjs findet nichts) woertlich nach dem Muster \`node scripts/vollcheck.cjs <Argumente> > /tmp/vc_${heute}_<N>.txt 2>&1; echo "EXIT=$?"; cat /tmp/vc_${heute}_<N>.txt\` (N = Voll-Check-Nummer; die Datei sichert auch Hard-Exit-Ausgaben, echo zeigt den Exit-Code, cat zeigt den Voll-Check — KEIN \`| tee\`, der Exit-Code ginge sonst verloren); eine Wiederholung nach Hard-Exit unter \`> /tmp/vc_${heute}_<N>b.txt 2>&1\` mit demselben Muster (Suffix b — NIE denselben Dateinamen ueberschreiben, 29.09. VC#17 ging so verloren; nur _N und _Nb werden eingelesen, eine dritte Form wie _Nc meldet loop_archiv.cjs als NICHT EINGELESEN); bei mehreren Loop-Starts am Tag fortlaufend nummerieren, nicht bei 1 neu beginnen. Diese Dateien sind die Quelle fuer \`node scripts/loop_archiv.cjs --datum ${heute}\` beim LOOP-STOPP; die cat-Ausgabe wird woertlich in den Chat uebernommen (sie IST der Voll-Check).`);
```

**Korrektur (a), Opus-Gegencheck 02.10.2026:** Der Redirect `> /tmp/vc_… 2>&1` versteckt die Ausgabe — deshalb ist das Aufrufmuster jetzt **fest** vorgegeben: `node scripts/vollcheck.cjs … > /tmp/vc_<D>_<N>.txt 2>&1; echo "EXIT=$?"; cat /tmp/vc_<D>_<N>.txt`. Kein `| tee` (Exit-Code ginge verloren). **Pflicht ist das Bash-Tool.** Unter PowerShell wird `/tmp` zu `C:\tmp` (am 02.10. beobachtet), `loop_archiv.cjs` liest aber `os.tmpdir()` = `C:\Users\<user>\AppData\Local\Temp` (= `/tmp` unter Git-Bash) und findet dann nichts. Rueckfall, falls doch einmal PowerShell-Dateien entstanden sind: `node scripts/loop_archiv.cjs --datum <datum> --vc-dir C:\tmp`.

## Aenderung 2 — Z. 236 (LOOP-STOPP-Zeile): `--protokoll <Datei>` → `--datum`, `loop_archiv.cjs` zwischen `loop_stopp.cjs` und `protokoll_bilanz.cjs`

Ersetze in der bestehenden `block.push('LOOP-STOPP (...)')`-Zeile den Teil

```
Exit 0 = keine Position: CronDelete + `node scripts/protokoll_bilanz.cjs --protokoll <Datei> --position-offen nein`.
```

durch

```
Exit 0 = keine Position: CronDelete + `node scripts/loop_archiv.cjs --datum <datum>` (C5, 02.10.2026: setzt scripts/loop_archiv/<datum>.txt = Tagesprotokoll aus /tmp/vc_<datum>_<N>[b].txt + Live-Gates aus gate_check_log.jsonl zusammen, inkl. Zeile "Exit-Code: <n>" je Live-Lauf; Exit 1 = Datei existiert schon oder keine VC-Datei -> Ausgabe lesen und im Abschluss melden, NIE --ueberschreiben (das ordnet nur Levi an); Nummernluecken/Hard-Exits/NICHT EINGELESEN aus der Ausgabe in den Abschluss uebernehmen; den Faktenprotokoll-Abschluss erst NACH loop_archiv.cjs an das Archiv anhaengen) + `node scripts/protokoll_bilanz.cjs --datum <datum> --position-offen nein` (seit 30.09.2026 --datum statt --protokoll <Datei>; liest genau das Archiv).
```

**Korrektur (c), Opus-Gegencheck 02.10.2026:** "ggf. --ueberschreiben nach Vergleich" ist aus dem LOOP-STOPP-Text gestrichen. Ueberschreiben gibt es nur auf ausdrueckliche Anweisung von Levi — am 30.09. traegt das Archiv einen manuell angehaengten Faktenabschluss, den ein Neulauf verlieren wuerde. Deshalb zusaetzlich der Satz "Faktenprotokoll-Abschluss erst NACH loop_archiv.cjs anhaengen" (loop_archiv.cjs legt die Datei an, der Abschluss kommt danach dazu; ein spaeterer Neulauf wird durch den Ueberschreibschutz Exit 1 abgewiesen). R2 (02.10.): die Fehlermeldung von `loop_archiv.cjs` bei existierendem Archiv sagt jetzt dasselbe ("Ueberschreiben ordnet nur Levi an …") und fordert nicht mehr zum `--ueberschreiben` auf — Skript und Prompt-Regel widersprechen sich nicht mehr.

**Korrektur (d), Opus-Gegencheck 02.10.2026:** Der Uebergabe-Hinweistext in `loop_stopp.cjs` Z. ~151 nennt weiterhin `--protokoll <Datei>`. `loop_stopp.cjs` wird bei der Aktivierung von C4/C5 **NICHT** mitgeaendert (zweites Live-Skript; Freeze: hoechstens eine Live-Aenderung pro Testtag). Korrektur erst nach der Endauswertung 08.10.2026 als eigener Schritt. Bis dahin ist die Zeile funktional korrekt: sie steht nur im Zweig "OFFENE POSITION: JA", und `protokoll_bilanz.cjs` akzeptiert `--protokoll <Pfad>` weiterhin (abwaertskompatibel).

## Aenderung 3 — Selbsttest-Fragmente (Z. 247)

```js
  for (const s of ['node scripts/vollcheck.cjs', '--state', '--testtag', 'ERSTER VOLL-CHECK DES TAGES (C4', `> /tmp/vc_${heute}_<N>.txt 2>&1`, 'node scripts/loop_archiv.cjs --datum <datum>']) if (!prompt.includes(s)) fehl.push(`"${s}" fehlt im Prompt (Template A / C4 / C5)`);
```

## Aktivierung (Levi-Entscheid 02.10.: ab Testtag 05.10.2026; Prompt-Aenderung NUR ausserhalb der Loop-Zeit 15:15 DE bis Loop-Stopp)

1. Die drei Aenderungen in `scripts/loop_prompt.cjs` einbauen (Fable) — ausserhalb der Loop-Zeit (vor 15:15 DE oder nach Loop-Stopp), `git diff --quiet HEAD` der uebrigen Live-Skripte belegen (nur `loop_prompt.cjs` aendert sich; `loop_stopp.cjs` NICHT, Korrektur (d)).
2. `node scripts/loop_prompt.cjs --testtag fiktiv` → Exit 0, Block „ERSTER VOLL-CHECK DES TAGES“ und „REDIRECT-PFLICHT“ sichtbar, LOOP-STOPP nennt `loop_archiv.cjs --datum`.
3. `npm test` (die P1-Tests in `tests/trading_scripts.test.js` pruefen nur Baustein-Anker/Template, kein Konflikt erwartet; ein Test auf die neuen Fragmente ist bei Aktivierung zu ergaenzen). Kein `test:e2e`.
4. **Opus-Kurzcheck des Prompt-Diffs** (`git diff HEAD -- scripts/loop_prompt.cjs` + Probe-Ausgabe aus Schritt 2) VOR der Aktivierung — Auflage 5 / R1 des Opus-Final-Gegenchecks 02.10.; erst nach Opus-OK gilt der Prompt als aktiviert (ab Testtag 05.10.).
5. Aktivierung ab Testtag 05.10.2026; Vorab-Erfolgskriterium beobachten: 0 Hard-Exits in VC#1/#2 an den naechsten 3 Testtagen.
6. Commit/Push nur durch Levi ([[feedback_commit_push_nur_levi]]).

**Why:** Levi hat ausdruecklich „Don't change a running system“ fuer den Cron-Testtag 02.10. verlangt und die Aktivierung auf den Testtag 05.10. gelegt; der Vorschlag haelt die exakten Zeilen fest, damit die Aktivierung keine Neuformulierung braucht. **How to apply:** Vor Testtag 05.10. (ausserhalb der Loop-Zeit) die drei Bloecke 1:1 einsetzen, Schritte 2-6 in dieser Reihenfolge (Prompt-Aenderung → `--testtag fiktiv`-Probe → `npm test` → Opus-Kurzcheck Diff → Aktivierung ab 05.10.).
