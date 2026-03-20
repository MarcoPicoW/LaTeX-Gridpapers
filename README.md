![Build examples](../../actions/workflows/pdflatex-examples.yml/badge.svg)
[![Latest Zip of PDFs](https://img.shields.io/badge/Zip_of_PDFs-latest-orange.svg?style=flat)](../gh-action-result/examples/pdfs.zip?raw=true)

# Graph papers in LaTeX: the *gridpapers* package

Make your own quadrille, graph, hex, etc. paper! Uses the
[PGF/TikZ](https://en.wikipedia.org/wiki/PGF/TikZ) package for LaTeX,
which should be part of any modern TeX installation. All colors and
spacing are customizable.

Note: This package is distinct from a different package with a similar
name, [graphpaper (on CTAN)](https://www.ctan.org/pkg/graphpaper).

---

## How it works

Loading `gridpapers` does **not** automatically draw a grid on any page.
The grid must be explicitly activated using one of the commands described
below. This allows the package to be used both for standalone graph paper
documents and for larger documents where only some pages should have a
grid background.

---

## Quick start

### Standalone graph paper document

To produce a single page of graph paper, activate the grid at the start
of the document body:

```latex
\documentclass{article}
\usepackage[pattern=std]{gridpapers}
\begin{document}
\gridpaperon
\thispagestyle{empty}
~
\end{document}
```

The `~` forces a non-empty body, otherwise LaTeX would not generate a
PDF. Run `pdflatex` twice to produce the output.

There are ready-to-use example files in the
[examples directory](./examples/). Each one has an almost-empty body
with a `\usepackage` statement you can customize.

### Grid pages inside a larger document

To insert blank grid pages inside a document alongside regular text,
use `\gridpages{n}`:

```latex
\documentclass{book}
\usepackage[pattern=std, textarea]{gridpapers}
\begin{document}

Regular text on a plain page.

\gridpages{3}

More regular text, back to plain pages.

\end{document}
```

---

## Commands

### `\gridpaperon`

Activates the grid pattern from the current page onwards, until
`\gridpaperoff` is called or the document ends.

### `\gridpaperoff`

Deactivates the grid pattern from the current page onwards. This is the
**default state** when the package is loaded.

### `\gridpages{n}`

Inserts `n` blank pages with the grid pattern. The grid is automatically
activated before the first page and deactivated after the last, so
surrounding pages are completely unaffected.

---

## Package options

All static configuration happens via the `\usepackage` command.

### `pattern=<n>`

Which pattern to use. Default: `std`.

| Name | Description |
|---|---|
| `std` | Quadrille, 10 squares per inch |
| `stdeight` | Quadrille, 8 squares per inch |
| `majmin` | Graph paper, 8 squares per inch with major grid every half-inch |
| `dot` | Dot grid |
| `hex` | Hexagonal grid |
| `hexup` | Hexagonal grid, rotated 90° |
| `tri` | Triangular grid |
| `iso` | Isometric grid |
| `lightcone` | Square grid with 45° light cone lines |
| `ruled` | Ruled page |
| `doubleruled` | Ruled page with alternating bold and light lines |

### `colorset=<n>`

Color preset. Default: `std`. A preset sets `majorcolor`, `minorcolor`,
and `bgcolor` all at once. You can start from a preset and override
individual colors.

| Name | Description |
|---|---|
| `std` | Cornflower blue on white |
| `precocious` | Warm rose tones |
| `ghostly` | Very light grey on white |
| `brickred` | Brick red on pale scarlet |
| `engineer` | Green on light green |
| `plumpad` | Cornflower on pale plum |

### Color overrides

- `majorcolor=<color>`: override the major grid color.
- `minorcolor=<color>`: override the minor grid color.
- `bgcolor=<color>`: override the background color.

Colors can be named colors or xcolor expressions, e.g. `cornflower!40`.

### `patternsize=<length>`

Override the pattern size. The meaning depends on the pattern — for
grid patterns it is the side of a square, for hex patterns it is the
side of a hexagon, etc. See the PDF documentation for full details.

### `dotsize=<length>`

Size of individual dots for `pattern=dot`. Default: `.7pt`.

### `fullpage` / `textarea`

- `fullpage`: the pattern fills the whole page including margins.
- `textarea`: the pattern fills only the text area.

At most one of these can be specified. If neither is given, the pattern
uses its own default.

### `geometry={<spec>}`

Page geometry specification using the syntax of the `geometry` package.
Ignored if `geometry` was loaded before `gridpapers`.

---

## Example: full customization

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

![Standard](/../screenshots/std.jpg "Standard")

![Quad](/../screenshots/quad.jpg "Quadrille")

![Hex](/../screenshots/hex.jpg "Hex")

![Dots](/../screenshots/dot.jpg "Dots")

![Light cone](/../screenshots/lightcone.jpg "Light cone")

![Precocious Engineer](/../screenshots/rosie.png "Precocious Engineer color scheme")

![Hex Engineer](/../screenshots/hexengineer.png "Hex grid with Engineering Pad color scheme")

---

## Credits

This package was created by [Robert McNees](http://jacobi.luc.edu/)
with additional contributions from
[Leo C. Stein](http://duetosymmetry.com/), and is maintained by both.
This material is subject to the
[LaTeX Project Public License 1.3c](https://www.ctan.org/license/lppl1.3),
(c) 2017-2021. The hexagon pattern code is
[due to Philippe Goutet](https://tex.stackexchange.com/questions/6019/drawing-hexagons/6128#6128).
