# Resolve historical records using current reusable entities

The user wants edits to saved meals, exercises, workouts, courses, and domains
to apply to earlier records as well as future use. We therefore retain live
references rather than snapshots: meal nutrition and course-domain membership
are resolved from current values, and workout logs record completion without
per-log exercise copies or overrides. This deliberately favors consistent
application of saved-entity edits over preserving exactly what was recorded at
the time; past totals and goal results can change, and the original values cannot
be reconstructed from these records alone.

## Consequences

- Changing a 500-calorie meal to 550 changes a historical 1.5-serving log from
  750 to 825 calories.
- Adding an exercise to a workout changes past workout displays, but not the
  number of completed workout logs.
- Moving a course to another domain transfers its historical session duration
  between domain totals and can change earlier scoped goal results.
- Historical screens and any derived totals must reflect edits to archived as
  well as active entities.

Source: [agreed specification](../../.scratch/habit-tracker/spec.md#dynamic-history).
