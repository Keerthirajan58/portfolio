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
_Fill this in once Phase 1 (scaffold) is done — then keep it updated._
- `npm run dev` — local dev server
- `npm run build` — production build
- `npm run lint` — lint
- `npm run typecheck` — TypeScript check (add the script if it's missing)

## Design principles — read before building any UI
- Avoid the generic "AI-generated portfolio" defaults: (a) cream background + serif display + terracotta accent, (b) near-black background + one neon accent, (c) newspaper-style broadsheet grid with hairline rules. Pick a direction that's actually specific to an AI/SWE/MLE portfolio, not a template.
- Work in two passes: first brainstorm a compact design-token system — 4–6 named hex colors, 2 paired typefaces (display + body), a layout concept, and ONE signature element — then critique that plan against the generic defaults above *before* writing any code. Present 2–3 short directions to the user and get a pick before locking tokens.
- Spend boldness in exactly one place (the signature element). Keep everything around it quiet and disciplined.
- Motion must be deliberate, never decorative for its own sake. One well-orchestrated moment (hero load-in, or a scroll story) beats scattered effects everywhere.
- Non-negotiable quality floor: responsive down to ~360px, visible keyboard focus states, `prefers-reduced-motion` respected everywhere (GSAP, Motion, and any WebGL scene need a static/reduced fallback), Lighthouse performance ≥ 90 on mobile.
- Any Three.js/WebGL content must lazy-load, cap particle/poly counts for low-end devices, and never delay first paint of text content — a recruiter skimming on a phone matters more than a fancy shader.

## Workflow rules
- Use Plan Mode for any phase touching more than 2–3 files. Don't jump straight to full implementation.
- Commit after every working milestone (scaffold, tokens locked, each section built, motion layer added, deploy) — small commits, not one giant one.
- Don't deploy or push to `main` without the user's explicit go-ahead.
- Delegate to the subagents in `.claude/agents/` for the tasks they own (content extraction, motion/accessibility QA, deployment) instead of doing that work in the main thread — keeps the main context focused on design and implementation.
