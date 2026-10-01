# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

Shimmy0530

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53#issuecomment-5923196419

````markdown
Plan for #53, built from my reproduction report above.

`(555) 123-4567` comes back unredacted and `detect()` returns `[]` for it; it should be replaced the same way `555-123-4567` is. The cause is in the `phone_us` pattern (`safety/pii_scrubber.py` line 16), and it has two parts:

- The separators are `[-.]?`, so the space after `)` stops the match.
- The leading `\b` sits before the optional `(`, so the paren can never be part of a match.

Running the pattern alone on `2f4e82f` shows both, plus a third format from `test_us_phone_formats` that step 2 of my report didn't reach, because the test stops at its first failing format:

```
'(555) 123-4567' []
'(555)123-4567' ['555)123-4567']
'+1 555 123 4567' []
Contact: +1 555 123 4567
```

What I'll change:

- Replace `phone_us` with `(?:\b\+?1[-. ]?)?(?:\(([0-9]{3})\)|\b([0-9]{3}))[-. ]?([0-9]{3})[-. ]?([0-9]{4})\b`. The area code is now either `(NNN)` or `\bNNN`, and a space is allowed as a separator.
- Remove the xfail markers from the four tests the issue names: `test_us_phone_number_redaction`, `test_us_phone_formats`, `test_detect_phone_pii`, and `test_phone_at_start_of_text`.
- Add one test asserting `scrub("Call me at (555)123-4567") == "Call me at [REDACTED]"`, so a stray `(` can't pass.

Not in scope:

- `test_mixed_pii_and_text` and the `street_address` pattern. That test fails because `street_address` matches `'5 years developing Python appl'`, not because of the phone pattern, so its xfail marker stays. My question above about whether it belongs to #53 is still open; unless told otherwise I'd raise it separately.
- `phone_intl`, `email`, `ssn`, and the `scrub()` / `detect()` code.
- Phone formats beyond the ones these four tests check, such as extensions and non-US numbers.
- The `+` left visible in `+[REDACTED]`, which the current pattern already does for `+1-555-123-4567`.

To test it, I'll re-run my report's steps:

- Step 1 should print `Call me at [REDACTED] or [REDACTED]`, and `detect()` should return a `phone_us` match for `(555) 123-4567`.
- Step 2 with `--runxfail` should drop to `1 failed`, the `street_address` one.
- `make check && make test-unit` should pass.

Two side effects I'll call out in the PR:

- Allowing spaces means a run like `100 200 3000` is now redacted.
- The `+` in `+1 555 123 4567` stays visible as `+[REDACTED]`. The current pattern does the same with `+1-555-123-4567`.

I haven't tested on Python 3.11 yet (CI's version); I'll confirm that before opening the PR.
````

---

## Your branch

**Branch**

fix/53-parenthesized-phone

**Evidence**

My Unit 2 repro steps, re-run in the same environment (Windows 11, Git Bash, Python 3.14.7, the
repo's `.venv`). Before is `2f4e82f`, upstream `main` and the commit my repro ran on; I re-ran it in
a `git worktree` and it matches what I posted in Unit 2. After is `e6b5758` on
`fix/53-parenthesized-phone`. The structlog `pii_detected` lines are trimmed, as in the repro.

Step 1, the issue snippet plus a dashed control:

```
$ .venv/Scripts/python -c "
from safety.pii_scrubber import PIIScrubber
s = PIIScrubber()
print(s.scrub('Call me at (555) 123-4567 or 555-123-4567'))
print(s.detect('Call me at (555) 123-4567'))
print(s.detect('Call me at 555-123-4567'))
"
```

Before (`2f4e82f`):

```
Call me at (555) 123-4567 or [REDACTED]
[]
[{'type': 'phone_us', 'value': '555-123-4567', 'start': 11, 'end': 23}]
```

After (`e6b5758`):

```
Call me at [REDACTED] or [REDACTED]
[{'type': 'phone_us', 'value': '(555) 123-4567', 'start': 11, 'end': 25}]
[{'type': 'phone_us', 'value': '555-123-4567', 'start': 11, 'end': 23}]
```

Step 2, the scrubber tests with the xfail markers ignored:

```
$ .venv/Scripts/python -m pytest tests/unit/test_pii_scrubber.py --runxfail -q --tb=line
```

Before (`2f4e82f`):

```
E   AssertionError: assert '[REDACTED]' in 'Call me at (555) 123-4567'
E   AssertionError: assert '[REDACTED]' in 'Contact: (555) 123-4567'
E   assert 0 > 0
E   AssertionError: assert '[REDACTED]' in '(555) 123-4567 is my phone number.'
E   assert 'Python' in "\n        Professional Background:\n        I worked at TechCorp for [REDACTED]ications.\n        Email: [REDACTED]\n        Phone: [REDACTED]\n        SSN: [REDACTED]\n        I'm skilled in AWS and Kubernetes deployment.\n        "
5 failed, 20 passed in 0.23s
```

After (`e6b5758`):

```
E   assert 'Python' in "\n        Professional Background:\n        I worked at TechCorp for [REDACTED]ications.\n        Email: [REDACTED]\n        Phone: [REDACTED]\n        SSN: [REDACTED]\n        I'm skilled in AWS and Kubernetes deployment.\n        "
1 failed, 25 passed in 0.28s
```

The four phone failures are gone. The one left is `test_mixed_pii_and_text`, the `street_address`
over-redaction, which the plan leaves out of scope. There are 26 tests instead of 25 because of the
new `test_parenthesized_phone_fully_redacted`. Run normally (markers active, as CI runs it), the
file gives `25 passed, 1 xfailed`, and `make test-unit` gives `380 passed, 49 xfailed`.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. `--limit 3` smoke run: pkg-01 `reject`, pkg-02 `accept`, pkg-03 `accept`, all three agreeing
   with gold: `agreement: 3/3 scored items`. Partial runs print no bar verdict.
2. `--include-calibration --only calib-01,calib-02,calib-03,calib-04`: `agreement: 0/0 scored items`,
   because calibration packages are never scored. All 4 agreed per item:

   ```
   calib-01  calib     accept  accept   yes
   calib-02  calib     reject  reject   yes
   calib-03  calib     reject  reject   yes
   calib-04  calib     reject  reject   yes
   ```

3. Full run, saved with `--save-run`: `agreement: 19/20 scored items  (bar: 18/20: PASS)`

**Package analysis**

`pkg-20`: ghostty-org/ghostty#11261, category `thread-convention`.

- **Gold label:** `reject`, "excellent bounded plan that follows the thread's direction, but the
  comment contains no AI-use disclosure and ghostty's stated policy requires disclosing all AI
  usage; every package here is treated as AI-assisted work"
- **Rubric decision:** `accept`: `pkg-20  thread-convention  reject  accept   NO     graded accept`

Why the rubric read it that way: the plan is strong on every other check. Its cause is backed by a
control step ("the same first test with the hyperlink start removed (no page growth mid-print)
passes"), its scope is bounded, and it names its own open risk. That leaves only
`comment-respects-thread` able to reject it. That check fails a comment that "does not break an
explicit requirement in the contribution policy (for example a required AI-use disclosure)", and
the policy line says "All AI usage in any form must be disclosed". But nothing in the package says
AI was used, so the grader most likely read the disclosure rule as conditional ("if you used AI")
and treated the comment's silence as compliance. The gold label works from the opposite premise,
"every package here is treated as AI-assisted work", and my rubric never states it. The miss is in
my pass condition, not in the grader: it names the disclosure as an example of a requirement
without saying that a missing disclosure fails.

**Check rationale**

| `cause-fits-repro` | The plan's stated cause (Diagnosis), read against every step and the Actual line of the repro evidence, including any step that changes one condition (a flag, an input, a component removed) | The stated cause explains the observed result of every repro step, and no repro step contradicts it. A cause taken from the thread or another commenter passes only if the repro evidence itself backs it; authority alone is not evidence. | required |

Why it reads this way:

- **"explains the observed result of every repro step":** the activity's sample rubric only asked
  whether the diagnosis "says a cause". Every wrong-cause plan says one, confidently. calib-03
  (sharkdp/bat#3831) blames the pager's key bindings, but its own repro shows step 3,
  `--paging=never` with color on, taking 25.8 s, while color off in the same pager takes under
  0.3 s. Checking the cause against every step is what catches that.
- **"including any step that changes one condition":** steps that flip one variable are the ones
  that can contradict a cause, so the evidence column points the grader straight at them.
- **"authority alone is not evidence":** calib-03's cause came from a commenter in the thread.
  I rejected letting a thread-sourced cause pass on its source, and pass it only when the repro
  backs it.
- I paired this with the procedure reading the repro evidence before the plan ("a plan states its
  cause confidently, and reading it first makes the evidence look like it agrees"). The result was
  `wrong-cause 4/4`.

**Trade-offs**

I narrowed `comment-respects-thread` to directions stated by maintainers (OWNER, MEMBER,
COLLABORATOR) plus explicit contribution-policy requirements. Non-maintainer chatter in a thread
can no longer fail a plan comment, which protects the clear accepts (`clear-accept 7/7`) and still
caught pkg-04, whose comment "never engages the owner's explicit direction in-thread" (`pkg-04
thread-convention  reject  reject   yes`).

The case I accept it will miss is pkg-20: a policy requirement that only applies under a condition
the package never shows, like AI use. My check names the disclosure but doesn't say that a missing
disclosure fails, so `thread-convention` came in at `1/2`. The fix would be one sentence in the
pass condition: "If the policy requires an AI-use disclosure, the comment must contain one." I
chose not to make it. The run already clears the bar at 19/20 with every category floor met, and
the change would need another full run to confirm it didn't flip a clear accept whose repo also
has a disclosure rule.
