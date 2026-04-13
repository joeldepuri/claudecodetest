# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

A collection of browser-based games — each game is a **single self-contained HTML file** with all CSS and JavaScript inlined. No build tools, no dependencies, no package manager. Open any file directly in a browser to play.

**GitHub:** https://github.com/joeldepuri/claudecodetest

## Running Games

```bash
# Windows — open in default browser
start shooter.html
start tictactoe.html
```

## Git Workflow

Commit and push after every meaningful change:

```bash
git add <specific-files>
git commit -m "short imperative subject under 72 chars"
git push
```

Never use `git add -A` or `git add .` — stage files explicitly. Always push immediately after committing.

## Architecture

### Single-file pattern
Each game is one `.html` file structured as:
```
<style>   — layout and non-game UI
<canvas>  — rendering surface (games use Canvas 2D API)
<script>  — all game logic
```

### shooter.html — Top-Down Shooter
- **State machine:** `MENU → PLAYING → LEVEL_COMPLETE → GAME_OVER → MENU` (constants in `S` object)
- **Entity classes:** `Player`, `Enemy`, `Bullet`, `Particle` — all drawn programmatically via Canvas API, no image assets
- **Game loop:** `requestAnimationFrame` with delta-time (`dt`) capped at 50ms to prevent physics tunneling on tab blur
- **Global arrays:** `bullets[]` and `particles[]` are module-level; `enemies[]` is reassigned per wave
- **Level data:** `LEVELS` array — each entry is an array of waves; each wave is an array of `{t: enemyType, n: count}`
- **Enemy types** (`EDEFS`): GRUNT, TANK, FLANKER, SHOOTER — defined with color, radius, speed, hp, score value
- **Input:** `keys{}` map for keyboard, `mouse{x,y,fire}` for pointer; `enterPressed()` guards against held-key repeat across state transitions
- **Screen shake:** module-level `shkMag/shkDur/shkX/shkY` — call `shake(mag, dur)`

### tictactoe.html — Tic Tac Toe
- Vanilla DOM manipulation, no canvas
- Win detection iterates `WINS` (8 line combinations) each move
