# Evidence guide: where proof lives in a reproduction package

Each section maps one proof family to its rubric check(s): where to
look in an eval bundle and in live mode, and what good looks like.

## Environment

Rubric check: `env-recorded`.

- Where it lives:
  - Eval bundle: the environment line or block in the candidate repro
    report, read against the issue body (the version, OS, and setup
    the reporter used) and the repo-facts block (latest release).
  - Live: the environment section of the student's repro draft, read
    against the issue body and the repo's latest release on GitHub.
- What good looks like: OS, tool version, and every factor the issue
  says changes the behavior (driver, build profile, browser language,
  shell, install method) are all named. If the version or platform
  differs from the issue's, the report says so in words ("issue is on
  13.0.0, I tested 15.2.0"). A missing record, or a silently older or
  different version, is a fail even when the rest is strong.

## Steps

Rubric check: `steps-rerunnable`.

- Where it lives:
  - Eval bundle: the steps in the candidate repro report, plus any
    input files, configs, or commands quoted in it; compare against
    the trigger in the issue body.
  - Live: the steps in the student's repro draft and anything it
    quotes or links publicly. Files in the student's working
    directory that the draft does not quote do not count.
- What good looks like: starting from a clean install or a public
  checkout, a stranger can run each step using only what the comment
  shows, and the steps reach the exact trigger the issue names (the
  same flag, input shape, or setting). Terse is fine. An input file
  may be pasted or described, as long as the description is precise
  enough to recreate it ("an env.yml with a valid dependencies list
  plus a category section" is enough). Fail when an input is private
  or unshared ("our monorepo", "my config"), or the steps omit the one
  condition the issue depends on.

## Behavior shown

Rubric check: `behavior-matches-issue`.

- Where it lives:
  - Eval bundle: the output excerpts, logs, exit codes, and described
    screenshots in the candidate repro report, compared to the
    symptom quoted in the issue body.
  - Live: the same artifacts in the student's repro draft, compared
    to the issue body and any maintainer notes in the thread.
- What good looks like: the artifact shows the issue's own failure,
  matched on kind (panic vs graceful error, runtime vs compile error,
  crash vs garbled output), message, and exit behavior. A control run
  (the same steps without the trigger, behaving correctly) strengthens
  it. Watch for adjacent symptoms: a changed input that yields a
  different error, or output showing only that the tool starts and
  runs. For a report that says it could not reproduce, look for steps
  that follow the issue's stated trigger conditions and a named
  difference from the reporter's setup; an admitted doubt about
  whether the trigger was reached is honesty, not a fail. A report
  that claims it DID reproduce gets no such allowance: its artifact
  must show the issue's symptom.

## Honesty

Rubric check: `claims-backed`.

- Where it lives:
  - Eval bundle: every claim sentence in the candidate claim comment
    and repro report (reproduced, confirmed, root cause, verified,
    guaranteed, also happens on X), each paired with the artifact
    that should back it.
  - Live: the same sentences in the student's drafts.
- What good looks like: each outcome claim (reproduced or not, cause,
  certainty, scope) points at something shown, and the stated outcome
  is what the artifacts show. Secondary observations stated plainly,
  such as "dropping -r gives the correct line numbers", do not need
  their own pasted output. Confidence words
  ("guaranteed", "verified this race condition", "on two machines")
  with nothing shown are a fail, as is a conclusion the report's own
  artifact contradicts. An honest cannot-reproduce passes: it says
  what was tried, shows the result, and names what differed from the
  reporter's setup.

## Comms

Rubric checks: `claim-specific`, `ai-policy-met`, `template-asks`
(preferred).

- Where it lives:
  - Eval bundle: the candidate claim comment read against the issue;
    the repo-facts block's `contribution policy` line (AI-use rules)
    and `bug reports` line (template asks), read against both
    comments.
  - Live: the student's claim draft against the issue; the repo's
    CONTRIBUTING, AI policy file, and issue templates on GitHub. In
    Path Review, the policy lives at `docs/CONTRIBUTING.md`.
- What good looks like:
  - Claim: names this issue's symptom, trigger, or component and a
    modest next step ("I'll reproduce on the current release and
    post what I find"). Boilerplate that fits any issue, a bare +1,
    or promises ("fix in 2 days, guaranteed") fail.
  - AI policy: treat the package as AI-assisted. If the policy
    requires disclosure, the comments name the tool and the extent
    of help. If it requires comments in the author's own words, the
    comments read as a person's specific account, not generated
    filler. No policy, or a permissive one with no disclosure rule,
    passes.
  - Template: the report covers what the bug-report template asks
    for, in any order or format.
