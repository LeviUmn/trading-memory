---
name: project-cdp-tab-fixes-fable-folgeauftrag-todo-2026-09-17
description: "TODO 17.09.2026 fuer Fable: 4 Opus-Auflagen + 2 Zusatzbefunde aus dem Gegencheck der CDP-Tab-Bug-Fixes vom 16.09. nachruesten, vor jedem Commit"
metadata: 
  node_type: memory
  type: project
  originSessionId: 4bedc910-34ac-4303-800f-158b4b77d9c8
  modified: 2026-09-16T15:07:53.819Z
---

## Ausgangslage (16.09.2026)

TradingView hatte zwei Chart-Tabs offen; der MCP-Server band sich blind an den ersten (falschen, nicht gerenderten) Tab — Folge: `capture_screenshot` hing unbegrenzt, UI-Klicks/Texteingabe liefen ins Leere. Opus hat die Ursache live diagnostiziert (`findChartTarget()` in `src/connection.js` ohne Sichtbarkeitspruefung), Fable hat 3 Fixes umgesetzt (`connection.js` Visibility-Probe, `tab.js` `resetConnection()` nach `switchTab()`, `capture.js` 10s-Timeout). Live verifiziert: funktioniert (`tv_health_check` zeigt `target_selection: "visibility_probe"`, `target_verified_rendered: true`, Screenshot lief sofort). Frischer Opus-Gegencheck: **FREIGEGEBEN MIT AUFLAGEN** — kein Scope-Creep, kein Regressionsrisiko, aber 4 offene Auflagen + 2 von Fable selbst notierte Luecken.

## TODO fuer Fable, 17.09.2026 morgens

1. **Fehlende Tests nachliefern (kritischste Auflage).** Fable hatte im Abschlussbericht "8 eigene Mock-Tests" gemeldet — `git status` zeigt aber keine neue/geaenderte Testdatei. Null Regressionsschutz fuer die 3 Fixes. Vor jedem Commit nachholen (Loader-Hook-Ansatz, der im Bericht beschrieben war, kann als Vorlage dienen: hidden-first/visible-second, Fallback+Warnung, switchTab-Rebind, Screenshot-Timeout).
2. **Grundannahme der Visibility-Probe noch nicht per echtem E2E verifiziert** (nur Mock-Ebene). Beim naechsten echten Zwei-Tab-Fall `tv_health_check` → `target_selection` gegenchecken; `fallback_first_match` statt `visibility_probe` waere ein Hinweis, dass der Fix in der Praxis doch nicht greift.
3. **`capture.js` heilt nach einem Timeout nicht selbst.** Der falsche/hängende Client bleibt gecacht (Liveness-Check `Runtime.evaluate('1')` überlebt auch auf einem Hintergrund-Tab) — jeder weitere Screenshot würde erneut die vollen 10s in den Timeout laufen. Nachruesten: nach Timeout `resetConnection()` aufrufen + einen Retry-Versuch.
4. **Lücke bei manuellem Tab-Wechsel durch den User.** Fix 1 (Visibility-Probe) wirkt nur beim initialen Verbindungsaufbau, Fix 2 (`resetConnection()`) nur wenn der Wechsel ueber das MCP-Tool `tab_switch` laeuft. Klickt Levi selbst in der TradingView-UI einen anderen Tab an, bleibt der Server-Cache auf dem alten Target haengen — das ist vermutlich exakt das heutige Ausloese-Szenario gewesen. Braucht eine eigene Lösung (z.B. periodische Visibility-Re-Probe, oder ein explizites "reconnect"-Tool).

## Von Fable selbst als offen notiert, von Opus als "beide echt" bestaetigt — eigener Folgeauftrag

5. **`src/core/batch.js:41`** hat denselben ungetimten `Page.captureScreenshot`-Call wie das urspruengliche `capture.js`-Problem — dort sogar schlimmer, weil er in einer Symbol-Schleife (`batch_run`) haengt und dadurch den kompletten Batch-Lauf unbegrenzt blockieren kann, nicht nur einen Einzelaufruf.
6. **`closeTab()`** dispatcht `Ctrl+W` per `Input.dispatchKeyEvent` an das aktuell verbundene (ggf. unsichtbare/falsche) Target und setzt den Verbindungs-Cache dabei nicht zurueck. Erklaert, warum Sonnets `tab_close`-Versuch heute wirkungslos war (`tabs_before`/`tabs_after` identisch trotz `success:true`). Analog zu Fix 2 sollte `closeTab()` nach erfolgreichem Schliessen ebenfalls `resetConnection()` aufrufen.

**Why:** Ohne diese Nachbesserungen ist der heutige Fix funktional, aber nicht regressionssicher und deckt nicht alle Ausloeser ab (v.a. Punkt 4 — der wahrscheinlichste Wiederholungsfall). [[feedback_modellwahl_trading]] (Rollenteilung Sonnet/Fable/Opus) gilt unveraendert: Fable setzt um, frischer Opus prueft danach erneut gegen, bevor committet wird.

**How to apply:** Morgen (17.09.2026) als erster Fable-Auftrag des Tages, vor jedem Commit der bisherigen 3 Fixes. Nach Umsetzung erneuter Opus-Gegencheck vor dem Commit (etablierte Kette: Opus analysiert → Fable setzt um → frischer Opus prueft).
