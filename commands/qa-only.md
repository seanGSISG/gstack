---
name: gstack:qa-only
description: "Report-only QA testing. Find bugs, report them, never fix."
argument-hint: [url] [--quick|--regression]
allowed-tools:
  - Bash
  - Read
  - Write
  - Grep
  - Glob
  - AskUserQuestion
---

Read `~/.claude/skills/gstack/qa-only/SKILL.md` and follow every instruction in it exactly.

User's request: $ARGUMENTS
