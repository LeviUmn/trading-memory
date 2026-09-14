---
name: project-fable-auftragsliste-2026-09-03
description: "Konsolidierte Arbeitsliste aus den beiden Opus-Testtag-Analysen 01.09. und 02.09.2026, zur wortwörtlichen Weitergabe an Fable: 25 deduplizierte Punkte in drei Kategorien (7 SKRIPT-FIX, 5 REGELWERK-FRAGE, 13 PROMPT/PROZESS-ERGÄNZUNG), sortiert nach Priorität (6 Hoch, 13 Mittel, 6 Niedrig). Entwurf, keine Maßnahme ist umgesetzt."
metadata: 
  node_type: memory
  type: project
  status: draft
  originSessionId: e1a9edc1-4a4d-4c86-b03e-98d36d93627e
  modified: 2026-09-03T09:23:31.080Z
---

# Auftragsliste für Fable — Konsolidierung der Testtag-Befunde 01./02.09.2026

**Status: Entwurf zur Weitergabe an Fable. Keine der hier genannten Maßnahmen ist umgesetzt, keine Regeländerung ist beschlossen.** Diese Liste ist eine Handlungsempfehlung, kein Beschluss. Punkte der Kategorie REGELWERK-FRAGE brauchen vor der Umsetzung eine ausdrückliche Entscheidung von Levi.

**Quellen:** [[project_testtag_analyse_2026-09-01]] (Tag 1) und [[project_testtag_analyse_2026-09-02]] (Tag 2). Beide Analysen enthalten bewusst keine Lösungsvorschläge; die Formulierungen „was zu tun ist" unten stammen aus dieser Konsolidierung, nicht aus den Quellanalysen.

**Kategorien:**
- **SKRIPT-FIX** — Änderung an einer konkreten Datei (Code/Datenstruktur). Keine Handelsregel betroffen, kann Fable direkt umsetzen.
- **REGELWERK-FRAGE** — inhaltliche Entscheidung am Handelsregelwerk. **Nicht ohne Levis Entscheidung umsetzen**, Fable liefert Entscheidungsvorlage/Optionen.
- **PROMPT/PROZESS-ERGÄNZUNG** — fehlende Pflichtzeile, Checklistenposition oder Formulierung im Loop-Ablauf/Tick-Prompt, den Sonnet täglich bekommt. Keine Skriptänderung.

Sortierung: Priorität, innerhalb der Priorität SKRIPT-FIX → REGELWERK-FRAGE → PROMPT/PROZESS-ERGÄNZUNG.

---

## Hoch

### 1. SKRIPT-FIX — `--cluster-level` darf 8c2 nicht mehr still abschalten
**Datei:** `scripts/gate_check.cjs` (Zeile ~995)
**Zu tun:** `--cluster-level` auf die Messfeld-Pflichtliste setzen. Fehlt der Parameter, darf das 8c2-Gate nicht mehr auf `pass: true` mit „Regel nicht ausgeloest" laufen, sondern muss als `UNKNOWN`/Messfeld-Ausnahme (A3) mit erzwungener `--grund-*`-Begründung ausgewiesen werden. Ziel: „nicht geprüft" muss im Output von „geprüft und unbedenklich" unterscheidbar sein.
**Quelle:** Tag 1 Abschnitt 1.6 / 3.3, Punch-Liste Hoch #2 — an Tag 1 bei #64/#66 real eingetreten, bei #66 mit SL exakt in der frisch per Docht getesteten Zone. Tag 2 Abschnitt 1.6 belegt, dass der Parameter gesetzt gate-entscheidend wirkt (einziger FAIL).

### 2. SKRIPT-FIX — Frische-Guard des Levelregisters muss ein implausibles Alter blockieren
**Datei:** `scripts/gate_check.cjs`, Registerprüfung gegen `scripts/level_register.json`
**Zu tun:** Ein negatives Registeralter darf nicht mehr nur gedruckt, sondern muss als FAIL/UNKNOWN behandelt werden. Zusätzlich eine Obergrenze für das zulässige Alter definieren und prüfen (Wert ist eine Regelfrage, siehe Punkt 21 — der Mechanismus selbst ist ein reiner Skript-Fix).
**Quelle:** Tag 1 Abschnitt 2 („Neue Lücke 1"), Punch-Liste Hoch #4 — Guard war den ganzen Tag inert („Alter −106 Min"), Session-Tief stand danach falsch im Register. Tag 2 Abschnitt 2 zeigt, dass ein positives, korrektes Alter („Alter 9 Min") erreichbar ist.

### 3. REGELWERK-FRAGE — Definition von 8d-Kriterium 2 verbindlich festschreiben
**Zu tun:** Entscheiden und ins Regelwerk schreiben, ob „Makro-Häufung" ein **fortbestehender Zustand** ist oder ein **neues Einzelereignis** verlangt, und was als zählendes Event gilt (Tag 2 arbeitete mit „≥2 Events von der Standard-Blackout-Liste am selben Tag, keine bloßen Marktreaktionen/News"). Danach als feste Definition in den Loop-Prompt, nicht mehr als tagesweise Ad-hoc-Festlegung.
**Warum wichtig:** steuert `--tier normal` vs. `--tier schock` und damit SL-Mindestdistanz (8c) und 8b1a.
**Quelle:** Tag 1 Abschnitt 1.3 / 6.1, Punch-Liste Mittel #6 (**neu bewertet auf Hoch**, weil es Gate-Eingaben steuert und Tag 2 den Fehler nur durch tagesweise Eigendisziplin vermied); Tag 2 Abschnitt 1.3.

### 4. PROMPT/PROZESS-ERGÄNZUNG — Zeitanker dauerhaft im Loop verankern
**Zu tun:** Die am 02.09. ad hoc eingeführte Zeitdisziplin fest in den Tick-/Loop-Prompt aufnehmen: bei **jedem** Fire zuerst echte DE-Systemzeit per `date` abrufen, sekundengenauen Zeitstempel in jede Voll-Check-/Quick-Tick-Überschrift schreiben, Minutenlogik und Nummerierung daran hängen (kein reiner Fire-Zähler), ausgefallene Fires als sichtbare Lücke ausweisen statt lautlos weiterzuzählen.
**Quelle:** Tag 1 Abschnitt 4, Punch-Liste Hoch #3 (19–20 Min Drift, Terminalbedingung real 22:04 statt 21:45); Tag 2 Abschnitt 4 (Drift per Screenshot-mtimes auf <1 Min verifiziert — Fix wirkt, ist aber bisher nur eine Tagesfestlegung).

### 5. PROMPT/PROZESS-ERGÄNZUNG — „Wörtlich" heißt wörtlich (dritte Wiederholung)
**Zu tun:** Pflichtformulierung in den Loop-Ablauf: Als „wörtlich" gekennzeichnete Skriptausgaben (`gate_check.cjs`, `cooldown_check.cjs`) dürfen nur per Datei-Read aus `scripts/last_gate_check.txt` bzw. `scripts/last_cooldown_check.txt` übernommen werden — vollständig, inklusive Messfeld-Ausnahmen, Register-/TP1-Herkunftszeilen, Rohwerten, Q-Score, `8b1-Schritt-5-Vorschau` und des Aufrufs mit Redirect `> scripts/last_gate_check.txt 2>&1; echo "Exit-Code: $?"`. Genau **eine** Fassung pro Ausgabe im Dokument; jede gekürzte Wiedergabe muss ausdrücklich als „gekürzt" gekennzeichnet werden und darf nicht „wörtlich" heißen.
**Quelle:** 28.08.-Befund, Tag 1 Abschnitt 3.2 / Punch-Liste Hoch #5, Tag 2 Abschnitt 3.1 / Punch-Liste Hoch #1. Einziger Hoch-Befund aus Tag 1, der an Tag 2 nicht behoben wurde. Flankierender Skript-Fix: Punkt 10.

### 6. PROMPT/PROZESS-ERGÄNZUNG — Halbierungsfenster 15:30–16:00 in der Format-Zeile mitführen
**Zu tun:** Analog zur Order-Sperre 15:00–15:30 eine Pflichtangabe in jeder Format-Zeile zwischen 15:30 und 16:00 („Halbierungsfenster aktiv") und eine explizite Ablauf-Markierung beim ersten Voll-Check danach.
**Quelle:** Tag 2 Abschnitt 1.4, Punch-Liste Mittel #6 (**neu bewertet auf Hoch**, weil es eine Positionsgrößen-/Risikoregel ist, die beim Echtgeld-Start unmittelbar Geld kostet). Am 02.09. nur einmal in der Vorbereitung erwähnt, in keinem Voll-Check; der einzige 2/2 fiel um 16:00:21 nur knapp daneben.

---

## Mittel

### 7. SKRIPT-FIX — Plausibilitätsguard für `x_last_fetch.json`
**Datei:** `scripts/x_last_fetch.json` (Schreibkonvention) + schreibende Stelle im Loop/Skript
**Zu tun:** Schreiben nur mit echtem UTC-Stempel und `Z`-Suffix; ein Stempel **in der Zukunft** (> jetzt) wird abgelehnt bzw. sichtbar gewarnt. Der Wert muss der Zeitstempel des tatsächlich jüngsten gesehenen Tweets sein, nicht ein pauschaler +10-Minuten-Fortschritt.
**Quelle:** Tag 1 Abschnitt 3.1, Punch-Liste Hoch #1 (Endstand ~2 h in der Zukunft, gesamte Tagesnachrichtenlage wäre aus dem nächsten Delta gefallen); Tag 2 Abschnitt 4.4 (an Tag 2 durch Disziplin vermieden — der Schutzmechanismus fehlt weiterhin). Priorität von Hoch auf Mittel gesenkt, weil der akute Dateizustand aus Tag 1 inzwischen bereinigt ist.

### 8. SKRIPT-FIX — Rundzahl-Kategorie im Levelregister entzirkularisieren
**Datei:** `scripts/level_register.json` (Struktur/Konvention) + Ausgabe in `gate_check.cjs`
**Zu tun:** Für die Kategorie „Rundzahlen" ist der Abrufweg „berechnet aus aktuellem Kurs, 50er-Schritte im Band" — der Registerabgleich kann per Konstruktion nicht fehlschlagen. Entweder unabhängigen Abrufweg definieren oder die Ausgabe für diese Kategorie nicht als „VERIFIZIERT" labeln, sondern als „generiert, keine unabhängige Verifikation".
**Quelle:** Tag 1 Abschnitt 2 („Neue Lücke 2"), Punch-Liste Mittel #8 (alle vier TP1 tautologisch verifiziert); Tag 2 Abschnitt 2, Punch-Liste Niedrig #7 — an Tag 2 nur deshalb nicht aufgetreten, weil TP1 ein PDL war.

### 9. SKRIPT-FIX — `--dual-gate-q2-budget-pct`: Formel festschreiben oder selbst berechnen
**Datei:** `scripts/gate_check.cjs`
**Zu tun:** Die Berechnungsformel im Skript dokumentieren und, wo möglich, aus bereits übergebenen Rohwerten selbst berechnen, sodass die Begründung „Berechnungsformel nicht eindeutig rekonstruierbar" als Messfeld-Ausnahme nicht mehr möglich ist.
**Quelle:** Tag 1 Abschnitt 1.6, Punch-Liste Mittel #10 (an #40/#42 berechnet, an #64/#66 weggelassen); Tag 2 Abschnitt 1.6 (dieselbe Begründung erneut verwendet).

### 10. SKRIPT-FIX — Gegenprüf-Helper für zitierte Skriptausgaben
**Zu tun:** Kleiner Helper, der den ins Protokoll übernommenen Ausgabeblock gegen `scripts/last_gate_check.txt` bzw. `scripts/last_cooldown_check.txt` diffed und Abweichungen meldet. Flankiert Punkt 5 maschinell, weil der Loop den eigenen Bericht bisher nie gegen die Quelle prüft.
**Quelle:** Tag 1 Abschnitt 3.2; Tag 2 Abschnitt 3.1 und Abschnitt 13 („Der Loop kann Fehler finden, die er *macht*, aber nicht Fehler, die er beim *Berichten* macht").

### 11. REGELWERK-FRAGE — Formel von 8d-Kriterium 3 überarbeiten
**Zu tun:** Entscheiden, ob (Hoch−Tief)/Tief am VIX die richtige Messgröße ist. Sie unterscheidet nicht zwischen Spike und Entspannung: Am 02.09. war K3 den ganzen Tag erfüllt und wurde bei **fallendem** VIX (16,82 → 15,12) immer deutlicher erfüllt (5,52 % → 11,24 %). Optionen: Richtungsbedingung ergänzen, auf VIX-Niveau/Anstieg umstellen, oder Kriterium ersetzen.
**Quelle:** Tag 2 Abschnitt 1.3, Punch-Liste Niedrig #13 (**neu bewertet auf Mittel**: ein dauerhaft erfülltes Kriterium, das nichts misst, würde 8d aus dem falschen Grund scharf schalten, sobald K1 mitzieht).

### 12. REGELWERK-FRAGE — Abbruch-Gate um „stille Defekte" erweitern
**Zu tun:** Entscheiden, ob das vierteilige Abbruch-Gate (vorzeitiges Ende, ausgefallene Voll-Checks, UNKNOWN-Ablehnung, Exit-1) um ein Kriterium für stille Defekte ergänzt wird — z. B. nachweislich falsche Aussagen im Faktenprotokoll, still deaktiviertes Hard-Gate, korrumpierte Statusdatei. Aktuell hätte Tag 1 mit allen drei Defekten das Gate ohne einen einzigen Treffer passiert.
**Quelle:** Tag 2 Abschnitt 7.3, Punch-Liste Niedrig #14 (**neu bewertet auf Mittel**, weil dieses Gate über die Fortsetzung der Testtag-Serie und damit mittelbar über den Echtgeld-Start entscheidet).

### 13. PROMPT/PROZESS-ERGÄNZUNG — Faktenprotokoll-Zahlen auszählen statt erinnern
**Zu tun:** Pflichtvorgabe für den Tagesabschluss: Jede Bilanzzahl (Voll-Check-Zahl, Quick-Tick-Zahl, 2/2-Momente, 7b1c-Anwendungen, gate_check-Verteilung, Bruch-Liste, Fire-Zahl) wird am fertigen Dokument ausgezählt, nicht aus dem Gedächtnis geschrieben. Die Bruch-/Lückenliste wird gegen das vollständige Minutenraster gestellt; das Wort „lückenlos" darf nur stehen, wenn diese Gegenrechnung vorliegt. Kann per Auszählungs-Helper unterstützt werden.
**Quelle:** Tag 1 Abschnitt 6 / Punch-Liste Mittel #9 und Niedrig #14, #15 (vier belegbare Fehler: 8d-Verlauf, 7b1c 3 statt 8, fehlender #70-Vorfall mit falscher „keine weiteren Vorfälle"-Behauptung, falsche Uhrzeit 18:51/17:51, Fire-Zahl ≈420 vs. 429); Tag 2 Abschnitt 3.3 / 6.6, Punch-Liste Mittel #3 (35 Minutenausfälle, 27 erfasst, 8 nicht — Anspruch „lückenlos" nicht gedeckt).

### 14. PROMPT/PROZESS-ERGÄNZUNG — Voll-Check-Nummer verbindlich in die Format-Zeile
**Zu tun:** Die Nummernpflicht („Voll-Check #n vollständig (MTF ✓ | …)") explizit im Tick-Prompt verankern. Diese Lücke stand bereits als Punkt 4 im 31.08.-Gegencheck und wurde nie geschlossen.
**Quelle:** Tag 1 Abschnitt 3.6, Punch-Liste Mittel #7 (ab #31 für 54 Voll-Checks ohne Nummer, prompt Nummerndrift #45/#44); Tag 2 Abschnitt 6.1 belegt, dass 84/84 erreichbar sind.

### 15. PROMPT/PROZESS-ERGÄNZUNG — „NEUER bestätigter Schluss" nur mit Timestamp-Nachweis
**Zu tun:** Im MTF-Block je Zeitebene den Unix-Open-Timestamp der Referenzkerze mitführen und gegen den Vorcheck vergleichen. „NEUER bestätigter Schluss" darf nur gemeldet werden, wenn der Timestamp gewechselt hat. Zusatzregel: zu einer Uhrzeit, zu der eine Zeitebene gar nicht schließen kann (z. B. 20:55 für 1H/15min), ist die Meldung unzulässig.
**Quelle:** Tag 2 Abschnitt 3.2, Punch-Liste Mittel #2 — VC#72 meldete drei von vier Ebenen falsch, zwei Werte ziffernidentisch mit den fünf Minuten zuvor als „kein neuer" bezeichneten; Folge: 1H-Zählung ab VC#72 um eins zu hoch, nie korrigiert.

### 16. PROMPT/PROZESS-ERGÄNZUNG — Terminalbedingung 21:45 im 21:45-Voll-Check prüfen
**Zu tun:** Pflichtzeile im Voll-Check um 21:45 (Fließtext **und** Format-Zeile): „Terminalbedingung 21:45 geprüft — [Position offen: Handlung / keine Position: nicht einschlägig]". Nicht erst im Faktenprotokoll-Abschluss.
**Quelle:** Tag 2 Abschnitt 1.5, Punch-Liste Mittel #5 (in VC#82 fehlend, erst in §6 des Abschlusses); Tag 1 Abschnitt 1.5 (dort im Voll-Check selbst dokumentiert — Rückschritt an Tag 2).

### 17. PROMPT/PROZESS-ERGÄNZUNG — Tweet-Fälligkeit rechnerisch bestimmen
**Zu tun:** Fälligkeit des Tweet-Checks aus dem Wasserstand in `x_last_fetch.json` berechnen, nicht per Etikett behaupten. „10-Min-Raster, fällig" darf nur stehen, wenn tatsächlich ≥10 Minuten vergangen sind. Zusätzlich: Über-Polling als mögliche Ursache leerer `@DeItaone`-Antworten mitprüfen.
**Quelle:** Tag 2 Abschnitt 3.4, Punch-Liste Mittel #4 (61 Abrufe auf 84 Voll-Checks statt ~42, ab VC#27 faktisch 5-Minuten-Takt); Tag 1 Abschnitt 3.1(a) (dieselbe Kadenz, dort mit Wasserstandsschaden).

### 18. PROMPT/PROZESS-ERGÄNZUNG — „2/2" als Zustand und als Trigger-Ereignis sauber trennen
**Zu tun:** Zwei getrennte Begriffe im Loop-Ablauf festschreiben („Dual-Gate-2/2-**Zustand**" vs. „2/2-**Trigger-Ereignis** mit vollem 7b1-5-Ablauf") und beide in der Tagesbilanz **getrennt** zählen. Ein 2/2-Zustand, der ohne vollen Ablauf verworfen wird (z. B. per 7b1c-Stale), muss mit Begründung in der Bilanz erscheinen.
**Quelle:** Tag 1 Abschnitt 1.1 und 6.5, Punch-Liste Mittel #11 (#48 fehlte ganz in der Bilanz) und Niedrig #13 (sechs Checks mit dem Etikett, gezählt wurden vier); Tag 2 Abschnitt 1.1 liefert mit der VC#7-Formulierung die brauchbare Vorlage.

### 19. PROMPT/PROZESS-ERGÄNZUNG — Laufende Messgrößen rückverfolgbar mitführen
**Zu tun:** 8a5-Zyklenzählung und die Kerzenzähler für `--kerzen-nas100` / `--kerzen-qqq` in jedem Voll-Check als fortlaufender Zählstand mitführen, damit sie am Trigger-Moment nicht per Messfeld-Ausnahme entfallen. Begründungen wie „im Protokollverlauf nicht lücken- und zweifelsfrei rückverfolgt" sollen dadurch strukturell unmöglich werden.
**Quelle:** Tag 1 Abschnitt 1.6, Punch-Liste Mittel #10 (Messdisziplin degradiert über den Tag: `--q1-reject`, `--q3-coherence`, `--qqq-volume-below-avg` entfielen); Tag 2 Abschnitt 1.6 und §5 des Faktenprotokolls (Rückverfolgbarkeitsproblem des Protokollformats, freiwillig eingeräumt).

---

## Niedrig

### 20. SKRIPT-FIX — Pflichtfeld `--entry-source` in `gate_check.cjs`
**Zu tun:** Analog zu `--tp1-level-source` ein Pflichtfeld für die Herkunft des Entry-Preises einführen (bestätigter Kerzenschluss / Registerlevel / Live-Kurs bei Gate-Aufruf), mit Messfeld-Ausnahme bei Fehlen.
**Quelle:** Tag 2 Abschnitt 8.4, Punch-Liste Niedrig #8 — Entry 29.008,45 bei VC#13 war weder Kerzenschluss noch Registerlevel, die Herkunft steht nirgends.

### 21. REGELWERK-FRAGE — Prüft das Register Herkunft oder auch Tragfähigkeit?
**Zu tun:** Entscheiden, ob ein Level, das intraday bereits durchhandelt wurde (Tag 2: TP1 = PDL 28.954,35, Tagestief lag 72 Pkt darunter), als TP1-Level abgewertet oder gesondert gekennzeichnet werden soll. „VERIFIZIERT" belegt derzeit nur die Herkunft, nicht die Tragfähigkeit.
**Quelle:** Tag 2 Abschnitt 8.4, Punch-Liste Niedrig #9.

### 22. REGELWERK-FRAGE — Frischefenster für Gate-Eingaben definieren
**Zu tun:** Festlegen, wie alt eine Referenzkerze je Zeitebene beim Gate-Aufruf höchstens sein darf. `element[0]` von `count:2` liefert je nach Feed-Rollover eine um eine Kerze schwankende Frische — auf 1H bis zu 60 Minuten; bei VC#13 waren 1H/15min sekundenaktuell, die 5min-Referenz fünf Minuten älter, der Gate-Snapshot also nicht zeitgleich. Liefert zugleich den Schwellenwert für Punkt 2.
**Quelle:** Tag 2 Abschnitt 1.2, Punch-Liste Niedrig #12.

### 23. PROMPT/PROZESS-ERGÄNZUNG — Screenshot-Namenskonvention festschreiben
**Zu tun:** Ein einziges Schema für den ganzen Handelstag, mit **realer** Zeit im Dateinamen (nicht der simulierten), und keine doppelte Endung. Der Dateiname ist der einzige automatisch erzeugte externe Zeitanker.
**Quelle:** Tag 1 Abschnitt 3.6, Punch-Liste Niedrig #12 (Schemawechsel bei #31 und #59, Zwischenschema kodierte die simulierte Zeit); Tag 2 Abschnitt 3.5, Punch-Liste Niedrig #10 (`…vollcheck22.png.png`, 63 Dateien, 63 von 84 Protokollreferenzen nicht wörtlich auflösbar).

### 24. PROMPT/PROZESS-ERGÄNZUNG — Zeitbasen-Konvention
**Zu tun:** Festschreiben: im Protokoll durchgehend DE-Ortszeit, in Statusdateien UTC mit `Z`-Suffix; jede von der Grundkonvention abweichende Angabe wird ausdrücklich gekennzeichnet und mit DE-Umrechnung ergänzt.
**Quelle:** Tag 1 Abschnitt 3.1(b) (DE-Zeit mit `Z`-Suffix in `x_last_fetch.json` geschrieben); Tag 2 Abschnitt 6.7, Punch-Liste Niedrig #15 („Bis ~14:35 Uhr (Registerzeit)" = 16:35 DE in einer sonst DE-basierten Ereignisliste).

### 25. PROMPT/PROZESS-ERGÄNZUNG — Einheitliche Zählweise der Bestätigungsebenen
**Zu tun:** Eine Zählweise festlegen und durchhalten — 2-beinig (NAS100 15min + QQQ 15min, 1H als Override). Die parallel verwendete „x von 3"-Zählung inklusive 5min-Ebene nimmt eine Ebene auf, die im Dual-Gate-Sinn kein Bein ist, und darf nicht nebenher laufen.
**Quelle:** Tag 2 Abschnitt 1.1, Punch-Liste Niedrig #11 (Wechsel ab ca. VC#48/VC#50, folgenlos, aber uneinheitlich).

---

## Übersicht

| Priorität | SKRIPT-FIX | REGELWERK-FRAGE | PROMPT/PROZESS | Summe |
|---|---|---|---|---|
| Hoch | 2 (#1, #2) | 1 (#3) | 3 (#4, #5, #6) | **6** |
| Mittel | 4 (#7–#10) | 2 (#11, #12) | 7 (#13–#19) | **13** |
| Niedrig | 1 (#20) | 2 (#21, #22) | 3 (#23–#25) | **6** |
| **Summe** | **7** | **5** | **13** | **25** |

**Neu bewertete Prioritäten gegenüber den Quellanalysen:** #3 (Mittel → Hoch), #6 (Mittel → Hoch), #7 (Hoch → Mittel), #11 (Niedrig → Mittel), #12 (Niedrig → Mittel). Jede Anhebung/Senkung ist am jeweiligen Punkt begründet.

**Nicht in dieser Liste enthalten** (reine Beobachtungen ohne Handlungsbedarf): Tag-1-Abschnitt 7 und Tag-2-Abschnitt 11 (Positivbefunde), Tag-2-Abschnitt 9 (bullische Erholungsphase — als plausibel bestätigt, kein Defekt), Tag-2-Abschnitt 7.1/7.3 zur Anwendung des Abbruch-Gates (korrekt angewendet; nur das Design steht als Punkt 12 auf der Liste), Tag-2-Abschnitt 8.1–8.3 (VC#13-Gate-Anwendung, als vorbildlich bewertet).

*Diese Liste ist ein Entwurf zur Weitergabe an Fable. Nichts davon ist umgesetzt oder beschlossen. Kein MEMORY.md-Eintrag.*
