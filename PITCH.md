# Colophon: a Quarto book scaffold that's already been through the wringer, so you don't have to

Quarto renders books nicely out of the box. Then you hit something a
real book needs — a caption that actually shows up in the List of
Tables, a wide table that rotates to landscape without wasting a
page, an appendix that reads "Appendix A" instead of "Chapter 11" —
and you discover the fix isn't in the defaults. It's buried in an old
GitHub issue somewhere, or a forum thread from three years ago.

Colophon is those fixes, already made. It's a Quarto scaffold that's
been tested against a real, 70-plus-page book, so you can write
Markdown and get a properly typeset PDF without wrestling LaTeX
chapter by chapter.

## What it actually solves

Front, main, and back matter page numbering that's correct across
the whole book — roman numerals through the front matter, arabic
through the chapters, roman again through the back matter — not just
in the one chapter you happened to test.

A List of Tables and List of Figures that only show up when there's
something to list. No empty, oddly-titled page in a book that
doesn't have any tables.

Figures and tables that render where you actually put them. LaTeX's
default behaviour is to float them wherever its algorithm decides,
which can mean several paragraphs — or even chapters — away from
where you wrote them.

Landscape pages, done properly. If a whole chapter is nothing but a
wide table, there's a landscape-only chapter opener so you're not
stuck with a wasted portrait page showing just the title.

Numbered appendices — "Appendix A," "Appendix B" — that switch
cleanly back to regular chapters or to roman-numbered back matter.
No manual renumbering.

Citation styling that actually works: APA by default, one YAML line
to swap it for something else, and a References section that's
populated instead of mysteriously empty.

A safer path for diagrams. Quarto's built-in Mermaid and Graphviz
rendering for PDF has a real Chrome dependency that trips people up
— this toolkit documents a way around it.

## Who it's for

Anyone writing something book-length in Quarto — a course, a training
manual, a certification guide, an internal handbook — who wants it to
look properly typeset without having to learn LaTeX first. You write
plain Markdown; the engine handles the LaTeX underneath. The handful
of things Markdown genuinely can't do on its own, like a captioned
table or a landscape page, come with working examples you can copy
and paste, not something you have to figure out yourself.

If you just need a single article, a plain Quarto setup is simpler.
Same if your output is a website rather than a PDF — this toolkit is
built for print.

## Honest scope

This has been tested thoroughly against one real, full-length book —
not yet against a wide range of other people's projects. If it
doesn't handle something you need, that's genuinely useful to know.
Open an issue.

It's also packaged as a proper Quarto project-type extension now.
Run `quarto add belpub/colophon-quarto` to add just the engine to a
project you already have, or clone the whole repo for the full
starter template, example content included.

**Repo:** https://github.com/belpub/colophon-quarto — see the [README](https://github.com/belpub/colophon-quarto/blob/main/README.md)
for setup, or [`docs/BOOK-CREATION-GUIDE.md`](https://github.com/belpub/colophon-quarto/blob/main/docs/BOOK-CREATION-GUIDE.md) if you're new to Quarto
and want a walkthrough from scratch.
