# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

- Where it lives: eval, the candidate plan's Diagnosis (or whatever
  section states the cause), read against the Repro evidence block's
  steps and its Expected and Actual lines. Live, the draft `plan.md`'s
  cause against the student's posted repro comment on the issue.
- What good looks like: the cause predicts the result of every repro
  step, including the steps that change one condition (a flag on or
  off, a component removed). If a step shows the bug with the blamed
  component removed, or no bug with only an unrelated condition
  changed, the cause is contradicted. A cause quoted from the thread
  counts only when the repro steps back it.

## Scope

- Where it lives: eval, the candidate plan's Scope section (in scope,
  not in scope) and its Changes list. Live, the same parts of the
  draft `plan.md`.
- What good looks like: every change traces back to the reproduced
  bug or to testing its fix. A drive-by rewrite adds changes that
  would still make sense if the bug did not exist: renames, refactors,
  dependency bumps, extra features, other bugs.

## Executability

- Where it lives: eval, the candidate plan's Changes or approach list.
  Live, the same in the draft `plan.md`.
- What good looks like: each change names a file, function, or module
  and the edit made there, so the first change could be started
  without asking the author. "Investigate", "refactor as needed",
  "TBD", or a change with no location is not executable.

## Test plan

- Where it lives: eval, the candidate plan's Test plan, paired with
  the Repro evidence block's numbered steps. Live, the draft
  `plan.md`'s test plan against the posted repro comment's steps.
- What good looks like: it re-runs a repro step (same input, same
  command), or a test that encodes that input, and states the result
  expected after the fix, which differs from the repro's Actual. "Run
  the tests" or "verify it works" names nothing observable.

## Honesty

- Where it lives: eval, the candidate plan's risks or unknowns section,
  plus certainty wording anywhere in the plan or comment ("this will
  fix", "the root cause is"). Live, the same in the drafts, plus the
  `## Deviations` section of `plan.md` after the build.
- What good looks like: claims stay inside what the repro tested, and
  what it did not test (other inputs, platforms, side effects) is named
  as unverified. A mid-build change of plan is recorded under
  Deviations, not only in the diff.

## Comms

- Where it lives: eval, the candidate plan comment, read against the
  Thread highlights entries whose author is OWNER, MEMBER, or
  COLLABORATOR, and against the Repo facts block's contribution policy
  line. Live, the draft comment against the issue thread (author
  association via `gh api repos/<owner>/<repo>/issues/<n>/comments
  --jq '.[].author_association'`) and the repo's contributing document
  (for the Path Review repo, `docs/CONTRIBUTING.md`).
- What good looks like: the comment does not push an approach a
  maintainer ruled out or go against a direction a maintainer stated,
  and it meets any explicit policy requirement (for example an AI-use
  disclosure). Comments from non-maintainers are context, not
  direction.
