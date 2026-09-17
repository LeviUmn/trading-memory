---
name: project-cdp-tab-fixes-committet-2026-09-17
description: "CDP-Tab-Bug-Fixes VOLLSTAENDIG ABGESCHLOSSEN 17.09.2026: ff1dd3b (Self-Heal/Session-Leak/Cooldown/rAF) + f81d818 (Restpunkte A2-A5 + replay_stop-Root-Cause-Fix), je 2x unabhaengiger Opus-Gegencheck FREIGEGEBEN MIT AUFLAGEN. Nur noch 4 kleine Folgepunkte offen."
metadata:
  type: project
  originSessionId: session_2026-09-17
  modified: 2026-09-17T10:15:27.666Z
---

## Was committet wurde (2 Commits, 17.09.2026)

**ff1dd3b:** Self-Heal bei manuellem Tab-Wechsel (`document.visibilityState`-Check in `getClient()`), Session-Leak-Fix (Serialisierung aller Pfade zu `connect()`), Cooldown-Fix (nur bei echtem Fallback armiert), rAF-Timing (Visibility-Probe-Deadline 800ms→1500ms — live gemessen: alte Deadline scheiterte bei fehlendem Fenster-Fokus in ~50% der Faelle, der 16.09.-Fix funktionierte bisher nur durch Glueck der Tab-Reihenfolge). `newTab()`/`switchTab()`/`closeTab()` melden ehrliches `success`. 31 Tests.

**f81d818 (Nachtrag):** Regressionstests fuer 2 Session-Leak-Luecken, die trotz Fix in ff1dd3b ungetestet waren (catch-Zweig-Serialisierung, halb-offene Sessions bei Enable-Fehler). Toter Parameter in `capture.js` entfernt. `npm test` laeuft jetzt nur noch 6 Offline-Suiten, `e2e.test.js` ueber eigenes `test:e2e`-Script + Source-Guard direkt in der Testdatei ausgelagert (verhindert versehentlichen Live-Kontakt zur echten App). **Root-Cause-Fix des Replay-Incidents:** `hideReplayToolbar()`-Aufrufe aus `e2e.test.js` entfernt (laut Commit `6ccac64` vom 03.04.2026 bewusst aus dem Kern entfernt, weil er den Toolbar-Status dauerhaft in den TradingView-Cloud-Account synct und Replay-Steuerung permanent beschaedigen kann — Levi hat 17.09. manuell verifiziert: **Replay-Funktion geht normal, kein Account-Schaden**). `stop()` verifiziert jetzt per Polling aktiv, ob Replay wirklich beendet wurde. Nebenfund: vorbestehender Windows-Pfadbug in `sanitization.test.js` liess 34 Security-Audit-Tests seit Langem nie laufen — gefixt.

**Kette:** Fable R1 (6 Punkte) → Opus-GC1 (fand P1 Session-Leak + P2 Cooldown-Bug real) → Fable R2 (P1-P5 + rAF-Live-Untersuchung) → Opus-GC2 (freigegeben, Live-Verifikation: rAF-Fix war nicht kosmetisch) → Commit ff1dd3b → Fable R3 (A2-A5 + Incident-Root-Cause) → Opus-GC3 (freigegeben, bestaetigte hideReplayToolbar-Cloud-Sync-Behauptung per Commit-Historie, pruefte settings.local.json-Berechtigungen) → Commit f81d818. Vier unabhaengige Opus-Gegenchecks insgesamt fuer diesen einen Bugfix-Strang — [[feedback_modellwahl_trading]] Rollenteilung hat durchgehend echte Defekte gefunden, die sonst durchgerutscht waeren.

**Nebenvorfall waehrend Fable R3:** `npm test` (vor dem Fix) hat versehentlich Replay auf Levis Live-Chart ausgeloest, TradingView musste neugestartet werden. Details + Lehre: [[feedback_npm_test_destruktiv_incident_2026-09-17]].

**Why:** Zeigt den Wert des Autor≠Pruefer-Musters sehr konkret — 4 unabhaengige Runden fanden jedes Mal noch echte Luecken (Session-Leak, Cooldown-Bug, rAF-Timing-Wirkung nur 50%, ungetestete Session-Leak-Klassen, Incident-Root-Cause).

## Offene Restpunkte (klein, nicht dringend)

1. Der neue E2E-Guard in `tests/e2e.test.js` ist selbst von keinem Test abgesichert (wuerde jemand ihn entfernen, faellt nichts auf)
2. `replay_stop` ruft nie `goToRealtime()` auf (nur die alte e2e-Suite tat das gepaart) — Entscheidung noetig, ob das als zweiter Heilungsschritt ergaenzt wird. Nicht live testbar ohne Replay-Risiko.
3. `.claude/settings.local.json` Zeilen 95/96 erlauben weiterhin pauschal Testlaeufe inkl. `tests/e2e.test.js` ohne Rueckfrage — durch den neuen Source-Guard entschaerft (bestaetigt: greift auch bei blankem `node --test`), aber Opus empfiehlt Defense-in-Depth: beide Zeilen loeschen. Levis Entscheidung, nicht automatisch umgesetzt.
4. `scripts/last_sl_vorpruefung.txt` & 9 weitere `.txt`/`.jsonl`-Laufzeitartefakte sind nicht in `.gitignore` (nur die `.json`-Zwillinge sind es) — bisher bewusst nicht mitcommittet, aber `.gitignore` sollte irgendwann erweitert werden, sonst landet Trading-Loop-Zustand versehentlich im Repo bei einem `git add -A`.

**How to apply:** Kein dringender Handlungsbedarf. Bei naechster Gelegenheit/Leerlauf als kleinen Fable-Auftrag ohne vollen Gegencheck-Zyklus erledigen.
