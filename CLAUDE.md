# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Single-file browser game: `shooter.html` — open directly in any browser, no build step, no dependencies.

## Architecture

Everything lives in `shooter.html` as inline `<script>`. Key sections:

- **Game loop** — `requestAnimationFrame` loop calling `update(dt, now)` then `draw()`. Delta time capped at 50ms.
- **State machine** — `state` string: `menu` → `playing` → `level_complete` → `game_over`
- **Entity arrays** — `enemies[]`, `bullets[]`, `enemyBullets[]`, `particles[]` managed per-frame
- **Spawning** — `enemiesToSpawn[]` queue drained by `spawnTimer` every `SPAWN_INTERVAL` ms; queue built by `prepareLevel()` from `getLevelConfig(level)`
- **Collision** — all entities are circles; `circleHit(ax,ay,ar, bx,by,br)` does `dx²+dy² < (r1+r2)²`
- **Drawing** — no sprites; everything drawn with Canvas 2D primitives (rects, arcs, transforms)
- **Enemy AI** — per-type logic inside `updateEnemy()`: grunt walks straight, charger pause/rush cycles, shooter keeps preferred distance and fires back
- **Level scaling** — levels 1–3 have fixed configs; level 4+ applies `Math.pow(1.2, extra)` count multiplier and `Math.pow(1.1, extra)` speed multiplier

## Adding enemies / levels

- Add a new type in the `templates` object inside `spawnEnemy()`
- Add AI logic branch in `updateEnemy()`
- Update `getLevelConfig()` to include the new type count
- Add a draw branch in `drawEnemy()` (or extend the existing body-drawing code)
