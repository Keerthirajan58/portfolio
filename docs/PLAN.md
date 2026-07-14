# Portfolio Rebuild Plan

Goal: replace the existing Bootstrap/HTML/CSS/JS portfolio with a fluid, motion-forward, modern portfolio for an early-career AI/ML software engineer, deployed free on Vercel.

Treat each phase below as its own Plan Mode session — propose the concrete steps for that phase, get a go-ahead, then execute. Don't try to build the whole site in one shot.

## Phase 0 — Backup & Discovery
- Confirm git is initialized and the current site is committed; create a working branch (e.g. `redesign`).
- Invoke the `content-extractor` subagent to read the existing site and produce `content/data.ts` (or `.json`) with: name, headline/tagline, bio, work experience (company, title, dates, bullets), education, skills/tools, projects (title, description, tech, links, images), contact/social links, resume file location.
- No design work yet.

## Phase 1 — Scaffold
- Create the Next.js app (TypeScript, App Router, Tailwind, ESLint) — either in place after moving the old site into `/legacy`, or in a fresh directory that later replaces root.
- Wire up Prettier + ESLint. Verify `npm run dev` runs with placeholder content.

## Phase 2 — Design System
- Follow the "Design principles" section in `CLAUDE.md`: brainstorm 2–3 short design directions (palette, type pairing, layout concept, one signature element) grounded in "early-career AI/ML engineer, job-seeking" — not generic templates.
- Present the directions to the user with a brief description of each (or a quick mockup) and get a pick before building further.
- Lock the chosen tokens into the Tailwind config / a `theme.ts`.

## Phase 3 — Core Build (static, no motion yet)
- Sections: Hero, About, Experience (timeline), Projects (grid or case studies), Skills, Contact/Footer (resume download, email, GitHub, LinkedIn).
- Fully responsive, real content from Phase 0, semantic HTML, accessible landmarks and heading structure.

## Phase 4 — Motion Layer
- Lenis for smooth scroll.
- Motion (`motion/react`) for micro-interactions, hover/tap states, hero load-in reveal.
- GSAP + ScrollTrigger for scroll-driven storytelling (pinned sections, staggered reveals) — used deliberately, not on every element.
- Optional: one ambient WebGL/Three.js moment in the hero (via React Three Fiber), lazy-loaded, with a static/reduced-motion fallback.
- Invoke the `motion-qa` subagent to check reduced-motion fallbacks, performance on a throttled CPU, and mobile behavior before moving on.

## Phase 5 — Content, SEO & Accessibility Polish
- Meta tags, Open Graph image, favicon, sitemap.xml, robots.txt.
- Run Lighthouse; fix anything under 90 on mobile performance/accessibility.
- Proofread all real content once more against the original site.

## Phase 6 — Deployment
- Invoke the `deploy-agent` subagent: push to GitHub, connect the repo to Vercel (free Hobby tier — appropriate since this is a personal, non-commercial portfolio), verify the production build, set up a custom domain if the user has one.
- Keep the old Netlify deployment live until the new site is verified and any DNS switch is confirmed.

## Phase 7 — Handoff
- Update the Commands section of `CLAUDE.md` with the real scripts.
- Write a short `README.md`: stack, how to run locally, how to deploy.
