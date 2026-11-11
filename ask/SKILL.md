---
name: ask
description: How to ask the user for decisions: question format with context and 2–4 options, a justified recommendation, the question tool or its fallbacks, grouping many questions, and how to read answers (explain-more requests, qualifiers, extra requests, "Other"). Used by the interview skill; use it whenever a skill needs the user to choose.
---

# Ask

Ask so the user can decide in one look, and read the answer completely.

## Before asking

- **Verify facts yourself** (files, docs, code, data) instead of asking the user.
- **Verify every example, number and file reference** you put into a question or an option against the code or data first. If one turns out wrong after the user answered, say so, correct it, and ask that question again: the answer may change.
- **Unfamiliar mechanism?** Put a concrete example into the question up front (a small before/after, a sequence, real measured numbers), not only a definition.

## A question

- Short header (≤12 chars); the body ends with a question mark.
- One or two sentences of context: what is settled, a concrete consequence per option, or a short parallel example. Never a bare question.
- 2–4 distinct options. Put a justified recommendation first, labelled "(Recommended)". No recommendation if the evidence doesn't favour one. Don't smuggle a recommendation into the wording.
- Use the glossary's terms (see the `glossary` skill).
- A named anchor (an ID like `O2`, `D3`, `ADR 0011`, an issue or requirement number) gets a compact description: `O2 (Undo after Focus)`, `#36 Log table`. Never a bare ID.
- Don't add "Other" (the tool does).

## Many questions

- At most 4 per call; consecutive calls when there are more.
- Group by area (feature, issue, file) and put the area into the header or the first words (`#36 Log table: …`).

## Asking

Use `AskUserQuestion`. If it isn't available, use the first fallback that works:

1. **A similar question tool** (any tool that shows options and returns the user's choice).
2. **An HTML widget** (`show_widget` after `read_me`): one block per question with header, context and options as radio buttons (checkboxes for multi-select), the recommendation pre-selected, plus a free-text field. One submit button calls `sendPrompt()` with all answers in the format below.
3. **A numbered list** in plain text:

   ```
   **1 · Location** — Where should the file live?
   A) ~/.claude/skills (Recommended) — …
   B) In the repo — …

   **2 · Format** — …
   A) …  B) …

   Answer e.g. `1-A, 2-B` · own answer: `2: <text>`
   ```

   Write the list in the conversation's language.

## Reading answers

- **Number + letter** (`1-A`; `1-AC` for multi-select). A number with free text counts as "Other".
- **Unanswered** → stays open; ask it again next time. **Silence or a timeout is never agreement.**
- **"Explain more" / "tell me more" / "explain simpler" / "repeat"** → the decision is *not* settled. Explain (worked example, before/after table, cost in real numbers), then ask the *same* decision again with the same options.
- **An option plus a qualifier** ("1, but the tool may still fetch other sources") → the option, refined by the qualifier; record the refinement with the decision.
- **An option plus an extra request** ("2, and add an issue for me to review …") → the decision, plus a new task; list the task explicitly in the next summary. Never drop the extra part.
- **"Other" and notes** → new information: update what you know, and re-check earlier decisions it touches.
- **A weak answer, or one that contradicts something settled** → say so kindly and specifically, and ask again.
