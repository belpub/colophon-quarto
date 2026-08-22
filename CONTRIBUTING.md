# Contributing

Thanks for considering it — this stays deliberately short.

## Found a bug?

Open an issue. The single most useful thing you can include is a **minimal reproduction**: the smallest `.qmd` snippet that shows the problem, plus what you expected versus what you got. "Chapter titles sometimes look wrong" is hard to act on; "a chapter title longer than one line shows uneven spacing on the wrapped line, here's the exact heading text" is something that can actually get fixed.

If you're not sure whether something's a real bug or you're just missing a step, ask anyway — an issue that turns out to be a documentation gap is still useful, since it means the docs need fixing.

## Want to fix something yourself?

Pull requests welcome, including small ones — a typo fix in the docs is a legitimate contribution, not just a full feature.

For anything that touches `preamble.tex` (the LaTeX engine, as opposed to `brand.tex`, which is just colors/fonts/text), please actually render before opening the PR, and briefly say what you tested. This toolkit's whole value is that its fixes were verified against real rendered output, not just plausible-looking LaTeX — a PR description like "tested with a chapter title long enough to wrap to two lines, confirmed even spacing" is exactly the kind of thing that keeps that true.

## Want to propose a new feature?

Open an issue first rather than going straight to a PR, especially for anything that changes `preamble.tex` — it's worth agreeing on the approach before investing the work, partly because this engine has a history of "obvious" fixes turning out to have non-obvious side effects once actually tested (see the comments throughout `preamble.tex` for some real examples of that).

## Code of conduct

Be the kind of person people want to collaborate with again. That's the whole policy.
