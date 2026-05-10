# Decision: Display book covers, not spines

**Date:** 2026-05-09  
**Status:** Decided  
**Weight:** Core

## Context
The original vision was glowing book spines. We questioned whether cover art was easier to procure and more visually interesting.

## Options Considered
- **Spines** — authentic bookshelf feel, but spine art is rarely available digitally; would require generating text-based spines
- **Covers** — widely available via Open Library (by ISBN) and Google Books API fallback; richer visual display

## Decision
Display full book covers. Cover images are readily available, cache locally after first fetch, and are more visually compelling at shelf scale.

## Consequences
- Cover art fetching becomes a first-class concern in the content layer
- Shelf compartment sizing needs to accommodate cover aspect ratios (roughly 2:3 portrait)
- No need to generate synthetic spine graphics
