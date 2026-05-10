# Decision: Shelf designer as first POC

**Date:** 2026-05-10  
**Status:** Decided  
**Type:** Architecture

## Context
Defining a shelf layout by describing it verbally or in ASCII is error-prone and frustrating. We need a way to define compartment geometry precisely that works for the builder (us) and for future users who want to define their own shelf designs.

## Options Considered
- **Manual coordinate entry** — user provides x, y, w, h per box as text; simple but still abstract
- **Visual shelf designer** — browser-based tool to draw and name compartments, export JSON config; solves the problem for us AND becomes a real feature for other users

## Decision
Build a visual shelf designer as the first POC. A standalone HTML/JS file (no Rails dependency) that:
- Renders a canvas with a 1" snap grid
- Click and drag to draw compartments
- Click a compartment to edit its name and exact x, y, w, h values
- Exports JSON design config

Start standalone so there's zero setup friction. Graduate into Rails later.

## Output format
```json
{
  "name": "my-shelf",
  "unit": "inches",
  "compartments": [
    { "id": "box1", "x": 5, "y": 0, "w": 12, "h": 10 },
    { "id": "box2", "x": 17, "y": 0, "w": 17, "h": 7 }
  ]
}
```

## Consequences
- First real code in the project
- Shelf design config format is established here — all future designs use this JSON structure
- The designer becomes a feature for other users to define their own shelf layouts
- No Rails needed for this POC — just open the HTML file in a browser
- Apply `/review` and layer honesty check before closing this POC (decisions 007, 008)
