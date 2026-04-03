# Networking — WebRTC P2P Multiplayer

## Connection Flow

```mermaid
sequenceDiagram
    participant Host
    participant Client

    Host->>Host: Create RTCPeerConnection
    Host->>Host: Create DataChannel ("game", unreliable)
    Host->>Host: Create & set local SDP offer
    Host->>Host: Encode offer as base64 invite code

    Host-->>Client: Share invite code (copy/paste)

    Client->>Client: Create RTCPeerConnection
    Client->>Client: Set remote description (decoded invite)
    Client->>Client: Create & set local SDP answer
    Client->>Client: Encode answer as base64 response code

    Client-->>Host: Share response code (copy/paste)

    Host->>Host: Set remote description (decoded response)

    Note over Host,Client: ICE negotiation completes, DataChannel opens

    Host->>Client: {t: "welcome", playerId: 2}
    Client->>Host: {t: "ready"}
```

## Gameplay Data Flow

```mermaid
graph LR
    subgraph Host
        HS[Full Simulation] --> SB[State Broadcast<br/>~20 Hz]
        IR[Input Receiver] --> HS
        HI[Host Input] --> HS
    end

    subgraph Client 1
        C1I[Local Input] --> C1S[Send Input<br/>~20 Hz]
        C1R[Receive State] --> C1L[Interpolate & Render]
    end

    subgraph Client 2
        C2I[Local Input] --> C2S[Send Input<br/>~20 Hz]
        C2R[Receive State] --> C2L[Interpolate & Render]
    end

    C1S -->|"{t:input, thrust, rot, fire}"| IR
    C2S -->|"{t:input, thrust, rot, fire}"| IR
    SB -->|"State snapshot JSON"| C1R
    SB -->|"State snapshot JSON"| C2R
```

## Network Protocol

### Client → Host Messages

| Type | Fields | Rate |
|------|--------|------|
| `input` | `thrust` (vec3), `rotation` (vec2), `fire` (bool), `boost` (bool) | ~20 Hz |
| `ready` | — | Once |
| `quit` | — | Once |

### Host → Client Messages

| Type | Fields | Rate |
|------|--------|------|
| `welcome` | `playerId`, `playerCount` | Once |
| `state` | `players[]`, `debris[]`, `projectiles[]`, `explosions[]`, `depth`, `score` | ~20 Hz |
| `playerJoined` | `playerId`, `name` | On event |
| `playerLeft` | `playerId` | On event |
| `gameOver` | `finalScore`, `depth`, `stats` | Once |

## DataChannel Configuration

```javascript
const dc = pc.createDataChannel("game", {
    ordered: false,       // No ordering guarantees (latest state wins)
    maxRetransmits: 0     // No retransmission (low latency over reliability)
});
```
