# AI Content Production Platform

**Multi-brand content production tool | New Seas | February–August 2026**

Designed and delivered an internal platform that automated repetitive content-production work while preserving each brand's voice and human editorial approval.

[View the visual case study and project documents](https://www.by-evamarquez.com/portfolio/ai-content-pipeline)

## Business results

| Result | Evidence |
| --- | --- |
| **50% smaller content team** | Reported by New Seas CEO Kai Cromwell |
| **Nearly $100K in annualized profit improvement** | CEO estimate; not audited realized profit |
| **From ~2 days to minutes per article** | Median keyword-to-document processing time: **4.7 minutes**, plus editorial review |
| **549 articles across 882 jobs** | Recorded production results |
| **Failure rate reduced from 25–40% to 4–8%** | Measured processing reliability after improvements |

## The problem

The team produced content for many e-commerce brands. Research, writing, formatting, review, and handoffs took roughly two days per article. Delays and failed jobs were difficult to diagnose.

**Goal:** produce review-ready drafts faster, at lower operational cost, without losing brand consistency or editorial control.

## What I delivered

- **Multi-brand support:** configurable brand profiles with separate voice, tone, audience, and content guidelines.
- **12-stage production system:** intake, research, planning, writing, revision, source checks, quality control, and editor handoff.
- **Reliable processing:** asynchronous jobs, progress tracking, retries, and visible errors.
- **Editorial workflow:** review-ready Google Docs rather than automatic publishing.
- **Operational handoff:** documentation and tutorials so the team could run the system.

The platform used **Netlify and Supabase**, alongside AI services and application integrations. The underlying implementation remains private.

## Capacity and scope

- **Tested:** 25 brand profiles and a batch of **250 topics** (25 brands × 10 topics).
- **Designed for:** at least 50 brand profiles; that capacity was **not load-tested**.
- **Technical scale documented:** 43 server functions and 12 application screens.
- A tested batch of 250 topics does not establish 250 simultaneous executions or unlimited capacity.

## My role

As **AI Engineer**, I owned process analysis, system design, implementation of the automation layer, reliability improvements, documentation, and operational handoff. Editors retained final content approval and publication decisions.

## Key decisions and challenges

- **Unreliable long-running jobs:** Split production into 12 trackable stages with retries and recovery paths. Measured failure rates fell from 25–40% to 4–8%.
- **Different voices across brands:** Stored brand profiles separately and passed relevant instructions to each stage. The system was tested with 25 brand profiles and a batch of 250 topics.
- **Speed without losing oversight:** Added quality checks and Google Docs handoff instead of automatic publishing. Median processing took 4.7 minutes per document, plus editor review.

## Project documentation

For more detail, see:

- [Project brief](docs/project-brief.md) — problem, scope, and success criteria
- [Pipeline design](docs/pipeline-design.md) — stages, architecture, and delivery decisions
- [Quality controls](docs/quality-controls.md) — checks, retries, and human review
- [Production evidence](docs/production-evidence.md) — output and reliability measurements
- [Close-out report](docs/close-out-report.md) — outcomes and lessons learned
- [Architecture overview](architecture.md) — high-level system design
- [Privacy boundaries](privacy.md) — what is deliberately not published

**Privacy:** This is a sanitized case study. No production source code, client data, proprietary prompts, credentials, or private infrastructure details are included.
