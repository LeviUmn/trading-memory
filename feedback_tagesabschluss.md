---
name: feedback-tagesabschluss
description: "Am Ende jedes Trading-Tages \"Tag Zusammenfassung speichern\" — strukturiertes Speichern aller Trade-Details, Fehler und Learnings, danach Git-Backup nach GitHub"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 607aa8f6-9958-4c1c-9c75-4afabcffb717
  modified: 2026-08-28T08:38:38.002Z
---

Wenn der User "Tag Zusammenfassung speichern" sagt, immer eine vollständige Tages-Zusammenfassung in einer neuen Memory-Datei speichern.

**Why:** User möchte aus jedem Trading-Tag lernen — Fehler und Erfolge dokumentieren damit das Trading mit der Zeit besser wird.

**How to apply:** Neue Datei erstellen z.B. `trading_YYYY-MM-DD.md` mit folgender Struktur:

```
## Trades
- Entry: Preis, Stückzahl, Kosten
- Nachkauf: (falls vorhanden)
- TP1: Preis, Stückzahl, Gewinn
- SL/TP2: Preis, Stückzahl, Gewinn
- Gesamtgewinn/-verlust

## Was gut lief ✅
- ...

## Fehler / Was besser sein könnte ❌
- ...

## Key Levels für nächsten Tag 📍
- Support: ...
- Resistance: ...

## Learnings 🧠
- ...

## Marktbedingungen
- VIX, Makro-Regime, besondere News
```

Danach MEMORY.md Index aktualisieren.

## Trade-DB-Eintrag (ergänzt 27.07.2026, seit Fable-Build der SQLite-Trade-DB)

Wenn der Tagesabschluss einen oder mehrere abgeschlossene Trades enthält (nicht bei reinen "kein Trade"-Tagen ohne neue Trade-Nummer): zusätzlich zur `trading_YYYY-MM-DD.md`-Datei für JEDEN neuen Trade `node scripts/add_trade.cjs` im Projektordner (`C:\Users\umnus\tradingview-mcp`) ausführen — mit allen bekannten Feldern (Setup, Entry, SL, TP, Hebel, Position, Ergebnis, Outcome, Regelbruch-Flag). Kein automatischer Import aus der Markdown-Datei — das ist ein expliziter, manueller Schritt, den ich (Claude) beim Tagesabschluss selbst ausführe, nicht der User.

**Why:** Die DB (`scripts/trade_db.cjs`+`trade_stats.cjs`) existiert seit 27.07.2026 für abfragbare Statistik (Win-Rate, RR, Erwartungswert, Monte-Carlo-Vorbereitung), bleibt aber leer/veraltet, wenn dieser Schritt vergessen wird — genau das gleiche Muster wie beim Git-Backup unten, das erst nachträglich als fester Bestandteil ergänzt wurde.

**How to apply:** Nach dem Speichern der Tages-Datei, vor dem Git-Backup: `node scripts/add_trade.cjs --date ... --phase ... --dir ... --result-eur ... --outcome ...` (weitere Felder siehe Kopfkommentar der Datei) für jeden neuen Trade des Tages aufrufen, danach zur Kontrolle `node scripts/trade_stats.cjs` laufen lassen und gegen die Markdown-Zahlen abgleichen.

## Reklassifizierung bestehender Trades (ergänzt 12.08.2026)

Reklassifizierung bestehender Trades: Sobald eine Review (Tagesabschluss oder Fable) die Einstufung eines bereits geloggten Trades ändert (Regelbruch neu/entfällt, Outcome-Wechsel), ist die DB-Aktualisierung (`rule_violation`/`outcome`/`notes` für die betroffene Trade-ID) Teil derselben Review-Session, kein Folgeschritt. Pflicht-Abschlusszeile: "DB-Sync: JA (Trade #XX aktualisiert) / NEIN (kein Fall heute)."

**Why:** Genau diese Lücke ließ Trades #9/#12/#31/#34 wochenlang mit einer veralteten Klassifizierung in der DB stehen — die jeweilige Review (u.a. [[feedback_regime_wechsel]], Tagesabschluss 05.08./07.08.) hatte den Regelbruch inhaltlich längst erkannt, aber nie in `rule_violation`/`notes` nachgetragen. Aufgefallen erst beim Phase-1-3-Regelwerk-Audit am 12.08.2026, nicht am Tag der jeweiligen Review selbst. Ein Vorschlag für ein automatisches Backstop-Script (`scripts/check_violation_sync.cjs`, würde Regelbruch-Schlüsselwörter aus den Tages-Dateien automatisch gegen die DB gegenchecken) steht im Raum, ist aber bewusst nicht Teil dieser Änderung — Levi entscheidet separat, ob/wann das gebaut wird.

**Abgrenzung:** Dieser Abschnitt betrifft die Korrektur bereits bestehender, schon geloggter Trade-Einträge. Für die Pflichtprüfung bei JEDEM NEUEN Trade ab jetzt siehe den folgenden Abschnitt "Regelkonformitäts-Pflichtzeile".

## Regelkonformitäts-Pflichtzeile (ergänzt 12.08.2026, Phase-1-3-Regelwerk-Audit)

Für JEDEN Trade des Tages (Win, Loss oder BE) gehört ab sofort eine Pflichtzeile zum Tagesabschluss, parallel zur "DB-Sync: JA/NEIN"-Zeile aus dem Abschnitt "Reklassifizierung bestehender Trades" oben:

`Regelkonformität geprüft: JA — Verstoß: JA/NEIN`

Wird `Verstoß: JA` gesetzt (egal ob der Trade gewonnen oder verloren hat), ist eine Fable-Review noch am selben Tag Pflicht (Sofort-Review), nicht erst bei der nächsten zufälligen Gelegenheit.

**Abgrenzung:** Dieser Abschnitt betrifft die Prüfung bei der Ersterfassung eines neuen Trades — im Unterschied zum Abschnitt "Reklassifizierung bestehender Trades" oben, der die nachträgliche Korrektur bereits geloggter, veralteter Einträge regelt. Beide Pflichtzeilen ("DB-Sync" und "Regelkonformität geprüft") gehören in jeden Tagesabschluss mit Trades, unabhängig voneinander.

**Why:** Volle Herleitung und Zahlen (6 von 6 nachträglich korrigierten Regelverstößen waren ausschließlich Verlust-Trades, kein einziger Gewinn-Trade) siehe [[feedback_regeldisziplin]], Abschnitt "Erkennungs-Zwang + Sofort-Review". Ohne diese Pflichtzeile wird die Regelkonformitätsprüfung bei Gewinn-Trades erfahrungsgemäß stillschweigend übersprungen.

**Ausdrücklich nicht Teil dieser Änderung:** keine automatische Positionsgrößen-Reduktion nach einem Verstoß, keine neue Cooldown-Regel-3 — Levi hat beides am 12.08.2026 explizit abgelehnt, diese Ergänzung ist rein Erkennung + Review, keine automatische Konsequenz.

## SL-Hit-Klassifizierung: Wick/Spike vs. bestätigter Reversal (ergänzt 21.08.2026, Fable-Review nach Trade #42/#43, umgesetzt nach Levi-Entscheidung)

Bei JEDEM SL-Hit gehört ab sofort eine zusätzliche Pflichtzeile zum Tagesabschluss/Sofort-Review, parallel zu "Regelkonformität geprüft" oben:

`SL-Hit-Typ: Wick/Spike (kein Kerzenschluss) ODER bestätigter Kerzenschluss — Punkt-11-Kriterien zum SL-Zeitpunkt: X/4 erfüllt`

War der SL primär durch einen Wick/Spike ausgelöst (kein 5-Min-Kerzenschluss jenseits des SL-Levels) UND waren die Punkt-11-Reversal-Kriterien ([[feedback_live_trading]]) zum Auslöse-Zeitpunkt NICHT vollständig (2-3/4) erfüllt, gilt das explizit als "SL-Hit ≠ bestätigtes Reversal" — der Trade-Ausgang (Verlust) bleibt unverändert bestehen, aber die Review-Sprache darf daraus NICHT automatisch ableiten, dass die zugrunde liegende Markteinschätzung/These falsch war. Beides wird getrennt bewertet: das Ergebnis (Geld verloren, zählt) und die Frage, ob die These inhaltlich widerlegt wurde (nur bei erfüllten Punkt-11-Kriterien der Fall).

**Why:** Trade #42/#43 (21.08.2026) wurden beide durch Wicks in der Zone 29.240-254 ausgestoppt, während zum Zeitpunkt des jeweiligen SL-Hits nur 1-2 von 4 Punkt-11-Kriterien erfüllt waren — ein Fable-Live-Check (21.08., ~35-40 Min NACH beiden SL-Hits) zeigte NAS100 5min und QQQ 15min beide noch unterhalb ihrer jeweiligen EMA50, also weiterhin nicht bestätigt. Der Fable-Review vom 21.08.2026 stellte fest, dass "SL wurde getroffen" in der Live-Kommunikation faktisch mit "der Markt hat die Short-These widerlegt" gleichgesetzt wurde, obwohl das nach den eigenen Kriterien zum Zeitpunkt nicht belegt war. Diese Vermischung erschwert eine ehrliche Nachbesprechung: ein Verlust durch eine unglückliche SL-Platzierung in einer mehrfach getesteten Zone (siehe [[feedback_chartanalyse]] Punkt 8c2) ist ein anderes Problem als ein Verlust durch eine tatsächlich falsche Markteinschätzung, und beide brauchen unterschiedliche Lehren.

**How to apply:** Bei jedem SL-Hit im Tagesabschluss/Sofort-Review zuerst prüfen, ob der letzte relevante 5-Min-Kerzenschluss VOR dem SL-Hit bereits jenseits des SL-Levels lag (dann: bestätigter Schluss, kein reiner Wick) oder ob der SL nur durch das Hoch/Tief innerhalb einer Kerze berührt wurde, ohne dass der Schlusskurs selbst das Level bestätigte (dann: Wick/Spike). Danach den Punkt-11-Stand zum exakten SL-Zeitpunkt dokumentieren (nicht rückwirkend zum späteren, ggf. schon weiter entwickelten Stand). Fehlt diese Zeile bei einem SL-Hit-Trade, gilt der Tagesabschluss als unvollständig — gleiche Behandlung wie die bestehende "Regelkonformität geprüft"-Zeile.

**Abgrenzung zu [[feedback_live_trading]] Punkt 11 (Volumen-Kriterium):** Das dort am 21.08.2026 ergänzte fünfte, dämpfende Volumen-Kriterium wirkt VOR/WÄHREND der Position (soll einen verfrühten Dreh-Vorschlag verhindern). Diese Regel hier wirkt NACH dem SL-Hit, in der Nachbesprechung — beide entstanden aus demselben Fable-Review, adressieren aber unterschiedliche Zeitpunkte im Prozess.

## Auflösen offener skipped_setups-Einträge (ergänzt 24.08.2026 Abend, Phase 0 der Studie "bessere Trades")

Vierter Pflichtpunkt beim Tagesabschluss, parallel zu "DB-Sync", "Regelkonformität geprüft" und "SL-Hit-Typ" oben: Für JEDES am Handelstag mit `scripts/add_skipped_setup.cjs` erfasste ausgelassene Setup (Tabelle `skipped_setups`, siehe [[project_studie_bessere_trades_2026-08-24]] Abschnitt 6) muss `hypothetisches_ergebnis` noch am selben Tagesabschluss nachgetragen werden — nicht offen bleiben.

**How to apply:** Pro offenem Eintrag mit `hypothetisches_ergebnis IS NULL` und `datum` = heute: `data_get_ohlcv` für den Rest der Session nach dem geplanten `uhrzeit`-Zeitpunkt prüfen — wäre der geplante Preis (`geplanter_entry`/`geplanter_sl`/`geplantes_tp1`) tatsächlich TP1, TP2, SL oder keins von beidem (`WEDER_NOCH`) getroffen worden? Danach direkt per DB-Write eintragen:

```
node -e "const {getDb,updateSkippedResult}=require('./scripts/trade_db.cjs'); const db=getDb(); updateSkippedResult(db, <id>, '<TP1|TP2|SL|WEDER_NOCH>'); db.close();"
```

Kein separates Update-Skript nötig für diesen einmaligen Nachtrag — `updateSkippedResult()` aus `trade_db.cjs` reicht. Offene Einträge vorher mit `node -e "const {getDb,allSkippedSetups}=require('./scripts/trade_db.cjs'); console.log(allSkippedSetups(getDb()).filter(s=>!s.hypothetisches_ergebnis))"` auflisten.

**Pflicht-Abschlusszeile:** `Skipped-Setups aufgelöst: JA (X von X) / NEIN (kein Fall heute)`.

**Why:** Genau diese Zahl (was hätten ausgelassene Setups gebracht) beantwortet Levis eigentliche Frage künftig per Query statt per Gedächtnis — bleibt sie unbefüllt, entsteht dieselbe stille Datenlücke, die bei `tp1_hit` für #31/#32 bereits einmal passiert ist (siehe [[project_studie_bessere_trades_2026-08-24]]).

**Abgrenzung:** Kein Voll-Check-Schritt, keine neue Pflichtzeile im 1-Minuten-Loop — ausschließlich ein Tagesabschluss-Schritt, analog zu den drei bestehenden Pflichtpunkten oben.

## SQL-Dump nach jeder DB-Änderung aktuell halten (ergänzt 24.08.2026, N-3-Fix, siehe [[project_opus_vollpruefung_2026-08-24]])

Fünfter Pflichtpunkt beim Tagesabschluss, parallel zu "DB-Sync", "Regelkonformität geprüft", "SL-Hit-Typ" und "Skipped-Setups aufgelöst" oben: Nach JEDEM `add_trade.cjs`-Aufruf (neuer Trade) UND nach jedem `gate_check.cjs --trade-id`-Rückschreiben des Tages `node scripts/dump_trades_sql.cjs > scripts/trades.sql` (NAS100) bzw. `node scripts/dump_trades_sql_dax.cjs > scripts/trades_dax.sql` (DAX, falls DAX-Trades betroffen) erneut ausführen und den aktualisierten Dump im selben Git-Backup mitcommitten.

**Why:** `scripts/trades.db`/`trades_dax.db` sind absichtlich in `.gitignore` (Binärdatei, kein sinnvoller Diff) — `trades.sql`/`trades_dax.sql` sind der einzige versionierte, diffbare Stand der Trade-Historie. Bis 24.08.2026 stand das Nachziehen nur im Kopfkommentar von `dump_trades_sql.cjs` selbst, nicht hier — der Dump war dadurch bereits einmal 7 Spalten + die komplette `skipped_setups`-Tabelle veraltet (Stand 24.08. Mittag vs. Abend), ohne dass das an einer Pflichtstelle aufgefallen wäre. Diese Ergänzung schließt genau diese Lücke, analog zum bereits bestehenden Muster der anderen vier Pflichtpunkte oben.

**Pflicht-Abschlusszeile:** `SQL-Dump aktualisiert: JA / NEIN (kein DB-Schreibzugriff heute)`.

**Bestandsaufnahme (N-22, 24.08.2026, reine Zählung, KEINE Kürzung — das wäre eine eigene Entscheidung):** Der Tagesabschluss hat damit aktuell **sechs** explizite Pflicht-Abschlusszeilen (DB-Sync, Regelkonformität geprüft, SL-Hit-Typ [nur bei SL-Hit], Skipped-Setups aufgelöst, SQL-Dump aktualisiert, B1-B3-Schattenmessung geprüft) plus zwei strukturelle Pflichtschritte ohne eigene Ja/Nein-Zeile (`add_trade.cjs`-Eintrag pro neuem Trade, Git-Commit+Push des `memory/`-Ordners). Diese Zahl ist bewusst nur dokumentiert, nicht bewertet — ob das zu viel/zu wenig ist, ist eine eigene, hier nicht getroffene Frage (zu unterscheiden von den *Live-Loop*-Pflichtzeilen in [[feedback_live_trading]]/[[feedback_chartanalyse]], die eine andere, deutlich größere Zählung haben und hier nicht mitgezählt sind).

## Auflösen offener B1-B3-Dual-Gate-Schattenmessung (ergänzt 25.08.2026, nach Opus-Zweitreview + Levi-Entscheidung)

Sechster Pflichtpunkt beim Tagesabschluss, parallel zu "DB-Sync", "Regelkonformität geprüft", "SL-Hit-Typ", "Skipped-Setups aufgelöst" und "SQL-Dump aktualisiert" oben: Für JEDEN am Handelstag abgelehnten/ausgelassenen Dual-Gate-Fall muss geprüft werden, ob das B1-B3-Schattenmessungs-Paket vollständig erfasst ist — nicht offen bleiben, damit die Daten bis zum #50-Review lückenlos vorliegen.

Konkret pro Fall:
- **B1:** War die Situationsklasse "Basis-Reclaim nach Session-Extrem" (5 Bedingungen, siehe [[feedback_chartanalyse]] 8a4) erfüllt? Falls ja, muss das korrekt im `--grund`-Feld von `add_skipped_setup.cjs` vermerkt sein.
- **B2:** Sind die beiden erweiterten Dual-Gate-Schattenwerte (`--dg-ema50-5min-diff-pct`, `--dg-vwap-band1-diff-pct`) nachgetragen, sofern die Rohdaten dafür noch verfügbar sind — zusätzlich zu den bereits bestehenden 15min-Werten (`--dg-abstand-pct`, `--dg-abstand-atr`, `--dg-q2-budget-pct`)?
- **B3:** Bei Fällen mit klar erkennbarem Impuls-Ursprung: ist der B3-Doppelanker-Wert in `gate_check.cjs` ergänzt? Rein informativ, kein Pflichtfeld — ändert nie den Ampel-Status.

**How to apply:** Dieselbe Fall-Liste durchgehen wie beim vierten Pflichtpunkt oben ("Skipped-Setups aufgelöst") — pro Dual-Gate-bedingtem Fall des Tages die drei Punkte B1/B2/B3 prüfen und, soweit die Rohdaten das hergeben, direkt nachtragen.

**Pflicht-Abschlusszeile:** `B1-B3-Schattenmessung geprüft: JA (X Fälle nachgetragen/vervollständigt) / NEIN (kein Dual-Gate-Ablehnungsfall heute)`.

**Why:** Der gesamte Wert von B1-B3 hängt daran, dass die Daten bis zum Review nach Trade #50 vollständig vorliegen — siehe [[project_testtag_analyse_2026-08-24]] Abschnitt 10/10a. Ohne festen Prüfpunkt entsteht sonst dieselbe Art stiller Datenlücke wie beim vierten Pflichtpunkt (dort: unbefüllte `hypothetisches_ergebnis`-Werte), die dann erst beim #50-Review auffällt, wenn sie nicht mehr zu schließen ist.

**Abgrenzung:** Kein Voll-Check-/1-Minuten-Loop-Schritt, ausschließlich Tagesabschluss. Eng verwandt mit, aber inhaltlich getrennt von "Skipped-Setups aufgelöst" oben: dort geht es um das hypothetische Ergebnis (TP1/TP2/SL/WEDER_NOCH) eines ausgelassenen Setups, hier um die Dual-Gate-Messwerte/Klassenzugehörigkeit (B1-B3) desselben Falls — beide Pflichtzeilen können für denselben Eintrag anfallen, sind aber unabhängig voneinander zu prüfen.

## Faktenprotokoll-Abschluss statt "Tagesabschluss" im Loop-Protokoll (ergänzt 28.08.2026, Opus-Vorschlag 10 aus [[project_testtag_analyse_2026-08-27]], Levi-Go nach Opus-Zweitklärung)

**Geltungsbereich:** Der letzte Abschnitt, den SONNET am Ende eines Loop-/Testtags ins Protokoll schreibt (bisher überschrieben mit "Tagesabschluss <Datum>"). NICHT betroffen ist die von Fable erstellte analytische Tages-Zusammenfassung (`trading_YYYY-MM-DD.md`, Struktur oben) — die bleibt bewertend, denn sie kommt vom Prüfer/Autor-Modell, nicht vom Ausführenden.

**Die Regel:** Der Abschnitt heißt ab jetzt **"Faktenprotokoll-Abschluss <Datum>"** und enthält AUSSCHLIESSLICH Fakten in fünf festen Unterpunkten:

1. **Zahlenbilanz** — Loop-Fenster, Anzahl Voll-Checks (aus der Uhrzeit berechnet, Lücken benannt), Quick-Ticks, Screenshots inkl. Zeitpunkt des letzten, Tweet-Fetches mit Zeitstempeln, Trigger-Momente, Trades, neue `skipped_setups`-Zeilen.
2. **Ereignisliste, chronologisch** — Uhrzeit, gemessener Wert, was geschah, Entry/kein Entry, welche Regelnummer angeführt wurde.
3. **Skript-Aufrufe** — jeder Aufruf mit wörtlicher Ausgabezeile (`gate_check.cjs`, `cooldown_check.cjs`, `add_skipped_setup.cjs`).
4. **Brüche und Unterlassungen** — ausgefallene Checks, Screenshot-Aussetzer, Template-/Session-Wechsel, fehlende Pflichtzeilen (Umsetzung der Offenlegungspflicht für Protokollbrüche, [[feedback_live_trading]] Punkt 9).
5. **Offene/unklare Punkte** — ohne Auflösungsversuch.

Fester Schlusssatz des Abschnitts: **"Bewertung und Einordnung erfolgen durch Fable/Opus, nicht in diesem Protokoll."**

**Verbotene Satztypen (explizit):** Gewichtungen ("wichtigster/stärkster/sauberster Vorfall"), Selbstbescheinigungen ("regelkonform", "korrekt", "diszipliniert", "sauber gehandhabt"), Vorgaben an den Prüfer ("zentraler Datenpunkt für das Review", "besonders zu beachten"), Lehren/Fazit ("die Lehre des Tages ist …"), Ursachendeutungen ("weil das Dual-Gate funktioniert hat"). **Wichtige Abgrenzung:** Der Verweis auf eine Regel ist ein Fakt ("Entry unterlassen, Begründung im Protokoll: kein frischer 8a2-Cross, Punkt 7b1") — die Schlussfolgerung daraus ("war korrekt") ist ein Urteil und verboten. Und ausdrücklich: **Ein selbst gefundener Prozessfehler ist ein Fakt und bleibt Pflicht** — diese Regel darf die Selbstoffenlegung nicht reduzieren (am 27.08. lag sie ohnehin bei null).

**Why:** Am Testtag 27.08.2026 enthielt Sonnets "Tagesabschluss" trotz Levis expliziter Vorgabe von 18:14 Uhr ("Sonnet bewertet den Tag NICHT selbst") drei Bewertungen: "Wichtigster Vorfall des Tages" (Gewichtung), "der zentrale Datenpunkt für das Opus-Review" (Prüfer-Steuerung) und "beide Situationen wurden regelkonform ohne Entry aufgelöst" (Selbstbescheinigung — die zudem unhaltbar war: 5 versäumte Pflicht-Fetches, Spike-Ausnahme 0/35). Die Rollentrennung ([[feedback_modellwahl_trading]]) existiert genau deshalb, weil Autor und Prüfer nicht dasselbe Modell sein dürfen — rahmt der Ausführende den Tag vorab als "regelkonform", muss der Prüfer diese Rahmung erst abräumen, statt neutral zu lesen. Quelle: [[project_testtag_analyse_2026-08-27]] Abschnitt 4.4 + 14 Vorschlag 10, Opus-Zweitklärung 28.08.2026.

## Git-Backup nach jedem Tagesabschluss (ergänzt 23.07.2026)

Nach dem Speichern der Tages-Zusammenfassung UND dem MEMORY.md-Update: den kompletten `memory/`-Ordner committen und zu `github.com/LeviUmn/trading-memory` (Private Repo) pushen.

**Why:** User-Entscheidung 23.07.2026 nach Einrichtung des Backups — ein Trading-Tag ist der natürliche Rhythmus, weil an diesem Punkt ohnehin schon alle Learnings/Regel-Änderungen des Tages fertig dokumentiert sind, kein separater Trigger nötig.

**How to apply:**
```
cd "<Pfad zu memory/>"
git add -A
git commit -m "<kurze Zusammenfassung des Tages, z.B. 'Session 23.07.2026 — ATR-Umstellung, Regel-Audit'>"
git push
```
Falls an dem Tag keine echten Trades liefen, aber Regel-/Memory-Änderungen gemacht wurden (wie am 23.07.2026), gilt derselbe Rhythmus — der Trigger ist der Tagesabschluss selbst, nicht ob ein Trade stattfand.
