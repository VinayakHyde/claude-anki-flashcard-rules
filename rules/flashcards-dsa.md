# DSA Problem Notes & Flashcard Guide

> **Goal:** Learn patterns and reasoning so you can solve variations, not memorize solutions you'll forget.

---

## The Core Principle

When you finish a problem, ask yourself: **"If the interviewer changed one thing about this problem, could I still solve it?"** If the answer is no, you memorized — you didn't learn. Every note and flashcard you make should serve this test.

---

## After Solving Every Problem: The 5-Minute Capture

Once you solve a problem (or study the solution), spend 5 minutes writing down these four things before moving on. Do this in a notebook, Notion, or a simple text file — wherever you'll actually do it consistently.

### 1. What Pattern Does This Problem Belong To?

Don't write the problem name. Write the _category of technique_ it uses.

- **Bad:** "Two Sum — use a hashmap"
- **Good:** "Complement search — when you need to find if a matching pair exists in a collection, a hashmap gives O(1) lookup for the complement of each element"

Common patterns to watch for: two pointers, sliding window, binary search on answer, BFS/DFS, topological sort, union-find, monotonic stack, prefix sum, backtracking with pruning, greedy with sorting, 1D DP, 2D DP, interval merging, fast & slow pointers.

### 2. What Was the Signal?

What words or constraints in the problem statement told you which pattern to use? This is the most important thing to capture because in an interview, recognizing the signal _is_ the hard part.

Examples:

- "Contiguous subarray" + "maximum/minimum sum or length" → sliding window
- "Sorted array" + "find pair" → two pointers from both ends
- "Find minimum/maximum that satisfies a condition" → binary search on answer
- "Number of ways to reach..." → dynamic programming
- "All possible combinations/permutations" → backtracking
- "Process nodes level by level" → BFS with queue
- "Dependencies / ordering" → topological sort

### 3. What Was the Key Insight?

Every non-trivial problem has one moment where the approach clicks — one insight that makes the O(n²) solution become O(n), or makes the brute force become elegant. Write that down in one or two sentences.

- **Bad:** "Use two pointers and move them inward"
- **Good:** "The shorter line limits the area. Moving the taller pointer can never help because the height is already capped by the shorter one. So you always move the shorter pointer — this is why greedy works here and you don't need to check all pairs."

### 4. What Would Break My Approach?

Think about edge cases and variations that would force you to adapt. This is what interviewers actually test.

- What if the input is empty? Has duplicates? Has negative numbers?
- What if the array isn't sorted?
- What if the answer needs to be "top K" instead of "the single best"?
- What if the constraint changes from "exactly K" to "at most K"?

---

## How to Make Anki Flashcards That Actually Work

### Cards You Should Make (Pattern Cards)

**Card Type 1 — Pattern Recognition (most important)**

- **Front:** A short problem description _without_ naming the LeetCode problem. Describe it like an interviewer would.
- **Back:** The pattern name + why it applies + the signal words that gave it away.

Example:

- Front: "Given a string, find the length of the longest substring with no repeating characters."
- Back: "Sliding window with a hashset. Signal: 'substring' (contiguous) + 'longest' (optimization) + constraint on content (no repeats). Expand right pointer, shrink left when constraint breaks."

**Card Type 2 — Key Insight**

- **Front:** "In [pattern] problems where [specific constraint], why does [approach] work?"
- **Back:** The reasoning in 2–3 sentences. Not code. Reasoning.

Example:

- Front: "In two-pointer problems on sorted arrays, why can you skip all duplicates after finding a valid pair?"
- Back: "Because the array is sorted, all duplicates are adjacent. If (a, b) is valid, then (a, b') where b' = b would be a duplicate answer. Moving past all copies of b avoids this without missing any unique pair."

**Card Type 3 — Complexity Tradeoff (make these sparingly)**

- **Front:** "What's the tradeoff between [approach A] and [approach B] for [problem type]?"
- **Back:** Time and space comparison + when to prefer one over the other.

Example:

- Front: "Recursive DFS with memoization vs. bottom-up DP table for grid path problems?"
- Back: "Same time complexity O(m×n). Recursion + memo uses O(m×n) stack space in worst case and risks stack overflow on large inputs. Bottom-up uses O(m×n) table but can often be optimized to O(n) with rolling array. Prefer bottom-up for interviews — easier to reason about and no stack risk."

### Cards You Should NEVER Make

- ❌ "What is the solution to Two Sum?" → You'll memorize code, not learn anything.
- ❌ A card with the full code on the back → You'll pattern-match syntax, not logic.
- ❌ A card for every single problem → You'll drown in reviews. Only card problems where you learned something new or struggled.

---

## Be Compact (MANDATORY)

Strip every word that isn't load-bearing. **Phrases beat full sentences.** DSA cards need reasoning, but reasoning ≠ verbose.

**Cut:**
- Articles ("the", "a") where readable without them
- Hedging/filler ("essentially", "basically", "really")
- Framing prose ("This is...", "It refers to...")
- Restating the question on the back

**Targets:** Front ≤ ~120 chars (problems are inherently a bit longer). Back: one phrase for pattern names, 1–3 short sentences for reasoning. Not paragraphs.

❌ BAD (verbose pattern card):
```
Front: "Given a string, find the length of the longest substring with no repeating characters."
Back: "This is a sliding window problem with a hashset. The signal that tells you this is the word 'substring' (which means contiguous) combined with 'longest' (which signals optimization), plus the constraint on the content (no repeats). The approach is to expand the right pointer, and shrink the left pointer when the constraint breaks."
```

✅ GOOD:
```
Front: "Longest substring with no repeating characters — pattern?"
Back: "Sliding window + hashset. Signal: 'substring' (contiguous) + 'longest' + content constraint. Expand right, shrink left on break."
```

**Test:** Could the back be a phrase or a single short sentence? If yes, make it one.

---

## Source Fidelity — Don't Hallucinate Variations

Cards from a source (video, PDF, article, lecture) must come from THAT source. If the source teaches Contains Duplicate, don't make cards about Contains Duplicate II or Missing Number — even though they're natural extensions you already know about.

**Why:** Anki is for consolidating knowledge you've studied, not extending it to material you haven't. A card on content you never actually studied is a card you'll grade "hard" forever, because the underlying understanding isn't there — you invented it, you never learned it.

**Where this rule gets violated:** two prompts in this document tempt you to generate variation cards when you shouldn't —

- The "What Would Break My Approach?" step in the 5-Minute Capture
- "Could I solve a harder variation?" in the Anti-Mugging-Up Checklist

Both of those are self-study prompts for *you* — things to think about while learning, to test whether you really understand. They are **not** templates for Anki card prompts. Explore variations in your problem-notes document; do not card them unless the source explicitly taught them.

**Test:** before carding any variation or related problem, ask — did the source material explicitly cover this one? If no, it's out of scope for this batch.

---

## The Weekly Review Ritual

Every Sunday (or whatever day works), spend 30 minutes doing this:

1. **Review your problem log from the week.** Look at the problems you struggled with — the ones where you needed hints or watched the solution.

2. **Find the common thread.** Are you consistently weak at DP? Do you keep missing edge cases with trees? Do you struggle to convert recursive solutions to iterative? Write down your top 1–2 weak areas.

3. **Pick 2 old problems from your weak area and redo them from scratch.** No peeking. Open a blank editor. If you can solve them in under 20 minutes with clean code, you've internalized the pattern. If you can't, your cards for that pattern need to be rewritten — they're testing the wrong thing.

4. **Update your Anki cards.** If a card was too easy three times in a row, consider deleting it — you've learned it. If a card keeps tripping you up, rewrite it to be more specific about what you actually don't understand.

---

## Template: Problem Notes Entry

Use this for each problem you solve. Keep it in a single running document or one file per topic.

```
## [Problem Name] — [Pattern]
**Date:** YYYY-MM-DD
**Difficulty:** Easy / Medium / Hard
**Solved independently:** Yes / Needed hints / Watched solution

**Signal:** What in the problem told me to use this pattern?
→

**Key Insight:** The one thing that makes this problem click.
→

**Approach in plain English (no code):**
→ Step 1:
→ Step 2:
→ Step 3:

**Time:** O(?)  **Space:** O(?)

**Edge cases I missed or would miss:**
→

**Variations this pattern applies to:**
→

**Anki cards made:** Yes / No (only if I learned something new)
```

---

## Quick Reference: Signals → Patterns

| Signal in Problem                              | Likely Pattern                       |
| ---------------------------------------------- | ------------------------------------ |
| "Sorted array" + "find pair/target"            | Two pointers                         |
| "Contiguous subarray/substring" + optimization | Sliding window                       |
| "Minimum/maximum that satisfies condition"     | Binary search on answer              |
| "Number of ways" / "Can you reach..."          | Dynamic programming                  |
| "All combinations/permutations/subsets"        | Backtracking                         |
| "Shortest path, unweighted"                    | BFS                                  |
| "Connected components / cycle detection"       | DFS or Union-Find                    |
| "Process in dependency order"                  | Topological sort                     |
| "Next greater/smaller element"                 | Monotonic stack                      |
| "Merge overlapping ranges"                     | Sort by start + greedy               |
| "Top K / Kth largest or smallest"              | Heap / Quickselect                   |
| "Prefix operations / range sum queries"        | Prefix sum array                     |
| "Detect cycle in linked list"                  | Fast & slow pointers                 |
| "Design a data structure"                      | Combine hashmap + linked list / heap |

---

## Code in Card Content (MANDATORY)

DSA cards often reference variables, function names, or short pseudocode snippets. Anki renders fields as HTML, not markdown — markdown backticks render as literal `` ` `` characters and break the visual cue.

| Use case | Required | NEVER |
|----------|----------|-------|
| Inline code, variable, identifier, complexity expression | `<code>O(n log n)</code>`, `<code>left++</code>` | `` `O(n log n)` ``, `` `left++` `` |
| Multi-line pseudocode / code block | `<pre><code>...</code></pre>` | triple-backtick fence |

This applies to every field on every note type. Even a single token (`<code>i</code>`, `<code>nums[i]</code>`) in prose must use `<code>` tags rather than backticks. Per "Cards You Should NEVER Make" above, full code blocks are rare on DSA cards — but when you do include code, format it correctly.

---

## Cloze + `<ol>` for enumerated items (PREFERRED)

DSA cards are usually Basic (pattern recognition, key insight, tradeoff). But when you do enumerate named items on a card — e.g., approaches to a problem with their complexities, or signal→pattern mappings — use Cloze type with `<ol>` and put both the name and its description **inside the same cloze bracket**.

✅ GOOD:
```
Text: "Duplicate-detection in an unsorted array:<ol><li>{{c1::Brute force pair check — <code>O(n²)</code> time, <code>O(1)</code> space}}</li><li>{{c2::Sort then scan adjacent — <code>O(n log n)</code> time, <code>O(1)</code> extra (or O(n) for stable)}}</li><li>{{c3::Hash set — <code>O(n)</code> time, <code>O(n)</code> space}}</li></ol>"
```

See `~/.claude/rules/flashcard-general.md` → "Cloze + `<ol>` for named enumerated items" for full rationale.

---

## Media File Naming (MANDATORY)

If you add images to DSA cards (diagrams of trees, graphs, DP tables, etc.):

1. **Never use generic names** like `image.png`, `diagram.png`, `tree.png`. They collide silently in Anki's flat media namespace.
2. **Always use descriptive, unique names**: `leetcode-42-trapping-water-heights.png`, `dp-knapsack-table-example.png`, `bfs-level-order-diagram.png`.
3. **Before copying to media**, `ls` the target filename first — never clobber existing files.

See `~/.claude/rules/flashcard-general.md` → "Media File Naming" for full rationale.

---

## Tagging

```
source::leetcode | source::url | source::video
source::book::{book-slug}::{chapter}        ← for books (e.g., CLRS, EPI, Cracking the Coding Interview)
topic::dsa::{pattern}                       ← e.g., topic::dsa::sliding-window, topic::dsa::two-pointers
difficulty::{easy|medium|hard}
```

**Book source format:** `source::book::<title-kebab>-<author-last-name>::<chapter-id>`

- Examples: `source::book::cracking-coding-interview-mcdowell::ch4`, `source::book::elements-programming-interviews-aziz::ch7`, `source::book::clrs-cormen::ch15`
- `<chapter-id>` is `ch<number>` for numbered chapters, or a slug for named sections.

When the source is a book, prefer this over `source::pdf`/`source::url` even if you read it as a PDF — the book identity matters more than the file format.

---

## The Anti-Mugging-Up Checklist

Before marking any problem as "done," run through this:

- [ ] Can I explain the approach to someone in plain English without mentioning code?
- [ ] Can I identify what _type_ of problem this is from the problem statement alone?
- [ ] Do I know _why_ this approach works, not just _that_ it works?
- [ ] Could I solve a harder variation that adds one more constraint?
- [ ] If I came back in two weeks, would I recognize the pattern or would I need to re-derive everything?

If any answer is no, you haven't learned the problem yet. Revisit it.
