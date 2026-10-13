# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Asteroids clone in plain HTML5 Canvas + vanilla JS (ES6+). No dependencies, no bundler, no package.json, no tests, no linter. README and code comments are in Spanish.

## Running

Open `index.html` directly in a browser, or serve statically with `npx serve .` (http://localhost:3000). There is no build step.

## Architecture

All game logic lives in a single script, `game.js`, loaded by `index.html` (800x600 `<canvas id="canvas">`). Constants `W`/`H` define the play field, which is toroidal (entities wrap at the edges).

- **Input**: `keys` / `justPressed` maps with a `pressed(code)` helper (one-shot presses for firing).
- **Entities**: classes `Bullet`, `Asteroid`, `Ship`, `Particle`. Asteroid size is an index (1=small, 2=medium, 3=large) into the `SPEEDS` and `POINTS` arrays; destroying one spawns two of the next size down.
- **State**: module-level globals (`ship`, `bullets`, `asteroids`, `particles`, `score`, `lives`, `level`) plus `state` = `'playing' | 'dead' | 'gameover'` and `deadTimer` for respawn delay.
- **Flow**: `initGame()` / `nextLevel()` / `spawnAsteroids()` (keeps a safe distance from the ship) / `killShip()` / `explode()`. The main `loop(ts)` computes a delta time `dt` in seconds and calls `update(dt)` then `draw()` (`drawHUD`, `drawOverlay`, `drawLifeIcon`). Movement is dt-based (e.g. thrust in px/s²), not frame-based.

Power-ups and the shooting-star asteroid were removed in earlier commits; the README still mentions them, so don't assume they exist.
