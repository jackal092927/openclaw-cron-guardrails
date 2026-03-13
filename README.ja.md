# OpenClaw Cron Guardrails

> ⚠️ 初期公開版です。フィードバック歓迎。
>
> **言語:** [English](README.md) | [简体中文](README.zh-CN.md) | **日本語** | [Español](README.es.md)

> エージェント向けスケジュール処理のための、安全で説明可能なガードレイヤー。

`openclaw-cron-guardrails` は、ユーザーやエージェント製品が定期実行タスクをより安全に作れるようにするためのスキルです。主に次を扱います：
- セッションターゲット
- 配信ルーティング
- タイムアウト設定
- 可視出力先の明示指定

---

## ✨ これは何か

このプロジェクトは、すでに独自の base model / planner / prompt understanding を持つ **OpenClaw 系システム**向けです。

万能な自然言語 cron 解釈器を目指しているわけではありません。

重視しているのは：
- よくある定期タスク要求の easy-start patterns
- deterministic な validation / render フロー
- visible delivery と multi-channel routing の安全化
- session target、delivery target、timezone、timeout の明確な guardrails

> scheduled agent actions のための **guardrails + validation + render layer** と考えると分かりやすいです。

---

## 🚀 インストール

### 方法 A — ClawHub からインストール

```bash
clawhub install openclaw-cron-guardrails
```

### 方法 B — GitHub から clone

```bash
git clone https://github.com/jackal092927/openclaw-cron-guardrails.git
```

手動で skills を管理している場合は、skill フォルダをローカルの skills ディレクトリに配置してください。

### 方法 C — パッケージ済み artifact を使う

このリポジトリには次のパッケージ済み skill も含まれます：

```text
openclaw-cron-guardrails.skill
```

---

## ⚡ クイックスタート

ユーザーが「将来の時刻に実行したい」または「繰り返し実行したい」agent アクションを求めるときに使います。例：

- `20分後に返信するようリマインドして`
- `毎日9時にこのDiscordスレッドへ日次サマリーを投稿して`
- `毎晩スキャンを実行して、結果は内部だけに保持して`
- `この現在のスレッドを10分ごとに前へ進めて`
- `なぜこの cron は間違った場所に送られたの？`

---

## 🧩 統合モード

この skill は **3つの統合スタイル** をサポートします：

### 1) Natural-language quick start
向いている場面：
- 人がそのまま依頼する
- フレンドリーなデフォルト導線が欲しい

### 2) Normalized-intent handoff
向いている場面：
- 上流のモデルがすでに意図解釈を終えている
- cron に落とす前に安定した中間層が欲しい

### 3) Spec-first deterministic path
向いている場面：
- 自然言語の便利さより予測可能性が重要
- 作成すべき job がすでに明確

参照：
- `openclaw-cron-guardrails/references/integration-modes.md`

---

## 🛠️ よくある例

### Reminder

```text
20分後に返信するようリマインドして
```

推奨される安全パターン：
- `main + systemEvent`

### Visible scheduled delivery

```text
毎日9時に overnight summary をこの Discord チャンネルへ投稿して
```

推奨される安全パターン：
- `isolated + explicit delivery.channel + explicit delivery.to`

### Internal worker

```text
毎晩スキャンを実行して、ローカル状態を更新し、何も投稿しないで
```

推奨される安全パターン：
- `isolated + delivery.mode:none`

### Current-thread push loop

```text
この現在のスレッドを10分ごとに前へ進めて
```

推奨される安全パターン：
- session/thread-aware scheduled agent action
- current-thread の意図を明示的に保持

---

## 📦 含まれるもの

このリポジトリには以下が含まれます：

- `SKILL.md` — トップレベルの skill 説明
- `references/` — patterns、pitfalls、diagnostics、examples、integration guidance
- `scripts/` — parse、validate、render、create、lint、doctor、fix の補助スクリプト
- `openclaw-cron-guardrails.skill` — 配布用パッケージ

---

## 🔍 この skill が防ぎたい問題

この skill は、実際にローカル cron 環境で観測された失敗パターンをもとに設計されています。たとえば：

- multi-channel 環境での delivery-route failure
- `channel=last` のような脆弱な暗黙ルーティング
- channel はあるが explicit target がない構成
- 非 trivial job に対する implicit / 短すぎる timeout

> これらは理論上の edge case ではなく、この skill が存在する理由そのものです。

---

## 🚫 非目標

このプロジェクトは次を目指していません：

- 万能な prompt-understanding layer
- OpenClaw cron docs の完全な代替
- あらゆる provider/runtime failure に対する保証
- すべての歴史的 cron job を自動修復する仕組み

上流システムがより強いモデルを持っているなら、それで構いません。この skill はそれを置き換えるのではなく、その**下で guardrails を提供する**ためのものです。

---

## 💬 フィードバック歓迎

特に知りたいのは：

- どの scheduled-task pattern がまだ扱いにくいか
- どの routing / delivery case をもっと安全にすべきか
- docs / examples のどこが分かりにくいか
- どこで早めの clarification が必要か

---

## 🔗 リンク

- GitHub: <https://github.com/jackal092927/openclaw-cron-guardrails>
- ClawHub: <https://clawhub.ai/skills/openclaw-cron-guardrails>
