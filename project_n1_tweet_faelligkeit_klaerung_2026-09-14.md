---
name: project-n1-tweet-faelligkeit-klaerung-2026-09-14
description: "Aufklaerung des N1-Widerspruchs aus dem Opus-Gegencheck P1 (14.09.2026): Gilt fuer den Tweet-Fetch das Kerzenraster allein (28.08.2026, feedback_live_trading.md) oder die Doppelbedingung Raster UND Delta>=10 (03.09.2026 Punkt 17, feedback_vollcheck_format.md + x_fetch_stamp.cjs + vollcheck.cjs + loop_prompt.cjs-String)? Ergebnis: AUSGANG 2 — die Doppelbedingung ist der aktuelle, maschinell erzwungene Stand; feedback_live_trading.md ist die einzige Datei, die noch die 28.08.-Fassung fuehrt, und die heutige W1-Korrektur hat sie versehentlich verschaerft statt nachgezogen. Der Nachzug war bereits am 03.09.2026 als K-1b/K-1c beauftragt und nie ausgefuehrt. Enthaelt 7 Fundstellen mit Vorher/Nachher-Vorschlag, zwei Folgefunde (Nachhol-Regel vom Skript nicht umgesetzt; 'last_fetch' ist der Zeitstempel des juengsten TWEETS, nicht des letzten Abrufs) und die Messung am Testtag 10.09. (0 echte Divergenzen an Raster-Slots, aber 11 UEBER-POLLING-Marken, davon mind. 4 allein durch die Delta-Haelfte erzeugt). Klaerung, keine Umsetzung, kein Commit."
metadata:
  node_type: memory
  type: project
  status: "F-B FREIGEGEBEN MIT AUFLAGEN (Opus-Gegencheck 14.09.2026) — Blocker aus dem P2-Gegencheck behoben: eigener Lauf 37 VC/3 h mit 0 falschen Nachholen-JA (alte Semantik: 37/37). Auflagen 1-3 erfuellt (8 eigene Grenzfaelle, 90-Min-Lauf mit ausgefallenem VC). AUFLAGE A-1 vor dem naechsten Testtag: Live-x_last_fetch.json hat noch kein last_poll -> einmal stempeln, sonst laeuft der Migrations-Fallback = altes Verhalten. Nachbesserungen N-1 (Stempel nicht maschinell erzwungen; ohne ihn kehrt der Blocker zu 12/13 zurueck) bis N-4. Opus-Fix O-1: Stempelpflicht in feedback_loop_ablauf_uebersicht.md Schritt 6 ergaenzt. Tests 90/90, loop_prompt Exit 0. Davor: N1-Textkorrektur FREIGEGEBEN (77/77)"
  originSessionId: session_01E4zVYdeDA5XiMXhwykDWcw
  modified: 2026-09-14T20:01:57.902Z
---

# N1 — Tweet-Fetch-Fälligkeit: Kerzenraster allein oder Doppelbedingung? (Klärung, 14.09.2026, Opus 5)

**Auftrag:** Den im Opus-Gegencheck P1 ([[project_cron_automatisierung_einschaetzung_2026-09-14]], Abschnitt 2 + Auflage N1) gefundenen Widerspruch lückenlos aufklären: Hat die heutige W1-Korrektur in [[project_pruefung_feedback_live_trading_2026-09-14]] eine legitime, später eingeführte UND-Verknüpfung entfernt?

**Was dieser Bericht ist:** eine Klärung mit Empfehlung. **An Regelwerk, Code, Tests oder DB wurde nichts geändert** — kein Commit, kein Push ([[feedback_dont_change_running_system]], Punkt 14 Regeländerungs-Tempo-Bremse). Alle Zitate sind am 14.09.2026 aus den Primärquellen gelesen (Regeldateien, `scripts/x_fetch_stamp.cjs`, `scripts/vollcheck.cjs`, `scripts/loop_prompt.cjs`, Git-Historie beider Repos, Testtag-Protokoll 10.09.), nicht aus den Berichten darüber ([[feedback_memory_pflege]], Zitierpflicht).

---

## 1. Kurzfazit

**AUSGANG 2 — mit einer Präzisierung.** Die Doppelbedingung (**Raster-Slot Minute % 10 == 0 UND Delta ≥ 10 Min**) ist der aktuelle, gültige und seit 09.09.2026 **maschinell erzwungene** Stand. Sie wurde am 03.09.2026 als „Punkt 17" eingeführt und steht heute in [[feedback_vollcheck_format]], [[feedback_loop_ablauf_uebersicht]], in `x_fetch_stamp.cjs`, in `vollcheck.cjs` (das einen Fetch ohne erfüllte Doppelbedingung als `ÜBER-POLLING ✗` brandmarkt) und im hartkodierten String von `loop_prompt.cjs`. **`feedback_live_trading.md` ist die einzige Quelle, die noch die reine 28.08.-Kerzenraster-Fassung führt** — an sieben Stellen.

**Die Präzisierung:** Die W1-Korrektur war in ihrem Kern **richtig** — die 25.08.-Behauptung „das Delta ist die *alleinige verbindliche* Fälligkeitsprüfung" ist tatsächlich aufgehoben, und die drei markierten Stellen (Z458/Z968/Z985) trugen zu Recht einen Überholungsmarker. Falsch ist nur die **Absolutheit der neuen Formulierung**: „Fällig ist der Fetch **AUSSCHLIESSLICH** bei echter Minute % 10 == 0; das Delta entscheidet **NICHT** mehr über die Fälligkeit". Damit wurde aus „Delta ist nicht *allein* maßgeblich" ein „Delta ist *gar nicht* maßgeblich" — und das negiert Bedingung (b) von Punkt 17 wörtlich. W1 muss also **nicht zurückgenommen, sondern zu Ende geführt** werden.

**Der Nachzug ist nicht neu, sondern seit elf Tagen offen und war bereits beauftragt.** Opus hat am 03.09.2026 in [[project_opus_finalcheck_2026-09-03]] als Befund **K-1b** exakt diese vier Fundstellen in `feedback_live_trading.md` benannt und den Nachzug als „Drei-Zeilen-Einzeiler … vor dem nächsten Testtag" gefordert; [[project_testtag_regelwerk_ueberarbeitung_2026-09-03]] Z38 führt ihn als offenen Punkt. Er wurde nie ausgeführt. Die W1-Korrektur hat heute die Gegenseite dieses offenen Widerspruchs verstärkt.

---

## 2. Vollständige Chronologie der Fälligkeitsregel (mit Belegen)

| # | Datum | Fassung | Beleg (Primärquelle) |
|---|---|---|---|
| 1 | 10.07.2026 | Tweet-Fetch bei **jedem 2. Voll-Check** (alle 10 Min), per eigener Zählung | `feedback_live_trading.md` Z450 |
| 2 | 13.07.2026 | Zählung ersetzt: echte Minute per `Bash date`, **% 10** | Z458 („How to apply, korrigiert 13.07.2026 — ersetzt die alte ‚intern mitzählen'-Anweisung") |
| 3 | 24.08.2026 | Schwelle auf **60 Min** (Opus-Vollcheck Punkt 11) | Z450 (durchgestrichen), Z454 |
| 4 | 25.08.2026 | Levi revidiert zurück auf **10 Min**; Mechanik wechselt von Modulo auf **Ist-Zeit-Delta** gegen `x_last_fetch.json`, ausdrücklich als „alleinige verbindliche Fälligkeitsprüfung" | Z452, Z458, Z985 |
| 5 | **28.08.2026** | **Kerzenraster-Fix:** Delta driftete (27.08.: fünf versäumte Pflicht-Fetches, Zeitstempel bis 7 Min neben dem echten Schreibzeitpunkt) → Fälligkeit **ausschließlich** echte Minute % 10 == 0; `x_last_fetch.json` nur noch Verpasst-Erkennung | Z462–Z468; `feedback_vollcheck_format.md` Z71 (dort heute **durchgestrichen**) |
| 6 | **03.09.2026** | **Punkt 17 — Doppelbedingung:** „‚fällig' darf in der Tweet-Check-Zeile nur stehen, wenn **BEIDE** Bedingungen erfüllt sind: (a) Raster-Slot … **UND** (b) das Minuten-Delta … ist tatsächlich ≥10". Gleichzeitig wird `x_fetch_stamp.cjs` der einzige Schreib- und Rechenweg. Anlass: am 02.09. degenerierte das behauptete 10-Min-Raster ab VC#27 auf einen faktischen 5-Min-Takt (61 Abrufe statt ~42) | `feedback_vollcheck_format.md` **Z89** (wörtlich, „ergänzt 03.09.2026, Punkt 17") + Z42 + Z71; `feedback_loop_ablauf_uebersicht.md` Z40; Commit `72ec8f6` (03.09.) legt `x_fetch_stamp.cjs` an — dessen `--check` rechnet zu diesem Zeitpunkt **nur das Delta** (`const faellig = deltaMin >= schwelle;`), das Raster wird von Hand geprüft |
| 7 | **09.09.2026** | **TODO 4:** Das Raster wandert in den Code. `x_fetch_stamp.cjs --check` druckt jetzt Delta-, Raster- **und** Doppelbedingungs-Zeile; `vollcheck.cjs` ruft es als Unterprozess auf und kippt die Format-Zeile in beide Richtungen. Anlass: am 09.09. liefen 18/18 Voll-Checks auf der falschen Rasterphase (alle Fetches bei Minute % 10 == 5) | Commit `c52346a` (09.09.); Kopfkommentar `x_fetch_stamp.cjs` („TODO 4 … zusaetzlich die RASTER-Bedingung … und das Ergebnis der DOPPELBEDINGUNG"); `vollcheck.cjs` Z805–831 |
| 8 | 14.09.2026 | **W1-Korrektur** in `feedback_live_trading.md`: die 25.08.-Delta-Festschreibung wird gestrichen, die Kerzenraster-Fassung mit „AUSSCHLIESSLICH" / „das Delta entscheidet NICHT mehr über die Fälligkeit" verschärft — ohne Kenntnis von Schritt 6/7 | `feedback_live_trading.md` Z458 (F1), Z968, Z985 (F2); [[project_pruefung_feedback_live_trading_2026-09-14]] Abschnitt „W1-W5 korrigiert" |

**Der entscheidende, bisher übersehene Beleg:** Am 03.09.2026 hat der Opus-Finalcheck den heutigen Zustand bereits vorhergesagt und beauftragt — [[project_opus_finalcheck_2026-09-03]], Befund **K-1b**:

> „Beide frisch korrigierten Templates verweisen für Bedingung (a) auf [[feedback_live_trading]] Punkt 9 (‚Kerzenraster-Fix'). Wer diesem Verweis folgt, liest dort weiterhin **das Gegenteil der neuen Regel** — an vier Stellen … Nachzug ist derselbe Einzeiler wie bei den anderen beiden Dateien: Historie-Klammer vor Z. 365/371, Item (2) in Z. 89–91 um ‚UND Delta ≥ 10 Min per `x_fetch_stamp --check`' ergänzen."

Die vier damals genannten Zeilen sind heute Z95–105 (Cron-Fence Item (2)), Z462/466, Z468 und Z443. **Alle vier stehen unverändert da**; drei weitere (Z458, Z968, Z981/985) sind heute dazugekommen bzw. wurden heute verschärft.

---

## 3. Die drei unabhängigen Quellen — Messung

| Quelle | Was sie sagt | Stand |
|---|---|---|
| **(a) Regeltext `feedback_live_trading.md`** (7 Fundstellen, s. Abschnitt 5) | Raster **allein**, „Delta entscheidet NICHT" | 28.08.2026, heute verschärft |
| **(a') Regeltext `feedback_vollcheck_format.md` Z42/Z71/Z89** | **Doppelbedingung**; die 28.08.-Fassung ist dort **ausdrücklich durchgestrichen** mit Vorsatz „Der folgende Absatz beschrieb die Raster-Bedingung noch als ALLEINIGE Fälligkeitsgrundlage und ist insoweit Historie" | 03.09.2026 |
| **(a'') Regeltext `feedback_loop_ablauf_uebersicht.md` Z40** | **Doppelbedingung**, identische Historie-Behandlung | 03.09.2026 |
| **(b) Code `x_fetch_stamp.cjs`** | `const rasterSlot = deMin % 10 === 0;` / `const deltaOk = deltaMin >= schwelle;` / **`const faellig = rasterSlot && deltaOk;`** — druckt wörtlich „Doppelbedingung (Raster UND Delta >= 10 Min): FAELLIG JA/NICHT FAELLIG" | 09.09.2026 |
| **(b') Code `vollcheck.cjs` Z805–831** | liest genau diese Zeile (`/FAELLIG JA/`) und kippt die Pflichtzeile: `faellig && !fetch` → `ÜBERFÄLLIG ✗`; `!faellig && fetch` → **`ÜBER-POLLING ✗`** | 09.09.2026 |
| **(c) Prompt-String `loop_prompt.cjs` Z200** | „`--tweet-fetch ja` NUR, wenn die **Doppelbedingung (Minute % 10 == 0 UND Delta >= 10)** laut Skript FAELLIG JA ergibt" | 09.09.2026 |

**Mehrheit 5:1 für die Doppelbedingung — aber die Mehrheit ist hier nicht das Argument.** Entscheidend ist dreierlei:

1. **Rezenz:** Die Doppelbedingung ist nachweislich sechs Tage jünger als die Raster-allein-Fassung, und ihre beiden Anlässe (02.09. Über-Polling, 09.09. falsche Rasterphase) liegen beide *nach* dem 28.08.
2. **Vollzug:** Nur die Doppelbedingung wird ausgeführt. Die Raster-allein-Fassung ist seit dem 09.09. nicht mehr befolgbar — wer ihr folgt und auf Raster-Slot ohne Delta fetcht, bekommt von `vollcheck.cjs` ein ✗ in dieselbe Pflichtzeile geschrieben. **Ein Blindfix in Richtung „Raster allein" würde einen Prompt erzeugen, der eine Handlung verlangt, die das Skript im selben Voll-Check als Regelbruch protokolliert.**
3. **Fachliche Prüfung (unabhängig von beidem):** Die beiden Bedingungen messen **verschiedene Dinge** und schließen sich nicht aus.
   - (a) Raster ist der **driftfreie Taktgeber**: Er beantwortet „ist jetzt ein Fetch-Zeitpunkt?" und ist die Antwort auf die 27.08.-Drift. Er ist notwendig.
   - (b) Delta ist ein **Doppel-Fetch-Schutz**: Er beantwortet „habe ich gerade eben schon abgerufen?" und ist die Antwort auf den 02.09.-Über-Polling-Schaden (drei leere `@DeItaone`-Antworten, plausible Rate-Limit-Folge). Er ist konservativ und **kann keinen legitim fälligen Fetch verhindern**: Da der Wasserstand nie *nach* dem letzten Abruf liegen kann, folgt aus „Delta < 10" zwingend „letzter Abruf liegt weniger als 10 Min zurück".
   
   Eine reine %10-Prüfung lässt den Anlass-Fetch-Fall offen (Kalender-Release 15:58, Raster-Slot 16:00 → zwei Abrufe in zwei Minuten). Eine reine Delta-Prüfung driftet (27.08.). **Die UND-Verknüpfung ist fachlich die richtige Konstruktion** — jede Bedingung schließt genau die Lücke der anderen.

**Ergebnis: Ausgang 2.** Es sind nicht zwei getrennte Konzepte (Ausgang 3) — beide Texte regeln denselben Gegenstand: wann in einem Voll-Check der Tweet-Fetch auszuführen ist und was in der Pflichtzeile `Tweet-Check:` stehen darf. Und der Skript-Kommentar ist kein Irrtum (Ausgang 1): „Punkt 17, 03.09.2026" ist als Regelherkunft korrekt; nur die **Maschinenumsetzung** des Rasters kam am 09.09. nach (TODO 4). Beide Daten sind jünger als der 28.08.

---

## 4. Zwei Folgefunde, die bei der Gelegenheit mit auf den Tisch gehören

Beide sind **neu** und in keinem der bisherigen Berichte enthalten. Keiner von beiden ändert das Urteil zu Ausgang 2, aber beide sind Teil derselben Regel und sollten in einem Zug mitentschieden werden.

### F-A · Die Nachhol-Regel ist vom Skript nicht umgesetzt — und wird von ihm sogar bestraft

`feedback_live_trading.md` Z468 (Nachhol-Regel, 28.08., **unverändert gültig**) verlangt: „Liegt der letzte Fetch-Zeitstempel VOR dem Beginn des letzten fälligen Raster-Slots, wurde ein Slot verpasst → beim **aktuellen** Voll-Check sofort nachholen und als `ÜBERFÄLLIG ✗ — Slot HH:M0 verpasst, jetzt nachgeholt` ausweisen." `feedback_vollcheck_format.md` Z42 führt denselben Zweig.

`x_fetch_stamp.cjs` kennt diesen Fall **nicht**: `faellig = rasterSlot && deltaOk` ist an der Minute % 10 == 0 verankert. Ein Nachhol-Fetch findet per Definition **außerhalb** des verpassten Slots statt (z.B. VC 16:05 nach verpasstem 16:00) → `rasterSlot = NEIN` → `NICHT FAELLIG` → `vollcheck.cjs` schreibt `ÜBER-POLLING ✗`. **Die geltende Nachhol-Regel und die geltende Doppelbedingung widersprechen sich im Code.** Der `ÜBERFÄLLIG`-Zweig in `vollcheck.cjs` (Z828) greift nur, solange man noch *innerhalb* der Rastermiute ist — also genau dann, wenn nichts verpasst wurde.

*Empfehlung (Code, nicht Regelwerk): `x_fetch_stamp.cjs --check` um eine dritte Ausgabezeile `Nachholen (letzter Fetch vor Beginn des letzten Raster-Slots HH:M0): JA/NEIN` ergänzen und `faellig = (rasterSlot && deltaOk) || nachholen` setzen. Das ist P2-Kaliber (Skriptänderung an einem Live-Pfad, eigener Gegencheck) — hier nur benannt.*

### F-B · `last_fetch` ist der Zeitstempel des jüngsten **Tweets**, nicht des letzten **Abrufs**

`x_fetch_stamp.cjs --set` erwartet laut eigenem Kopfkommentar ausdrücklich „den UTC-Zeitstempel des **JUENGSTEN tatsaechlich gesehenen Tweets** (NICHT ‚jetzt + Raster', NICHT DE-Ortszeit)". Das „Delta" ist damit das **Alter des jüngsten Tweets**, nicht der Abstand zum letzten Poll. Konsequenzen, beide am 10.09. messbar:

- **Bedingung (b) ist an ruhigen Tagen praktisch inert:** Am 10.09. lief der Wasserstand ab 16:44 nicht mehr weiter (keine neuen Tweets), während real alle 10 Min gefetcht wurde. Das Protokoll zeigt „letzter Fetch vor 46 / 56 / 66 Min" bei Fetches im 10-Minuten-Takt — Delta ≥ 10 war den ganzen Nachmittag trivial erfüllt. Der Über-Polling-Schutz greift also ausgerechnet dann nicht, wenn nichts passiert.
- **Bedingung (b) erzeugt Falsch-✗, wenn Nachrichten fließen:** Wird nach einem korrekten Raster-Fetch `--set` auf einen frischen Tweet gesetzt, *bevor* `vollcheck.cjs` läuft, misst die Nachprüfung ein Delta von wenigen Minuten → `NICHT FAELLIG` → `ÜBER-POLLING ✗` auf einen regelkonformen Fetch. Vier so dokumentierte Fälle am 10.09. (VC#45/47/53/55, Protokoll Z1919/Z2034/Z2356/Z2460: „kein echter Doppel-Fetch, sondern eine Variante des bereits dokumentierten Timing-Artefakts").

*Das ist kein Argument gegen die Doppelbedingung, sondern gegen die aktuelle Semantik des Wasserstands. Sauber wäre: zwei Felder (`last_fetch` = Abrufzeit für Bedingung (b), `last_tweet` = Wasserstand für den Digest). Ebenfalls P2-Kaliber, hier nur benannt.*

---

## 5. Konkreter Korrekturvorschlag (Ausgang 2)

**Grundsatz: eine Fassung gilt, die andere trägt einen Marker.** Empfohlen wird, `feedback_live_trading.md` auf den 03.09.-Stand nachzuziehen (nicht umgekehrt) — weil Code, zwei weitere Regeldateien und der Cron-Prompt-String bereits dort stehen und weil ein Rückbau `x_fetch_stamp.cjs`, `vollcheck.cjs`, `loop_prompt.cjs` und zwei Regeldateien mitziehen müsste.

**Sieben Fundstellen in `feedback_live_trading.md`** (Zeilennummern am Stand 14.09., 1.229 Zeilen, nach P1):

**(1) Z95–105 — Cron-Fence Item (2), CRON-BAUSTEIN-FORMAT-TWEET. Die wichtigste Stelle: sie wandert seit P1 bei JEDEM Fire in den Cron-Prompt.**

*Vorher (wörtlich):*
```
(2) Tweet-Fetch-Fälligkeit am FESTEN KERZENRASTER prüfen (Levi-Vorgabe 28.08.2026,
    "Kerzenraster-Fix" — ersetzt die Delta-seit-letztem-Ist-Abruf-Logik, siehe Punkt 9):
    Ist die echte Minute (per Punkt 9a) % 10 == 0 → Pflicht-Fetch (= jede zweite
    abgeschlossene 5-Min-Kerze, Slots :00/:10/:20/:30/:40/:50). Zusätzlich
    x_last_fetch.json lesen ...
```
*Nachher (Vorschlag, wörtlich):*
```
(2) Tweet-Fetch-Fälligkeit = DOPPELBEDINGUNG, gerechnet statt behauptet (Punkt 17,
    03.09.2026; maschinell seit 09.09.2026, TODO 4): (a) echte Minute (per Punkt 9a)
    % 10 == 0 (Kerzenraster-Fix 28.08.2026, Slots :00/:10/:20/:30/:40/:50) UND
    (b) Delta aus x_last_fetch.json >= 10 Min. Beides rechnet
    `node scripts/x_fetch_stamp.cjs --check` fertig; NUR "FAELLIG JA" rechtfertigt
    --tweet-fetch ja. vollcheck.cjs ruft den Check selbst auf und kippt die
    Format-Zeile in beide Richtungen (fällig+nein = ÜBERFÄLLIG ✗, nicht
    fällig+ja = ÜBER-POLLING ✗). Zusätzlich Verpasst-Erkennung (Zeitstempel in
    Ortszeit, NIE roh/UTC, siehe Punkt 9 "Zeitzonen-Fix"): Liegt der letzte Fetch
    VOR dem Beginn des letzten fälligen Raster-Slots → Slot verpasst → JETZT
    nachholen und als ÜBERFÄLLIG ausweisen.
    ODER anlassbezogen bei einem bekannten Kalender-Release innerhalb ±15 Min.
    Pflicht-Ausgabezeile "Tweet-Check: ..." (siehe Punkt 9 und
    [[feedback_vollcheck_format]]) — IMMER ausgeben, auch bei "nichts Neues" oder
    "nicht fällig", nie stillschweigend weglassen.
```
> **Zwei harte Auflagen für diese Stelle:** (i) Die Zeichenkette `Pflicht-Ausgabezeile "Tweet-Check: ..."` muss **wörtlich erhalten** bleiben — `tests/trading_scripts.test.js` Z1689 pinnt sie per Regex am echten Regelwerk, und `loop_prompt.cjs` prüft `Tweet-Check:` als Selbsttest-Fragment (Exit 1, Loop startet nicht). (ii) Die beiden Anker `<!-- CRON-BAUSTEIN-FORMAT-TWEET-START/-ENDE -->` und die Fence-Zeilen nicht anfassen.

**(2) Z443 (Voll-Check-Katalog):** „an jedem Voll-Check auf Raster-Minute % 10 == 0 … + anlassbezogen" → ergänzen: „**UND Delta ≥ 10 Min aus `x_last_fetch.json` (Doppelbedingung seit 03.09.2026, Punkt 17 — `x_fetch_stamp.cjs --check` rechnet beides)**"; die Historie-Kette am Zeilenende um „→ Doppelbedingung Raster UND Delta (03.09.2026)" verlängern.

**(3) Z458 (How to apply 2b, heute per W1/F1 geändert):** Der heute eingefügte Satz *„seit 28.08.2026 ist auch der 10-Min-Delta-Auslöser aufgehoben: Fällig ist der Fetch AUSSCHLIESSLICH bei echter Minute % 10 == 0 an der Systemzeit; das Delta dient nur noch der Verpasst-Erkennung"* → ersetzen durch: *„28.08.2026: das Delta **allein** löst nicht mehr aus (Kerzenraster-Fix). **Seit 03.09.2026 (Punkt 17) ist das Delta als ZWEITE Bedingung zurück: fällig = Raster-Slot % 10 == 0 UND Delta ≥ 10 Min, gerechnet von `x_fetch_stamp.cjs --check`.** Das Delta ist damit Mitbedingung UND Verpasst-Erkennung, aber nie allein maßgeblich."*

**(4) Z462 (Überschrift Kerzenraster-Fix):** „— ab jetzt die ALLEINIGE verbindliche Fälligkeitsmechanik, ersetzt die Delta-Prüfung vom 25.08.2026" → „— ersetzt die Delta-Prüfung vom 25.08.2026 als *alleinige* Quelle; **seit 03.09.2026 Bedingung (a) der Doppelbedingung, siehe Nachtrag unten**". Darunter (nach Z466) ein neuer Kurzabsatz: *„**Nachtrag 03.09.2026 (Punkt 17) — Doppelbedingung:** Der Raster-Slot ist seither notwendig, aber nicht hinreichend. Zusätzlich muss das Delta ≥ 10 Min sein (Doppel-Fetch-Schutz nach dem Über-Polling-Schaden vom 02.09.2026). Beides rechnet `node scripts/x_fetch_stamp.cjs --check`; seit 09.09.2026 ruft `vollcheck.cjs` es selbst auf. Volle Fassung: [[feedback_vollcheck_format]] ‚Tweet-Fälligkeit rechnerisch'."*

**(5) Z468 (Nachhol-Regel):** „`x_last_fetch.json` … ist aber NICHT mehr die Fälligkeitsquelle — nur noch die Verpasst-Erkennung" → „… ist seit 03.09.2026 **wieder Fälligkeits-Mitbedingung (b)** (Delta ≥ 10 Min) **und zusätzlich** die Verpasst-Erkennung: …". Der Rest des Absatzes bleibt wörtlich. **Offene Frage F-A hier als Klammer vermerken**, solange sie nicht entschieden ist.

**(6) Z472 (dangling Querverweis, = K-1c vom 03.09., seit elf Tagen offen):** „Ausgabeformat der Pflichtzeile: siehe [[feedback_vollcheck_format]] (dort **Kerzenraster-Fassung 28.08.2026**)" → der so benannte Absatz ist dort seit 03.09. **durchgestrichene Historie**. Ersetzen durch: „(dort ‚Tweet-Fälligkeit rechnerisch', 03.09.2026)".

**(7) Z968 (Punkt 9, Anti-Drift (b)) und Z981/Z985 (Punkt 9a):** dieselbe Änderung wie (3) — „AUSSCHLIESSLICH … Delta entscheidet NICHT mehr über die Fälligkeit" → „Raster ist Bedingung (a), Delta ≥ 10 Min Bedingung (b); fällig nur, wenn beide erfüllt sind (Punkt 17, 03.09.2026)". Die drei Formatbeispiele der Pflichtzeile bleiben wörtlich.

**`scripts/loop_prompt.cjs` Z200:** inhaltlich **korrekt, kein Änderungsbedarf**. Optional kosmetisch die Herkunft ergänzen („Punkt 17, 03.09.2026 / maschinell TODO 4, 09.09.2026"). Nach der Fence-Korrektur steht dieselbe Regel zweimal im Prompt (Fence + String) — sachlich dann **gleichlautend**; die Entdopplung ist Kandidat Nr. 1 für **P3** ([[project_cron_automatisierung_einschaetzung_2026-09-14]] Abschnitt 4), nicht für jetzt.

**Betrifft die Korrektur die heutige P1-Automatisierung?** **Nein, im Gegenteil — sie profitiert davon.** P1 liest die Fence bei jedem `loop_prompt.cjs`-Lauf live aus der Regeldatei; eine Textänderung zwischen den Ankern wandert ohne Code-Anfassen in den nächsten Cron-Prompt. **Kein Code, keine Tests, keine Anker sind zu ändern** — sofern die beiden Auflagen unter (1) eingehalten werden. Vor dem Loop-Start wie üblich `node scripts/loop_prompt.cjs --testtag fiktiv --terminal-zeit <…>` laufen lassen (Exit 0 + Selbsttest) und `node --test tests/trading_scripts.test.js` gegenprüfen.

---

## 6. Praktische Relevanz — wie oft ändert sich das Verhalten wirklich?

**Gemessen am einzigen Testtag, an dem beide Bedingungen maschinell mitliefen (10.09.2026, 37 `Tweet-Check:`-Zeilen, 24 wörtliche `x_fetch_stamp.cjs --check`-Zitate):**

| Konstellation | Anzahl | Bedeutung |
|---|---|---|
| Raster-Slot **JA** + Delta **JA** (beide Regeln: fällig) | **5** (17:00/17:10/17:30/17:40/17:50) | kein Unterschied |
| Raster-Slot **JA** + Delta **NEIN** (Raster-allein: fällig / Doppelbedingung: nicht fällig) | **0** | **die einzige echte Divergenz — trat nicht auf** |
| Raster-Slot **NEIN** (beide Regeln: nicht fällig) | 19 | kein Unterschied |

**Ehrliche Einordnung der Datenlage:** Der Unterschied zwischen beiden Fassungen ist an genau einem Tag maschinell messbar gewesen und trat dort **kein einziges Mal** auf. Vor dem 09.09. wurde das Raster nie gerechnet, deshalb sind die älteren Protokolle für diese Frage wertlos. **Die Datenbasis für „wie oft macht es einen Unterschied" ist n=1 Tag, 5 Raster-Slots — das trägt keine statistische Aussage.** Der Divergenzfall ist konstruktiv selten: Er verlangt einen Fetch **weniger als 10 Minuten vor** einem Raster-Slot, also praktisch nur einen Anlass-Fetch (Kalender-Release ±15 Min) oder einen Nachhol-Fetch — beides typischerweise **an einem Nachrichten-Moment**, also selten, aber teuer.

**Was am 10.09. dagegen sehr wohl messbar war (und die eigentliche praktische Last der heutigen Konstruktion ist):** **11 `ÜBER-POLLING ✗`-Marken auf 37 Tweet-Check-Zeilen**, davon nach eigener Protokollanalyse

- **4 direkt durch die Delta-Hälfte erzeugt** (F-B: `--set` vor dem finalen `vollcheck.cjs`-Aufruf drückt das Delta auf ~4 Min — VC#45/47/53/55, im Protokoll je als „kein echter Doppel-Fetch" ausgewiesen),
- **mindestens 2 durch die Raster-Hälfte** (der Raster-Slot verstrich während der Datenerhebung, `--jetzt` alterte auf Minute :04 bzw. :21),
- der Rest echte Off-Raster-Fetches.

26 Erwähnungen von „Timing-Artefakt" im Protokoll eines einzigen Tages. **Jede dieser Falsch-Marken kippt die `Format:`-Zeile auf ✗ und geht in `protokoll_bilanz.cjs` ein.** Das ist der Punkt, an dem diese Regel heute real Schaden anrichtet — nicht die Frage Raster-allein vs. Doppelbedingung. Die Abhilfe liegt in F-A/F-B, nicht in der Wahl zwischen den beiden Fassungen.

---

## 7. Blocker-Einschätzung

**Kein Blocker für den nächsten Testtag — aber der billigste Fix mit dem besten Verhältnis, den es derzeit offen gibt.**

Begründung:
- **Die ausführende Instanz ist eindeutig.** Seit 09.09. entscheidet nicht der Regeltext, sondern `x_fetch_stamp.cjs`, und `vollcheck.cjs` erzwingt dessen Urteil in der Pflichtzeile. Selbst wenn Sonnet die Fence-Fassung liest und auf einem Raster-Slot ohne Delta fetcht, bleibt der Schaden auf ein ✗ in einer Protokollzeile beschränkt — es geht kein Trade, kein Gate und kein Level daran verloren.
- **Die Fehlrichtung ist die harmlose.** Der Widerspruch kann nur zu *einem Fetch zu viel* führen (Über-Polling), nie zu einem verpassten. Verpasste Fetches — die teure Richtung, 27.08. — fängt in beiden Fassungen die Verpasst-Erkennung ab (mit der Einschränkung aus F-A).
- **Er ist elf Tage alt und hat in dieser Zeit 0 belegte Divergenzen produziert** (Abschnitt 6).

**Was dagegen spricht, ihn weiter offen zu lassen:** Er steht seit P1 bei **jedem Fire** in beiden Fassungen nebeneinander im Cron-Prompt (Fence sagt Raster, hartkodierter String sagt Doppelbedingung) — ein Leser im 5-Minuten-Takt muss sich zwischen zwei Sätzen entscheiden, die einander widersprechen. Genau dafür ist nach [[feedback_regeldisziplin]] kein Platz. Der Fix ist eine Textkorrektur an sieben Stellen **ohne jede Code-, Test- oder Ankeränderung**, Aufwand ~20 Minuten, Risiko nahe null.

**Empfehlung zur Reihenfolge:** Abschnitt 5 (reine Textangleichung) **vor dem nächsten Testtag**; F-A und F-B (Skriptänderungen) **nach** P2 und mit eigenem Gegencheck — sie sind echte Regel-/Codeänderungen und fallen unter Punkt 14 (ein Paket pro Testtag).

---

## 8. Was Levi entscheiden muss

1. **Ausgang 2 bestätigen:** Gilt die Doppelbedingung (Raster % 10 == 0 **UND** Delta ≥ 10 Min) als die verbindliche Fassung — und wird `feedback_live_trading.md` an den sieben Stellen aus Abschnitt 5 nachgezogen? *(Fable-Empfehlung: ja.)*
2. **F-A (Nachhol-Regel):** Soll `x_fetch_stamp.cjs` den Nachhol-Fall als dritten Fälligkeitsgrund rechnen (`faellig = (rasterSlot && deltaOk) || nachholen`) — oder wird die Nachhol-Regel im Regelwerk stattdessen gestrichen? **Eines von beidem ist nötig; heute widersprechen sich Regel und Code.** *(Empfehlung: rechnen, nicht streichen — die Regel existiert wegen fünf versäumter Fetches am 27.08.)*
3. **F-B (Wasserstand-Semantik):** Bleibt `last_fetch` der Zeitstempel des jüngsten Tweets (dann ist Bedingung (b) an ruhigen Tagen wirkungslos und erzeugt an Nachrichtentagen Falsch-✗), oder wird die Abrufzeit als zweites Feld geführt? *(Empfehlung: zweites Feld, aber erst nach P2.)*
4. **Nachtrag in [[project_pruefung_feedback_live_trading_2026-09-14]]:** Der dortige W1-Abschnitt („‚alleinige verbindliche' steht ungestrichen nur noch beim Kerzenraster-Fix") sollte den Verweis auf diese Klärung tragen, damit die nächste Prüfrunde nicht erneut auf die 28.08.-Fassung hin korrigiert.

## ENTSCHIEDEN 14.09.2026 (Levi): Textkorrektur umsetzen, dann Gegencheck, dann P2 inkl. F-A/F-B

Levi: "Ja bitte jetzt an Fable zur Korrektur geben und danach mit Opus Gegencheck machen. Erst wenn alles Fehlerfrei ist weiter zu P2 mit den zwei weitren kleinen Bugs, die ebenfalls dann gefixed werden müssen". Ablauf: (1) Fable korrigiert die 7 benannten Stellen in feedback_live_trading.md auf die UND-Verknuepfung (Raster %10 UND Delta >=10), reiner Text, keine Code-/Test-/Anker-Aenderung noetig. (2) Opus-Gegencheck der Textkorrektur. (3) NUR wenn dieser Gegencheck fehlerfrei/ohne Blocker ist: P2 wird beauftragt UND umfasst zusaetzlich F-A (Nachhol-Fetch wird von x_fetch_stamp.cjs faelschlich als UEBER-POLLING bestraft) und F-B (last_fetch-Zeitstempel meint den letzten Tweet statt den letzten Abruf) als Code-Fixes. (4) eigener Opus-Gegencheck fuer P2 inkl. F-A/F-B.

---

## N1-Textkorrektur umgesetzt 14.09.2026 (Fable)

**Umfang:** Reine Textarbeit an `feedback_live_trading.md` (1.228 → 1.234 Zeilen). Kein Code, keine Tests, keine Anker angefasst; `x_fetch_stamp.cjs`, `vollcheck.cjs`, `loop_prompt.cjs` unverändert. F-A/F-B bewusst NICHT angefasst (P2). Referenz für den Wortlaut: `x_fetch_stamp.cjs` Z150–159 (`deltaOk = deltaMin >= schwelle`, `rasterSlot = deMin % 10 === 0`, `faellig = rasterSlot && deltaOk`) und `vollcheck.cjs` Z805–831 (kippt `tweetLine` UND `tweetFormat` → Tweet-Check- und Format-Zeile in beide Richtungen; deshalb steht im neuen Text "Tweet-Check- und Format-Zeile", nicht nur "Format-Zeile" wie im Vorschlag Abschnitt 5 (1)).

**Die 8 korrigierten Stellen (Zeilennummern = neuer Stand):**

| # | Stelle | Vorher (Kern) | Nachher (Kern) |
|---|---|---|---|
| 1 | Z95–106, Cron-Fence Item (2) `CRON-BAUSTEIN-FORMAT-TWEET` | „Tweet-Fetch-Fälligkeit am FESTEN KERZENRASTER prüfen … Ist die echte Minute % 10 == 0 → Pflicht-Fetch" | „Tweet-Fetch-Fälligkeit = DOPPELBEDINGUNG, gerechnet statt behauptet (Punkt 17, 03.09.2026; maschinell seit 09.09.2026, TODO 4): (a) echte Minute % 10 == 0 … UND (b) Delta aus x_last_fetch.json >= 10 Min. Beides rechnet `node scripts/x_fetch_stamp.cjs --check`; NUR "FAELLIG JA" rechtfertigt --tweet-fetch ja. vollcheck.cjs … kippt die Tweet-Check- und die Format-Zeile in beide Richtungen (fällig+nein = ÜBERFÄLLIG ✗, nicht fällig+ja = ÜBER-POLLING ✗)." — Verpasst-Erkennung, Anlass-Trigger und die Zeile `Pflicht-Ausgabezeile "Tweet-Check: ..."` wörtlich erhalten, beide Anker unverändert (je genau 1×, 6 Anker gesamt) |
| 2 | Z443, Voll-Check-Katalog | „auf Raster-Minute % 10 == 0 (…)** + anlassbezogen … Historie: … → festes Kerzenraster statt Ist-Zeit-Delta (28.08.2026)" | „… % 10 == 0 (…) UND Delta ≥ 10 Min aus `x_last_fetch.json` (Doppelbedingung seit 03.09.2026, Punkt 17 — `x_fetch_stamp.cjs --check` rechnet beides, nur "FAELLIG JA" ist fällig)** … Historie: … → Doppelbedingung Raster UND Delta (03.09.2026, Punkt 17; maschinell seit 09.09.2026, TODO 4)" |
| 3 | Z462, How to apply 2b (W1/F1-Satz) | „seit 28.08.2026 ist auch der 10-Min-Delta-Auslöser aufgehoben: Fällig ist der Fetch AUSSCHLIESSLICH bei echter Minute % 10 == 0 …; das Delta dient nur noch der Verpasst-Erkennung" — plus die Vorklammer „die folgende Delta-Beschreibung gilt nur noch für die Verpasst-Erkennung, nicht mehr als Fälligkeitsquelle" | „28.08.2026: das Delta ALLEIN löst nicht mehr aus (Kerzenraster-Fix). Seit 03.09.2026 (Punkt 17) ist das Delta als ZWEITE Bedingung zurück: fällig = Raster-Slot echte Minute % 10 == 0 UND Delta ≥ 10 Min, gerechnet von `node scripts/x_fetch_stamp.cjs --check` … Das Delta ist damit Mitbedingung UND Verpasst-Erkennung, aber nie allein maßgeblich" — W1-Wortlaut nur noch als Zitat mit Datum („noch am 14.09.2026 per N1-Klärung … gezogen"); Vorklammer auf „galt vom 28.08. bis 02.09.2026 …; seit 03.09.2026 wieder Mitbedingung (b)" gezogen (im Bericht nicht eigens genannt, gleiche Zeile) |
| 4 | Z466 Überschrift Kerzenraster-Fix + NEUER Absatz nach Z470 | „— ab jetzt die ALLEINIGE verbindliche Fälligkeitsmechanik, ersetzt die Delta-Prüfung vom 25.08.2026" | „— ersetzt die Delta-Prüfung vom 25.08.2026 als *alleinige* Fälligkeitsquelle; seit 03.09.2026 Bedingung (a) der Doppelbedingung, siehe Nachtrag unten" + neuer Absatz **„Nachtrag 03.09.2026 (Punkt 17) — Doppelbedingung"** (notwendig-nicht-hinreichend, Fällig = (a) UND (b), `rasterSlot && deltaOk`, Ausgabezeile des Skripts, vollcheck.cjs-Kippen, fachliche Begründung aus Abschnitt 3, Verweis K-1b) |
| 5 | Z472, Nachhol-Regel | „ist aber NICHT mehr die Fälligkeitsquelle — nur noch die Verpasst-Erkennung" | „(seit 03.09.2026 ausschließlich per `x_fetch_stamp.cjs --set …`), ist seit 03.09.2026 wieder Fälligkeits-Mitbedingung (b) (Delta ≥ 10 Min, nie allein maßgeblich — Nachtrag oben) und zusätzlich die Verpasst-Erkennung" — Rest wörtlich; am Ende F-A-Klammer („Offen — F-A …: Nachhol-Fetch außerhalb der Raster-Minute wird derzeit als NICHT FAELLIG/ÜBER-POLLING ✗ ausgewiesen; Code-Fix im separaten P2-Schritt; Regel gilt bis dahin unverändert") |
| 6 | Z476, Querverweis (= K-1c) | „(dort Kerzenraster-Fassung 28.08.2026)" | „(dort "Tweet-Fälligkeit rechnerisch", 03.09.2026 — die Kerzenraster-Fassung 28.08.2026 ist dort seit 03.09.2026 durchgestrichene Historie)" |
| 7a | Z974, Punkt 9 (b) | „Fällig ist der Fetch AUSSCHLIESSLICH bei echter Minute % 10 == 0 … Das Delta entscheidet NICHT mehr über die Fälligkeit, sondern nur noch über die Verpasst-Erkennung" + Vorklammer „gilt … nicht mehr als Fälligkeitsmechanik" + „maßgeblich ist dort die Kerzenraster-Fassung 28.08.2026" | „seit 28.08.2026 (Kerzenraster-Fix) ist das Raster Bedingung (a) …; seit 03.09.2026 (Punkt 17) ist das Delta ≥ 10 Min Bedingung (b). Fällig ist der Fetch nur, wenn BEIDE erfüllt sind — gerechnet von `node scripts/x_fetch_stamp.cjs --check` …, seit 09.09.2026 automatisch in `vollcheck.cjs`. Das Delta dient zusätzlich der Verpasst-Erkennung" + W1-Zitat als Historie + „maßgeblich ist dort die Doppelbedingungs-Fassung 03.09.2026 "Tweet-Fälligkeit rechnerisch" …"; die drei Formatbeispiele wörtlich unverändert |
| 7b | Z987, Punkt 9a How to apply | „die Fälligkeit läuft über das feste Kerzenraster (echte Minute % 10 == 0), `x_last_fetch.json` dient nur noch der Verpasst-Erkennung/Nachhol-Regel" | „das feste Kerzenraster … ist Bedingung (a); seit 03.09.2026 (Punkt 17) ist das Delta ≥ 10 Min … Bedingung (b) — fällig nur, wenn BEIDE erfüllt sind (…); `x_last_fetch.json` dient zusätzlich der Verpasst-Erkennung/Nachhol-Regel (…; Delta-Ära 25.-27.08. mit Delta ALLEIN … — als Mitbedingung neben dem Raster kann das Delta keinen legitim fälligen Fetch mehr verhindern)" |
| 7c | Z991, Punkt 9a Why (%10-Streichung) | „Verbindlich ist seit 28.08.2026 ausschließlich das Kerzenraster …; die Delta-Prüfung … dient nur noch der Verpasst-Erkennung" + Schlussklammer „seither ist die %10-Prüfung am Kerzenraster verbindlich" | „Verbindlich ist seit 03.09.2026 (Punkt 17) die Doppelbedingung: das Kerzenraster … ist Bedingung (a), das Delta ≥ 10 Min … ist Bedingung (b); fällig nur, wenn beide erfüllt sind" + W1-Zitat als Historie; Schlussklammer „— seit 03.09.2026 als Bedingung (a) neben dem Delta ≥ 10 Min als Bedingung (b) —"; Überschrift „…umgekehrt, seit 03.09.2026 Doppelbedingung; W1-Korrektur + N1-Nachzug 14.09.2026" |
| **8 (übersehen)** | Z464, Why (Modulo-Ersatz, 25.08.) — im Bericht NICHT gelistet | „*(Festschreibung vom 25.08.2026 — AUFGEHOBEN 28.08.2026 durch den Kerzenraster-Fix unten; identisch markiert …)*" — ein Leser konnte „Delta aufgehoben" lesen, ohne die Rückkehr am 03.09. zu sehen | „… AUFGEHOBEN 28.08.2026 durch den Kerzenraster-Fix unten; die Delta-Prüfung selbst kehrt am 03.09.2026 als Bedingung (b) der Doppelbedingung zurück, aber nie wieder als alleinige Quelle; identisch markiert …" |

**Nachscan der ganzen Datei** (Suchbegriffe „AUSSCHLIESSLICH bei", „entscheidet NICHT", „nur noch der/die Verpasst", „nicht mehr die Fälligkeitsquelle", „nicht mehr als Fälligkeits", „ALLEINIGE verbindliche", „Kerzenraster-Fassung 28.08.2026)"): nur noch 2 Treffer (Z462, Z974), beide das in Anführungszeichen zitierte, als überholt datierte W1-Zitat — keine lebende Raster-allein-Regel mehr. „Doppelbedingung" jetzt 9× in der Datei (vorher 1×, nur in der Loop-Start-Checkliste). Z470 „Die Regel:" und Z474 „Abgrenzung zum Modulo-Einwand" blieben im Wortlaut raster-bezogen — sie beschreiben Bedingung (a) und sind durch Überschrift + Nachtrag eindeutig eingeordnet; nicht geändert.

**Auflagen geprüft:** `Pflicht-Ausgabezeile "Tweet-Check: ..."` steht wörtlich (Z107); Anker `<!-- CRON-BAUSTEIN-FORMAT-TWEET-START/-ENDE -->` je genau 1×, alle 6 Anker vorhanden; Fence-Zeilen unverändert.

**Testergebnis:** `node --test tests/trading_scripts.test.js` → **77/77 pass, 0 fail, Exit 0** (unverändert).

**Echtlauf `node scripts/loop_prompt.cjs --testtag fiktiv --terminal-zeit 19:00`:** Exit 0, stderr leer. Kopfzeile: „Cron-Bausteine (P1, 14.09.2026): 3/3 per Anker aus feedback_live_trading.md (Stand 2026-09-14 17:10 DE) injiziert — MTF 11 Zeilen | FORMAT-TWEET 24 Zeilen | PFLICHTZEILEN 299 Zeilen." Generierter Prompt Z27–34 wörtlich:

```
(2) Tweet-Fetch-Fälligkeit = DOPPELBEDINGUNG, gerechnet statt behauptet (Punkt 17,
    03.09.2026; maschinell seit 09.09.2026, TODO 4): (a) echte Minute (per Punkt 9a)
    % 10 == 0 (Kerzenraster-Fix 28.08.2026, Slots :00/:10/:20/:30/:40/:50) UND
    (b) Delta aus x_last_fetch.json >= 10 Min. Beides rechnet
    `node scripts/x_fetch_stamp.cjs --check` fertig; NUR "FAELLIG JA" rechtfertigt
    --tweet-fetch ja. vollcheck.cjs ruft den Check selbst auf und kippt die
    Tweet-Check- und die Format-Zeile in beide Richtungen (fällig+nein = ÜBERFÄLLIG ✗,
    nicht fällig+ja = ÜBER-POLLING ✗). ...
```
Der hartkodierte String (Prompt Z51: „--tweet-fetch ja NUR, wenn die Doppelbedingung (Minute % 10 == 0 UND Delta >= 10) laut Skript FAELLIG JA ergibt") sagt jetzt dasselbe wie die Fence — der Widerspruch im Cron-Prompt ist weg (Entdopplung bleibt P3-Kandidat).

**Offene Fragen für den Opus-Gegencheck:**
1. **Anlass-Trigger vs. Skript (gleiche Klasse wie F-A, im Bericht nicht benannt):** Z462 und Fence sagen weiterhin „Kalender-Release ±15 Min → Fetch, auch wenn das Delta unter 10 Min liegt". `x_fetch_stamp.cjs` kennt den Anlass-Fall nicht → ein regelkonformer Anlass-Fetch außerhalb des Rasters wird von `vollcheck.cjs` als ÜBER-POLLING ✗ markiert. Nicht geändert (wäre Regel-/Code-Entscheidung) — Vorschlag: in den P2-Scope zu F-A aufnehmen.
2. **Präzisierung gegenüber dem Vorschlag:** Fence sagt „Tweet-Check- und die Format-Zeile" statt nur „Format-Zeile" (aus `vollcheck.cjs` Z826–831 abgeleitet). Bitte bestätigen.
3. **F-A-Klammer in der Nachhol-Regel** enthält den Satz „das ✗ ist in diesem Fall als bekanntes Skript-Artefakt zu benennen" — als Protokoll-Hinweis gemeint, keine neue Regel. Falls Opus das als Regeländerung wertet: streichen.
4. Punkt 4 aus Abschnitt 8 (Nachtrag in [[project_pruefung_feedback_live_trading_2026-09-14]] zum W1-Abschnitt) war nicht Teil des Auftrags und ist NICHT erledigt — offen.

---

## Opus-Gegencheck N1-Textkorrektur, 14.09.2026

**Auftrag:** Fables Textkorrektur unabhaengig pruefen (Autor != Pruefer, [[feedback_modellwahl_trading]]). Alle Zitate am 14.09.2026 selbst aus den Primaerquellen gelesen, nicht aus Fables Bericht uebernommen. Kein Commit, kein Push, `trades.db` unberuehrt.

### Gesamturteil: FREIGEGEBEN — kein Blocker. **P2 inkl. F-A/F-B kann beauftragt werden.**

Die Korrektur ist sachlich richtig, vollstaendig genug und erzeugt keinen neuen Widerspruch. Drei kleine, eindeutige Dinge habe ich selbst nachgezogen (unten O-1 bis O-3, reiner Text). Datei weiterhin **1.234 Zeilen**, Tests **77/77**, `loop_prompt.cjs` Exit 0 mit leerem stderr.

### 1. Die acht Stellen — einzeln nachgelesen

Alle acht inhaltlich so vorgefunden, wie Fable sie beschreibt; die Zitate stimmen. Jede Stelle liest sich im Kontext eindeutig: die geltende Doppelbedingung steht fett/ungestrichen, die 28.08.- und W1-Fassungen nur noch als datiertes Zitat bzw. Durchstreichung.

| Fable-Nr. | Zeile laut Bericht | Zeile tatsaechlich | Befund |
|---|---|---|---|
| 1 | Z95-106 | Z95-107 (Fence Z86-113) | korrekt; Anlass-Trigger, Verpasst-Erkennung und Pflichtzeile woertlich erhalten |
| 2 | Z443 | **Z447** | korrekt, Zeilennummer im Bericht veraltet |
| 3 | Z462 | Z462 | korrekt |
| 4 | Z466 + neuer Absatz | Z466 (Ueberschrift) + Z472 (Nachtrag) | korrekt |
| 5 | Z472 | **Z474** | korrekt, Zeilennummer im Bericht veraltet |
| 6 | Z476 | **Z478** | korrekt |
| 7a/7b/7c | Z974/987/991 | Z974/987/991 | korrekt |
| 8 | Z464 | Z464 | korrekt |

*Info (kein Mangel in der Sache):* drei der Zeilennummern im Umsetzungsbericht sind um 2-4 Zeilen veraltet. Inhalt und Reihenfolge stimmen; nur die Navigation im Bericht ist ungenau.

### 2. Vollstaendigkeits-Scan — **eine neunte Stelle gefunden (O-1, selbst gefixt)**

Ganze Datei gegen „AUSSCHLIESSLICH", „entscheidet NICHT", „nur noch der/die Verpasst-Erkennung", „ALLEINIGE/alleinige", „Kerzenraster-Fassung", „%10 / % 10", „Raster", „x_fetch_stamp", „x_last_fetch", „Doppelbedingung" gescannt. Fables Nachscan-Bilanz stimmt: die verbliebenen Treffer in Z462 und Z974 sind das in Anfuehrungszeichen gesetzte, datierte W1-Zitat; die Treffer in Z779/951/1062/1127 betreffen andere Regeln (Q-Score, Register, AVWAP, Punkt-12-Scope).

**Uebersehen wurde aber Z470** — Fable hat sie gesehen und bewusst stehen gelassen („beschreibt Bedingung (a), durch Ueberschrift + Nachtrag eindeutig eingeordnet"). Ich werte das anders: Der Satz begann woertlich mit **„Die Regel: Der Tweet-Fetch ist faellig bei jeder zweiten abgeschlossenen 5-Minuten-Kerze"** — fett, freistehend, mit dem Wort „faellig" und ohne jede Einschraenkung. Das ist exakt die Textsorte, die N1 ueberhaupt ausgeloest hat: ein Leser, der einem Querverweis auf „Punkt 9 Kerzenraster-Fix / Die Regel" folgt, landet auf einer Hinreichend-Aussage, die der Code nicht teilt. Die Einordnung steht zwar zwei Zeilen darueber und zwei Zeilen darunter — aber genau dieses Argument („der Kontext ordnet es ja ein") hat den K-1b-Nachzug elf Tage lang offen gehalten.

**O-1 (Nachbesserung, selbst gefixt):** Z470 lautet jetzt „**Die Regel (Bedingung (a) der Doppelbedingung — seit 03.09.2026 nur noch NOTWENDIG, nicht hinreichend; die zweite Bedingung steht im Nachtrag unten, Opus-Gegencheck N1 14.09.2026):** Der **Raster-Slot ist erreicht** bei jeder zweiten abgeschlossenen 5-Minuten-Kerze — operativ: …" (Rest woertlich unveraendert). Kein Regelinhalt geaendert, nur die Hinreichend-Behauptung entfernt. Ausserhalb aller Anker.

*Keine zehnte Stelle.* Z454 (Ueberschrift „alle 10 Minuten + anlassbezogen") und Z476 („Abgrenzung zum Modulo-Einwand") beschreiben Taktfrequenz bzw. Bedingung (a) und behaupten keine Faelligkeit — sie bleiben zu Recht unveraendert.

### 3. Fachliche Exaktheit gegen `x_fetch_stamp.cjs` — bestaetigt, mit zwei Praezisierungen

`x_fetch_stamp.cjs` Z150-159 selbst gelesen: `deltaOk = deltaMin >= schwelle` (Default 10), `deMin` = Minute in `Europe/Berlin`, `rasterSlot = deMin % 10 === 0`, **`faellig = rasterSlot && deltaOk`**. Die neue Fence-Formulierung („(a) echte Minute % 10 == 0 … UND (b) Delta aus x_last_fetch.json >= 10 Min … NUR ‚FAELLIG JA' rechtfertigt --tweet-fetch ja") beschreibt das **exakt**, nicht nur sinngemaess.

Zwei Praezisierungen, beide **Info**, keine Nachbesserung:
- **Feldbenennung:** Der Text sagt „Delta aus `x_last_fetch.json`" — genannt ist die **Datei**, gerechnet wird aus dem Feld `last_fetch` darin. Das ist korrekt und im Projektjargon eingebuergert. Die *Semantik* dieses Zeitstempels (juengster **Tweet**, nicht letzter **Abruf** — F-B) steht korrekt und unveraendert in Z474 („`--set <UTC-Zeitstempel des juengsten gesehenen Tweets>`"). Der Text behauptet an keiner der neun Stellen, das Delta sei der Abstand zum letzten Poll — die Korrektur hat F-B also weder verschleiert noch verschlimmert.
- **Rundung:** `deltaMin` ist `Math.round(...)`; ein reales Delta von 9,6 Min erfuellt (b). Im Regeltext nicht erwaehnt, praktisch irrelevant (Grenzfall von 24 Sekunden), fuer P2 hoechstens eine Fussnote.

### 4. Praxis-Check — drei gerechnete Faelle (`--jetzt`-Zeitanker, Wegwerf-Wasserstand im Scratchpad)

| Fall | Zeitanker | letzter Fetch | Skript | Was der korrigierte Text sagt |
|---|---|---|---|---|
| 1 | 16:40 DE | 16:35 DE (Delta 5) | `Raster-Slot: JA` / `faellig (>=10): NEIN` / **`NICHT FAELLIG (Delta 5 < 10)`** | (a) erfuellt, (b) nicht → nicht faellig. **identisch** |
| 2 | 16:40 DE | 16:20 DE (Delta 20) | `Raster-Slot: JA` / `JA` / **`FAELLIG JA`** | beide erfuellt → faellig, `--tweet-fetch ja`. **identisch** |
| 3 (F-A) | 16:45 DE | 16:20 DE (Delta 25) | `Raster-Slot: NEIN` / `JA` / **`NICHT FAELLIG (kein Raster-Slot)`** | Doppelbedingung: nicht faellig — **Nachhol-Regel Z474: trotzdem sofort nachholen**. Bekannte Divergenz, als F-A ausgewiesen |

**Kein Grenzfall, in dem die neue Textfassung vom Code abweicht** — ausser dem bereits als F-A dokumentierten Nachhol-Fall, der im Text an Ort und Stelle als offen gekennzeichnet ist. Damit ist genau das erreicht, was der Auftrag wollte: Text und Maschine sagen dasselbe, und wo sie es (noch) nicht tun, steht es dabei.

### 5. Die drei Fassungen im Cron-Prompt — alle DREI selbst geprueft

Das war der eigentliche Ausloeser von N1 (Doppeltext-Divergenz bei jedem Fire):

| Quelle | Wortlaut | Stand |
|---|---|---|
| Fence `feedback_live_trading.md` Z95-107 | „Tweet-Fetch-Faelligkeit = DOPPELBEDINGUNG … (a) echte Minute % 10 == 0 … UND (b) Delta aus x_last_fetch.json >= 10 Min" | Doppelbedingung |
| hartkodierter String `loop_prompt.cjs` Z200 | „--tweet-fetch ja NUR, wenn die Doppelbedingung (Minute % 10 == 0 UND Delta >= 10) laut Skript FAELLIG JA ergibt" | Doppelbedingung |
| `feedback_vollcheck_format.md` Z89 („Tweet-Faelligkeit rechnerisch", Punkt 17) | „nur, wenn BEIDE Bedingungen erfuellt sind: (a) Raster-Slot … UND (b) das Minuten-Delta … ist tatsaechlich >=10" | Doppelbedingung |

**Deckungsgleich.** Gegenprobe am echten Lauf `node scripts/loop_prompt.cjs --testtag fiktiv --terminal-zeit 19:00`: Exit 0, stderr leer, Kopfzeile „3/3 per Anker … FORMAT-TWEET 24 Zeilen"; im erzeugten Prompt steht die Doppelbedingung in Z27 (Fence) **und** Z51 (String) — keine gegenlaeufige Aussage mehr im Prompt. Die Entdopplung bleibt P3-Kandidat, ist aber kein Widerspruch mehr, sondern eine Redundanz.

### 6. Die beiden harten Auflagen — nachgezaehlt

- **Pflicht-Ausgabezeile:** `Pflicht-Ausgabezeile "Tweet-Check: ..."` steht woertlich in Z107, innerhalb der Fence, ungestrichen. `tests/trading_scripts.test.js` Z1689 (`assert.match(rr.stdout, /Pflicht-Ausgabezeile "Tweet-Check: \.\.\."/)`) laeuft gegen das **echte** Regelwerk und ist gruen; der Selbsttest in `loop_prompt.cjs` Z67 (`['Tweet-Check:', 'Format: Fließtext']`) greift ebenfalls (beide Fragmente in Z107 bzw. Z94).
- **Anker:** genau 6 Ankerzeilen (Z68/82 MTF, Z86/113 FORMAT-TWEET, Z127/429 PFLICHTZEILEN), **je genau 1x**; Fence-Zeilen unveraendert. Der Test prueft das zusaetzlich selbst.

### 7. Fables vier offene Punkte — entschieden

1. **Anlass-Trigger +/-15 Min: dieselbe Fehlerklasse wie F-A — bestaetigt, gehoert in den P2-Scope.** Gleiche Mechanik: ein regelkonformer Fetch, den `faellig = rasterSlot && deltaOk` strukturell nicht darstellen kann → `NICHT FAELLIG` → `vollcheck.cjs` schreibt `UEBER-POLLING ✗` auf einen regelkonformen Vorgang. **Ein Unterschied fuer den Bau:** Den Nachhol-Fall kann das Skript aus `x_last_fetch.json` + Uhrzeit selbst herleiten; den Anlass-Fall **nicht** — es kennt keinen Kalender. P2 braucht dafuer einen expliziten Eingang (z.B. `--anlass "<Release, Zeit>"`, durchgereicht von `vollcheck.cjs`), sonst ist der Zweig nicht pruefbar, sondern nur behauptbar — und damit genau das, was Punkt 17 abschaffen sollte. Nicht selbst geaendert (Code + Regelentscheidung). **Fuer P2 vorgemerkt: F-A, F-B und F-C (Anlass-Trigger).**
2. **„Tweet-Check- und Format-Zeile" statt nur „Format-Zeile": sachlich RICHTIG — freigegeben.** Selbst an `vollcheck.cjs` Z826-831 geprueft: jeder Zweig setzt **beides**, `tweetLine` (die `**Tweet-Check:**`-Pflichtzeile) und `tweetFormat` (der Baustein in der `Format:`-Zeile) — z.B. `faellig && !fetch` → `UEBERFAELLIG ✗` + `tweetFormat = '✗'`; `!faellig && fetch` → `UEBER-POLLING ✗` + `tweetFormat = '✗ (Ueber-Polling …)'`. Fables Praezisierung ist genauer als mein urspruenglicher Vorschlag in Abschnitt 5 (1). Bleibt so.
3. **Protokoll-Hinweis „das ✗ ist in diesem Fall als bekanntes Skript-Artefakt zu benennen": gestrichen (O-2).** Das ist keine Beobachtungsnotiz, sondern eine normative Anweisung („ist … zu benennen") mit einer unerwuenschten Nebenwirkung: Sie eroeffnet einen Erklaerkanal, mit dem sich ein ✗ als Artefakt wegschreiben laesst — dieselbe Klasse „Etikett statt Rechnung", gegen die Punkt 17 gebaut wurde, und am 10.09. bereits real gelebt (26x „Timing-Artefakt" an einem Tag). Sie war von Levis Entscheidung („REINE Textarbeit, Korrektur auf die Doppelbedingung") nicht gedeckt und faellt unter Punkt 14 (Regelaenderungs-Tempo-Bremse). Die *Information* geht nicht verloren: der Satz davor benennt den Skript-Stand weiterhin vollstaendig. Die Klammer endet jetzt mit „… Code-Fix im separaten P2-Schritt; die Nachhol-Regel selbst gilt bis dahin unveraendert.)" — ob der ✗-Fall im Protokoll eine Sonderbehandlung bekommt, entscheidet Levi mit P2, nicht dieser Textnachzug.
4. **Nachtrag im Pruefbericht: nachgeholt (O-3), war nicht vernachlaessigbar.** [[project_pruefung_feedback_live_trading_2026-09-14]] fuehrte den W1-Abschnitt weiter mit „verbindlich ist ausschliesslich das Kerzenraster" — eine kuenftige Pruefrunde haette daraus erneut in Richtung „Raster allein" korrigiert (genau der Mechanismus, der W1 ueberhaupt erzeugt hat). Der Abschnitt traegt jetzt direkt unter seiner Ueberschrift einen **UEBERHOLT-Kasten** mit der geltenden Doppelbedingung und dem Verweis hierher. Reiner Querverweis, kein Regelinhalt.

### 8. Was Opus geaendert hat (3 Fixes, alles reiner Text, kein Code, keine Tests, keine Anker)

| ID | Datei/Stelle | Aenderung | Klasse |
|---|---|---|---|
| O-1 | `feedback_live_trading.md` Z470 | „Die Regel: Der Tweet-Fetch ist faellig bei …" → „Die Regel (Bedingung (a) … NOTWENDIG, nicht hinreichend …): Der Raster-Slot ist erreicht bei …" | Nachbesserung (9. Fundstelle) |
| O-2 | `feedback_live_trading.md` Z474 (F-A-Klammer) | Schlusssatz „das ✗ ist in diesem Fall als bekanntes Skript-Artefakt zu benennen" gestrichen | Nachbesserung (ungedeckte Regelaenderung) |
| O-3 | `project_pruefung_feedback_live_trading_2026-09-14.md`, Abschnitt „W1 — Tweet-Fetch-Faelligkeit" | UEBERHOLT-Kasten mit Verweis auf diese Klaerung eingefuegt | Nachbesserung (Rueckfall-Schutz) |

Zeilenzahl `feedback_live_trading.md` unveraendert **1.234**; Anker 6/6 je 1x; Pflichtzeile Z107 woertlich.

### 9. Verifikation nach den Opus-Fixes

- `node --test tests/trading_scripts.test.js` → **77/77 pass, 0 fail** (unveraendert vor und nach den Fixes).
- `node scripts/loop_prompt.cjs --testtag fiktiv --terminal-zeit 19:00` → **Exit 0, stderr leer**, 3/3 Bausteine per Anker.
- `x_fetch_stamp.cjs --check` in drei Konstellationen gegen den Regeltext gerechnet (Abschnitt 4) — keine Abweichung ausser dem dokumentierten F-A.
- Kein Commit, kein Push; `trades.db` nicht angefasst.

### 10. Freigabe

**FREIGEGEBEN, kein Blocker. P2 kann jetzt beauftragt werden und umfasst F-A, F-B und zusaetzlich F-C (Anlass-Trigger, gleiche Fehlerklasse wie F-A).** Empfehlung fuer den P2-Auftrag: F-A und F-C zusammen bauen (beide erweitern denselben Ausdruck `faellig = (rasterSlot && deltaOk) || nachholen || anlass`), F-B als zweites Feld (`last_poll` neben `last_fetch`) getrennt halten und beide Migrationen mit eigenem Gegencheck — F-B aendert die Bedeutung einer persistierten Datei und beruehrt `feedback_session_update.md` Schritt 2.

## ENTSCHIEDEN 14.09.2026 (Levi): F-B beauftragt

Levi: "Ja bitte an Fable weitergeben" (nach Opus-Gegencheck P2 Kernteil: F-A x F-B-Wechselwirkung real reproduziert, Blocker fuer den naechsten Testtag, F-B muss sofort beauftragt werden). F-B-Auftrag an Fable MIT den zwei vom Gegencheck mitgelieferten Auflagen: (1) Bedingung (b)/Delta-Rundungsfalle mit umbauen (Math.round-Kippfall bei ~10 Min Delta), (2) last_fetch bekommt ein zweites Feld fuer die tatsaechliche Abrufzeit, Rueckwaertsguard nur fuer den Wasserstand (juengster Tweet), Test "ruhiger Nachmittag ueber mehrere VCs". Danach eigener Opus-Gegencheck.

---

## F-B umgesetzt 14.09.2026 (Fable)

**Auftrag:** Blocker-Auflage aus dem Opus-Gegencheck P2 ([[project_cron_automatisierung_einschaetzung_2026-09-14]] Abschnitt 1): `last_fetch` meinte den jüngsten TWEET, nicht den letzten ABRUF → an ruhigen Nachmittagen meldete `Nachholen` an jedem Off-Raster-VC fälschlich JA. Kein Commit, kein Push; Live-Datei `scripts/x_last_fetch.json` NICHT angefasst (steht noch im Vor-F-B-Format, siehe offener Punkt 4).

### Was geändert wurde

**Dateiformat `x_last_fetch.json` — zwei Felder plus Rotation (`scripts/x_fetch_stamp.cjs`, Kopfkommentar „DATEIFORMAT"):**
- `last_fetch` = **Wasserstand** (jüngster gesehener Tweet). Unverändert: `start_time` für den Digest, Doppel-Lieferungs-Schutz, Rückwärts-Guard in `--set`.
- `last_poll` = **letzter tatsächlicher Abruf** (neu). Bei JEDEM Stempeln auf „jetzt" (bzw. `--jetzt`) gesetzt. Kein Rückwärts-Guard.
- `prev_poll` = Abruf davor (neu, rotiert). Zweck: liegt `last_poll` im **laufenden** Voll-Check (Stempel lief vor `vollcheck.cjs` — bei Release-Momenten laut [[feedback_live_trading]] „Reihenfolge bei Voll-Check auf einen exakten Makro-Release-Zeitpunkt" sogar vorgeschrieben: „zuerst Tweets, DANACH Voll-Check"), rechnet `--check` gegen `prev_poll`. Ohne das hätte F-B das 10.09.-„Timing-Artefakt" (VC#45/47/53/55: `--set` vor `vollcheck.cjs` → ÜBER-POLLING ✗ auf einen korrekten Fetch) von „nur bei frischem Tweet" auf „immer" verschärft.

**Modi (`x_fetch_stamp.cjs`):** `--set <UTC jüngster Tweet>` schreibt `last_fetch` + `last_poll` (+ `prev_poll`); **neu `--polled`** = Abruf ohne neuen Tweet, schreibt nur `last_poll`/`prev_poll`, Wasserstand bleibt. Vor F-B hieß die Regel „bei leerem Ergebnis kein Set" — genau das ließ die Abrufzeit stehen. `--jetzt` gilt jetzt auch für `--set`/`--polled` (Zeitanker für `last_poll`, Tests/B9-Muster). `--check`: neue Zeilen `last_poll (…)`, `Abruf-Referenz: last_poll | prev_poll … | last_fetch (FALLBACK …)`; `Delta:`-Zeile trägt abgeschnittene Minuten + exakten Wert + Quelle + Slot-Delta; `Nachholen`-Zeile sagt `JA (letzter Abruf HH:MM:SS < Slot-Beginn HH:M0)`; der frühere F-A/F-B-Vorbehalts-Hinweis ist weg (nur noch im Migrations-Fallback).

**`scripts/vollcheck.cjs`:** liest `deltaOk` aus der Delta-Zeile des Skripts (nicht mehr `delta >= 10`), zitiert `Abruf-Referenz:` in der Pflichtzeile **nur** in den Sonderfällen (prev_poll / FALLBACK), ÜBERFÄLLIG-Text sagt „letzter Abruf vor N Min in einem früheren Raster-Slot — Delta-Bedingung laut Skript erfüllt" statt „Delta 9 Min ≥ 10"; n.a.-Zweig nutzt `deltaOk`; die drei „Wasserstand danach per --set setzen"-Hinweise heißen jetzt „danach stempeln: --set … bzw. --polled"; JSON `out.tweet` trägt `deltaOk` + `abrufReferenz`. **`scripts/loop_prompt.cjs` Z200:** Stempel-Satz erweitert (`--set` ODER `--polled`, Reihenfolge zu vollcheck.cjs egal, „ein Abruf ohne Stempel gilt beim nächsten Voll-Check als verpasster Slot"). Prompt 44.976 → 45.678 B, Selbsttest Exit 0.

**Regeltexte (reine Präzisierung, keine neue Regel):** [[feedback_live_trading]] Cron-Fence Item (2) (Bedingung (b) = „Delta seit dem letzten ABRUF … Feld last_poll, NICHT der Tweet-Wasserstand"; Verpasst-Erkennung gegen `last_poll`; Stempelpflicht `--set`/`--polled`; Anker 6/6 je 1×, `Pflicht-Ausgabezeile "Tweet-Check: ..."` wörtlich Z111), Z447 Katalog, Z462 How to apply, **neuer Absatz „Nachtrag F-B 14.09.2026"** nach dem Punkt-17-Nachtrag (beide Felder, Slot-Delta, Rundungsfalle, prev_poll, Why 10.09.), Z474 Nachhol-Regel (gegen `last_poll`; die seit P2 veraltete Klammer „F-A: rechnet den Nachhol-Fall noch nicht" auf „maschinell seit 14.09.2026" gezogen), Punkt 9 (b), Zeitzonen-Fix (b), Punkt 9a How to apply. [[feedback_session_update]] Schritt 2: „bei leerem Fetch-Ergebnis: `--polled`" statt „kein Set", Absatz „Zwei Felder seit F-B". [[feedback_vollcheck_format]] Z42 Template-Klammer + Z89 Punkt 17 (F-B-Klammer). [[feedback_loop_ablauf_uebersicht]] Z40.

### Die drei Auflagen

**Auflage 1 — Rundungsfalle:** Bedingung (b) ist jetzt **slot-basiert**: `deltaOk = (Zeitanker − Beginn des 10-Min-Raster-Slots, in dem der letzte Abruf lag) ≥ Schwelle` — Lesart „kein zweiter Abruf im selben 10-Min-Slot" (mit Schwelle 10 exakt äquivalent zu „Abruf lag in einem früheren Slot"). Echte Sekunden allein lösen den Fall NICHT (9,17 < 10 bleibt), deshalb die erste Opus-Option (slot-basiert wie F-A). Gerundet wird nur für die Anzeige, und zwar **abgeschnitten**, damit nie „Delta 10 < 10" erscheint. Beleg (Skript, Wegwerf-Datei, Zeitanker exakt aus dem Gegencheck-Bericht): Abruf `17:01:10`, Anker `17:10:20` →
`Delta: 9 Min seit letztem Abruf (exakt 9,2 Min; last_poll 14.9.2026, 17:01:10) | faellig (>= 10 Min): JA (Slot-Delta 10,3 Min ab Beginn des Abruf-Slots 17:00 — F-B: kein zweiter Abruf im selben 10-Min-Slot, rundungsfest)` / `Doppelbedingung (Raster UND Delta >= 10 Min): FAELLIG JA` / `Faelligkeit gesamt: FAELLIG JA — Grund: Raster+Delta`. Gegenprobe gleicher Slot (Abruf 17:00:10, Anker 17:09:46): `faellig (>= 10 Min): NEIN (Abruf liegt im laufenden Raster-Slot 17:00, Slot-Delta 9,8 < 10 — naechste Faelligkeit ab 17:10 DE)`. Test: „Auflage 1 — Rundungsfalle exakt" (Skript + `vollcheck.cjs` ✓/ÜBERFÄLLIG-Text, JSON `delta:9, deltaOk:true`).

**Auflage 2 — Rückwärtsguard nur Wasserstand:** `--set` mit älterem `last_fetch` → Exit 1, Datei byte-identisch (auch `last_poll` unverändert; Fehlertext verweist auf `--polled`); `--polled` mit früherem Zeitanker wird geschrieben; `--set` mit gleichem Wasserstand erlaubt („nur Abrufzeit fortgeschrieben"). Test: „Stempeln: …".

**Auflage 3 — ruhiger Nachmittag:** Test „Auflage 3 — ruhiger Nachmittag ueber 60 Min": 12 Voll-Checks im 5-Min-Rhythmus, Wasserstand konstant (Tweet 76 Min vor der Strecke), `--polled` nur an den 6 Raster-Slots (~40 s nach dem Zeitanker, NACH `vollcheck.cjs`); pro VC geprüft: `Nachholen (…): NEIN`, Raster-VC `FAELLIG JA — Grund: Raster+Delta` + Pflichtzeile `fällig+durchgeführt ✓`, Off-Raster `NICHT FAELLIG` + `nicht fällig, … letzter Fetch vor 4 Min` + Format `Tweet-Check n.a.`; nirgends ÜBERFÄLLIG/ÜBER-POLLING/`Tweet-Check ✗`; Wasserstand am Ende identisch. Variante „Stempel VOR vollcheck.cjs" (8 VCs): Raster-VC bleibt ✓ mit zitierter `Abruf-Referenz: prev_poll …` in der Pflichtzeile. Dazu „F-A mit dem neuen Feld" (echt verpasster Slot wird weiterhin erkannt; nach dem Nachhol-Stempel ist der nächste Raster-Slot regulär fällig) und „Migration" (nur `last_fetch` → FALLBACK laut, `vollcheck.cjs` zitiert es, erster `--polled` beendet den Fallback).

### Tests

`node --test tests/trading_scripts.test.js`: **84/84 → 90/90 pass, 0 fail** (6 neue Tests im Block „F-B (14.09.2026)"). Bestehende F-A-/F-C-Tests auf das neue Feld umgestellt (Wasserstand eine Stunde älter als der Abruf, damit sie `last_poll` prüfen, nicht den Fallback); Sandbox-Grunddatei und Helfer schreiben beide Felder. **Eine Erwartung bewusst geändert** (siehe offener Punkt 1).

### Echtlauf „ruhiger Nachmittag" (Sandbox-Kopie von `scripts/`, Wasserstand 16:44 DE, Stempel NACH `vollcheck.cjs`, Zeitanker 17:00:20 … 17:20:20 DE), `**Tweet-Check:**`-Zeilen wörtlich (Skript-Zitat gekürzt):

```
VC 17:00 → Raster-Slot 17:00 fällig+durchgeführt ✓ (nichts_Neues; letzter Fetch vor 9 Min; Grund laut Skript: Raster+Delta)
           Abruf ohne neuen Tweet gestempelt (--polled, F-B 14.09.2026): last_poll = 2026-09-14T15:00:58.000Z = 14.9.2026, 17:00:58
VC 17:05 → nicht fällig, nächster Raster-Slot 17:10, letzter Fetch vor 4 Min (Doppelbedingung: kein Raster-Slot; Nachholen NEIN, Anlass NEIN)
VC 17:10 → Raster-Slot 17:10 fällig+durchgeführt ✓ (…; letzter Fetch vor 9 Min; Grund laut Skript: Raster+Delta)
VC 17:15 → nicht fällig, nächster Raster-Slot 17:20, letzter Fetch vor 4 Min (Doppelbedingung: kein Raster-Slot; Nachholen NEIN, Anlass NEIN)
VC 17:20 → Raster-Slot 17:20 fällig+durchgeführt ✓ (…)
Datei am Ende: {"last_fetch":"2026-09-14T14:44:00.000Z","last_poll":"2026-09-14T15:20:58.000Z","prev_poll":"2026-09-14T15:10:58.000Z"}
```
Vergleich Opus-Lauf 2 (vor F-B): dieselbe Strecke lieferte an jedem Off-Raster-VC `ÜBERFÄLLIG ✗ — Slot … verpasst`. **Die F-A×F-B-Wechselwirkung ist damit behoben** — `Nachholen` bleibt NEIN, solange real gestempelt wurde.

### Offene Punkte für den Opus-Gegencheck

1. **Slot-basierte (b) ändert einen Fall gegenüber P2 (bewusst, im Test dokumentiert):** Off-Raster-Abruf um :X5 (Nachhol-/Anlass-Fetch) und Raster-VC um :Y0 → jetzt `FAELLIG (Raster+Delta)` (5-Min-Abstand einmalig); der P2-Regressionstest sagte `NICHT FAELLIG (Delta 5 < 10)`. Grund: mit dem wörtlichen Delta wäre :Y0 nicht fällig, :Y5 dann per Nachholen („Slot :Y0 verpasst") — der Loop rastet **dauerhaft auf der Phase :X5** ein (09.09.-Muster „falsche Rasterphase"); slot-basiert re-synct er sofort. Gleiches gilt für Anlass 15:58 → Raster 16:00 (dort erlaubt F-C ohnehin jeden VC im ±15-Fenster). Bitte bestätigen oder Alternative benennen.
2. **Auftragstext „jeder Aufruf (--check oder --set) schreibt das Feld" bewusst NICHT für `--check` umgesetzt:** `--check` läuft in jedem Voll-Check (5-Min-Takt); würde es `last_poll` stempeln, wäre das Delta nie ≥ 10 und kein Fetch je fällig (Selbstblockade). Der „Abruf" ist der X-API-Poll, nicht der Skriptaufruf → daher `--polled` für leere Abrufe. Das ist eine **neue Pflicht-Handlung im Loop** (Regeltext Session-Update Schritt 2 / Fence / loop_prompt-String geändert) — bitte als Levi-Entscheidung bestätigen.
3. **Fenster „Abruf des laufenden Voll-Checks" (Zeitanker −30 s … +6 Min) ist eine Heuristik.** Grenzfall: ein Stempel des VORIGEN VC weniger als 30 s vor dem aktuellen Zeitanker würde dem laufenden VC zugerechnet (Rechnung dann gegen `prev_poll`, u.U. falsches ÜBERFÄLLIG). Bei korrektem Loop (`date` zuerst, Punkt 9a) liegt der Stempel immer nach dem Anker.
4. **Migration:** `scripts/x_last_fetch.json` steht live noch auf `{"last_fetch":"2026-09-11T17:49:36.000Z"}` (kein `last_poll`) → bis zum ersten Stempel rechnet `--check` im ausgewiesenen FALLBACK (= altes Verhalten). Der nächste Session-Update-Fetch (`--set`) schreibt das Feld; bewusst nicht von Hand vorbelegt (Live-Datei, „jetzt" als Abrufzeit wäre erfunden). Empfehlung: Loop-Start-Checkliste um „Abruf-Referenz: last_poll im ersten Voll-Check prüfen" ergänzen (nicht gemacht — Regeländerung).
5. Anzeige: `Delta:`/„letzter Fetch vor N Min" sind jetzt **abgeschnittene** Minuten (vorher `Math.round`) — 9,6 Min zeigt „9", nicht „10". Gewollt (kein „10 < 10"), aber eine sichtbare Änderung im Protokoll.
6. F-A-Klammer Z474 („rechnet den Nachhol-Fall noch nicht") war seit P2 sachlich falsch und ist mitgezogen — Status-Nachzug, keine Regeländerung; N-3 aus dem P2-Gegencheck (Nachhol-✗ ohne F-B-Vorbehalt) erledigt sich, weil die Referenz jetzt zitiert wird.
7. Prompt-Größe +702 B (Fence +4 Zeilen, String) → weiter N-4/P3.

**Geänderte Dateien:** `scripts/x_fetch_stamp.cjs`, `scripts/vollcheck.cjs`, `scripts/loop_prompt.cjs`, `tests/trading_scripts.test.js`; Memory: `feedback_live_trading.md`, `feedback_session_update.md`, `feedback_vollcheck_format.md`, `feedback_loop_ablauf_uebersicht.md`, diese Datei, `MEMORY.md`. Nicht angefasst: Q1–Q4, Hard-Gates, Ampel, P1-Cron-Anker, Level-Ping-Pong/9b (hängen nicht am Feld — geprüft: `position_tick.cjs`/`protokoll_bilanz.cjs`/`gate_check.cjs` lesen `x_last_fetch.json` nicht), `trades.db`, Live-`x_last_fetch.json`.

---

## Opus-Gegencheck F-B, 14.09.2026

**Auftrag:** Fables F-B-Umsetzung unabhängig prüfen (Autor ≠ Prüfer, [[feedback_modellwahl_trading]]). Alles unten selbst gemessen — eigene Sandbox (Kopie von `scripts/`), eigener Voll-Check-Harness, eigene Läufe; kein Zahlenwert aus Fables Bericht übernommen ([[feedback_memory_pflege]], Zitierpflicht). Kein Commit, kein Push, `trades.db` unberührt, Live-`scripts/x_last_fetch.json` unberührt.

### Gesamturteil: FREIGEGEBEN MIT AUFLAGEN — der Blocker ist behoben, kein neuer Blocker

Der Reproduktionsfall aus dem P2-Gegencheck ist tot: über **37 Voll-Checks / 3 Stunden** meldet `Nachholen` **0×** fälschlich JA (vorher: 37/37). Auflagen 1–3 sind erfüllt und halten auch meinen eigenen Grenzfällen stand. Eine Auflage (A-1) muss **vor** dem nächsten Testtag ausgeführt werden, vier Nachbesserungen sind benannt, eine Ergänzung habe ich selbst gemacht (O-1). Tests **90/90**, `loop_prompt.cjs` Exit 0 mit leerem stderr.

---

### 1. Der Blocker-Reproduktionsfall — selbst nachgebaut (die wichtigste Einzelprüfung)

Aufbau exakt wie im P2-Gegencheck: ruhiger Nachmittag, Wasserstand fix (jüngster Tweet 15:44, keine neuen Tweets), reale Fetches **korrekt auf jedem 10-Min-Raster-Slot**, Stempel ~45 s nach dem Zeitanker. 37 Voll-Checks im 5-Min-Takt, 16:00 → 19:00 DE, `x_fetch_stamp.cjs --check` je Voll-Check.

| Messgröße | Ergebnis |
|---|---|
| Voll-Checks mit fälschlichem `Nachholen: JA` (Slot war bedient) | **0 / 37** |
| Raster-Slots, die fälschlich NICHT fällig waren | **0 / 18** |
| Off-Raster-Voll-Checks, die fälschlich fällig waren | **0 / 19** |
| Wasserstand am Ende | unverändert `13:44Z`; `last_poll`/`prev_poll` sauber rotiert |

**Gegenprobe mit derselben Strecke und der alten Semantik** (Datei nur mit `last_fetch`, also der Migrations-Fallback = Vor-F-B-Verhalten): **`Nachholen: JA` in 37/37 Voll-Checks**, ab dem zweiten Voll-Check durchgehend `FAELLIG JA — Grund: Nachholen`. Der Harness misst also nachweislich genau den Befund, um den es ging — und F-B beseitigt ihn vollständig.

**Downstream in `vollcheck.cjs`** (eigener 90-Min-Lauf, s. Abschnitt 4): 9× `fällig+durchgeführt ✓`, 7× `nicht fällig`, **0× ÜBER-POLLING**. Die F-A×F-B-Wechselwirkung ist damit bestätigt behoben.

### 2. Auflage 1 (Rundungsfalle) — eigene Grenzfälle, nicht Fables Beispiel

Acht selbst gerechnete Fälle (Zeitanker per `--jetzt`, Wegwerf-Datei im Scratchpad):

| Fall | letzter Abruf | Zeitanker | echtes Delta | (b) | fachlich richtig? |
|---|---|---|---|---|---|
| G1 Abruf exakt auf der Slot-Grenze | 17:00:00 | 17:10:00 | 10,00 Min | **JA** | ✓ neuer Slot |
| G3 1 s nach der Slot-Grenze | 17:00:01 | 17:10:00 | 9,98 Min | **JA** | ✓ genau der Kippfall, den `Math.round` verloren hätte |
| G5 zweiter Abruf im SELBEN Slot | 17:00:10 | 17:09:46 | 9,60 Min | **NEIN** | ✓ Doppel-Fetch im Slot bleibt gesperrt (`naechste Faelligkeit ab 17:10`) |
| G7 Off-Raster :55 → Raster :00 | 16:55:30 | 17:00:20 | 4,83 Min | **JA** | bewusst geändert, s. Abschnitt 5 Punkt 1 |
| G2b sehr langsamer Vor-VC, Stempel 21 s vor dem Anker | 16:59:59 (prev 16:50:45) | 17:00:20 | — | **JA** gegen `prev_poll` | ✓ der 16:50-Slot war bedient |
| G6b Stempel des LAUFENDEN VC (Release-Reihenfolge) | 17:10:05 (prev 17:00:45) | 17:10:20 | — | **JA** gegen `prev_poll` | ✓ `Abruf-Referenz` weist es aus |
| G8 langer Ausfall | 16:20:10 | 17:05:20 | 45,2 Min | JA + `Nachholen JA (Slot 17:00)` | ✓ |
| G2c Stempel 40 s vor dem Anker (außerhalb der 30-s-Toleranz) | 16:59:40 (prev 16:50:45) | 17:00:20 | 0,67 Min | **JA** | **Grenzfall, s. N-3** |

**Bilanz:** Die slot-basierte Lesart ist in jedem geprüften Fall rundungsfest und blockiert **keinen** legitimen Raster-Fetch; ein zweiter Abruf im selben Slot bleibt zuverlässig gesperrt (G5). Die Anzeige nennt abgeschnittene Minuten **und** den exakten Wert — „Delta 10 < 10" kann nicht mehr entstehen. **Auflage 1 erfüllt.** Zwei Grenzfälle erkaufen das (G7 = bewusst, G2c = N-3), beide in der harmlosen Richtung „ein Fetch zu viel", beide laut im Protokoll.

### 3. Auflage 2 (Rückwärtsguard) — selbst getestet, inklusive der Frage nach dem Edge Case

- **`--set` mit älterem Wasserstand:** Exit 1, Fehlertext nennt `--polled`, Datei **byte-identisch** (auch `last_poll` unberührt). ✓
- **`--polled` rückwärts** (Zeitanker 16:40, während `last_poll` auf 17:00:45 stand): wird **geschrieben**, kein Guard — genau wie beauftragt (der Guard gilt nur dem Wasserstand). ✓

**Zur ausdrücklich gestellten Frage, ob ein rückwärts laufendes `last_poll` das Delta künstlich vergrößert: ja, das tut es — und es ist ein echter, wenn auch unwahrscheinlicher Edge Case.** Nach dem Rückwärts-Stempel oben meldete der 17:05-Check `Nachholen: JA (Slot 17:00 verpasst)`, obwohl der 17:00-Slot 45 s nach 17:00 bedient worden war (die korrekte Abrufzeit war durch die Rotation nach `prev_poll` gewandert). Folge: ein falsches `ÜBERFÄLLIG ✗` plus ein überflüssiger Abruf. **Bewertung: kein Blocker** — auslösbar nur über (a) ein explizites `--jetzt` in der Vergangenheit, das im Loop nicht vorkommt (Regeltext und `loop_prompt.cjs`-String schreiben `--polled` ohne `--jetzt`; `--jetzt` ist im Kopfkommentar ausdrücklich als Test-/B9-Muster geführt), oder (b) einen Uhr-Rücksprung, der im Handelsfenster 15:30–22:00 DE nicht aus der Sommerzeit-Umstellung entstehen kann. Fehlerrichtung wieder „ein Fetch zu viel", und laut. Als **N-4** notiert.

### 4. Auflage 3 — eigener, längerer Lauf (90 Min, mit zwei Störungen)

Eigener Voll-Check-Harness (echtes `vollcheck.cjs --state --testtag fiktiv --dry-run`, 19 Voll-Checks über 90 Min), ruhiger Nachmittag, **zwei absichtliche Störungen**: ein komplett ausgefallener Voll-Check (19:30) und ein um 4,5 Min verzögerter Stempel (20:00).

```
18:50 Raster-Slot 18:50 fällig+durchgeführt ✓        ... (Wechsel ✓ / nicht fällig über 8 VCs)
19:30 — VOLL-CHECK AUSGEFALLEN (kein Fire, kein Fetch, kein Stempel)
19:35 ÜBERFÄLLIG ✗ — Slot 19:30 verpasst, jetzt nachgeholt ✓    <- korrekt erkannt
19:40 Raster-Slot 19:40 fällig+durchgeführt ✓                    <- Re-Sync, KEIN Phasen-Lock
19:45 nicht fällig | 19:50 ✓ | 19:55 nicht fällig | 20:00 ✓ (Stempel bewusst 4,5 Min verzögert)
20:05 ÜBERFÄLLIG ✗ — Slot 20:00 verpasst, jetzt nachgeholt ✓     <- FALSCH, s. N-2
20:10 Raster-Slot 20:10 fällig+durchgeführt ✓ | 20:15 nicht fällig | 20:20 ✓
90-Min-Bilanz: fällig+durchgeführt ✓ 9 | nicht fällig 7 | ÜBERFÄLLIG 2 | ÜBER-POLLING 0
```

**Auflage 3 erfüllt.** Der echt verpasste Slot (19:30) wird erkannt, nachgeholt, und der Takt **synchronisiert sich am nächsten Raster-Slot von selbst zurück** — das ist der empirische Beleg für Fables Punkt 1. Die zweite `ÜBERFÄLLIG`-Marke ist ein Artefakt der Fenster-Heuristik (N-2), kein Fehler der F-B-Logik.

### 5. Fables sieben offene Punkte — entschieden

1. **Slot-basierte (b) ändert den Fall `:X5 → :Y0` auf FAELLIG: BESTÄTIGT.** Die Begründung ist stichhaltig, und ich habe die Alternative durchgerechnet: mit dem wörtlichen Delta bliebe `:Y0` nicht fällig, `:Y5` würde per Nachholen fällig — der Loop rastet **dauerhaft** auf der Phase `:X5` ein (exakt das 09.09.-Muster „18/18 Voll-Checks auf Minute % 10 == 5"), und jeder dieser Fetches trüge ein `ÜBERFÄLLIG ✗`. Der Preis der slot-basierten Lesart ist **genau ein** 5-Minuten-Abstand je Re-Sync-Ereignis, danach wieder 10 Min (im 90-Min-Lauf sichtbar: 19:35 → 19:40 → 19:50). Der Preis der Alternative wäre ein Dauerzustand. **Die gewählte Variante ist die richtige.**
2. **`--polled` als neue Loop-Pflicht: die Lücke ist NICHT real — die Pflicht ist verankert.** Das war der wahrscheinlichste verbleibende Blocker-Kandidat, deshalb vollständig durchsucht. Gefunden an fünf Stellen, zwei davon in dem Text, der im Trigger-Moment garantiert gelesen wird:
   - `feedback_live_trading.md` **Cron-Fence Item (2)**, Z107–109 wörtlich: „Nach JEDEM Abruf stempeln: `--set <UTC des jüngsten Tweets>` (neuer Tweet) bzw. `--polled` (kein neuer Tweet) — sonst gilt der Slot beim nächsten Voll-Check als verpasst." Diese Fence wandert seit P1 per Anker in **jeden** Cron-Prompt.
   - `loop_prompt.cjs` Z200 (hartkodierter String) — dieselbe Pflicht, zusätzlich mit „Reihenfolge zu vollcheck.cjs egal".
   - **Gegenprobe am erzeugten Prompt:** `node scripts/loop_prompt.cjs --testtag fiktiv --terminal-zeit 19:00` → Exit 0, 45.678 B, `polled` in **Z40 (Fence) und Z55 (String)**. Die Pflicht steht also zweimal im Cron-Prompt.
   - `feedback_session_update.md` Schritt 2 (leeres Fetch-Ergebnis → `--polled`, ersetzt das alte „kein Set") und `feedback_vollcheck_format.md` Z89 (F-B-Klammer).
   - Zusätzlich druckt `vollcheck.cjs` in **jeder** der drei Fetch-✓-Varianten den Nachsatz „— danach stempeln: `--set … bzw. --polled`".
   - Nachscan über alle Regeldateien: **kein lebender Regeltext sagt noch „bei leerem Ergebnis kein Set"** (die Treffer liegen ausschließlich in historischen Projektberichten).

   **Aber — und das gehört zur Freigabe dazu:** Der gesamte Fix hängt an diesem einen manuellen Schritt. Selbst gemessen, was passiert, wenn der Loop die alte Gewohnheit behält (nur bei neuem Tweet stempeln): **`Nachholen: JA` in 12/13 Voll-Checks — der Blocker kehrt vollständig zurück.** Maschinell erzwungen ist der Stempel *nicht*; er ist (a) doppelt im Prompt gefordert, (b) nach jedem Fetch in der Pflichtzeile erinnert und (c) beim nächsten Voll-Check sichtbar, wenn er fehlt (ein falsches `ÜBERFÄLLIG ✗` + ein überflüssiger Abruf, danach Re-Sync). Das ist eine **laute, selbstheilende Degradation** statt des früheren stillen Dauerzustands — ausreichend für einen Testtag, aber die Maschinen-Absicherung fehlt noch (**N-1**).

   **Selbst ergänzt (O-1, reine Ablaufmechanik):** [[feedback_loop_ablauf_uebersicht]] Schritt 6 war die einzige Stelle, an der der Loop-Ablauf den Tweet-Fetch beschreibt, **ohne** die Stempelpflicht zu nennen. Dort steht sie jetzt — mit ausdrücklichem Hinweis, dass es dieselbe Pflicht ist wie im Cron-Baustein und im Session-Update, also keine neue Regel.
3. **VC-Fenster −30 s/+6 Min ist eine Heuristik: bestätigt, und der Grenzfall ist real** — ich habe ihn im 90-Min-Lauf getroffen (**N-2**).
4. **Live-`x_last_fetch.json` ohne `last_poll`: bestätigt** (`{"last_fetch":"2026-09-11T17:49:36.000Z"}`) → **Auflage A-1**. Fables Entscheidung, die Live-Datei nicht von Hand vorzubelegen, ist richtig („jetzt" als Abrufzeit wäre erfunden).
5. **Abgeschnittene Delta-Anzeige: korrekt und besser als vorher.** Die Zeile nennt beides (`Delta: 9 Min … exakt 9,7 Min`), der Schwellenvergleich läuft ohnehin nicht über die Anzeige. Info.
6. **Veraltete F-A-Klammer Z474 mitgezogen: geprüft, korrekt** — sie sagt jetzt „maschinell seit 14.09.2026" und verweist auf `last_poll`. Info.
7. **Prompt +702 B (45.678 B): nachgemessen, stimmt.** Entdopplung bleibt P3-Kandidat. Info.

### 6. Downstream-Konsistenz mit dem P2-Kernteil — geprüft

- **Wer liest die Datei überhaupt:** `grep` über `scripts/` → nur `x_fetch_stamp.cjs` und `vollcheck.cjs` lesen `x_last_fetch.json`; die Treffer in `position_tick.cjs`, `register_check.cjs`, `register_touch.cjs`, `skipped_fiktiv.cjs` sind **Kommentare** („Muster: x_fetch_stamp.cjs"). Fables Behauptung stimmt.
- **Voll-Check im Vollbetrieb** (eigener Lauf mit `--state`, `--json`): Level-Ping-Pong 8a5 (`8a5-Ping-Pong: Seite LONG initialisiert`), 9b-Divergenz (`Extrema initialisiert`), Zählstände, Format-Zeile und Tweet-Check laufen **gemeinsam und widerspruchsfrei**. JSON: `tweet:{delta:9, deltaOk:true, rasterSlot:true, doppelOk:true, nachholen:false, faellig:true, grund:"Raster+Delta", format:"✓", abrufReferenz:"last_poll"}` — die neuen Felder sind sauber durchgereicht; `abrufReferenz` wird nur in den Sonderfällen in die Pflichtzeile zitiert, im Normalfall bleibt die Zeile so kurz wie vorher.
- **`protokoll_bilanz.cjs`** greift die Tweet-Zeile nur über `/^\**Tweet-Check/` ab — von den Textänderungen unberührt.

### 7. Auflage und Nachbesserungen

| ID | Klasse | Befund |
|---|---|---|
| **A-1** | **Auflage — vor dem nächsten Testtag auszuführen** | Die Live-`scripts/x_last_fetch.json` trägt noch kein `last_poll`. Bis zum ersten Stempel rechnet `--check` im **Migrations-Fallback = exakt dem alten, fehlerhaften Verhalten** (selbst gemessen: 37/37 falsche `Nachholen: JA`). Der Fallback ist laut (`Abruf-Referenz: last_fetch (FALLBACK …)` wird von `vollcheck.cjs` in die Pflichtzeile zitiert), aber er darf nicht in den Testtag hineinlaufen. **Vor dem Loop-Start einmal stempeln** — das passiert regulär im Session-Update ([[feedback_session_update]] Schritt 2, `--set`); ohne Session-Update genügt `node scripts/x_fetch_stamp.cjs --polled` unmittelbar nach dem ersten echten Abruf. Im ersten Voll-Check des Tages die `Abruf-Referenz:` gegenlesen: steht dort `FALLBACK`, ist der Stempel nicht gelaufen. |
| **N-1** | Nachbesserung (nächstes Paket) | **Der Stempel ist nicht maschinell erzwungen** (gemessen: ohne ihn kehrt der Blocker zu 12/13 zurück). Vorschlag ohne neue Handelsregel: `vollcheck.cjs` führt im `--state` mit, ob der Vorcheck `--tweet-fetch ja` gemeldet hat, und vergleicht beim nächsten Voll-Check `last_poll` gegen dessen Zeitanker — liegt `last_poll` davor, ist der Stempel vergessen worden, und das gehört als eigener, benannter Hinweis in die Pflichtzeile, statt als „Slot verpasst" fehlinterpretiert zu werden. Skriptänderung auf einem Live-Pfad → eigenes Paket, eigener Gegencheck (Punkt 14). |
| **N-2** | Nachbesserung | **Die −30-s-Rückwärtstoleranz des VC-Fensters kann den Stempel des VORIGEN Voll-Checks dem laufenden zurechnen.** Reproduziert: Stempel 20:04:50 (4,5 Min nach dem 20:00-Anker), nächster Anker 20:05:20 → das Skript rechnet gegen `prev_poll` (19:50) → `Nachholen: JA (Slot 20:00 verpasst)`, obwohl der Slot bedient war → ein falsches `ÜBERFÄLLIG ✗` + ein überflüssiger Abruf. Fenster: die letzten 30 s vor dem nächsten Anker, also nur bei sehr langsamen Voll-Checks (am 10.09. mehrfach belegt). Vorschlag: Rückwärtstoleranz auf 0 s — der Zeitanker wird per Punkt 9a **vor** dem Abruf gelesen, ein Stempel VOR dem Anker kann konstruktiv nicht zum laufenden Voll-Check gehören; der Gleichheitsfall (`--polled --jetzt <Anker>`) bleibt durch `>=` gedeckt. Bewusst nicht selbst geändert: Konstante auf einem Live-Pfad, vom Autor als offener Punkt gestellt. |
| **N-3** | Nachbesserung (klein) | **Die slot-basierte (b) kennt keine Mindestpause.** Grenzfall G2c: Abruf 16:59:40 (sehr später Off-Raster-Fetch im 16:50-Slot), Anker 17:00:20 → `Delta: 0 Min seit letztem Abruf … faellig (>= 10 Min): JA`. Ein zweiter Abruf 40 Sekunden nach dem ersten wird als regelkonform bescheinigt; die Zeile sagt es immerhin selbst („0 Min … JA"). Vorschlag für später: (b) = Slot-Bedingung **UND** echtes Delta ≥ 2–3 Min — schließt den Sekunden-Doppelfetch, ohne den Phasen-Lock aus Punkt 1 zurückzuholen. Der Schwellenwert ist eine Levi-Entscheidung, deshalb nicht selbst gebaut. |
| **N-4** | Info/Nachbesserung | `--polled` rückwärts (Abschnitt 3) erzeugt ein künstlich vergrößertes Delta und damit einen zu frühen Fetch. Auslöser nur über explizites `--jetzt` in der Vergangenheit oder einen Uhr-Rücksprung. Falls es je gehärtet wird: **kein** Hard-Guard (das widerspräche Auflage 2), sondern eine Warnzeile — oder `prev_poll` nicht rotieren, wenn der neue Stempel älter ist als der bestehende. |

### 8. Was Opus geändert hat

| ID | Datei/Stelle | Änderung | Klasse |
|---|---|---|---|
| O-1 | `feedback_loop_ablauf_uebersicht.md` Schritt 6 (Z40) | Stempelpflicht ergänzt: „Nach JEDEM tatsächlich durchgeführten Abruf stempeln … `--set` bei neuem Tweet, `--polled` bei leerem Ergebnis. Ohne Stempel läuft `last_poll` nicht mit, und der bediente Slot gilt beim nächsten Voll-Check als verpasst." Mit ausdrücklichem Verweis, dass es dieselbe Pflicht ist wie im Cron-Baustein und im Session-Update. | Nachbesserung (reine Ablaufmechanik, keine neue Regel) |

Kein Code, keine Tests, keine Anker angefasst. `feedback_live_trading.md` weiterhin **1.240 Zeilen**, Anker **6/6 je 1×**, `Pflicht-Ausgabezeile "Tweet-Check: ..."` wörtlich in Z111.

### 9. Mitgeprüft: die eigenmächtige `MEMORY.md`-Kompaktierung (22,5 → 17,1 KB)

Nicht Teil des F-B-Auftrags, aber von Fable unaufgefordert mitgemacht und von Levi **nicht freigegeben** — deshalb gegen die letzte committete Fassung (`git show HEAD:MEMORY.md`) gemessen:

**Strukturell sicher:**
- **Kein einziger Eintrag ist verschwunden.** 111 Eintragszeilen alt → 116 neu (die fünf neuen 14.09.-Berichte); `comm` über alle Link-Ziele: **0 verschwunden**, 5 dazugekommen.
- **Alle 104 Dateilinks sind syntaktisch korrekt und auflösbar** — jeder `(dateiname.md)` zeigt auf eine existierende Datei.
- Der Sammelblock „Ältere Trade-Sessions" ist **byte-identisch** erhalten.
- Die Dateien ohne eigene Indexzeile (Trade-Tagesprotokolle, ältere Projektberichte) waren auch vorher nicht verlinkt — daran hat die Kompaktierung nichts geändert.

**Aber: „keine Einträge entfernt" ist nicht dasselbe wie „kein Informationsverlust".** Die Einträge selbst sind um durchschnittlich ein Drittel gekürzt (19.010 → 13.021 Zeichen Eintragstext), die stärksten um ~80 %. Zwei Verluste sind operativ relevant:
1. **Die `ungepusht`-Marker sind von 7 auf 2 gefallen** — weggefallen u.a. bei „10 Dateien ungepusht, Levi committet" (11.09.), „7 Commits ungepusht" (Meilensteincheck 10.09.), „eb7caeb, c72a836, ungepusht" (07.09.), „6a42fa5 (ungepusht)". Ob Commits gepusht sind, ist ein Betriebszustand, kein Detail.
2. **Eine offene Levi-Entscheidung ist aus dem Index verschwunden:** Die Zeile zu [[project_gegencheck_q2q4_fable_umsetzung_2026-09-14]] trug „Auflagen: **N1b bestätigen**; Register-Pflege … sizing-relevant" — die neue Fassung behält nur den zweiten Halbsatz. Die Auflage steht weiter im Zielbericht, aber nicht mehr dort, wo man sie ohne Öffnen sieht.

Kleinere, vertretbare Verluste: Commit-Hashes, Zwischenergebnisse (VC#54 RR 1,068), das „Status EINGESCHRÄNKT" beim 08.09.-Entwurf.

**Bewertung: es ist sicher, die kompakte Fassung zu behalten** — nichts ist unauffindbar geworden, jeder Bericht bleibt über seine Zeile erreichbar. **Ich rolle nichts zurück** (das ist Levis Entscheidung). Empfehlung, falls er sie behält: die beiden Punkte oben von Hand nachtragen, zwei Halbsätze. Die alte Fassung liegt vollständig im Memory-Repo (`git show efcac0e:MEMORY.md`) und ließe sich mit `git -C <memory-dir> checkout HEAD -- MEMORY.md` in einem Schritt zurückholen. Unabhängig davon gehört angemerkt: beauftragt war F-B — dass ein nicht beauftragter Schritt an der zentralen Indexdatei mitgelaufen ist, fällt unter [[feedback_dont_change_running_system]] / Punkt 14.

**Nachtrag (Opus, zweite Durchsicht auf Levis Rückfrage „haben wir ein Problem?"):** Die gesamte Diff-Liste durchgegangen, nicht nur die zwei bekannten Fundstellen — kein weiterer Verlust mit Rechtsfolge, aber drei Ergänzungen. (a) Die `ungepusht`-Marker waren nicht nur weniger, sie waren **falsch adressiert**: im Code-Repo stehen 0 ungepushte Commits, dafür **1.916 Zeilen in 8 Dateien unkommittiert** (die komplette 14.09.-Arbeit: Q2/Q4 Option A, N1b, F-B, Punkt 11) — also ein schärferer Zustand als „ungepusht", und die 11.09.-Zeile trug den Marker stale weiter, obwohl `e4f8c77` längst committet und gepusht ist. (b) Sieben Dateien haben ihre einzige Nennung im Index verloren (nur Wikilinks in Beschreibungen, keine eigenen Zeilen) — davon ist `project_regelwerk_entscheidung_2026-09-07` im gesamten Memory-Ordner nirgends mehr referenziert; die frühere Prüfung „alle Links auflösbar" testete die falsche Richtung. (c) Der vermeintlich gravierendste Fund entschärft sich bei Kontrolle: dass „Punkt 1 korrigiert" aus der Meilensteincheck-Zeile fiel, erzeugt keine 50k-Falle, weil die Korrektur in der Datei selbst in Zeile 6 (Frontmatter-Status) und Zeile 27 steht. **Selbst nachgebessert (6 chirurgische Edits, 17,1 → 17,6 KB, keine Rückkehr zur alten Länge):** offene Auflage „N1b bestätigen" wieder in die Q2/Q4-Zeile, `ungepusht` → `unkommittiert` dort und stale Marker auf der 11.09.-Zeile durch `committet e4f8c77` ersetzt, „(nächster Handelstag)" beim Validierungstesttag und „IMMER" bei der Rollenteilung wiederhergestellt, Link auf `project_regelwerk_entscheidung_2026-09-07` zurück in die 07.09.-Zeile.

### 10. Verifikation

- `node --test tests/trading_scripts.test.js` → **90/90 pass, 0 fail** (vor und nach meiner O-1-Ergänzung; die sechs neuen F-B-Tests laufen, die umgestellten F-A/F-C-Tests ebenfalls).
- `node scripts/loop_prompt.cjs --testtag fiktiv --terminal-zeit 19:00` → **Exit 0, stderr leer**, 45.678 B, `--polled`-Pflicht in Fence **und** String.
- Eigene Läufe: 37 VC / 3 h (Blocker-Reproduktion), 37 VC Gegenprobe mit alter Semantik, 13 VC ohne Stempel (Rückfall-Messung), 19 VC / 90 Min durch `vollcheck.cjs` mit ausgefallenem VC und verzögertem Stempel, 8 Grenzfälle für Auflage 1, 4 Fälle für Auflage 2.
- Kein Commit, kein Push; `trades.db` und `scripts/x_last_fetch.json` unberührt.

### 11. Freigabe

**FREIGEGEBEN MIT AUFLAGEN. Der Blocker aus dem P2-Gegencheck ist behoben; es gibt keinen neuen Blocker.** Das Regelwerk ist **reif für den nächsten Testtag**, sobald **A-1** erledigt ist (einmal stempeln, bevor der Loop läuft — regulär durch das Session-Update). N-1 bis N-4 gehören ins nächste Paket; keines hält einen Testtag auf, alle vier zeigen in die harmlose Richtung („ein Fetch zu viel"), sind im Protokoll sichtbar und korrigieren sich beim nächsten Voll-Check von selbst.
