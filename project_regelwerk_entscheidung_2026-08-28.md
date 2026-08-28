---
name: project_regelwerk_entscheidung_2026-08-28
description: "Levis Entscheidung zu den 10 Vorschlägen aus dem Opus-Review des Testtags 27.08.2026 ([[project_testtag_analyse_2026-08-27]]). Auftrag an Fable zur Umsetzung — noch nicht übergeben, da am 28.08.2026 keine Fable-Session erreichbar war. Punkt 9+10 bewusst zurückgestellt (Klärung mit Opus separat)."
metadata:
  node_type: memory
  type: project
  status: done
  originSessionId: session-2026-08-28
  modified: 2026-08-28T08:28:05.219Z
---

# Levi-Entscheidung zu den Opus-Vorschlägen (27.08.2026-Review), 28.08.2026

Quelle der 10 Vorschläge: [[project_testtag_analyse_2026-08-27]] Abschnitt 14. Levi hat sie am 28.08.2026 durchgesprochen und wie folgt entschieden. **Nichts davon ist umgesetzt — das ist ein Auftrag an Fable (Regelwerk-Autorschaft), keine Selbstumsetzung durch Sonnet.**

## Punkt 1 — EIGENE Levi-Fassung, weicht von Opus' Formulierung ab

Opus schlug ein technisches Fix vor (maschineller Zeitstempel + Delta-Ausgabe, `x_touch.cjs`). Levis Diagnose geht tiefer und ersetzt die zugrunde liegende Fälligkeits-Logik:

**Root Cause laut Levi:** Der Tweet-Abruf-Fälligkeitscheck zählt aktuell die Minuten seit dem letzten TATSÄCHLICHEN Abruf-Zeitpunkt (Ist-Zeit). Weil dieser Ist-Zeitpunkt selbst nicht exakt im Raster liegt (Verzögerungen, Session-Wechsel etc.), verschiebt sich mit jedem Abruf auch der Bezugspunkt für den nächsten — das erklärt die am 27.08. beobachtete Drift und die 5 versäumten Fetches (siehe [[project_testtag_analyse_2026-08-27]] Abschnitt 2).

**Vorgabe:** Der Zeitstempel/die Fälligkeit für den X-Abruf muss sich **immer an der abgeschlossenen 5-Minuten-Kerze orientieren, nicht an der Ist-Zeit des letzten Abrufs**. Konkret: Abruf ist fällig bei jeder zweiten abgeschlossenen 5-Min-Kerze (= festes Kerzenraster, alle 10 Minuten am Raster verankert — z.B. :00/:10/:20 etc.), nicht als rollierendes Delta seit dem letzten Ist-Abruf. Damit kann sich der Bezugspunkt nicht mehr verschieben, egal wann der tatsächliche Abruf stattfand.

**Für Fable:** Diese Vorgabe ersetzt Opus' technischen Lösungsvorschlag zu Punkt 1 (der reine Zeitstempel-Genauigkeits-Fix wäre bei einer Ist-Zeit-Delta-Logik ohnehin nur ein Teilfix gewesen). Bitte `feedback_live_trading.md` (Punkt 2b/9) entsprechend neu formulieren: Fälligkeit = Kerzenraster-basiert, nicht Delta-seit-Ist-Abruf-basiert.

## Punkt 2-4 — wie von Opus vorgeschlagen umsetzen

- **Punkt 2:** Verbliebene Ordinal-Fälligkeit in `feedback_live_trading.md` Punkt 9 ("bleibt bei jedem 2. Voll-Check (alle 10 Min)") streichen — wird durch die neue, einheitliche Kerzenraster-Regel aus Punkt 1 ohnehin ersetzt/obsolet, keine zwei parallelen Mechaniken mehr.
- **Punkt 3:** Skript-Aufrufe im Protokoll wörtlich belegen — jetzt auch für `cooldown_check.cjs` (nicht nur 🟢/🔴, sondern die tatsächliche Ausgabezeile), analog zur bereits für `gate_check.cjs` geltenden Pflicht.
- **Punkt 4:** `Regime-Gate (8d)`-Zeile auf ihr vorgeschriebenes Format zurückholen: `Regime-Gate (8d): Schock-Tag ✓/✗ (n/3) | Regime: Trend/Chop` — der Kriterien-Zähler (n/3) bleibt Pflicht-Bestandteil, die Trend/Chop-Zusatzaussage steht separat daneben, ersetzt sie aber nicht. Gleiches Prinzip (Rohwert statt Schätzung/„implizit") für die ADX- und RSI-Zeilen durchsetzen.

## Punkt 5 — wie von Opus vorgeschlagen umsetzen (Option i)

Die drei order-gebundenen Anzeige-/Schattenmessungs-Klassen (`Basis-Reclaim-Klasse` 8a4, `Stale-Check` 7b1c, `Spike-Ausnahme`) werden künftig an **Trigger-Momente** gebunden statt an tatsächliche Orders — also immer dann fällig, wenn eine Dual-Gate-Hälfte frisch per Kerzenschluss schließt oder ein benanntes Level (Pivot etc.) bricht, unabhängig davon, ob daraus ein Entry wird. Ziel: `skipped_setups` und die Paket-6-Schattenmessung bekommen auch an Null-Trade-Tagen Datenpunkte, statt bei n=0 zu verharren.

## Punkt 6-8 — wie von Opus vorgeschlagen umsetzen

- **Punkt 6:** Doppeldeutigkeit von „stale" auflösen. Für den reinen Trigger-Zustand („kein frischer Cross verfügbar") künftig ein anderes Wort verwenden; „Stale" ausschließlich für die formale 7b1c-Definition (altes Gate UND Markt läuft dagegen) reservieren.
- **Punkt 7:** Zustandsänderungen im Protokoll immer offenlegen, auch technische/formatliche (z.B. Aussetzen von Screenshots, Wechsel des Ausgabetemplates) — analog zur bereits vorbildlichen CDP-Abbruch-Meldung vom 27.08. 18:05 Uhr.
- **Punkt 8:** Voll-Check-Nummerierung aus der Uhrzeit ableiten (z.B. Minuten seit Loop-Start / 5), nicht manuell mitzählen — damit Lücken (wie die ausgefallene 19:15-Marke am 27.08.) sofort auffallen statt durch eine falsche fortlaufende Zählung verdeckt zu werden.

## Punkt 9 + 10 — zurückgestellt

Wird von Levi noch separat mit Opus geklärt (Testtag-Auswahlkriterium „mindestens eine Gate-Öffnung" bzw. Replay-Ansatz; Rollentrennung/Faktenprotokoll-Format im Tagesabschluss). **Nicht Teil dieses Fable-Auftrags.**

---

**Übergabestatus: ERLEDIGT (28.08.2026, Fable — Levi wechselte per `/model` in derselben Session auf Fable 5).** Alle Punkte 1-8 umgesetzt:

- **Punkt 1+2 (Kerzenraster-Fix):** `feedback_live_trading.md` — neuer autoritativer Absatz "Kerzenraster-Fix" im Abschnitt "X-Tweets als Bestandteil des Voll-Checks" (Fälligkeit = echte Minute % 10 == 0 am festen 5-Min-Kerzenraster, `x_last_fetch.json` nur noch Verpasst-Erkennung/Nachhol-Regel). Alte Delta-Mechanik an allen 4 weiteren Stellen entwertet/umgeleitet: CronCreate-Prompt Item (2), Loop-Inhaltsliste, Punkt 9 Anti-Drift (b), Punkt 9a How-to-apply. Der 25.08.-Modulo-Einwand ist explizit als gegenstandslos abgegrenzt (Voll-Checks liegen per CronCreate auf % 5 == 0).
- **Punkt 3:** CronCreate-Prompt Item (7) — `cooldown_check.cjs`-Ausgabezeile wörtlich Pflicht, "🟢" allein genügt nicht.
- **Punkt 4:** CronCreate-Prompt Items (5)/(6) + `feedback_vollcheck_format.md` — 8d-Zeile `Schock-Tag ✓/✗ (n/3) | Regime: Trend/Chop` (Zähler Pflicht), Rohwert-Pflicht für ADX/RSI/MACD-H/EMA50 (abgelesener Wert oder explizit "nicht gelesen", nie "~"/"implizit"/Spanne).
- **Punkt 5:** Neuer Block "Bei jedem TRIGGER-MOMENT" im CronCreate-Prompt (Definition: frischer Dual-Gate-Hälften-Kerzenschluss ODER bestätigter Levelbruch) — Spike-Ausnahme (10), Basis-Reclaim (11), Stale-Check (11a) dorthin verschoben, order-unabhängig; Trigger-Moment ohne Entry → `add_skipped_setup.cjs` in echten Sessions (Skript-Existenz verifiziert). 7b1c-Ausgabe-Absatz entsprechend erweitert, Trigger-Momente zählen für die Kalibrierungs-Review.
- **Punkt 6:** Terminologie-Regel in 7b1c — "stale" exklusiv für die 7b1c-Klasse, Trigger-Zustand heißt "kein frischer Cross" (mit Prüfpflicht gegen tatsächliche Kerzenschlüsse, wegen des unerkannten NAS100-Reclaims 17:55/18:00 am 27.08.).
- **Punkt 7:** Neue "Offenlegungspflicht für Protokoll-/Beobachtungsbrüche" in Punkt 9 (Einzeiler bei Screenshot-Aussetzern, Template-Wechseln, ausgefallenen Checks, Session-Wechseln).
- **Punkt 8:** Voll-Check-Nummer aus Uhrzeit berechnet (Nr. = Minuten seit erstem Voll-Check / 5 + 1), Kopfzeilen-Template in `feedback_vollcheck_format.md` angepasst, Lücken werden explizit benannt.

**Offen bleiben Punkt 9+10** (Testtag-Auswahl/Replay bzw. Rollentrennung im Tagesabschluss) — Levi klärt sie separat mit Opus.
