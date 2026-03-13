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

## Known issues this skill is designed to catch

This skill was shaped by real cron failure patterns observed in local use, including:
- delivery-route failures in multi-channel setups
- fragile implicit routing such as `channel=last`
- explicit channel without explicit target
- implicit or too-short timeout on non-trivial jobs

These are not theoretical edge cases; they are part of the reason this skill exists.

## Status

Current release status: early public release / feedback-seeking.
