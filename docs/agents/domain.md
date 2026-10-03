# Domain docs

This repository uses a single context. Domain knowledge lives in `CONTEXT.md` at the root; architectural decision records live in `docs/adr/`.

## Before exploring

Read `CONTEXT.md` if it exists, then any ADRs relevant to the area being discussed or changed.

If these files do not exist, proceed silently. Do not create placeholder domain documents. The domain-modeling skill creates them as terminology and decisions are resolved with the project owner.

## Vocabulary

Use the terms defined in `CONTEXT.md` in issue titles, proposals, code, and tests. Avoid synonyms the glossary explicitly rejects.

If a needed concept is missing, establish whether it is a real domain concept before adding it through domain-modeling.

## Decision records

If a proposal contradicts an existing ADR, identify the conflict and explain why the decision may need revisiting. Do not silently override an existing decision.

Keep Wayfinder decision details in their GitHub tickets. Domain documents can capture the resulting vocabulary and architectural decisions, with links back to the relevant issues.
