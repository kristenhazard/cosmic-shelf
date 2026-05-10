# Decision: Keep POCs small and reviewable

**Date:** 2026-05-09  
**Status:** Decided  
**Type:** Practice

## Context
AI can generate large amounts of code quickly. Large chunks are hard to reason about, hard to review, and hard to roll back if the approach turns out to be wrong.

## Decision
POCs should do one thing. If a POC starts touching multiple layers or solving multiple problems, split it. A good POC fits in a single conversation turn worth of review.

## Consequences
- Bad decisions are caught at the smallest possible scope and cost
- Each decision record maps cleanly to a discrete, reviewable unit of work
- Slightly more overhead in scoping work upfront — worth it for maintainability
