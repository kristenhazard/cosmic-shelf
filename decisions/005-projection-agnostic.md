# Decision: Projection technology is not locked in

**Date:** 2026-05-09  
**Status:** Decided

## Context
Original plan assumed Pepper's Ghost (hidden monitor + 45° acrylic panel). User wants to experiment with different projection technologies as the project evolves.

## Options Considered
- **Pepper's Ghost optimized** — simpler initially, pure black background, mirrored output; but constrains future experimentation
- **Projection-agnostic with adapter pattern** — slightly more structure upfront, but any display technology is a valid target

## Decision
Architecture must not assume any specific projection technology. A projection adapter layer handles technology-specific concerns (mirroring, rotation, black background requirement, window vs. kiosk mode). Pepper's Ghost is the first adapter, not the only one.

## Consequences
- Pure black backgrounds remain the safe default (required by Pepper's Ghost) but are not hardcoded as universal
- Renderer output is always projection-neutral
- Experimenting with holographic pyramids, plain monitors, etc. requires only a new adapter config
