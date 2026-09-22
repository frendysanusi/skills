# Personal agent skills

Agent Skills, one folder per skill under `skills/`. Conventions and the
validation gate are in [AGENTS.md](AGENTS.md).

## Mine

| Skill | What it does |
| --- | --- |
| `create-jira-ticket` | Files a Jira issue from a brief via the Atlassian MCP, including parent, assignee, and backlog placement. |
| `github-issue-pr` | Files GitHub issues and PRs with `gh`. Issues describe behaviour, PRs carry code internals. |
| `unslopify-comment` | Keeps code comments and docstrings few, short, and load-bearing. Defaults to none, and never touches the code. |
| `pr-review` | Traces every PR claim to the code, proves numbers by running them, and triages what to raise. Posts to GitHub only when asked. |
| `verify-first-build-least` | Ground the change in the real code, build the smallest thing that works, then fold what you wrote into what already exists. |

## Copied

Unmodified copies. Refreshing overwrites them, so any local edit is lost.

| Skill | What it does | Source |
| --- | --- | --- |
| `caveman` | Terse replies that keep the technical content. | [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) |
| `caveman-commit` | Conventional Commits, subject under 50 characters. | [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) |
| `humanizer` | Rewrites AI-sounding prose without changing what it says. | [blader/humanizer](https://github.com/blader/humanizer) |

## Install

```bash
npx skills add . --list
npx skills add . --skill pr-review -a claude -g -y
```

From GitHub, on another machine:

```bash
npx skills add frendysanusi/skills --skill pr-review -a claude -g -y
```

This machine symlinks instead, so an edit here is live in the next session:

```bash
for d in skills/*/; do ln -sfn "$PWD/$d" ~/.claude/skills/"$(basename "$d")"; done
```

## Refresh the copies

```bash
curl -sL https://raw.githubusercontent.com/JuliusBrussee/caveman/main/skills/caveman/SKILL.md -o skills/caveman/SKILL.md
curl -sL https://raw.githubusercontent.com/JuliusBrussee/caveman/main/skills/caveman-commit/SKILL.md -o skills/caveman-commit/SKILL.md
curl -sL https://raw.githubusercontent.com/blader/humanizer/main/SKILL.md -o skills/humanizer/SKILL.md
```
