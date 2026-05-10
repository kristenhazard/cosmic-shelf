# Decision: Regularly check that logic lives in the right layer

**Date:** 2026-05-09  
**Status:** Decided  
**Type:** Practice

## Context
The four-layer architecture (content → design → renderer → projection adapter) only works if each layer stays honest about its responsibilities. Drift happens gradually and is hard to reverse once embedded.

## Decision
At each POC review, explicitly ask: is this logic in the right layer? Key smells to watch for:
- Renderer knowing about projection technology
- Content layer caring about layout or display
- Design config containing rendering logic instead of declarative data
- Projection adapter containing business logic

If a smell is found, fix it before closing the POC — not later.

## Consequences
- Layer boundaries stay clean as the codebase grows
- New projection adapters and shelf designs remain easy to add
- Slightly more discipline required during review
