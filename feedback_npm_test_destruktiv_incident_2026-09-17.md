---
name: feedback-npm-test-destruktiv-incident-2026-09-17
description: "17.09.2026: npm test loeste versehentlich Replay auf Levis Live-Chart aus (e2e.test.js testete destruktiv gegen die echte App), TradingView musste neustarten. Root Cause (hideReplayToolbar) gefunden+behoben, e2e.test.js vom Default-npm-test entkoppelt (Commit f81d818). BEHOBEN."
metadata:
  type: feedback
  originSessionId: session_2026-09-17
  modified: 2026-09-17T10:15:46.495Z
---

## Regel: `npm test` NICHT ungeprüft ausführen, solange `e2e.test.js` im Default-Script steckt

Fable hat am 17.09.2026 im Rahmen der CDP-Tab-Bug-Fixes `npm test` laufen lassen. Das `test`-Script schliesst `tests/e2e.test.js` ein, das gegen die ECHTE, laufende TradingView-Instanz testet (nicht gegen Mocks) — inkl. einer Replay-Suite (`selectFirstAvailableDate`, `doStep`, `toggleAutoplay`, `buy`), Symbol-Wechsel (`setSymbol('AAPL')`), Zeichnungen (`createShape`), Pine-Save (Ctrl+S). Der Lauf hat Replay-Modus auf Levis echtem Chart-Tab gestartet und dabei die Kursserie auf 2021 gesetzt. `stopReplay()`/`goToRealtime()`/`leaveReplay()` liefen alle `success:true`, aber `is_replay_started` blieb `true` — der Replay-Zustand liess sich programmatisch NICHT beenden. Levi musste TradingView manuell komplett neu starten.

Ein unabhaengiger Opus-Gegencheck hat das nachtraeglich eingeordnet: der fehlschlagende `replay_stop`-Test in der e2e-Suite ist NICHT ein unabhaengiger, hinnehmbarer Testfehler, sondern IST der Incident — ein echter, ungeloester Produktfehler ("Replay laesst sich nicht sauber verlassen"), der eigenstaendig behoben werden muss, nicht nur kosmetisch abgehakt werden darf.

**Why:** Fuer ein Live-Trading-Werkzeug ist es ein Fussangel-Design, dass der Standard-Testbefehl destruktiv gegen die echte, gerade von Levi genutzte App laeuft. Jeder kuenftige Pruef-/Fix-Durchgang, der reflexartig `npm test` tippt, kann den Chart wieder kapern — insbesondere wenn parallel dazu (wie hier) ein CDP-Self-Heal-Mechanismus geaendert wird, der Tab-Bindungen zwischen `replay_start` und `replay_stop` verschieben koennte.

## STAND 17.09.2026 abends: BEHOBEN (Commit f81d818)

`e2e.test.js` ist aus dem Default-`npm test` herausgeloest (nur noch 6 Offline-Suiten, 282 Tests), zusaetzlich ein Source-Guard DIREKT in `tests/e2e.test.js` (skip ausser bei explizitem `TV_E2E_CONFIRM=1` oder `npm run test:e2e`) — greift nachweislich auch bei einem blanken `node --test tests/`, unabhaengig vom package.json-Script. Root Cause des Incidents gefunden und gefixt: `hideReplayToolbar()`-Aufrufe in `e2e.test.js` (Funktion synct Toolbar-Status dauerhaft in den TradingView-Cloud-Account, absichtlich aus dem Kern entfernt seit Commit `6ccac64`) entfernt, `stop()` verifiziert jetzt aktiv per Polling. Levi hat 17.09. manuell verifiziert: Replay-Funktion geht normal, kein Account-Schaden entstanden.

**Weiterhin offen (klein):** `.claude/settings.local.json` Zeilen 95/96 erlauben weiterhin pauschal Testlaeufe inkl. `tests/e2e.test.js` ohne Rueckfrage — durch den Guard entschaerft, aber nicht entfernt (Levis Entscheidung). `replay_stop` ruft weiterhin nie `goToRealtime()` auf, nur Erkennung (`stop_not_confirmed`), keine automatische Heilung. Details: [[project_cdp_tab_fixes_committet_2026-09-17]].

**How to apply (weiterhin gueltig als generelle Regel, nicht nur fuer diesen Vorfall):**
1. Vor jedem `npm run test:e2e` oder direktem Aufruf von `tests/e2e.test.js` (auch durch Subagenten) IMMER Levi vorwarnen — das testet weiterhin destruktiv gegen die echte laufende App (Replay, Symbol-Wechsel, Zeichnen, Pine-Save), der Guard verhindert nur den versehentlichen/beilaeufigen Fall.
2. Normales `npm test` ist seit f81d818 wieder gefahrlos (nur Mock-Suiten).
