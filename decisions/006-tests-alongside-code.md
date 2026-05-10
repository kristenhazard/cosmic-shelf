# Decision: Write tests alongside code, not after

**Date:** 2026-05-09  
**Status:** Decided  
**Type:** Practice

## Context
AI-assisted development can generate code quickly, creating a temptation to defer testing. Untested code accumulates debt that compounds as the project grows.

## Decision
Write tests as each piece is built, not as cleanup afterward. Priority order:
1. Rails model tests — data integrity, validations, associations
2. Job tests — sync logic, error handling, idempotency
3. Renderer and adapter — lower priority initially, test logic not visuals

## Consequences
- Each POC is not "done" until tests exist for its core behavior
- Slower in the short term, significantly more maintainable as the codebase grows
- Bugs in sync logic (the most critical path) are caught before they corrupt local data
