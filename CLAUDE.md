# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains a single self-contained HTML file, [car_race_game.html](car_race_game.html) — a browser-based pixel-art car racing game. There is no build system, package manager, or test suite; all HTML, CSS, and JavaScript live in this one file.

## Running the Game

Open `car_race_game.html` directly in a browser (e.g. `open car_race_game.html` on macOS). No server or build step is required.

## Architecture

Everything is contained in `car_race_game.html`:

- **Layout/CSS**: A `#track` element contains two `.lane` divs (Player and Computer), each with a `.car` div rendering an inline pixel-art SVG car. A `#finish` div renders the checkered finish line near the right edge, and `#countdown` is an absolutely-positioned overlay centered on the track for the "3-2-1-GO" countdown.
- **Game state machine**: A single `state` variable cycles through `idle` → `countdown` → `racing` → `finished`.
- **Controls**: Keyboard listener on `window` — `M` starts/restarts the race (triggers `startCountdown()`), `X` boosts the player car's speed while `state === 'racing'`.
- **Movement model**: Car positions (`playerPos`, `computerPos`) are pixel offsets translated via CSS `transform`. The player's speed decays each frame (`PLAYER_DECAY`) plus a small base drift, so the player must repeatedly press `X` to maintain speed. The computer's speed is randomized once per race (`computerSpeed`) with small per-frame variance, simulating AI difficulty variation.
- **Game loop**: `gameLoop()` runs via `requestAnimationFrame`, updating positions each frame until either car reaches `FINISH_LINE` (computed from track width minus finish-line offset and car width), then calls `finishRace()` to declare a winner and stop the loop.
