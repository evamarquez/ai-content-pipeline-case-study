# Pipeline design

## Delivery model

The system uses twelve visible stages rather than one long generation request:

1. Brief intake
2. Brand context
3. Research and sources
4. Outline
5. Draft
6. Revision
7. Humanization
8. Quality gates
9. Recovery
10. Document creation
11. Editor handoff
12. Completion record

## Key decisions

### Self-chaining stages

Long work was split into specialised passes that chained through job state. Each stage received a fresh execution window and a clear output contract.

### Configuration instead of prompt sprawl

Brand information lived in structured profiles. Each stage received only the context it needed, making changes safer and easier to reason about.

### Handoff as a product feature

The output was not considered complete when text was generated. It was complete when an editor could open the document, understand what had been checked, and make the final decision.

## Ownership

The delivery owner designed the workflow, implemented the automation layer, fixed reliability issues, documented the system, and prepared the handoff. Editorial approval remained with the content team.