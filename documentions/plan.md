# STREET RUSH DUO — 2-Hour Hackathon Execution Plan

**Platform:** iPhone Duo simulator using Bitrig, SwiftUI + SpriteKit  
**Build window:** 120 minutes  
**Goal:** A reliable, attractive 60-second playable demo showing a mechanic that benefits specifically from the unfolded Duo.  
**Visual target:** Nostalgic, colorful, chunky early mobile-runner aesthetic. Original art, characters, sounds and branding only.

## Product pitch

**Street Rush Duo** is a bright, arcade-style endless runner where the unfolded iPhone becomes a window into **two simultaneous routes**. One runner auto-moves down an active three-lane street. The other half previews a synchronized alternate route, including obstacles and bonus coins. Press **ROUTE SWAP** to jump between them while maintaining momentum. The fold is not merely extra screen space: it makes the other route continuously visible and turns route planning into the central skill.

**One-sentence demo line:** “A runner where the other road is always in sight—and folding the phone changes the way you play.”

## Locked scope: P0 must ship

1. Native iOS application launching in the iPhone Duo simulator.
2. Attractive main menu: illustrated street backdrop, cheerful original skater, big **PLAY** button.
3. Playable auto-scrolling runner: **three lanes, swipe left/right, jump, slide**, obstacle collisions, coins, increasing score, retry.
4. Unfolded layout: **two side-by-side route views** with the **same simulation clock and shared score**. One route active, one route preview. Large **SWAP ROUTE** button; switching changes the active route without resetting progress.
5. The active route has a clear outline/marker; the preview route has reduced HUD clutter and still visibly scrolls.
6. At least one scripted demo moment: barrier on active route, clear/coin-rich alternative route, swap, collect coins, celebrate.
7. Folded/compact state: one active full-size route, alternate route accessible via a route toggle or compact preview.
8. Functional pause, game over and retry; simulator tested.

**P1 only if P0 is done:** polished coin particles, one extra backdrop, bounce animations, a simple hinge-triggered pause, best score via UserDefaults, sound with mute.  
**Do not build:** character shop, multiple levels, login, leaderboards, AI, multiplayer, complex 3D models, procedural world generator, real purchases, elaborate mission system. Make every displayed button work or omit it.

## Paste this exact prompt into Bitrig

> Build **Street Rush Duo**, a native iPhone Duo game in SwiftUI and SpriteKit. It's a colorful nostalgic endless runner inspired by the friendly, chunky, graffiti-covered visual language of classic mobile games, but with fully original character/art/branding. Use sunny blue skies, warm orange city buildings, teal railings, yellow collectible coins, bold white outlined text and large yellow/blue rounded buttons. No cyberpunk, neon-glass UI or asset copying.
>
> On the unfolded inner display, create two side-by-side gameplay panels separated visually at the fold. **Left = Main Street**, **Right = Bonus Alley**. They are synchronized three-lane forward-scrolling perspectives driven by **one shared game state**. The runner is visible and controllable in only one active route at a time; the other side shows an animated preview of upcoming obstacles and coins. Put a prominent **SWAP ROUTE** button near the bottom of the inactive route or in an easily accessible safe zone away from the hinge. A route swap seamlessly transfers the runner to the corresponding lane of the other route while preserving distance, coin total, jump state where feasible and score. Briefly animate the transition. Players must be able to look ahead at the second screen to plan the swap.
>
 Gameplay: start from a working PLAY button, auto-forward movement, 3 lanes, swipe left/right to change lanes, swipe up to jump, swipe down to slide, stationary-pattern obstacles moving toward the runner, coin pickup, collision detection, pause, game-over summary with score/coins/distance and RETRY. Include a scripted opening 20 seconds so the demo shows a roadblock on one screen while the alternate screen offers a coin trail. Continue into a looping obstacle sequence. Prioritize reliable responsiveness over complex art. Use SwiftUI for menus/HUD and SpriteKit for the running views; prefer simple original programmatically generated sprites and perspective scaling over an unfinishable 3D scene.
>
 For the folded state, show one full-size playable route and an obvious route-switch control. Detect Duo geometry/hinge using APIs confirmed available in the project; if hinge APIs are unavailable, respond to window size/orientation without inventing API names. Layout must not place crucial information under the hinge. Maintain game state while unfolding/refolding, if the simulator supports the transition. Start by generating the minimal compiling project and running it in the iPhone Duo simulator. Build in discrete milestones, compile after each one and fix build/runtime issues before adding the next feature. Do not implement nonessential menus until the core runner and route swap work. A stable polished 60-second demo is the goal.

## UI/UX spec

| Screen | Left half | Right half | Important details |
|---|---|---|---|
| Start, unfolded | Full-height smiling skater illustration, animated city, oversized yellow PLAY | Small logo, simple blue HOW TO PLAY and optional SOUND buttons, sample alternate road | Keep Play immediately discoverable. |
| Run, unfolded | Main Street runner, score/coins, moving lane obstacles | Bonus Alley preview, extra coins, bold SWAP ROUTE control | Active-route pill/colored border; both roads scroll together. After swap, the right becomes active and left becomes preview. |
| Run, folded | Single active road | — | Compact top score and bottom route toggle; maintain playability. |
| Pause | Frozen game | Frozen game + resume/restart overlay | Preserve current run. |
| Game over | Freeze the last game scene, cheerful character reaction | Score, coins, distance, prominent RETRY button | RETRY resets both routes and shared data. |

**Palette:** Sky `#61C9F8`; sunny yellow `#FFC940`; coral `#FF784B`; grass/green `#70CB53`; royal blue `#2577CB`; warm cream `#FFF3D6`; ink `#20324A`.  
**Art direction:** Original cartoon skater with cap and backpack; painted sidewalks, rounded trams or street barriers, comic-style bursts, stickers, spray-paint shapes and clouds. Aim for bright depth and legible silhouettes—not a visual replica of Subway Surfers.  
**Typography:** Bold rounded system font plus dark stroke/shadow on big score and headers. Touch targets at least ~44 pt.  
**Interaction:** Native gestures in the *active* route only. Dedicated route swap button kept well away from the hinge. Distinct 150–250 ms lane/swap animations. Clear collision feedback, coin pop and subtle screen shake. Respect reduced motion if inexpensive.

## Technical architecture (small and testable)

- `StreetRushDuoApp.swift`: app entry.
- `GameContainerView.swift`: adaptive unfolded/folded SwiftUI layout and menu/pause/results presentation.
- `GameStore.swift`: single `ObservableObject` / `@Observable` state for phase, active route, player lane, jump/slide cooldown, score, coins, distance, health (one-hit fail for MVP), pause.
- `RunnerScene.swift`: SpriteKit pseudo-3D renderer, with reusable route configuration (`mainStreet` vs `bonusAlley`). Two scene views should read the **same game clock**, or one shared scheduler updates both route models. Avoid running two independent timers.
- `TrackModel.swift`: seeded or scripted obstacle/coin sequence by route and elapsed time; simple collision rules.
- `Assets.swift`: color palette, gradients, basic sprites/icons (generate inside code if assets unavailable).

**Rendering shortcut:** Draw road as trapezoid with converging lane markers; render obstacles/coins based on normalized depth (0 far away, 1 near runner), increasing scale as they approach. Move obstacle positions toward the bottom at a fixed speed. Use only three X lane coordinates. This is sufficient to evoke a 3D runner without modeling a full 3D world.

**Shared timing:** `GameStore` owns a single monotonic elapsed time / fixed-frame update. Main and bonus route entities are determined from the same `distance`, so route previews always stay synchronized. Route switching toggles only `activeRoute`, preserves `distance`, and ensures collisions are evaluated on the new active route. Add a short swap invulnerability window (~0.35–0.5 s) to prevent unfair instant collisions.

**Controls:** L/R swipe = lane -1/+1, upward = jump for ~0.7 s, downward = slide ~0.7 s. Route swap is a button; do not overload swipe semantics. Add a brief tutorial overlay. Mouse testing: tap onscreen arrow/jump/slide controls as a fallback if simulator gestures are inconvenient; these can be hidden for final capture.

**Device API rule:** Check the actual Bitrig project and installed SDK before referencing `onHingeChange` or any other hinge/arrangement API. The core game must work by reading available scene/window geometry even if a specialized API differs or is unavailable.

## Detailed minute-by-minute schedule

| Time | Workstream | Owner suggestion | Exit condition |
|---|---|---|---|
| 0–10 min | Start Bitrig app, select iPhone Duo simulator, paste prompt; create skeleton | Builder | Clean build + main menu renders. |
| 10–30 min | One playable main route: background, moving track, 3 lanes, swipe controls | Builder | Player can run and dodge in simulator. |
| 30–45 min | Coins, two obstacle types, collision/game over/retry, score | Builder | Complete 30-second single-route loop. |
| 45–65 min | Unfolded dual-route geometry, shared simulation state, distinct street/alley views | Builder | Both screens scroll in sync; one active. |
| 65–80 min | Working SWAP ROUTE mechanic, preview/active styling, scripted showcase challenge | Builder | Swap solves a visible obstacle; no score reset. |
| 80–95 min | Polished nostalgic UI, menu/results, clouds/stickers/button bounce | Designer + builder | Consistent original visual style. |
| 95–108 min | Fold/unfold and orientation testing; fix clipping/hinge overlap | Tester + builder | Core loop still playable in both configurations. |
| 108–120 min | Rehearse demo, screen-record backup, fix only blockers | Presenter + team | Smooth 45–60-second live walkthrough. |

**If only one teammate is coding:** Designer prepares exact colors/layout and screenshots while builder works in Bitrig; presenter handles the script and simulator QA. **Do not merge competing source edits during the final 15 minutes.**

## Milestone-specific Bitrig follow-up prompts

**After first generation (core game):**
> Stop adding cosmetic screens. Make PLAY launch a genuinely playable endless-runner loop. Verify lane changes, jump, slide, one obstacle, one coin, score and working RETRY directly in the simulator. Fix compilation/runtime errors before proceeding.

**Once the runner works (Duo feature):**
> Add synchronized Main Street and Bonus Alley to the unfolded Duo layout. Both panels must run from the same game clock. Only one route has a controllable runner; the other is an upcoming-obstacle preview. Implement SWAP ROUTE that preserves lane, distance and score, moves the runner between routes and visually marks which route is active. Add the scripted showcase: obstacle wall approaching on Main Street, clear coin trail on Bonus Alley. Ensure no important controls overlap the hinge.

**Once route swap works (polish):**
> Make the game feel like an original cheerful, nostalgic arcade runner: sunny sky, bright city storefronts, cartoon skater with backpack, chunky blue and yellow buttons, white outlined score, warm graffiti accents, satisfying coin pop and subtle bounce. Keep all buttons functional. Improve visuals without changing reliable gameplay logic.

**Final QA:**
> Use Bitrig's iPhone Duo simulator to test unfolded, folded and partially folded states. Verify PLAY, swipe controls, route swap, coins, collision, RETRY, pause and resuming a run. Fix all crashes, clipped HUD, hinge overlap and state resets. Do not add new features.

## Acceptance checklist

- [ ] App builds and opens in the Duo simulator without crash.
- [ ] Main menu's PLAY button launches a run.
- [ ] Character auto-runs; at least left/right swipes work; jump/slide if time allows.
- [ ] Coins update a visible counter and collision can end a run.
- [ ] Two route views remain in sync when unfolded.
- [ ] Route swap genuinely changes the active playable route, not just its color.
- [ ] One scripted situation rewards a well-timed route swap.
- [ ] Folded layout is usable; hinge hides no important action.
- [ ] RETRY starts a clean fresh run on both routes.
- [ ] Demo can be completed in 45–60 seconds without relying on fake screens.

## Fallback ladder / scope cuts

1. **If SpriteKit integration stalls by minute 30:** draw the runner and scrolling track using SwiftUI Canvas + a single timer. A functional 2.5D runner beats an unbuilt 3D one.
2. **If independent previews lag by minute 65:** simplify the alternate road to a lightweight animated view generated from the same obstacle array; never fork game logic.
3. **If gesture testing stalls:** use on-screen L/R/JUMP/SLIDE controls for the demo, retaining swipe as an enhancement.
4. **If fold API stalls:** use adaptive width/layout based on available geometry and show the transition manually. Avoid claiming hinge behavior that isn't implemented.
5. **If full visuals aren't ready:** preserve smooth road motion, bold UI and 2–3 memorable original assets. Skip character selection/shop/world map.
6. **If 15 minutes remain and the app runs:** stop feature development, record a successful take and rehearse.

## Demo script (50–60 seconds)

**0–8s:** Open the app unfolded. “Classic runners let you see one road. Duo lets us see *two* at the same time.” Tap PLAY.  
**8–23s:** Run and collect coins on Main Street. Point out that Bonus Alley has its own incoming coin trail and fewer obstacles.  
**23–38s:** A barrier approaches on the left. “Because the other route is visible, I can plan ahead.” Tap SWAP ROUTE; runner shifts to the right and collects the bonus coins.  
**38–47s:** Fold the device to the compact layout (only if confirmed working) and demonstrate continuity.  
**47–60s:** Show game over and RETRY or one extra swap. “We turned the fold into a new gameplay decision, not just a bigger screen.”

## Final deliverables

- Live Bitrig iPhone Duo simulator project that plays from start to retry.
- One short backup screen recording of the successful swap sequence.
- A screenshot of the unfolded game with both routes visible.
- This plan and a one-sentence pitch; **no requirement to produce an App Store-ready game**.

## Reference / compatibility notes

Bitrig's September 18, 2026 announcement says it can build native SwiftUI apps and test them in a folding 3D iPhone Duo simulator, including supported hinge callbacks. Confirm symbols in your installed SDK before depending on them: https://bitrig.com/blog/bitrig-builds-iphone-duo-apps. Event details emphasize newly possible Duo interactions and explicitly say a polished demo is sufficient: https://luma.com/yc-meetup-4378.
