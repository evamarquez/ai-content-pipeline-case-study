# Project brief

## Business problem

The content team was spending roughly two days moving one article from keyword and brand brief through research, drafting, formatting, review, and delivery. The main bottleneck was the number of handoffs and the lack of a clear recovery path when an asynchronous step failed.

## Goal

Create a reliable path from keyword and brand profile to a reviewable Google Doc while keeping editorial judgment with the content team.

## Scope

- Brief and brand-context intake
- Research, outline, writing, revision, and humanization
- Source verification and quality checks
- Job state, recovery, and progress visibility
- Google Docs handoff and editor review

## Constraints

- AI output is a draft, not an automatic publication decision.
- Provider latency and incomplete responses must be recoverable.
- Production data, credentials, client names, and internal URLs stay private.
- The public case must separate workflow evidence from downstream marketing claims.

## Definition of done

- The workflow is visible as resumable stages.
- A failed job includes enough context to diagnose and recover it.
- A quality-checked document reaches an editor.
- Production volume and failure patterns can be reported honestly.