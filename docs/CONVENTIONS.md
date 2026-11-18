# Coding Guidelines (Agent & Human Reference)

Project: **az-metertaxi** — static Astro landing page for Meter Taxi.

This file is the shared source of truth for conventions. Tool-specific files
(`CLAUDE.md`, `.cursor/rules/*`, `.github/copilot-instructions.md`) summarize it.

---

## Stack

| Aspect | Choice |
| --- | --- |
| Framework | Astro 6 (`output: "static"`) |
| Language | TypeScript (strict) |
| Styling | Plain CSS inside `.astro` `<style>` (no Tailwind/preprocessor) |
| Client JS | Vanilla DOM inside `.astro` `<script>` |
| Node | `>=22.12.0` |

## Component anatomy

```astro
---
// 1. Frontmatter: imports, typed data, constants
---

<!-- 2. Markup -->

<style is:global>
  /* 3. Styles (scoped by default; is:global for shared utilities) */
</style>

<script>
  // 4. Client behavior (only when needed)
</script>
```

- Files: `src/components/PascalCase.astro`, pages in `src/pages/`.
- Data-driven lists: typed arrays in frontmatter + `.map()` in markup.
- Icons: SVG strings in data + `set:html`.
- Guard DOM access in scripts.

## Design tokens (current, hardcoded values)

| Token | Value |
| --- | --- |
| Background gradient | `#0f0c29 → #302b63 → #24243e` |
| Accent gradient | `#818cf8 → #c084fc` |
| Primary ink | `#fff`, muted `rgba(255,255,255,0.6/0.4)` |
| Font | `"Inter"` |
| Radius (pill / card / icon) | `2rem` / `0.75–0.875rem` / `12–14px` |

### Animation utilities (reuse, don't reinvent)

- Scroll reveal (IntersectionObserver in `index.astro`): `.reveal`, `.reveal-left`, `.reveal-right`, `.reveal-scale` → get `.revealed`.
- Entrance keyframes: `.animate-fade-up`, `.animate-fade-down`, `.animate-fade-scale`, `.animate-slide-left`, `.animate-slide-right`.
- Delays: `.delay-1..9` (entrance), `.reveal-delay-1..5` (scroll reveal).
- Keyframes: `fadeUp`, `fadeDown`, `fadeScale`, `slideLeft`, `slideRight`, `floatSlow`.

## Responsive breakpoints

Mobile-first, in `min-width` order: `600px` (tablet/2-col), `768px`, `1024px` (desktop), `1400px`.

## Data shape — `apps` (AppSection.astro)

```ts
{
  name: string;
  description: string;
  iconSvg: string;      // inline SVG string, rendered via set:html
  apkUrl: string;       // "#" when not available
  playStoreUrl: string; // "#" when not available
  appStoreUrl: string;  // "#" when not available
  comingSoon?: boolean;
}
```

## Accessibility & SEO

- Semantic landmarks: one `<header>`, `<main>`, `<footer>`.
- Icon-only links: add `title` / `aria-label`.
- Keep OG / Twitter / canonical meta in `index.astro`; mirror for new pages.

## Guardrails (do not)

- Add dependencies beyond `astro` without approval.
- Introduce Tailwind/UI libraries or convert CSS.
- Reformat unrelated files.
- Change `astro.config.mjs` `output`/`base` without confirming deployment.
- Commit secrets.

## Known placeholders / debt

- Most `playStoreUrl` / `appStoreUrl` are `"#"`; only Driver App has a real `apkUrl`.
- Typo: `Header.astro` nav "Downlaod" (should be "Download").
- `Footer.astro`: company link uses `rel="stylesheet"` (should be a normal link).

## Verify

```bash
npm run build   # must pass before finishing
npm run dev     # for interactive checks
```
