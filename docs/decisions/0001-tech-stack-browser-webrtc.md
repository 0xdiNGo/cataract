# 0001 — Browser-based with WebRTC cooperative multiplayer

## Status
Accepted

## Context
Cataract is a 3D space survival game that needs to:
- Run in a web browser with no install
- Support cooperative multiplayer over the network
- Be easy to develop and iterate on

We have a proven reference implementation in [SpaceBashers](https://github.com/0xdiNGo/spacebashers) that demonstrates a zero-dependency, single-file browser game with WebRTC P2P multiplayer. That project uses:
- Single `index.html` (no build tools, no npm, no bundler)
- HTML5 Canvas 2D rendering
- WebRTC DataChannels for P2P networking (no server required)
- Web Audio API for procedural sound
- Authoritative host model (host simulates, clients send input, receive state)

Cataract requires 3D rendering, which makes raw Canvas 2D insufficient. The options are:
1. Raw WebGL — maximum control, zero deps, but extremely verbose for a full 3D game
2. Three.js via CDN — mature, well-documented, single script tag import
3. Babylon.js via CDN — heavier, more engine-like, overkill for this scope

## Decision
Follow the SpaceBashers architecture pattern with one addition:

- **Single `index.html`** — all game logic, rendering, networking, and audio in one file
- **Three.js via CDN** — the only external dependency, loaded as a script tag
- **WebRTC DataChannels** — peer-to-peer cooperative multiplayer, no server
- **Web Audio API** — procedural sound generation, no audio files
- **Authoritative host model** — host runs the simulation, broadcasts state snapshots; clients send input only
- **No build tools** — no npm, no bundler, no transpiler. Open the file in a browser and play.

Three.js is chosen over raw WebGL because writing a full 3D scene (camera, lighting, particle systems, 3D models) in raw WebGL would dominate development time without adding value. Three.js via CDN keeps the single-file simplicity while making 3D practical.

## Consequences

**Benefits:**
- Zero setup for players — just open a URL (GitHub Pages compatible)
- Zero setup for developers — edit one file, refresh browser
- Proven networking model from SpaceBashers
- Three.js handles the heavy 3D lifting (scene graph, shaders, particles)

**Trade-offs:**
- Single file will get large as the game grows (manageable with good code organization via sections/comments)
- Three.js CDN is a runtime dependency (if CDN is down, game won't load — mitigated by pinning a version)
- WebRTC requires manual SDP exchange (copy-paste invite codes) unless we add a signaling server later
- No TypeScript, no modules — relies on discipline for code organization
