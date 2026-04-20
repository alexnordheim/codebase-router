---
name: auto-navigate
description: Routes Claude to the correct codebase area based on natural language. Automatically active in every session — no invocation needed.
---

# Codebase Router — Always Active

## Session Start
IMMER: Wenn `.routing/INDEX.md` im Projekt-Root existiert, lies es sofort und
still am Session-Anfang. Kein Output. Nur Kontext laden.

## Navigation
Wenn der User einen Bereich, ein Feature oder eine Komponente nennt:
1. Mappe seine Worte auf eine Area in INDEX.md (fuzzy match)
2. Lies `[area]/_.md`
3. Identifiziere den relevantesten Node für die Aufgabe
4. Lies diesen Node
5. Bestätige in einem Satz:
   "Ich bin im [Area/Node] Kontext. Relevante Files: [list]. Was soll ich tun?"

Nie mehr laden als: INDEX.md + 1 area _.md + max. 2 node files gleichzeitig.

## Fuzzy Matching
- Mappe ähnliche Worte/Konzepte zur nächsten Area
- Bei Unklarheit: wähle wahrscheinlichste, erwähne kurz:
  "Ich route zu '[area]' — passt das?"
- Nie eine lange Rückfrage stellen

## Cross-Area Arbeit
Wenn der User sagt "mach das auch für [X]" oder "ändere das gleiche in [X]":
1. Identifiziere den parallelen Bereich aus INDEX.md
2. Lies dessen _.md + relevanten Node (~25 Zeilen zusätzlich)
3. Wende die gleiche Änderung an
4. Bleibe in beiden Kontexten bis die Aufgabe fertig ist

## Routing Updates nach File-Änderungen
Nach jedem Write/Edit/MultiEdit prüfen ob Routing-Update nötig.
Update (silent, kein Output an User) wenn:
  ✓ Neues File erstellt → in den relevanten Node eintragen
  ✓ File umbenannt/verschoben → Pfad im Routing updaten
  ✓ Neuer Export / neue Klasse / neuer Typ / neues Interface → in Node ergänzen
  ✓ Komplett neuer Feature-Ordner → neue Area + INDEX.md Eintrag erstellen

Kein Update bei:
  ✗ Reine Logik-Änderungen
  ✗ Styling / Formatierung
  ✗ Bug-Fixes ohne neue Strukturen

## Primary Ownership Regel
Jedes File gehört genau einer Area. Bestimmung nach Priorität:
1. Ordner-Struktur (src/features/tracking/ → tracking)
2. Import-Häufigkeit (wird öfter von tracking als von ui importiert → tracking)
3. Dateiname (TrackingScreen → tracking)
4. Fallback → shared/

Files die in mehr als 2 Areas genutzt werden → shared/

## Changelog
Bei jedem Routing-Update `.routing/changelog.md` ergänzen:
  [YYYY-MM-DD] [area/node]: [was geändert — ein Halbsatz]

## Session Summary
Nur wenn User explizit fragt ("was haben wir geändert?", "summary", "überblick"):
Lies `.routing/changelog.md`, gib kompakten Überblick der heutigen Änderungen.
