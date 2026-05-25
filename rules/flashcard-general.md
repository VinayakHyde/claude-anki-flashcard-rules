# Flashcard Creation Rules - General

## Card Creation Workflow (MANDATORY)

**IMPORTANT:** Never create cards directly in Anki. Always follow this workflow:

### Step 1: Present Proposed Cards as a Full-JSON Dump

Before creating any cards, present the COMPLETE proposed set at once — every card's full JSON in one response, NOT one at a time. Precede each card with a short title line: `Card N — <short title>`. After the dump, give a one-line totals summary (count, breakdown by note type, any images referenced) and ask: **"Approve all, or edit specific ones?"**

Card-by-card approval is too slow for batches of 3+. A single-pass review lets the user batch-approve or call out specific edits.

The JSON format for an individual card:

```json
{
  "type": "Basic",
  "deck": "Learning",
  "fields": {
    "Front": "What is the capital of France?",
    "Back": "Paris"
  },
  "tags": ["topic::geography", "type::definition", "source::url"]
}
```

For Cloze cards:
```json
{
  "type": "Cloze",
  "deck": "Learning",
  "fields": {
    "Text": "The capital of France is {{c1::Paris}}"
  },
  "tags": ["topic::geography", "type::definition", "source::url"]
}
```

### Step 2: Wait for User Approval

- Wait for explicit approval. Accepted forms: **batch** ("approve all", "yes", "add them") or **curation** ("keep all except 3 and 7", "edit card 5 to say X", "drop 9").
- If the user requests edits, update only the affected cards' JSON and re-present THOSE cards — do not re-dump the whole set.
- Once all remaining cards are approved, proceed to Step 3.

### Step 3: Batch-Create in Anki

Once approved, use `mcp__anki__batch_create_notes` to create all approved cards in a single call (split into batches of 10–20 if you have more than 20).

**Set `allowDuplicate: true`** whenever the note type's first field will repeat across the batch. Anki's default duplicate check compares ONLY the first field of each note type — so if you create six APF: Simple cards all with `★ Title="Python"`, only the first will succeed and the rest fail with `cannot create note because it is a duplicate`. The flag bypasses this; nothing is actually duplicate from your perspective. Required when:

- Multiple APF: Simple (3.0.0) cards are in one batch (always — `★ Title` repeats)
- Multiple cards share a common primary field (Front/Question/Title)
- You explicitly want to override Anki's dedup check

---

## The 10 Essential Rules

| # | Rule | Description |
|---|------|-------------|
| 1 | Understand First | Never create cards from content not fully understood |
| 2 | Minimum Information | One atomic fact per card |
| 3 | Precise Questions | Question must permit exactly ONE answer |
| 4 | No Raw Lists | Never ask "List all X" - use overlapping cloze |
| 5 | No Yes/No | Convert to cloze with context |
| 6 | Use Images | Include diagrams, charts from source |
| 7 | Context-Free | Add topic prefix, standalone comprehension |
| 8 | Cloze for Facts | Use cloze for definitions, dates, formulas |
| 9 | Basic for Concepts | Use Basic Q&A for explanations |
| 10 | Source | Always include source reference (date tracked natively by Anki) |

---

## Anti-Patterns (Block These)

```
❌ "Describe X" → Too broad
❌ "What is an example of X?" → Infinite answers
❌ "Is X true?" → Yes/No doesn't aid memory
❌ "What are the 5 types of..." → Enumeration
❌ "List all..." → Raw list
```

---

## Lists on the back = Cloze (MANDATORY)

If a card's back is list-like — multiple comparable items, multi-approach comparison, pipeline stages, taxonomy, enumerated tradeoffs — it MUST be a cloze card with overlapping deletions, **not** a Basic card with `<br>` separators.

**Why:** overlapping cloze turns each item into its own active-recall event. A Basic card with a `<br>`-separated list tests only "can you recite the whole thing at once", which gets learned as one chunk — you'll either remember all items or none, and you won't notice which item is the weak link.

❌ BAD — Basic with `<br>`:
```
Front: "Three approaches to duplicate-detection"
Back:  "Brute force — O(n²)<br>Sort+scan — O(n log n)<br>Hash set — O(n)"
```

✅ GOOD — Cloze (with descriptions inside each cloze per the rule below):
```
Text: "Three approaches to duplicate-detection: {{c1::brute force — O(n²)}}, {{c2::sort+scan — O(n log n)}}, {{c3::hash set — O(n)}}"
```

Rule of thumb: if you find yourself writing `<br>` three or more times in a Back, or enumerating items with numbers/bullets, stop — make it a cloze.

---

## Cloze + `<ol>` for named enumerated items (PREFERRED)

When a cloze list enumerates **named items with brief definitions** (e.g., the four Agile Manifesto values, the steps of a loop, the practices of a methodology), wrap it in `<ol>` and put both the name and its description **inside the same cloze bracket**. This combines active recall (one cloze per item) with visual list structure.

❌ BAD — bare comma-separated cloze prose:
```
Text: "Build–Measure–Learn loop: {{c1::Build — minimum viable product}}, {{c2::Measure — how customers respond}}, {{c3::Learn — was the assumption validated?}}"
```

✅ GOOD — cloze wrapped in `<ol>`:
```
Text: "<i>Build–Measure–Learn</i> loop:<ol><li>{{c1::<b>Build</b> — minimum viable product (MVP)}}</li><li>{{c2::<b>Measure</b> — how real customers respond}}</li><li>{{c3::<b>Learn</b> — was the assumption validated?}}</li></ol>"
```

For parallel named values (no canonical order), it's also fine to split a single item's name and description into two clozes that share the same number, so both reveal together:
```
Text: "<ol><li>{{c1::Individuals and interactions}} over {{c1::processes and tools}}</li>...</ol>"
```

Per "Cloze Card Construction" below, the description must live INSIDE the cloze — not after it — so it isn't a free hint.

Use `<ol>` when items have meaningful order or canonical numbering; `<ul>` when they're parallel and unordered.

---

## When you do use a list, use `<ol>` or `<ul>` (NOT `<br>` separators)

The rule above pushes most lists into cloze form. But some cards genuinely need a list inside Basic content — narrative walkthroughs (multi-step worked examples where the steps are a cohesive story, not enumerable independent items), or descriptions/sub-points inside a cloze card. When a list IS the right format, use proper HTML list markup: `<ol>` for ordered/numbered lists, `<ul>` for unordered/bulleted lists. Wrap each item in `<li>...</li>`.

❌ BAD — `<br>`-separated faux list:
```
<b>Step 1:</b> Examine search terms<br><br><b>Step 2:</b> Examine search results<br><br><b>Step 3:</b> ...
```

✅ GOOD — proper `<ol>`:
```
<ol><li><b>Search terms:</b> ...</li><li><b>Search results:</b> ...</li><li>...</li></ol>
```

Use `<ol>` when items have a meaningful order (steps, ranking, sequence). Use `<ul>` when they're parallel/unordered (options, properties, comparisons).

The "Step N:" prefix becomes implicit from `<ol>` numbering — drop redundant prefixes when converting from `<br>`-form to list-form.

`<br>` is still fine for separating distinct sections of prose (e.g., "Workflow:" / "Failure observed:" / "Task:" sections on a card front) — the rule applies specifically to enumerated/bulleted items.

---

## Self-Sufficiency (Rule 7 in practice)

Every card must be comprehensible on its own, not as part of the batch you created it in. Anki shows one card at a time with gaps of days or weeks between related cards; if a card relies on "the other cards about this problem are nearby", that context will have decayed by the next review.

**Check every card by asking:** if I saw this card alone, with no other cards visible, would I understand what's being asked?

❌ BAD — assumes the subject from surrounding cards:
```
Front: "Why does a hash set turn this problem from O(n²) into O(n)?"
```
(*What* problem? If this card appears in isolation between a probability card and a Python card, it's meaningless.)

✅ GOOD — self-contained:
```
Front: "Duplicate-detection in an array: why does a hash set turn the problem from O(n²) into O(n)?"
```

Cost: a few extra characters to name the subject on each card. Benefit: the card still means something two months from now, when you've made a hundred other cards on unrelated topics.

---

## Source Attribution on the Front (MANDATORY for book/article batches)

When a batch of cards comes from a single identifiable source — a book, a paper, a talk, a long-form article — put an **italicized source prefix** inline at the start of the Front (or `Text` for Cloze cards), followed by a space, then the question. This makes the card's origin obvious mid-review and helps you mentally re-enter the source's context.

**Format:** `<i>{Compact Source} -</i> {question}`

- For books: `<i>{Author Last} ({Book Title}) -</i>` — e.g., `<i>Cagan (Inspired) -</i>`, `<i>Newport (Deep Work) -</i>`, `<i>Kleppmann (DDIA) -</i>`
- For papers/articles: `<i>{Author Last} ({Short Title}) -</i>` — e.g., `<i>Dean (MapReduce) -</i>`
- For talks/videos: `<i>{Speaker} ({Talk Title}) -</i>`

**Style notes:**

- Italic, not bold — lighter visual weight, doesn't compete with the question.
- Plain ASCII hyphen `-`, not an em-dash (`—`).
- No `<br>` — a single space after `</i>` is enough; italics + hyphen do the visual separation.
- Compact form (`Cagan (Inspired)`, not `INSPIRED by Marty Cagan`) — keep the re-anchor short. No chapter title or number; the `source::book::...::ch12` tag already encodes the chapter.

**Examples:**

```
Front: "<i>Cagan (Inspired) -</i> Product management: which working relationship is most critical to PM success?"
```
```
Text: "<i>Newport (Deep Work) -</i> Two opposing forces in deliberate practice:<ol><li>{{c1::focused attention on a specific skill}}</li><li>{{c2::immediate feedback that corrects approach}}</li></ol>"
```

**Why both prefix AND tag?**

- **Tag** (`source::book::inspired-cagan::ch12`) makes cards filterable and searchable.
- **Front prefix** makes the source visible *during review*, without flipping to the back or hovering the tag bar.

**When NOT to use a source prefix on the Front:**

- Standalone facts where the source is incidental (e.g., a one-off definition pulled from a quick web search) — the `source::url` tag is enough.
- Cards from your own notes/experience — no canonical source to cite.
- DSA pattern cards — the pattern name itself disambiguates; source goes in the tag only.

The rule applies whenever the source has a *title worth remembering* and you'd benefit from re-anchoring to it during review.

---

## Be Compact (MANDATORY)

Strip every word that isn't load-bearing. **Phrases beat full sentences.** A back of two sentences is probably two cards.

**Compact ≠ cryptic.** The front must make clear *what is being asked*. A single bare term like `"OSI"` is ambiguous (full form? definition? layer count?) — disambiguate with a short cue: `"OSI — full form?"` or `"OSI — define."`. Compactness is about cutting filler, not cutting clarity.

**Cut:**
- Articles ("the", "a") where readable without them
- Hedging/filler ("essentially", "basically", "really", "in this case")
- Framing prose ("This is...", "It refers to...", "The idea is...")
- Restating the question on the back
- Anything the card-taker can infer from the question

**Targets:** Front ≤ ~80 chars typically. Back as short as the answer allows — often a single phrase, name, or number.

❌ BAD (verbose, two facts on one back):
```
Front: "What communication abstraction did client-server architecture give birth to?"
Back: "It gave birth to Remote Procedure Call (RPC), which is the idea of invoking a procedure that runs on a remote machine."
```

✅ GOOD (split, terse):
```
Front: "Client-server: what abstraction came from it?"
Back: "RPC."
```
```
Front: "What is RPC?"
Back: "Invoke a procedure on a remote machine."
```

**Test:** Could the back be a phrase instead of a sentence? If yes, make it one.

---

## Cloze Card Construction

When writing cloze cards with definitions or descriptions, put them **inside** the cloze bracket, not outside. Descriptions outside the cloze reveal the answer when the blank is shown — they act as free hints.

❌ BAD: `The {{c1::Shell}} is the outermost OS layer that handles user interaction.`
   (The description after the blank gives away the answer.)

✅ GOOD: `{{c1::The Shell — the outermost OS layer that handles user interaction}}.`
   (Description hidden together with the term.)

For paired/multiple clozes in the same sentence:

❌ BAD: `{{c1::Shell}} (outermost layer) and {{c2::Kernel}} (core component)`

✅ GOOD: `{{c1::Shell — outermost layer}} and {{c2::Kernel — core component}}`

The only text that should live outside cloze brackets is framing/context that applies equally regardless of which blank is being tested.

---

## Handling Lists/Sequences

When source has a list (e.g., "stages of ML pipeline"):

```
1. MNEMONIC CARD
   Front: "ML pipeline stages mnemonic?"
   Back: "CPTED: Collection → Preprocessing → Training → Evaluation → Deployment"

2. OVERLAPPING CLOZE
   Text: "ML Pipeline: {{c1::Collection}} → {{c2::Preprocessing}} → {{c3::Training}} → {{c4::Evaluation}} → {{c5::Deployment}}"

3. INDIVIDUAL CONCEPT CARDS (one per item)
   Front: "What happens in the Preprocessing stage of ML?"
   Back: "Data cleaning, handling missing values, feature engineering"

4. CONTEXT CARDS (why this order)
   Front: "Why does Preprocessing come before Training?"
   Back: "Models learn from data. Dirty data = poor model (garbage in, garbage out)"
```

---

## Format Requirements

| Element | Target / Limit |
|---------|----------------|
| Front | aim ≤ 80 chars; hard cap 200 |
| Back | aim for a phrase; hard cap 1000 |
| MathJax inline | `\(...\)` |
| MathJax block | `\[...\]` |
| Images | `<img src="filename.png">` |
| Inline code | `<code>...</code>` |
| Multi-line code | `<pre><code>...</code></pre>` |

See "Be Compact" above — limits are ceilings, not goals.

---

## Code in Card Content (MANDATORY)

**Never use markdown backticks** (`` ` `` or ```` ``` ````) inside any card field. Anki renders fields as HTML, not markdown — backticks display as literal characters, breaking the visual cue you'd expect them to give.

| Use case | Required form | NEVER |
|----------|---------------|-------|
| Inline code, identifiers, filenames, flags | `<code>my_var</code>` | `` `my_var` `` |
| Multi-line code blocks | `<pre><code>line1\nline2</code></pre>` | ` ```...``` ` fences |

This applies to every field on every note type — Front, Back, Text, Question, Answer, Solution, KeyInsight, Hints, Sample, Key point, etc. Even a single word like a function name or a flag must use `<code>...</code>`, not backticks.

---

## Media File Naming (MANDATORY)

**Anki's media folder is a flat namespace.** Every card references images by filename only — there are no subfolders. A collision silently breaks other cards that use the same name, because only one file can exist per name at any time.

### Rules

1. **Never use generic filenames.** `image.png`, `screenshot.png`, `photo.png`, `diagram.png`, `img.png`, `untitled.png` are banned. They WILL collide with existing cards.

2. **Always use descriptive, unique names** that encode the card's topic and specifics:
   - ❌ `image.png`
   - ❌ `diagram.png`
   - ✅ `os-intro-architecture.png`
   - ✅ `schaums-ch8-cartesian-quadrants.png`
   - ✅ `calc-ch14-first-derivative-test-min.png`

3. **Before copying any file into the Anki media folder**, check whether that filename already exists. If it does, rename yours — never clobber. Use:
   ```bash
   ls "~/Library/Application Support/Anki2/<profile>/collection.media/<filename>"
   ```
   If the `ls` succeeds, pick a different filename.

4. **If the user supplies a generic-named source image** (e.g., they paste `image.png`), rename it to a descriptive name BEFORE copying into media and update the card's `<img src="...">` to match.

5. **When reviewing a set of cards to create**, scan all `<img src=...>` references for generic names. Reject and rename before creation.

### Why this matters

A generic filename in Anki's media folder is a ticking bomb — the next time anyone creates a card using the same name, either the new card is broken or all the old ones are. This happened before; the rule exists because of that incident.

---

## Deck Organization

- **Single deck:** Use `Learning`
- **No subdecks:** Don't create `Math::Algebra`, just use tags
- **Why:** Interleaved review (mixing topics) improves long-term retention

---

## Tagging

```
source::pdf | source::url | source::image
source::book::{book-slug}::{chapter}        ← for books
topic::{subject}::{subtopic}
type::definition | type::formula | type::concept
```

**Book source format:** `source::book::<book-slug>::<chapter-id>`

- `<book-slug>` is `<title-kebab>-<author-last-name>` (e.g., `inspired-cagan`, `thinking-fast-slow-kahneman`, `deep-work-newport`)
- `<chapter-id>` is `ch<number>` for numbered chapters (e.g., `ch12`), or a slug for named sections (e.g., `intro`, `appendix-a`)
- Examples: `source::book::inspired-cagan::ch12`, `source::book::deep-work-newport::ch3`

When the source is a book, prefer this over `source::pdf`/`source::url` even if you read it as a PDF — the book identity matters more than the file format.

---

## Validation

1. **Format:** Length, structure, no anti-patterns
2. **Content:** Answer matches source, single unambiguous answer
3. **Action:** BLOCK AND FIX before proceeding

---

## Duplicates

1. Exact duplicate → Skip
2. Similar + can improve → Update existing
3. Similar + can't improve → New card with different angle
4. No similar → Create new
