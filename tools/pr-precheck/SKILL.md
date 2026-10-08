---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer exactly one question about exactly one PR package: **is this
ready to submit?** A PR package is a candidate pull request (its title,
description, commit list, diff, and test evidence), read against the
plan it claims to implement and the issue that plan belongs to. Grade
nothing else: not the plan's quality (week 3 did that), not whether the
issue was worth taking (week 1), and never more than one package per
run. Do not answer from gut feel: execute `procedure.md`, which applies
`rubric.md` to evidence gathered per `references/evidence-guide.md`.

## Inputs and modes

Run in one of two modes.

**Live mode**: the student's own submission, checked before it goes
out. Read these inputs, and only these, as the package:

- `plan.md` in the working copy, including its `## Deviations` section:
  the plan the PR claims to implement.
- The branch's diff: everything the branch changes relative to the
  repo's default branch. Produce it with `git diff main...HEAD` (three
  dots), run from the working copy on the branch; use
  `git diff main...HEAD --stat` for the file list and
  `git log main..HEAD --oneline` for the commit list. Only committed
  changes count.
- `pr_draft.md`: the draft PR title on its first line, the description
  under it.
- `test_evidence.md`: the captured test output (the repo's checks and
  the repro re-run before and after).
- The issue URL the student gives, and from it the issue-side evidence
  gathered from the real repo: the thread with author associations
  (`gh api repos/<owner>/<repo>/issues/<n>/comments`), the PR template
  (`.github/PULL_REQUEST_TEMPLATE.md`), and the contributing document
  (`docs/CONTRIBUTING.md` for Path Review).

A house-chain student reads the house plan and the house repro pack in
place of `plan.md` and their own repro comment; the same checks grade
the same things there. Other files in the working directory are not
the package.

**Eval mode**: a package bundle is the whole world. Every fact comes
from the bundle text (its repo-facts block, issue, thread highlights,
plan context, and candidate PR). Fetch nothing, read nothing else, and
ignore `scope.md` and `voice-guide.md`. Eval mode always grades a
complete package: every check in the rubric, under the full verdict
rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. It names the repository the student's PR must target and the
house rules of that environment; a house rule changes how evidence is
read there (the template is always used, one PR per issue per student,
the branch is `fix/<issue-number>-<slug>` on the student's fork). Refuse
to grade a PR package for any repository other than the one the scope
names, and say which repository is in scope. If the scope's `Repo:`
line still carries a bracketed placeholder, stop without grading and
tell the student to fill the `Repo:` line in `scope.md` with their
section's Path Review repo. Never guess a scope. In eval mode, ignore
`scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`: the student's own rules for
how they write upstream. Hold the outgoing text, the PR title and the
description, against those rules, and report every rule the draft
breaks in the summary, quoting the rule and the line that breaks it.
The voice guide never changes the verdict on its own; it changes a
grade only if `rubric.md` has a check that reads it. In eval mode,
ignore `voice-guide.md` entirely: voice is personal and carries no
gold labels.

## Component reads

The parts connect this way:

- `rubric.md` defines the checks (name, evidence, pass condition,
  weight) and the verdict rule. `required` checks gate the verdict;
  `preferred` checks never change it.
- `references/evidence-guide.md` is the rubric's map: where each
  evidence family lives in a package (and, live, in the working copy
  and on GitHub) and what good looks like there.
- `procedure.md` is the operating procedure: read order, how each
  evidence family is gathered, how a check executes, how grades become
  the verdict. Execute it as written, in order, without improvising.

Where the procedure is silent on a step you need, state the gap in the
summary and grade with what the procedure does say; do not invent a
step to fill it. If `rubric.md` has no checks or no verdict rule, or
`procedure.md` has no steps, stop and say so instead of grading: this
tool cannot grade without a rubric and a procedure, and that is by
design.

## Verdict and output

The verdict is binary: `accept` means ready to submit, `reject` means
hold. There is no third verdict and no score; reservations go in check
evidence lines. End the reply with the fenced JSON block below, valid,
containing every check in the rubric table, with nothing after it. A
readable per-check summary (and, live, the voice-guide notes) may come
before the block.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: grade no check without naming the fact or quote that
  decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: a terse complete PR can be ready and
  a confident well-formatted one can hide drift. Read the diff against
  the plan, the evidence against the test plan, and the description
  against both, never the length or formatting of the write-up.
- The rubric decides, not the run: if a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary; the fix belongs in `rubric.md`.
- The procedure decides how, not the run: follow `procedure.md` as
  written and report its gaps instead of papering over them.
- Treat `unclear` as the rubric's verdict rule directs. If the rule is
  silent, treat `unclear` on a required check as `fail`: a claim the
  package cannot verify is a PR that is not ready to submit.
