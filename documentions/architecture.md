# FOLD RUSH — Architecture

## 0. Architecture Goal

The architecture must optimize for:

1. **Duo hinge responsiveness**
2. **fast iteration in Bitrig**
3. **minimal moving parts**
4. **safe four-hour scope**
5. **a polished 20–30 second demo**

This is not a production game architecture. It is a hackathon architecture built to make one interaction feel exceptional.

---

## 1. Technology Choices

### Primary stack

- **Swift**
- **SwiftUI** for app shell, start/game-over UI, and Duo API integration
- **SpriteKit** for the runner scene, timing, animation, particles, collisions, and lightweight 2.5D arcade presentation
- **Bitrig** as the primary build/iteration environment
- **Xcode 27.1 beta** as the authoritative compiler/debugger/runtime when deeper inspection is required
- **iPhone Duo simulator** for all judging-critical interaction testing

### Why SpriteKit

SpriteKit gives the team a small, native, deterministic game loop without requiring a full 3D engine or third-party dependency.

For this hackathon, the goal is not photorealistic 3D. The physical 3D Duo simulator already provides the foldable-device spectacle. The in-app scene only needs to sell:

- forward motion;
- track perspective;
- runner movement;
- obstacle timing;
- connection / ramp / grind effects.

Do not introduce Unity, Unreal, web views, React Native, or a heavy external engine unless the current native approach becomes impossible.

---

## 2. System Diagram

```text
                     +----------------------+
                     |   iPhone Duo Hinge   |
                     +----------+-----------+
                                |
                                v
                     +----------------------+
                     | SwiftUI Root View    |
                     | onHingeChange        |
                     | division-region read |
                     +----------+-----------+
                                |
                                v
                     +----------------------+
                     |   HingeStateStore    |
                     | angle / status       |
                     | light smoothing      |
                     +----------+-----------+
                                |
                                v
                     +----------------------+
                     | FoldGeometryModel    |
                     | panel geometry       |
                     | connection distance  |
                     | ramp pitch           |
                     +----------+-----------+
                                |
                                v
                     +----------------------+
                     |   FoldRushScene      |
                     | SpriteKit game loop  |
                     +----+--------+--------+
                          |        |
             +------------+        +-------------+
             v                                   v
     +---------------+                    +---------------+
     | Track System  |                    | Runner System |
     | gap / ramp /  |                    | auto-run      |
     | hinge grind   |                    | jump / grind  |
     +-------+-------+                    +-------+-------+
             |                                    |
             +----------------+-------------------+
                              v
                     +----------------------+
                     |  FX / Audio / Haptics|
                     +----------------------+
```

---

## 3. Core Data Flow

### 3.1 Hinge input

`onHingeChange` is attached at the SwiftUI root that hosts the game.

It receives the latest `DeviceHingeContext` and extracts:

- current hinge angle;
- hinge status;
- availability.

The app must treat the local Xcode 27.1 SDK as the source of truth for exact beta API signatures and enum cases.

Do not invent status names from memory.

### 3.2 Division region

The app queries the active **division reserved region** to determine the real crease location in view coordinates.

Use that position to:

- anchor the broken-track gap;
- place hinge-specific sparks/effects;
- align the hinge-grind rail;
- prevent hard-coding “screen center” unless absolutely necessary.

If division-region integration becomes a blocker, temporarily approximate the crease with the display midpoint for the first proof, then return to the real API once the core hinge interaction works.

### 3.3 Geometry model

Raw angle values should not directly fire arbitrary game actions.

Convert hinge input into a small `FoldGeometryModel` representing physical relationships such as:

- normalized fold amount;
- estimated panel orientation;
- track-endpoint separation;
- connection confidence;
- effective ramp pitch;
- whether the geometry is entering/leaving an alignment window.

The game consumes **geometry**, not raw angle thresholds whenever possible.

---

## 4. Geometry Strategy

Do not over-engineer exact physical-device mathematics.

The purpose of the geometry model is to make the interaction coherent, smooth, and visually believable in the simulator.

### Broken-track connection

Model one endpoint on each side of the crease.

As the hinge changes:

- calculate/approximate how close the two modeled endpoints are;
- expose a `connectionProgress` from 0...1;
- show magnetic anticipation near 1;
- mark track as connected only when within tolerance.

This keeps the game rule tied to a geometric relationship instead of an arbitrary `if angle < X` command.

### Ramp

Map the relative panel orientation to an effective ramp pitch.

The launch impulse should be derived from that pitch.

The game can clamp values to keep the sequence reliable and fun.

### Hinge grind

Use the division-region centerline as the visual rail path.

The grind is a scripted set piece, but its effect intensity can react continuously to fold geometry.

---

## 5. Game State Machine

Keep the game deterministic and scriptable.

Recommended states:

```text
start
  -> runningIntro
  -> gapApproach
  -> waitingForConnection
  -> crossingConnectedTrack
  -> rampApproach
  -> waitingForRamp
  -> airborne
  -> grindApproach
  -> hingeGrinding
  -> finishBurst
  -> gameOver / replay
```

This is intentionally not a full procedural endless-runner system.

The judging loop should be scripted enough that the same exciting moments happen reliably every time.

---

## 6. Scene Composition

### `FoldRushScene`

Owns:

- game state machine;
- runner position/timing;
- track segment nodes;
- train/barrier nodes;
- coins;
- particles;
- camera impulses;
- score/distance;
- success/fail conditions.

### Runner

Use one original runner sprite/animation set.

Required states only:

- run;
- jump;
- grind;
- stumble/fail optional.

No character customization architecture.

### Track

Track can be represented as reusable visual segments with a strong perspective illusion.

Required segment types:

- normal rail;
- gap left;
- gap right;
- connection bridge;
- ramp;
- hinge/grind rail.

### Obstacles

One train is enough.

One generic low barrier may be added only if implementation is ahead of schedule.

---

## 7. Rendering Approach

### Principle

Let the **3D Duo simulator provide the physical fold**.

Do not waste time building a fake 3D folding phone inside the app.

The SpriteKit scene should span the Duo display and visually anchor important track pieces around the actual crease.

### 2.5D look

Use:

- perspective-scaled track textures;
- larger foreground / smaller horizon objects;
- vertical motion / scale changes for forward travel;
- parallax background layers;
- speed lines;
- particles;
- subtle camera scale/position impulses.

This can evoke a familiar 3D endless-runner experience without building a real 3D engine.

---

## 8. Suggested Project Structure

Keep folders shallow.

```text
FoldRush/
  App/
    FoldRushApp.swift
    RootView.swift

  Duo/
    HingeStateStore.swift
    FoldGeometryModel.swift
    DivisionRegionReader.swift

  Game/
    FoldRushScene.swift
    GamePhase.swift
    RunnerNode.swift
    TrackController.swift
    ObstacleController.swift
    ScoreController.swift

  UI/
    StartView.swift
    GameHUDView.swift
    GameOverView.swift
    TutorialOverlay.swift

  Effects/
    EffectController.swift
    AudioController.swift
    HapticController.swift

  Assets.xcassets/
    Runner/
    Track/
    Environment/
    UI/
    FX/
```

Do not add repositories, service layers, networking modules, databases, or elaborate protocols.

---

## 9. Responsibility Boundaries

### SwiftUI layer

Responsible for:

- app lifecycle;
- start/game-over overlays;
- Duo hinge listener;
- division-region discovery;
- displaying SpriteKit scene;
- simple state transition between menu/game/retry.

### Hinge / geometry layer

Responsible for:

- latest hinge angle/status;
- smoothing;
- derived geometry values;
- no rendering;
- no game-specific asset code.

### SpriteKit layer

Responsible for:

- game timing;
- runner;
- world;
- obstacle sequencing;
- visual response to geometry;
- particles;
- score.

### Effect layer

Responsible for:

- snap sound;
- jump sound;
- grind loop;
- coin sound;
- sparks;
- haptics if available.

---

## 10. Hinge Input Quality

### Smoothing

Raw hinge input may jitter.

Apply only light smoothing so movement remains immediate.

Prefer:

- small moving average / exponential smoothing;
- hysteresis around connect/disconnect tolerance;
- rate limiting only if simulator events become noisy.

Avoid heavy animation lag.

### Connection hysteresis

Prevent rapid connect/disconnect flicker near the geometric threshold.

Use separate conceptual thresholds for:

- “snap into connected state”;
- “release from connected state.”

Exact values should be tuned empirically in the simulator.

---

## 11. Performance Rules

Target smooth simulator performance rather than maximal rendering complexity.

Rules:

- preload textures/audio;
- reuse nodes and particle emitters;
- avoid creating large objects every frame;
- keep physics bodies minimal;
- use scripted collision/state transitions where that is more reliable than full physics;
- avoid network calls during gameplay;
- keep all judging-critical assets local.

The demo should work if Wi-Fi disappears.

---

## 12. Bitrig Workflow

Bitrig should be the project's primary creation environment because the team also wants to compete for the Bitrig-specific prize.

Workflow:

1. Create the native project in Bitrig after the official hack begins.
2. Use Bitrig to iterate against its Duo simulator workflow.
3. Keep the project native and Xcode-compatible.
4. Use Xcode directly only when needed for compiler diagnostics, Instruments, asset inspection, or beta-API debugging.
5. Return to Bitrig for the main implementation flow when practical.
6. Keep evidence of Bitrig usage naturally through project/chat/history/screenshots if needed for submission.

Do not distort the project simply to use more Bitrig features. The game experience remains the priority.

---

## 13. Build Order

Architecture must be implemented vertically, not layer-by-layer.

Bad:

- build all menus;
- build full runner;
- build score system;
- integrate hinge last.

Correct:

1. blank app + hinge angle visible internally;
2. simple runner + simple track;
3. hinge geometry moves/connects track;
4. runner successfully crosses;
5. only then add art/audio/polish;
6. add ramp;
7. add grind;
8. add menu/game over last.

The first hour must prove the central interaction.

---

## 14. Failure Modes and Fallbacks

### `onHingeChange` integration blocks

- verify local SDK signatures;
- isolate a minimal SwiftUI view showing angle/status;
- do not debug inside the full game scene;
- only reconnect to SpriteKit after minimal proof works.

### Division region blocks

- use midpoint approximation temporarily;
- finish central hinge interaction;
- return to reserved-region integration later.

### SpriteKit visual complexity blocks

- simplify to stylized 2D rail world;
- prioritize satisfying connection and particles over realistic perspective.

### Ramp physics blocks

- use a deterministic scripted jump whose magnitude is derived from ramp pitch;
- do not waste time on real rigid-body simulation.

### Hinge grind blocks

- remove it before destabilizing the broken-track demo.

The project is successful with one extraordinary Duo interaction.

---

## 15. Architecture Principle

> **Hinge data becomes geometry. Geometry becomes game state. Game state becomes spectacle.**

Do not let the architecture drift into “hinge angle triggers command.”
