# Decision Driven Development (DDD)

## What it is

Decision Driven Development is a lightweight methodology for AI-assisted software development. Instead of maintaining a spec that describes what the code does, you maintain a record of *why* decisions were made. The code is the live truth. The decisions explain how you got there.

## The problem it solves

Spec Driven Development asks you to write a document before you build, then keep it current as the code evolves. In practice, the spec lags the code, becomes unreliable, and feels like a second job. Most people stop maintaining it.

DDD sidesteps this because decision records are **append-only**. You never update a decision — you supersede it with a new one. There is no "keep it current" burden. The record of what you decided and why is frozen in time at the moment of highest understanding: right after you've dialoged through the tradeoffs and chosen a path.

## The process

```
Dialog → POC (if needed) → Decision record
```

1. **Dialog first.** Talk through the problem, surface tradeoffs, reach clarity before writing any code.
2. **POC if needed.** For uncertain decisions, build the smallest possible thing to validate the approach. Review it before declaring it done.
3. **Record the decision.** Capture what was decided, what was considered, and why. One file per decision.

Never start building without a decision to justify it.

## Folder structure

```
decisions/
  template.md
  000-architecture-overview.md
  001-first-decision.md
  002-second-decision.md
  ...
  INDEX.md          ← add when records exceed ~30
```

Decisions are numbered sequentially. Lower numbers are foundational; higher numbers build on or supersede them.

## Decision record template

```markdown
# Decision: [Title]

**Date:** YYYY-MM-DD
**Status:** Draft | Decided | Superseded
**Type:** Architecture | Practice | Tool
**Weight:** Genesis | Core | Pivot | Practice
**Supersedes:** (decision number, if applicable)

## Context
What problem or question prompted this decision?

## Options Considered
- **Option A** — pros / cons
- **Option B** — pros / cons

## Decision
What we decided and why.

## Consequences
What becomes easier, harder, or constrained as a result.
```

## Decision weights

| Weight | Meaning |
|---|---|
| **Genesis** | Foundational — everything else is built on this. Stack choice, core architecture, the process itself. |
| **Core** | Load-bearing — changing this would ripple widely. Key technology or design choices. |
| **Pivot** | A change of direction — supersedes or significantly redirects prior thinking. |
| **Practice** | How we work — process, testing, review conventions. Important but not architectural. |

When building an INDEX.md at scale, Genesis and Pivot records are the ones to read first.

## What earns a record

Not every decision needs a record. Apply this test: *would a new team member be confused or make a different choice without knowing this?*

| Record it | Don't record it |
|---|---|
| Architectural choices | Function names |
| Technology selections | File organization discoverable by reading the code |
| Practices the team has agreed to follow | Anything the code makes self-evident |
| Decisions where you weighed real tradeoffs | Implementation details |
| Anything that would surprise a new contributor | |

The discipline is knowing what **not** to record as much as what to record. Over-recording creates noise that makes the valuable records harder to find.

## Scaling

At ~30+ decisions, add an `INDEX.md` — one line per decision — so you can scan quickly and pull only what's relevant. This prevents the records from flooding AI context windows during reviews.

## Four practices that keep the code maintainable

These work alongside DDD and should themselves be recorded as practice decisions:

1. **Tests alongside code, not after.** Each POC is not done until tests exist for its core behavior.
2. **Review before closing a POC.** Run a code review before declaring an approach decided. AI-generated code can be subtly wrong.
3. **Layer honesty checks.** At each review, ask: is this logic in the right layer? Fix drift before it compounds.
4. **Keep POCs small.** If a POC touches multiple concerns, split it. A good POC fits in a single reviewable unit.

## Integration with AI tools

DDD pairs naturally with AI-assisted development because:

- **Dialog is the first step** — AI is a natural dialog partner for surfacing tradeoffs
- **Decision records become AI context** — future AI sessions can read the records to understand the codebase without re-deriving decisions from code
- **Append-only records match AI memory** — AI tools can reference past decisions without needing to track edits
- **POC reviews keep AI output honest** — the review gate catches the subtle errors AI code is prone to

In Claude Code specifically, reference your `decisions/` folder in `CLAUDE.md` so every session starts with awareness that the records exist.

## What DDD is not

- It is not a replacement for good code, tests, or documentation
- It is not a process for capturing every micro-decision
- It is not a spec — the code is the spec, the decisions explain the journey
