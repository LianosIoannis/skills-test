# Habit Tracking

Personal records of meals, completed workouts, and learning, with recurring goals
that measure those records. Each user's records and reusable items are private.

## Language

### Meals

**Meal**:
A reusable named meal with protein, carbohydrates, fat, and calories defined per serving.
_Avoid_: Food item, ingredient, meal log when referring to the reusable meal.

**Serving**:
The meal-specific unit for its nutrition values; a consumed quantity can be a fraction or multiple of that unit.

**Meal log**:
A dated record of consuming a quantity of servings of a meal, with an optional time of day.
_Avoid_: Meal when referring to a consumption record.

### Training

**Exercise**:
A reusable named activity with a default number of sets and default duration per set.

**Workout**:
A reusable ordered collection of exercise occurrences.
_Avoid_: Workout session, workout log when referring to the reusable workout.

**Exercise occurrence**:
One position occupied by an exercise in a workout, with optional overrides of its default sets and set duration; the same exercise can occupy multiple positions.
_Avoid_: Completed exercise, exercise log.

**Workout override**:
A set count or set duration specified for one exercise occurrence in place of the exercise's corresponding default.
_Avoid_: Log override, actual sets.

**Workout log**:
A dated record that a workout was completed, optionally with a time of day; each record represents one completed workout session.
_Avoid_: Workout when referring to a completion record, workout snapshot.

### Learning

**Domain**:
A user-created learning subject that groups courses, such as Programming.
_Avoid_: Tag, course category as alternate names for this concept.

**Course**:
A named learning subject or course belonging to one domain, such as Angular within Programming.

**Course session**:
A dated record of time spent on a course, measured in whole minutes, optionally with a time of day; attendance and independent study have the same meaning here.
_Avoid_: Attendance record, course when referring to a session.

### Records and lifecycle

**Log entry**:
A collective term for a meal log, workout log, or course session.
_Avoid_: Habit, daily record when referring to one entry.

**Log date**:
The explicit calendar date to which a log entry belongs, unchanged by a user's later timezone changes.
_Avoid_: Creation date, timestamp.

**Dynamic history**:
History interpreted using the current properties and relationships of referenced meals, exercises, workouts, courses, and domains, so past totals and results can change.
_Avoid_: Snapshot history, immutable history.

**Protected entity**:
A reusable entity that has ever been referenced by a log entry, another reusable entity, or a goal and therefore remains permanently ineligible for deletion.
_Avoid_: Currently used entity, archived entity as synonyms for protection.

**Archived entity**:
A reusable entity withdrawn from new selection while retained for existing references and available for editing and restoration.
_Avoid_: Deleted entity, frozen entity.

### Goals

**Goal**:
A recurring objective with a fixed metric, daily or weekly recurrence, and optional learning scope, evaluated independently of other goals.
_Avoid_: Habit, deadline, target as synonyms for the entire goal.

**Metric**:
The quantity measured by a goal: calories, protein, carbohydrates, fat, completed workout count, or learning minutes.

**Goal scope**:
An optional course or domain restricting the learning sessions relevant to a goal; an unscoped learning goal covers all learning.
_Avoid_: Filter when referring to the goal's fixed scope.

**Goal period**:
A particular calendar day or Monday-through-Sunday week over which a goal is evaluated; the user's timezone determines which period is current.
_Avoid_: Rolling window, deadline.

**Target**:
A metric's required minimum, permitted maximum, or inclusive range; workout-count and learning-duration targets are minimums.
_Avoid_: Goal when referring only to its numeric requirement.

**Goal definition**:
The name and target applicable from a particular goal period onward, until another definition takes effect; earlier periods retain their applicable definition.
_Avoid_: Current target when referring to an earlier period's definition.

**Ended goal**:
A goal whose applicability stops at the beginning of its ending period while earlier applicable periods remain part of history.
_Avoid_: Deleted goal, achieved goal.

**Goal progress**:
The measured amount from relevant log entries in a goal period, compared with that period's target.

**Goal result**:
The assessment of a period's progress against its applicable definition; it can change when historical records or their referenced entities change.
_Avoid_: Permanent achievement, immutable result.

**No data**:
The absence of relevant log entries for a goal period, distinct from a measured zero and from failure to meet a target.
_Avoid_: Zero activity, failed goal as synonyms.
