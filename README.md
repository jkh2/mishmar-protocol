# Mishmar Protocol


![Current Mishmar Protocol gameplay](image1.png)

_The current playable browser build: the House, two power banks, a deployed four-drone
sector, an incoming scout, and the layered space battlefield._

<details>
<summary><strong>Visual north star / concept reference</strong></summary>

![Mishmar Protocol visual direction](image1.png)

_This is concept/reference art, not an in-game screenshot. Its black-and-gold House,
descending threat, and illuminated terrain informed the current visual direction._

</details>

**מִשְׁמָר** — *the watch; the guard post.*

A 2D defense game about powerless machines and a finite amount of power. You protect a
dark-gold pyramid bearing an illuminated Bet (**בּ**) — the House — from drones descending
from the top of the screen. Your defenders start dim, inert, and useless. Everything you do
is about getting power into them, deciding how to divide it, and choosing where it goes.

This repository currently holds a **playable vertical slice**: the complete movement and combat
loop, its first enemy type, onboarding, tactical sound, and a coherent visual battlefield. The
economy and larger campaign are deliberately not built yet. It is a static HTML game with a
small CC0 asset folder, no build step, and no code dependencies.

### What exists today

- A finite energy packet that is both the gathering grip and the deployment budget.
- Physical drone recruitment: defenders bend toward the pointer and must actually arrive.
- Commit, carry, release, autonomous patrol, combat, burnout, recovery, and fleet growth.
- Escalating waves of scouts that pressure the House and the surviving power banks.
- A seeded space scene with layered stars, nebula light, a ringed planet, mountain ridges,
  cratered terrain, perspective seams, and illuminated power conduits.
- Distinct defender and enemy silhouettes, charged thruster trails, firing flashes, structural
  damage, electrical battery jolts, pyramid shockwaves, particles, and screen shake.
- Pooled tactical sound effects plus synthesized pointer charge and infrastructure explosions.
- A live HUD, contextual first-wave guidance, pause/restart/mute controls, reduced-motion
  behavior, and an on-canvas fault screen for recoverable debugging.

---

## Play it

### ▶ [TRY DEMO HERE](https://jameskeithharwood.com/mishmar-protocol)

Open `index.html` in any modern browser. That's the whole install.

### The loop, in one sentence

Draw a packet of energy from a battery, sweep up as many dim drones as you dare while it
burns down, press to commit the squad, drag them where you want them, and release — whatever
energy survived the trip is split among however many drones you're still holding.

### Controls

| Action | What it does |
|---|---|
| **Move the pointer** | Dim drones inside the magnetic radius bend toward you and are recruited when they arrive |
| **Click a lit battery** | Draws one energy packet. Its pointer ring shrinks and its electronic tone falls as the packet burns down |
| **Press and hold** | Commits the squad you've gathered. Gathering stops — this is your decision point |
| **Drag** | Carries the squad. Energy is still burning, so distance costs power |
| **Release** | Sets them down. They light up and patrol that sector on their own |
| **Space** | Pause |
| **R** | Restart |
| **M** | Mute or restore sound effects |

### The rule that matters

One packet is a **fixed energy budget**, split at the moment you release:

```
charge_per_drone = min(max_charge, remaining_energy / drones_held)
```

Drain is flat, so a drone at 25% burns out in roughly a quarter of the time a drone at 100%
does. That single rule is the entire tactical game:

- **Three drones, dropped close.** They burn white-gold, do full damage, and hold a sector
  for about fourteen seconds of active fighting. A scalpel.
- **Twelve drones, hauled across the field.** They arrive faint, do a quarter damage, and
  fade in a few seconds. A wall — broad coverage, briefly.
- **Take too long.** You're standing over a big swarm with nothing left to give it.

The number beside your cursor (`drop → 8 × 44%`) is what you'd get if you released right now.
It falls every time another drone joins, and it keeps falling while you carry them. Watch it.

### What can go wrong

- **The packet runs dry mid-carry.** The squad falls dim exactly where you're standing. No
  penalty is applied — you simply have to walk back to a battery and come get them.
- **Powerless drones get knocked aside.** They can't be destroyed — the fleet is an asset you
  keep. Charged drones lose charge on contact instead. The pyramid fabricates one more after
  every wave, so the fleet grows slowly; what limits you is that one packet split across thirty drones
  isn't worth splitting.
- **Batteries take damage.** A damaged battery rebuilds packets more slowly; a destroyed one
  stops entirely. The two banks sit at opposite ends of the ground, so losing one doesn't just
  halve your supply — it doubles your travel.
- **Scouts divide pressure by target class.** Each new scout has a 45% chance to choose a live
  battery and otherwise attacks the pyramid. If one bank is destroyed, that battery pressure
  concentrates on the survivor rather than increasing; destroyed banks are never selected.
- **The pyramid falls.** Its seams fail, its surface fractures, and the Bet dims as it takes
  damage. At zero, the run ends.

### Reading the field

Charge is shown two ways on purpose, so it survives a crowded screen and doesn't depend on
color vision: **brightness** (white-gold → gold → amber → faint) and a **charge arc** drawn
as a ring around each drone. Battery health is shown as pips rather than a color. Sectors held
by patrolling squads draw a faint dashed circle. Charged defenders add directional thruster
trails and a brief firing flash; powerless defenders keep the same recognizable vector body
but lose the glow and trail.

Infrastructure impacts are intentionally different. A battery breach produces a compact,
steel-white electrical fork. A pyramid breach throws a larger warm ember burst, stronger shake,
and an expanding shockwave. The sound follows the same language: batteries are tighter and
sharper; the House carries a longer, lower pressure boom.

Pointer energy is also audible: drawing a packet starts a quiet synthesized tone whose pitch
falls from 310 Hz toward 68 Hz with the actual remaining charge. Deployment, depletion, pause,
mute, restart, game over, and background-tab visibility changes all release the tone cleanly.

The backdrop is presentation-only. Its seeded stars, ridgelines, craters, and rocks remain
stable rather than flickering between frames. Motion reduction disables scenery drift and
twinkle, screen shake, and the pyramid shockwave while reducing particle and electrical-bolt
counts; the gameplay state and readable impact cues remain intact.

---

## Design rules

These are settled decisions, not open questions. They're recorded here so the next change
doesn't quietly undo one of them.

1. **Energy is the grip.** No packet, no magnetism. You cannot pre-gather a swarm and then
   go shopping for power — the clock starts when you draw, and gathering happens under it.
2. **Capture requires arrival, not overlap.** Drones accelerate toward the pointer at a capped
   speed and are only recruited when they physically reach it. Slamming the mouse onto a drone
   doesn't grab it. The visible bend cannot be skipped by the input device.
3. **The press is a commitment.** Holding stops gathering. That's the moment you decide the
   squad is the squad.
4. **Release is deployment, not attack.** Squads set down hold and patrol the sector where you
   left them, engaging what comes near, until they burn out.
5. **Burnt-out drones stay where they fought.** They go dim, drift, and are collectable again.
   The field ends up recording the history of the battle.
6. **The fleet is never lost, only unpowered.** Growth is self-limiting: the split rule makes a
   large fleet useless to charge all at once, so the real decision becomes *which* drones to
   gather, not how many you own.
7. **Difficulty ramps by behavior, not health.** The first three waves are eased on count,
   spawn spacing, and descent speed, converging to the normal curve by wave four with no cliff.
8. **Nothing keys off pointer speed.** An earlier build deployed squads on a fast flick, which
   fired accidentally because gathering *requires* moving fast. Press-and-drag replaced it.
9. **Target-class pressure stays stable.** The 45% battery-target chance applies to the battery
   class as a whole, not independently to each bank. Losing a bank concentrates the same
   pressure instead of silently making all later waves more battery-heavy.

---

## Tuning

Everything is in the `CFG` object at the top of the file. No balance value lives in game logic.

| Knob | Current | What it controls |
|---|---|---|
| `pointer.energy` | `3.0` | Total charge units in one packet. The whole split model scales from this |
| `pointer.burnSeconds` | `12.0` | How long a packet survives in your hand. Timer and budget are the same variable |
| `pointer.magnetRadius` | `115` | Gathering reach |
| `pointer.patrolRadius` | `58` | How far a set-down squad drifts from its anchor |
| `drone.drainPerSec` | `0.052` | Charge lifetime. Full ≈ 19s idle, ≈ 14s firing; quarter ≈ 4.8s |
| `drone.maxCharge` | `1.0` | Ceiling on any single drone |
| `stages` | 4 bands | Charge thresholds, damage multipliers, and colors |
| `battery.cooldown` | `2.6` | Seconds to rebuild a packet |
| `battery.damagedPenalty` | `2.2` | Cooldown multiplier at zero battery health |
| `enemy.scout.batteryTargetChance` | `0.45` | Chance a new scout targets one of the currently living banks |
| `impact.battery / impact.pyramid` | 2 profiles | Particle count, shake, color, electrical jolt or shockwave, and sampled impact accent |
| `sound.chargeTone` | `68-310 Hz` | Synthesized pointer-energy pitch range, gain, attack, release, and glide |
| `sound.impactExplosion` | 3 layers | Synthesized crack, filtered debris body, and falling pressure boom for infrastructure breaches |
| `drone.trail* / flashSeconds` | `3 steps / 0.09s` | Charged-drone direction trail and firing recoil flash |
| `wave.easeWaves/easeSpawn/easeSpeed/easeCount` | `3 / 2.3 / 0.60 / 0.55` | The onboarding ramp |
| `wave.dronesPerWave` | `1` | Replacements the pyramid fabricates after each wave |
| `startingDrones` | `14` | Drones on the field at the start of a run |

The loop runs a fixed 120 Hz simulation decoupled from rendering, capped at 8 catch-up steps
per frame so a stall can't spiral. Faults are caught per-frame and drawn on screen with the
error message and stack line; the loop keeps running and `R` clears the fault.

### Technical shape

- `index.html` contains the game, Canvas 2D renderer, simulation, input, and audio integration.
- The logical battlefield is `960 × 600` and scales responsively through CSS.
- Gameplay advances on a fixed `1/120s` step; rendering follows `requestAnimationFrame`.
- Dense sampled effects use small reusable `<audio>` voice pools with event throttling.
- Continuous charge audio and three-layer breach explosions use lazily created Web Audio nodes.
- Explosion nodes disconnect after their envelopes, and the generated noise buffer is reused.
- The mute preference is stored locally. The game has no analytics, network gameplay dependency,
  package manager, framework, build tool, or runtime model call.

---

## What isn't built yet

Deliberately excluded, in this order:

- **The economy.** Salvage, repair, and fabrication. The rule was to prove the loop is fun
  before tuning numbers that only matter if it is.
- **The enemy roster.** Only the scout exists. Bomber, jammer, siphon, armored drone, and
  carrier/commander are designed but not implemented.
- **Levels.** Waves currently escalate on a formula. The plan is ten acts of ten, with a boss
  and checkpoint every tenth level, driven by a configurable threat budget.
- **Production completion.** Music, a final mix, settings, saves, broader accessibility options,
  and continued art/animation polish beyond the current battlefield pass.

## What comes after

AI integration is intentionally the *last* thing, not the first. The base game must be complete
and enjoyable without a model in the loop. When it is, the intelligence housed in the Bet-marked
pyramid can be reached through three engine-neutral interfaces already planned for: a compact
battle-state snapshot, a bounded set of legal actions and advice outputs, and an append-only
event log with deterministic wave seeds. It must tolerate latency or total unavailability, and
must never be required for pause, save, combat timing, or completion.

---

## Open questions

Honest ones, still unanswered:

- Does the trip to the battery read as **tension** or as **tax**? If those seconds feel like
  standing around rather than repositioning, the fix is a shorter cooldown and batteries placed
  further apart — not a looser rule.
- Does twelve faint drones **ever** beat four bright ones? If the answer is never, the split
  model is wrong and the game should go flat-rate.
- At a twelve-second burn, is running the packet dry a **risk players choose to take**, or just a
  mistake they learn to avoid? If nobody ever gambles on one more drone, the burn is too short
  for the gamble to be real.

---

## Art and audio provenance

Runtime use from Kenney's
[Space Shooter Redux](https://opengameart.org/content/space-shooter-redux) asset pack is limited
to the subtle tiled star texture, the enemy scout sprite, and the sampled `.ogg` effects. Those
source assets are CC0 1.0; the original license is preserved in `assets/kenney/LICENSE.txt`, and
the exact files are listed in [ASSET_CREDITS.md](ASSET_CREDITS.md).

The defender silhouette is original vector canvas artwork. The pyramid, batteries, charge cores,
charge arcs, projectiles, damage states, particles, shockwave, electrical forks, nebula, planet,
terrain ridges, craters, rocks, perspective grid, and power conduits are also drawn by Mishmar's
renderer. The scenery layout is procedurally generated from a fixed seed.

Sound begins only after the first player gesture, in accordance with browser autoplay rules.
Rapid combat sounds use pooled sampled voices. The falling pointer tone and the infrastructure
explosions are synthesized with Web Audio; each explosion combines a transient crack, filtered
noise body, and descending low-frequency pressure wave through a compressor.

`image1.png` is documentation-only concept/reference art and is not loaded by the game.
`gameplay.png` is a capture of the current playable browser build.
