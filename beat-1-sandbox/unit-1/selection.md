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

Ranked read-out

Accepted — #53 · PII scrubber fails to redact parenthesized US phone numbers (top pick)

- commits-alive pass — most recent human commit 2026-09-16T21:42:18Z by Aburke225, 5 days old.
- responds-to-issues pass — 0 comments, and opened 2026-09-10 by Aburke225 (author_association: COLLABORATOR); rubric passes this check on both clauses.
- unclaimed pass — no assignees, no cross-referenced PRs in the timeline, and 0 open PRs in the entire repo.
- newcomer-scope pass (preferred) — carries the good first issue label and the body has a runnable repro plus four named failing tests in tests/unit/test_pii_scrubber.py.

Accepted — #49 · Write a contributor onboarding guide

- commits-alive pass — same repo fact.
- responds-to-issues pass — 0 comments; opened 11 days ago by a collaborator.
- unclaimed pass — no assignees, no linked PRs, no claim comments.
- newcomer-scope pass (preferred) — no beginner label (devops, docs, tests, tier-3), but the body names the exact files to edit: docs/ONBOARDING.md (new) and docs/CONTRIBUTING.md. Passes on the second clause only; effort stated as 4–7 hours.

Accepted — #51 · Add a database migration validation step to CI

- commits-alive pass — same repo fact.
- responds-to-issues pass — 0 comments; opened 11 days ago by a collaborator.
- unclaimed pass — no assignees, no linke
- newcomer-scope pass (preferred) — no beginner label (enhancement, devops, tests, tier-3), but names
  .github/workflows/ci.yml and scripts/vases on the files clause only; 5–7 hours,and requires standing up a fresh database in CI.

All three clear every required check, so all three are accept. Nothing was blocked by the Path Review house
rule — there were no classmate claim commf them.

One caveat on the ranking: scope.md's fitceholder text (Write a few sentenceshere.), so I could not rank by your actual background. The order above is driven by the preferred check
instead: #53 is the only candidate that sewcomer-scope (maintainer-applied goodfirst issue + tier-1, with a repro you can paste into a REPL and failing tests that tell you when you're
done), while #49 and #51 are both tier-3 k that pass only on the named-filesclause. Fill in the fit profile and I'll re-rank.

[
{
"item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53",
"checks": [
{"name": "commits-alive", "grade": "pass", "evidence": "Most recent default-branch commit 2026-09-16T21:42:18Z by human author Aburke225, 5 days before today (2026-09-21)."},
{"name": "responds-to-issues", "grade": "pass", "evidence": "0 comments on the thread and issue opened 2026-09-10 by Aburke225 (author_association: COLLABORATOR)."},
{"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], no cross-referenced PRs in the issue timeline, and 0 open PRs in the repo."},
{"name": "newcomer-scope", "grade": "pass", "evidence": "Labels include 'good first issue'; body gives a runnable repro and names 4 failing tests in tests/unit/test_pii_scrubber.py."}
],
"verdict": "accept"
},
{
"item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/49",
"checks": [
{"name": "commits-alive", "grade": "pass", "evidence": "Most recent default-branch commit 2026-09-16T21:42:18Z by human author Aburke225, 5 days before today (2026-09-21)."},
{"name": "responds-to-issues", "grade": "pass", "evidence": "0 comments on the thread and issue opened 2026-09-10 by Aburke225 (author_association: COLLABORATOR)."},
{"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], no cross-referenced PRs in the issue timeline, and 0 open PRs in the repo."},
{"name": "newcomer-scope", "grade": "pass", "evidence": "No beginner label (devops/docs/tests/tier-3), but body names specific files to edit: new docs/ONBOARDING.md and docs/CONTRIBUTING.md."}
], "verdict": "accept" },
{
"item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/51", "checks": [ {"name": "commits-alive", "grade": recent default-branch commit 2026-09-16T21:42:18Z by human author Abuy (2026-09-21)."}, {"name": "responds-to-issues", "gr"0 comments on the thread and issue opened 2026-09-10 by Aburke225 (author_associat
{"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], no cross-referenced PRs in the issue timeline, and 0 open PRs in the repo."},
{"name": "newcomer-scope", "grade"beginner label
(enhancement/devops/tests/tier-3), but bo edit: .github/workflows/ci.yml and
scripts/validate_migrations.sh."}
],
"verdict": "accept"
}
]

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

0/20 (crashed due to setup), 12/20 (failed on responds-to-issues), 18/20.

**Issue analysis**

Issue `issue-01`.
Rubric's decision: Accept.
Gold label: Accept.
Reasoning: In my earlier iteration, the rubric rejected this issue because it strictly required a maintainer comment within 30 days. After revising the `responds-to-issues` check to allow issues with zero comments to pass, the rubric correctly evaluated the lack of comments as an acceptable baseline for a new, unclaimed issue rather than a sign of an unresponsive maintainer. This aligned the rubric's logic with the gold label.

**Check rationale**

`responds-to-issues` | Issue comment thread | Maintainers have not ignored questions or PRs from contributors. If there are no comments from the community, or if the issue was recently opened by a maintainer, this check passes.

Reasoning: I originally required a maintainer response within the last 30 days, which failed every candidate. I updated it to this wording because open-source maintainers frequently open "good first issues" and wait for contributors to find them. Penalizing an issue just because it has been sitting quietly without comments causes the rubric to falsely reject perfectly viable, unclaimed beginner tickets.

**Trade-offs**

By allowing issues with zero comments to pass automatically, this updated check gives up the guarantee that the maintainer is currently actively monitoring that specific ticket. It creates a slight blind spot where the rubric might accept an issue from a repository where a maintainer opened a batch of tickets months ago but has since abandoned the project and stopped reviewing PRs entirely.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. Fit to interests and time: This issue is a great fit for my available time because it seems like a straightforward, clearly scoped problem. Since I already have experience working with Python, tackling this bug is a perfect warm-up. It allows me to get comfortable with the project's workflow and contribution process without overwhelming myself right out of the gate.

2. Verdict vs. Rubric: The skill's verdict correctly identified the highly actionable scope of Issue 53, specifically noting the runnable reproduction steps and the named failing tests. However, as the tool explicitly noted in its output, my scope.md fit profile still contained placeholder text. Because of this, the automated rubric could not evaluate my actual background, and I had to personally weigh my existing knowledge of Python to determine that the test_pii_scrubber.py fix was the right match for my skill level.

3. Claiming difficulty: Because this is a well-documented "good first issue" with clear tests, it is a highly attractive target. I anticipate the difficulty in claiming it will be moderately high, as I will likely be competing with other contributors who also want an accessible first issue, meaning I will need to act quickly to lock it down.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
