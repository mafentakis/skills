# skills

Personal agent skills for Claude Code.

| Skill | What it does |
|---|---|
| [interview](interview/SKILL.md) | Interviews you until shared understanding is reached before implementing. Corrects the request against the project vocabulary first, keeps the glossary up to date, scales its effort (`effort=low\|med\|high\|max`), and falls back to a widget or numbered list when no question tool is available. |
| [gloss](gloss/SKILL.md) | Reference gloss: every reference (issue/PR, file, section, branch, port, earlier decision) gets a short summary, `#98 – Fleet incident for shared Problem pattern`. `/gloss` re-explains the last output in that style; `/gloss persist` writes the rule into AGENTS.md / CLAUDE.md. |

## Agents

| Agent | What it does |
|---|---|
| [status-reporter](agents/status-reporter.md) | Read-only (haiku): open PRs/issues and CI, worktrees and dev servers, running agents, and a "review/test next" recommendation, written in the `gloss` style. Copy to `~/.claude/agents/`. |

## Install

Copy a skill folder into `~/.claude/skills/`:

```bash
git clone https://github.com/mafentakis/skills.git
cp -r skills/interview ~/.claude/skills/
```
