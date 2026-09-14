---
name: project_gegencheck_fable_umsetzung_2026-09-09_runde3
description: "Opus-Gegencheck Runde 3 (09.09.2026): C1-C6/B9-B11 alle live bestätigt korrekt (53+29 A3-Felder einzeln durchgetestet). Trotzdem EINGESCHRÄNKT — D5 [blockierend, Regression aus dem B9-Fix]: der C3-Register-Touch-Ablauf läuft in eine Endlosschleife (HART-Meldung wiederholt sich), weil vollcheck.cjs beim erneuten Aufruf denselben --jetzt-Wert wiederverwendet statt die Zeit neu zu lesen. D3 [wichtig]: --grund-richtung stürzt mit Stacktrace ab. D1 [wichtig]: SL-Vorprüfungs-UNTAUGLICH-Urteil (Normalfall, nicht Ausnahme) flutet die 2/2-ohne-7b1-Statistik und den Trigger-Kandidaten-Zähler mit Doppelzählungen. Plus zwei Template-Ungenauigkeiten (C3-Ablauf unvollständig beschrieben, --register-touch-Beschreibung irreführend)."
metadata: 
  node_type: memory
  type: project
  status: open
  modified: 2026-09-09T11:48:21.236Z
  originSessionId: f86d6469-f4c5-46be-9c92-a1637d9a3ef6
---

## Kurzfazit

Dritte Gegencheck-Runde: Opus hat C1-C6, B9-B11 und den `--grund-stale-n`-Nachfund aus [[project_gegencheck_fable_umsetzung_2026-09-09_runde2]] geprüft — **alle vollständig bestätigt**, keine Fable-Behauptung widerlegt. Opus hat dabei systematisch ALLE 53 A3-Felder von `vollcheck.cjs` und alle 29 von `position_tick.cjs` einzeln mit `--grund-<feld>` durchgetestet: "vollständig neben LUECKE" ist nicht mehr konstruierbar (C2 vollständig geschlossen). B11 aus Runde 1 war ein Fehlbefund (kein echter Defekt in `add_trade.cjs`).

**Trotzdem erneut EINGESCHRÄNKT** — fünf neue Befunde, einer davon eine Regression, die durch den B9-Fix selbst entstanden ist.

## Neue Defekte

**D5 [blockierend, Regression aus B9]:** Der B9-Fix (`--jetzt` an `register_check.cjs` durchreichen) bricht den gerade erst reparierten C3-Ablauf. `register_touch.cjs` stempelt `updated` weiterhin mit der ECHTEN `Date.now()`, aber der zweite `vollcheck.cjs`-Aufruf nach einem Touch nutzt (wenn man dasselbe `--jetzt` wiederverwendet, was naheliegt) einen jetzt in der Vergangenheit liegenden Zeitanker — der frische `updated`-Stempel liegt dann "in der Zukunft" relativ zu diesem alten `--jetzt`, und `register_constants.cjs` stuft das als HART ein (>1 Min Zukunftsstempel = korrupt). Ergebnis: Touch ausführen → derselbe HART-Fehler mit identischer Handlungsanweisung → Endlosschleife. Mit frisch gelesenem `--jetzt` beim zweiten Aufruf läuft es sofort durch (von Opus verifiziert) — aber weder das Template noch die Fehlermeldung sagen das. Tritt an einem Testtag regelmäßig auf: bei jedem Register-Refresh (alle ~60 Min) und beim ersten Voll-Check nach einem Register vom Vortag.

**D3 [wichtig]:** `--grund-richtung "unklar"` lässt `vollcheck.cjs` mit einem rohen `TypeError: Cannot read properties of null (reading 'toUpperCase')` abstürzen (Zeile ~287, fehlender Null-Guard) — der einzige der 53 A3-Auswege, der so endet statt sauber Exit 1 mit Meldung zu geben. Ausgerechnet das Feld für die Setup-Richtung.

**D1 [wichtig]:** Die SL-Vorprüfung läuft seit B1 korrekt ab ≥1 Bein — sie druckt aber bei jedem eher üblichen Fall (SL-Abstand nicht winzig) "URTEIL: SL-ANKER DERZEIT UNTAUGLICH" als Normalergebnis, nicht als Ausnahme. `protokoll_bilanz.cjs` zählt JEDES Vorkommen dieser Zeile als "2/2 ohne 7b1"-Abweichung — bei 46 Voll-Checks an einem Trendtag entstehen dadurch Dutzende falsche Abweichungsmeldungen, die den einen echten Fall verrauschen. Zusätzlich: derselbe UNTAUGLICH-Fall wird gleichzeitig als `art:"sl-vorpruefung-untauglich"` UND als `art:"ein-bein"` ins Trigger-Kandidaten-Log geschrieben — Doppelzählung desselben Kandidaten (verfälscht TODO 12).

**D4 [mittel]:** Der `ein-bein`-Kandidaten-Log-Eintrag wird VOR dem Register-Block angehängt — der C3-Ablauf (Exit 1 → Touch → Neuaufruf) erzeugt dadurch zwei identische Log-Zeilen für denselben Voll-Check.

**D2 [klein]:** `position_tick.cjs` ohne `--dir` (mit `--grund-dir`) rechnet still mit `LONG` als Default-Vorzeichen — bei einer eindeutigen Short-Position zeigt der Block dann einen Buchgewinn statt eines Buchverlusts (falsches Vorzeichen), obwohl die Format-Zeile korrekt kippt. Dieselbe Fehlerklasse wie die ursprüngliche P5/TODO-10-Bilanzzahl-Problematik.

## Template-Ungenauigkeiten (in feedback_vollcheck_format.md)

1. Die C3-Ablaufbeschreibung ("Exit 1/2 → register_touch.cjs → vollcheck.cjs erneut aufrufen") ist unvollständig — führt wegen D5 nur zum Ziel, wenn beim Neuaufruf `--jetzt` frisch gelesen wird, nicht wiederverwendet.
2. Die Beschreibung "`--register-touch` ist jetzt nur noch optionale Anzeige" ist irreführend: ohne passenden Log-Eintrag KIPPT es die Format-Zeile (MIT LÜCKEN), bei Exit 1/2 zusammen mit einer Behauptung löst es sogar Hard-Exit 1 aus (Widerspruchsprüfung). Besser: "weglassen; nur angeben, wenn der Touch tatsächlich in diesem Slot gelaufen ist."
3. Zwei zusätzliche `.gitignore`-Zeilen für `last_vollcheck.txt`/`last_position_tick.txt` (falls Datei-Redirect genutzt wird) fehlen weiterhin — B7 hat nur die drei anderen Statusdateien ergänzt.

## Gesamturteil

`EINGESCHRÄNKT` — nicht wegen der beauftragten Fixes (vollständig und korrekt), sondern wegen D5/D3/D1. Fail-safe bleibt gewahrt: kein Befund erschleicht eine Freigabe.

**Trockenlauf ist inhaltlich schon möglich** — Opus hat Template A und B wörtlich ausgeführt, beide Exit 0 und "vollständig" (bei frischem Register <60 Min). Aber: solange D5 offen ist, ist ein Trockenlauf, der den Register-Refresh-Ablauf einschließt, nicht repräsentativ für einen echten Testtag (dort ist ein Register vom Vortag der Normalfall, nicht die Ausnahme).

**Empfohlene Reihenfolge:**
1. Fable: D5 + D3 fixen (beide wenige Zeilen laut Opus)
2. Trockenlauf: ein Voll-Check + ein Positions-Tick, wörtlich aus Template A/B, inkl. Register-Refresh-Fall
3. Fable: D1, D4, D2 fixen, plus die drei Template-Korrekturen (kann auch parallel zu 2 laufen, da nicht blockierend)
4. Erst danach nächster Testtag

Nichts committet, keine Memory-Dateien von Fable/Opus geändert (die Template-Korrekturen aus diesem Bericht sind noch vom Hauptagenten nachzutragen).
