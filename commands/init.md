# /codebase-router:init

Scannt das aktuelle Projekt und baut das `.routing/` Netzwerk auf.

## Ablauf

### 1. Check
Wenn `.routing/INDEX.md` bereits existiert:
Frage kurz: "Routing existiert bereits — rebuild oder skip?"

### 2. Projekt scannen
- Top-Level Ordnerstruktur lesen
- Tech-Stack erkennen (package.json, requirements.txt, go.mod, etc.)
- Feature-Ordner und funktionale Ordner identifizieren

### 3. Areas identifizieren
Priorität:
- Feature-Ordner (features/, modules/, pages/, screens/) → je eine Area
- Funktionale Ordner (auth/, api/, ui/, db/, hooks/, services/) → je eine Area
- Max. 8 Areas; kleinere Ordner unter "misc" zusammenfassen
- Shared utilities → eigene "shared" Area

### 4. Nodes pro Area identifizieren
- Area-Ordner scannen
- Files nach Funktion gruppieren: models/types | api/endpoints | ui/components | hooks/utils
- Max. 6 Nodes pro Area
- Areas mit weniger als 5 Files: flach halten (nur _.md, keine Sub-Nodes)

### 5. Tiefe bestimmen
- < 10k LOC → 2 Layer (INDEX + area/_.md, keine Nodes)
- 10k–50k LOC → 3 Layer (INDEX + area/_.md + nodes)
- 50k+ LOC → 4 Layer (INDEX + area/_.md + nodes + sub-nodes)
- Wenn ein Node > 15 Zeilen würde → eigene Sub-Nodes erstellen

### 6. .routing/ erstellen

Exakte Formate einhalten:

INDEX.md:
```
# ROUTER
stack: [stack]

AREAS
[name]  → [area]/_.md  | [3-4 keywords]
```

area/_.md:
```
# [AREA]
root: [path]

NODES
[name]  → [area]/[node].md  | [key types/functions]

DEPS: [andere areas]
```

area/node.md:
```
# [AREA]/[NODE]
[file1.ts]
[file2.ts]

[KeyType]: {field1 field2 field3}
[KeyFn]: (params) → return

CHANGED: [YYYY-MM-DD]
DEPS: [area/node]
```

changelog.md:
```
# Changelog
[YYYY-MM-DD] init — [X] areas [Y] nodes
```

### 7. Bestätigung
Output: "Routing initialisiert. [X] Areas, [Y] Nodes. Welchen Bereich möchtest du bearbeiten?"
