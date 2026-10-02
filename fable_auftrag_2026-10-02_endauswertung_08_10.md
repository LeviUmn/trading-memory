---
name: fable_auftrag_2026-10-02_endauswertung_08_10
description: "Opus-Auftrag an Fable 02.10.2026: Endauswertung auf 08.10.2026 vorbereiten (H1-Stichtag als dokumentierte Aenderung der Vorabregistrierung, Endauswertungs-Block in tagesmomente --auswertung), Messhygiene (protokoll_bilanz-Regex, Bars-Ende-Warnung, skipped terminalkurs, H1-Kleinmaengel B1/H-a), Loop-Betrieb (Start-Vorlage, loop_archiv-Automatik); DST-Fix zurueckgestellt; KEINE Regelaenderung; Fable setzt nur um, Commit/Push nur Levi"
metadata:
  type: project
  originSessionId: opus-auftrag-endauswertung-2026-10-02
  modified: 2026-10-02T08:24:57.612Z
---

# Fable-Auftrag 02.10.2026: Endauswertung 08.10.2026 + Messhygiene

**Grundlage:** Levi-Entscheidungen 02.10.2026 ([[project_endauswertung_08_10_vorgezogen_2026-10-02]]):
1. Der 01.10. zaehlt.
2. Die Endauswertung wird vom 30.10. auf den **08.10.2026** vorgezogen.
3. **Keine Regelaenderung jetzt.**

Quellen: [[opus_bericht_testtag_2026-10-01]] (H, Entscheidungen), [[opus_nachtrag_testtag_2026-10-01_1900-2000]], [[project_h1_q2_trendkontext_vorabkriterium_2026-10-01]], [[project_h1_kleinmaengel_offen_2026-10-01]], [[project_testtag_2026-09-23_besprechung_ausstehend]] Abschnitt 5.

**Rolle:** Fable setzt nur um. **Kein `git add`/commit/push** ([[feedback_commit_push_nur_levi]]). Am Ende melden: "bereit zum Commit" und warten.

**Zeitfenster:** Skripte, die der laufende Loop nutzt (`loop_prompt.cjs`, `vollcheck.cjs`, `gate_check.cjs`, `loop_stopp.cjs`), an Testtagen nicht zwischen 15:15 DE und Loop-Stopp aendern. Alle anderen Dateien dieses Auftrags laufen nur im Tagesabschluss.

Alle Zeilennummern hat Opus am 02.10. vormittags am Arbeitsbaum verifiziert (HEAD 7690d41).

---

## A. H1-Endauswertung auf 08.10.2026 (`scripts/analyse/h1_auswertung.cjs`, 664 Zeilen, committet in 7690d41)

**Einordnung:** Das ist eine **Aenderung der Vorabregistrierung**. K1-K10 und R6-a wurden am 01.10. mit dem Stichtag 30.10. festgeschrieben. Die Aenderung ist zulaessig, weil zwei Bedingungen erfuellt sind:
- Sie beruht auf Levis ausdruecklicher Entscheidung mit dokumentiertem Grund ("das reicht mir an Dauer"), wie Abschnitt 4 der H1-Datei es verlangt.
- Sie ist nicht ergebnisgetrieben: Bisher wurde nur der Zaehlstand gesehen, Zelle A hat n = 0, R-Werte von H1 wurden nicht gesichtet.

**K1-K10, die Checkliste 3.4, das Mindest-n aus 3.3 und R1-R7 bleiben inhaltlich unveraendert.**

**A1. Konstanten (Z. 105-109)**
- `ENDSTICHTAG = '2026-10-08'`; `ENDSTICHTAG_ABSCHLUSS_HM` bleibt `'20:05'`.
- `ENDSTICHTAG_VARIANTE` bekommt einen neuen Text: "Levi-Entscheid 02.10.2026: Stichtag vorgezogen von Fr 30.10.2026 (Levi-Entscheid (a) 01.10.2026) auf Do 08.10.2026 (Tagesabschluss), Aenderung der Vorabregistrierung, Grund Levi: 'das reicht mir an Dauer'; K1-K10 unveraendert; kein zweiter Ausloeser, keine Verlaengerung; Mindest-n verfehlt = H1 nicht belegt".
- Neue Konstante `ENDSTICHTAG_URSPRUENGLICH = '2026-10-30'`, nur fuer die Anzeige.

**A2. Pflichtzeile "Stichprobe kleiner als geplant"**
- Neue Zeile in **jedem** Echt-Lauf (`--zaehlstand`, `zwischen`, `end`; Text und `--json`):
  `STICHPROBE KLEINER ALS BEI REGISTRIERUNG GEPLANT: Stichtag 2026-10-08 statt 2026-10-30 (Levi 02.10.2026); gezaehlte Tage d=<n> (moeglich max. 6 Handelstage 01.-08.10. statt 22 bis 30.10.), Zelle A FLOOR n=<n>, AUTO n=<n>; Zaehlung ab 01.10.2026.`
- Die Zeile enthaelt **keine** R-Werte, Quoten oder Posterior.
- Im JSON kommt dafuer ein neuer Schluessel (z. B. `stichprobe_hinweis`) in die S8-Allowlist (Z. ~318-320). Die Zaehlstand-Schutztests muessen gruen bleiben.

**A3. Kopf- und Hinweistexte nachziehen**
Betroffene Stellen:
- Z. 13-15 (end-Zeile)
- Z. 22-25: Der Satz "Stichtag 30.10.2026 liegt sicher danach" ist jetzt **falsch** und wird ersetzt, siehe A5.
- Z. 31-39 (Stichtag-Absatz)
- Z. 112-114 (`HINWEIS_ECHT_AUSWERTUNG` / `HINWEIS_ECHT_ZAEHLSTAND`)

Dazu ein datierter Aenderungsvermerk im Kopf: "AENDERUNG 02.10.2026 (Levi): ENDSTICHTAG 30.10. -> 08.10.2026, Grund …, Kriterien unveraendert".

**A4. Selbsttest anpassen. Er bricht sonst an diesen Stellen:**
- **Z. 507:** Erwartet `2026-10-30`; neuer Sollwert `2026-10-08`.
- **Z. 558-564:** Uhrzeiten auf den neuen Stichtag umstellen:
  - 07.10. 23:59 und 08.10. 20:04 → nicht zulaessig
  - 08.10. 20:05, 09.10. 00:00 und 02.11. → zulaessig
  - MEZ/MESZ-Test: 08.10. ist MESZ, also 18:05Z = 20:05 DE zulaessig und 18:04Z nicht
  - Z. 562 nutzt "15.10. 16:00 = vor Stichtag"; das muss ein Datum vor dem 08.10. werden
  - Regexe `/2026-10-30 20:05 DE/` und `/Levi-Entscheid \(a\)/` an den neuen Text anpassen
- **Z. 586/589:** Der CLI-Test ist an die Systemuhr gebunden (bekannte Eigenschaft) und muss in beiden Zweigen gruen bleiben.
- **Z. 613-619, Divergenztag 27.10. im Echtmodus:** Dieser Test **bricht**, weil der 27.10. jetzt nach dem Stichtag liegt und ignoriert wird.
  - Umbauen, sodass die K5/K9-Fensterlogik weiter geprueft wird.
  - Vorzug: Den Stichtag ausschliesslich als **internen** Funktionsparameter injizierbar machen, analog `jetztMs` (kein CLI-Parameter, keine Umgebungsvariable, S5 bleibt).
  - Zusaetzlich ein neuer Test: Ein Moment vom 27.10. wird im Echtmodus als `nach_endstichtag` gezaehlt.
- **Z. 506 (e2 = 02.11.):** bleibt gueltig.

**A5. Kollisionen (Opus hat sie geprueft). Fable dokumentiert nur und aendert keine Kriterien.**

| Stelle | Kollision | Behandlung |
|---|---|---|
| 3.3 Mindest-n (≥ 15 unabh. Momente aus ≥ 6 Tagen, Zelle A) | Bis 08.10. gibt es hoechstens 6 Handelstage (01., 02., 05.-08.10.). Das Mindest-n ist nur erreichbar, wenn **jeder** Tag nach K9 zaehlt **und** Zelle A ≥ 15 Momente hat. Realistisch ist der Ausgang "zu selten, nicht belegt" (3.6), Q-ROT bleibt dann Veto. | Unveraendert lassen. In A2 und im Memory ausdruecklich vermerken. |
| Abschnitt 1 "Auswertung erst nach Freeze-Ende" | Bisher lag der 30.10. "sicher danach". Am 08.10. greift die Freeze-Abbruchregel nur, wenn 02., 05., 06., 07. und 08.10. alle bewertbar sind (dann 10/10). Faellt ein Tag aus, liegt der 08.10. **vor** dem Freeze-Ende. | Lesart nach Levi-Entscheid 2: Der Stichtag ist ein **fester Kalendertag** (wie R6-a: "die Uhr ist datenunabhaengig"). Das Skript prueft den Freeze-Stand weiterhin nicht. Im Kopf und im Memory steht: "Endauswertung 08.10. auch bei < 10 bewertbaren Tagen (Levi 02.10.); ob das so gemeint ist, bestaetigt Levi" (Rueckfrage R-1). |
| R6-a "Fortsetzung nur als neue Hypothese auf Daten ab 31.10.2026" | Folgeaenderung des Stichtags | Wird zu "ab 09.10.2026". Als Folgeaenderung vermerken, keine inhaltliche Aenderung. |
| K3 Haelften | Bei d ≤ 6 Tagen sind es 3 gegen 3 (bzw. ⌈d/2⌉) | Unveraendert. Hat eine Haelfte keinen Zelle-A-Moment, gilt "NICHT AUSWERTBAR" (R1). |
| K9 "tagesmomente-bewertbar" | Kopplung nur an Tagesdaten, nicht an den 30.10. | Unveraendert. Am 08.10. muessen Bars-Sicherung und `tagesmomente --datum 2026-10-08` **vor** dem end-Lauf stehen (A6). |
| DST-Randbedingung (H1-Datei Z. 155) | Die Woche 26.-30.10. liegt jetzt nach dem Stichtag | Fuer H1 obsolet. Vermerken; der DST-Fix bleibt aus anderen Gruenden noetig (C8). |
| Zwischenauswertung (R6) | Praktisch gegenstandslos (Schwelle vor dem 08.10. nicht erreichbar) | Unveraendert. |

**A6. Memory-Nachtrag**

Fable darf **nur** `memory/project_h1_q2_trendkontext_vorabkriterium_2026-10-01.md` ergaenzen. Nichts loeschen, nichts umformulieren, alter Text bleibt als Protokoll.
- Am Dateiende einen neuen Abschnitt "## 8. Aenderung der Vorabregistrierung 02.10.2026 (Levi)" anfuegen. Er enthaelt:
  - alt/neu-Stichtag, Grund, "K1-K10 unveraendert", "nicht ergebnisgetrieben (Zelle A n=0, keine R-Werte gesichtet)"
  - die Kollisionstabelle A5
  - den Ablauf am 08.10.: Loop-Stopp → Bars sichern (≥ 20:00) → `tagesmomente --datum` → skipped/kombi-Nachtraege → `tagesmomente --auswertung` → ab 20:05 `h1_auswertung --echt --auswertung end` (genau einmal) → Opus-Gegencheck → Besprechung mit Levi
- In den Zeilen 18, 142 und 165 (R6-a) **nur** einen Verweis-Zusatz "(geaendert 02.10.2026 → 08.10.2026, s. Abschnitt 8)" anhaengen.
- B1 erledigen: In Z. 163 die Zeilenzahl "651" durch "siehe `wc -l`" ersetzen bzw. als veraltet markieren. "untracked/nicht committet" ist ebenfalls ueberholt (7690d41).
- In `MEMORY.md` die H1-Indexzeile nach der Index-Konvention **ersetzen** (nicht anhaengen).

**Akzeptanz A**
- `node scripts/analyse/h1_auswertung.cjs --selbsttest` → alles gruen (Anzahl melden; vorher 89/89).
- `node scripts/analyse/h1_auswertung.cjs --echt --zaehlstand` → Exit 0, die STICHPROBE-Zeile erscheint, keine R-Werte. Erwartung ohne neue Testtage: d = 1 (2026-10-01), Zelle A n 0.
- `node scripts/analyse/h1_auswertung.cjs --echt --auswertung end` heute → Exit 1, die Meldung nennt `2026-10-08 20:05 DE`, stdout ist leer.
- `node scripts/analyse/h1_auswertung.cjs` (Probelauf) → die Probelauf-Zahlen sind unveraendert (FLOOR 17 gezaehlt; A 2 / B 7 / C 4 / D 4).

**Nicht-Ziele A**
- Keine Aenderung an `MIN_N_ZELLE`, `MIN_TAGE_ZELLE`, Prior, Checkliste, K-Konstanten, `K9_FENSTER_PRUEFEN`, Z_START.
- Keine Uebersteuerung per CLI oder Umgebungsvariable.
- **Kein** `--auswertung end`-Lauf vor dem 08.10. 20:05.

---

## B. Freeze-Ende / Endauswertungs-Rahmen (`scripts/tagesmomente.cjs`)

**Befund (Opus):**
- Der 30.10. steht in `tagesmomente.cjs` **nirgends** als Freeze-Datum. Die Freeze-Regel ist eine reine Bedingung: Z. 56 `FREEZE_TAGE_MIN = 5`, `FREEZE_AUTO_MIN = 20`, `FREEZE_TAGE_MAX = 10`; Z. 287-292 die Rechnung; Z. 312-315 die Zeilen `FREEZE-ENDE ERREICHT` und `Definition`.
- Der 30.10. steht nur in der H1-Datei bzw. im H1-Skript (Teil A).
- In `tagesmomente.cjs` Z. 234 steht 26.-30.10. als `DIVERGENZ_FENSTER`. Das ist der DST-Kalender und **nicht anfassen**.

**Stand 02.10. vormittags:**
- 5 bewertbare Tage (25., 28., 29., 30.09., 01.10.), AUTO 10/20, Abbruchregel 5/10.
- Mit Testtagen am 02., 05., 06., 07. und 08.10. greift die Abbruchregel exakt am 08.10. (10/10), aber nur, wenn alle fuenf Tage bewertbar sind.
- AUTO lieferte bisher etwa 2 unabhaengige Bewegungen pro Tag. 20 bis zum 08.10. sind unwahrscheinlich.

**B1. Neuer Block in `auswertung()`, direkt nach der Zeile `FREEZE-ENDE ERREICHT` (Z. 314)**
- Neue Konstante `ENDAUSWERTUNG_STICHTAG = '2026-10-08'` (Levi 02.10.2026).
- Rechnung nur ueber bewertbare Tage mit `datum <= ENDAUSWERTUNG_STICHTAG`, ab `FREEZE_ZAEHLBEGINN`.
- Ausgabe, woertlich und ehrlich:
  - `ENDAUSWERTUNG (Levi-Entscheid 02.10.2026, Stichtag 2026-10-08, vorgezogen von 30.10.): bewertbare Tage <n>/10, AUTO <n>/20`
  - `Freeze-Ende laut Definition 28.09.: erreicht regulaer | erreicht ueber Abbruchregel (10 Tage) | NICHT erreicht (<n>/10 Tage) — Endauswertung trotzdem zum Stichtag (Levi)`
  - Bei AUTO < 20: `AUTO-Kriterium NICHT erreicht (<n>/20) → Variante AUTO 'zu selten, nicht belegt'`
  - Je Variante: `Urteil nach Beleg-Regel (Ø R >= +0,10 UND Ø R ohne besten Tag >= 0): belegt | nicht belegt | nicht pruefbar`, bei nicht erreichtem Freeze-Ende mit dem Zusatz `— unter Vorbehalt: Freeze-Definition nicht erfuellt (Levi entscheidet)`
  - Vor dem 08.10. (letzter bewertbarer Tag < Stichtag): `Stand vor Stichtag, Endauswertung nach Tagesabschluss 08.10.2026`
- **Rein additiv:** Die Zeilen `FREEZE-ENDE ERREICHT` und `Definition` sowie die bestehenden Variantenzeilen bleiben **byte-identisch**. Der Block ist rein additiv.

**B2. Doku**
- In `memory/project_testtag_2026-09-23_besprechung_ausstehend.md` am Ende von Abschnitt 5 **eine** datierte Zeile **anhaengen**: "02.10.2026 (Levi): Endauswertung auf 08.10.2026 vorgezogen; Schwellen unveraendert; Ausgabe-Block ENDAUSWERTUNG in tagesmomente --auswertung". Nichts loeschen.
- `feedback_tagesabschluss.md` Punkt 3 bekommt den Zusatz "am 08.10. zusaetzlich: ENDAUSWERTUNG-Block woertlich ins Protokoll".
- `MEMORY.md`: Freeze-Indexzeile **ersetzen**.

**Akzeptanz B**
- `node scripts/tagesmomente.cjs --auswertung` → Exit 0. Die Zeile `FREEZE-ENDE ERREICHT: nein — … AUTO 10/20 (fehlen 10; Abbruchregel: 5/10 …)` ist unveraendert. Neu erscheint `ENDAUSWERTUNG … bewertbare Tage 5/10, AUTO 10/20`, `NICHT erreicht (5/10 Tage)`, `AUTO-Kriterium NICHT erreicht (10/20)` und `Stand vor Stichtag`.
- Vorher/nachher-Diff der Ausgabe darf nur die neuen Zeilen zeigen. Dazu die Ausgabe **vor** der Aenderung in `%TEMP%` sichern und danach `diff` laufen lassen. Kein `git stash` und kein Checkout.
- Synthetischer Test in `tests/tagesmomente.test.js` mit 10 bewertbaren Tagen bis 08.10. und AUTO 12 → "erreicht ueber Abbruchregel" + "AUTO-Kriterium NICHT erreicht". Ein Tag nach dem 08.10. zaehlt im Block nicht mit.

**Nicht-Ziele B:** Keine Aenderung an `FREEZE_*`, `BELEG_AVG_R`, `VC_MIN_TAG`, `LUECKE_MAX_BARS`, `FREEZE_ZAEHLBEGINN`, den Bewertbarkeitsregeln oder `DIVERGENZ_FENSTER`.

---

## C. Kleinmaengel (nur Messhygiene/Betrieb, keine Gate-Wirkung)

**C1. `protokoll_bilanz.cjs`: Regex fuer Live-gate_check-Aufrufe (messverfaelschender Bug)**

**Ursache:** Die Archivzeilen lauten `> node scripts/gate_check.cjs --jetzt … --testtag fiktiv --entry …` (loop_archiv/2026-10-01.txt Z. 2510/2566/2626/2694). Gesucht wird aber `/gate_check\.cjs --entry/`.

**Achtung, zwei Fallen (von Opus am Code geprueft):**
- (a) Die naive Form `gate_check\.cjs .*--entry` trifft **auch die 44 Vorpruefungs-Zitate** (`Aufruf: gate_check.cjs --sl-vorpruefung --sl-auto --entry …`). Damit ergaeben sich 48 statt 4 Aufrufe.
- (b) Opus' Vorschlag aus dem Vorbericht, `(?!.*--sl-vorpruefung)`, **schliesst die Live-Aufrufe ebenfalls aus**, weil sie `--sl-vorpruefung-ref` enthalten. Ergebnis wieder 0.

**Soll:** Eine gemeinsame Konstante, getestet von Opus auf einer In-Memory-Kopie:
`const GATE_LIVE_RE = /gate_check\.cjs\b(?!.*\s--sl-vorpruefung(?=\s|$))(?=.*\s--entry\s)/;`

Einsetzen an diesen Stellen:
- Z. 238 und Z. 273 (Pflicht)
- Z. 326, die Break-Bedingung der Vorpruefungsschleife: `--entry` darf dort nicht mehr nur direkt hinter `gate_check.cjs` erkannt werden; Live-Zeilen mit `--jetzt` vorne muessen ebenfalls abbrechen
- Z. 385 (Format-Verifikation `gate-check`)
- Doku-Kommentare Z. 22 und Z. 350-351 sowie der Meldungstext Z. 596 ("gate_check.cjs … --entry"-Kommandozeile)

**Akzeptanz C1 (Opus-Referenzwerte)**
`node scripts/protokoll_bilanz.cjs --datum <d> --position-offen nein`:

| Datum | vorher | nachher (Soll) |
|---|---|---|
| 2026-10-01 | Exit 1; "Aufrufe gesamt: 0"; 2 Abweichungen | **Exit 0**; "gate_check.cjs-Aufrufe gesamt: 4 — Status: FAIL 2 \| PASS 2"; Zeilen 2510/2566/2626/2694 mit Exit-Code 2/2/0/0, alle `--chasing yes`; 0 Abweichungen |
| 2026-09-30 | Exit 1 | unveraendert: Exit 1, 4 Aufrufe (PASS 1 \| nicht gefunden 2 \| FAIL 1), Abweichungen nur "Doppelte VC-Nummern 10, 34" und "Screenshot 58/57" |
| 2026-09-29 | Exit 0 | unveraendert: 3 Aufrufe, FAIL 2 \| PASS 1, Exit-Codes 2/2/0 |

Zusaetzlich einen Unit-Test in `tests/trading_scripts.test.js` (describe protokoll_bilanz) mit drei synthetischen Zeilen:
- Live-Zeile mit `--jetzt` vorne und `--sl-vorpruefung-ref` → zaehlt
- Vorpruefungs-Zitat `gate_check.cjs --sl-vorpruefung --sl-auto --entry` → zaehlt nicht
- Regime-Zeile `gate_check.cjs --tier chop --chop-flip` → zaehlt nicht

**C2. Warnung "Bars-Datei endet vor dem 20:00-Stichtag"** (Messsicherung; nur HINWEIS, Bewertbarkeit unveraendert)
- **`tagesmomente.cjs`:** In `barsStatus` (Z. 100-106) bzw. in der Konsolenausgabe (Z. ~356) eine eigene Zeile, wenn Ende der letzten Bar < `--bis`:
  `HINWEIS: Bars-Datei endet <HH:MM> DE vor Horizont <bis> DE — Bars per data_get_ohlcv nachholen und neu rechnen, sonst Tag nicht bewertbar`
  Das gilt auch, wenn die Luecke innerhalb von `LUECKE_MAX_BARS = 1` liegt (dann nur der Hinweis, Status bleibt `ok`).
- **`skipped_fiktiv.cjs`** (nach Z. 348/352) und **`kombi_fiktiv.cjs`** (nach Z. 262): Wenn `bars_bis` < Stichtag `bis`:
  `WARNUNG: Bars enden <HH:MM> DE vor Stichtag <HH:MM> DE — Ergebnis vorlaeufig (OFFEN = Artefakt des kurzen Horizonts); Bars nachholen, Nachtrag wiederholen`
  Kein neues Logfeld.
- **`protokoll_bilanz.cjs`:** Neue Option `--bars <Pfad>`, Default `scripts/nas100_5m_<datum>.json`. Neue **HINWEIS-Zeile** (keine ABWEICHUNG, kein Exit-Effekt):
  - fehlt die Datei: "Bars-Datei fehlt"
  - endet sie vor 20:00 DE: "endet HH:MM"
  - liegt der juengste loop_stopp-Eintrag vor 20:00 DE: "Loop-Stopp HH:MM vor 20:00 — die Messung endet nicht mit dem Loop: Bars bis 20:00 sichern (Opus-Bericht 01.10., H Prio 3)"

**Akzeptanz C2** (nur auf Kopien in `%TEMP%`, `--file`/`--dry-run` nutzen):
- `tagesmomente --datum 2026-10-01 --bars scripts/nas100_5m_2026-10-01.bis1900.bak.json --dry-run` → HINWEIS genau einmal, momente_log unberuehrt (sha1 vorher = nachher).
- Mit `nas100_5m_2026-10-01.json` (letzte Bar 17:55Z) → kein HINWEIS.
- protokoll_bilanz 01.10.: Bars-HINWEIS 0×, Loop-Stopp-HINWEIS 1× (19:01). Mit `--bars <bak>`: Bars-HINWEIS 1×. 30.09.: 0×.
- Exit-Codes wie in C1.

**C3. `skipped_fiktiv.cjs` Z. 330: Terminalkurs bei Neu-Nachtrag**
- **Ist:** `e.terminalkurs = barsCalc.exit === 'OFFEN' ? barsCalc.exitKurs : e.terminalkurs ?? null;`. Bei SL-HIT/TP1-HIT bleibt ein alter Wert stehen (01.10. 16:07/17:06: 30332,55 aus dem 19:00-Lauf).
- **Soll:** Mit `barsCalc` wird `terminalkurs` **immer** aus diesem Lauf gesetzt: bei OFFEN `exitKurs`, sonst der Close der letzten gewerteten Bar vor dem Stichtag (`rechneBars` muss ihn dafuer zurueckgeben, z. B. `lastClose`).
- `ergebnis_r`, `exit_*`, `mfe/mae` bleiben unveraendert.
- **Akzeptanz:**
  - Unit-Test: Eintrag mit altem `terminalkurs` und Bars mit SL-Hit → `terminalkurs` = letzter Close, `ergebnis_r` = −1.
  - Auf einer Kopie von `skipped_setups_fiktiv.jsonl` (`--file %TEMP%\…`) den Nachtrag fuer die 01.10.-Eintraege 16:07 und 17:06 wiederholen → `terminalkurs` ≠ 30332,55 (Erwartung: Close der 19:55-Bar = 30531,85), `ergebnis_r` −1,00 unveraendert.
  - **Das echte Log wird nicht neu geschrieben.** Ob der Restwert im echten Log korrigiert wird, entscheidet Levi.

**C4. Loop-Start-Vorlage (`scripts/loop_prompt.cjs`): Hard-Exit beim ersten VC verhindern**
- **Muster:** 4 Loop-Start-Hard-Exits in 3 Tagen (29.09. VC#17, 30.09. VC#1+#2 `--stale-n`, 01.10. VC#1 `--sl-anker`/`--cluster-level`).
- **Soll:** Nach dem `--state`-Absatz (Z. 198) ein Block "ERSTER VOLL-CHECK DES TAGES". Er verlangt, dass VC#1 **immer** `--sl-anker <Anker>` und `--cluster-level <register|none>` traegt, `none` mit Begruendungssatz, dazu `--stale-n <0-5>` oder `--grund-stale-n "<Begruendung>"`, unabhaengig von der Beinzahl.
- Falls `vollcheck.cjs` diese Werte bei 0 Beinen ablehnt: **nicht** den Code lockern, sondern im Block den richtigen Weg nennen und an `vollcheck.cjs` pruefen. Das Ergebnis meldet Fable.
- Dazu die Redirect-Pflicht fuer jeden VC-Lauf: `> /tmp/vc_<datum>_<N>.txt 2>&1`, Hard-Exit-Dateien mit Suffix `b`. Das ist heute Teil von Levis Zusatzsatz und Voraussetzung fuer C5.
- Selbsttest-Fragment in der `selbsttest`-Liste ergaenzen.
- **Vorab-Erfolgskriterium (Opus):** 0 Hard-Exits in VC#1/#2 an den naechsten 3 Testtagen (Basis 4 in 3 Tagen).
- **Akzeptanz:** `node scripts/loop_prompt.cjs --testtag fiktiv` → Exit 0, der Block erscheint, die bestehenden Selbsttests sind gruen.

**C5. `loop_archiv/<datum>.txt` automatisch erzeugen (Auftrag B-Rest)**
- Neues Skript `scripts/loop_archiv.cjs --datum YYYY-MM-DD [--vc-dir <Pfad>] [--out-dir <Pfad>] [--dry-run] [--ueberschreiben]`. Vorlage ist `C:\Users\umnus\.claude\jobs\9f4b9f35\tmp\assemble_archiv.cjs` (33 Zeilen, hart auf 2026-10-01).
- Vorgaben:
  - Datum parametrisieren; VC-Dateien `vc_<datum>_<N>[b].txt` aus `os.tmpdir()` (Default)
  - Live-Gates aus `gate_check_log.jsonl` mit `modus === 'live'`, je Lauf `> node scripts/gate_check.cjs <argv>` + output + `[stderr]` + **`Exit-Code: <n>`** (wird von `protokoll_bilanz.cjs` Z. 259 geparst)
  - Format exakt wie in der Vorlage
  - Am Ende: Nummernluecken und Hard-Exit-Dateien melden
- **Schutz:** Existiert das Archiv schon, gibt es ohne `--ueberschreiben` Exit 1, und nichts wird geschrieben. Die bestehenden `loop_archiv/*.txt` sind tabu (siehe D).
- `loop_prompt.cjs` Z. 236 (LOOP-STOPP): nach `loop_stopp.cjs` und vor `protokoll_bilanz.cjs` den Aufruf `node scripts/loop_archiv.cjs --datum <datum>` einfuegen. Der LOOP-STOPP-Text nennt bisher `--protokoll <Datei>`; korrekt ist seit 30.09. `--datum <datum>`, mitkorrigieren.
- `feedback_tagesabschluss.md` Abschnitt 1a: einen Satz **anhaengen** ("seit <Datum> automatisch per loop_archiv.cjs; manueller Weg bleibt Rueckfall").
- **Akzeptanz:**
  - Wenn `%TEMP%\vc_2026-10-01_*.txt` noch existieren: `node scripts/loop_archiv.cjs --datum 2026-10-01 --out-dir %TEMP%\la_test` → Datei **byte-identisch** zu `scripts/loop_archiv/2026-10-01.txt` (2758 Zeilen); `fc /b` bzw. `cmp` melden.
  - Wenn die Dateien fehlen: synthetischer Test mit 3 VC-Dateien + 1 Hard-Exit + 2 Live-Gates.
  - Ohne `--out-dir` auf dem bestehenden 01.10. → Exit 1, Archiv unveraendert (sha1).

**C6. h1_auswertung H-a: Kopf und Selbsttest**
- Z. 4-5 "SCHREIBT NICHTS" praezisieren: "schreibt nie echte Logs; nur `--selbsttest` schreibt synthetische Logs in ein Temp-Verzeichnis und startet Kindprozesse".
- Den S8-Block Z. 568-593 in `try { … } finally { fs.rmSync(tmp, { recursive: true, force: true }); }` fassen.
- **Akzeptanz:** Selbsttest gruen; danach kein `h1_selbsttest_*`-Verzeichnis in `%TEMP%` (vorher/nachher zaehlen).

**C7. Pruefauftrag (KEINE Aenderung): H1-Trendzelle am 01.10.**
- Alle 4 FLOOR-Momente des 01.10. landen in "Trend nein".
- **Vorbefund Opus** aus `oneh_shadow_log.jsonl`, nur Bias-Felder, keine R-Werte: VC#1-#7 (15:26-15:56) haben `bias['1h'] = 'long'` (der letzte geschlossene 1H-Bar 14:00 lag ueber der EMA50), ab VC#8 (16:01) `short`.
  - Nach Trendkontext-Bedingung 1 / K6 ("`bias['1h']` aller Shadow-Eintraege von Session-Start bis zum Moment == dir") ist deshalb **jeder** Short-Moment des Tages "Trend nein". Das ist definitionsgemaess korrekt.
- **Fable prueft am Code** (`trendkontext()`), ob genau dieser Grund greift: Zuordnung, Grundtext, Bedingung 1 gegen Bedingung 2. Gemeldet werden **nur Zuordnungsgruende und Zaehlungen** (Sichtungsverbot: keine R-Werte, keine Zellenergebnisse).
- Den Befund als Messgrenze in Abschnitt 8 der H1-Datei vermerken: "Trendtage, die gegen den 1H-Stand der Eroeffnung laufen, fallen per Definition aus Zelle A". **Keine Kriterienaenderung**; das ist Material fuer die Besprechung nach dem 08.10.

**C8. DST-Fix `vollcheck.cjs` (Z. 750-751 `orderSperre`/`halbierung`, hart 15/16 DE): ZURUECKGESTELLT**
- Nicht in diesem Auftrag.
- Begruendung: kein Bezug zum 08.10. und zur H1-Endauswertung (siehe A5); die Mengenbremse gilt.
- Der Fix bleibt faellig, **bevor** ein Loop im Fenster 26.-30.10. laeuft. Opus schreibt bis spaetestens Mo 19.10. einen eigenen Auftrag ([[project_vollcheck_dst_fix_todo_2026-10]]). Die dortige Zeilenangabe "Z. 401-404" ist veraltet und steht jetzt bei Z. 750-751.

---

## D. Reihenfolge, Verifikation, Rueckmeldung, Nicht anfassen

**Reihenfolge**
1. **C1** (unabhaengig, sofort; heutiger Tagesabschluss profitiert)
2. **A** (inkl. C6, gleiche Datei; danach C7 als Lesepruefung)
3. **B**
4. **C2**, **C3**
5. **C4** + **C5**: Diese aendern den Loop-Prompt. Nur ausserhalb der Loop-Zeit umsetzen; aktiv erst ab dem Testtag, den Levi nennt (Freeze: hoechstens eine Live-Aenderung pro Testtag; C4+C5 gelten zusammen als eine Betriebsaenderung, keine Gate-Wirkung).

Abhaengigkeiten:
- C5 braucht die Redirect-Konvention aus C4.
- B1 und A2 sind voneinander unabhaengig.
- Der end-Lauf am 08.10. braucht A + B + den Tagesabschluss (A6).

**Verifikation (sicher)**
- `npm test`: seit f81d818 nur Offline-Suiten. **NIE `npm run test:e2e`**, kein `node --test tests/` ohne Pfadliste, kein `tests/e2e.test.js` ([[feedback_npm_test_destruktiv_incident_2026-09-17]]).
- `node scripts/analyse/h1_auswertung.cjs --selbsttest`
- `node scripts/loop_prompt.cjs --testtag fiktiv` (nur Ausgabe)
- Die Akzeptanzbefehle aus A-C. Alles, was Logs schreibt (`tagesmomente` ohne `--dry-run`, `skipped_fiktiv`/`kombi_fiktiv --nachtrag`), **nur** mit `--file` auf eine Kopie in `%TEMP%`.
- Vor und nach den Tests sha1 von `momente_log.jsonl`, `skipped_setups_fiktiv.jsonl`, `kombi_fiktiv_log.jsonl`, `gate_check_log.jsonl` und `loop_archiv/*.txt` vergleichen und melden (muss identisch sein).

**Rueckmeldeformat (an Levi/Opus)**
1. Je Datei: was geaendert, Diff-Zusammenfassung (Zeilen ±, Funktionen)
2. Testergebnisse (Befehl → Exit/Anzahl), inkl. der Tabelle C1 und den sha1-Vergleichen
3. Befund C7 (nur Gruende und Zaehlungen)
4. Abweichungen vom Auftrag mit Grund
5. Offene Punkte
6. Zum Schluss woertlich: **"bereit zum Commit"**, ohne `git add`/commit/push

**Nicht anfassen**
- **Regelwerk-Logik:** Q-ROT, Q-Score/Q1-Q4, Gates (`gate_check.cjs`-Logik, 7b1, 8b/8c/8c2, Dual-Gate, 1H-Override, 13.1, Punkt 11), alle Schwellen und Konstanten
- **H1-Kriterien:** K1-K10, Checkliste, Mindest-n, Prior
- **Freeze-Schwellen:** `FREEZE_*`, `BELEG_AVG_R`
- **Laufende Logs:** `momente_log.jsonl`, `skipped_setups_fiktiv.jsonl`, `kombi_fiktiv_log.jsonl`, `gate_check_log.jsonl`, `oneh_shadow_log.jsonl`, alle `*.bak`/`*.bis1900.bak*`
- **Tagesdaten:** `/tmp`- bzw. `%TEMP%`-Dateien `vc_*`, `scripts/loop_archiv/*`, `scripts/nas100_5m_2026-10-01.json` (+ bak)
- **Kurzblock-Bereinigung** in `feedback_live_trading.md` Z. 116-141 (nicht Teil dieses Auftrags, Levi-Freigabe offen)

---

## E. Gegencheck-Punkte (Opus nimmt nach Umsetzung ab)

1. **H1-Stichtag:** `ENDSTICHTAG = '2026-10-08'`; end-Sperre greift heute (Exit 1, stdout leer), ab 08.10. 20:05 DE frei (Selbsttest mit injizierter Uhr); K-Konstanten, Mindest-n und Checkliste per Diff unveraendert.
2. **Dokumentation der Aenderung:** Skriptkopf **und** Abschnitt 8 der H1-Datei tragen Datum, Grund, "Kriterien unveraendert" und die Kollisionen (A5). Alter Text unveraendert erhalten (Diff nur additiv bis auf B1).
3. **Stichprobe-Zeile:** erscheint in allen drei Echt-Modi (Text + JSON); die S8-Allowlist und die Zaehlstand-Schutztests sind gruen; keine R-Werte in der Zeile.
4. **Divergenztag-Test umgebaut:** Der Stichtag ist nur intern injizierbar, kein CLI/ENV-Weg; Daten vom 27.10. werden im Echtmodus als `nach_endstichtag` gezaehlt.
5. **tagesmomente --auswertung:** Bestehende Zeilen byte-identisch; ENDAUSWERTUNG-Block zeigt 5/10, AUTO 10/20, "NICHT erreicht", "AUTO-Kriterium NICHT erreicht"; der synthetische Test fuer die Abbruchregel ist gruen.
6. **protokoll_bilanz:** 01.10. Exit 0 mit 4 Aufrufen (2/2/0/0); 29./30.09. unveraendert; Vorpruefungs-Zitate zaehlen nicht; Bars-/Loop-Stopp-HINWEIS ohne Exit-Effekt.
7. **Log-Integritaet:** sha1-Vergleich aller laufenden Logs und Archive vorher = nachher; C3 nur auf Kopie belegt; `loop_archiv.cjs` reproduziert den 01.10. byte-identisch (oder synthetisch) und verweigert das Ueberschreiben.
8. **Betrieb:** Der Loop-Prompt enthaelt den VC#1-Block, die Redirect-Pflicht und `loop_archiv.cjs` im LOOP-STOPP (mit `--datum`); `npm test` ist gruen; kein test:e2e; kein Commit/Push.

---

## Rueckfragen an Levi (vor bzw. waehrend der Umsetzung)

- **R-1:** Gilt die Endauswertung am 08.10. auch dann, wenn bis dahin weniger als 10 bewertbare Tage vorliegen (Freeze-Ende laut Definition also nicht erreicht)? Opus-Lesart: ja, fester Kalendertag. Die Variantenurteile stehen dann "unter Vorbehalt".
- **R-2:** Ehrliche Erwartung fuer H1 am 08.10.: Mit hoechstens 6 Handelstagen ist das registrierte Mindest-n (Zelle A ≥ 15 aus ≥ 6 Tagen) praktisch nicht erreichbar. Nach C7 faellt jeder Trendtag gegen den Eroeffnungs-1H-Stand ohnehin aus Zelle A. Ergebnis wird sehr wahrscheinlich "zu selten, nicht belegt".
  - Eine Q-ROT-Aenderung danach ist dann **keine** H1-Folge, sondern eine neue Levi-Entscheidung auf duenner Datenbasis.
  - Das sollte in der Besprechung so benannt werden.
- **R-3:** Ab wann sollen C4/C5 (Loop-Prompt) aktiv werden (02.10. oder 05.10.)?
- **R-4:** Soll Fable den terminalkurs-Restwert im echten `skipped_setups_fiktiv.jsonl` (01.10. 16:07/17:06) per Neu-Nachtrag korrigieren? Opus empfiehlt nein (ohne R-Wirkung, Log nicht anfassen).
