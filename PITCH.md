# Colophon — a Quarto book scaffold that's already hit the sharp edges, so you don't have to

Quarto renders books beautifully out of the box, right up until you need something a real book actually needs: a caption that shows up in the List of Tables, a wide table that should rotate to landscape without wasting a page, an appendix that numbers itself "Appendix A" instead of "Chapter 11." None of these are exotic — every book-length document eventually needs them — but the fixes aren't in Quarto's defaults, and they're scattered across old GitHub issues and forum threads if you go looking.

**Colophon is those fixes, pre-built and tested against a real 70+ page book**, so you can write Markdown and get a properly typeset PDF without hand-fighting LaTeX chapter by chapter.

## What it actually solves

- **Front/main/back matter page numbering** that's correct across the whole book (roman → arabic → roman), not just the first chapter you tested.
- **A List of Tables/Figures that only appears when there's something to list** — no empty, oddly-titled page in a book with no tables.
- **Figures and tables that render exactly where you put them** — not wherever LaTeX's default float algorithm decides to defer them to, which can be several paragraphs or even chapters away.
- **Landscape pages done properly**, including a landscape-only chapter opener that doesn't waste a page on a bare portrait title before the actual wide content starts.
- **Numbered appendices** ("Appendix A," "Appendix B," ...) that switch back to regular chapter numbering or roman-numbered back matter cleanly, with zero manual bookkeeping.
- **Citation styling** (APA by default, swappable to anything via one YAML line) that actually produces a populated References section, not an empty one.
- **A documented, safer path for diagrams** (Mermaid, Graphviz) that sidesteps a real, confirmed Chrome-dependency trap in Quarto's native diagram-to-PDF pipeline.

## Who it's for

Anyone writing a book-length document in Quarto — a course book, training manual, certification guide, internal handbook, technical reference — who wants it to look genuinely professionally typeset without becoming a LaTeX expert first. You write plain Markdown; the toolkit's engine handles the LaTeX invisibly underneath. The few things Markdown genuinely can't express on its own (a captioned table, a landscape page) come with working, copy-pasteable examples — not left for you to reverse-engineer.

It's probably not what you want for a single one-off article (a plain Quarto setup is simpler for that) or for HTML/website output — this toolkit's value is almost entirely in PDF/print typesetting.

## Honest scope

This has been tested thoroughly against one real, full-length book — not yet against a wide range of community use. If something doesn't handle your case, that's genuinely useful to know; please open an issue.

**Repo:** https://github.com/belpub/colophon-quarto/ — see the README for setup, or `docs/BOOK-CREATION-GUIDE.md` for a from-scratch walkthrough if you're new to Quarto entirely.
