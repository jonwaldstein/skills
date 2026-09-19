---
description: Write or rewrite a Linear ticket in Jon's voice - a plain-language description for stakeholders and QA, steps to check it, and a separate Technical details section for engineers. Use for "create a Linear ticket", "write the ticket", "update the ticket description", "file this in Linear", or /linear-ticket.
---

# Linear ticket

Write the ticket the way Jon writes it: plain, short, for the people who are not going to read
the code. Product, support and QA read the top. Engineers scroll to Technical details. Same voice
as the `pr` skill; the difference is that a ticket outlives the PR, so it says what the change
does for the site owner, not what the diff does.

## Before writing

Gather, do not guess:

- What the change does for someone using the site, in their words. Read the PR body, the diff
  or the plan file until you can say it without a class name.
- Whether a ticket already exists: `list_issues` with a `query` on the title words and the team.
  Update it in place rather than creating a second one.
- The PR number, base branch and whether it is stacked on another open PR.
- Team and state. GiveWP feature and perf work goes in **Software Team** (`SOFT-*`). Support
  intake lives in Software Maintenance Team (`SMTNC-*`), security in `SVUL`. State is
  `Code Review` when the PR is already open, otherwise `In Progress` or `Backlog`. Assign Jon.
  Project only if Jon named one or a sibling ticket has it.

## Shape

```markdown
One or two short paragraphs. What was wrong or missing for the person using the site, and
what is true now. Plain words. If a one-time update or a visible side effect comes with it,
say so here in one sentence.

Optional second paragraph when part of the audience is excluded or unaffected (an add-on that
keeps the old behaviour, a setting that has to be on).

**How to check**

1. Numbered steps a QA person can follow without opening the code.
2. Each step names the screen, the action, and what they should see.
3. Three to five steps. No test commands here; those go under Technical details if at all.

**Technical details**

* Bullets for engineers. Class and function names, query shapes, migrations, filters, why the
  fix is where it is. Measurements live here, one bullet, before and after.
* Three to six bullets. Say what changed and why, not the history of finding it.

**Pull requests**

* #1234 (base: `develop`)
* #1235 (base: `feature/first-branch`)   <- one line per PR, stacked order
```

## Rules

- **The top is non-technical.** Say "the Donations screen", "campaign totals", "the Reports
  page", not class names or table names. If a sentence above Technical details needs backticks,
  move it down.
- **Length.** Description one or two paragraphs. How to check three to five steps. Technical
  details three to six bullets. If it is longer, cut.
- **Voice.** Plain, present tense, no hedging, no marketing, no emoji. Contractions are fine.
- **Numbers.** One sentence of scale at the top is fine ("minutes to under a second"). Exact
  measurements go in one Technical details bullet. No tables.
- **No process narration.** Nothing about how the bug was found, what CodeRabbit said, or what
  was tried first.
- **Title.** A short sentence fragment describing the outcome, no type prefix (that is for the
  PR). "Fast donations list table on very large sites", not "Enhancement: ...".
- **Stacked PRs.** One ticket per PR. Each later ticket gets `blockedBy` the one below it, and
  its first paragraph says it builds on the earlier change in plain words.

## Publishing

Use the Linear MCP tools:

- Create: `save_issue` with `team`, `title`, `description`, `assignee: "me"`, `state`, and
  `links: [{url: <PR url>, title: <PR title>}]` so the PR shows as an attachment.
- Update: `save_issue` with `id` and `description` (or `patch` for a small change).
- Stack: a second `save_issue` with `id` and `blockedBy: ["SOFT-…"]` once the lower ticket exists.
- Pass real newlines in `description`, not `\n` escapes.
- Put the ticket key back in the PR body (`Resolves SOFT-1234.`) with the `pr` skill if the PR
  does not have it yet.

## Example

```markdown
On sites with hundreds of thousands of donations, the Donations screen could take minutes to
load a page. After this change the same pages load in under a second. Small sites see no
difference in what the screen shows, just faster loads.

A one-time database update runs when the site upgrades. On a very large site it takes about a
minute and runs in the background.

**How to check**

1. Open Donations on a site with a lot of donations. Page one and the last page each load in about a second.
2. Filter by status and search by name and by email. Results match what the site showed before.
3. Go to Donations > Tools > Data. The update named "Add composite indexes to the donation meta table" shows as complete.

**Technical details**

* The list pages on donation IDs first, joining only the meta the active filters and sort need, then hydrates that page.
* Migration `AddIndexesToDonationMetaTable` replaces the single-column `give_donationmeta` indexes with `(donation_id, meta_key(191))` and `(meta_key(191), meta_value(191))`.
* Measured at one million donations: page one 118 s to 0.28 s, last page 162 s to 0.49 s.

**Pull requests**

* #8339 (base: `fix/donations-list-mode-filter-grouping`)
```
