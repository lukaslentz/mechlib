# mechlib v3

TikZ library for consistent mechanics graphics.

## Dokumentations- und Kommentierungsprinzip

Der Quellcode ist modular und ausführlich kommentiert. Jede `.tex`-Datei beginnt mit einem Kopfblock, der Aufgabe, öffentliche API und wichtige Entwurfsentscheidungen beschreibt. Innerhalb der Dateien markieren Abschnittskommentare die einzelnen Key-Familien bzw. TikZ-Pics. Kommentare dokumentieren insbesondere nicht offensichtliche Geometrie, exportierte Anker, Layer-Reihenfolge und Gründe für technische Implementierungsentscheidungen.

Die Kommentare sind **keine zusätzliche Laufzeitabhängigkeit** und ändern die erzeugte Grafik nicht. Generierte Dateien wie `.aux`, `.log`, `.pdf` und `.synctex.gz` sind Build-Artefakte und können naturgemäß nicht als LaTeX-Quellcode kommentiert werden. Maßgeblich sind die `.tex`-Quelldateien und diese README.

## Modulkonzept

- `core/` definiert globale visuelle und geometrische Konventionen.
- `elements/` enthält atomare mechanische Bausteine.
- `annotations/` enthält Beschriftungs- und Lastsymbole.
- `examples/` enthält ausführlich erläuterte Referenz- und Testdokumente.
- `legacy-*.tex` ist ein bewusst getrenntes Migrationsarchiv der v1-Pics und wird nicht automatisch geladen.

## Architecture

```text
mechlib/
├── mechlib.tex
├── core/
│   ├── colors.tex
│   ├── defaults.tex
│   ├── layers.tex
│   └── styles.tex
├── elements/
│   ├── bodies.tex
│   ├── connectors.tex
│   ├── supports.tex
│   └── trusses.tex
├── annotations/
│   ├── coordinates.tex
│   ├── dimensions.tex
│   ├── loads.tex
│   └── mechanics.tex
└── examples/
    ├── api-demo.tex
    ├── legacy-oscillators.tex
    ├── legacy-pendula.tex
    └── mechlib-examples.tex
```

## Public API

All public pics are namespaced with `mech/`.

### Bodies

```latex
\pic (m1) at (3,0) {mech/mass={label=$m_1$,width=1.5cm,height=1cm}};
\pic (m2) at (6,0) {mech/point-mass={label=$m_2$}};
\pic (cart) at (0,0) {mech/cart={label=$m$,ground=true}};
\pic {mech/beam={from=(A),to=(B),height=3mm}};
```

### Supports

```latex
\pic (A) at (0,0) {mech/pinned-support={label=$A$}};
\pic (B) at (5,0) {mech/roller-support={label=$B$}};
\pic (wall) at (0,0) {mech/fixed-support={angle=90}};
```

Fixed supports export three standardized connection anchors:

```text
-connection-upper
-connection-middle
-connection-lower
```

Named body pics behave like regular TikZ nodes. For example:

```latex
\pic (m1) at (3,0) {mech/mass={label=$m_1$}};

\draw (m1.east) -- (m1.north east);
\pic {mech/spring={from=(wall-connection-upper),to=(m1.north west),label=$c$}};
```

The usual TikZ anchors are available on `mech/mass`, `mech/point-mass`, and
`mech/cart`:

```text
.center  .north  .south  .east  .west
.north east  .north west  .south east  .south west
```

Masses export the same three vertical levels on both sides:

```text
-west-upper    -east-upper
-west-middle   -east-middle
-west-lower    -east-lower
```

All of these anchors use the shared `\\mechConnectionOffset`. If wall and mass
centers have the same y-coordinate, corresponding upper/middle/lower anchors
are therefore exactly on the same height. Use `middle` for a single connector
and `upper`/`lower` for parallel spring-damper pairs.

### Connectors

Universal pattern: `from`, `to`, `label`.

```latex
\pic {mech/spring={from=(A),to=(B),label=$c$}};
\pic {mech/damper={from=(A),to=(B),label=$d$}};
\pic {mech/rod={from=(K1),to=(K2),label=1}};
```

### Loads

```latex
\pic at (A) {mech/force={angle=-90,length=2cm,label=$F$}};
\pic {mech/force={from=(A),to=(B),label=$F$}};
\pic at (A) {mech/moment={direction=ccw,label=$M$}};
\pic {mech/distributed-load={from=(A),to=(B),start value=0,end value=1,offset=3mm,label=$q(x)$}};
```

### Dimensions and coordinates

```latex
\pic {mech/dimension={from=(A),to=(B),label=$l$,offset=-5mm}};
\pic at (O) {mech/angle-dimension={start angle=0,end angle=50,label=$\alpha$}};
\pic at (O) {mech/axis={label=$x$,angle=0}};
\pic at (O) {mech/coordinate-system={x label=$x$,y label=$y$}};
```

### Mechanical annotations

```latex
\pic at (S) {mech/internal-forces={normal=$N$,shear=$Q$,moment=$M$,side=right}};
\pic at (A) {mech/fixed-reactions={x=$A_x$,y=$A_y$,moment=$M_A$}};
```

## Composite pictures from v1

All previous non-atomic pic definitions were moved to `examples/` as a migration archive:

- `legacy-pendula.tex`: `pendel`, `bewegtespendel`
- `legacy-oscillators.tex`: all previous one-, two-, three- and n-mass oscillator pics

They intentionally remain in their original v1 form so no old geometry is silently changed.
They still refer to the old v1 atom names (`wagen`, `feder`, `daempfer`, ...). New figures should be composed from the `mech/*` atomic API. During migration, use the files as source examples and replace the old composite pics one by one.

## Base path

By default the loader assumes a folder named `mechlib`:

```latex
\input{mechlib/mechlib}
```

For a long or different path, define the base path before loading:

```latex
\newcommand{\mechlibpath}{../../../support/mechlib}
\input{\mechlibpath/mechlib}
```


### Distributed load options

`offset` shifts the complete distributed load perpendicular to its baseline. The default is `0mm`. `arrowhead length` controls the arrowhead size and defaults to `2mm`. Arrows shorter than their arrowhead are suppressed automatically.
