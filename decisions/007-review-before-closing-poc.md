# Decision: Run /review before closing a POC

**Date:** 2026-05-09  
**Status:** Decided  
**Type:** Practice

## Context
AI-generated code can be subtly wrong in ways that look correct on the surface. POCs that aren't reviewed before being declared "done" can embed flawed patterns that propagate through the codebase.

## Decision
Before declaring a POC complete and moving on, run `/review`. Treat the review output as a gate — address any structural issues before the decision record is written and the approach is locked in.

## Consequences
- POCs take slightly longer to close
- Bad patterns are caught at the smallest possible scope
- Decision records reflect reviewed, not just working, code
