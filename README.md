# Personal agent skills

This repo has installable Agent Skills. Edit mine here. Outsourced skills are copies: overwrite them from upstream when you want a newer version. A local edit to an outsourced file is discarded on refresh. If you need a real fork, copy it to a new name under Mine.

## Mine

| Skill | What it does |
| --- | --- |
| `create-jira-ticket` | Files a Jira issue from a brief via the Atlassian MCP (format, parent, assignee, backlog vs sprint). |
| `github-issue-pr` | Files GitHub issues and PRs with `gh`. Issues describe behaviour; PRs can mention code internals. |
| `pr-review` | Traces PR claims to the code, proves numbers with a dry run, and triages what to raise. Posts to GitHub only when asked. |
| `verify-first-reuse-first` | For unfamiliar code: check the real code first, ship the smallest increment, then fold new helpers into existing ones. |

## Outsourced

| Skill | What it does | Source |
| --- | --- | --- |
| `caveman` | Terse replies that keep the technical content, six intensity levels. | [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) |
| `caveman-commit` | Conventional Commits: subject ≤50 chars, body only when the why is not obvious. | [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) |
| `humanizer` | Drops common AI writing patterns. Based on Wikipedia's Signs of AI writing. | [blader/humanizer](https://github.com/blader/humanizer) |
| `karpathy-guidelines` | No speculative code, surgical diffs, explicit assumptions, check that it worked. | [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) |
| `ponytail` | Smallest change that works: YAGNI, stdlib first. | [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) |
| `ponytail-review` | Review that only hunts over-engineering: what to delete and what replaces it. | [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) |

## Install

```bash
npx skills add . --list
npx skills add . --skill pr-review -a claude -g -y
```

From GitHub:

```bash
npx skills add frendysanusi/skills --list
npx skills add frendysanusi/skills --skill pr-review -a claude -g -y
```

## Refresh outsourced skills

From the repo root. Each command overwrites the local copy.

```bash
curl -sL https://raw.githubusercontent.com/JuliusBrussee/caveman/main/skills/caveman/SKILL.md -o caveman/SKILL.md
curl -sL https://raw.githubusercontent.com/JuliusBrussee/caveman/main/skills/caveman-commit/SKILL.md -o caveman-commit/SKILL.md
curl -sL https://raw.githubusercontent.com/blader/humanizer/main/SKILL.md -o humanizer/SKILL.md
curl -sL https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/skills/karpathy-guidelines/SKILL.md -o karpathy-guidelines/SKILL.md
curl -sL https://raw.githubusercontent.com/DietrichGebert/ponytail/main/skills/ponytail/SKILL.md -o ponytail/SKILL.md
curl -sL https://raw.githubusercontent.com/DietrichGebert/ponytail/main/skills/ponytail-review/SKILL.md -o ponytail-review/SKILL.md
```
