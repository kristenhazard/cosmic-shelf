# Decision: Rails + Hotwire + SQLite + ActionCable

**Date:** 2026-05-09  
**Status:** Decided  
**Weight:** Genesis

## Context
Needed a backend stack for the Raspberry Pi 5. Original plan was FastAPI + SQLite. User is fluent in Rails and JavaScript.

## Options Considered
- **FastAPI + SQLite** — lightweight, fast, Python; but unfamiliar to user, slower to iterate
- **Rails + SQLite + ActionCable + Hotwire** — heavier than FastAPI but well within Pi 5 capacity at this scale; familiar stack means faster, more confident development

## Decision
Rails full-stack with SQLite, ActionCable for WebSocket, Hotwire/Stimulus for the PWA remote. Familiarity wins at personal project scale.

## Consequences
- ActiveJob handles sync jobs (no separate worker process needed for simple cases)
- ActionCable replaces need for a standalone WebSocket server
- Hotwire means minimal custom JS for the remote control PWA
- Rails weight is acceptable on Pi 5 for this use case
