# Decision: AI-Native SDLC as the development process

**Date:** 2026-05-09  
**Status:** Decided  
**Weight:** Genesis  
**Type:** Practice

## Context
Researched how frontier AI companies (Anthropic, OpenAI, Google, Meta) actually manage their SDLC. Found strong convergence on a five-phase model that validates and extends our Decision Driven Development approach.

## What frontier companies do

**Anthropic**: CLAUDE.md as versioned architectural memo + interview-first SPEC.md workflow + parallel Claude instances (2-4 is the sweet spot). 90% of Claude Code is written by Claude Code itself. Engineers spend 70%+ of time reviewing rather than writing.

**OpenAI**: ~100-line AGENTS.md as table of contents pointing to a `docs/` directory. Doc-gardening agent scans for stale docs and opens fix PRs. Zero manually-written lines in their harness engineering experiment — 3.5 PRs/engineer/day.

**Google**: 75% of new code is AI-generated. Every commit still passes human review and automated testing. Engineers are increasingly reviewers, not writers.

**Consensus**: Spec-before-code, but spec is a *dialog output* not an upfront document. Context quality is the fundamental constraint. Evals in CI are non-negotiable.

## Decision
Adopt the five-phase AI-native SDLC for all development on this project:

1. **Spec/Plan** — dialog-first. Capture intent in a decision record before writing code. For larger features, generate a SPEC.md via dialog.
2. **Context Assembly** — CLAUDE.md, relevant decision records, and current codebase state are the context. Keep them pruned and accurate.
3. **Agent Execution** — Claude implements against the spec. POCs are small and isolated (decision 009).
4. **Verification** — tests run, `/review` runs, layer honesty is checked (decisions 006, 007, 008). Evals gate merges.
5. **Knowledge Update** — decision record written, CLAUDE.md updated if needed, evals grow from any failures caught.

## Consequences
- Every phase is explicit — no skipping straight to code
- The `decisions/` folder serves as both the spec system and the knowledge base
- Evals become a required gate, not optional (see decision 012)
- As the project grows, add a `decisions/INDEX.md` (~100 lines max) and let individual records stay deep
- This process applies to both Cosmic Shelf and the FTA project at Fulcrum — it is stack-agnostic
