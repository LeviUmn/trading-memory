---
name: feedback-commit-push-nur-levi
description: Git-Commit UND Push beauftragt ausschliesslich Levi selbst — weder Sonnet noch Fable/Opus-Subagenten committen oder pushen von sich aus
metadata:
  node_type: memory
  type: feedback
  originSessionId: 3c4562d6-d0b9-4432-8f7a-392557ffe83f
  modified: 2026-09-30T09:26:15.317Z
---

Commit und Push (Code-Repo und Memory-Repo) werden **nur auf ausdruecklichen Auftrag von Levi** ausgefuehrt. Weder Sonnet noch ein an Fable/Opus delegierter Subagent committet oder pusht eigenstaendig, auch nicht "nach dem Gegencheck" oder weil es in einem Auftragspaket vorgesehen war.

**Why:** Am 30.09.2026 pushte Sonnet 3 Commits (57c711a, ee27c70, cfad5c3) nach dem Opus-Gegencheck, weil Levis "alles so umsetzen" als Freigabe gelesen wurde; Levi rief "stop kein push" — zu spaet. Fable hatte zuvor ausserdem eigenstaendig committet (cfad5c3, Memory e07da1d). Levi: "ICH beauftrage immer den Push und Commit, nicht Sonnet!" Allgemeine Zustimmung zu einem Auftragspaket oder "Opus-Empfehlung" ist KEINE Push-/Commit-Freigabe.

**How to apply:** Subagenten-Prompts immer mit "KEIN git commit, KEIN git push, Aenderungen nur im Arbeitsverzeichnis lassen" versehen. Nach Umsetzung + Opus-Gegencheck melden "bereit zum Commit/Push", Diff-Umfang nennen und auf Levis ausdruecklichen Befehl warten. Gilt zusaetzlich zu [[feedback_order_bestaetigung]]-Denke (aktiv bestaetigen lassen). Memory-Repo LeviUmn/trading-memory ist derzeit oeffentlich — dort nie ohne Levis Ansage pushen (siehe MEMORY.md).
