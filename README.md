# Personal agent skills

Agent Skills, one folder per skill under `skills/`. Conventions and the
validation gate are in [AGENTS.md](AGENTS.md).

## Mine

| Skill | What it does |
| --- | --- |
| `create-jira-ticket` | Files a Jira issue from a brief via the Atlassian MCP, including parent, assignee, and backlog placement. |
| `github-issue-pr` | Files GitHub issues and PRs with `gh`. Issues describe behaviour, PRs carry code internals. |
| `research-report` | Plans and runs any research or experiment through code (baselines, controls, repeated runs), tries third-party tools in isolation, and publishes an evidence-backed HTML report that leads with the answer. |
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

Every skill, user-level, for Claude Code:

```bash
npx skills add frendysanusi/skills -g -s '*' -a claude-code -y
```

For other agents, change `-a` or use `--all` for every agent. Run it from any
directory. Without `-g` it installs into the current
directory as a project.

After pushing a change:

```bash
npx skills update -g
```

## Refresh the copies

```bash
curl -sL https://raw.githubusercontent.com/JuliusBrussee/caveman/main/skills/caveman/SKILL.md -o skills/caveman/SKILL.md
curl -sL https://raw.githubusercontent.com/JuliusBrussee/caveman/main/skills/caveman-commit/SKILL.md -o skills/caveman-commit/SKILL.md
curl -sL https://raw.githubusercontent.com/blader/humanizer/main/SKILL.md -o skills/humanizer/SKILL.md
```
