# Barton AI Coding Baseline

> Karpathy's rules fused with cross-session context management. One file, zero friction.

## What Is This

This project combines two things into a single, portable framework:

| Component | Origin | What It Solves |
|-----------|--------|----------------|
| Rules 1-4: Think Before Coding, Simplicity First, Surgical Changes, Goal-Driven Execution | Based on [Andrej Karpathy](https://github.com/karpathy)'s observations on LLM coding pitfalls | How AI codes within a session |
| Rule 5: Baseline Context + `BASELINE.md` | Original contribution by [Barton](https://github.com/bartonzzb) | What happens between sessions |

Karpathy identified that LLMs make bad assumptions, overcomplicate code, and make unrelated edits. The existing solutions (e.g., [andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills)) package his 4 rules into CLAUDE.md — but they solve only half the problem.

**The missing half**: when a new session starts, the AI forgets everything. Project state, scope boundaries, in-progress tasks, forbidden zones — gone. You spend 10 minutes re-explaining context every time.

This project fuses Karpathy's 4 rules with a 5th original rule (Baseline Context) and a `BASELINE.md` template to solve both halves.

## The 5 Rules

### Rule 1: Think Before Coding *(Karpathy)*

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them — don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

### Rule 2: Simplicity First *(Karpathy)*

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

### Rule 3: Surgical Changes *(Karpathy)*

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it — don't delete it.

### Rule 4: Goal-Driven Execution *(Karpathy)*

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

### Rule 5: Baseline Context *(Barton - Original)*

**Read before acting. Stay in bounds. Never lose context.**

On session start:
- If `BASELINE.md` exists in the project root, read it before responding to any request.
- On receiving "baseline restore", read `BASELINE.md` first, restore full context, then respond.

Before starting any task:
- Read `BASELINE.md` to confirm current state and scope boundaries.
- No out-of-scope changes without explicit owner approval.
- No deploy/release without explicit owner approval.

After completing any task:
- Update `BASELINE.md`: completed tasks, current version, in-progress status.
- Before ending the session, remind the owner to confirm whether `BASELINE.md` needs updating.

## Quick Start

1. Copy `BASELINE.md` to your project root
2. Fill in the fields (takes 2 minutes)
3. Copy the integration file for your AI tool into the appropriate location:

| Tool | File | Location |
|------|------|----------|
| Claude Code | `integrations/claude-code/CLAUDE.md` | Project root or `.claude/settings.json` |
| Cursor | `integrations/cursor/baseline-rules.mdc` | `.cursor/rules/` |
| Windsurf | `integrations/windsurf/.windsurfrules` | Project root |
| GitHub Copilot | `integrations/copilot/copilot-instructions.md` | `.github/copilot-instructions.md` |

## What Goes in BASELINE.md

| Field | Purpose | Example |
|-------|---------|---------|
| Project Info | What is this project | "Liquid cooling pump controller, v2.3" |
| Tech Stack | Core technologies | "STM32 HAL, FreeRTOS, C99" |
| In Progress | What's being worked on | "T4: UART driver refactor" |
| Completed | What's done | "T1: ADC calibration - 2024-03-15" |
| Boundaries | What NOT to touch | "Never modify safety_monitor.c without approval" |
| Owner Approval | Change control log | "2024-03-20: Approved T5 scope change" |

## BASELINE.md Format Rules

1. **Lean**: Each field, 1-3 lines max. No history logs.
2. **Current state only**: BASELINE is a snapshot, not a changelog.
3. **Completed tasks**: ID + name + date only. No details.

## Why It Works

Karpathy's insight: *"LLMs are very good at looping until a specific goal is met — don't tell it what to do, give it success criteria and let it execute."*

This project extends that insight: the same LLM capability works for context management — if the context exists in a structured, minimal file that the AI reads at session start. A 15-line `BASELINE.md` that the AI actually reads beats a 500-page wiki that it ignores.

## Acknowledgments

- Rules 1-4 are based on [Andrej Karpathy](https://github.com/karpathy)'s public observations on LLM coding behavior, as popularized by [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills).
- Rule 5 (Baseline Context) and the `BASELINE.md` template are original contributions.

## License

MIT

---

**Karpathy's rules make AI code better. Barton's baseline makes AI remember. Fused into one.**
