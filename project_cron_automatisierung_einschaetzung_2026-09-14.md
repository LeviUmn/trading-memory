---
name: project-cron-automatisierung-einschaetzung-2026-09-14
description: "Einschaetzung (Opus 5, 14.09.2026): Lohnt es sich, die restlichen manuell kopierten CronCreate-Pflichtbausteine nach dem Muster von loop_prompt.cjs zu automatisieren? Kernbefund: Der Cron-Prompt hat seit 09.09. ZWEI Haelften — eine generierte (14.708 B) und eine von Hand kopierte (28.202 B in drei Code-Fences) — die sich thematisch ueberlappen, getrennt gepflegt werden und nachweislich auseinandergelaufen sind (W4: Schritt 0 fehlte in Item (8); N5: die dreigliedrige SL-Formel stand seit 11.09. korrekt im generierten Block und bis 14.09. falsch im kopierten). Empfehlung: Ja, aber als Anker-basierte Volltext-Injektion der drei bereits existierenden Code-Fences (Ansatz A), NICHT als weitere hartkodierte Strings. Die heute geaenderten Items (4)/(6b)/(8) brauchen KEINE Sonderprioritaet — sie stehen bereits auf der sicheren Mechanismus-Stufe. Keine Umsetzung, kein Commit."
metadata:
  node_type: memory
  type: project
  status: "P2 Kernteil FREIGEGEBEN MIT AUFLAGEN (Opus-Gegencheck 14.09.2026, kein Blocker gegen das Paket): 8a5-Level-Ping-Pong + 9b-Divergenz in vollcheck.cjs korrekt (eigene Zyklen-/Divergenz-Sequenzen, Pflichtzeile exakt ab 3. Zyklus, 12/13-Kerzen- und Reset-(b)-Grenzfall geprueft, alle 4 Fable-Lesarten bestaetigt und als O-2 in feedback_chartanalyse.md nachgetragen). F-A x F-B live reproduziert und SCHLIMMER als gemeldet: an ruhigen Tagen meldet JEDER Off-Raster-VC 'Nachholen JA' -> falsches kippendes UEBERFAELLIG-Kreuz, und ein befolgter 5-Min-Takt wird als 'regelkonform, KEIN Ueber-Polling' bescheinigt (Punkt-17-Schutz kehrt sich um) -> HARTE AUFLAGE: kein Testtag ohne F-B; F-B ist der zwingend naechste Schritt (mit Auflage: Bedingung (b) mitumbauen, Math.round-Rundungsfalle). O-1 selbst gefixt: --anlass/--tweet-anlass war ein Freibrief (jeder Text = faellig) -> Release-Uhrzeit wird jetzt gegen +/-15 Min gerechnet, ohne HH:MM Hard-Exit 1. Tests 84/84, loop_prompt Exit 0 (44.976 B). Vorher: P1 FREIGEGEBEN MIT AUFLAGEN, N1 geklaert."
  originSessionId: session_01E4zVYdeDA5XiMXhwykDWcw
  modified: 2026-09-14T19:06:56.157Z
---

# Cron-Prompt-Automatisierung — lohnt sich der Ausbau von `loop_prompt.cjs`? (Einschätzung, 14.09.2026)

**Auftrag:** Levis Frage nach der heutigen N5-Korrektur — der `vollcheck.cjs`-Teil des CronCreate-Prompts wird seit 09.09.2026 von `scripts/loop_prompt.cjs` erzeugt, der Rest (MTF-Vierschritt, Tweet-Check, Positions-Zeilen, Cron-Items) wird weiter von Hand aus `feedback_live_trading.md` kopiert. Soll das auch automatisiert werden?

**Was dieser Bericht ist:** eine Empfehlung. **An Code, Regelwerk oder DB wurde nichts geändert**, kein Commit, kein Push ([[feedback_dont_change_running_system]]). Alle Zahlen sind am 14.09.2026 am Dateisystem bzw. an einem echten `loop_prompt.cjs`-Lauf gemessen ([[feedback_memory_pflege]], Zitierpflicht). Vorgeschichte: [[project_punkt11_sl_empfehlung_2026-09-14]] (N5), [[project_pruefung_feedback_live_trading_2026-09-14]] (W4), [[project_testtag_analyse_2026-09-09]] (TODO 1), Kadenz-Grundlage: [[feedback_loop_tick_kadenz]].

---

## 1. Kurzfazit

**Ja, automatisieren — aber anders als die 09.09.-Lösung.** Der Cron-Prompt besteht heute aus zwei getrennt gepflegten Hälften: 14.708 B kommen aus `loop_prompt.cjs` (gemessen, Lauf `--testtag fiktiv --terminal-zeit 19:00`, Exit 0), 28.202 B werden aus drei Code-Fences in `feedback_live_trading.md` von Hand kopiert (Z68–80 + Z84–105 = 3.156 B, Z119–419 = 25.046 B). Beide Hälften sagen zu denselben sieben Themen etwas — in unterschiedlichen Worten, ohne Abgleichmechanismus. Genau dort sind heute **zwei belegte Drift-Fälle** aufgeschlagen: W4 (Item (8) ließ den bindenden Schritt 0 aus) und N5 (die dreigliedrige SL-Formel stand seit 11.09. korrekt im generierten Block und bis 14.09. falsch im kopierten Zweitexemplar).

**Der Hebel ist deshalb nicht "mehr Text generieren", sondern "die zweite Textquelle abschaffen":** die drei Blöcke sind bereits Code-Fences — also exakt die Struktur, die `loop_prompt.cjs` mit `fencesNach()` schon liest. Sie mit expliziten Ankern aus der Regeldatei zu injizieren ist eine kleine Ergänzung (Ansatz A), macht die Fehlerklasse "Kopieren vergessen / veraltete Fassung kopiert" strukturell unmöglich und kostet keinen einzigen zusätzlichen Prompt-Byte.

**Zur Prioritätsfrage nach den frischen 14.09.-Änderungen: nein, die brauchen keine Sonderbehandlung** — Item (4) druckt `position_tick.cjs` seit heute selbst, Item (6b) steht im injizierten Template-A-Block, Item (8) trägt die richtige Formel im generierten Block seit dem 11.09. Alle drei sind bereits abgesichert. Akut ungesichert ist etwas anderes (Abschnitt 4).

---

## 2. Bestandsaufnahme — was gehört in den Cron-Prompt, und wie ist es heute abgesichert

### 2.1 Die Mechanismus-Leiter

Aus der Projekthistorie lassen sich vier Absicherungsstufen unterscheiden, in absteigender Verlässlichkeit:

| Stufe | Mechanismus | Beleg |
|---|---|---|
| **1** | Ein Skript **druckt die Pflichtzeile selbst** oder erzwingt ein Feld per A3-Hard-Exit | 09.09.–14.09.: keine dieser Zeilen ist seither ausgefallen |
| **2** | Text steht im **generierten** Prompt-Teil (`loop_prompt.cjs`) | seit 09.09. im Einsatz, testgedeckt (14 Referenzen in `tests/trading_scripts.test.js`) |
| **3** | Text steht in einem **von Hand kopierten** Fence der Regeldatei | 3 Ausfälle (27.08., 02.09., 09.09.) + 2 Drift-Fälle (W4, N5) |
| **4** | Regel steht nur als **Fließtext** im Regelwerk | 08.09.: 13 Pflichtzeilen in 46/46 Voll-Checks ausgefallen |

Die ganze Frage lautet damit präzise: *Was steht heute auf Stufe 3 und 4 — und was davon lässt sich hochstufen?*

### 2.2 Inventar der Cron-Prompt-Bausteine

| # | Baustein | Fundstelle | Heutige Stufe | Geändert seit Einführung | Vergessens-Vorfall dokumentiert? |
|---|---|---|---|---|---|
| A | **MTF-Vierschritt-Block** (15→60→5, QQQ-Durchlauf) | Fence Z68–80 | **3** — 0 Treffer im generierten Block (`chart_set_timeframe` 0×). **Aber:** `vollcheck.cjs` macht `--close-1h/--ema50-1h/--rsi-1h/--macdh-1h/--struktur-1h/--ref-1h` zu A3-Pflichtfeldern (Z510/519) und rechnet die MTF-Frische ab Kerzenschluss (Z681) → faktisch zusätzlich Stufe 1 abgesichert | 16.07. verankert, 31.07. (QQQ zurück auf 15), 26.08., 31.08. (AVWAP-Session-Instanz) | **Ja, zwei** — Trade #18/#19, [[feedback_mtf_voll_check_wiederholt_vergessen]] |
| B | **Format-Zeile + Tweet-Check** | Fence Z84–105 | **1+3** — `vollcheck.cjs` ruft `x_fetch_stamp.cjs --check` selbst auf (TODO 4, 09.09.) und kippt die Format-Zeile bei Widerspruch; der Fence ist Erklärtext dazu | 27.07. (Einführung), 28.08. (Kerzenraster-Fix), 09.09. (Erzeugungsweg) | **Ja** — zweimaliger Tweet-Fetch-Ausfall 27.07.; 09.09. 18/18 falsche Rasterphase |
| C | **Loop-Start-Checkliste + Templates A/B** | Z109–120 Fließtext, Quelle `feedback_vollcheck_format.md` | **2** — `loop_prompt.cjs` | 09.09. (Einführung), 10.09., 11.09., 12.09. — **5 Commits in 4 Tagen** | **Ja** — 0/18 Nutzung am 09.09., dritter Rückfall (27.08./02.09./09.09.) |
| D | **Positions-Zeilen (1)–(4)**: Stall-Check, Positions-Kasten, Volumen-Check, Punkt-11-Kriterien + Vollexit-Schatten | Fence Z119–419, Items (1)–(4) | **1** — `position_tick.cjs` druckt alle vier selbst (Stall 3×, Volumen 9×, AVWAP 13×, `Vollexit-Schatten` 1× im Quelltext) | 26.08. (Einführung), 14.09. (Schattenzeile + `[Voll-Check]`-Marker) | Ja (26.08.-Soll/Ist-Abgleich) |
| E | **Voll-Check-Zeilen (5)–(6f)**: Regime-Gate, ADX, Chasing-Status, Retest-Zeitbox, Register-Check, 1H-Schatten, SL-Anker-Vorprüfung | Fence Z119–419 | **1** — alle von `vollcheck.cjs` gedruckt (Regime 2×, ADX 1×, Chasing 2×, Retest 16×, Register 11×, 1H-Schatten 5×, SL-Vorprüfung 9× im Quelltext) | 26.08./28.08./31.08./04.09./07.09./09.09./11.09./14.09. | Ja, mehrfach (08.09.: 13 Zeilen in 46/46 aus) |
| F | **(7) Cooldown-Check-Aufrufweg** inkl. Datei-Mitschnitt + Exit-Code-Zeile | Fence Z119–419 | **3 allein** — `cooldown_check` 0 Treffer im generierten Block | 04.07., 28.08., 31.08. (zweimal: `\| tee`-Rückbau, PowerShell `2>&1`) | Ja — 28.08. nur "🟢" statt Skript-Ausgabe protokolliert |
| G | **(8) `gate_check.cjs`-Aufrufweg** + Gate-Eingaben-Pflichtzeile + Befüllungspflicht + SL-Formel | Fence Z119–419 | **3 teils** — SL-Formel und A3-Pflicht stehen im generierten Block, **aber** `last_gate_check.txt` 0×, `Gate-Eingaben` 0×, die Exit-Code-Zeile 0× | 31.08. (3×), 07.09., 11.09., **14.09. (W4+N5)** | Ja — 28.08. "wörtlicher" Block war redigiert (7 Zeilen fehlten); **W4/N5 = Drift genau hier** |
| H | **(9) Chop-Check vor Voll-Entry** | Fence Z119–419 | **3 allein** — 0 Treffer im generierten Block | 03.08. | — |
| I | **(10)/(11)/(11a)/(11b) Trigger-Moment-Zeilen** | Fence Z119–419 | **1** — Spike-Ausnahme (5×/12×), Basis-Reclaim (1×), Stale-Check (5×), QQQ-EMA50-Lage (4×/2×) alle in den Skripten | 25.08., 27.08., 28.08., 31.08. | Ja — 27.08. Spike-Ausnahme 0/35 |
| J | **(11c) Level-Ping-Pong (8a5)** | Fence Z119–419 | **3 allein** — **0 Treffer in `vollcheck.cjs` UND `position_tick.cjs`**, 0 im generierten Block | 31.08. (Einführung) | — (Klasse ist noch nie ausgelöst worden) |
| K | **9b-Divergenz-Check vor jeder Order** | Fence Z119–419 | **3 allein** — **0 Treffer in beiden Skripten**, 0 im generierten Block | 27.08. (Einführung + Format-Nachbesserung am selben Tag) | **Ja** — Testtag 24.08., Short #T1 |
| L | **(12)/(12a) AVWAP-Anker setzen/zurücksetzen** | Fence Z119–419 | **3** — `position_tick.cjs` liest den Anker zurück (`--avwap-anker`), die Tool-Handlung selbst steht nur im Fence; `AVWAP` 1× im generierten Block (Template B) | 26.08., 31.08. (Ereignis-/Session-Instanz), 31.08. (Reset-Regel) | **Ja** — 28.08.: Entry-Anker blieb nach Breakeven-Stop bis Sessionende stehen |
| M | **(13) Solo-Mandat bei "Stop"** | Fence Z119–419, Punkt 15 | **2 indirekt** — `loop_stopp.cjs` Exit 2 im generierten Block, das Wort "Solo-Mandat" 0× | 26.08. | Ja (25.08./08.09., Meilensteincheck Punkt 4) |
| N | **`add_trade.cjs --dry-run` nach fiktivem Trade** | Fließtext nach dem Fence | **4** — 0 Treffer im generierten Block, nicht einmal im Fence | 31.08. | **Ja** — 28.08. 0 Aufrufe trotz abgeschlossenem fiktiven Trade, Kriterium 4 des Validierungstesttags gescheitert |

### 2.3 Die zwei Zahlen, die alles erklären

- **Änderungsfrequenz des handkopierten Fences Z119–419:** 11 verschiedene Änderungsdaten in drei Wochen (24.08., 26.08., 27.08., 28.08., 31.08., 04.09., 07.09., 09.09., 10.09., 11.09., 14.09.) — allein der 31.08. hinterließ 20 Datumsmarker. Ein Baustein, der alle zwei Tage geändert wird, aber per Hand kopiert werden muss, ist per Konstruktion irgendwann veraltet im Prompt.
- **Thematische Doppelung zwischen beiden Hälften** (Treffer generierter Block vs. Fence Z119–419): `SL = MAX` 1/2 · `8c-Floor` 2/3 · `cluster-level` 3/5 · `Register-Check` 1/2 · `add_skipped_setup` 2/2 · `punkt11-signal` 2/1 · `Q-Score` 2/1 · `Regime` 1/2. Jedes dieser Themen wird heute an **zwei** Stellen in **zwei** Formulierungen gepflegt.

---

## 3. Muster-Analyse — was `loop_prompt.cjs` am 09.09. wirklich gelöst hat

**Der verbreitete Kurzschluss wäre: "es hat geholfen, weil der Text jetzt im Prompt steht".** Das stimmt nur zur Hälfte — der Text stand beim MTF-Block seit 16.07. im Prompt und ist trotzdem nie generiert worden. Was am 09.09. tatsächlich neu war, sind drei Eigenschaften, und sie sind unterschiedlich gut übertragbar:

| Eigenschaft | Was sie leistet | Übertragbar auf A/F/G/H/J/K/L/M/N? |
|---|---|---|
| **(a) Einzige Quelle** — Template A/B werden aus `feedback_vollcheck_format.md` gelesen, nicht abgetippt | Regelwerk und Prompt können nicht auseinanderlaufen | **Ja, 1:1** — die drei Blöcke sind bereits Code-Fences, `fencesNach()` kann sie lesen |
| **(b) Pflichtparameter mit Hard-Exit** (`--testtag`, Quelle fehlt → Exit 1) | Sessionentscheidungen können nicht vergessen werden | **Nein** — das ist parametrisch, es gibt bei A/F/G/H/J/K/L/M keine Variable |
| **(c) Injektion in die Kommandozeile** (`--state`, `--testtag` wandern in Template A) | steht bei JEDEM Fire in der CLI, nicht nur bei VC#1 | **Nein** — betrifft nur maschinell verarbeitete Felder |

**Das ist die entscheidende Unterscheidung, die Levis Frage aufwirft:** der `vollcheck.cjs`-Teil ist mechanisch/parametrisch, der MTF-Block und die Cron-Items sind Handlungsanweisungen in Prosa. **Für (b) und (c) ist das ein echter Unterschied — für (a) nicht.** (a) verlangt keine Parametrisierung, nur einen stabilen Lesepunkt. Und (a) ist genau die Eigenschaft, an der W4 und N5 gescheitert sind.

**Der unangenehme Zusatzbefund:** `loop_prompt.cjs` hat (a) nur für Template A/B umgesetzt. Alles andere im Block sind **hartkodierte `block.push(...)`-Strings** (Z103–145) — also eine *dritte* Textquelle neben Regelwerk-Fließtext und Regelwerk-Fence. Bei N5 war es ausgerechnet diese dritte Quelle, die als einzige korrekt war (dreigliedrige SL-Formel seit 11.09.), während der handkopierte Fence bis heute Mittag `SL=MAX(Struktur,Floor)` sagte. Das ist Glück gewesen, kein Mechanismus: Fable hatte den String beim 11.09.-TODO mitgezogen, den Fence nicht. **Wer jetzt weitere Bausteine als hartkodierte Strings nach `loop_prompt.cjs` schreibt, vervielfacht dieses Problem statt es zu lösen.**

---

## 4. Konkreter Umsetzungsvorschlag, priorisiert

### P1 — Anker-basierte Volltext-Injektion der drei Fences (Ansatz A)

**Was:**
1. **In `feedback_live_trading.md`** je eine unsichtbare Ankerzeile unmittelbar VOR die öffnende Fence setzen (HTML-Kommentar, ändert an der Darstellung nichts):
   `<!-- CRON-BAUSTEIN: mtf -->` vor Z68 · `<!-- CRON-BAUSTEIN: format-tweet -->` vor Z84 · `<!-- CRON-BAUSTEIN: pflichtzeilen -->` vor Z119.
2. **In `loop_prompt.cjs`** eine zweite Quelldatei aufnehmen (`--regelwerk-datei`, Kandidatenpfade analog `MEMORY_DIR_CANDIDATES`) und `fencesNach()` mit Anker-Regex wiederverwenden — die Funktion kann das heute schon, sie braucht nur eine andere Markerzeile. Fehlender Anker / nicht extrahierbare Fence → **Exit 1 mit derselben Meldung wie heute bei Template A/B** ("Quelle reparieren, NICHT aus dem Gedächtnis ersetzen").
3. **Reihenfolge im Block:** MTF-Fence + Format/Tweet-Fence vor die Templates, Pflichtzeilen-Fence dahinter — also genau die Reihenfolge, in der beide Hälften heute per Hand zusammengeklebt werden.
4. **Regeltext-Konsequenz (wichtig, sonst Doppeltext):** Loop-Start-Checkliste Schritt 2 sagt heute "*zusätzlich* zu den bestehenden Pflichtbausteinen (MTF-Vierschritt, Tweet-Check, Positions-Zeilen bleiben unverändert Pflicht)". Das muss zu "**die Ausgabe IST der vollständige Cron-Prompt — nichts mehr von Hand dazukopieren**" werden.
5. **Selbsttest (Schritt 3) erweitern** um drei Zeichenketten aus den neu injizierten Teilen, z.B. `chart_set_timeframe(60)`, `cooldown_check.cjs`, `last_gate_check.txt`.

**Warum genau so:** Anker statt Prosa-Regex ist hier nicht Geschmackssache. Die Einleitungssätze der drei Fences sind lange, datierte Prosazeilen ("**Pflicht-Baustein im CronCreate-Prompt: MTF-Zwei-Schritt-Block wörtlich ausschreiben (verankert 16.07.2026 …)**") — und es stehen **30 Kürzungsvorschläge K1–K30 aus [[project_pruefung_feedback_live_trading_2026-09-14]] offen**, die genau solche Einleitungen treffen würden. Ein Regex darauf bricht beim ersten Kürzungsdurchgang. Ein HTML-Kommentar-Anker überlebt jede Prosaänderung und fällt beim Kürzen sichtbar auf.

**Effekt:** Bausteine A, F, G, H, J, K, L, M steigen von Stufe 3 auf Stufe 2. Die Fehlerklasse "Kopieren vergessen / veraltete Fassung kopiert" (= W4, N5, 27.08., 02.09., 09.09.) verschwindet vollständig. Prompt-Größe bleibt unverändert bei ~43 KB — es wird nichts hinzugefügt, nur der Kopierweg ersetzt.

### P2 — Die zwei Pflichtzeilen, die KEIN Skript druckt, in die Skripte holen (Stufe 1 statt Stufe 2)

`Level-Ping-Pong` (11c) und `9b-Divergenz-Check` sind die **einzigen** zwei Pflichtzeilen des ganzen Fences mit 0 Treffern in `vollcheck.cjs` *und* `position_tick.cjs`. Der 9b-Check hat einen dokumentierten Ausfall (Short #T1 am 24.08., der Grund seiner Einführung). Für diese beiden ist mehr Prompt-Text die schwächere Antwort: sie gehören als gedruckte Zeile in `vollcheck.cjs` (11c, zählerbasiert — der 8a5-Zyklenzähler liegt bereits im State) bzw. `gate_check.cjs` (9b, als Anzeige-Zeile mit eigenem A3-Feld).

**Das ist der Punkt, an dem ich Fable lieber Code als Injektion geben würde** — und er hat mehr Wert als P1, wenn nur eines davon gemacht wird.

### P3 — Entdopplung der hartkodierten Strings (erst nach einem sauberen Testtag)

Nach P1 stehen sieben Themen zweimal im Prompt (Abschnitt 2.3). Die hartkodierten `block.push`-Absätze Z116–129 inhaltlich gegen den injizierten Fence abgleichen; wo deckungsgleich: auf eine Verweiszeile eindampfen. Zielgröße ~30 KB statt 43 KB pro Fire. **Nicht gleichzeitig mit P1 machen** — sonst ist bei einem Problem am nächsten Testtag nicht unterscheidbar, ob die Injektion oder die Streichung schuld war ([[feedback_dont_change_running_system]]).

### P4 — Ansatz B als Dauer-Netz, nicht als Ersatz

`node scripts/loop_prompt.cjs --pruefe-prompt <datei>` mit einer Stichwortliste, die aus **derselben Quelle** abgeleitet wird (nicht handgepflegt — sonst hat man die Liste als vierte Textquelle). Fängt den Restfall "Prompt nach dem Generieren von Hand nachbearbeitet" und ist billig. **Aber: Ansatz B allein wäre die falsche Wahl** — er erkennt, dass ein Stichwort fehlt, nicht dass die kopierte Fassung *veraltet* ist. W4 und N5 hätte er beide durchgelassen, weil die Stichwörter ("SL", "MAX", "gate_check.cjs") dastanden. Genau deshalb ist A hier nicht durch B ersetzbar.

### Nicht automatisieren

- **`--testtag` / `--terminal-zeit`** — echte Sessionentscheidungen, bereits korrekt als Pflichtparameter gelöst.
- **Die Regelwerks-Prosa außerhalb der Fences** — sie ist Begründung ("Why"), nicht Anweisung. Sie gehört nicht in den Prompt, der jede Minute feuert.
- **Baustein N (`add_trade.cjs --dry-run`)** steht heute auf Stufe 4 und hat einen dokumentierten Ausfall — aber er gehört nicht in den 1-Min-Prompt, sondern in den Tagesabschluss ([[feedback_tagesabschluss]]) bzw. in `protokoll_bilanz.cjs` als Abschlussprüfung. Ihn in den Cron-Prompt zu heben wäre die falsche Stelle.

### Zur Prioritätsfrage aus dem Auftrag: die frischen 14.09.-Änderungen

Die Vermutung "frisch geändert = akut fehleranfällig, wie beim MTF-Block" trifft hier **nicht** zu — verifiziert:

| Frische Änderung | Status |
|---|---|
| **Item (8), SL-Formel (N5)** | Der generierte Block trägt `SL = MAX(Anker ∓ 0,5×ATR, sichere Cluster-Kante ∓ 0,5×ATR, 8c-Floor)` seit dem 11.09. wörtlich. Heute wurde der *handkopierte* Zweittext nachgezogen. **Stufe 2 war durchgehend intakt.** |
| **Item (4), Vollexit-Schatten 2/4** | Seit heute druckt `position_tick.cjs` die Zeile selbst inkl. `[Voll-Check]`-Marker → **Stufe 1**, das stärkste Mittel. |
| **Item (6b), `--punkt11-signal` ≥1 Kriterium** | Steht in `feedback_vollcheck_format.md` Z150, innerhalb der zweiten Fence nach `*Template A` → **wird injiziert, Stufe 2.** |

**Keine der drei braucht Sonderpriorität.** Akut ungesichert (Stufe 3 *ohne* Skript-Rückfall) sind stattdessen: (7) Cooldown-Aufrufweg, (8) Aufrufweg/Gate-Eingaben-Zeile, (9) Chop-Check, (11c), 9b-Divergenz, (12)/(12a). Die erfasst P1 gesammelt, P2 die zwei schwersten einzeln.

---

## 5. Aufwand und Risiko

**Aufwand (grob, für Fable):**

| Paket | Umfang | Einschätzung |
|---|---|---|
| P1 | 3 Ankerzeilen im Regelwerk + ~40–60 Zeilen in `loop_prompt.cjs` (zweite Quelldatei, Kandidatenpfade, `fencesNach`-Wiederverwendung, 3 Exit-1-Pfade) + Regeltext-Anpassung Loop-Start-Checkliste Schritt 2/3 + 3–4 Tests | **kleine Ergänzung**, kein Umbau — `fencesNach()` und die Fail-Safe-Logik existieren bereits |
| P2 | zwei neue Ausgabezeilen + A3-Feld in `gate_check.cjs`, Zählerlogik 8a5 in `vollcheck.cjs` (State ist da) + Tests | **mittel** — echte Skriptänderung an zwei Live-Pfaden, braucht Gegencheck |
| P3 | Urteilsarbeit, kein Code-Problem | **mittel**, aber erst nach einem sauberen Testtag |
| P4 | ~20 Zeilen + 1 Test | **klein** |

**Risiken der Automatisierung selbst — ehrlich:**

1. **Formatbruch der Lesequelle.** Das ist die reale Gefahr (W1–W5 zeigen, wie volatil diese Datei ist, und K1–K30 stehen noch offen). Drei Gegenmittel, alle schon im bestehenden Muster vorhanden: expliziter Anker statt Prosa-Regex · Hard-Exit 1 mit "Quelle reparieren, NICHT aus dem Gedächtnis ersetzen" · ein Test, der den Block gegen Regelwerksinhalt prüft (`tests/trading_scripts.test.js` Z1607 macht das heute schon für `--ema50-5min`). **Fail-Richtung ist sicher:** Anker weg → Exit 1 → Loop startet nicht. Das ist die richtige Richtung; die heutige Alternative ist "Loop startet mit veraltetem Text", und genau das ist dreimal passiert.
2. **Doppeltext.** Wird P1 gebaut, aber die Loop-Start-Checkliste nicht angepasst, steht jeder Baustein zweimal im Prompt. Deshalb ist Schritt 4 von P1 kein Beiwerk, sondern Teil der Lieferung.
3. **Scheinsicherheit.** Injektion garantiert, dass der Text *dasteht* — nicht, dass er befolgt wird. Am 09.09. wurde `vollcheck.cjs` 0/18 genutzt, obwohl es freigegeben war; der Prompt hat das gedreht, aber Stufe 1 (Skript druckt/erzwingt) bleibt strikt stärker als Stufe 2. Wo eine Zeile maschinell erzeugbar ist, ist Injektion die zweitbeste Lösung.
4. **Kein neues Risiko:** die Prompt-Größe. P1 ersetzt einen Kopierweg, es kommt kein Byte hinzu. P3 würde sie sogar senken — relevant, weil der Prompt bei jedem Fire durchläuft ([[feedback_loop_tick_kadenz]]) und am 09.09. 21 von 22 fehlenden Quick-Tick-Minuten direkt hinter einem Voll-Check lagen.

---

## 6. Gegenprobe — spricht etwas für den Kopiervorgang?

Ich habe nach dem stärksten Gegenargument gesucht: *Der manuelle Kopiervorgang ist ein bewusster Moment, in dem vor dem Loop-Start noch einmal gegengelesen wird.*

**Empirisch hält das nicht.** Der Kopiervorgang ist dreimal komplett ausgefallen (27.08., 02.09., 09.09.) und hat zweimal veralteten Text weitergereicht (W4: Schritt 0 fehlte seit 07.09. in Item (8) — gefunden erst am 14.09. durch eine Volllesung, nicht durch einen Loop-Start; N5: falsche SL-Kurzformel vom 10.09. bis 14.09., während der Loop am 10.09. 40× einen SL exakt auf dem Anker setzte). Ein Kontrollmoment, der fünfmal nicht kontrolliert hat, ist kein Kontrollmoment.

**Trotzdem drei Einschränkungen, die ich nicht wegdiskutiere:**

1. **Der Kopiervorgang ist heute der einzige Zeitpunkt, an dem der Fence überhaupt gelesen wird.** Fällt er weg, liest den Block niemand mehr — auch nicht der, der ihn ändert. Das ist real. Die Antwort darauf ist aber nicht "weiter kopieren", sondern **den Selbsttest aus P4 behalten** und beim Loop-Start eine Zeile "3 Bausteine injiziert, Quelle Stand \<Datum\>, \<n\> Zeilen" ausgeben, damit eine stille Änderung nicht unbemerkt mitläuft.
2. **Der Fence ist nach der 09.09.-Skriptumstellung vermutlich in Teilen überflüssig geworden.** Die Items (1)–(6f) und (10)–(11b) beschreiben Zeilen, die die Skripte heute selbst drucken — das sind grob 15 der 25 KB. Sie mechanisch weiter zu injizieren konserviert Text, den man eigentlich prüfen und kürzen müsste. **Automatisierung darf hier nicht die Kürzungsfrage ersetzen** (K1–K30 sind ohnehin offen). Deshalb P3 und deshalb ausdrücklich *nach* P1: erst den Mechanismus sichern, dann den Inhalt eindampfen — nicht beides gleichzeitig.
3. **Eine Vollautomatisierung kann die Disziplinfrage nicht lösen.** Der Meilensteincheck 24.08.–10.09. hat sie klar benannt: das Regelwerk wird besser, die Ausführung nicht. `loop_prompt.cjs` hat den Prompt zuverlässig gemacht — dass am 10.09. trotzdem 40× ein SL auf dem Anker landete, lag nicht am Prompt-Mechanismus. **Erwarte von P1 die Beseitigung einer Fehlerklasse, nicht einen besseren Testtag.**

**Fazit der Gegenprobe:** Automatisieren ja — aber mit dem klar ausgesprochenen Vorbehalt, dass P1 nur eine von mehreren Fehlerklassen schließt, und dass die eigentlich wertvollere Bewegung dieselbe ist, die am 09.–14.09. schon dreimal funktioniert hat: **eine Pflichtzeile vom Prompt in ein Skript verlegen** (Stufe 2 → Stufe 1). Deshalb steht P2 inhaltlich über P1, auch wenn P1 billiger ist.

---

## 7. Offene Punkte für Levi

1. **P1 freigeben?** (Anker + Injektion der drei Fences, kleine Ergänzung, kein Umbau)
2. **P2 freigeben?** (Level-Ping-Pong + 9b-Divergenz in die Skripte — der inhaltlich stärkere, aber teurere Schritt)
3. **P3/K1–K30:** Soll der Pflichtzeilen-Fence nach P1 auf das gekürzt werden, was die Skripte *nicht* schon drucken? Das ist eine Kürzungsentscheidung, keine Mechanismusentscheidung — gehört zu [[project_pruefung_feedback_live_trading_2026-09-14]].
4. **Reihenfolge:** Punkt 14 (Regeländerungs-Tempo-Bremse) rät zu einem Paket pro Testtag. Empfehlung: **P1 allein vor dem nächsten Testtag**, P2 danach, P3/P4 zuletzt.

**Unabhängig von jeder Entscheidung gilt die Auflage aus dem heutigen Gegencheck weiter:** Vor dem nächsten Testtag muss der Cron-Block ab Z119 in seiner aktuellen Fassung neu in den CronCreate-Prompt kopiert werden — solange P1 nicht gebaut ist, bleibt das ein Handgriff, den nur ein Mensch machen kann.

## ENTSCHIEDEN 14.09.2026 (Levi): Gestuftes Vorgehen P1 -> Gegencheck -> P2 -> Gegencheck

Levi: "Ja bitte so lösen, P1 an Fable zur Umsetzung, danach Opus gegenprüfen und wenn Opus sagt alles in Ordnung, dann P2 behandeln von Fable, damit es auch seinen eigenen Gegencheck bekommt". Ablauf: (1) Fable setzt P1 um (loop_prompt.cjs liest die 3 Cron-Bausteine per Anker direkt aus feedback_live_trading.md statt Handkopie). (2) Opus-Gegencheck P1. (3) NUR bei FREIGEGEBEN/FREIGEGEBEN MIT AUFLAGEN ohne Blocker: Fable setzt P2 um (Level-Ping-Pong 11c + 9b-Divergenz-Check in vollcheck.cjs/position_tick.cjs nachbauen, 0 Treffer im Code bestätigt). (4) eigener Opus-Gegencheck P2. Bis P1 verifiziert ist, gilt weiter die Auflage aus dem Punkt-11/N5-Gegencheck: Cron-Block ab Z119 vor dem naechsten Testtag von Hand neu kopieren.

## P1 umgesetzt 14.09.2026 (Fable) — Opus-Gegencheck ausstehend

**Was gebaut wurde (kein Commit, kein Push — Levi committet selbst):**

1. **Sechs HTML-Kommentar-Anker in `feedback_live_trading.md`** (unsichtbar beim Rendern, je als eigene Zeile unmittelbar vor der öffnenden / nach der schließenden Fence; Fence-Inhalte byteweise unverändert; Datei 1.222 → 1.229 Zeilen inkl. Checkliste):
   - `<!-- CRON-BAUSTEIN-MTF-START -->` Z68 / `<!-- CRON-BAUSTEIN-MTF-ENDE -->` Z82 — Fence Z69–81, 11 Zeilen, 1.111 B (MTF-Vierschritt-Block)
   - `<!-- CRON-BAUSTEIN-FORMAT-TWEET-START -->` Z86 / `…-ENDE -->` Z109 — Fence Z87–108, 20 Zeilen, 1.701 B (Format-Zeile + Tweet-Check)
   - `<!-- CRON-BAUSTEIN-PFLICHTZEILEN-START -->` Z123 / `…-ENDE -->` Z425 — Fence Z124–424, 299 Zeilen, 25.037 B (Pflichtzeilen (1)–(13))
   - Verifikation der alten Grenzen: Z68–80 / Z84–105 / Z119–419 stimmten am aktuellen Text noch exakt (W1–W5/N5 lagen innerhalb der großen Fence, nicht an den Rändern).
   - **Der große Block bleibt EIN Block:** Er ist eine einzige Markdown-Fence; ein Split hätte die Fence aufgebrochen (Strukturänderung über „Anker drumherum" hinaus) und P1 mit der noch offenen Kürzungsfrage (P3/K1–K30) vermischt — genau das, was Abschnitt 4 P3 verbietet. Aufteilen bleibt nach einem sauberen Testtag als Teil von P3 möglich.
2. **`scripts/loop_prompt.cjs`:** zweite Quelldatei `feedback_live_trading.md` (`--regelwerk-datei <Pfad>`, sonst dieselben `MEMORY_DIR_CANDIDATES`; gemeinsame Hilfsfunktion `findeQuelle()` für beide Quellen). Tabelle `CRON_BAUSTEINE` (Anker, Titel, Selbsttest-Kernfragmente); `bausteinZwischenAnkern()` extrahiert die EINE Fence zwischen Start-/Ende-Anker (Vergleich per `trim()`, Anker muss eigene Zeile sein). **Exit-1-Pfade mit Nennung:** Start fehlt / Ende fehlt / beide fehlen / Anker mehrfach / Ende vor Start / keine bzw. mehr als eine Fence / Fence leer — alle drei Bausteine werden geprüft und in EINER Meldung aufgelistet („2 von 3 fehlerhaft: … FEHLT (Ende-Anker steht in Z81) … ok (20 Zeilen)"), Schlusstext „Quelle reparieren (Anker beim Kuerzen mit entfernt? -> wieder einsetzen), NICHT aus dem Gedaechtnis ersetzen, NICHT von Hand kopieren. Loop NICHT starten. Exit 1." Datei nicht gefunden = Exit 1 wie bei der Format-Datei. `--nur-templates` braucht das Regelwerk weiterhin nicht.
   **Einbau-Reihenfolge** (Abschnitt 4 P1 Schritt 3): `date`-Zeile → MTF → Format/Tweet → bestehender Voll-Check-Aufruf (Template A) → Template B → **Pflichtzeilen** → Terminalbedingung → LOOP-STOPP → Quick-Tick. Jeder Baustein bekommt eine Kopfzeile `--- CRON-BAUSTEIN <Titel> — woertlich aus feedback_live_trading.md, Anker <Name>, <n> Zeilen ---`; Zeile 2 des Blocks meldet „Cron-Bausteine (P1, 14.09.2026): 3/3 per Anker aus feedback_live_trading.md (Stand <mtime DE>) injiziert — MTF 11 Zeilen | FORMAT-TWEET 20 Zeilen | PFLICHTZEILEN 299 Zeilen. Diese Ausgabe IST der vollstaendige Cron-Prompt — nichts mehr von Hand dazukopieren." (= das in Abschnitt 6 Punkt 1 geforderte Sichtbarmachen einer stillen Quelländerung).
   **Selbsttest jetzt im Code** (bisher nur Regeltext, Checkliste Schritt 3): am FERTIGEN Prompt `node scripts/vollcheck.cjs`, `--state`, `--testtag` + je Baustein Kernfragment IM Baustein-Text (`chart_set_timeframe(60)` / `Tweet-Check:` / `cooldown_check.cjs` + `last_gate_check.txt`) + Baustein-Text IM Prompt. Fehlschlag = `SELBSTTEST FEHLGESCHLAGEN … Exit 1`, nichts auf stdout. Schützt gegen eine Kürzung, die den Anker stehen lässt, aber den Kern der Anweisung entfernt.
3. **Regeltext-Konsequenz (P1 Schritt 4, außerhalb der Fences):** Loop-Start-Checkliste Schritt 1 (Ankerlesen erwähnt, „beim Kürzen stehen lassen"), Schritt 2 („sie IST der vollständige Cron-Prompt, nichts mehr von Hand dazukopieren"), Schritt 3 (Selbsttest-Zeichenketten um die vier Kernfragmente erweitert; Skript führt ihn selbst aus, trotzdem am eingefügten Prompt gegenprüfen). Die Einleitungsprosa der drei Fences („wörtlich in den Prompt übernehmen") wurde NICHT angefasst — sie bleibt sachlich richtig, nur macht das jetzt das Skript.
4. **Tests `tests/trading_scripts.test.js`:** Sandbox-Helfer `regelwerkDatei()` (drei Ankerpaare; steuerbar: Anker weg / Fence weg / Text ersetzt / Doppel-Anker); bestehende loop_prompt-Tests bekommen `--regelwerk-datei` (Sandbox statt Memory-Datei). Neuer Block „P1 Cron-Automatisierung": (a) Volltext + Reihenfolge + Kopfzeilen + keine Ankerzeilen im Prompt + Echtlauf gegen das ECHTE Regelwerk (3 eindeutige Ankerpaare, Exit 0, Kernsätze `chart_set_timeframe(60) auf NAS100 …`, `(13) Solo-Mandat`, `9b-Divergenz-Check`, `(11c) "Level-Ping-Pong`, `last_gate_check.txt 2>&1; echo "Exit-Code: $?"`); (b) 7 Defektfälle je Exit 1 mit Ankernennung, stdout leer, Datei nicht gefunden; (c) Selbsttest bei weggekürztem Fragment (1 und 2 Befunde) und bei Template A ohne `vollcheck.cjs`. **Ergebnis: 77/77 grün (vorher 74/74).**

**Echtlauf** `node scripts/loop_prompt.cjs --testtag fiktiv --terminal-zeit 19:00` gegen die echte Datei: Exit 0, stderr leer, **43.496 B / 418 Zeilen** (Abschnitt 1 schätzte ~43 KB: 14.708 + 28.202 B) — Auszug ab Zeile 6 des generierten Prompts:

```
--- CRON-BAUSTEIN MTF-Vierschritt-Block (Voll-Check, Punkt 2b) — woertlich aus feedback_live_trading.md, Anker CRON-BAUSTEIN-MTF, 11 Zeilen ---
Wenn aktuelle Minute % 5 == 0 → Voll-Check (Punkt 9), darin als fester Vier-Schritt-Block:
chart_set_timeframe(15) auf NAS100 → Werte lesen →
chart_set_timeframe(60) auf NAS100 → Werte + letzte 5 Bars auf HH-HL/LH-LL-Struktur lesen →
zurück auf chart_set_timeframe(5) auf NAS100 →
…
Der 1H-Schritt (NAS100) UND der QQQ-Schritt (falls Gate offen) sind NICHT optional — ein
Voll-Check ohne den 60-Min-Wechsel bzw. ohne den QQQ-Durchlauf gilt als NICHT durchgeführt
```
Positionsmarker im Output: MTF Z6–17, Format/Tweet Z19–39, `node scripts/vollcheck.cjs --state … --testtag fiktiv` Z43, `Situativ:` Z103, Pflichtzeilen Z110–409 (`(13) Solo-Mandat` Z404), Terminalbedingung Z411, Ende Z418. Negativlauf mit einer Kopie ohne `MTF-START` und `PFLICHTZEILEN-ENDE`: Exit 1, stdout 0 B, beide Anker benannt.

**Ist die Auflage „Cron-Block ab Z119 vor dem nächsten Testtag neu kopieren" erledigt?** Mechanisch ja: Schritt 1 der Checkliste (`loop_prompt.cjs`) liefert jetzt den aktuellen Stand aller drei Fences automatisch, ein Handgriff existiert nicht mehr — die Fehlerklasse ist geschlossen, nicht nur ihre letzte Instanz. **Formal bleibt sie laut Levis Entscheidung (Abschnitt „ENTSCHIEDEN") bis zum Opus-Gegencheck bestehen**; mit FREIGEGEBEN ist sie durch Checkliste Schritt 1–3 ersetzt und kann gestrichen werden. Empfehlung: nach dem Gegencheck den Satz „solange P1 nicht gebaut ist …" in Abschnitt 7 als erledigt markieren.

**Offene Punkte für den Opus-Gegencheck (nichts davon geändert):**
1. **Doppeltext bleibt absichtlich** (P3, nicht P1): die MTF-Fence endet mit „Sonst (Minute % 5 != 0) → Quick-Tick (Punkt 3)." und der generierte Block trägt seine eigene Voll-Check-Weiche + Quick-Tick-Zeile; die Format/Tweet-Fence beschreibt die Fetch-Regel als „Minute % 10 == 0 → Pflicht-Fetch + x_last_fetch.json lesen", der hartkodierte String sagt „Doppelbedingung (Minute % 10 == 0 UND Delta >= 10) laut Skript" — **zwei Formulierungen derselben Regel, die Fence nennt das Delta nicht.** Das ist genau die Drift-Klasse aus Abschnitt 2.3; Kandidat Nr. 1 für P3.
2. Selbsttest-Kernfragmente sind vier hartkodierte Strings in `CRON_BAUSTEINE` (die aus Abschnitt 4 P1 Schritt 5 vorgeschlagenen plus `Tweet-Check:`). Bewusst minimal; wer die Fence-Kerne umformuliert, muss die Tabelle nachziehen — Exit 1 macht das sichtbar, nicht stumm.
3. Anker werden mit `trim()` verglichen — Einrückung egal, aber der Anker muss eine eigene Zeile sein; ein Anker INNERHALB einer Fence würde als Fence-Text mitinjiziert (so nicht gesetzt, nur als Regel für künftige Bearbeiter).
4. Das Memory-Git meldet beim `diff` „LF will be replaced by CRLF" (autocrlf) — `loop_prompt.cjs` entfernt `\r` vor dem Parsen, Anker-/Fence-Erkennung ist davon unabhängig; nur zur Kenntnis.
5. Prosa-Inkonsistenz außerhalb der Blöcke: die Einleitung sagt „MTF-Zwei-Schritt-Block", die Fence „Vier-Schritt-Block" — Kandidat für K1–K30, nicht für P1.
6. Prompt-Größe unverändert ~43 KB pro Fire (Abschnitt 5 Risiko 4 bestätigt: hinzu kamen nur 3 Kopfzeilen + 1 Statuszeile, ~600 B).

**Geänderte Dateien:** `scripts/loop_prompt.cjs`, `tests/trading_scripts.test.js`, `memory/feedback_live_trading.md` (6 Ankerzeilen + Checkliste Schritt 1–3), dieser Bericht, `MEMORY.md`-Indexzeile. `vollcheck.cjs` / `position_tick.cjs` / `gate_check.cjs` unangetastet (P2).

---

## Opus-Gegencheck P1, 14.09.2026 — FREIGEGEBEN MIT AUFLAGEN (kein Blocker)

**Auftrag:** Fables P1-Umsetzung unabhängig prüfen (Autor ≠ Prüfer, [[feedback_modellwahl_trading]]). Kein Commit, kein Push, `trades.db` unberührt. Geprüft wurde am Code und an der Datei, nicht am Bericht: `scripts/loop_prompt.cjs` vollständig gelesen, die sechs Anker in `feedback_live_trading.md` selbst aufgesucht, neun Kürzungs-Simulationen gegen Sandbox-Kopien der echten Regeldatei gefahren, der 43-KB-Echtlauf gegen die Quell-Fences diffed.

**Urteil: FREIGEGEBEN MIT AUFLAGEN. Kein Blocker. P2 kann beauftragt werden.** Ein Fix habe ich selbst gemacht (F1, unten), vier Nachbesserungen bleiben offen, davon eine (N1) inhaltlich schwer.

### 1. Anker-Robustheit — der wichtigste Punkt, und er hält

Neun Simulationen gegen Sandbox-Kopien (die echte Datei wurde nicht angefasst), jeweils `node scripts/loop_prompt.cjs --testtag fiktiv --terminal-zeit 19:00 --regelwerk-datei <Kopie>`:

| # | Simulierte Kürzung | Ergebnis | Bewertung |
|---|---|---|---|
| A | Prosa-Satz **unmittelbar vor** `MTF-START` und **unmittelbar nach** `FORMAT-TWEET-ENDE` gelöscht | Exit 0, 43.396 B, MTF-Block **byte-identisch** | genau das Ziel von P1: Anker überlebt Prosaänderung |
| B | nur die Ankerzeile `MTF-START` gelöscht ("sieht wie toter Text aus") | Exit 1, stdout 0 B, Anker benannt + Z-Nummer des Gegenstücks | laut, richtige Fail-Richtung |
| C | beide `PFLICHTZEILEN`-Anker gelöscht | Exit 1, "UND … FEHLEN", andere zwei als `ok` ausgewiesen | laut |
| D | alle sechs Ankerzeilen gelöscht (Kürzung räumt "unsichtbare" Zeilen weg) | Exit 1, "3 von 3 fehlerhaft" in EINER Meldung | laut |
| F | schließende Fence-Zeile des großen Blocks gelöscht | Exit 1, "erwartet genau EINE Code-Fence … gefunden 1" | laut |
| H | Anker eingerückt | Exit 0 | `trim()`-Vergleich hält |
| I | Block samt Ankern dupliziert | Exit 1, "mehrfach vorhanden (2x, 2x) — nicht eindeutig" | laut |
| **E** | **großer Block auf 2 Zeilen ausgeweidet, beide Selbsttest-Fragmente bleiben stehen** | **Exit 0**, Prompt 18.439 statt 43.496 B, alle Pflichtzeilen weg | **stilles Fehlverhalten — der eine Pfad, den es gab** |
| **G** | **Item (9) Chop-Check (1 Zeile) aus dem Block gekürzt** | **Exit 0**, 43.321 B, "Chop-Check" 0× im Prompt | **still** |

**Befund:** Struktur-Defekte (Anker, Fence) sind ausnahmslos laut und benannt — das ist sauber gebaut. Der stille Pfad lag nicht bei den Ankern, sondern beim **Selbsttest**: vier hartkodierte Fragmente für 330 Fence-Zeilen. In Fall E stand in Zeile 2 des Prompts zwar korrekt "PFLICHTZEILEN 2 Zeilen" — ein Mensch hätte es sehen *können*. Verlassen kann man sich darauf nicht; genau diese Klasse ("steht da, wird nicht gelesen") ist die Begründung des ganzen Projekts.

**F1 (selbst gefixt, `scripts/loop_prompt.cjs` + `tests/trading_scripts.test.js`):** Die `selbsttest`-Fragmente in `CRON_BAUSTEINE` von 4 auf 9 erweitert, nach einer ausgeschriebenen Auswahlregel im Code: **gepinnt wird, was kein Skript von sich aus druckt** (Stufe 3 der Mechanismus-Leiter aus Abschnitt 2.1) — MTF zusätzlich `pane_focus` (der zweite historisch ausgefallene Schritt, QQQ-Durchlauf), FORMAT-TWEET zusätzlich `Format: Fließtext` (bisher war nur die Tweet-Hälfte des Bausteins gepinnt), PFLICHTZEILEN zusätzlich `(9) "Chop-Check`, `(12) Entry-AVWAP-Anker setzen`, `(13) Solo-Mandat`. Nachgemessen: Fall E → jetzt Exit 1 mit 3 Befunden, Fall G → jetzt Exit 1 mit 1 Befund, **Echtlauf gegen die echte Datei byte-identisch zu vorher (43.496 B)**. Bewusst NICHT gepinnt: `9b-Divergenz` und `(11c) Level-Ping-Pong` — das sind die beiden P2-Kandidaten, sie sollen nach P2/P3 in die Skripte umziehen dürfen, ohne den Loop-Start zu blockieren; im Echtlauf-Test (a) sind sie ohnehin abgesichert.

**Erkennungsschwelle danach, ehrlich:** Ein Fragment ist eine Zeichenkette, kein Umfang. Wer *einen* Satz aus Item (6c) streicht, bleibt weiter unentdeckt (Exit 0) — das ist unvermeidbar, weil das Skript eine gewollte Kürzung (P3/K1–K30) nicht von einer versehentlichen unterscheiden kann. Was F1 schließt, ist der Fall "ein ganzes Pflicht-Item verschwindet". Siehe N2.

### 2. Der Doppeltext-Fund kippt anders als vermutet — und ist kein P1-Blocker

Die Vermutung im Prüfauftrag war: der hartkodierte Tweet-String in `loop_prompt.cjs` trage eine ältere, durch die heutige W1-Korrektur überholte Fassung ein. **Das ist nicht der Fall — es ist umgekehrt.** Beide Texte selbst nachgelesen:

- **Injizierte Fence** (`feedback_live_trading.md` Z87–108, Stand 28.08.2026): "Ist die echte Minute … % 10 == 0 → Pflicht-Fetch. Zusätzlich `x_last_fetch.json` lesen … Liegt der letzte Fetch VOR dem Beginn des letzten fälligen Raster-Slots → verpasst → JETZT nachholen." Also: **Raster allein macht fällig, Delta nur Verpasst-Erkennung.**
- **Hartkodierter String** (`loop_prompt.cjs`, unverändert seit 09.09.2026/TODO 4): "`--tweet-fetch ja` NUR, wenn die **Doppelbedingung (Minute % 10 == 0 UND Delta >= 10)** laut Skript FAELLIG JA ergibt."
- **Das ausführende Skript** `x_fetch_stamp.cjs --check`: `const faellig = rasterSlot && deltaOk;` — Doppelbedingung. `vollcheck.cjs` liest genau diese Zeile und kippt die Format-Zeile bei Widerspruch: ein Fetch ohne erfüllte Doppelbedingung wird als "**ÜBER-POLLING ✗**" protokolliert.
- **`feedback_vollcheck_format.md` Z42/Z71/Z89:** "Fälligkeit = DOPPELBEDINGUNG, **Stand 03.09.2026**" — und markiert die reine Kerzenraster-Fassung ausdrücklich als "insoweit Historie".

**Damit ist der hartkodierte String die NEUESTE Fassung (03.09.), die injizierte Fence die ÄLTERE (28.08.).** Die heutige W1-Korrektur hat die Kerzenraster-Fassung in `feedback_live_trading.md` geschärft ("Fällig ist der Fetch AUSSCHLIESSLICH bei echter Minute % 10 == 0; das Delta entscheidet NICHT mehr über die Fälligkeit") — **ohne die Punkt-17-Änderung vom 03.09. zu berücksichtigen, die die Delta-Prüfung als zweite UND-Bedingung wieder eingeführt hatte.** Die beiden Regeldateien widersprechen sich also seit dem 03.09., und die W1-Korrektur hat den Widerspruch heute unbeabsichtigt vertieft statt geschlossen.

**Warum trotzdem kein P1-Blocker:** Der Widerspruch ist älter als P1, keiner der beiden Texte wurde heute im Zuge von P1 angefasst, und vor P1 stand dieselbe Fence per Handkopie an derselben Stelle im Prompt. P1 macht den Widerspruch weder größer noch kleiner — es macht ihn dauerhaft. **Und ein Blindfix wäre falsch:** Wer jetzt den Skript-String auf "Raster allein" umschreibt, produziert einen Prompt, der eine Handlung verlangt, die `vollcheck.cjs` im selben Voll-Check als Über-Polling ✗ brandmarkt. Das ist eine Regelentscheidung (welche Fassung gilt: 28.08. oder 03.09.?), keine Textangleichung — deshalb **benannt, nicht selbst gefixt** (N1).

**Praktische Reichweite:** Die beiden Fassungen unterscheiden sich nur, wenn zwischen zwei Raster-Slots ein anlassbezogener Fetch (Kalender-Release ±15 Min) lag — dann sagt die Fence "Pflicht-Fetch", Skript und Prompt-String sagen "nicht fällig". Selten, aber genau an einem Nachrichten-Moment.

### 3. Reihenfolge, Vollständigkeit, Zahlen — nachgemessen

`node scripts/loop_prompt.cjs --testtag fiktiv --terminal-zeit 19:00` gegen die echte Datei: **Exit 0, stderr 0 B, 43.496 B / 418 Zeilen** — Fables Zahlen stimmen auf das Byte. Die drei injizierten Blöcke einzeln gegen die Quell-Fences diffed: **alle drei byte-identisch** (11 / 20 / 299 Zeilen, wie angegeben). Im Prompt: 0 Ankerzeilen, 0 Fence-Zeilen, genau 3 Kopfzeilen. Reihenfolge bestätigt: MTF Z6–17 → Format/Tweet Z19–39 → Template A (`--state … --testtag fiktiv`) Z43 → Template B → Pflichtzeilen Z110–409 → Terminalbedingung Z411 → LOOP-STOPP → Quick-Tick Z418. Die Anker liegen tatsächlich außerhalb der Fences, je auf eigener Zeile (per `cat -A` geprüft).

Der Diff der Regeldatei gegen `efcac0e` zeigt Inhaltsänderungen *innerhalb* der großen Fence (Item (8) W4/N5, Vollexit-Schatten, `--cluster-level tief-hoch`, Kippsignal-Definition) — die stammen sämtlich aus den früheren 14.09.-Paketen und tragen ihre Datumsmarker im Text; P1 selbst hat nur die sechs Ankerzeilen und die Checkliste Schritt 1–3 hinzugefügt. Die Einleitungsprosa der drei Fences ist unangetastet.

### 4. Testabdeckung

Die 7 Defektfälle in Test (b) sind **unterscheidbar und sinnvoll** (Start fehlt / Ende fehlt / beide + zweiter Baustein gleichzeitig / Doppel-Anker / keine Fence / leere Fence / Datei nicht gefunden) — keine Wiederholung, jeder prüft eine eigene Codeverzweigung, jeder prüft zusätzlich `stdout === ''`. Test (a) fährt zusätzlich einen **Echtlauf gegen die echte Regeldatei** und pinnt dort `(13) Solo-Mandat`, `9b-Divergenz-Check`, `(11c) "Level-Ping-Pong` und den `last_gate_check.txt … Exit-Code`-Aufrufweg — das ist die stärkste Einzelprüfung des Pakets, weil sie echte Regelinhalte gegen echte Kürzungen sichert (aber nur zur Testzeit, nicht beim Loop-Start; deshalb F1). Ich habe die Fixture um die neuen Fragmente ergänzt, ohne die Zeilenzahlen der Sandbox-Fences zu verändern, und die Befund-Erwartungen in (c) nachgezogen (1→2 bzw. 2→6 Befunde, mit je eigener Assertion auf die neuen Pins).

**`node --test tests/trading_scripts.test.js`: 77/77 PASS, 0 fail** (nach F1 erneut gelaufen, identische Zahl wie Fables Stand).

### 5. `--regelwerk-datei` und Pfad-Fallback

`findeQuelle()` ist für beide Quellen dieselbe Funktion — das ist die richtige Konsolidierung, der alte Sonderweg für `feedback_vollcheck_format.md` ist restlos ersetzt (Exit-1-Meldung inklusive: nennt beide Kandidatenpfade und den passenden Parameter). Nachgesehen: **im Repo existiert kein `memory/`-Verzeichnis**, beide Dateien werden also heute zwingend aus `~/.claude/projects/…/memory` gelesen — keine Split-Quelle möglich. Hinweis für später (N4): Legt jemand ein `memory/` im Repo an und kopiert nur EINE der beiden Dateien dorthin, lesen Templates und Cron-Bausteine aus verschiedenen Verzeichnissen, ohne dass das auffällt. Die Kopfzeile des Blocks nennt nur die Basenames.

### 6. "Kopier-Auflage erledigt"? — fast, aber Fables Formulierung ist zu absolut

Richtig ist: **die Fehlerklasse "veraltete Fassung kopiert" (W4, N5) ist geschlossen** — es gibt keinen Kopiervorgang mehr, der veralten könnte. Nicht geschlossen ist die Klasse **"der Prompt wird gar nicht erst richtig erzeugt"**: `loop_prompt.cjs` aufzurufen ist weiterhin ein Handgriff (Checkliste Schritt 1, Stufe 3 — genau die Stufe, die am 27.08./02.09./09.09. dreimal ausgefallen ist). Wird das Skript nicht aufgerufen, fehlt jetzt **alles** im Prompt statt nur der Templates; der Hebel ist größer geworden, in beide Richtungen. Dazu zwei Restfälle: `--nur-templates` druckt die Cron-Bausteine bewusst nicht (reiner Quellen-Dump, korrekt so), und eine Handbearbeitung *nach* dem Generieren fängt nur der Hand-Selbsttest (Schritt 3). **Auflage:** Der Satz in Abschnitt 7 ("solange P1 nicht gebaut ist, bleibt das ein Handgriff") kann gestrichen werden; an seine Stelle gehört: "Der Kopierweg entfällt. Der **Aufruf** von `loop_prompt.cjs` bleibt der eine verbleibende Handgriff — Checkliste Schritt 1, und er ist jetzt der einzige Punkt, an dem der ganze Prompt hängt."

### 7. "Zwei-Schritt" vs. "Vier-Schritt" (Fables Punkt 4): präexistent, kein P1-Bezug

Nachgesehen im Memory-Git: beide Formulierungen (Einleitungsprosa "MTF-Zwei-Schritt-Block", Fence "fester Vier-Schritt-Block") stammen aus `379f452` (Initial-Backup) und stehen seither unverändert nebeneinander. Der Widerspruch ist durch P1 **sichtbar geworden, nicht entstanden** — der Fence-Text gilt (vier Schritte: 15 → 60 → 5 → QQQ), die Prosa ist die Zwei-Schritt-Ära vor der QQQ-Erweiterung 16.07. **Info, K-Kandidat, kein Blocker.** Die Prosa wandert nicht in den Prompt.

### Offene Punkte (Auflagen)

**N1 · Tweet-Fälligkeit: Kerzenraster allein (28.08., `feedback_live_trading.md`) oder Doppelbedingung (03.09., `feedback_vollcheck_format.md` + `x_fetch_stamp.cjs` + Prompt-String)? — SCHWERE HOCH, Levi-Entscheidung.** Solange das offen ist, steht im Cron-Prompt bei jedem Fire beides. Reparaturweg danach: EINE der beiden Fassungen anpassen, die andere mit Überholungsmarker versehen, und das ausführende Skript in dieselbe Richtung ziehen. Klasse W1 — gehört in [[project_pruefung_feedback_live_trading_2026-09-14]] nachgetragen, weil die dortige W1-Korrektur den 03.09.-Stand nicht kannte.

**N2 · Kürzung innerhalb der Fence bleibt teilweise still.** F1 fängt den Wegfall ganzer Pflicht-Items, nicht den Wegfall einzelner Sätze. Billigste echte Verschärfung wäre P4 (`--pruefe-prompt`) plus ein Zeilenzahl-Vergleich: die Statuszeile nennt "PFLICHTZEILEN 299 Zeilen" — wer sie gegen den Vortag prüft, sieht jede Kürzung. Solange das niemand tut, ist die Zeile Dekoration.

**N3 · Checkliste Schritt 3 führt die Selbsttest-Zeichenketten als Handliste** (vier Stück) und ist damit die dritte Stelle, an der dieselbe Liste gepflegt wird (Code, Checkliste, Berichte). Nach F1 sind es neun im Code. Vorschlag: Checkliste auf "die Kernfragmente aus `CRON_BAUSTEINE`, mindestens `node scripts/vollcheck.cjs` / `--state` / `--testtag`" umstellen und die Aufzählung dem Skript überlassen — dieselbe Einzige-Quelle-Logik wie bei den Fences. **Nicht selbst gemacht** (Regeltext, außerhalb des Prüfauftrags).

**N4 · Pfad-Split (siehe 5), niedrige Schwere:** wenn beide Quellen aus verschiedenen Verzeichnissen kommen können, sollte die Kopfzeile den vollen Pfad statt des Basename nennen.

### Geänderte Dateien (Opus, kein Commit, kein Push)

`scripts/loop_prompt.cjs` (F1: `CRON_BAUSTEINE`-Fragmente 4 → 9 + Auswahlregel als Kommentar, Kopfkommentar nachgezogen), `tests/trading_scripts.test.js` (Fixture trägt die neuen Fragmente bei unveränderten Zeilenzahlen; (c) prüft sie einzeln), dieser Bericht, `MEMORY.md`-Indexzeile. Regelwerk, `vollcheck.cjs`, `x_fetch_stamp.cjs`, `trades.db`: **unberührt.**

### Antwort auf die Entscheidungsfrage

**P2 kann jetzt beauftragt werden.** P1 ist funktionsfähig, testgedeckt und fail-safe in die richtige Richtung; N1 ist ein älterer Regelwiderspruch, der P2 (Level-Ping-Pong 11c + 9b-Divergenz in die Skripte) weder berührt noch blockiert — im Gegenteil: P2 hebt genau die zwei Zeilen von Stufe 3 auf Stufe 1, die ich in F1 bewusst nicht ans Anker-Gate gebunden habe.

## ENTSCHIEDEN 14.09.2026 (Levi): P2 gestaffelt — zuerst Kern+F-A/F-C, F-B separat

Levi: "So wie du es vorschlägst bitte umsetzen lassen" (Opus-Vorschlag aus dem N1-Textkorrektur-Gegencheck). Ablauf: (1) Fable setzt in EINEM Auftrag um: Level-Ping-Pong (11c) + 9b-Divergenz-Check in vollcheck.cjs/position_tick.cjs nachbauen (bisher 0 Treffer im Code) UND F-A+F-C gemeinsam in x_fetch_stamp.cjs (Nachhol-Fetch + Anlass-Trigger faelschlich als UEBER-POLLING markiert, dieselbe Fehlerklasse, ein Fix). (2) Opus-Gegencheck dieses Pakets. (3) NUR bei FREIGEGEBEN/FREIGEGEBEN MIT AUFLAGEN ohne Blocker: F-B (last_fetch-Semantik, zweites Feld fuer Abrufzeit, beruehrt feedback_session_update.md Schritt 2) als eigener Fable-Auftrag mit eigenem Opus-Gegencheck.

---

## P2 Kernteil + F-A/F-C umgesetzt 14.09.2026 (Fable) — Opus-Gegencheck ausstehend, F-B danach separat

**Umfang:** Levis Entscheidung (Abschnitt „ENTSCHIEDEN … P2 gestaffelt"): Level-Ping-Pong (11c/8a5) + 9b-Divergenz-Check in `vollcheck.cjs` nachgebaut, F-A (Nachhol-Fetch) + F-C (Anlass-Trigger) gemeinsam in `x_fetch_stamp.cjs` gefixt. **Kein Commit, kein Push** (Levi committet selbst). F-B (last_fetch-Semantik) NICHT angefasst. Keine Änderung an Q1–Q4, Hard-Gates, Ampel-Logik, P1-Cron-Ankern. Alle Zahlen unten am 14.09.2026 gemessen ([[feedback_memory_pflege]]).

**Vorab selbst verifiziert:** `grep -i "ping.pong|divergenz|MACD"` über `vollcheck.cjs` und `position_tick.cjs` — 0 Treffer für Ping-Pong/Divergenz (MACD-Treffer sind ausschließlich die bestehenden Reversal-Kriterien K1/K4 und die MTF-Zeilen); im ganzen `scripts/`-Verzeichnis nur `loop_prompt.cjs` Z63 (Kommentar „bewusst nicht gepinnt") und ein Backfill-Skript. Die Null-Treffer-Behauptung aus Abschnitt 2.2 (J/K) stimmt.

### Teil A — Level-Ping-Pong 8a5 (`scripts/vollcheck.cjs`, `pingPongFortschreiben()` + Einbau vor der Zählstände-Zeile)

- **State-Block** `letzter.level_ping_pong` (Muster `chop_streak`; im Re-Run wie alle Zähler ab `vorletzter`): `{ seite, seite_seit_ts, letzte_ref_5m, serie: { zentrum, band_pkt, seit_ts, zyklen, qqq_bestaetigt, adx_beginn, ausserhalb_kerzen, zyklen_liste[{nr, ts, von, nach, kerzen, qqq, adx, close, ema50}] } | null, letzter_reset_grund }`.
- **Eingaben — alle bereits vorhanden, KEIN neuer Parameter:** `--close-5m`/`--ema50-5m` (Seite = Schluss vs. EMA50), `--atr` (Band 0,5×, Reset 1,5×), `--ref-5m` (Kerzenschluss = Open + 5 Min → „seit HH:MM", Kerzenabstand), QQQ-15min-Bein (`--qqq-close-15m`/`--qqq-ema50-15m`), `--adx`. Fehlt eines per `--grund-*` → „nicht berechenbar (…) — Block unverändert" (kein Hard-Exit, kein LUECKE-Baustein — dem GEMESSEN-KEIN-GATE-Charakter entsprechend). Ohne `--state`: „ohne --state nicht fortschreibbar".
- **Zyklus** = Cross (bestätigter 5min-Schluss wechselt die EMA50-Seite) ≤ 12 Kerzen nach dem Schluss, der die bisherige Seite etabliert hat, während EMA50 im Band. **Band** = EMA50 ±0,5×ATR beim ersten Cross der Serie, ab da fixiert (Zentrum UND Breite). **Reset:** (a) Tageswechsel (State wird ohnehin verworfen → Block startet mit Reset-Grund „(a) Tageswechsel"), (b) Schluss > 1,5×ATR jenseits der Bandkante für ≥ 6 Kerzen, (c) EMA50 selbst außerhalb des Bands. Nach Reset eröffnet der nächste Cross eine neue Serie (Zähler bei 1, nicht fortgeführt).
- **Ausgabe:** ab dem 3. Zyklus die Pflichtzeile **exakt** im 8a5-Format: `**Level-Ping-Pong:** 3. Zyklus am selben EMA50-Band (Zentrum 29530, ±15,0 Pkt), seit 16:40, QQQ-Bestätigungen: 1/3, ADX(5min) bei Zyklusbeginn: 24,5`. Davor eine anders benannte Vorstufen-Zeile `**8a5-Zyklen (maschinell, Vorstufe Level-Ping-Pong — GEMESSEN, KEIN GATE):** …` (damit „Level-Ping-Pong:" im Protokoll ausschließlich die Pflichtzeile ist). Ableitungen stehen in der Zustandsspeicher-Zeile (`8a5-Ping-Pong: Serie eroeffnet … Zyklus 1 abgeschlossen …`).
- **Konsequenz KEINE:** kein Baustein in der Format-Zeile, kein Lückengrund, kein Exit-Einfluss, `--zyklus-8a5` (manueller Zähler) unverändert — weicht er vom maschinellen Stand ab, nur ein Hinweis in der Zustandsspeicher-Zeile. JSON: `out.levelPingPong`.

### Teil B — 9b-Divergenz-Check (`scripts/vollcheck.cjs`, `divergenzFortschreiben()`)

- **Welche „9b" gemeint ist:** `feedback_chartanalyse.md` 9b = Divergenz-Regel (23.06.2026, „Kurs hält Hochs, Histogram fällt") — daraus stammt die Pflichtzeile `9b-Divergenz-Check: MACD-H(5min) …` in `feedback_live_trading.md` Z375 (27.08.2026 Paket 6/6a, Anlass Short #T1 vom 24.08.). Das andere „9b" (`feedback_live_trading.md` Punkt 11, Reversal-Management, Umbenennungs-Hinweis Z1010) hat mit der Zeile nichts zu tun. Verifiziert am Zeilenformat („MACD-H(5min) … divergent … gegen Richtung") und am Begründungstext („der MACD war bullisch divergent").
- **Kein neuer Parameter:** `--macdh-5m` und `--close-5m` (beide A3-Pflicht) plus `--richtung`. Die Regel steht in `vollcheck.cjs` (Auftragsvorgabe), nicht in `gate_check.cjs` wie im Abschnitt-4-Vorschlag — dort wäre sie eine reine Behauptung per A3-Feld gewesen; hier wird sie aus der Historie gerechnet.
- **State-Block** `letzter.divergenz_9b`: `{ kurs_hoch, kurs_hoch_ts, macdh_bei_hoch, kurs_tief, kurs_tief_ts, macdh_bei_tief, baerisch_seit_ts, bullisch_seit_ts, letzte_ref_5m }` — Tages-Extrema der 5min-SCHLÜSSE und MACD-H **an** diesem Extremum (Lehrbuch: Momentum an aufeinanderfolgenden Hochs vergleichen, kein laufendes MACD-H-Maximum).
- **Divergenz:** neues Schluss-Hoch mit MACD-H < MACD-H am bisherigen Hoch = bärisch (Long-Kontext: WARNUNG); spiegelbildlich bullisch am Tief. „seit HH:MM" = Schluss des Erstbefunds. Aufgelöst, wenn ein neues Extremum mit bestätigtem Momentum kommt oder MACD-H das Niveau am Referenz-Extremum wieder überschreitet.
- **Ausgabe** exakt drei Bausteine, Belegzahlen dahinter in eckiger Klammer: `**9b-Divergenz-Check:** MACD-H(5min) Divergenz erkannt seit 16:50 → divergent → WARNUNG: gegen Richtung [bärisch: Schluss-Hoch 29560 (16:50) bei MACD-H 0,40; MACD-H jetzt 0,40, Tages-Schluss-Hoch/-Tief 29560/29520]` bzw. `… keine Divergenz → positiv → kein Widerspruch [MACD-H jetzt 0,80, …]`; `--richtung short` bei bärischer Divergenz → `kein Widerspruch (Divergenz IN Richtung)`; `--richtung none` mit Divergenz → `WARNUNG: gegen Richtung (keine Setup-Richtung — bärisch, gegen LONG)`. Ohne `--state`: `Divergenz nicht prüfbar (ohne --state keine Extremum-Historie) → positiv → nicht prüfbar`. Kein Baustein, kein Gate. JSON: `out.divergenz9b`.

### Teil C — F-A + F-C (`scripts/x_fetch_stamp.cjs` Z150ff, Downstream `vollcheck.cjs` Tweet-Block)

- **F-A Nachholen** (vom Skript selbst gerechnet): `letzterSlotMs` = Beginn der letzten bereits VERGANGENEN %10-Minute (in der Raster-Minute: der Slot davor — der laufende Slot ist nicht „verpasst", sondern über Raster+Delta fällig); `nachholen = ts < letzterSlotMs`. **F-C Anlass** (nur von außen): `--anlass "<Release HH:MM>"`, nacktes `--anlass` = Exit 1. `faellig = (rasterSlot && deltaOk) || nachholen || anlass`.
- **Neue Ausgabezeilen** (die bisherigen 4 unverändert, die zitierte `Doppelbedingung …`-Zeile bleibt das (a)∧(b)-Urteil): `Nachholen (Nachhol-Regel 28.08.2026 — letzter Fetch vor Beginn des letzten Raster-Slots 16:30): JA (last_fetch 14.9.2026, 16:28:00 < Slot-Beginn 16:30)` / `NEIN` · `Anlass (Kalender-Release +/-15 Min, nur von aussen per --anlass): JA "CPI 14:30"` / `NEIN (kein --anlass)` · **`Faelligkeit gesamt: FAELLIG JA — Grund: Raster+Delta | Nachholen (Slot 16:30 verpasst) | Anlass "…"`** bzw. `NICHT FAELLIG (Raster+Delta NEIN, Nachholen NEIN, Anlass NEIN)` — nennt den tatsächlich greifenden Grund (alle zutreffenden, durch `|`). Zusätzlich ein Hinweis bei Nachholen JA zum F-B-Vorbehalt (s.u.).
- **Downstream `vollcheck.cjs`:** neuer optionaler Parameter `--tweet-anlass "<Release HH:MM>"` (nie nackt → Hard-Exit 1), wird als `--anlass` durchgereicht; liest `Faelligkeit gesamt:` (Fallback Doppelbedingung). Neue Zweige: Nachholen+ja → `**Tweet-Check:** ÜBERFÄLLIG ✗ — Slot 16:30 verpasst, jetzt nachgeholt ✓ (Nachhol-Regel 28.08.2026 / F-A: …; kein Raster-Slot, Minute 35 % 10 = 5 — regelkonform, KEIN Über-Polling)`, Baustein `Tweet-Check ✓ (Slot 16:30 verpasst, nachgeholt)` (kippt NICHT — Kopfkommentar: „überfällig und nicht nachgeholt" kippt); Nachholen+nein → `ÜBERFÄLLIG ✗ — Slot … verpasst, NICHT nachgeholt … → jetzt nachholen` (kippt); Anlass+ja → `Anlass-Fetch ✓ — "CPI 14:30" (…)`, Baustein `✓ (Anlass)`; Anlass+nein → `ÜBERFÄLLIG ✗ — Anlass "…" gemeldet …, aber --tweet-fetch nein`. ÜBER-POLLING ✗ nur noch, wenn alle drei Gründe NEIN sind (die Zeile nennt das: „Nachholen NEIN, kein Anlass per --tweet-anlass"). Das Zitat in eckiger Klammer trägt jetzt zusätzlich die `Faelligkeit gesamt`-Zeile und ein evtl. `--anlass`. JSON `out.tweet` um `doppelOk, nachholen, nachholSlot, anlass, anlassOk, grund, format` erweitert.
- **Außerhalb der Skripte:** `loop_prompt.cjs` Z200 (hartkodierter Tweet-String, bestehender Satz um „ODER Nachholen: JA … ODER --tweet-anlass" ergänzt — kein neuer String); `feedback_vollcheck_format.md` Template A situative Fence: neue Zeile `… bekannter Kalender-Release innerhalb ±15 Min …: --tweet-anlass "<Release HH:MM>"` nach `--tweet-news` (wird per P1 injiziert). Echtlauf `loop_prompt.cjs --testtag fiktiv --terminal-zeit 19:00`: Exit 0, 44.412 B (vorher 43.496 B; +916 B = Satz + Template-Zeile).

### Tests — `node --test tests/trading_scripts.test.js`: **84/84 pass, 0 fail** (vorher 77/77)

Neuer Block „P2 Kernteil + F-A/F-C (14.09.2026)", 7 Tests, alle mit slot-alignierten Zeitankern (10-Min-Grenze vor zwei Stunden — nicht von der Wanduhr-Minute abhängig): (1) Zyklus-Erkennung über 6 Voll-Checks mit `--state` (Seite initialisiert → Serie/Zyklus 1 → 2 → kein Cross → 3), Pflichtzeile erst ab 3 und per Regex im exakten Format inkl. `seit` = Schluss der Cross-Kerze, QQQ 1/3, kein Baustein in der Format-Zeile, JSON; (2) Reset (b) über 6 Kerzen inkl. Zyklus 4 vorher, Grund im State/Vorstufen-Zeile, neue Serie danach bei 1; (3) Reset (c) ohne Cross, Reset (a) über Tageswechsel (Serie gestern → heute verworfen), Cross nach 13 Kerzen = 0 Zyklen; (4) 9b: keine Divergenz / erkannt seit HH:MM (Zeile per `assert.equal` wörtlich) / Short-Kontext IN Richtung / Auflösung / ohne `--state`; (5) F-A `x_fetch_stamp.cjs`: Nachholen JA off-raster, Raster+Delta ohne Nachholen (Delta exakt 10), NICHT FAELLIG alle NEIN, Raster+Delta UND Nachholen; (6) F-C: `--anlass` erzwingt, nackt Exit 1, ohne = unverändert; (7) `vollcheck.cjs`: Nachhol-Fetch ✓ ohne ÜBER-POLLING (Format-Baustein geprüft), Nachholen+nein ✗, JSON-Grund, Anlass-Fetch ✓, **Regression** ohne `--tweet-anlass` = ÜBER-POLLING ✗, Anlass+nein ✗, n.a.-Zeile, nacktes `--tweet-anlass` Exit 1. Bestehende Tests unverändert (kein bestehender Test pinnt die Tweet-Zeile; `protokoll_bilanz.cjs` liest `^\**Tweet-Check` — Präfix unverändert).

### Echtlauf (Scratchpad-Sandbox, Kopie der Skripte, `--testtag fiktiv`, keine echten Logs; Zeitanker 16:35–16:55 DE)

```
**Level-Ping-Pong:** 3. Zyklus am selben EMA50-Band (Zentrum 29530, ±15,0 Pkt), seit 16:40, QQQ-Bestätigungen: 1/3, ADX(5min) bei Zyklusbeginn: 24,5
**9b-Divergenz-Check:** MACD-H(5min) Divergenz erkannt seit 16:50 → divergent → WARNUNG: gegen Richtung [bärisch: Schluss-Hoch 29560 (16:50) bei MACD-H 0,40; MACD-H jetzt -0,20, Tages-Schluss-Hoch/-Tief 29560/29518]
**Tweet-Check:** ÜBERFÄLLIG ✗ — Slot 16:30 verpasst, jetzt nachgeholt ✓ (Nachhol-Regel 28.08.2026 / F-A: letzter Fetch vor 7 Min lag vor Slot-Beginn; nichts Neues; kein Raster-Slot, Minute 35 % 10 = 5 — regelkonform, KEIN Über-Polling) [x_fetch_stamp.cjs --check --jetzt 2026-09-14T14:35:20Z → "Delta: 7 Min | faellig (>= 10 Min): NEIN (naechste Faelligkeit in 3 Min)" | "Raster: Minute 35 % 10 = 5 -> Raster-Slot (Minute % 10 == 0): NEIN (Zeitanker 16:35 DE, --jetzt 2026-09-14T14:35:20Z)" | "Faelligkeit gesamt: FAELLIG JA — Grund: Nachholen (Slot 16:30 verpasst)" | Exit 0] — Wasserstand danach per x_fetch_stamp.cjs --set setzen
**Format:** … | Tweet-Check ✓ (Slot 16:30 verpasst, nachgeholt) | …
```
`x_fetch_stamp.cjs --check --jetzt 2026-09-14T14:35:20Z --anlass "CPI 14:30"` (last_fetch 16:32, Delta 3): `Doppelbedingung …: NICHT FAELLIG (kein Raster-Slot + Delta 3 < 10)` / `Nachholen (…16:30): NEIN` / `Anlass (…): JA "CPI 14:30"` / **`Faelligkeit gesamt: FAELLIG JA — Grund: Anlass "CPI 14:30"`**. Derselbe Aufruf ohne `--anlass`: `Faelligkeit gesamt: NICHT FAELLIG (Raster+Delta NEIN, Nachholen NEIN, Anlass NEIN)` + Punkt-17-Hinweis. Zustandsspeicher-Zeile im Lauf: `8a5-Ping-Pong: Serie eroeffnet: erster Cross LONG→SHORT um 16:40, Band fixiert 29530 ±15,0 Pkt; Zyklus 1 abgeschlossen (LONG→SHORT, 1 Kerze seit Etablierung, QQQ nicht bestaetigt) | 9b-Divergenz: keine Divergenz (kein neues Schluss-Extremum) | Hinweis: manueller Zaehler --zyklus-8a5 0 weicht vom maschinellen 8a5-Zyklus 1 ab (beide angezeigt, keine Konsequenz)`.

### Offene Punkte für den Opus-Gegencheck (nichts davon selbst entschieden)

1. **F-A × F-B — der wichtigste Punkt, bewusst spec-konform gebaut und NICHT wegdiskutiert:** Die Nachhol-Regel setzt voraus, dass `last_fetch` die Abrufzeit ist. Bis F-B ist es der Zeitstempel des jüngsten TWEETS (Kopfkommentar `--set`); ein Abruf ohne neuen Tweet bewegt ihn nicht. Folge an ruhigen Tagen (10.09.: Wasserstand stand ab 16:44): jeder Off-Raster-Voll-Check meldet `Nachholen: JA` → ohne Fetch `ÜBERFÄLLIG ✗ — Slot verpasst, NICHT nachgeholt` (kippt), obwohl der Slot bedient wurde. Das ist die Regel + F-B, nicht der Code — aber es ist real. Empfehlung: **F-B unmittelbar nach diesem Gegencheck und VOR dem nächsten Testtag** (Abrufzeit als zweites Feld; `nachholen` dann gegen die Abrufzeit rechnen, `delta` vorerst unverändert). Alternative, nicht gebaut: bis F-B `nachholen` nur als *Erlaubnis* (kein ÜBER-POLLING) werten, nicht als *Pflicht* — das wäre eine Regelabweichung und braucht Levis Wort. Der Skript-Hinweis bei Nachholen JA benennt den Vorbehalt.
2. **8a5-Lesarten** (im Code-Kommentar ausgeschrieben): (i) Kerzenabstand ab dem Schluss, der die Seite etabliert hat (nicht „vorheriger Schluss", was unter 5-Min-Kadenz fast immer 1 wäre und die 12-Kerzen-Regel entwerten würde); ein zu später Cross eröffnet die Serie, zählt aber keinen Zyklus; (ii) Reset (b) misst ab Bandkante mit aktuellem ATR; (iii) QQQ-Bestätigung = QQQ-15min-Bein auf der neuen NAS100-Seite beim Cross; (iv) „ADX bei Zyklusbeginn" = Serienbeginn (je Zyklus zusätzlich im State). Bitte bestätigen oder korrigieren — Anzeige-Only, jede Änderung ist eine Zeile.
3. **9b-Lesart:** Referenz = Tages-Extrema der Schlüsse seit dem ersten Voll-Check mit MACD-H an diesem Extremum; Auflösung bei bestätigtem Momentum. Kein Zeitverfall (Divergenz bleibt bis Auflösung/Tagesende). Die Zeile erscheint an JEDEM Voll-Check (Obermenge von „vor jeder Order") — die Trigger-Moment-Pflicht selbst bleibt Regeltext.
4. **Zwei 8a5-Zähler nebeneinander:** `--zyklus-8a5` (manuell, A3, State-fortgeschrieben) bleibt unverändert neben dem maschinellen Block; Abweichung wird nur als Hinweis ausgewiesen. Ob der manuelle Zähler später durch den maschinellen ersetzt wird (Template A + A3-Spec), ist eine eigene Entscheidung (P3-Kaliber).
5. **Reset (b) bei VC-Lücke** zählt die Referenz-Kerzendifferenz (neue Kerzen), nicht die Beobachtungen — bei Lücken wird also angenommen, die unsichtbaren Zwischenkerzen lagen ebenfalls außerhalb.
6. **`x_fetch_stamp.cjs` ohne `--jetzt`** rechnet Nachholen gegen die Systemzeit; im Loop kommt `--jetzt` von `vollcheck.cjs` (eine Zeitquelle) — bei einem älteren `--jetzt` (Datenerhebung über die Rastermiute hinaus, 10.09.-Muster) verschiebt sich auch der „letzte Slot" entsprechend; keine neue Fehlerklasse, nur zur Kenntnis.
7. Prompt-Größe +916 B (Tweet-Satz + Template-A-Zeile) — P3-Kandidat wie zuvor.

**Geänderte Dateien:** `scripts/vollcheck.cjs` (Helfer `pingPongFortschreiben`/`divergenzFortschreiben`, Einbau vor Zählstände, Tweet-Block, `--tweet-anlass`-Guard, State-Felder, JSON, Kopfkommentar), `scripts/x_fetch_stamp.cjs` (Nachholen/Anlass/Gesamt-Zeile, `--anlass`, Kopfkommentar), `scripts/loop_prompt.cjs` (Z200 Tweet-Satz), `tests/trading_scripts.test.js` (+7 Tests), `memory/feedback_vollcheck_format.md` (Template A situativ `--tweet-anlass`), dieser Bericht, `MEMORY.md`-Indexzeile. Unangetastet: `gate_check.cjs`, `position_tick.cjs`, `protokoll_bilanz.cjs`, Regeltexte 8a5/9b/Punkt 9, `trades.db`.

---

## Opus-Gegencheck P2 Kernteil + F-A/F-C, 14.09.2026

**Auftrag:** Fables P2-Paket unabhängig prüfen (Autor ≠ Prüfer, [[feedback_modellwahl_trading]]). Alles unten selbst am Code und an eigenen Läufen gemessen, nicht aus Fables Bericht übernommen: `x_fetch_stamp.cjs` und die neuen `vollcheck.cjs`-Teile vollständig gelesen, eigene Sandbox (Kopie von `scripts/`) mit eigenem Voll-Check-Harness, eigene Zyklen-/Divergenz-/Fälligkeits-Sequenzen. Kein Commit, kein Push, `trades.db` unberührt.

### Gesamturteil: FREIGEGEBEN MIT AUFLAGEN — **F-B kann und MUSS jetzt beauftragt werden**

Der Code ist spec-konform, sauber gebaut und testgedeckt; die drei Teile tun, was sie sollen. **Ein Befund hat Blocker-Gewicht — aber gegen den nächsten TESTTAG, nicht gegen dieses Paket:** die F-A×F-B-Interaktion ist real, ich habe sie live reproduziert, und sie ist schlimmer als Fable sie einordnet (Abschnitt 1). Ihr Heilmittel ist exakt der nächste geplante Schritt. Eine Nachbesserung habe ich selbst gebaut (O-1: `--anlass` war ein Freibrief), eine zweite als Regeltext-Nachzug (O-2), drei bleiben offen. Tests **84/84** vor und nach meinen Fixes.

---

### 1. F-A × F-B — selbst nachgebaut. **Blocker für den nächsten Testtag (kein Blocker für die Freigabe)**

Fable hat den Punkt korrekt gemeldet, aber zu milde eingeordnet („Fehlrichtung ist ein Fetch zu viel — harmlos"). **Das stimmt nicht.** „Ein Fetch zu viel" ist in diesem Projekt kein harmloser Fehler, sondern der dokumentierte Schaden vom 02.09.2026 (61 Abrufe auf 84 Voll-Checks, drei leere `@DeItaone`-Antworten, plausible Rate-Limit-Folge) — der Anlass, aus dem Punkt 17 überhaupt gebaut wurde.

**Lauf 1 — Fälligkeitsrechner, ruhiger Nachmittag** (Wasserstand fix, weil keine neuen Tweets; reale Fetches laufen regelkonform auf jedem Raster-Slot). `x_fetch_stamp.cjs --check` über neun aufeinanderfolgende Voll-Checks:

| Zeitanker | Doppelbedingung | Nachholen | Fälligkeit gesamt |
|---|---|---|---|
| 16:50 | NICHT FAELLIG (Delta 6 < 10) | NEIN | NICHT FAELLIG |
| 16:55 | NICHT FAELLIG (kein Raster-Slot) | **JA (Slot 16:50)** | **FAELLIG JA — Nachholen** |
| 17:00 | FAELLIG JA | **JA (Slot 16:50)** | FAELLIG JA — Raster+Delta \| Nachholen |
| 17:05 | NICHT FAELLIG | **JA (Slot 17:00)** | **FAELLIG JA — Nachholen** |
| 17:10 … 17:30 | wechselnd | **durchgehend JA** | **jeder VC fällig** |

Ab dem ersten Voll-Check, der mehr als einen Slot hinter dem jüngsten Tweet liegt, meldet das Skript **an JEDEM Voll-Check `Nachholen: JA`** — auch an denen, an denen der Slot nachweislich bedient wurde. Genau wie vorhergesagt.

**Lauf 2 — Downstream in `vollcheck.cjs`**, derselbe Nachmittag, Fetches korrekt nur auf den Raster-Slots:

```
VC 17:45 (kein Fetch): **Tweet-Check:** ÜBERFÄLLIG ✗ — Slot 17:40 verpasst, NICHT nachgeholt … → jetzt nachholen
   -> Format-Baustein: Tweet-Check ✗
VC 17:50 (Fetch):      Raster-Slot 17:50 fällig+durchgeführt ✓
VC 17:55 (kein Fetch): **Tweet-Check:** ÜBERFÄLLIG ✗ — Slot 17:50 verpasst, NICHT nachgeholt … → jetzt nachholen
   -> Format-Baustein: Tweet-Check ✗
```
**Jeder zweite Voll-Check trägt ein falsches ✗**, und es ist ein *kippendes* ✗. Gegenprobe am selben Voll-Check, nur mit F-B-Semantik im Wasserstand (letzter ABRUF statt jüngster Tweet), JSON verglichen:

- heute: `tweet:{delta:21, nachholen:true, faellig:true, format:"✗"}` · `luecken:[…, "Tweet-Check ✗", …]`
- mit F-B: `tweet:{delta:5, nachholen:false, faellig:false, format:"n.a."}` · `luecken:[…]` — **die Lücke verschwindet vollständig.**

Der Unterschied ist also zu 100 % F-B, nicht F-A.

**Lauf 3 — und das ist der eigentliche Schaden.** Was passiert, wenn der Loop der Anweisung „→ jetzt nachholen" folgt? Ruhiger Tag, echter Fetch lief um 18:00, VC 18:05 fetcht erneut (= 5-Minuten-Takt):

```
**Tweet-Check:** ÜBERFÄLLIG ✗ — Slot 18:00 verpasst, jetzt nachgeholt ✓ (… kein Raster-Slot, Minute 05 % 10 = 5
— regelkonform, KEIN Über-Polling)
**Format:** … | Tweet-Check ✓ (Slot 18:00 verpasst, nachgeholt) | …
```

**Der Über-Polling-Schutz aus Punkt 17 ist auf einer ruhigen Strecke damit nicht nur wirkungslos, sondern kehrt sich um:** die Maschine *fordert* den Zusatz-Fetch an jedem Off-Raster-VC und *bescheinigt* ihm anschließend Regelkonformität. Das 02.09.-Muster (5-Min-Takt) entsteht erneut — diesmal mit maschinellem Segen und ohne die ✗-Marke, die es am 10.09. wenigstens noch sichtbar gemacht hat. Größenordnung am Maßstab 10.09. (Wasserstand stand ab 16:44, Session bis 19:25): ~16 Off-Raster-Voll-Checks, also ~16 falsche ÜBERFÄLLIG-✗ bzw. ~16 überflüssige Abrufe — zum Vergleich: der ganze 10.09. hatte 11 ÜBER-POLLING-Marken.

**Einordnung:** Der Code ist **nicht** falsch. `nachholen = ts < letzterSlotMs` ist die wörtliche Umsetzung der Nachhol-Regel und wäre mit korrekter Wasserstand-Semantik richtig (eigene Gegenprobe: Abruf 17:00:20 → VC 17:05 → `Nachholen: NEIN`). Falsch ist die **Bedeutung des Feldes**, und das ist F-B. F-A ist ohne F-B nicht betriebstauglich — die beiden gehören fachlich zusammen, die Staffelung hat sie getrennt.

**Konsequenz, klar gesagt:**
- **Kein Testtag mit F-A ohne F-B.** Das ist die einzige harte Auflage dieses Gegenchecks.
- **F-B ist damit nicht nur „kann beauftragt werden", sondern der zwingend nächste Schritt** — vor jedem Testtag, vor P3, vor allem anderen.
- Fables nicht gebaute Alternative („Nachholen bis F-B nur als *Erlaubnis*, nicht als *Pflicht-Ausweis*") habe ich **bewusst nicht** eingebaut: sie ändert die Rechtsfolge der Nachhol-Regel und ist von keiner Entscheidung gedeckt (Punkt 14). Sie wäre nur nötig, wenn F-B sich verzögert — dann aber mit Levis Wort.

**Zwei Auflagen für den F-B-Auftrag, beide selbst gerechnet:**

1. **Die Rundungsfalle.** Sobald `last_fetch` die Abrufzeit ist, liegt das Delta zwischen zwei Raster-Fetches bei ziemlich genau 10 Minuten — und `deltaMin` ist `Math.round(...)`. Gemessen mit Abrufzeit 17:01:10 (leicht verspäteter Fetch im 17:00-Slot) und Zeitanker 17:10:20: `Delta: 9 Min | faellig (>= 10 Min): NEIN` → `Nachholen: NEIN` → **`NICHT FAELLIG` auf einem regulären Raster-Slot**; ein korrekter Fetch würde dort als `ÜBER-POLLING ✗` gebrandmarkt. Heute tritt das nicht auf, weil der Wasserstand fast immer älter ist. **F-B muss Bedingung (b) mit umbauen** — z.B. „letzter Abruf liegt vor Beginn des aktuellen Raster-Slots" statt „Delta ≥ 10" (slot-basiert wie F-A), oder Toleranz/`Math.floor`. Ohne das tauscht F-B eine Fehlerklasse gegen eine andere.
2. **Zwei Felder, nicht eines.** `last_fetch` (Wasserstand/Digest) bleibt für den Session-Update-Weg gebraucht ([[feedback_session_update]] Schritt 2); die Abrufzeit gehört als zweites Feld daneben. Der `--set`-Guard „nie rückwärts" darf nur für den Wasserstand gelten, nicht für die Abrufzeit.

---

### 2. Level-Ping-Pong 8a5 — korrekt, eigene Sequenzen bestätigen es

Eigener Lauf, sieben Voll-Checks mit `--state`-Persistenz, EMA50 29530, ATR 30 (Band ±15), Schlusskurse 29541/29520/29541/29545/29520/29544/29519:

| VC | Ergebnis |
|---|---|
| 1 | Seite LONG initialisiert, keine Serie · Vorstufen-Zeile, **keine** Pflichtzeile |
| 2 | Serie eröffnet, Band fixiert 29530 ±15,0 · **Zyklus 1** · keine Pflichtzeile |
| 3 | **Zyklus 2** · keine Pflichtzeile |
| 4 | kein Cross, Zähler unverändert 2 |
| 5 | **Zyklus 3** → `**Level-Ping-Pong:** 3. Zyklus am selben EMA50-Band (Zentrum 29530, ±15,0 Pkt), seit 16:55, QQQ-Bestätigungen: 1/3, ADX(5min) bei Zyklusbeginn: 24,5` |
| 6/7 | 4./5. Zyklus, `seit` und Zentrum bleiben am Serienanker |

**Zeichenweise gegen den Regeltext geprüft** ([[feedback_chartanalyse]] 8a5): Format identisch, Pflichtzeile **genau** ab dem 3. Zyklus, davor eine anders benannte Vorstufen-Zeile (gute Entscheidung — so bleibt `Level-Ping-Pong:` im Protokoll eindeutig die Pflichtzeile). Kein Baustein in der Format-Zeile, kein Gate, kein Exit-Einfluss: **Konsequenz KEINE ist eingehalten.**

**Eigene Grenzfälle:**
- **genau 12 Kerzen** seit Etablierung → Zyklus zählt; **13 Kerzen** → `Cross LONG→SHORT ohne Zyklus (13 Kerzen seit Etablierung > 12)`, Serie wird trotzdem eröffnet. Beides regelkonform („höchstens 12").
- **Reset (b) exakt an der Schwelle:** 45,0 Pkt über der Bandkante (= 1,5×ATR) löst **nicht** aus, 45,1 Pkt startet den Zähler. Korrekt zu „um **>**1,5× ATR".
- Resets (a)/(b)/(c) und die Wiederaufnahme mit Zähler 1 zusätzlich über Fables Tests gegengelesen — sie prüfen State **und** Ausgabe, nicht nur eine Zeile.

**Fables vier Lesarten (offener Punkt 2) — alle vier bestätigt**, eine mit Begründung, die ins Regelwerk gehört:
1. **Kerzenabstand ab dem etablierenden Schluss: RICHTIG, und die einzig sinnvolle Lesart.** Die Alternative („voriger Schluss") macht den Abstand unter 5-Min-Kadenz fast immer 1 und die 60-Minuten-Grenze zu totem Recht. 8a5 misst laut eigenem Text „wiederholte FRISCHE Crosses" — ein Seitenwechsel nach langem Verweilen ist kein Ping-Pong.
2. **Reset (b) ab Bandkante, aktueller ATR:** „das Band **um** >1,5× ATR verlässt" = über die Kante hinaus. Bestätigt. Die leichte Asymmetrie (Bandbreite fixiert, Reset-Schwelle laufend) ist vom Text gedeckt — er fixiert ausdrücklich nur das Band.
3. **QQQ-Bestätigung = QQQ-15min-Bein auf der neuen Seite:** bestätigt; 8a5 definiert den Begriff nicht, und das 15min-Bein ist die kanonische QQQ-Größe des Projekts (Dual-Gate). *Info:* Gate zu / Bein unbekannt zählt in `n`, nicht in `m` — die Zeile untertreibt damit eher, als dass sie behauptet. Richtige Richtung.
4. **ADX am Serienbeginn:** bestätigt — derselbe Anker wie das `seit HH:MM` derselben Zeile, alle Serienangaben also konsistent; der ADX **jedes** Zyklus liegt zusätzlich im State und geht nicht verloren. Die Formulierung „bei Zyklusbeginn" bleibt zweideutig, das Ergebnis ist es nicht.

**O-2 (Nachbesserung, selbst gemacht):** Alle vier Lesarten standen bisher nur im Code-Kommentar. Das ist genau die Konstellation, aus der N1 entstanden ist (Maschine rechnet X, Regeltext sagt Y oder schweigt). In [[feedback_chartanalyse]] 8a5 steht jetzt ein Absatz **„Maschinelle Umsetzung + Lesart (P2, 14.09.2026 … Präzisierung, KEINE Regeländerung)"** mit allen vier Punkten. Außerhalb aller Cron-Anker, kein Test betroffen.

**Zu Fables offenem Punkt 4 (zwei Zähler nebeneinander): einverstanden, P3.** Der manuelle `--zyklus-8a5` bleibt, die Abweichung wird als Hinweis ausgewiesen (in meinem Lauf gesehen). Das ist die richtige Zwischenstufe — erst messen, ob der maschinelle Zähler stimmt, dann den manuellen ablösen.

---

### 3. 9b-Divergenz-Check — korrekt, und die richtige Regel getroffen

**Zuerst die Verwechslungsfrage geprüft** (Auftrag): [[feedback_live_trading]] Z1010 dokumentiert die Umbenennung des *Reversal*-„9b" zu Punkt 11. Fable hat die **richtige** Regel gebaut: [[feedback_chartanalyse]] 9b (Divergenz, 23.06.2026, „Kurs hält Hochs, Histogram fällt") — belegt durch das Zeilenformat in Punkt 2b („MACD-H(5min) … divergent … gegen Richtung", 27.08.2026, Anlass Short #T1 vom 24.08.) und dadurch, dass Punkt 11 eine 3-von-4-Kriterienliste für den Dreh ist, keine Ausgabezeile. **Bestätigt.**

Eigene Szenarien (je 3–4 Voll-Checks mit State):

| Szenario | Ergebnis |
|---|---|
| Kurs 29540→29555→29570, MACD-H 1,2→0,9→0,5, `--richtung long` | `Divergenz erkannt seit 15:55 → divergent → WARNUNG: gegen Richtung [bärisch: …]` — „seit" bleibt am Erstbefund |
| Kurs **und** MACD-H steigen (1,2→1,4→1,8) | `keine Divergenz → positiv → kein Widerspruch` |
| neue Tiefs mit steigendem MACD-H, `--richtung short` | `WARNUNG: gegen Richtung`; derselbe Befund bei `--richtung long` → `kein Widerspruch (Divergenz IN Richtung)` |
| Hoch 3 mit MACD-H über Hoch 2, aber unter Hoch 1 | aufgelöst (rollende Referenz) |

Format exakt dreiteilig wie im Regeltext, Belegzahlen in eckiger Klammer dahinter (gute Zugabe: die Zeile ist nachrechenbar statt nur behauptet). Kein Baustein, kein Gate — regelkonform.

**Fables Lesart (offener Punkt 3): bestätigt.** Tages-Schluss-Extrema als Referenz mit MACD-H **an** diesem Extremum ist die Lehrbuch-Konstruktion (aufeinanderfolgende Hochs vergleichen, nicht gegen ein laufendes MACD-H-Maximum); kein Zeitverfall ist richtig, weil die Regel keinen nennt; die Zeile an JEDEM Voll-Check statt nur „vor jeder Order" ist eine Obermenge der Pflicht und kostet nichts. **O-2 gilt auch hier:** Lesart als Absatz in [[feedback_chartanalyse]] 9b nachgetragen, inklusive der Einschränkung „Extrema seit dem ersten Voll-Check des Tages".

*Info, kein Mangel:* Die Belegklammer nennt „Tages-Schluss-Hoch/-Tief"; gemeint ist „seit dem ersten Voll-Check des Tages". An einem Loop, der um 15:30 startet, ist das nicht dasselbe wie das Tageshoch. Steht jetzt im Regeltext; die Zeile selbst habe ich nicht angefasst (Format ist testgepinnt, Nutzen gering).

---

### 4. F-A/F-C isoliert — Grundlogik sauber

Mit garantiert korrekter Wasserstand-Semantik (Wegwerf-Datei, `--file`, `--jetzt`-Zeitanker) durchgerechnet:

| Fall | Erwartung | Skript |
|---|---|---|
| Abruf 17:00:20, VC 17:05 off-Raster | kein verpasster Slot | `Nachholen: NEIN`, `NICHT FAELLIG` ✓ |
| Abruf 16:50, VC 17:05 (17:00 wirklich verpasst) | nachholen | `Nachholen: JA (Slot 17:00)`, `FAELLIG JA — Grund: Nachholen` ✓ |
| Abruf 17:00, VC 17:10 Raster | Raster+Delta | `FAELLIG JA — Grund: Raster+Delta` ✓ |
| Abruf 16:55, VC 17:00 Raster | Delta 5 < 10, nichts verpasst | `NICHT FAELLIG`, alle drei NEIN ✓ |

Die Slot-Arithmetik (`letzterSlotMs`, in der Raster-Minute der Slot **davor**) ist richtig konstruiert — der laufende Slot ist nicht „verpasst", sondern über (a)+(b) fällig. Die Begründungszeile nennt bei mehreren Gründen alle, getrennt durch `|`. Kein Doppelzählen, keine Zeitzonenfalle (Berlin-Offset ist stundengenau, DE-Minuten liegen auf UTC-Minuten).

---

### 5. Downstream `vollcheck.cjs` — ÜBER-POLLING nur noch bei allen drei NEIN: bestätigt

Am Code nachvollzogen (der `ÜBER-POLLING`-Zweig ist das `else if (tweetFetch === true)` **nach** den `faellig`-Zweigen, also strukturell nur bei `!faellig` erreichbar) und in eigenen Läufen bestätigt. Die `Faelligkeit gesamt:`-Zeile wird korrekt gelesen (`FAELLIG JA`, Grund hinter `— Grund: `), mit Fallback auf die Einzelzeilen, falls sie fehlt — defensiv und richtig herum. Die Klammer-Zitate tragen jetzt die Gesamt-Zeile und ggf. `--anlass`, d.h. der Protokollleser kann das Urteil nachrechnen.

**Vollständigkeits-Scan (Auftrag Punkt 8):** `position_tick.cjs` hat **null** Tweet-Bezug (geprüft, nicht angenommen). `protokoll_bilanz.cjs` zählt nur die Anwesenheit der `Tweet-Check`-Pflichtzeile (`/^\**Tweet-Check/`) — Präfix unverändert, keine Auswertung von ✓/✗/Über-Polling, also keine stille Folgewirkung. `abschluss.cjs`/`validierung_check.cjs`: kein `x_fetch`-Bezug. **Keine übersehene Stelle.** Einzige echte Downstream-Wirkung ist der Format-Baustein → `luecken` → `vollstaendig` — und genau die trifft der F-A×F-B-Befund (Abschnitt 1).

---

### 6. `--anlass`-Design — **war ein Freibrief. Selbst gefixt (O-1).**

Fables Bau: nacktes `--anlass` = Exit 1, jeder nicht-leere Text = fällig. **„Nackt = Exit 1" ist richtig, aber bei weitem nicht genug.** Im Auslieferungszustand machte **jeder beliebige Text** den Fetch fällig — Fables eigenes Beispiel (`--anlass "CPI 14:30"` an einem Zeitanker um 16:35/16:45) liegt mehr als zwei Stunden außerhalb des Fensters, das der Parameter behauptet, und wurde trotzdem als `FAELLIG JA` akzeptiert. Damit gilt:

- Setzt der Loop den Parameter versehentlich dauerhaft, ist der Über-Polling-Schutz aus Punkt 17 **komplett** abgeschaltet, und zwar unsichtbar (keine ✗-Marke mehr, sondern `Faelligkeit gesamt: FAELLIG JA — Grund: Anlass "…"`).
- Das ist exakt die Klasse „Etikett statt Rechnung", gegen die Punkt 17 gebaut wurde — und exakt das, was ich im N1-Gegencheck (Abschnitt 7 Punkt 1) als Bedingung formuliert hatte: „sonst ist der Zweig nicht prüfbar, sondern nur behauptbar". Die Hälfte davon (expliziter Eingang) ist gebaut, die andere (Prüfbarkeit) fehlte.

**O-1 (Nachbesserung, selbst gebaut — die Regel nennt ein rechenbares Kriterium, also wird es gerechnet):**
- `x_fetch_stamp.cjs`: liest die Uhrzeit **aus** dem Anlasstext und rechnet den Abstand zum Zeitanker (DE-Ortszeit, Tagesgrenze zirkulär). `anlassOk` nur bei Abstand ≤ 15 Min. Drei Ausgabeformen: `JA "…" (Release 17:05 DE, 10 Min vom Zeitanker 17:15 DE, Fenster +/-15 Min)` · `NEIN — "…" nennt 14:30 DE, das liegt 165 Min vom Zeitanker entfernt (> 15 Min, Fenster zu) — kein Faelligkeitsgrund` · `NEIN — "…" nennt keine Uhrzeit HH:MM, … nicht pruefbar`.
- `vollcheck.cjs`: `--tweet-anlass` ohne HH:MM = **Hard-Exit 1** (gleiche Klasse wie nackt). Liegt die Zeit außerhalb des Fensters, fällt ein gemeldeter Fetch regulär in den ÜBER-POLLING-Zweig zurück — die Zeile nennt den Grund jetzt beim Namen: `Anlass "…" zaehlt NICHT (Release ausserhalb ±15 Min zum Zeitanker …)`.
- Textquellen nachgezogen, damit Regel und Maschine dasselbe sagen: [[feedback_vollcheck_format]] Template A (situative Zeile), `loop_prompt.cjs` Z200 (Tweet-Satz), [[feedback_live_trading]] „Anlass-Trigger unverändert" (Klammer; außerhalb aller Cron-Anker).
- **Tests erweitert** (3 neue Fälle im F-C-Test, 2 in der `vollcheck.cjs`-Prüfung): Release im Fenster = Grund · Release 65 Min entfernt = **kein** Grund, `NICHT FAELLIG` · Text ohne Uhrzeit = kein Grund · derselbe Off-Raster-Fetch mit Anlass außerhalb = weiterhin `ÜBER-POLLING ✗` · `--tweet-anlass "CPI-Zahlen"` = Exit 1. Die fest verdrahteten „CPI 14:30"-Beispiele der bestehenden Tests laufen jetzt gegen einen aus dem Zeitanker abgeleiteten Wert.

**Was O-1 NICHT leistet — ehrlich:** Ob um 14:30 wirklich ein Release war, kann kein Skript wissen. Wer `--tweet-anlass "Egal <jetzt+2min>"` schreibt, kommt weiterhin durch. Aber: die Behauptung muss jetzt zur Uhr passen, sie steht wörtlich in der Pflichtzeile und im JSON (`out.tweet.anlass`), und sie lässt sich gegen den Wirtschaftskalender des Session-Updates gegenlesen. Aus einem Freibrief ist ein nachprüfbarer Beleg geworden — mehr ist ohne Kalenderanbindung nicht zu haben.

---

### 7. Tests — selbst gelaufen, und selbst gegengelesen

`node --test tests/trading_scripts.test.js` → **84/84 pass, 0 fail** (vorher 77/77), vor und nach meinen Fixes; Laufzeit ~25 s.

Die sieben neuen Tests prüfen wirklich das Behauptete, nicht nur die Oberfläche — stichprobenartig gegengelesen: die Zyklenzählung läuft über sechs echte `vollcheck.cjs`-Aufrufe mit State-Persistenz (nicht simuliert), die Pflichtzeile per Regex im **exakten** 8a5-Format inklusive `seit`-Wert aus dem Kerzenschluss, das Ausbleiben der Zeile vor dem 3. Zyklus mit `doesNotMatch`, die 9b-Zeile einmal per `assert.equal` **wörtlich**, die Resets je einzeln mit Prüfung von State **und** Ausgabe, und die F-A/F-C-Tests enthalten eine echte Regression (derselbe Fetch ohne `--tweet-anlass` muss ÜBER-POLLING bleiben). Die Zeitanker sind slot-aliniert und liegen zwei Stunden in der Vergangenheit — die Tests hängen nicht an der Wanduhr-Minute. Gute Arbeit.

*Eine Lücke, die die Tests nicht schließen (und auch nicht schließen können):* Der F-A×F-B-Fall ist nicht testbar, solange `last_fetch` zwei Bedeutungen tragen kann — die Tests setzen den Wasserstand immer als „letzter Abruf". Genau deshalb ist er den Tests durchgegangen. **Auflage für F-B: ein Test, der einen ruhigen Nachmittag über mehrere Voll-Checks fährt** (Wasserstand steht, Fetches laufen auf dem Raster) **und nachweist, dass kein Off-Raster-VC ÜBERFÄLLIG meldet.**

---

### 8. Was Opus geändert hat (kein Commit, kein Push)

| ID | Datei/Stelle | Änderung | Klasse |
|---|---|---|---|
| O-1 | `scripts/x_fetch_stamp.cjs` (Anlass-Block, Ausgabezeile, Kopfkommentar), `scripts/vollcheck.cjs` (`--tweet-anlass`-Guard, ÜBER-POLLING-Text, Kopfkommentar), `scripts/loop_prompt.cjs` Z200, `feedback_vollcheck_format.md` Template A, `feedback_live_trading.md` „Anlass-Trigger", `tests/trading_scripts.test.js` (+5 Fälle) | `--anlass`/`--tweet-anlass`: Release-Uhrzeit wird gegen das ±15-Min-Fenster **gerechnet**; ohne HH:MM Hard-Exit 1, außerhalb des Fensters kein Fälligkeitsgrund | **Nachbesserung** (Über-Polling-Schutz war umgehbar) |
| O-2 | `feedback_chartanalyse.md` 8a5 + 9b | Lesarten der maschinellen Umsetzung als Absatz im Regeltext (bisher nur Code-Kommentar) — Präzisierung, keine Regeländerung | **Nachbesserung** (N1-Fehlerklasse: Maschine rechnet, Regeltext schweigt) |

Nicht angefasst: `position_tick.cjs`, `gate_check.cjs`, `protokoll_bilanz.cjs`, die 8a5-/9b-/Punkt-9-Regelinhalte selbst, alle sechs Cron-Anker, `trades.db`.

**Verifikation nach den Fixes:** Tests **84/84**; `node scripts/loop_prompt.cjs --testtag fiktiv --terminal-zeit 19:00` → **Exit 0, stderr leer, 3/3 Bausteine per Anker, 44.976 B**; F-A×F-B-Simulation und die 8a5-/9b-Sequenzen nach dem Fix erneut gefahren, unverändertes Verhalten.

---

### 9. Offene Nachbesserungen (nicht selbst gemacht, kein Blocker)

- **N-1 · F-B muss Bedingung (b) mit umbauen** (Rundungsfalle, Abschnitt 1) — gehört in den F-B-Auftrag, nicht daneben.
- **N-2 · Ruhiger-Nachmittag-Test** als Teil von F-B (Abschnitt 7).
- **N-3 · Die Nachhol-✗-Zeile trägt den F-B-Vorbehalt nicht.** `x_fetch_stamp.cjs` druckt den Hinweis, `vollcheck.cjs` zitiert nur Delta-/Raster-/Gesamt-Zeile — im Protokoll steht das falsche ✗ also ohne Warnung. Erledigt sich mit F-B; nur relevant, falls F-B sich verzögert.
- **N-4 · Prompt-Größe** 43,5 → 45,0 KB pro Fire (P1 43.496 B → jetzt 44.976 B, davon ~560 B aus O-1). Weiterhin P3-Kandidat, keine neue Lage.

### Antwort auf die Entscheidungsfrage

**FREIGEGEBEN MIT AUFLAGEN — kein Blocker gegen das Paket. F-B kann beauftragt werden und sollte es sofort werden.** Level-Ping-Pong und 9b sind fertig, richtig und ohne Konsequenzrisiko (reine Anzeige); F-C ist nach O-1 prüfbar statt behauptbar; F-A ist korrekt gebaut, aber **ohne F-B nicht betriebstauglich** — die einzige harte Auflage lautet: **kein Testtag, bevor F-B steht.**
