---
name: interview
description: Interview the user until shared understanding is reached (confidence >90%, no unresolved material branch) before implementing. Corrects the request against the project vocabulary first and keeps the glossary up to date. Maps the task as a design tree, asks frontier questions in rounds, shows a one-line progress bar each round.
---

# Interview

Interview the user until confidence is above 90% and no open decision could materially change the outcome. Ask only what matters; never repeat answered questions or pad the interview.

Work fast: keep the decision tree in your head, not on the page. Don't narrate your reasoning, format choices or tree updates.

## Start

If the request already shows the goal and the user's level, go to Round 0. Only if it is genuinely unclear what kind of help is wanted, ask one calibrating question first (e.g. "A rough plan, a full spec, or one specific decision?"). Verify facts yourself (files, docs, code) instead of asking the user.

## Effort

Every run has a level: `low`, `med`, `high` or `max`. The level controls the interview's research depth, its question budget and the session's reasoning effort. The confidence threshold is 90 % on every level.

| Level | Research before asking | Rounds | Session effort |
|---|---|---|---|
| low | only files the user named | 1 | low |
| med | targeted search | ≤3 | medium |
| high | code, docs, ADRs | ≤6 | high |
| max | also git history and scenario stress-tests | no limit | max |

- **Choosing.** An argument `effort=<level>` always wins. Without one, estimate the level from: the number of open high-impact decisions, how hard they are to reverse, how many modules or files are involved, and how many terms are new. If you can't tell, use `med`. Show the level in Round 0 as one line with the reason, e.g. `Aufwand: high, weil 3 schwer umkehrbare Entscheidungen. (effort=… zum Ändern)`. The user may override it at any time.
- **Session effort.** If a tool for setting the session's reasoning effort exists (e.g. `set_session_effort` in the desktop app), note the current effort and set it to the level's value. The user approves each change. If no such tool exists, say once that only research and question budget follow the level.
- **Raising.** Raise the level automatically when the run turns out harder than estimated: contradictions, many new terms, a wider scope than estimated, or the round limit reached with confidence still under 90 %. Note the change in the recap (`Aufwand: med → high, weil …`) and update the session effort. Never lower a level automatically.

## Round 0: Vocabulary

Goal: the user gets used to the project's vocabulary, and the glossary stays current.

1. **Find the glossary.** In this order: `CONTEXT-MAP.md` (then the context the topic belongs to), `CONTEXT.md`, `GLOSSARY.md`, `UBIQUITOUS_LANGUAGE.md`. Check the repo root and the module the request is about. If none exists, create a root `CONTEXT.md` only when the first term is settled.
2. **Check the request.** Find words that differ from the glossary: synonyms, terms listed as _Avoid_, vague or overloaded words.
   - Clear match → replace it.
   - A word that fits several entries → put it as an option question in Round 0 (options = the matching glossary terms).
   - A term that's new to the project → don't define it here. Add it as a decision to the design tree (see Glossary upkeep).
3. **Show the corrected request.** Show only the rewritten request, with each replaced term in **bold**. No extra table. If nothing differs, write one line: `Vokabular passt.`

4. **Show the effort level** (see Effort) as the line under the corrected request.

Round 0 has no progress line. If no questions are needed, it is only the corrected request and the effort line.

## Design tree (internal)

Each decision has: prerequisites, impact (high/medium/low), status (open/blocked/settled). Mark the origin of every settled decision:

- `[USER]` decided or confirmed by the user
- `[AGENT · DATA]` decided by you from verified evidence within delegated scope (name the evidence)
- `[ASSUMPTION]` low-impact default, not confirmed

Evidence can establish constraints, never the user's preferences. Real tradeoffs go to the user unless delegated.

## Rounds

The frontier = open decisions whose prerequisites are settled. Each round:

1. Resolve facts through research where possible, as deep as the effort level allows.
2. Show the progress line (below).
3. Ask the whole frontier (see Asking, 1–4 questions per call, consecutive calls if needed).
4. After all answers, give a 2–3 line recap and start the next round if needed.

Freeze the frontier during a round. If an answer invalidates a pending question, skip it and say so in the recap.

### Progress line (every round, before the questions)

One line, 10 segments (🟩 filled, ⬜ empty, filled = round(confidence ÷ 10)), ending with the current effort level:

`🟩🟩🟩🟩🟩🟩⬜⬜⬜⬜ Runde 2 · 62 % · 5/8 geklärt · 3 offen · high`

Never skip it.

### Questions

- Short header (≤12 chars), body ends with a question mark.
- One or two sentences of context so the user can choose: what is settled, a concrete consequence per option, or a short parallel example. Never a bare question.
- 2–4 distinct options. Put a justified recommendation first, labelled "(Recommended)". No recommendation if the evidence doesn't favor one. Don't smuggle a recommendation into the question wording.
- Use glossary terms in questions and options.
- When a question or option refers to a named anchor in a document (an ID like `O2`, `D3`, `ADR 0011`, a requirement or test-case number), add a compact description of what it means, e.g. `O2 (Undo after Focus)`. Never show a bare ID.
- Don't add "Other" (the tool does). Treat "Other" and notes as new information and update the tree.
- Silence or a timeout is never agreement.

### Asking

Use `AskUserQuestion`. If it isn't available, use the first fallback that works:

1. **A similar question tool** the agent has (any tool that shows options and returns the user's choice).
2. **An HTML widget** (`show_widget` after `read_me`): one block per question with header, context, and options as radio buttons (or checkboxes for multi-select), the recommendation pre-selected, plus a free-text field. One submit button calls `sendPrompt()` with all answers in the format below.
3. **A numbered list** in plain text:

   ```
   **1 · Speicherort** — Wo soll die Datei liegen?
   A) ~/.claude/skills (Recommended) — …
   B) Im Repo — …

   **2 · Format** — …
   A) …  B) …

   Antwort z. B.: `1-A, 2-B` · eigene Antwort: `2: <Text>`
   ```

Parse answers as question number + option letter (`1-A`; `1-AC` for multi-select). A number with free text counts as "Other". If a question is left unanswered, it stays open. Ask for it again in the next round.

### Recap

```
[USER] decisions made
[AGENT · DATA] decision — Evidence: …
[ASSUMPTION] default and why it's fine
Vokabular: X → Y · neu im Glossar: Term
Open: remaining uncertainty · Confidence: Y% → X%
```

Omit empty lines. Report any changed earlier decision and the decisions that must be revisited.

## Glossary upkeep (every round)

- **Keep checking.** Answers, notes and "Other" text are checked against the glossary too. A word that differs goes in the recap's `Vokabular:` line (`X → Y`).
- **New terms are decisions.** A term that's new to the project goes into the design tree as an open decision ("what is it, and how is it different from Y?"). Ask about it like any other decision, using a concrete scenario that tests its boundary when that helps. Write it to the glossary only once it is settled, and list it under `neu im Glossar:` in that recap.
- **Write right away.** Write each settled term to the glossary as soon as it is settled, not in a batch at the end.
- **Avoid list.** When a correction is confirmed, add the rejected word to that entry as an alias to avoid (`_Avoid_` or the file's equivalent).
- **Format.** Follow the existing file's format, including any fields it already has (e.g. a `**Model:**` line). In a newly created `CONTEXT.md` (format of the `domain-modeling` skill): domain terms only, one or two sentences each, no implementation details or class names.
- **Cross-check the code.** If something the user says about the domain contradicts the code (model names, behavior), ask about it as a question. Don't silently pick one side.
- **ADRs sparingly.** Offer an ADR only when a decision is hard to reverse, surprising without context, and the result of a real tradeoff. Use the repo's ADR folder and format (e.g. `docs/adr/`), or create `docs/adr/` when the first ADR is needed.

## Confidence

Your honest 0–100% estimate that you could act on the user's intent without guessing about anything material. Weight by impact, not by share of questions answered.

- Any unresolved high-impact decision caps it at 90%.
- Contradictions or missing facts block completion.
- Surprising answers can lower it. A finished round alone never raises it.

## Finish

Stop when confidence >90%, no material open branch remains, and low-impact assumptions are explicit. Show the final progress line, then a short plain-text summary: goal and success criteria, settled decisions by branch with origin markers, evidence for each `[AGENT · DATA]` decision, remaining assumptions, exclusions, and glossary changes made during the interview. If you changed the session effort, set it back to the value it had before the interview, so implementation runs at the user's own setting. Ask the user to confirm or correct before implementation (unless they already said to proceed). Corrections that reopen material branches resume the interview.

## Tone

Warm, direct, no cheerleading. Treat the user as a capable adult. If an answer is weak or contradicts something settled, say so kindly and specifically. If you're unsure, say so.
