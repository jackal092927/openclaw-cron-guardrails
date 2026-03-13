---
name: openclaw-cron-guardrails
description: Create, review, repair, or route OpenClaw cron jobs and other scheduled agent actions with safe defaults and explicit delivery/session routing. Use when the user asks in natural language for reminders, alarms, recurring nudges, periodic scans, daily/nightly jobs, repeated prompt injection into the current session/thread, or any `openclaw cron add/edit/run` workflow. Especially use this in multi-channel setups (for example Discord + Feishu) or when delivery routing, `channel=last`, `sessionTarget`, `timeoutSeconds`, thread targeting, or natural-language schedule parsing could go wrong.
---

# OpenClaw Cron Guardrails

Use this skill when the user wants a scheduled or repeated action, even if they never say `cron`.

## What this skill is for

This skill is a guardrails layer for scheduled agent actions.

Treat OpenClaw cron as:
- scheduler
- agent runtime
- delivery router

The timer is usually not the hard part. The hard part is choosing the right task type, session target, and delivery target.

## Use this workflow

1. Classify the request.
2. Choose the safest job pattern.
3. Explain what will happen.
4. Create, inspect, or modify the job.
5. Verify after creation/edit.

## First classify the request

Choose one bucket first:

1. **Plain reminder**
- Example: `20 分钟后提醒我回消息`
- Default: `main + systemEvent`
- Use for lightweight reminders and nudges.

2. **Recurring reminder**
- Example: `每天早上提醒我看日历`
- Usually still `main + systemEvent`
- Set timezone explicitly if wall-clock time matters.

3. **Internal worker**
- Example: `每晚跑一次 scan，写本地状态就行`
- Default: `isolated + delivery.mode:none`
- Use for background chores, scans, maintenance, and noisy internal jobs.

4. **Scheduled visible delivery**
- Example: `每天 9 点把总结发到 Discord`
- Default: `isolated + explicit delivery.channel + explicit delivery.to`
- Do not rely on implicit `last` in multi-channel setups.

5. **Session/thread push or prompt injection**
- Example: `每隔 10 分钟推一下当前 thread`
- Preserve current-session / current-thread intent explicitly.
- Do not silently convert this into generic visible delivery.

6. **Diagnose / repair existing cron**
- Example: `这个 job 为什么没发出来` / `修一下这个 cron`
- Inspect first. Do not recreate blindly.

## Safe defaults

Use these defaults unless the user clearly wants something else:

- reminder → `main + systemEvent`
- background worker → `isolated + no-deliver`
- visible scheduled post → `isolated + explicit channel + explicit to`
- recurring job → explicit `tz`
- non-trivial task → `timeoutSeconds >= 180`

## When to ask instead of guessing

Ask a focused clarification if any of these are unclear:

- reminder vs worker vs visible scheduled post
- current thread/current session vs explicit external target
- concrete `channel` given but `to` missing
- research-task semantics are mixed into an otherwise normal reminder request
- wall-clock schedule matters but timezone is not clear

Do not hide ambiguity with clever defaults when the job could post to the wrong place.

## Output contract

When helping with a cron request, prefer this response order:

1. detected task type
2. chosen safe pattern
3. exact command or exact JSON/tool payload
4. why this pattern is safe
5. verification step

Keep the top-level explanation short and predictable.

## Verification after create/edit

Always verify after creating or editing a job:

1. `openclaw cron list`
2. `openclaw cron runs --id <jobId> --limit 5`
3. if safe, `openclaw cron run <jobId>`
4. confirm:
- correct `sessionTarget`
- correct payload kind
- correct timezone
- explicit delivery target, or explicit `none`
- plausible timeout

## Repair workflow

When repairing an existing job:

1. read the existing job
2. inspect recent runs
3. classify the failure
4. fix the root cause, not just the symptom

Common failure buckets:
- delivery-target ambiguity
- auth/permission/channel issue
- timeout too short
- schedule/timezone issue
- prompt/content issue

For a fuller triage flow, read `references/diagnostics.md`.

## What to read

Read only what you need.

### Beta-essential references
- `references/intent-routing.md` — intent buckets and normalization schema
- `references/patterns.md` — safe default patterns
- `references/pitfalls.md` — common failure modes and anti-patterns
- `references/public-examples.md` — prompt-first examples for real user requests

### Deeper references
- `references/integration-modes.md` — natural-language vs normalized-intent vs spec-first integration paths
- `references/diagnostics.md` — structured triage and repair flow
- `references/nl-parser-examples.md` — natural-language parser examples
- `references/intent-to-spec-examples.md` — normalized intent → cron spec examples
- `references/spec-examples.md` — cron spec JSON examples
- `references/target-helpers.md` — explicit target string guidance

## Scope boundary

Do not turn this skill into a universal prompt-understanding layer.

Its job is to:
- provide easy-start patterns
- enforce safe routing/session/delivery defaults
- support deterministic validate/render/create flows

Upstream products or base models may do richer prompt understanding than this skill does. That is fine.
Prefer stable interfaces over trying to cover every possible natural-language corner case here.

## Scripts

Use scripts when the user wants a deterministic, reusable cron definition instead of ad-hoc flag assembly.

### Parse / normalize
- `scripts/parse_nl_intent.py`
- `scripts/intent_to_cron_spec.py`

### Validate / render / create
- `scripts/validate_cron_spec.py`
- `scripts/render_cron_command.py`
- `scripts/create_cron.py`

### Inspect / doctor / repair
- `scripts/lint_existing_crons.py`
- `scripts/cron_fix.py`
- `scripts/cron_doctor.py`

## Non-goals

This skill is not:
- a full replacement for OpenClaw cron docs
- a guarantee against every runtime/provider failure
- a license to guess external delivery targets when intent is ambiguous

Prefer explicit, boring, reproducible cron definitions over clever shorthand.
