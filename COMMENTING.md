# Kommentierungsumfang

Alle wartbaren Quell- und Dokumentationsdateien des Projekts sind ausführlich kommentiert.

## Kommentierte Bibliotheksquellen

- `mechlib.tex`
- `core/colors.tex`
- `core/defaults.tex`
- `core/layers.tex`
- `core/styles.tex`
- `elements/bodies.tex`
- `elements/connectors.tex`
- `elements/supports.tex`
- `elements/trusses.tex`
- `annotations/coordinates.tex`
- `annotations/dimensions.tex`
- `annotations/loads.tex`
- `annotations/mechanics.tex`

## Kommentierte Beispiele und Migrationsquellen

- `examples/api-demo.tex`
- `examples/cantilever-triangular-load.tex`
- `examples/node-api-test.tex`
- `examples/truss-list-example-template-style.tex`
- `examples/legacy-oscillators.tex`
- `examples/legacy-pendula.tex`

Die Legacy-Dateien enthalten zusätzlich einen ausführlichen Hinweis zum Migrationsstatus und zu ihren historischen Abhängigkeiten.

## Build-Artefakte

Dateien mit den Endungen `.aux`, `.log`, `.pdf` und `.synctex.gz` werden von LaTeX erzeugt. Sie sind keine editierbaren Quellmodule und können daher nicht mit LaTeX-Kommentaren versehen werden.

## Prüfung

Die eigenständigen Beispiel-/Testdokumente wurden nach der Kommentierung erneut mit LuaLaTeX kompiliert:

- `api-demo.tex`
- `cantilever-triangular-load.tex`
- `node-api-test.tex`
- `truss-list-example-template-style.tex`
