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

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/1

**Verdict output**

All three accepted, in fit order

1. #1 — Duplicate embeddings on re-ingest — best fit: a SQLAlchemy ORM bug (db_session.query("IngestedSource") passed a string instead of the model class), labeled devops, in Python backend ingestion code. It sits directly on the relational-database and backend/infra axis of your profile, and the root cause is already pinpointed in the body.
- commits-alive pass — newest main commit 2026-09-16 by Aburke225 (human), 13 days ago
- responds-to-issues pass — latest non-maintainer comment (SKC999) is 2026-09-28, 1 day old
- unclaimed pass — no assignees, no linked PRs; SKC999's claim is a classmate's, which the house rule tells you to ignore
- newcomer-scope pass (preferred) — no beginner label, but the body names ingestion/pipeline.py and core/models/ingested_source.py and identifies the exact faulty call

2. #16 — Failed tool calls never reach the review output — Python backend work in agent/orchestrator.py and the service layer, so it exercises your Python depth, but there's no database or infrastructure angle.
- commits-alive pass — same commit evidence
- responds-to-issues pass — zero comments
- unclaimed pass — no assignees, no linked PRs, no claim comments
- newcomer-scope pass (preferred) — body names both files to edit

3. #39 — Review progress indicator — weakest fit: three of its five files are React/TypeScript frontend (ReviewPage.tsx, useReviewStatus.ts, types/index.ts). There is a real backend slice (adding the missing progress_pct column to the Review model and populating it), but the bulk of the work is UI, away from your stated interests.
- commits-alive pass — same commit evidence
- responds-to-issues pass — zero comments
- unclaimed pass — no assignees, no linked PRs, no claim comments
- newcomer-scope pass (preferred) — body names all five files to edit

One thing to go in with eyes open on #1: SKC999's repro report says they reproduced only part of it — the skip check fails and embeddings are recomputed, but they saw no duplicate vectors in ChromaDB for identical content. That doesn't affect any check, but it means the issue title may overstate the observable symptom, so budget time to pin down what "duplicate" actually means before you write the fix.

[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/1",
    "checks": [
      {"name": "commits-alive", "grade": "pass", "evidence": "Newest main commit 2026-09-16 by human Aburke225, 13 days before today (2026-09-29)"},
      {"name": "responds-to-issues", "grade": "pass", "evidence": "Latest non-maintainer comment (SKC999) is 2026-09-28, 1 day old, under the 14-day bound"},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignees and no cross-referenced or linked PRs; SKC999's claim comment is a classmate's, which the Path Review house rule says does not block"},
      {"name": "newcomer-scope", "grade": "pass", "evidence": "No beginner label, but body names specific files ingestion/pipeline.py and core/models/ingested_source.py plus the exact faulty query call"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/16",
    "checks": [
      {"name": "commits-alive", "grade": "pass", "evidence": "Newest main commit 2026-09-16 by human Aburke225, 13 days before today (2026-09-29)"},
      {"name": "responds-to-issues", "grade": "pass", "evidence": "Issue has 0 comments, so it passes automatically"},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignees, no cross-referenced or linked PRs in the timeline, and no claim comments"},
      {"name": "newcomer-scope", "grade": "pass", "evidence": "No beginner label, but body names specific files agent/orchestrator.py and core/services/review_service.py"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/39",
    "checks": [
      {"name": "commits-alive", "grade": "pass", "evidence": "Newest main commit 2026-09-16 by human Aburke225, 13 days before today (2026-09-29)"},
      {"name": "responds-to-issues", "grade": "pass", "evidence": "Issue has 0 comments, so it passes automatically"},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignees, no cross-referenced or linked PRs in the timeline, and no claim comments"},
      {"name": "newcomer-scope", "grade": "pass", "evidence": "No beginner label, but body names five specific files spanning frontend/src and core/"}
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

1. Fit to interests and time: Issue #1 exactly matches my focus on backend infrastructure and databases. Resolving a SQLAlchemy ORM bug in the Python ingestion pipeline allows me to work directly with database models and backend logic. This aligns with my technical goals much better than issue #39, which involves significant React and TypeScript UI work that I want to avoid.

2. Verdict vs. Rubric: The automated rubric successfully passed all three issues by validating repo health and correctly applying the course house rule that ignores classmate claim comments. However, the raw pass/fail rubric could not account for my technical preferences. The tool's contextual ranking was necessary to evaluate the files involved and elevate #1 to the top spot, recognizing that its database-centric scope fit my profile while downgrading #16 and #39 for lacking infrastructure relevance.

3. Claiming difficulty: I expect high difficulty in securing this issue. A classmate has already staked a claim in the comments. While the course house rules allow me to ignore their comment, I will need to move fast and submit a pull request before they do. I also need to allocate extra time to investigate the symptom discrepancy they noted regarding duplicate vectors in ChromaDB to ensure I actually solve the root cause.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
