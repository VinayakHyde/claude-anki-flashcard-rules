# Flashcard Creation Rules - Coding

## Card Creation Workflow (MANDATORY)

**IMPORTANT:** Never create cards directly in Anki. Always follow this workflow:

### Step 0: Check Available Note Types

Before creating coding cards, call `get_note_type_info` to check if the **badlydrawnrob/anki** note types are available (from reference deck `APF (3.0.0) — Demo`):

| Note Type | Use Case |
|-----------|----------|
| **APF: Simple (3.0.0)** | Basic code card (pattern recognition, concepts) |
| **APF: Missing! (3.0.0)** | Fill-in-the-blank for code (like cloze) |
| **APF: Draw! (3.0.0)** | Diagram/visual cards |

**Prefer these note types for code cards** - they have syntax highlighting and mobile-optimized design. Note types are global in Anki (not deck-specific), so they work in any deck including `Learning`.

### Step 1: Present Proposed Cards as a Full-JSON Dump

Before creating any cards, present the COMPLETE proposed set at once — every card's full JSON in one response, NOT one at a time. Precede each card with a short title line: `Card N — <short title>`. After the dump, give a one-line totals summary (count, breakdown by note type, any images referenced) and ask: **"Approve all, or edit specific ones?"**

Card-by-card approval is too slow for batches of 3+. A single-pass review lets the user batch-approve or call out specific edits.

The JSON format for an individual card:

**Using badlydrawnrob/anki note types (PREFERRED for code):**
```json
{
  "type": "APF: Simple (3.0.0)",
  "deck": "Learning",
  "fields": {
    "★ Title": "Python",
    "☆ Subtitle": "iterate over dict key-value pairs",
    "★ Sample (code block)": "<pre><code>my_dict = {'a': 1, 'b': 2}</code></pre>",
    "★ Key point (code block)": "<pre><code>for key, value in dict.items():</code></pre>",
    "★ Key point notes": "Returns key-value tuples"
  },
  "tags": ["topic::coding::python", "type::pattern", "source::url"]
}
```

**APF: Simple (3.0.0) fields:**
- `★ Title` - Language/topic (required)
- `☆ Subtitle` - The question/task (optional)
- `☆ Syntax (inline code)` - Inline syntax hint (optional)
- `★ Sample (code block)` - Example code context (required)
- `★ Key point (code block)` - The answer code (required)
- `★ Key point notes` - Brief explanation (required)
- `✎ Other notes` - Additional info (optional)

**Fallback to Basic (if badlydrawnrob types unavailable):**
```json
{
  "type": "Basic",
  "deck": "Learning",
  "fields": {
    "Front": "Python: iterate over dict key-value pairs",
    "Back": "<pre><code>for key, value in dict.items():</code></pre>"
  },
  "tags": ["topic::coding::python", "type::pattern", "source::url"]
}
```

For Cloze/Missing Word cards:
```json
{
  "type": "Cloze",
  "deck": "Learning",
  "fields": {
    "Text": "List comprehension: [{{c1::expr}} for {{c2::item}} in {{c3::iterable}}]"
  },
  "tags": ["topic::coding::python", "type::syntax", "source::url"]
}
```

### Step 2: Wait for User Approval

- Wait for explicit approval. Accepted forms: **batch** ("approve all", "yes", "add them") or **curation** ("keep all except 3 and 7", "edit card 5 to say X", "drop 9").
- If the user requests edits, update only the affected cards' JSON and re-present THOSE cards — do not re-dump the whole set.
- Once all remaining cards are approved, proceed to Step 3.

### Step 3: Batch-Create in Anki

Once approved, use `mcp__anki__batch_create_notes` to create all approved cards in a single call (split into batches of 10–20 if you have more than 20).

**Always set `allowDuplicate: true` when the batch contains 2+ APF: Simple (3.0.0) cards.** Anki's default duplicate check compares ONLY the first field — `★ Title` — and APF cards almost always share the same Title (e.g. "Python", "TypeScript"), so without this flag everything after the first card fails with `cannot create note because it is a duplicate`. The cards aren't actually duplicate from your perspective; the check is overly strict for this note type.

Same flag applies anywhere the primary field repeats across the batch (e.g., multiple Basic cards with identical Front, MathConcept cards with the same Question).

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

## Handling Code/Programming

**Prioritize:**
- Pattern recognition ("Python: iterate dict" → `for k,v in d.items()`)
- Concepts ("What is a closure?")
- Gotchas from real bugs
- Mental models

**Skip:**
- Exact syntax (searchable)
- Framework-specific APIs
- Method signatures

---

## Be Compact (MANDATORY)

Strip every word that isn't load-bearing. **Phrases beat full sentences.** A back of two sentences is probably two cards.

**Cut:**
- Articles ("the", "a") where readable without them
- Hedging/filler ("essentially", "basically", "really")
- Framing prose ("This is...", "It refers to...", "The idea is...")
- Restating the question on the back

**Targets:** Front ≤ ~80 chars typically. Back as short as the answer allows — name + one-line explanation max. Code snippets are an exception (they're the answer).

❌ BAD:
```
Front: "What is the closure abstraction in JavaScript and how does it work?"
Back: "A closure is a function in JavaScript that retains access to variables defined in its enclosing lexical scope, even after that outer function has returned."
```

✅ GOOD (split, terse):
```
Front: "What is a closure?"
Back: "Function that retains access to its enclosing scope."
```

**Test:** Could the back be a phrase instead of a sentence? If yes, make it one.

---

## What to Card vs Skip

| ✅ PRIORITIZE | ❌ AVOID |
|---------------|----------|
| Data structures (map, reduce, filter) | Framework-specific APIs |
| Protocols (HTTP, SQL, JSON) | Proprietary service details |
| CS concepts (regex, threading) | Trendy/unproven features |
| Language fundamentals | Exact syntax (just look it up) |
| Common patterns & idioms | Large code blocks |
| Error patterns you keep hitting | Niche library internals |

---

## Card Types for Programming

```
1. PATTERN RECOGNITION (most valuable)
   Front: "Python: iterate over dict key-value pairs"
   Back: "for key, value in dict.items():"

2. CONCEPT/DEFINITION
   Front: "What is a closure?"
   Back: "A function retaining access to variables from its enclosing scope"

3. SYNTAX CLOZE (frequently-used patterns)
   Text: "List comprehension: [{{c1::expr}} for {{c2::item}} in {{c3::iterable}}]"

4. "WHEN TO USE" CARDS
   Front: "When use Set instead of List?"
   Back: "Need: unique elements, O(1) lookup, no order requirement"

5. ERROR/GOTCHA CARDS (from real bugs)
   Front: "Python: Why does lst.append() return None?"
   Back: "append() modifies in-place, returns None. Use lst + [item] for new list."

6. MENTAL MODEL CARDS
   Front: "Pandas: relationship between DataFrame and Series?"
   Back: "DataFrame = dict of Series (columns), Series = 1D labeled array"
```

---

## Code Card Templates

**Reference deck in Anki:** `APF (3.0.0) — Demo`

This deck contains code-specific note types from [badlydrawnrob/anki](https://github.com/badlydrawnrob/anki):
- **APF: Simple (3.0.0)** - basic code card
- **APF: Missing! (3.0.0)** - fill in the blank for code
- **APF: Draw! (3.0.0)** - diagram/visual cards

Features: syntax highlighting, mobile-optimized design. Note types are global in Anki, so they work in any deck.

**IMPORTANT:** Before creating code cards:
1. Call `list_note_types` to check if APF types are available
2. Call `get_note_type_info` for the chosen type to get exact field names
3. Use APF note types instead of Basic/Cloze when available
4. Only fall back to Basic/Cloze if APF types are not installed

---

## Cloze + `<ol>` for enumerated items (PREFERRED)

When a code card enumerates named items (HTTP methods, async primitives, dict iteration patterns, common gotchas), use Cloze type with `<ol>` and put both the name and its description **inside the same cloze bracket**.

✅ GOOD:
```
Text: "Python iteration patterns over a dict:<ol><li>{{c1::<code>for k in d</code> — iterate keys (default)}}</li><li>{{c2::<code>for v in d.values()</code> — iterate values}}</li><li>{{c3::<code>for k, v in d.items()</code> — iterate key-value pairs}}</li></ol>"
```

See `~/.claude/rules/flashcard-general.md` → "Cloze + `<ol>` for named enumerated items" for full rationale and the alternative split-cloze form for parallel/unordered items.

---

## Code Formatting

- Keep code under ~32 chars wide (mobile-friendly)
- Use syntax highlighting
- 2-space indentation
- Minimal working example, not full context

### NEVER use markdown backticks in card content

Anki renders fields as HTML, not markdown. Backticks display as literal `` ` `` characters and the code styling never kicks in.

| Use case | Required | NEVER |
|----------|----------|-------|
| Inline code, identifier, flag, filename | `<code>os.path.join</code>` | `` `os.path.join` `` |
| Multi-line block | `<pre><code>...</code></pre>` | triple-backtick fence |

This applies to every field on every note type (Front, Back, Text, ★ Sample, ★ Key point, ✎ Other notes, etc.). Even a single function name in prose must be `<code>fn_name</code>`, not `` `fn_name` ``.

### Avoiding Blank Lines in Multi-line Code

Using `\n` or newlines between `<pre><code>` blocks creates unwanted blank lines in Anki. To avoid this, break out of `</code>` and back into `<code>` on the same line:

**❌ BAD (creates blank lines):**
```html
<pre><code>from langchain_community\
  .document_loaders import PyPDFLoader

loader = PyPDFLoader("file.pdf")</code></pre>
```

**✅ GOOD (no blank lines):**
```html
<pre><code>from langchain_community/</code>.document_loaders import PyPDFLoader<code>
loader = PyPDFLoader("file.pdf")</code></pre>
```

The `/</code>text<code>` pattern lets you continue on the same visual line without creating gaps.

---

## Format Requirements

| Element | Target / Limit |
|---------|----------------|
| Front | aim ≤ 80 chars; hard cap 200 |
| Back | aim for a phrase (code snippets exempt); hard cap 1000 |
| MathJax inline | `\(...\)` |
| MathJax block | `\[...\]` |
| Images | `<img src="filename.png">` — see Media File Naming below |
| Inline code | `<code>...</code>` (NEVER backticks) |
| Multi-line code | `<pre><code>...</code></pre>` (NEVER triple-backtick fences) |

See "Be Compact" above — limits are ceilings, not goals.

---

## Media File Naming (MANDATORY)

Anki's media folder is a flat namespace. Generic filenames collide silently and break other cards.

1. **Never use generic names** like `image.png`, `screenshot.png`, `diagram.png`, `code.png`. Banned.
2. **Always use descriptive, unique names** encoding language + topic:
   - ✅ `python-dict-membership-benchmark.png`
   - ✅ `js-promise-chain-diagram.png`
   - ✅ `react-render-lifecycle-mount.png`
3. **Before copying to media**, check the filename doesn't already exist:
   ```bash
   ls "~/Library/Application Support/Anki2/<profile>/collection.media/<filename>"
   ```
   If the file exists, rename yours.
4. **If a card uses multiple images**, number them with the topic prefix:
   `python-list-comp-step1.png`, `python-list-comp-step2.png`, etc.

See `~/.claude/rules/flashcard-general.md` → "Media File Naming" for full rationale.

---

## Deck Organization

- **Single deck:** Use `Learning`
- **No subdecks:** Don't create `Coding::Python`, just use tags
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

- `<book-slug>` is `<title-kebab>-<author-last-name>` (e.g., `pragmatic-programmer-hunt`, `clean-code-martin`, `designing-data-intensive-apps-kleppmann`)
- `<chapter-id>` is `ch<number>` for numbered chapters (e.g., `ch12`), or a slug for named sections
- Example: `source::book::clean-code-martin::ch3`

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
