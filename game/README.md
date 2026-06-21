# Aqua Blaster

A browser-based arcade shooter built with vanilla HTML5 Canvas and Web Audio API. No dependencies, no build step — one self-contained file.

**Play it:** [4abhik.github.io/game](https://4abhik.github.io/game)

---

## Gameplay

You are a water-gun-wielding hero defending against waves of fire demons. Shoot fireballs out of the air, destroy demons, and survive all 5 levels.

### Controls

| Action | Keys / Input |
|--------|-------------|
| Move   | Arrow Keys or `A` / `D` |
| Aim    | Mouse (or touch) |
| Shoot  | Left Click, Hold Space, or touch |

### Scoring

| Event | Points |
|-------|--------|
| Extinguish a fireball | +10 |
| Destroy a demon | +25 |
| Hit by a fireball | -5 |

You start with **3 lives**. Taking a hit triggers 2.5 seconds of invincibility.

---

## Levels

| # | Name | Demons | HP | Fire Rate | Notes |
|---|------|--------|----|-----------|-------|
| 1 | Ember Valley   | 3 | 1 | 2800 ms | Straight shots |
| 2 | Inferno Hills  | 4 | 2 | 2200 ms | Straight shots |
| 3 | Blaze Canyon   | 5 | 2 | 1700 ms | Arc fireballs introduced |
| 4 | Hellfire Peaks | 6 | 3 | 1200 ms | Arc fireballs |
| 5 | The Inferno    | 8 | 3 | 750 ms  | Fast arc fireballs |

Arc fireballs have gravity applied mid-flight, making them harder to intercept.

---

## Technical Details

### Stack
- **Rendering:** HTML5 Canvas 2D (800 × 576, pixel-art style)
- **Audio:** Web Audio API — all sounds synthesised at runtime, no audio files
- **Input:** Keyboard, mouse, and touch (mobile-friendly)

### Architecture

The entire game lives in `game/index.html`. Key globals:

| Symbol | Description |
|--------|-------------|
| `ST` | State enum — `MENU`, `PLAY`, `CLEAR`, `OVER`, `WIN` |
| `state` | Current game state |
| `P` | Player object (`x`, `y`, `aim` angle, `spd`) |
| `demons[]` | Active demon entities |
| `fireballs[]` | Active fireball projectiles |
| `shots[]` | Active water-shot projectiles |
| `parts[]` | Particle effects |
| `lvl` | Current level (1–5) |
| `score` | Current score |
| `lives` | Remaining lives |
| `invT` | Invincibility timer (seconds remaining) |
| `flashT` | Screen flash timer (seconds remaining) |

### Main Loop

`loop(now)` runs every frame via `requestAnimationFrame`. It handles both update and draw in one pass:

1. **Update** — player movement, demon AI, projectile physics, collisions
2. **Draw** — background → particles → fireballs → shots → demons → player → HUD → post-FX

Update functions are prefixed `upd*`; draw functions are prefixed `drw*`.

### Audio

All audio is synthesised from scratch each time it plays:

| Function | Sound |
|----------|-------|
| `tone(f, t, dur, type, vol)` | Shaped oscillator note (square / sine / sawtooth / triangle) |
| `nburst(t, dur, vol, freq, q)` | Band-pass filtered noise burst |
| `sfxShoot()` | Water gun fire |
| `sfxSplash()` | Fireball extinguished |
| `sfxHit()` | Player hit |
| `sfxDead()` | Demon destroyed |
| `sfxLevelUp()` | Level complete fanfare |
| `musicStep()` | One tick of the chiptune background loop (148 BPM) |

The background music uses a 16-step melody (`MEL`) and an 8-step bass line (`BAS`), driven by `setInterval` at the BPM rate.

### Collision Detection

Circle-vs-circle using squared distance (`d2()`):

- Water shots vs fireballs — radius 26 px combined
- Water shots vs demons — radius 38 px combined
- Fireballs vs player — radius 29 px combined (skipped during invincibility)

---

## Running Locally

Open `game/index.html` directly in any modern browser — no server needed.
