# AGENTS.md — Nikunj Pro Games

## Project overview

Single-file static website (`index.html`) — all HTML, CSS, and JavaScript are embedded in one file. There is no build pipeline, no framework, and no package dependencies beyond two CDN resources (Google Fonts, Font Awesome).

## Key directories

```
/
└── index.html        # Entire application — structure, styles, and logic
└── README.md         # User-facing docs
└── AGENTS.md         # This file
```

## Architecture

Everything lives in `index.html` in three logical sections:

1. **`<style>` block** — all CSS, organized with comment banners:
   - CSS custom properties (color palette, shadows) at the top
   - Section-by-section rules matching the page layout
   - Animation keyframes co-located with the components that use them

2. **HTML body** — semantic sections: `header`, `#home` (hero), `#games`, `#trending`, `#leaderboard`, `#about`, `footer`, plus two modal overlays.

3. **`<script>` block** — plain JavaScript in named IIFE/function sections:
   - `PARTICLE SYSTEM` — canvas-based floating stars using `requestAnimationFrame`
   - `GAME DATA` — static arrays (`GAMES`, `TRENDING`, `LEADERBOARD`)
   - `RENDER FUNCTIONS` — `renderGames()`, `renderTrending()`, `renderLeaderboard()`
   - `FILTER TABS` — event delegation on the tabs container
   - `MOBILE NAV` — hamburger toggle
   - `LOGIN MODAL` — open/close/validate helpers
   - `GAME MODAL + MINI-GAME` — canvas-based click-the-target game with spawn/particle/collision logic
   - `INIT` — calls all three render functions on page load

## Coding conventions

- CSS custom properties (`--purple-1`, `--bg`, `--neon-purple`, etc.) for all brand colors — change the palette in one place.
- `clamp()` for all heading font sizes — no media-query font overrides needed.
- Glassmorphism cards use `backdrop-filter: blur()` + semi-transparent `background` + neon `border`.
- All animations use `transform` and `opacity` for GPU compositing (60 fps target).
- The particle canvas is `position:fixed` with `pointer-events:none` so it never blocks clicks.
- Game state is held in a plain `gameState` object; `gameLoop` holds the rAF handle for clean cancellation.

## Non-obvious decisions

- The particle canvas height is set to `document.body.scrollHeight` (with a 200ms delay) so particles cover the full page, not just the viewport.
- The mini-game canvas uses a logical resolution of 640×380 with CSS `width:100%` so it scales to any container size without blurring.
- `IntersectionObserver` drives the active nav-link highlight rather than a scroll listener to avoid jank.
