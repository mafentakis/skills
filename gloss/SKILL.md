---
name: gloss
description: Reference gloss — write every reference (issue/PR number, file, document section, branch, port, status code, earlier session decision) with a short summary next to it, "#98 – Fleet incident for shared Problem pattern", never a bare ID. `/gloss` re-explains the last assistant message (or the last N with `/gloss N`) in this style; `/gloss persist` writes the style rule into the project's AGENTS.md / CLAUDE.md. Use when the user finds agent output full of unexplained references.
---

# Gloss

Make references readable at a glance: **reference – 2–6 word summary**.

## The style

- **Format:** `reference – summary`, with an en dash. The summary says what the thing *is or does*, in 2–6 words. No status word unless the status is the point (`blocked`, `CI failing`, `waiting for you`).
- **Every reference, every time it first appears in a message.** Repeats in the same message may stay short.
- **Never a bare ID.** Not `#98`, not `ELK-INDEXES.md`, not `Round 2`.
- **Verify before glossing.** Look up the real title, heading or content (`gh issue view`, read the file, re-read the session). A wrong summary is worse than none; if you can't verify, write `?` instead of guessing.
- **Don't pad.** Plain words stay plain. Gloss only things the reader would have to look up.

| Type | Example |
|---|---|
| a) GitHub issue / PR | `#98 – Fleet incident for shared Problem pattern` · `PR #100 – start-server project skill` |
| b) Document section | `ELK-INDEXES.md § Which index holds what – field lookup per locker` · `app/AGENTS.md – agent rules for the app` |
| c) Session section | `Round 1 · Ref format – dash style, no status` · `Agent scope – user-scope agent, style embedded` |
| d) Branch / port / code | `feature/spinner-74 – spinner fix worktree` · `:8101 – dev server for #89` · `HTTP 503 – health check failing` |

## `/gloss` — explain the last output

Input: the last assistant message. `/gloss N` takes the last N messages.

1. Find every reference in it (IDs, files, sections, branches, ports, codes, earlier decisions).
2. Look each one up (see *Verify*).
3. Re-output the same content with each reference glossed. Keep the original wording and order; add nothing new and cut nothing. If a reference can't be verified, mark it `?` and say so in one line at the end.

## `/gloss persist` — keep the style

Writes the rule below into the instructions so every later answer follows it.

1. **Choose the target.** Default proposal: the project's `AGENTS.md` if it exists (nearest to the working directory), else `CLAUDE.md`, else offer to create `AGENTS.md`. `persist=user` targets `~/.claude/CLAUDE.md`. Ask which one; the user may name another.
2. **Show the diff, wait for OK, then write.** If a gloss rule is already there, update it in place instead of adding a second one.
3. **Rule text** (keep it this short):

   ```markdown
   ## Reference gloss
   Write every reference (issue/PR number, file, document section, branch, port, status code, earlier decision)
   as `reference – 2–6 word summary`, never a bare ID. Verify the summary against the source first. Add a status
   word only when the status is the point.
   ```
4. Subagent prompts don't inherit this file's content automatically: when delegating, paste the rule into the agent's instructions.

## Notes

- Works with the `ask` skill: its "named anchor gets a compact description" rule is the same idea, in parentheses form.
- Language of the summaries follows the conversation; references stay as written.
