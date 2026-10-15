# Contribution: Failure to create portable copy when path is a top-level drive with no backslash

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** shimmy0530
**GitHub Username:** Shimmy0530
**Issue:** https://github.com/nvaccess/nvda/issues/20159
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: (2026-10-14) Installed issue-scout over my unit 1 skill, wrote its search strategy, and patched my rubric for real-world repos**

I built a search strategy for the scout: a Python bug query and a second
query I switched from Kotlin to JavaScript after the first scan, because
JavaScript is more flexible and widely used, and more familiar and
interesting to me. The first scans accepted repos that don't credit outside
contributors (chaoscypher's bot says a PR "is read as a bug report" and the
fix ships internally), and also passed issues that already had recent claim
comments, because my rubric still followed the Path Review rule that claims
don't count. Being recognized as a contributor matters to me, so I added a
required `contribution-credited` check and made `unclaimed` block any claim
comment from the last 30 days; the unit 1 eval still agreed on all 9
accept and policy items I re-ran.

**Unit 6: (2026-10-14) Installed the voice-guard on my posting doorway, patched my scout's scope check, and wrote repo memory for NVDA**

The voice-guard stopped its wiring test as expected and passed my claim on
nvaccess/nvda#20159 at both the manual check and the hook. Nothing fought me
during the guard install beyond Windows' default cp1252 encoding, which would
have crashed the checker and let posts through unchecked, so `PYTHONUTF8=1`
was added to the hook. For the repo memory, the most useful parts are the
guidance from the closed PRs: three PRs on this issue got closed, which means
they didn't do something right, and I can learn from it.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I asked AI to give me a list of five to ten issues from the Issue Selection
site that it thought I would like, then reviewed them manually and picked the
ones I found most interesting. I graded my picks directly with issue-scout,
and it accepted nvaccess/nvda#20159 with every required check passing, all
three preferred checks passing, and a difficulty note of straightforward. It
is a very clear-cut issue and should be fairly straightforward to fix;
AI-assisted, I don't expect it to take two weeks. The expectations in the
repository are very clear and should be easy to follow. The fact that three PRs were already closed because
they didn't listen shows that the maintainer actually cares about the
repository, and that I should be able to just follow the rules.

I hope to learn the process of actually getting a PR through a real review,
and the steps CodePath is teaching. Because the issue is on the easier,
straightforward side, I'll be able to learn the mechanics more than the
coding. I think the scout held up. My one disagreement was
yunaremaia/taintrace#81, which my rubric accepted in all three scans: the
repository looks naked, with a lot of AI-generated issues and comments, and it
doesn't look like the maintainer actually cares about it as much, so I dropped
it.

Claim comment: https://github.com/nvaccess/nvda/issues/20159#issuecomment-6183111660

**Toolkit**

- Tool: issue-scout, in direct mode: I graded my Issue Selection site picks
  with `issue-scout: grade <URL>` after patching my rubric's scope check to
  `scope-feasible` with a difficulty note.
- What it produced: an accept on nvda#20159 with all six required and all
  three preferred checks passing, and a straightforward difficulty note
  (maintainer seanbudd had already decided the fix is a clearer error
  message).
- Difficulties: my rubric accepted yunaremaia/taintrace#81 in all three scans
  even though the repo looks naked, with a lot of AI-generated issues and
  comments.
- What changed: I added `yunaremaia/taintrace` to a blocklist in my search
  strategy's ranking filters so later scans drop it, and logged the dated
  friction line in `scan-notes.md`.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
