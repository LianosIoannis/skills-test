# Preserve goal definitions by calendar period

Although logged history is dynamic, the user wants previous periods judged
against the goal expectations that applied to those periods. A goal's metric,
recurrence, and scope remain fixed; name and target edits take effect from the
current period onward, with only the latest edit retained within that period.
This chooses period-level definition history over either fully dynamic targets
or a separate historical definition for every edit, preserving earlier
expectations without retaining intermediate changes within a period.

## Consequences

- A weekly target changed on Friday applies to that entire Monday-start week,
  including matching logs from before Friday; previous weeks keep their targets.
- Ending a goal excludes the current period onward and preserves earlier
  applicable periods; changing its identity requires a new goal.
- Retaining a historical definition does not freeze its result: dynamic meal
  values, course membership, and edited logs can still change the assessment.

Source: [agreed specification](../../.scratch/habit-tracker/spec.md#definition-history).
