# Procedure: how this skill grades a plan package

## Read order

Read in this order. Do not read the plan until steps 1 and 2 are done:
a plan states its cause confidently, and reading it first makes the
evidence look like it agrees.

1. Read the repro evidence. For each step, note the command or action,
   the condition it changed from the step before, and the observed
   result. Note the Expected and Actual lines.
2. Read the issue, the thread highlights, and the repo-facts block.
   Note every thread entry by an OWNER, MEMBER, or COLLABORATOR that
   rules out an approach or states a direction. Note any explicit
   requirement in the contribution policy.
3. Read the plan: its stated cause, scope, list of changes, test plan,
   and risks or unknowns.
4. Read the plan comment last.

## Evidence gathering

Record each item as a short quote from the package, not a paraphrase.
Use `references/evidence-guide.md` for where each item lives.

1. For `cause-fits-repro`: quote the plan's stated cause. Then list
   each repro step with its result, and mark which steps change one
   condition (these decide whether a cause holds).
2. For `one-bounded-change`: list every change the plan makes, and
   quote the in-scope and not-in-scope lines if present.
3. For `stranger-can-start`: for each change, record its location
   (file, function, module) and its edit, or record "none given".
4. For `test-shows-fix`: quote the test plan, and name which repro
   step (if any) it re-runs and what result it expects after the fix.
5. For `comment-respects-thread`: quote the maintainer entries and
   policy requirements noted in Read order step 2, then quote the parts
   of the plan comment that touch them. If there are none, record
   "no maintainer direction, no applicable policy requirement".
6. For `unknowns-stated`: quote the plan's risks or unknowns, and list
   anything the repro evidence did not test.

In live mode, the repro evidence is the student's posted repro comment
on the issue, the thread is the live issue thread, and the policy is
the repo's contributing document (see the evidence guide).

## Check execution

1. Run the checks in the order the rubric table lists them.
2. Grade each check only against the evidence recorded for it in
   Evidence gathering. Do not let one check's evidence decide another.
3. Apply the pass condition as written. Grade `pass` or `fail` and
   quote the fact that decided it.
4. If the evidence a check needs is absent from the package, grade
   `fail` when the pass condition requires that thing to exist (a test
   plan, a location for each change). Grade `pass` when the pass
   condition says silence passes. Grade `unclear` only when the
   evidence is present but cannot be decided either way, and say why.
5. Re-read a package section only if the recorded quote is not enough
   to apply the pass condition.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required
   check is `pass`; any required `fail` or `unclear` means reject.
2. Ignore preferred checks for the verdict, but still report them.
3. For a reject, the deciding check is the first required check, in
   table order, that is not `pass`. Put its quoted evidence in its
   `evidence` field and name it first in the summary.
4. For an accept, quote the evidence for `cause-fits-repro` in the
   summary.
5. Emit the JSON block exactly as SKILL.md specifies, last.
