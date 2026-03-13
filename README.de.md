# OpenClaw Cron Guardrails

> ⚠️ Frühe öffentliche Version. Feedback willkommen.
>
> **Sprachen:** [English](README.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [日本語](README.ja.md) | [Español](README.es.md) | [Français](README.fr.md) | **Deutsch**

> Eine sichere und nachvollziehbare Schicht für geplante Aufgaben in Agentensystemen.

`openclaw-cron-guardrails` hilft Nutzer:innen und Agentenprodukten dabei, geplante Aufgaben mit sichereren Defaults zu erstellen für:
- Session Targeting
- Delivery Routing
- Timeout-Auswahl
- explizite sichtbare Ausgabesteuerung

---

## ✨ Was ist das?

Dieses Projekt ist für **OpenClaw-ähnliche Systeme** gedacht, die bereits ein eigenes Base Model, einen Planner oder eine Prompt-Understanding-Schicht haben.

Es versucht **nicht**, ein universeller Natural-Language-Cron-Interpreter zu sein.

Stattdessen konzentriert es sich auf:
- easy-start patterns für häufige Scheduling-Anfragen
- deterministische validation/render-Flows
- sichereren Umgang mit visible delivery und multi-channel routing
- klare guardrails rund um session target, delivery target, timezone und timeout

> Man kann es als **guardrails + validation + render layer** für geplante Agentenaktionen verstehen.

---

## 🚀 Installation

### Option A — Installation über ClawHub

```bash
clawhub install openclaw-cron-guardrails
```

### Option B — Von GitHub klonen

```bash
git clone https://github.com/jackal092927/openclaw-cron-guardrails.git
```

Wenn du Skills manuell verwaltest, kopiere den Skill-Ordner anschließend in dein lokales Skills-Verzeichnis.

### Option C — Das gepackte Artefakt verwenden

Dieses Repository enthält auch ein gepacktes Skill-Artefakt:

```text
openclaw-cron-guardrails.skill
```

---

## ⚡ Schnellstart

Nutze es, wenn der Nutzer eine zukünftige oder wiederholte Agentenaktion anfordert, zum Beispiel:

- `Erinnere mich in 20 Minuten daran zu antworten`
- `Veröffentliche die tägliche Zusammenfassung um 9 Uhr in diesem Discord-Thread`
- `Führe nachts einen Scan aus und halte ihn intern`
- `Schiebe diesen aktuellen Thread alle 10 Minuten weiter`
- `Warum wurde dieser Cron an den falschen Ort geschickt?`

---

## 🧩 Integrationsmodi

Dieser Skill unterstützt **drei Integrationsstile**:

### 1) Natural-language quick start
Geeignet, wenn:
- ein Mensch die Anfrage direkt stellt
- du einen freundlicheren Standardpfad willst

### 2) Normalized-intent handoff
Geeignet, wenn:
- ein Upstream-Modell die Intention bereits interpretiert hat
- du vor dem Rendern des Crons eine stabile Zwischenschicht möchtest

### 3) Spec-first deterministic path
Geeignet, wenn:
- Vorhersagbarkeit wichtiger ist als Natural-Language-Komfort
- das System bereits genau weiß, welcher Job erstellt werden soll

Siehe:
- `openclaw-cron-guardrails/references/integration-modes.md`

---

## 🛠️ Häufige Beispiele

### Reminder

```text
Erinnere mich in 20 Minuten daran zu antworten
```

Erwartetes sicheres Muster:
- `main + systemEvent`

### Visible scheduled delivery

```text
Veröffentliche die Nachtzusammenfassung jeden Tag um 9 Uhr in diesem Discord-Kanal
```

Erwartetes sicheres Muster:
- `isolated + explicit delivery.channel + explicit delivery.to`

### Internal worker

```text
Führe nachts einen Scan aus, aktualisiere den lokalen Status und poste nichts
```

Erwartetes sicheres Muster:
- `isolated + delivery.mode:none`

### Current-thread push loop

```text
Schiebe diesen aktuellen Thread alle 10 Minuten weiter
```

Erwartetes sicheres Muster:
- session/thread-aware scheduled agent action
- explizite Beibehaltung des current-thread

---

## 📦 Was ist enthalten?

Dieses Repository enthält:

- `SKILL.md` — Top-Level-Anweisungen des Skills
- `references/` — patterns, pitfalls, diagnostics, examples und Integrationshinweise
- `scripts/` — Helfer für parse, validate, render, create, lint, doctor und fix
- `openclaw-cron-guardrails.skill` — gepacktes Artefakt für die Distribution

---

## 🔍 Welche Probleme soll dieser Skill erkennen?

Dieser Skill basiert auf real beobachteten Cron-Fehlern in lokalen Setups, darunter:

- delivery-route failures in multi-channel Umgebungen
- fragiles implizites Routing wie `channel=last`
- expliziter Channel ohne explizites Target
- impliziter oder zu kurzer Timeout bei nicht-trivialen Jobs

> Das sind keine theoretischen Edge Cases; sie gehören zu den Gründen, warum dieser Skill existiert.

---

## ✅ Worin dieser Skill gut ist

- sicherere Erstellung von scheduled tasks
- besser erklärbare routing-/delivery-defaults
- strukturierte validation vor der Erstellung
- Erkennen riskanter visible-delivery-Konfigurationen
- Unterstützung sowohl human-friendly als auch system-friendly Integrationspfade

---

## 🚫 Nicht-Ziele

Dieses Projekt versucht **nicht**, Folgendes zu sein:

- eine universelle prompt-understanding-Schicht
- ein vollständiger Ersatz für die OpenClaw-Cron-Dokumentation
- eine Garantie gegen alle provider/runtime-Fehler
- ein System, das automatisch alle historischen Cron-Jobs repariert

Wenn ein Upstream-System bereits ein stärkeres Modell hat, ist das völlig in Ordnung: Dieser Skill ist dafür gedacht, **darunter** zu liegen, nicht es zu ersetzen.

---

## 💬 Feedback willkommen

Besonders hilfreiches Feedback:

- welche scheduled-task patterns sich noch unbequem anfühlen
- welche routing-/delivery-Fälle sicherer sein sollten
- welche Teile der Doku/Beispiele noch unklar sind
- wo der Skill früher um Klarstellung bitten sollte

---

## 🔗 Links

- GitHub: <https://github.com/jackal092927/openclaw-cron-guardrails>
- ClawHub: <https://clawhub.ai/skills/openclaw-cron-guardrails>

---

## 🧪 Status

Aktueller Veröffentlichungsstatus: **early public release / feedback-seeking**.
