# Game Loop

Main loop phases and state transitions.

```mermaid
stateDiagram-v2
    [*] --> MainMenu

    MainMenu --> Playing: Start Game
    MainMenu --> [*]: Quit

    Playing --> Paused: Pause
    Paused --> Playing: Resume
    Paused --> MainMenu: Quit to Menu

    Playing --> GameOver: Hull Destroyed
    GameOver --> Playing: Restart (R)
    GameOver --> MainMenu: Quit to Menu

    state Playing {
        [*] --> ProcessInput
        ProcessInput --> UpdatePhysics
        UpdatePhysics --> GenerateDebris
        GenerateDebris --> DetectCollisions
        DetectCollisions --> ResolveDamage
        ResolveDamage --> UpdateProgression
        UpdateProgression --> Render
        Render --> [*]
    }
```

## Loop Phases

| Phase | Description |
|-------|-------------|
| **ProcessInput** | Read keyboard/mouse, map to movement and fire commands |
| **UpdatePhysics** | Apply thrust, gravity, update positions and velocities |
| **GenerateDebris** | Spawn/despawn debris based on current depth and density curve |
| **DetectCollisions** | Check player-debris and projectile-debris intersections |
| **ResolveDamage** | Apply shield/hull damage from collisions, fragment destroyed rocks |
| **UpdateProgression** | Update depth, score, difficulty parameters |
| **Render** | Draw scene, particles, HUD |
