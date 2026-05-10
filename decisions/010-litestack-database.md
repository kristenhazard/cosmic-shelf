# Decision: Litestack over vanilla SQLite or PostgreSQL

**Date:** 2026-05-09  
**Status:** Decided  
**Type:** Architecture

## Context
Needed a database strategy for a single-user Rails app running on Raspberry Pi 5. Three realistic options considered.

## Options Considered
- **PostgreSQL** — industry standard, full-featured, concurrent writes; but overkill for single-user, requires separate process, more RAM, more to maintain on a Pi
- **Vanilla SQLite** — file-based, zero config, great Pi fit; but requires separate solutions for queue, cache, and ActionCable
- **Litestack** — SQLite-backed implementations of everything: database, cache, queue (solid_queue), and ActionCable (solid_cable); entire Rails stack runs with zero external dependencies

## Decision
Litestack. The whole app — database, background jobs, caching, WebSocket — runs on SQLite with no external processes. Rails 8 makes this first-class. Perfect fit for a self-contained Pi deployment.

## Consequences
- Zero external dependencies on the Pi (no Redis, no separate queue process)
- Entire app state lives in SQLite files — simple to backup and move
- If Cosmic Shelf ever becomes a hosted multi-user service, PostgreSQL migration would be needed — acceptable tradeoff given current scope
- Add `litestack` gem to Gemfile; configure solid_queue, solid_cache, solid_cable accordingly
