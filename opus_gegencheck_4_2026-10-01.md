---
name: opus_gegencheck_4_2026-10-01
description: "Vierter Opus-Gegencheck 01.10.2026: Nachpruefung des abgebrochenen Fable-Auftrags S5/S7/S8/F5 + Hinweise a-d (h1_auswertung.cjs, H1-Datei, Testdatei); Ergebnis VOLLSTAENDIG, 1 Kleinstkorrektur"
metadata:
  node_type: memory
  type: project
  originSessionId: 9f4b9f35-da7a-456c-9a16-094c4f451adb
  modified: 2026-10-01T13:09:16.182Z
---

# Opus-Gegencheck 4 (01.10.2026): Fable-Auftrag S5–S8 / F5 / Hinweise a–d

Alles lief read-only. Experimente, Mutanten und Testläufe liefen nur auf Kopien in `%TEMP%\opus_gegencheck4` (`logs`, `mut`, `wt`). Die echten Logs (15× `scripts/*.jsonl` + `trades.db`) sind vorher und nachher sha1-identisch (**16/16**) und gleich dem Stand von Gegencheck 3. Momentdaten ab 01.10. wurden nur gezählt: momente 351/**0**, shadow 519/**0**, kombi 6/**0**, nicht-ISO 0. Es lief kein Hintergrund-Testprozess (nur MCP-Server-/xurl-node-Prozesse von 10:07/10:14, nichts beendet).

## 1. Gesamturteil

**Fable-Auftrag VOLLSTÄNDIG umgesetzt.** Alle neun Soll-Punkte sind erfüllt und funktional nachgewiesen. Gefunden habe ich nur eine sachliche Kleinstungenauigkeit in der H1-Datei (Zeilenzahl, s. B1) und einige nicht blockierende Hinweise.

Fables letzte Aussage „651 Zeilen“ ist überholt. Die Datei hat jetzt **664 Zeilen, LF**. Das Skript wurde um 13:23:03 nochmals geändert, also nach Fables Lauf 1 (13:22:35) und während Lauf 2. Lauf 3 fand nie statt. Für den Endstand gelten deshalb meine Läufe, nicht Fables: Selbsttest, 2 Vollläufe und 27 Mutanten, alle auf dem aktuellen Stand.

## 2. Soll-Punkte 1–9

| # | Status | Beleg |
|---|---|---|
| 1 F5 | **erledigt** | **Skript:** 0 Treffer „Freeze-Ende laut Regel“ außer in den Selbsttest-Negativprüfungen Z. 564/565. Der Kopf Z. 29-44 nennt „R6-a, ENTSCHIEDEN — Levi-Entscheid (a) vom 01.10.2026“, 30.10.2026, kein Freeze-Ende, kein 2. Auslöser, keine Verlängerung. Konstante `ENDSTICHTAG_VARIANTE` steht in Z. 109.<br>**H1-Datei:** R6-Zeile Z. 142, Abschnitt S5–S8/F5 Z. 143 und R6-a Z. 165 sind „ENTSCHIEDEN“. Abschnitt 1 Z. 17 enthält die Klarstellung „auch die Zwischenauswertung frühestens nach Freeze-Ende“. Die DST-Randbedingung (26.–30.10., Loop 14:30, vollcheck-DST-Fix vorher) steht in Z. 155, korrekt als „NOCH NICHT ERLEDIGT“ markiert. Frontmatter Z. 3 ist aktualisiert. |
| 2 N1/S5 | **erledigt** | `ENDSTICHTAG='2026-10-30'`, `ENDSTICHTAG_ABSCHLUSS_HM='20:05'` (Z. 108). `endFreigabe()` Z. 346-351: Lesart „DE-Datum > Stichtag ODER == Stichtag und ≥ 20:05“, dokumentiert in Z. 35-38 und H1 Z. 142, mit Begründung „Uhr statt letzter gezählter Tag“. Die Sperre greift in `main()` **vor dem Lesen der Logs** (Z. 639) und nochmals in Z. 651. Der Vermerk „Endauswertung nur einmal …“ steht in Z. 110, 357 und 411. Das Echt-Fenster ist zusätzlich ≤ 30.10. (Z. 235/243). Die Uhr ist nur über den Funktionsparameter injizierbar.<br>**Umgehungsversuche, alle Exit 1 mit 0 B stdout:** `--jetzt`, `--now`, `--stichtag`, `--auswertung=end`, `END`, `--echt true`, `TZ=Pacific/Kiritimati`, `TZ=UTC`, Env `ENDSTICHTAG`, `H1_JETZT`, `NODE_ICU_DATA`, `LANG=C`. Die Zeitzone kommt über `Intl … timeZone:'Europe/Berlin'`, kein `TZ=`.<br>**Selbsttests:** 01.10. 12:00 / 29.10. 23:59 / 30.10. 20:04 → gesperrt. 30.10. 20:05 / 31.10. 00:00 / 02.11. → frei. 30.10. 18:05Z (19:05 MEZ) gesperrt, 19:05Z frei. Die DST-Umstellung 25.10. ist damit korrekt berücksichtigt. |
| 3 N3/S7 | **erledigt** | `DATUM_ISO` Z. 111. `istIso` und `imFenster` in `auswerten()` Z. 234-243. Der Filter in `main()` Z. 644 prüft nur noch `ts`, das Datum wird in `auswerten()` geprüft und gezählt. Ausweisung „Datum nicht ISO … ausgeschlossen“ in Z. 376. Selbsttests Z. 491-502 und CLI Z. 574-590: „01.10.2026“ und Zahl-Datum werden nicht gesichtet, auch nicht mit `--details`. |
| 4 N4/S8 | **erledigt** | Allowlist Z. 317-340, angewandt auf Objekt und CLI-JSON. `spawnSync` auf `main()` Z. 567-593: `--zaehlstand --json`, `zwischen --json`, Text, `end`, Probelauf, `--echt` ohne Modus. Die Regex Z. 343 deckt ASCII-Minus, Unicode-Minus und Plus ab, außerdem `x,xxx` und `%`.<br>**Eigene Mutation:** 12 GC3-Mutanten (Ma–Ml inkl. Mb/Mc/Me/**Mf**) plus 15 neue (N1–N15): **27/27 gefangen**. |
| 5a H9 | **erledigt** | Testdatei Z. 5580 „nicht geprueft“. Der Diff gegen die GC3-Kopie (CR-bereinigt) ist **genau diese eine Zeile**. |
| 5b | **erledigt** | `HINWEIS_ECHT_ZAEHLSTAND` / `_AUSWERTUNG`, `hinweisEcht()` Z. 113-115, Verwendung Z. 367/648/652. Die Zählstand-Ausgabe sagt „jederzeit zulaessig“. |
| 5c | **erledigt** | Z. 297-299: bei Ø = 0 → „nicht vergleichbar (Ø FLOOR = 0,00 hat kein Vorzeichen)“. Der Probelauf zeigt das jetzt statt „JA“. |
| 5d | **erledigt** | `haelften()` liefert `n_haelfte1/avg_haelfte2`, Ausgabe „Haelfte 1/Haelfte 2“ (Z. 284). Kein anderer Verbraucher der alten Schlüssel (grep). |
| 6 Integrität | **erledigt** | `gate_check` 8140580e, `vollcheck` 934f8837, `loop_prompt` dd6d09be: CR-bereinigt hash-gleich zu GC3, mtimes 10:50/11:08/11:45 unverändert. `tagesmomente.test.js` ist identisch.<br>**Statistik:** `lgamma`/`betacf`/`betaI`/`betaISimpson`/`betaPosterior`/`statistik`/`trendkontext`/`tageInfo` und alle K-/KRIT-/BETA-/MIN-Konstanten sind diff-gleich zur GC3-Kopie. `haelften` unterscheidet sich nur durch die Umbenennung. Die P-Werte im Probelauf sind gleich GC3 (0,402/0,133/0,623/0,613/0,274).<br>**K1–K10-Tabelle** (H1 Z. 117-126, sha1 der Zeilen 84c9acdf…) gegen den Auftrag-B-Bericht programmatisch verglichen (Umlaute normalisiert): K1/4/5/6/9/10 wortgleich. K2/K3 weichen nur durch Backticks ab (`exit_art`, `r_rr1`), K7/K8 durch Zusatzsätze ohne Bedeutungsänderung. Das ist der vorbestehende, in GC3 als „unverändert“ akzeptierte Stand; eine Fable-Änderung ist nicht erkennbar. Einschränkung: Es gibt keinen GC3-Snapshot der H1-Datei für einen Byte-Vergleich.<br>**MEMORY.md:** mtime 12:12, also vor GC3; von Fable nicht angefasst. |
| 7 Funktion | **erledigt** | `--selbsttest` **89/89**, Exit 0, Temp-Verzeichnis wieder gelöscht.<br>**Probelauf auf Kopien:** FLOOR gezählt 17; A 2 / B 7 / C 4 / D 4; AUTO 1/3/2/2; Kombi ROT 3 Σ +1,89; UNBEKANNT 1; ZU WENIG DATEN. 0 Momente ab 01.10. ausgegeben.<br>**S1-Versuche weiter Exit 1:** `--echt nein/false`, `--echt=true`, `--ECHT`, `--foo`, `bar`, `--json wert`, `--momente-log` ohne Pfad, `--auswertung` ohne `--echt`, `--echt` ohne Modus, `--zaehlstand`+`--auswertung`.<br>`--echt --zaehlstand` sowie `zwischen --json` liefern 0 verbotene Schlüssel und keine R-Werte. |
| 8 Tests | **erledigt** | 2 eigene Vollläufe (Kopie `wt`): **227/227** und **227/227** (je ~101 s). |
| 9 Halbfertig | **erledigt** | `node --check` ok für alle 5 Dateien. Kein Mischen von CRLF und LF innerhalb einer Datei: h1, Testdatei und H1-Datei sind reines LF; gate/vollcheck/loop_prompt sind reines CRLF, wie ausgecheckt (`autocrlf=true`, HEAD-Blobs LF). Keine Debug-Reste und keine TODO. `git status` ist exakt der erwartete Stand, keine Temp-Dateien im Repo. |

## 3. Befunde nach Schwere

**Blockierend:** keine.

**B1 (Kleinstkorrektur, Memory):** H1-Datei Z. 163 (R3) nennt „**651 Zeilen**“, tatsächlich sind es **664**. Rein dokumentarisch.

**Hinweise (nicht blockierend, kein Auftrag nötig):**
- **H-a:** Der Kopfkommentar Z. 4-5 sagt „SCHREIBT NICHTS (… noch sonstwo)“. `--selbsttest` schreibt seit S8 aber synthetische Logs nach `os.tmpdir()` und startet Kindprozesse (Z. 16-17 dokumentiert das). Das ist ein Widerspruch im Kopf. Außerdem fehlt ein `try/finally`: Wirft der Block, bleibt das `h1_selbsttest_*`-Verzeichnis liegen. Echte Logs sind nie betroffen.
- **H-b (systemisch, nicht behebbar):** Die Sperre S5 lässt sich umgehen über (1) die Systemuhr und (2) `require('…/h1_auswertung.cjs').auswerten({…, modus:'echt'})`, denn `auswerten` wird exportiert. Beides ist bewusster Regelbruch, kein Unfallpfad. R6 bleibt in diesem Rest Disziplinsache, ebenso „`zwischen` nur einmal“.
- **H-c:** Der CLI-Selbsttest „Sperre vor dem Lesen“ läuft nur bis 30.10. 20:05 (systemuhrabhängig). Danach prüft er den Freigabepfad. Die Uhrlogik bleibt über die injizierten Tests abgedeckt.
- **H-d:** Der Text-Selbsttest blendet Zeilen mit „HINWEIS:“ aus. Ein Leck genau in einer Hinweiszeile würde er nicht fangen. Theoretisch, kein Mutant dazu gefunden.
- **H-e (Betrieb, bekannt):** DST-Woche 26.–30.10. Der Loop muss um 14:30 starten und der vollcheck-DST-Fix muss vorher erledigt sein (H1 Z. 155). Sonst fällt die letzte Woche vor dem Stichtag per K5 komplett aus.

## 4. Testergebnisse

- `--selbsttest`: **89/89** (Fable-Vorstand 62/62).
- Vollläufe `node --test tests/trading_scripts.test.js tests/tagesmomente.test.js`: **2× 227/227**. Fables Läufe 1/2 (`%TEMP%\fable_gc3_nachbesserung`) waren ebenfalls 227/227; Lauf 2 überlappt die letzte Skriptänderung, betrifft aber keine Testdatei.
- **Mutationen** (`%TEMP%\opus_gegencheck4\mut`): **27/27 gefangen**.
  - GC3 Ma–Ml: 12/12, darunter die früher unentdeckten Mb, Mc, Me und Mf.
  - Neu, 15/15:
    - Stichtagsprüfung: end immer frei, `>=` statt `>`, beide main-Sperren entfernt, UTC statt DE-Zeit
    - Datum und Fenster: ISO-Prüfung entfernt, Echt-Fenster ohne Obergrenze, Probefenster bis 31.10.
    - Hinweise: Hinweis-Tausch, Ø-0-Vergleich entfernt, Vermerk „nur einmal“ entfernt
    - Zählstand: zwischen unter Schwelle als Vollbericht, Kombi-Σ im Zählstand-JSON/-Text/main-JSON
    - Hälften: `n_h1`-Schlüssel
  - Fables eigene Mutantenliste (16/16) habe ich nur gelesen, bestätigt durch meine Läufe.
- sha1 echte Logs vorher = nachher: **16/16**.

## 5. Restauflagen

Kein neuer Fable-Auftrag nötig. Optional, beim nächsten ohnehin anstehenden Edit:
- (1) H1-Datei Z. 163: „651“ → „664 Zeilen“.
- (2) Skript-Kopf Z. 4-5 mit Z. 16-17 in Einklang bringen („schreibt nichts außer den Selbsttest-Temp-Dateien“) und den Selbsttest-Block in `try/finally` setzen.

Beides ändert weder Statistik noch Auswertungsregeln. Der erste `--echt --auswertung`-Lauf ist aus Sicht dieser Prüfung freigegeben. `end` ist technisch ohnehin erst ab 30.10.2026 20:05 DE möglich.

## 6. Commit-Reife (Commit/Push nur durch Levi)

| Paket | Datei(en) | Reife |
|---|---|---|
| **Commit 1 (Code-Repo)** | `scripts/gate_check.cjs`, `scripts/vollcheck.cjs`, `scripts/loop_prompt.cjs`, `tests/trading_scripts.test.js` | **commitreif.** Stand GC3 plus die eine H9-Textzeile; 2× 227/227. |
| **Commit 2** | `scripts/analyse/h1_auswertung.cjs` | **commitreif** (89/89, 27/27 Mutanten). Optional H-a vorher. |
| **Memory-Repo (nur lokal, KEIN Push, Repo öffentlich)** | wie GC3-Liste + `opus_gegencheck_4_2026-10-01.md`; `project_h1_q2_…` | **commitreif**, optional B1 vorher. MEMORY.md wurde von Fable nicht angefasst; die Index-Zeile für GC4 setzt Levi bzw. die Hauptsession. |
| **NICHT committen** | `scripts/loop_archiv/`, `scripts/last_gate_check_1942*_exit1.txt`, `scripts/analyse/backtest_2026-09-2{4,5}.cjs` | unverändert untracked |

**Nicht geprüft:**
- `backtest_*`
- Byte-Vergleich der H1-Datei gegen den GC3-Stand (kein Snapshot vorhanden; ersatzweise Vergleich mit der Quelle Auftrag B)
- ein dritter eigener Volllauf
