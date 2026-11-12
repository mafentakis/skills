---
name: vis-interview
description: The interview skill plus diagrams – shows a repo diagram (picked or generated) with the change under discussion highlighted, inline in the chat.
---

# Vis-interview

Run the **`interview` skill** exactly as written: rounds, progress line, `ask`, `glossary`, `decision-log`, finish and check. This skill adds one thing: an annotated diagram for questions that change something structural.
*Fallback without `interview`:* tell the user it is missing, then interview in rounds anyway (progress line, 2–4 options with a recommendation, recap), and add the visuals below.

Arguments: everything `interview` takes (e.g. `effort=high`), plus `vis=ask|auto|off`. The default is `ask`.

## When a visual helps

A question is **structural** when its options differ in components, flows, calls, or states. Naming, scope, priority and wording questions get no visual. At most one visual per round.

## Choosing the visual (vis=ask)

Before the first structural question, and again whenever a later one touches a different flow, offer the choice as one extra question in the same `ask` call (header `Grafik` / `Diagram`):

1. **Find candidates.** Use the steps under "Find a source" in `visualize.md`: Mermaid blocks in the repo, `CONTEXT.md`, ADRs. Keep the ≤3 that match the question best.
2. **Options**, in this order:
   - each candidate as `<file>:<line> – <what it shows in ≤6 words>` (first one "(Recommended)" when it clearly fits);
   - **Generate from code**, naming the type you'd draw and why (sequence for calls between parts, flowchart for data and wiring, state diagram for UI or lifecycle states);
   - **No diagram**.
3. **Remember the answer** for that flow for the rest of the interview. Don't ask again for the same flow. A note like "always generate" or "never" applies to all later flows.

With `vis=auto`, skip the question: take the best candidate, or generate one when there is none. With `vis=off`, no visuals, unless the user asks for one ("zeig die Grafik").

## Showing it

Follow `visualize.md` for preparing the source, annotating (neu / geändert / entfällt) and the widget template.

- **Order in a round:** progress line → diagram → questions.
- **Per option:** when the options differ structurally, show one annotated variant per option (≤3), each titled with its option letter in the prose above it.
- **Prose:** name the source (`docs/foo.md:42` or "generated from `FooService.java`, `bar.effects.ts`") and say what the colours mean. No explanatory text inside the widget.
- **Honesty:** a repo diagram that no longer matches the code gets a note, and the annotation follows the code. A generated diagram shows only what you read in the code. Never present an invented flow as the current state.
- **Render errors:** the template prints Mermaid errors in red. Mention it, fix the source once, and render again. If it fails again, fall back to a small SVG or skip the visual.

## Finish

In the summary, list the diagrams that were shown. For each generated one or each repo diagram that turned out stale, offer once in one line to save the updated Mermaid into the docs ("Diagramm in `docs/…` aktualisieren?"). The interview's own confirm/check offers stay as they are.
