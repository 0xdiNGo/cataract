# System Architecture

High-level overview of Cataract's core systems and how they communicate.

```mermaid
graph TD
    subgraph "Host (authoritative)"
        Input[Input System] --> Navigation[Navigation / Physics]
        Input --> Combat[Combat System]
        RemoteInput[Remote Player Input<br/>via WebRTC] --> Navigation
        RemoteInput --> Combat

        Navigation --> GameState[Game State]
        Combat --> GameState

        GameState --> Debris[Debris Field Generator]
        GameState --> Damage[Damage System]
        GameState --> Progression[Progression / Scoring]
    end

    subgraph "Rendering (all peers)"
        Debris --> Rendering[Three.js Renderer]
        Damage --> Rendering
        Navigation --> Rendering
        Combat --> Rendering
        Progression --> HUD[HUD Overlay]
        Damage --> HUD
        Rendering --> Display[Canvas Output]
        HUD --> Display
    end

    subgraph "Networking"
        GameState -->|"Snapshot @ 20 Hz"| Broadcast[WebRTC Broadcast]
        Broadcast -->|"State JSON"| Clients[Remote Clients]
        Clients -->|"Input JSON"| RemoteInput
    end

    subgraph "Audio"
        Combat --> Audio[Web Audio API]
        Damage --> Audio
        Progression --> Audio
    end
```

## Component Descriptions

| Component | Role |
|-----------|------|
| **Input System** | Captures keyboard/mouse events, maps to game actions |
| **Remote Player Input** | Receives input from connected peers via WebRTC DataChannels |
| **Navigation / Physics** | Applies thrust, gravity, collision detection with debris |
| **Combat System** | Weapon firing, projectile tracking, hit resolution |
| **Game State** | Central state: all player positions, velocities, shields, score, depth |
| **Debris Field Generator** | Procedurally spawns and despawns debris based on depth |
| **Damage System** | Processes collisions, applies shield/hull damage |
| **Progression / Scoring** | Tracks depth, calculates score, adjusts difficulty |
| **Three.js Renderer** | 3D scene, particles, lighting, camera modes |
| **HUD Overlay** | Shields, hull, depth, score, boost fuel, player indicators |
| **WebRTC Broadcast** | Serializes and sends state snapshots to all connected peers |
| **Web Audio API** | Procedural sound effects (no audio files) |

See also: [Networking Diagram](networking.md) for WebRTC connection and protocol details.
