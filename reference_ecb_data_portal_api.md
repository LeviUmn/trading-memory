---
name: reference-ecb-data-portal-api
description: "ECB Data Portal REST-API — kostenlose EZB-Primärquelle für Leitzinsen/Makrodaten, kein API-Key nötig"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 51fca406-b869-4659-8541-11810063992f
  modified: 2026-08-06T09:57:26.201Z
---

Live entdeckt und verifiziert 06.08.2026 (Levi hat die Doku-URL + PDF geliefert, siehe [[project_dax_erweiterung]] To-do 7). Genutzt in `scripts/eu_snapshot_dax.py`.

**Basis-URL:** `https://data-api.ecb.europa.eu/service/data/{flowRef}/{key}?parameters`

- `flowRef`: Dataflow-ID, z.B. `FM` (Financial Market Data), `ICP` (Indices of Consumer Prices). Alle Dataflows: `https://data-api.ecb.europa.eu/service/dataflow`
- `key`: Dimensionen dot-getrennt (Reihenfolge nach DSD), leer lassen = Wildcard, `+` = OR. Beispiel für EZB-Hauptrefinanzierungssatz: `D.U2.EUR.4F.KR.MRR_FR.LEV`
- Wichtige Parameter: `format=jsondata` (auch `csvdata`/`genericdata`/`structurespecificdata`), `lastNObservations=N`, `startPeriod`/`endPeriod`, `detail=dataonly|serieskeysonly|nodata`
- **Kein API-Key nötig**, nur HTTPS

**Verifiziert funktionierend:**
- Hauptrefinanzierungssatz: `FM/D.U2.EUR.4F.KR.MRR_FR.LEV` — taggenau aktuell (getestet 06.08.2026: 2,4%, Datum = Abfragetag)
- Einlagesatz: `FM/D.U2.EUR.4F.KR.DFR.LEV` — ebenso taggenau (2,25%)

**Bekannte Lücke:** `ICP/M.U2.N.000000.4.ANR` (versuchte HICP-Jahresrate) lieferte nur Stand Dezember 2025 (8 Monate alt) — falsche Dimensions-Kombination, nicht aufgelöst. Für HICP/BIP/Arbeitslosigkeit/Bundrendite wird stattdessen FRED genutzt (siehe `scripts/eu_snapshot_dax.py`), das dort aktuell und zuverlässig lief. Bei Bedarf später die korrekte ICP-Serie via `detail=serieskeysonly`-Discovery-Query auflösen.

**Warum relevant:** Löst den in der Fable-Review vom 05.08.2026 genannten Vorbehalt "FRED ist nur Sekundärquelle, verzögert gegenüber EZB SDW direkt" — für die Zinssätze jetzt durch echte Primärquelle ersetzt, kostenlos und ohne Zusatzaufwand ggü. FRED.
