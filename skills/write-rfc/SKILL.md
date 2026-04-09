---
name: write-rfc
description: Use when drafting or revising an engineering RFC for peer review, especially when inputs are partial or messy and the output must follow a strict section template with explicit assumptions, trade-offs, risks, rollout, and open questions.
---

# Write RFC

Produce a single, review-ready Markdown RFC for senior engineers and tech leads.

## Input Contract

Accept user-provided material in any quality/shape, including:

- Background / Context
- Goals
- Non-goals
- Scope
- Constraints
- Initial ideas / candidate approaches
- Related materials (issues/PRs/docs)

Treat user-provided content as source of truth.

## Output Contract

- Return exactly one Markdown document in English
- No preamble, no side commentary
- Keep content concrete and concise
- Do not use marketing language
- Do not invent unknown facts

If critical information is missing:

- State explicit assumptions in relevant sections, and/or
- Add items under `Open Questions`

## Required RFC Structure

Use exactly this section order and naming:

1. Title
2. Authors
3. Status
4. Creation Date
5. Summary
6. Motivation & Background
7. Goals
8. Non-Goals
9. Proposed Design / Approach
   - Architecture Overview
   - Data Model / APIs / Interfaces
   - Edge Cases & Failure Modes
10. Alternatives Considered
11. Impact & Risks
    - Backward Compatibility
    - Security / Privacy
    - Performance / Cost
12. Migration / Rollout Plan
    - Phases
    - Rollback Strategy
13. Testing & Validation Plan
14. Open Questions
15. References

## Authoring Workflow

1. Parse and normalize user input into the required sections.
2. Preserve explicit user constraints and scope boundaries.
3. Convert vague statements into:
   - concrete assumptions, or
   - open questions (if unresolved).
4. In `Proposed Design / Approach`, describe architecture, interfaces, and failure handling clearly enough for implementation planning.
5. In `Alternatives Considered`, include 2-3 realistic approaches when relevant, with brief trade-off comparison.
6. In `Impact & Risks`, include blast radius and rollback implications.
7. In `Testing & Validation Plan`, include actionable verification:
   - what to test
   - how to test
   - what metrics/signals define success/failure
8. In `References`, include only materials provided or clearly implied by user context.

## Quality Bar

Before finalizing, verify:

- Section names and order exactly match the required structure.
- No fabricated facts, IDs, decisions, or timelines.
- Assumptions are explicitly marked.
- Open questions cover all critical unknowns.
- Trade-offs are explicit (not hidden).
- Rollout and rollback are both present and operationally concrete.
- Testing plan is executable and measurable.
