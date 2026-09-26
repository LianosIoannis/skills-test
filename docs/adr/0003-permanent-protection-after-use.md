# Protect reusable entities permanently after first reference

The agreed lifecycle makes ever having been referenced the boundary for
permanent deletion, rather than whether references currently exist. A reference
from a log, another reusable entity, or a goal permanently protects its target;
archiving and restoration provide the lifecycle for used entities while keeping
them editable for dynamic history. This trades later cleanup flexibility for a
stable protection rule: removing all references never makes a used entity
eligible for deletion again.

## Consequences

- Current reference counts alone cannot determine deletion eligibility; prior
  use must remain known after references are removed.
- Protection and archival are independent: a protected entity may be active,
  and restoring an archived entity never clears its protection.
- Existing references remain valid, including archived exercises in active
  workouts; new selection excludes archived entities.
- An archived domain additionally blocks new sessions for its courses, while
  existing scoped goals remain active until explicitly ended.

Source: [agreed specification](../../.scratch/habit-tracker/spec.md#reusable-entity-lifecycle).
