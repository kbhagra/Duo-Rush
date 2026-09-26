# FOLD RUSH — AI RULES

## Purpose

These are non-negotiable instructions for Codex / Bitrig / any coding agent working on FOLD RUSH during the hackathon.

The goal is not to maximize code output. The goal is to produce a **stable, polished, memorable iPhone Duo demo** within four hours.

---

## 1. Hackathon Rule Compliance

### Absolute rule

**All implementation code must be created during the official hackathon window.**

Before the hack begins, only planning, architecture, prompts, design concepts, and non-code assets may be prepared.

If the user has not explicitly confirmed that hacking has begun, do not generate implementation files.

Once the hack begins, implementation may proceed normally.

### Do not copy external project code

Do not copy source code from:

- Subway Surfers;
- DuoBird;
- other public game repositories;
- previous hackathon projects;
- sample projects beyond tiny API-usage references where licensing and event rules permit.

Write original implementation code during the event.

---

## 2. Product Lock

The product is **FOLD RUSH**.

Do not pivot the project into:

- FUSE;
- an AI agent;
- a finance product;
- a generic hinge-controlled runner;
- a two-pane productivity app;
- a camera experience;
- a multiplayer app;
- a normal Subway-Surfers clone.

The locked product thesis is:

> **The runner moves automatically. The player physically reshapes the level with the iPhone Duo hinge.**

The locked demo thesis is:

> **The screen isn't showing the track. The screen is the track.**

Only change this if the user explicitly authorizes a pivot.

---

## 3. Core Mechanic Rule

Never reduce the project to:

- open hinge = right;
- close hinge = left;
- fold = jump;
- angle threshold = button press.

The central implementation must follow:

> **hinge input -> derived geometry -> track relationship -> gameplay consequence**

Examples:

- fold geometry causes two track endpoints to become connectable;
- panel orientation determines effective ramp pitch;
- crease position defines hinge-grind location.

A threshold may be used internally for stability, but the user-facing logic must remain geometry-driven.

---

## 4. Technology Rules

### Required

- Swift
- SwiftUI
- native Apple frameworks
- iPhone Duo simulator
- Xcode 27.1 beta SDK

### Preferred

- SpriteKit for gameplay and effects
- Bitrig as primary creation/iteration environment

### Avoid

- Unity
- Unreal
- React Native
- Flutter
- web views as the game engine
- third-party game engines
- backend services
- network dependencies
- AI APIs
- databases
- package-manager dependencies unless absolutely necessary

The fewer dependencies, the safer the demo.

---

## 5. Apple API Rules

### `onHingeChange`

Use Apple's actual beta API from the local SDK.

Do not invent signatures, types, enum cases, or status names.

If compilation disagrees with memory/documentation, trust the installed Xcode 27.1 SDK.

### Division / reserved region

Use the actual division reserved region when feasible to anchor crease-specific visuals and logic.

If this blocks the MVP, temporarily use the screen midpoint, clearly mark it as a temporary fallback, and return to the real API after the core demo works.

### `ArrangementView`

Use only if it materially helps a supporting UI state.

Do not force it into the game merely to claim another API.

### `CameraCaptureAccessory`

Do not use it for FOLD RUSH.

The game must remain simulator-first and camera-independent.

---

## 6. Coding Style

### Keep files small

Prefer focused files with one responsibility.

Avoid giant 1,000-line views or scenes.

### Prefer explicit state

Use a small game phase/state machine rather than hidden timing hacks scattered across files.

### Prefer deterministic behavior

For the judging sequence, scripted reliability is better than physically accurate but unstable simulation.

### Prefer direct code

Do not create abstract protocols, factories, repositories, service layers, coordinators, or generic frameworks unless the code is genuinely reused.

This is a four-hour hack.

### Main-thread discipline

UI and SpriteKit scene mutations must stay safe for the relevant Apple framework execution model.

Do not introduce unnecessary concurrency.

---

## 7. Change Management Rules

### Never rewrite working systems casually

Once a milestone works:

1. preserve it;
2. commit/save checkpoint;
3. make the next change incrementally.

Do not replace a working scene with a “cleaner architecture” during the final two hours.

### Build after every meaningful change

After changing:

- hinge integration;
- game state;
- track geometry;
- asset loading;
- scene lifecycle;

build and run immediately.

Do not stack ten speculative edits before compiling.

### Fix root causes

If a bug appears, diagnose before rewriting.

Do not solve compiler errors by deleting core functionality.

---

## 8. Scope Rules

### Must prioritize

1. hinge input works;
2. track reacts;
3. broken track connects;
4. runner crosses;
5. interaction feels satisfying;
6. demo is reliable.

### Add only after core works

- ramp jump;
- hinge grind;
- coins;
- score;
- menu;
- game over;
- tutorial;
- additional polish.

### Never spend hack time on

- shops;
- skins;
- missions;
- world maps;
- accounts;
- cloud save;
- leaderboards;
- monetization;
- multiple environments;
- complex procedural generation;
- generic settings pages.

---

## 9. Visual / IP Rules

The project may evoke the nostalgic endless-runner genre but must use original expression.

Do not use:

- Subway Surfers logo/name;
- Jake or recognizable characters;
- Subway Surfers screenshots/assets;
- original game music/sounds;
- copied UI;
- ripped models/textures;
- copied promotional art.

Use original:

- character;
- title;
- environment;
- trains;
- coins;
- icons;
- UI;
- audio.

Working brand is **FOLD RUSH**.

---

## 10. Game Feel Rules

Every core action needs anticipation + payoff.

### Track connection

Before connection:

- glow increases;
- magnetic attraction becomes visible;
- sound tension rises.

At connection:

- metallic snap;
- sparks;
- tiny camera impulse;
- runner immediately commits to crossing.

### Ramp

The player must see the incline forming before launch.

Jump must visually follow the ramp rather than look like a hidden button-triggered animation.

### Hinge grind

Make the crease visually important.

Use:

- sparks;
- trailing light;
- grind audio;
- clear attachment to the crease.

Do not bury the hinge under HUD elements.

---

## 11. Performance Rules

Target smooth, reliable simulator performance.

- preload assets;
- reuse nodes;
- avoid excessive particle counts;
- avoid unnecessary physics bodies;
- keep background layers lightweight;
- avoid real-time network calls;
- avoid expensive per-frame allocations;
- prefer scripted transitions when they look equivalent.

A stable 30-second demo beats a technically ambitious stuttering game.

---

## 12. Debugging Rules

When Duo API integration fails:

1. create the smallest possible isolated reproduction;
2. display/log the raw hinge values;
3. verify simulator movement produces events;
4. only then reconnect the game.

When gameplay fails:

1. freeze art polish;
2. test with simple colored rectangles/nodes;
3. prove state transition;
4. restore visuals after behavior works.

Never debug visual polish and core logic simultaneously.

---

## 13. Bitrig Prize Rules

The team wants a credible shot at the Bitrig-specific prize.

Therefore:

- create the project in Bitrig;
- use Bitrig meaningfully for implementation/iteration;
- use the Duo simulation workflow there when practical;
- keep the project native and exportable/openable in Xcode;
- do not switch permanently to another environment unless Bitrig blocks critical progress.

If Xcode is needed for debugging, use it. Winning the overall hackathon is more important than performative tool usage.

---

## 14. Two-Person Collaboration Rules

Avoid both people editing the same file at the same time.

Recommended split:

### Person A — Core interaction

- hinge API;
- division region;
- geometry model;
- game state;
- broken-track mechanic.

### Person B — Presentation

- original assets;
- SpriteKit scene visuals;
- particles;
- sounds;
- menu/HUD;
- ramp/grind polish once core hooks are available.

Integrate frequently.

Do not allow one branch to drift for two hours before merging.

---

## 15. Agent Communication Style

When asked to implement a task:

1. state the smallest change needed;
2. identify files to edit;
3. implement only that scope;
4. build/test;
5. report exact result;
6. suggest the next highest-value step.

Do not dump five future features into one change.

If uncertain about a beta API, say so and inspect the local SDK/docs instead of hallucinating.

---

## 16. Time-Aware Behavior

### First hour

Do not polish menus.

Prove:

> live hinge -> track geometry -> connection -> runner crossing.

### Middle two hours

Add the two remaining spectacle moments and polish the core.

### Final hour

Stabilize.

Do not refactor architecture.

Do not add new major features.

Do not upgrade dependencies.

Do not touch working code without a clear reason.

### Final 20 minutes

Only:

- bug fixes;
- build verification;
- demo rehearsal;
- presentation text;
- submission packaging.

No new mechanics.

---

## 17. Definition of Done

The project is done when:

- the judge can understand the interaction immediately;
- the Duo hinge visibly changes the world;
- the broken-track moment works every time;
- at least one additional spectacle moment works reliably;
- the app launches cleanly;
- retry works;
- assets are original;
- the demo can be repeated multiple times without manual repair;
- the team can explain the API use in one sentence.

Do not confuse “more features” with “more done.”

---

## 18. Final Rule

Whenever there is a conflict between:

- adding another feature, and
- making the fold interaction feel better,

**improve the fold interaction.**
