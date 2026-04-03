# Cataract — Architecture

> **Start here.** This document describes the overall system, key abstractions, and where to find things.

## Overview

Cataract is a 3D cooperative multiplayer space survival game set in the Oort Cloud. Players pilot probes through procedurally generated debris fields, destroying threats and surviving together as long as possible.

See [Game Design Document](docs/GAME_DESIGN.md) for full gameplay details.

## Tech Stack

| Layer | Technology | Notes |
|-------|-----------|-------|
| **Language** | Vanilla JavaScript (ES6+) | No TypeScript, no transpiler |
| **3D Rendering** | [Three.js](https://threejs.org/) via CDN | Only external dependency |
| **Networking** | WebRTC DataChannels | Peer-to-peer, no server required |
| **Audio** | Web Audio API | Procedural sound, no audio files |
| **Build** | None | Single `index.html`, open in browser |
| **Hosting** | GitHub Pages | Static file, zero infrastructure |

**Reference implementation:** [SpaceBashers](https://github.com/0xdiNGo/spacebashers) — same single-file architecture, WebRTC P2P, procedural audio, authoritative host model.

See ADRs: [0001-tech-stack](docs/decisions/0001-tech-stack-browser-webrtc.md), [0002-multiplayer-model](docs/decisions/0002-cooperative-multiplayer-model.md)

## Project Structure

```
cataract/
├── index.html               # The entire game — open in browser to play
├── ARCHITECTURE.md           # This file — start here
├── SKILL.md                  # Development guidelines and coding standards
├── CHANGELOG.md              # Notable changes log
├── docs/
│   ├── GAME_DESIGN.md        # Game design document
│   ├── diagrams/             # Mermaid diagrams
│   │   ├── architecture.md   # System component overview
│   │   ├── game-loop.md      # Main loop and state transitions
│   │   ├── module-deps.md    # Module dependency graph
│   │   └── networking.md     # WebRTC P2P multiplayer flow
│   └── decisions/            # Architecture Decision Records (ADRs)
└── LICENSE
```

## Key Systems

| System | Responsibility |
|--------|---------------|
| **Game Loop** | `requestAnimationFrame` tick/update/render cycle |
| **Navigation** | Player movement, thrust, gravity wells, physics integration |
| **Debris Field** | Procedural generation, density scaling by depth |
| **Combat** | Pulse cannon firing, hit detection, rock fragmentation |
| **Damage** | Shield degradation, hull integrity, collision response |
| **Progression** | Depth tracking, scoring, difficulty curve |
| **Rendering** | Three.js 3D scene, particles, camera modes (chase/cockpit) |
| **Input** | Keyboard/mouse handling, control mapping |
| **Networking** | WebRTC DataChannels, host authority, state broadcast |
| **Audio** | Web Audio API procedural sound effects |

## Multiplayer Architecture

Cataract uses an **authoritative host model** over WebRTC:

- **Host** creates the game and generates invite codes (base64-encoded SDP offers)
- **Clients** paste invite codes to establish peer-to-peer DataChannel connections
- **Host** runs the full simulation (physics, spawning, collision, damage)
- **Clients** send input only (thrust, rotation, fire, boost) at ~20 Hz
- **Host** broadcasts state snapshots (positions, debris, scores, shields) at ~20 Hz
- **Clients** interpolate between snapshots for smooth rendering

Co-op design: shared debris field, shared depth, team scoring, respawn on death.

## Code Organization

Since the game lives in a single `index.html`, code is organized into clearly commented sections:

```
<!-- HTML structure: canvas + UI overlays -->
<style> /* CSS: HUD, menus, overlays */ </style>
<script src="three.js CDN"></script>
<script>
// ============================================================
// CONSTANTS & CONFIG
// ============================================================

// ============================================================
// GAME STATE
// ============================================================

// ============================================================
// THREE.JS SCENE SETUP
// ============================================================

// ============================================================
// INPUT HANDLING
// ============================================================

// ============================================================
// AUDIO ENGINE
// ============================================================

// ============================================================
// NETWORKING (WebRTC)
// ============================================================

// ============================================================
// DEBRIS FIELD GENERATOR
// ============================================================

// ============================================================
// PHYSICS & NAVIGATION
// ============================================================

// ============================================================
// COMBAT SYSTEM
// ============================================================

// ============================================================
// DAMAGE SYSTEM
// ============================================================

// ============================================================
// PROGRESSION & SCORING
// ============================================================

// ============================================================
// RENDERING & PARTICLES
// ============================================================

// ============================================================
// HUD
// ============================================================

// ============================================================
// GAME LOOP
// ============================================================
</script>
```

## Naming Conventions

- **Functions:** `camelCase` — `updatePhysics()`, `spawnDebris()`, `createHostOffer()`
- **Constants:** `UPPER_SNAKE_CASE` — `TICK_RATE`, `MAX_PLAYERS`, `SHIELD_REGEN_RATE`
- **Variables:** `camelCase` — `playerShield`, `debrisField`, `netHostPcs`
- **Network prefixes:** `net` prefix for all networking state — `netHostPcs`, `netRemoteInputs`, `netLatestSnap`
- **DOM IDs:** `kebab-case` — `#game-canvas`, `#invite-code`, `#hud-shields`

## Getting Started

```bash
# No build step required. Just serve the file:
# Option 1: Python
python3 -m http.server 8000

# Option 2: Node
npx serve .

# Then open http://localhost:8000 in your browser.
```

For multiplayer:
1. Host presses the **Host Game** button — an invite code appears
2. Host shares the code with friends
3. Friends press **Join Game**, paste the code
4. Host pastes back the response code
5. Game begins when host starts
