---
name: research-report
description: >
  Plan and run any research or experiment through code, then publish the result
  as an elegant, evidence-backed HTML report that leads with the answer. Works
  for benchmarks, comparisons of tools or approaches, evaluations against known
  answers, performance and cost studies, spikes and exploratory investigations.
  Covers framing the question, baselines and controls, reproducible repeated
  measurement, safely trying third-party models or packages, analysis with
  spread and limitations, and report design. Use when asked to research,
  investigate, benchmark, compare, evaluate, test or try something ("compare A
  with B", "is X faster", "retest it N times", "try this model", "measure the
  cost"), to write findings up as a report, or to fix or restyle a research
  report.
---

# Research Report

Answer one question with evidence the reader can check. Every number in the report comes from something you ran through code and saved, never from reasoning, memory or a hand edit. Classifying documents, timing an API, comparing two libraries and measuring a migration's cost all follow the same method.

**Tradeoff:** This is slower than a quick try. Repeated runs, saved raw data and a controlled setup cost time, and the conclusions are only worth something with them. If the user wants a quick look, run once, say so, and label the result as preliminary.

## 1. Frame the question

Before running anything long, write down and agree with the user:

- **The question**, and the decision it informs.
- **The options or conditions** being compared, including the current baseline.
- **The measures**: one primary measure (the headline) and a few secondary ones, such as accuracy, latency, cost, error rate, throughput or effort.
- **The inputs and scope**: what is in, what is out, and why.
- **What would count as a clear answer**, so the result cannot be read either way afterwards.

## 2. Measure through code

- Drive the real system with a script. It saves one raw-results file per run, holding each item's inputs, outputs, measures, errors and run metadata (date, versions, configuration, machine). The report is built from these files only.
- Repeat anything non-deterministic, **3 runs** unless told otherwise, and report the spread as well as the average. State `n` everywhere.
- Measure the production-like case: cold caches, realistic data, billed prices. If warm or ideal conditions would flatter a result, say which one you measured.
- Never read secrets or `.env` files. Credentials come from the environment the user already set up. If a key is missing or access is blocked, stop and ask the user to supply it or run the step; do not look for another way in.
- Keep runs from disturbing each other. Jobs that share a GPU, a rate limit or a database skew each other's timings, so run timing-sensitive work one after another, or say next to the numbers that they ran together.
- Report inputs that could not be processed as not measured; never drop them silently. On long runs, give progress when asked: items done, failures so far, and an ETA.

## 3. Controls and fair comparison

- Give every option the same inputs, preprocessing, instructions and environment. Record the exact prompts, configs or parameters and show them verbatim in an appendix.
- Change one variable at a time. If you suspect a confound (wording, a size limit, a threshold, warm-up), add a variant that changes only that, and report both.
- Record anything that silently degrades an option: truncation, timeouts, retries, rate limiting, fallbacks.
- Decide fallbacks and safety nets explicitly. "X alone" means its result stands; "X with fallback" is a different option. Say which one you measured.

## 4. When results are scored against a known answer

This applies to evaluations such as classification, extraction and QA. Skip it for pure performance or cost studies.

- Take ground truth from something that already exists (a folder, a label column, a spec, a reference output) and state the mapping. Do not invent labels.
- Decide up front how edge cases count: an `unknown`, a refusal or a crash is usually wrong, not skipped.
- Report the rate over every item × run, each item's most common outcome, and whether it changed between runs.

## 5. When trying something third-party

A package, model or service you have not used before is untrusted until reviewed. Recap what it is (from its own docs) and your plan, and get a yes, before installing anything.

1. **Review before install.** Fetch the artifact only. Check its hash against the registry's raw metadata, not a web summary. Read the code for network calls, environment and credential access, `subprocess`/`eval`/`exec`, unsafe deserialisation (pickle, `torch.load`), remote-code flags and default bind addresses.
2. **Isolate.** A separate environment in a scratch directory, with every cache pointed there too. Install and run with an empty environment (`env -i`) so no app credential is visible.
3. **Pin and verify.** Fixed versions and revisions, and hashes checked for downloaded weights or binaries. Prefer safe formats.
4. **Run locally and offline** where possible: bound to `127.0.0.1`, only what is under test loaded.
5. **Swap in, do not rebuild.** If it speaks the same interface as what it replaces, point the existing client at it and change nothing else. Send dummy credentials so real ones never leave.
6. **Clean up** when the user is done: stop processes, delete the environment, downloads and caches, and confirm nothing was left behind. Keep only the raw results.

## 6. Analyse

- Show the spread, not just the average, and per-segment breakdowns where the inputs differ in kind. Do not read much into small differences at small `n`; say when a gap is within run-to-run variation.
- When a result is surprisingly bad or good, open the raw data before explaining it. A surprising number often has a mechanical cause: a threshold, a limit, a fallback, a cache. Show the evidence for the cause.
- Keep facts apart from interpretation. Quote and link the source for any claim about how a tool was built, trained or configured.
- Write the limitations: what the setup cannot tell you, and what would change the answer.

## 7. Build the report

Build it from a template plus a small generator that injects the raw results as data. The generator reads prompts and configs from their source files, needs no secrets, and still renders when an optional option or variant is missing. Change the template and rerun the generator; never hand-edit only the generated page.

**Structure, answer first:**

1. **Scoreboard**: one card per main option, with the primary measure large and the secondary measures and deltas against the baseline below. Highlight the recommended option. Put side experiments in a separate, compact, muted row with a one-line explanation.
2. **Three to five key findings.** Each says something the scoreboard does not.
3. **Breakdowns**: charts and tables by segment.
4. **Per-item detail** where items exist.
5. **Method and limitations.**
6. **Appendices**: the per-item results in full, and the exact prompts, configs or questions as-is (for example pretty-printed JSON), side by side per variant.

There is no closing summary that restates the page.

**Design: elegant, simple, eye-catching.** What readers corrected on past reports:

- **Lists, not commas.** Any enumeration (items, reasons, variants, IDs) is a bulleted list.
- **Status follows what matters.** Row flags reflect the main options only. Side experiments get their own columns, so they cannot flag every row.
- **Colour sparingly.** No row or cell background fills. Mark rows with a thin left bar and a small badge. Normal results are neutral grey and only problems are red; colour everywhere reads as noise.
- **Chips breathe.** Labels and numbers are pills with real padding, and secondary values sit on their own line.
- **Nothing hides behind hover.** Hover may add detail, but a table somewhere spells out every value.
- **Charts.** Validate the categorical palette for colour-blind separation and contrast, in light and dark, before use. Put a value label on every bar. Keep one colour per option across the scoreboard and every chart.
- **Layout and language.** Wide enough for the widest table, headers allowed to wrap, first column sticky in wide tables, light and dark themes, and every string in each language the report offers.

## 8. Verify and hand over

- Run the page's script outside a browser in every language (a stubbed DOM is enough) and fail on `undefined`, `NaN` or `[object`. Check the totals against the raw results.
- Do not loop on screenshots. Take one only when the user asks for a visual check or reports a visual problem, and stop when told.
- Hand over plainly: the answer, what was run and how many times, what failed and why, the files changed, and anything still running or left installed.
