# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

This is a static website repository published to **https://4abhik.github.io** via GitHub Pages. It contains two projects:

- `index.html` — Personal website for Abhik Lal Mukherjee (startup consultant, Bangalore)
- `game/index.html` — "Aqua Blaster" browser game (Canvas 2D + Web Audio API)

No build step, no dependencies, no package manager. Everything is vanilla HTML/CSS/JS in self-contained files.

## Deploying changes

After editing any file, commit and push:

```powershell
git add -A
git commit -m "describe change"
git push origin main
```

GitHub Pages picks up the push automatically. The live site updates within ~60 seconds.

## Architecture

### Personal site (`index.html`)
Single-file, no framework. Sections flow: Nav → Hero → About → Services → How I Work → Contact. Styles are in a `<style>` block using CSS custom properties (`--ink`, `--accent`, etc.). The contact form uses a JS `handleSubmit()` function at the bottom — it currently shows a success state only (no backend). Google Fonts (Inter + Playfair Display) are loaded via `<link>`.

### Game (`game/index.html`)
Single-file Canvas 2D game. All audio is synthesised at runtime using the Web Audio API (no audio files). Key globals:

- `state` — game state machine (`ST.MENU / PLAY / CLEAR / OVER / WIN`)
- `P` — player object (`x`, `y`, `aim` angle, `spd`)
- `demons[]`, `fireballs[]`, `shots[]`, `parts[]` — entity arrays
- `lvl`, `score`, `lives`, `invT` (invincibility timer), `flashT` (screen flash timer)

Level configs are in the `LVLS` array (5 entries). Each controls demon count, HP, fire rate, fire speed, demon move speed, and whether arc trajectories are used.

The main loop (`loop(now)`) handles both update and draw each frame. Entity update functions are prefixed `upd*`, draw functions `drw*`.

Audio: `tone()` plays a shaped oscillator note; `nburst()` generates filtered noise. `musicStep()` is called on a `setInterval` at the BPM rate to drive the chiptune background loop.

## Owner contact details (in index.html)
- Email: 4abhik@gmail.com
- Phone: +91 99001 73976
- LinkedIn: https://www.linkedin.com/in/abhiklalmukherjee/
- GitHub Pages URL: https://4abhik.github.io
