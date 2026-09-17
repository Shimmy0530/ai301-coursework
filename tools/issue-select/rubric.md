# Rubric: is this a good first issue?

Five `required` checks, one per failure mode that actually strands a
newcomer's first contribution: the repo is dead, the repo is read-only, the
work is not one newcomer-sized piece, somebody else is already on it, or the
project's contribution policy rejects the way I work. Any one of them is
enough to sink an issue however good the rest of it looks, so all five gate
the verdict. Three `preferred` checks rank the issues that survive.

Every date threshold is measured against the capture date stated in the
bundle (eval mode), or against today (live mode).

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `repo-alive` | Repo facts: the dates in "last 5 default-branch commits" and "last push to any branch", compared with the capture date | At least one default-branch commit dated within 90 days of the capture date. A bot-authored commit counts only when it merges a human's pull request. Note this check deliberately does not gate on maintainer reply latency: healthy busy repos routinely leave most issues unanswered, so latency is a preferred signal below rather than a gate. | required |
| `repo-not-archived` | Repo facts: the `archived:` flag on the repo line | `archived: no`. An archived repo is read-only and cannot take a pull request at all. | required |
| `scope-newcomer-sized` | The issue title and body, plus any comment from an Owner, Member, or Collaborator in the thread | Fails if any of: the body is a list of links to *other* GitHub issues meant to be split into separate PRs (a tracking issue), rather than describing one piece of work directly; an Owner/Member/Collaborator explicitly discourages it as too advanced, deeply architectural, or not beginner-friendly, or the thread shows a real design question left genuinely unresolved (no maintainer decision, and prior linked PRs on it closed unmerged); it's a usage/support question, not a change request. A maintainer diagnosing a bug and naming concrete causes or fixes (even ones mentioning threads, concurrency, or specific files) is a help, not a scope failure — that is the normal shape of a well-scoped bug report. Otherwise passes — a terse body, several related edits toward one goal, or an unpolished writeup are not scope failures. | required |
| `unclaimed` | Repo facts: "this issue: assignees" and "linked PRs" with each PR's state; plus the comment thread | Passes when all three hold: assignees is none; no linked pull request is in `open` state; and no Owner, Member, or Collaborator comment reserves the issue for a named person. A linked pull request that is closed and unmerged is an abandoned attempt, not a claim, and a bare claim comment with no open pull request behind it does not block. | required |
| `policy-allows-ai-assist` | Repo facts: the "contribution policy" line, covering CONTRIBUTING.md, the contributor docs it links to, any AI policy file, and PR templates | Fails only on an outright prohibition of AI-assisted contributions, such as "we do not accept AI-generated code or documentation". Conditions are terms to follow, not bans, and pass: disclosure, personally understanding the change, testing it, human review of AI output. No stated policy passes. | required |
| `maintainer-answers` | Repo facts: "maintainer first-response sample", plus author_association on the comments in this thread | At least one sampled issue got a first response from an Owner, Member, or Collaborator, or one of them has commented in this thread. | preferred |
| `newcomer-signposted` | The issue labels and body | Carries a good-first-issue, help-wanted, or easy label, or the body names the files to touch or lists acceptance criteria. | preferred |
| `still-shipping` | Repo facts: "latest release" and its date, compared with the capture date | A release published within 12 months of the capture date. Never a gate: plenty of live projects publish no releases at all. | preferred |

## Verdict rule

`accept` if and only if every `required` check grades `pass`. A single
`required` check grading `fail` or `unclear` produces `reject`. `unclear`
counts as `fail`: a first issue whose evidence cannot be verified is not a
first issue to take.

`preferred` checks never change the verdict. They rank the issues that were
accepted: among accepted candidates prefer the one passing more preferred
checks, and name which ones in the summary.
