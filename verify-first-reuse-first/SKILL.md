---
name: verify-first-reuse-first
description: >-
  A disciplined loop for implementing a feature, fix, or ticket in an existing
  codebase: ground every assumption against the real code BEFORE writing, build
  the smallest correct increment, then do a dedicated reuse-first pass that folds
  bespoke code into existing helpers. Use this whenever you're about to modify a
  non-trivial codebase you don't fully know — especially when the user says
  "verify first", "reuse first", "don't assume", "check the code first", "make it
  minimal", "what can we reuse", "is there an existing method", or asks you to
  implement a ticket/spec. Reach for it even when the user just hands you a task in
  an unfamiliar repo; the cost of guessing wrong there is high. Pairs with general
  coding-care guidelines (surgical changes, simplicity) rather than replacing them.
---

# Verify First, Reuse First

Most avoidable coding mistakes come from two moments of overconfidence: writing
code against an *imagined* version of the codebase, and leaving behind code that
*reimplements* something the codebase already had. This skill is a loop that
attacks both. It is slower up front and faster overall, because it spends cheap
reads to avoid expensive rewrites.

The loop has three phases. Don't skip phase 1 to "save time" — a wrong assumption
discovered after you've written tests and code costs far more than the read would
have. And don't skip phase 3 — first-draft code that works is rarely the code you
want to keep.

---

## Phase 1 — Ground yourself before writing a line

The goal is to replace every assumption your plan depends on with an observed
fact. You are looking for the specific shapes, signatures, and behaviors your
code will touch — not a general tour.

**Read the spec/ticket as the source of truth.** If there's a ticket, issue, or
written request, extract the actual acceptance criteria and, just as important,
the dependencies and constraints. Separate what's buildable now from what's
blocked on something that doesn't exist yet. Surface that split early — it
reshapes scope before you've invested in the wrong part.

**Map what already exists before designing.** Search the codebase for the
relevant subsystem: the models, the existing validators/handlers, the layer
where similar logic lives. A broad read-only sweep (delegate it if the codebase
is large) tells you where your change belongs and what it can lean on. You're
answering "where does this go and what's already here," not "how do I write it."

**Verify the exact things your code will touch.** Open the real files and
confirm:
- the precise data shape (don't assume field names, nesting, or that a
  `breakdown` is keyed the way you'd expect — go look)
- the exact signature and *direction* of functions you'll call or extend (a
  check that computes `expected = f(x)` is not the same as one that corrects `x`)
- the existing behavior you might duplicate or contradict
- where the code actually runs (write-time vs read-time, per-item vs batch,
  inside a transaction or not) — this often decides your whole approach

When you catch yourself about to write code based on what a function is *probably*
called or what a dict *probably* contains, stop and read it. State the facts you
verified back to the user briefly; it's how they catch a wrong premise before it
becomes a wrong diff.

**Surface tensions; recommend, then ask.** When verification reveals a genuine
fork the user owns — where the fix should live, which value to trust, how wide
the scope is — present it as a decision with your recommendation first, not an
open-ended menu. Decisions with a conventional default don't need a question;
just state the default you're taking and move on. Reserve questions for forks
where the answer actually changes what you build.

---

## Phase 2 — Build the smallest correct increment

Implement one slice at a time — one acceptance criterion, one fix — not the whole
feature at once. Smaller increments are easier to verify and easier to throw away
if a premise was wrong.

**Stay surgical.** Every changed line should trace to the task. Match the
surrounding code's style, naming, and idioms even where you'd personally differ.
Don't "improve" adjacent code, reformat untouched lines, or fix pre-existing
issues you happen to notice — mention them instead. When your change orphans an
import or variable, clean up *that*; leave pre-existing dead code alone.

**Make success checkable.** Turn the task into something you can run: write the
tests, then make them pass. Tests double as a clarity check — if you can't write
a coherent test, the numbers in your example probably aren't coherent either.
Keep test data telling a real story (consistent values, not arbitrary ones).

**Run the gates and read them honestly.** Run the project's tests, linter,
formatter, and type checker. When they complain, separate what *your* change
introduced from what was already failing — fix the former, leave the latter
(note it). If a tool flags a line you didn't touch, confirm it's pre-existing
(e.g. check it against the base branch) before deciding it's not yours. Report
outcomes plainly: if something fails or was skipped, say so.

---

## Phase 3 — The reuse-first second pass

This is the phase everyone skips, and it's where good code is separated from
working code. Once the increment passes, deliberately re-read your own diff with
one question: **does anything here already exist in the codebase?**

For each helper, fetch, coercion, or chunk of logic you wrote, search the repo
for a method that does the same job:
- a repository/query method that already does the lookup you hand-rolled
- a shared utility that already does the coercion/parsing/formatting
- a near-twin private method elsewhere that should be promoted and shared
- dead-but-correct code that does exactly what you need and just needs a caller

When you find one, prefer reusing it — even if it means a small refactor like
promoting a private helper to public, or extracting one shared function from two
near-duplicates. Two implementations of the same idea is the thing to eliminate;
the net line count should usually go *down* in this phase.

Be careful that reuse preserves behavior. If you unify two near-duplicates,
check their semantics actually match (e.g. does one treat `""` as `0` and the
other as `None`?) and pick the established one so existing callers don't shift
under you. Reuse that quietly changes behavior is worse than honest duplication.

Then ask whether the code can simply be *less*: collapse defensive ladders that
the data shape doesn't warrant, fold a re-read into the value you already
computed, delete a layer of indirection. Lean on the types/shape that flow from
the boundary instead of re-checking them at every step — and if the real problem
is that the boundary is untyped, name that as the root cause rather than papering
over it at every call site.

Run the gates again after the cleanup. A reuse pass that breaks a test isn't
done.

---

## After the work

Record the non-obvious decisions somewhere durable (a memory, a PR description,
a ticket comment) — the *why* behind scope calls, design forks, and accepted
trade-offs, not things the code or git history already say. The next person
(possibly you) shouldn't have to re-derive the reasoning.

---

## The shape of it, in one breath

Read the spec and the real code until your plan rests on facts, not guesses →
build the smallest correct slice, surgically, with tests and green gates →
re-read your diff and fold everything you can into what already exists, then make
it smaller → write down why. Verifying first prevents the wrong diff; reusing
second prevents the redundant one.
