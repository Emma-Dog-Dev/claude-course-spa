# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project: Claude Course - First

This repo contains two co-located pieces:

1. **Nexus SPA** — a standalone Apple-style product page for a brain-chip-to-LLM product (`my-app/public/nexus.html`). Pure HTML/CSS/JS, no build step. Design reference: `apple.png`.
2. **React App** — a scaffolded Vite + React 19 app (`my-app/`) that currently serves as the dev server host. The React app itself (`src/App.jsx`) is still boilerplate.

## Commands

All commands run from `my-app/`:

```bash
npm run dev       # Start dev server → http://localhost:5173 (or next available port)
npm run build     # Production build → my-app/dist/
npm run preview   # Preview production build
npm run lint      # ESLint
```

**Accessing the Nexus SPA:**
- Dev: `http://localhost:5173/nexus.html` (served as static from `public/`)
- Direct: open `my-app/src/index.html` in a browser (no server needed)

## Architecture

### File layout
```
my-app/
  index.html          # Vite entry point — mounts the React app (#root)
  vite.config.js      # Vite config with @vitejs/plugin-react
  src/
    main.jsx          # React entry — renders <App> into #root
    App.jsx           # Root React component (boilerplate counter)
    App.css / index.css
    index.html        # Nexus SPA source file (pure HTML — not a React component)
  public/
    nexus.html        # Nexus SPA served as static asset (copied from src/index.html)
    vite.svg
```

### Two-track serving model
- `/` → Vite serves `my-app/index.html`, which boots the React app via `src/main.jsx`
- `/nexus.html` → Vite serves `public/nexus.html` as a raw static file (no bundling)
- Anything in `public/` is served at the root path without processing

### Nexus SPA design system (derived from `apple.png`)
All CSS lives in a single `<style>` block inside `nexus.html`. Key tokens:

| Token      | Value     | Role                        |
|------------|-----------|-----------------------------|
| `--sky`    | `#e8f4fd` | Hero/section backgrounds    |
| `--blue`   | `#0071e3` | Primary CTAs, accents       |
| `--text`   | `#1d1d1f` | Headings and body           |
| `--gray`   | `#6e6e73` | Secondary text              |

- Font: `-apple-system, BlinkMacSystemFont, "SF Pro Display", "Segoe UI", sans-serif`
- Hero headline: `clamp(52px, 8vw, 96px)`, weight `700`, letter-spacing `-2px`
- Accent gradient: `#0071e3 → #5e5ce6 → #bf5af2`
- Cards: `border-radius: 18px`, hover `translateY(-4px)`
- Nav: sticky, `backdrop-filter: blur(20px)`, `rgba(255,255,255,0.82)`
- Scroll reveal: `.fade-up` class toggled via `IntersectionObserver`
- All illustrations are inline SVG — no external image assets

### Nexus SPA section order
`nav → hero (floating chip SVG) → tagline → highlights grid → 3 feature rows → stats → LLM compatibility → specs → pricing → CTA → footer`

**Important:** when editing `nexus.html`, keep both copies in sync:
- `my-app/src/index.html` (source)
- `my-app/public/nexus.html` (served copy)
