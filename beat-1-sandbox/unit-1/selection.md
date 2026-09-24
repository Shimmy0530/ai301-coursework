# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

[https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53)

**Verdict output**

Issue #53 passes all five required checks, so the verdict is accept. It's a small Python regex bug with the failing tests already named, which suits your fit profile well.

Required checks (all pass):

- repo-alive: pass. The newest commit on main is from 2026-09-16 (today), by a human, Aburke225.
- repo-not-archived: pass. isArchived: false.
- scope-newcomer-sized: pass. It's one bug: `safety/pii_scrubber.py` misses phone numbers written as (555) 123-4567. The issue includes steps to reproduce and names 4 failing tests. It isn't a tracking issue and has no sub-issues.
- unclaimed: pass. Nobody is assigned, no pull request is linked or mentions it, and there are 0 comments.
- policy-allows-ai-assist: pass. `docs/CONTRIBUTING.md`, the PR template and the issue templates say nothing about AI.

Preferred checks (2 of 3 pass):

- maintainer-answers: pass. Aburke225, who has Collaborator access, commented on issues #52 and #43 today.
- newcomer-signposted: pass. It's labeled good first issue and tier-1, and the body names `pii_scrubber.py` and `tests/unit/test_pii_scrubber.py`.
- still-shipping: fail. The repo has published no releases. This check never affects the verdict.

Why it fits you: the fix is a regex change in `safety/pii_scrubber.py`. That's all Python, and you don't need to touch the frontend.

Before you open a pull request: the failing tests carry `@pytest.mark.xfail(strict=True)` markers. Your pull request has to remove those markers, or CI fails with `XPASS(strict)` once your fix makes the tests pass.

Don't comment on the issue yet. `docs/CONTRIBUTING.md` tells you to comment before you start work, but claiming the issue is Unit 2's lesson.

Next step: add #53 and this grading to the "chosen issue" section of `beat-1-sandbox/unit-1/selection.md`. That's about 10 minutes.

```
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53",
  "checks": [
    {"name": "repo-alive", "grade": "pass", "evidence": "Latest main commit 2026-09-16T21:42:18Z by Aburke225 (human), 0 days before today"},
    {"name": "repo-not-archived", "grade": "pass", "evidence": "gh repo view: isArchived false"},
    {"name": "scope-newcomer-sized", "grade": "pass", "evidence": "Single bug: phone regex in pii_scrubber.py misses '(555) 123-4567'; repro steps and 4 named failing tests; no sub-issues, no maintainer discouragement"},
    {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; no linked or cross-referenced PRs; 0 comments"},
    {"name": "policy-allows-ai-assist", "grade": "pass", "evidence": "docs/CONTRIBUTING.md, PULL_REQUEST_TEMPLATE.md and ISSUE_TEMPLATE/* contain no AI policy; silence passes"},
    {"name": "maintainer-answers", "grade": "pass", "evidence": "Collaborator Aburke225 commented on issues #52 and #43 on 2026-09-16"},
    {"name": "newcomer-signposted", "grade": "pass", "evidence": "Labels 'good first issue' and 'tier-1'; body names pii_scrubber.py and tests/unit/test_pii_scrubber.py"},
    {"name": "still-shipping", "grade": "fail", "evidence": "latestRelease: null (no releases published)"}
  ],
  "verdict": "accept"
}
```

---



## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

19/20 
(First Run)

**Issue analysis**

`issue-20`: excalidraw/excalidraw#11811, "Add company logo shape to the toolbar" (category `scope`).

- **Rubric decision:** `accept`. From `eval-run.txt`: `issue-20  reject  accept   NO     graded accept`
- **Gold label:** `reject`, "one-line feature wish with no spec and a product decision hiding inside"

**Why the rubric said** `accept`**:** the verdict rule is "`accept` if and only if every `required` check grades `pass`", and all five required checks pass on this bundle's evidence:

- `repo-alive`: "2026-08-04 by dwelle: feat(packages/excalidraw): ViewportStatusFrame & factor out user-follow state (#11819)". That is one day before the 2026-08-05 capture.
- `repo-not-archived`: "repo: excalidraw/excalidraw (128988 stars, archived: no)"
- `unclaimed`: "this issue: assignees: none; linked PRs: none", and the thread reads "(no comments)".
- `policy-allows-ai-assist`: "contribution policy (CONTRIBUTING.md): no statement on AI or contribution tooling". The rubric says "No stated policy passes."
- `scope-newcomer-sized`: none of its fail triggers fires. The issue isn't a list of links to other issues. Nobody from the project discourages it, because there are no comments at all. It asks for a change, not for support. The design-question trigger needs "the thread shows a real design question left genuinely unresolved (no maintainer decision, and prior linked PRs on it closed unmerged)", but with zero comments and no linked PRs there is no thread to show one. So the check falls through to "Otherwise passes — a terse body, several related edits toward one goal, or an unpolished writeup are not scope failures."

**Why that is wrong:** the unsettled product decisions are in the body, not the thread: "Logo asset TBD.", "Likely surface: `packages/excalidraw` editor (toolbar + element), possibly app wiring in `excalidraw-app` if needed.", and a "fixed company logo" in a general-purpose whiteboard. It was "opened by cursor[bot] (NONE)", and the author left "Have you checked our roadmap" unchecked. No maintainer has said the feature should exist. My scope check only looks for unresolved design in the thread and counts a terse body as a pass, so an underspecified feature request that nobody has discussed gets through. This is the run's only miss (`scope 3/4`).

**Check rationale**

From `tools/issue-select/rubric.md`, as currently written:

```text
| `repo-alive` | Repo facts: the dates in "last 5 default-branch commits" and "last push to any branch", compared with the capture date | At least one default-branch commit dated within 90 days of the capture date. A bot-authored commit counts only when it merges a human's pull request. Note this check deliberately does not gate on maintainer reply latency: healthy busy repos routinely leave most issues unanswered, so latency is a preferred signal below rather than a gate. | required |
```

Why each part has its current form:

- **"default-branch commit":** a first PR only matters if someone can review and merge it. Commits landing on the main branch are the most direct proof that a maintainer is still doing that. A push to a side branch can be abandoned work.
- **"within 90 days":** a quarter is long enough to cover a maintainer's vacation or a slow stretch between releases, and short enough that a repo nobody has touched in months doesn't pass as active.
- **"counts only when it merges a human's pull request":** dependency bots and CI can keep committing to a repo nobody maintains. A bot merging a person's PR still means a human reviewed it, so that one counts.
- **"does not gate on maintainer reply latency":** busy, healthy projects leave most issues unanswered because maintainers spend their time on code and PRs. Gating on replies would reject the most active repos, so reply speed only ranks issues, in `maintainer-answers`.

**Trade-offs**

**Case it will miss:** a repo where the owner keeps committing their own work but ignores outside pull requests. The check proves someone is working, not that anyone will review a newcomer's PR, and with latency only a preferred signal, no required check catches that repo. **Also given up:** projects that are healthy but slow, like a hobby tool updated twice a year, fail the 90-day window even though a PR might still get merged.

---



## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit and time:** it's a Python regex fix in `safety/pii_scrubber.py`, which matches my Python and security-tooling background and keeps me out of the frontend. The scope is small: one phone pattern plus four named failing tests, so it fits within the unit alongside other work.
2. **What the verdict got right, and what I weighed:** the skill correctly found the repo active, the issue unclaimed, no AI-use ban, and a good-first-issue label with named files and tests. All five candidates I graded were accepted and tied on preferred checks, so the rubric couldn't choose between them. I picked on fit. I also weighed a risk the rubric can't see: a looser phone pattern can start redacting things that aren't phone numbers, so the fix needs care beyond making the tests pass.
3. **Difficulty claiming it:** low. The course's house rule says classmates' claim comments don't block an issue, and credit comes from the PR I open. The likely friction is other students choosing the same beginner-labeled issue, so I expect overlapping PRs.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.