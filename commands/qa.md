---
name: gstack:qa
description: "QA test a web app, find bugs, fix them with atomic commits."
argument-hint: [url] [--quick|--regression]
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - Grep
  - Glob
  - AskUserQuestion
---

Read `~/.claude/skills/gstack/qa/SKILL.md` and follow every instruction in it exactly.

User's request: $ARGUMENTS
