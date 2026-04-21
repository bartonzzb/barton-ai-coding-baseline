# AI Coding Baseline

> Stop losing context between sessions. One file, zero friction.

## The Problem

Karpathy's 4 rules solve **how AI codes**. BASELINE.md solves **what happens between sessions**.

Every AI coding assistant (Claude Code, Cursor, Windsurf, Copilot, WorkBuddy) has the same flaw: when a new session starts, it forgets everything. Project state, scope boundaries, in-progress tasks, forbidden zones — gone. The result:

- AI re-introduces bugs you already fixed
- AI modifies modules it shouldn't touch
- AI "improves" code that was intentionally left as-is
- You spend 10 minutes re-explaining context every session

## The Solution

A single `BASELINE.md` file in your project root. The AI reads it at session start, updates it at session end. Context persists. Boundaries are enforced.

## Quick Start

1. Copy `BASELINE.md` to your project root
2. Fill in the fields (takes 2 minutes)
3. Add this instruction to your AI tool's system prompt or rules file:

```
On session start, read BASELINE.md in the project root. 
Before any task, confirm current state and scope boundaries. 
After completing any task, update BASELINE.md.
```

That's it. No config files, no plugins, no dependencies.

## What Goes in BASELINE.md

| Field | Purpose | Example |
|-------|---------|---------|
| Project Info | What is this project | "Liquid cooling pump controller, v2.3" |
| Tech Stack | Core technologies | "STM32 HAL, FreeRTOS, C99" |
| In Progress | What's being worked on | "T4: UART driver refactor" |
| Completed | What's done | "T1: ADC calibration - 2024-03-15" |
| Boundaries | What NOT to touch | "Never modify safety_monitor.c without approval" |
| Owner Approval | Change control log | "2024-03-20: Approved T5 scope change" |

## Rules

1. **Lean**: Each field, 1-3 lines max. No history logs.
2. **Current state only**: BASELINE is a snapshot, not a changelog.
3. **Completed tasks**: ID + name + date only. No details.
4. **Before any task**: Read BASELINE to confirm state and boundaries.
5. **After any task**: Update BASELINE with new state.
6. **No deploy/release without owner approval.**
7. **On "baseline restore" command**: Read BASELINE, restore full context, then respond.

## Why It Works

AI assistants are excellent at following structured instructions — if those instructions exist in context. BASELINE.md ensures critical project state is always in context, without manual re-explanation.

The file is deliberately minimal. A 15-line file that the AI actually reads is infinitely more valuable than a 500-line document that the AI ignores.

## Compatible With

- Claude Code (add to `CLAUDE.md` or `.claude/settings.json`)
- Cursor (add to `.cursor/rules/`)
- Windsurf (add to `.windsurfrules`)
- WorkBuddy (use as skill reference)
- GitHub Copilot (add to `.github/copilot-instructions.md`)
- Any tool that reads project files — just put `BASELINE.md` in your root

## License

MIT

---

**Karpathy's rules make AI code better. BASELINE makes AI remember. Use both.**
