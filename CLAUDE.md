# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

A classic Tetris implementation in vanilla JavaScript using HTML5 Canvas. No build step, no dependencies, no package.json — just three files (`index.html`, `style.css`, `game.js`) that run directly in a browser.

## Running the game

Open `index.html` directly, or serve it statically:

```bash
python3 -m http.server 8000   # then open http://localhost:8000
# or
npx serve .
```

There is no build, lint, or test tooling in this repo.

## Architecture

All game logic lives in `game.js` (~300 lines, single file, no modules). Key pieces:

- **Board model**: `board` is a `ROWS × COLS` matrix where each cell is `0` (empty) or an integer 1–7 identifying the piece color (index into `COLORS`).
- **Pieces**: `PIECES` defines each tetromino as a square matrix. Rotation is done via `rotateCW`, a transpose-based matrix rotation (no piece-specific rotation tables).
- **Collision** (`collide`): checks board bounds and existing filled cells for a given shape/offset.
- **Wall kicks** (`tryRotate`): after rotating, tries offsets `[0, -1, 1, -2, 2]` columns until a non-colliding position is found.
- **Game loop** (`loop`): driven by `requestAnimationFrame`; accumulates elapsed time in `dropAccum` and advances the piece one row when it exceeds `dropInterval`.
- **Locking/clearing**: `lockPiece` → `merge` (writes piece into `board`) → `clearLines` (scans bottom-up, splices full rows, unshifts empty ones, recalculates score/level/dropInterval).
- **Scoring/level**: `LINE_SCORES = [0, 100, 300, 500, 800]` × current level; level = `floor(lines / 10) + 1`; `dropInterval = max(100, 1000 - (level-1)*90)` ms. Hard drop scores 2 pts/cell dropped, soft drop 1 pt/row.
- **Ghost piece**: `ghostY()` projects the current piece straight down until collision; drawn at `globalAlpha = 0.2`.
- **State**: global mutable variables (`board`, `current`, `next`, `score`, `lines`, `level`, `paused`, `gameOver`, etc.) reset in `init()`, which is also called by the restart button.

Control flow: `init()` → `spawn()` (pulls `next` into `current`, generates a new `next`, calls `endGame()` if the new piece immediately collides) → `requestAnimationFrame(loop)`. Keyboard input is handled by a single `keydown` listener that dispatches on `e.code`.

If you change `COLS`, `ROWS`, or `BLOCK` in `game.js`, also update the `<canvas id="board">` `width`/`height` in `index.html` to match (`COLS × BLOCK`, `ROWS × BLOCK`).
