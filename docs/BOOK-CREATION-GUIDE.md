# Book Creation Guide — Colophon

This is the onboarding and day-to-day reference for producing book-
style PDFs (course books, manuals, handbooks) with this Quarto
scaffold. Part 1 gets a new machine from zero to a working PDF —
this toolkit renders with placeholder branding out of the box, so
you can confirm the whole pipeline works before customizing
anything. Part 2 is the how-to reference for everything you'll do
after that: writing chapters, tables, citations, and adapting the
branding to a real company.

For the *why* behind some of this scaffold's less obvious design
choices — Quarto/Pandoc quirks we had to work around — see
`README.md` in the project root. This guide tells you what to do;
the README explains why it has to be done that way.

---

## Contents — Part 1: Setup

1. [What you're installing, and why](#1-what-youre-installing-and-why)
2. [Install Quarto](#2-install-quarto)
3. [Install VS Code](#3-install-vs-code)
4. [Install the Quarto VS Code extension](#4-install-the-quarto-vs-code-extension)
5. [Install a LaTeX distribution](#5-install-a-latex-distribution)
6. [Get the project](#6-get-the-project)
7. [First render](#7-first-render)
8. [Live preview while writing](#8-live-preview-while-writing)

## Contents — Part 2: How-To Reference

9. [Project structure map](#9-project-structure-map)
10. [Adding a new chapter](#10-adding-a-new-chapter)
11. [Grouping chapters into Parts](#11-grouping-chapters-into-parts)
12. [Appendices](#12-appendices)
13. [Editing the title page](#13-editing-the-title-page)
14. [Editing brand colors and fonts](#14-editing-brand-colors-and-fonts)
15. [Writing tables](#15-writing-tables)
16. [Figures](#16-figures)
17. [Diagrams (Mermaid, Graphviz, and other tools)](#17-diagrams-mermaid-graphviz-and-other-tools)
18. [Landscape (wide) pages](#18-landscape-wide-pages)
19. [Footnotes](#19-footnotes)
20. [Citations and bibliography](#20-citations-and-bibliography)
21. [Page breaks](#21-page-breaks)
22. [Adding back-matter pages](#22-adding-back-matter-pages)
23. [Common mistakes and what actually goes wrong](#23-common-mistakes-and-what-actually-goes-wrong)
24. [Quick-reference cheat sheet](#24-quick-reference-cheat-sheet)

---

# Part 1: Setup

## 1. What you're installing, and why

| Tool | What it does | Required? |
|---|---|---|
| **Quarto** | The command-line tool that turns your `.qmd` (Markdown) files into the finished PDF. Everything else supports this. | Yes |
| **VS Code** | The editor you'll write chapters in. Any text editor technically works, but VS Code's Quarto extension gives you a live preview and syntax help. | Recommended |
| **Quarto extension for VS Code** | Adds `.qmd` syntax highlighting, a render button, and a live preview pane inside VS Code. | Recommended |
| **A LaTeX distribution (TinyTeX)** | Quarto doesn't typeset PDFs itself — it hands off to LaTeX (specifically `xelatex`, per this project's config) to do the actual page layout. TinyTeX is a small, Quarto-managed LaTeX install that's enough for this project. | Yes, for PDF output |

You do **not** need to separately install any fonts to get started.
This toolkit's default `brand.tex` uses TeX Gyre Termes and TeX Gyre
Heros — freely-licensed fonts that ship bundled with every standard
TeX/TinyTeX install — so it renders correctly on a fresh machine
with nothing extra to source or configure. When you're ready to swap
in a real brand's custom fonts, that's a `brand.tex` edit (see
§13) — put the font files in a `fonts/` folder and point `brand.tex`
at them (see the comments in `brand.tex` for the exact pattern).

## 2. Install Quarto

Go to **[quarto.org/docs/get-started](https://quarto.org/docs/get-started)**
and download the installer for your OS (Windows `.msi`, macOS `.pkg`,
or a Linux package/tarball). Run it like any other installer.

**Verify it worked** — open a terminal (macOS/Linux: Terminal;
Windows: PowerShell or Command Prompt) and run:

```bash
quarto --version
```

You should see a version number (e.g. `1.10.18`). If you get
"command not found," the installer may not have added Quarto to your
system `PATH` — restart your terminal first; if it still fails,
re-run the installer or check Quarto's install docs for your OS.

## 3. Install VS Code

Download from **[code.visualstudio.com](https://code.visualstudio.com)**
and install normally. If your organization already provides VS Code
through a managed software catalog, use that instead.

## 4. Install the Quarto VS Code extension

1. Open VS Code.
2. Click the **Extensions** icon in the left sidebar (or press
   `Ctrl+Shift+X` / `Cmd+Shift+X`).
3. Search for **"Quarto"** — the official one is published by
   *Quarto*, extension ID `quarto.quarto`.
4. Click **Install**.

This gives you: syntax highlighting for `.qmd` files, a **Render**
button in the editor toolbar, a live preview pane, and inline error
messages if something in a chapter file is malformed.

## 5. Install a LaTeX distribution

Quarto can install and manage a lightweight LaTeX distribution
(TinyTeX) for you — this is the easiest path and what this project
was built and tested against. In a terminal:

```bash
quarto install tinytex
```

This downloads TinyTeX (a few hundred MB) and registers it with
Quarto. You only need to do this once per machine.

**About missing LaTeX packages:** this project's `preamble.tex` uses
several LaTeX packages (`fancyhdr`, `pdflscape`, `booktabs`,
`titlesec`, `setspace`, `xcolor`, and others). You do **not** need to
install these individually — TinyTeX automatically detects and
downloads any package it's missing the first time it's needed during
a render. This means:

- Your **very first render** on a new machine may take noticeably
  longer than usual (TinyTeX is fetching packages in the background)
  and needs an active internet connection.
- Every render after that is fast, since the packages are now
  installed locally.

If your organization already has a full TeX Live or MacTeX
installation and would rather use that instead of TinyTeX, that
works too — just make sure it's on your system `PATH` and skip this
step. Confirm either way with:

```bash
quarto check
```

This prints a health report — Quarto version, TinyTeX/LaTeX
detection, and Chromium (used for other Quarto formats, not
relevant here) — and flags anything missing.

## 6. Get the project

Get the project folder onto your machine — via `git clone` if it's
in a repository, or by unzipping a shared archive. Either way, you
should end up with a folder (e.g. `colophon-quarto/`)
containing `_quarto.yml`, the `.qmd` chapter files, `brand.tex`,
`preamble.tex`, and so on. Open that **folder** in VS Code (File →
Open Folder — not File → Open, which opens a single file).

## 7. First render

From VS Code's integrated terminal (Terminal → New Terminal), or any
terminal `cd`'d into the project folder, run:

```bash
quarto render
```

This builds the whole book. On success, you'll see
`Output created: _book/colophon-quarto.pdf` — open that file to check
the result. `quarto render` is the correct, complete command for
this project every time; you don't need any extra flags.

If it fails, read the terminal output — Quarto/LaTeX error messages
usually name the exact file and line. See
[§23](#23-common-mistakes-and-what-actually-goes-wrong) for the
mistakes most likely to trip you up in *this* project specifically.

## 8. Live preview while writing

Instead of re-running `quarto render` after every edit, you can keep
a live preview open:

```bash
quarto preview
```

This opens the rendered output in a browser tab and re-renders
automatically whenever you save a `.qmd` file. (In VS Code with the
Quarto extension installed, you can also just click the **Preview**
button in the editor toolbar instead of typing the command.) Close
the preview with `Ctrl+C` in the terminal it's running in when you're
done.

---

# Part 2: How-To Reference

## 9. Project structure map

| File / folder | What it is |
|---|---|
| `_quarto.yml` | The book's master config — chapter order, page format, fonts/margins hookup, citation source. |
| `index.qmd` | Required by Quarto as the book's home page; doubles as the Copyright page here. |
| `title-page.tex` | Title page *layout* — you shouldn't need to edit this. |
| `title-page-content.tex` | Title page *text* — title, subtitle, tagline, logo. Edit this instead. |
| `preface.qmd`, `contents.qmd`, `introduction.qmd` | Front matter: Preface, Table of Contents + List of Tables, Introduction. |
| `chapter-01.qmd`, `chapter-02.qmd` | The numbered chapters. Add more the same way — see [§10](#10-adding-a-new-chapter). |
| `references.qmd` | Where the bibliography actually prints (back matter). |
| `references.bib` | Your citation source (BibTeX format). |
| `closing.qmd` | Back matter — organization bio, thank-you page. |
| `brand.tex` | Colors, fonts, and the book title used in the running header. The one file you edit to make this toolkit look like *your* brand — see [§14](#14-editing-brand-colors-and-fonts). |
| `preamble.tex` | The structural engine — page layout, table mechanics, header/footer behavior, front/back-matter page numbering. Brand-agnostic; shouldn't need to change. |
| `logo-placeholder.png` | Generic placeholder logo — replace with a real one and update `title-page-content.tex` to match. |
| `docs/BOOK-CREATION-GUIDE.md` | This file. |
| `README.md` | Deep-dive notes on *why* the scaffold is built the way it is — read this when something behaves unexpectedly. |

## 10. Adding a new chapter

1. Create a new `.qmd` file, e.g. `chapter-03.qmd`.
2. Start it with a normal top-level Markdown heading:
   ```markdown
   # Your Chapter Title

   Chapter content goes here, written as plain Markdown.
   ```
3. Add the filename to the `chapters:` list in `_quarto.yml`, in the
   position you want it to appear:
   ```yaml
   book:
     chapters:
       - ...
       - chapter-02.qmd
       - chapter-03.qmd   # <- new chapter
       - references.qmd
       - closing.qmd
   ```
4. Render. That's it — page numbering, the Table of Contents, chapter
   numbering, and headers all update automatically.

By default, `introduction.qmd`'s heading is unnumbered
(`# Introduction {.unnumbered}`), so your first real chapter after it
becomes Chapter 1 — not Chapter 2. If you'd rather the Introduction
itself be Chapter 1, remove `{.unnumbered}` from that one heading;
every later chapter renumbers itself automatically either way, no
other changes needed.

Write chapter prose exactly like you would in Typora or any Markdown
editor: headings, **bold**, *italic*, lists, images, and simple
tables all work with no special syntax. You only need raw LaTeX
(the `` ```{=latex} `` blocks you'll see in `chapter-01.qmd`) for the
handful of things Markdown can't express — landscape tables, shaded
table cells, and captions positioned below a table. Everything else
is just Markdown.

## 11. Grouping chapters into Parts

Not used by default in either toolkit — the example content is a
flat chapter list — but it's supported and tested. Add a `part:`
entry to `_quarto.yml`'s `chapters:` list:

```yaml
book:
  chapters:
    - index.qmd
    - preface.qmd
    - contents.qmd
    - introduction.qmd
    - part: part-one.qmd       # a file containing just `# Part Title`
      chapters:
        - chapter-01.qmd
        - chapter-02.qmd
    - part: part-two.qmd
      chapters:
        - chapter-03.qmd
    - references.qmd
    - closing.qmd
```

Each `part:` value points to a `.qmd` file, typically just a
level-one heading (`# Part One: Foundations`) — that becomes the part
title. A short overview paragraph right after the heading works too
and correctly lands on the same page as the title, not stranded
alone on the page before it. Chapter numbers keep counting straight
through the book (they don't restart at 1 for each part) — that's
standard convention and matches what most books do. Part numbering
("Part I," "Part II") and styling are automatic — no extra setup
needed, and this doesn't affect anything else (page numbering, TOC,
headers, citations) in the rest of the book. See README's "Parts"
section for the full detail on what was actually tested and
confirmed, including why this needed titlesec's `\titleclass`
mechanism specifically rather than the more obvious kernel-level fix.

## 12. Appendices

`\giEnableAppendix` (in `preamble.tex`) switches from numbered
chapters to lettered appendices — "Appendix A," "Appendix B," instead
of continuing "Chapter N." Call it at the end of the *last numbered
chapter's* file (same placement rule as `\giEnableMainMatter`/
`\giEnableBackMatter` — Quarto moves raw LaTeX found before a heading
to after it, so this can't go at the top of the first appendix file):

```latex
\giEnableAppendix
```

Then write each appendix exactly like a normal chapter — a `#`
heading, or (for a landscape-only appendix, common for checklist
tables/templates/diagrams in an appendix) `\giLandscapeChapterOpener`
exactly as documented in [§18](#18-landscape-wide-pages). No other
syntax changes needed — table/figure numbering, the List of
Tables/Figures, and cross-referencing all correctly switch to
letter-based numbering ("Table A.1," "Figure B.1") automatically.

**If back matter (References, About, Thank You) follows the
appendices**, move `\giEnableBackMatter` to the end of the *last
appendix* file instead of the last numbered chapter — back matter
should come after the appendices, not before them.

Confirmed working by building and rendering a real 3-appendix test,
including one landscape-only appendix — see README's "Appendices"
section (in `preamble.tex`'s comments) for the full detail on what
was tested.

## 13. Editing the title page

Edit **`title-page-content.tex`**, not `title-page.tex`. It's a
short file of plain text values:

```latex
\newcommand{\giTitlePageTitle}{Your Book Title}
\newcommand{\giTitlePageCode}{COURSE-CODE}
\newcommand{\giTitlePageSubtitle}{A one-line subtitle describing this book}
\newcommand{\giTitlePageTagline}{A short tagline or description that\\expands on what this book covers}
\newcommand{\giTitlePageLogo}{logo-placeholder.png}
\newcommand{\giTitlePageLogoWidth}{0.42}
```

Change the text between the curly braces and render — no LaTeX
layout knowledge required. `title-page.tex` is the design/structure
that reads these values; you shouldn't need to open it for a normal
title or subtitle update.

## 14. Editing brand colors and fonts

Both live in `brand.tex`:

- **Colors** are defined once near the top as named LaTeX colors
  (`giPrimary`, `giCrimson`, `giSecondary`, `giInk`, etc.) and reused
  everywhere — change a color's hex value there and it updates
  throughout the book.
- **Fonts**: by default, `\setmainfont{TeX Gyre Termes}` is the body
  text font and `\headingfont` (TeX Gyre Heros) is used for chapter
  titles and headings — both ship with any standard TeX install, no
  extra files needed. To switch to a real brand's custom fonts,
  replace those two blocks with the `Path=fonts/` pattern commented
  in-line in `brand.tex`: add the new font's static-weight `.ttf`
  files (Regular/Bold/Italic/Bold Italic — not the variable-font
  version) to a `fonts/` folder, then point `\setmainfont`/
  `\newfontfamily` at them. Keep the filename base consistent with
  the family name passed to fontspec — a family name with a space in
  it won't match filenames without one, which is a real, easy-to-hit
  render error (see the comment above that block in `brand.tex`).
  Also keep the `Mapping = tex-text` option on the
  `\newfontfamily\headingfont{...}` block if you swap fonts —
  without it, em dashes and en dashes in chapter/section titles
  render as literal `---`/`--` instead of real dash characters, even
  though body text renders them correctly (`\setmainfont` gets this
  font feature automatically; `\newfontfamily` doesn't).

## 15. Writing tables

**Simple tables** (narrow, no special shading) can be plain Markdown:

```markdown
| Term  | Definition          |
|-------|----------------------|
| Asset | Something of value  |
| Risk  | Potential for loss  |
```

**Table header** (a descriptive label above the table) is just an
ordinary bold paragraph — no special syntax:

```markdown
**Core risk terminology**

| Term  | Definition |
...
```

**Captions** always need raw LaTeX, even for a simple table — Pandoc's
native Markdown caption syntax hardcodes the caption *above* the
table with no way to move it. To get a caption *below* (this
project's convention), write the table as raw LaTeX and place
`\caption{}\label{}` right before `\end{longtable}`. Copy the pattern
from either table in `chapter-01.qmd` rather than starting from
scratch. Two things to always do when you add a captioned table:

- **Don't** wrap it in `\def\LTcaptype{none}` — that's Pandoc's own
  marker for *uncaptioned* tables; leaving it on a captioned table
  stops it from numbering or appearing in the List of Tables.
- **Do** label it `\label{tbl-yourname}` (the `tbl-` prefix matters)
  so you can reference it elsewhere in the text with
  `@tbl-yourname`, which auto-resolves to "Table 2.1" (or whatever
  it's numbered) and stays correct if tables get reordered later.

**Wide tables with more columns than fit in portrait** — see
[§18](#18-landscape-wide-pages).

**Column widths** — for a raw-LaTeX table, don't leave columns
equal-width. Weight each column's `p{}` width by how much text it
typically holds (a "Notes" column needs far more room than a
"Status" column), not by its header text length. The weights need to
sum to 1.0 — see the worked comment block above the table in
`chapter-01.qmd`.

**Shading colors** (header row, first column, alternating rows,
divider lines) are all pre-defined brand colors in `brand.tex`
(`giTableHeaderShade`, `giTableFirstColShade`, `giTableAltRowShade`,
`giTableDivider`). Apply them with `\rowcolor{}` (whole row) or
`\cellcolor{}` (single cell, overrides the row's color) — see
`chapter-01.qmd`'s landscape table for a fully worked example
combining all three.

**List of Tables** fills in automatically from every properly
captioned table in the book (in reading order) — nothing to
maintain by hand.

**Tables don't have a separate alt-text attribute** — a table's
accessible description is just its caption text; there's no raw-
LaTeX equivalent of a figure's `fig-alt`. See [§16](#16-figures)
below for the caption-vs-alt-text distinction that *does* apply to
figures, and README's "Captions, alt text, and the Lists of
Tables/Figures" section for the full explanation with worked
examples if you're unsure whether something's written correctly.

## 16. Figures

Plain Markdown image syntax works, no raw LaTeX needed — simpler
than tables, which need raw LaTeX for a caption:

```markdown
![Caption text goes here.](image.png){#fig-yourname width="40%"}
```

The caption prints *below* the image by default — Pandoc's normal
behavior for figures, no override needed the way tables required
one. Same `fig-` labeling convention as tables' `tbl-` prefix: it's
what makes `@fig-yourname` resolve elsewhere in the text to
"Figure 3.1" (or whatever it's numbered) and enters it into the List
of Figures. See `@fig-logo` in `chapter-02.qmd` for a working
example.

**Caption vs. `fig-alt` — these control two different things.** The
bracketed text `![...]` is the *caption* — short, visible, printed
under the image, and what shows up in the List of Figures. The
optional `fig-alt="..."` attribute is *alt text* — a separate,
invisible accessibility description for screen readers that never
prints and never appears in the List of Figures:

```markdown
![The inside-out trust model.](diagram.png){#fig-model
fig-alt="The illustration depicts the inside-out trust model,
featuring four central components in Layer 0 at its core, encircled
by Layers 1 to 3, with the six governance layers forming the
outermost perimeter."}
```

Keep the caption short and scannable — that's what ends up in the
list — and let `fig-alt` carry the longer, more literal description.
Writing a long accessibility description as the caption (or vice
versa) is the single most common mistake here. See README's
"Captions, alt text, and the Lists of Tables/Figures" section for
the full explanation.

**Figures render exactly where you place them in the source** —
`fig-pos: "H"` in `_quarto.yml` forces this; without it, a figure can
drift several paragraphs (even sections) away from where it's
written, since LaTeX's default float behavior doesn't guarantee
source-order placement. Already set for you; nothing to configure.

**List of Figures (and List of Tables) is chapters-only —
enforced automatically.** Caption a front- or back-matter image or
table the same normal way you would in a chapter — it'll still show
up on the page, it just won't appear in either list. Nothing special
to remember or avoid; `preamble.tex` handles this. (Small cosmetic
note: a captioned figure/table outside a chapter still shows a
numbered "Figure N.N:"/"Table N.N:" prefix on the page, since the
counters are one continuous global sequence — just not listed. See
`closing.qmd` for a working example, and README's "Captions, alt
text, and the Lists of Tables/Figures" section for the full
explanation of how and why.)

A book with no tables at all (or no figures at all) doesn't end up
with an empty, oddly-titled List of Tables/Figures page — that list
simply doesn't appear, heading and all, when there's nothing to put
in it. Nothing to configure; this is automatic and the two lists are
controlled independently of each other.

## 17. Diagrams (Mermaid, Graphviz, and other tools)

Don't use Quarto's native `` ```{mermaid} ``/`` ```{dot} `` code
blocks for PDF output — both require a headless Chrome install that's
genuinely fragile (confirmed directly: failed in one real environment
due to network restrictions) and matches a long trail of upstream
bugs specific to PDF rendering (cropped diagrams, indefinite hangs,
failures with more than one diagram per document). HTML output
doesn't have these problems; PDF does, and PDF is what this toolkit
produces.

**Instead: generate the diagram externally, export to PNG, embed it
as a normal figure** — same pipeline as any other image in this
project (`fig-pos: "H"` placement, `#fig-` crossref labels, List of
Figures behavior all apply the same way):

```markdown
![A governance review process.](figures/review-process.png){#fig-review width="70%"}
```

**PNG, not SVG** — LaTeX can't embed SVG without an Inkscape
conversion step this toolkit doesn't install; PNG works natively.

**Graphviz** (`dot`) — flowcharts, hierarchies, dependency graphs,
org charts. No browser dependency for local generation:
```bash
dot -Tpng diagram.dot -o diagram.png
```
Use `rankdir=LR` for a more compact, book-page-friendly layout than
the default top-to-bottom.

**Mermaid** — sequence diagrams, gantt charts, state machines,
anything with a time/process dimension Graphviz doesn't represent
naturally. The Mermaid *library* (what generates the PNG) is free
and open source — not the same thing as "Mermaid Chart," a separate
paid product from the same team. Generate a PNG via
[mermaid.live](https://mermaid.live) (free web editor, no install)
or the `@mermaid-js/mermaid-cli` npm package for a scriptable
pipeline (`mmdc -i diagram.mmd -o diagram.png -s 3` — the `-s 3`
gives print-resolution output; the default is tuned for screens).

See README's "Diagrams" section for full working examples, tips on
matching diagram colors/fonts to the brand, and the reasoning behind
avoiding Quarto's native diagram code blocks for PDF.

## 18. Landscape (wide) pages

Wrap the content in a `landscape` block, and call the two provided
header/footer macros right inside it — this is the one place you
can't skip them, since a landscape page's header/footer needs
special handling (see README's "Header / footer" section for why):

```latex
\begin{landscape}
\giLandscapeHeader

... your wide table here ...

\giLandscapeFooter
\end{landscape}
```

Everything before and after the `landscape` block stays normal
portrait — only that one page rotates.

**If a chapter's only content is a landscape table/figure** (nothing
portrait comes before it), use `\giLandscapeChapterOpener{Title}` and
`\giLandscapeChapterHeader` instead of a normal Markdown heading and
`\giLandscapeHeader` — otherwise the chapter wastes a whole page
showing just the title before the table starts on the next page. See
README's "A chapter whose only content is a landscape table or
figure" section for the full pattern and why it needs a different
mechanism than the normal case above.

## 19. Footnotes

Work with zero setup, using standard Markdown syntax:

```markdown
This claim needs support.[^1]

[^1]: Here's the supporting detail.
```

Pandoc converts this straight to a LaTeX `\footnote{}`. Endnotes
(notes collected at the chapter or book end instead of per-page) are
*not* currently supported by this scaffold — if you need them later,
that's a real addition (an LaTeX package plus a filter), not a
config toggle; ask before assuming it's a quick change.

**"Note with key 'N' defined ... but not used" warning during
render**: this means a footnote is *defined* somewhere
(`[^N]: some text`) but never actually *referenced* anywhere in the
body text (`[^N]`) — usually leftover from editing or reordering
content, where the inline marker got deleted or renamed but the
definition stayed behind. Search your `.qmd` files for `[^N]:` to
find the orphaned definition, then either add the missing `[^N]`
reference back into the text or delete the unused definition. This
is Pandoc telling you about your content, not a scaffold bug —
harmless to the render itself (the PDF still builds correctly), but
worth cleaning up since an unused footnote usually signals lost
content.

## 20. Citations and bibliography

1. Add your reference to `references.bib` (standard BibTeX format —
   `sample.bib`-style entries work as-is).
2. Cite it anywhere in ordinary chapter prose:
   - `@citekey` → inline: "as Smith and Jones (2021) explain..."
   - `[@citekey]` → parenthetical: "...is well established (Smith &
     Jones, 2021)."
   - `[@key1; @key2]` → combine multiple sources in one citation.
3. Render. The reference list itself prints on the References page
   (`references.qmd`) — you don't need to build or format it by
   hand.

Citations only resolve in actual Markdown text — **not** inside a
`` ```{=latex} `` raw block (so not inside a table's `\caption{}`,
for instance). Keep citations in the surrounding prose instead.

**APA 7th edition is the default citation style** in this toolkit —
`csl: apa.csl` in `_quarto.yml`, `apa.csl` in the project root.
Nothing to configure for a normal book; citations and the reference
list format themselves correctly automatically. To use a different
style, get the matching `.csl` file (search
[zotero.org/styles](https://www.zotero.org/styles)), place it in the
project root, and update `csl:` to point at it — see README's
"Citation style" section for the full detail.

## 21. Page breaks

Chapter breaks happen automatically — you never need to force one
between chapters. To force a break *within* a chapter:

```markdown
{{< pagebreak >}}
```

## 22. Adding back-matter pages

Add a new `{.unnumbered}` heading to `closing.qmd` (or create a new
`.qmd` file and append it to `chapters:` after `closing.qmd`) — same
process as a normal chapter, just with `{.unnumbered}` on the
heading so it doesn't get a chapter number. Page numbers continue
automatically in the roman-numeral back-matter sequence.

## 23. Common mistakes and what actually goes wrong

These are mistakes specific to *this* scaffold's quirks — worth
skimming once so you recognize the symptom later.

| If you... | What breaks | Why |
|---|---|---|
| Put a `\mainmatter`/`\backmatter`/similar page-break-forcing command *before* a chapter's Markdown heading, in the same file | The command fires *after* the heading instead — splitting that chapter across an extra blank page | Quarto reorders any raw LaTeX found before a heading to *after* it. The fix is always the same: put the command at the very *end* of the *previous* file instead. |
| Add a `.qmd` file to `chapters:` with no Markdown heading at all | A stray, numbered "Chapter N" appears on an otherwise-blank page | Quarto auto-wraps heading-less content in a numbered `\chapter{}`. Give it `# {.unnumbered .unlisted}` instead. |
| Re-enable `toc: true` in `_quarto.yml` | The Table of Contents jumps to the wrong position in the book | This project places the TOC manually (`contents.qmd`) for position control; `toc: true` overrides that with Quarto's default (wrong) placement. Leave it `false`. |
| Use a caption on a table still wrapped in `\def\LTcaptype{none}` | The caption doesn't number and the table doesn't show up in the List of Tables | That wrapper is Pandoc's own marker for *uncaptioned* tables — remove it once a table gets a real caption. |
| Put an `@citekey` inside a table's `\caption{}` | The citation doesn't resolve (prints literally as `@citekey` or breaks the build) | Citations only resolve in real Markdown text, not inside raw LaTeX blocks. Move it to the surrounding prose. |
| Label a raw-LaTeX table `\label{tab:x}` instead of `\label{tbl-x}` | Render succeeds, but you get a "non-tbl label" warning and can't `@tbl-x`-reference it elsewhere | Quarto's crossref system specifically looks for the `tbl-` prefix. Harmless to leave, free to fix — see [§15](#15-writing-tables). |
| Change the brand fonts without matching filename base to family name | Render fails outright with a "file not found" font error | `fontspec`'s `Path=`/`UprightFont=*-Regular` pattern substitutes the literal family name into the filename — a space in one and not the other breaks the match. |
| Expect a captioned figure/table outside a chapter to show *no* number on the page | It still shows a "Figure N.N:"/"Table N.N:" prefix, just doesn't appear in the List of Figures/Tables | The figure/table counter is one continuous global sequence LaTeX doesn't reset between front/main/back matter — only the *list entry* is suppressed outside chapters, not the on-page number. If the number itself is unwanted on a specific page, omit the caption instead (see [§16](#16-figures)). |
| Expect the Introduction to be Chapter 1 by default | Your first real chapter after it is Chapter 1, and Introduction shows no chapter number | `introduction.qmd`'s heading is `# Introduction {.unnumbered}` out of the box — deliberate, matching how most books number their introduction. Remove `{.unnumbered}` from that one heading if you want Introduction itself to be Chapter 1 instead. |
| Wonder why a book with no tables (or no figures) doesn't show an empty "List of Tables"/"List of Figures" page | It doesn't — this is intentional, not a bug | `\giOptionalTOCList` in `preamble.tex` checks whether each list actually has entries before printing a heading for it at all. Nothing to configure; the two lists are controlled independently of each other. |
| Expect a part-overview paragraph to land right below the part title | It shows up alone on the NEXT page instead, with the title stranded on an otherwise-blank page before it | Standard LaTeX forces a fresh page after every `\part` title by default (the classic "divider page" convention) — `preamble.tex` overrides this (`\titleclass{\part}{top}`) so overview prose stays on the same page. Already fixed in both toolkits; only relevant if you're troubleshooting a customized copy of `preamble.tex` that's lost this line. |
| Expect a figure to render exactly where it's written in the source | It can drift — sometimes several paragraphs or sections away, even splitting a sentence across a page break | LaTeX's default figure placement doesn't guarantee source-order rendering. Fixed via `fig-pos: "H"` in `_quarto.yml`, already set in both toolkits — only relevant if troubleshooting a project where that setting was removed or a `_quarto.yml` predates this fix. |
| Give a chapter only landscape content (no portrait text first) and use a normal Markdown heading anyway | Wastes a whole page: the title sits alone on an otherwise-blank portrait page before the landscape content starts on the next page | Use `\giLandscapeChapterOpener{Title}` + `\giLandscapeChapterHeader` instead — see [§18](#18-landscape-wide-pages) — which shows the title horizontally as part of the landscape page's own header instead of a separate page. |
| Type an em dash or en dash directly into a chapter/section heading | It renders as a literal `---`/`--` instead of a real dash, even though the same character in body text right below it renders correctly | `\setmainfont` gets fontspec's `Mapping=tex-text` feature automatically; `\newfontfamily` (used for `\headingfont`) doesn't. Already fixed in both toolkits' `brand.tex`; only relevant if you're troubleshooting a swapped-in custom heading font that's missing that option. |
| Write a long chapter title that wraps to two lines | The wrapped-to line shows visibly uneven, overly wide gaps between words, worse the fewer words on that line | LaTeX fully justifies `\Huge`-size text by default, and stretching a handful of huge words to exactly fill the line width looks bad. Already fixed via `\raggedright` in `\titleformat{\chapter}` in both toolkits' `preamble.tex` — only relevant if troubleshooting a customized copy that's lost this setting. Long two-line Part titles were never affected — Parts already use `\centering`. |

## 24. Quick-reference cheat sheet

```
Render the book:              quarto render
Live preview while writing:   quarto preview
Health check (installs etc):  quarto check
Add a chapter:                new .qmd file + add to chapters: in _quarto.yml
Force a page break:           {{< pagebreak >}}
Cite a source:                @citekey   or   [@citekey]
Footnote:                     text[^1]  ...  [^1]: note text
Cross-reference a table:      \label{tbl-name} in the table, @tbl-name in prose
Cross-reference a figure:     {#fig-name} in the image, @fig-name in prose
Edit title page text:         title-page-content.tex
Edit brand colors/fonts:      brand.tex
Where the bibliography prints: references.qmd
```
