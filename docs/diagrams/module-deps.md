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
    Renderer[Renderer]
    HUD[HUD]
    State[Game State]
    Audio[Audio]

    Physics --> State
    Physics --> Input
    Combat --> State
    Combat --> Input
    Combat --> Physics
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
```

## Dependency Notes

- **Game State** is the root dependency — nearly everything reads from it.
- **Input** has no dependencies; it only produces events.
- **Renderer** and **HUD** are leaf consumers — nothing depends on them.
- **Audio** is a leaf consumer driven by combat and damage events.
