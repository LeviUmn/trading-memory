---
name: feedback-draw-list-getchartapi-bug
description: "draw_list warf wiederholt 'getChartApi is not defined' — Root-Cause gefunden + Fix vorgenommen (04.08., Fable), Live-Test durch Levi in aktiver Session noch ausstehend"
metadata:
  node_type: memory
  type: project
  originSessionId: session-2026-08-04
  modified: 2026-08-04T15:33:44.552Z
---

**Status: Fix vorgenommen, live-Test durch Levi noch ausstehend** (nicht GELÖST — siehe unten für den Unterschied)

Am 04.08.2026 im 1-Min-Loop (Trade-Vorbereitung, Session-Start) mehrfach reproduziert: `draw_list` liefert konsequent `{"success": false, "error": "getChartApi is not defined"}`, auch nach mehreren Minuten/mehreren Voll-Checks unverändert. `draw_shape` funktioniert dabei weiterhin normal (neue Linien wurden erfolgreich gezeichnet und mit Entity-IDs bestätigt) — nur das Auslesen der Zeichnungsliste ist betroffen.

**Kontext:** Bereits am 03.08.2026 (Trade #29) trat der gleiche Fehler auf ("alte Linien konnten wegen getChartApi-Tool-Fehler nicht sofort entfernt werden"), damals als "später gelöst" vermerkt — ist also kein einmaliger Vorfall, sondern ein durchgehender, nicht intermittierender Fehler (siehe Root-Cause unten — er trat bei jedem einzigen Aufruf auf, nicht nur manchmal).

**Praktische Auswirkung am 04.08.:** Eine alte, nicht mehr zuordenbare Chart-Linie ("PP 28.987,4", Ursprung unbekannt, keine der heute berechneten Pivot-Level) konnte nicht programmatisch identifiziert/entfernt werden — Levi musste sie manuell im Chart löschen. Workaround bis zum Fix: visueller Screenshot-Check statt `draw_list`.

## Root-Cause (gefunden 04.08.2026, Fable-Tagesabschluss)

In `src/core/drawing.js` werden `evaluate` und `getChartApi` aus `connection.js` **umbenannt** importiert: `import { evaluate as _evaluate, getChartApi as _getChartApi, ... } from '../connection.js'`. Ein Helper `_resolve(deps)` löst daraus die tatsächlich zu benutzenden lokalen Namen `evaluate`/`getChartApi` auf (mit optionalem Dependency-Injection-Override für Tests).

Von den fünf exportierten Funktionen rief **nur `drawShape` diesen Helper korrekt auf** (`const { evaluate, getChartApi } = _resolve(_deps);` als erste Zeile). Die anderen vier — `listDrawings`, `getProperties`, `removeOne`, `clearAll` — riefen stattdessen direkt die nackten, nie im Modul-Scope gebundenen Namen `getChartApi()` und `evaluate()` auf. Da der Import sie unter `_evaluate`/`_getChartApi` bindet, existierten `evaluate`/`getChartApi` als bloße Bezeichner im Modul schlicht nicht → `ReferenceError: getChartApi is not defined`, bei **jedem** Aufruf, nicht nur gelegentlich. `listDrawings()` ruft `getChartApi()` als allerersten Schritt auf, weshalb `draw_list` konsequent sofort fehlschlug — exakt das beobachtete Verhalten.

**Warum `draw_shape` nie betroffen war:** Es ist die einzige der fünf Funktionen, die den `_resolve(_deps)`-Helper tatsächlich aufruft, bevor sie `getChartApi`/`evaluate` benutzt.

## Was geändert wurde

**Datei:** `src/core/drawing.js` — die vier betroffenen Funktionen (`listDrawings`, `getProperties`, `removeOne`, `clearAll`) rufen jetzt jeweils als erste Zeile `const { evaluate, getChartApi } = _resolve(_deps);` auf, exakt wie `drawShape` es bereits vorher tat. Dafür wurde allen vier Funktionen ein optionaler `_deps`-Parameter hinzugefügt (für Test-Injection, gleiches Muster wie bei `drawShape`).

**Datei:** `tests/sanitization.test.js` — neue Test-Suite "drawing.js — getChartApi/evaluate resolution (regression for 'getChartApi is not defined')" mit 6 Tests: bestätigt für alle vier reparierten Funktionen, dass sie über `_deps` injizierte Mock-Implementierungen von `evaluate`/`getChartApi` tatsächlich aufrufen und nicht mehr crashen, plus ein Quellcode-Audit-Test, der sicherstellt, dass jede der fünf exportierten Funktionen `_resolve(_deps)` aufruft (verhindert stillschweigende Regression, falls künftig eine neue Funktion nach demselben falschen Muster ergänzt wird).

## Verifikation

Statisch: `node --test tests/sanitization.test.js` — alle 6 neuen Tests grün, keine Regression bei den bestehenden `drawShape`-Tests (34/34 relevante Tests bestanden, ein vorbestehender, unabhängiger Windows-Pfad-Bug in einem anderen Test in derselben Datei betrifft nicht diesen Fix).

Zusätzlich live verifiziert (unerwartet — es war tatsächlich eine aktive CDP-Verbindung zu Levis TradingView-Chart vorhanden): Per separatem, eigenständigem Node-Prozess (nicht der laufende MCP-Server, der noch den alten Code im Speicher hat) direkt gegen den echten CDP-Port getestet — `listDrawings()` lief fehlerfrei durch, und ein voller Round-Trip (`draw_shape` → `draw_list` fand die neue Linie → `draw_remove_one` entfernte sie sauber wieder) war erfolgreich. Chart wurde unverändert/sauber hinterlassen.

**Warum trotzdem nicht "GELÖST":** Der aktuell laufende MCP-Server-Prozess, den Levis aktive Trading-Session über die MCP-Tools nutzt, hat den alten (fehlerhaften) Code bereits beim Start in den Speicher geladen — die Code-Änderung auf der Festplatte wirkt sich dort erst nach einem Neustart des MCP-Servers/der Session aus. Die obige Live-Verifikation lief bewusst über eine komplett separate Verbindung, um die laufende Session nicht zu stören, bestätigt den Fix also nur auf Code-Ebene, nicht innerhalb von Levis tatsächlich genutztem Tool-Pfad. Erst wenn `draw_list` in einer neu gestarteten aktiven Session über die normalen MCP-Tools erfolgreich läuft, darf der Status auf GELÖST gesetzt werden.

**How to apply:** Bei der nächsten Session-Aufnahme (nach MCP-Server-Neustart) `draw_list` einmal regulär über die Tools aufrufen und bestätigen, dass kein `getChartApi`-Fehler mehr auftritt — dann diese Datei auf GELÖST setzen. Bis dahin bleibt der bisherige Workaround (visueller Screenshot-Check nach Zeichnungs-Änderungen) als Sicherheitsnetz sinnvoll.
