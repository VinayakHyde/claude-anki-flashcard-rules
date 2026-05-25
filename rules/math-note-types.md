# Math Flashcard Note Types

Reference for the three custom note types used in the math flashcard system.
Styled to match the APF (badlydrawnrob/anki) mobile-friendly design patterns.

**Deck:** `Temp` (for testing — move cards to `Learning` once validated)

---

## 1. MathConcept

**Purpose:** Atomic mathematical knowledge — definitions, theorems, formulas, properties. Each concept gets 2-3 cards from different angles (algebraic, geometric, verbal, "when does this fail?").

**Layer:** Layer 1 (Concept cards) — ~40% of all cards

**Review time:** 5-15 seconds

### Fields

| Field | Required | Description |
|-------|----------|-------------|
| `Question` | Yes | One atomic question about a single piece of knowledge |
| `Answer` | Yes | Precise, minimal answer (aim for under 20 words) |
| `AlternateAngle` | No | A different way to think about the same thing (geometric vs algebraic, intuitive vs formal) |
| `Topic` | Yes | Topic area (e.g., "conditional-probability", "expected-value") |
| `Source` | No | Book/chapter/section reference |

### Card Template: "Concept"

**Front:**
```html
<!-- MathConcept: front -->
<div id="front">
  <section class="gl-Card">
    <header>
      <h1>{{Topic}}</h1>
      <h2>Concept</h2>
    </header>
    <div class="mc-Question">
      {{Question}}
    </div>
  </section>
</div>
```

**Back:**
```html
<!-- MathConcept: back -->
<div id="reverse">
  <section class="gl-Card">
    <header>
      <h1>{{Topic}}</h1>
      <h2>Concept</h2>
    </header>
    <div class="mc-Question">
      {{Question}}
    </div>

    <div id="answer">
      <div class="mc-Answer">
        {{Answer}}
      </div>
      {{#AlternateAngle}}
      <div class="mc-Alternate">
        <b>Another way to see it:</b> {{AlternateAngle}}
      </div>
      {{/AlternateAngle}}
      {{#Source}}
      <footer>
        {{Source}}
        <small>{{Tags}}</small>
      </footer>
      {{/Source}}
    </div>
  </section>
</div>
```

### Example Cards (Linearity of Expectation)

**Card 1 — State the formula:**
```json
{
  "type": "MathConcept",
  "deck": "Temp",
  "fields": {
    "Question": "State the linearity of expectation property.",
    "Answer": "\\(E[aX + bY] = aE[X] + bE[Y]\\) for any random variables \\(X, Y\\) and constants \\(a, b\\).",
    "AlternateAngle": "Expectation is a linear operator — it distributes over addition and pulls out constants, just like integration.",
    "Topic": "Expected Value",
    "Source": "Ross Ch 4"
  },
  "tags": ["technique::expected-value", "card-type::concept", "source::ross::ch4", "difficulty::easy", "date::2026-02"]
}
```

**Card 2 — When does it NOT apply?:**
```json
{
  "type": "MathConcept",
  "deck": "Temp",
  "fields": {
    "Question": "Does linearity of expectation require independence?",
    "Answer": "No. \\(E[X + Y] = E[X] + E[Y]\\) holds for ANY random variables, dependent or independent.",
    "AlternateAngle": "This is what makes indicator variable arguments so powerful — you never need to check independence.",
    "Topic": "Expected Value",
    "Source": "Ross Ch 4"
  },
  "tags": ["technique::expected-value", "card-type::concept", "source::ross::ch4", "difficulty::easy", "date::2026-02"]
}
```

**Card 3 — Intuition:**
```json
{
  "type": "MathConcept",
  "deck": "Temp",
  "fields": {
    "Question": "Give a one-sentence intuition for why \\(E[X+Y] = E[X] + E[Y]\\) even without independence.",
    "Answer": "Expectation sums over all outcomes weighted by probability — rearranging the sum by X-values vs Y-values gives the same total.",
    "AlternateAngle": "Geometrically: the center of mass of a combined system is the weighted average of individual centers of mass.",
    "Topic": "Expected Value",
    "Source": "Ross Ch 4"
  },
  "tags": ["technique::expected-value", "card-type::concept", "source::ross::ch4", "difficulty::easy", "date::2026-02"]
}
```

---

## 2. MathProblem

**Purpose:** Full problem-solving cards. Used for both technique-recognition (Layer 2) and worked-problem (Layer 3) cards. The technique is hidden on the front, forcing genuine strategy selection.

**Layer:** Layer 2 (Technique-recognition, ~25%) and Layer 3 (Worked-problem, ~20%)

**Review time:** 1-5 minutes (solve on paper before flipping)

### Fields

| Field | Required | Description |
|-------|----------|-------------|
| `Problem` | Yes | Full problem statement with ALL conditions. Never mention the technique. |
| `Solution` | Yes | Complete worked solution with numbered steps. Use `\( \)` inline, `\[ \]` display, `<br>` for line breaks. |
| `Technique` | Yes | Primary technique name (shown only on back). From the taxonomy. |
| `Hints` | No | A gentle nudge WITHOUT naming the technique. E.g., "Consider decomposing into simpler events." |
| `KeyInsight` | Yes | The ONE core idea that makes this solvable. Single sentence. The most valuable field. |
| `Source` | No | Book/chapter/problem number |
| `Difficulty` | Yes | `easy` (textbook drill) / `medium` (interview-level) / `hard` (olympiad) |

### Card Template: "Solve"

**Front:**
```html
<!-- MathProblem: front -->
<div id="front">
  <section class="gl-Card">
    <header>
      <h1>Problem</h1>
      {{#Difficulty}}
        <h2>{{Difficulty}}</h2>
      {{/Difficulty}}
    </header>
    <div class="mp-Problem">
      {{Problem}}
    </div>
    {{#Hints}}
    <details class="mp-Hints">
      <summary>Hint</summary>
      <div>{{Hints}}</div>
    </details>
    {{/Hints}}
  </section>
</div>
```

**Back:**
```html
<!-- MathProblem: back -->
<div id="reverse">
  <section class="gl-Card">
    <header>
      <h1>Problem</h1>
      {{#Difficulty}}
        <h2>{{Difficulty}}</h2>
      {{/Difficulty}}
    </header>
    <div class="mp-Problem">
      {{Problem}}
    </div>

    <div id="answer">
      <div class="mp-Solution">
        {{Solution}}
      </div>
      <div class="mp-KeyInsight">
        <b>Key insight:</b> {{KeyInsight}}
      </div>
      <div class="mp-Technique">
        <b>Technique:</b> {{Technique}}
      </div>
      {{#Source}}
      <footer>
        {{Source}}
        <small>{{Tags}}</small>
      </footer>
      {{/Source}}
    </div>
  </section>
</div>
```

### Example Card

```json
{
  "type": "MathProblem",
  "deck": "Temp",
  "fields": {
    "Problem": "A bag contains 3 red and 5 blue balls. Two balls are drawn without replacement. Given that the second ball is red, what is the probability that the first ball was also red?",
    "Solution": "Let \\(R_1\\) = first ball red, \\(R_2\\) = second ball red.<br><br>By Bayes' theorem:<br>\\[P(R_1 | R_2) = \\frac{P(R_2 | R_1) \\cdot P(R_1)}{P(R_2)}\\]<br><br><b>Step 1:</b> \\(P(R_1) = \\frac{3}{8}\\)<br><br><b>Step 2:</b> \\(P(R_2 | R_1) = \\frac{2}{7}\\)<br><br><b>Step 3:</b> \\(P(R_2) = \\frac{2}{7} \\cdot \\frac{3}{8} + \\frac{3}{7} \\cdot \\frac{5}{8} = \\frac{3}{8}\\)<br><br><b>Step 4:</b> \\(P(R_1|R_2) = \\frac{2}{7}\\)",
    "Technique": "Bayes' theorem with law of total probability",
    "Hints": "What does knowing the second ball's color tell you about the composition when the first was drawn?",
    "KeyInsight": "By symmetry, P(R\u2082) = P(R\u2081) = 3/8 (position doesn't matter without replacement), so Bayes simplifies to P(R\u2082|R\u2081) = 2/7.",
    "Source": "Ross Ch 3, Example 3.5",
    "Difficulty": "easy"
  },
  "tags": ["technique::conditional-probability::bayes-theorem", "source::ross::ch3", "difficulty::easy", "card-type::technique-recognition", "date::2026-02"]
}
```

### KeyInsight Guidelines

The KeyInsight should capture what an expert would say if asked "what's the trick here?" Examples:

- "The key is recognizing that each pair contributes independently to the total, so use indicator variables for each pair."
- "Condition on the first step to get a recursive equation, then solve the resulting system."
- "The events aren't independent — draw the tree to see the sequential Bayesian update."

---

## 3. MathPool

**Purpose:** Eliminate pattern-matching entirely. Front shows a technique name and toggle-able problem statements. Pick one, solve on paper, then flip to see solutions. The problem you pick changes every review.

**Layer:** Layer 4 (Topic-pool cards) — ~15% of all cards

**Review time:** 3-5 minutes

**When to create:** When 3+ problems from the source use the same primary technique.

### Fields

| Field | Required | Description |
|-------|----------|-------------|
| `TechniqueName` | Yes | Specific technique name (e.g., "Linearity of Expectation with Indicator Variables") |
| `TechniqueDescription` | Yes | 1-2 sentences: when to use this technique and its key setup pattern |
| `ProblemList` | Yes | 3-10 problems, each wrapped in a `<details>` toggle (shown on front) |
| `Solutions` | Yes | Full solutions for each, numbered to match. Each inside its own `<details>` toggle (shown on back only). |
| `Topic` | Yes | Aggregated topic tag |
| `Source` | No | Aggregated from constituent problems |

### Card Template: "Pool Practice"

**Front:** Shows technique info + toggle-able problem statements (pick one, solve on paper)
```html
<!-- MathPool: front -->
<div id="front">
  <section class="gl-Card">
    <header>
      <h1>Random Practice</h1>
    </header>
    <div class="mpl-Front">
      <div class="mpl-TechniqueName">{{TechniqueName}}</div>
      <div class="mpl-TechniqueDesc">{{TechniqueDescription}}</div>
      <p><em>Pick a problem, solve on paper, then flip.</em></p>
    </div>
    <div class="mpl-ProblemList">
      {{ProblemList}}
    </div>
  </section>
</div>
```

**Back:** Same front content + solutions (each in its own toggle)
```html
<!-- MathPool: back -->
<div id="reverse">
  <section class="gl-Card">
    <header>
      <h1>Random Practice</h1>
    </header>
    <div class="mpl-Front">
      <div class="mpl-TechniqueName">{{TechniqueName}}</div>
      <div class="mpl-TechniqueDesc">{{TechniqueDescription}}</div>
    </div>
    <div class="mpl-ProblemList">
      {{ProblemList}}
    </div>
    <div id="answer">
      <div class="mpl-Solutions">
        {{Solutions}}
      </div>
      {{#Source}}
      <footer>
        {{Source}}
        <small>{{Tags}}</small>
      </footer>
      {{/Source}}
    </div>
  </section>
</div>
```

### Example Card

```json
{
  "type": "MathPool",
  "deck": "Temp",
  "fields": {
    "TechniqueName": "Linearity of Expectation with Indicator Variables",
    "TechniqueDescription": "Use when counting expected number of objects with a property. Define an indicator for each object, then sum expectations. Works even without independence.",
    "ProblemList": "<details><summary><b>Problem 1</b></summary>A class of 30 students sits randomly. What is the expected number of students sitting in their assigned seat?</details><details><summary><b>Problem 2</b></summary>Ten married couples sit randomly around a circular table with 20 seats. What is the expected number of couples sitting next to each other?</details><details><summary><b>Problem 3</b></summary>A deck of 52 cards is shuffled. What is the expected number of cards that are in the same position as in the original sorted deck?</details>",
    "Solutions": "<details><summary>Problem 1</summary>Let \\(X_i = 1\\) if student \\(i\\) sits in their seat. \\(E[X_i] = 1/30\\). By linearity, \\(E[X] = 30 \\cdot (1/30) = 1\\).</details><details><summary>Problem 2</summary>Let \\(X_i = 1\\) if couple \\(i\\) is adjacent. Each person has 2 neighbors out of 19 others, so \\(P(\\text{spouse is neighbor}) = 2/19\\). \\(E[X] = 10 \\cdot 2/19 = 20/19\\).</details><details><summary>Problem 3</summary>Let \\(X_i = 1\\) if card \\(i\\) is in position \\(i\\). \\(E[X_i] = 1/52\\). By linearity, \\(E[X] = 52/52 = 1\\). Same structure as Problem 1.</details>",
    "Topic": "expected-value",
    "Source": "Ross Ch 4, Various"
  },
  "tags": ["technique::expected-value", "card-type::pool", "source::ross::ch4", "date::2026-02"]
}
```

---

## CSS (shared across all three note types)

Follows the APF design system: CSS variables, system fonts, responsive sizing, night mode support, safe area handling for notched devices.

```css
/* ---------------------------------------------------
   Math Flashcard Note Types
   Styled after badlydrawnrob/anki APF design system
   Mobile-first, responsive, night mode support
   ------------------------------------------------- */

/* ---- CSS Variables ---- */
:root {
  /* Spacing (rem-based, from APF) */
  --spacing-px: 0.0625rem;
  --spacing-micro: 0.125rem;
  --spacing-quarter: 0.25rem;
  --spacing-half: 0.5rem;
  --spacing-one: 1rem;
  --spacing: 1.5rem;
  --spacing-two: 2rem;

  /* Fonts (system stack, from APF) */
  --system-ui: system-ui, -apple-system, sans-serif;
  --system-mono: ui-monospace;
  --fallback-mono: 'Roboto Mono', monospace, monospace;
  --font-family: var(--system-ui);
  --font-family-mono: var(--system-mono), var(--fallback-mono);

  /* Font sizes (rem-based for scaling) */
  --font-size: 1rem;          /* 16px base */
  --font-size-m: 0.875rem;    /* 14px */
  --font-size-mm: 0.75rem;    /* 12px */
  --font-size-p: 1.25rem;     /* 20px */
  --font-size-pp: 1.5rem;     /* 24px */

  /* Line height */
  --line-height: 1.5;

  /* Colors — light theme */
  --color-text: #1a1a1a;
  --color-text-light: #586e75;
  --color-text-muted: #93a1a1;
  --color-bg: #fefefe;
  --color-bg-header: #eceff1;
  --color-bg-header-title: #fff;
  --color-border: #ccc;
  --color-technique: #2aa198;
  --color-technique-bg: #f0fafa;
  --color-insight: #b58900;
  --color-insight-bg: #fdf6e3;
  --color-hint: #6c71c4;
  --color-alternate-bg: #eee8d5;
  --color-pool-name: #268bd2;
  --color-link: #268bd2;

  /* Border radius (from APF) */
  --border-radius: var(--spacing-micro);
}

/* ---- Base reset ---- */
* { box-sizing: border-box; }

.card {
  color: var(--color-text);
  font-family: var(--font-family);
  font-size: var(--font-size);
  line-height: var(--line-height);
  background: var(--color-bg);
  padding: 0;
  margin: 0;
  -webkit-text-size-adjust: 100%;
}

/* ---- Card container (APF .gl-Card pattern) ---- */
.gl-Card {
  background: transparent;
  border-radius: var(--border-radius);
  overflow: hidden;
  max-width: 700px;
  margin: 0 auto;
}

/* ---- Header (APF pattern) ---- */
.gl-Card header {
  position: relative;
  display: flex;
  flex-wrap: wrap;
  text-align: center;
  justify-content: space-evenly;
  align-items: baseline;
  background: var(--color-bg-header);
  border: var(--spacing-px) solid var(--color-bg-header);
  border-bottom: transparent;
  border-top-left-radius: var(--border-radius);
  border-top-right-radius: var(--border-radius);
}

.gl-Card header h1 {
  flex: 0 0 100%;
  margin: 0 0 var(--spacing-one);
  padding: var(--spacing-half);
  border-bottom: var(--spacing-px) solid var(--color-border);
  background: var(--color-bg-header-title);
  font-weight: 700;
  font-size: var(--font-size-p);
  line-height: var(--line-height);
}

.gl-Card header h1:only-child {
  margin-bottom: 0;
}

.gl-Card header h2 {
  font-weight: normal;
  font-size: var(--font-size);
  line-height: var(--line-height);
  margin: 0 0 var(--spacing-half);
  padding: var(--spacing-half);
}

/* ---- Content areas ---- */
.mc-Question,
.mp-Problem,
.mpl-Front {
  padding: var(--spacing) var(--spacing-one);
  font-size: var(--font-size-p);
  line-height: 1.6;
}

/* ---- Answer area ---- */
#answer {
  border-top: 2px solid var(--color-border);
}

/* MathConcept answer */
.mc-Answer {
  padding: var(--spacing) var(--spacing-one);
  font-size: var(--font-size);
  line-height: 1.6;
}

.mc-Alternate {
  margin: 0 var(--spacing-one) var(--spacing);
  padding: var(--spacing-half) var(--spacing-one);
  color: var(--color-text-light);
  background: var(--color-alternate-bg);
  border-radius: var(--border-radius);
  font-size: var(--font-size-m);
}

/* MathProblem solution */
.mp-Solution {
  padding: var(--spacing) var(--spacing-one);
  font-size: var(--font-size);
  line-height: 1.8;
}

.mp-KeyInsight {
  margin: 0 var(--spacing-one) var(--spacing-half);
  padding: var(--spacing-half) var(--spacing-one);
  color: var(--color-insight);
  background: var(--color-insight-bg);
  border-left: 3px solid var(--color-insight);
  border-radius: var(--border-radius);
  font-size: var(--font-size-m);
}

.mp-Technique {
  margin: 0 var(--spacing-one) var(--spacing);
  padding: var(--spacing-half) var(--spacing-one);
  color: var(--color-technique);
  background: var(--color-technique-bg);
  border-radius: var(--border-radius);
  font-style: italic;
  font-size: var(--font-size-m);
}

/* MathProblem hints */
.mp-Hints {
  margin: 0 var(--spacing-one) var(--spacing);
}

.mp-Hints summary {
  cursor: pointer;
  color: var(--color-link);
  font-weight: 700;
}

.mp-Hints div {
  padding: var(--spacing-half) 0;
  color: var(--color-hint);
  font-size: var(--font-size-m);
}

/* MathPool front */
.mpl-Front {
  text-align: center;
}

.mpl-TechniqueName {
  font-size: var(--font-size-pp);
  font-weight: bold;
  color: var(--color-pool-name);
  margin-bottom: var(--spacing-half);
}

.mpl-TechniqueDesc {
  color: var(--color-text-light);
  font-size: var(--font-size);
  margin-bottom: var(--spacing);
}

/* MathPool problem list & solutions */
.mpl-ProblemList {
  padding: var(--spacing) var(--spacing-one);
  font-size: var(--font-size);
  line-height: 1.6;
  text-align: left;
}

.mpl-Solutions {
  margin: 0 var(--spacing-one) var(--spacing);
}

.mpl-Solutions summary {
  cursor: pointer;
  color: var(--color-link);
  font-weight: 700;
}

.mpl-Solutions div {
  padding: var(--spacing-half) 0;
  font-size: var(--font-size-m);
  line-height: 1.6;
}

/* Nested details inside solutions (per-problem reveals) */
.mpl-Solutions details {
  margin: var(--spacing-half) 0;
  padding: var(--spacing-half);
  background: var(--color-bg-header);
  border-radius: var(--border-radius);
}

.mpl-Solutions details summary {
  font-size: var(--font-size-m);
}

/* ---- Footer (APF pattern) ---- */
.gl-Card footer {
  margin-bottom: var(--spacing);
  padding: 0 var(--spacing-one);
  color: var(--color-text-muted);
  font-size: var(--font-size-mm);
}

.gl-Card footer small {
  display: block;
  color: var(--color-text-muted);
  text-align: center;
}

/* ---- MathJax display math ---- */
.MathJax_Display,
mjx-container[display="true"] {
  overflow-x: auto;
  overflow-y: hidden;
  max-width: 100%;
  padding: var(--spacing-quarter) 0;
}

/* ---- Images ---- */
img {
  max-width: 100% !important;
  height: auto !important;
  vertical-align: middle;
}

/* ---- Mobile responsive (portrait) ---- */
@media (orientation: portrait) {
  .mc-Question,
  .mp-Problem,
  .mpl-Front {
    font-size: var(--font-size);
  }

  .mpl-TechniqueName {
    font-size: var(--font-size-p);
  }
}

/* ---- Night mode ---- */
.nightMode .card {
  --color-text: #e0e0e0;
  --color-text-light: #aaa;
  --color-text-muted: #777;
  --color-bg: #1e1e1e;
  --color-bg-header: #101010;
  --color-bg-header-title: #000;
  --color-border: #444;
  --color-technique-bg: #1a2e2e;
  --color-insight-bg: #2a2510;
  --color-alternate-bg: #2a2a2a;
}

.nightMode .gl-Card {
  border: 1px solid #444;
}

.nightMode .gl-Card header {
  background: var(--color-bg-header);
  border-color: transparent;
}

.nightMode .gl-Card header h1 {
  background: var(--color-bg-header-title);
}

.nightMode .mpl-Solutions details {
  background: #2a2a2a;
}
```

---

## Technique Taxonomy (for tagging)

| Technique Tag | Covers |
|---------------|--------|
| `conditional-probability` | Bayes' theorem, law of total probability, independence |
| `expected-value` | Linearity of expectation, indicator variables, iterated expectation |
| `first-step-analysis` | Recursive equations, conditioning on first step |
| `combinatorics` | P&C, stars-and-bars, inclusion-exclusion, pigeonhole, derangements |
| `markov-chains` | Absorbing chains, steady-state, gambler's ruin |
| `generating-functions` | MGF, PGF, convolution |
| `variance-covariance` | Var computation, conditional variance, covariance |
| `continuous-distributions` | CDF method, transformations, order statistics |
| `geometric-probability` | Area ratios, random points, integration |
| `classical-puzzles` | Birthday, coupon-collector, secretary, Monty Hall |
| `strategy-games` | Optimal stopping, game theory, adversarial |

---

## Tagging Convention

All cards get tags from these hierarchies:

```
technique::{primary-technique}
source::{book-abbreviation}::{chapter-or-section}
difficulty::{easy|medium|hard}
card-type::{concept|technique-recognition|worked-problem|pool}
date::{YYYY-MM}
```

**Book abbreviations:** `ross`, `green-book`, `mosteller`, `brainstellar`, `schaums`

---

## Formatting Rules

- Inline math: `\( \)` (not `$`)
- Display math: `\[ \]` (not `$$`)
- Line breaks in solutions: `<br>`
- In JSON strings: double-escape as `\\(`, `\\)`, `\\[`, `\\]`
- LaTeX braces near Anki cloze syntax: add spaces (`} }`)
- Deck: `Temp`
- Concept cards created before problem cards (foundations first)

## Mobile-Friendly Checklist

- [ ] All sizes use `rem` units (not `px`) for proper scaling
- [ ] System font stack (`system-ui`) — no custom font downloads needed
- [ ] `max-width: 100%` on MathJax display math — prevents horizontal overflow
- [ ] `overflow-x: auto` on display math — scrollable if still too wide
- [ ] `img { max-width: 100% !important }` — images never overflow
- [ ] Portrait media query reduces font sizes for narrow screens
- [ ] Night mode via `.nightMode` class (AnkiDroid + AnkiMobile)
- [ ] `-webkit-text-size-adjust: 100%` — prevents iOS auto-sizing
- [ ] CSS in card Styling section (not inline `<style>`) — required for AnkiDroid
- [ ] No fixed widths — everything flows to container
