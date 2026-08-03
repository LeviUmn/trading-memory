---
name: project-mac-umzug
description: "Geplanter Umzug des Trading-Setups von Windows auf Mac Mini (ca. September 2026) + mobiles Setup MacBook Pro 2024 + iPad Air — noch nicht gestartet, wartet auf aktives User-Signal"
metadata:
  node_type: memory
  type: project
  originSessionId: session-2026-08-03
  modified: 2026-08-03T15:01:34.681Z
---

Am 03.08.2026 besprochen, bewusst auf später verschoben.

**Plan:** Sobald der Mac Mini da ist (voraussichtlich September 2026), verschwindet das Windows-Trading-Setup komplett. Drei Monitore für den Mac Mini bereits gekauft. Zusätzlich existiert ein mobiles Setup (MacBook Pro 2024 + iPad Air als Zusatzmonitor), das dann parallel/simultan mit demselben Sync-Mechanismus eingerichtet wird.

**Offenes technisches Problem (noch ungelöst, erst am echten Gerät zu klären):** Der aktuelle Memory-Ordner ist an den Windows-Projektpfad gekoppelt (`C:\Users\umnus\...\C--Users-umnus-tradingview-mcp\memory\`). Unklar, ob Claude Code auf einem Mac automatisch denselben Memory-Kontext lädt (auch nach `git pull`) oder einen neuen leeren Kontext anlegt, weil sich der Pfad-Hash ändert. Gleiches Betriebssystem (Mac Mini + MacBook, beide macOS) reduziert das Risiko gegenüber Windows→Mac, löst es aber nicht zwangsläufig vollständig, da beide Geräte eigene Home-Verzeichnisse haben können.

**Why:** User will beim Geräte-Wechsel (z.B. heute auf einem Gerät getradet, morgen auf einem anderen) nie mit veraltetem Memory-Stand starten. Bisheriger GitHub-Backup ist nur Einbahnstraße (Push am Tagesende, kein automatischer Pull beim Start).

**How to apply:** NICHTS jetzt tun. Erst aktiv werden, wenn der User explizit meldet "Mac Mini ist da". Dann in dieser Reihenfolge:
1. Vollständige Neuinstallation auf dem Mac Mini begleiten — alle APIs/MCP-Server (TradingView-MCP, X/xapi-MCP, etc.) müssen neu eingerichtet werden, das wird laut User "etwas aufwändiger", explizite Hilfe zugesagt.
2. Live am echten Gerät testen, ob/wie der Memory-Kontext geladen wird (siehe offenes technisches Problem oben).
3. Je nach Testergebnis: `git pull` als expliziten ersten Schritt in [[feedback_session_update]] Schritt 1 ergänzen (Vorschlag lag schon auf dem Tisch, aber bewusst zurückgestellt bis zum echten Test).
4. Sobald Mac Mini eingerichtet ist, dasselbe Sync-Setup auch für das mobile MacBook-Pro/iPad-Air-Setup übernehmen (simultan, nicht separat).

**Trigger für Wiederaufnahme:** User-Zitat 03.08.2026: "ich gebe dir aktiv Bescheid, sobald der Mac mini da ist" — kein automatisches Wiederaufgreifen vorher.
