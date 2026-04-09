---
name: write-rfc
description: Draft or revise a design RFC that explains architecture from first principles — why the system exists, how layers interact, what trade-offs were made, and where it's headed. Use when writing or improving technical design documents, RFCs, or architecture overviews.
---

# Write Design RFC

Produce a **design-driven** technical document that helps readers (teammates, reviewers, your future self) quickly understand *why* a system is designed the way it is.

## Core Principles

1. **Read the code first, then write the doc.** Ground the architecture in code facts, not imagination.
2. **"Why" matters more than "what."** Every design decision must explain its motivation and trade-offs.
3. **Write like you're talking to a colleague.** Avoid academic tone and template bloat — use concrete examples instead of abstract descriptions.
4. **If one sentence is enough, don't use a paragraph.**

## Writing Process

### Step 1: Understand the System (Required)

Before writing anything, figure out:

- Directory structure and layering (which directories / modules / files)
- Where the entry points are, how data flows through the system
- What the core abstractions are (key types, interfaces, registries)
- What the existing tests are actually testing

Method: read code, run commands, look at tests — don't just read existing docs.

### Step 2: Define the Document Skeleton

Organize in the following order; merge or skip sections that don't apply:

```
1. One-line summary (what the system is, what problem it solves)
2. Why it exists (motivation — describe 2-3 pain points)
3. Overall architecture (layers / modules, ASCII diagram or mermaid)
4. What each layer does (responsibilities, key abstractions, data flow)
5. Key design decisions (list 3-5, each explaining "what was chosen / why / what it costs")
6. Future evolution (brief list)
```

### Step 3: Write Each Section

**One-line summary**
- Use the pattern "X is a Y that does Z."
- Example: `agent-cli is a "dual-entry, single-datasource" analysis tool — both humans and AI Agents use the same underlying capabilities to analyze build data.`

**Why it exists**
- Skip "background" — go straight to pain points.
- Each pain point: **bold keyword** + one-sentence explanation.

**Overall architecture**
- Prefer ASCII art or simple mermaid; don't draw overly complex diagrams.
- Use a table to clarify what each layer "cares about / doesn't care about."

**What each layer does**
- Anchor descriptions with concrete file names / function names / type names.
- Include key code structures (pseudocode or simplified), don't paste large blocks of source code.
- Use "not X, but Y" phrasing to clarify commonly misunderstood designs.

**Key design decisions**
- For each decision, use the three-part format: "Choice → Benefit → Cost."
- Don't just list benefits — always state the cost.

**Future evolution**
- Use a numbered list, one sentence per item.
- Only include actionable directions, not vague aspirations.

## Quality Checklist

Self-review after writing:

- [ ] An engineer unfamiliar with the project can read and understand the core design in 5 minutes
- [ ] Every technical term is explained or exemplified on first use
- [ ] No "correct but useless" filler (e.g., "this system has good extensibility")
- [ ] Code paths and file names match the actual repository (verified by reading code)
- [ ] The design decisions section covers at least 3 key choices, each with trade-off explanations

## Anti-patterns

| Don't do this | Do this instead |
|--------------|----------------|
| Blindly follow an RFC template, filling every section | Only write sections that carry real information |
| "This system uses a layered architecture" | Draw a diagram + use a table to clarify each layer's responsibility |
| Paste large blocks of source code | Include simplified structures / pseudocode, use file paths to guide readers to the source |
| Only list interface definitions | Thread them together with a complete call-flow walkthrough |
| "Extensible," "high cohesion," "low coupling" | State specific design choices and their costs |
| Inconsistent use of bilingual terminology | Decide the term on first occurrence, keep it consistent throughout |

## Language & Formatting

- Either Chinese or English is fine; follow the project's primary language. Keep domain-specific English terms as-is in Chinese docs (e.g., handler, planner, MCP).
- Use `code font` for file names, function names, and commands.
- Use **bold** for key concepts (at most 1-2 per paragraph).
- Keep paragraphs short — no more than 4 sentences each.
