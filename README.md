# AI content pipeline

An internal content production system that turned a keyword and a brand profile into a reviewable Google Doc.

This is a sanitized case study. The source code and client-specific material are not published.

## At a glance

| | |
|---|---|
| Role | AI Engineer and delivery owner for the automation layer |
| Scope | 12 pipeline steps, 43 server functions, 12 screens |
| Output | 549 articles across 882 jobs |
| Median throughput | 4.7 minutes from keyword to Google Doc |
| Reliability | Failure rate reduced from 25-40% to 4-8% |
| Human control | Editorial review stayed in the loop |

## The problem

The content team was producing long-form articles for multiple e-commerce brands. A single article could take roughly two days once research, outlining, drafting, formatting, internal review, and delivery were included.

The bottleneck was not simply writing. It was the number of handoffs around writing, and the fact that a failure in one asynchronous step could leave a job incomplete without giving the next person enough information to recover it.

## Goal and implementation plan

The goal was to remove repetitive production work while keeping editorial judgment with the team.

The implementation plan was to:

1. Break the workflow into visible, resumable stages instead of one long request.
2. Keep brand context in structured configuration rather than duplicating it across prompts.
3. Add quality gates before the document reached an editor.
4. Make failures diagnosable and the handoff usable by non-developers.

## What I built

- A database-backed job flow from keyword and brand profile to reviewable document.
- Twelve visible pipeline stages with separate server functions behind them.
- Brand context injection so each stage received the instructions relevant to that stage.
- Research, brief, writing, revision, humanization, source verification, and delivery steps.
- A Google Docs handoff so editors could work in the tool they already used.
- A diagnosable progress view showing the step, job identifier, last message, and recovery context.

## Architecture and production flow

```text
keyword + brand profile
          |
          v
   create content job
          |
          v
  brief and research stages
          |
          v
  outline -> section writing
          |
          v
 revision -> humanization
          |
          v
 source checks -> QA
          |
          v
      Google Doc
          |
          v
     human editor
```

The expanded model is in [architecture.md](architecture.md).

### Production and reliability

#### Self-chaining instead of one long request

The first version tried to keep too much work inside a single request. Timeouts were the predictable result. Revision was split into specialised passes that chained through the job state, giving each pass a fresh execution window.

This made the system easier to retry and easier to diagnose. It also made the pipeline visible as a sequence of decisions instead of one opaque generation call.

#### Configuration instead of prompt sprawl

Brand information lived in structured profiles. The pipeline injected only the context needed by each stage. Adding or updating a brand therefore changed data and configuration, not the logic of every prompt.

#### Quality gates before delivery

The system did not publish directly. It generated a draft, ran checks, and handed the result to an editor. Source verification and QA were separate stages because a link existing is not the same as an assertion being correct.

#### Failure data is part of the product

The progress screen was changed from a generic error state to a useful one: stage name, job identifier, last message, and a way to copy the details. A failed job that can be understood is cheaper to recover than a successful-looking job that silently produced incomplete work.

## Results

- 549 articles were produced across 882 jobs.
- Median time from keyword to Google Doc was 4.7 minutes.
- The measured failure rate moved from 25-40% in the early period to 4-8% after the asynchronous architecture, queueing, retries, and stage-specific fixes were in place.
- Production went from roughly two days per article to minutes plus editor review.

## Failure analysis and lessons learned

An early content-quality score returned 97/100. That number looked reassuring, but the evaluator was rewarding criteria that did not prove the article was good. After the evaluation logic was corrected, representative scores were 85, 75, and 44 depending on the case.

The lesson was not that the system suddenly became worse. The measurement became more honest. A score is only useful when the thing being scored matches the quality claim being made.

The same audit found four uncomfortable implementation facts:

- Nine production functions had no source in the repository.
- Four completed features had no caller.
- The in-app Help described eight steps although the pipeline had twelve.
- The progress bar counted to the wrong total.

These were documented for handoff instead of being hidden behind a polished demo.

## Security and operational hardening

- Removed a leaked service-account key and hardened the ignore rules.
- Added an SSRF guard.
- Fixed authentication on the brand-scrape stage.
- Added rate limiting to the audit endpoint.
- Added source verification and a safer failure path for incomplete generation.

## Handoff and post-launch operations

- The team received 11 operating tutorials and 18 screenshots.
- SOPs made it possible for non-developers to operate the system after handoff.
- Editors retained the final review and publication decision in Google Docs.
- The known implementation gaps and recovery context were documented for the next operator.

## What I owned

I owned the process analysis, the workflow design, the implementation of the automation layer, the reliability fixes, the documentation, and the handoff. Editorial decisions and final publication stayed with the content team.

## Privacy boundary

See [privacy.md](privacy.md) for the exact boundary around what is intentionally omitted.
