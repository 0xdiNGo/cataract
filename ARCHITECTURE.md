# Cataract — Architecture

> **Start here.** This document describes the overall system, key abstractions, and where to find things.

## Overview

Cataract is a 3D space survival game set in the Oort Cloud. The player pilots a probe through procedurally generated debris fields, destroying threats and surviving as long as possible.

See [Game Design Document](docs/GAME_DESIGN.md) for full gameplay details.

## Tech Stack

> **TODO:** Tech stack has not been chosen yet. This section will be updated once a rendering engine, language, and framework are selected. See `docs/decisions/` for ADRs once decisions are made.

## Project Structure

```
cataract/
├── ARCHITECTURE.md          # This file — start here
├── SKILL.md                 # Development guidelines and coding standards
├── CHANGELOG.md             # Notable changes log
├── docs/
│   ├── GAME_DESIGN.md       # Game design document
│   ├── diagrams/            # Mermaid diagrams
│   │   ├── architecture.md  # System component overview
│   │   ├── game-loop.md     # Main loop and state transitions
│   │   └── module-deps.md   # Module dependency graph
│   └── decisions/           # Architecture Decision Records (ADRs)
└── src/                     # Source code (structure TBD with tech stack)
```

## Key Systems

The game is built around these core systems:

| System | Responsibility |
|--------|---------------|
| **Game Loop** | Tick/update/render cycle |
| **Navigation** | Player movement, physics, gravity wells |
| **Debris Field** | Procedural generation, density scaling by depth |
| **Combat** | Weapon firing, hit detection, rock fragmentation |
| **Damage** | Shield degradation, hull integrity, collision response |
| **Progression** | Depth tracking, scoring, difficulty curve |
| **Rendering** | 3D scene, particles, camera modes |
| **Input** | Keyboard/mouse handling, control mapping |

## Naming Conventions

> **TODO:** Document naming conventions once the language/framework is chosen.

## Getting Started

> **TODO:** Build and run instructions will be added once the tech stack is selected.
