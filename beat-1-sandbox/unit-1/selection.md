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

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/51

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

Ranked read-out

Repo codepath/pathreview-ai301-fa26-s3 — all three candidates are inside the scoped source. Path Review house rule applied: classmates' claim comments were disregarded for unclaimed.

Accepted, in fit order

1. #51 — Add a database migration validation step to CI — best fit: it is squarely DevOps + relational databases (spin up a fresh DB, run migrations in order, diff against the SQLAlchemy models), edited in .github/workflows/ci.yml and scripts/validate_migrations.sh — the CI/database/backend intersection in your profile.

- commits-alive pass — newest main commit 2026-09-16 by Aburke225 (human), 12 days ago
- responds-to-issues pass — zero comments
- unclaimed pass — no assignees, no linked PRs, no claim comments
- newcomer-scope pass (preferred) — no beginner label, but the body names the two specific files to edit

2. #49 — Contributor onboarding guide (issue → PR lifecycle) — clean and unclaimed, but it is documentation work; only the make test-integration addition touches tooling, so it exercises less of your Python/infra depth than #51.

- commits-alive pass — same commit evidence
- responds-to-issues pass — zero comments
- unclaimed pass — no assignees, no linked PRs, no claim comments
- newcomer-scope pass (preferred) — body names docs/ONBOARDING.md (new) and docs/CONTRIBUTING.md

Rejected

#53 — PII scrubber parenthesized phone numbers — sunk by unclaimed: PR #77 (pakmultilinks-dot, open, body "Fixes #53") is a formally linked open PR, and the rubric requires 0 open linked PRs.

- commits-alive pass; responds-to-issues pass — latest non-maintainer comment 2026-09-27, 1 day old; newcomer-scope pass — carries the good first issue label.

One tension worth noting: the house rule neutralizes classmates' claim comments and justifies it with "course credit attaches to the pull request you open, not to whether it merges" — reasoning that would apply just as well to a classmate's open PR. But the rule names only claim comments and says everything else applies as written, so the linked-PR conjunct stands and #53 rejects. If you want classmates' PRs treated like their claim comments, that belongs in the rubric or the house rule, not in this run.

[
{
"item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/51",
"checks": [
{"name": "commits-alive", "grade": "pass", "evidence": "Newest main commit 2026-09-16 by human Aburke225, 12 days before today (2026-09-28)"},
{"name": "responds-to-issues", "grade": "pass", "evidence": "Issue has 0 comments, so it passes automatically"},
{"name": "unclaimed", "grade": "pass", "evidence": "No assignees, no cross-referenced or linked PRs in the timeline, and no claim comments"},
{"name": "newcomer-scope", "grade": "pass", "evidence": "No beginner label, but body names specific files: .github/workflows/ci.yml and scripts/validate_migrations.sh"}
],
"verdict": "accept"
},
{
"item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/49",
"checks": [
{"name": "commits-alive", "grade": "pass", "evidence": "Newest main commit 2026-09-16 by human Aburke225, 12 days before today (2026-09-28)"},
{"name": "responds-to-issues", "grade": "pass", "evidence": "Issue has 0 comments, so it passes automatically"},
{"name": "unclaimed", "grade": "pass", "evidence": "No assignees, no cross-referenced or linked PRs in the timeline, and no claim comments"},
{"name": "newcomer-scope", "grade": "pass", "evidence": "No beginner label, but body names specific files: docs/ONBOARDING.md (new) and docs/CONTRIBUTING.md"}
],
"verdict": "accept"
},
{
"item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53",
"checks": [
{"name": "commits-alive", "grade": "pass", "evidence": "Newest main commit 2026-09-16 by human Aburke225, 12 days before today (2026-09-28)"},
{"name": "responds-to-issues", "grade": "pass", "evidence": "Latest non-maintainer comment (tbnguye9) is 2026-09-27, 1 day old, under the 14-day bound"},
{"name": "unclaimed", "grade": "fail", "evidence": "PR #77 by pakmultilinks-dot is open and its body says 'Fixes #53', so the issue has 1 open linked PR"},
{"name": "newcomer-scope", "grade": "pass", "evidence": "Carries the 'good first issue' label and the body includes runnable reproduction steps"}
],
"verdict": "reject"
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

`responds-to-issues` | Issue comment thread | If the thread contains comments from non-maintainers, the most recent non-maintainer comment must either be less than 14 days old or have a subsequent reply from a project maintainer. Issues with zero comments pass automatically.

Reasoning: I originally required a maintainer response within the last 30 days, which failed every candidate. I updated it to this mechanical 14-day wording because open-source maintainers frequently open "good first issues" and wait for contributors to find them. Penalizing an issue because it has been sitting without comments causes the rubric to falsely reject viable, unclaimed beginner tickets.

**Trade-offs**

By allowing issues with zero comments to pass automatically, this updated check gives up the guarantee that the maintainer is currently actively monitoring that specific ticket. It creates a slight blind spot where the rubric might accept an issue from a repository where a maintainer opened a batch of tickets months ago but has since abandoned the project and stopped reviewing PRs entirely.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. Fit to interests and time: Issue #51 aligns with my goal of deepening my backend and infrastructure skills. Adding a database migration validation step to CI directly matches my stated interest in DevOps and database management. It provides practical exposure to cloud automation through `ci.yml` and relational database operations by spinning up a fresh database to validate SQLAlchemy migrations, which is a better use of my time than taking on documentation tasks.

2. Verdict vs. Rubric: The automated rubric correctly failed my original target (#53) after catching an open linked PR from another contributor, while passing #51 and #49 based on repo health and scope. However, the pass/fail rubric could not evaluate my specific technical preferences. The tool's contextual ranking stepped in to weigh the distinct files involved (`ci.yml` vs. `ONBOARDING.md`), elevating #51 over #49 because its CI requirements actually map to my fit profile.

3. Claiming difficulty: I anticipate the difficulty in claiming this issue will be low. Unlike #53, #51 does not carry a "good first issue" label and requires setting up a database in CI, which naturally filters out contributors looking for a quick fix. The actual challenge will be executing the estimated 5–7 hours of infrastructure work, rather than competing with others to get the issue assigned.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
