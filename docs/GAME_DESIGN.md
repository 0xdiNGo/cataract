# Cataract — Game Design Document

## Game Concept

You pilot a deep-space probe through the **Oort Cloud**—humanity's first expedition into the primordial debris field at the solar system's edge. The cloud is a cascading torrent of ancient icy bodies, cometary fragments, and stellar remnants left over from the solar system's formation billions of years ago.

Your mission: navigate deeper into the cloud, destroy threatening debris before it damages your ship, and survive as long as possible while collecting data and resources.

Play solo or bring friends — Cataract supports **cooperative multiplayer** where up to 4 players share the same debris field and work together to survive.

---

## Core Gameplay

### Navigation

- Free 3D flight through dense fields of slow-moving icy boulders and comet chunks
- Gravity wells and orbital mechanics subtly affect your trajectory—you're not fighting physics, you're dancing with it
- Dense regions get progressively more dangerous the deeper you venture

### Combat

- Lock onto and shoot incoming rocks with your pulse cannon
- Different debris sizes require different tactics (small chunks explode in one hit; massive icebergs need sustained fire)
- Destroyed rocks create smaller fragments—risk/reward on what you destroy

### Damage System

- Your shields degrade with each collision
- Shield regeneration is slow—you need to actively *avoid* things, not just tank damage
- Hull integrity is visible; one critical hit and you're done

### Progression

- Depth meter shows how far into the cloud you've penetrated
- Scoring based on debris destroyed, distance traveled, and shields remaining
- Procedurally generated debris fields that get denser and more chaotic deeper in

---

## Visual Style

- **Color palette:** Deep blues, purples, blacks with icy whites and pale blues for the comet chunks
- **Camera perspective:** Third-person chase cam or cockpit view (toggleable)
- **Particles:** Solar wind, dust trails, glowing debris impacts
- **Scale:** Make the rocks feel massive and ancient—you're tiny against primordial forces

---

## Controls

| Input | Action |
|-------|--------|
| WASD / Arrow Keys | Translational movement (strafe/forward/backward) |
| Mouse | Aim and rotate view |
| Spacebar | Fire weapon |
| Shift | Boost/evasive maneuver (limited fuel) |
| R | Reset/respawn |

---

## Difficulty Curve

| Phase | Description |
|-------|-------------|
| **Early** | Sparse, slow-moving debris, generous shields |
| **Mid** | Denser fields, faster rocks, shield regen slows |
| **Late** | Chaotic maelstrom, massive boulders, one mistake kills you |
| **Endless** | No end state—pure survival arcade mode |

---

## Cooperative Multiplayer

- **Up to 4 players** in the same debris field via peer-to-peer WebRTC (no server needed)
- **Host/join model:** Host generates an invite code, friends paste it to connect
- **Shared depth meter** — the team advances together
- **Team scoring** with individual contribution tracking
- **Protect each other** by destroying debris in teammates' paths
- **Respawn on death** — co-op is about teamwork, not permadeath. Brief invulnerability on respawn.
- **If the host disconnects, the session ends** (no host migration in v1)

---

## Technical Platform

- Runs in any modern web browser — no install, no plugins
- Single `index.html` file, hostable on GitHub Pages
- Three.js for 3D rendering (loaded via CDN)
- Procedural audio via Web Audio API — no sound files to load

---

## Why "Cataract"?

A cataract is a waterfall—a cascading, unstoppable flow. The Oort Cloud isn't a gentle field; it's a torrent of ancient debris falling inward toward the sun, and you're flying straight into it.
