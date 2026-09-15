---
name: feedback-zeitzone
description: "User lebt in Deutschland, will alle Uhrzeiten in Ausgaben (Loop-Ticks, Session-Updates, Timestamps) in deutscher Ortszeit, nicht UTC. Seit 14.09.2026 zusätzlich KANONISCHE STELLE für die EU/US-Sommerzeit-Divergenz-Fenster (datierte Merkliste 2026-2028, per Node-ICU gemessen; nächstes: Mo 26.10.-Fr 30.10.2026, dort ET = DE − 5 h, US-Open 14:30 DE, 8:30-ET-Releases 13:30 DE; Konsequenztabelle, Verfallsregel ab Herbst 2028 = UNBEKANNT + Node-Pflichtcheck). Alle anderen Dateien verweisen nur hierher."
metadata: 
  node_type: memory
  type: feedback
  originSessionId: b9585154-6b19-4cde-b4d2-f0df64bb9338
  modified: 2026-09-15T01:36:11.623Z
---

Ab 22.07.2026: Alle Uhrzeiten in Claude-Ausgaben (Loop-Ticks, Voll-Checks, Session-Updates, Trade-Notizen) in deutscher Ortszeit angeben, nicht UTC.

**Why:** User lebt in Deutschland, UTC-Zeitstempel mussten bisher immer gedanklich umgerechnet werden (z.B. "16 Uhr DE" vs. "14:00 UTC" in denselben Nachrichten).

**Korrigiert 27.07.2026 (echter Fehler live aufgetreten):** Die alte Annahme unten ("`date` liefert immer UTC, manueller +2h-Offset nötig") ist NICHT mehr korrekt — Stand 27.07.2026 liefert der bare `date`-Befehl (ohne `-u`) bereits die korrekte DE-Ortszeit direkt (verifiziert gegen `date -u`: Differenz exakt +2h CEST, UND gegen die vom User live bestätigte tatsächliche Uhrzeit). Ursache der Diskrepanz zur alten Notiz unbekannt (evtl. Sandbox-Update, oder die alte Annahme war nie ganz korrekt) — **Konsequenz:** Ab sofort den bare `date`-Befehl (ohne `-u`, ohne manuellen Offset) direkt als DE-Ortszeit verwenden. NICHT mehr zusätzlich +2h/+1h manuell addieren — das führte am 27.07.2026 zu einem realen 2-Stunden-Fehler (16:44 statt korrekt 14:54 behauptet), vom User live korrigiert.

**How to apply (korrigiert):**
- `date` (Bash-Tool, ohne `-u`) direkt als DE-Ortszeit verwenden — kein manueller Offset mehr
- Vor jeder wichtigen zeitkritischen Aussage (Session-Start, Loop-Beginn) den Wert kurz gegen `date -u` plausibilisieren (Differenz sollte +2h in CEST/+1h in CET sein) — weicht das spürbar ab, aktiv beim User nachfragen statt stur der Rechnung zu vertrauen (siehe [[feedback_verify_dont_cave]])
- Interne Zeitstempel-Vergleiche (z.B. QQQ-Session-Gate, Bar-Timestamps aus `data_get_ohlcv`) bleiben technisch in UTC, da die Chart-APIs UTC liefern — nur die für den User sichtbare Anzeige ist die bereits lokal-korrekte `date`-Ausgabe
- Winterzeit-Umstellung (letzter Oktober-Sonntag CEST→CET): danach erneut gegen `date -u` verifizieren, ob die Differenz noch stimmt (+1h statt +2h). **Achtung, die USA stellen an einem anderen Sonntag um** — in der Woche dazwischen gilt ET = DE − 5 h statt − 6 h, siehe Abschnitt „EU/US-Sommerzeit-Divergenz" unten (seit 14.09.2026)

~~Alte, überholte Annahme (bis 27.07.2026 fälschlich angewendet):~~ "Sandbox hat keine Zeitzonendatenbank, `date` liefert immer UTC, manueller Offset per `date -u -d '+2 hours'` nötig" — diese Annahme war Stand 27.07.2026 nicht mehr zutreffend und führte zu einem realen Fehler.

## Zeitbasen-Konvention (ergänzt 03.09.2026, Punkt 24 aus [[project_fable_auftragsliste_2026-09-03]])

Zwei feste Zeitbasen, nie vermischt:
- **Protokolle/Ausgaben** (Loop-Ticks, Voll-Checks, Ereignislisten, Tages-Dateien): durchgehend **DE-Ortszeit** — wie oben seit 22.07.2026, jetzt ausdrücklich auch für jede Ereignisliste im Faktenprotokoll.
- **Statusdateien** (`x_last_fetch.json`, `level_register.json` `updated`, alle maschinenlesbaren Zeitstempel): durchgehend **UTC mit `Z`-Suffix** — nie DE-Ortszeit mit `Z` (exakt dieser Mix schob am 01.09.2026 den Tweet-Wasserstand ~2 h in die Zukunft; `x_fetch_stamp.cjs` und der `gate_check.cjs`-Frische-Guard lehnen das seit 03.09.2026 hart ab).
- **Jede Angabe, die von ihrer Grundkonvention abweicht, wird ausdrücklich gekennzeichnet UND mit DE-Umrechnung ergänzt** — z.B. "14:35Z (= 16:35 DE)". Eine unkommentierte Fremdzeitbasis mitten in einer DE-basierten Liste ("Bis ~14:35 Uhr (Registerzeit)" am 02.09.2026, real 16:35 DE) ist ein Protokollbruch.

## TZ-Umgebungsvariable ist in dieser Sandbox WIRKUNGSLOS (Tatsachen-Befund 14.09.2026, Opus-Verifikation)

**Nur eine Faktenkorrektur zur Doku, keine Regeländerung.** Die Empfehlung oben (bare `date` = DE-Ortszeit) ist am 14.09.2026 erneut live verifiziert und **stimmt weiterhin** — ergänzt wird nur eine Falle, die bisher nirgends dokumentiert war.

Messung (Bash-Tool, 14.09.2026 22:50 DE):

| Befehl | Ergebnis | Bewertung |
|---|---|---|
| `date "+%A, %d.%m.%Y %H:%M (%z)"` | `Monday, 14.09.2026 22:50 (+0200)` | **KORREKT** (CEST) |
| `date -u "+%A %H:%M"` | `Monday 20:50` | korrekt (UTC) |
| `TZ="Europe/Berlin" date "+%H:%M %Z (%z)"` | `20:50 GMT (+0000)` | **FALSCH — 2 h zu früh** |
| `TZ="America/New_York" date "+%H:%M %z"` | `20:50 +0000` | **FALSCH — 6 h daneben** |
| `ls /usr/share/zoneinfo` | `No such file or directory` | keine Zonendatenbank vorhanden |

**Root Cause:** Die Sandbox hat keine tzdata/zoneinfo. Ein gesetztes `TZ=` kann deshalb nicht aufgelöst werden und fällt **still** auf UTC zurück — ohne Fehlermeldung, mit plausibel aussehendem Output. Der bare `date` bekommt seinen korrekten `+0200`-Offset dagegen vom Windows-Host durchgereicht (`%z` stimmt, `%Z` bleibt leer — das leere Zonenkürzel ist normal und kein Defekt).

**Konsequenz — `TZ=` NIE setzen:**
- **Richtig:** `date "+%A, %d.%m.%Y %H:%M (%z)"` → DE-Ortszeit inkl. Wochentag, Offset zur Selbstkontrolle (muss `+0200` in CEST / `+0100` in CET sein) — seit 14.09.2026 als Schritt 0 „Zeitanker Session" im Session-Update verankert (sekundengenaue Variante plus `date -u`-Zeile, siehe [[feedback_session_update]]).
- **Falsch:** `TZ="Europe/Berlin" date …` — ein gesetztes `TZ` macht den sonst korrekten Befehl kaputt.
- **Besonders gefährlich:** `TZ="America/New_York" date` liefert ebenfalls stumm UTC — für US-Sessionzeiten/RTH-Fragen also unbrauchbar. US-Zeit stattdessen aus DE-Zeit ableiten (ET = DE − 6 h, solange beide Sommerzeit haben — **ob das gerade gilt, sagt die Fenster-Tabelle im Abschnitt „EU/US-Sommerzeit-Divergenz" unten; in den dort gelisteten Fenstern ist ET = DE − 5 h**) oder per Node.
- **Robuster Fallback mit echter Zonendatenbank (Node bringt eigenes ICU mit, liefert sogar deutsche Wochentagsnamen):**
  `node -e "console.log(new Date().toLocaleString('de-DE',{timeZone:'Europe/Berlin',weekday:'long',day:'2-digit',month:'2-digit',year:'numeric',hour:'2-digit',minute:'2-digit'}))"` → `Montag, 14.09.2026, 22:50`

## EU/US-Sommerzeit-Divergenz — Fenster, in denen ET = DE − 5 h gilt (KANONISCHE STELLE, ergänzt 14.09.2026)

*(Teil B des Folgeauftrags [[project_fable_auftrag_kalendercheck_dst_2026-09-14]], Fund 3 aus dem Opus-Gegencheck in [[feedback_datum_verifikation_fehler_2026-09-14]]. Der Satz oben „ET = DE − 6 h, solange beide Sommerzeit haben" war richtig, hatte aber keinen Trigger für den Fall, dass sie es **nicht** beide haben. Diese eine Stelle trägt die Tabelle — **alle anderen Dateien verweisen nur hierher** (Prinzip „per Verweis statt per Kopie"; zwei Kopien einer Datumstabelle driften garantiert auseinander). Wer die Tabelle verlängert, tut das NUR hier.)*

**Das Problem in einem Satz:** EU und USA stellen an verschiedenen Sonntagen um (EU: letzter So März / letzter So Oktober; USA: zweiter So März / erster So November). In den Lücken dazwischen beträgt der Versatz **ET = DE − 5 h statt − 6 h**, und alles, was an einem US-Zeitpunkt hängt, liegt in DE-Zeit **eine Stunde früher** — die US-Session öffnet um **14:30 DE statt 15:30 DE**. Das Herbstfenster ist konstruktionsbedingt immer exakt 7 Tage lang (5 Handelstage); das Frühjahrsfenster schwankt je nach Lage der Sonntage zwischen 11 und 23 Tagen (2026: 21 Tage, 2027 und 2028: je 14) — „ungefähr drei Wochen im Frühjahr" beschrieb nur das Jahr 2026, ist keine allgemeine Regel.

### (1) Fenster-Tabelle — bindende Prüfgrundlage (per Node-ICU gemessen am 14.09.2026, nicht gerechnet)

Prüfung = **Datumsvergleich**: Liegt das T0-Datum aus Schritt 0 ([[feedback_session_update]]) auf einem der „betroffenen Handelstage"? Dann ist das Divergenz-Fenster aktiv.

| Fenster | EU stellt um | USA stellt um | betroffene Handelstage | Versatz im Fenster |
|---|---|---|---|---|
| **Herbst 2026 (nächstes)** | So **25.10.2026** (CEST→CET) | So **01.11.2026** (EDT→EST) | **Mo 26.10. – Fr 30.10.2026** (5 HT) | ET = DE − 5 h |
| Frühjahr 2027 | So 28.03.2027 (CET→CEST) | So 14.03.2027 (EST→EDT) | Mo 15.03. – Fr 26.03.2027 (10 HT) | ET = DE − 5 h |
| Herbst 2027 | So 31.10.2027 | So 07.11.2027 | Mo 01.11. – Fr 05.11.2027 (5 HT) | ET = DE − 5 h |
| Frühjahr 2028 | So 26.03.2028 | So 12.03.2028 | Mo 13.03. – Fr 24.03.2028 (10 HT) | ET = DE − 5 h |
| Herbst 2028 | So 29.10.2028 | So 05.11.2028 | Mo 30.10. – Fr 03.11.2028 (5 HT) | ET = DE − 5 h |

*Fußnote: Das Frühjahrsfenster 2026 (Mo 09.03. – Fr 27.03.2026, 15 Handelstage) ist vorbei und steht deshalb nicht in der Tabelle — es erklärt nur, woher die Angabe „≈3 Wochen" im Gegencheck stammt. Außerhalb der gelisteten Fenster gilt ganzjährig ET = DE − 6 h (beide Sommerzeit ODER beide Winterzeit). Präzisierung 15.09.2026 (Fable, Re-Verifikation; am 15.09.2026 von Opus nachgemessen und an den Rändern korrigiert): Die Spalte nennt bewusst nur die Handelstage — physisch beginnt jedes Fenster bereits am **ersten** Umstellungssonntag und umfasst die Wochenendtage dazwischen (z.B. Sa 31.10.2026, Sa/So 20./21.03.2027). **Die beiden Rand-Sonntage sind aber nur zum Teil im Fenster, und zwar asymmetrisch** — das ist für ein Sonntagabend-Update vor dem CFD-Open (ab ca. 23:00 DE) der ganze Unterschied:
> - **Erster Umstellungssonntag = Fenster AKTIV am Abend.** Gemessen: So 25.10.2026 23:00 DE → `GMT+1 | GMT-4` = 5 h; So 14.03.2027 23:00 DE → `GMT+1 | GMT-4` = 5 h.
> - **Zweiter Umstellungssonntag = Fenster am Abend bereits VORBEI.** Die zweite Umstellung passiert früh am Morgen dieses Sonntags (US: 2:00 ET ≈ 07:00 DE; EU: 02:00 UTC = 03:00 DE), also lange vor dem CFD-Open. Gemessen: So 01.11.2026 23:00 DE → `GMT+1 | GMT-5` = 6 h, **kein Fenster**; So 28.03.2027 23:00 DE → `GMT+2 | GMT-4` = 6 h, **kein Fenster**.
>
> Merksatz: **ab dem ersten Umstellungssonntag, bis einschließlich Samstag vor dem zweiten.** Der Sonntag, an dem die zweite Seite umstellt, gehört nicht mehr dazu. Genau das ist der „Fenster-Rand"-Fall, für den (4) den Node-Gegencheck vorsieht — an beiden Rand-Sonntagen wird gemessen, nicht geschlossen.*

**Warum eine datierte Liste bindend ist und nicht die Berechnungsregel (entschieden, nicht zur Disposition):**
1. **Eine Sonntagsberechnung im Kopf ist exakt die Fehlerklasse, gegen die Schritt 0 gebaut wurde.** Der Vorfall vom 14.09.2026 entstand, weil ein Wochentag aus dem Kopf gesetzt statt gemessen wurde. Eine Regel, die „welcher Sonntag ist der zweite im März" als täglichen Denkschritt einbaut, importiert dieselbe Fehlerquelle in die Prüfung, die sie verhindern soll. Ein Datumsvergleich („liegt T0 zwischen X und Y?") ist dagegen ein Vergleich, kein Rechenschritt.
2. **Die Länge des Frühjahrsfensters schwankt** (11–23 Tage). Eine Faustformel im Kopf liefert in den meisten Jahren eine falsche Fenstergrenze.
3. **Eine Liste verfällt still** — ihr einziger echter Nachteil; die Verfallsregel in (5) überführt ihn in einen sichtbaren Abbruch statt in ein stilles Falschergebnis.
4. Es gibt zusätzlich einen **Messweg statt Rechenweg** (Node-Gegencheck in (4)) — „gemessen statt geschätzt", wie überall im Projekt.

### (2) Konsequenztabelle — was im Fenster in DE-Zeit wandert und was nicht

Im Fenster liegt **alles, was an einem US-Zeitpunkt hängt, eine Stunde früher in DE-Zeit**. Alles, was an einem deutschen/europäischen Zeitpunkt hängt (XETRA-Open, ZEW, Ifo, EZB), verschiebt sich **nicht**.

| Anker | normal (DE) | im Divergenz-Fenster (DE) |
|---|---|---|
| US-Open 9:30 ET | 15:30 | **14:30** |
| US-Close 16:00 ET | 22:00 | **21:00** |
| 8:30-ET-Releases (CPI, Core CPI, PCE, PPI, NFP, Jobless Claims, GDP) | 14:30 | **13:30** |
| ADP 8:15 ET | 14:15 | **13:15** |
| 10:00-ET-Releases (ISM, JOLTS, CB Consumer Confidence) | 16:00 | **15:00** |
| FOMC-Entscheid 14:00 ET / Pressekonferenz 14:30 ET | 20:00 / 20:30 | **19:00 / 19:30** |
| Order-Sperre 15:00–15:30 ([[feedback_vollcheck_format]]) | 15:00–15:30 | **14:00–14:30** |
| Halbierungsfenster erste 30 Min nach Open ([[feedback_trading_zeitfenster]]) | 15:30–16:00 | **14:30–15:00** |
| Kern-Entry-Zone (Open+30 Min bis Open+2,5 h) | 16:00–18:00 | **15:00–17:00** |
| Zweites Fenster (vor US-Close) | 20:00–22:00 | **19:00–21:00** |
| 7e-Zone „reguläre US-Session" ([[feedback_live_trading]] 7e) | 15:30–22:00 | **14:30–21:00** |

**AUSLEGUNG, ausdrücklich als solche gekennzeichnet (Levi kann sie im Gegencheck kippen):** Dass die **sessionverankerten** Fenster mitwandern (Order-Sperre, Halbierungsfenster, Kern-Entry-Zone, zweites Fenster), ist eine Auslegung — sie folgt daraus, dass diese Fenster über ihren Abstand zum US-Open definiert und begründet sind (Open-Volatilität, Volumen-Peak am Open, siehe [[feedback_trading_zeitfenster]] und der RVOL-Absatz in [[feedback_live_trading]] Punkt 11), nicht über eine Wanduhrzeit. **Das ist keine inhaltliche Änderung der Handelsregeln:** Die Halbierung bleibt Halbierung, die Sperre bleibt Sperre, die Kern-Entry-Zone bleibt zwei Stunden lang — nur ihre DE-Uhrzeit verschiebt sich an diesen fünf bzw. zehn Handelstagen. Die Release-Zeilen (8:30/10:00 ET, FOMC) und die 7e-Zone sind dagegen keine Auslegung, sondern reine Umrechnung.

### (3) Berechnungsregel — NUR Dokumentation zur Herleitung und Verlängerung der Tabelle

EU: Sommerzeit beginnt am **letzten Sonntag im März**, endet am **letzten Sonntag im Oktober**. USA: Sommerzeit beginnt am **zweiten Sonntag im März**, endet am **ersten Sonntag im November**. Divergenz-Fenster = die Werktage zwischen dem US-Umstellungssonntag und dem EU-Umstellungssonntag (Frühjahr: US zuerst; Herbst: EU zuerst). **Diese Regel dient ausschließlich dazu, die Tabelle in (1) herzuleiten und zu verlängern — sie wird NICHT als täglicher Kopfrechenschritt benutzt.** Täglich gilt nur der Datumsvergleich gegen die Tabelle.

### (4) Node-Gegencheck — Messweg (optional im Normalfall, PFLICHT bei Status UNBEKANNT nach (5))

Node bringt seine eigene Zonendatenbank (ICU) mit — auch in dieser Sandbox, aus demselben Grund, aus dem der Node-Fallback in Schritt 0 funktioniert, während `TZ=` still auf UTC fällt (Abschnitt oben). Der tatsächliche Offset ist damit **messbar**, nicht nur berechenbar:

```
node -e "const d=new Date();const f=(tz)=>new Intl.DateTimeFormat('de-DE',{timeZone:tz,timeZoneName:'shortOffset',hour:'2-digit',minute:'2-digit',hour12:false}).format(d);console.log('DE',f('Europe/Berlin'),'| ET',f('America/New_York'))"
```

Lesart: `GMT+2 | GMT-4` oder `GMT+1 | GMT-5` → Differenz 6 h, kein Fenster. `GMT+1 | GMT-4` → Differenz **5 h, Fenster aktiv**. Für einen Stichtag statt „jetzt": `new Date('2026-10-28T12:00:00Z')` einsetzen.

**Verifikation (Opus 14.09.2026; Fable 14.09.2026 unabhängig wiederholt an allen 18 Fenstergrenzen 2026–2028):** `2026-10-28` → `DE 13:00 GMT+1 | ET 08:00 GMT-4` (5 h, Fenster bestätigt); `2026-11-01` → `ET 07:00 GMT-5` (wieder 6 h, Fenster beendet); `2026-03-08` → `DE GMT+1 | ET GMT-4` (Frühjahrsfenster 2026 offen); `2026-03-29` → `DE GMT+2` (beendet). Fables Messung: jeder erste und letzte Handelstag aller fünf Tabellenfenster liefert `GMT+1 | GMT-4`, jeder Sonntag davor/danach die 6-h-Konstellation — **die Tabelle in (1) ist damit gemessen, nicht geschätzt.**

**Regel dazu:** Im Normalfall genügt der Blick in die Tabelle (ein Datumsvergleich, kein Tool-Call — Schritt 0 bleibt bei genau einem Zeit-Call). Der Node-Aufruf ist der Gegencheck **bei Zweifel, an den Fenster-Rändern (Umstellungswochenende, erster/letzter Fenstertag)** und **verpflichtend** im Fall (5). Weicht die Messung jemals von der Tabelle ab: **nicht** die Tabelle stillschweigend anpassen, sondern melden — dann stimmt eine Annahme nicht und Levi entscheidet.

### (5) Verfallsregel — der Preis der Listenlösung

Die Tabelle deckt bis einschließlich **Herbst 2028** ab (letzter erfasster Termin: So 05.11.2028). Liegt T0 **nach diesem Termin** und wurde die Tabelle nicht verlängert, gilt der Fenster-Status als **UNBEKANNT** — dann ist der Node-Gegencheck aus (4) **Pflicht** (statt optional), das Ergebnis wird ins DST-Suffix der T0-Kopfzeile übernommen UND die Tabelle wird um die fehlenden Zeilen verlängert (Herleitung nach (3), Messung nach (4), Eintrag NUR hier). **„Unbekannt" wird nie stillschweigend als „kein Fenster" behandelt.** Damit wird der einzige echte Nachteil der Merkliste — das stille Verfallen — in einen sichtbaren, handhabbaren Fall überführt.

### (6) Wo die Prüfung ausgelöst wird — und Abgrenzung

- **Auslöser:** das immer vorhandene DST-Suffix an der Pflicht-Kopfzeile `Zeitanker T0: … | DST: …` in Schritt 0 ([[feedback_session_update]]) — keine neue Pflichtzeile, die Durchsetzung wird von der Kopfzeile geerbt. Format und Rechtsfolge stehen dort, nicht hier.
- **Verweise (keine Kopien):** [[feedback_live_trading]] 7e (Zonentabelle; die Delta-Messung bleibt verbindlich), [[feedback_trading_zeitfenster]] (Zeitfenster-Tabelle wandert mit), [[feedback_vollcheck_format]] (Sperr-/Halbierungsmarker mit verschobener Zeit + Grund), [[feedback_session_update]] Schritt 4 (30.07.2026-Spiegelregel: Referenzliste wandert mit; Reihenfolge DST-Status VOR Kalender-Datumscheck K2).
- **Die Zeitbasen-Konvention oben bleibt unberührt:** Statusdateien weiter UTC mit `Z`-Suffix, Ausgaben weiter DE-Ortszeit. Die Divergenz betrifft ausschließlich die **Umrechnung DE ↔ ET** und die daraus abgeleiteten Session-/Release-Uhrzeiten. Der bare `date` liefert DE-Zeit im Fenster genauso korrekt wie sonst — das Fenster ändert nichts an Schritt 0 selbst, nur an der Ableitung von US-Zeiten daraus.
