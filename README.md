# skills

Personal [Claude Code](https://claude.com/claude-code) skills.

| Skill | What it does |
|-------|--------------|
| [pr](pr/SKILL.md) | Write or rewrite a pull request description: brief, upbeat, with QA steps a non-developer can follow. |
| [linear-ticket](linear-ticket/SKILL.md) | Write or rewrite a Linear ticket: plain-language lead for stakeholders, steps to check for QA, a Technical details section for engineers. |
| [changelog](changelog/SKILL.md) | Write a changelog entry: one plain sentence about the outcome for a site owner, never the implementation. |

## Install

Symlink a skill into `~/.claude/skills/`:

```sh
git clone https://github.com/jonwaldstein/skills.git ~/Repos/skills
ln -s ~/Repos/skills/pr ~/.claude/skills/pr
```
