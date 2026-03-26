# The *gridpapers* package

![Build examples](../../actions/workflows/pdflatex-examples.yml/badge.svg)
[![Latest Zip of PDFs](https://img.shields.io/badge/Zip_of_PDFs-latest-orange.svg?style=flat)](../gh-action-result/examples/pdfs.zip?raw=true)

Make your own quadrille, graph, hex, dot, isometric, and ruled paper
directly in LaTeX. Uses [PGF/TikZ](https://en.wikipedia.org/wiki/PGF/TikZ),
which is part of any modern TeX installation. All colors, sizes, and page
geometry are fully customizable.

**Maintained by Marco Keller.**
Based on the original work of
[Robert McNees](http://jacobi.luc.edu/) and
[Leo C. Stein](http://duetosymmetry.com/),
who created the package architecture, all TikZ patterns, and the color
presets. This version extends their work with automatic grid activation,
explicit page-control commands, and this rewritten documentation.

> **Note:** This package is distinct from the similarly named
> [graphpaper](https://www.ctan.org/pkg/graphpaper) package on CTAN.

---

## How it works

Loading `gridpapers` automatically draws the chosen grid pattern on
**every page** of the document. If you only want the grid on selected
pages — for example, a few blank note pages inside a book — pass the
`manualactivation` option and use the control commands described below.

---

## Installation

If `gridpapers` is already provided by your TeX distribution (TeX Live,
MiKTeX, …), install it through the distribution's package manager.

For a manual installation, copy `gridpapers.sty` into the same directory
as your LaTeX source, or into any directory on TeX's search path.

To regenerate `gridpapers.sty` from source:

```
pdflatex gridpapers.ins
```

---

## Quick start

### Standalone graph paper

```latex
\documentclass{article}
\usepackage[pattern=std]{gridpapers}
\begin{document}
\thispagestyle{empty}
~
\end{document}
```

The `~` forces a non-empty body; without any content LaTeX would not
produce a PDF. Run `pdflatex` twice to get the output.

### Grid pages inside a larger document

```latex
\documentclass{book}
\usepackage[pattern=std, textarea, manualactivation]{gridpapers}
\begin{document}

Regular text on a plain page.

\gridpages{3}

More regular text, back to plain pages.

\end{document}
```

---

## Package options

All configuration is done through the `\usepackage` options, using
key=value syntax.

### `pattern=<name>`

Selects the background pattern. Default: `std`.

| Name | Description |
|---|---|
| `std` | Quadrille, 10 squares per inch |
| `stdeight` | Quadrille, 8 squares per inch |
| `majmin` | Graph paper, 8 squares per inch with major grid every half inch |
| `dot` | Dot grid |
| `hex` | Hexagonal grid (flat-top orientation) |
| `hexup` | Hexagonal grid, rotated 90° (pointy-top orientation) |
| `tri` | Triangular grid |
| `iso` | Isometric grid (triangles rotated 90°) |
| `lightcone` | Square grid with dashed 45° light-cone diagonals |
| `ruled` | Ruled page with bold horizontal lines |
| `doubleruled` | Ruled page with bold lines alternating with lighter lines |

Each pattern has its own default page geometry and default coverage
(full page or text area only). All defaults can be overridden.

### `colorset=<name>`

Selects a color preset. Default: `std`. A preset sets `majorcolor`,
`minorcolor`, and `bgcolor` all at once.

| Name | Description |
|---|---|
| `std` | Cornflower blue on white |
| `precocious` | Warm rose tones on cream |
| `ghostly` | Very faint grey on white |
| `brickred` | Brick red on pale scarlet |
| `engineer` | Green on light green (classic engineering pad) |
| `plumpad` | Cornflower blue on pale plum |

### Color overrides

You can start from a preset and override individual colors:

- `majorcolor=<color>` — override the major grid / border color.
- `minorcolor=<color>` — override the minor grid / fill color.
- `bgcolor=<color>` — override the page background color.

Colors accept any named color or an [xcolor](https://ctan.org/pkg/xcolor)
blend expression such as `cornflower!40`.

### `patternsize=<length>`

Overrides the default size of the pattern. The meaning of this length
depends on the pattern:

| Pattern | Meaning of `patternsize` | Default |
|---|---|---|
| `std` | Side of one square | `0.1in` |
| `stdeight` | Side of one square | `0.125in` |
| `majmin` | Side of one minor square | `0.125in` |
| `dot` | Spacing between dots | `0.1in` |
| `hex`, `hexup` | Side length of one hexagon | `0.1666in` |
| `tri`, `iso` | Side length of one triangle | `0.25in` |
| `lightcone` | Side of one square | `0.25in` |
| `ruled` | Vertical spacing between lines | `0.2in` |
| `doubleruled` | Spacing between adjacent lines | `0.125in` |

### `dotsize=<length>`

Controls the radius of individual dots for `pattern=dot`. Default: `.7pt`.

### `fullpage` / `textarea`

- `fullpage` — the pattern covers the entire page, including margins.
- `textarea` — the pattern covers only the text area.

At most one of these may be specified. If neither is given, the pattern
uses its own default.

### `geometry={<spec>}`

Page geometry specification using the syntax of the
[geometry](https://ctan.org/pkg/geometry) package. If `geometry` was
loaded before `gridpapers`, this option is ignored and a warning is
emitted.

### `manualactivation`

By default the grid is active on every page from the start of the
document. Passing `manualactivation` reverts to opt-in mode: the grid
starts **off** and must be switched on explicitly with `\gridpaperon`.
Use this option when `gridpapers` is loaded inside a larger document
and the grid should appear only on selected pages.

---

## Commands

### `\gridpaperon`

Activates the grid from the current page onwards, until `\gridpaperoff`
is called or the document ends. In the default mode this command is
redundant, but it can be used to re-enable the grid after a
`\gridpaperoff`.

### `\gridpaperoff`

Deactivates the grid from the current page onwards. Issues a `\clearpage`
internally so the last gridded page is flushed before the state changes.
This is the initial state when `manualactivation` is used.

### `\gridpages{n}`

Inserts `n` blank pages with the grid pattern. The grid is activated
automatically before the first page and deactivated after the last, so
surrounding pages are completely unaffected.

In `book` and `scrbook` classes with the `openright` option active, a
blank padding page is inserted automatically when needed to preserve
correct recto/verso page layout.

---

## Examples

### Classic engineering pad

```latex
\documentclass{article}
\usepackage[pattern=majmin, colorset=engineer]{gridpapers}
\begin{document}
\thispagestyle{empty}
~
\end{document}
```

### Hex grid on A4, full page

```latex
\documentclass{article}
\usepackage[pattern=hex, colorset=engineer,
  fullpage, geometry={a4paper}]{gridpapers}
\begin{document}
\thispagestyle{empty}
~
\end{document}
```

### Custom colors

Named colors and xcolor blends can be used freely:

```latex
\documentclass{article}
\usepackage{xcolor}
\definecolor{mydeepgreen}{rgb}{0.07, 0.56, 0.04}
\usepackage[pattern=majmin,
  majorcolor=mydeepgreen,
  minorcolor={mydeepgreen!40}]{gridpapers}
\begin{document}
\thispagestyle{empty}
~
\end{document}
```

### Full customization

Triangular grid on A4 with 2 cm margins, text area only, 0.75 cm
triangles, engineer colors but white background:

```latex
\usepackage[pattern=tri,
  patternsize=0.75cm,
  textarea,
  colorset=engineer,
  bgcolor=white,
  geometry={a4paper, margin=2cm}]{gridpapers}
```

---

## Gallery

![Standard](https://raw.githubusercontent.com/mcnees/LaTeX-Graph-Paper/screenshots/std.jpg "Standard")

![Quad](https://raw.githubusercontent.com/mcnees/LaTeX-Graph-Paper/screenshots/quad.jpg "Quadrille")

![Hex](https://raw.githubusercontent.com/mcnees/LaTeX-Graph-Paper/screenshots/hex.jpg "Hex")

![Dots](https://raw.githubusercontent.com/mcnees/LaTeX-Graph-Paper/screenshots/dot.jpg "Dots")

![Light cone](https://raw.githubusercontent.com/mcnees/LaTeX-Graph-Paper/screenshots/lightcone.jpg "Light cone")

![Precocious Engineer](https://raw.githubusercontent.com/mcnees/LaTeX-Graph-Paper/screenshots/rosie.png "Precocious Engineer color scheme")

![Hex Engineer](https://raw.githubusercontent.com/mcnees/LaTeX-Graph-Paper/screenshots/hexengineer.png "Hex grid with Engineering Pad color scheme")

---

## License

This material is subject to the
[LaTeX Project Public License 1.3c](https://www.ctan.org/license/lppl1.3).

Copyright (C) 2026 Marco Keller.
Based on original work (C) 2017–2021 Robert McNees and Leo C. Stein.

The hexagon pattern code is
[due to Philippe Goutet](https://tex.stackexchange.com/questions/6019/drawing-hexagons/6128#6128).
