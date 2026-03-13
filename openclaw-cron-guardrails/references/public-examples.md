# Public Examples

Use these examples to keep the skill prompt-first and user-shaped.

## 1) Plain reminder

User says:
- `20 分钟后提醒我回消息`
- `Remind me in 20 minutes to check the GPU run`

Preferred pattern:
- `main + systemEvent`

Why:
- simplest path
- avoids isolated delivery ambiguity

## 2) Recurring reminder

User says:
- `每天早上 9 点提醒我看 inbox`
- `Every weekday at 9am remind me to review email`

Preferred pattern:
- recurring `main + systemEvent`
- explicit timezone

Why:
- still a reminder, not a background worker

## 3) Internal background worker

User says:
- `每晚跑一次 scan，更新本地状态，不用发消息`
- `Run a nightly maintenance job and don't post anything`

Preferred pattern:
- `isolated + delivery.mode:none`

Why:
- background task
- not user-facing
- safest default for noisy work

## 4) Visible scheduled delivery

User says:
- `每天 9 点把 overnight summary 发到这个 Discord 频道`
- `Post the daily digest to this thread every morning`

Preferred pattern:
- `isolated + explicit delivery.channel + explicit delivery.to`

Why:
- visible output must have explicit route
- do not depend on implicit `last`

## 5) Current thread push loop

User says:
- `每隔 10 分钟推一下当前 thread`
- `Keep nudging this session forward every 15 minutes`

Preferred pattern:
- session/thread-aware scheduled agent action
- preserve current-thread intent explicitly

Why:
- this is not the same as a generic visible scheduled post

## 6) Diagnose an existing job

User says:
- `这个 cron 为什么没发出来？`
- `Why did this job go to the wrong place?`

Preferred pattern:
- inspect existing job
- inspect recent runs
- classify the failure before editing

Why:
- recreating blindly hides the real failure mode

## 7) Repair an existing job

User says:
- `把这个 job 改成发到当前 thread`
- `Increase the timeout on this cron and keep everything else the same`

Preferred pattern:
- modify only the broken dimension

Why:
- safer than rebuilding the job from scratch

## 8) Ambiguous request that should trigger clarification

User says:
- `每天早上帮我看一下 papers`
- `Set up a recurring task to keep an eye on this`

Likely clarification:
- Is this a plain reminder, a research task, or an internal monitoring worker?
- Should results be posted anywhere, or kept internal only?

Why:
- the request is scheduled, but the task type is not specific enough to create safely
