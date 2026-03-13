# OpenClaw Cron Guardrails

> ⚠️ Versión pública temprana. Se agradece feedback.
>
> **Idiomas:** [English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | **Español**

> Una capa segura y explicable para tareas programadas en sistemas de agentes.

`openclaw-cron-guardrails` ayuda a usuarios y productos de agentes a crear tareas programadas con defaults más seguros para:
- session targeting
- delivery routing
- timeout choices
- explicit visible-output handling

## Qué es esto

Este proyecto está pensado para sistemas tipo **OpenClaw** que ya tienen su propio base model, planner o capa de comprensión de prompts.

No intenta ser un intérprete universal de cron en lenguaje natural.

Se centra en:
- easy-start patterns para solicitudes comunes
- flujos deterministas de validation / render
- manejo más seguro de visible delivery y multi-channel routing

Para la documentación completa, consulta el README en inglés.

- GitHub: <https://github.com/jackal092927/openclaw-cron-guardrails>
- ClawHub: <https://clawhub.ai/skills/openclaw-cron-guardrails>
