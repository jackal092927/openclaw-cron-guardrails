# OpenClaw Cron Guardrails

> ⚠️ 早期公开版本，欢迎反馈。
>
> **语言：** [English](README.md) | **简体中文** | [日本語](README.ja.md) | [Español](README.es.md) | [Français](README.fr.md) | [Deutsch](README.de.md)

> 一个安全、可解释的 agent 定时任务防护层。

`openclaw-cron-guardrails` 用来帮助用户和 agent 产品更安全地创建定时任务，重点处理：
- session targeting
- delivery routing
- timeout 选择
- 显式可见输出目标

---

## ✨ 这是什么

这个项目面向的是 **OpenClaw 及类似系统**——也就是上层已经有自己的 base model、planner 或 prompt understanding 能力的系统。

它**不是**一个“万能自然语言 cron 理解器”。

它更关注的是：
- 常见定时任务请求的 **easy-start patterns**
- **deterministic validation/render** 流程
- 更安全的 visible delivery 与 multi-channel routing 处理
- 对 session target、delivery target、timezone、timeout 的明确 guardrails

> 你可以把它理解成一个用于 scheduled agent actions 的 **guardrails + validation + render layer**。

---

## 🚀 安装

### 方式 A — 从 ClawHub 安装

```bash
clawhub install openclaw-cron-guardrails
```

### 方式 B — 从 GitHub 克隆

```bash
git clone https://github.com/jackal092927/openclaw-cron-guardrails.git
```

如果你是手动管理本地 skills，再把 skill 目录放入你的 skills 文件夹中。

### 方式 C — 使用打包产物

仓库中也附带打包好的 skill 文件：

```text
openclaw-cron-guardrails.skill
```

---

## ⚡ 快速开始

当用户想要“定时”或“重复”执行某个 agent 动作时，就可以使用它，例如：

- `20 分钟后提醒我回复消息`
- `每天 9 点把日报发到这个 Discord thread`
- `每晚跑一次 scan，但不要发消息`
- `每隔 10 分钟推进一下当前 thread`
- `为什么这个 cron 发错地方了？`

---

## 🧩 集成模式

这个 skill 支持 **三种集成方式**：

### 1) Natural-language quick start
适合：
- 人类直接提需求
- 想要一条更友好的默认路径

### 2) Normalized-intent handoff
适合：
- 上游模型已经理解了用户意图
- 想在真正渲染 cron 之前先经过一个稳定中间层

### 3) Spec-first deterministic path
适合：
- 可预测性比自然语言便利性更重要
- 系统已经明确知道要创建什么 job

参考：
- `openclaw-cron-guardrails/references/integration-modes.md`

---

## 🛠️ 常见示例

### Reminder

```text
20 分钟后提醒我回复消息
```

推荐安全模式：
- `main + systemEvent`

### Visible scheduled delivery

```text
每天 9 点把 overnight summary 发到这个 Discord 频道
```

推荐安全模式：
- `isolated + explicit delivery.channel + explicit delivery.to`

### Internal worker

```text
每晚跑一次 scan，更新本地状态，不要发消息
```

推荐安全模式：
- `isolated + delivery.mode:none`

### Current-thread push loop

```text
每隔 10 分钟推进一下当前 thread
```

推荐安全模式：
- session/thread-aware scheduled agent action
- 显式保留 current-thread 语义

---

## 📦 仓库内容

本仓库包含：

- `SKILL.md` — skill 顶层说明
- `references/` — patterns、pitfalls、diagnostics、examples、integration guidance
- `scripts/` — parse、validate、render、create、lint、doctor、fix 等辅助脚本
- `openclaw-cron-guardrails.skill` — 可直接分发的打包产物

---

## 🔍 这个 skill 主要解决什么问题

它的设计来自真实本地 cron 故障，而不是纯理论推演，例如：

- multi-channel 环境下的 delivery-route failure
- `channel=last` 这类脆弱的隐式路由
- 只给 channel、不写 explicit target
- 对非 trivial job 使用 implicit / too-short timeout

> 这些不是假想边角案例，而正是这个 skill 存在的原因之一。

---

## ✅ 它擅长什么

- 更安全地创建 scheduled task
- 提供更可解释的 routing / delivery 默认行为
- 在创建前做结构化校验
- 捕捉 risky visible-delivery 配置
- 同时支持 human-friendly 和 system-friendly 的集成方式

---

## 🚫 不做什么

这个项目**不**试图成为：

- 万能 prompt-understanding 层
- OpenClaw cron 官方文档的完整替代品
- 对所有 provider/runtime failure 的万能保证
- 能自动修好所有历史 cron job 的系统

如果上游系统已经有更强的模型，那很好——这个 skill 的定位是**在它下面**提供 guardrails，而不是替代它。

---

## 💬 欢迎反馈

最有价值的反馈包括：

- 哪些 scheduled-task pattern 还不够顺手
- 哪些 routing/delivery case 还可以更安全
- 哪些 docs/examples 还不够清楚
- 哪些地方应该更早触发 clarification

欢迎通过 GitHub issues / discussions 提建议。

---

## 🔗 链接

- GitHub: <https://github.com/jackal092927/openclaw-cron-guardrails>
- ClawHub: <https://clawhub.ai/skills/openclaw-cron-guardrails>

---

## 🧪 当前状态

当前发布状态：**early public release / feedback-seeking**。
