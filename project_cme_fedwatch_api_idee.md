---
name: project-cme-fedwatch-api-idee
description: "21.08.2026 Fable-Review: CME FedWatch API für 'start update dich' Schritt 3/4 geprüft — kein GO für kostenpflichtige API, kostenlose Alternative empfohlen"
metadata: 
  node_type: memory
  type: project
  originSessionId: 10b391f3-70a9-4919-a6bf-cfff4acb3ed2
  modified: 2026-08-21T13:08:25.812Z
---

Levi fragte, ob eine CME-FedWatch-API-Anbindung den [[feedback_session_update]]-Ablauf (Schritt 3/4) verbessern würde. Fable-5-Review (Recherche via WebSearch) ergab:

- FedWatch ist keine eigene Datenquelle, sondern eine aus 30-Day-Fed-Funds-Futures-Preisen abgeleitete Wahrscheinlichkeitsverteilung.
- Es existiert eine offizielle CME-API (EOD ~25$/Monat, Intraday teureres Enterprise-Tier), aber auch eine kostenlose Alternative: Python-Pakete `cme-fedwatch`/`pyfedwatch` berechnen dieselben Werte ohne API-Key direkt aus öffentlichen Futures-Preisen.
- **Kein GO für kostenpflichtige API.** Begründung: FedWatch-Werte ändern sich nur bei neuen Makrodaten/Fed-Kommentaren (Tagesfrequenz), nicht intraday — für Levis 5-Min-NAS100-Trading strukturell der falsche Hebel. Zusätzlich teilweise redundant zu den X-Tweets (Kobeissi/DeItaone posten solche %-Werte ohnehin).

**Why:** Anders als die offene [[project_tradingeconomics_api_idee]] (149$/Monat) liegt hier KEINE nachgewiesene Zuverlässigkeitslücke vor — TradingEconomics soll ein bestehendes Kalender-Scraping-Problem lösen (zweimal live fehlgeschlagen), FedWatch wäre eine reine Erweiterung ohne bestehenden Schmerzpunkt. Beide Entscheidungen bewusst getrennt behandeln, nicht bündeln.

**How to apply:** Falls Levi die kostenlose Variante will, gehört sie in Schritt 3 (FRED, strukturell) des Session-Update-Ablaufs — entweder WebFetch auf die öffentliche cmegroup.com-FedWatch-Seite oder ein kleiner Skript-Call mit `pyfedwatch`. Noch nicht umgesetzt, nur empfohlen. Die kostenpflichtige EOD-API erst erwägen, falls sich der freie Weg als unzuverlässig erweist (gleicher Maßstab wie bei TradingEconomics).
