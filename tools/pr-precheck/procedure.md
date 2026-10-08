# Procedure: how this tool grades a PR package

## Read order

Read in this order. Do not read the title or description until steps 1
through 6 are done: the description is the author's claim about the
diff, and reading it first makes the diff look like it agrees.

1. Read the repo-facts block (live: the repo's PR template file and its
   contributing document, plus `scope.md`'s house rules). Write down
   every ask on the `pull requests` line (template sections, checklist,
   issue-reference line, changelog or whatsnew entry, tests), every
   check command the repo names, and whether the contribution policy
   states an AI-use rule.
2. Read the plan context (live: `plan.md`, including `## Deviations`,
   or the house plan). Write down: the Files list; the in-scope and
   not-in-scope lines; each deliverable the scope and approach name;
   each deviation or deferral note; the test plan's steps, marking
   which are repro re-runs of a failure mode and which are controls;
   the repro input that showed the bug and the plan's expected-after.
3. Read the issue and thread highlights. Write down each entry by an
   OWNER, MEMBER, or COLLABORATOR that states a direction or asks a
   question about the change.
4. Read the diff (live: `git diff main...HEAD` from the working copy,
   on the branch). For every file, list every hunk with one line saying
   what it does, and mark each hunk: fix, test, entry file, or other.
5. Read the commit list (live: `git log main..HEAD --oneline`).
6. Read the test evidence (live: `test_evidence.md`). For each command
   or input shown, write down what it ran and the result shown before
   and after.
7. Read the title and description last (live: `pr_draft.md`, title on
   the first line). Write down every sentence that claims what the
   diff does, does not do, or adds, and every template section with
   whether it carries content specific to this change.

## Evidence gathering

Record each item as a short quote from the package, not a paraphrase.
`references/evidence-guide.md` says where each item lives.

1. For `diff-within-plan`: pair the file list from step 4 with the Files
   list from step 2. For each file, quote the plan line that names it
   or record "not named". For each hunk marked "other", quote it and
   quote the not-in-scope line it touches, if any, and any deviation
   note that records it.
2. For `plan-delivered`: for each deliverable from step 2, name the hunk
   that delivers it or record "absent". For each absent one, quote the
   plan's deferral or deviation note (or "no note") and the description
   sentence that states the omission (or "not stated").
3. For `description-matches-diff`: for each claim from step 7, name the
   hunk that makes it true or record the hunk that makes it false (an
   unplanned hunk against "exactly" or "no changes beyond"; a missing
   hunk against "X added").
4. For `repro-rerun-shown`: quote the repro input from step 2, then
   quote the evidence command or input that re-runs it and the before
   and after results shown. Record which test-plan failure modes are
   re-run and which are not. Record whether the input shown is the
   bug's input, a control, or an unrelated path.
5. For `repo-checks-run`: quote each check command named in the
   evidence or description with its stated outcome, or record "no
   command named".
6. For `no-debris`: quote every hunk line that is a debug statement,
   commented-out code, an unused definition, a TODO/FIXME/WIP/XXX
   marker, or a hunk that changes no functional line. Record "none"
   when there is nothing to quote.
7. For `template-asks-met`: for each ask from step 1, quote the
   description section or diff hunk that honors it, or record
   "missing". Record "no asks stated" when the repo states none.
8. For `ai-disclosure`: quote the policy line from step 1 and, if it
   requires disclosure, the description sentence that discloses AI use,
   or record "no disclosure".
9. For `thread-direction-honored`: quote each maintainer entry from
   step 3 and the description or diff part that answers it, or record
   "no maintainer direction".

## Check execution

1. Run the checks in the order the rubric table lists them.
2. Grade each check only against the evidence recorded for it in
   Evidence gathering. One check's evidence does not decide another,
   except where the rubric's pass condition names the other check
   (`description-matches-diff` reads `diff-within-plan`'s finding).
3. Apply the pass condition as written. Grade `pass` or `fail`, and
   quote the fact that decided it in the check's evidence line.
4. When the evidence a check needs is absent from the package, grade
   `fail` if the pass condition requires that thing to exist (a repro
   re-run, a named check command, a required section, a required
   disclosure). Grade `pass` when the pass condition says absence
   passes (no asks stated, no policy, no maintainer direction). Grade
   `unclear` only when the evidence is present but cannot be decided
   either way, and say why in the evidence line.
5. Re-read a package section only when the recorded quote is not
   enough to apply the pass condition.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required check
   is `pass`; any required `fail` or `unclear` means reject.
2. Report preferred checks but do not let them change the verdict.
3. For a reject, the deciding check is the first required check, in
   table order, that is not `pass`. Name it first in the summary, with
   its quoted evidence.
4. For an accept, lead the summary with the `diff-within-plan`
   evidence (the plan line that covers each changed file).
5. In live mode, after the summary, list any voice-guide rule the title
   or description breaks, quoting the rule; this never changes the
   verdict.
6. Emit the JSON block exactly as SKILL.md specifies, last, with every
   check from the rubric table in it.
