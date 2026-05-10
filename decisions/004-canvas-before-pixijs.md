# Decision: Start with plain Canvas API, consider PixiJS later

**Date:** 2026-05-09  
**Status:** Decided

## Context
The renderer needs to draw book covers to screen. PixiJS v8 (WebGL) was in the original plan. Evaluating whether it's necessary from the start.

## Options Considered
- **PixiJS v8** — GPU-accelerated, built-in filters (glow, blur), scene graph, smooth animations; but adds dependency and complexity
- **Plain Canvas API** — zero dependencies, simpler mental model, sufficient for a mostly-static grid of images

## Decision
Start with plain Canvas API. The renderer layer is isolated, so switching to PixiJS later is low cost. Introduce PixiJS only if we hit performance limits on Pi 5 or need richer visual effects (glow, animated transitions).

## Consequences
- No PixiJS dependency to start
- Renderer code stays simple and easy to reason about
- If visual effects become a priority, migration path is clear and contained to the renderer layer
