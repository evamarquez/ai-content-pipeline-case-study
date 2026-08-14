# Architecture: AI content pipeline

This document expands the public case study without reproducing the private implementation.

## System boundary

```text
                     +------------------+
                     |  Brand profiles  |
                     +--------+---------+
                              |
                              v
keyword --------------> +-----+----------+
                        |  Job creation  |
                        |  and state     |
                        +-----+----------+
                              |
                              v
                     +--------+--------+
                     |  Async pipeline |
                     |  12 stages      |
                     +---+----+----+---+
                         |    |    |
                         v    v    v
                    research  writing  QA
                         |    |    |
                         +----+----+
                              |
                              v
                     +--------+--------+
                     |  Google Doc     |
                     |  editor handoff |
                     +-----------------+
```

## Pipeline responsibilities

The public model groups the twelve stages by responsibility rather than exposing internal function names:

1. Create and validate the job.
2. Load the brand context.
3. Collect research inputs.
4. Build the content brief.
5. Build the outline.
6. Write sections.
7. Revise for structure and completeness.
8. Humanize the draft.
9. Verify sources and links.
10. Run quality checks.
11. Prepare the document and metadata.
12. Deliver the draft for human review.

The implementation used 43 server functions across these responsibilities. Each function had one job and wrote enough state for the next stage to continue or retry.

## State and recovery

```text
pending -> running -> waiting_for_review -> completed
                     |
                     +-> failed -> retryable / needs diagnosis
```

Long-running work did not rely on one request remaining open. Stages wrote progress, specialised revision passes chained from the stored state, and the progress view surfaced the last known failure.

## Reliability controls

- Per-stage status and timestamps.
- Retry boundaries around transient external calls.
- Queueing for bulk work.
- A fallback path for JavaScript-heavy source sites.
- Rate limiting on audit operations.
- A failure panel that included stage, job ID, and last message.
- Source verification separate from link-existence checks.

## Why this shape

The main design constraint was recoverability. A single large AI request was simpler to start but difficult to retry, difficult to time, and difficult to explain to an editor. Smaller stages created more functions, but they also created observable boundaries and made the system operable by people who did not build it.

