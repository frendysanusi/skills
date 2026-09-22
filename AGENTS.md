# Repository Guidelines

## Project Structure & Module Organization

This repository publishes installable Agent Skills.

- `skills/<skill-name>/SKILL.md` is the required source for each skill.
- `skills.sh.json` controls grouping and display on skills.sh.
- `README.md` documents what each skill does, how to install, and how to refresh the copied skills.

There is no application source tree, build output, or bundled test suite.

## Build, Test, and Development Commands

- `npx skills add . --list` checks local repository discovery and lists all skills.
- `npx skills use . --skill <skill-name>` renders a skill prompt bundle without installing it.
- `python3 ~/.agents/skills/skill-creator/scripts/quick_validate.py skills/<skill-name>` validates a skill folder's required frontmatter and naming.
- `ln -sfn "$PWD/skills/<skill-name>" ~/.claude/skills/<skill-name>` installs a skill by symlink, so an edit here is live in the next session.

Run validation after every `SKILL.md` or metadata edit.

## Coding Style & Naming Conventions

Write skills in concise Markdown with YAML frontmatter containing `name` and `description`. Skill folder names and `name` values must be lowercase hyphen-case, for example `verify-first-reuse-first`. Keep descriptions trigger-focused, naming what the skill does and the phrasings that should make an agent reach for it.

Only `name` and `description` load into the system prompt; a skill's body loads when it triggers. Give each skill one job, because two skills on the same trigger both load.

## Testing Guidelines

There are no unit tests. Treat validation as the test gate:

1. Run `quick_validate.py` on each edited skill. `ponytail` reports one expected warning for `argument-hint`, which Claude Code supports and the validator does not.
2. Run `npx skills add . --list` and confirm the expected skill count.
3. For behavior-sensitive edits, run `npx skills use . --skill <skill-name>` and read the rendered instructions top to bottom.

## Commit & Pull Request Guidelines

History uses conventional commits such as `feat: add humanizer` and `docs(pr-review): add comment template, posting steps`.

For PRs, include a short summary, affected skill names, validation commands run, and any intentional scope limits.

## Agent-Specific Instructions

Do not commit private logs, credentials, local machine paths inside skill instructions, or generated artifacts. Never copy skill files into `~/.claude/skills/`; symlink them so this repo stays the only source. Keep skill instructions harness-neutral unless a skill explicitly targets one agent. Prefer focused edits over broad rewrites.
