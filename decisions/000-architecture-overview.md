# Decision: Four-layer architecture diagram

**Date:** 2026-05-09  
**Status:** Decided

## Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                        COSMIC SHELF                             │
└─────────────────────────────────────────────────────────────────┘

  EXTERNAL SOURCES          RAILS BACKEND (Raspberry Pi 5)
  ─────────────────         ──────────────────────────────────────
  Audible CLI ──────────►  [ Content Layer ]
  Kindle/Playwright ────►  │  Books, covers, metadata (SQLite)   │
  Open Library API ─────►  │  Sync jobs (systemd 3am)            │
  Google Books API ─────►  └──────────────┬──────────────────────┘
                                          │
                           ┌──────────────▼──────────────────────┐
                           │  Shelf Design (pluggable config)    │
                           │  layout · spacing · effects · theme │
                           └──────────────┬──────────────────────┘
                                          │ design JSON
                           ┌──────────────▼──────────────────────┐
                           │  Renderer (Canvas API / PixiJS)     │
                           │  draws book covers to canvas        │
                           └──────────────┬──────────────────────┘
                                          │
                           ┌──────────────▼──────────────────────┐
                           │  Projection Adapter                 │
                           │  transforms · inversion · kiosk     │
                           │  ┌─────────┐ ┌─────────┐ ┌───────┐ │
                           │  │Pepper's │ │Pyramid  │ │Monster│ │
                           │  │ Ghost   │ │         │ │       │ │
                           │  └─────────┘ └─────────┘ └───────┘ │
                           └─────────────────────────────────────┘

  REMOTE CONTROL
  ──────────────
  Phone/Tablet PWA ◄──── ActionCable (WebSocket) ◄──── Rails
  (Hotwire/Stimulus)      cosmic.local
```

## Key principles

- Data flows down through the layers
- The projection adapter is the only layer that knows about physical display hardware
- Each layer is independently swappable
- See `003-four-layer-architecture.md` and `005-projection-agnostic.md` for the reasoning behind this structure
