# Domain docs

## Layout and reading rules

This repository uses a single-context layout:
- `CONTEXT.md` at the repository root holds domain terminology.
- `docs/adr/` holds architectural decisions.

Before exploring the codebase, read CONTEXT.md and ADRs relevant
to the area of work.

If these files are absent, proceed silently. The domain-modeling
skill creates them when terms or decisions are resolved.

## Vocabulary

Use the terms defined in CONTEXT.md when naming domain concepts
in issues, proposals, hypotheses, and tests.

If a needed concept is missing, reconsider the terminology or
note the gap for domain-modeling.

## ADR conflicts

Explicitly flag proposals that contradict an existing ADR,
identify the ADR, and explain why the decision merits reopening.
