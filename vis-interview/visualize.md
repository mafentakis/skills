# Visualize: annotated diagrams

How to show a diagram during an interview round, picked from the repo or generated from the code, with the change a question is about highlighted.

## Find a source

1. `git grep -l '```mermaid'` across the repo; also check `CONTEXT.md`, `CONTEXT-MAP.md` and the ADR folder (e.g. `docs/adr/`).
2. Pick the block whose nodes, participants or states match the flow or module the frontier question touches.
3. No matching diagram: generate one (next section). Never invent a "current state" you haven't verified in code or docs.

## Generate from code

When the user picks "Generate from code" or no diagram exists:

1. Read the code that the question touches: entry point, the calls it makes, and the stores or tables it writes. Stay inside what the effort level allows.
2. Pick the type:
   - `sequenceDiagram` for calls between parts in order;
   - `flowchart LR` for wiring and data flow;
   - `stateDiagram-v2` for UI or lifecycle states.
3. Use real names from the code (class, component, endpoint, table). 6–12 nodes. Leave out logging, mapping and other glue.
4. Draw the current state first. Then annotate the change as below, never mixed in unmarked.

## Prepare the source

- Copy the block as-is, then clean it:
  - Quote every label: `A["…"]` and `-->|"…"|`. Brackets, slashes or pipes inside a label (`[/smart-search/text/]`, `|[data]|`) break the parser otherwise.
  - Drop emoji.
  - Keep `<br/>`.
- Trim it to the part the question is about. 6–12 nodes is the sweet spot.

## Annotate inside the Mermaid source

Never draw overlay arrows on the rendered SVG: node coordinates aren't known before render.

**flowchart / graph / stateDiagram:**

```
classDef neu stroke:#1D9E75,stroke-width:2px,stroke-dasharray:5 3,fill:#1D9E7522
classDef geaendert stroke:#BA7517,stroke-width:2px,fill:#BA751722
classDef entfaellt stroke:#E24B4A,stroke-width:2px,stroke-dasharray:2 3,fill:#E24B4A14,color:#888780
class NewNode neu
class ChangedNode geaendert
class OldNode entfaellt
```

- New edge: `A -.->|"neu: …"| B`.
- Changed edge: color it with `linkStyle <index> stroke:#BA7517,stroke-width:2px` (index = 0-based order of edges in the source).
- Removed edge: `linkStyle <index> stroke:#E24B4A,stroke-dasharray:2 3`.

**sequenceDiagram** (no classDef):

```
rect rgba(29,158,117,0.15)
  A->>B: neuer Schritt
end
rect rgba(186,117,23,0.15)
  B->>C: geänderter Schritt
end
Note over A,B: neu: …
```

## Widget template

Call `read_me` (module `diagram`) once per session before the first `show_widget`. Keep the init block exactly like this: font settings drive layout measurement.

```html
<h2 class="sr-only">ONE-SENTENCE SUMMARY</h2>
<div id="d"></div>
<div style="display:flex;gap:16px;margin-top:8px;font-size:12px;color:var(--text-secondary)">
  <span style="display:flex;align-items:center;gap:4px"><span style="width:14px;height:10px;border:2px dashed #1D9E75;border-radius:2px"></span>neu</span>
  <span style="display:flex;align-items:center;gap:4px"><span style="width:14px;height:10px;border:2px solid #BA7517;border-radius:2px"></span>geändert</span>
  <span style="display:flex;align-items:center;gap:4px"><span style="width:14px;height:10px;border:2px dotted #E24B4A;border-radius:2px"></span>entfällt</span>
</div>
<script type="module">
import mermaid from 'https://esm.sh/mermaid@11/dist/mermaid.esm.min.mjs';
const m = document.documentElement.dataset.mode;
const dark = m ? m === 'dark' : matchMedia('(prefers-color-scheme: dark)').matches;
await document.fonts.ready;
mermaid.initialize({ startOnLoad: false, theme: 'base', fontFamily: '"anthropic-sans", sans-serif',
  themeVariables: { darkMode: dark, fontSize: '13px', fontFamily: '"anthropic-sans", sans-serif',
    lineColor: dark ? '#9c9a92' : '#73726c', textColor: dark ? '#c2c0b6' : '#3d3d3a',
    primaryColor: dark ? '#2c2c2a' : '#f1efe8', primaryBorderColor: dark ? '#5f5e5a' : '#b4b2a9' } });
const src = `MERMAID SOURCE`;
try { document.getElementById('d').innerHTML = (await mermaid.render('d-svg', src)).svg; }
catch (err) { document.getElementById('d').innerHTML = '<pre style="font-size:12px;color:var(--text-danger);white-space:pre-wrap">' + String(err.message || err).replace(/</g,'&lt;') + '</pre>'; }
</script>
```

- Escape backticks and `${` inside `src`.
- Always show all three legend items, even when the diagram uses fewer, so every diagram reads the same.
- Make a node clickable: add `click NodeId call sendPrompt("…")` with `securityLevel: 'loose'`. Only do this when a follow-up question really helps.
