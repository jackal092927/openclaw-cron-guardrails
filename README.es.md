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

---

## ✨ Qué es esto

Este proyecto está pensado para sistemas tipo **OpenClaw** que ya tienen su propio base model, planner o capa de comprensión de prompts.

No intenta ser un intérprete universal de cron en lenguaje natural.

Se centra en:
- easy-start patterns para solicitudes comunes
- flujos deterministas de validation / render
- manejo más seguro de visible delivery y multi-channel routing
- guardrails claros sobre session target, delivery target, timezone y timeout

> Piénsalo como una **capa de guardrails + validation + render** para acciones programadas de agentes.

---

## 🚀 Instalación

### Opción A — Instalar desde ClawHub

```bash
clawhub install openclaw-cron-guardrails
```

### Opción B — Clonar desde GitHub

```bash
git clone https://github.com/jackal092927/openclaw-cron-guardrails.git
```

Si gestionas skills manualmente, copia la carpeta del skill a tu directorio local de skills.

### Opción C — Usar el artefacto empaquetado

Este repositorio también incluye el artefacto empaquetado:

```text
openclaw-cron-guardrails.skill
```

---

## ⚡ Inicio rápido

Úsalo cuando el usuario pida una acción futura o repetida del agente, por ejemplo:

- `Recuérdame en 20 minutos que responda`
- `Publica el resumen diario en este hilo de Discord a las 9am`
- `Ejecuta un escaneo nocturno y mantenlo interno`
- `Empuja este hilo actual cada 10 minutos`
- `¿Por qué este cron se fue al lugar equivocado?`

---

## 🧩 Modos de integración

Este skill soporta **tres estilos de integración**:

### 1) Natural-language quick start
Mejor cuando:
- una persona lo pide directamente
- quieres una ruta por defecto más amigable

### 2) Normalized-intent handoff
Mejor cuando:
- un modelo upstream ya interpretó la intención
- quieres una capa intermedia estable antes de renderizar el cron

### 3) Spec-first deterministic path
Mejor cuando:
- la predictibilidad importa más que la conveniencia del lenguaje natural
- el sistema ya sabe exactamente qué job debe crear

Ver:
- `openclaw-cron-guardrails/references/integration-modes.md`

---

## 🛠️ Ejemplos comunes

### Reminder

```text
Recuérdame en 20 minutos que responda
```

Patrón seguro esperado:
- `main + systemEvent`

### Visible scheduled delivery

```text
Publica el resumen de la noche en este canal de Discord a las 9am cada día
```

Patrón seguro esperado:
- `isolated + explicit delivery.channel + explicit delivery.to`

### Internal worker

```text
Ejecuta un escaneo nocturno, actualiza el estado local y no publiques nada
```

Patrón seguro esperado:
- `isolated + delivery.mode:none`

### Current-thread push loop

```text
Empuja este hilo actual cada 10 minutos
```

Patrón seguro esperado:
- session/thread-aware scheduled agent action
- preservación explícita del current-thread

---

## 📦 Qué incluye

Este repositorio contiene:

- `SKILL.md` — instrucciones de alto nivel del skill
- `references/` — patterns, pitfalls, diagnostics, examples e integración
- `scripts/` — helpers para parse, validate, render, create, lint, doctor y fix
- `openclaw-cron-guardrails.skill` — artefacto empaquetado para distribución

---

## 🔍 Qué problemas intenta detectar

Este skill nació de fallos reales observados en cron locales, incluyendo:

- delivery-route failures en entornos multi-channel
- routing implícito frágil como `channel=last`
- channel explícito sin target explícito
- timeout implícito o demasiado corto para jobs no triviales

> No son edge cases teóricos; son parte de la razón por la que este skill existe.

---

## 🚫 No objetivos

Este proyecto **no** intenta ser:

- una capa universal de prompt understanding
- un reemplazo completo de la documentación de cron de OpenClaw
- una garantía contra todos los fallos de provider/runtime
- un sistema que repare automáticamente todos los cron históricos

Si el sistema upstream ya tiene un modelo más fuerte, perfecto: este skill está diseñado para ir **debajo** de él, no para reemplazarlo.

---

## 💬 Feedback bienvenido

Especialmente interesa saber:

- qué patrones de scheduled tasks todavía se sienten incómodos
- qué casos de routing/delivery deberían ser más seguros
- qué partes de la documentación o ejemplos siguen poco claras
- dónde el skill debería pedir aclaración antes

---

## 🔗 Enlaces

- GitHub: <https://github.com/jackal092927/openclaw-cron-guardrails>
- ClawHub: <https://clawhub.ai/skills/openclaw-cron-guardrails>
