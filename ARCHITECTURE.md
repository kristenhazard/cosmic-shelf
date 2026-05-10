# Architecture Overview

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
                           │  │Pepper's │ │Pyramid  │ │Monitor│ │
                           │  │ Ghost   │ │         │ │       │ │
                           │  └─────────┘ └─────────┘ └───────┘ │
                           └─────────────────────────────────────┘

  REMOTE CONTROL
  ──────────────
  Phone/Tablet PWA ◄──── ActionCable (WebSocket) ◄──── Rails
  (Hotwire/Stimulus)      cosmic.local
```

The four layers stack vertically — data flows down. The projection adapter is the only layer that knows about physical display hardware. See `decisions/` for the reasoning behind each layer.
