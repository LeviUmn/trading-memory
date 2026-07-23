---
name: reference-cli-start
description: Wie der User die echte Claude Code CLI mit TradingView-MCP-Verbindung startet
metadata: 
  node_type: memory
  type: reference
  originSessionId: 607aa8f6-9958-4c1c-9c75-4afabcffb717
---

Der User nutzt zwei verschiedene Umgebungen: eine App-Oberfläche (ohne lokale MCP-Server-Unterstützung, nur Computer-Steuerung als Fallback) und die echte Claude Code CLI im Terminal (mit vollen 78 TradingView-Tools).

**So startet der User die CLI-Version mit TradingView-Tools (Stand 22.06.2026):**
```cmd
cd C:\Users\umnus\tradingview-mcp
claude
```
Dann: `Start update dich`

**Wichtig:** `.mcp.json` muss im Projektordner selbst liegen (`C:\Users\umnus\tradingview-mcp\.mcp.json`), nicht in `~\.claude\.mcp.json` — sonst verbindet sich der MCP-Server nicht. Das war ein einmaliger Setup-Fehler, der am 22.06.2026 behoben wurde.

**Falls Probleme:** `/mcp` im CLI eingeben zeigt Verbindungsstatus. Sollte zeigen: `tradingview · connected · 78 tools`.
