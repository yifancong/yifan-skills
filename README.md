# yifan-skills

A small collection of reusable agent “skills” for Codex-style workflows.

## Skills

| Skill | What it helps with | Install |
| --- | --- | --- |
| Axios to Fetch Migration | Migrate JavaScript/TypeScript codebases from `axios` to `fetch` with behavior-preserving refactors and test-first validation. | `npx skills add yifancong/yifan-skills --path skills/axios-to-fetch-migration` |
| Evaluate Skill Quality | Audit a skill package with a 75-point rubric and report `P0/P1/P2` risks across security, frontmatter, structure, resources, markdown, quality, and scripts. | `npx skills add yifancong/yifan-skills --path skills/evaluate-skill-quality` |
| Write RFC | Draft clear, review-ready engineering RFCs from partial or messy inputs using a strict, senior-review oriented structure. | `npx skills add yifancong/yifan-skills --path skills/write-rfc` |

## Quick start

Install a skill from this repo:

```bash
npx skills add yifancong/yifan-skills --path skills/<skill-name>
```

Then follow the workflow in the skill’s `SKILL.md`.

If your agent terminal supports slash-commands, you may be able to invoke a skill as:

```text
/<skill-name>
```

## Repository structure

```text
skills/
  <skill-name>/
    SKILL.md
    agents/            # optional agent configs
    references/        # optional checklists / deep notes
```

## Adding a new skill

1. Create `skills/<skill-name>/` (kebab-case recommended).
2. Add `SKILL.md` with a clear trigger, workflow, and exit criteria.
3. Add `references/` for supporting notes and checklists if useful.
4. Keep steps executable (commands, verification, rollback notes).

## License

No license file is included yet.
