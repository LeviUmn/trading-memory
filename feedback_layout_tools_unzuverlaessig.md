---
name: feedback-layout-tools-unzuverlaessig
description: "TradingView-Layout-Tools (layout_switch, 'Neues Layout erstellen' mit 'In neuem Tab öffnen') verhalten sich nicht wie erwartet — beim DAX-Layout-Aufbau 07.08.2026 fast das live NAS100-Layout überschrieben"
metadata:
  node_type: memory
  type: feedback
  originSessionId: session-2026-08-07
  modified: 2026-08-07T10:53:51.582Z
---

Beim Aufbau des DAX-Chart-Layouts (07.08.2026, [[project_dax_erweiterung]] To-do 10) zwei technische Fallen im TradingView-MCP-Toolset entdeckt, die zu einem Beinahe-Vorfall führten (das live NAS100+QQQ-Setup wäre fast versehentlich mit DAX-Inhalt überschrieben worden — rechtzeitig erkannt und korrigiert, kein Datenverlust).

**Why:** Der Vorfall entstand nicht durch einen einzelnen Fehler, sondern durch zwei sich verstärkende technische Eigenheiten, die beide nicht offensichtlich waren.

**Falle 1 — "Neues Layout erstellen" mit "In neuem Tab öffnen" wechselt den CDP-Kontext nicht zuverlässig.** Trotz aktiviertem Häkchen blieben nachfolgende Tool-Calls (`chart_set_symbol`, `chart_manage_indicator`, `pane_focus` etc.) auf dem ALTEN, ursprünglichen Tab/Layout hängen, statt auf das neu erstellte Layout zu wirken — `tab_list` zeigte durchgehend nur einen einzigen Tab. Ergebnis: Die neuen Symbol-/Indikator-Änderungen landeten im alten Layout (das zu dem Zeitpunkt "NAS100" hieß), nicht im neuen "DAX"-Layout, das im Hintergrund unberührt blieb (und stattdessen einen TradingView-Standard-Chart wie AAPL/1M enthielt).

**Falle 2 — `layout_switch` (internal_api) meldet "success", ohne den sichtbaren Chart-Inhalt tatsächlich zu wechseln.** Mehrfach getestet: Aufruf gab `{success: true, action: "switched"}` zurück, aber `chart_get_state`/`pane_list`/Screenshot direkt danach zeigten weiterhin den vorherigen Inhalt. Erst ein ECHTER UI-Klick (`ui_click` auf den Layout-Namen in der "Kürzlich verwendet"-Liste im "Layouts verwalten"-Menü) hat zuverlässig umgeschaltet — inklusive eines echten "Vorher speichern?"-Dialogs, wenn ungespeicherte Änderungen vorlagen.

**How to apply:** Bei künftiger Layout-Arbeit (mehrere gespeicherte Layouts anlegen/wechseln):
1. Nach JEDER Layout-Erstellung/jedem Wechsel per Screenshot den Layout-Namen oben rechts im Header verifizieren, BEVOR weitere Änderungen gemacht werden — nicht auf die Tool-Response allein verlassen.
2. Für echtes Wechseln zwischen Layouts `ui_click` auf den Eintrag in der "Kürzlich verwendet"-Liste (`save-load-menu` → Eintrag anklicken) verwenden, nicht `layout_switch`.
3. Vor jedem Speichern (`Ctrl+S` bzw. `ui_keyboard`) den aktuell angezeigten Layout-Namen gegenprüfen — ein Speichern unter dem falschen Namen überschreibt das falsche Layout permanent.
4. Bei mehreren parallelen Live-Setups (wie NAS100 + DAX) ist "Automatisch speichern" ein zusätzliches Risiko, weil Änderungen auch ohne expliziten Save-Klick committen können, sobald TradingView den Auto-Save-Mechanismus (nicht zuverlässig vorhersehbar wann) auslöst — im Zweifel nach jedem größeren Änderungsschritt aktiv per Screenshot kontrollieren, welches Layout gerade aktiv/unsaved ist.

**Kontext:** Vor diesem Vorfall existierte nur EIN gespeichertes Layout ("Nicht benannt"), das NICHT dem live laufenden NAS100+QQQ-Setup entsprach — das laufende Setup war nur ein nicht explizit benannter Sitzungszustand. Im Zug dieser Session wurde das korrigiert: "NAS100" und "DAX" sind jetzt beide als benannte, verifizierte, getrennte Layouts gespeichert (siehe [[project_dax_erweiterung]] To-do 10, erledigt 07.08.2026).
