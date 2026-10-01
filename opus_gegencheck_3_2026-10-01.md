---
name: opus_gegencheck_3_2026-10-01
description: "Dritter Opus-Gegencheck 01.10.2026: Nachpruefung S1-S4/F1-F4/H9 (h1_auswertung.cjs, H1-Datei 7a, Testdatei), Empfehlung Stichtag R6-a"
metadata:
  node_type: memory
  type: project
  originSessionId: 9f4b9f35-da7a-456c-9a16-094c4f451adb
  modified: 2026-10-01T10:47:56.367Z
---

# Opus-Gegencheck 3 (01.10.2026): Nachbesserungen S1–S4, F1–F4, H9

Alles lief read-only. Experimente und Mutanten nur in `%TEMP%\opus_gegencheck3` (wt, wt_h9, logs, synth, mut). Die echten Logs (15× `scripts/*.jsonl` + `scripts/trades.db`) sind vorher und nachher sha1-identisch (16/16). Echte Momentdaten ab 01.10.: **0 Zeilen**, nur gezählt (momente 351/0, shadow 519/0 ab 01.10. DE, kombi 6/0).

## Gesamturteile

1. **h1_auswertung.cjs: commitfähig JA (Commit 2).**
   - S1–S4 sind umgesetzt, die Statistik ist unverändert (diff gegen die Gegencheck-2-Kopie), 62/62, und der Probelauf ist unabhängig reproduziert.
   - Nichts blockiert den Commit.
   - **Vor dem ersten `--echt --auswertung`-Lauf** (nicht vor dem Commit) sind aber die Auflagen **S5–S8** nötig. Grund: Das Skript schützt den Auswertungsplan R6 an der entscheidenden Stelle nicht. `--auswertung end` ist jederzeit beliebig oft möglich und liefert volle R-Werte (Befund N1).
2. **H1-Datei (Abschnitt 7/7a): FREIGEGEBEN MIT 1 AUFLAGE (F5).**
   - K1–K10 sind unverändert.
   - (ii) und R7 sind inhaltlich deckungsgleich mit dem Skript.
   - R1/R2/R4/H8/H9 sind korrekt vermerkt.
   - Aber: R6 führt „Stichtag = Freeze-Ende laut Regel“ in der Spalte „Verbindliche Festlegung (Levi)“. Das hat weder Opus empfohlen noch Levi entschieden (Befund N2).
3. **Gesamtpaket: commitreif.**
   - Commit 1 ist unverändert seit Gegencheck 2, plus H9.
   - Commit 2 kann folgen. S5–S8 und F5 können als Folgecommit kommen, müssen aber vor dem ersten `--echt --auswertung` erledigt sein. Commit/Push nur durch Levi.
   - Exakte Liste:
     - **Commit 1 (Code-Repo):** `scripts/gate_check.cjs`, `scripts/vollcheck.cjs`, `scripts/loop_prompt.cjs`, `tests/trading_scripts.test.js`
     - **Commit 2:** `scripts/analyse/h1_auswertung.cjs`
     - **Memory-Repo, nur lokal, KEIN Push (Repo öffentlich):** `MEMORY.md`, `feedback_live_trading.md`, `opus_vorschlag_2026-09-30.md`, `opus_antwort_d_qrot_2026-10-01.md`, `opus_bericht_testtag_2026-09-30.md`, `opus_gegencheck_auftrag_b_h1_2026-10-01.md`, `opus_gegencheck_2_2026-10-01.md`, `opus_gegencheck_3_2026-10-01.md`, `project_h1_q2_trendkontext_vorabkriterium_2026-10-01.md`, `project_testtag_2026-09-30_abschluss.md`
     - **NICHT:** `scripts/loop_archiv/`, `scripts/last_gate_check_1942*_exit1.txt`, `scripts/analyse/backtest_2026-09-2{4,5}.cjs`

## Testergebnis

- Eigene Vollläufe mit `node --test tests/trading_scripts.test.js tests/tagesmomente.test.js` auf der Kopie `wt`:
  - Lauf 1: **227/227**
  - Lauf 2: **227/227** (je ~97 s)
- Fables 3 Läufe (`%TEMP%\fable_s1s4_runs`): je 227/227, bestätigt. `--selbsttest`: **62/62**.
- H9-Skip-Pfad selbst ausgelöst (Kopie, `USERPROFILE`/`HOME` ins Leere): `t.diagnostic` erscheint, Test pass. Bei erreichbarer Datei kein Unterschied.

## Fable-Checkliste 1–10

| # | Urteil | Beleg |
|---|---|---|
| 1 S1 | **erfüllt** | `h1_auswertung.cjs:97-113`. 31 Umgehungsversuche, alle Exit 1: `--echt nein/false/1/ja/TRUE`, `--echt=true`, `"--echt "`, `--ECHT`, `--foo`, `bar`, `""`, `-`, `--`, `--json wert`, `--momente-log` ohne Pfad, `--auswertung` falsch/ohne `--echt`, `--zaehlstand`+`--auswertung`. Echtmodus nur mit `--echt` und `--echt true`. Einziges Durchrutschen: `--echt nein --echt` (letztes Flag gewinnt), harmlos. |
| 2 S2 | **erfüllt** | 0 Treffer „Levi-Entscheid ausstehend“. K1–K10-Kommentare Z. 46-72 tragen „Levi 01.10.2026 freigegeben“, K7 ist korrekt „offen“. |
| 3 S3 | **erfüllt, 1 Lücke** | Z. 77-83, 230-242, 251, 261, 330-331. Randfälle synthetisch geprüft: nur UNBEKANNT → BESTANDEN + Vermerk, getrennt ausgewiesen; ROT ohne Nachtrag → NICHT AUSWERTBAR; Einträge nach dem letzten gezählten Tag werden ignoriert und gezählt; der Pflichtvermerk steht in Zeile, Punkt 5 und Gesamturteil. **Lücke (N3):** Das Datum wird nur als String verglichen. Ein Kombi-Eintrag mit `datum "01.10.2026"` landet im **Probelauf** (`"01.10.2026" < "2026-10-01"`) samt r_primaer im Bericht. Für momente gilt dasselbe. Real liegen aktuell 357/357 im ISO-Format vor, es gibt also keinen aktuellen Schaden. |
| 4 S4 | **teilweise** | Text und JSON von `--echt --zaehlstand` sind auf synthetischen 01.10.+-Logs frei von R/Treffer/Posterior/Urteil, ebenso `--auswertung zwischen` unter der Schwelle (JSON: 0 verbotene Schlüssel). **Mutationen** (12 auf Kopien): 8 gefangen (u. a. avg-Feld, zwischen-immer-zulässig, UNBEKANNT-als-ROT, Kombi-Obergrenze, Join `kand[0]`, `--echt <Wert>`). **NICHT gefangen:** (Mb) Feld `p_quote_ueber_50` im Zählstand-JSON, (Mc) R-Mittel unter anderem Feldnamen, (Me) Posterior-Wert `0,500` im Zählstand-Text, (Mf) `...out` im Zählstand-JSON in `main()`. Mf **leckt per CLI nachweislich** `urteil`, `checkliste` und `sum_r_primaer`. Ursache: Der Selbsttest prüft eine Schlüssel-Blacklist und testet `main()` gar nicht. Die Text-Regex fängt negative Werte nicht (`−` Unicode, Ausgabe nutzt ASCII `-`). |
| 5 Abw. | s. unten | |
| 6 F4/R7 | **erfüllt** | Skript Z. 27-30 und H1-R7 stimmen inhaltlich überein. Die H1-Datei ergänzt „/nicht numerisch“, was `Number.isFinite` entspricht. Selbsttests Z. 455-462: Join-Rückfall (letzter ≤ Moment, 16:05/2,4), bias null, abstand fehlt. Divergenztag E2E Z. 464-469. |
| 7 Integrität | **erfüllt** | Probelauf auf Kopien: „Zeilen gesamt: momente 351, shadow 519, kombi 6“, außerhalb 0/0/0 ✓. sha1 16/16 identisch. 0 Schreib-/spawn-Aufrufe im Skript. |
| 8 Nachrechnung | **erfüllt** | Eigener Code (`eigen3.cjs`, feste MESZ, exakte Binomialsumme) reproduziert **alle** Zellen. FLOOR gezählt 17: A 2/Σ 0,00, B 7/−1,00/Ø −0,143/ohne besten −0,20/P 0,402, C 4/+2,17/Ø 0,542/ohne +0,52, D 4/−2,35/P 0,133. AUTO 8: A 1/+1,00/P 0,623, B 3/+1,00/P 0,613, C 2/−0,20/P 0,274, D 2/−0,18/P 0,274. Kombi ROT 0,23+0,05+1,61 = **+1,89**, UNBEKANNT −0,10. 8 gezählte Tage identisch. |
| 9 Statistik | **erfüllt** | `lgamma/betacf/betaI/betaPosterior`, alle Konstanten `BETA_PRIOR/KRIT_/MIN_N` sowie `statistik/haelften/trendkontext/tageInfo` sind identisch zur GC2-Kopie (`%TEMP%\opus_gegencheck2\wt`). Die einzigen `<`-Zeilen im diff betreffen Kopf, Kombi, Ausgabe, parseArgs und exports. Zusätzlich 5 P-Werte per Binomialsumme gleich (s. 8). |
| 10 Freeze | **erfüllt** | `gate_check.cjs` (8140580e), `vollcheck.cjs` (934f8837) und `loop_prompt.cjs` (dd6d09be) sind CR-bereinigt hash-gleich zur GC2-Kopie. mtimes 10:50/11:08/11:45 liegen vor GC2. loop_prompt-Hunks gegen HEAD: 201, 204–207, 216, alle bereits in GC1/GC2 geprüft. Testdatei gegen die GC2-Kopie: **nur** Z. 5560 (`(t) =>`) und Z. 5580 (`else t.diagnostic(...)`). A1/A2 sind unverändert vorhanden. |

## Neue Befunde nach Schwere

**Blockierend:** keine.

**Auflagen:**

- **N1 (R6 im Code nicht gestützt, wichtigster Punkt):** `--echt --auswertung end` ist jederzeit und beliebig oft zulässig (`auswertungsArt` Z. 282 `zulaessig: true`, „Stichtag wird NICHT geprüft“). Es druckt volle R-Werte, Posterior und Checkliste, auch unter Mindest-n (synthetisch belegt: Zelle A n 8 → Σ/Ø/P sichtbar). Damit ist „höchstens eine Zwischen- und eine Endauswertung“ reine Disziplinsache, und genau der Weg des optional stopping (`end` heute, morgen, …) bleibt offen. Fables Abweichung 1 (`--echt` verlangt einen Modus) schließt nur den Unfall-Pfad.
- **N2 (H1-Datei R6):** „Stichtag = Freeze-Ende laut Regel“ steht als „Verbindliche Festlegung (Levi 01.10.2026)“. Opus hat das nicht empfohlen (GC2: „z. B. 30.10.2026 oder nach 12 gezählten Tagen“), Levi hat „Opus' Empfehlungen 1:1“ freigegeben. Der Satz ist eine Fable-Interimsregel und steht wortgleich im Skript (Z. 24, 282, 335). Die Restunklarheit R6-a ist zwar korrekt benannt, die Tabellenzeile überzieht aber.
- **N3 (Datumsformat):** Fehlende ISO-Prüfung (s. Punkt 3). Ein nicht-ISO-Datum ab 01.10. würde im Probelauf **gesichtet**. Das untergräbt den Integritätsschutz, praktisch unwahrscheinlich, Fix ist 1 Zeile.
- **N4 (Selbsttest S4 zu schwach):** Mutanten Mb/Mc/Me/Mf werden nicht gefangen (s. Punkt 4).

**Hinweise:**

- H9-Text sagt „ÜBERSPRUNGEN, **nicht bestanden**“, der Test ist aber pass. Besser „nicht geprüft“.
- `HINWEIS_ECHT` („erst NACH dem Freeze-Ende vorgesehen“) wird auch bei `--zaehlstand` gedruckt. Laut H1-Datei ist das jederzeit zulässig, die Meldung widerspricht sich also.
- „Zeilen gesamt (gültiges JSON)“ zählt momente erst nach dem String-Filter `datum`/`ts` (Z. 492), kaputte Zeilen fallen still weg. Real 351 = 351.
- „AUTO weicht im Vorzeichen ab: JA“ auch bei FLOOR-Ø = 0,00 (`Math.sign(0)`, Probelauf).
- Weitere Ausschlussgründe außerhalb von R7 („dir fehlt“, „kein Shadow-Log/-Eintrag“): konservativ, ausgewiesen, aber nicht in R7.
- H1-Datei Abschnitt 1 („Auswertung erst nach Freeze-Ende“) vs. R6 („Zwischenauswertung, sobald 15/6 erreicht“): klarstellen, dass auch die Zwischenauswertung frühestens nach Freeze-Ende kommt. Praktisch ohnehin, s. R6-a.
- **Betrieb (wichtig für R6-a):** Im DST-Fenster 26.–30.10. verlangt K9 `session_start 14:30`, K5 verlangt Shadow ab ≤ 14:45. Startet der Loop dort wie gewohnt 15:30, fallen **alle** Momente dieser Woche für H1 aus (K5). Der Selbsttest Z. 466-469 bestätigt die Mechanik.

**MEMORY.md (Codepoints):** H1-Zeile Z. 7 = **198** ✓ (A6 erledigt), Z. 8 = 177 ✓. Weiter > 200: Index-Zeilen **Z. 9 (Testtag 30.09., 216)** und **Z. 19 (Freeze, 208)**, beide vom 30.09. abends (uncommittet), nicht von heute. Z. 3 (Kopf, 269) und Z. 85 (Altblock, 829) sind keine Index-Zeilen und stehen so in HEAD. **Keine Zeile von heute zu lang.**

## Bewertung der Fable-Abweichungen

Fables Liste für Abweichung 2–4 lag mir nicht vor. Bewertet habe ich die im Code erkennbaren Abweichungen.

1. **`--echt` verlangt `--zaehlstand` oder `--auswertung`: BEHALTEN.**
   - Kein Nutzen geht verloren (`--selbsttest` bleibt frei).
   - Jeder Echt-Lauf muss seinen Zweck deklarieren, der Bericht trägt die Art im Kopf, und ein versehentlicher Voll-Lauf ist ausgeschlossen.
   - Er schließt aber N1 nicht, s. S5.
2. **`--echt true` akzeptiert:** akzeptabel.
3. **`zwischen` unter Schwelle → Zählstand (Exit 0):** akzeptabel, besser als Exit 1, keine R-Werte.
4. **`--zaehlstand` im Probelauf, „Zeilen gesamt“:** akzeptabel.
5. **Interimsregel „Stichtag = Freeze-Ende“:** **nicht akzeptabel als Festlegung**, s. N2/F5. Als Platzhalter mit „offen“-Markierung ginge sie.

## Empfehlung R6-a (Stichtag der Endauswertung), für Levi

**Empfehlung: fester Kalenderstichtag Fr 30.10.2026. Letzter einbezogener Tag ist der 30.10., ausgewertet wird beim Tagesabschluss. Der Stichtag ist ausdrücklich NICHT das Freeze-Ende und hat keinen zweiten Auslöser („12 Tage“).**

Begründung:
- **Freeze-Ende ist als Stichtag ungeeignet.** Die Regel (Freeze-Datei Abschnitt 5: ≥ 5 bewertbare Tage ab 25.09. UND AUTO ≥ 20, sonst Abbruch nach dem 10. bewertbaren Tag, Schätzung ca. Do 08.10.) ist eine datengetriebene Bedingung und kein Datum. Ab 01.10. lägen bis dahin höchstens ~6 Tage. Zelle A verlangt ≥ 15 aus ≥ 6 Tagen, die einzige Endauswertung wäre also sicher „zu selten“ und damit verbraucht. Der Druck, danach „noch etwas zu verlängern“, entstünde nach Sicht der R-Werte, und genau das ist das optional stopping, das R6 verhindern soll.
- **30.10.** entspricht Opus' Zeitplan „Ende Oktober“, ist datenunabhängig und liegt sicher nach dem Freeze-Ende. Damit gilt „Auswertung erst nach Freeze“ automatisch.
- **Kein „oder 12 gezählte Tage“.** Ein zweiter Auslöser verkürzt nur das n und schafft eine zweite Zählgröße.
- **Ehrliche Erwartung (nur Häufigkeit aus Altdaten, keine Ergebnisse):** Im Probelauf hatte Zelle A 2 Momente an 8 gezählten Tagen. Bei ähnlicher Rate liefert der 30.10. (≤ ~21 Handelstage) eher ~5 als 15. H1 endet dann voraussichtlich mit „zu selten, nicht belegt“. Deshalb **jetzt und vorab** entscheiden:
  - **(a)** Am 30.10. ist Schluss, H1 gilt bei Mindest-n-Verfehlung als nicht belegt. Eine Fortsetzung wäre nur als neue, neu pre-registrierte Hypothese auf Daten ab 31.10. möglich.
  - **(b)** Eine einmalige, **heute** festgelegte Verlängerung bis Fr 27.11.2026 als letzte Endauswertung. Dann darf `end` am 30.10. unter Mindest-n **keine R-Werte** zeigen (S6).
  - Ich empfehle **(b)**, weil die Datenlage es braucht und die Regel vorab steht.
- **Voraussetzung:** Im DST-Fenster 26.–30.10. startet der Loop **14:30 DE** (sonst K5-Ausfall der ganzen Woche). Der DST-Fix in vollcheck ([[project_vollcheck_dst_fix_todo_2026-10]]) muss vor dem 26.10. erledigt sein.

## Restauflagen (Fable-Auftrag, kein Gate-/Live-Code, Freeze unberührt)

1. **F5 (H1-Datei):** In der R6-Zeile von Abschnitt 7a „Stichtag = Freeze-Ende laut Regel“ aus der Spalte „Verbindliche Festlegung“ entfernen. Bis zu Levis Entscheid steht dort „Stichtag: OFFEN (R6-a), kein `--echt --auswertung` vor Festlegung“, danach Levis Datum und Variante (a)/(b). Abschnitt 1 ergänzen: Die Zwischenauswertung kommt ebenfalls frühestens nach Freeze-Ende.
2. **S5 (Skript, R6 hart):** Konstante `ENDSTICHTAG` (null bis Levi-Entscheid). `--auswertung end` → Exit 1, wenn `ENDSTICHTAG` null ist oder der letzte gezählte Tag bzw. das heutige DE-Datum < `ENDSTICHTAG` ist. Kopf- und Ausgabetexte Z. 24, 282, 335 an F5 angleichen. Selbsttests: end vor Stichtag → Fehler, ab Stichtag → ok.
3. **S6 (nur bei Levi-Variante (b)):** `end` am ersten Stichtag unter Mindest-n gibt wie `zwischen` nur den Zählstand plus „ZU WENIG DATEN, Verlängerung bis <Datum> vorab festgelegt“ aus, ohne R-Werte. Der finale Stichtag gibt immer den Vollbericht.
4. **S7 (Datumsformat, N3):** In `auswerten()` und `main()` nur `datum` gemäß `/^\d{4}-\d{2}-\d{2}$/` zulassen. Abweichende Zeilen werden gezählt und als „ungültiges Datum, ausgeschlossen“ ausgewiesen, im Probelauf **ohne Inhalt**. Selbsttest mit `"01.10.2026"`.
5. **S8 (Selbsttest S4 härten, N4):**
   - Zählstand-JSON per **Allowlist** der Schlüssel prüfen statt per Blacklist.
   - Ein Test, der `main()` per `spawnSync` mit `--echt --zaehlstand --json` und mit `--echt --auswertung zwischen --json` (unter Schwelle) auf synthetischen Temp-Logs aufruft und verbotene Schlüssel sowie Zahlformate `[+-]\d,\d\d` ausschließt (ASCII-Minus!).
   - Ziel: Die Mutanten Mb/Mc/Me/Mf werden gefangen.
6. **Kosmetik (optional):** H9-Text „nicht bestanden“ → „nicht geprüft“; `HINWEIS_ECHT` im Zählstand-Modus anpassen; AUTO/FLOOR-Vorzeichen bei Ø = 0 „nicht prüfbar“; Hälften „Hälfte 1/2“ statt „H1/H2“ (Kollision mit Hypothese H1); MEMORY.md Z. 9/19 ≤ 200.
7. Danach ein kurzer Opus-Gegencheck nur für S5–S8/F5. Der erste `--echt --auswertung`-Lauf erst danach. `--echt --zaehlstand` ist bis dahin zulässig.

**Nicht geprüft:** `backtest_*`-Skripte; `feedback_live_trading.md` außerhalb von B2a/B5; ein dritter eigener Volllauf (2 eigene + 3 Fable-Läufe grün).
