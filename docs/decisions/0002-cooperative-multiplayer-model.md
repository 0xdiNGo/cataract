# 0002 — Cooperative multiplayer model

## Status
Accepted

## Context
Cataract supports cooperative multiplayer — multiple players share the same debris field and work together to survive. We need to decide how to synchronize game state across players.

SpaceBashers uses an authoritative host model over WebRTC DataChannels:
- Host creates RTC offers, encodes as base64 invite codes
- Clients paste invite codes to establish P2P connections
- Host runs the full game simulation
- Clients send input at ~20 Hz (left/right/fire flags)
- Host broadcasts state snapshots at ~20 Hz
- Clients render the received state directly

This model is simple, proven, and cheat-resistant (host is authoritative).

## Decision
Use the same authoritative host model adapted for Cataract's 3D co-op:

- **Host** runs the full simulation: physics, debris spawning, collision detection, damage
- **Clients** send input only: thrust vector, rotation, fire, boost
- **Host** broadcasts state snapshots: all player positions, debris positions, projectiles, scores, shield/hull states
- **DataChannels** configured as `{ordered: false, maxRetransmits: 0}` for low-latency unreliable delivery (latest state always wins)
- **Snapshot rate**: ~20 Hz (same as SpaceBashers)
- **Client-side interpolation**: Smooth rendering between snapshots using lerp on positions/rotations

### Co-op Specific Design
- All players share one debris field and one depth meter
- Players can protect each other by destroying debris in each other's paths
- Scoring is shared (team score) with individual contribution tracking
- If any player's hull is destroyed, they respawn with a brief invulnerability period (co-op is about teamwork, not permadeath)

## Consequences

**Benefits:**
- Single source of truth prevents desync
- Simple client implementation (input + render only)
- Proven pattern from SpaceBashers

**Trade-offs:**
- Host has zero latency advantage (acceptable in co-op since players aren't competing)
- If host disconnects, game ends (no host migration — keep it simple for v1)
- Input latency for clients (~50ms snapshot interval + network RTT)
