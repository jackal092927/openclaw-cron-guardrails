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

## これは何か

このプロジェクトは、すでに独自の base model / planner / prompt understanding を持つ **OpenClaw 系システム**向けです。

万能な自然言語 cron 解釈器を目指しているわけではありません。

重視しているのは：
- よくある定期タスク要求の easy-start patterns
- deterministic な validation / render フロー
- visible delivery と multi-channel routing の安全化

詳しくは英語版 README を参照してください。

- GitHub: <https://github.com/jackal092927/openclaw-cron-guardrails>
- ClawHub: <https://clawhub.ai/skills/openclaw-cron-guardrails>
