---
name: project_vollcheck_ausgabeformat_vereinfachung_todo_2026-09-16
description: "TODO (16.09.2026, nur notiert, nicht besprochen/umgesetzt): Levi will das Ausgabeformat der 1-Min-/5-Min-Ticks (Quick-Tick/Voll-Check) optisch/nutzerfreundlicher fuer die Ausfuehrung gestalten. Er braucht als Mensch nur die Kerndaten, der Rest kann vom System verarbeitet werden ohne dass er es jedes Mal lesen muss. Morgen in Ruhe mit Opus und Fable besprechen."
metadata:
  node_type: memory
  type: project
  status: "BEWAEHRUNGSTAG 23.09.2026 GESCHEITERT (3/5 Kriterien) — RUECKBAU per git checkout main durchgefuehrt, Branch kurzblock-bewaehrung bleibt als Referenz. Fable-Reparaturliste fuer 24.09. notiert, danach neuer Bewaehrungstag noetig."
  originSessionId: session_current
  modified: 2026-09-23T18:19:16.014Z
---

## Nachtrag 23.09.2026 Abend: BEWÄHRUNGSTAG AUSGEWERTET — RÜCKBAU

Opus-Analyse (frischer Subagent, Rohdaten selbst nachgezählt) des Bewährungstags: **3 von 5 Kriterien erfüllt (2 Archiv-Vollständigkeit, 3 keine verpassten Eskalationen, 5 Format-Zeilen-Logik byte-identisch — `npm test` 354/354 grün), 2 klar verfehlt:**
- **Kriterium 1 (≤8 Zeilen/≤1.200 Zeichen): 0 von 47 Voll-Checks erfüllt** (Median 13 Zeilen/4.875 Zeichen, Max 16/6.274). Ursache: die Dauer-Warnungen X1 (SL-Anker-Reset fällig) und X2 (TP1-Fenster) standen in ALLEN 47 Blöcken im vollen Wortlaut und machten allein 38,9% des gesamten Kurzblock-Texts aus, dazu der 8d-Regime-Hinweis (fehlender VIX-Wert) in allen 47. Kürzung ggü. Vollblock nur ~50% statt der geplanten ~87%.
- **Kriterium 4 (≥95% wörtliche Übernahme): nur 81,9% zeilenweise / 66% byte-identisch.** Sonnet hat in 17 von 47 Fällen (VC#1, #3-10, #47-54) genau diese überlangen Dauer-Warnungen von Hand gekürzt/umformuliert statt wörtlich zu übernehmen — Kriterium 1 und 4 sind ursächlich verknüpft: zu lange Pflichtzeilen führen zu Handkürzung.

Nebenfunde für Fable (unabhängig vom Kurzblock-Konzept): X1-Anker wurde trotz mindestens eines bestätigten Lower Highs (~17:01, Rücklauf 167 Pkt) nie nachgezogen (Prozessversäumnis, kein Regelbruch, X1 ist reine Diagnose); Whipsaw-Trigger um 20:00 DE wurde in Sonnets Freitext fälschlich als "Dual-Gate 2/2→1/2" bezeichnet obwohl die Maschinenzeile im selben Block weiter 2/2 zeigte (5m ist Trigger-Ebene, kein Gate-Bein); 6 von 7 ausgefallenen VC-Nummern lagen im Zeitfenster kurz vor einem Tweet-Slot (Muster, Ursache nicht belegt); ÜBER-POLLING-Meldungen (4x) waren harmlose Fehlalarme (Nachhol-Fetch lief knapp vor dem Voll-Check-Aufruf); negative Referenzkerzen-Alter in 42/47 Checks wurden von `vollcheck.cjs` fälschlich als ✓ gewertet (Zeile 1440); Entry-Leiter zeigt hartkodiert "Rücklauf/HL" auch bei Short (sollte LH heißen, Zeile 576).

**Entscheidung nach E5 (vorab von Levi akzeptierte Regel "sonst automatischer Rückbau ohne weitere Diskussion"):** `git checkout main` durchgeführt (Arbeitsbaum stand clean, nur eine untracked Scratch-Datei). Branch `kurzblock-bewaehrung` bleibt als Referenz/Ausgangspunkt für die Fable-Reparatur am 24.09. **PFLICHT-ERINNERUNG unten ist damit erledigt (Rückbau-Fall), kein Merge nötig.**

**Fable-Reparaturliste für 24.09.2026** (danach neuer Bewährungstag ansetzen): (1) Dauer-Warnungen, die sich seit dem Vorlauf nicht geändert haben, automatisch zu einer Kurzform verdichten (Skript-generiert, nicht Sonnet von Hand) und die Vollzeile nur beim ersten Auftreten/bei Änderung zeigen; (2) Längen-Obergrenze pro ⚠-Zeile (~160 Zeichen + Hash-Verweis ins Archiv); (3) 8d-VIX-Lücke an der Wurzel beheben (VIX-Eingabe als Pflichtfeld im Loop-Ablauf); (4) Sonnet-Regel schärfen: Kurzblock unverändert als Codeblock, kein angehängter Freitext im selben Block; (5) die 5 Nebenfunde oben (negative Referenzkerzen-Alter als ✗, LH/HL richtungsabhängig, X6c-Begründung für ausgefallene VC-Nummern korrigieren, Über-Polling-Fehlalarm entschärfen).

---

## Nachtrag 23.09.2026 (vormittags): UMGESETZT + FREIGEGEBEN (Ausgangslage vor dem Bewährungstag)

Opus hat ein Konzept entwickelt ("Trennung Beleg vs. Anzeige"): der volle Pflichtzeilen-Block bleibt technisch unverändert und wird bei jedem Lauf vollständig in ein Tagesarchiv (`scripts/loop_archiv/<Datum>.txt`) geschrieben; zusätzlich erzeugt das Skript selbst einen kompakten Kurzblock (~8 Zeilen statt ~9.500 Zeichen) für den Chat. Bei jeder Abweichung/Handlungsnotwendigkeit eskaliert das Skript fail-safe die betroffene Vollzeile automatisch mit. Im Trigger-Moment gibt es (nach Levis begründetem Einwand gegen einen automatischen Vollblock) stattdessen einen kompakten **Entry-Kasten** direkt aus `gate_check.cjs` (Status/Entry/SL/TP1/TP2/Größe/Q-Score/Halbierung).

Fable hat umgesetzt (3 Runden: Erstumsetzung, Auflagen A1-A5 aus Gegencheck 1, Auflagen B1-B4 aus Gegencheck 2). Zwei unabhängige frische Opus-Gegenchecks live durchgeführt (Code lesen + selbst ausführen, nicht nur Bericht glauben) — Runde 1: FREIGEGEBEN MIT AUFLAGEN (A1 Exit-Code-Bug bei Komma-Eingabe, A2 fehlendes Zeitfenster im Entry-Kasten — beide Pflicht, behoben). Runde 2: FREIGEGEBEN MIT AUFLAGEN (B1 Widerspruch "Zeitfenster frei" während Order-Sperre — Pflicht, behoben). Runde 3 (gezielter Nachcheck): **FREIGEGEBEN**, Kurzblock-Format bereit für den ersten Testtag. `npm test` 354/354 grün, Golden-Regressionstest bestätigt: die alte Pflichtzeilen-Logik ist byte-identisch unverändert.

**Committet 23.09.2026:** Auf separatem Branch `kurzblock-bewaehrung` (Commit `c0de57b`, gepusht auf `origin`), nach Opus-Empfehlung (Denkfehler "nicht committen, um zurückzukönnen" — Versionskontrolle funktioniert umgekehrt, Commit ist die Rückfalloption). **Der lokale Arbeitsbaum steht seither auf diesem Branch, nicht mehr auf `main`** — bei der nächsten Session-Start-Routine (Schritt 0) prüfen, welcher Branch ausgecheckt ist. Rückbau bei Misserfolg: `git checkout main` (altes Format sofort wieder aktiv, Branch bleibt als Referenz). Bei Erfolg: Merge nach `main`.

**Bewährungstag (E5, von Levi akzeptiert):** Der erste echte Testtag mit dem neuen Format entscheidet — erfüllt er die 5 vorab festgelegten Kriterien (Lesestoff ≤8 Zeilen/1.200 Zeichen Kurzblock, 100% Archiv-Vollständigkeit, 0 verpasste Eskalationen, ≥95% wörtliche Übernahme im Transkript, Format-Zeilen-Logik byte-identisch), bleibt es dabei — sonst automatischer Rückbau ohne weitere Diskussion.

**Bewährungstag ist HEUTE, 23.09.2026** (Levi-Tagesplan: 15:10 Start-Update, 15:30-20:00 Testtag, danach Opus-Analyse mit Bericht; Fixes durch Fable erst am Folgetag 24.09.2026). Läuft auf dem Branch `kurzblock-bewaehrung` — das ist derselbe Testtag, der über die 5 Erfolgskriterien entscheidet.

**PFLICHT-ERINNERUNG (Levi-Auftrag 23.09.2026, nach Abschluss des heutigen Testtags sofort prüfen — bevor mit etwas anderem weitergemacht wird):** Nach dem ersten Testtag auf `kurzblock-bewaehrung` die 5 Kriterien auswerten (aus `scripts/loop_archiv/`, `vollcheck_log.jsonl`, Transkript). **Bei Erfolg: `git checkout main && git merge kurzblock-bewaehrung && git push` NICHT vergessen** — sonst bleibt das neue Format dauerhaft nur auf dem Feature-Branch hängen und der nächste Handelstag liefe wieder auf dem alten `main`-Stand, ohne dass jemand es bemerkt (der Arbeitsbaum-Branch wird beim Session-Start ja geprüft, aber das ersetzt nicht den Merge-Schritt). Bei Misserfolg: `git checkout main` reicht, Branch bleibt als Referenz stehen, Ergebnis hier nachtragen.

**Why:** Levi hat explizit erkannt, dass ein erfolgreicher Bewährungstag allein nichts nützt, wenn niemand aktiv den Merge nach `main` auslöst — das ist ein eigener, leicht vergessbarer Schritt nach der reinen Kriterien-Auswertung.
**How to apply:** Diese Zeile gilt als offen, bis entweder der Merge nachweislich passiert ist (Commit-Hash hier nachtragen) oder der Rückbau dokumentiert wurde. Bei jedem Session-Start, solange diese Datei/Zeile noch "offen" im MEMORY.md-Index steht, aktiv nachfragen bzw. selbst prüfen, ob der Testtag inzwischen stattgefunden hat.

**Why:** Zeigt, dass sich ein Zielkonflikt (weniger Lesestoff vs. volle Regel-Durchsetzung) technisch auflösen lässt, wenn die Anzeige vom Skript selbst erzeugt wird statt von Sonnet von Hand verdichtet — und dass der Autor≠Prüfer-Gegencheck bei einem großen Umbau auch reale, sicherheitsrelevante Bugs fängt (A1 hätte live den Exit-Code eines Gate-Laufs verfälschen können).
**How to apply:** Vor dem nächsten Live-Testtag: Code committen (Levi entscheidet). Nach dem ersten Testtag mit neuem Format: Bewährungstag-Kriterien auswerten (Sonnet/Opus), Ergebnis hier nachtragen.

---

# TODO: Ausgabeformat 1-Min-/5-Min-Ticks nutzerfreundlicher gestalten

**Levis Wunsch, wörtlich sinngemäß (16.09.2026):** Er möchte optisch bei den 1-Min- und 5-Min-Ticks etwas verändern, damit es für ihn als Mensch nutzerfreundlicher zur Ausführung ist. Er braucht für sich persönlich nur die Kerndaten:

- Marktveränderung im Chart, Richtung short/long (inkl. Indikatoren)
- Info, was erreicht werden muss (damit ein Setup gültig wird / Entry ausgelöst wird)
- Wann ein Entry für long oder short vorliegt
- Alle 10 Min: die Tweet-Zusammenfassung

**Der Rest der heutigen Pflicht-Ausgabezeilen** (Register-Check, SL-Anker-Vorprüfung-Details, Zählstände, Zeitbox-Mechanik, MTF-Frische-Zeile, etc.) soll seiner Einschätzung nach weiterhin vom System verarbeitet/durchgesetzt werden — er muss es aber nicht jedes Mal lesen/sehen.

**Ausdrücklicher Auftrag:** Nur notieren, NICHT jetzt umsetzen oder entscheiden. Soll morgen (2026-09-17) in Ruhe gemeinsam mit Opus (Konzept/Abwägung, was wirklich reduzierbar ist ohne Regel-Durchsetzung zu verlieren) und Fable (Umsetzung) besprochen werden.

**Vermutlich betroffen (zur Einordnung, nicht geprüft):** [[feedback_vollcheck_format]] (aktuelle Vorgabe: Fließtext mit ✓/✗ pro Punkt, keine Tabelle), der komplette Pflichtzeilen-Katalog in [[feedback_live_trading]] Abschnitt 9 (Voll-Check-Rhythmus) und die Quick-Tick-Definition. Wichtig für die morgige Besprechung: viele der "unnötigen" Zeilen sind Pflichtzeilen aus früheren Opus-Auflagen (z. B. Formatvorlagen, die laut [[project_memory_gesamtbericht_2026-09-14]] Kategorie 4 als "NICHT anfassen" gelten, weil jede im Output gegengeprüft wird) — eine reine Kürzung der ANZEIGE für Levi ist etwas anderes als eine Kürzung der zugrunde liegenden REGEL/Prüfung, die im Hintergrund weiterlaufen muss. Diese Unterscheidung dürfte der Kern der morgigen Diskussion sein.
