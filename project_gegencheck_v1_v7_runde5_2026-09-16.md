---
name: project_gegencheck_v1_v7_runde5_2026-09-16
description: "Opus-Gegencheck Runde 5 (16.09.2026) der Fable-Korrektur zu den 5 Restpunkten N4-N7+B-Rest aus [[project_gegencheck_v1_v7_runde4_2026-09-16]] (Diff gegen bereits gepushten Stand 79573f5). FREIGEGEBEN MIT AUFLAGEN -- Diff NICHT commit-reif. N5 (Test-Isolation) und N7 (Dry-Run-Marker an der V1-AUSSICHTSLOS-Zeile) sauber bestaetigt per eigener CLI-Gegenprobe/Mutationsvergleich gegen 79573f5, keine Doppelmarkierung mit dem N1-Marker, keine Live-Faelschung. ECHTE REGRESSION gefunden bei B-Rest: der von Fable zugesagte 'Notfallpfad fuer eine einzelne budgetsprengende Zeile' existiert im Code nicht wirklich -- bei genau 1 stderr-Zeile ueber 2000 Zeichen wird `out` leer und der komplette Diagnoseinhalt geht verloren (nur noch der Kappungs-Marker steht im Log), waehrend der bereits GEPUSHTE Stand 79573f5 in diesem Fall wenigstens die ersten 2000 Zeichen zeigte. Genau dieser Ein-Zeilen-Hard-Exit-Fall ist nicht exotisch (der eigene V4-Test nutzt ihn mit --richtung bogus). Zusaetzlich 2 kleinere B-Rest-Nebenbefunde (verwaistes Trennzeichen-Praefix, Wortbruch trotz 'an Zeilengrenzen'-Marker bei einer einzelnen ueberlangen letzten Zeile). N4 TEILWEISE: der neue Hinweistext ist fuer A1/A2 korrekt, aber der unveraenderte alte Halbsatz 'kein Trigger-Kandidaten-Eintrag' stimmt im Gate-Modus nicht (appendTriggerKandidat() hat keinen Dry-Run-Guard) -- vorbestehender Bug (auch in 79573f5 schon so), aber die Doku-Korrektur hat ihn nicht mitgenommen, obwohl das der Auftrag war. N6 TEILWEISE: Hinweistext erscheint bei JEDEM fehlerfreien Dry-Run, auch wenn V3 aus anderem Grund (kein fiktiver Tag, Status != FAIL) ohnehin nichts geschrieben haette -- irrefuehrende Kausalitaetsbehauptung, reine Anzeige. 104/104 Tests weiterhin gruen, keine Nebenwirkung auf V1-V7 oder Runden 1-4."
metadata:
  node_type: memory
  type: project
  status: "FREIGEGEBEN MIT AUFLAGEN 16.09.2026 -- Auflage 1 (B-Rest-Notfallpfad, echte Regression ggu. bereits gepushtem 79573f5) MUSS vor Commit behoben werden. Auflage 2 (N4-Trigger-Log-Luecke, Levi-Entscheidung ob Code oder nur Doku) + Auflage 3 (N6-Formulierung) koennen in Folge-Commit. Diff weiterhin unkommittiert."
  originSessionId: session_current
  modified: 2026-09-16T11:41:58.962Z
---

# Opus-Gegencheck Runde 5: N4-N7 + B-Rest (16.09.2026)

**Prüfling:** `git diff 79573f5` (bereits gepushter Stand!) — `scripts/gate_check.cjs`, `scripts/vollcheck.cjs`, `tests/trading_scripts.test.js`, +39/−5. **Methode:** eigener Testlauf (4×), `npm run test:all`, `git stash`-Vergleich gegen 79573f5 für jeden Einzelbefund, eigene CLI-Gegenproben. Bezug: [[project_gegencheck_v1_v7_runde4_2026-09-16]] (fand die 5 Restpunkte).

## Gesamturteil: FREIGEGEBEN MIT AUFLAGEN — Diff ist so noch NICHT commit-reif

**Tests:** 104/104, viermal identisch. `npm run test:all`: 134/135 (nur `e2e.test.js`, umgebungsbedingt).

## N5 (Test-Isolation) — BESTÄTIGT

Isoliert per `--test-name-pattern`: neuer Stand grün, alter Stand (`git stash` → 79573f5) rot — die Kopplung war real und ist jetzt weg. Keine Nebenwirkung auf Folgetests (beide beginnen wieder mit `clean()`).

## N7 (Dry-Run-Marker an V1-AUSSICHTSLOS) — BESTÄTIGT, kein Risikopfad

Entgegen der Befürchtung ist es keine blinde String-Nachbearbeitung, sondern eine gezielte Array-Element-Ersetzung mit `startsWith` (nicht `includes`) auf ein Feld, das strukturell höchstens einen AUSSICHTSLOS-Eintrag enthalten kann. Eigene Gegenproben: reiner `unsolvable`-Fall, `entryFenster.leer`-Fall, Kombination mit einer anderen Zeile die "AUSSICHTSLOS" nur im Fließtext enthält (kein Fehltreffer), Kombination mit dem N1-markierten Gate-Gedächtnis (keine Doppelmarkierung, unterschiedliche Textstellen), Live-Lauf (0 Treffer für beide Marker). Alles wie behauptet.

## N4 (Nutzungshinweis) — TEILWEISE

Der neu hinzugefügte Teil zu A1/A2 ist sachlich korrekt und live nachgemessen. **Aber:** der unveränderte alte Halbsatz "kein Trigger-Kandidaten-Eintrag" gilt nur im `--sl-vorpruefung`-Modus — im Gate-Modus schreibt `appendTriggerKandidat()` auch bei `--dry-run` ungeschützt eine Zeile nach `trigger_kandidaten_log.jsonl` (Nenner der Trigger-Statistik in `protokoll_bilanz.cjs`). Das ist ein vorbestehender Bug (auch in `79573f5` schon so, keine Regression durch diesen Diff), aber die N4-Doku-Korrektur hat genau diese Lücke nicht mitgenommen, obwohl der Auftrag lautete "der Hinweis soll die echte Wirkung nennen".

## N6 (SKIPPED-SETUP-Dry-Run-Hinweis) — TEILWEISE

Der Hinweis erscheint korrekt bei jedem fehlerfreien Dry-Run — aber auch dann, wenn V3 aus einem anderen Grund (kein fiktiver Tag, Status ≠ FAIL) ohnehin nichts geschrieben hätte. Die Formulierung "V3-Auto-Erfassung uebersprungen" behauptet dort eine Schreibwirkung, die nie stattgefunden hätte. Reine Anzeige, aber genau die Art Halbwahrheit, die N6 eigentlich beseitigen sollte.

## B-Rest (stderr-Kappung) — TEILWEISE, mit einer ECHTEN REGRESSION gegenüber dem bereits gepushten Stand

Der Hauptfall (mehrere mittellange Zeilen, Gesamtlänge >2000) funktioniert wie zugesagt: Kappung an Zeilengrenzen, kein Wortbruch, letzte Zeile (die eigentliche Abbruchmeldung) bleibt vollständig erhalten, Marker mit korrekter Zeichenzahl.

**Der zugesagte "Notfallpfad für eine einzelne budget-sprengende Zeile" existiert im Code nicht real:** Bei genau 1 stderr-Zeile über 2000 Zeichen läuft die Kopf-Schleife nie (`i < Z.length - 1` = `i < 0`), der Tail wird nicht angehängt, `out` bleibt leer — im Log steht dann **nur noch der Kappungs-Marker, kein einziges Zeichen des eigentlichen Fehlers**. Live-Vergleich:
- **Neuer Stand:** `[gekappt an Zeilengrenzen, 2670 Zeichen gesamt]` — Inhalt: nichts.
- **Bereits gepushter Stand `79573f5`:** zeigte in diesem Fall wenigstens die ersten 2000 Zeichen, inkl. des fehlerhaften Parameters.

Das ist eine **Verschlechterung gegenüber dem Stand, der schon live ist** — und der Ein-Zeilen-Hard-Exit ist kein Exotenfall: der eigene V4-Regressionstest nutzt ihn selbst (`--richtung bogus`). Für ein Log, das explizit gebaut wurde, weil am 15.09. Hard-Exit-Gründe sonst nur aus einem 74-MB-Transkript rekonstruierbar waren, ist Informationsverlust in genau diesem Fall der falsche Preis für saubere Zeilengrenzen im Normalfall.

Zwei kleinere Nebenbefunde an derselben Stelle: ein verwaistes `" | … | "`-Trennzeichen-Präfix, wenn der Kopf leer bleibt; und bei einer einzelnen sehr langen letzten Zeile wird trotzdem mitten im Wort abgeschnitten, während der Marker weiterhin "an Zeilengrenzen" behauptet (Kappungsart wird nicht unterschieden).

## Regressionscheck — BESTÄTIGT

104/104 stabil über 4 Läufe, `npm run test:all` unverändert (nur der bekannte, umgebungsbedingte e2e-Fehler). Keine Nebenwirkung auf V1-V7 oder die Ergebnisse der Runden 1-4 — insbesondere kollidiert der neue N7-Marker nicht mit dem C1-Regex aus Runde 4 (unterschiedliche Zeile, unterschiedlicher Text, im kombinierten Lauf je 1 Treffer, 0 Zeilen mit beiden Markern).

## Auflagen

**Auflage 1 (B-Rest, MUSS vor dem Commit) — Notfallpfad wirklich füllen.** Wenn `kopf` leer bleibt bzw. `out` kürzer als das verfügbare Budget ist, auf zeichenweise Kappung des gejointen Gesamtstrings zurückfallen (`full.slice(0, BUDGET - suffix.length)`), und den Marker dabei unterscheidbar machen (z. B. "gekappt mitten in der Zeile" statt "an Zeilengrenzen"), damit niemand fälschlich eine saubere Grenze annimmt. Zusätzlich das verwaiste Trennzeichen-Präfix unterdrücken, wenn der Kopf leer ist.

**Auflage 2 (N4, Levi-Entscheidung, kann in Folge-Commit) —** entweder `appendTriggerKandidat()` im Gate-Modus ebenfalls gegen `--dry-run` schützen (konsistent zu A1, ändert aber den Nenner der bestehenden Trigger-Statistik — deshalb keine Fable-Alleinentscheidung) oder den Nutzungshinweis auf die tatsächliche Wirkung korrigieren.

**Auflage 3 (N6, Anzeige, kann in Folge-Commit) —** Formulierung nur setzen, wenn V3 tatsächlich gegriffen hätte, oder neutral fassen ("keine automatische Fiktiv-Erfassung in diesem Lauf").

## Fazit

Auflage 1 ist der einzige echte Blocker — sie macht `vollcheck_log.jsonl` in einem realistischen Fall (Ein-Zeilen-Hard-Exit) schlechter als der bereits gepushte Stand. N5 und N7 sind sauber und brauchen keine Nacharbeit. Auflage 2+3 sind reine Konsistenzpunkte ohne Dringlichkeit.

Vorgänger: [[project_gegencheck_v1_v7_runde4_2026-09-16]], [[project_gegencheck_v1_v7_runde3_2026-09-16]], [[project_gegencheck_v1_v7_runde2_2026-09-16]], [[project_gegencheck_v1_v7_fable_umsetzung_2026-09-16]], [[project_testtag_analyse_2026-09-15]] (V1-V7-Ursprung).
