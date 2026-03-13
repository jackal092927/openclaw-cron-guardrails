# openclaw-cron-guardrails

A safe, explainable scheduled-task layer for agent systems.

`openclaw-cron-guardrails` helps users and agent products create scheduled tasks with safer defaults around:
- session targeting
- delivery routing
- timeout choice
- explicit visible-output handling

## What it is

This project is designed for OpenClaw-like systems that already have their own base model or planner.
It does **not** try to be a universal natural-language interpreter.

Instead, it focuses on:
- easy-start patterns for common scheduled-task requests
- deterministic validation/render flows
- safer handling of visible delivery and multi-channel routing

## Integration modes

1. Natural-language quick start
2. Normalized-intent handoff
3. Spec-first deterministic path

See `openclaw-cron-guardrails/references/integration-modes.md`.

## Packaging

A packaged skill artifact is included:
- `openclaw-cron-guardrails.skill`

## Status

Current release status: early public release / feedback-seeking.
