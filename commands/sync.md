# /codebase-router:sync

Manueller Routing-Check und Update für das gesamte Projekt.

## Ablauf
1. INDEX.md lesen → alle Areas
2. Für jede Area:
   a. Prüfe ob referenzierte Files noch existieren (umbenannt/gelöscht?)
   b. Prüfe ob neue Files in Area-Ordnern entstanden sind
   c. Prüfe ob neue Exports/Types in bestehenden Files
   d. Node-Files updaten wenn nötig
3. changelog.md updaten
4. Report: "Sync abgeschlossen. [X] Updates in [Y] Areas."
