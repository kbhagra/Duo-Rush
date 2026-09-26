# FOLD RUSH — Product Requirements Document

## 0. Status

**Working title:** FOLD RUSH  
**Tagline:** **Bend the world. Keep running.**  
**Event:** Bitrig Hacks — iPhone Duo Edition  
**Primary objective:** Build the most memorable, creative, Duo-native experience possible in the 4-hour hack window.  
**Secondary objective:** Be a credible contender for the **Best Project Created in Bitrig** prize by building the project in Bitrig and using its Duo workflow as the primary development environment.

---

## 1. Product Thesis

FOLD RUSH is a nostalgic endless-runner experience designed around one idea:

> **The screen is not displaying the level. The physical screen is the level.**

Classic endless runners ask the player to move a character through a fixed flat world. FOLD RUSH reverses that relationship. The runner moves automatically; the player physically reshapes the world with the iPhone Duo hinge.

The hinge is **not a left/right button** and is **not a generic slider**. The live fold geometry changes whether track segments connect, how steep a ramp is, and how the runner traverses the world.

The experience should feel immediately familiar to anyone who grew up with mobile endless runners, then surprise them with an interaction that only makes sense on a folding device.

---

## 2. Why This Exists

The hackathon judging signal emphasizes:

- creative, thoughtful use of iPhone Duo's new APIs;
- something meaningfully new;
- experiences substantially less compelling or impossible before Duo;
- a polished demo is enough; a full production app is not required.

FOLD RUSH is designed to optimize for those criteria rather than feature count.

The intended judge reaction is:

> “I know this kind of game — wait, the PHONE itself is changing the level.”

Then:

> “Give me that. I want to try it.”

---

## 3. Core Interaction

### 3.1 What the player controls

The runner moves automatically.

The player controls the **physical geometry of the track** by opening and closing the Duo.

The game continuously reads hinge state using Apple's `onHingeChange` API. The hinge angle is converted into a model of the two display planes. Gameplay consequences derive from that geometry.

### 3.2 What the hinge does

The hinge has three core gameplay consequences:

1. **Track connection**  
   Two disconnected track endpoints become connected when their modeled physical relationship reaches the required tolerance.

2. **Ramp pitch**  
   The fold angle determines the incline of a track section. The runner's jump/launch behavior derives from that incline rather than from a discrete “jump” command.

3. **Hinge traversal / grind**  
   A special track segment runs directly through the real Duo division/crease. The runner grinds or transitions through the hinge with sparks, sound, and a high-impact visual payoff.

The central design rule is:

> **The angle changes geometry. Geometry changes gameplay.**

Never implement the central mechanic as `angle threshold -> arbitrary command` when a geometry-derived relationship can be used instead.

---

## 4. The 30-Second Judge Demo

This is the product. Everything else is secondary.

### Beat 1 — Familiarity (0–5 sec)

- Bright, original endless-runner environment.
- Runner moves automatically.
- Rails/tracks, coins, trains, barriers.
- Player immediately understands the genre.
- Minimal HUD: score / distance only.

### Beat 2 — Broken Track (5–11 sec)

A visible gap appears directly around the Duo crease.

While flat, the route is impossible.

The player folds the Duo.

As the live hinge geometry changes, the two rails visually and logically approach a valid connection.

Near alignment:

- subtle magnetic pull;
- rail glow;
- increasing metallic tension sound.

At alignment:

- **CLACK**;
- sparks;
- tiny camera shake;
- track locks;
- runner crosses immediately.

This must be the first unmistakable “Duo moment.”

### Beat 3 — Train / Ramp (11–19 sec)

A train blocks the route.

The player changes the fold geometry so the track becomes an incline.

The runner uses the physically derived ramp pitch to launch over the train.

Payoff:

- whoosh;
- coin arc;
- landing impact;
- short slow-motion or speed burst if cheap to implement.

### Beat 4 — Hinge Grind (19–27 sec)

The track converges onto the physical crease.

The runner locks onto a rail aligned with the hinge.

- sparks;
- metallic grind sound;
- bright trail;
- score multiplier or coin burst.

The player changes the fold while the runner is on the crease, making the physical device and game world visibly respond together.

### Beat 5 — Title Payoff (27–30 sec)

Runner exits the hinge section.

Show:

**FOLD RUSH**  
**Bend the world. Keep running.**

Optional tiny line:

**Built in Bitrig for iPhone Duo.**

---

## 5. What Makes It Meaningfully Duo-Native

### 5.1 Not “Subway Surfers with a hinge control”

Reject any design where:

- opening = right;
- closing = left;
- quick fold = jump;
- hinge angle simply maps to lane index.

That is too close to “hinge as controller/button” and public demos like hinge-controlled Flappy Bird.

### 5.2 The world obeys the device shape

The core difference is:

- the runner is not commanded by the hinge;
- the **level geometry** is derived from the hinge;
- track topology can change because the display itself can stop being flat;
- the real division/crease is visually central to the experience.

### 5.3 Remove Duo and the mechanic collapses

A flat-screen port could imitate the idea with an artificial slider, but the direct physical mapping disappears:

> bend hardware -> bend world

That physical congruence is the interaction we are demonstrating.

---

## 6. Apple / Duo API Use

### Required

#### `View.onHingeChange(isEnabled:_:)`

Use live `DeviceHinge` angle/status changes as the input for the game's fold geometry model.

Official reference:  
https://developer.apple.com/documentation/swiftui/view/onhingechange(isenabled:_:)

#### Division / reserved region around the hinge

Read the active division region so gameplay can anchor special content around the actual crease rather than a guessed center line.

Relevant Apple reference:  
https://developer.apple.com/videos/play/tech-talks/111463/

### Optional / only if useful

#### `ArrangementView`

Can be used for start/game-over/supporting UI if it materially helps the app adapt to Duo posture. It is **not** the core invention and must not consume implementation time.

Official reference:  
https://developer.apple.com/documentation/swiftui/arrangementview

### Explicitly not required

#### `CameraCaptureAccessory`

Do not base FOLD RUSH on camera capture. The game should be fully demonstrable in the Duo simulator.

Reference:  
https://developer.apple.com/documentation/swiftui/cameracaptureaccessory

### Toolchain

- Xcode 27.1 beta
- iOS 27.1 iPhone Duo simulator
- Bitrig as primary build/iteration environment

Xcode release notes:  
https://developer.apple.com/documentation/xcode-release-notes/xcode-27_1-release-notes

---

## 7. Visual Direction

### Desired feeling

- nostalgic mobile endless runner;
- saturated, energetic, arcade-like;
- immediately readable in under one second;
- polished enough to invite touching/playing;
- cleaner than a production free-to-play game's menu system.

### Allowed inspiration

Use the visual language of the endless-runner genre:

- rails;
- trains;
- coins;
- barriers;
- third-person runner;
- speed lines;
- bold score presentation;
- bright city / rail-yard environment.

### IP rule

Do **not** use or reproduce:

- Subway Surfers name or logo;
- Jake, Tricky, Fresh, or recognizable characters;
- copyrighted art, trains, backgrounds, music, sounds, icons, typography, or exact UI;
- copied source code or assets.

Create an original runner, original environment, original audio, and original brand.

The goal is **nostalgic familiarity without copying protected expression**.

---

## 8. MVP Scope

### Must ship

1. Original start screen with one obvious **PLAY** action.
2. One runner moving automatically.
3. One continuous track/world spanning the inner Duo display.
4. Live hinge angle read successfully.
5. Actual Duo division/crease location available or safely approximated only if API access blocks progress.
6. Broken-track sequence controlled by hinge-derived geometry.
7. Train/ramp sequence controlled by hinge-derived geometry.
8. Hinge-grind sequence.
9. Coins or equivalent collectible feedback.
10. Score/distance.
11. Fail/retry loop.
12. Sound + particles for the core three moments.
13. Reliable 20–30 second demo.

### Should ship if time allows

- simple intro tutorial: **“BEND THE PHONE TO BEND THE TRACK.”**
- a magnetic pre-snap effect before track connection;
- speed increase after successful maneuvers;
- basic haptic feedback where available;
- simple high score;
- one additional obstacle that reuses existing geometry logic.

### Do not build

- shop;
- character roster;
- cosmetic system;
- world map;
- missions;
- accounts;
- backend;
- multiplayer;
- AI;
- networking;
- procedural city generation;
- production monetization;
- leaderboard service;
- multiple full environments;
- elaborate tutorial flow.

---

## 9. UX Screens

Keep the app to four screens/states.

### 1. Start

- FOLD RUSH logo
- runner hero art
- **PLAY**
- tiny subtitle: **Bend the world. Keep running.**

### 2. Gameplay

- full-screen experience
- score/distance only
- no clutter
- crease visually integrated into world

### 3. Game Over

- score
- distance
- **RETRY**

### 4. Optional Tutorial Overlay

One line only:

> **BEND THE PHONE TO BEND THE TRACK**

Dismiss automatically after first successful fold interaction.

---

## 10. Game Feel Requirements

The project wins or loses on feel.

### Track connection must include

- magnetic anticipation before connection;
- bright contact effect;
- metallic snap sound;
- sparks;
- brief camera impulse;
- immediate runner continuation.

### Ramp launch must include

- obvious incline before launch;
- launch sound;
- arc that visually makes sense;
- satisfying landing;
- coins or particles in the air.

### Hinge grind must include

- runner/board/rail visually locked to the crease;
- sparks;
- continuous sound whose intensity can react to fold geometry;
- high score/coin payoff.

### Responsiveness

The visual response to hinge movement must feel immediate. Avoid sluggish smoothing. Apply only enough filtering to remove jitter.

---

## 11. Demo Pitch

Target: 20–30 seconds before questions.

Suggested pitch:

> “We grew up playing endless runners by swiping characters around a flat world.”

Runner begins.

> “But iPhone Duo's world doesn't have to stay flat.”

Fold -> rails connect.

> “So instead of moving the player, we physically reshape the level.”

Ramp -> train jump -> hinge grind.

> **“The screen isn't showing the track. The screen is the track.”**

End on title.

Do not lead with implementation details. Show the magic first.

---

## 12. Success Criteria

A successful submission satisfies all of these:

- A judge understands the mechanic in <10 seconds.
- The first Duo-specific payoff occurs within ~10 seconds.
- The demo does not require a verbal explanation of hinge thresholds.
- The game remains playable/reliable for repeated demos.
- The central mechanic cannot be summarized as “fold to press a button.”
- The crease is visibly part of the level.
- The 30-second loop looks polished even if the rest of the app is tiny.
- No copyrighted Subway Surfers assets/code are used.
- The project is created during the official hack window.
- Bitrig is the primary build workflow so the same project can credibly compete for the Bitrig-specific prize.

---

## 13. Kill / Pivot Criteria

The team has only four hours.

If, within roughly the first 45–60 minutes of implementation, the team cannot demonstrate:

> live hinge movement -> visible/logical change in track geometry -> runner can cross a connection created by folding

then simplify immediately.

Do not spend three hours building menus or generic runner mechanics while the Duo interaction remains unproven.

Fallback simplification order:

1. Keep only broken-track connection.
2. Remove train/ramp if necessary.
3. Remove hinge grind if necessary.
4. Keep one polished 10-second interaction rather than three broken interactions.

The Duo moment is the product.

---

## 14. Final Product Principle

Every implementation decision should answer:

> **Does this make folding the iPhone feel like part of the game world?**

If no, it is probably not worth building during the hackathon.
