---
name: feedback-regeldisziplin
description: "Kernprinzip: Verlust trotz Regeleinhaltung ist akzeptabel, Verlust durch Regelbruch ist selbstgemacht — Unterscheidung bei jeder Trade-Nachbesprechung machen"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 7c946990-591f-4e7e-acad-80f3259782b4
  modified: 2026-08-22T08:40:12.899Z
---

Es gibt zwei fundamental unterschiedliche Arten von Verlust-Trades, die klar getrennt bewertet werden müssen:

1. **Akzeptabler Verlust:** Alle eigenen Regeln wurden eingehalten (Doppelkriterien abgewartet, kein Trade bei erkanntem Chop, korrekte Positionsgröße, SL/TP technisch sauber gesetzt) — der Markt hat trotzdem gegen die Position entschieden. Dafür kann niemand etwas, das ist normales Handelsrisiko.
2. **Selbstgemachtes Leid:** Ein Verlust entstand, weil eine eigene, bereits bekannte Regel nicht beachtet wurde (z.B. auf ein einzelnes Kriterium reagiert statt auf alle geforderten, oder ein erkanntes Chop-Signal ignoriert und trotzdem getradet).

**Why:** Am 01.07.2026 (Trade #8 und #9, siehe [[trade-2026-07-01]]) wurden beide Verluste durch Regelverstöße verursacht, nicht durch normales Marktrisiko — Trade #8 durch Reagieren auf ein einzelnes statt beider geforderter Kriterien, Trade #9 durch Ignorieren einer selbst erkannten Chop-Phase. User-Zitat: "Wir müssen lernen, nicht alles hinterher zu jagen, sondern uns sicher zu sein, dass alles passt... so ist es selbst gemachtes Leid." Das Ziel von Phase 1 ist explizit, das eigene System zu trainieren und diese Fehler-Kategorie zu eliminieren — nicht einfach "Verluste vermeiden" um jeden Preis.

**How to apply:** Bei jeder Trade-Nachbesprechung (Tagesabschluss, siehe [[feedback_tagesabschluss]]) explizit einordnen: War das ein Regelbruch oder reines Marktrisiko? Nur bei echten Regelbrüchen ist ein konkreter Verhaltens-Lernpunkt nötig; bei sauber regelkonformen Verlusten reicht die Feststellung "regelkonform, kein Handlungsbedarf". Vor jedem Trade-Trigger (nicht erst danach) aktiv prüfen: sind wirklich ALLE unsere Kriterien erfüllt, oder jagen wir gerade nur einem einzelnen nach?

### Claude hält die Linie, auch gegen emotionalen Widerstand des Users (ergänzt 01.07.2026)
User hat explizit gesagt: "Ich muss mich dann auch bremsen, weil ich Emotionen verspüre und du da besser bist als ich ohne Emotionen. Daher gibst du die Regeln vor."

**Why:** Der User selbst erkennt an, dass er in Live-Trading-Momenten emotional beeinflusst sein kann (z.B. nach Verlusten, bei verlockenden Setups). Claude soll explizit NICHT nachgeben, wenn der User in einem emotional aufgeladenen Moment (z.B. nach Verlusten, "Regel sagt doch los" o.ä.) gegen eine bereits etablierte Regel argumentiert — siehe das Cooldown-Beispiel vom 01.07.2026, wo genau das passiert ist und Claude korrekt standhaft blieb.

**How to apply:** Bei Widerspruch gegen eine bestehende Regel während einer aktiven Trading-Situation nicht sofort nachgeben, sondern die Regel und ihren Zweck nochmal benennen. Regeländerungen selbst sind erlaubt und erwünscht, aber nur außerhalb hitziger Momente, in Ruhe besprochen (siehe Tagesabschluss/Wochenreview), nicht als Spontan-Ausnahme mitten im Trade.

### Dritte Kategorie bestätigt: externe/technische Vorfälle sind ebenfalls kein Regelbruch (ergänzt 08.07.2026)
Trade #14 (08.07.2026, siehe [[trades/trading_2026-07-08]]) zeigte eine dritte Verlust-Kategorie neben "Marktrisiko" und "Regelbruch": ein Verlust durch einen externen technischen Vorfall (Scalable-Capital-SL-Blockade verhinderte rechtzeitiges Nachziehen/Schließen). User bewertete den Tag trotzdem ausdrücklich als prozessual solide — die Prozessqualität (Analyse, Signale, Dokumentation) wird unabhängig vom €-Ergebnis bewertet, wenn externe Faktoren (Broker-Ausfall, privater Notfall) das Ergebnis beeinflusst haben, nicht die eigene Entscheidungsfindung.

### Regelbruch-Klassifizierung gilt auch bei Gewinn-Trades (ergänzt 09.07.2026, Fable-5-Review)
Die Regelbruch/Marktrisiko-Unterscheidung wurde bisher nur bei Verlust-Trades konsequent angewendet — Gewinn-Trades mit Regelverstoß wurden nicht als Verstoß gezählt, sondern nur am Ergebnis gemessen. Das ist Survivorship-Bias: ein Regelbruch bleibt ein Regelbruch, unabhängig davon, ob der Markt ihn diesmal belohnt hat. Zwei konkrete übersehene Fälle:
- **Trade #13 (07.07.2026):** Chop-Erkennungsregel ignoriert (4+ gescheiterte EMA-Anläufe hätten laut Regel halbe Positionsgröße nahegelegt), trotzdem volle Größe gehandelt — im Log nur als "bewusste Abweichung" erwähnt, nicht als Verstoß gezählt. War der beste Trade der Phase, was das Risiko erhöht, dass die Chop-Regel künftig als "nicht so wichtig" unterlaufen wird.
- **Trade #15 (08.07.2026):** Dual-Gate-Bestätigung erneut nur asymmetrisch erfüllt (zweiter Fall nach Trade #13) — NQ1! nur per Live-Tick statt Kerzenschluss bestätigt.

**Why:** Fable-5-Review (09.07.2026) stellte fest, dass Regelverstöße bislang nur über die Verlust-Statistik sichtbar wurden — dadurch bleibt unklar, wie oft Regeln tatsächlich gebrochen werden, wenn der Ausgang zufällig positiv ist. Ohne diese Sichtbarkeit erodieren Regeln schleichend ("hat doch letztes Mal geklappt").

**How to apply:** Bei JEDER Trade-Nachbesprechung (Gewinn oder Verlust) explizit prüfen und im Trade-Log/der Tagesdatei vermerken, ob alle zum Zeitpunkt geltenden Regeln eingehalten wurden — unabhängig vom Ergebnis. Regelverstöße bei Gewinn-Trades genauso benennen wie bei Verlust-Trades, auch wenn kein unmittelbarer Handlungsbedarf am Ergebnis besteht (das Geld bleibt gewonnen) — der Handlungsbedarf betrifft die Disziplin für künftige Trades.

### Erkennungs-Zwang + Sofort-Review (ergänzt 12.08.2026, Phase-1-3-Regelwerk-Audit)

Das Prinzip aus dem Abschnitt oben (Regelbruch-Klassifizierung gilt auch bei Gewinn-Trades, 09.07.2026) blieb über einen Monat unmechanisiert — es lebte nur als Vorsatz, ohne erzwungenen Check. Ergebnis: Von den 6 beim Audit vom 12.08.2026 nachträglich korrigierten Regelverstößen (u.a. Trades #9, #12, #31, #34) war keiner ein Gewinn-Trade, alle 6 waren Verluste — die Prüfung fand also faktisch weiterhin nur bei Verlusten statt, genau das Muster, das der 09.07.-Eintrag verhindern sollte. Zwei Mechanismen schließen die Lücke:

**a) Erkennungs-Zwang:** Im Tagesabschluss bekommt JEDER Trade, unabhängig von Win/Loss/BE, eine Pflichtzeile — parallel zur bereits bestehenden "DB-Sync: JA/NEIN"-Zeile (siehe [[feedback_tagesabschluss]]):
`Regelkonformität geprüft: JA — Verstoß: JA/NEIN`
Diese Zeile ist selbst nicht optional — ihr Fehlen im Tagesabschluss ist damit direkt sichtbar (nicht erst nachträglich beim nächsten Audit), und sie kann bei Gewinn-Trades nicht mehr stillschweigend übersprungen werden.

**b) Sofort-Review:** Sobald `Verstoß: JA` ausgelöst wird — bei Win ODER Loss —, ist eine noch am selben Tag durchgeführte Fable-Review Pflicht. Formalisiert, was bei Verlust-Fällen bereits informell passierte (#31 am 05.08., #34 am 07.08.), aber bei Gewinn-Fällen bisher nie ausgelöst wurde.

**Why:** Ohne einen Zwangs-Punkt im Prozess selbst bleibt die Erkennung von der Tagesform/Aufmerksamkeit abhängig — die Zahlen (6/6 Korrekturen nur bei Verlusten) zeigen, dass genau das bisher passiert ist.

**Ausdrücklich nicht Teil dieser Änderung (von Levi abgelehnt, 12.08.2026):** keine automatische Positionsgrößen-Reduktion (50%-Floor) nach einem Verstoß, keine neue Cooldown-Regel-3 (Verstoß-Häufigkeits-Zähler) in `cooldown_check.cjs`. Diese Ergänzung ist rein auf Erkennung + Review beschränkt, keine automatische Konsequenz.

**How to apply:** Siehe [[feedback_tagesabschluss]], Abschnitt "Regelkonformitäts-Pflichtzeile" für den genauen Ablauf im Tagesabschluss.

### Levis persönlicher Fokus auf Durchsetzung/Konsistenz (ergänzt 22.08.2026, nach Fable-Win/Loss-Vergleich)

Fables Vergleichsanalyse aller 43 Trades (22.08.2026) fand, dass die größte Verlust-Kategorie (50,5% aller Verlust-Euro, Kategorie B) NICHT fehlendes Wissen war, sondern bereits existierende, bekannte Regeln, die im Moment nicht angewendet wurden — explizit als "kein Wissensproblem, ein Durchsetzungsproblem" benannt. Levis Reaktion darauf: "Da bin ich auch am Lernen und es wird besser bei mir mit der Durchsetzung und Konsistenz. Das habe ich mir selber persönlich fest an die Stirn geschrieben."

**Why:** Das ist ein expliziter persönlicher Fokuspunkt, kein einmaliger Kommentar — relevant dafür, wie künftige Reviews/Tagesabschlüsse eingeordnet werden.

**How to apply:** Bei künftigen Regelkonformitäts-Auswertungen (siehe Abschnitt oben) gezielt darauf achten, ob sich die Kategorie-B-Quote (Regelbruch trotz bereits bekannter Regel) über die Zeit tatsächlich senkt — nicht nur pauschal glauben, dass es "besser wird", sondern anhand der DB/Tagesabschlüsse verifizieren (siehe [[feedback_verify_dont_cave]] und [[feedback_coaching_ehrlichkeit]]). Echte Verbesserung ehrlich anerkennen, wenn die Zahlen sie stützen — aber nicht vorschnell bestätigen, nur weil der Wunsch geäußert wurde.
