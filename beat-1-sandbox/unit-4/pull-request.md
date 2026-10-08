# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/105

**Branch**

`fix/53-parenthesized-phone`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

One full run: **20/20** scored items, `categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`, `agreement: 20/20 scored items  (bar: 18/20: PASS)`. That is the run in `eval-run.txt`.

Before it I did one `--limit 3` smoke run (pkg-01, pkg-02, pkg-03: 3/3, one package each from standards-wall, clear-accept, and silent-drift) to confirm the harness accepted all four files and that the JSON block parsed. No revision happened between the smoke run and the full run, and none after it.

**Package analysis**

`pkg-13` (neovim/neovim#41337, category clear-accept). My rubric decided **accept**; the gold label is **accept**.

This is the package I most expected to get wrong, because the PR delivers less than the issue asks for: the plan says `%` expansion "is NOT covered by this change; if it proves messy it will be deferred with a note", and the description repeats it: "Deviation note, recorded in the plan before this PR: percent-sign environment expansion (`%VAR%`) is NOT handled here". A rubric that reads "less than everything" as drift rejects this. Mine passed it on `plan-delivered` because that check's pass condition says a deliverable may be absent when "its absence is recorded in the plan (a deviation or deferral note) AND stated in the description", and both halves are there. `diff-within-plan` passed because the two changed files are the two the plan names (`runtime/lua/vim/ui.lua`, `test/functional/lua/ui_spec.lua`) and the only hunks are the caret-escape line, an explanatory comment, and the new test. `repro-rerun-shown` passed on the `:lua =vim.ui.open(...):wait()` line with `before: { code = 1, stderr = "'t' is not recognized ..." }` and `after: { code = 0, stderr = "" }`, which is the plan's expected-after. `template-asks-met` passed because neovim's template "asks for a Problem section and a Solution section" and the description has exactly those two headings with content specific to the change. The deferral is disclosed in both places, so the rubric treats it as an honest shortfall, which is what the gold note calls it ("honestly disclosed shortfall ... ready as scoped").

**Check rationale**

Quoted from `tools/pr-precheck/rubric.md`:

> | `repro-rerun-shown` | The PR's test evidence, read against the plan's test plan and the repro input it names | The evidence re-runs the input or command that showed the bug (the plan's repro step, or a test that encodes that input) and names the observable result before and after, where the after is the plan's expected-after and differs from the before. Every failure mode the plan's test plan names as a repro to re-run is shown; a control may be stated as unchanged. Fails when the evidence is a summary with no input named ("tested locally", "works on my machine", "verified for a day"), or when the only input shown is a control or a path the change does not touch. | required |

It reads that way because of a conflict between the in-class activity and the gold labels. The activity's rule for calib-03 was strict: "For the Test evidence rows, a yes needs a command and its printed output. A note after # saying what it printed doesn't count." I started to write that rule into the check, then read the clear-accept packages: pkg-11's evidence is `$ rg hi  # before: rg: ./.gitignore: line 1: error ... # after: searches normally, no error`, pkg-13's is `-- before: { code = 1 ... } -- after: { code = 0 ... }`, and pkg-20's is `# before: ESC[?997;2n  # after: ESC[?997;1n`. All three are notes after a comment marker, and all three are gold accepts. A "printed output only" rule would have rejected three of the seven clear-accepts. So I rejected the activity's wording in favour of "names the observable result before and after, where the after is the plan's expected-after and differs from the before": what decides it is whether a specific observable (an exit code, an output line, a returned value) is named for the bug's own input, not whether the text is raw stdout. The two "fails when" clauses are what still catch the not-tested packages: pkg-04 and pkg-10 are summaries with no input named ("tested locally", "verified working on my machine for a full day"), and pkg-07 and pkg-14 run only a control or an unchanged path. The second sentence ("every failure mode the plan's test plan names ... is shown; a control may be stated as unchanged") is there for calib-04, where repro 2 is never re-run, while keeping pkg-13 and pkg-20, whose controls are only stated as unchanged.

**Trade-offs**

A case I accept this check will miss: it cannot tell a real before/after annotation from a fabricated one. "`# after: exit 0`" typed from memory looks identical to one copied from a terminal, so a contributor who writes the expected result instead of running it passes `repro-rerun-shown`. The activity's stricter rule would catch some of that, at the cost of rejecting pkg-11, pkg-13, and pkg-20. With 7 clear-accepts in the set and 3 of them annotated this way, the strict rule cannot reach 18/20, so the lenient wording is the right call for this instrument; in live mode the gap is covered by my voice guide ("paste the output, don't retell it"), which the skill reports on the PR text even though it never changes the verdict.

Nothing changed elsewhere, and here is how I know: the rubric was written once from the gold-label notes and the eleven packages I read before the first run, the smoke run agreed 3/3, and the first and only full run agreed 20/20 with every category matched, so no check was loosened or tightened after any result and no canary was needed. One thing I did not keep: I ran the full run without `--out results.json`, so the per-check grades behind each verdict are not on disk (the harness only fills the `note` column on disagreements, and there were none). I can say every reject category has a check written for its gold-note reason (`diff-within-plan` and `description-matches-diff` for silent-drift, `repro-rerun-shown` and `repo-checks-run` for not-tested, `no-debris` for unreviewable, `template-asks-met` and `ai-disclosure` for standards-wall), but I cannot quote the deciding check for any of the thirteen rejects from this run.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
