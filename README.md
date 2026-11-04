# skills

Personal agent skills for Claude Code.

| Skill | What it does |
|---|---|
| [interview](interview/SKILL.md) | Interviews you until shared understanding is reached before implementing. Corrects the request against the project vocabulary first, keeps the glossary up to date, scales its effort (`effort=low\|med\|high\|max`), and falls back to a widget or numbered list when no question tool is available. |

## Install

Copy a skill folder into `~/.claude/skills/`:

```bash
git clone https://github.com/mafentakis/skills.git
cp -r skills/interview ~/.claude/skills/
```
