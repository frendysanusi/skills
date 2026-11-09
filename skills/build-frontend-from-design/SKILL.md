---
name: build-frontend-from-design
description: >
  Build or change a frontend screen from a design (Figma frames or
  screenshots) so it matches the design, reuses the app's components without
  editing shared ones, and is checked in a real browser before it is called
  done. Use when implementing a screen, page, form, or UI ticket from Figma or
  screenshots, or when the user says "follow Figma", "match the design",
  "check it in the browser", or sends a screenshot of something that looks
  wrong.
---

# Build frontend from design

## 1. Read the sources

- The design decides layout, text, colours, placeholders, and which fields
  exist. A product spec or demo decides flow and domain rules where the design
  is silent. When they conflict, ask.
- List every frame and overlay the screen needs before writing code. A field in
  the design but not in the code, or the reverse, is a bug.
- Lists, options, and labels come from the API, never hardcoded. When the API
  sends codes, the frontend translates them.

## 2. Build

- Reuse the app's components and theme tokens. Check which token family the
  design uses; never hardcode a colour.
- Don't edit a shared component for one screen. Copy its look into a
  screen-local component. If a shared change is unavoidable, ask first.
- Give each control type one design across the screen: every dropdown, date
  field, and read-only field looks and behaves the same.
- Apply these everywhere:
  - Values the system fills in are read-only, with a grey background.
  - Buttons get a pointer cursor; disabled ones get `not-allowed`.
  - Placeholders show the format (`0.000`, `HH:MM`), not the unit.
  - Help text the design puts behind an (i) icon stays there.
- Put every string in the locale files for every language. Test with real
  translations, because a global `$t` mock hides missing keys.
- Guard the states a user can reach:
  - saving before the field that names the record is chosen;
  - defaults counted as edits;
  - a late async answer for a screen already left;
  - a reload losing a lock or token;
  - server errors that never reach their field.
- Keep logic in the store or a composable, not the template.

## 3. Check in a real browser

Unit tests don't show what a user sees.

- Drive a separate browser profile, for example Playwright's `connectOverCDP`
  against a browser started with a debugging port. The user logs in by hand.
- Screenshot each changed section and compare it with the design.
- Measure layout at a common laptop width. If `scrollWidth > clientWidth` on a
  container, content is hidden behind a scroll, and to the user a hidden
  column is a missing feature.
- Say when you save test data into shared records, and offer to delete it.

## 4. Review and clean up

- Get an independent review of the diff. Verify each finding against the code
  before fixing it, and report what you skipped and why.
- Check the backend for endpoints and response fields this branch added that
  the frontend never reads. Other services may use them too, so search their
  callers before proposing a removal, and don't remove anything without
  asking.
- Comments say only what the code can't. No spec versions, document names, or
  links.
- Run the repo's linter, its formatter on touched files only, and the full unit
  suite. Say which failures aren't yours.

## 5. Ship

- Fetch, then move onto the latest default branch. Keep local work without
  `git stash`: save a patch, move the branch, re-apply it, and resolve
  conflicts.
- Make one commit per repo with a one-line message in the repo's convention.
  Commit and push only when asked.
