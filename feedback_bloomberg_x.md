---
name: feedback-bloomberg-x
description: "Für Bloomberg Makrodaten auf X immer @DeItaone verwenden, nicht @economics oder andere Accounts"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 220d4224-24b6-42aa-87ad-8a89ca782b91
---

Für Bloomberg-Makrodaten auf X **immer @DeItaone** verwenden — nicht @economics, @Bloomberg oder andere Accounts.

**Why:** User hat explizit korrigiert. @DeItaone ist der Bloomberg-Echtzeit-Datenfeed auf X (Wirtschaftsdaten, PMI, JOLTS, NFP etc.).

**How to apply:** Bei jeder Suche nach Makrodaten via X API zuerst `get_users_by_username("DeItaone")` → dann `get_users_posts` mit der User-ID. Niemals @economics oder @Bloomberg für Datenpunkte verwenden.
