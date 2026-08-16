---
name: pr-review
description: >
  Deep pull-request review that verifies every claim against the code before
  asserting it, proves numeric findings with a runnable dry run, and triages
  what is worth raising. Use whenever the user asks to review a PR, review
  changes, look at a diff, check a branch, or says "review #N", "what's wrong
  with this PR", "is this mergeable", or hands you a PR URL. Reach for it even
  when the ask sounds casual ("take a look at 414?"), because the value is the
  verification discipline, not the reading. Posts findings to GitHub only when
  the user asks for that.
---

# PR Review

Review by proving things, not by recognizing patterns. A finding you have not traced to the code is a guess, and a guess costs the author more time than saying nothing.

**Tradeoff:** This biases toward fewer, verified findings over broad coverage. If the user wants a quick skim, give one and say that is what it is.

## 1. Ground First

**Read the intent before the diff. Read the PR without touching the tree.**

- If given a ticket or spec link, read it first. The diff shows what the code does; only the spec says what it was meant to do.
- Read the PR description and commit messages. Authors state tradeoffs there and sometimes admit what was deferred.
- Fetch as a ref, not a checkout: `git fetch origin pull/<N>/head:pr-<N>`. Avoid `gh pr checkout`, which mutates a tree that may hold uncommitted work. Mention the ref afterwards so it can be deleted.
- Look at source and tests separately. Where the logic concentrates is where attention belongs.

No spec is fine. Review the diff on its own terms.

## 2. Verify Before Asserting

**The spec, the commit message, and the variable name tell you intent. Only the code tells you behavior.**

Every plausible finding gets checked, and a real share will not survive. Four that did not, from one review:

- "This path is probably unreachable" → the loop reset the variable unconditionally. Reachable, and the finding was backwards.
- "This bug is latent" → an endpoint called it directly. Live, not latent.
- "This endpoint is missing auth" → sibling endpoints used the same framework default. Not a finding.
- "This field feeds the checker" → the spec said so; nothing read it.

Before describing what a change breaks:
- Find the caller and read it. A field nothing reads cannot break anything.
- Read back every line range you cite. Wrong line numbers make an author distrust the rest of the review.
- Check CI, then interrogate what green means. A suite can pass while never reaching the changed paths, and a test can assert the wrong value.

When verification contradicts something you already said, correct it in one sentence and move on.

## 3. Prove Numbers By Running Them

**Never reason about arithmetic in prose. Execute it.**

- Write the few lines and run them. This is how you find out you were wrong before the author does.
- Choose inputs that make the right answer obvious. A boundary at the exact midpoint forces a symmetric result, so any asymmetry is the code's doing rather than the input's.
- A worked example the reader can check in their head is the most persuasive thing in a review.

## 4. Defect vs Decision

**Do not prescribe a fix when the code cannot tell you which behavior was intended.**

Some findings are bugs. Others are places where two behaviors are both defensible. For the second kind, write a question: state both options and what each costs.

A confident wrong fix is worse than a clear question, and the author may know a domain constraint you do not.

## 5. Triage and Report

**Thirty items get skimmed. Six get acted on.**

Rank findings, raise what deserves attention now, and hold the rest in the summary with a one-line reason so any can be promoted.

Fair reasons to hold: it needs a pathological input, the affected field has no reader yet, it is performance with no wrong output, or the impact turned out smaller than first described.

Each finding: claim, mechanism, worked example, consequence, one sentence of fix direction.

❌ "There might be an issue with how the boundary is calculated, you may want to double-check the bunker handling."

✅ "**Boundary ROB double-counts the pre-boundary bunker.** `total_drawdown` is full-leg consumption and `bdv` is also split pro-rata below, so the bunker is charged once and credited once. Depart 100, arrive 60, BDV 20, frac 0.5: the code gives 70 and splits 40/20; correct is 80 and 30/30. The total conserves either way, so nothing looks lost, but each year is off by a third."

## 6. Post Only When Asked

**Default output is a written summary. Never submit on the user's behalf.**

When asked to post, put comments in their *pending* review so they read and submit them.

- GitHub allows one pending review per user per PR, and REST returns 422 if one exists. Find the existing pending review and append with the GraphQL `addPullRequestReviewThread` mutation against its id.
- Comments must anchor inside a diff hunk. Outside one they fail silently or degrade to file-level. Check hunk ranges before choosing anchors.
- Keep internal priority labels out of comment bodies. Severity ranking is for the summary, not the author's inbox.

Reviews and reports. Does not fix code, push, or submit.
