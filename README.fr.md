# OpenClaw Cron Guardrails

> ⚠️ Version publique précoce. Les retours sont les bienvenus.
>
> **Langues :** [English](README.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [日本語](README.ja.md) | [Español](README.es.md) | **Français** | [Deutsch](README.de.md)

> Une couche sûre et explicable pour les tâches planifiées dans les systèmes d’agents.

`openclaw-cron-guardrails` aide les utilisateurs et les produits d’agents à créer des tâches planifiées avec des choix plus sûrs pour :
- le ciblage de session
- le routage de livraison
- le choix du timeout
- la gestion explicite des sorties visibles

---

## ✨ Qu’est-ce que c’est ?

Ce projet est conçu pour des systèmes de type **OpenClaw** qui disposent déjà de leur propre base model, planner ou couche de compréhension des prompts.

Il ne cherche **pas** à devenir un interpréteur universel de cron en langage naturel.

Il se concentre plutôt sur :
- des easy-start patterns pour les demandes planifiées les plus courantes
- des flux déterministes de validation / render
- une gestion plus sûre des visible delivery et du multi-channel routing
- des guardrails explicites autour du session target, du delivery target, du timezone et du timeout

> On peut le voir comme une **couche guardrails + validation + render** pour les actions planifiées des agents.

---

## 🚀 Installation

### Option A — Installer depuis ClawHub

```bash
clawhub install openclaw-cron-guardrails
```

### Option B — Cloner depuis GitHub

```bash
git clone https://github.com/jackal092927/openclaw-cron-guardrails.git
```

Si vous gérez vos skills manuellement, copiez ensuite le dossier du skill dans votre répertoire local de skills.

### Option C — Utiliser l’artefact empaqueté

Ce dépôt inclut aussi un artefact empaqueté :

```text
openclaw-cron-guardrails.skill
```

---

## ⚡ Démarrage rapide

Utilisez-le quand l’utilisateur demande une action future ou répétée d’un agent, par exemple :

- `Rappelle-moi dans 20 minutes de répondre`
- `Publie le résumé quotidien dans ce thread Discord à 9h`
- `Lance un scan nocturne et garde-le en interne`
- `Fais avancer ce thread actuel toutes les 10 minutes`
- `Pourquoi ce cron est-il parti au mauvais endroit ?`

---

## 🧩 Modes d’intégration

Ce skill prend en charge **trois styles d’intégration** :

### 1) Natural-language quick start
Idéal quand :
- une personne fait la demande directement
- vous voulez une voie par défaut plus conviviale

### 2) Normalized-intent handoff
Idéal quand :
- un modèle amont a déjà interprété l’intention
- vous voulez une couche intermédiaire stable avant de rendre le cron

### 3) Spec-first deterministic path
Idéal quand :
- la prédictibilité compte plus que la commodité du langage naturel
- le système sait déjà exactement quel job doit être créé

Voir :
- `openclaw-cron-guardrails/references/integration-modes.md`

---

## 🛠️ Exemples courants

### Reminder

```text
Rappelle-moi dans 20 minutes de répondre
```

Schéma sûr attendu :
- `main + systemEvent`

### Visible scheduled delivery

```text
Publie le résumé de la nuit dans ce canal Discord à 9h chaque jour
```

Schéma sûr attendu :
- `isolated + explicit delivery.channel + explicit delivery.to`

### Internal worker

```text
Lance un scan nocturne, mets à jour l’état local et ne publie rien
```

Schéma sûr attendu :
- `isolated + delivery.mode:none`

### Current-thread push loop

```text
Fais avancer ce thread actuel toutes les 10 minutes
```

Schéma sûr attendu :
- session/thread-aware scheduled agent action
- préservation explicite du current-thread

---

## 📦 Contenu du dépôt

Ce dépôt contient :

- `SKILL.md` — instructions de haut niveau du skill
- `references/` — patterns, pitfalls, diagnostics, examples et guidance d’intégration
- `scripts/` — helpers pour parse, validate, render, create, lint, doctor et fix
- `openclaw-cron-guardrails.skill` — artefact empaqueté pour la distribution

---

## 🔍 Quels problèmes ce skill cherche à éviter

Ce skill a été façonné à partir de vrais échecs observés sur des cron locaux, notamment :

- des delivery-route failures dans des environnements multi-channel
- des routages implicites fragiles comme `channel=last`
- un channel explicite sans target explicite
- un timeout implicite ou trop court pour des jobs non triviaux

> Ce ne sont pas des edge cases théoriques ; ils font partie de la raison d’être de ce skill.

---

## ✅ Ce que ce skill fait bien

- création plus sûre de scheduled tasks
- defaults plus explicables pour routing et delivery
- validation structurée avant création
- détection des configurations risquées de visible delivery
- prise en charge d’intégrations à la fois human-friendly et system-friendly

---

## 🚫 Non-objectifs

Ce projet **n’essaie pas** d’être :

- une couche universelle de prompt understanding
- un remplacement complet de la documentation cron d’OpenClaw
- une garantie contre toutes les erreurs provider/runtime
- un système de réparation automatique pour tous les anciens cron jobs

Si le système amont dispose déjà d’un modèle plus fort, c’est très bien : ce skill est conçu pour se placer **en dessous**, pas pour le remplacer.

---

## 💬 Retours bienvenus

Retours particulièrement utiles :

- quels scheduled-task patterns restent maladroits
- quels cas de routing/delivery devraient être plus sûrs
- quelles parties des docs/exemples restent floues
- où le skill devrait demander une clarification plus tôt

---

## 🔗 Liens

- GitHub: <https://github.com/jackal092927/openclaw-cron-guardrails>
- ClawHub: <https://clawhub.ai/skills/openclaw-cron-guardrails>

---

## 🧪 Statut

Statut actuel : **early public release / feedback-seeking**.
