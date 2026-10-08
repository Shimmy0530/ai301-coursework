# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

- Where it lives: eval, the Plan context block's "Files:" list, its
  "Scope" and "Not in scope" sentences, and any deferral or deviation
  note, read against every file header and hunk in the Candidate PR's
  Diff, and against the Description's claims about what changed. Live,
  `plan.md` (its Files, Scope, and `## Deviations` sections) against
  `git diff main...HEAD` run from the working copy on the branch, and
  against the description in `pr_draft.md`.
- What good looks like: every changed file is one the plan names (a
  named folder or "a changelog entry" counts), and every hunk is the
  fix, a test of it, or an entry file asked for. Drift in the "more"
  direction is a hunk adding a flag, option, setting, feature, rename,
  or rewrite the plan did not ask for, often in a file the plan never
  names. Drift in the "less" direction is a named deliverable missing
  from the diff. An honest deviation re-ties either one only when the
  plan records it AND the description states it; a note in only one
  place is not disclosure. A description that claims "exactly as
  planned" over an extra hunk, or claims an entry the diff lacks, is
  drift, not a comms problem.

## Test evidence (harness category: not-tested)

- Where it lives: eval, the Candidate PR's Test evidence section (and
  a Testing section in the Description, if present), read against the
  Plan context's test plan and the repro input it names, and against
  the check commands the repo-facts block or the plan names. Live,
  `test_evidence.md` and the Testing section of `pr_draft.md` against
  the test plan in `plan.md` and the repo's stated checks (for Path
  Review: `make test-unit`, `make test-integration`, `make lint`,
  `make typecheck`).
- What good looks like: the bug's own input or command is re-run and
  the result is named before and after, the after being the plan's
  expected-after and different from the before. A before/after noted
  next to the command counts when it names the observable (an exit
  code, an output line, a returned value); "works now" names nothing.
  Every failure mode the test plan lists as a repro is shown; a control
  may be stated as unchanged. The repo's suite or check command is
  named with its outcome, and a failing check reported with its reason
  is still evidence. Not decisive: "tested locally", "verified on my
  machine", "all tests pass" with no command, or evidence that runs
  only a control or a path the change does not touch.

## Diff quality (harness category: unreviewable)

- Where it lives: eval, every hunk of the Candidate PR's Diff and the
  Commits list. Live, `git diff main...HEAD` and
  `git log main..HEAD --oneline`.
- What good looks like: each hunk is the fix, its test, or a required
  entry, and nothing else rides along. Debris tells: debug output
  statements (print, eprintln, console.log, dbg), commented-out code
  (including a commented-out first attempt or debug line), functions
  or variables nothing calls (including ones behind allow(dead_code)),
  TODO/FIXME/WIP/XXX markers, hunks that only re-indent, re-order
  imports, or re-print unchanged lines, and commits named "wip",
  "fix", or "cleanup while debugging". Not debris: explanatory
  comments, docstrings, and unchanged lines that re-appear inside a
  hunk that also changes a functional line.

## Standards and comms (harness category: standards-wall)

- Where it lives: eval, the Repo facts block's `pull requests` line
  (template sections, checklist, issue-reference line, changelog or
  whatsnew entry, tests) and its `contribution policy` line (any AI-use
  rule), read against the Description and the Diff; the Thread
  highlights entries by OWNER, MEMBER, or COLLABORATOR, read against
  the Description and Diff. Live, the repo's
  `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`, plus
  `scope.md`'s house rules (the template is always used, and the
  course asks for an AI-use disclosure in Notes for Reviewers), against
  `pr_draft.md`; the thread via
  `gh api repos/<owner>/<repo>/issues/<n>/comments` with
  `.author_association`.
- What good looks like: each stated section or checklist appears with
  content specific to this change, a required "closes #N" or "fixes
  #N" line is present, a required changelog or whatsnew entry is in
  the diff, and tests asked for behavior changes are in the diff.
  Where the policy requires it, the description says in the author's
  words how AI was used; every package is treated as AI-assisted work.
  A maintainer's stated direction is followed and a maintainer's
  question about the design is answered. Not compliant: a required
  checklist or issue line simply absent, a section left blank or
  filled with boilerplate, a required entry claimed but not in the
  diff, or no disclosure under a policy that requires one. Whether the
  description's claims match the diff is plan fidelity, above.
