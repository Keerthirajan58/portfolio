# Portfolio Rebuild — Project Memory

## What this is

A personal portfolio for an early-career AI/ML Software Engineer & ML Engineer who is actively job hunting. We are replacing a 5-year-old Bootstrap/HTML/CSS/JS site with a modern, fluid, motion-forward portfolio. Full tech stack replacement is authorized. Only the **content** (bio, experience, education, skills, projects, contact info) must be preserved — none of the old code needs to survive.

Read `docs/PLAN.md` for the phased roadmap before starting work each session.

## Source of truth for content

The old site (present in this repo pre-rebuild) is the ONLY source for facts: job titles, dates, degrees, project descriptions, links, resume file. Never invent, embellish, or "improve" experience. If something is ambiguous or missing, ask the user — don't guess. See the `content-extractor` subagent for how this gets pulled out.

## Tech stack (locked decisions — don't relitigate mid-project)

- Framework: Next.js (App Router) + TypeScript
- Styling: Tailwind CSS
- UI motion: Motion for React (`motion/react` — this is the renamed Framer Motion; do not install the old `framer-motion` package)
- Scroll storytelling: GSAP + ScrollTrigger, plus Lenis for smooth/inertia scrolling
- Optional ambient 3D: Three.js via React, React Three Fiber + drei — only where it earns its keep (see Design principles)
- Deployment target: Vercel, free Hobby tier
- Package manager: npm

## Commands

- `npm run dev` — local dev server
- `npm run build` — production build
- `npm run start` — run the production build locally
- `npm run lint` — ESLint (flat config, Next.js core-web-vitals + TS rules, Prettier conflicts disabled)
- `npm run typecheck` — TypeScript check (`tsc --noEmit`)
- `npm run format` — Prettier, writes in place (`legacy/`, `node_modules/`, `.next/`, `public/` excluded via `.prettierignore`)

Stack notes for future sessions: App Router lives in `src/app`, real content lives in `src/content/data.ts` (see its header comment for provenance), Tailwind is v4 (`@tailwindcss/postcss`, tokens go in `globals.css` via `@theme`, not a `tailwind.config.ts`). The old Bootstrap site is preserved untouched under `/legacy` for reference — nothing in it is imported by the new app.

## Design principles — read before building any UI

- Avoid the generic "AI-generated portfolio" defaults: (a) cream background + serif display + terracotta accent, (b) near-black background + one neon accent, (c) newspaper-style broadsheet grid with hairline rules. Pick a direction that's actually specific to an AI/SWE/MLE portfolio, not a template.
- Work in two passes: first brainstorm a compact design-token system — 4–6 named hex colors, 2 paired typefaces (display + body), a layout concept, and ONE signature element — then critique that plan against the generic defaults above _before_ writing any code. Present 2–3 short directions to the user and get a pick before locking tokens.
- Spend boldness in exactly one place (the signature element). Keep everything around it quiet and disciplined.
- Motion must be deliberate, never decorative for its own sake. One well-orchestrated moment (hero load-in, or a scroll story) beats scattered effects everywhere.
- Non-negotiable quality floor: responsive down to ~360px, visible keyboard focus states, `prefers-reduced-motion` respected everywhere (GSAP, Motion, and any WebGL scene need a static/reduced fallback), Lighthouse performance ≥ 90 on mobile.
- Any Three.js/WebGL content must lazy-load, cap particle/poly counts for low-end devices, and never delay first paint of text content — a recruiter skimming on a phone matters more than a fancy shader.

## Locked design direction — "Latent Space" (red/white)

Chosen from 3 brainstormed directions (Phase 2), then refined twice by the owner. Concept: a vector-embedding-space aesthetic — ties directly to the embeddings/semantic-search work in his projects — expressed as a light, red-accented palette with one interactive signature element.

- **Palette** (`src/app/globals.css` `@theme`, mirrored in `src/lib/theme.ts` for JS/canvas use): `paper #FDFCFC` (bg), `surface #F6EEEE` (panels), `ink #171313` (text), `ink-muted #6E6467`, `crimson #DF212F` (primary accent — nudged from the original #E23744 in Phase 5 to clear WCAG AA text contrast, barely perceptible), `wine #7A1F2B` (secondary accent). Fixed light theme — no dark-mode toggle.
- **Type**: display = Unbounded (variable, bold/geometric — chosen over Instrument Sans for a more premium/distinctive feel), body = Inter (variable). Both via `next/font/google`.
- **Layout concept**: clustered/organic rather than a strict grid — content in loosely grouped blocks, project cards arranged like scatterplot clusters.
- **Signature element** (built in Phase 4): an interactive node-scatter in the hero. Owner explicitly upgraded this from the original "SVG or Canvas, no WebGL" note to **WebGL via React Three Fiber** (`src/components/NodeCluster/`). Progressive-upgrade chain: static SVG renders first (SSR/no-JS/reduced-motion/Save-Data/no-WebGL all stay on it); the three.js scene lazy-loads in an async chunk after `requestIdleCallback`; frameloop pauses offscreen/hidden-tab; a lost GL context demotes back to SVG permanently. Light background → soft alpha-blended crimson/wine discs, never additive blending (washes out on paper). Labels are crisp HTML overlaid on the canvas, never 3D text.
- Explicitly avoids the generic pitfalls: not cream (crisp white/rose-white instead), not serif, not a flat single neon accent (two considered reds), not a broadsheet grid.

## Workflow rules

- Use Plan Mode for any phase touching more than 2–3 files. Don't jump straight to full implementation.
- Commit after every working milestone (scaffold, tokens locked, each section built, motion layer added, deploy) — small commits, not one giant one.
- Don't deploy or push to `main` without the user's explicit go-ahead.
- Delegate to the subagents in `.claude/agents/` for the tasks they own (content extraction, motion/accessibility QA, deployment) instead of doing that work in the main thread — keeps the main context focused on design and implementation.
