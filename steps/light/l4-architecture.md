# Light Mode — Phase L4: Architecture

**Mode:** Light (`--light`)
**Merges:** Full Mode Phase 6 (Tech Architecture) + Phase 7 (Product Architecture)
**Input required:** `docs/02-positioning-strategy.md` + `docs/05-copy/` (from L2 and L3)
**Output:** `docs/06-architecture.md` + `docs/06-tech-rules.md` + `docs/07-product-architecture.md`

---

## Purpose

Choose the stack, define the technical rules, and map the product structure in one pass. No ADRs, no adversarial review, no multi-option evaluation. One opinionated decision per choice, with a brief rationale.

---

## Step 1 — Stack choice

Read `docs/02-positioning-strategy.md` (product type, screens, user flows) and `docs/context.md` (product type, target platform).

Select the stack using these defaults. Override only if the user has stated a specific constraint or preference.

**Web / Marketing site:**
- Framework: Astro (static) or Next.js (dynamic / SSR needed)
- Styling: Tailwind CSS
- CMS: Contentlayer (Astro) or Sanity (if non-technical editors will manage content)
- Deployment: Vercel

**SaaS web app:**
- Frontend: Next.js (App Router) + Tailwind CSS
- Backend: Next.js API routes (simple) or NestJS (complex business logic)
- Database: PostgreSQL via Prisma ORM
- Auth: NextAuth.js / Auth.js or Clerk
- Deployment: Vercel (frontend + API routes) or Vercel + Railway (separate NestJS backend)

**Mobile app:**
- Framework: React Native + Expo
- Navigation: Expo Router (file-based)
- State: Zustand (simple) or React Query + Zustand (server state + client state)
- Backend: Supabase (fast) or NestJS + PostgreSQL (custom)
- Distribution: EAS Build + EAS Submit

**API / Backend service:**
- Framework: NestJS + TypeScript
- Database: PostgreSQL via Prisma ORM (relational) or MongoDB (document)
- Auth: JWT + Passport.js or API keys
- Deployment: Railway or Fly.io

**E-commerce:**
- Framework: Next.js + Tailwind CSS
- Payments: Stripe
- Database: PostgreSQL via Prisma ORM
- Deployment: Vercel

State the chosen stack with a 1-sentence rationale for each technology. No multi-option comparison tables.

---

## Step 2 — Generate `docs/06-tech-rules.md`

Write 5–10 rules that the codebase must follow. These are non-negotiable constraints for the coding phase. Keep them short and actionable.

Rules must cover:
1. **File/folder structure** — Where things live (e.g., "All API routes go in `src/app/api/`; no logic in route handlers — extract to service files")
2. **Component rules** (if frontend) — Component size limit, prop conventions, state management boundary
3. **Data validation** — Where validation happens (e.g., "Validate at the API boundary using Zod; never trust client-sent data")
4. **Error handling** — How errors propagate (e.g., "All async functions return `Result<T, AppError>`; never throw in business logic")
5. **Environment secrets** — How env vars are managed (e.g., "All secrets in `.env.local` (never committed); access via `process.env` with a typed config object")
6. **Testing boundary** — What gets tested (e.g., "Unit test business logic; integration test API routes; skip snapshot tests")
7. **Dependency management** — When to add a package (e.g., "Add a dependency only if it solves a problem you can't solve in < 20 lines")
8. **Naming conventions** — File names, component names, variable naming style

Add 1–2 product-specific rules if the product type requires them (e.g., "All Stripe webhook handlers must verify the signature before processing" for e-commerce).

Format:

```markdown
# Tech Rules

## Rule 1 — [Category]
[Rule statement in 1-2 sentences]

## Rule 2 — [Category]
[Rule statement in 1-2 sentences]

...
```

---

## Step 3 — Screen / page flow and navigation structure

Read the screen/page list from `docs/02-positioning-strategy.md`. For each screen/page, define:

1. **Entry points** — How does the user reach this screen/page?
2. **Exit points** — Where can the user go from here?
3. **Auth requirement** — Public, authenticated only, or conditional

Draw the navigation structure as a text diagram:

**Web example:**
```
/ (Homepage)
├── /pricing
├── /about
├── /blog
│   └── /blog/[slug]
├── /signup
├── /login
└── /dashboard (auth required)
    ├── /dashboard/settings
    └── /dashboard/[feature]
```

**Mobile example:**
```
(Onboarding flow — unauthenticated)
Splash → ValueProp1 → ValueProp2 → SignUp / Login

(Main app — authenticated)
Tab 1: Home
  └── DetailScreen
Tab 2: [Feature]
  └── ActionScreen
    └── ConfirmationScreen
Tab 3: Profile
  └── Settings
```

---

## Step 4 — URL / route patterns

Define the URL or route naming conventions:

**Web:**
```
/[resource]                    → resource index (e.g., /projects)
/[resource]/[id]               → resource detail (e.g., /projects/123)
/[resource]/[id]/[action]      → resource action (e.g., /projects/123/edit)
/api/[resource]                → API route (e.g., /api/projects)
/api/[resource]/[id]           → API resource endpoint
```

**Mobile (Expo Router):**
```
/(tabs)/index                  → Tab 1 (Home)
/(tabs)/[feature]              → Tab 2
/(tabs)/profile                → Tab 3
/(auth)/login                  → Login screen (outside tabs)
/(auth)/signup                 → Signup screen
/[resource]/[id]               → Detail screens
```

Adjust patterns to fit the specific screens/pages identified in Step 3.

---

## Step 5 — Save outputs

### `docs/06-architecture.md`

```markdown
# Technical Architecture

## Stack

### Frontend / App
[Stack choices with rationale]

### Backend / API (if applicable)
[Stack choices with rationale]

### Database
[Choice with rationale]

### Auth
[Choice with rationale]

### Deployment
[Choice with rationale]

## Navigation structure
[Text diagram from Step 3]

## URL / Route patterns
[From Step 4]
```

### `docs/06-tech-rules.md`

[Content from Step 2 — 5–10 rules]

### `docs/07-product-architecture.md`

```markdown
# Product Architecture

## Screen / Page inventory

| Screen / Page | Entry points | Exit points | Auth |
|---|---|---|---|
[One row per screen/page]

## Core user flows (mapped to screens)

Flow 1: [Name]
[Step = Screen name → Step = Screen name → Outcome]

Flow 2: [Name]
[...]

Flow 3: [Name]
[...]
```

### Update `docs/context.md`:

```markdown
## Phase L4 — Architecture
Stack: [1-line summary]
Screens mapped: [count]
Tech rules: [count] rules in docs/06-tech-rules.md
```

---

## Gate — PASS / FAIL

### PASS
- Stack chosen for all layers (frontend, backend if applicable, database if applicable, auth if applicable, deployment)
- `docs/06-tech-rules.md` contains 5–10 rules covering the required categories
- Every screen/page from `docs/02-positioning-strategy.md` has entry/exit/auth defined
- URL / route patterns documented
- Core user flows mapped to actual screens

```
GATE L4: PASS
→ Proceed to UI Design phase
```

### FAIL
State which criterion failed.

```
GATE L4: FAIL
Issue: [specific criterion that failed]
Required fix: [what must change]
```

---

## Trade-offs vs Full mode

| Full mode | Light mode |
|---|---|
| ADRs (Architecture Decision Records) for major choices | No ADRs — one opinionated choice per layer |
| Adversarial review of tech architecture | No adversarial review |
| Full `docs/06-tech-rules.md` (no limit) | 5–10 rules max — focused on the highest-impact constraints |
| Tech architecture and product architecture in separate phases | One combined pass |
| PASS / WARN / FAIL gate | PASS / FAIL only |

If the product involves unusual architectural risk (high concurrency, complex multi-tenant data isolation, regulatory constraints), Full mode is recommended. Light mode assumes a standard web or mobile product with conventional architecture needs.
