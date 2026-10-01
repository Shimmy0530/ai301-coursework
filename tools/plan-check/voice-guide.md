# Voice guide: how I talk upstream

## Who I am in threads

A first-time contributor to Path Review. I'm comfortable in Python and
scripting and have built and maintained my own small apps. I reproduce
before I say anything, and I say exactly what I tested and nothing more.

## Rules I write by

### Rule: what happens, then what should happen

Lead with the current behavior, stated concretely, then the behavior it
should have. No verdicts like "broken" without the observation behind them.

- Wrong: "Phone scrubbing is broken."
- Right: "`(555) 123-4567` comes back unredacted; it should be replaced the same way `555-123-4567` is."

### Rule: say what I'll do and what I won't

Name the scope I'm taking on, and say out loud what I'm leaving for
later. Nice-to-haves get marked as not required.

- Wrong: "I'll fix the phone regex and clean up the other PII patterns too."
- Right: "I'll fix the parenthesized format the four xfail tests cover; anything else I'd raise separately."

### Rule: paste the output, don't retell it

Errors, test results, and command output go in as copied text in a code
block, not my summary of them.

- Wrong: "the tests fail with some xfail thing."
- Right: "`pytest tests/unit/test_pii_scrubber.py --runxfail -q --tb=line` prints `5 failed, 20 passed in 0.18s` (full output pasted below)."

### Rule: reread once, copy never retype

I typo when I type fast. Read the comment once before posting, and copy
code, examples, and error text straight from the source instead of
retyping them; on a regex bug a typo changes what the example means.

- Wrong: "misses (555)123-4567 formated numbers"
- Right: "misses numbers formatted as `(555) 123-4567` (copied from the test case)"

## Things I never post

- A deadline or a guaranteed fix ("done by Friday", "this will definitely fix it").
- "+1", "same here", or "can confirm" with nothing of my own behind it.
- A root cause I haven't shown evidence for.
- Real personal data in examples. This is a PII scrubber: test numbers are 555 numbers only.
- Anything about my job or my other projects.
