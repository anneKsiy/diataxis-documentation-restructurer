# diataxis-documentation-restructurer

A Claude skill that analyzes and restructures existing technical documentation using the [Diátaxis framework](https://diataxis.fr) as a pragmatic thinking tool.

Give it a repo with sprawling docs, a mega-README, or a `docs/` folder full of mixed-purpose files, and it will inventory, classify, and propose a clearer structure — then execute the restructuring if you approve.

## Install

Download the latest `.skill` file from [Releases](../../releases) and add it to your Claude skills:

- **Claude Code:** Place the `.skill` file in your project or user skills directory
- **Claude.ai:** Upload the `.skill` file in your project settings

## What it does

Point the skill at a repo's documentation and it will:

1. **Inventory** every doc file, noting what each covers and whether it mixes purposes
2. **Classify** each section (not just each file) into the four Diátaxis types — tutorials, how-to guides, reference, and explanation
3. **Propose** a new folder structure with a migration plan, gap analysis, and ADR recommendations
4. **Execute** the restructuring once you approve — splitting mixed-purpose files, adding cross-references, and slimming the README into a signpost
5. **Sanity-check** the result against a set of reader-experience questions

The skill triggers on prompts like "restructure these docs", "this README is a mess", "help me organize our documentation", "audit the docs in this repo", or any mention of Diátaxis by name.

## Example

Before — a 350-line mega-README containing a tutorial, config reference tables, API docs, deployment procedures, and architectural explanation all in one file:

```
repo/
└── README.md (350 lines, everything)
```

After — the skill proposes and creates:

```
repo/
├── README.md (20 lines, signpost with links)
├── adr/
│   └── 001-stateless-architecture.md
└── docs/
    ├── tutorials/
    │   └── getting-started.md
    ├── how-to/
    │   ├── deploy-to-staging.md
    │   ├── deploy-to-production.md
    │   ├── scale-the-bridge.md
    │   └── troubleshoot-common-issues.md
    ├── reference/
    │   ├── configuration.md
    │   └── api.md
    └── explanation/
        └── architecture-and-design.md
```

## Principles

The skill follows a few key principles beyond the base Diátaxis model:

**Pragmatic, not dogmatic.** Diátaxis is a compass, not a constitution. Small repos won't get forced into four empty folders. A 150-line README for a focused CLI tool might be perfectly fine as-is — the skill will say so.

**Classify sections, not just files.** A single file often contains multiple types. The skill identifies where the boundaries are within a document, which is where the real value of splitting comes from.

**Consistency reduces cognitive load.** When a borderline section has a sibling that's already been split out (e.g., "Write a custom connector" is a standalone how-to), the parallel case should follow the same pattern ("Write a custom transform" also gets its own how-to) — even if it's slightly thinner.

**Recursive growth.** Categories that outgrow a single file sprout subfolders. Troubleshooting that starts as one file can become `how-to/troubleshooting/` when it hits 10+ items. Conversely, a file that's too thin to justify its existence gets folded into a parent.

**ADRs live outside the Diátaxis hierarchy.** Design-decision content scattered through docs gets proposed as formal Architecture Decision Records in a dedicated `adr/` folder — they serve maintainers, not consumers, and deserve their own home.

**The README is a signpost.** If it's doing too much, the skill slims it down to an orientation document that tells you what the project is and links into the docs structure.

## What's in the skill

```
diataxis-documentation-restructurer/
├── SKILL.md                          # Workflow, principles, edge cases
└── references/
    ├── diataxis-model.md             # Condensed quadrant model, compass, anti-patterns
    └── further-reading.md            # Curated links to canonical Diátaxis sources
```

## Background

This skill was born from frustration with documentation content sprawl — large READMEs that are difficult to follow, random docs placed in a `docs/` folder with no real structure, files that outgrow their initial filename and intent.

[Diátaxis](https://diataxis.fr) (by Daniele Procida) provides a framework for thinking about this problem. It identifies four kinds of documentation — tutorials, how-to guides, reference, and explanation — each serving a different user need at a different moment. The real value isn't in rigid taxonomy; it's in simplifying how we think about writing docs. As one commenter on the [Hacker News thread](https://news.ycombinator.com) put it:

> Diátaxis is a great way to structure documentation, but I think its real value is in simplifying how we think about writing docs. It shifts the focus from trying to cram everything into one 'perfect document' to recognizing that different users have different needs.

This skill operationalizes that thinking into something Claude can apply to your repos.

## Contributing

PRs welcome. If you use the skill on a repo and find it makes a bad call — wrong classification, over-restructuring a small project, missing a split it should have caught — please open an issue with the before/after so we can iterate on the skill's instructions.

## Attribution

The [Diátaxis framework](https://diataxis.fr) was created by [Daniele Procida](https://github.com/evildmp). This skill references and builds upon his work, which is published under [CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). The skill itself does not bundle content from the Diátaxis repo — it contains an original condensed reference and links to the canonical sources.
