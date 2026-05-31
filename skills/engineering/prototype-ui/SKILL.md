---
name: prototype-ui
description: Build 3 radically different UI implementations for a feature as isolated dev routes/views, iterate with user feedback, then promote the winner. Use when building new UI, modifying existing views/pages/components, prototyping interfaces, exploring design options, or user says "prototype".
---

# Prototype UI

Build 3 divergent UI prototypes as isolated dev routes, gather feedback, converge on a winner. Works across SPA (React, Vue, Svelte, Solid), SSR (Next.js, Remix, Rails, Django, Phoenix), and non-framework (static HTML/CSS/JS) projects.

## Constraints

- **DO NOT** modify existing production views, pages, or components outside the prototype sandbox
- All data must be mocked inline (constants, fixture files, in-memory objects) — no real DB queries or live API calls
- Use only existing component primitives and theme tokens (CSS variables, design system) already defined in the project
- Consult a `building-components` skill (if present) for component API patterns
- Consult a `user-interface` skill (if present) for accessibility and interaction standards
- Run the project's standard check command (lint + typecheck + tests, e.g. `npm run check`, `mix test`, `pnpm lint`, `make ci`) before presenting prototypes

## Workflow

### 1. Understand the feature

Ask the user:
- What feature or page are they building?
- What data/entities are involved?
- Any hard constraints (mobile-first, specific components, accessibility targets, etc.)?

### 2. Study existing patterns

Before writing any code, identify and read:
- The directory containing existing views/pages/routes — for structure and conventions
- The shared component library / design system entry point — for available primitives and layout helpers
- The routing configuration (router file, file-based routing convention, or static page index) — to understand how new routes are added
- Any existing "dev", "playground", "sandbox", or "kitchen sink" area where in-progress UI lives; create one if none exists

### 3. Build 3 prototypes in parallel

Use 3 parallel sub-agents (one per variant). Each agent creates:

**A view/page module** at a sandbox path appropriate to the project:
- SPA with file-based routing: `src/routes/dev/{feature}-{variant}/...` or `src/pages/dev/{feature}-{variant}.tsx`
- SPA with explicit router: a new component plus a route entry guarded by a dev flag
- SSR framework: a route handler + template under a `dev/` or equivalent prefix
- Non-framework: a standalone HTML file under `prototypes/{feature}-{variant}/`

**A route/link entry** wired up so the prototype is reachable, guarded behind a dev-only condition (env var, build flag, or simply unlinked-but-reachable in prod-disabled paths).

Each prototype **MUST** be radically different. Vary along these axes:
- Card grid vs table vs timeline/feed
- Minimal vs dense vs progressive-disclosure
- Action-first vs browse-first vs search-first

### 4. Verify

Run the project's check command to ensure all prototypes build, typecheck, lint, and pass tests.

### 5. Present and iterate

Tell the user the 3 prototypes are ready with their URLs (or file paths, for static prototypes). Ask:
- Which prototype resonates most?
- What works well in each?
- What feels wrong or missing?

Iterate on the preferred prototype based on feedback. Keep asking until satisfied.

### 6. Promote the winner

Once the user picks a final prototype:
1. Delete the rejected prototype files
2. Remove their routes / link entries from the router or nav config
3. Remove any nav items, fixtures, or dev-only registrations that pointed to them
4. Keep the winner in the dev sandbox for reference, or move it to its production location if requested
5. Re-run the project's check command to verify cleanup left no dangling imports or broken routes
