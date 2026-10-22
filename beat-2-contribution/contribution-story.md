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
**Pull Request:** https://github.com/nvaccess/nvda/pull/21045

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

**Unit 7: (2026-10-21) Installed the ship-gate on my PR doorway, wrote my NVDA manifest, and shipped nvaccess/nvda#21045 after two blocks**

The gate's first run on my real branch blocked: my local `master` was a week
stale, so the diff showed 104 files instead of 3, and nothing showed the bug
before the fix. The second run blocked on a copyright header change I hadn't
put in my plan, so I recorded it as a deviation. After I pasted every output
into code blocks instead of retelling it, and fixed my voice guide's title
rule to follow NVDA's plain-sentence titles, the gate passed, and the PR
opened through it. My manifest runs five checks from NVDA's contributing
guide, with `runlint.bat` excluded because it rewrites files.

**Unit 8: (2026-10-22) Installed the PR-monitor, wrote my playbook, and ran it on nvaccess/nvda#21045**

The free `--state` read classed my PR as FEEDBACK-IN, but the only comment was
the Codex bot's no-findings summary, so my playbook now says a bot summary
with no findings isn't feedback. My first full run refused because the
playbook was still the template; once I filled it (7 days of quiet before a
nudge, based on NVDA's "within a couple of days" and 0-4 days to first review
in their last 15 merged PRs), it drafted a wait note instead of a reply. I
took that move: nothing posted, and no nudge before about 2026-10-29.

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

When you create a portable copy of NVDA and type a drive letter with no
backslash after it, like `D:`, NVDA rejects it with a general "absolute path"
error that never says why. Rejecting it is correct: `D:` means the current
folder on drive D, not the root of the drive. The problem is the message.
It tells you the path "must start with a drive letter", and `D:` already
does. Since #19666 its own example is `C:`, which is also a bare drive and
gets the same error.

**Expected Behavior**

NVDA should still reject `D:`, but the message should say what's wrong:
the drive letter needs a backslash after it, so `D:\` for the root of the
drive or `D:\folder` for a folder on it. The general message's example should
be `C:\`.

**Current Behavior**

On master, typing `D:` and pressing Continue shows:

```
Please specify the absolute path where the portable copy should be created. It must start with a drive letter (e.g. C:). It may include system variables (e.g. %temp%, %homepath%) as placeholders for parts of the path.
Current path: D:.
```

**Affected Components**

- `source/gui/installerGui.py`: `PortableCreaterDialog.onCreatePortable`,
  which checked the path and showed the error, and the new
  `_validatePortableDirectory` helper the check moved into.
- `tests/unit/test_installer.py`: where the new tests live.
- `user_docs/en/changes.md`: the change log entry.

**Toolkit**

- Tool: issue-scout in direct mode (`issue-scout: grade <URL>`), then reading
  the issue thread and the three closed PRs myself.
- What it produced: an accept with a straightforward difficulty note, and the
  maintainer's decision that the fix is a clearer message, not accepting `D:`.
- Difficulties: the scout grades whether an issue is a good pick, not what
  the bug actually is, and the issue had moved since it was filed: #19666
  changed the example from `C:\` to `C:`, so the issue's quoted text no longer
  matched master.
- What changed: I only understood the full problem after reproducing it and
  seeing the `C:` example myself. I asked about it on the thread, and seanbudd
  answered "Yes, please update the example", which added it to the fix.

---

## Reproduction Process

**Environment Setup**

- OS: Windows 11 (build 10.0.26300), Git 2.55.0.
- Project: nvaccess/nvda at `20ae5c856` (master, 2026-10-15), cloned with
  `git clone --recursive`, then `gh repo fork --remote` so `origin` is my
  fork Shimmy0530/nvda and `upstream` is nvaccess/nvda.
- Toolchain, per `projectDocs/dev/createDevEnvironment.md`: Visual Studio
  Build Tools 2026 with the components in the repo's `.vsconfig`
  (`setup.exe modify --config .vsconfig`), Python 3.13.15 64-bit from
  python.org (installed per-user with winget), uv 0.13.1.
- Setup: `uv sync`, then `scons source --all-cores` (finished with
  "scons: done building targets."), then `runnvda.bat` to run from source.

Challenges and fixes:

- Build Tools 2026 was already installed but was missing 8 of the 9
  components `.vsconfig` lists (C++ workload, Clang, ARM64 and ATL tools,
  Windows 11 SDK 10.0.22621). Importing `.vsconfig` into the installer added
  them.
- uv 0.11.19 was older than the 0.12.5 the repo's `ensureuv.ps1` requires;
  `uv self update` fixed it.
- `uv sync` refused uv's own Python download, because `pyproject.toml` sets
  `python-preference = "only-system"`, and my system 3.13 was the Microsoft
  Store's 3.13.14. Installing python.org 3.13.15 fixed it.
- `uv sync` then failed compiling `fast-diff-match-patch` until the Visual
  Studio C++ components finished installing.
- Launching `scons.bat` from Claude Code's shell failed three times with
  "'scons.bat' is not recognized", because that shell sets
  `NoDefaultCurrentDirectoryInExePath`. Calling it as `.\scons.bat` worked;
  in a normal Command Prompt plain `scons source` works.
- The clone lives in OneDrive, and OneDrive turned parts of `.venv` into
  read-only cloud placeholders, so `uv sync` failed with "Access is denied".
  Pinning the folder to the device (`attrib -U +P -R ".venv\*" /S /D`) fixed
  it the first time; the second time, deleting the broken wxPython
  `dist-info` folders and running `uv sync --reinstall-package wxpython` did.

**Steps to Reproduce**

1. Build and run NVDA from source on master `20ae5c856`: `scons source`,
   then `runnvda.bat`.
2. Create portable copy is hidden from the Tools menu when running from
   source (`source/gui/__init__.py:713`), so open Tools > Python console and
   run `import gui; gui.mainFrame.onCreatePortableCopyCommand(None)`.
3. In the Create Portable NVDA dialog, type `D:` as the folder and press
   Continue. Observed: an Error dialog reading

   ```
   Please specify the absolute path where the portable copy should be created. It must start with a drive letter (e.g. C:). It may include system variables (e.g. %temp%, %homepath%) as placeholders for parts of the path.
   Current path: D:.
   ```

**Reproduction Evidence**

- **Commit showing reproduction:** https://github.com/Shimmy0530/nvda/commit/35a4f5740 (adds the 10 tests; run against master's `installerGui.py` they fail 10 of 10 with `AttributeError`)
- **Screenshots/logs:** screenshot of the error dialog over the Create
  Portable NVDA dialog, taken 2026-10-15 on `20ae5c856`; the full error
  text is pasted in step 3 above.
- **My findings:** the bug reproduces on current master. The message has
  changed since the issue was filed: the issue quotes the example as `C:\`,
  and since commit `a8eae3d65` (#19666, 2026-02-23) it reads `C:`, which is
  itself a bare drive that gets this same error. I posted the reproduction
  and asked whether the clearer message should also change that example to
  `C:\`:
  https://github.com/nvaccess/nvda/issues/20159#issuecomment-6183684091

**Toolkit**

- Tool: repro-check, plus the voice-guard on the posting doorway. I posted
  the claim and the repro report through
  `claude "run this command: gh issue comment ... --body-file ..."` so the
  guard checked each one first.
- What it produced: the guard passed both comments. repro-check, run on the
  posted claim and report afterwards, said reject on one check:
  `env-recorded`, because my report never named my Windows version.
- Difficulties: the menu item I needed is hidden when NVDA runs from source,
  so I had to open the dialog from the Python console, and my first command
  there failed with `NameError: name 'gui' is not defined` until I imported
  it. My claim also said I'd reproduce "on the current alpha" when I actually
  used master.
- What changed: the comment was already posted, so I put the environment
  (Windows 11 build 26300, no add-ons) into the PR description instead, where
  reviewers read it.

---

## Solution Approach

**Analysis**

`PortableCreaterDialog.onCreatePortable` in `source/gui/installerGui.py`
expands the path with `os.path.expandvars`, then checks it with
`os.path.isabs`. For `D:` that check is correctly `False`, because `D:` with
nothing after the colon means the current folder on that drive. Every path
that fails it gets the same general message, so nothing tells the user the
missing backslash is the problem, and the message's example `C:` fails the
same check.

**Proposed Solution**

I followed seanbudd's decision and SaschaCowley's review on the closed PR
#20446. The checks move out of the dialog into a helper,
`_validatePortableDirectory`, that returns the absolute path or raises
`DisplayableError`, so it can be tested without the GUI. If
`os.path.splitroot` finds a drive but no root (`D:`, `D:folder`), the helper
raises a new message saying the drive letter must be followed by a
backslash. Everything else keeps the general message, with its example
changed to `C:\`.

Not changing: accepting `D:` or guessing a root for it, how valid paths are
handled, the "Create a new folder" option, the non-empty folder warnings, or
the User Guide (it doesn't quote this error).

**Implementation Plan**

1. Add `_validatePortableDirectory` to `source/gui/installerGui.py` with the
   empty, expand, `isabs` and `abspath` steps, the new bare-drive message, and
   `C:\` as the general example.
2. Change `PortableCreaterDialog.onCreatePortable` to call it and show errors
   with `e.displayError(self)`.
3. Add `Test_validatePortableDirectory` to `tests/unit/test_installer.py`:
   empty, `D:`, `c:`, `D:folder`, `folder`, `\folder`, `D:\`,
   `D:\portable\NVDA`, a UNC share and a system variable.
4. Add a Bug Fixes line to `user_docs/en/changes.md`.

Deviations:

- 2026-10-21: added `Shimmy0530` to the copyright headers of the two files I
  changed, as `projectDocs/dev/copyrightHeaders.md` asks. It wasn't in the
  plan, and the ship-gate's second run blocked on exactly that until I
  recorded it here and in `plan.md`.

**Toolkit**

- Tool: plan-check on my `plan.md`.
- What it produced: accept, with all five required checks passing. The
  preferred `unknowns-stated` check failed because the plan didn't say what I
  hadn't tested.
- Difficulties: I never posted a separate plan comment, because the approach
  was already settled on the thread. My plan-check's procedure didn't say
  what to do then, so it graded my claim in its place without a rule saying it
  should.
- What changed: I added an Unverified list to `plan.md` (`c:` and UNC only
  unit-tested, UNC copy creation never tried, the new message in English until
  translated), and patched plan-check's `procedure.md` with a step for the
  no-plan-comment case. Its eval still agreed 4/4 on the items I re-ran.

---

## Testing Strategy

Two observable checks prove the fix.

1. **My reproduction, re-run before and after.** Before, on master, `D:`
   showed the general message with "(e.g. C:)". After, from a `scons dist`
   build of my branch, I typed each input into Create portable copy myself:
   - `D:` and `D:folder` show "The drive letter must be followed by a
     backslash: use D:\ for the root of the drive, or D:\folder for a folder on
     it."
   - `folder` shows the general message, now with "(e.g. C:\)".
   - An empty path shows "Please specify a directory in which to create the
     portable copy."
   - `%temp%\nvdaPortableTest2` still created a portable copy: "Successfully
     created a portable copy of NVDA at ...\nvdaPortableTest2\NVDA".
2. **The repo's own checks, with the new tests.** The 10 tests in
   `Test_validatePortableDirectory`, run against master's `installerGui.py`:

   ```
   AttributeError: module 'gui.installerGui' has no attribute '_validatePortableDirectory'
   Ran 10 tests in 0.014s
   FAILED (errors=10)
   ```

   On my branch, the full suite (`rununittests`):

   ```
   Ran 1505 tests in 18.519s
   OK (skipped=5)
   ```

   ruff check, ruff format --check, pyright and `runcheckpot` all passed too.

Not tested by hand: `c:` and a UNC share (unit tests only), and creating a
portable copy on a UNC share.

**Toolkit**

- Tool: the ship-gate, running my `ship-manifest.md` (`uv sync`, ruff and
  pyright, `runcheckpot`, `rununittests`, `runlicensecheck`), plus a
  `scons dist` build for the manual test.
- What it produced: all five repo checks passed on every run, and
  `test_evidence.md` with each command's output copied verbatim.
- Difficulties: the dialog is hidden when NVDA runs from source, so testing
  that a real portable copy still gets created needed a full `scons dist`
  build. `runlicensecheck` prints only a package table with no summary line,
  so there was nothing short to paste.
- What changed: I added a before-and-after section to `test_evidence.md`,
  running the new tests against master's code to show they fail without the
  fix, after the gate blocked on the missing "before".

---

## Implementation Notes

**2026-10-15**

Claimed the issue, built NVDA from source and reproduced `D:` on master
`20ae5c856`. Noticed the message's example was now `C:` and asked about it on
the thread; seanbudd gave the claim a thumbs up and answered "Yes, please
update the example".

**2026-10-21**

Branched `fix/20159-bare-drive-portable-path` from upstream/master
`40e2df64c`. Committed the tests first (they fail on master), then the fix,
then the change log line, then the copyright headers. A wrong turn: my first
tests had single backslashes where Python needed double ones, and the test
file wouldn't even import (`SyntaxError: (unicode error)`). Ran the full
manifest, then tested a `scons dist` build by hand with `D:`, `D:folder`,
`folder`, an empty path and a real portable copy.

**2026-10-21**

Ran the ship-gate four times before shipping. Run 1 blocked because my local
`master` was a week stale, so the diff showed 104 files instead of 3, and
because nothing showed the before state. Run 2 blocked on the copyright
header change I hadn't put in the plan. Run 3 passed with voice notes (output
retold instead of pasted, unverified cases missing from Known issues). Run 4
passed clean. Opened the PR as a draft through the gated `gh pr create`, then
marked it ready for review.

**2026-10-22**

A Codex review bot reported no findings on `ff90fb210`. Installed the
PR-monitor and wrote my playbook; its run drafted a wait note, so I posted
nothing. Checked the PR against the Assignment 5 rubric before submitting.

**Toolkit**

- Tool: git and the NVDA build scripts (`scons.bat`, `rununittests.bat`),
  with the ship-gate on the PR doorway.
- What it produced: four commits that each tell one step: tests, fix, change
  log, headers.
- Difficulties: the stale local `master`, and OneDrive breaking `.venv`
  twice, which also blocked the gate's `uv sync` check while I was shipping.
- What changed: local `master` now tracks `upstream/master`, and the next
  clone goes outside OneDrive.

---

## Pull Request

**PR Link:** https://github.com/nvaccess/nvda/pull/21045

**PR Description:** It opens with one line: "This is my first contribution
to NVDA. I used Claude Code to help write it; I reviewed every change and did
the manual testing below myself." The rest is NVDA's template filled as
written: `Fixes #20159`, the summary, the user facing changes, the approach
(the helper from the #20446 review, `os.path.splitroot` for the bare drive),
the testing with before and after output pasted in code blocks, the
environment, what I didn't test by hand, and the checklist with each item
explained.

**Maintainer Feedback:**

- 2026-10-22: no maintainer feedback yet. A Codex review bot
  (`chatgpt-codex-connector`) reviewed commit `ff90fb210` and reported no
  findings. CI waits for a maintainer to approve it on a first PR.
- 2026-10-22: still no human review. My PR-monitor drafted a wait note, not a
  reply, under my 7-day threshold; I posted nothing.

**Status:** Awaiting review (marked ready for review on 2026-10-21).

**Toolkit**

- Tool: the ship-gate (my pr-precheck plus my `ship-manifest.md`) and the
  voice-guard, both on `gh pr create`; after it opened, the PR-monitor with my
  `playbook.md`.
- What it produced: two blocks and two passes on the manual doorway, then a
  pass at the door that opened #21045. The monitor's first full run drafted
  a wait note: no human review yet, under my 7-day threshold.
- Difficulties: the gate compared against a stale local `master`, and the
  hook run of `uv sync` hit OneDrive's read-only `.venv` and blocked twice
  before the PR could open. The voice-guard also stopped my draft on "`runlicensecheck`
  also exits 0." because that retold output instead of pasting it.
- What changed: I recorded deviations in `plan.md`, added the before state to
  the evidence, pasted every output in code blocks, and patched my voice
  guide's title rule: it required `type(scope):` titles, but NVDA uses plain
  sentences, so the rule now follows the repo's own convention.

---

## Learnings and Reflections

**Technical Skills Gained**

- Building NVDA from source: Visual Studio components from `.vsconfig`, uv,
  `scons source` and `scons dist`.
- Windows path rules: `D:` and `D:folder` are drive-relative, and
  `os.path.splitroot` tells a bare drive apart from a rooted path.
- Moving a GUI check into a function that raises `DisplayableError`, so it can
  be unit tested without mocking anything.
- Writing commits that each tell one step, with the failing test first.

**Challenges Overcome**

- The dev environment: eight missing Visual Studio components, an old uv, the
  wrong Python, and a shell that wouldn't run `scons.bat` without `.\`.
- OneDrive breaking `.venv`, twice.
- The gate blocking my own PR twice, and each block pointing at something real:
  a stale base branch, a missing before state, and a change I hadn't planned.

**What I'd Do Differently Next Time**

Clone outside OneDrive from the start. Update local `master` from upstream
before comparing anything against it. Put the Windows version in the repro
report the first time, and run the gate before I think I'm finished, not
after.

---

## Resources Used

- NVDA's contributing guide, `projectDocs/dev/contributing.md`: the PR rules
  and the AI policy ("All PRs must be submitted by a human").
- `projectDocs/dev/createDevEnvironment.md` and `buildingNVDA.md`: how I set up
  and built NVDA.
- The issue thread, https://github.com/nvaccess/nvda/issues/20159: seanbudd's
  "make the error message more clear" and "Yes, please update the example".
- SaschaCowley's review on the closed PR
  https://github.com/nvaccess/nvda/pull/20446: use `os.path.splitroot`, move
  the check into a helper that raises `DisplayableError`, don't mock
  `os.path`.
