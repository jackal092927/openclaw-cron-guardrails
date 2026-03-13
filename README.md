# OpenClaw Cron Guardrails

> ⚠️ Early public release. Feedback welcome.
>
> **Languages:** **English** | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [日本語](README.ja.md) | [Español](README.es.md)

> A safe, explainable scheduled-task layer for agent systems.

`openclaw-cron-guardrails` helps users and agent products create scheduled tasks with safer defaults around:
- session targeting
- delivery routing
- timeout choice
- explicit visible-output handling

---

## ✨ What this is

This project is designed for **OpenClaw-like systems** that already have their own base model, planner, or prompt-understanding layer.

It does **not** try to be a universal natural-language cron interpreter.

Instead, it focuses on:
- **easy-start patterns** for common scheduled-task requests
- **deterministic validation/render flows**
- **safer handling** of visible delivery and multi-channel routing
- **clear guardrails** around session target, delivery target, timezone, and timeout choices

> Think of it as a **guardrails + validation + render layer** for scheduled agent actions.

---

## 🚀 Installation

### Option A — Install from ClawHub

```bash
clawhub install openclaw-cron-guardrails
```

### Option B — Clone from GitHub

```bash
git clone https://github.com/jackal092927/openclaw-cron-guardrails.git
```

Then copy the skill folder into your local skills directory if you manage skills manually.

### Option C — Use the packaged artifact

This repo also includes a packaged skill artifact:

```text
openclaw-cron-guardrails.skill
```

---

## ⚡ Quick start

Use it when the user asks for scheduled or repeated agent actions, for example:

- `Remind me in 20 minutes to reply`
- `Post the daily summary to this Discord thread at 9am`
- `Run a nightly scan and keep it internal`
- `Push this current thread every 10 minutes`
- `Why did this cron go to the wrong place?`

---

## 🧩 Integration modes

This skill supports **three integration styles**:

### 1) Natural-language quick start
Best when:
- a human is asking directly
- you want a friendly default path

### 2) Normalized-intent handoff
Best when:
- an upstream model already interpreted the request
- you want a stable middle layer before cron rendering

### 3) Spec-first deterministic path
Best when:
- predictability matters more than natural-language convenience
- your system already knows exactly what job should be created

See:
- `openclaw-cron-guardrails/references/integration-modes.md`

---

## 🛠️ Common examples

### Reminder

```text
Remind me in 20 minutes to reply
```

Expected safe pattern:
- `main + systemEvent`

### Visible scheduled delivery

```text
Post the overnight summary to this Discord channel at 9am every day
```

Expected safe pattern:
- `isolated + explicit delivery.channel + explicit delivery.to`

### Internal worker

```text
Run a nightly scan, update local state, and do not post anything
```

Expected safe pattern:
- `isolated + delivery.mode:none`

### Current-thread push loop

```text
Push this current thread every 10 minutes
```

Expected safe pattern:
- session/thread-aware scheduled agent action
- explicit current-thread preservation

---

## 📦 What's included

This repo contains:

- `SKILL.md` — top-level skill instructions
- `references/` — patterns, pitfalls, diagnostics, examples, integration guidance
- `scripts/` — parse, validate, render, create, lint, doctor, and fix helpers
- `openclaw-cron-guardrails.skill` — packaged artifact for distribution

---

## 🔍 What problems this is designed to catch

This skill was shaped by real cron failure patterns observed in local use, including:

- delivery-route failures in multi-channel setups
- fragile implicit routing such as `channel=last`
- explicit channel without explicit target
- implicit or too-short timeout on non-trivial jobs

> These are not theoretical edge cases; they are part of the reason this skill exists.

---

## ✅ What this skill is good at

- safer scheduled-task creation
- explainable routing and delivery defaults
- structured validation before creation
- catching risky visible-delivery configurations
- supporting both human-friendly and system-friendly integration paths

---

## 🚫 Non-goals

This project is **not** trying to be:

- a universal prompt-understanding layer
- a full replacement for OpenClaw cron docs
- a guarantee against every provider/runtime failure
- a fully automatic repair system for every historical cron job

If an upstream system already has a stronger model, that is fine — this skill is designed to sit **under** it, not replace it.

---

## 💬 Feedback welcome

Best feedback areas:

- scheduled-task patterns that still feel awkward
- routing/delivery cases that should be safer
- docs/examples that are still unclear
- where the skill should ask for clarification earlier

GitHub issues / discussions are the best place to report problems or improvement ideas.

---

## 🔗 Links

- GitHub: <https://github.com/jackal092927/openclaw-cron-guardrails>
- ClawHub: <https://clawhub.ai/skills/openclaw-cron-guardrails>

---

## 🧪 Status

Current release status: **early public release / feedback-seeking**.
