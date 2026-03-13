# OpenClaw Cron Guardrails

> ⚠️ 早期公開版本，歡迎回饋。
>
> **語言：** [English](README.md) | [简体中文](README.zh-CN.md) | **繁體中文** | [日本語](README.ja.md) | [Español](README.es.md) | [Français](README.fr.md) | [Deutsch](README.de.md)

> 一個安全、可解釋的 agent 定時任務防護層。

`openclaw-cron-guardrails` 用來協助使用者與 agent 產品更安全地建立定時任務，重點處理：
- session targeting
- delivery routing
- timeout 選擇
- 顯式可見輸出目標

---

## ✨ 這是什麼

這個專案面向的是 **OpenClaw 及類似系統**——也就是上層已經有自己的 base model、planner 或 prompt understanding 能力的系統。

它**不是**一個「萬能自然語言 cron 理解器」。

它更關注的是：
- 常見定時任務請求的 **easy-start patterns**
- **deterministic validation/render** 流程
- 更安全的 visible delivery 與 multi-channel routing 處理
- 對 session target、delivery target、timezone、timeout 的明確 guardrails

> 你可以把它理解成一個用於 scheduled agent actions 的 **guardrails + validation + render layer**。

---

## 🚀 安裝

### 方式 A — 從 ClawHub 安裝

```bash
clawhub install openclaw-cron-guardrails
```

### 方式 B — 從 GitHub clone

```bash
git clone https://github.com/jackal092927/openclaw-cron-guardrails.git
```

如果你是手動管理本地 skills，再把 skill 目錄放進你的 skills 資料夾中。

### 方式 C — 使用打包產物

倉庫中也附帶打包好的 skill 檔案：

```text
openclaw-cron-guardrails.skill
```

---

## ⚡ 快速開始

當使用者想要「定時」或「重複」執行某個 agent 動作時，就可以使用它，例如：

- `20 分鐘後提醒我回覆訊息`
- `每天 9 點把日報發到這個 Discord thread`
- `每晚跑一次 scan，但不要發訊息`
- `每隔 10 分鐘推進一下目前 thread`
- `為什麼這個 cron 發錯地方了？`

---

## 🧩 整合模式

這個 skill 支援 **三種整合方式**：

### 1) Natural-language quick start
適合：
- 人類直接提需求
- 想要一條更友善的預設路徑

### 2) Normalized-intent handoff
適合：
- 上游模型已經理解了使用者意圖
- 想在真正渲染 cron 之前先經過一個穩定的中間層

### 3) Spec-first deterministic path
適合：
- 可預測性比自然語言便利性更重要
- 系統已經明確知道要建立什麼 job

參考：
- `openclaw-cron-guardrails/references/integration-modes.md`

---

## 🛠️ 常見範例

### Reminder

```text
20 分鐘後提醒我回覆訊息
```

建議安全模式：
- `main + systemEvent`

### Visible scheduled delivery

```text
每天 9 點把 overnight summary 發到這個 Discord 頻道
```

建議安全模式：
- `isolated + explicit delivery.channel + explicit delivery.to`

### Internal worker

```text
每晚跑一次 scan，更新本地狀態，不要發訊息
```

建議安全模式：
- `isolated + delivery.mode:none`

### Current-thread push loop

```text
每隔 10 分鐘推進一下目前 thread
```

建議安全模式：
- session/thread-aware scheduled agent action
- 顯式保留 current-thread 語義

---

## 📦 倉庫內容

本倉庫包含：

- `SKILL.md` — skill 頂層說明
- `references/` — patterns、pitfalls、diagnostics、examples、integration guidance
- `scripts/` — parse、validate、render、create、lint、doctor、fix 等輔助腳本
- `openclaw-cron-guardrails.skill` — 可直接分發的打包產物

---

## 🔍 這個 skill 主要解決什麼問題

它的設計來自真實本地 cron 故障，而不是純理論推演，例如：

- multi-channel 環境下的 delivery-route failure
- `channel=last` 這類脆弱的隱式路由
- 只給 channel、不寫 explicit target
- 對非 trivial job 使用 implicit / too-short timeout

> 這些不是假想邊角案例，而正是這個 skill 存在的原因之一。

---

## ✅ 它擅長什麼

- 更安全地建立 scheduled task
- 提供更可解釋的 routing / delivery 預設行為
- 在建立前做結構化校驗
- 捕捉 risky visible-delivery 配置
- 同時支援 human-friendly 與 system-friendly 的整合方式

---

## 🚫 不做什麼

這個專案**不**試圖成為：

- 萬能 prompt-understanding 層
- OpenClaw cron 官方文件的完整替代品
- 對所有 provider/runtime failure 的萬能保證
- 能自動修好所有歷史 cron job 的系統

如果上游系統已經有更強的模型，那很好——這個 skill 的定位是**在它下面**提供 guardrails，而不是取代它。

---

## 💬 歡迎回饋

最有價值的回饋包括：

- 哪些 scheduled-task pattern 還不夠順手
- 哪些 routing/delivery case 還可以更安全
- 哪些 docs/examples 還不夠清楚
- 哪些地方應該更早觸發 clarification

歡迎透過 GitHub issues / discussions 提建議。

---

## 🔗 連結

- GitHub: <https://github.com/jackal092927/openclaw-cron-guardrails>
- ClawHub: <https://clawhub.ai/skills/openclaw-cron-guardrails>

---

## 🧪 目前狀態

目前發布狀態：**early public release / feedback-seeking**。
