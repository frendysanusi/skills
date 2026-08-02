# Personal Agent Skills

This repository contains installable Agent Skills.

## Skills

### caveman

Ultra-compressed terse-response mode that cuts output tokens while keeping full technical accuracy, across six intensity levels.

### karpathy-guidelines

Behavioral guidelines that reduce common LLM coding mistakes: no speculative code, surgical diffs, explicit assumptions, and verifiable success criteria.

### verify-first-reuse-first

A three-phase loop for unfamiliar codebases — ground every assumption against real code, build the smallest correct increment, then fold bespoke code into existing helpers.

### create-jira-ticket

Turns a rough brief into a well-structured Jira issue via the Atlassian MCP, handling description formatting, parent/epic linking, assignee resolution, and backlog or sprint placement.

### github-issue-pr

Drafts and files GitHub issues and pull requests with the `gh` CLI, keeping issues behaviour-only and PR change sections internals-aware.

## Local Validation

List skills from this repository:

```bash
npx skills add . --list
```

Install the caveman skill into Claude Code from this local checkout:

```bash
npx skills add . --skill caveman -a claude -g -y
```

Install the verify-first-reuse-first skill into Claude Code from this local checkout:

```bash
npx skills add . --skill verify-first-reuse-first -a claude -g -y
```

Replace `claude` with another supported agent target when installing for a different harness, and `--skill` with any skill name listed above.

## Publishing

Push this repository to GitHub, then install it from the published source:

```bash
npx skills add frendysanusi/skills --list
npx skills add frendysanusi/skills --skill caveman -a claude -g -y
npx skills add frendysanusi/skills --skill verify-first-reuse-first -a claude -g -y
```

Once the repository is installed through the `skills` CLI, skills.sh can discover it through CLI telemetry and create the public listing.
