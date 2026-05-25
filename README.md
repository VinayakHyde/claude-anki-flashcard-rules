# claude-anki-flashcard-rules

Opinionated rules for generating high-quality Anki flashcards with [Claude Code](https://claude.com/claude-code) and the [Anki MCP server](https://github.com/scorzeth/anki-mcp-server).

These are the same rules I use day-to-day — dropped into `~/.claude/rules/` and referenced from a global `CLAUDE.md` so Claude follows them every time I ask it to make cards from a PDF, article, video, or problem set.

## How these rules came together

I built these by firing off deep-research runs for each broad category I was making flashcards on — general spaced-repetition principles, math problem-solving retention, coding/programming recall, DSA pattern recognition — and then distilling the findings into concrete, enforceable rules Claude can actually follow. The recommendations here aren't pulled out of thin air; they're grounded in the research that came back from each of those deep dives (cognitive science on interleaving and the testing effect, Matuschak's prompt-design properties, Nielsen's Ankification, Rohrer on blocked-vs-interleaved practice, etc.), then shaped by what I learned from actually using the cards in review.

## What's in here

| File | Purpose |
|------|---------|
| [`rules/flashcard-general.md`](rules/flashcard-general.md) | Baseline rules for any flashcard: the 10 essential rules, anti-patterns, the mandatory propose-then-batch workflow, compactness, cloze + `<ol>` for enumerated items, source attribution, deck/tag conventions. |
| [`rules/flashcard-math.md`](rules/flashcard-math.md) | Math-specific rules built around a four-layer card architecture (concept / technique-recognition / worked-problem / topic-pool) using three custom note types. Designed for problem-solving retention, not factual recall. |
| [`rules/math-note-types.md`](rules/math-note-types.md) | Full specifications (fields, templates, CSS) for the three custom math note types: `MathConcept`, `MathProblem`, `MathPool`. Styled after the [badlydrawnrob/anki](https://github.com/badlydrawnrob/anki) APF design system — mobile-first, night-mode-aware. |
| [`rules/flashcard-coding.md`](rules/flashcard-coding.md) | Coding rules, prioritizing pattern recognition and concepts over framework APIs. Uses the badlydrawnrob APF note types when available (syntax highlighting, mobile-friendly), falls back to Basic/Cloze otherwise. |
| [`rules/flashcards-dsa.md`](rules/flashcards-dsa.md) | DSA problem rules built around a 5-minute capture (pattern → signal → key insight → what would break it). Pattern-recognition cards beat problem-specific cards. |

## How to use these with Claude Code

1. Clone or download this repo:
   ```bash
   git clone https://github.com/VinayakHyde/claude-anki-flashcard-rules.git
   ```
2. Copy the rules into your global Claude Code config:
   ```bash
   mkdir -p ~/.claude/rules
   cp claude-anki-flashcard-rules/rules/*.md ~/.claude/rules/
   ```
3. Reference the matching rule when asking Claude to make cards:
   ```
   Follow @~/.claude/rules/flashcard-math.md and make flashcards from this PDF
   ```
   ```
   Follow @~/.claude/rules/flashcards-dsa.md for this LeetCode problem
   ```
4. (Optional) Add a pointer in your global `~/.claude/CLAUDE.md` so Claude knows the rules exist without you having to `@`-mention every time. Example:
   ```markdown
   ## Flashcard Creation

   Rules live at `~/.claude/rules/`:
   - `flashcard-general.md` — General content
   - `flashcard-math.md` — Math
   - `flashcard-coding.md` — Code
   - `flashcards-dsa.md` — DSA
   ```

## Prerequisites

- **Anki** running locally with the [AnkiConnect](https://ankiweb.net/shared/info/2055492159) add-on.
- An MCP server bridging Claude Code to AnkiConnect, e.g. [anki-mcp-server](https://github.com/scorzeth/anki-mcp-server).
- The custom math note types (`MathConcept`, `MathProblem`, `MathPool`) created in Anki — full specs in [`rules/math-note-types.md`](rules/math-note-types.md).
- For code cards, the [badlydrawnrob/anki](https://github.com/badlydrawnrob/anki) APF note types are preferred but optional — the coding rules fall back to Basic/Cloze if unavailable.

## Design philosophy

A few principles thread through all the rules:

- **One atomic fact per card.** No "list all X". No yes/no.
- **Propose first, batch-create after approval.** Never write directly to Anki.
- **Be compact.** Phrases beat sentences. A back of two sentences is probably two cards.
- **Cloze + `<ol>` for enumerated items.** Each named item gets its own cloze with the description *inside* the bracket, not after it.
- **One deck (`Learning`), tags for organization.** Interleaved review beats blocked subdecks — this is grounded in Rohrer's research.
- **Source attribution on the front for books and long-form sources.** Italicized prefix like `<i>Cagan (Inspired) -</i>` re-anchors the card during review.
- **HTML, not markdown.** Anki renders fields as HTML — use `<code>` and `<pre><code>`, never backticks; `<ol>`/`<ul>`, never `<br>`-separated lists.

## License

MIT — use, fork, adapt freely.
