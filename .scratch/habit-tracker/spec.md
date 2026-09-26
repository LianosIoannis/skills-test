# Habit tracker: first local milestone

Status: Agreed
Agreed on: 2026-09-27

## Purpose and delivery

Build a complete locally running, multi-user web application for logging meals,
completed workouts, and course sessions, and evaluating recurring goals from those
records. Include clear local setup instructions, database migrations, and seed
data where appropriate.

Required stack: Angular 22 installable PWA, Node.js with TypeScript, Express,
Prisma 7, and SQLite. Reading and writing data require an internet connection;
offline synchronization is outside this milestone.

This specification records agreed product requirements. Dependency compatibility
and concrete implementation choices must be verified during implementation.

## Accounts and ownership

- Open registration and login using email and password.
- Each user owns private meals, exercises, workouts, domains, courses, goals,
  and log entries. Enforce ownership on reads and mutations, including references
  between entities; another user's identifiers must not grant access.
- No sharing, public templates, or social features.
- Email verification, password recovery, and email delivery are deferred. Keep
  authentication structured so these capabilities can be added later.

## Meals and meal logs

A reusable meal has a required name and nutrition values per serving: protein,
carbohydrates, and fat in grams, plus calories. Nutrition values are nonnegative.
Calories are entered explicitly rather than calculated from macronutrients.

A meal log references a saved meal and contains an explicit calendar date,
optional time of day, and a serving quantity greater than zero. Decimal servings
such as 0.5 and 1.5 are supported. Nutrition contribution equals the meal's current
per-serving values multiplied by logged servings.

Users can edit the date, selected meal, servings, and optional time.

## Exercises, workouts, and workout logs

An exercise is a separately saved reusable entity with a name, default number
of sets, and default set duration. Sets and duration are positive integers;
duration is stored in seconds.

A workout contains an ordered list of exercise occurrences. The same exercise
may appear multiple times. Each occurrence references an exercise and can
override its default sets and/or set duration for that workout. Without an
override, the current exercise default applies. An override remains in effect
when the exercise default changes.

Exercise duration is effective sets multiplied by effective set duration. Rest
time, repetitions, and weights are outside this milestone.

A workout log references a saved workout and records its completion on an
explicit calendar date, with an optional time. It contributes one completed
session to workout-count goals. Multiple logs of the same workout count as
separate sessions.

Users can edit the date, selected workout, and optional time. There are no
per-log exercise overrides or copied workout snapshots. Display the current
workout composition and current effective exercise values, including later
changes to ordering, additions, removals, defaults, and workout overrides.

## Domains, courses, and course sessions

Users create domains, such as Programming, and named courses belonging to a
domain, such as Angular. A course session references a course and contains an
explicit calendar date, optional time, and a positive whole-minute duration.
Attendance and independent study are treated alike.

Users can edit the date, selected course, duration, and optional time. Courses
can move between domains. Historical sessions use their course's current domain,
so a move changes historical domain totals and applicable goal results.

## Logging and calendar rules

- Each submission creates a separate log entry. The same saved item can be
  logged multiple times on the same date.
- Quick logging allows creating missing reusable entities without abandoning
  the logging flow.
- Users can create and edit past records and delete individual mistaken logs.
- Each user has a configurable timezone. It determines today, the current goal
  period, and whether a proposed date is in the future.
- Weeks start on Monday.
- Records belong to explicit calendar dates. Changing timezone does not move
  existing records between dates.
- Optional time of day is descriptive and does not determine date ownership.
- Future scheduling is excluded; log dates cannot be future dates in the user's
  configured timezone.

## Dynamic history

Historical records reference current saved entities rather than snapshots.
Editing an active or archived meal, exercise, workout, course, or domain must
be reflected in historical displays and any affected totals and goal results.

For example, changing a meal from 500 to 550 calories changes a past one-serving
log to 550 calories. Changing a workout changes its past displayed composition.
Moving a course changes which domain receives its historical learning duration.

Goal definitions are the deliberate exception: retain the definition applicable
to each previous period, while evaluating it against dynamically resolved logs.

## Reusable entity lifecycle

Apply these rules to meals, exercises, workouts, courses, and domains:

- An entity that has never been referenced may be permanently deleted.
- Once referenced by a log, another reusable entity, or a goal, it is permanently
  protected from deletion. Removing all references later does not remove this
  protection. For example, adding an exercise to a workout protects the exercise;
  assigning a course to a domain protects the domain.
- Protected entities can be archived. Archived entities can be restored and
  remain editable; edits continue to affect historical records.
- Archived entities remain valid through existing references but are excluded
  from normal selection for new references.
- An archived exercise already in an active workout does not prevent that
  workout being logged. It cannot be newly selected for a workout occurrence.
- Archiving a domain makes its courses unavailable for new course sessions
  until the domain is restored. A course's own archived state still applies.
- Archived courses and domains cannot be selected as scopes for new goals.
- Existing goals referencing archived entities remain active until explicitly
  ended; archiving does not silently end them.

## Goals

Goals recur daily or weekly and are evaluated independently. Multiple goals may
target the same metric, including both daily and weekly goals.

| Category | Metric | Target types | Optional scope |
| --- | --- | --- | --- |
| Nutrition | Calories, protein, carbohydrates, or fat | Minimum, maximum, range | None |
| Training | Completed workout session count | Minimum | None |
| Learning | Total session duration in minutes | Minimum | Domain or course |

Nutrition target values are nonnegative; a range requires minimum <= maximum.
Workout targets are positive integers. Learning targets are positive whole
minutes. A learning goal without a scope counts all learning duration.

### Definition history

- A new goal starts in the current daily or weekly period. Its calculation
  includes matching logs throughout that period, including earlier dates in
  the same week.
- Metric, daily/weekly period, and optional course/domain scope are fixed after
  creation. Changing these requires ending the goal and creating another.
- Name and target values can change. Changes apply from the current period
  onward; earlier periods retain their previous definitions.
- Multiple edits in the same current period retain only the latest definition
  for that period.
- Once active, a goal cannot be permanently deleted. Ending it excludes it
  starting with the current period while preserving prior periods for reporting.
- Historical scoped goals retain their original domain/course reference. A
  course move can change which logs match a historical domain goal without
  changing that goal's definition.

### Progress and achievement

Compute progress automatically from the relevant logged records:

- Nutrition: sum current meal nutrition multiplied by servings.
- Training: count completed workout logs.
- Learning: sum session minutes, applying the current course/domain relationship
  when filtering by scope.

A period without relevant logs is "no data", not automatically zero or failed.
Once relevant logs exist, calculate using the available records. A relevant meal
log with zero of a nutrient is data; absence of relevant logs is distinct.

Minimum targets are met at or above the target, maximums at or below it, and
ranges include both endpoints. During active periods, maximum/range goals show
provisional status such as "within target so far"; final achievement is evaluated
after the period ends. Closed-period results can subsequently change when logs
or their referenced entities are edited. Partial logging cannot establish that
a day's record is complete.

## Interface

The dashboard prioritizes today and shows:

- Today's nutrition totals.
- Daily and weekly goal progress.
- Quick actions to log a meal, workout, or course session.
- Today's entries.
- A small week/date navigation control.

A separate history page provides a monthly calendar and browsing of previous
days. Management screens support reusable entities and goals, including editing,
archiving/restoring where applicable, and permitted deletion. Logging supports
inline creation of missing reusable entities while retaining the entry context.
The interface supports use on phones and installation as a PWA.

## Acceptance criteria

1. Following the setup instructions produces a working local app and SQLite
   database using the specified stack and migrations.
2. Two registered users cannot access or mutate each other's data, including
   by supplying another user's entity IDs in requests.
3. A 1.5-serving log of a 500-calorie meal contributes 750 calories. Editing the
   saved meal to 550 updates that past contribution to 825 calories.
4. A workout can contain repeated exercise occurrences with separate overrides.
   Editing defaults affects unoverridden values; changing workout composition
   updates past workout displays. Each workout log still counts once.
5. Moving a course between domains updates historical domain learning totals
   and scoped goal results while preserving the historical goal definitions.
6. A goal target edited during this week changes this week's definition without
   replacing last week's. A second edit this week replaces this week's version.
   Ending the goal removes it from this week onward while retaining prior weeks.
7. Empty relevant periods show "no data". Relevant logged zero nutrition is
   evaluated numerically. Active maximum/range results are provisional.
8. Removing every reference to a previously used entity does not enable its
   permanent deletion. Archive, restore, and archived editing follow the rules
   above, including exercise and domain dependency behavior.
9. Changing timezone preserves log dates. Current-period and future-date checks
   use the selected timezone and Monday-start weeks.
10. Users can create missing items within logging, log duplicates intentionally,
    edit past entries, and delete mistakes. Invalid numeric values are rejected.
11. Dashboard and monthly history expose the agreed information, and the Angular
    application is installable as a PWA without promising offline data access.

## Outside the first milestone

Public deployment, production infrastructure, monitoring, email delivery,
verification and recovery, offline synchronization, sharing/social features,
public templates, future scheduling, deadline-based goals, workout rest times,
repetitions/weights, and per-log exercise overrides.
