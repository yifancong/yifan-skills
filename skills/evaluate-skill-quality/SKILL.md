---
name: evaluate-skill-quality
description: Use when auditing a Codex skill package and producing a scored quality and security report before publishing, sharing, or merging skill updates.
---

# Evaluate Skill Quality

Audit a skill folder and produce a scorecard aligned to a `75`-point rubric with `P0/P1/P2` issue severity.

Use this skill when a user asks for:

- Skill quality review
- Skill safety review
- Release-readiness check before publish
- Regression check after editing `SKILL.md`, `agents/openai.yaml`, `references/`, or `scripts/`

## Required Input

- Target skill directory path (for example: `skills/my-skill`)

## Required Output

Return one report with these sections in order:

1. `Score Summary`
2. `Category Scores`
3. `P0/P1/P2 Counts`
4. `Findings` (ordered by severity, with file paths and line numbers when possible)
5. `Top Fixes` (short, actionable)

Use this format:

```text
Total: <score>/75
Risk: <Low|Medium|High>
P0: <n> | P1: <n> | P2: <n>

Category:
- security: <x>/30
- frontmatter: <x>/20
- resources: <x>/3
- structure: <x>/5
- markdown: <x>/14
- quality: <x>/1
- scripts: <x>/2
```

## Workflow

1. Inventory the package.
2. Run checks from `references/rubric.md`.
3. Record pass/fail per rule id.
4. Convert pass rate to category score.
5. Assign severity (`P0/P1/P2`) to each failed rule.
6. Write the final report.

## Inventory Commands

Run these before scoring:

```bash
ls -la <skill-dir>
find <skill-dir> -maxdepth 3 -type f | sort
rg -n "^---|^name:|^description:" <skill-dir>/SKILL.md
```

## Scoring Model

Category max points:

- `security`: 30
- `frontmatter`: 20
- `resources`: 3
- `structure`: 5
- `markdown`: 14
- `quality`: 1
- `scripts`: 2

For each category:

`category_score = round(category_max * passed_rules / total_rules)`

Total score:

`total = sum(category_scores)` (max `75`)

## Severity Model

- `P0`: security-critical issues (RCE patterns, embedded secrets, malicious payload signals)
- `P1`: correctness and packaging risks (invalid frontmatter, missing mandatory files, broken scripts)
- `P2`: quality and maintainability gaps (docs clarity, markdown hygiene, metadata quality)

Risk level:

- `High`: any `P0`, or total `< 60`
- `Medium`: no `P0` and (`P1 > 0` or total `< 70`)
- `Low`: otherwise

## Rule Source

Use `references/rubric.md` as the source of truth for rule ids, rule groups, and detection hints.

## Guardrails

- Do not auto-fix unless user asks.
- Quote exact failed rule ids.
- Prefer exact file paths and line references.
- Mark uncertain matches as `needs-manual-review` instead of hard-failing.
