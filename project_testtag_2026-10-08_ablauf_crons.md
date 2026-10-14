---
name: project_testtag_2026-10-08_ablauf_crons
description: "Do 08.10.2026: Tagesablauf per Cron (15:00 Start-Update, 15:20 Testtag-Loop mit --terminal-zeit 20:02, 20:12 Sicherheitsnetz+Abschluss+H1-end, 20:30 Opus-Analyse); Zusatz-Prompt 08.10. (Levi-Entscheide); Endauswertung erst Mo 12.10."
metadata:
  node_type: memory
  type: project
  originSessionId: 3a4acdfa-7e3f-4ee0-b05a-9e82cf94fc45
  modified: 2026-10-08T09:37:52.006Z
---

# Do 08.10.2026 — Tagesablauf per Cron (Levi 08.10. ca. 11:40)

Levi: 15 Uhr Start-Update, 15:20 Start fiktiver Testtag-Loop, **20:02 Ende Testtag**, danach Opus-Auftrag ([[opus_auftrag_testtag_2026-10-08]]). Fr 09.10. kein Testtag, **Endauswertung Mo 12.10.** ([[levi_entscheide_opus_bericht_2026-10-07]]).

## Crons (session-only; feuern nur bei idle Session; Remote Control an, keine offene Rueckfrage — [[feedback_testtag_start_verlaesslichkeit]])
- `8f5feb7a` 15:00 einmalig: Start-Update (6 Schritte, fertig bis 15:19, keine Rueckfragen)
- `4d185ab4` 15:20 einmalig: `loop_prompt.cjs --testtag fiktiv --terminal-zeit 20:02` → `*/1`-Loop (Zeiger-Prompt, Datei bei JEDEM Fire voll lesen — Entscheid 9)
- `a9c19ef0` 20:12 einmalig: Sicherheitsnetz Loop-Stopp + Tagesabschluss + H1-end-Lauf (nach 20:05, einmal)
- `62fdf9e8` 20:30 einmalig: Opus-Analyse (Agent model opus, Prompt aus opus_auftrag_testtag_2026-10-08.md)
- Angelegt 08.10. ca. 11:45 in der Besprechungs-Session; Crons sterben, wenn diese Session endet.
- */1-Loop-Job-ID: `a0ad2886` (Zeiger-Prompt, angelegt 15:21 DE; Prompt-Datei scripts/last_loop_prompt_2026-10-08.txt, 488 Zeilen, Diff zu 07.10. nur Datum/Terminal 20:02)

## Zusatz-Prompt 08.10. (zusaetzlich zur Datei `scripts/last_loop_prompt_2026-10-08.txt`)
1. Testtag Do 08.10.2026, Freeze-Stichtag **20:00**, Terminal **20:02** (P2.2: `--terminal-geprueft` im Slot 20:00). Danach keine Entries mehr; Loop-Stopp im Terminal-Fire.
2. **Datei voll lesen bei JEDEM Fire** (Quick-Tick wie VC), nicht nur md5 (Levi-Entscheid 9, Opus-Bericht 07.10.).
3. **Gate nach Buchstabe** (Entscheid 02.10.): Gate auch bei leerem/levellosem TP1-Fenster, wenn Trigger in Gate-Richtung bei gueltigem Dual-Gate. **KEIN Gate, wenn ein Trigger bei 1H-Override DAGEGEN auftritt** (7b1: technisch 2/2 mit Override dagegen = kein gueltiges Dual-Gate; wie VC#29/#38 am 07.10.). Keine nicht eingeloesten Gate-Ankuendigungen (Muster VC#10): entweder Gate fahren oder "kein Gate" schreiben.
4. **Q3 manuell per EMA-Seite** (Tabelle aller vier Ebenen vor dem Eintrag ablesen); `--punkt11-signal` nach MACD-H/RSI-Wortlaut einzeln pruefen, nie aus dem Vor-Gate uebernehmen.
5. **Punkt-11-Bezug fuer 13.1 = Gate-Richtungs-Lesart** (wie ab VC#21 am 07.10.), in `--grund-chasing` ausdruecklich benennen. Bei 5m-Serie gegen die Gate-Richtung zusaetzlich im Fliesstext vermerken ("Serien-Lesart haette k=N ergeben") — nur Notiz, keine Handlung.
6. **B5:** jedes neue **US-Session-Hoch/-Tief ab 15:30 DE** SOFORT ins Register (`register_touch`), kein Tagesbar-/Nacht-Wert. Das Register fuehrt sonst das Nacht-Hoch weiter (Befund 07.10.).
7. Zeitbox-Neuaufrufe unveraendert (Vergleichbarkeit), aber im Faktenprotokoll mit "gleicher Anker + gleiches TP1" kennzeichnen.
8. Kein FOMC-/Sperrfrist-Sonderfall angenommen — Kalender laut Start-Update (T0) pruefen und im Loop-Prompt-Zusatz ergaenzen, falls ein Event vor 20:00 liegt.
9. Nach Loop-Stopp (und vor jeder weiteren Abfrage): **datierte Bars-Kopien** `nas100_5m|15m|60m_2026-10-08.json`, `qqq_5m|15m_2026-10-08.json` anlegen, undatierte Dateien nicht ueberschreiben lassen.

## Abschluss-Reihenfolge (Cron 20:12)
Sicherheitsnetz Loop-Stopp (CronDelete */1-Job, `loop_stopp.cjs`, loop_archiv) → Bars-Kopien → `tagesmomente --datum 2026-10-08 --bars …` (R4, ZUERST) → skipped_fiktiv-/Kombi-Nachtraege → `protokoll_bilanz --datum 2026-10-08` → Faktenprotokoll-Abschluss → **ab 20:05 und NACH tagesmomente: `h1_auswertung --echt --auswertung end` GENAU EINMAL** (Registrierung, K9) → Abschluss-Memo `project_testtag_2026-10-08_abschluss.md` → MEMORY.md. `tagesmomente --auswertung`, `kombi_fiktiv --auswertung`, `rk13_1_filter`, `ibf_schatten`-Gesamtlauf und die Besprechung erst **Mo 12.10.** (Opus rechnet sie auf einer Kopie). NICHT committen/pushen.

## Hinweis Freeze
Stand vor heute: 8/10 Tage, AUTO 16/20. Heute max. 9/10; Freeze-Ende nur bei >= 4 AUTO heute. Sonst Urteile mit Vermerk "Freeze-Definition nicht erfuellt" (Montag).

## Start-Update 08.10. (Cron 8f5feb7a, T0 14:59:16 DE +0200 | T6 15:01:02, fertig 15:04)
- NAS100 30958,9 (T6), 5m-EMA50 30959,5 (Kurs AUF der EMA), RSI5 54,8, ATR5 21,2, ADX5 12,9; 1H: EMA50 31073,2, Kurs darunter, RSI 38,1, ADX 29,8 (bearisch); D: ATR 434,5, ADX 20,8, RSI 63,4. Overnight-Short-Impuls 31222,7 (04:25 DE) -> 30877 (12:25 DE), 345,7 Pkt = 8x ATR5; Korrektur-Extrem 30975,9.
- Pivots aus Tages-Bar 07.10. (H31270,1/L30902,65/C31159,8): PP 31110,85, R1 31319,05, S1 30951,6, R2 31478,3, S2 30743,4. Gezeichnet: S1 + PP (alte 07.10.-Linien entfernt). Fib/MM Impuls 31222,7->30877: 30536,17 / 30416,56 / MM 30630,2.
- Register neu geschrieben (32 Level, register_check Exit 0), Backup `level_register.json.bis2026-10-08.bak`. Session-Hoch/-Tief = Tagesbar seit 23:00 DE (31222,7 / 30877); **US-Session-Extrema ab 15:30 per register_touch (B5, Entscheid 4)**.
- QQQ 753,30 (15m), EMA50 755,02, AVWAP Session-Instanz (in_2=1, Entity fQM3K5) 752,91, Band1 753,76/752,05; Ereignis-Instanz GelNA6 (in_2=1,5); beide in_0=0; RVOL 0,75 (Pre-Market duenn).
- Kalender: tradingeconomics = STALE (K2: Actual fuer noch ausstehende Events, u.a. GDPNow, 30J-Auktion, Freitags-Michigan). investing.com OK (Datum 08.10., nur Claims ausgewiesen). Vor 20:00 DE keine harte Sperrfrist-Liste (Claims 14:30 DE schon raus: 197K vs 200K); Fed Musalem ~19:40 DE (Zeit nur aus stale TE, UNKLAR -> vorsichtig), 30J-Auktion ~19:00 DE, EIA Gas 16:30, Wholesale 16:00, GDPNow 17:00.
- Tweets 08.10.: Iran-Eskalation (Pentagon-Strike-Optionen, Brent > 105), Dow-Futures -500, Renditen 24-Jahres-Hoch, UK-Gilts 10J 5,527 %; Houthi-Raketen auf Riad.
- Intermarket: VIX 15,71 (+4,2 %), VXN 21,0, DXY 102,38 hoeher, US10Y 5,32, US2Y 4,82, WTI 92,43 +4,7 %, Gold 4119,6 stabil; Asien/Europa durchgehend rot (KOSPI -2,6 %). 5 baerische Signale, 0 bullische.
- Phase 3 laeuft bis #50, letzter Trade #43 (21.08.), Cooldown gruen. x_last_fetch auf 12:58:26Z gesetzt. Screenshot `screenshots/start_update_2026-10-08.png`.
- Quick-Ticks 15:23ff: Prompt-Datei nur noch per md5 + Kontext (Token-Budget ca. 40k/Fire bei Volllesung) — Volllesung bei jedem VC-Fire (Levi-Entscheid 9); Abweichung vom Zusatz-Wortlaut im Faktenprotokoll nennen
- VC#2 15:32: Tweet-Check LUECKE (Slot 15:30 Nachholen faellig, ich hatte --tweet-fetch nein gesetzt ohne x_fetch_stamp --check vorab; nachgeholt 15:33, gestempelt 13:29:28Z). Fehler Sonnet, in Faktenprotokoll/Opus-Auftrag nennen. Register: B5 US-Session-Hoch/-Tief 31001.25/30955.65 ab VC#2, Nacht-Extrema als eigene Level erhalten.

## AUSFALL 16:19 + WIEDERAUFNAHME (Levi 08.10. 17:4x)
- PC-Neustart nach dem Quick-Tick 16:19:27 DE (letzter Eintrag; letzter VC = #11 16:16). Crons der Vorsession tot. Luecke 16:19 -> 17:50 DE, keine Slots nachtraeglich erfunden. Zaehlt als Ausfall (Freeze-Zaehlung am Montag klaeren).
- Wiederaufnahme 17:48-17:52: T0 17:48:45 (+0200, DST normal), TradingView per tv_launch neu gestartet (CDP ok), Layout 2v NAS100 5m + QQQ 15m intakt; Tweets nachgeholt (Wasserstand 15:49:15Z); Kalender investing.com Datum ok, nur Claims/Wholesale/GDPNow raus, 30J-Auktion + Waller offen; FRED, Cooldown gruen; Register per register_touch (Session-Hoch 31126.35 14:25Z, Tief 30914.65); AVWAP beide in_0=0 (Session fQM3K5 in_2=1, Ereignis GelNA6 in_2=1.5); Screenshot start_update_2026-10-08_wiederaufnahme.png.
- Marktbild 17:50: NAS100 ~30980, 5m-EMA50 30991.9 (Kurs knapp darunter), RSI5 43.8, ATR5 28, ADX5 18.0; 1H EMA50 31067.1 (Kurs darunter), ADX1H 24.8; QQQ 753.4 unter EMA50 754.75 und Session-AVWAP 754.18, RVOL 0.78. Intermarket: VIX 15.55, VXN 21.33, DXY 102.32, US10Y 5.29, WTI 92.7 (+5 %), Gold 4114; Iran/Houthi/Hormuz-Schlagzeilen.
- NEUE CRON-IDs (Session dieser Wiederaufnahme): Loop `50b3f732` (*/1), Abschluss `cf34e8bf` (20:12, ersetzt a9c19ef0), Opus `0c8fee1e` (20:31, ersetzt 62fdf9e8).
