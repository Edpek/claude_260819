# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A collection of small, self-contained front-end prototypes. Each top-level folder is a single-page app consisting of one `index.html` file with inline `<style>` and `<script>` — no build step, no bundler, no package manager, no external dependencies (not even a CDN script). Everything is vanilla HTML/CSS/JS.

- `creative-toy/index.html` — 파티클 플레이그라운드: canvas particle playground (trail/burst/orbit modes) driven by mouse or touch, animated via `requestAnimationFrame`.
- `game/index.html` — 네온 스네이크: canvas-based Snake game with keyboard, on-screen d-pad, and swipe controls; high score persisted to `localStorage`.
- `productivity-tool/index.html` — 포커스 타이머: Pomodoro-style focus timer (work/short-break/long-break modes) plus a to-do list; both timer session count and todos persisted to `localStorage`.

UI text/labels in all three apps are in Korean.

## Running

There is no build, install, lint, or test tooling in this repo. Open a prototype directly in a browser:

```bash
start creative-toy/index.html
start game/index.html
start productivity-tool/index.html
```

Since each app is a single file, just editing `index.html` and reloading the browser is the entire dev loop.

## Architecture notes

Each `index.html` follows the same internal shape:
- CSS custom properties (`:root { --bg; --text; ... }`) define the color palette at the top of `<style>`.
- All JS is wrapped in a single IIFE at the bottom of the file, with no modules, no build-time transforms, and no shared code between the three apps — they are fully independent.
- State that should survive a reload (todo list, Pomodoro session count, Snake high score) is read/written directly to `localStorage` with plain `JSON.parse`/`JSON.stringify`; there is no abstraction layer over it.
- `game/index.html` and `creative-toy/index.html` both drive a `<canvas>` with a manual animation loop (`setInterval` for the Snake game tick, `requestAnimationFrame` for the particle loop) and handle mouse, touch, and keyboard input directly via DOM event listeners — no input library.

When modifying one of these apps, keep changes self-contained within its single HTML file — there's no shared infrastructure to extend, and introducing one (build tooling, shared JS/CSS) would be a bigger change than these prototypes call for.
