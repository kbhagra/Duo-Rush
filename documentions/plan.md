# FOLD RUSH — Hackathon Execution Plan

## Mission

Build one polished 20–30 second iPhone Duo experience in four hours:

> **An endless runner where the runner moves automatically and the player physically reshapes the track with the Duo hinge.**

The primary success condition is not “we built a game.”

It is:

> **A judge sees the physical fold change the level and immediately wants to try it.**

---

## 0. Before the Official Coding Window

Allowed preparation only:

- keep these planning docs ready;
- prepare original visual assets if permitted by event rules;
- prepare original sound assets if permitted;
- confirm Xcode 27.1 beta and Duo simulator are installed;
- confirm Bitrig is signed in and ready;
- decide team roles;
- rehearse the pitch conceptually.

Do **not** create implementation code before the official start.

---

# PHASE 1 — PROVE THE DUO MECHANIC

## 11:30–11:40 — Project creation

### Goal

Get a clean native project running immediately.

### Actions

- Create **FOLD RUSH** project in Bitrig.
- Open/run in iPhone Duo simulator.
- Confirm target uses iOS 27.1 SDK.
- Establish simple SwiftUI root.
- Add SpriteKit host/scene shell.

### Exit condition

Blank game scene launches reliably in Duo simulator.

Do not add menus yet.

---

## 11:40–12:00 — Hinge proof

### Goal

Prove live hinge data works before building gameplay.

### Actions

- Wire `onHingeChange` at SwiftUI root.
- Read/display/log current angle and hinge status.
- Move the 3D Duo simulator through several folds.
- Verify updates are continuous enough for gameplay.
- Add a tiny hinge state store.
- Query division reserved region if straightforward.

### Exit condition

Moving the simulated hinge changes a visible debug value in real time.

If not working by 12:00:

- stop all game work;
- isolate a minimal hinge test;
- verify exact local SDK API signatures.

---

## 12:00–12:20 — Minimal runner

### Goal

Get motion on screen with placeholder art.

### Actions

- Add simple runner node.
- Add perspective rail/track placeholder.
- Runner advances automatically.
- Add fixed camera/world movement illusion.
- No coins, no trains, no art polish.

### Exit condition

Runner appears to move forward continuously.

---

## 12:20–12:30 — Broken-track prototype

### Goal

Prove the entire thesis in ugly form.

### Actions

- Place left/right track endpoints around crease.
- Feed hinge angle into `FoldGeometryModel`.
- Derive connection progress.
- Visually move/animate the endpoint relationship.
- When geometry is valid, connect track.
- Allow runner to cross.

### HARD CHECKPOINT — 12:30

The team must be able to demonstrate:

> **fold Duo -> track relationship changes -> track connects -> runner crosses**

If this works, continue with FOLD RUSH.

If it does not work, simplify aggressively for 15–20 minutes.

Do not continue building generic game content while this is broken.

---

# PHASE 2 — MAKE THE CORE FEEL GREAT

## 12:30–1:00 — Track connection polish

### Goal

Turn the proof into the first judge-worthy moment.

### Add

- magnetic anticipation;
- rail glow;
- snap effect;
- sparks;
- metallic sound;
- tiny camera impulse;
- automatic runner continuation;
- connection hysteresis so it does not flicker.

### Exit condition

The broken-track sequence is satisfying enough to demo alone.

---

## 1:00–1:20 — Original visual pass

### Person A

Continue improving geometry reliability.

### Person B

Replace placeholders with original:

- runner;
- rail texture;
- train/environment art;
- coin/icon;
- simple background layers.

### Rule

Do not copy Subway Surfers assets or branding.

Aim for nostalgic endless-runner energy, not imitation.

---

## 1:20–1:40 — Ramp sequence

### Goal

Create the second distinct use of fold geometry.

### Actions

- Add train obstacle.
- Derive effective ramp pitch from hinge geometry.
- Show ramp visually changing as the Duo folds.
- Trigger a deterministic jump whose magnitude follows ramp pitch.
- Add landing.

### Exit condition

Judge can visually understand:

> **I created the ramp by changing the phone's shape.**

Do not implement complex physics if scripted motion looks better.

---

# PHASE 3 — BUILD THE SPECTACLE

## 1:40–2:00 — Hinge grind

### Goal

Create the signature crease moment.

### Actions

- Anchor special rail to division region / crease.
- Transition runner onto it.
- Add sparks.
- Add grind audio loop.
- React effect intensity to fold geometry if cheap.
- Exit back onto normal track.

### Exit condition

The hinge itself is visibly part of the level.

If this causes instability, remove it and invest in gap/ramp polish instead.

---

## 2:00–2:20 — Coins, score, fail/retry

### Add only essentials

- coin line / collection;
- score or distance;
- simple crash/fall state;
- retry button;
- reliable restart of scripted sequence.

### Exit condition

A judge can play/retry without developer intervention.

---

## 2:20–2:40 — Start screen + tiny tutorial

### Start screen

- FOLD RUSH logo
- original hero art
- **PLAY**
- tagline: **Bend the world. Keep running.**

### Tutorial

One line:

> **BEND THE PHONE TO BEND THE TRACK**

Auto-dismiss after first successful hinge interaction.

Do not build settings, characters, shops, or missions.

---

# PHASE 4 — POLISH TO WIN

## 2:40–3:00 — Audio / particles / juice

Prioritize in this order:

1. rail snap sound;
2. sparks;
3. jump whoosh;
4. landing impact;
5. grind sound;
6. coin sound;
7. subtle music only if already available and original/licensed.

Add haptics only if simulator/device workflow supports them without risk.

---

## 3:00–3:10 — Visual cleanup

Check:

- no debug labels;
- no clipping around hinge;
- HUD does not cover crease;
- consistent fonts;
- original branding;
- readable score;
- clean start/retry loop;
- no placeholder rectangles unless intentional.

---

## 3:10–3:20 — Reliability pass

Run the full sequence repeatedly.

Test:

- app cold launch;
- Play;
- first gap;
- fold connection;
- ramp;
- grind;
- finish/fail;
- Retry;
- repeat.

Fix only high-impact failures.

Do not refactor.

---

## 3:20–3:30 — Freeze code and rehearse

### No new mechanics

Only fix catastrophic bugs.

### Rehearse exact 20–30 second pitch

Suggested script:

> “We grew up playing endless runners by swiping characters around a flat world.”

Start run.

> “But iPhone Duo's world doesn't have to stay flat.”

Fold -> rails connect.

> “So instead of moving the player, we physically reshape the level.”

Ramp / jump / grind.

> **“The screen isn't showing the track. The screen is the track.”**

Title.

Optional final sentence:

> “We built the entire experience in Bitrig using Duo's live hinge APIs.”

Stop talking.

Let judges react.

---

# TWO-PERSON TASK SPLIT

## Person A — Duo / gameplay owner

Own:

- project skeleton;
- `onHingeChange`;
- hinge state store;
- division region;
- fold geometry;
- track connection;
- runner/game state;
- ramp behavior;
- integration.

## Person B — experience / polish owner

Own:

- original art/assets;
- start/game-over UI;
- particles;
- audio;
- coins;
- HUD;
- environment layers;
- grind visuals;
- demo/pitch polish.

### Shared

- test every milestone together;
- integration every ~20–30 minutes;
- both understand how to recover the demo if one person is talking to judges.

---

# FALLBACK LADDER

If time collapses, cut in this exact order.

### Full target

1. Broken track
2. Ramp/train
3. Hinge grind
4. Coins/score
5. Start/game over

### If behind

Drop hinge grind first.

Keep:

- broken track;
- ramp;
- retry.

### If very behind

Drop ramp.

Build one immaculate interaction:

> runner approaches impossible gap -> judge folds Duo -> rails snap together -> runner crosses -> title.

A 10-second magical demo is better than a 30-second broken one.

---

# DECISION RULES DURING THE HACK

When deciding between two tasks, choose the one that improves one of these:

1. Duo necessity
2. immediate comprehension
3. physical satisfaction
4. reliability
5. visual polish

Do not optimize for feature count.

---

# DEMO QUALITY CHECKLIST

Before submission, answer YES to as many as possible:

- Does the fold visibly change the level?
- Is the runner mostly automatic rather than controlled by hinge-as-buttons?
- Is the crease part of the gameplay?
- Does the first “wow” happen within 10 seconds?
- Is the project obviously an endless runner without explanation?
- Are all assets original?
- Does retry work?
- Can the sequence run multiple times?
- Is the project built primarily in Bitrig?
- Can we explain the Duo API use in one sentence?
- Can we demo without internet?
- Can we recover quickly from a failed run?

---

# FINAL PRIORITY

If the game looks simple but the hinge interaction feels incredible, that is success.

If the game looks feature-rich but the hinge feels like a gimmick, that is failure.

**Protect the interaction.**
