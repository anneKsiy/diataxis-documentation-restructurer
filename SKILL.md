---
name: diataxis-documentation-restructurer
description: >
  Analyze and restructure existing technical documentation using the Diátaxis framework. Use this skill whenever someone asks to reorganize, restructure, audit, or improve the structure of documentation — especially when dealing with large READMEs, sprawling docs/ folders, mixed-purpose documents, or content that has outgrown its original filename and intent. Also trigger when someone mentions Diátaxis by name, asks to classify documentation into tutorials/how-tos/reference/explanation, wants to split a monolithic doc into focused pieces, or asks for a documentation audit. Even if the user just says "clean up these docs" or "this README is a mess" or "help me organize our documentation" — this skill applies.
---
 
# Diátaxis Documentation Restructurer
 
This skill helps restructure technical documentation using the Diátaxis model as a pragmatic thinking tool — not a rigid rulebook.
 
## Philosophy
 
Diátaxis identifies four kinds of documentation (tutorials, how-to guides, reference, explanation), each serving a different user need. Its real value is in **simplifying how we think about writing docs** — shifting from trying to cram everything into one "perfect document" to recognizing that different readers have different needs at different moments.
 
**Be pragmatic, not dogmatic.** Diátaxis is a compass, not a constitution. Some content resists clean classification, and that's fine. Small repos might not need four top-level folders. A short README might serve its users perfectly well as-is. The goal is clarity for the reader, not taxonomic purity.
 
Before starting, read `references/diataxis-model.md` in this skill's directory for the full model details, diagnostic compass, and common anti-patterns. This reference is essential context — consult it before classifying anything. If you encounter boundary cases where the condensed reference isn't enough (e.g., a piece of content that could be either tutorial or how-to), consult `references/further-reading.md` for links to the canonical Diátaxis pages that go deep on each type.
 
---
 
## Workflow
 
### Step 1: Inventory the existing documentation
 
Read every documentation file in the repo or project. This means the README, anything in `docs/`, and any other markdown/text files that serve a documentation purpose (CONTRIBUTING.md, CHANGELOG, wiki pages if provided, etc.).
 
For each file, note:
- Its current filename and location
- Its approximate length
- A brief summary of what it covers
- Whether it mixes multiple Diátaxis types (this is the key diagnostic)
 
Present this inventory to the user as a concise table or summary before proceeding. Don't restructure anything yet — the user needs to see and validate your read of the landscape first.
 
### Step 2: Classify the content
 
For each section or chunk of content (not just per file — a single file often contains multiple types), classify it using the Diátaxis compass:
 
| Is it about... | Doing (action) | Knowing (cognition) |
|---|---|---|
| **Learning (acquisition)** | Tutorial | Explanation |
| **Working (application)** | How-to guide | Reference |
 
The most telling diagnostic: **who is this content for, and what are they trying to do right now?**
- Learning by following along → Tutorial
- Solving a specific problem at work → How-to guide
- Looking up a fact or specification → Reference
- Understanding why something is the way it is → Explanation
 
Flag any content that serves multiple purposes — these are your primary candidates for splitting.
 
Present the classification to the user. Highlight the mixed-purpose documents and explain what you see. For example: "Your README is currently part tutorial (the quickstart section), part reference (the configuration table), and part explanation (the architecture overview). Splitting these would make each one easier to find and maintain."
 
### Step 3: Propose the new structure
 
Based on the classification, propose a folder structure. The canonical Diátaxis structure looks like this:
 
```
docs/
├── tutorials/
│   ├── getting-started.md
│   └── your-first-deployment.md
├── how-to/
│   ├── configure-authentication.md
│   ├── migrate-from-v1.md
│   └── troubleshoot-connection-issues.md
├── reference/
│   ├── configuration.md
│   ├── api.md
│   └── cli.md
└── explanation/
    ├── architecture.md
    └── design-decisions.md
```
 
**But adapt to reality.** Consider the following before proposing:
 
- **Small projects:** If the repo only has a handful of docs, a flat `docs/` folder with clear filenames and a one-line type annotation in each file might be better than four mostly-empty subdirectories.
- **Existing conventions:** If the project already uses a structure that works, keep it. Don't move things just to satisfy the model — only move things that would genuinely be easier to find in a new location.
- **The README question:** A README should remain at the repo root. It typically should be a concise entry point that orients the reader and links into the docs structure. It is not itself a tutorial, reference, or explanation — it's a signpost. If the current README is doing too much, propose slimming it down to an orientation document with links.
- **Naming:** Use clear, human-readable filenames that tell you what the doc is about. `configure-ssl.md` over `ssl.md`. `getting-started.md` over `tutorial-001.md`. Titles should describe the content, not the Diátaxis category — the folder provides that context.
 
**When a category outgrows a single file, let it sprout a subfolder.** The same way a single file that grows too large should be split into multiple files, a folder category that grows too crowded should organize into subfolders. For example:
- Troubleshooting might start as one `troubleshoot-common-issues.md` in `how-to/`, but once you have 10+ distinct troubleshooting guides, create `how-to/troubleshooting/` and give each problem its own file.
- Configuration reference might start as one `configuration.md` in `reference/`, but if it grows to cover environment variables, DSL config, dynamic config, and feature flags, split it into `reference/configuration/environment-variables.md`, `reference/configuration/feature-flags.md`, etc.
- This is a general principle: any category folder whose contents become hard to scan should subdivide. The threshold is a judgment call, but ~10 files or a single file exceeding ~300 lines are reasonable signals.
 
**Conversely, don't create files that are too thin to justify their existence.** If a piece of content is only a small table or a handful of lines, fold it into a related file rather than giving it its own document. A tiny `error-handling.md` with one retry config table is better absorbed into `configuration.md` — it's still config. The test: would a reader looking for this content naturally look in the parent file? If yes, fold it in. Only promote it to its own file when it grows enough to stand on its own.
 
Present the proposed structure alongside a migration plan: which content moves where, what gets split, what gets merged, and what's missing (gaps the user should consider filling).
 
### Step 4: Execute the restructuring
 
Once the user approves (or adjusts) the plan:
 
1. **Create the new directory structure.**
2. **Split and relocate content.** When splitting a mixed-purpose file:
   - Each resulting file should be self-contained and coherent on its own
   - Add cross-references between related pieces (e.g., a how-to guide linking to the relevant reference, a tutorial linking to explanation for readers who want to go deeper)
   - Preserve the original voice and phrasing where possible — this is restructuring, not rewriting (unless the user asks for rewriting too)
3. **Rewrite the README** as a concise orientation document if it was previously overloaded. It should:
   - Briefly state what the project is and does (1-3 sentences)
   - Link to the tutorial(s) for newcomers
   - Link to how-to guides for common tasks
   - Link to reference for technical details
   - Optionally link to explanation for background
4. **Add a lightweight index or nav** if the docs folder has more than ~8 files. A `docs/README.md` or `docs/index.md` that lists docs grouped by type, with one-line descriptions.
 
### Step 5: Review and sanity-check
 
After restructuring, do a quick self-review:
 
- Can a newcomer find the tutorial within 10 seconds of opening the docs?
- Can a working developer find the configuration reference without reading through learning material?
- Are how-to guide titles specific enough to be scannable? ("How to configure X" not just "Configuration")
- Is any explanation buried where it interrupts action-oriented content?
- Do files link to related content in other quadrants where it would help the reader?
- Does anything feel forced into a category? If so, flag it to the user — pragmatism over dogma.
 
---
 
## Handling Edge Cases
 
**Content that genuinely belongs in two categories:** Some content sits right on a boundary. A configuration guide might be both reference (the table of options) and how-to (the steps to set them up). In these cases, either keep it as one document with clear internal sections, or split it — whichever serves the reader better. Ask the user what they prefer, and explain the trade-off.
 
**Favour consistency over minimising file count.** When a borderline section has a clear sibling that's already been split out, prefer splitting it out too — even if it creates a slightly thinner file. For example, if "Write a custom connector" is a standalone how-to guide, then "Write a custom transform" should probably be one too, rather than staying inline in the transforms reference. Uniform physical arrangement reduces cognitive load: readers learn the pattern once ("task-oriented guides live in `how-to/`") and can rely on it everywhere. When in doubt, ask: would a reader have to learn two different conventions to navigate this documentation? If yes, unify them.
 
**Very small repos:** If the entire documentation is a 50-line README, don't propose a four-folder hierarchy. Instead, suggest how to organize the README's sections using Diátaxis thinking, or recommend a lightweight expansion when the project grows.
 
**Documentation that's mostly missing:** If the repo has barely any docs, the Diátaxis model still helps — use it to identify *what's missing*. "You have reference content but no tutorial. A getting-started guide would help new contributors onboard." Present gaps as opportunities, not failures.
 
**User doesn't want all four categories:** That's completely fine. Not every project needs tutorials. Not every project needs deep explanation. Diátaxis describes four possible types; it doesn't mandate all four exist. Meet the user where they are.
 
**Monorepos or multi-package repos:** Each package might need its own documentation structure. Propose whether docs should live at the package level, the repo level, or both, based on how independently the packages are used.
 
**Architecture Decision Records (ADRs):** ADRs capture the *why* behind significant technical decisions — technology choices, design patterns, trade-offs accepted. While they're a form of explanation, they sit outside the Diátaxis hierarchy because their audience and lifecycle are different. Consumer-facing explanation docs help users understand the system; ADRs help *maintainers and future team members* understand why the system is the way it is. Treat ADRs as follows:
- If the repo already has an `adr/` or `decisions/` folder, leave it where it is. Don't absorb it into `docs/explanation/`.
- If the repo has design-decision content scattered through other docs (e.g., "Why we chose X over Y" paragraphs embedded in READMEs or architecture docs), **proactively propose extracting them into formal ADRs** in a dedicated `adr/` folder at the repo root. Prefer individual ADR files over a catch-all `design-decisions.md` — each decision deserves its own record, and the ADR format itself is a useful practice to introduce. Not all engineers are familiar with ADRs, and the exposure alone helps adoption across a team.
- The README signpost can optionally link to the ADR folder for contributors who want to understand the decision history, but ADRs don't need to be prominently featured alongside consumer-facing docs.
- When creating an `adr/` folder, suggest a lightweight format: one file per decision, numbered sequentially (e.g., `001-use-librdkafka.md`), with a consistent structure (context, decision, consequences). Don't over-engineer the template — the value is in capturing decisions, not in format compliance.
 
---
 
## Tone and Approach
 
When presenting your analysis and recommendations to the user:
 
- Be direct about what's wrong, but not judgmental. Documentation sprawl is normal — it happens to every project that's alive and being worked on. Mixed-purpose docs are the natural result of engineers writing what they know, when they know it.
- Explain *why* splitting or moving content helps the reader, not just that it satisfies a model.
- If the user pushes back on a classification or proposed move, listen. They know their users and codebase better than you do. The model serves them, not the other way around.
- Avoid jargon from the Diátaxis framework itself unless the user is already familiar with it. "This section reads more like background context than step-by-step instructions" is better than "This is explanation mixed into a how-to guide" for someone who hasn't read Procida's work.
 
