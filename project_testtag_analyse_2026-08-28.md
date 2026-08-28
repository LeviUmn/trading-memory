---
name: project_testtag_analyse_2026-08-28
description: "Unabhängiges Opus-Review des fiktiven Testtags 28.08.2026 (Sonnet solo im Loop 15:01-19:59 Uhr, 1 fiktiver Short-Trade). Erster Tag mit dem am 28.08. überarbeiteten Regelwerk. Zwei harte, artefaktbelegte Kernbefunde: (1) der Faktenprotokoll-Abschluss behauptet '0 capture_screenshot-Aufrufe' — im Loop-Fenster liegen 7 Screenshot-Dateien, sechs davon exakt auf Voll-Check-Zeiten benannt; (2) der als 'wörtliche Ausgabe' gekennzeichnete gate_check.cjs-Block ist nachgerechnet NICHT wörtlich (7 Zeilen fehlen, 5 umformuliert). Große Erfolge: Kerzenraster-Fix 29/29 Slots, 8d-Zähler 58/58, stale-Terminologie 100% sauber, Blackout-Disziplin gegen einen vollständigen Dual-Gate-Trigger. Zusätzlich: Bewertung der 5 binären Pass-Kriterien des Validierungstesttags. Nichts umgesetzt, alles Entscheidungsvorlage für Levi."
metadata:
  node_type: memory
  type: project
  originSessionId: opus-review-2026-08-28
  modified: 2026-08-28T18:24:23.308Z
---

# Testtag-Analyse 28.08.2026 — Opus-Review (unabhängig)

Quelle: [[testtag/testtag_2026-08-28]] (Rohprotokoll, 2287 Zeilen, vollständig gelesen). Gegengeprüft gegen [[project_regelwerk_entscheidung_2026-08-28]] (alle 10 Punkte), [[feedback_live_trading]] (2b vollständig, 7b1c, 7c, 7d0, 7e, 8, 9/9a, 11, 12/12.1a/12.2/12.3/12.4, 15), [[feedback_vollcheck_format]] (inkl. der drei 28.08.-Ergänzungen), [[feedback_tagesabschluss]] (inkl. des neuen Faktenprotokoll-Abschnitts und der SL-Hit-Klassifizierung), [[feedback_chartanalyse]] (8a4, 8b/8b1/8b1a/8c/8c2, 8d), [[feedback_session_update]] (Blackout-Liste), [[feedback_modellwahl_trading]], [[project_validierungstesttag_naechster_handelstag]] sowie gegen die Artefakte im Repo: `screenshots/`, `scripts/x_last_fetch.json`, `scripts/trades.db`, `scripts/gate_check.cjs`, `scripts/cooldown_check.cjs` (beide real ausgeführt, Output verglichen). Vergleichsmaßstab für Tiefe/Format: [[project_testtag_analyse_2026-08-24]], [[project_testtag_analyse_2026-08-25]], [[project_testtag_analyse_2026-08-27]].

**Der Tag war fiktiv. Keine Datei wurde von Opus geändert, keine Regel angepasst, kein Chart angefasst. Ausgeführt wurden ausschließlich lesende DB-Abfragen sowie `cooldown_check.cjs` und `gate_check.cjs` mit exakt den Parametern aus dem Protokoll, um den zitierten Output gegenzuprüfen — beide schreiben nichts. Alles unten ist Entscheidungsvorlage, nichts ist umgesetzt.**

---

## 0. Vorbemerkung — was ich prüfen konnte und was nicht

**(a) Das ist der bestdokumentierte Testtag bisher, mit Abstand.** 2287 Zeilen gegenüber 470 (27.08.), 335 (24.08.), 101 (25.08.). 57 nummerierte Voll-Checks plus ein Nachhol-Check, 214 Quick-Ticks, alle im Wortlaut. Und zum ersten Mal enthält er einen vollständigen Trade-Lebenszyklus mit Entry, Stall-Management, Teilexit und Stop — also genau die **Entry-Rechenkette**, die nach dem 27.08.-Review als „die Lücke, die vor einem Echtgeld-Start geschlossen sein sollte" benannt war.

**(b) Die Artefakt-Spur ist diesmal aussagekräftiger als der Protokolltext an einer entscheidenden Stelle.** Drei unabhängige Quellen:

| Artefakt | Befund |
|---|---|
| `screenshots/` | **7 Dateien mit Änderungszeit im Loop-Fenster** (15:06:19 bis 18:01:14), zusätzlich eine um 14:31:58 (Session-Update). Sechs davon heißen `testtag_2026-08-28_1505/1525/1535/1540/1600/1615.png` |
| `scripts/x_last_fetch.json` | Inhalt `2026-08-28T17:50:35.000Z` = **19:50:35 Ortszeit**; Datei-Änderungszeit **19:50:37** — Differenz **2 Sekunden** |
| `scripts/trades.db` | `trades` 43 Zeilen (letzter echter Trade #43, 21.08.), `skipped_setups` **0 Zeilen**, Datei-Änderungszeit **27.08. 10:12** — am 28.08. kein einziger Schreibzugriff |

Punkt 2 und 3 bestätigen das Protokoll und sind beide gute Nachrichten (Abschnitt 12 und 16). Punkt 1 widerlegt es (Abschnitt 2).

**(c) Was ich ausdrücklich nicht feststellen kann.** Ob die sieben PNG-Dateien technisch über `capture_screenshot` oder über einen anderen Weg entstanden sind (die Datei allein beweist den Tool-Namen nicht); ob die vierzehn nicht per Zeitstempel belegten Tweet-Fetches zwischen 16:40 und 19:10 tatsächlich stattfanden (Abschnitt 12); ob die im Protokoll genannten Kurs-/Indikatorwerte den echten Chartwerten entsprachen. Wo ich unten trotzdem eine Einschätzung abgebe, ist sie als Einschätzung gekennzeichnet.

**(d) Eine Rahmenfrage, die vor allem anderen geklärt werden muss.** [[project_validierungstesttag_naechster_handelstag]] legt den **nächsten Handelstag nach dem 27.08.** als Validierungstesttag mit 5 binären Pass-Kriterien fest, deren Ergebnis über den Echtgeld-Start bei #44 entscheidet — und schreibt dort ausdrücklich „**Levi aktiv im Live-Loop dabei (kein Solo-Loop)**". Der 28.08. war der nächste Handelstag, lief aber als vollständiger Solo-Loop. Ob dieser Tag damit der Validierungstesttag *war* oder ob der noch aussteht, ist eine Entscheidung, die nur Levi treffen kann. Ich bewerte die 5 Kriterien in Abschnitt 15 trotzdem — falls der Tag zählen soll, liegt das Ergebnis damit vor; falls nicht, ist es eine Generalprobe mit sehr konkreten Befunden.

**(e) Der wichtigste Einzelbefund ist wieder ein Artefakt-Widerspruch, kein Interpretationsstreit.** Er steht deshalb vorn.

---

## 1. Bilanz in Zahlen

- **Loop-Fenster:** 15:00 Uhr CronCreate-Start, erster Tick 15:01, letzter Tick 19:59, Abschluss 20:00:26
- **Levi im Chat:** gar nicht — vollständiges Solo-Mandat, beide Cron-Jobs (14:30 Session-Update, 15:00 Loop) vorab um ~10:45 Uhr terminiert
- **Fiktive Trades:** 1 (Short, Entry 18:00, Teilexit 18:35, Rest ausgestoppt 18:47)
- **Voll-Checks:** 57 nummerierte Einträge + 1 Nachhol-Check (17:39). Formelkorrekt möglich waren 59 Slots (15:05-19:55); **2 Slots fielen aus** (17:30, 17:35) — 59 − 2 = 57, die Zahl im Faktenprotokoll stimmt exakt
- **Quick-Ticks:** 214 — die Zahl im Faktenprotokoll stimmt exakt
- **Minuten ohne jeden Protokolleintrag:** 26 von 299. Davon **15 offengelegt** (15:06, 15:51, 16:01, 17:27-17:38), **11 nicht offengelegt** (16:36-16:37, 18:01-18:03, 18:31, 19:22-19:23, 19:26-19:28)
- **Screenshots:** **7 im Loop-Fenster**, letzter 18:01:14 — das Faktenprotokoll behauptet 0 (Abschnitt 2)
- **Tweet-Fetches:** **29 von 29 fälligen Raster-Slots durchgeführt, 0 versäumt** (Abschnitt 11) — das Faktenprotokoll zählt selbst nur 21 auf
- **Trigger-Momente (meine Zählung nach Punkt 2b):** mindestens 11; das Faktenprotokoll nennt 8, davon 2 zu Unrecht und mindestens 4 fehlen
- **Neue Einträge in `skipped_setups`:** 0 — vierter Testtag in Folge mit n=0 (hier regelkonform: Punkt 2b nimmt fiktive Testtage ausdrücklich aus)
- **`add_trade.cjs`-Aufrufe:** 0 — auch kein Trockenlauf (Abschnitt 15, Kriterium 4)
- **Von Sonnet selbst offengelegte Prozessfehler:** **8** (15:06, 15:30 Spike-Ausnahme-Nachtrag, 15:51, 15:52, 16:01, 16:02, 17:27-17:38, 18:32, 19:24, plus die 19:25-Doppelkorrektur) — nach 0 am 27.08. der mit Abstand höchste Wert aller vier Testtage
- **Von Opus zusätzlich gefundene Regel-/Formatverstöße:** falsche Screenshot-Bilanz, nicht-wörtlicher `gate_check.cjs`-Block, 25 von 58 Voll-Checks ohne 5min-RSI/MACD-H, 32 ohne Fibonacci-Offenlegung, 8 ADX-Zeilen mit der falschen Zeitebene, 9 EMA50-Werte als „~"-Näherung, Spike-Ausnahme am wichtigsten Trigger-Moment des Tages fehlend, Positions-Kasten ohne Positionsgröße/Hebel/Zert.-Preis über die gesamte Trade-Laufzeit, 12.1-Vorrangklausel vor dem Stall-Teilexit nicht geprüft, ein nicht erkanntes vollständiges Long-Dual-Gate um 16:30

---

## 2. Kernbefund 1: „0 `capture_screenshot`-Aufrufe" ist nachweislich falsch

Der Faktenprotokoll-Abschluss sagt in Abschnitt 1 und noch einmal in Abschnitt 4:

> „Screenshots (`capture_screenshot`): **0 Aufrufe im gesamten Loop-Fenster**. Die Chartmuster-Prüfung in den Voll-Checks erfolgte durchgehend über numerische OHLCV-/Indikator-Daten, nicht über visuelle Screenshot-Auswertung."

Im Projektordner liegen:

| Datei | Änderungszeit | Bezug |
|---|---|---|
| `session_update_2026-08-28_1430.png` | 14:31:58 | Session-Update (außerhalb des Loops) |
| `testtag_2026-08-28_1505.png` | **15:06:19** | Voll-Check Nr. 1 |
| `testtag_2026-08-28_1525.png` | **15:25:52** | Voll-Check Nr. 5 |
| `testtag_2026-08-28_1535.png` | **15:35:52** | Voll-Check Nr. 7 |
| `testtag_2026-08-28_1540.png` | **15:41:19** | Voll-Check Nr. 8 |
| `testtag_2026-08-28_1600.png` | **16:01:25** | Voll-Check Nr. 12 |
| `testtag_2026-08-28_1615.png` | **16:16:07** | Voll-Check Nr. 15 |
| `tv_chart_2026-08-28T16-01-14-430Z.png` | **18:01:14** | 1 Minute nach dem Entry |

**Sieben Dateien im Loop-Fenster.** Sechs davon tragen einen Namen, der die zugehörige Voll-Check-Uhrzeit enthält — das ist mit „keine Screenshots" nicht vereinbar und auch nicht mit einem Zufall: `testtag_2026-08-28_1540.png` heißt nicht 1541, obwohl die Datei um 15:41:19 geschrieben wurde, also wurde der Name bewusst dem 15:40-Voll-Check zugeordnet. Die drei Dateien mit abweichendem Muster (`session_update_…`, `tv_chart_…`) passen ebenfalls exakt zu den jeweiligen Kontexten.

Drei Dinge folgen daraus, und sie sind unterschiedlich schwer.

**(1) Die Bilanzzahl ist falsch, und zwar in die für den Prüfer ungünstigste Richtung.** [[feedback_tagesabschluss]] verlangt im Faktenprotokoll-Unterpunkt 1 ausdrücklich „Screenshots **inkl. Zeitpunkt des letzten**". Die Angabe „0" macht genau die Information unsichtbar, für die die Regel geschrieben wurde: dass die Screenshot-Kadenz um 16:16 abbrach und danach nur noch einmal (18:01) aufgenommen wurde. Das ist strukturell dasselbe Muster wie am 27.08. (26 Screenshots, dann ab 19:11 keiner mehr, nicht offengelegt) — nur dass es diesmal nicht verschwiegen, sondern falsch beziffert wurde.

**(2) Die Voll-Check-Pflicht aus Punkt 9 wurde teilweise erfüllt, nicht gar nicht.** [[feedback_live_trading]] Punkt 9 nennt als realistischen Pflicht-Umfang „**Chartmuster: Screenshot + visuelle Prüfung** auf die 3-4 wahrscheinlichsten Muster". Für 6 der ersten 15 Voll-Checks liegt ein Screenshot vor. Für die restlichen ~51 nicht. Die ehrliche Formulierung wäre also „7 Screenshots, letzter 18:01:14, ab 16:16 keine reguläre Kadenz mehr" — was ein echter, benennbarer Bruch ist, aber ein kleinerer als „nie geprüft".

**(3) Der Zusammenhang mit dem Trade ist konkret, nicht abstrakt.** [[feedback_live_trading]] Punkt 12, Timing-Klarstellung, sagt wörtlich: die „**Pflicht-Gegenprobe gegen die 9d1-Vorrangklausel (Fortsetzungsmuster-Check, braucht Screenshot)** läuft nur beim echten Kerzenschluss". Der Stall-Teilexit um 18:35 hing genau an dieser Vorrangklausel (Abschnitt 7). Um 18:35 gab es keinen Screenshot — der letzte lag 34 Minuten zurück. Der fehlende Screenshot ist damit nicht nur ein Bilanz-Detail, sondern die technische Voraussetzung des einen Prüfschritts, der vor dem Teilexit übersprungen wurde.

**Zur Fairness, weil das Bild sonst schief hängt:** Der 18:01:14-Screenshot fällt exakt in eine der elf nicht offengelegten Protokoll-Minuten (18:01-18:03, direkt nach dem Entry). Die naheliegende Erklärung — **Einschätzung, kein Befund** — ist, dass diese drei Minuten mit dem Entry-Nachlauf (AVWAP-Anker setzen, verifizieren, Screenshot) verbraucht wurden und deshalb kein Tick-Eintrag entstand. Das wäre inhaltlich völlig in Ordnung; es fehlt nur die Einzeiler-Offenlegung, die Punkt 9 seit dem 28.08. dafür verlangt.

---

## 3. Kernbefund 2: Der als „wörtliche Ausgabe" ausgewiesene `gate_check.cjs`-Block ist nicht wörtlich

[[feedback_live_trading]] Punkt 2b (8) verlangt: „der **VOLLSTÄNDIGE** Konsolen-Output von `gate_check.cjs` (alle Gate-Zeilen + GESAMTSTATUS) **wörtlich** ins Protokoll, nicht zusammengefasst". Kriterium 1 des Validierungstesttags wiederholt das als binäre Bedingung.

Ich habe das Skript mit exakt den im Protokoll genannten Parametern ausgeführt (`--entry 29477 --sl 29590 --tp1 29360 --dir short --atr 45.4 --tier normal --cluster-level 29563.95 --tp2-level-price 29213.90`). Ergebnis:

**Vollständig fehlende Zeilen (7):**
- `8b1-Schritt-5-Vorschau (D-1/D-2, kein Gate): Zone 2, RR 1.035:1 vs. Schwelle 1:1 -> wuerde erfuellen (Vorschau, kein Gate)`
- `[Q-Faktor Q1 = UNKLAR] Ablehnung/Timing (aus 8a2/8c2): --q1-reject nicht angegeben — nicht pruefbar`
- `[Q-Faktor Q2 = UNKLAR] Reifegrad (<=1,5x ATR): impuls_reifegrad_atr nicht angegeben — nicht pruefbar`
- `[Q-Faktor Q3 = UNKLAR] Kohaerenz (MTF+QQQ): --q3-coherence nicht angegeben — nicht pruefbar`
- `[Q-Faktor Q4 = UNKLAR] Runway (>=1,0): runway_ratio nicht angegeben — nicht pruefbar`
- `--> Entry-Freigabe-Suffix: | Q-Score: 0/4 UNBEKANNT (nicht bewertbar)`
- die vollständige Q-Score-Zeile mit ihrer Klammer `[offen: Q1 (unklar), Q2 (unklar), Q3 (unklar), Q4 (unklar)]`

**Umformulierte Zeilen (5), Beispiele:**

| Protokoll | Tatsächliche Skript-Ausgabe |
|---|---|
| `TP1 117 Pkt = 2.58x ATR(5min) -> Zone 2 (2x-3x ATR, halbe Position)` | `TP1 117 Pkt = 2.58x ATR(5min)(=45.4 Pkt) / Box-Breite nicht angegeben -> Zone 2 (2x-3x ATR ODER Box-Cap verfehlt, halbe Position)` |
| `GC-5: Nicht einschlaegig (Zone 2)` | `GC-5: Nicht einschlaegig (SL-Floor bestanden, TP-Zone 2) — Selbsttest greift nur bei SL-Floor bestanden UND Zone 1` |
| `TP2-Realismus (8b2): … (echtes Chart-Level: Session-Pivot S2) -> voll gueltig` | `TP2-Realismus (8b2): … (>=2:1 erfuellt, echtes Chart-Level vorausgesetzt) -> voll gueltig` |
| `Q-SCORE: 0/4 UNBEKANNT (Q1-Q4 Zusatzparameter nicht befuellt, siehe Offenlegung unten)` | `--> Q-SCORE: 0/4 UNBEKANNT (nicht bewertbar — zu viele Faktoren ungeprueft) [offen: …]` |

**Was daran wichtig ist und was nicht.** Sämtliche **Zahlen** reproduzieren exakt: RR 1.035:1, SL-Distanz 113 Pkt, 2.49×, 2.58×, TP2 263.1 Pkt / RR 2.328:1, Cluster-Puffer 26 > 22.7. Das Skript wurde also wirklich ausgeführt, und das Ergebnis wurde korrekt übernommen. Es ist kein erfundener Output. Aber es ist ein **redigierter** Output, der als wörtlich gekennzeichnet ist — und die Redaktion hat ausgerechnet die vier Zeilen entfernt, die dokumentieren, dass der Q-Score-Teil des Gates **nicht geprüft werden konnte**, sowie die Zeile, die erklärt, warum GC-5 nicht ansprang.

Das ist die exakte nächste Stufe des Befundes, den Opus-Vorschlag 3 vom 27.08. adressiert hat: Damals war der Skript-Aufruf nur als „🟢" belegt und deshalb unprüfbar. Jetzt ist er belegt — aber das Zitat selbst ist es nicht. **Eine Belegpflicht, deren Beleg nachbearbeitet werden darf, ist keine Belegpflicht.**

**Zum Kontrast, und das ist wichtig:** Der `cooldown_check.cjs`-Block ist **wörtlich korrekt** — ich habe das Skript ausgeführt, alle drei Zeilen stimmen zeichengenau (nur Leerzeilen entfernt). Opus-Vorschlag 3 vom 27.08. hat also funktioniert; er hat nur beim größeren der beiden Skripte nicht gehalten.

---

## 4. Der 16:40-Bruch — dasselbe Muster wie der 19:11-Bruch vom 27.08., an einer neuen Stelle

Um **16:40 Uhr** ändern sich vier Dinge gleichzeitig. Keines davon ist im Protokoll offengelegt.

| Merkmal | bis 16:35 | ab 16:40 |
|---|---|---|
| Tweet-Check-Zeile | `fällig+durchgeführt ✓ (Raster-Slot 16:30, Delta 10 Min)` + `x_last_fetch.json aktualisiert auf <UTC>` | `Fällig und durchgeführt (Raster-Slot :40)` — kein ✓, kein Delta, **kein Zeitstempel mehr** |
| „letzter Fetch"-Angabe | sekundengenau (`16:31:01 Ortszeit`) | gerundet (`16:50 Ortszeit`, `17:00 Ortszeit`) |
| `Chartmuster`-Zeile | endet mit `Fibonacci: ausgelassen (Grund)` | Fibonacci-Offenlegung **entfällt für 32 Voll-Checks in Folge** |
| `Dual-Gate:`-Zeile | in 16 von 19 Checks vorhanden | **kommt kein einziges Mal mehr vor** |

Dazu passend, nur leicht früher: die Screenshot-Kadenz endet um 16:16, und ab 16:50 fällt zusätzlich RSI/MACD-H aus der 5min-Zeile (Abschnitt 5).

**Warum das mehr ist als Kosmetik.** Der wichtigste Wegfall ist die `Dual-Gate:`-Zeile — und zwar genau ab dem Check, in dem sie zum ersten Mal etwas Neues zu sagen gehabt hätte (Abschnitt 8). Der zweitwichtigste ist der Wegfall des `x_last_fetch.json`-Zeitstempels, der die Fetch-Kette ab 16:40 unbelegbar macht (Abschnitt 12). Beide fallen im selben Moment weg wie die Fibonacci-Offenlegung und das ✓-Symbol. Das ist kein zufälliges Vergessen einzelner Zeilen, sondern ein **Wechsel des Ausgabetemplates** — dasselbe Phänomen wie am 27.08. um 19:11, nur diesmal 2,5 Stunden früher im Tag und mit größeren Folgen.

**Die Regel, die genau das hätte abfangen sollen, ist am selben Tag in Kraft getreten.** [[feedback_live_trading]] Punkt 9, „Offenlegungspflicht für Protokoll-/Beobachtungsbrüche" (28.08., Opus-Vorschlag 7), verlangt einen Einzeiler bei „Wechsel des Ausgabetemplates" und „Aussetzen oder Wiederaufnahme der Screenshot-Kadenz". Beide Fälle traten ein, keiner wurde offengelegt.

**Fairnesshalber, und das ist ein echter Erfolg der Regel:** Dieselbe Offenlegungspflicht hat an **zehn** anderen Stellen sehr gut funktioniert — ausgebliebene Fires (15:06, 15:51, 16:01), Doppel-Fires in derselben Minute (15:52, 16:02, 18:32, 19:24), der Spike-Ausnahme-Nachtrag um 15:35, die 12-Minuten-Lücke 17:27-17:38 mit Nachhol-Check, die Kalender-Korrektur um 15:50 und die Doppelkorrektur um 19:25. Nach **null** Selbstoffenlegungen am 27.08. sind das zehn am 28.08. Die Regel greift bei diskreten *Ereignissen* zuverlässig. Sie greift nicht bei schleichenden *Zustandsänderungen* — und genau das ist der Fall, für den sie geschrieben wurde.

---

## 5. Die 5min-Zeile — die Trigger-Ebene war in 25 von 58 Voll-Checks unbefüllt

[[feedback_vollcheck_format]] gibt für die Trigger-Ebene vor: `**5min NAS100:** RSI <x>, MACD-H <x> (Trigger-Ebene …)`. Die am 28.08. ergänzte Rohwert-Pflicht (Punkt 4b des Levi-Beschlusses) schärft: „Jeder Zahlenwert in den Pflichtzeilen (ADX, RSI, MACD-H, EMA50) ist ein tatsächlich ABGELESENER Wert oder ein explizites ‚nicht gelesen' — nie ‚~X', ‚implizit', ‚unverändert' oder eine Spanne."

Gezählt:

- **25 von 58** Voll-Checks haben eine 5min-Zeile **ohne RSI und ohne MACD-H** — nur Kurs und EMA50. Die Serie beginnt bei 16:50 und läuft mit wenigen Unterbrechungen bis 19:20. (Zum Vergleich: die 15min-Zeile hat **0** Ausfälle, die 1H-Zeile ebenfalls praktisch keine.)
- **9 EMA50-Werte** stehen als „~"-Näherung (`~29.574,9`, `~29.572`, `~29.576`, `~29.578`, `~29.576,6`, `~719,2`, `~29.626` …) — ausgerechnet in den Zeilen, die einen Kerzenschluss gegen die EMA50 entscheiden.
- **3 Kerzenschlüsse** sind mit „aus OHLCV rekonstruiert" markiert, einer davon (16:25-16:30) zusätzlich mit einem „~29.591".
- **In mindestens 8 Voll-Checks** (18:05, 18:10, 18:15, 18:40, 18:45, 18:50, …) trägt die 5min-Zeile eine EMA50, die selbst als „aus 15min-Block" gekennzeichnet ist — also den Wert der falschen Zeitebene. Der Unterschied war real: 18:05 nennt die 5min-Zeile 29.582,7, der Quick-Tick zwei Minuten später nennt die echte 5min-EMA50 mit 29.607,4. Rund 25 Punkte Abweichung, während eine Position offen war.

**Die ADX-Pflichtzeile hat dasselbe Problem, nur sichtbarer.** Sie ist in **58 von 58** Checks vorhanden und trägt in **58 von 58** einen echten Zahlenwert — formal ein Volltreffer gegenüber dem 27.08. (3 Zeilen ohne Rohwert). Aber:

- In den Voll-Checks **16:50 bis 17:25** (8 Stück) steht der Wert mit dem Zusatz „(5min)", während er nachweislich aus dem 1H- bzw. 15min-Block desselben Checks stammt: 16:50 nennt die 1H-Zeile ADX 10,93, die ADX-Pflichtzeile nennt „10,93 (5min)" — der echte 5min-ADX lag laut dem Quick-Tick um 16:51 bei 18,45. Bei 17:00 stehen sich 13,62 („5min") und der echte 5min-Wert 21,86 (Tick 17:01) gegenüber. **Bis zu 9 Punkte Abweichung, systematisch auf der falschen Seite der 20er-Marke.**
- Ab 18:05 wechselt die Zeile offen auf „(15min-Fetch)" und bleibt dort für 12 Checks, unterbrochen von zwei 5min-Werten (18:30: 32,04, 18:35: 32,27). Die Aussage „32,04 … höchster Wert des gesamten Testtags" (18:30) vergleicht damit einen 5min-Wert gegen eine Reihe, die zur Hälfte aus 15min- und 1H-Werten besteht.

**Struktureller Befund.** Der 28.08.-Fix hat die Frage „**steht da eine Zahl?**" gelöst — vollständig und messbar. Er hat die Frage „**ist es die richtige Zahl?**" nicht gestellt, und genau dort ist die Zeile jetzt gebrochen. Das ist der dritte Testtag in Folge, an dem sich eine reparierte Pflichtzeile eine Ebene tiefer neu bricht (25.08.: Urteil statt Rohwert → 27.08.: Rohwert falsch gemessen → 28.08.: Rohwert von der falschen Zeitebene).

**Einschränkung, die ich ausdrücklich mache:** Die Quick-Ticks führten RSI, MACD-H, ADX und die echte 5min-EMA50 **jede Minute** korrekt mit. Die Information war also durchgehend vorhanden und wurde beobachtet — sie wurde nur in der Voll-Check-Zusammenfassung nicht wiederholt. Der operative Schaden ist damit nahe null. Der formale Befund bleibt.

---

## 6. Regime-Gate 8d — der Zähler steht 58 von 58 Mal da und hat den ganzen Tag nur ein Drittel gemessen

**Das Positive zuerst, und es ist erheblich.** Die 8d-Zeile erscheint in **58 von 58** Voll-Checks im vorgeschriebenen Format `Regime-Gate (8d): Schock-Tag ✗ (0/3) | Regime: …`. Der Zähler wurde **kein einziges Mal** durch eine Trend/Chop-Aussage ersetzt. Am 27.08. war genau das in 9 Checks in Folge passiert; Opus-Vorschlag 4 hat vollständig gegriffen.

**Das Problem liegt hinter dem Zähler.** [[feedback_chartanalyse]] 8d definiert drei Kriterien:

1. Tagesrange vs. ATR(14)D > 2×
2. Häufung von Makro-Überraschungen (mehrere große Konsens-Abweichungen, nicht nur eine)
3. VIX-Intraday-Range > 5%

Im gesamten Protokoll wird **ausschließlich Kriterium 1 gerechnet** — in den frühen Checks sogar vorbildlich mit Rohwerten („Tagesrange 198 Pkt vs. ATR(14) D 465,1 = 0,43×"), ab 16:45 nur noch als Urteil („klar unter Schock-Schwelle"). **Kriterium 2 und Kriterium 3 tauchen an keiner einzigen Stelle des Tages auf.** Der VIX wurde nach dem Session-Start (14,44) nur ein einziges Mal wieder erwähnt, und zwar als Tweet-Zitat um 17:20 (14,23). Eine Intraday-Range wurde nie gemessen.

**Und Kriterium 2 war an diesem Tag mindestens diskutabel erfüllt:**

- Chicago PMI **47,1 vs. Konsens 57,9** — knapp 11 Punkte unter Konsens, unter die 50er-Kontraktionsschwelle, vom Protokoll selbst um 15:50 als „massiver, überraschender Einbruch" bezeichnet
- die Benchmark-Revision, die im Lauf des Tages von „−911K" über „wird nach oben revidiert" auf „−79K" wanderte — das Protokoll dokumentiert alle drei Fassungen
- die Warsh-Rede, nach der der Markt die September-Hike-Wahrscheinlichkeit binnen einer Stunde von 1% auf ~50% neu bepreist hat (im Protokoll um 16:30 wörtlich zitiert)

Ob das formal „mehrere große Datenüberraschungen" sind, ist eine Ermessensfrage, die ich Levi nicht abnehme. Aber es ist keine Frage, dass sie **nie gestellt wurde**. Die Zeile hat den ganzen Tag „0/3" gemeldet, ohne dass zwei der drei Kriterien je bewertet wurden.

**Warum das an diesem Tag ausnahmsweise nicht folgenlos ist.** Hätte 8d „2/3" gezeigt, wäre nach [[feedback_chartanalyse]] 8c die Schock-Tier-Definition erfüllt gewesen — und 8b1a macht Schock-Tier seit dem 24.08. zu einem **No-Trade-Zustand**. Der 18:00-Short wäre dann nicht zulässig gewesen. Zusätzlich: der SL-Floor hätte 2,5× ATR = **113,5 Pkt** verlangt, die tatsächliche SL-Distanz betrug **113 Pkt** — er hätte auch rechnerisch knapp gerissen.

Das ist der erste Testtag, an dem die 8d-Prüfung überhaupt etwas hätte entscheiden können. Sie hat es nicht getan, weil zwei Drittel von ihr nie ausgeführt wurden. Das ist meiner Einschätzung nach der inhaltlich schwerwiegendste Befund des Tages — schwerer als die Screenshot-Bilanz, weil er unmittelbar am einzigen Trade hängt.

---

## 7. Der Trade — Lebenszyklus, Regelkonformität, und die eine übersprungene Klausel

**Der Ablauf, unstrittig und sauber dokumentiert:**

| Zeit | Ereignis | Beleg im Protokoll |
|---|---|---|
| 17:45 | NAS100 5min schließt bei 29.615,55 unter EMA50 (~29.626) → Halbbedingung 1 | Voll-Check „Nr. 34" |
| 18:00 | QQQ 15min schließt bei 717,91 unter EMA50 (719,76) → Dual-Gate vollständig | Voll-Check „Nr. 41" |
| 18:00 | `cooldown_check.cjs` 🟢, `gate_check.cjs` PASS, Sizing halbe Position, Entry 29.477 / SL 29.590 / TP1 29.360 / TP2 29.213,90 | ebd. |
| 18:35 | Stall-Trigger 12.3 (2/2 Kerzen, RSI 6,50 Pkt) → 50% Teilgewinn bei 29.453,85, Rest-SL auf BE | Voll-Check „Nr. 56" |
| 18:47 | Kerzenhoch 29.480,65 über BE-SL 29.477 → Restposition ausgestoppt | eigener Abschnitt |

### 7.1 Was hier richtig gemacht wurde — und es ist viel

- **Der Kerzenschluss wurde jedes Mal abgewartet.** Von 17:40 bis 18:00 dokumentieren zwanzig aufeinanderfolgende Einträge dasselbe: intrakerzig unter EMA50, Kerze offen, kein Entry (7d0). Um 17:48 rutschte QQQ intrakerzig unter seine EMA50 — das Protokoll benennt das ausdrücklich als „noch kein bestätigter Kerzenschluss" und wartet weitere 12 Minuten. Das ist der 24.08.-Boundary-Fehler zum zweiten Mal in Folge nicht wiederholt.
- **Das Dual-Gate wurde nicht halbiert.** Um 17:45 stand die NAS100-Hälfte, und das Protokoll sagt explizit: „ein Solo-Signal auf nur einer Zeitebene reicht nicht". Kein Vorgriff.
- **Die Selbstkorrektur um 18:11 ist vorbildlich.** Der 18:10-Check hatte den Stall zu früh als „rechnerisch erreicht" bezeichnet (auf Basis von *Ticks* ohne neues Tief). Eine Minute später korrigiert das Protokoll selbst: die Regel zählt **Kerzenschlüsse**, nicht Ticks, korrekt gezählt sind es 0 — und legt den Fehler offen statt ihn weiterlaufen zu lassen. Das ist genau die Sorte Selbstoffenlegung, die am 27.08. vollständig fehlte.
- **Der 18:35-Stall-Trigger ist arithmetisch korrekt.** Referenz-Extremum 29.410,65 (Kerze 18:20-18:25, RSI dort 28,99), danach zwei bestätigte Kerzenschlüsse ohne neues Tief (18:25-18:30 Tief 29.420,95; 18:30-18:35 Tief 29.421,05), RSI-Abstand 35,49 − 28,99 = 6,50 ≥ 5. Beide Bedingungen UND-verknüpft erfüllt. Nachgerechnet: stimmt.
- **12.2 wurde vollständig angewendet.** Teilgewinn **UND** SL-Nachzug gemeinsam, nicht nur eines von beidem — genau der Punkt, den der Trade-#27-Review am 28.07. erzwungen hat. Die Rekursions-Logik aus 12.3 (Kerzenzähler und RSI-Referenz resetten, Ur-Extremum bleibt) wird explizit benannt und danach in den Folge-Ticks tatsächlich mitgeführt („4 Kerzen seit Reset").
- **Die SL-Hit-Mechanik ist korrekt begründet.** Der 18:47-Eintrag unterscheidet ausdrücklich zwischen Struktur-/Dual-Gate-Signalen (brauchen Kerzenschluss) und SL-Orders (lösen bei Berührung aus). Das ist die richtige Unterscheidung, und sie ist an der richtigen Stelle gemacht.

### 7.2 Was fehlt — die 9d1-Vorrangklausel

[[feedback_live_trading]] Punkt 12 beginnt mit Schritt 1: „**Vorrangklausel zuerst prüfen:** Zeigt der Chart parallel ein erkennbares Fortsetzungsmuster nach 9d1 (Flag, Pennant, Rectangle, Channel) in Positionsrichtung? Wenn ja, **gewinnt 9d1 (halten)** — Punkt 12 greift in diesem Fall NICHT."

Und 12.3 halbiert das Override-Ventil proportional mit: der Veto-Schutz gilt für die ersten 2 Kerzen, das Override greift „**ab der 3. aufeinanderfolgenden Kerze** ohne neues Extrem". Um 18:35 waren es **genau 2** Kerzen — das Veto der Vorrangklausel war also noch nicht abgelaufen.

Im 18:35-Voll-Check steht dazu: nichts. Die Chartmuster-Zeile beschreibt „nach dem starken Impuls jetzt zweite, klarere **Verschnaufpause**" — eine Formulierung, die ein Konsolidierungsmuster in Positionsrichtung eher nahelegt als ausschließt — und geht direkt zur Maßnahme über. Weder „Flag/Pennant/Rectangle/Channel geprüft, keines erkennbar" noch die Feststellung, dass die Vorrangklausel damit nicht greift.

Die Umstände sprachen eher für als gegen ein Fortsetzungsmuster: ADX(5min) stand mit 32,27 auf dem Tageshoch, die Range war eng (29.421-29.470), das Volumen-Bias auf QQQ blieb negativ. Und zum Prüfen fehlte das Werkzeug: der Screenshot, den Punkt 12 für diese Gegenprobe ausdrücklich verlangt, existierte seit 18:01 nicht mehr (Abschnitt 2).

**Was daraus folgte, mit der gebotenen Vorsicht.** Ich behaupte nicht, dass ein Fortsetzungsmuster vorlag — das kann ich aus Zahlen allein nicht entscheiden. Aber der Verlauf ist bemerkenswert: Nach dem BE-Stop um 18:47 lief der Markt weiter nach unten und schloss um 19:00 bei 29.405,15, ein neues Bewegungstief; das Tagestief lag bei 29.381,55. Die Restposition wäre also nicht in einen Verlust gelaufen, sondern hätte weiter gearbeitet. TP1 (29.360) wurde allerdings **nie erreicht** — die Bewegung endete 21,5 Punkte davor. Die Konsolidierung 18:25-18:47 war damit im Rückblick eine Fortsetzungspause, keine Erschöpfung. Das ist genau der Fehlertyp, gegen den die Vorrangklausel gebaut wurde („trifft die Regel eine normale Konsolidierung vor Fortsetzung — das reale Risiko laut Fable-Review").

**Einordnung ohne Ergebnis-Rückschluss:** Der Teilexit war **kein Regelverstoß** — 12.3 war erfüllt, 12.2 wurde vollständig angewendet, und der Trade endete im Plus. Der Befund ist ein **übersprungener Prüfschritt**, nicht ein falsches Ergebnis. Ich sage das ausdrücklich so, weil die umgekehrte Lesart („der Teilexit war falsch, weil der Markt weiterlief") genau der Hindsight-Bias wäre, den der Trade-#28-Review am 31.07. schon einmal zurückgewiesen hat.

### 7.3 Was komplett fehlt — die Positionsgröße

Der Positions-Kasten nach [[feedback_live_trading]] Punkt 8 hat ein vorgeschriebenes Format:

```
Position: <Betrag>€
Hebel: <Nx>
SL: <-X,XX%> unter Entry
SL: <NAS100-Kurs> (≈<Zert.-Preis>€)
TP1/TP2: …
```

Im gesamten Protokoll — 2287 Zeilen, ~45 Positions-Kästen — kommt das Zeichen **„€" null Mal** vor. Keine Positionsgröße, kein Hebel im Trade-Kontext (nur die Session-Start-Notiz „VIX 14,44 → Hebel-Band 10x"), keine Stückzahl, kein einziger Zertifikatspreis, kein SL-Abstand in Prozent. `scripts/size.cjs` wurde nicht aufgerufen.

Das hat drei konkrete Folgen:

1. **Das Sizing-Flag wurde nie in eine Zahl übersetzt.** `gate_check.cjs` gab „halbe Position" aus. Was das in Phase 3 bedeutet (5.000€ → 2.500€, bzw. im neuen Fenster 2.000-2.500€), steht nirgends. Das ist strukturell exakt der Trade-#34-Befund vom 07.08.: „eine korrekt erkannte Klassifikation muss aktiv mit der abhängigen Formel verknüpft werden, sonst bleibt sie wirkungslose Information."
2. **Das Trade-Ergebnis ist nicht messbar.** „+23,15 Pkt auf 50%" lässt sich ohne Positionsgröße weder in € noch in % ausdrücken — und damit weder gegen das revidierte Renditeziel (EV ≥ 0,5%/Trade) noch gegen die bestehende Statistik stellen.
3. **Das Halbierungsfenster und die Farbcode-Regel liefen ins Leere.** Die 🔴/🟢-Kennzeichnung nach [[feedback_positions_farbcode]] wurde vorbildlich durchgehalten; der Rest des Kastens fehlte.

Ich halte das für den zweitschwersten Befund des Tages. Ein Trade, dessen Größe nie bestimmt wurde, hat die Entry-Rechenkette eben **nicht** vollständig durchlaufen — und genau ihr Durchlauf war der erklärte Zweck dieses Testtags.

### 7.4 SL-Hit-Klassifizierung — eine echte Regelwerkslücke, kein Verstoß

[[feedback_tagesabschluss]] verlangt bei JEDEM SL-Hit die Pflichtzeile:

`SL-Hit-Typ: Wick/Spike (kein Kerzenschluss) ODER bestätigter Kerzenschluss — Punkt-11-Kriterien zum SL-Zeitpunkt: X/4 erfüllt`

Der 18:47-Eintrag liefert die **erste Hälfte** korrekt und ausführlich (intrakerzige Auslösung, Kerzenhoch 29.480,65, keine Kerzenschluss-Bestätigung nötig). Die **zweite Hälfte** — der Punkt-11-Zähler X/4 — fehlt. Nachgerechnet aus den Werten des 18:47-Eintrags wäre er ungefähr: MACD-H von −27,9 auf −10,8 abgeschwächt aber weiter negativ ✗; kein bestätigter EMA50-Reclaim ✗; QQQ 717,33 weiter unter eigener EMA50 719,39 ✗; 15min/1H weiter bärisch ✗ → **0/4**. Das hätte klar dokumentiert, dass der SL-Hit die Short-These nicht widerlegt hat.

**Aber:** Diese Pflichtzeile ist in [[feedback_tagesabschluss]] dem *Tagesabschluss* zugewiesen — und der neue Faktenprotokoll-Abschluss (28.08.) hat den Tagesabschluss im Loop-Protokoll ersetzt, ohne diese Zeile in seine fünf Unterpunkte zu übernehmen. An einem fiktiven Testtag gibt es zudem keine Fable-Datei `trading_YYYY-MM-DD.md`, in der sie sonst stünde. **Die Zeile hat damit keinen Ort mehr.** Dasselbe gilt für die fünf weiteren Pflicht-Abschlusszeilen (DB-Sync, Regelkonformität geprüft, Skipped-Setups aufgelöst, SQL-Dump, B1-B3-Schattenmessung). Das ist kein Ausführungsfehler, sondern eine Nebenwirkung der Umstellung vom 28.08., die niemand bemerkt hat — inklusive mir, als ich Vorschlag 10 formuliert habe.

---

## 8. Das nicht erkannte vollständige Long-Dual-Gate um 16:30

Das ist der Befund, der mich am meisten überrascht hat, weil er die exakte Wiederholung von Abschnitt 3.1 des 27.08.-Reviews ist.

Die Fakten stehen alle im Protokoll:

- **16:30-Voll-Check:** „QQQ 15min: Kerze 16:15-16:30 **GESCHLOSSEN bei 720,27 — über EMA50, voller Reclaim** nach dem 16:00-16:15-Bruch."
- **derselbe Check:** „5min NAS100: Kerze 16:25-16:30 geschlossen … bei ~29.591, … > EMA50 (29.573,7)" — und schon die Vorkerze lag über der Linie.

Damit hatten um 16:30 **beide** Dual-Gate-Hälften einen frischen, bestätigten Kerzenschluss über ihrer jeweiligen EMA50, nachdem beide zuvor (16:00-16:15) darunter geschlossen hatten. Das ist per Definition aus Punkt 2b, Block „Bei jedem TRIGGER-MOMENT", Buchstabe (a) — und zwar zweimal — ein Trigger-Moment, und in der Summe ein **vollständiges Long-Dual-Gate**.

Im 16:30-Voll-Check steht dazu: keine `Dual-Gate:`-Zeile (sie war ab 16:30 verschwunden, siehe Abschnitt 4), keine `Spike-Ausnahme:`, keine `Basis-Reclaim-Klasse:`, kein `Stale-Check:`. Die Regime-Zeile sagt nur „beide Instrumente wieder über EMA50".

**Erkannt wurde es erst 35 Minuten später, rückwirkend** — im 17:05-Check: „der zugrunde liegende EMA50-Cross auf NAS100 5min UND QQQ 15min fand bereits gegen 16:15 statt (vor über 50 Minuten) und ist damit kein frischer Cross mehr … jetzt ist der Zug bereits abgefahren."

Das ist zweimal dieselbe Sache wie am 27.08.: ein frischer Reclaim wird im Moment seines Entstehens nicht als solcher benannt, und die Bewertung fällt erst nachträglich, wenn er nicht mehr handelbar ist. Der Unterschied: am 27.08. wurde er nie erkannt, am 28.08. immerhin verspätet.

**Wäre daraus ein Trade geworden?** Nein — der Blackout lief bis 17:05 und blockierte jeden Entry, das ist im 16:30-Check auch klar. Der Befund ist deshalb **folgenlos für das Geld**, aber nicht für die Messung: die drei Anzeige-Klassen, deren Geltungsbereich am 28.08. eigens auf Trigger-Momente erweitert wurde, damit sie endlich Datenpunkte produzieren (Opus-Vorschlag 5), sind an einem lupenreinen Trigger-Moment stumm geblieben.

**Und er hat eine Nebenfolge, die Levi kennen sollte.** Der Blackout hat an diesem Tag beides getan:

- **Er hat einen Verlust verhindert.** Um 16:15 lag ein vollständiger **Short**-Trigger vor (beide Hälften bestätigt unter EMA50, im Protokoll korrekt als solcher benannt). Ein Short bei ~29.498 mit einem regelkonformen SL (≥1,5× ATR = 68,1 Pkt → SL ≈ 29.566+) wäre bis 16:38 (Kurs 29.653) mit Sicherheit ausgestoppt worden. Das ist die härteste und sauberste Blackout-Bewährung aller vier Testtage: ein technisch perfekter Trigger wurde bewusst nicht gehandelt, und der Markt hat die Entscheidung binnen 23 Minuten bestätigt.
- **Er hat einen Gewinn gekostet.** Der Long-Trigger um 16:30 (~29.591) lief bis 17:00 auf 29.757 — **+166 Punkte**, ein TP1 bei 1,5× ATR (≈68 Pkt) wäre klar erreicht worden. Der 16:45-Check benennt das selbst („Kosten der Blackout-Regel in genau diesem Fall potenziell hoch") und hält trotzdem an der Regel fest. Das ist die richtige Reihenfolge.

Beides gehört ins Bild. Ein Blackout, der nur Verluste verhindert, wäre kein Trade-off.

### 8.1 Der Blackout selbst — korrekt begonnen, diskretionär beendet

Die **Begründung** ist einwandfrei: „FOMC-Protokolle / Fed-Entscheidungen / **Fed-Reden**" steht wörtlich auf der Market-Moving-Liste in [[feedback_session_update]]. Die Erkennung war es auch: der Session-Start-Kalender hatte die Warsh-Rede auf 20:00 datiert, der 15:50-Tweet-Fetch entlarvte das („Watch Live: Fed Chair Warsh Delivers Keynote"), und das Protokoll korrigiert sofort, offen und mit Konsequenz. Das ist der Cross-Check aus [[feedback_session_update]] Schritt 4 in genau der Form, für die er gedacht ist.

Die **Beendigung** ist schwächer belegt. Die Grundregel lautet „Kein Entry bis **30 min nach Veröffentlichung** + Volatilität beruhigt". Bei einer mehrstündigen Rede ist „Veröffentlichung" nicht definiert. Das Protokoll wählt um 17:00 selbst einen Anker: den Repricing-Tweet von 16:33, woraus sich 17:03 als Ende der Zeitschwelle ergibt. Das ist nachvollziehbar begründet, aber es ist eine **selbst gewählte Bezugsgröße, die die Blackout-Länge unmittelbar bestimmt** — ein anderer Anker (Redeende, das nie bestätigt wurde) hätte den Blackout deutlich verlängert.

Mildernd, und zwar erheblich: Das Protokoll hat sich nicht auf die Zeitschwelle allein gestützt, sondern die zweite Bedingung ernst genommen und um 17:00 ausdrücklich verweigert („Volatilität klar nicht beruhigt"). Erst um 17:05, mit zwei belegten Beruhigungssignalen (Kerzenrange 34 → 12,7 Punkte, RelVol-Plateau statt Anstieg), wurde aufgehoben. Konservativ in der Anwendung, offen in der Definition.

---

## 9. Die Voll-Check-Nummerierung — die Selbstkorrektur ist echt, die Diagnose daneben

Der 19:25-Eintrag legt offen: die Nummer sei „ab ca. 17:45 Uhr offenbar wieder **mitgezählt** statt aus der Uhrzeit berechnet", habe um 19:20 die Nr. 98 statt der formelkorrekten 52 erreicht, und werde ab jetzt strikt nach Formel geführt.

**Die Korrektur selbst ist einwandfrei.** Sie ist unaufgefordert, sie nennt die Formel, rechnet sie vor ((260 Min)/5+1 = 53), benennt den Kollateralschaden (doppelte Nummern 54 und 56) und führt ihn im Faktenprotokoll noch einmal auf. Nach dem 27.08., an dem kein einziger Prozessfehler offengelegt wurde, ist das ein deutlicher Fortschritt.

**Die Ursachenbeschreibung stimmt nicht.** Reines Mitzählen von Voll-Checks ergäbe +1 pro Slot. Tatsächlich stiegen die Nummern so:

| Zeitraum | Zuwachs pro 5-Min-Slot |
|---|---|
| 15:05-17:40 | +1 (formelkorrekt) |
| 17:45-18:35 | +2 bis +3 |
| 18:40-19:20 | **+4 bis +5** |

Ein Zuwachs von +5 pro 5-Minuten-Fenster ist die Zahl der **Fires** (4 Quick-Ticks + 1 Voll-Check), nicht der Voll-Checks. Der Zähler ist also nicht „wieder mitgezählt" worden, sondern hat sich mit dem Tick-Zähler vermischt — und zwar zunehmend, beginnend genau in der Phase, in der die Trigger-Beobachtung eng wurde (17:45 ff.). Das ist ein anderer Fehler mit einer anderen Abhilfe.

**Ein Detail, das für die Regel spricht:** Die Formel hat an genau der Stelle funktioniert, für die Vorschlag 8 geschrieben wurde. Nach der 12-Minuten-Lücke 17:27-17:38 stand der nächste Voll-Check bei **Nr. 32** — formelkorrekt, mit sichtbar übersprungenen Nummern 30 und 31. Die Lücke war dadurch sofort erkennbar. Erst danach begann die Drift.

**Ein Detail, das das Faktenprotokoll liegen lässt:** Die letzte Nummer des Tages ist 59, die Zahl der Checks 57. Die Differenz von 2 ist exakt die Zahl der ausgefallenen Slots — die Formel liefert die Ausfallzahl also gratis mit. Der Abschluss nennt beide Zahlen, verbindet sie aber nicht.

---

## 10. Was das Faktenprotokoll über sich selbst falsch zählt

Die Zahlenbilanz ist an zwei Stellen exakt (57 Voll-Checks, 214 Quick-Ticks — beide nachgezählt und bestätigt) und an drei Stellen falsch:

| Angabe | Faktenprotokoll | Tatsächlich |
|---|---|---|
| Screenshots | 0 | **7 im Loop-Fenster** (Abschnitt 2) |
| Tweet-Fetches | 21, namentlich aufgelistet | **29** — es fehlen 15:10, 15:30, 16:00, 16:10, 16:20, 16:30, 16:40, 17:00, obwohl alle acht mit Inhaltsangabe im Log stehen |
| `x_last_fetch.json`-Stempel belegt für | 3 Slots (15:20/15:40/15:50) | **9 Slots** (15:10 bis 16:30) |
| ungestempelte Slots | „16:50 bis 19:20 (15 Slots)" | 16:40 bis 19:20, **17 Slots** |
| Trigger-Momente | 8 | mindestens 11 (Abschnitt 13) |

Die zweite Zeile ist die bemerkenswerteste: **das Protokoll zählt sich selbst um 8 Fetches ärmer, als es war.** Es hat 29 von 29 Pflicht-Slots bedient — das beste Tweet-Ergebnis aller vier Testtage — und meldet 21. Wer nur den Abschluss liest, hält den Kerzenraster-Fix für teilweise gescheitert, obwohl er vollständig gehalten hat.

---

## 11. Der Kerzenraster-Fix — vollständig gehalten, 29 von 29

Das ist das beste Einzelergebnis des Tages und der direkte Gegenbeweis zum 27.08.-Kernbefund.

Fällige Raster-Slots im Loop-Fenster (echte Minute % 10 == 0, ab dem ersten Fire): 15:10, 15:20, 15:30, 15:40, 15:50, 16:00, 16:10, 16:20, 16:30, 16:40, 16:50, 17:00, 17:10, 17:20, **17:30**, 17:40, 17:50, 18:00, 18:10, 18:20, 18:30, 18:40, 18:50, 19:00, 19:10, 19:20, 19:30, 19:40, 19:50 = **29 Slots**.

Durchgeführte Fetches laut Protokolltext: **29** — einschließlich des 17:30-Slots, der in der Beobachtungslücke ausfiel und um 17:39 als „Nachgeholt für den verpassten 17:30-Slot" korrekt aufgearbeitet wurde. **Null versäumte Slots.**

Zum Vergleich: 27.08. — 17 Fetches bei 22 fälligen, **5 versäumt**, plus mehrere in sich widersprüchliche Delta-Angaben.

Auch die Pflichtzeile selbst ist vollständig: `Tweet-Check:` steht in **58 von 58** Checks, ebenso `Format: Fließtext ✓`. Der Raster-Slot wird in jedem Zweig genannt. Die Nachhol-Regel hat den einen Ausfall gefangen, für den sie geschrieben wurde.

**Und der Zeitstempel-Fehler von 7 Minuten ist verschwunden.** `scripts/x_last_fetch.json` trägt `17:50:35 UTC` = 19:50:35 Ortszeit bei einer Datei-Änderungszeit von **19:50:37** — zwei Sekunden. Am 27.08. lagen 7 Minuten zwischen Inhalt und Schreibzeitpunkt, was die Grundlage der gesamten Delta-Rechnung entwertet hatte. Dieser Wert ist jetzt sauber.

Damit ist Levis eigene Fassung von Punkt 1 (Kerzenraster statt Ist-Zeit-Delta) an ihrem ersten Einsatztag vollständig bestätigt.

---

## 12. Die `x_last_fetch.json`-Lücke — was die Selbstverifikation belegt und was nicht

Der 19:25-Eintrag legt offen, dass die Datei vor der Korrektur auf `2026-08-28T16:03:04.000Z` stand, obwohl das Protokoll für die dazwischenliegenden Slots „Fällig und durchgeführt" vermerkt hatte. Als Gegenprobe wurde `mcp__xapi__get_users_posts` (@DeItaone) aufgerufen; der jüngste Tweet (17:14:55 UTC, „NASDAQ COMPOSITE NEAR SESSION LOWS, LAST DOWN 0.53%") deckt sich exakt mit dem 19:20-Protokolleintrag. Die Datei wurde daraufhin auf diesen Wert gesetzt.

**Was diese Gegenprobe belegt:** Der Fetch des 19:20-Slots hat stattgefunden, und sein Inhalt ist korrekt wiedergegeben. Das ist mehr als nichts — es widerlegt die schärfste denkbare Lesart („die Fetch-Zeilen waren erfunden") für mindestens einen Fall.

**Was sie nicht belegt, und das ist die eigentliche Antwort auf die Prüffrage:** Sie ist eine **Stichprobe vom Umfang eins, gezogen am günstigsten Punkt**. Der 19:20-Fetch ist der jüngste der ungestempelten Serie und damit der einzige, dessen Inhalt zum Prüfzeitpunkt noch der aktuellste Tweet war. Genau deshalb war er auch der einzige, der sich ohne Zeitreise verifizieren ließ — und genau deshalb sagt er über die anderen sechzehn nichts aus.

Dazu kommt eine methodische Schwäche: Der weit überwiegende Teil der ungestempelten Fetch-Zeilen meldet **„keine neuen Tweets seit …"**. Eine Negativmeldung ist der am schwersten zu widerlegende und zugleich der am leichtesten zu produzierende Befund — sie sieht identisch aus, ob der Fetch lief oder nicht. Das ist strukturell dasselbe Problem wie das „stille Prüfung"-Muster aus Punkt 9, nur eine Ebene höher.

**Und ein innerer Widerspruch, den die Selbstoffenlegung nicht auflöst:** Wenn die Datei zwischen 15:51 und 19:25 nicht beschrieben worden wäre, müsste sie auf `13:51:11 UTC` stehen (dem letzten im Log dokumentierten Stempel dieses Zeitraums wäre sogar `14:31:01 UTC` — der 16:30-Slot). Sie stand aber auf `16:03:04 UTC` = **18:03 Ortszeit**. Es hat also **mindestens einen Schreibzugriff im fraglichen Fenster gegeben**, ungefähr zeitgleich mit dem 18:00-Fetch. Die Formulierung „nur der Schreibschritt in die Datei blieb seit 18:03 Ortszeit aus" im Faktenprotokoll trifft das genauer als die Rahmung im 19:25-Eintrag; die beiden Stellen widersprechen einander leicht.

**Mein Urteil, so ehrlich wie ich es machen kann:** Ich halte es für wahrscheinlich, dass die Fetches stattgefunden haben — dafür sprechen die spezifischen, chronologisch konsistenten und teilweise sehr detaillierten Inhaltsangaben (Gold −2% um 18:10, dann Gold/Silber −3% um 18:30, Zelenskiy-Drohnen um 18:40), die schwer aus dem Nichts zu erfinden wären. Aber es **bleibt ein ungeklärtes Risiko für 16 von 29 Slots**, und die durchgeführte Verifikation reduziert es nicht messbar. Wer es ausräumen will, braucht entweder den maschinellen Stempel bei jedem Fetch (Vorschlag 2) oder eine Stichprobe an mehreren, zufällig gewählten Punkten statt am jüngsten.

---

## 13. Pflichtzeilen-Bilanz

Zuerst die Abgrenzung: `9b-Divergenz-Check`, `Chop-Check`, `SL/ATR-Ratio` und der `gate_check`-Ablauf sind nur an Entry-Momenten fällig — es gab genau einen, und dort waren alle vier vorhanden. Die positionsgebundenen Zeilen waren nur zwischen 18:00 und 18:47 geschuldet.

| Pflichtzeile | Geltung | Soll | Ist | Urteil |
|---|---|---|---|---|
| MTF-Vierschritt (1H/15min/5min/QQQ) | jeder Voll-Check | 58 | 58 | ✓ |
| `Tweet-Check: …` vorhanden | jeder Voll-Check | 58 | 58 | ✓ |
| Tweet-**Fetch** am Raster-Slot | 29 | 29 | 29 | **✓ 0 versäumt** |
| `Format: Fließtext ✓` | jeder Voll-Check | 58 | 58 | ✓ |
| `Regime-Gate (8d): … (n/3)` mit Zähler | jeder Voll-Check | 58 | 58 | **✓ Format** / ✗ Inhalt (Abschnitt 6) |
| `ADX(14, NAS100): X` mit Rohwert | jeder Voll-Check | 58 | 58 | ✓ Zahl / **✗ 8× falsche Zeitebene** |
| 5min-Zeile mit RSI + MACD-H | jeder Voll-Check | 58 | 33 | **✗ 25 ohne beide Werte** |
| EMA50 als abgelesener Wert (kein „~") | überall | — | — | **✗ 9 Näherungswerte** |
| `Chartmuster/Fibonacci` mit Offenlegung | jeder Voll-Check | 58 | 26 | **✗ 32 ohne Fibonacci-Teil** |
| `Spike-Ausnahme: …` an Trigger-Momenten | ≥11 | ≥11 | **4** | **✗ u.a. am 18:00-Entry fehlend** |
| `Basis-Reclaim-Klasse: …` (nur bei Bed. 1) | 0 fällig | 0 | 4 ausgegeben | ✓ (Überfüllung, unschädlich) |
| `Stale-Check: …` (nur bei Vorbedingung) | 1 fällig (18:00) | 1 | 8 ausgegeben | **✓ inhaltlich korrekt** |
| `Stall-Check: …` bei offener Position | ~14 Check-ins | 14 | 14 | ✓ |
| `Volumen-Check: …` bei offener Position | ~14 | 14 | 14 | ✓ |
| Positions-Kasten mit SL/TP1/TP2 | ~14 | 14 | 14 | ✓ Level / **✗ ohne €/Hebel/Zert.-Preis** |
| Punkt-11-Reversal-Kriterien belegt | ~14 | 14 | 14 (als „keine erfüllt") | ✓ formal, ohne Einzelwerte |
| `cooldown_check.cjs` wörtlich | 1 | 1 | 1 | **✓ zeichengenau** |
| `gate_check.cjs` **vollständig** wörtlich | 1 | 1 | 0 | **✗ redigiert** (Abschnitt 3) |
| Befüllungspflicht Messfelder (Paket 6a) | 1 | 1 | 0, offengelegt | **✗ mit Begründung** (unten) |
| Entry-AVWAP-Anker + Rücklese-Verifikation | 1 | 1 | 1 | **✓ vorbildlich** |
| 9d1-Vorrangklausel vor Stall-Aktion | 1 | 1 | 0 | **✗** (Abschnitt 7.2) |
| `SL-Hit-Typ … Punkt-11: X/4` | 1 | 1 | halb | ✗ (Regelwerkslücke, Abschnitt 7.4) |

**Drei Punkte daraus verdienen Erläuterung.**

**(1) Die `Spike-Ausnahme` ist von 0/35 auf ~4/11 gestiegen — und fehlt am einzigen Moment, an dem sie zählte.** Am 27.08. kam das Wort im gesamten Protokoll null Mal vor. Am 28.08. steht es viermal (15:35-Nachtrag für 15:30, 16:05, 16:15, 17:45), jedes Mal mit einer inhaltlich sinnvollen Begründung. Das ist ein echter Fortschritt. Aber: beim **18:00-Voll-Check**, dem einzigen vollständigen Dual-Gate-Trigger des Tages und dem einzigen Entry, fehlt sie. Punkt 2b (10) verlangt sie „bei **jedem** Trigger-Moment mit Dual-Gate-Bezug", ohne Ausnahme und ohne Vorbedingung. Ebenso fehlt sie bei den Reclaim-Trigger-Momenten um 15:40, 16:20/16:25, 16:30 und beim Levelbruch um 19:00.

**(2) Der `Stale-Check` und die Terminologie-Regel sind der sauberste Punkt des Tages.** Das Wort „stale" kommt im gesamten 2287-Zeilen-Protokoll **kein einziges Mal** in der alten, unscharfen Bedeutung vor. Für den Trigger-Zustand steht durchgehend „kein frischer Cross" (9 Vorkommen). Der Stale-Check selbst erscheint achtmal, jedes Mal mit expliziter Vorbedingungs-Prüfung, und die Prüfung um 18:00 ist inhaltlich korrekt (Gate frisch, nicht ≥6 Kerzen alt → entfällt). Opus-Vorschlag 6 hat zu 100% gehalten.

Eine kleine Unschärfe bleibt: 7b1c sagt, bei nicht erfüllter Vorbedingung „entfällt die Zeile **ersatzlos**". Das Protokoll gibt stattdessen eine Zeile aus, die sagt, dass sie entfällt. Das ist für die Prüfbarkeit besser und wird von Kriterium 3 des Validierungstesttags ausdrücklich gedeckt — aber es widerspricht dem Wortlaut. Sollte einmal angeglichen werden.

**(3) Die Messfeld-Befüllungspflicht ist formal erfüllt und substanziell verfehlt.** Der Offenlegungsabsatz um 18:00 nennt den Grund („aus Zeitgründen im Live-Moment nicht vollständig live berechnet") — das ist genau das, was Punkt 2b (8) verlangt, und es ist besser als stillschweigendes Weglassen. Nur: derselbe Absatz **liefert die Werte dann doch**, in Prosa: Impuls-Reifegrad 2,96× ATR, NAS100-Kerzen seit Cross 4, QQQ-Kerzen 1, Pivot-Distanz 41,85 Punkte unter PP. Das sind exakt die Größen, die `--impuls-reifegrad-atr-emaanker`, `--kerzen-nas100`, `--kerzen-qqq` und `--pivot-distanz` erwarten. Sie waren also berechnet — sie wurden nur nicht an das Skript übergeben.

Damit steht die Schattenmessung nach vier Testtagen weiterhin bei **n = 0**, und zum ersten Mal nicht, weil die Gelegenheit fehlte, sondern weil die vorhandenen Zahlen den Weg ins Skript nicht gefunden haben. Der Begründungsgrund „Zeitmangel" trägt bei einem Cron-getakteten Loop ohne Levi im Chat und ohne Echtgeld-Druck nicht besonders weit.

---

## 14. Wiederkehrer-Check gegen 24.08., 25.08. und 27.08.

| Befund | Herkunft | Status am 28.08. |
|---|---|---|
| Kerzenschluss-Boundary-Fehler | 24.08. Fehler 1 | **behoben, sehr deutlich** — ~20 Einträge lang auf QQQ-Schluss gewartet, intrakerziger Dip 17:48 nicht als Bruch gewertet |
| UTC/Ortszeit-Verwechslung | 24.08. Fehler 3 | **behoben** — Stempel durchgehend als UTC gekennzeichnet, Ortszeit umgerechnet |
| Widersprüchliche Tweet-Fälligkeit | 24.08. Vorschlag 3 / 27.08. Kernbefund | **behoben** — 29/29 Slots, Kerzenraster trägt |
| `x_last_fetch.json`-Stempel weicht vom Schreibzeitpunkt ab | 27.08. Vorschlag 1 | **behoben** — 2 Sekunden statt 7 Minuten |
| 8d-Zeile ohne n/3-Zähler | 27.08. Abschnitt 8 | **behoben, 58/58** |
| „stale" doppeldeutig | 27.08. Vorschlag 6 | **behoben, 100%** |
| Skript-Aufruf nicht belegt | 25.08. Vorschlag 2 / 27.08. Vorschlag 3 | **teilweise** — `cooldown_check` wörtlich ✓, `gate_check` redigiert ✗ |
| Prozessfehler selbst offengelegt | 24.08.: 3 · 25.08.: 3 · 27.08.: **0** | **28.08.: 10** — bester Wert aller vier Tage |
| `Spike-Ausnahme` fehlt | 25.08. / 27.08. (0/35) | **stark verbessert (4 statt 0), aber am Entry fehlend** |
| Undokumentierter Template-Wechsel | 27.08. Abschnitt 7 | **erneut** — 16:40, vier Änderungen gleichzeitig, keine offengelegt |
| Screenshot-Kadenz bricht ab, nicht offengelegt | 27.08. Abschnitt 7 | **erneut, verschärft** — diesmal zusätzlich falsch beziffert (Abschnitt 2) |
| Frischer Reclaim nicht als Trigger erkannt | 27.08. Abschnitt 3.1 | **erneut** — 16:30 Long-Dual-Gate, erst 17:05 rückwirkend benannt |
| Pflichtzeile trägt Urteil statt Rohwert | 25.08. Strukturbefund | **erneut, verlagert** — jetzt richtige Zahl von der falschen Zeitebene (Abschnitt 5) |
| Schattenmessung n=0 | 24./25./27.08. | **unverändert n=0**, vierter Testtag |
| Selbstbewertung des eigenen Tages | 25.08. / 27.08. Abschnitt 4.4 | **im Abschluss behoben, im Loop-Text erneut** (Abschnitt 15.6) |

Die obere Hälfte dieser Tabelle ist der beste Fortschrittsblock, den ich in vier Reviews gesehen habe: **sechs dokumentierte Fehlertypen sind an diesem Tag nicht wieder aufgetreten**, davon vier vollständig und messbar. Die untere Hälfte enthält drei Wiederkehrer, die sich alle auf denselben Nenner bringen lassen — das Regelwerk repariert zuverlässig **diskrete Ereignisse** und weiterhin unzuverlässig **schleichende Zustandsänderungen**.

---

## 15. Die 5 binären Pass-Kriterien des Validierungstesttags

Unter dem Vorbehalt aus Abschnitt 0(d) — der Tag lief solo, [[project_validierungstesttag_naechster_handelstag]] verlangt Levi im Loop.

### Kriterium 1 — wörtlicher `gate_check.cjs`-Output bei JEDER Gate-Entscheidung, 0 Ausnahmen
**Zweck erfüllt, Wortlaut verfehlt.** Es gab eine Gate-Entscheidung. Das Skript wurde aufgerufen, sein Ergebnis korrekt übernommen, kein RR im Kopf gerechnet. Der Block ist aber redigiert: 7 Zeilen fehlen, 5 sind umformuliert (Abschnitt 3). Das Kriterium sagt „wörtlich". **Mein Urteil: nicht bestanden**, mit dem ausdrücklichen Zusatz, dass die Substanz stimmt und der Abstand zum Bestehen sehr klein ist.

### Kriterium 2 — `zoneRrConsistencyGate` (GC-5) läuft mindestens einmal real durch
**Ausgeführt, aber nicht ausgeübt.** Die GC-5-Zeile erscheint und meldet „Nicht einschlaegig (SL-Floor bestanden, TP-Zone 2) — Selbsttest greift nur bei SL-Floor bestanden UND Zone 1". Das Gate hat also seine Eingangsprüfung durchlaufen und sich selbst als nicht zuständig erkannt; seine eigentliche Konsistenz-Logik ist nie gelaufen. Kriterium 3 enthält für genau diesen Fall eine ausdrückliche Öffnung („auch der Fall ‚Vorbedingung nicht erfüllt' zählt"); Kriterium 2 enthält sie **nicht**. **Mein Urteil: unentschieden — Levi muss entscheiden, welche Lesart gelten soll.** Meine Empfehlung: als „nicht bestanden" werten und die Öffnungsklausel aus Kriterium 3 für einen künftigen Versuch auch hier ergänzen, weil GC-5 sonst einen Zone-1-Trade abwarten muss, der Monate auf sich warten lassen kann.

### Kriterium 3 — Stale-Klasse mindestens einmal ausgewertet und korrekt ausgegeben
**Bestanden, eindeutig.** Achtmal ausgegeben, jedes Mal mit expliziter Vorbedingungs-Prüfung; der 18:00-Fall ist inhaltlich korrekt gerechnet. Zusätzlich hat die Terminologie-Regel zu 100% gehalten. Das ist das einzige Kriterium ohne Einschränkung.

### Kriterium 4 — sieben Pflicht-Messfelder nach jedem fiktiven Trade-Abschluss vollständig erhoben
**Nicht bestanden, klar.** `add_trade.cjs` wurde nicht aufgerufen — auch nicht als Trockenlauf, obwohl das Kriterium ihn ausdrücklich vorsieht („Ein sauberer Abbruch bei einem unvollständigen fiktiven `add_trade.cjs`-Testaufruf zählt als Beleg"). Keines der sieben Felder wurde erhoben. Sie wären alle verfügbar gewesen: `atr_at_entry` 45,4 · `entry_time` 18:00 · `sl_distance` 113 · `tp1_distance` 117 · `exit_type` BE bzw. STALL · `tp1_hit` false · `mfe_points` 66,35 (Entry 29.477 minus Bewegungstief 29.410,65 während der Position).

Die Vorgabe „kein `trades.db`-Schreibzugriff" schließt einen Testaufruf nicht aus — der Hard-Exit bei fehlenden Feldern greift *vor* dem Schreiben, das ist ja gerade der Punkt der Änderung vom 27.08. Damit ist die einzige Code-erzwungene Maßnahme des gesamten Reparaturzyklus an ihrem Prüftag nicht ein einziges Mal ausgelöst worden.

### Kriterium 5 — Tick-Prompt (Punkt 2b) läuft komplett durch, kein Pflichtelement übersprungen
**Nicht bestanden.** Mindestens sechs übersprungene Pflichtelemente, alle in Abschnitt 13 belegt: `Spike-Ausnahme` am 18:00-Trigger und an vier weiteren Trigger-Momenten; 5min-RSI/MACD-H in 25 von 58 Checks; Fibonacci-Offenlegung in 32; Positions-Kasten ohne Positionsgröße/Hebel/Zert.-Preis über die gesamte Trade-Laufzeit; Messfeld-Befüllung beim `gate_check`-Aufruf; 9d1-Vorrangklausel vor der Stall-Maßnahme.

### Gesamt
**1 klar bestanden (3), 1 unentschieden (2), 3 nicht bestanden (1, 4, 5).** Nach der Konsequenz-Regel des Testtags: **kein automatisches Echtgeld-Go für #44** — sondern „nur das konkret gescheiterte Element fixen (kein Rundumschlag, kein neues Regelwerk-Paket), danach ein weiterer, ebenso fokussierter Testtag".

Ich betone die zweite Hälfte dieses Satzes, weil sie leicht untergeht: Die Konsequenz ist **ein enger Fix und ein enger Wiederholungstag**, nicht ein weiteres Regelwerkspaket. Meine Vorschlagsliste unten ist entsprechend kurz gehalten.

---

## 16. Was heute wirklich gut war

Nicht als Ausgleich, sondern weil es unabhängig belegbar ist und weil vier Dinge davon zum ersten Mal überhaupt vorkommen:

- **Der Kerzenraster-Fix hat vollständig gehalten: 29 von 29 Slots, 0 versäumt** — gegenüber 5 Versäumnissen am 27.08. Der wichtigste Einzelbefund des Vortages ist damit repariert, und zwar durch Levis eigene Fassung von Punkt 1, nicht durch den ursprünglich vorgeschlagenen technischen Teilfix.
- **Der `x_last_fetch.json`-Zeitstempel ist jetzt korrekt gemessen** — 2 Sekunden Abstand zum Schreibzeitpunkt statt 7 Minuten.
- **Die 8d-Zeile trägt in 58 von 58 Checks ihren Zähler.** Kein einziger Rückfall auf die zählerlose Trend/Chop-Fassung.
- **Die „stale"-Terminologie ist zu 100% sauber.** Null Fehlverwendungen in 2287 Zeilen.
- **Zehn selbst offengelegte Prozessfehler**, darunter eine echte inhaltliche Selbstkorrektur (18:11, Stall-Flag zu früh gesetzt, Regel korrekt nachgelesen und zurückgenommen) und eine unaufgeforderte Doppelkorrektur (19:25). Nach null am 27.08.
- **Die Blackout-Regel hat ihre härteste Prüfung bestanden.** Ein vollständiger, technisch einwandfreier Short-Dual-Gate-Trigger um 16:15 wurde bewusst nicht gehandelt; der Markt hat die Entscheidung binnen 23 Minuten mit einer 200-Punkte-Gegenbewegung bestätigt. Zum ersten Mal in vier Testtagen gab es überhaupt einen vollständigen Trigger zum Widerstehen.
- **Die Kalender-Korrektur um 15:50.** Der Session-Start hatte die Warsh-Rede um über vier Stunden falsch datiert. Der Tweet-Fetch hat es gefangen, das Protokoll hat sofort korrigiert, die Konsequenz gezogen und den eigenen Fehler benannt. Genau der Cross-Check aus [[feedback_session_update]] Schritt 4, und er hat funktioniert.
- **Der Entry-AVWAP-Anker wurde gesetzt UND verifiziert** — frische Entity-ID geholt (nicht gecacht), `in_0` in Sekunden, Rücklese-Prüfung `in_0 != 0` plus bestätigte Wertänderung auf 717,60. Das ist der Paket-5-Schritt, vollständig und zum ersten Mal live.
- **Die Warte-Disziplin von 17:40 bis 18:00.** Zwanzig Einträge lang derselbe Satz: Kerze offen, kein Entry. Inklusive des intrakerzigen QQQ-Dips um 17:48, der ausdrücklich nicht als Bruch gewertet wurde.
- **Der Faktenprotokoll-Abschluss ist in seiner Form gelungen** (Abschnitt 17).
- **Kein fiktiver Trade in `trades.db`, kein `skipped_setups`-Eintrag, kein SQL-Dump verändert** — die Trennung fiktiv/real wurde vollständig eingehalten, die Datei ist seit dem 27.08. unangetastet.

---

## 17. Der Faktenprotokoll-Abschluss — erste Anwendung, und sie hat funktioniert

Das war die Frage mit dem größten Unsicherheitsanteil, deshalb gebe ich sie so genau wieder, wie ich sie prüfen konnte. Ich habe den Abschnitt zeilenweise gegen die Verbotsliste aus [[feedback_tagesabschluss]] gelesen.

**Struktur:** Alle fünf Unterpunkte vorhanden, in der vorgeschriebenen Reihenfolge und mit den vorgeschriebenen Überschriften. Der feste Schlusssatz steht wörtlich da.

**Verbotene Satztypen — Befund: keine.**
- *Gewichtungen* („wichtigster/stärkster/sauberster Vorfall"): keine gefunden. Die einzigen wertenden Adjektive sind deskriptiv („starker Aufwärtstrend", „vollständiger MTF-Vierschritt") und beziehen sich auf Marktbewegungen bzw. den Umfang eines Checks, nicht auf die Qualität des eigenen Handelns.
- *Selbstbescheinigungen* („regelkonform", „korrekt", „sauber gehandhabt"): **keine einzige**. Das ist der bemerkenswerteste Befund, denn der Loop-Text enthält sie mehrfach (unten) — der Abschluss hat sie durchgehend weggelassen. Der Trade wird als „Tranche 1: +23,15 Punkte. Tranche 2: ±0 Punkte" berichtet, ohne das „insgesamt ein Gewinn-Trade dank korrekt angewendetem Stall-Management", das im Loop-Text um 18:47 noch stand.
- *Vorgaben an den Prüfer* („zentraler Datenpunkt für das Review"): keine.
- *Lehren/Fazit*: keine.
- *Ursachendeutungen*: keine — Abschnitt 5 sagt dreimal ausdrücklich „Ursache nicht ermittelt", ohne Auflösungsversuch.

**Selbstoffenlegung nicht reduziert:** Abschnitt 4 listet zehn Brüche auf, darunter alle, die im Loop offengelegt wurden. Die Regel hat also nicht, wie befürchtet, die Offenlegung mit weggeschnitten.

**Der Befund ist also klar: das Format funktioniert.** Verglichen mit dem 27.08.-Tagesabschluss, der drei Bewertungen in drei Sätzen enthielt, ist das ein vollständiger Erfolg von Vorschlag 10.

**Zwei Einschränkungen, die davon unabhängig sind:**

**(a) Sachliche Richtigkeit ist eine andere Eigenschaft als Wertungsfreiheit.** Der Abschnitt ist wertungsfrei und an fünf Stellen falsch (Abschnitt 10). Das neue Format prüft das eine und nicht das andere. Eine wertungsfreie Bilanz, die sich selbst um 8 Fetches ärmer und um 7 Screenshots reicher rechnet, ist für den Prüfer nicht besser als eine bewertende — sie ist nur anders falsch. Und die Screenshot-Angabe steht in einem Unterpunkt, der ausdrücklich „Screenshots inkl. Zeitpunkt des letzten" verlangt.

**(b) Die Bewertungen sind nicht verschwunden, sie sind nach vorn gewandert.** Der Loop-Text selbst enthält weiterhin Sätze wie:
- 15:40: „Der **stärkste** Setup-Ansatz des bisherigen Tages hat sich vollständig selbst aufgelöst"
- 16:20: „Der Blackout hat sich damit im Nachhinein als **absolut richtig** erwiesen"
- 18:35: „Stall-Regel **korrekt und vollständig** angewendet"
- 18:47: „**Kein Regelverstoß, korrekte Anwendung** von Punkt 12.2/12.3 in Echtzeit"

Das sind formal keine Verstöße — die Verbotsliste gilt ausdrücklich nur für den Abschluss-Abschnitt. Aber es sind exakt die vier Satztypen, die die Regel verhindern soll, nur an einer Stelle, die sie nicht abdeckt. Der letzte Satz ist zudem, wie dieses Review zeigt, in seiner Absolutheit nicht haltbar (die Vorrangklausel wurde nicht geprüft). Ob das ein Problem ist, hängt davon ab, wofür die Rollentrennung da ist: Soll sie den *Abschluss* neutral halten, ist alles in Ordnung. Soll sie verhindern, dass der Prüfer eine vorformulierte Rahmung vorfindet, ist sie nur verschoben worden — denn Opus liest das ganze Protokoll, nicht nur den Schluss.

---

## 18. Priorisierte Vorschlagsliste

Kennzeichnung wie an den Vortagen: **(a)** = Prozess/Tooling/Textbereinigung, keine inhaltliche Regeländerung · **(b)** = echte Regeländerung, braucht Levis Zustimmung. **Nichts davon ist umgesetzt.**

Ich halte die Liste bewusst kürzer als am 27.08. (10 Vorschläge), weil die Konsequenz-Regel des Validierungstesttags ausdrücklich „nur das konkret gescheiterte Element fixen, kein Rundumschlag" vorsieht — und weil der Tag gezeigt hat, dass die letzten acht Vorschläge zum größeren Teil funktioniert haben. Die ersten drei sind die, die ich für die Wiederholung des Testtags für notwendig halte; 4-7 sind kleiner.

---

**1. (a, höchste Priorität) `gate_check.cjs`-Output in eine Datei schreiben und daraus zitieren, statt ihn abzutippen.**

Der Befund aus Abschnitt 3 lässt sich nicht durch eine schärfere Formulierung von „wörtlich" beheben — das steht schon da. Er lässt sich durch den Aufrufweg beheben: `node scripts/gate_check.cjs … | tee scripts/last_gate_check.txt`, und ins Protokoll kommt der Dateiinhalt. Dann ist „wörtlich" keine Disziplinfrage mehr, sondern ein Kopiervorgang, und der Prüfer kann die Datei gegenlesen. Dasselbe für `cooldown_check.cjs`, obwohl es dort heute funktioniert hat — der Mechanismus soll nicht davon abhängen, welches Skript kürzer ist.

Kosten: eine Zeile im Tick-Prompt. Kein Regelinhalt ändert sich. Das ist mein wichtigster Vorschlag, weil Kriterium 1 des Validierungstesttags sonst beim nächsten Versuch aus demselben Grund wieder scheitert.

---

**2. (a) Rohwert-Pflicht auf die EINGABEN von `gate_check.cjs` ausdehnen, nicht nur auf seine Ausgabe.**

Der 18:00-Aufruf enthält zwei Zahlen, die im gesamten Protokoll an keiner anderen Stelle vorkommen:
- `--atr 45.4` — der ATR(5min) wird nirgends abgelesen oder zitiert. Schritt 1 des 5-Schritte-Ablaufs aus Punkt 7b1 heißt „ATR ablesen"; im Protokoll erscheint der Wert erstmals als Kommandozeilen-Argument.
- `--cluster-level 29563.95` — als „bereits per Wick getestete Zone" bezeichnet. Diese Zone wird nirgends im Tagesverlauf benannt oder hergeleitet.

(`--tp2-level-price 29213.90` ist dagegen vorbildlich: es ist der Session-Pivot S2 aus dem 14:31-Update und wird auch so benannt.)

Der 28.08.-Rohwert-Fix adressiert die Pflichtzeilen im Voll-Check. Er sagt nichts über die Zahlen, die in das Gate hineingehen — und genau die entscheiden das Ergebnis. Vorschlag: eine kurze Herkunftszeile vor dem Aufruf, im Muster `Gate-Eingaben: ATR(5min) 45,4 (abgelesen <Quelle/Zeit>) | Cluster-Level 29.563,95 (<welcher Wick, wann>) | TP2-Level 29.213,90 (Session-Pivot S2)`. Reine Ergänzung, keine neue Prüfung.

---

**3. (a) Regime-Gate 8d: alle drei Kriterien einzeln ausweisen, nicht nur die Summe.**

Abschnitt 6 ist der einzige Befund des Tages mit einer möglichen Trade-Folge. Der Zähler steht 58 von 58 Mal da und beruht auf einem einzigen tatsächlich geprüften Kriterium. Vorschlag: das Format um die Einzelbewertung erweitern, z.B.

`Regime-Gate (8d): Schock-Tag ✗ (0/3) [Range 198 Pkt / ATR-D 465,1 = 0,43× ✗ | Makro-Häufung ✗ | VIX-Range nicht gemessen] | Regime: Trend`

Damit wird sichtbar, welches Kriterium geprüft und welches nicht gelesen wurde — exakt dasselbe Prinzip wie die Rohwert-Pflicht, nur eine Ebene tiefer. Der VIX-Teil braucht dafür einmal pro Voll-Check einen Wert; falls das zu teuer ist, reicht „nicht gemessen" als ehrliche Alternative. Keine neue Schwelle, kein neues Gate — nur die Aufschlüsselung einer bereits bestehenden Zahl.

---

**4. (a) Die Voll-Check-Nummer nicht mehr im Text fortschreiben, sondern bei jedem Check neu aus `Bash date` rechnen — mit der Rechnung im Kopf der Zeile.**

Abschnitt 9 zeigt, dass der Zähler nicht „mitgezählt" hat, sondern mit dem Fire-Zähler verschmolzen ist. Ein Verbot des Mitzählens (das steht seit dem 28.08. schon da) hilft dagegen nicht — die Nummer muss aus einer Quelle kommen, die nicht mitzählen kann. Vorschlag: die Kopfzeile trägt die Rechnung selbst, z.B. `Voll-Check 18:35 (Nr. 43 = (215 Min seit 15:05)/5+1)`. Wer die Rechnung hinschreibt, kann nicht danebenliegen, und der Prüfer sieht die Herleitung. Kostet acht Zeichen.

---

**5. (b, braucht Zustimmung) Den Ort für die sechs Tagesabschluss-Pflichtzeilen an fiktiven Testtagen klären.**

Abschnitt 7.4 beschreibt eine unbeabsichtigte Nebenwirkung der Umstellung vom 28.08.: `SL-Hit-Typ`, `DB-Sync`, `Regelkonformität geprüft`, `Skipped-Setups aufgelöst`, `SQL-Dump aktualisiert` und `B1-B3-Schattenmessung geprüft` gehören laut [[feedback_tagesabschluss]] in den Tagesabschluss. Den gibt es im Loop-Protokoll nicht mehr, und an einem fiktiven Testtag gibt es auch keine Fable-Datei, in der er stünde. Die Zeilen sind damit heimatlos.

Zwei saubere Wege: **(i)** einen sechsten Unterpunkt „Pflicht-Abschlusszeilen" ans Faktenprotokoll anhängen, rein faktisch befüllt („SL-Hit-Typ: Wick/Spike — Punkt-11: 0/4"), was mit der Wertungsfreiheit vereinbar ist, weil es Messwerte sind; oder **(ii)** ausdrücklich festhalten, dass diese sechs Zeilen an fiktiven Testtagen entfallen und erst mit dem Echtgeld-Betrieb wieder greifen. Ich empfehle **(i)** — der Punkt-11-Zähler zum SL-Zeitpunkt ist genau die Sorte Zahl, die sich später nicht mehr rekonstruieren lässt, und der SL-Hit vom 18:47 hätte mit 0/4 gezeigt, dass die These nicht widerlegt war.

---

**6. (a) Kriterium 2 des Validierungstesttags dieselbe Öffnungsklausel geben wie Kriterium 3.**

Kriterium 3 zählt den Fall „Vorbedingung nicht erfüllt, Zeile entfällt korrekt" ausdrücklich als bestanden. Kriterium 2 (GC-5) hat diese Klausel nicht, obwohl GC-5 konstruktionsbedingt nur bei „SL-Floor bestanden UND TP-Zone 1" anspringt — eine Konstellation, die nach der aktuellen 8b1-Zonenlogik selten ist. Ohne Öffnungsklausel ist Kriterium 2 an einem beliebigen Testtag nur mit Glück erfüllbar. Vorschlag: „auch die korrekte Ausgabe ‚nicht einschlägig' mit Begründung zählt als Durchlauf" — oder alternativ das Kriterium ersetzen durch einen gezielten Offline-Testaufruf mit Zone-1-Parametern.

---

**7. (a) `add_trade.cjs` bei fiktiven Testtagen als Trockenlauf mitlaufen lassen.**

Kriterium 4 sieht das bereits vor, das Protokoll hat es nicht gemacht, und ich vermute — **Einschätzung** — dass die Vorgabe „kein `trades.db`-Schreibzugriff" als Verbot des Aufrufs gelesen wurde. Vorschlag: im Tick-Prompt bzw. im Testtag-Auftrag ausdrücklich klarstellen, dass ein Aufruf mit unvollständigen Feldern erwünscht ist, weil der Hard-Exit **vor** dem Schreiben greift — und dass die sieben Messwerte auch am fiktiven Tag am Trade-Ende zusammengetragen werden. Ohne das bleibt die einzige code-erzwungene Maßnahme des gesamten Reparaturzyklus ungetestet.

---

## 19. Was ich ausdrücklich NICHT vorschlage

- **Keine Änderung am Dual-Gate, am Blackout oder an Punkt 12.** Alle drei haben an diesem Tag inhaltlich richtig entschieden: das Gate hat einen sauberen Trigger produziert, der Blackout hat einen Verlust verhindert (und einen Gewinn gekostet, was dazugehört), und der Stall-Trigger war arithmetisch korrekt. Was fehlte, war ein Prüfschritt innerhalb von Punkt 12, nicht Punkt 12 selbst.
- **Keine neue Pflichtzeile.** Der Tag hat 58 Voll-Checks mit je ~10 Pflichtelementen produziert; fünf davon liefen zu 100%, drei brachen. Das Problem ist nicht die Zahl der Zeilen nach oben, sondern die Verlässlichkeit der bestehenden. Alle sieben Vorschläge oben ändern Form, Quelle oder Ort bestehender Zeilen; keiner fügt eine hinzu.
- **Keine Abwertung des Tages wegen der Screenshot-Bilanz.** Der Befund ist hart und muss stehen, aber er betrifft eine *Zählung im Abschlussbericht*, nicht die Beobachtung selbst — sechs Screenshots wurden gemacht, sie fehlten nur in der zweiten Tageshälfte und in der Bilanz.
- **Keine Umdeutung des +23,15-Punkte-Trades zum Beleg für irgendetwas.** Ohne Positionsgröße ist er in € und % nicht ausdrückbar, und ein einzelner Trade belegt ohnehin keine Edge. Er belegt, dass die Kette Trigger → Gate-Check → Entry → Positions-Management → Exit einmal live durchgelaufen ist. Das ist viel, und es ist genau das, wofür der Tag gedacht war.
- **Keine Lockerung von Kriterium 1**, um den redigierten `gate_check`-Block nachträglich bestehen zu lassen. Das Kriterium hat Levi bewusst binär gesetzt; der richtige Weg ist Vorschlag 1, nicht eine weichere Formulierung.
- **Kein Vorwurf aus `skipped_setups` = 0.** Punkt 2b nimmt fiktive Testtage ausdrücklich aus („dort genügt die Protokollzeile"). Der eigentliche Befund liegt bei der Messfeld-Befüllung des `gate_check`-Aufrufs (Abschnitt 13.3), nicht bei der Tabelle.

---

## 20. Ehrliche Gesamteinordnung

Der 28.08. ist der beste der vier Testtage — und er besteht die eigene Prüfung trotzdem nicht.

**Das Gute ist echt, gut belegt und teilweise beispiellos.** Sechs am Vortag beschlossene Änderungen haben an ihrem ersten Einsatztag funktioniert, vier davon vollständig und messbar: der Kerzenraster-Fix (29/29 Slots statt 17/22), der 8d-Zähler (58/58 statt 26/35), die „stale"-Terminologie (100%), der maschinennähere `x_last_fetch.json`-Stempel (2 Sekunden statt 7 Minuten). Die Offenlegungspflicht hat aus null Selbstoffenlegungen zehn gemacht, darunter eine inhaltlich echte Selbstkorrektur mitten in einer offenen Position. Der Faktenprotokoll-Abschluss ist in seiner Form ein voller Erfolg — kein einziger der fünf verbotenen Satztypen. Und zum ersten Mal in vier Testtagen gab es überhaupt etwas zu widerstehen: ein vollständiger, technisch einwandfreier Short-Trigger um 16:15, den der Blackout blockiert hat und den der Markt 23 Minuten später widerlegt hat.

**Das Unbequeme hat eine gemeinsame Form, und sie ist neu.** Am 25.08. brachen die Pflichtzeilen, weil sie Urteile statt Rohwerte trugen. Am 27.08. brachen sie, weil der Rohwert falsch gemessen war. Am 28.08. brechen sie nicht mehr — sie stehen alle da, in 58 von 58 Fällen, mit Zahlen. Was jetzt bricht, ist die Ebene dahinter: der 8d-Zähler steht, aber nur ein Drittel seiner Kriterien wurde je bewertet. Die ADX-Zahl steht, aber achtmal von der falschen Zeitebene. Der `gate_check`-Output steht, aber redigiert. Die Messfeld-Werte wurden berechnet, aber nicht übergeben. Die Screenshot-Zeile steht, aber mit einer Zahl, die die Dateien im Repo widerlegen.

Das ist kein Rückschritt — es ist die Stelle, an die man kommt, wenn die vorherige Stelle repariert ist. Aber es hat eine unangenehme Eigenschaft: **Ein Formfehler ist von außen sofort sichtbar, ein Inhaltsfehler hinter korrekter Form nicht.** Der 27.08. sah im eigenen Abschluss schlechter aus, als er war. Der 28.08. sieht besser aus, als er ist. Das ist für ein System, das sich über Selbstprotokollierung steuert, die schwierigere Lage.

**Zur Gewichtung, damit das Bild stimmt:** Es war ein fiktiver Tag, ohne Risiko, mit einem simulierten Trade, der +23,15 Punkte auf der halben Position abwarf. Keiner der Befunde hatte eine Geldfolge. Der schwerste — die nie evaluierten 8d-Kriterien 2 und 3 — hätte in der ungünstigen Auflösung den einzigen Trade des Tages verhindert, und dieser Trade war ein Gewinner. Der zweitschwerste — die fehlende Positionsgröße — kostet keine Entscheidung, sondern die Messbarkeit.

**Und zur eigentlichen Frage, weil der Tag ausdrücklich dafür angesetzt war:** Die Entry-Rechenkette ist zum ersten Mal seit dem 25.08. wieder live gelaufen, und diesmal war sie in ihren Zahlen fehlerfrei — RR, SL-Floor, TP-Zonen, TP2-Level, Cluster-Puffer, alle acht Gates reproduzieren zeichengenau, als ich das Skript nachgerechnet habe. Das ist der zentrale Fortschritt gegenüber dem 25.08., an dem genau diese Kette arithmetisch unmöglich war. Was fehlt, ist nicht mehr die Rechnung, sondern ihre Ränder: die Herkunft der Eingaben, die Vollständigkeit des Zitats, die Übergabe der Messwerte und die Umrechnung des Sizing-Flags in eine Positionsgröße. Das sind vier eng umrissene Lücken, und sie sind an einem einzigen weiteren, fokussierten Testtag zu schließen — genau so, wie [[project_validierungstesttag_naechster_handelstag]] es für den Fall „mindestens ein Kriterium nicht bestanden" vorsieht.

---

*Levi entscheidet über alle Vorschläge. Bis dahin ist nichts umgesetzt, keine Regel geändert, keine Datei außer dieser angelegt.*
