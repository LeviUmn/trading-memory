---
name: project_gegencheck_v1_v7_runde6_2026-09-16
description: "Opus-Gegencheck Runde 6 (16.09.2026), Nachpruefung der Runde-5-Auflagen 1-3 (B-Rest-Regression, N4-Doku, N6-Formulierung). FREIGEGEBEN, Diff commit-reif -- kein Blocker mehr. Auflage 1 (die echte Regression aus Runde 5) per gezielter Mutation scharf geprueft: M1 (Notfallpfad-Fix zurueckgebaut) macht den neuen Test rot und reproduziert exakt den Runde-5-Fund (leeres Log, nur Marker), M2 (beide Marker-Varianten identisch gemacht) ebenfalls rot -- Test ist also scharf, nicht nur zufaellig gruen. 7 eigene CLI-Gegenproben (1-Zeile 2500/5000 Zeichen, Runde-4-Hauptfall 6x400 Zeichen, leerer Kopf, letzte-Zeile-allein-ueber-Budget, knapp-ueber-Budget, teilweise-passender-Kopf) zeigen: Budget nie ueberschritten, IMMER sinnvoller Inhalt geloggt (der leere String aus Runde 5 ist in keiner Konstellation mehr reproduzierbar), die zwei Marker vertauschen sich nie, N stimmt in 7/7 Faellen, Runde-4-Hauptfall verhaelt sich unveraendert (keine Nebenwirkung des Notfallpfads), verwaistes Trennzeichen-Praefix ist weg. Auflage 2 bestaetigt reine Doku (kein Code-Diff in dem Hunk), sachlich korrekt gegen den Code gelesen. Auflage 3 per 4 echten CLI-Laeufen bestaetigt (fiktiv+FAIL zeigt Hinweis, echt/kein-Modus/UNKNOWN zeigen ihn nicht). 104/104 Tests, 3x stabil, keine Nebenwirkung auf Runde 4 (C1-Tests weiterhin gruen). Sechs NICHT blockierende Restpunkte fuer eine spaetere Runde notiert, wichtigster: Auflage 3 und der N7-Marker sind von KEINEM Test abgesichert -- ein Rueckfall auf den kritisierten Runde-5-Zustand bliebe unbemerkt (Mutationsbeleg: Suite bleibt 104/104 gruen, wenn man die Auflage-3-Bedingung zuruecksetzt)."
metadata:
  node_type: memory
  type: project
  status: "FREIGEGEBEN 16.09.2026, commit-reif. 6 nicht blockierende Restpunkte fuer spaetere Runde (v.a. fehlende Testabsicherung fuer Auflage 3/N7). Diff weiterhin unkommittiert (Levi)."
  originSessionId: session_current
  modified: 2026-09-16T12:03:13.397Z
---

# Opus-Gegencheck Runde 6: Nachprüfung Auflage 1-3 aus Runde 5 (16.09.2026)

**Prüfling:** `git diff 79573f5` (Endzustand), `scripts/gate_check.cjs`, `scripts/vollcheck.cjs`, `tests/trading_scripts.test.js`, +71/−7. **Methode:** eigener Testlauf (3×), `npm run test:all`, zwei gezielte Mutationsproben (M1/M2) gegen den neuen Test, 7 eigene CLI-Gegenproben in Sandbox-Kopie, 4 CLI-Läufe für Auflage 3. Bezug: [[project_gegencheck_v1_v7_runde5_2026-09-16]] (fand die Regression).

## Gesamturteil: FREIGEGEBEN, Diff commit-reif

**Tests:** 104/104, dreimal stabil (+1 neuer Test gegenüber Runde 5). `npm run test:all`: 134/135, nur `e2e.test.js` (umgebungsbedingt).

## Auflage 1 (B-Rest-Regression) — BESTÄTIGT, scharf per Mutation geprüft

**M1** (Notfallpfad-Fix zurückgebaut, Runde-5-Zustand rekonstruiert, exakt 2 Zeilen Diff): neuer Test wird **rot**, Fehlermeldung reproduziert Runde 5s Fund wortwörtlich (Log = nur Marker, kein Zeichen des Fehlers). **M2** (beide Marker-Varianten identisch gemacht): ebenfalls rot — der Test pinnt auch die Unterscheidbarkeit. Der Test ist also nachweislich scharf, nicht nur zufällig grün.

7 eigene CLI-Gegenproben (Tabelle im Volltext): 1-Zeile 2500/5000 Zeichen, Runde-4-Hauptfall (6×400 Zeichen), leerer Kopf, letzte-Zeile-allein-über-Budget, knapp-über-Budget, teilweise-passender-Kopf. Ergebnis durchgehend: Budget (2000) nie überschritten, in jeder Konstellation wird sinnvoller Inhalt geloggt — der leere String aus Runde 5 ist nicht mehr reproduzierbar. Die beiden Marker ("an Zeilengrenzen" vs. "mitten in der Zeile") erscheinen jeweils im richtigen Fall und vertauschen sich nie. N (Gesamtlänge im Marker) stimmt in 7/7 Fällen. Der Runde-4-Hauptfall verhält sich unverändert (keine Nebenwirkung des neuen Notfallpfads). Das verwaiste `" | … | "`-Präfix bei leerem Kopf ist weg. **Fazit: der neue Stand ist sogar besser als der bereits gepushte `79573f5`** (der zeigte die ersten 2000 Zeichen ohne jeden Kappungs-Hinweis; jetzt 1949 Zeichen + expliziter Marker mit Gesamtlänge).

## Auflage 2 (N4-Doku) — BESTÄTIGT

Der betreffende Hunk besteht ausschließlich aus Kommentarzeilen, kein Code-Verhalten geändert. Sachlich gegen den Code gelesen: alle vier Aussagen (C4-Geltungsbereich, Gate-Modus-Lücke, A1, A2) stimmen mit den tatsächlichen Codezeilen überein. Die Verhaltensfrage bleibt wie gewünscht offen und bei Levi.

## Auflage 3 (N6-Formulierung) — BESTÄTIGT, 4/4 Fälle live geprüft

Vier echte CLI-Läufe: (a) Dry-Run, fiktiv+FAIL → Hinweis erscheint (korrekt, V3 hätte gegriffen — Gegenprobe: derselbe Lauf live schreibt tatsächlich eine Zeile). (b1) kein `testtag_modus` → kein Hinweis. (b2) `testtag_modus: echt` → kein Hinweis. (c) fiktiv+UNKNOWN → kein Hinweis (V3 greift nur bei FAIL). Alle vier wie erwartet. Der N7-Marker an der AUSSICHTSLOS-Zeile wurde im selben Block mitgeprüft und ist ebenfalls wahrheitsgemäß (erscheint nur im Dry-Run, konsistent mit A2).

## Regressionscheck — BESTÄTIGT

3× Volllauf stabil, `cli.test.js`+`pine_analyze.test.js` 29/29, keine Nebenwirkung auf Runde 4 (beide C1-Tests weiterhin grün, Runde-4-Hauptfall bleibt im "an Zeilengrenzen"-Pfad). Repo nach allen Gegenproben unverändert.

## Sechs nicht blockierende Restpunkte für eine spätere Runde

1. **Wichtigster Punkt:** Auflage 3 (N6-Formulierung) und der N7-Marker sind von keinem Test abgesichert — per Mutation belegt: setzt man die Auflage-3-Bedingung auf den kritisierten Runde-5-Zustand zurück, bleibt die Suite weiterhin 104/104 grün. Ein Rückfall bliebe unbemerkt. Empfehlung: 3 Assertions im bestehenden C1-Test ergänzen (fiktiv+FAIL zeigt Suffix, echt+FAIL und fiktiv+UNKNOWN nicht).
2. Der Mehrzeilen-Pfad ("an Zeilengrenzen", Runde-4-Hauptfall) hat weiterhin keinen eigenen Test — funktioniert nachweislich korrekt, aber ungeschützt.
3. N im Marker ist nicht gepinnt (Mutation mit `N = BUDGET` statt echter Länge bleibt grün, weil die Test-Regex nur `\d+` prüft).
4. Kosmetik: Budget-Unterausnutzung bei nicht passendem Kopf (kein Fehler, nur konservativ).
5. Praktische Einordnung: der häufigste reale Mehrzeilen-Hard-Exit (A3-Pflichtfelder, ~40 Felder) kommt nur auf ~842 Zeichen — unter Budget. Der praktisch erreichbare Überlauf war genau der jetzt behobene Ein-Zeilen-Fall. Die Priorität der Korrektur war richtig.
6. Offene Verhaltensentscheidung (weiterhin bei Levi, nicht Fable): soll `appendTriggerKandidat()` im Gate-Modus künftig auch gegen `--dry-run` geschützt werden?

## Fazit

Diff ist commit-reif. Nicht mitcommitten: die untracked `scripts/last_*.txt`/`sl_anker_wechsel_log.jsonl` (Laufzeit-Artefakte).

Vorgänger: [[project_gegencheck_v1_v7_runde5_2026-09-16]], [[project_gegencheck_v1_v7_runde4_2026-09-16]], [[project_gegencheck_v1_v7_runde3_2026-09-16]], [[project_gegencheck_v1_v7_runde2_2026-09-16]], [[project_gegencheck_v1_v7_fable_umsetzung_2026-09-16]], [[project_testtag_analyse_2026-09-15]] (V1-V7-Ursprung).
