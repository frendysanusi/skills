---
name: write-implementation-plan
description: >
  Write an implementation plan for a ticket or feature as a local HTML file:
  grounded in the real code, with decisions to make, backend and frontend
  changes, tests, and an estimate by user story at agent pace. Use when the
  user asks for a plan, an implementation plan, a plan "in HTML", an estimate
  or story points for a ticket, or questions to ask before building.
---

# Write an implementation plan

## 1. Ground it

- Rank the sources: the spec first, then the demo or prototype for flow, then
  reference notes. For screens the design decides layout. Follow the
  highest-ranked source and raise conflicts as questions instead of picking.
- Read the code each part touches. Write "what exists today" from what you saw,
  with file paths.
- Work out what each design link or frame means for the build.
- A schema change, migration, or backfill is a question for the user, not a
  plan step.

## 2. Write it

Write a standalone HTML file in the repo root. Keep it local; don't publish or
upload it.

- Full width. Lists and tables, not paragraphs. No meta header with ticket
  keys or status.
- English only unless the user asks for another language.
- Light theme by default, with a sun/moon icon toggle.
- Diagrams are inline SVG. Check that every arrow ends on its box.

Sections, skipping what doesn't apply:

1. Flow
2. What exists today
3. Decisions needed: each with options, your recommendation first
4. Where the design differs from the spec or backend: one proposal per row
5. Backend changes and frontend changes: files and endpoints
6. Security, when relevant
7. Tests
8. Estimate:
   - by user story, mapped to its acceptance criteria;
   - at agent pace, about 1 point a day: the build at agent speed, plus review,
     QA, and waiting on people;
   - story points per ticket;
   - a "Not in the estimate" list.

## 3. Ask only what's needed

- Split domain questions (for the spec owner) from technical ones (for the
  team).
- Drop anything the code or docs already answer, and list it under "Already
  answered".
- Keep each question short, and merge similar ones.

## 4. Review and decide

- Get an independent review of the plan. Verify each finding against the code,
  then fold in what holds.
- When the user answers, move the item from "Decisions needed" to "Decided"
  with the answer and date. Don't ask it again.
- Then draft the ticket in a local `.md` next to the plan: background,
  requirements per repo, acceptance criteria, and estimate.
