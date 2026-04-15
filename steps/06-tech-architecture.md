# Phase 6 — Technical Architecture

**Invoke:** Platform skill loaded based on product type detected in Phase 1 (see Step 0b).

## Purpose

Choose the right tools for the job. The technical stack must match the product type detected in Phase 1 — a cross-platform mobile app needs different infrastructure than a SaaS with multi-tenant databases, which is different again from a static marketing site. Over-engineering wastes time; under-engineering creates problems at scale. This phase produces two artifacts: an architecture decision record and an immutable technical constitution that Phase 9 must follow.

## Step 0 — Load prior context

Read `docs/context.md` and extract:
- **`product_type`** — determines which stack patterns apply
- **`target_platform`** — determines deployment targets and toolchain
- **Feature list** from Phase 3 — determines complexity of backend, auth, state
- **Audience scale** from Phase 2 — determines infrastructure sizing
- **Constraints** from Phase 1 — budget, timeline, team size, existing infrastructure

## Step 0b — Load platform skill conventions

The architecture decisions in this phase must align with the coding conventions Phase 9 will enforce. To ensure consistency, load the platform skill's conventions section BEFORE making stack decisions.

Based on `product_type` and `target_platform` from `docs/context.md`:

| Product type | Skill to load | What to read |
|---|---|---|
| Mobile (Flutter) | `code-flutter` | "Flutter coding conventions" section — architecture, state management, navigation, data patterns |
| Mobile (React Native) | `stack-mobile-rn` | Conventions and patterns |
| Mobile (iOS/Swift) | `stack-ios-swift` | Architecture patterns, SwiftUI conventions |
| Mobile (Android/Kotlin) | `stack-android-kotlin` | Compose patterns, Jetpack conventions |
| Web (frontend) | `stack-web-frontend` | React/Vue/Next.js conventions |
| Web (backend) / SaaS / API | `stack-web-backend` | NestJS/Express conventions |

Find the skill via Glob: `**/skills/[skill-name]/SKILL.md`. Read only the conventions/patterns section — don't load the full implementation workflow yet (that's for Phase 9).

**Why this matters:** If `code-flutter` says "use Riverpod for state management" but Phase 6 recommends BLoC in the Technical Constitution, Phase 9 will have a conflict. Loading the platform skill's conventions first prevents this drift.

If the platform skill is not installed, use your built-in knowledge and document the chosen conventions explicitly in the Technical Constitution (Step 6). Phase 9 will follow whatever the constitution says.

## Step 1 — Analyze technical needs

Map each requirement to a technical decision dimension:

| Dimension | Questions to answer |
|---|---|
| **Rendering strategy** | Static? SSR? CSR? Native? Which pages/screens need which? |
| **Data persistence** | Read-heavy (PostgreSQL)? Document model (MongoDB)? No backend (localStorage/AsyncStorage)? |
| **Auth complexity** | None? Simple email/password? OAuth? Multi-tenant SSO? |
| **Real-time needs** | None? WebSockets? Server-sent events? Push notifications? |
| **Payments** | None? One-time (Stripe/Lemon Squeezy)? Subscriptions? Marketplace? |
| **Content management** | Developer only? Marketing team? (→ CMS need) |
| **Traffic expectations** | Hobby? <10k/mo? >100k/mo? Viral spike risk? |
| **Compliance** | GDPR? HIPAA? PCI-DSS? |

## Step 2 — Recommend a stack

Choose the stack that fits the product type and scale. Recommend the primary option and one alternative. Never present more than 2 — too many choices stall the project.

### Stack patterns by product type

**Web (marketing site, landing page, portfolio)**
- Primary: Astro + Tailwind + Vercel/Netlify (zero JS default, excellent Core Web Vitals)
- Alternative: Next.js + Tailwind + Vercel (if blog, dynamic routes, or ISR needed)
- CMS: Contentful, Sanity, or Markdown files depending on update frequency

**Mobile — React Native / Expo**
- Primary: Expo (managed) + NativeWind + Expo Router + Supabase/Firebase
- Alternative: Expo (bare) + React Navigation if custom native modules needed
- State: Zustand (local) + TanStack Query (server state)
- Push: Expo Notifications → FCM/APNs

**Mobile — Flutter**
- Primary: Flutter + Riverpod + GoRouter + Supabase/Firebase
- Alternative: Flutter + BLoC if team knows it and project is complex
- Push: Firebase Messaging

**Mobile — Swift/SwiftUI (iOS)**
- Primary: SwiftUI + Combine/async-await + CoreData/SwiftData + CloudKit
- Alternative: UIKit if targeting iOS 14 or complex custom UI

**Mobile — Kotlin/Jetpack Compose (Android)**
- Primary: Jetpack Compose + ViewModel + Room + Retrofit + Hilt
- Alternative: XML Views if targeting API 21 with complex animations

**SaaS**
- Frontend: Next.js App Router + Tailwind + shadcn/ui + Vercel
- Backend: NestJS + PostgreSQL (Prisma) + Redis (caching/queues)
- Auth: Clerk or NextAuth.js (web) + Supabase Auth (if using Supabase)
- Multi-tenant: Row-level security (PostgreSQL RLS) or schema-per-tenant
- Payments: Stripe with webhooks + Stripe Customer Portal
- Email: Resend + React Email

**API / Backend service**
- Primary: NestJS + PostgreSQL (Prisma) + Redis + Docker + Railway/Render
- Alternative: Fastify (if performance-critical) or Express (if minimal surface)
- API spec: OpenAPI 3.x (auto-generated from decorators)
- Auth: JWT + refresh tokens OR API keys for service-to-service
- Queues: BullMQ (Redis-backed) for async jobs

**E-commerce**
- Primary: Next.js + Tailwind + Stripe + Vercel
- Alternative: Next.js + Shopify Storefront API (if large catalog)
- CMS: Sanity for product content + images
- Inventory: Simple (DB field) or Inventory management service

### Hosting cost benchmarks (monthly at launch scale)
| Option | Cost | Suitable for |
|---|---|---|
| Vercel/Netlify free tier | $0 | < 100GB bandwidth, personal projects |
| Vercel Pro | $20 | Production with team + preview URLs |
| Railway Starter | $5-20 | Node.js + PostgreSQL backend |
| Supabase free | $0 | Auth + DB for < 500MB |
| Supabase Pro | $25 | Production DB with daily backups |

## Step 3 — Define project structure

Produce the directory tree adapted to the chosen stack. For monorepos (SaaS with web + API), use the workspace structure. Keep it minimal — only directories that will actually be created in Phase 9.

Example for SaaS (Next.js + NestJS):
```
apps/
  web/            # Next.js frontend
    src/
      app/        # App Router pages
      components/ # Reusable UI
      lib/        # Utilities, API client
  api/            # NestJS backend
    src/
      modules/    # Feature modules
      common/     # Shared guards, decorators, pipes
packages/
  types/          # Shared TypeScript types
  ui/             # Shared component library (if needed)
```

## Step 4 — List all integrations

For each third-party integration, specify:
- Service name + plan tier
- What it replaces (don't reinvent what a service does well)
- SDK/package to use
- Environment variables it requires

## Step 5 — Adversarial review (mandatory)

After proposing the architecture, run a second pass that challenges every major decision. This is not optional — it catches problems before they compound.

For each major choice, ask and answer:

**Scalability challenge:**
> "What happens if traffic or users scale 10x in 3 months?"
Document the bottleneck and the migration path (not the solution — that would be premature optimization).

**Alternative challenge:**
> "Why this stack instead of [most obvious alternative]?"
If you can't articulate a clear reason, reconsider the choice.

**Risk challenge:**
> "What are the top 3 risks of this choice?"
Document each risk with: likelihood (low/medium/high), impact (low/medium/high), mitigation.

**Dependency challenge:**
> "What third-party services is this product critically dependent on? What's the fallback if one goes down?"
Document single points of failure.

Document all answers as **Architecture Decision Records (ADRs)** in `docs/06-architecture.md`:

```markdown
## ADR-001: [Title]

**Status:** Accepted
**Context:** [Why this decision was needed]
**Decision:** [What was chosen]
**Alternatives considered:** [What else was evaluated and why rejected]
**Consequences:** [Trade-offs, risks, known limitations]
**Scalability note:** [What breaks at 10x and the migration path]
```

## Step 6 — Generate the Technical Constitution

The Technical Constitution is an immutable reference file that Phase 9 loads as non-negotiable context. Every agent, every developer, every code review must comply with it. It prevents the drift that happens when different parts of a codebase use different patterns for the same problem.

Save as `docs/06-tech-rules.md`:

```markdown
# Technical Constitution — [Project Name]

> This file is immutable context for Phase 9 (Code). Every spec, every PR, every
> code review must comply. Deviations require an explicit ADR in docs/06-architecture.md.

## Stack (locked)

| Layer | Technology | Version | Notes |
|---|---|---|---|
| [layer] | [tech] | [x.y.z] | [pinned reason if any] |

## Critical implementation rules

Rules are grouped by severity:

### MUST (non-negotiable)
- All HTTP calls go through `src/lib/apiClient.ts` (singleton with auth headers, error normalization, retry logic). No raw `fetch` calls outside this file.
- All DB writes happen inside transactions. No bare `prisma.model.create()` — use `prisma.$transaction()`.
- All state via [chosen state manager]. No component-level useState for shared state.
- Auth guards on every protected route — no route is implicitly protected.
- Environment variables through `src/config/env.ts` with Zod validation. No `process.env.X` in business logic.
- [Add product-specific MUST rules]

### SHOULD (strong preference, document exceptions)
- Server components by default in Next.js — Client components only when interactive.
- Optimistic updates for mutations that affect visible UI.
- All user-facing strings in `src/i18n/` — even if single-language at launch.
- [Add product-specific SHOULD rules]

### MUST NOT (explicitly forbidden)
- No `any` TypeScript types in production code.
- No direct DB access from frontend code.
- No secrets in client-side code or git history.
- No business logic in UI components.
- [Add product-specific MUST NOT rules]

## Directory conventions

[Short rules for where things live — one rule per ambiguous case]

## Naming conventions

[File naming, component naming, API route naming — whatever matters for this stack]

## Error handling contract

[How errors are handled, what shape they take, where they are caught]

## Testing requirements

[Minimum coverage, which layers need tests, what mocking strategy]
```

Adapt the rules to the product type. Mobile apps have different patterns (navigation guards, AsyncStorage vs SecureStore, gesture handling) than SaaS backends (transaction patterns, queue job naming, webhook idempotency). The constitution should reflect the actual patterns of the chosen stack.

## Step 7 — Generate linter config mirroring the constitution

The Technical Constitution is enforced by convention. But conventions can be bypassed — a tired developer at 2am will write `print()` instead of `logger.d()`. Machine-enforced rules catch what convention doesn't.

After generating `docs/06-tech-rules.md`, create a linter config file that mirrors the MUST/MUST NOT rules:

| Stack | Linter config file | Tool |
|-------|-------------------|------|
| Flutter/Dart | `analysis_options.yaml` | `dart analyze` |
| TypeScript (Next.js, NestJS) | `.eslintrc.json` or `eslint.config.mjs` | ESLint |
| Swift | `.swiftlint.yml` | SwiftLint |
| Kotlin | `detekt.yml` or `.editorconfig` | detekt / ktlint |
| React Native | `.eslintrc.json` | ESLint |

### What to enforce in the linter

Map each MUST NOT from the constitution to a linter rule:

**Flutter example:**
```yaml
# analysis_options.yaml — mirrors docs/06-tech-rules.md
analyzer:
  errors:
    avoid_print: error          # MUST NOT: no print() in production
    avoid_dynamic_calls: error  # MUST NOT: no dynamic types
  language:
    strict-casts: true
    strict-raw-types: true

linter:
  rules:
    avoid_print: true
    prefer_const_constructors: true
    prefer_single_quotes: true
    always_use_package_imports: true
    avoid_relative_lib_imports: true
    prefer_final_locals: true
    unnecessary_late: true
```

**TypeScript example:**
```json
{
  "rules": {
    "no-console": "error",
    "@typescript-eslint/no-explicit-any": "error",
    "no-restricted-imports": ["error", { "patterns": ["../../../*"] }]
  }
}
```

The linter config doesn't replace the constitution — it enforces the subset that can be machine-checked. The remaining rules (architecture patterns, naming conventions, error handling) are enforced by code review in Phase 9 Step 3.

## Deliverable

Save `docs/06-architecture.md` with: stack choice + rationale, project structure, integrations list with environment variables, deployment plan, cost estimate, and all ADRs from the adversarial review.

Save `docs/06-tech-rules.md` as the Technical Constitution. This file is loaded as immutable context by Phase 9.

Generate the linter config file at the project root (e.g., `analysis_options.yaml`, `.eslintrc.json`) mirroring the MUST NOT rules.

Update `docs/context.md` with: chosen stack, hosting, key integrations, and the path to the Technical Constitution.

## Gate — Critères de passage vers Phase 7

### Input contract
- [ ] `docs/context.md` contient `product_type` et `target_platform` (Phase 1)
- [ ] `docs/03-product-strategy.md` existe et liste les features/pages/screens (Phase 3)

### Output contract

#### Automatiques (Claude vérifie)
- [ ] `docs/06-architecture.md` existe et contient : stack choisie, structure du projet, intégrations avec variables d'env, plan de déploiement, estimé de coût, au moins 2 ADRs
- [ ] `docs/06-tech-rules.md` existe et contient : tableau de stack avec versions, section MUST, section MUST NOT
- [ ] Linter config file exists at project root (e.g., `analysis_options.yaml`, `.eslintrc.json`) mirroring MUST NOT rules
- [ ] `docs/context.md` contient la section "Phase 6 — Technical Architecture"
- [ ] L'adversarial review a été conduite (scalabilité 10x, alternatives, risques documentés)

#### Validation utilisateur (bloquant)
- [ ] L'utilisateur a validé le choix de stack et le budget estimé
- [ ] L'utilisateur a lu et accepté les règles critiques de la Technical Constitution

### Verdict: PASS | WARN | FAIL
- **PASS**: Tous les critères remplis
- **WARN**: Stack validée, constitution incomplète sur certains patterns — documenter les manques et compléter en début de Phase 9
- **FAIL**: Stack non validée OU constitution absente — bloquer avant de continuer

## Checkpoint

Present the architecture and Technical Constitution. When validated: **"Phase 6 complete. Moving to Phase 7 — Product Architecture."** Then read `steps/07-product-architecture.md`.
