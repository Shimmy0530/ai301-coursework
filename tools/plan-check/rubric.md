# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `cause-fits-repro` | The plan's stated cause (Diagnosis), read against every step and the Actual line of the repro evidence, including any step that changes one condition (a flag, an input, a component removed) | The stated cause explains the observed result of every repro step, and no repro step contradicts it. A cause taken from the thread or another commenter passes only if the repro evidence itself backs it; authority alone is not evidence. | required |
| `one-bounded-change` | The plan's scope statement (in scope / not in scope) and its list of changes, read against the reproduced bug | Every listed change is needed to fix the reproduced bug or to test that fix. Fails if any change is unrelated to it: a refactor, rename, dependency bump, new feature, or fix for a different bug. | required |
| `stranger-can-start` | The plan's list of changes or approach | Each change names where it happens (file, function, or module) and what the edit is. Someone new to the repo could begin the first change without asking the author. Fails if any change is "investigate", "refactor as needed", "TBD", or has no location. | required |
| `test-shows-fix` | The plan's test plan, read against the repro evidence's steps | The test re-runs at least one repro step, or a test that encodes that step's input, and states the observable result expected after the fix. That result would differ if the bug were still present. | required |
| `comment-respects-thread` | The plan comment, read against thread entries by maintainers (author association OWNER, MEMBER, or COLLABORATOR) and the repo-facts contribution policy line | The comment does not propose an approach a maintainer has ruled out or contradict a direction a maintainer stated, and does not break an explicit requirement in the contribution policy (for example a required AI-use disclosure). Passes when no maintainer has stated a direction and the policy states no requirement that applies. | required |
| `unknowns-stated` | The plan's risks or unknowns, read against what the repro evidence did not test | The plan claims no more certainty than the repro evidence supports, and names what remains unverified. | preferred |

## Verdict rule

Accept if every required check passes. Reject if any required check
fails. `unclear` on a required check counts as fail. Preferred checks
never change the verdict.
