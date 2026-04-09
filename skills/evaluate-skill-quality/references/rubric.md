# Skill Evaluation Rubric (75 points)

This rubric follows the same category layout shown in the target evaluation page:

- security: 30
- frontmatter: 20
- resources: 3
- structure: 5
- markdown: 14
- quality: 1
- scripts: 2

## 1) Security (16 rules, max 30)

Rules extracted from the evaluation detail:

1. `malicious.pipe_execution`
2. `malicious.crypto_mining`
3. `sec-executable-binary`
4. `sec-shell-bash-interactive`
5. `sec-shell-netcat-exec`
6. `sec-shell-powershell-iex2`
7. `sec-shell-powershell-iex`
8. `sec-shell-pipe-to-shell`
9. `sec-python-subprocess-shell`
10. `sec-python-pickle`
11. `sec-python-eval-exec`
12. `sec-secret-slack-token`
13. `sec-secret-gitlab-pat`
14. `sec-secret-github-token`
15. `sec-secret-aws-access-key`
16. `sec-secret-private-key`

Default severity:

- `P0`: 1, 2, 8, 12-16
- `P1`: 3-7, 9-11

Detection hints:

```bash
# suspicious shell pipe execution
rg -n "curl\\b.*\\|\\s*(sh|bash|zsh)|wget\\b.*\\|\\s*(sh|bash|zsh)" <skill-dir>

# crypto mining indicators
rg -n "xmrig|minerd|cryptonight|stratum\\+tcp|coinhive" <skill-dir>

# executable binaries
find <skill-dir> -type f -perm -111

# reverse shell / netcat / powershell IEX patterns
rg -n "bash -i|nc -e|netcat -e|Invoke-Expression|\\bIEX\\b|-EncodedCommand|/enc\\b" <skill-dir>

# dangerous python usage
rg -n "subprocess\\.(run|Popen|call).*(shell\\s*=\\s*True)|pickle\\.(load|loads)\\(|\\beval\\(|\\bexec\\(" <skill-dir>

# embedded secrets
rg -n "xox[baprs]-|glpat-|gh[pousr]_|AKIA[0-9A-Z]{16}|ASIA[0-9A-Z]{16}|BEGIN (RSA|DSA|EC|OPENSSH) PRIVATE KEY" <skill-dir>
```

## 2) Scripts (2 rules, max 2)

Rules extracted from the evaluation detail:

1. `python-syntax-error` (`P1`)
2. `script-not-utf8` (`P1`)

Detection hints:

```bash
find <skill-dir>/scripts -type f -name "*.py" -print0 | xargs -0 -I{} python3 -m py_compile "{}"
find <skill-dir>/scripts -type f -name "*.py" -print0 | xargs -0 file -I
```

## 3) Frontmatter (11 rules, max 20)

Rules extracted from the evaluation detail:

1. `compatibility-empty`
2. `description-angle-brackets`
3. `invalid-name-length`
4. `invalid-name-hyphens`
5. `invalid-name-format`
6. `description-empty`
7. `missing-description`
8. `missing-name`
9. `duplicate-frontmatter-keys`
10. `frontmatter-invalid-syntax`
11. `missing-frontmatter`

Default severity:

- `P1`: 3, 4, 5, 7, 8, 9, 10, 11
- `P2`: 1, 2, 6

Detection hints:

```bash
# basic frontmatter presence
head -n 40 <skill-dir>/SKILL.md

# key fields
rg -n "^name:\\s*.+|^description:\\s*.+" <skill-dir>/SKILL.md

# invalid name format (must be hyphen-case lowercase)
rg -n "^name:\\s*[A-Z_]|^name:\\s*.*[^a-z0-9-].*" <skill-dir>/SKILL.md

# duplicate keys (manual review if parser unavailable)
rg -n "^(name|description|compatibility):" <skill-dir>/SKILL.md
```

## 4) Structure (2 rules, max 5)

Rules extracted from the evaluation detail:

1. `name-dir-mismatch` (`P1`)
2. `missing-skill-md` (`P1`)

Detection hints:

```bash
test -f <skill-dir>/SKILL.md
# compare folder basename to frontmatter name
basename <skill-dir>
rg -n "^name:\\s*" <skill-dir>/SKILL.md
```

## 5) Resources (3 rules, max 3)

Compatibility rules aligned to the same scoring category:

1. `resources-missing-openai-yaml` (`P2`)
2. `resources-openai-yaml-missing-interface` (`P1`)
3. `resources-openai-default-prompt-stale` (`P2`)

Detection hints:

```bash
test -f <skill-dir>/agents/openai.yaml
rg -n "^version:|^interface:|display_name:|short_description:|default_prompt:" <skill-dir>/agents/openai.yaml
```

## 6) Markdown (14 rules, max 14)

Compatibility rules aligned to the same scoring category:

1. `markdown-missing-h1` (`P2`)
2. `markdown-missing-overview` (`P2`)
3. `markdown-missing-trigger-guidance` (`P2`)
4. `markdown-missing-workflow` (`P1`)
5. `markdown-missing-output-contract` (`P1`)
6. `markdown-missing-verification-step` (`P1`)
7. `markdown-missing-severity-guidance` (`P2`)
8. `markdown-heading-jump` (`P2`)
9. `markdown-empty-section` (`P2`)
10. `markdown-codeblock-no-language` (`P2`)
11. `markdown-broken-relative-link` (`P2`)
12. `markdown-path-not-monospace` (`P2`)
13. `markdown-no-failure-handling` (`P1`)
14. `markdown-overlong-skill` (`P2`)

Detection hints:

```bash
rg -n "^# " <skill-dir>/SKILL.md
rg -n "^## " <skill-dir>/SKILL.md
rg -n "```$" <skill-dir>/SKILL.md
rg -n "\\[[^]]+\\]\\(([^)]+)\\)" <skill-dir>/SKILL.md
wc -l <skill-dir>/SKILL.md
```

## 7) Quality (1 rule, max 1)

Compatibility rule aligned to the same scoring category:

1. `quality-non-actionable-instructions` (`P2`)

Signal:

- Vague instructions without executable steps, concrete checks, or output contract.
