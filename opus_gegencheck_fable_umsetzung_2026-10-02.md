---
name: opus_gegencheck_fable_umsetzung_2026-10-02
description: "Opus-Gegencheck 02.10.2026 der Fable-Umsetzung 'Endauswertung 08.10. + Messhygiene' (E1-E8): E1-E7 ABGENOMMEN (E5 mit Auflage), E8 MIT AUFLAGE (C4/C5-Prompt bewusst nicht eingebaut, Levi: ab 05.10.); alle Zahlen selbst nachgerechnet; Logs sha1 identisch; kein Live-Skript geaendert; Commit freigegeben JA (selektives git add, nur Levi); 6 Auflagen an Fable, davon 2 vor 08.10. Pflicht; Vorschlag C4/C5 nur mit 4 Korrekturen einsatzreif"
metadata:
  type: project
  originSessionId: opus-gegencheck-fable-umsetzung-2026-10-02
  modified: 2026-10-02T09:02:26.466Z
---

# Opus-Gegencheck 02.10.2026 — Fable-Umsetzung "Endauswertung 08.10.2026 + Messhygiene"

Quellen: [[fable_auftrag_2026-10-02_endauswertung_08_10]] (E = Pruefpunkte, D = Verifikation), [[fable_umsetzung_2026-10-02_endauswertung_08_10]], [[fable_vorschlag_c4_c5_loop_prompt_2026-10-02]], `git diff` am Arbeitsbaum (HEAD 7690d41). Alle Befehle selbst ausgefuehrt (02.10. ca. 10:55-11:30 DE). Arbeitskopien lagen nur in `%TEMP%\opus_gc_0210`, echte Logs blieben unberuehrt. Keine Code-/Log-/Memory-Aenderung ausser dieser Datei, kein git add/commit/push, kein test:e2e.

## Ergebnis je Punkt

**E1 H1-Stichtag: ABGENOMMEN.**
- Der Diff zeigt `ENDSTICHTAG = '2026-10-08'`, `ENDSTICHTAG_URSPRUENGLICH = '2026-10-30'` (nur Anzeige) und den neuen Text in `ENDSTICHTAG_VARIANTE`.
- Unveraendert laut Diff (grep auf +/- Zeilen: 0 Treffer): MIN_N_ZELLE, MIN_TAGE_ZELLE, K-Konstanten, Prior, Checkliste, K9_FENSTER, Z_START, trendkontext/statistik/haelften/tageInfo.
- `--selbsttest`: Exit 0, **93 OK, 0 Fehler**. Danach liegen 0 `h1_selbsttest_*`-Verzeichnisse in %TEMP%.
- `--echt --auswertung end` heute: **Exit 1, stdout 0 Bytes**. stderr nennt "zulaessig erst ab 2026-10-08 20:05 DE".
- Freigabe ab 08.10. 20:05 ist per injizierter Uhr getestet: 07.10. 23:59 nein, 08.10. 20:04 nein, 20:05 ja, MESZ 18:04Z/18:05Z.
- Probelauf mit HEAD-Kopie verglichen: Der Diff zeigt nur Label und Pfadschreibweise. Werte unveraendert: FLOOR gezaehlt 17, A 2 / B 7 / C 4 / D 4.

**E2 Dokumentation: ABGENOMMEN.**
- Der Skriptkopf enthaelt den Vermerk "AENDERUNG 02.10.2026" mit Grund, "K1-K10 ... UNVERAENDERT", "nicht ergebnisgetrieben" und den Kollisionen (1)-(7).
- Der Satz "sicher danach" ist ersetzt, die Begruendung steht dabei.
- H1-Datei: Abschnitt 8 ist vollstaendig (alt/neu, Grund, Zulaessigkeit, Kollisionstabelle A5, C7-Messgrenze, Ablauf 08.10., Stand).
- Laut `git diff --word-diff` ist alles **rein additiv**: nur `{+…+}`, kein geloeschtes Wort ausser dem `modified`-Stempel. Die Verweis-Zusaetze stehen 3x. "651 Zeilen" ist erhalten und als veraltet markiert (B1).

**E3 STICHPROBE-Zeile: ABGENOMMEN.**
- Sie steht in `--zaehlstand` (Text Z.4, JSON `stichprobe_hinweis`) und in `zwischen`. Der end-Pfad ist per Selbsttest S8 abgedeckt.
- Inhalt: d=1, Zelle A FLOOR n=0, AUTO n=0. Keine R-Werte. Die Allowlist ist erweitert, die Zaehlstand-Schutztests sind gruen.
- Abweichung "Zaehlung ab 2026-10-01" (ISO statt 01.10.2026): **vertretbar**. Der S7-Leckschutz zaehlt "01.10.2026"-Vorkommen, und ISO ist ohnehin das kanonische Format.

**E4 Divergenztag-Test: ABGENOMMEN.**
- `auswerten({ …, endstichtag = ENDSTICHTAG })`; `main()` uebergibt keinen Stichtag.
- Es gibt kein `process.env` im Skript, und `OPTIONEN_BEKANNT` ist ohne `endstichtag`. Ein Test belegt: `--endstichtag` ergibt "unbekannte Option".
- Neuer Test: Der 27.10. zaehlt im Echtmodus als `nach_endstichtag` (momente 21, kombi 1).

**E5 tagesmomente --auswertung: ABGENOMMEN MIT AUFLAGE (Auflagen 1+2).**
- Vergleich HEAD-Kopie gegen neu (`diff`): **genau 7 neue Zeilen**, alle bestehenden Zeilen byte-identisch.
- Inhalt der neuen Zeilen: "bewertbare Tage 5/10, AUTO 10/20", "NICHT erreicht (5/10 Tage)", "AUTO-Kriterium NICHT erreicht (10/20)", 3x "nicht belegt — unter Vorbehalt", "Stand vor Stichtag".
- Die synthetischen Tests sind gruen: Abbruchregel 10 Tage + AUTO 12; der Tag 09.10. zaehlt im Block nicht.
- Abweichung bei der Block-Position (nach der Definitionszeile statt zwischen FREEZE und Definition): **vertretbar**. Die Definition ist die eingerueckte Fortsetzung der FREEZE-Zeile, und so bleibt der Diff rein additiv.
- Die beiden Schwaechen (Auflagen 1+2) stammen aus meiner Spezifikation, nicht von Fable.

**E6 protokoll_bilanz: ABGENOMMEN.** HEAD-Kopie (Sandbox-Kopie von scripts/) gegen neu:

| Datum | HEAD | neu |
|---|---|---|
| 01.10. | Exit 1, 0 Aufrufe, 2 Abweichungen | **Exit 0**, 4 Aufrufe FAIL 2 / PASS 2, Zeilen 2510/2566/2626/2694, Exit-Codes 2/2/0/0, alle `--chasing yes`, 0 Abweichungen |
| 30.09. | Exit 1 | Exit 1 unveraendert, 4 Aufrufe (PASS 1 / nicht gefunden 2 / FAIL 1); Diff nur die neue Bars-Sicherungs-Zeile |
| 29.09. | Exit 0 | Exit 0 unveraendert, 3 Aufrufe; Diff nur die neue Bars-Sicherungs-Zeile |

- Regex-Pruefung, alt/neu/naiv ueber alle 4 Archive und `testtag_2026-09-01…10/15.md`:
  - Die neue Regex unterscheidet sich von der alten **nur** an den 4 Live-Zeilen vom 01.10.
  - 0 Falschtreffer: Vorpruefungs-Zitate 23/56/44/47 je Tag werden nicht gezaehlt; die naive Form haette 26/62/48/47 gezaehlt.
  - 0 Verpasser.
- **25.-28.09.:** Es gibt kein Protokoll (kein loop_archiv, kein testtag-md), deshalb ist dort keine Pruefung moeglich.
- Restgrenze: `\s--entry\s` verlangt ein Leerzeichen nach `--entry`. Die Form `--entry=` wird nie verwendet, deshalb unkritisch.
- C2-Hinweise:
  - 01.10.: Loop-Stopp-HINWEIS 1x (19:01), Bars-HINWEIS 0x.
  - Mit `--bars …bis1900.bak.json`: Bars-HINWEIS 1x (endet 19:05). Exit 0 bleibt, also kein Exit-Effekt.
  - tagesmomente `--dry-run` mit bak: HINWEIS 1x; mit voller Datei 0x.
  - Die neuen barsStatus-Felder landen **nicht** im momente_log (Eintrag nutzt nur `bs.status`/`bs.grund`).

**E7 Log-Integritaet: ABGENOMMEN.**
- sha1 vorher = nachher (`diff` leer) nach allen meinen Laeufen inkl. npm test. Geprueft wurden: momente_log f88c4caf, skipped 283168ec, kombi 56021432, gate_check_log 92e2c767, oneh_shadow 51ea2f09, nas100_5m_2026-10-01 1626a30f, loop_archiv 09-23/29/30/10-01 (5a4ea99e/b8ec7696/c1275ae5/d7cdf6ee) und alle `*.bak*`.
- C3 auf einer Kopie (`--file`): 16:07 und 17:06 haben jetzt terminalkurs **30531.85** (vorher 30332.55), ergebnis_r -1 und SL-HIT bleiben unveraendert. Mit den Bak-Bars kommt die WARNUNG 1x. Kombi-WARNUNG auf Kopie: bak 1x, voll 0x.
- `loop_archiv.cjs --datum 2026-10-01 --out-dir %TEMP%`: `cmp` **IDENTISCH** (2758 Zeilen, 45 VC-Dateien, 1 Hard-Exit, 4 Live-Gates).
- Ohne `--out-dir`: Exit 1 "existiert bereits", Archiv-sha1 unveraendert.
- Gegenprobe 30.09.: Das Ergebnis weicht nur um die 4 Zeilen `Exit-Code:` (gibt es erst seit dem 01.10.-Format) und um den manuell angehaengten Sonnet-Faktenabschluss ab. Erwartet, kein Fehler.

**E8 Betrieb: MIT AUFLAGE (bewusst offen nach Levi-Entscheid R-3 "ab 05.10.").**
- Der VC#1-Block, die Redirect-Pflicht und `loop_archiv.cjs` im LOOP-STOPP sind **nicht** im Prompt. Richtig fuer heute.
- `npm test` (nur Offline-Suiten laut package.json): Exit 0, **412/412 pass**, 67 Suites. Kein test:e2e, kein Commit/Push durch Fable (git status: nur Arbeitsbaum).

## Heutiger Cron-Testtag (15:30 DE) — unberuehrt

- `git hash-object` gegen `HEAD:` ergibt **gleich** fuer loop_prompt, vollcheck, quick_tick, gate_check, x_fetch_stamp, loop_stopp, quote_check, register_check/constants/touch.
- Kein Live-Skript `require`t oder spawnt eines der geaenderten Skripte. vollcheck spawnt nur x_fetch_stamp, register_check, register_touch und gate_check.
- `node scripts/loop_prompt.cjs --testtag fiktiv`: Exit 0, Ausgabe sha1 4b61c1be. Der Prompt ist identisch zu gestern, weil die Datei = HEAD ist. Er enthaelt 0x "ERSTER VOLL-CHECK"/"loop_archiv.cjs".
- Die geaenderten Skripte (protokoll_bilanz, tagesmomente, skipped/kombi_fiktiv) laufen nur im Tagesabschluss bzw. Nachtrag. Der heutige Abschluss profitiert von C1 (sonst wieder 0 Aufrufe / Exit 1).
- **loop_stopp.cjs Z.~151** (nennt `--protokoll <Datei>`): fuer heute **kein Problem**.
  - Die Zeile steht nur im Zweig "OFFENE POSITION: JA", und am fiktiven Testtag gibt es keine echte Position.
  - `protokoll_bilanz.cjs` akzeptiert `--protokoll <Pfad>` weiterhin (abwaertskompatibel).
  - Die LOOP-STOPP-Zeile im Prompt nennt ebenfalls noch `--protokoll <Datei>`. Sonnet sollte im Abschluss `--datum <datum>` nutzen. So ist es Stand seit 30.09. (feedback_tagesabschluss 1a), unveraendert.

## Auflagen an Fable (nummeriert)

1. **(vor 08.10. Pflicht) Kalendertag statt Datenlage bei "Stand vor Stichtag".** In `endauswertungZeilen` haengt die Zeile am letzten **bewertbaren** Tag. Ist der 08.10. selbst nicht bewertbar (z. B. Loop-Ausfall, K9), steht am Abend der Endauswertung faelschlich "Stand vor Stichtag".
   - Soll: Der Zeitpunkt des Laufs entscheidet (DE-Zeit < 08.10. 20:00 → "Stand vor Stichtag"). Ist der Stichtag erreicht und der 08.10. nicht bewertbar, steht dort ausdruecklich "08.10. nicht bewertbar — Endauswertung mit <n> Tagen (Levi R-1)".
   - Die Uhr wird wie bei `endFreigabe(jetztMs)` nur intern injiziert. Dazu ein Test. (Spezifikationsfehler Opus.)
2. **(vor 08.10. Pflicht) AUTO-Urteil konsistent machen.** Bei AUTO < 20 kann die AUTO-Variantenzeile rechnerisch "belegt" zeigen, waehrend die Zeile darueber "AUTO 'zu selten, nicht belegt'" sagt.
   - Soll: Bei AUTO < FREEZE_AUTO_MIN lautet das AUTO-Urteil im ENDAUSWERTUNG-Block fest "zu selten, nicht belegt (AUTO <n>/20)". Dazu ein Test.
   - Die bestehenden Zeilen bleiben byte-identisch.
3. **loop_archiv.cjs: unerkannte VC-Dateien melden.** Die Regex kennt nur `_N` und `_Nb`. Eine zweite Wiederholung (`_Nc`, `_Nbb`) oder Tippformen wuerden **still ignoriert**.
   - Soll: Jede Datei `vc_<datum>_*`, die nicht passt, wird in der Zusammenfassung als "NICHT EINGELESEN" genannt.
   - Alternativ `_N[b-z]?` zulassen und so dokumentieren.
4. **MEMORY.md:** Die Indexzeile "Endauswertung auf 08.10.2026 vorgezogen …" endet noch mit "Fable-Auftrag h1 `end`-Stichtag offen". Diese Zeile nach der Index-Konvention **ersetzen** durch "umgesetzt 02.10., Opus-Gegencheck ABGENOMMEN".
5. **Vorschlag C4/C5 vor Aktivierung (05.10.) anpassen.** Die Punkte stehen unten. Danach `loop_prompt.cjs --testtag fiktiv` + npm test, Opus-Kurzcheck des Prompt-Diffs.
6. **Commit-Umfang dokumentieren** (Hinweis an Levi in der Rueckmeldung): Code-Commit **nur** die 7 geaenderten Dateien + `scripts/loop_archiv.cjs`.
   - **Nicht** mitnehmen: `scripts/loop_archiv/*.txt`, `*.bis1900.bak`, `last_*.txt`, `analyse/backtest_2026-09-2*.cjs` (alles untracked). Ob die Archive versioniert werden, ist eine eigene Levi-Entscheidung.

## Vorschlag C4/C5 (nicht umgesetzt) — Sicherheitsbewertung fuer den Ersteinsatz 05.10.

Der Ansatz ist richtig: eine Betriebsaenderung ohne Gate-Wirkung. vollcheck nimmt `--sl-anker`/`--cluster-level`/`--stale-n` bei 0 Beinen an; das habe ich am Code geprueft (Z.1184/1191 `wenn: slVorpruefungFaellig`, Z.1376-1384). **Einsatzreif nur mit diesen Korrekturen:**

- **(a) Redirect versteckt die Ausgabe.** Mit `> /tmp/vc_… 2>&1` sieht Sonnet den Voll-Check nicht mehr direkt.
  - Woertliches Muster vorgeben: `node scripts/vollcheck.cjs … > /tmp/vc_<D>_<N>.txt 2>&1; echo "EXIT=$?"; cat /tmp/vc_<D>_<N>.txt`. Kein `| tee`, weil der Exit-Code sonst verloren geht.
  - Pflicht ist das **Bash-Tool**. Unter PowerShell wird `/tmp` zu `C:\tmp` (hier beobachtet), dann findet loop_archiv nichts. Rueckfall: `--vc-dir C:\tmp`.
- **(b) "IMMER --sl-anker … unabhaengig von der Beinzahl" ist bei 0 Beinen unterbestimmt.** Ohne Richtung gibt es keine SL-Seite; P1 prueft dann nicht.
  - Zudem landet der Wert mit `--nas-bars-5m` als `anker_live` in `anker_auto_log.jsonl` (Z.2110-2137). Das ist eine Messreihe.
  - Soll: Den Block praezisieren ("bei 0 Beinen: Anker der 1H-Bias-Richtung; ab 1 Bein: SL-Seite der Bein-Richtung, P1"). Per `vollcheck.cjs --dry-run` belegen, dass ein 0-Bein-Anker weder P2/P3-Zaehlung noch anker_auto_log verfaelscht. Sonst den Anker bei 0 Beinen ausdruecklich weglassen und nur `--cluster-level`/`--stale-n` verlangen.
- **(c) LOOP-STOPP "ggf. --ueberschreiben nach Vergleich" streichen.** Ueberschreiben gibt es nur auf ausdrueckliche Anweisung von Levi. Grund: Am 30.09. traegt das Archiv einen manuell angehaengten Faktenabschluss, den ein Neulauf verlieren wuerde.
  - Zusatzsatz: "Faktenprotokoll-Abschluss erst NACH loop_archiv.cjs anhaengen."
- **(d) loop_stopp.cjs Z.~151 nicht im selben Schritt aendern** (zweites Live-Skript, Freeze: hoechstens eine Live-Aenderung pro Tag). Auf die Zeit nach der Endauswertung 08.10. verschieben; bis dahin ist die Zeile funktional korrekt.
- Messwirkung waehrend 01.-08.10.: C4 senkt Hard-Exits und kann dadurch Abdeckung/VC-Zahl je Tag leicht erhoehen. Kriterien und Gates aendert es nicht. In der Endauswertung als "Betriebsaenderung ab 05.10." vermerken.

## Aussage fuer Levi

**Commit freigegeben: JA.** Code-Repo selektiv: 7 geaenderte Dateien + `scripts/loop_archiv.cjs`. Memory-Repo: die von Fable geaenderten/neuen Dateien + diese Datei. Commit und Push beauftragt **nur Levi** ([[feedback_commit_push_nur_levi]]). Die Auflagen 1-2 sind bis spaetestens Mi 07.10. umzusetzen (danach Opus-Kurzcheck). Sie blockieren den Commit nicht, weil sie nur die Anzeige am 08.10. betreffen. Der heutige Cron-Testtag bleibt unberuehrt, egal ob vor oder nach 15:30 committet wird.
