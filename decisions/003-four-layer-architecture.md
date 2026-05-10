# Decision: Four-layer architecture

**Date:** 2026-05-09  
**Status:** Decided

## Context
Needed an architecture that supports multiple projection technologies and multiple shelf designs without coupling them together.

## Options Considered
- **Monolithic renderer** — simpler initially, but locks in one display approach
- **Four-layer separation** — more upfront structure, but each layer is independently swappable

## Decision
Four distinct layers, loosely coupled:
1. **Content layer** — books, covers, metadata (Rails, SQLite). Projection-agnostic.
2. **Shelf design** — pluggable config (JSON or Ruby module) defining layout, spacing, effects. Swappable without touching renderer.
3. **Renderer** — JS canvas that takes a design config and book list and draws covers. Knows nothing about projection.
4. **Projection adapter** — thin config: CSS transforms, inversion, orientation, fullscreen. Swapping projection = swapping adapter.

## Consequences
- New projection technologies require only a new adapter, not a renderer rewrite
- New shelf designs require only a new config, not code changes
- Slightly more upfront structure to set up correctly
- The renderer must be kept projection-agnostic — enforce this as a hard rule
