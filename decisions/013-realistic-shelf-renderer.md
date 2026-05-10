# Decision: Renderer displays a realistic shelf, not floating covers on black

**Date:** 2026-05-10  
**Status:** Decided  
**Type:** Architecture  
**Weight:** Pivot  
**Supersedes:** Implicit assumption from early Pepper's Ghost framing

## Context
Early project framing assumed a pure black background with glowing/floating book covers — an aesthetic tied specifically to Pepper's Ghost projection. This assumption was baked into the visual thinking even after decision 005 established the architecture as projection-agnostic. The goal is to mirror the physical shelf digitally, not to create a sci-fi floating-books effect.

## Options Considered
- **Black background, floating covers** — optimized for Pepper's Ghost; looks holographic but doesn't resemble the physical shelf; locks visual design to one projection technology
- **Realistic shelf rendering** — warm wood tones, shelf geometry, covers sitting in compartments; mirrors the physical shelf; projection-agnostic; the Pepper's Ghost adapter handles black background if/when needed

## Decision
The renderer draws a realistic-looking shelf — shelf geometry, wood tones, covers positioned within compartments — that visually mirrors the physical shelf. The renderer knows nothing about projection technology.

Black backgrounds, mirroring, and inversion are handled by the projection adapter layer (decision 005), not the renderer. For the home alpha (plain monitor), the renderer output IS the display output.

## Consequences
- Renderer design focuses on realism and shelf aesthetics, not holographic effects
- Pure black background is NOT a renderer constraint — it is a Pepper's Ghost adapter constraint
- The digital shelf looks like the physical shelf, making the "mirror" concept literal
- Any projection technology can display the renderer output; adapters handle display-specific transforms
- Glow, bloom, and sci-fi effects are valid future enhancements but are not the default aesthetic
