---
name: glossary
description: Check a request or text against the project glossary (CONTEXT.md and similar), rewrite words that differ, keep the glossary current.
---

# Glossary

Keep the project's words consistent: the text uses the glossary's terms, and the glossary knows every term that was settled.

## Find the glossary

In this order: `CONTEXT-MAP.md` (then the context the topic belongs to), `CONTEXT.md`, `GLOSSARY.md`, `UBIQUITOUS_LANGUAGE.md`. Check the repo root and the module the text is about. If none exists, create a root `CONTEXT.md` only when the first term is settled.

## Check a text

Find words that differ from the glossary: synonyms, terms listed as _Avoid_, vague or overloaded words.

- **Clear match** → replace it.
- **A word that fits several entries** → ask which one is meant (options = the matching glossary terms).
- **A term that's new to the project** → don't define it on the spot. It is an open decision: what is it, and how is it different from the nearest existing term? A concrete scenario that tests its boundary helps.

Show the rewritten text with each replaced term in **bold**, no extra table. If nothing differs, say so in one line, in the conversation's language (e.g. "Vocabulary fits." / "Vokabular passt.").

## Keep the glossary current

- **Write when settled.** Write a term to the glossary as soon as it is settled, not in a batch at the end, and name it in the reply ("new in the glossary: Term").
- **Avoid list.** When a correction is confirmed, add the rejected word to that entry as an alias to avoid (`_Avoid_` or the file's equivalent).
- **Format.** Follow the existing file's format, including fields it already has (e.g. a `**Model:**` line). In a newly created `CONTEXT.md`: domain terms only, one or two sentences each, no implementation details or class names.
- **Cross-check the code.** If the text or the user says something about the domain that contradicts the code (model names, behaviour), ask about it. Don't silently pick one side.
- **Keep checking.** Answers and notes during a conversation are checked too; a word that differs is reported as `X → Y`.
