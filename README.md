# Titan Business Pros — Website

The marketing site for **Titan Business Pros**, a small Oklahoma-based software and app development company. Live at **[www.titanbusinesspros.com](https://www.titanbusinesspros.com)**, served by GitHub Pages directly from this repo (see `CNAME`).

## Stack

Plain HTML/CSS/JS — no build step, no framework, nothing to `npm install`. Everything lives in one file, `index.html`. The only external dependency is [Three.js](https://threejs.org/), loaded from a CDN (`cdn.jsdelivr.net`), which powers the WebGL effects in the dark hero sections.

## Structure

It's a single-page app with a small client-side router — `showView(name, anchorId)`, defined in the inline `<script>` near the bottom of `index.html`. Six views, each an `<section id="view-*" class="view">`:

| View | Purpose |
|---|---|
| `home` | Landing page (placeholder — content to be filled in) |
| `platform` | The original homepage pitch — licensing the Town Fuss social platform |
| `about` | Company info, stats, reviews/GitHub proof |
| `glossary` | 50 plain-English tech term definitions, with search |
| `licensing` | Town Fuss licensing pricing, terms, and revenue projections |
| `products` | Apps & platforms for sale/license, with a coverflow carousel |

**Navigation:** full nav bar + CTA button on desktop. On mobile (≤760px) the header collapses to just the logo and a ☰ button — tapping it opens `#mobileNavOverlay`, a full-screen menu. Every page has to be reachable from that overlay, not just the desktop nav bar, or it's effectively unreachable on a phone.

## The 3D system

Across a few rounds the site picked up a fairly extensive layer of WebGL/CSS 3D effects — a live particle field and a lit, rippling centerpiece in every dark hero, cursor-tilt cards, a custom GLSL plasma shader, scroll-scrubbed animation, a page-flip view transition, a coverflow carousel, and more. It's organized as a few IIFEs near the bottom of the inline `<script>`, labeled in comments ("3D SYSTEM INIT" / "3D SYSTEM v2 INIT" / "3D SYSTEM v3").

If you're extending it:
- Gate anything non-essential behind `prefers-reduced-motion`, and anything that could jank a touch device (scroll effects, the page-flip transition, the magnetic cursor) behind `pointer: fine`.
- Keep it progressive enhancement — if the Three.js CDN fails to load, or `WebGLRenderer` throws, the site should still work as a plain page.
- Watch the CSS cascade: an unconditional base rule placed *after* a `@media` override in the stylesheet wins and silently breaks that override. (This exact mistake once made the mobile menu button invisible.)

## Testing before you push

There's no automated test suite — verification is a headless Playwright pass covering:
- All six views, at both a desktop viewport **and** a real mobile device profile (e.g. `playwright.devices['iPhone 13']`). A narrow desktop window alone doesn't set `pointer: coarse` / `hover: none`, so it won't catch touch-specific bugs.
- A `prefers-reduced-motion: reduce` pass.
- Zero `pageerror` / console `error` events — that's the real safety net.

## Assets

- `Titan Logo-2.png` / `Logo-Fav.png` — logo and favicon.
- `products/*.png` — the product flyer images on the Products page.

## Deploy

Push to `main`. GitHub Pages serves it directly — no build or CI step.
