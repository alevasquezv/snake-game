# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

A single-file browser Snake game: `snake.html`. No build step, no dependencies, no package manager. Open the file directly in a browser to run it.

```bash
open snake.html
```

## Architecture

Everything lives in `snake.html` — inline `<style>`, HTML structure, and a `<script>` block. There is no bundler, framework, or external file.

**Game loop**: `requestAnimationFrame` calls `tick(timestamp)`, which uses a delta-time accumulator to fire game logic ticks at the current `speed` interval (ms) independently of frame rate.

**Key state variables**:
- `snake` — array of `{x, y}` cells, head at index 0
- `dir` / `dirQueue` — current direction and buffered input queue (max 2)
- `food` / `specialFood` — normal food cell and optional special item (`{x, y, type, spawnTime}`)
- `obstacles` — array of wall cells that grow with score
- `effectActive` — current power-up effect `{type, endTime}` or `null`
- `speed` / `baseSpeed` — current tick interval and score-derived base (modified by slow effect)

**Special food cycle**: every 5 normal foods eaten (`foodsEaten % 5 === 0`), `trySpawnSpecial()` places a bonus item of type `gold`, `slow`, or `boost` alongside the normal food. It despawns after 8 seconds.

**Rendering**: all drawing is done in `draw(timestamp)` via Canvas 2D. Floating score text uses absolutely-positioned DOM elements inside `.screen-wrap`. Particles are tracked in the `particles` array and drawn each frame.

**Audio**: Web Audio API oscillators synthesized inline in `playEat()`, `playBonus()`, `playGameOver()`. `AudioContext` is created lazily on first keypress.

**Persistence**: high score stored in `localStorage` under key `snakeHi`.

## Git workflow

Commit and push to GitHub after every meaningful change:

```bash
git add snake.html
git commit -m "short imperative summary"
git push
```

Remote: `https://github.com/alevasquezv/snake-game` (branch `main`).
