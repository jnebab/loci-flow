# Markdown output guideline

For the markdown branch of the visual-output choice (explanations,
comparisons, walkthroughs written as markdown instead of an HTML artifact).
Covers what [i-have-adhd](https://github.com/ayghri/i-have-adhd) doesn't —
its rules target task answers ("run X, then edit Y:42"); these target
explanatory content.

- **Lead with the conclusion.** State the answer or the key finding in the
  first line or two, before the supporting detail.
- **Mermaid for anything structural.** A flow, a hierarchy, a state
  machine, or a sequence of steps with branches — render it as a
  ` ```mermaid ` fence, not a prose description of the shape.
- **Tables only for enumerable facts.** Rows that are genuinely parallel
  items with the same fields (a comparison, a stage list). Don't force a
  table onto a single narrative point.
- **Scannable headers.** A reader should be able to get the gist from
  headers alone before reading a single paragraph.
- **No wall-of-text paragraphs.** If a paragraph runs past ~4 sentences,
  it's probably two ideas — split it or convert to a list.
- **Cap nesting at two levels.** A bullet under a bullet under a bullet
  is a sign the content wants headers or a table instead.

If [i-have-adhd](https://github.com/ayghri/i-have-adhd) is installed,
apply it first for structural directness, then these rules for anything
it doesn't cover (mermaid, tables, nesting). If it isn't installed, these
rules alone carry the markdown path.
