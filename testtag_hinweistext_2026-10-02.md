---
name: testtag_hinweistext_2026-10-02
description: "Opus 02.10.2026 ~12:10: Antwort auf Levis Frage 'Muss ich wieder einen Hinweistext an Sonnet schicken?' (Nein, Sonnet baut den Wrapper selbst; von Levi nur Start-Trigger + Remote Control) + fertiger Cron-Wrapper fuer den fiktiven Testtag Fr 02.10.2026 (NFP-Tag) + Sonnet-Checkliste vor 15:25; keine Regelwerk-/Prompt-Aenderung"
metadata:
  type: project
  originSessionId: opus-hinweistext-testtag-2026-10-02
  modified: 2026-10-02T09:56:32.843Z
---

# Hinweistext / Cron-Wrapper fiktiver Testtag Fr 02.10.2026

## 1. Antwort an Levi

**Nein, du musst den Hinweistext nicht selbst schicken.** Sonnet baut den Wrapper aus der Vorlage unten und dem Stand aus dem Start-Update. Die Marktdaten (Kalender, Tageslage, Register, AVWAP-Entities) kann ohnehin nur Sonnet um 15:00 liefern, nicht du. Der eigentliche Pflichtblock kommt woertlich aus `loop_prompt.cjs`.

**Was von dir kommen muss (nicht delegierbar):**
1. **Start-Trigger:** "start update dich" um 15:00, falls er nicht schon per Cron vorgemerkt und von dir bestaetigt ist. Regel seit 24.09.: kein Testtag-Start ohne deine ausdrueckliche Bestaetigung.
2. **Remote Control an**, bevor du weggehst. Eine offene Rueckfrage blockiert den Cron (Fall 25.09.).
3. **Commit/Push**, falls gewuenscht. Das macht nur Sonnet auf deinen Befehl, nie automatisch.

**Was nicht von dir kommen muss:**
- Blackout/Kalender: NFP 14:30 DE ist vor dem Loop-Start durch. Die Sperrfrist-Logik regelt das Regelwerk selbst (`--blackout` im gate_check), Sonnet ermittelt das im Start-Update.
- Die Opus-Analyse nach dem Loop-Stopp hast du fuer heute bereits freigegeben. Sie steht unten im Wrapper.

Ein eigener Text von dir ist nur noetig, wenn du **inhaltlich** etwas anders willst als unten, z.B. einen frueheren Stopp oder keine Opus-Analyse.

**Freeze-Pruefung des VC#1-Hinweises:** vereinbar.
- Er erinnert nur an Felder, die **Template A schon heute verlangt**: `--sl-anker`/`--cluster-level` Pflicht ohne Ausweg, sobald die Vorpruefung faellig ist (>= 1 Bein), und `--stale-n`. Das steht in last_loop_prompt Z. 88/91/106/144/145.
- Er aendert weder `loop_prompt.cjs` noch ein Skript und hat keine Gate-Wirkung. Die "eine Live-Aenderung pro Testtag" wird also nicht verbraucht.
- **Nicht uebernommen** sind die C4-Neuerungen, die erst ab 05.10. gelten: die Forderung "VC#1 traegt IMMER `--cluster-level` auch bei 0 Beinen", der REDIRECT-PFLICHT-Block im Prompt und `loop_archiv.cjs` im LOOP-STOPP-Text des Prompts.
- Die Redirect-Konvention und der manuelle Aufruf von `loop_archiv.cjs` sind wie am 01.10. **Prozesshinweise** (feedback_tagesabschluss 1a erlaubt den manuellen Aufruf ausdruecklich), kein Prompt-Bestandteil.
- **Messhinweis:** Das C4-Vorab-Kriterium (0 Hard-Exits in VC#1/#2 an 3 Testtagen) zaehlt erst ab 05.10. Der 02.10. geht dort nicht ein.

## 2. Fertiger Wrapper-Text (Sonnet setzt ihn VOR den Pflichtblock in den CronCreate-Prompt; <...> aus dem Start-Update fuellen)

```
FIKTIVER TESTTAG Fr 02.10.2026 (NAS100, --testtag fiktiv). Loop-Fenster bis Terminalzeit 20:02 DE. Das Start-Update (T0 <HH:MM:SS> DE, Register, Cooldown <Status>, Tweet-Wasserstand <ISO>) ist erledigt.

Hinweis fuer heute: Die Punkte (3) KURZBLOCK und (4) ENTRY-KASTEN im Cron-Prompt sind seit 23.09. zurueckgebaut – ignorieren (kein Kurzblock, kein --zeige), Vollblock wie gewohnt ausgeben. Der Pflichtblock ist heute UNVERAENDERT gegenueber gestern (C4/C5 gelten erst ab Testtag 05.10.).

Jeden vollcheck.cjs-Lauf IM BASH-TOOL (nicht PowerShell – dort wird /tmp zu C:\tmp) sichern: node scripts/vollcheck.cjs … > /tmp/vc_2026-10-02_<N>.txt 2>&1; echo "Exit $?"; cat /tmp/vc_2026-10-02_<N>.txt – Wiederholung nach Hard-Exit als /tmp/vc_2026-10-02_<N>b.txt mit demselben Muster; nie eine Datei ueberschreiben oder loeschen, keine dritte Form (_Nc); bei erneutem Loop-Start fortlaufend weiternummerieren. Die cat-Ausgabe ist der Voll-Check und wird woertlich uebernommen.

ERSTER VOLL-CHECK (Erinnerung an bestehende Template-A-Pflichten, keine neue Regel): Am 01.10. endete VC#1 mit Hard-Exit (--sl-anker/--cluster-level fehlten bei 1 Bein short), am 30.09. VC#1+#2 wegen --stale-n. Deshalb VC#1 und VC#2 von Anfang an vollstaendig nach Template A: sobald >= 1 Bein gerichtet ist (15m-Close ungleich EMA50 bei NAS100 oder QQQ) --sl-anker <Struktur-Anker 7b1 3a auf der SL-Seite der Bein-Richtung, P1> UND --cluster-level <register|Preis|tief-hoch|none – none nur mit Begruendung im Fliesstext>; immer --stale-n <0-5> (bzw. --grund-stale-n nur, wo das Skript es vorsieht). Bei 0 Beinen --sl-anker weglassen (kein Dummy). Zustandsspeicher scripts/vollcheck_state.json stammt vom 01.10. (datum_de 2026-10-01): beim ERSTEN Voll-Check die Startwerte per CLI setzen (kerzen-nas100, kerzen-qqq, zyklus-8a5, k-ohne-signal, impuls-pkt, erster-vollcheck = dieser Slot); meldet das Skript den Alt-State, nur nach Vorgabe des Skripts reagieren (--*-reset-grund), nichts loeschen.

PFLICHTBLOCK (Loop-Start-Checkliste): Der vollstaendige, von `node scripts/loop_prompt.cjs --testtag fiktiv --terminal-zeit 20:02` erzeugte LOOP-START-PFLICHTBLOCK (Selbsttest Exit 0, <Zeilenzahl> Zeilen) steht in scripts/last_loop_prompt_2026-10-02.txt und gilt WOERTLICH als Teil dieses Prompts. BEI JEDEM FIRE: ist der Block nicht mehr vollstaendig in deinem Kontext (erster Fire, nach Kontext-Zusammenfassung), ZUERST die Datei komplett per Read lesen (2 Read-Aufrufe, offset/limit), dann befolgen — nichts aus dem Gedaechtnis, kein Abkuerzen. Kernpunkte (Details nur dort): (a) bare `date` lesen, --jetzt = diese Zeit als UTC-ISO mit Z; (b) Minute % 5 == 0 -> Voll-Check per vollcheck.cjs (Template A, nie von Hand; mind. die ersten DREI Voll-Checks unveraenderte Skript-Ausgabe), davor MTF-Vierschritt 15m->60m->5m inkl. QQQ-Pane falls Gate offen, am Ende Tweet-Check/Format-Zeile; sonst Quick-Tick per quick_tick.cjs (erste Ausgabezeile woertlich); (c) bei offener Position zusaetzlich Template B position_tick.cjs; (d) 2/2-Trigger: SL-Anker-Vorpruefung (--sl-auto, --cluster-level), Live-gate_check.cjs mit allen A3-Feldern + --testtag fiktiv + --blackout, Ausgabe aus scripts/last_gate_check.txt woertlich, TP1/SL IMMER aus dem Live-Lauf (B2a); PASS + Q-ROT -> ausgelassen, skipped_fiktiv/Kombi-Nachtrag beim Tagesende; (e) Register: neue Session-Hoch/-Tief SOFORT per register_touch.cjs --session-hoch|--session-tief <Preis> --note "VC#<n> …" (B5, Preis aus Chart-Bars, nicht quote_get; US-Session ab 15:30 DE – das US-Session-Hoch, nicht das Tageshoch); register_check.cjs bei jedem Voll-Check; (f) Tweet-Fetch nur bei FAELLIG JA laut x_fetch_stamp.cjs --check, danach --set/--polled stempeln.

KALENDER HEUTE (aus dem Start-Update, DE-Zeit): NFP 14:30 war schon raus (<Ist> vs. <Konsens>, <Revision>); weitere Releases im Loop-Fenster: <aus Start-Update – Termin, Konsens; nichts ergaenzen, was dort nicht steht>. Blackout-/Sperrfrist-Logik nach Regelwerk und --blackout; das NFP-Fenster ist beim Loop-Start abgelaufen, aber NFP-Tag = erhoehte Volatilitaet/Nachwirkung (Whipsaw, weite ATR) – das ist Kontext, KEIN zusaetzliches Gate. Halbierungsfenster 15:30-16:00 DE (Positionsgroesse halbieren; DST normal: ET = DE - 6h).

TAGESLAGE aus dem Start-Update (nur Kontext, KEIN Gate): NAS100 <Kurs> (Hoch <x> / Tief <y>), 5m-EMA50 <..>, ATR(5m) <..>, ATR-D <..>, ADX 5m/1H/D <..>, 1H-EMA50 <..>, QQQ <..> (AVWAP Session <..>, EMA50-15m <..>, RVOL <..>), VIX <..>, VXN <..>, US10Y <..>, DXY <..>, Brent <..>. Register (<n> Level) frisch <HH:MM> DE: PP <..>, R1 <..>, S1 <..>, R2 <..>, S2 <..>, PDH <..>, PDL <..>. AVWAP QQQ: Session-Instanz in_2=<..> (Entity <..>), Ereignis-Instanz in_2=<..> (Entity <..>).

ZAEHLSTAND (nur Kontext, KEIN Gate, kein Grund fuer anderes Handeln): Freeze 5/5 bewertbare Tage, AUTO 10/20, Endauswertung 08.10.2026; der heutige Tag zaehlt nur mit Bars bis 20:00 – deshalb Loop NICHT vor 20:02 beenden, ausser Levi stoppt ausdruecklich (dann trotzdem Bars bis 20:00 sichern).

LOOP-STOPP / TAGESABSCHLUSS (20:02, heute MIT Opus-Analyse):
1. `node scripts/loop_stopp.cjs --jetzt <UTC-ISO Z> --grund "<Anlass>"` – Ausgabe woertlich. Exit 2 (offene Position) -> KEIN CronDelete, 15b; Exit 0 -> CronDelete; Exit 1 -> Position von Hand bestaetigen.
2. Archiv: `node scripts/loop_archiv.cjs --datum 2026-10-02` (manueller Aufruf nach feedback_tagesabschluss 1a; liest /tmp/vc_2026-10-02_<N>[b].txt + Live-Gates aus gate_check_log.jsonl, schreibt scripts/loop_archiv/2026-10-02.txt inkl. "Exit-Code:"-Zeilen). NIE --ueberschreiben (das ordnet nur Levi an). Exit 1 -> Ausgabe lesen, im Abschluss melden; Nummernluecken/Hard-Exits/NICHT EINGELESEN in den Abschluss uebernehmen. Rueckfall: manueller Zusammenbau nach 1a.
3. `node scripts/protokoll_bilanz.cjs --datum 2026-10-02 --position-offen nein` (liest genau das Archiv; --datum statt --protokoll <Datei>).
4. Bars NACH 20:00:00 sichern: data_get_ohlcv NAS100, 5, count 300, save_path scripts/nas100_5m_2026-10-02.json (muss 14:30-20:00 DE abdecken, letzte Bar 19:55). Dann `node scripts/tagesmomente.cjs --datum 2026-10-02 --bars scripts/nas100_5m_2026-10-02.json` und `--auswertung` (FREEZE-ENDE-Zeile woertlich).
5. skipped_fiktiv.cjs --list --offen + Nachtraege mit --bars (Stichtag 20:00, bei ausgelassenem PASS --grund-auslassen); protokoll_bilanz.cjs --kombi-log + kombi_fiktiv.cjs --nachtrag <id> --bars ….
6. Memory memory/project_testtag_2026-10-02_abschluss.md (Kennzahlen NUR aus Logs/Skriptausgaben, nicht aus dem Chat; offene Punkte).
7. DANACH Opus-Analyse anstossen (heute ausdruecklich erlaubt): Opus-Subagent mit Auftrag "Analyse fiktiver Testtag 02.10.2026 nach Vorlage opus_bericht_testtag_2026-10-01.md; Skriptlaeufe nur auf Kopien; Ergebnis in memory/opus_bericht_testtag_2026-10-02.md; keine Regelwerk-Aenderung (Freeze bis Endauswertung 08.10.)". Ergebnis Levi kurz melden.
KEIN git add/commit/push (nur auf Levis Auftrag); /tmp/vc_2026-10-02_* nicht loeschen.
```

## 3. Cron-Ausdruck (Empfehlung)

- Ein einzelner Standard-Cron-Ausdruck kann "15:25 bis 20:02" nicht exakt abbilden. Empfehlung: **einen** Minuten-Job `* 15-20 * * *` **erst um ca. 15:24/15:25 anlegen**, also nach Abschluss des Start-Updates. Dadurch gibt es keine Fires vor 15:25.
- Am Ende sorgt der Terminal-Baustein im 20:00-Voll-Check (20:02 liegt im 20:00-Slot) fuer den Loop-Stopp, danach folgt das CronDelete. Deshalb darf der Ausdruck **nicht** nur `15-19` lauten, sonst fehlt der 20:00-Fire.
- Nach dem Anlegen mit CronList pruefen, dass die Zeit als lokale DE-Zeit gelesen wird. Kein separater 20:00-Job noetig.
- Fallback im Wrapper ist nicht noetig. Faellt das CronDelete aus, fuehrt jeder Fire nach 20:02 nur den LOOP-STOPP-Ablauf aus (steht im Pflichtblock).

## 4. Checkliste Sonnet vor 15:25

1. **Keine offene Rueckfrage an Levi** am Ende des Start-Updates oder im Loop. Was unklar ist, wird mit der konservativen Regel entschieden und im Abschluss gemeldet. Die Session muss idle sein, sonst feuert der Cron nicht (25.09.). Levi bestaetigen lassen, dass Remote Control an ist.
2. **Pflichtblock frisch erzeugen:** `node scripts/loop_prompt.cjs --testtag fiktiv --terminal-zeit 20:02` muss Exit 0 liefern, die Datei `scripts/last_loop_prompt_2026-10-02.txt` muss existieren, und die Zeilenzahl kommt in den Wrapper. Der Block bleibt unveraendert, nichts von Hand ergaenzen.
3. **State-Startwerte vorbereiten:** In `vollcheck_state.json` steht noch `datum_de` 2026-10-01. VC#1 setzt die Startwerte per CLI, und nur auf Skriptmeldung wird mit `--*-reset-grund` reagiert. Die VC#1-Parameter aus dem Wrapper vorab bereitlegen (Bein-Richtung, Struktur-Anker 3a, `--cluster-level`, `--stale-n`).
4. **Log-Frische-Gegencheck um ca. 15:31:** `quick_tick_log.jsonl` und `vollcheck_log.jsonl` brauchen Eintraege von heute, und `vollcheck_state.json` muss `datum_de` 2026-10-02 zeigen. Ein Eintrag in CronList allein beweist nichts. Fehlt etwas, Levi aktiv melden (Remote Control).
5. **Register/Session-Extrema:** Ab 15:30 das US-Session-Hoch und -Tief eintragen, nicht das Tageshoch (Befund 01.10.). NAS100- und QQQ-Pane sowie RVOL/AVWAP sind im Start-Update Schritt 6 geprueft.
