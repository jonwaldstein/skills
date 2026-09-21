---
description: Write or rewrite a GiveWP changelog entry (changelog/*.yaml) in Jon's voice - one plain sentence about the outcome for a site owner, never the implementation. Use for "add a changelog entry", "changelog", "write the changelog", "trim the changelog", or /changelog.
---

# Changelog entry

Write the line a site owner reads on WordPress.org before clicking Update. They do not know
what a listener, a repository or a query is, and they do not care. They want to know what got
better and whether it affects them. One sentence, plain words, outcome first.

## Before writing

Gather, do not guess:

- The diff or PR body, until you can say what changed without a class name.
- The PR type prefix (`Feature:`, `Enhancement:`, `Fix:`, `Security:`). The changelog `type` is
  the same word, lowercased. Never `tweak`.
- Whether an entry already exists in `changelog/` for this branch. Rewrite it in place instead
  of adding a second file.
- A PR that does not add or change production code (tests, docs, chores) gets no entry.

## Shape

One YAML file in `changelog/`, named after the branch:

```yaml
significance: patch          # patch, minor (new feature), major (breaking)
type: enhancement            # feature, enhancement, fix, security
entry: Improved donation processing speed by skipping unnecessary offline donation email checks
timestamp: 2026-09-21T14:00:00.000Z
```

Create it with the CLI so the filename and timestamp match the repo convention:

```
npx --no-install changelogger add --auto-filename -s patch -t enhancement -e "<entry>"
```

## Rules

- **One sentence.** No period at the end. No second clause explaining how. If the sentence
  needs "by", "using", "via" or "through" to make sense, you are describing the mechanism; cut
  it or replace it with the effect.
- **Outcome, not implementation.** Say what the site owner or donor sees, or what stops
  happening. "Donations process faster", "the Donations screen loads in about a second",
  "recurring donors no longer get a duplicate receipt". Never a class, hook, table, function,
  query or file name. Never "legacy", "listener", "repository", "meta", "model", "index".
- **Name the screen or feature the way the admin UI does.** "the Donations screen", "the form
  builder", "campaign totals", "the donor dashboard", "PayPal Donations", "Stripe".
- **Verb first, past tense, the way the compiled changelog reads.** `Fix:` entries start with
  "Fixed" or "Resolved an issue where". `Enhancement:` entries start with "Improved", "Added",
  or the thing that now happens ("Donation forms now show a loading state"). `Feature:`
  entries start with "Added". `Security:` entries say "Added additional validation to..." or
  "Hardened..." and never describe the exploit.
- **No numbers unless a site owner would notice them.** "minutes to under a second" is fine.
  "20 fewer queries" is not.
- **Scope in plain words when it matters.** "on very large sites", "when the offline gateway is
  enabled", "in the visual form builder". Otherwise leave it out.
- **Nothing about the process.** Not how it was found, not what CodeRabbit said, not which
  ticket. The ticket key goes in the PR body, not here.
- **Security entries** are deliberately vague. Follow the existing pattern in `readme.txt`
  and never name the attack, the parameter or the file.

## Examples

Before and after, from real entries:

| Too technical | Entry |
|:--|:--|
| Offline donation email listeners now check the gateway before loading the legacy payment object, saving about 20 queries on every non-offline donation | Improved donation processing speed by skipping unnecessary offline donation email checks |
| Replaced the axios HTTP client with WordPress core's apiFetch in the donor dashboard, reports, onboarding wizard, and the log and migration list tables | Improved reliability of the donor dashboard, reports and onboarding wizard |
| The donations list pages on donation IDs first and joins only the meta the active filters need | The Donations screen now loads in about a second on sites with hundreds of thousands of donations |
| Fixed the revenue table index migration adding duplicate indexes when it runs more than once | Fixed a database update that could run more than once and slow down the Donations screen |

Good as written:

- `Fixed a campaign's default donation form appearing unpublished in the form builder`
- `Donation form embeds now show a loading state while the form loads`
- `Added the ability to embed donation forms on any website with a copy-paste snippet from the form builder`
- `Resolved an issue where resuming a paused Stripe subscription triggered a fatal error`

## Publishing

- Write the file, then show the `entry` line in the reply so it can be checked at a glance.
- Commit it with the change it describes, in the same PR. Use `Chore:` if it is the only thing
  in the commit.
