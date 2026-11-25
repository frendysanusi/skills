---
name: frenwork
description: >
  frennn's build workflow as one named pass: ground the change in real code,
  build the least that works, then self-review before handing it back. A thin
  orchestrator over write-implementation-plan, verify-first-build-least,
  build-frontend-from-design, and unslopify-comment so the agent runs the whole
  loop without being told each step. Use only when the user types
  /frenwork or says "run frenwork" or "frenwork this"; do not auto-trigger on
  ordinary coding tasks.
---

# frenwork

One entry point for how a change gets built here, so the same loop runs every
time without re-explaining it: review the real code, build the least that works,
reuse instead of duplicating, and keep each change where it belongs.

This is an orchestrator. The real instructions live in the skills it names;
follow each one in full when you reach its step. Scale it to the task: a one-line
fix does not need the whole loop. Stop before commit and PR: frenwork ends at a
reviewed, working diff handed back to the user.

## Guardrails

These hold across every step. They're the corrections that don't live in
another skill, so follow them without being reminded.

- **Don't run the app or its services.** Dev servers, workers, and other
  long-running processes are the user's to start. Ask them to run it and report
  back instead of launching it yourself.
- **Raise schema and data changes before building them.** A migration, a model
  or data-model change, or a backfill is a decision to surface first, not
  something to fold quietly into the diff.
- **Scope your test, lint, and format runs to what you touched.** Run the tests
  for the files you changed with the repo's own runner, and format only the
  lines or files your change touched, never a whole directory, which churns
  files that aren't yours.
- **Never open secret files.** For `.env` files and anything named like
  credentials or a token, refer to them by filename and don't read their
  contents.
- **Don't stash or discard the user's working tree.** They keep their own work
  in progress; compare with `git show` or a worktree instead of `git stash`.
- **Ask only what's the user's to decide, and batch it.** A fork with a
  conventional default needs no question, so take the default and say so. Save
  real questions for one pass instead of drip-feeding them.

## 1. Plan: write it, then self-review it

Follow **write-implementation-plan** to produce the plan: ground it in the real
code, lay out the decisions with your recommendation on each, the backend and
frontend changes, the tests, and the estimate by user story.

Then self-review the plan before building from it. Re-read it against the code
the way you'd review someone else's: is every "what exists today" claim backed
by a file you opened, is each decision real and scoped, does anything need the
user's answer first? Report what you verified and any fork you're taking, so a
wrong premise gets caught before it turns into a wrong diff.

## 2. Build: least that works

Follow **verify-first-build-least**, steps 2 and 3. Climb the reuse ladder,
build one slice at a time, match the surrounding style, and prove it with the
repo's own test, lint, and type commands. Build nothing speculative.

When the change is a frontend screen, follow **build-frontend-from-design** for
the build: it reuses the app's components, matches the design, guards the states
a user can reach, and verifies the result in a real browser, screenshotting each
section against the design and checking nothing is hidden behind a scroll. Unit
tests don't show what a user sees. Take it through its browser check and review,
but stop before its ship step; commit and PR stay yours.

## 3. Review: before handoff

Re-read your own diff the way you'd review someone else's:

- **Reuse and duplication.** Does any of this already exist here? Run the reuse
  pass, verify-first-build-least step 4.
- **Placement.** Is each change in the layer it belongs to, not bolted onto the
  path the ticket happened to name?
- **Structure and simplification.** Can it be less? Collapse what the data shape
  does not warrant.
- **Comments.** If you wrote any, follow **unslopify-comment**.

Run the gates one more time, then hand back the diff with a short note: what you
changed, what you verified, and any corner you cut on purpose.

## 4. Commit and PR: only when the user asks

frenwork ends at a reviewed diff. Committing, pushing, and opening a PR are the
user's call, so never do them on your own. Ask first.

When the user does ask:

- **Commit.** One commit per repo, with a one-line subject in the repo's
  convention.
- **PR.** Keep it simple and easy to read. Lead with a one-line summary of what
  changed and why, then short sections built from bullets, numbered points, or a
  table, never walls of prose. Include only what a reviewer needs: what changed,
  how to test it, and anything to watch. Follow the repo's PR template when it
  has one.
