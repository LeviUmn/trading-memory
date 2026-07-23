---
name: project-tradingview-launch
description: "TradingView is installed as MSIX (Windows Store package), not a classic exe. The MSIX path must be in health.js for tv_launch to work."
metadata: 
  node_type: memory
  type: project
  originSessionId: 607aa8f6-9958-4c1c-9c75-4afabcffb717
---

TradingView is installed as an MSIX package (not a classic installer) at:
`C:\Program Files\WindowsApps\TradingView.Desktop_3.2.0.7916_x64__n534cwy3pjxzj\TradingView.exe`

This path was added to the `win32` candidates list in `src/core/health.js` so that `tv_launch` can find and start it automatically with `--remote-debugging-port=9222`.

**Why:** The standard paths (`%LOCALAPPDATA%\TradingView\`, `Program Files\TradingView\`) don't exist — the app comes from tvd-packages.tradingview.com as MSIX.

**How to apply:** If tv_launch fails to find TradingView, check that the MSIX path is still in the candidates list in `src/core/health.js`. The version number in the path may change after updates.
