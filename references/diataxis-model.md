# The Diátaxis Model — Quick Reference

Diátaxis identifies four kinds of documentation, each serving a different user need.
The two key axes are:

- **Action vs. Cognition** (doing things vs. understanding things)
- **Acquisition vs. Application** (learning/studying vs. working/applying)

This produces four quadrants:

```
                  ACQUISITION                    APPLICATION
              (study / learning)            (work / applying)
           ┌─────────────────────────┬─────────────────────────┐
  ACTION   │       TUTORIALS         │      HOW-TO GUIDES      │
  (doing)  │   learning-oriented     │     task-oriented        │
           │   "a lesson"            │     "a recipe"           │
           ├─────────────────────────┼─────────────────────────┤
 COGNITION │      EXPLANATION        │       REFERENCE          │
 (knowing) │  understanding-oriented │   information-oriented   │
           │  "a discussion"         │   "a lookup table"       │
           └─────────────────────────┴─────────────────────────┘
```

---

## Tutorials (learning-oriented)

**Purpose:** Guide a beginner through a learning experience so they gain confidence and familiarity.

**Characteristics:**
- The user is *at study* — they're learning, not trying to get real work done
- Always practical: the user *does* things under the instructor's guidance
- Follows a carefully managed path with a guaranteed successful outcome
- Analogous to a driving lesson: the point is skill-building, not getting from A to B
- Avoids explanation — just enough context to keep moving ("We use HTTPS because it's more secure")
- Uses a contrived, controlled environment (sample data, sandbox, known-good setup)
- Concrete and particular, not general

**Signals in existing docs:** "Getting started", "Your first X", step-by-step introductions, onboarding walkthroughs, content that assumes zero prior knowledge.

**Common problems:** Overloaded with explanation. Conflated with how-to guides. Broken by product changes. Too ambitious in scope.

---

## How-To Guides (task-oriented)

**Purpose:** Help a competent user accomplish a specific, real-world task.

**Characteristics:**
- The user is *at work* — they have a concrete problem to solve right now
- Addresses a specific goal: "How to configure X", "How to migrate from Y to Z"
- Assumes the reader already knows the basics and is familiar with the tools
- Like a recipe: a professional chef still follows one to make sure they do it correctly
- Practical usability over completeness — doesn't need to be end-to-end
- Should not include extended explanation (link to it instead)
- Title should clearly state what problem it solves

**Signals in existing docs:** "How to...", troubleshooting sections, configuration guides, deployment instructions, migration guides, operational procedures.

**Common problems:** Conflated with tutorials. Defined around tool operations instead of user needs. Overloaded with tangential reference material.

---

## Reference (information-oriented)

**Purpose:** Provide accurate, complete, factual technical descriptions for a user who is working and needs to look something up.

**Characteristics:**
- Pure information: facts, specifications, parameters, return types, configuration options
- Like a dictionary, encyclopedia, or tidal chart
- The user consults it; they don't read it cover-to-cover
- Neutral tone — no opinions, no persuasion, minimal interpretation
- Structure should mirror the codebase or system it describes
- Must be accurate and kept in sync with the product
- Austere and to-the-point

**Signals in existing docs:** API docs, CLI reference, configuration option lists, environment variable tables, schema descriptions, architecture specs that are purely descriptive.

**Common problems:** Drifts into explanation (the "why" starts creeping in). Mixed with how-to steps. Incomplete or out of date because no one maintains it.

---

## Explanation (understanding-oriented)

**Purpose:** Provide context, background, and deeper understanding. Answer "why?" questions.

**Characteristics:**
- The user is *at study* — but studying concepts, not learning hands-on skills
- Can approach the subject from different angles
- Can include opinions, perspectives, historical context, alternatives considered
- Connects things together and illuminates the bigger picture
- Not tied to specific steps or actions
- Not urgent in the way how-to guides are, but equally important for mastery

**Signals in existing docs:** "Background", "Architecture overview", "Design decisions", "Why we chose X", discussions of trade-offs, conceptual overviews.

**Note on ADRs:** Architecture Decision Records are explanation-flavoured content, but they serve a different audience (maintainers and team members, not consumers) and have a different lifecycle (one-per-decision, append-only). They typically live in their own `adr/` folder at the repo root rather than inside `docs/explanation/`. See the main SKILL.md for how to handle them.

**Common problems:** Shoved into tutorials where it disrupts learning. Missing entirely because engineers assume "the code is the documentation." Scattered as asides throughout how-to guides.

---

## The Compass — Diagnostic Questions

When looking at a piece of content, ask:

| Question | If yes → |
|---|---|
| Does it guide the user through doing something to learn? | **Tutorial** |
| Does it help the user accomplish a specific task at work? | **How-to guide** |
| Does it describe the machinery factually? | **Reference** |
| Does it help the user understand why or how things fit together? | **Explanation** |

If the answer is "more than one of these" — the content is mixed-purpose and should probably be split.

---

## Common Anti-Patterns

1. **The Mega-README:** A single file that starts as a quickstart tutorial, morphs into reference, digresses into explanation, and has how-to sections buried in the middle. Split it.

2. **The `docs/` Junk Drawer:** Files accumulate with no organizing principle. `setup.md` is actually a tutorial + reference hybrid. `architecture.md` is half explanation, half outdated how-to. Classify and restructure.

3. **Explanation in Tutorials:** A tutorial that stops to explain the theory behind every step. The learner's focused attention dissolves. Keep tutorials lean; link to explanation.

4. **How-To Masquerading as Tutorial:** "Getting Started" that actually assumes you already know the system and is really a "How to set up your dev environment." Call it what it is.

5. **Reference That Teaches:** API docs that include multi-paragraph explanations of design philosophy. Move the philosophy to explanation; keep reference austere.

6. **Content That Outgrew Its Filename:** `deployment.md` started as a short how-to but now contains tutorials, reference tables, troubleshooting, and architectural discussion. It needs to be decomposed.
