# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

Shimmy0530

---



## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53#issuecomment-5806315655

````markdown
Hi, I'd like to work on this as my first contribution to PathReview.

Per the issue, `(555) 123-4567` passes through `scrub()` unredacted and `detect()` returns `[]` for it, while `555-123-4567` is redacted. Both formats should be caught.

My next step is to set up from `docs/SETUP.md`, run the issue's snippet and the four named tests in `tests/unit/test_pii_scrubber.py` on current `main`, and post what I see here before changing anything. I'm keeping the fix to the parenthesized format those tests cover, and I'll check that a looser pattern doesn't start redacting text that isn't a phone number. Any other formats I'd raise separately.
````

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53#issuecomment-5806553206

````markdown
Reproduction report for #53.

Result: reproduced. `(555) 123-4567` is not redacted by `scrub()` and not found by `detect()`, while `555-123-4567` is handled by both. I also found a fifth test marked for this issue whose failure is not a phone-number problem (details at the end).

**Environment**

- Code: my fork at `2f4e82f` (same as current `main`)
- OS: Windows 11 Home 10.0.26200, 64-bit, commands run in Git Bash
- Python: 3.14.7 in the `.venv` built by `make setup` (CI uses 3.11; I also got the same snippet output earlier on 3.13.14)
- Setup: followed `docs/SETUP.md` (`.env` from `.env.example`, `docker compose up -d`, `make setup`). This bug only needs the Python venv; Postgres and Redis aren't involved.
- API key: I left `.env` at its defaults. `docs/SETUP.md` step 2 says to "set your OPENROUTER_API_KEY (required for AI features)", but `.env.example` has no such variable and defaults to `LLM_PROVIDER=mock`, so no key was needed here. #73 already tracks this mismatch for `README.md`; `docs/SETUP.md` line 47 carries the same stale instruction.

**Steps**

1. From the repo root, run the issue's snippet plus a dashed-format control:

```
$ .venv/Scripts/python -c "
from safety.pii_scrubber import PIIScrubber
s = PIIScrubber()
print(s.scrub('Call me at (555) 123-4567 or 555-123-4567'))
print(s.detect('Call me at (555) 123-4567'))
print(s.detect('Call me at 555-123-4567'))
"
Call me at (555) 123-4567 or [REDACTED]
[]
[{'type': 'phone_us', 'value': '555-123-4567', 'start': 11, 'end': 23}]
```

(structlog `pii_detected` lines trimmed from the output.)

2. Run the scrubber tests with the xfail markers ignored:

```
$ .venv/Scripts/python -m pytest tests/unit/test_pii_scrubber.py --runxfail -q --tb=line
E   AssertionError: assert '[REDACTED]' in 'Call me at (555) 123-4567'
E   AssertionError: assert '[REDACTED]' in 'Contact: (555) 123-4567'
E   assert 0 > 0
E   AssertionError: assert '[REDACTED]' in '(555) 123-4567 is my phone number.'
E   assert 'Python' in "\n        Professional Background:\n        I worked at TechCorp for [REDACTED]ications.\n        Email: [REDACTED]\n        Phone: [REDACTED]\n        SSN: [REDACTED]\n        I'm skilled in AWS and Kubernetes deployment.\n        "
5 failed, 20 passed in 0.18s
```

(Only the `E` lines and the summary kept.)

**Expected vs actual**

- Expected: `(555) 123-4567` comes back as `[REDACTED]` from `scrub()`, and `detect()` returns a `phone_us` match for it, the same as the dashed format.
- Actual: the parenthesized number passes through unchanged and `detect()` returns `[]`. The first four failures above are the four tests the issue names.

**Fifth test marked for #53**

`test_mixed_pii_and_text` also carries `reason="issue #53: ..."`, but its failure is over-redaction, not a missed phone number: `"Python"` is gone from the output. `detect()` shows which pattern did it:

```
$ .venv/Scripts/python -c "
from safety.pii_scrubber import PIIScrubber
print(PIIScrubber().detect('I worked at TechCorp for 5 years developing Python applications.'))"
[{'type': 'street_address', 'value': '5 years developing Python appl', 'start': 25, 'end': 55}]
```

So a phone-only fix won't make that test pass, and its marker can't be removed with the other four unless the `street_address` match is addressed too. I'd like to know whether that's intended as part of #53 or should be split out before I go further.
````

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. `--only pkg-02,pkg-09,pkg-20` (smoke run on the riskiest three): `agreement: 3/3 scored items`
2. First full run: `agreement: 14/17 scored items`, with `3 item(s) errored; fix and re-run.` pkg-04, pkg-07 and pkg-11 contain emoji, and Windows piped the prompt to `claude -p` as cp1252, so those three never got graded. Of the 17 that did, three disagreed: pkg-03 (`failed: claims-backed`), pkg-05 (`failed: steps-rerunnable, template-asks`) and pkg-09 (`failed: behavior-matches-issue`).
3. `--only calib-01,calib-02,calib-03,calib-04 --include-calibration` with `PYTHONUTF8=1` (encoding fix check, never scored): 4 of 4 calibration packages agreed, `agreement: 0/0 scored items`.
4. Final full run after revising three pass conditions, saved with `--save-run`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`

**Package analysis**

`pkg-09`: sharkdp/fd#2033, category `clear-accept`.

- **Gold label:** `accept`, "honest cannot-reproduce: real attempt at the argument-size reordering with marker-order artifacts, names what differed (uniform name lengths, 2 MiB ARG_MAX) and what a triggering setup likely needs"
- **Rubric decision:** `reject` on my first full run (`failed: behavior-matches-issue`), `accept` in the final run: `pkg-09    accept  accept   yes`. It also came back `accept` in my first 3-package run with the same rubric, so the first version of the check was unstable on this package.

Why the rubric read it that way: the report says "Result: I could NOT reproduce scenario 2" and then admits its own setup may have missed the trigger: "my padding approach may not achieve that, since fd appears to flush both command buffers at the same file-count boundary on this input (uniform name lengths)." My first cannot-reproduce clause said "pass if the artifacts show the attempt ran the issue's actual trigger." The report's own words say it can't be sure the trigger ran, so a strict read failed it for being honest. That's backwards: an honest cannot-reproduce that names what differed is exactly what the gold label rewards. The revised check passes a cannot-reproduce when its steps follow the issue's stated trigger conditions and it names what may have differed, and leaves judging the honesty of that account to `claims-backed`.

**Check rationale**

| `behavior-matches-issue` | The report's artifacts (output excerpts, logs, exit codes, described screenshots) read against the exact symptom in the issue body. Evidence guide: Behavior shown. | Pass if an artifact shows the issue's own symptom (same error kind, same failure mode, same exit behavior), not an adjacent one such as a graceful validation error where the issue reports a crash. For a report that states it could NOT reproduce, pass if its steps follow the trigger conditions the issue states and the report names what may have differed from the reporter's setup; an admitted uncertainty about whether the trigger was reached is honesty, not failure (whether the account is honest is graded by `claims-backed`). This cannot-reproduce allowance never applies to a report that claims it reproduced the issue. Fail if the artifacts show a different behavior, show only that the tool runs, or are absent. | required |

Why it reads this way:

- **"same error kind, same failure mode, same exit behavior":** the wrong-target packages all look like reproductions. pkg-02 shows "a graceful arg-validation error (exit 1), narrated as the reported capacity-overflow crash (exit 101)" and calib-03 shows an HCL syntax error instead of the issue's panic. Matching on the kind of failure, not on "an error appeared", is what catches them.
- **"show only that the tool runs":** pkg-14's artifacts are a version banner and a session list; nothing shows the blank pane. Output that exists isn't output that shows the bug.
- **The cannot-reproduce clause is the revision.** It used to say "pass if the artifacts show the attempt ran the issue's actual trigger", which failed pkg-09 (see Package analysis). I rewrote it to accept a cannot-reproduce that follows the issue's trigger conditions and names what may have differed, and I moved the honesty judgment to `claims-backed` so one check isn't doing two jobs.
- **"never applies to a report that claims it reproduced the issue":** I added this with the loosening so the new allowance can't become a back door for wrong-target reports, which all claim success.

**Trade-offs**

Loosening `behavior-matches-issue` could have let a wrong-target package through, so I added the calibration packages to the confirming run as canaries and checked that every package the change could touch stayed rejected. From `eval-run.txt`:

```
pkg-02    reject  reject   yes
pkg-08    reject  reject   yes
pkg-16    reject  reject   yes
pkg-17    reject  reject   yes
calib-03  reject  reject   yes
```

and `wrong-target 4/4` on the categories line. They held because each of them claims it reproduced the bug, and the new clause only applies to reports that say they could not.

Case it will miss: a report that says "could not reproduce" and follows the issue's steps, but on a setup that could never trigger the bug (an old version, or the wrong platform) and names some other difference as the likely reason. The cannot-reproduce allowance would pass it on this check. The only thing between it and an `accept` is `env-recorded` catching the unstated version or platform gap; if the report states the gap but draws the wrong conclusion from it, my rubric accepts a useless report.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.