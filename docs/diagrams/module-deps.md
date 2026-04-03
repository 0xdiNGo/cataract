# Module Dependencies

How the major modules depend on each other. Arrows point from dependent to dependency.

```mermaid
graph BT
    Input[Input]
    Physics[Physics / Navigation]
    Combat[Combat]
    Debris[Debris Generator]
    Damage[Damage]
    Progression[Progression]
    Renderer[Three.js Renderer]
    HUD[HUD]
    State[Game State]
    Audio[Web Audio]
    Net[WebRTC Networking]

    Physics --> State
    Physics --> Input
    Physics --> Net
    Combat --> State
    Combat --> Input
    Combat --> Physics
    Combat --> Net
    Debris --> State
    Debris --> Physics
    Damage --> State
    Damage --> Physics
    Damage --> Combat
    Progression --> State
    Progression --> Damage
    Renderer --> State
    Renderer --> Physics
    Renderer --> Debris
    Renderer --> Combat
    HUD --> State
    HUD --> Progression
    HUD --> Damage
    Audio --> State
    Audio --> Combat
    Audio --> Damage
    Net --> State
    Net --> Input
```

## Dependency Notes

- **Game State** is the root dependency — nearly everything reads from it.
- **Input** has no dependencies; it only produces events.
- **WebRTC Networking** reads from State (to broadcast snapshots) and writes to Input (remote player inputs).
- **Physics** and **Combat** consume remote input via the Networking module.
- **Three.js Renderer**, **HUD**, and **Web Audio** are leaf consumers — nothing depends on them.
