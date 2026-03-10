# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev      # Start Vite dev server (localhost:5173)
npm run build    # Build for production
```

No linter or test runner is configured.

## Project Overview

**"Same Track, Different World"** — a brand experience comparison tool for Wolff Olins × Eurostar. It simulates how four rail brands (Eurostar, Virgin, Uber Rail, Italo/Trenitalia) would handle the same 5-stage passenger journey through different push notifications and UX touchpoints.

## Architecture

### Main App: `same-track-different-world.jsx`

A single-file monolithic React component. All data, sub-components, and logic live here.

**Key data structures:**

- **`B`** — Brand design system object. One entry per brand (`eurostar`, `virgin`, `uber`, `trenitalia`), each containing CSS gradient strings, colour tokens (`accent`, `glass`, `pill`, `wallpaper`, `glow`), and text contrast values. This is the single source of truth for all brand colours.

- **`N`** — Content map keyed by `"brand-stage"` (e.g. `"eurostar-0"`, `"virgin-3"`). Each entry has `notifs[]` (push notification objects), `insight` (one-line strategic observation), `commentary` (paragraph), and `stats[]` (label/value pairs).

- **`STAGES`** — Array of 5 journey stages: Book (0), Prepare (1), Wait (2), Travel (3), Arrive (4).

**Key components (all in same file):**

- `Phone` — Renders an iPhone-style mockup with brand wallpaper, dynamic island, lock screen time, and notification cards.
- Main `App` — Manages `brand` and `stage` state, renders the split-screen comparison layout with left panel (insight + stats) and right panel (phone mockup).

### Static Pages: `public/`

Standalone HTML files not processed by Vite — served directly. These are captured into Figma using the Figma HTML-to-design script:

```html
<script src="https://mcp.figma.com/mcp/html-to-design/capture.js" async></script>
```

Current files: `confirmation-trenitalia.html`, `confirmation-uber.html`, `confirmation-virgin.html`, `color-palette.html`. All follow the same pattern: centred card on `#F5F5F5` background, brand colours applied inline.

## Brand Colour Reference

| Brand      | Primary       | Accent / Secondary     |
|------------|---------------|------------------------|
| Eurostar   | `#106DFF`     | `#FFC305` (yellow)     |
| Virgin     | `#E31E23`     | `#1A1A1A` (charcoal)   |
| Uber       | `#000000`     | `#276EF1` (blue)       |
| Trenitalia | `#C8102E`     | `#00665B` (green)      |

Note: The `B` object in `same-track-different-world.jsx` uses a **darker, richer** variant of these palettes for the dark-mode phone UI (e.g. Eurostar uses deep navy `#00205a` not `#106DFF` as the bg). The `public/` HTML files use the official brand palette colours above.

## Figma Integration

The `public/` HTML pages are designed to be captured into Figma via the MCP HTML-to-design capture script. To send a page to Figma, include the capture script tag and use the `generate_figma_design` MCP tool with the page's localhost URL.
