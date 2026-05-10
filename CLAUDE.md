# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

Cosmic Shelf displays a user's digital book collection (Audible, Kindle) as book covers arranged on a physical shelf. It is designed to be shared — different users will have different shelf designs and different projection setups. Modularity at both the design and display layer is a first-class concern.

## Architecture

Four distinct layers, kept loosely coupled:

**1. Content layer** (`app/models`, `app/jobs`)
Rails models and background jobs. Books, covers, metadata. Syncs Audible via `audible-cli` and Kindle via Playwright (persistent browser session — user logs in once). Cover images and metadata fetched from Open Library with Google Books fallback, cached locally. Projection-agnostic: this layer knows nothing about how content is displayed.

**2. Shelf design** (`app/designs/` or `config/designs/`)
Pluggable design configs (Ruby modules or JSON) defining cover layout, sizing, spacing, visual effects, and orientation. Users choose or create a design. A design is swappable without touching rendering code.

**3. Renderer** (`app/javascript/renderer/`)
Vanilla JS or PixiJS that takes a design config and a book list and draws to a canvas. Knows nothing about projection technology — it just renders to screen coordinates.

**4. Projection adapter** (`app/javascript/projection/`)
Thin config layer: screen orientation, CSS transforms, inversion (Pepper's Ghost needs mirrored output), window mode vs. fullscreen kiosk. Swapping projection technology means swapping adapter config, not rewriting the renderer.

**Remote control** (`app/javascript/remote/` + ActionCable)
Hotwire/Stimulus PWA for phone/tablet. Controls which shelf design is active, triggers syncs, browses the library. Communicates via ActionCable WebSocket — no separate WebSocket server needed.

## Tech Stack

- **Backend:** Ruby on Rails + Litestack (SQLite-backed database, queue, cache, and ActionCable — zero external dependencies)
- **Real-time:** ActionCable via Litestack (solid_cable)
- **Frontend remote:** Hotwire (Turbo + Stimulus) — minimal custom JS
- **Display renderer:** PixiJS v8 or plain Canvas API, driven by design config JSON
- **Target hardware:** Raspberry Pi 5, Chromium in kiosk mode
- **Network:** mDNS at `cosmic.local`
- **Sync schedule:** systemd timer (nightly, ~3am)

## Commands

```bash
# Start development server
bin/rails server

# Run background jobs (sync)
bin/jobs   # or: bundle exec sidekiq / solid_queue

# Run tests
bin/rails test
bin/rails test test/models/book_test.rb   # single test file

# JS build (if using jsbundling-rails)
yarn build --watch

# Lint
bundle exec rubocop
yarn lint
```

## Decision Records

Significant decisions are tracked in `decisions/`. Each file records what was decided, what was considered, and why. Before proposing changes that touch architecture or core conventions, check the relevant decision record. When a new decision is made, create the next numbered file using `decisions/template.md`.

## Key Design Rules

- **Projection-agnostic renderer:** Never let rendering code assume a specific projection technology. Pepper's Ghost, holographic pyramid, plain monitor — all are valid targets.
- **Design modularity:** A "shelf design" is a config/module, not hardcoded logic. New designs should require zero changes to the renderer core.
- **Renderer draws a realistic shelf** — warm wood tones, shelf geometry, covers in compartments. It mirrors the physical shelf visually. Black backgrounds and mirroring are projection adapter concerns, not renderer concerns.
- **Cover images are the primary visual unit** — not spines. Source from Open Library first, Google Books as fallback, cache locally after first fetch.
- **No internet required at runtime** — all metadata and covers are cached locally in SQLite after sync.
- **Audible/Kindle auth is out-of-band** — never automate those login flows. `audible-cli` handles Audible auth; Playwright session handles Kindle. User logs in manually once.
