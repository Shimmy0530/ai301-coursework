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

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```
[
  {"item":"https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53","checks":[
    {"name":"repo-alive","grade":"pass","evidence":"default-branch commits today 2026-09-16"},
    {"name":"repo-not-archived","grade":"pass","evidence":"isArchived: false"},
    {"name":"scope-newcomer-sized","grade":"pass","evidence":"single regex fix in pii_scrubber.py with repro script and 4 named failing tests"},
    {"name":"unclaimed","grade":"pass","evidence":"assignees: none, comments: none, no linked/mentioned PRs"},
    {"name":"policy-allows-ai-assist","grade":"pass","evidence":"CONTRntain no AI-use language"},
    {"name":"maintainer-answers","grade":"pass","evidence":"Aburke225 (COLLABORATOR) replied on issues #52 and #43"},
    {"name":"newcomer-signposted","grade":"pass","evidence":"carries good-first-issue label; names pii_scrubber.py and 4 tests"},
    {"name":"still-shipping","grade":"fail","evidence":"gh release lis
  ],"verdict":"accept"},
  {"item":"https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69","checks":[
    {"name":"repo-alive","grade":"pass","evidence":"default-branch com
    {"name":"repo-not-archived","grade":"pass","evidence":"isArchived: false"},
    {"name":"scope-newcomer-sized","grade":"pass","evidence":"single fallback-path fix in output_parser.py, named xfail test to remove"},
    {"name":"unclaimed","grade":"pass","evidence":"assignees: none, cooned PRs"},
    {"name":"policy-allows-ai-assist","grade":"pass","evidence":"no AI-use restriction found"},
    {"name":"maintainer-answers","grade":"pass","evidence":"Aburke225 (COLLABORATOR) replied on issues #52 and #43"},
    {"name":"newcomer-signposted","grade":"pass","evidence":"good-firsarser.py and its test file"},
    {"name":"still-shipping","grade":"fail","evidence":"no releases published"}
  ],"verdict":"accept"},
  {"item":"https://github.com/codepath/pathreview-ai301-fa26-s3/issues
    {"name":"repo-alive","grade":"pass","evidence":"default-branch commits today 2026-09-16"},
    {"name":"repo-not-archived","grade":"pass","evidence":"isArchived: false"},
    {"name":"scope-newcomer-sized","grade":"pass","evidence":"single .faithfulness_checker.py with repro script"},
    {"name":"unclaimed","grade":"pass","evidence":"assignees: none, comments: none, no linked/mentioned PRs"},
    {"name":"policy-allows-ai-assist","grade":"pass","evidence":"no AI-use restriction found"},
    {"name":"maintainer-answers","grade":"pass","evidence":"Aburke225 ues #52 and #43"},
    {"name":"newcomer-signposted","grade":"pass","evidence":"good-first-issue label; names the file and one failing test"},
    {"name":"still-shipping","grade":"fail","evidence":"no releases published"}
  ],"verdict":"accept"},
  {"item":"https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57","checks":[
    {"name":"repo-alive","grade":"pass","evidence":"default-branch commits today 2026-09-16"},
    {"name":"repo-not-archived","grade":"pass","evidence":"isArchived:
    {"name":"scope-newcomer-sized","grade":"pass","evidence":"single filter fix in tech_detector.py with repro script and 2 named tests"},
    {"name":"unclaimed","grade":"pass","evidence":"assignees: none, comments: none, no linked/mentioned PRs"},
    {"name":"policy-allows-ai-assist","grade":"pass","evidence":"no AI
    {"name":"maintainer-answers","grade":"pass","evidence":"Aburke225 (COLLABORATOR) replied on issues #52 and #43"},
    {"name":"newcomer-signposted","grade":"pass","evidence":"good-first-issue label; names file and 2 tests"},
    {"name":"still-shipping","grade":"fail","evidence":"no releases pu
  ],"verdict":"accept"},
  {"item":"https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73","checks":[
    {"name":"repo-alive","grade":"pass","evidence":"default-branch com
    {"name":"repo-not-archived","grade":"pass","evidence":"isArchived: false"},
    {"name":"scope-newcomer-sized","grade":"pass","evidence":"two-file doc/config sync, 1-2h estimate"},
    {"name":"unclaimed","grade":"pass","evidence":"assignees: none, cooned PRs"},
    {"name":"policy-allows-ai-assist","grade":"pass","evidence":"no AI-use restriction found"},
    {"name":"maintainer-answers","grade":"pass","evidence":"Aburke225 (COLLABORATOR) replied on issues #52 and #43"},
    {"name":"newcomer-signposted","grade":"pass","evidence":"good-firses"},
    {"name":"still-shipping","grade":"fail","evidence":"no releases published"}
  ],"verdict":"accept"}
]

```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

**Issue analysis**

[One scored issue, identified by id (`issue-01` through `issue-20`; the `calib-`
issues are not scored). State your rubric's decision, the gold label, and the
reasoning that produced your rubric's result.]

**Check rationale**

[One check from the `rubric.md` uploaded to `tools/issue-select/`, quoted as it is
currently written, with the reasoning behind its current form.]

**Trade-offs**

[What the quoted check gives up. Any one of these is a complete answer: an issue whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three:

1. The issue's fit to your interests and to the time available.
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
3. The anticipated difficulty in claiming it.]

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
