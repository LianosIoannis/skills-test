# Issue tracker: Local Markdown

Issues and specs live in `.scratch/`.

## Conventions

- One feature per directory: `.scratch/<feature-slug>/`.
- Specs: `.scratch/<feature-slug>/spec.md`.
- Tickets: `.scratch/<feature-slug>/issues/<NN>-<slug>.md`,
  numbered from `01`, one file per ticket.
- Record triage state in a `Status:` line near the top.
  Use the role strings in `triage-labels.md`.
- Append conversation under `## Comments`.

When publishing, create the appropriate spec or ticket file and any
missing directories. When fetching a ticket, read its referenced file.

## Wayfinding operations

- Map: `.scratch/<effort>/map.md`, containing Notes,
  Decisions-so-far, and Fog.
- Child tickets: `.scratch/<effort>/issues/NN-<slug>.md`.
  Include the question and a `Type:` line:
  `research`, `prototype`, `grilling`, or `task`.
- Dependencies: `Blocked by: NN, NN`. A ticket is unblocked
  when every listed dependency is resolved.
- Frontier: open, unblocked, unclaimed tickets, lowest number first.
- Claim: save `Status: claimed` before starting work.
- Resolve: append `## Answer`, set `Status: resolved`, and add
  a gist and link to the map's Decisions-so-far section.

Wayfinding uses its own claimed/resolved lifecycle; ordinary triage
uses the roles in `triage-labels.md`.
