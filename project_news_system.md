---
name: project-news-system
description: "News-System für Trading — RSS läuft, X MCP aktiv seit 30.06.2026"
metadata: 
  node_type: memory
  type: project
  originSessionId: 607aa8f6-9958-4c1c-9c76-4afabcffb717
---

News-Integration für Session-Start und Live-Trading aufgebaut.

**Why:** Markt-News (Trump-Tweets, Fed-Statements, Breaking News) bewegen Nasdaq oft bevor RSS-Feeds reagieren. X MCP gibt Echtzeit-Zugriff, RSS ist gut für Morgen-Überblick.

**How to apply:** Beim Session-Start immer `node scripts/news_rss.cjs` ausführen. Für Echtzeit-Breaking-News X MCP tools nutzen (Suche, News, Trends).

### Aktiv — X MCP (ersetzt RSS, Live-Daten)
- Konfiguration: `.mcp.json` → `xapi` mit Bearer Token (App-only, read-only)
- Watchlist: `scripts/x_watchlist.json` (3 Accounts mit IDs gespeichert)
- Letzter Fetch: `scripts/x_last_fetch.json` (wird nach jedem Update aktualisiert)
- Accounts: @DeItaone (2704294333), @KobeissiLetter (3316376038), @zerohedge (18856867)
- Tweets werden still verarbeitet — fließen in Bias ein, nicht separat ausgegeben
- **Credits: $25 aufgeladen 30.06.2026** — reicht ~1-2 Monate (~$1/Handelstag)

### Abgelöst — RSS (nicht mehr verwenden)
- `scripts/news_rss.cjs` — ersetzt durch X MCP, nicht mehr beim Session-Start ausführen
