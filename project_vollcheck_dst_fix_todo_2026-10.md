---
name: project-vollcheck-dst-fix-todo-2026-10
description: "TO-DO faellig vor 26.10.2026: scripts/vollcheck.cjs Z. 401-404 verdrahtet Order-Sperre/Halbierungsfenster fest auf 15:00/15:30/16:00 DE -- im EU/US-Sommerzeit-Divergenz-Fenster (naechstes: Mo 26.10.-Fr 30.10.2026, siehe feedback_zeitzone.md) laufen beide real eine Stunde frueher. Kein Gate-Loch (7b1/7e binden weiter), aber Skript meldet in der ersten realen Handelsstunde faelschlich 'kein Sperr-/Halbierungsfenster'. Von Opus im Gegencheck 15.09.2026 gefunden und bestaetigt, bewusst nicht gefixt (ausserhalb Gegencheck-Mandat)."
metadata:
  node_type: memory
  type: project
  status: "TO-DO, faellig vor 26.10.2026, noch nicht begonnen"
  originSessionId: a2b3dbfe-4ee4-466f-855e-65c519af6e1f
  modified: 2026-09-15T02:00:00.000Z
---

# TO-DO vor 26.10.2026: vollcheck.cjs DST-Fenster nachruesten

**Herkunft:** [[feedback_datum_verifikation_fehler_2026-09-14]], Abschnitt „Opus-Gegencheck Kalendercheck+DST, 15.09.2026", Punkt 8.

## Befund

`scripts/vollcheck.cjs` Z. 401-404:
```
const { h: deH, m: deM } = deParts(jetztMs);
const orderSperre = deH === 15 && deM < 30;
const halbierung = deH === 15 && deM >= 30;
const halbierungAbgelaufen = deH === 16 && deM < 5;
```
`deParts()` rechnet die DE-Zeit korrekt (Node-ICU, `Europe/Berlin`) — der Fehler sind die **hart verdrahteten Stunden 15/16**. Im EU/US-Sommerzeit-Divergenz-Fenster (siehe [[feedback_zeitzone]], Fenster-Tabelle) laufen Order-Sperre (14:00-14:30 DE) und Halbierungsfenster (14:30-15:00 DE) real eine Stunde frueher. Das Skript wuerde beide Marker eine Stunde zu spaet setzen — in der ersten realen Handelsstunde des Fensters meldet es faelschlich "kein Sperr-/Halbierungsfenster", waehrend beide aktiv sind, und setzt sie spaeter, wenn sie laengst vorbei sind.

## Dringlichkeit

Kein Schaden vor dem **naechsten Fenster Mo 26.10.-Fr 30.10.2026**. Kein Gate-Loch (Entry-Gate haengt an 7b1/7e, nicht an diesen Markern). Trotzdem: eigener kleiner Code-Auftrag, **faellig vor dem 26.10.2026**, nicht auf den letzten Drueck.

## Empfohlener Loesungsweg (Opus-Vorschlag, nicht entschieden)

Saubere Variante passend zum Projektprinzip "gemessen statt geschaetzt": Offset zur Laufzeit per Node-ICU aus dem `--jetzt`-Zeitanker messen (`Intl.DateTimeFormat` mit `timeZoneName:'shortOffset'` fuer `Europe/Berlin` und `America/New_York`, Differenz 5 h = Divergenz-Fenster aktiv), statt die Fenster-Tabelle als Konstante in den Code zu kopieren (Doppelpflege-Risiko gegenueber [[feedback_zeitzone]]).

**Buendelungsvorschlag (Opus):** Zusammen mit dem noch offenen DAX-Wasserstand-Fund (`feedback_session_update_dax.md` Z. 42, Fund 2 aus dem 14.09.-Gegencheck, DAX-Block ruht) angehen, sobald der DAX-Block wieder anlaeuft — beides ist "kleine Nacharbeit, kein aktueller Blocker".

## Naechster Schritt

Wenn faellig (spaetestens Anfang/Mitte Oktober 2026, rechtzeitig vor dem 26.10.): Opus schreibt einen praezisen Fable-Auftrag (analog zum Stil der bisherigen Schritt-0-/Kalendercheck-Auftraege), Fable setzt um, Opus-Gegencheck, dann Levi-Freigabe zum Commit/Push. Keine Umsetzung vor dieser Kette.
