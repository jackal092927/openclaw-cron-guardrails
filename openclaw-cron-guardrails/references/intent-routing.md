# Intent Routing for Natural-Language Cron Requests

This skill is primarily for upstream agents, not for direct human CLI usage.

When the user speaks in natural language, map the request into one of these intent buckets before choosing cron structure.

## Intent bucket 1 — Reminder / alarm

Typical phrases:
- `提醒我`
- `设个闹钟`
- `给我设个闹钟`
- `X 分钟后提醒我`
- `明天早上提醒我`
- `remind me`
- `alarm`

Default mapping:
- `sessionTarget: main`
- `payload.kind: systemEvent`
- one-shot schedule preferred

Do not overcomplicate these into isolated jobs unless the user explicitly wants background agent execution.

## Intent bucket 2 — Repeat loop / bounded repeat

Typical phrases:
- `每隔 X 分钟`
- `每 X 小时`
- `每隔 X 分钟跑一次`
- `执行 N 次`
- `总共执行 N 次`
- `未来 X 分钟执行 N 次`
- `loop`
- `tick`
- `循环跑`

Default mapping:
- reminder-like repeat → `main`
- worker-like repeat → `isolated + delivery.mode:none`

Important field extraction:
- interval
- total runs or stop condition
- whether output should be visible

## Intent bucket 3 — Session/thread prompt injection or push loop

Typical phrases:
- `对当前 session 做一个 prompt 注入`
- `给当前 agent 注入 prompt`
- `每隔 X 分钟 push 一下`
- `持续推进这个 agent`
- `继续推这个 thread`
- `push it as far as you can`

Default mapping:
- treat as session-aware scheduled agent action
- preserve current-session / current-thread semantics explicitly
- do not rely on `channel=last` when delivery matters

If the request is really “nudge the current agent/thread repeatedly”, prefer a pattern that binds explicitly to the current context.

## Intent bucket 4 — Scheduled worker / scan / monitor / digest

Typical phrases:
- `每天`
- `每晚`
- `定时`
- `daily`
- `nightly`
- `scan`
- `watch`
- `monitor`
- `定时检查`
- `定时汇总`
- `定时抓取`

Default mapping:
- internal work → `isolated + delivery.mode:none`
- visible report → `isolated + explicit delivery.channel + delivery.to`

## Recommended extraction schema

Before using scripts or rendering commands, normalize the request into this shape:

```json
{
  "intentType": "reminder | repeat-loop | session-injection | scheduled-worker",
  "scheduleType": "at | every | cron",
  "timeExpression": "natural-language time or cron expression",
  "interval": "optional",
  "runCount": 0,
  "stopCondition": "optional",
  "targetScope": "main | current-session | current-thread | explicit-target | internal-worker",
  "deliveryMode": "none | announce | webhook",
  "userVisible": true,
  "taskText": "what the job should do"
}
```

Then map that normalized form into the concrete cron spec.

## Trigger guidance for skill activation

This skill should trigger not only on explicit `cron` mentions, but also on natural-language scheduling requests such as:
- reminders
- alarms
- recurring nudges
- prompt injections into the current session/thread
- daily/weekly/nightly scans and summaries

If the request is obviously about scheduling future or repeated agent action, this skill is likely relevant even if the word `cron` never appears.
