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

amlodzianowska

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/1#issuecomment-6255411539

```markdown
I'd like to take this issue.

Ingesting the same source twice doesn't skip the second run. `_check_skip()` in
`ingestion/pipeline.py` passes the string `"IngestedSource"` to `db_session.query()`
instead of the model class. The query fails, the broad `except Exception` catches it,
and the source gets chunked and embedded again.

Plan:
- Set up the project with Docker following `README` instructions
- Run the same markdown document through the pipeline twice
- Check whether the second run is skipped and whether the vector store ends up with duplicate chunks

I'll post a report here with my environment, the commands I ran, and the output.
If I can't reproduce it, I'll say so and explain what I tried.
```

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/1#issuecomment-6256099799

`````markdown
## Repro report for #1

I reproduced part of this issue. The already-ingested check fails on every run, so ingesting the same content twice runs the full pipeline again and sends every chunk to the embedding provider a second time. I did not see duplicate vectors in ChromaDB in my run. I also tried it end-to-end through the UI, but the review flow does not call `IngestionPipeline`, so the check in this issue never runs there. Details below.

### Environment

- Commit: `2f4e82f`
- macOS 26.7.1, Apple Silicon (arm64)
- Python 3.14.6 in the project's `.venv` (created by `make setup`)
- chromadb 1.5.9, SQLAlchemy 2.1.5, structlog 26.1.0
- For the UI attempt: Postgres, Redis, and ChromaDB running in Docker (`docker compose up -d`), set up with `make setup` per `docs/SETUP.md`

### End-to-end attempt (UI)

Steps:

1. Start the services and the app: `docker compose up -d`, `make setup`, `make run`.
2. Open http://localhost:5173 and log in as `user1@example.com`.
3. Click **Start New Review**, enter `u1` as the GitHub username, and upload `samlpe-readme.md`.
4. Repeat step 3 with the same file.
5. Check the database:

````
$ docker compose exec db psql -U pathreview -d pathreview_dev -c 'select count(*) from ingested_sources;' -c 'select count(*) from reviews;'
 count 
-------
     0
(1 row)

 count 
-------
     3
(1 row)
````

Reviews are saved, but `ingested_sources` is empty.

The review flow never called `IngestionPipeline`: none of its `Starting ... ingestion` lines appear in the backend output. Instead, each submission logged `github_ingestion_failed` while building the `IngestedSource` record. The relevant lines from the backend output in my terminal:

````
2026-10-18 17:16:51 [error    ] github_ingestion_failed        error="'raw_data' is an invalid keyword argument for IngestedSource" request_id=077934fd-466b-4aae-ac91-aa0b4c89d8f5 username=u1
2026-10-18 17:17:03 [error    ] github_ingestion_failed        error="'raw_data' is an invalid keyword argument for IngestedSource" request_id=a0b795b3-6eec-4dc5-aa95-a92b8e4cbae2 username=u1
````

So in the UI flow, the check in this issue never runs, and the `IngestedSource` record is never created.

### Script reproduction

The script calls `IngestionPipeline.ingest_readme()` directly, twice, with the same profile ID, repo name, and content. It uses an in-memory ChromaDB collection, a SQLAlchemy session on in-memory SQLite, and a stand-in embedding provider that records every text it receives. It also runs the query `_check_skip()` builds outside its `try/except` to show the exact error, and captures the pipeline's log events for each attempt.

After `make setup`, save the script below in the repo root and run, providing a path to sample readme as an argument:

````bash
source .venv/bin/activate
python repro_issue_1.py <path_to_file>/<sample_readme.md>
````

<details>
<summary><code>repro_issue_1.py</code></summary>

````python
"""Reproduce issue #1: re-ingesting identical content is never skipped.

Usage (from the repo root, with .venv active):
    python repro_issue_1.py path/to/file.md

Ingests the given Markdown file twice through IngestionPipeline.ingest_readme()
and reports whether the second attempt was skipped.
"""

import platform
import sys
from importlib.metadata import version
from pathlib import Path

import chromadb
from sqlalchemy import create_engine
from sqlalchemy.orm import Session
from structlog.testing import capture_logs

from ingestion.pipeline import IngestionPipeline

CHECK_FAILED = "Could not check if source already ingested"
SKIPPED = "Source already ingested, skipping"
RECORDED = "Recording ingested source"


class SpyEmbedder:
    """Stand-in embedding provider that records every text it receives."""

    def __init__(self):
        self.texts = []

    def embed(self, texts):
        self.texts.extend(texts)
        return [[float(len(t)), 0.5, 0.25] for t in texts]


def probe_query(session):
    """Run the same query _check_skip() builds, outside its try/except."""
    try:
        session.query("IngestedSource").filter_by(source_id="probe").first()
    except Exception as exc:
        return f"raises {type(exc).__name__}: {exc}"
    return "does not raise"


def main():
    if len(sys.argv) != 2:
        sys.exit("usage: python repro_issue_1.py path/to/file.md")

    source = Path(sys.argv[1])
    content = source.read_text(encoding="utf-8")

    print(f"platform    {platform.platform()}")
    print(f"python      {sys.version.split()[0]}")
    for pkg in ("chromadb", "sqlalchemy", "structlog"):
        print(f"{pkg:<11} {version(pkg)}")
    print(f"input       {source} ({len(content)} chars)")
    print()

    session = Session(create_engine("sqlite://"))
    print(f'query("IngestedSource") {probe_query(session)}')
    print()

    store = chromadb.EphemeralClient().get_or_create_collection("issue_1_repro")
    embedder = SpyEmbedder()
    pipeline = IngestionPipeline(
        vector_db=store, db_session=session, embedding_provider=embedder
    )

    results = []
    for attempt in (1, 2):
        with capture_logs() as events:
            result = pipeline.ingest_readme("profile-repro", "repro-repo", content)
        results.append(result)
        names = [e.get("event") for e in events]

        print("*" * 40)
        print(f"attempt {attempt}")
        print(f"  skipped            {result.skipped}")
        print(f"  chunks             {result.chunk_count}")
        print(f"  texts embedded     {len(embedder.texts)} (running total)")
        print(f"  vectors in store   {store.count()}")
        print(f"  check failed       {CHECK_FAILED in names}")
        print(f"  skip logged        {SKIPPED in names}")
        print(f"  record logged      {RECORDED in names}")
        if not events:
            print("  (no log events captured)")
        print()

    stored = store.get()["documents"]
    print(f"documents in store {len(stored)}, unique {len(set(stored))}")
    print()

    if results[1].skipped:
        print("RESULT: second attempt was skipped. Bug NOT reproduced.")
    else:
        print("RESULT: second attempt was not skipped. Bug reproduced.")


if __name__ == "__main__":
    main()
````

</details>

### Output

````
platform    macOS-26.7.1-arm64-arm-64bit-Mach-O
python      3.14.6
chromadb    1.5.9
sqlalchemy  2.1.5
structlog   26.1.0
input       sample_readme.md (925 chars)

query("IngestedSource") raises ArgumentError: Textual column expression 'IngestedSource' should be explicitly declared with text('IngestedSource'), or use column('IngestedSource') for more specificity

****************************************
attempt 1
  skipped            False
  chunks             7
  texts embedded     7 (running total)
  vectors in store   7
  check failed       True
  skip logged        False
  record logged      True

****************************************
attempt 2
  skipped            False
  chunks             7
  texts embedded     14 (running total)
  vectors in store   7
  check failed       True
  skip logged        False
  record logged      True

documents in store 7, unique 7

RESULT: second attempt was not skipped. Bug reproduced.
````

### What happened

I expected the second attempt to return `skipped=True` without calling the embedding provider. Instead it returned `skipped=False`, produced the same 7 chunks, and sent all 7 to the embedding provider again (14 texts total).

`_check_skip()` passes the string `"IngestedSource"` to `db_session.query()` instead of the model class. SQLAlchemy rejects it with an `ArgumentError` while building the query, before anything reaches the database, so the database backend doesn't change this. The `except Exception` block turns the error into the "Could not check if source already ingested" warning and returns `None`, so the pipeline keeps going. The output shows that warning on both attempts and the skip message on neither.

`_record_ingested_source()` has the same gap from the other side. `record logged True` only shows the function ran. Its body in `ingestion/pipeline.py` logs and does nothing else, so no row is ever saved:

````python
        try:
            # This is a placeholder for actual database recording
            # In a real implementation, would create IngestedSource record
            logger.info(
                "Recording ingested source",
                source_id=source_id,
                source_type=source_type,
                profile_id=profile_id,
                chunk_count=chunk_count,
            )
        except Exception as e:
            logger.error(
                "Failed to record ingested source",
                source_id=source_id,
                error=str(e),
            )
````

Even with a working query, there would be nothing to find.

The vector store did not double in my run. It held 7 vectors after both attempts, all 7 documents unique. So the embeddings were regenerated, but I did not see duplicate vectors stored. I have not checked yet why the count stayed the same. The script's last line says "Bug reproduced" because it only tests whether the second attempt was skipped. The duplicate vectors part of the issue was not reproduced.

### Next steps

- Look into why nothing is saved to `ingested_sources`. In the pipeline, `_record_ingested_source()` only logs. In the review flow, building the `IngestedSource` record fails with `'raw_data' is an invalid keyword argument for IngestedSource`.

I'll post what I find here.

I used an AI assistant to help with the script and this report. I ran every command myself, and the output above is copied from my terminal.
`````

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run: `agreement: 17/20 scored items  (bar: 18/20: below the bar)`. Disagreed on pkg-03, pkg-09 and pkg-10, all gold `accept` that my rubric rejected.
2. Full run with `--save-run eval-run.txt`, after revising `behavior-matches-issue` and `claims-match-evidence`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`.

**Package analysis**

**pkg-10** (starship/starship#7648). Gold label: `accept`. My rubric in the first full run: `reject`, with the note `failed: behavior-matches-issue`. In the final run: `accept`.

The report is an honest cannot-reproduce in a different environment. It opens with "Result: cannot reproduce on Linux + zsh with the report's exact layout and config." and states "The report is macOS + fish 4.7.1; shell and OS both differ, starship version matches."

My check at the time required "the conditions the issue names were set up and the symptom was checked for." The grader read "the conditions" as all of them. The OS and shell from the issue were not set up, so the check failed, even though the artifact shows the trigger was run (the symlinked `cd ~/projects/app-dir` with `repo_root_style = "bold red"`) and the symptom was checked for: "`starship explain` in that directory lists the `directory` module with the contracted repo path; it is not omitted."

The gold label accepts it because it is the kind of report my own evidence guide calls good: "A report saying it could not reproduce the issue, with the commands it ran, is a good report." After my revision the check passes it, because it now reads: "An attempt that could not match every condition the issue names (a different OS, shell or input) still passes when the report names each difference".

**Check rationale**

`behavior-matches-issue`, as it now reads:

> If the report concludes it reproduced the issue, the artifact shows the issue's behavior, not an adjacent one: a different error, a setup failure, or a related symptom. If the report concludes it could not reproduce, the artifact instead shows a faithful attempt: the trigger the issue names was run and the symptom was checked for. An attempt that could not match every condition the issue names (a different OS, shell or input) still passes when the report names each difference; it fails if the trigger itself was never run. Either way, the report saying so does not count; only the artifact does.

The earlier version said the attempt must show "the conditions the issue names were set up and the symptom was checked for." In my first full run that rejected pkg-09 and pkg-10, both honest cannot-reproduce reports that named exactly what differed from the issue's setup. I changed the requirement to the trigger and the symptom check, and allowed differences as long as the report names each one. I kept "it fails if the trigger itself was never run" so that a cannot-reproduce which tested something else still fails.

I rejected the TOLERANCE option at the bottom of the rubric, "Accept if at most one required check fails." Its own note says it "accepts any package whose only defect is a single loud one, including one that fails conventions alone", which would have let the disclosure package through.

**Trade-offs**

Loosening `behavior-matches-issue` and `claims-match-evidence` risked flipping reject packages to accept, mainly `no-evidence` and `wrong-target`. The confirming full run shows they held: `categories: clear-accept 8/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`. Both were 4/4 in the first run as well.

The case I accept the check will now miss: a cannot-reproduce that names its differences, but where those differences make the attempt meaningless, such as testing an OS-specific bug on a different OS. My check passes that report because it is honest about the gap, and leaves it to the maintainer to judge whether the attempt counts.
