<p align="center">
  <img src="assets/colophon-logomark-banner.png" width="550" alt="Colophon logo">
</p>

# Colophon — a Quarto Book Scaffold

[![Latest Release](https://img.shields.io/github/v/release/belpub/colophon-quarto)](https://github.com/belpub/colophon-quarto/releases/latest)

A Quarto and LaTeX scaffold for turning Markdown into properly
typeset book PDFs: course books, manuals, certification guides,
technical handbooks. The idea is that you shouldn't have to
hand-fight LaTeX every time you add a chapter, a table, or a
landscape page.

**Why this exists.** 

Quarto renders books well out of the box, but a
handful of things real books need aren't solved by the defaults: a
caption that actually shows up in the List of Tables, a wide table
that rotates to landscape without wasting a page, an appendix that
reads "Appendix A" instead of "Chapter 11," page numbering that stays
correct across front matter, chapters, and back matter. None of these
are exotic requirements. They just aren't obvious to fix, and the
fixes aren't documented in any one place. This toolkit is those
fixes, already made, so you don't have to rediscover each one the
hard way.

**What's already handled for you:**
- Front, main, and back matter with correct, continuous page
  numbering — roman numerals, then arabic, then roman again — across
  the whole book
- A List of Tables and List of Figures that only show up when
  there's something to list, instead of an empty page with a title
  and nothing under it
- Figures and tables that render exactly where you place them,
  rather than wherever LaTeX's default float behaviour happens to
  push them
- Landscape pages for wide tables, including a landscape-only chapter
  opener so a chapter that's nothing but a wide table doesn't waste a
  page on a bare portrait title first
- Numbered appendices ("Appendix A," "Appendix B," and so on) that
  switch cleanly back to regular chapters or to roman-numbered back
  matter
- APA citation formatting out of the box, and one YAML line to swap
  it for a different style
- A documented way to add diagrams (Mermaid, Graphviz) that avoids a
  real Chrome dependency issue in Quarto's built-in PDF rendering

**Honest scope note:** this has been tested thoroughly against one
real, full-length book — 70+ pages, several parts, landscape
appendices, dozens of captioned tables — through repeated, deliberate
bug-hunting. It hasn't yet been tested against a wide range of other
people's projects. If you run into something it doesn't handle,
please open an issue. That's exactly how this gets more solid.

## Who this is for

You're a good fit for this toolkit if you're writing something
book-length — a course book, training manual, certification guide,
internal handbook, technical reference — not a single article or a
slide deck. You want the finished result to actually look
professionally typeset: proper front and back matter, working
cross-references, a real List of Tables, without hand-coding LaTeX
yourself. You're comfortable writing plain Markdown, or willing to
learn it (it's smaller than it sounds), but you don't want to become
a LaTeX expert just to get a clean PDF out the other end. And you'd
rather start from something that's already hit its sharp edges than
find them yourself three chapters into your own book.

You're probably not the right fit if you need a single one-off
document — a plain Quarto setup is simpler for that — or if your
output is a website rather than a PDF. This toolkit's whole value is
in the print-specific typesetting problems it solves, and those
problems mostly don't exist on the web.

**You don't need to know LaTeX to use this day to day.** 

You write Markdown, and `preamble.tex` and `brand.tex` handle the LaTeX
underneath, invisibly. The only time you'll touch raw LaTeX directly
is for the handful of things Markdown genuinely can't express on its
own — a captioned table, a landscape page — and those come with
working examples you can copy and paste, not something you have to
work out yourself.

New to this project? Start with [**`docs/BOOK-CREATION-GUIDE.md`**](https://github.com/belpub/colophon-quarto/blob/main/docs/BOOK-CREATION-GUIDE.md) —
it walks through installing Quarto, VS Code, and TinyTeX from
scratch, then covers the everyday tasks: adding a chapter, tables,
citations, branding. Think of this README as the layer underneath
that guide — the reasoning behind the less obvious design choices,
for whoever ends up maintaining or extending this.

## What this is
A brand-agnostic starting point for producing book-style PDFs —
course books, manuals, handbooks — with Quarto and LaTeX. It renders
out of the box with zero setup: no fonts to source, no logo
required, generic placeholder colors. That's deliberate, so you can
confirm the whole pipeline works before you customize anything.
Turning it into a real brand later is a small, contained edit (see
"Making this your own" below), not a rewrite.

## Before you render
One-time setup: `quarto install tinytex` (see
[`docs/BOOK-CREATION-GUIDE.md`](https://github.com/belpub/colophon-quarto/blob/main/docs/BOOK-CREATION-GUIDE.md) for full installation steps if this is
a new machine). That's it — everything else this toolkit needs
ships with it.

## Getting a project — two ways
The simplest option, and what most people want, is to **clone the
whole repo**. The engine — `preamble.tex`, plus the format-level
defaults it sets, like `fig-pos`, `documentclass`, and page geometry
— already lives in `_extensions/colophon/`, packaged as a proper
Quarto [project-type
extension](https://quarto.org/docs/extensions/project-types.html).
There's nothing extra to install. `quarto render` works the moment
you clone.

If you already have a Quarto book project going and just want this
toolkit's machinery, without its example content, you can **add just
the engine**:
```bash
quarto add belpub/colophon-quarto
```
This installs into `_extensions/belpub/colophon/` — namespaced by
the GitHub owner. That's worth knowing because the project type
reference in your own `_quarto.yml` needs the same namespacing:
```yaml
project:
  type: belpub/colophon
```
`type: colophon` on its own won't resolve. (This README used to say
otherwise — the mistake only surfaced once someone actually ran the
install command instead of trusting the theory of how it should
work, which is a good reminder to test rather than assume when you're
writing your own extension.) Once it's installed, copy in `brand.tex`,
`title-page.tex`, and `title-page-content.tex` from this repo as your
starting point. Those three live at the project root, not inside the
extension, because they're exactly the files you're meant to
customize per book.

When a later engine update comes along, pull it in with:
```bash
quarto update extension belpub/colophon-quarto
```

That one command is really the point of packaging this as an
extension rather than something you just copy. A future fix to the
engine reaches you without touching anything you've written. Testing
the actual conversion surfaced a real bug worth knowing about, too:
the extension mechanism loads `preamble.tex` *before* `brand.tex`,
the opposite of what a plain file list would do. That broke exactly
one line — `preamble.tex` was setting the body text color directly,
before `brand.tex`'s colors even existed yet. The fix was wrapping
that one line in `\AtBeginDocument{}`, so it waits until the whole
preamble has loaded. Safe either way this toolkit ends up being used.

## Rendering
    quarto render

## Live preview while writing
    quarto preview

## Making this your own — brand.tex and title-page-content.tex
Two files hold everything specific to *your* book; nothing else in
the project should need to change:

- **`brand.tex`** — colors, fonts, and the running-header book title.
  Ships with generic placeholder colors and two freely-licensed fonts
  (TeX Gyre Termes for body, TeX Gyre Heros for headings) that come
  bundled with every standard TeX install — no font files to source
  just to get started. When you have real brand fonts, replace the
  two font blocks with the `Path=fonts/` pattern that's commented
  in-line in `brand.tex` (put the actual static-weight `.ttf` files
  in a `fonts/` folder first — variable-font files don't work
  reliably with LaTeX's font engine, and the font family name you
  pass to `\setmainfont`/`\newfontfamily` needs to exactly match your
  files' basename, e.g. family `YourFont` needs files named
  `YourFont-Regular.ttf`, not `Your Font-Regular.ttf` — a mismatch
  here is a real, easy-to-hit render error).
- **`title-page-content.tex`** — the actual words on the title page
  (title, subtitle, tagline, which logo file to use). Placeholder
  text throughout, ready to replace.
- **`logo-placeholder.png`** is a generic stand-in so the title page
  renders correctly out of the box. Replace it with a real logo file
  and update the filename in `title-page-content.tex` to match.

Everything else — `preamble.tex` (the structural engine: page
layout, table mechanics, front/back-matter page numbering, landscape
handling) — is brand-agnostic by design and shouldn't need to
change. See "Front matter / main matter architecture" below for why
that separation matters and what happens if the two get mixed back
together.

## Cover page (optional)
Off by default — most books built on this toolkit go straight from
nothing to the title page, and that stays true unless you turn this
on. To add a full-bleed cover image before the title page, add
`cover-page.tex` to `include-before-body` in `_quarto.yml`, right
before `title-page.tex`:
```yaml
include-before-body:
  - cover-page.tex
  - title-page.tex
```
Then set `\giCoverPageImage` in `title-page-content.tex` to your own
image file (it defaults to `cover-placeholder.png`, a working
example sized to the right proportions).

"Full-bleed" means the image fills the entire physical page, edge to
edge, with none of the rest of the book's normal 2cm margin. That's
done with `\newgeometry{margin=0pt}` right before the image and
`\restoregeometry` right after — both part of the `geometry`
package, already loaded for everything else in this document, so
nothing new to install. Tested end to end: the cover page itself has
no page number and isn't part of the roman/arabic numbering sequence
at all, the title page right after it renders with normal margins
restored, and nothing about the rest of the book's page numbering
shifts by adding it.

The image gets stretched to fill the page exactly, not cropped, so
it needs to already be sized to A4's proportions (roughly 0.707:1,
width to height) to avoid looking distorted —
`cover-placeholder.png` (1240×1754px) is a working example of that
ratio to match if you're sizing your own.

One distinction worth knowing if this book is ever going to a
professional print shop: what's here is full-bleed within the page's
own trim size, not press-ready bleed that extends past it. A
physically printed, trimmed book cover usually needs actual bleed —
commonly 3mm past the edge, so a slight trim misalignment doesn't
leave a white sliver — and that's normally supplied as a separate
file built to the printer's own template, not generated by this same
pipeline. For a PDF that's viewed on screen, printed at home or in an
office, or sent to a simpler print-on-demand service that just wants
a full-page image, what's here is the right tool.

## Editing
- Write chapter prose in Typora or VS Code — plain Markdown.
- To add a new chapter, create a .qmd file and add its filename to
  the `chapters:` list in `_quarto.yml`.
- `chapter-01.qmd` has a working example of a landscape wide table,
  with brand-colored header/first-column shading and proportional
  (not equal-width) columns.
- `chapter-02.qmd` has a working example of a manual page break.
- `example.qmd` is a quick-reference chapter — every distinctive
  pattern in one place, with no narrative around it: captioned
  tables, captioned figures, landscape tables, and the two patterns
  that can't run inside an ordinary chapter (the landscape-only
  chapter opener, switching to appendices), shown as plain text
  instead since they can't actually execute there. Delete it once
  you've started writing your own content and don't need the cheat
  sheet anymore.

## Em dashes and en dashes in headings
Chapter titles and section headings render em dashes (—) and en
dashes (–) correctly out of the box, but it took a real fix to get
there, and it's worth knowing about if you ever add a new custom
font. The body text font gets this for free: `\setmainfont` picks up
fontspec's `Mapping=tex-text` feature automatically, which is what
turns LaTeX's `---`/`--` conventions into actual em/en-dash glyphs.
`\newfontfamily` — used for `\headingfont`, the font chapter and
section titles are set in — doesn't get this automatically. Without
it, a heading with an em dash renders as a literal `---` right on the
page, even while body text with the exact same character renders
correctly just below it. Both this toolkit's and its sibling's
`brand.tex` add `Mapping = tex-text` to the `\newfontfamily
\headingfont{...}` line specifically to fix this, and it works the
same way whether the heading font comes from local files or a system
font (this toolkit's default, TeX Gyre Heros, is the latter). If you
ever swap in a different heading font, carry that option over into
the new font's own `\newfontfamily` block — leave it out and this
exact bug comes back.

## Long chapter titles wrapping to two lines
`\titleformat{\chapter}` in `preamble.tex` sets `\raggedright` on the
title text. That's fixing a real defect, not adding a safeguard
against a hypothetical one. A long chapter title that wraps to a
second line otherwise gets fully justified at `\Huge` size — LaTeX's
normal behavior for a paragraph — and stretching a handful of huge
words to exactly fill a line's width leaves visibly uneven, overly
wide gaps between them, worse the fewer words that line has. A real
long title ("Dissecting Twelve Unique Security Failures and
Governance Layers Across a Decade") showed the problem clearly, and
`\raggedright` fixes it with no effect on short, single-line titles —
nothing to configure per chapter either way. Parts don't need the
same fix: `\titleformat{\part}` already centers its text instead of
justifying it, so a long two-line part title was never affected.

## Colors
Generic placeholders, ready to replace in `brand.tex`:

| Color | Hex | Used for |
|---|---|---|
| Primary (`giPrimary`) | `#1E3A5F` | Headings, primary UI |
| Secondary (`giSecondary`) | `#4A4A68` | Secondary accents, subsections |
| Accent (`giCrimson`) | `#A6303F` | Core accent — titles, rules. Also doubles as the risk-table "danger" color. |
| Ink (`giInk`) | `#1A1A1A` | Body text |

There are also supporting tints and neutrals — warning, success,
accent link color, grays — all defined in `brand.tex`, all
placeholders too.

## Logo
`logo-placeholder.png` is a generic stand-in, referenced from
`title-page-content.tex` rather than hardcoded into the title page
layout itself (see "Making this your own" above). See "Logo usage"
further down for how to replace it and size a real logo correctly.

## Known ordering fix (already applied)
Quarto's `toc: true` auto-inserts the Table of Contents at a fixed
template location, and that location lands before a custom title
page. This scaffold turns that off (`toc: false`) and places the TOC
manually instead, via `contents.qmd`, positioned correctly after the
Preface and before the Introduction. If you ever re-enable `toc:
true`, this problem comes right back — leave it as `false`.

## Front matter / main matter architecture (read before editing)
This scaffold's page-numbering and chapter-numbering correctness
depends on a few non-obvious things Quarto's book template does
under the hood. If you restructure the front matter, keep these in
mind:

1. **The title page lives in `title-page.tex`, not in a `.qmd`
   file**, and is pulled in via `include-before-body`. Any raw LaTeX
   content placed *before* a Markdown heading inside a `.qmd` chapter
   file gets silently reordered by Quarto to *after* that heading in
   the rendered output — so a title page written inside `index.qmd`
   above a heading does not render first. Putting it in
   `include-before-body` guarantees it's the literal first thing in
   the document, with no `\chapter` wrapper (which would otherwise
   force an extra blank page ahead of it).
2. **`index.qmd` is required by Quarto** as the book's home page and
   must appear first in `book: chapters:` — this can't be removed.
   Rather than waste that mandatory slot on a blank page, it doubles
   as the Copyright page here.
3. **Quarto/Pandoc's book template unconditionally inserts its own
   `\mainmatter`** immediately before your `chapters:` content begins
   (i.e. right before Copyright), regardless of any `\frontmatter`/
   `\mainmatter` you write yourself. Left alone, this silently forces
   arabic page numbers on Copyright/Preface/TOC. `preamble.tex`
   neutralizes that one automatic call and exposes `\giEnableMainMatter`
   instead — fired exactly once, at the end of `contents.qmd`, right
   before the Introduction chapter. Don't add your own raw
   `\mainmatter` elsewhere; use `\giEnableMainMatter` at the one spot
   main matter should actually begin.
4. **A `.qmd` chapter file with no heading at all gets auto-wrapped
   in a numbered `\chapter{}`** by Quarto (a stray, visible "Chapter
   N" on a blank page). Give any heading-less content an
   `{.unnumbered .unlisted}` heading instead (see `contents.qmd`).
5. **`\tableofcontents` prints its own internal chapter heading.**
   Pairing it with your own Markdown heading produces two
   chapter-openings in a row (an extra blank page). `contents.qmd`
   uses `\@starttoc{toc}` instead — it renders only the entries,
   under the Markdown heading you control.
6. **Any raw LaTeX command that forces its own page break
   (`\frontmatter`, `\mainmatter`, `\backmatter`, `\cleardoublepage`,
   etc.) must execute *before* the next chapter's heading, never
   after** — otherwise the reordering behavior in point 1 will insert
   the break in the middle of that chapter (heading on one page, body
   text pushed to the next). The fix is always the same: put the
   command at the very end of the *previous* file, not the top of the
   next one. See `\backmatter` at the end of `chapter-02.qmd`.

## Parts (grouping chapters under Part I, Part II, ...)
Neither toolkit uses Parts by default — the example content is a
flat chapter list — but Quarto's native support for them works
cleanly with everything else in this scaffold. That's been checked
by actually building a Parts-based version and looking at the
numbering, the TOC, and the running headers, not just reading
Quarto's own documentation on the feature. To add a Part, use a
`part:` entry in `_quarto.yml`'s `chapters:` list:

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
level-one Markdown heading (`# Part One: Foundations`) — that
heading text becomes the part title. A short overview paragraph
right after the heading works too, so it isn't limited to
heading-only files (see the next point below). Quarto also accepts a
quoted string directly in the YAML instead of a file — `part: "Part
One: Foundations"` — but a file is the more common pattern, and the
one that's actually been tested here.

Here's what to expect, based on a real test render rather than the
docs alone:
- **Part numbering is automatic** — "Part I," "Part II" — and styled
  to match the rest of the book through the `\titleformat{\part}`
  block in `preamble.tex`: crimson label, navy title, crimson rule,
  centered, the same visual language as a chapter opening, just
  larger and on its own divider page. This styling does nothing if
  you never use `part:`, so it's safe to leave in place either way.
- **A part-overview paragraph lands on the same page as the title,
  not a separate one.** The standard LaTeX book class always forces
  a fresh page right after a Part title, even with nothing else on
  it — the classic print convention of a Part as its own bare
  "divider" page. That's not what most people expect from a short
  intro paragraph sitting right under a `# Part Title` heading, so
  `preamble.tex` overrides it. `\titleclass{\part}{top}`, placed
  right before the `\titleformat{\part}` block, reclassifies `\part`
  from titlesec's default "page" class to "top" — the same class
  `\chapter` uses — which keeps the title starting its own fresh
  page but stops it from also forcing whatever comes after onto a
  separate one. This had to go through titlesec's own `\titleclass`
  mechanism specifically: titlesec fully replaces LaTeX's `\part`
  command internally, so the more traditional kernel-level fix
  (redefining `\@endpart`) silently does nothing once titlesec is
  loaded. Worth knowing if you ever go digging into how this works.
  A heading-only part file is unaffected either way — its first
  chapter still starts its own fresh page regardless, since
  `\chapter` forces that independently of `\part`.
- **Chapter numbers don't reset per part.** They count straight
  through the whole book — Chapter 1, 2, 3, and so on, regardless of
  which part they fall in — which is the standard convention for
  most books. `\thepart` defaults to roman numerals; switch to
  `\arabic{part}` in `preamble.tex`'s `\titleformat{\part}` block if
  you'd rather have "Part 1, Part 2."
- **The Table of Contents nests correctly on its own.** Part entries
  show up bold and prominent, with their chapters indented
  underneath, no extra work needed.
- **Running headers aren't affected.** Inside a chapter's pages, the
  header still correctly shows that chapter's name (via
  `\leftmark`), not the part name.
- **Every other custom mechanism in this scaffold keeps working as
  normal** — front/back matter page numbering, the chapters-only
  gate on the List of Tables/Figures, landscape pages, citations.
  Parts sit at a structural level Quarto handles independently of
  all of it.

## Header / footer
- Chapter-opening pages show no header — just a page number in the
  footer — since the large chapter title on that page already states
  where you are. Only continuation pages get the running header.
- The running header shows one item at a time, alternating by page:
  the current chapter name on odd pages, the book title
  (`\giBookTitle` in `brand.tex`) on even pages. This is
  deliberate — putting both on the same line risks collision or an
  ugly wrap if either title is long. The header box wraps to a second
  line gracefully for long titles rather than overflowing.
- Landscape pages don't use the running header/footer at all.
  `pdflscape` rotates the *body* content of a `landscape` block, but
  has no effect on `\fancyhead`/`\fancyfoot`, which draw against the
  physical (portrait) page edges — so on a landscape page they'd
  render sideways in the margin. Instead, `\giLandscapeHeader` /
  `\giLandscapeFooter` (in `preamble.tex`) print a matching header/
  footer line as ordinary content *inside* the `landscape` block, so
  it's part of the same rotated box as the table and reads normally
  horizontal. `\giLandscapeFooter` uses `\vfill` to anchor the page
  number to the physical bottom of the page regardless of how much
  content the table takes up — always call `\giLandscapeHeader` right
  after `\begin{landscape}` and `\giLandscapeFooter` right before
  `\end{landscape}` — see `chapter-01.qmd`.

## A chapter whose only content is a landscape table or figure
Use `\giLandscapeChapterOpener{Title}` and `\giLandscapeChapterHeader`
(both in `preamble.tex`) instead of a normal Markdown heading and
`\giLandscapeHeader` — see the working example in `chapter-01.qmd`
if one's present in this project, or build one following the pattern
below.

The problem this solves: a normal Markdown `# Heading` always
produces a full portrait page (the chapter title, styled large, on
its own page) before any content follows. That's the right behavior
when real portrait content follows the title on the same page — but
if a chapter's first (or only) content is a landscape table/figure,
the portrait title page ends up nearly blank (title only — nothing
else fits before the landscape rotation needs its own page), and the
actual content only starts on the page *after* that. Confirmed and
fixed by testing a chapter whose entire content was one landscape
table: the wasted page is gone, and the chapter title now appears
horizontally, as part of the landscape page's own header, right
above the table.

Usage — raw LaTeX, not a Markdown heading, since the whole point is
to skip the normal heading-driven title page. In your `.qmd` file's
chapter content, this replaces the normal `# Heading`:

```latex
\giLandscapeChapterOpener{A Chapter That Is Only A Landscape Table}
\begin{landscape}
\giLandscapeChapterHeader

... table or figure, exactly as in a normal landscape block ...

\giLandscapeFooter
\end{landscape}
```

(Wrap the block above in a ```` ```{=latex} ````/```` ``` ```` raw
LaTeX fence in the actual `.qmd` file, the same as any other raw
LaTeX content in this project.)

`\giLandscapeChapterOpener{Title}` does the same bookkeeping a normal
chapter heading does — advances the chapter counter, adds the Table
of Contents entry, sets the running header text for later pages —
just without typesetting a separate portrait title page.
`\giLandscapeChapterHeader` (no arguments — it reuses the title
`\giLandscapeChapterOpener` already stored) shows "Chapter N: Title"
horizontally at the top of the landscape page itself, in place of the
normal `\giLandscapeHeader`.

**If the chapter has any portrait content before the landscape block
— even one paragraph — use a normal Markdown heading and
`\giLandscapeHeader` instead**, exactly as documented above; this
pair is specifically for the landscape-content-only case.

This surfaced a real, separate bug worth knowing about, already
fixed as part of building this: a chapter `.qmd` file with no
Markdown heading at all (true here, since the opener replaces the
heading with raw LaTeX) makes Quarto auto-insert an empty `\chapter{}`
of its own — the book project model expects every chapter file to
correspond to some heading, and falls back to a blank one when it
finds none. Left alone, that produced an even worse version of the
original problem (a second, completely blank "Chapter N" page,
consuming a chapter number `\giLandscapeChapterOpener`'s own numbering
then had to increment past). Chasing that down surfaced a second,
more fundamental bug in the fix itself, corrected in the same pass:
naively redefining `\chapter` to ignore an empty title broke every
*starred* chapter call in the whole project (`\chapter*{Copyright}`,
`\chapter*{Preface}`, `\chapter*{Introduction}`, etc.) — LaTeX's
undelimited-argument grabbing took the literal `*` character as the
"title" for each of those, which is non-empty, so they silently fell
through to an *unstarred*, numbered chapter call instead. The
corrected version in `preamble.tex` uses proper `\@ifstar`-based
dispatch: a starred call goes straight through to the real `\chapter*`
untouched, and the empty-title check only ever applies to the plain,
unstarred form Quarto's auto-inserted wrapper actually uses.

## Table styling
- Tables render left-aligned (flush with body text), not centered —
  `\LTleft`/`\LTright` in `preamble.tex` override `longtable`'s
  default centering behavior for every table in the book, Pandoc-
  generated or raw.
- Column widths in raw-LaTeX tables are weighted by expected content
  length, not equal — Pandoc's auto-generated Markdown tables always
  split width equally across columns, which wastes space on short
  columns and forces long-text columns to wrap onto extra lines
  instead of using the room available. The `chapter-01.qmd` example
  weights each `p{}` column width (`\real{0.09}`, `\real{0.28}`,
  etc. — the eight values sum to 1.0) based on how much text each
  column typically holds, not its header text. When you add a table
  with very different column content, adjust these weights to match;
  there's no formula, just eyeball roughly how wide each column's
  *typical* content needs to be.
- Three brand shading colors are defined in `brand.tex`:
  `giTableHeaderShade` (header row), `giTableFirstColShade`
  (first column), `giTableAltRowShade` (alternating body rows —
  defined but unused by default, see below), and `giTableDivider`
  (hairline row rules). Pandoc's
  Markdown-table output can't target the header row or a single
  column — only whole-row striping via `\rowcolors`. For header/
  column-level shading, write the table as raw LaTeX and combine
  `\rowcolor` (whole row) with `\cellcolor` (single cell, overrides
  the row color) — see the worked example in `chapter-01.qmd`.
- That example uses hairline dividers between rows instead of
  alternating (zebra) row shading. Zebra striping plus rules on the
  same table tends to compete visually — two separate row-separation
  techniques doing the same job — and reads as busier/less clean than
  picking one. Dividers plus the header/first-column shading is the
  current recommendation; `giTableAltRowShade` is still defined if
  you'd rather swap back to zebra striping for a given table (just
  use `\rowcolor{giTableAltRowShade}` on alternating rows instead of
  `\arrayrulecolor{giTableDivider}\hline` between them).

## Back matter
`closing.qmd` holds unnumbered back-matter pages (institute bio,
thank-you) after the last numbered chapter. Add more the same way —
new `{.unnumbered}` headings in that file, or new files appended to
`chapters:` after it. Call `\giEnableBackMatter` at the true end of
the last numbered chapter (see `chapter-02.qmd`) — same "must fire
before the next heading, not after" rule as `\giEnableMainMatter`.
It switches page numbering back to roman, *continuing* from where
front matter left off (front matter ends at "iii" → back matter
starts at "iv") rather than restarting at "i" — this keeps every
page label in the document unique, which matters if anyone ever
needs to cite or reference a specific page. That continuation point
is captured by `\giMarkFrontMatterEnd`, called at the end of
`contents.qmd` right before `\giEnableMainMatter` — if you add or
remove front-matter pages, that's automatic; you don't need to
hand-count anything.

## Known limitation
Per-row \rowcolor commands (e.g. to shade just the header row a
different color than the body) don't work with Pandoc's
auto-generated Markdown tables — only with hand-written raw LaTeX
tables. The current setup zebra-stripes the body rows and leaves the
header distinguished by its border rule, which covers most needs.
If a specific table needs a truly shaded header, that one table
would need to be written as raw LaTeX instead of a Markdown table
(see `chapter-01.qmd` for a full worked example with header,
first-column, and alternating-row shading combined).

## Logo usage
`logo-placeholder.png` (500×150) is a generic placeholder, sized to
match a typical horizontal logo lockup. Once you have a real logo:

- If it's a wide/horizontal lockup, replace `logo-placeholder.png`
  directly (same dimensions) and update the filename in
  `title-page-content.tex` if you rename the file.
- If your real logo is square or vertical instead, that's fine too —
  just update `\giTitlePageLogoWidth` in `title-page-content.tex` to
  a width that reads well at that aspect ratio (a square logo
  usually wants a narrower width than a wide horizontal one).
- If you have multiple logo variants (horizontal + square/icon-only,
  say), use the horizontal one for the large title-page placement,
  and reserve the square/icon one for anywhere small — a running-
  header mark, favicon-style badge, certificate seal — where a wide
  lockup's fine print would blur out at small size.

Whatever you use, check its resolution against how large it renders:
a 500px-wide file at the current title-page size (`0.42\textwidth`
on an A4 page) renders around 175–180 DPI — fine for on-screen PDF
viewing, not for print. If this book ever needs to be printed, or
the logo needs to render larger, get a vector version (SVG/EPS/PDF)
or a much higher-resolution raster (1500–2000px+) instead, so it
scales cleanly to any size used across the book.

## Captions, alt text, and the Lists of Tables/Figures

**The short version, if you just want the two correct patterns:**

A captioned table (raw LaTeX, always — see why below):
```latex
\begin{longtable}[]{@{}
  >{\raggedright\arraybackslash}p{0.3\linewidth}
  >{\raggedright\arraybackslash}p{0.6\linewidth}
@{}}
\toprule
\textbf{Term} & \textbf{Definition} \\
\midrule
\endhead
Asset & Something of value \\
\bottomrule
\caption{\label{tbl-terms}Definitions used throughout this chapter.}
\end{longtable}
```

A captioned figure (plain Markdown, no raw LaTeX needed):
```markdown
![Definitions used throughout this chapter.](diagram.png){#fig-terms width="60%" fig-alt="A diagram showing how the four key terms relate to one another."}
```

Both of those, written exactly like that, will render correctly on
the page **and** show up correctly in the List of Tables/Figures,
with working `@tbl-terms`/`@fig-terms` cross-references. The rest of
this section explains the two things people most often trip up on:
the difference between a *caption* and *alt text*, and why tables and
figures need different syntax in the first place.

### Caption vs. alt text — these are two different things
- **Caption** — the visible text printed under the table/figure on
  the page, and the exact text that shows up as the entry in the
  List of Tables/Figures. For a table, this is whatever's inside
  `\caption{...}`. For a figure, this is the text inside the square
  brackets: `![`**this part**`](image.png)`.
- **Alt text** — a separate, *invisible* description for
  accessibility (screen readers, PDF accessibility tools) — it never
  prints on the page and never appears in the List of Figures. For a
  figure, this is the optional `fig-alt="..."` attribute. **Tables
  don't have a separate alt-text attribute in this toolkit** — a
  table's accessible description is just its caption; there's no
  raw-LaTeX equivalent of `fig-alt` for a `longtable`.

Confusing the two is the single most common mistake here — writing a
long accessibility description as the caption (so it prints as
awkwardly long text under the image) or a caption as the alt text
(so it never shows up in the List of Figures, since the List only
ever reads the *caption*, not `fig-alt`). The caption should be
short — what you'd want to see if you were scanning the List of
Figures — while `fig-alt` can be longer and more literally
descriptive, since a screen-reader user only ever hears it once,
in place of seeing the image itself.

A figure with both, correctly separated:
```markdown
![**The inside-out trust model** — Layer 0's four components at the
core, surrounded by Layers 1 through 3.](diagram.png){#fig-model
fig-alt="The illustration depicts the inside-out trust model,
featuring four central components in Layer 0 at its core, encircled
by Layers 1 to 3, with the six governance layers forming the
outermost perimeter."}
```
The bracketed text is short and scannable — exactly what you'd want
in the List of Figures. `fig-alt` is longer and more literal, since
its only job is standing in for the image for someone who can't see
it.

### Tables — raw LaTeX, always, for a caption
- A *table header* — the bold label right above a table, like
  "Sample requirements register" in `chapter-01.qmd` — is just an
  ordinary bold Markdown paragraph. No special syntax; works the
  same above a Markdown table or a raw-LaTeX one, and is unrelated to
  the caption itself (a table can have a header, a caption, both, or
  neither).
- *Captions* need raw LaTeX, always. Pandoc's native Markdown table
  caption syntax (a `:` line after the table) hardcodes the caption
  **above** the table with no way to move it. To get a caption below
  the table, write the table as raw LaTeX and place
  `\caption{}\label{}` near the end, right before `\end{longtable}`
  — see either table in `chapter-01.qmd` for the pattern. Do **not**
  wrap a captioned table in `\def\LTcaptype{none}` — that's Pandoc's
  own convention for suppressing numbering on *uncaptioned* tables;
  leaving it on a captioned table stops it from numbering and keeps
  it out of the List of Tables.
- Citations don't resolve inside a raw LaTeX block — Quarto's
  citation processor only sees actual Markdown text, not content
  inside `` ```{=latex} `` fences. Keep any `@citekey` in the prose
  around the table, not inside `\caption{}` itself.
- Label raw-LaTeX tables `\label{tbl-yourname}` — the `tbl-` prefix
  matters. With it, Quarto's crossref system treats the table like
  any native one, so `@tbl-yourname` elsewhere in the text
  auto-resolves to "Table 2.1" (or whatever it ends up numbered),
  and stays correct if tables get added/reordered later — see
  `@tbl-terms`/`@tbl-controls` in `chapter-01.qmd` for a working
  example. Without the `tbl-` prefix (e.g. a plain `\label{tab:x}`),
  rendering still works fine, but you'll see a `quarto render`
  warning like "Raw LaTeX table found with non-tbl label" — harmless
  to leave (the PDF still builds correctly, the table just isn't
  reachable via `@key` cross-references), but the `tbl-` prefix is
  free to add and worth doing from the start.
- The List of Tables sits right after the Table of Contents in
  `contents.qmd`, using the same `\@starttoc` pattern that placed the
  TOC itself (`\@starttoc{lot}` instead of `\@starttoc{toc}` — see
  the comments there for why `\listoftables` isn't used directly). It
  fills in automatically from every properly-captioned table in the
  book, in reading order, no manual maintenance needed. **A table
  with no `\caption{}` at all never appears here** — that's normal
  and expected for an uncaptioned reference table you don't want
  listed, not a bug.

### Figures — plain Markdown, no raw LaTeX needed
- Figures are simpler than tables here — plain Markdown image syntax
  works, and the caption prints **below** the image by default
  (Pandoc's normal behavior for figures — no override required the
  way tables needed one):
  ```markdown
  ![Caption text goes here.](image.png){#fig-yourname width="40%"}
  ```
- Same `fig-` labeling convention as tables' `tbl-` prefix — it's
  what makes `@fig-yourname` resolve elsewhere in the text to
  "Figure 3.1" (or whatever it's numbered) and enters it into the
  List of Figures. See `@fig-logo` in `chapter-02.qmd` for a working
  example. **The `#fig-yourname` label is what makes an image count
  as a captioned figure at all** — an image with no label and no
  bracketed caption text renders as a plain, uncaptioned picture,
  same as an uncaptioned table: correct if that's what you want,
  invisible to the List of Figures if not.
- The List of Figures sits right after the List of Tables in
  `contents.qmd`, same `\@starttoc{lof}` pattern as the other two.
- **The List of Figures, and the List of Tables, only count what's
  inside a real chapter — and that's enforced automatically, not
  just something to remember.** Pandoc treats any standalone
  Markdown image with alt text as a captioned figure by default, so
  a completely ordinary-looking image in a front- or back-matter
  file — a logo on an "About" page, say — would otherwise silently
  end up in the List of Figures too. That actually happened during
  development, from a perfectly normal `![...](...)` image, not from
  a mistake anywhere in the source. The "LIST OF FIGURES / LIST OF
  TABLES — CHAPTERS ONLY" section in `preamble.tex` fixes this at
  the mechanism level: front- and back-matter files can caption
  images and tables freely, and they're automatically kept out of
  both lists, nothing to avoid on your end. One small cosmetic side
  effect worth knowing about: a captioned figure or table outside a
  chapter still shows a numbered "Figure N.N:"/"Table N.N:" prefix on
  the page itself, since LaTeX's figure and table counters are one
  global sequence that doesn't reset between front, main, and back
  matter — it just won't appear in either list. If that number looks
  wrong on a specific page, leaving the caption off entirely is still
  a fine option, same as always — see `closing.qmd` for a working
  captioned example either way.

## Figure placement (drifting away from its source position)
`_quarto.yml` sets `fig-pos: "H"`, which forces every figure to
render exactly where it appears in the source, instead of LaTeX's
usual floating behavior.

This is worth explaining honestly, because it needed correcting once
already, and the mistake is worth recording so it doesn't happen
again. An earlier version of this note claimed `[H]` placement was
already Quarto's default in this setup. That conclusion came from
checking only the first few figures in a longer test document, which
happened to have `[H]` — not from checking all of them, which turned
out to matter. A real user report, with their actual chapter content
(a technical book chapter with a diagram placed right after a
subheading), showed the figure landing two subsections later,
splitting a sentence across a page break. Testing it properly the
second time — every figure in an eight-image stress test, not just
the first few — showed Quarto does *not* add `[H]` by default:
figures use a bare `\begin{figure}`, subject to normal LaTeX
float-queueing, which can legitimately push a figure many paragraphs,
even whole sections, away from where it was written. `fig-pos: "H"`
is what actually closes that gap — re-running the exact reported case
with it in place put the figure back where it belonged, right below
its intended subheading.

One extra safety net was tried and abandoned: automatic
`\FloatBarrier` insertion at every chapter and section boundary, via
`\usepackage[section]{placeins}`, layered on top of `[H]`. It broke
chapter titles outright — titlesec, which this project uses for
chapter and section styling, fully replaces LaTeX's own sectioning
commands, and `placeins`'s automatic hook into `\chapter` isn't
compatible with that. It was reverted rather than shipped, and isn't
part of this project — don't add it back without solving that
incompatibility first. `fig-pos: "H"` on its own has been enough in
every case tested so far.

## Diagrams (Mermaid, Graphviz, and other tools)
Quarto has native, built-in support for both Mermaid and Graphviz —
a `` ```{mermaid} `` or `` ```{dot} `` fenced code block right in
your `.qmd` file, no external tool needed to write the diagram
syntax. This toolkit deliberately doesn't use that native support
for PDF output. Use it for HTML if you ever build that, but not
here. The reason is concrete, not theoretical: rendering either a
Mermaid or a Graphviz code block to PDF requires a headless
Chrome/Chromium install, and that dependency turned out to be
genuinely fragile — `quarto install chromium` failed outright in one
real environment because of network restrictions, and it matches a
long, still-active trail of upstream bugs specific to PDF output:
diagrams getting cropped, renders that hang indefinitely, failures
that only show up once a document has more than one diagram in it.
HTML output doesn't have these problems. PDF does. For a book or
course-material toolkit where PDF is the actual deliverable, that's
not a dependency worth carrying.

The reliable pattern instead is to generate the diagram externally,
export it to PNG, and embed it as a normal figure — the same figure
pipeline this toolkit already uses everywhere else, with correct
placement (`fig-pos: "H"`), correct captioning, and correct
chapters-only List of Figures behavior. This has been tested end to
end: generate a diagram with `dot`, embed the PNG with plain
Markdown image syntax, and it renders cleanly with correct numbering
and placement.

```markdown
![A governance review process.](figures/review-process.png){#fig-review width="70%"}
```

**Export to PNG, not SVG.** LaTeX can't embed SVG without an
Inkscape conversion step this toolkit doesn't install — an SVG image
reference fails the render outright. PNG works natively with
`\includegraphics`, no extra dependency needed.

### Graphviz — recommended for flowcharts, hierarchies, dependency graphs
Best for anything that's fundamentally nodes-and-edges without a
time/sequence dimension: process flows, org charts, decision trees,
architecture diagrams, dependency graphs. Mature (decades-old,
extremely stable), free, open source, and — unlike Mermaid — needs
no browser dependency at all for local generation, since `dot`
renders directly to an image file:

```bash
dot -Tpng diagram.dot -o diagram.png
```

A working example, matching the style used elsewhere in this
project (rounded boxes, left-to-right flow — more compact for a
book page than the default top-to-bottom):

```dot
digraph G {
  rankdir=LR;
  node [shape=box, style=rounded, fontname="Helvetica"];
  Request -> Review -> Approve -> Implement;
  Review -> Reject;
}
```

**Tips:**
- `rankdir=LR` (left-to-right) usually fits a portrait book page
  better than the default top-to-bottom layout, which gets tall fast.
- Set `fontname` to something close to the book's body font (e.g.
  `"Helvetica"` or `"Arial"`) — Graphviz's default font looks
  noticeably different from everything else on the page otherwise.
- Keep diagrams narrow rather than wide — a graph much wider than it
  is tall will get shrunk hard by `width=` to fit the text column,
  making labels tiny. If a diagram is naturally wide, consider a
  landscape page (see "A chapter whose only content is a landscape
  table or figure" above) instead of shrinking it to fit portrait.
- Graphviz is installed by default on most Linux distributions and
  available via `apt install graphviz` / `brew install graphviz` /
  the official Windows installer — check with `dot -V` before
  assuming it's missing.

### Mermaid — recommended for sequence diagrams, gantt charts, state diagrams
Best for anything with a time or process-flow dimension Graphviz
doesn't represent naturally: sequence diagrams (who calls whom, in
what order), gantt charts, state machines, user journey maps,
entity-relationship diagrams. The Mermaid *library* — the free,
open-source, MIT-licensed diagramming syntax Quarto embeds — is
**not** the same thing as "Mermaid Chart," a separate paid hosted
editor product from the same team. Nothing in this toolkit's
recommended workflow touches that paid product; both the syntax and
the rendering tooling below are free.

Two ways to generate a PNG:
- **[mermaid.live](https://mermaid.live)** — free web editor, paste
  the diagram syntax, export PNG directly. No install, good for a
  one-off diagram.
- **`mermaid-cli`** (the `@mermaid-js/mermaid-cli` npm package) — for
  a scriptable, repeatable pipeline if you're generating many
  diagrams or want them regenerated automatically when the source
  changes: `npm install -g @mermaid-js/mermaid-cli`, then
  `mmdc -i diagram.mmd -o diagram.png`.

A working example:

```mermaid
sequenceDiagram
  participant User
  participant App
  participant API
  User->>App: Request access
  App->>API: Validate credentials
  API-->>App: Token
  App-->>User: Access granted
```

**Tips:**
- Export at 2x or 3x scale (`mmdc -i diagram.mmd -o diagram.png
  -s 3`) for print-quality resolution — the default export
  resolution is tuned for screens, and looks visibly soft printed at
  book page size.
- Match Mermaid's theme colors to the brand palette in `brand.tex`
  where practical (Mermaid supports a `%%{init: {'theme': ...}}%%`
  directive, or full custom theming via `themeVariables`) — a
  diagram in Mermaid's default blue/purple next to brand-navy text
  reads as visibly off-brand.
- Keep one diagram type consistent across a whole book/course where
  possible (e.g., always Graphviz for architecture diagrams, always
  Mermaid for process sequences) — mixing styles for the same kind
  of content within one book looks inconsistent to a reader even
  when each individual diagram is fine on its own.

### General workflow, both tools
- Keep the diagram source files (`.dot`/`.mmd`) in the project
  alongside the generated PNGs — a `figures/` or `diagrams/` folder
  is a reasonable convention — so a diagram can be edited and
  regenerated later rather than only existing as a flattened image.
- Use `#fig-` prefixed labels on the embedded image, exactly as any
  other figure, so `@fig-yourname` cross-referencing works and it's
  correctly counted for the List of Figures — see "Figures and the
  List of Figures" above.
- If a diagram needs to be wide rather than tall, a landscape page
  (or, if it's the only thing in its chapter, the landscape chapter
  opener above) fits it better than shrinking it into a portrait
  column.

## A book with no tables, or no figures
Both the List of Tables and List of Figures disappear entirely —
heading, page, everything — when there's nothing to put in them, so
a book without any tables (or without any figures) doesn't end up
with an empty, oddly-titled page. This is handled automatically by
`\giOptionalTOCList` in `preamble.tex`, called for each list in
`contents.qmd`; nothing to configure per book. The two lists are
controlled independently — a book with tables but no figures gets a
List of Tables and no List of Figures page, and vice versa.

## The Introduction's chapter number
`introduction.qmd`'s heading is `# Introduction {.unnumbered}` by
default — it appears in the Table of Contents and still starts page
numbering at 1, but doesn't consume "Chapter 1"; your first real
chapter becomes Chapter 1. This matches how most books handle their
introduction. If you'd rather have the Introduction itself *be*
Chapter 1 (some books do it that way), just remove `{.unnumbered}`
from that heading — nothing else needs to change; every chapter after
it renumbers itself automatically either way.

## Citation style
This toolkit ships with APA 7th edition as its default, set via `csl:
apa.csl` in `_quarto.yml`. The `apa.csl` file — fetched from the
official [citation-style-language/styles](https://github.com/citation-style-language/styles)
project, the same source Zotero pulls from — sits in the project
root alongside `references.bib`.

APA is the starting default because it's the most widely recognized
citation convention for professional and practitioner-oriented
content — business, leadership, technical training — which makes it
a reasonable default for most course books built on this toolkit, as
opposed to Chicago (more common in humanities publishing) or IEEE
(specific to engineering and CS journals). This has been checked by
actually rendering and reading the output, not just trusted from the
config: in-text citations use "&" between authors, and the reference
list follows APA's author-date structure (`Author, A. A., & Author,
B. B. (Year). Title. Source.`), both correct APA 7 conventions.

If a specific brand or course wants its own house style, download
the matching `.csl` file from the [Zotero Style
Repository](https://www.zotero.org/styles) — search by name
("Chicago," "IEEE," "Vancouver," and so on), and download the
"in-text" or "author-date" variant rather than "note," unless
footnote-style citations are actually what you want. Drop it in the
project root and point `csl:` in `_quarto.yml` at it. Nothing else
needs to change — the citation syntax in your chapter files
(`@citekey`, `[@citekey]`) stays exactly the same no matter which
style is active. Only the rendered formatting changes.

## Citations & bibliography
- The citation source is `references.bib` (BibTeX format), wired up
  through the top-level `bibliography:` key in `_quarto.yml`.
- Cite with `@citekey` for an inline citation ("as Smith and Jones
  (2021) explain...") or `[@citekey]` for a parenthetical one
  ("...is well established (Smith & Jones, 2021)"). Combine multiple
  sources in one bracket with `[@key1; @key2]`. This works anywhere
  in regular Markdown prose, including inside a Markdown table's own
  caption — just not inside a raw LaTeX block (see above).
- Quarto's default behavior is to append the reference list to the
  very end of the document, wherever that happens to land — the same
  "wrong position" problem the Table of Contents has.
  `references.qmd` takes control of this with a `::: {#refs} :::`
  div, which is where Quarto actually prints the list, placed here
  as its own back-matter page instead of trusting the default. If
  you ever want citations to compile somewhere else, move this div —
  not the `bibliography:` setting.

## First thing to check on your first render
If Quarto auto-generates a second title page, remove any `title:` /
`subtitle:` fields from _quarto.yml — index.qmd supplies the title
page directly.

## License
MIT — see [`LICENSE`](https://github.com/belpub/colophon-quarto/blob/main/LICENSE). Use this for anything: a commercial course, an
internal handbook, a personal book project. Attribution is
appreciated but not required. If you build on it and find something
worth sharing back, a pull request is very welcome.
