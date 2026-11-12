---
name: interview
description: Interview the user until shared understanding (>90 % confidence) before implementing – vocabulary check, question rounds, a progress line each round.
---

# Interview

Interview the user until confidence is above 90% and no open decision could materially change the outcome. Ask only what matters; never repeat answered questions or pad the interview.

Work fast: keep the decision tree in your head, not on the page. Don't narrate your reasoning, format choices or tree updates.

**Uses three skills:** `glossary` (vocabulary), `ask` (how to ask and read answers), `decision-log` (origins, superseded decisions, built vs. change). If one isn't installed, use the one-line fallback given where it is referenced.

**Language:** all fixed labels (progress line, recap, notes) follow the conversation's language. The examples below show English with German in brackets.

## Start

If the request already shows the goal and the user's level, go to Round 0. Only if it is genuinely unclear what kind of help is wanted, ask one calibrating question first (e.g. "A rough plan, a full spec, or one specific decision?"). Verify facts yourself (files, docs, code) instead of asking the user.

## Effort

Every run has a level: `low`, `med`, `high` or `max`. It controls research depth, the question budget and the session's reasoning effort. The confidence threshold is 90 % on every level.

| Level | Research before asking | Rounds | Session effort |
|---|---|---|---|
| low | only files the user named | 1 | low |
| med | targeted search | ≤3 | medium |
| high | code, docs, ADRs | ≤6 | high |
| max | also git history and scenario stress-tests | no limit | max |

- **Choosing.** An argument `effort=<level>` always wins. Without one, estimate it from the number of open high-impact decisions, how hard they are to reverse, how many modules or files are involved, and how many terms are new; if unsure, `med`. Show it in Round 0 as one line with the reason, e.g. `Effort: high, because 3 hard-to-reverse decisions. (effort=… to change)` (`Aufwand: high, weil …`). The user may override it at any time.
- **Session effort.** If a tool for the session's reasoning effort exists (e.g. `set_session_effort`), note the current effort and set the level's value; the user approves each change. If no such tool exists, say **once per session** (not every run) that only research and question budget follow the level.
- **Raising.** Raise the level automatically when the run is harder than estimated (contradictions, many new terms, wider scope, or the round limit reached under 90 %). Note it in the recap (`Effort: med → high, because …`) and update the session effort. Never lower it automatically.

## Round 0: Vocabulary

Check the request against the project's glossary with the **`glossary` skill**, and show the corrected request with replaced terms in **bold** (or one line "Vocabulary fits." / "Vokabular passt."). New terms become decisions in the design tree. Then the effort line. Round 0 has no progress line.
*Fallback without `glossary`:* look for `CONTEXT.md` / `GLOSSARY.md`, replace clear synonyms, ask about ambiguous ones, write settled terms back.

## Design tree (internal)

Each decision has prerequisites, impact (high/medium/low) and status (open/blocked/settled). Mark the origin of every settled decision with the **`decision-log` skill** (`[USER]`, `[AGENT · DATA]` with evidence, `[ASSUMPTION]`; superseded decisions kept with time).
*Fallback without `decision-log`:* mark [USER] / [AGENT · DATA] + evidence / [ASSUMPTION]; when a decision changes, say which one it replaces.

With many decisions (more than about 8), group them by area (feature, issue, file) and work area by area.

## Rounds

The frontier = open decisions whose prerequisites are settled. Each round:

1. Resolve facts through research where possible, as deep as the effort level allows.
2. Show the progress line.
3. Ask the whole frontier with the **`ask` skill** (1–4 questions per call, consecutive calls if needed). It also covers: verifying examples before using them, "explain more" answers (explain, then re-ask), answers with a qualifier or an extra request.
   *Fallback without `ask`:* `AskUserQuestion` with 2–4 options and a justified "(Recommended)" first; else a numbered list (`1-A, 2-B`). "Explain more" means: explain with an example, then ask the same question again.
4. After all answers, give a 2–3 line recap (format in `decision-log`) and start the next round if needed.

Freeze the frontier during a round. If an answer invalidates a pending question, skip it and say so in the recap. Glossary upkeep (`glossary`) runs on answers and notes too.

### Progress line (every round, before the questions)

One line, 10 segments (🟩 filled, ⬜ empty, filled = round(confidence ÷ 10)), ending with the effort level, in the conversation's language:

`🟩🟩🟩🟩🟩🟩⬜⬜⬜⬜ Round 2 · 62 % · 5/8 settled · 3 open · high` (`Runde 2 · 62 % · 5/8 geklärt · 3 offen · high`)

Never skip it.

### New facts during the interview

Results that arrive while questions are open (a test, a measurement, a background agent's report) are checked against the settled decisions before Finish. If one bears on a decision, say so and offer to re-open it (`decision-log`, New facts).

## Confidence

Your honest 0–100 % estimate that you could act on the user's intent without guessing about anything material. Weight by impact, not by share of questions answered.

- Any unresolved high-impact decision caps it at 90 %.
- Contradictions or missing facts block completion.
- Surprising answers can lower it. A finished round alone never raises it.

## Finish

Stop when confidence >90 %, no material open branch remains, and low-impact assumptions are explicit. Show the final progress line, then a short plain-text summary:

- goal and success criteria;
- settled decisions by branch with origin markers, evidence for each `[AGENT · DATA]`;
- when decisions concern something already built: **confirm as built (docs only)** vs. **changes to build**, with one work item (issue) offered per change (`decision-log`);
- new tasks from answers with an extra request;
- remaining assumptions, exclusions, glossary changes.

If you changed the session effort, set it back. Ask the user to confirm or correct before implementation (unless they already said to proceed), and offer the consistency check in one line: "Check the answers with reworded questions? (check)" (`Antworten mit umformulierten Fragen prüfen? (check)`). Corrections that reopen material branches resume the interview.

## Consistency check (on request)

Runs when the user accepts the offer, passes `check`, or asks to check their answers.

1. Ask every settled decision again, **reworded as a concrete situation** ("It is 14:03:20 and you click 'Last hour'. Where should the window end?" instead of "Which end of a quick range should hold?").
2. **No "(Recommended)" labels, shuffled option order**, different grouping than before, so neither the earlier answer nor a recommendation steers the check.
3. Progress line with a check label: `Check 2 · 4/14 checked` (`Prüfrunde 2 · 4/14 geprüft`).
4. Then a table: decision · first answer · check answer · ✓/✗.
5. For every ✗: say which earlier answer it contradicts and resume the interview on that branch until both agree. All ✓: the summary stands.

## Tone

Warm, direct, no cheerleading. Treat the user as a capable adult. If an answer is weak or contradicts something settled, say so kindly and specifically. If you're unsure, say so.
