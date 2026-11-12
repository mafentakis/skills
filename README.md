# skills

Personal agent skills for Claude Code.

| Skill | What it does |
|---|---|
| [interview](interview/SKILL.md) | Interviews you until shared understanding is reached before implementing. Checks the request against the project vocabulary, maps a design tree, asks in rounds with a progress line, scales its effort (`effort=low\|med\|high\|max`), splits "confirm as built" from "changes to build", and offers a consistency check with reworded questions at the end. Uses `glossary`, `ask` and `decision-log` (with a short fallback when one is missing). |
| [glossary](glossary/SKILL.md) | Checks a request or a text (spec, PR, docs page) against the project's glossary (`CONTEXT.md` …), rewrites differing words, and keeps the glossary current with settled terms and _Avoid_ aliases. |
| [ask](ask/SKILL.md) | How to ask for decisions: context and 2–4 options with a justified recommendation, the question tool or its fallbacks (widget, numbered list), grouping, and reading answers ("explain more", qualifiers, extra requests). |
| [decision-log](decision-log/SKILL.md) | Records decisions with origin (`[USER]`, `[AGENT · DATA]`, `[ASSUMPTION]`) and date, keeps superseded ones traceable, and turns "changes to build" into one issue each. |
| [gloss](gloss/SKILL.md) | Reference gloss: every reference (issue/PR, file, section, branch, port, earlier decision) gets a short summary, `#98 – Fleet incident for shared Problem pattern`. `/gloss` re-explains the last output in that style; `/gloss persist` writes the rule into AGENTS.md / CLAUDE.md. Runs only when you type `/gloss` (`disable-model-invocation`), so its description costs no context. |

## Agents

| Agent | What it does |
|---|---|
| [status-reporter](agents/status-reporter.md) | Read-only (haiku): open PRs/issues and CI, worktrees and dev servers, running agents, and a "review/test next" recommendation, written in the `gloss` style. Copy to `~/.claude/agents/`. |

## Install

Copy the skill folders into `~/.claude/skills/` (`interview` uses the other three):

```bash
git clone https://github.com/mafentakis/skills.git
cp -r skills/interview skills/glossary skills/ask skills/decision-log skills/gloss ~/.claude/skills/
```

## An older copy on claude.ai

If you also uploaded `interview` to claude.ai (Settings → Skills), Claude Code shows it as `anthropic-skills:interview` next to this one, with an older description. Update it there with this version or remove it, so `/interview` always runs the same skill.
