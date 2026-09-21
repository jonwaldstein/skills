---
description: Write or rewrite a pull request description in Jon's voice - brief, upbeat, one paragraph and/or a few bullets, with QA and testing steps a non-developer can follow. Use for "write the PR", "PR description", "open a PR", "update the PR body", or /pr.
---

# PR description

Write the description the way Jon writes it: short, plain, a little upbeat. The reader is a
reviewer or a QA teammate who wants to know what changed and how to check it. Nobody reads a wall
of text; the diff is right there for anyone who wants the details.

## Before writing

Gather, do not guess:

- The diff (`git diff <base>...HEAD` or `gh pr diff <number>`) and the commit messages.
- The ticket key from the branch name (`SOFT-1234` style) if there is one.
- Whether the branch is stacked on another PR (`gh pr list --head <base-branch>`).
- Whether a PR already exists (`gh pr view`). If so, edit it in place and keep anything below the
  checklist intact: demo links, Loom links, CodeRabbit or other bot blocks.

## Shape

```markdown
## Description

Resolves SOFT-1234.            <- only when there is a ticket
Stacked on #1234.              <- only when the branch builds on another open PR

One or two sentences in plain words saying what this does for the person using it.
An exclamation mark or one emoji is fine when the change is fun. 😄

- Optional: up to three bullets for the parts a reviewer or QA would otherwise miss.
- Each bullet one sentence. Say what it does, not how it is built.

## Testing Instructions

1. Numbered steps a QA person can follow without opening the code.
2. Each step says what to do and what they should see.
3. Three to five steps. End with the test command if there is one.

## Pre-review Checklist

- [x] Acceptance criteria satisfied and tested
- [x] Relevant `@since` tags added to code docblocks
- [x] Changelog entry added
- [x] E2E or unit tests added and passing locally   <- drop the lines that do not apply
```

## Rules

- **Changelog.** If the PR changes production code and `changelog/` has no entry for the branch,
  write one with the `changelog` skill before publishing.

- **Length.** The Description section is one paragraph and/or at most three bullets. If it is
  longer, cut. Never add Affects, Visuals, Background, Architecture or Implementation sections.
- **Voice.** First person plural or none ("This adds…", "Now the form…"). Casual but not sloppy.
  No hedging, no marketing. Contractions are fine. One emoji at most, and only when it fits.
- **Non-technical by default.** Say "the donation form", "the checkout page", "the settings
  screen", not class names. Name a file, function or attribute only when the reviewer has to go
  there, and never more than one or two in the whole description.
- **Testing steps are for QA, not developers.** "Throttle the network in dev tools" is fine.
  "Check that the `data-embed-shape` attribute parses" is not. Each step names the screen, the
  action, and the expected result. Put the automated test command last as its own step.
- **No measurements or tables.** If numbers matter, one sentence.
- **No narration of the process.** Nothing about how the change was found, what was tried, or
  what a review said.
- **Title.** `Feature:`, `Enhancement:`, `Fix:`, `Docs:`, `Tests:` or `Chore:` prefix, then a
  short lowercase sentence fragment. Never `Tweak:`.
- **Attribution.** End the body (above any preserved demo or bot sections) with:

  ```
  🤖 Generated with [Claude Code](https://claude.com/claude-code)
  ```

## Example

```markdown
## Description

Resolves SOFT-4297.

This adds a loading state for donation form embeds! Now with a fancy skeleton loader 😄

- On-page embeds show a grey skeleton shaped like the form while it loads, so the page barely moves when the form appears.
- If the form never loads, a link to open it on its own page shows up after ten seconds.

## Testing Instructions

1. Embed a form on a page with `[give_form id="X"]`, throttle the network in dev tools and reload. You should see a grey outline of the form, then the form fades in over it.
2. Block the form request in dev tools and reload. After about ten seconds an "Open donation form" link appears and opens the form in a new tab.
3. Switch the form to the modal display style and repeat step 2. The link appears inside the dialog.
4. `npm run test:e2e -- tests/e2e/donation-forms.spec.ts -g "embed formats"` covers all of the above.
```

## Publishing

- New PR: `gh pr create --base develop --title "<Type>: <title>" --body-file <file>`.
- Existing PR: `gh pr edit <number> --body-file <file>`. Write the body to the scratchpad first,
  then read the current body and splice preserved sections back in below the checklist.
- Show the final body in the reply so it can be skimmed without opening GitHub.
