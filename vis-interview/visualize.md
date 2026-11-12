# Visualize: annotated SVG diagrams

How to show a diagram during an interview round as a hand-laid **inline SVG** (no Mermaid rendering), with the change a question is about highlighted. Repo diagrams are only a **source** to read nodes and edges from; the picture is always drawn as SVG.

Call `read_me` (module `diagram`) once per session before the first `show_widget`, and follow its SVG rules: `viewBox="0 0 680 H"`, `role="img"` with `<title>` and `<desc>`, text classes `t` / `ts` / `th`, color classes `c-*`, 0.5px strokes, the arrow marker, sentence case, no overlaps.

## Find a source

1. `git grep -l '```mermaid'` across the repo; also check `CONTEXT.md`, `CONTEXT-MAP.md`, the ADR folder (e.g. `docs/adr/`), and checked-in `.svg` / `.drawio` / `.puml` files.
2. Pick the one whose nodes, participants or states match the flow or module the frontier question touches. Read its nodes and edges; don't render it.
3. Nothing matches: generate from code (next section). Never invent a "current state" you haven't verified in code or docs.

## Generate from code

1. Read the code the question touches: entry point, the calls it makes, the stores or tables it writes. Stay inside what the effort level allows.
2. Pick the shape:
   - **flow**: wiring and data flow, boxes and arrows;
   - **sequence**: calls between parts in order, lanes;
   - **states**: UI or lifecycle states, boxes with transition arrows.
3. Use real names from the code (class, component, endpoint, table). Leave out logging, mapping and other glue.

## Trim before drawing

- **≤8 nodes per SVG.** Keep the nodes the change touches plus one neighbor on each side; drop the rest. A source with more becomes an overview (boxes only, ≤2 arrows) plus one detail SVG for the changed part.
- Labels: title ≤20 chars, subtitle ≤5 words (e.g. the endpoint, the port, the store slice). Long names get shortened in the box and spelled out in the prose.

## Layout

**Flow (left → right or top → down):**
- Columns at x = 40, 200, 360, 520 (box width 120–140, gap ≥ 20) for up to 4 tiers; or rows at y = 40, 140, 240, 340 for top-down.
- Two-line boxes: height 56, title `th` at y+20, subtitle `ts` at y+38, both `text-anchor="middle" dominant-baseline="central"`. One-line boxes: height 44.
- Box width ≥ max(title chars × 8, subtitle chars × 7) + 24.
- Arrows run box edge to box edge. If a straight line would cross another box, route an L-shaped `<path fill="none">` around it.

**Sequence:**
- ≤5 participants as boxes in a row at the top (width 110, centers evenly spaced from x=95 to x=585), dashed lifelines (`class="leader"`) straight down.
- One horizontal arrow per message, 36px apart, label (`ts`, ≤4 words) 6px above the arrow, centered between the two lifelines. Return messages dashed.

**States:** like flow; one box per state, transition labels above the arrows (≤3 words).

**Height:** viewBox H = lowest element + 40 (the legend counts).

## Annotate

| Mark | Box | Arrow |
|---|---|---|
| unchanged | `class="box"` (neutral) | `class="arr"` |
| **neu** | `c-teal`, `stroke-dasharray="5 3"` and `stroke-width="1"` on the rect | `stroke="#1D9E75" stroke-width="1.5" stroke-dasharray="5 3"`, `fill="none"`, arrow marker |
| **geändert** | `c-amber`, `stroke-width="1"` | `stroke="#BA7517" stroke-width="1.5"` |
| **entfällt** | `c-red`, `stroke-dasharray="2 3"`, `opacity="0.6"` | `stroke="#E24B4A" stroke-dasharray="2 3"`, `opacity="0.6"` |

- Mark only what the option changes; everything else stays neutral, so the change is the only colour on the page.
- New arrows may carry a short `ts` label like "neu: GET" placed in clear space, never on the line.
- Pair colour with the dash pattern (accessibility): never colour alone.
- For sequence diagrams, mark a new or changed message by its arrow style, and put a pale `c-teal` / `c-amber` rect (rx 4, behind the arrows) around a block of several new or changed messages.

## Legend (always)

A single row at the bottom, inside the SVG: three small sample rects (16×10, rx 2) with the patterns above and `ts` labels "neu", "geändert", "entfällt", starting at x=40, 100px apart. Always all three, so every diagram reads the same.

## Per option

When the options differ structurally, draw one SVG per option (≤3), same layout and node positions in each, so only the marks change between them. One SVG per `show_widget` call, prose with the option letter between them.

## Clickable nodes

Wrap a box in `<g class="node" onclick="sendPrompt('…')">` only when a follow-up question really helps (e.g. "Was macht `OwnerSearchDrawer` heute?").

## Checklist before showing

- Every box fits its longest label; no two boxes in a row closer than 20px.
- No arrow crosses a box it doesn't connect; no label sits on a line.
- Nothing outside x 0–680 / y 0–H; every `<text>` has a class.
- Every connector `<path>` has `fill="none"`.
