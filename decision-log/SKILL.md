---
name: decision-log
description: Record decisions with their origin ([USER], [AGENT · DATA] with evidence, [ASSUMPTION]) and date, keep superseded decisions traceable, and split "confirm as built" from "changes to build" with one issue per change. Used by the interview skill; also usable alone ("record what we decided in #35–#41", "write the decisions to the plan").
---

# Decision log

Every decision says who made it, on what basis, and whether it still holds.

## Origin markers

- `[USER]` decided or confirmed by the user.
- `[AGENT · DATA]` decided by the agent from verified evidence within delegated scope; name the evidence (file, query, measurement).
- `[ASSUMPTION]` a low-impact default, not confirmed; say why it's fine.

Evidence can establish constraints, never the user's preferences. Real tradeoffs go to the user unless delegated.

## Superseded decisions

When a decision changes, keep both: `[USER] 14:10 "I merge myself" → replaced 15:02 by [USER] "You merge"`. Summaries show only the current one; the record keeps the chain. A [USER] decision that the build or an agent changed without asking is flagged "to confirm" until the user confirms or reverses it.

## New facts

When evidence arrives that bears on a settled decision (a test result, a measurement, a reviewer's finding), surface it before acting on the decision: "this may change what you decided: …". Re-open it if the user wants.

## Built behaviour: confirm or change

When decisions concern something that is already built, split them:

- **Confirm as built** → documentation only (the docs and the plan record the confirmation and its date).
- **Changes to build** → one work item per change (an issue in the project's tracker when there is one), each with the decision, a concrete example, and the tests that prove it.

## Recording

Write decisions where the project keeps them, in the project's format:

- **Issues:** tick the decision's checkbox and append the outcome with date and origin (`→ **Decided 2026-11-08 [USER]:** …`); new work becomes a new checkbox. Don't close an issue that still has open boxes.
- **Plan or spec:** a section per feature with decisions, not in scope, tests; deviations of the build are noted with the decision they changed.
- **Feature or docs pages:** a decisions table (decision · origin · where decided) and a "Replaced" list (old → new).

## Recap format

```
[USER] decisions made
[AGENT · DATA] decision — Evidence: …
[ASSUMPTION] default and why it's fine
Open: remaining uncertainty
```

Omit empty lines. Report any changed earlier decision and the decisions that must be revisited.
