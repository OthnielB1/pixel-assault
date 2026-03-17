# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

Browser-based games built as single self-contained HTML files — no build tools, no dependencies, no server required. Open any `.html` file directly in a browser to run.

## Project Structure

- `tictactoe.html` — Two-player Tic Tac Toe
- `shooter.html` — Pixel Assault, a retro top-down shooter

## Architecture: shooter.html

All game logic lives in a single `<script>` block. Key sections in order:

- **`rr()`** — utility for drawing rounded rectangles on canvas
- **`LEVELS[]` / `getLvl()`** — level config table; levels beyond index 5 are computed dynamically
- **`G`** — single global state object holding all runtime state (game state, entities, timers, input)
- **Entity functions** — `mkPlayer`, `mkEnemy`, `mkBoss`, `tryDropPU` create entities as plain objects
- **Draw functions** — `drawPlayer`, `drawEnemy`, `drawBoss`, `drawPU`, `drawBullets`, `drawParticles`, `drawHUD`, `drawMenu`, `drawPaused`, `drawGameOver`, `drawLevelComplete`
- **`update(now)`** — main update tick; skips entirely when `G.state === 'PAUSED'`
- **`render()`** — draws the current frame; applies screen shake via `ctx.save/translate/restore`
- **`loop(now)`** — `requestAnimationFrame` loop calling `update` then `render`

### Game States
`MENU → PLAYING ↔ PAUSED → GAME_OVER → MENU`
`PLAYING → LEVEL_COMPLETE → PLAYING` (repeats per level)

State is changed only through `setState(s)`, which also clears held input keys and mouse state.

### Input
- Mouse position and `down` flag tracked on `G.mouse`
- Keys tracked on `G.keys` by `e.code`
- Shooting is driven by `G.mouse.down` checked each update tick; `cooldown` on the player object rate-limits fire

## Git Workflow

Always commit after meaningful changes and push to keep GitHub in sync:

```bash
git add <file>
git commit -m "descriptive message"
git push
```

Remote: `https://github.com/OthnielB1/pixel-assault`
