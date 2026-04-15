# Phase 7 — Product Architecture

**Invoke:** `site-architecture` skill (for web products) — built-in expertise for mobile/API/SaaS.

## Purpose

Product architecture is about how the pieces connect — screens, routes, navigation, deep links, API resources, webhooks. Good architecture makes every feature discoverable and every entry point intentional. Bad architecture makes users get lost, developers create duplicate routes, and search engines fail to index the right content.

This phase is the bridge between what exists (content strategy, features, copy) and what gets designed and built. Its output — the navigation/flow map — is the exact input Phase 8 needs to design from. Phase 9 uses it to know what files to create.

This phase adapts entirely to the product type detected in Phase 1.

## Step 0 — Load prior context

Read `docs/context.md` and extract:
- **`product_type`** — determines which architecture pattern to apply
- **`target_platform`** — determines navigation paradigm (web routing vs. native navigation)
- **Page/screen/endpoint list** from `docs/03-product-strategy.md`
- **Tech stack** from `docs/06-architecture.md` — routing constraints (App Router vs Pages Router, Expo Router vs React Navigation, etc.)

---

## Product type: Web (marketing site, blog, portfolio, landing page)

### Web — Architecture process

1. Read the `site-architecture` skill's SKILL.md (sibling directory). Follow its process, feeding it:
   - Page list and each page's purpose (Phase 3)
   - Tech stack and routing constraints (Phase 6)
   - SEO priorities

2. The skill produces:
   - **Page hierarchy** — which pages are top-level, which are nested
   - **Navigation design** — header items, footer structure
   - **URL patterns** — clean, SEO-friendly, consistent
   - **Internal linking strategy** — which pages link to which
   - **Sitemap plan** — for `sitemap.xml`

3. Document as `docs/07-product-architecture.md` (see Output Format below).

---

## Product type: Mobile app (React Native / Flutter / Swift / Kotlin)

### Mobile — Architecture process

1. **Map screen inventory** — list every screen from Phase 3 and classify each:
   - **Entry points**: Splash, Onboarding, Login, Register, Forgot Password
   - **Tab root screens**: Home/Feed, Search/Discover, Create/Add, Notifications, Profile
   - **Child screens**: Detail views, settings sub-screens, flows
   - **Modal screens**: Overlays, action sheets, full-screen modals
   - **Auth-gated screens**: Which screens require authentication?

2. **Design the navigation structure** — choose and justify the navigation paradigm:
   - **Tab + Stack** (most common): Bottom tabs for main sections, stack navigator within each
   - **Drawer + Stack**: Side drawer for infrequent sections + stacks
   - **Stack-only**: Linear flows (onboarding, checkout, settings)
   - **Mixed**: Tab at root with modal stacks for creation flows
   
   Document the hierarchy:
   ```
   Root Navigator
   ├── Auth Stack (unauthenticated)
   │   ├── Splash
   │   ├── Onboarding (swipeable)
   │   ├── Login
   │   └── Register
   └── Main Tab Navigator (authenticated)
       ├── Home Stack
       │   ├── HomeScreen
       │   └── DetailScreen
       ├── Search Stack
       │   └── SearchScreen
       └── Profile Stack
           ├── ProfileScreen
           └── SettingsScreen
   ```

3. **Define the deep link schema** — every deep-linkable screen needs a URI:
   ```
   [appscheme]://
   ├── home
   ├── item/:id
   ├── profile/:userId
   ├── notifications
   └── settings/notifications
   ```
   
   Also define Universal Links (iOS) / App Links (Android) if the product has a web counterpart.

4. **Map push notification triggers** — for each notification type, document:
   - Trigger condition (what causes it)
   - Target screen (which screen opens on tap)
   - Deep link URI to that screen
   - Required notification payload fields

5. **Document gesture and transition patterns** — per navigator:
   - Default transition (slide, modal, fade)
   - Custom back gesture behaviors (swipe-to-dismiss on modals)
   - Shared element transitions if any

6. **Platform-specific notes** (if cross-platform):
   - iOS-specific: Tab bar style, navigation bar, safe area behavior
   - Android: Back button handling, material transitions, edge-to-edge

---

## Product type: SaaS

### SaaS — Architecture process

1. **Map features by user role** — create a role × feature matrix:
   ```
   Feature                | Free | Pro | Admin | Super Admin
   ─────────────────────────────────────────────────────────
   Dashboard overview     |  ✓  |  ✓  |  ✓   |    ✓
   Project creation       |  3  |  ∞  |  ∞   |    ∞
   Team members           |  ✗  |  ✓  |  ✓   |    ✓
   Billing management     |  ✗  |  ✓  |  ✗   |    ✓
   Admin panel            |  ✗  |  ✗  |  ✓   |    ✓
   ```

2. **Design the navigation structure** — web app navigation:
   - **Sidebar** (preferred for feature-rich SaaS): always visible, collapsible on mobile
   - **Topbar** (preferred for focused tools): breadcrumb + action area
   - **Hybrid**: Sidebar for sections + topbar for context + breadcrumbs
   
   Document sidebar/topbar items per role.

3. **Define the URL structure** for the web app:
   ```
   /                          → Marketing homepage (Phase 3)
   /login                     → Auth
   /register                  → Auth
   /app/                      → App root (auth-gated)
   /app/dashboard             → Main overview
   /app/[resource]/           → Resource listing
   /app/[resource]/[id]       → Resource detail
   /app/[resource]/[id]/edit  → Edit form
   /app/settings/             → Settings root
   /app/settings/billing      → Billing
   /app/settings/team         → Team management
   /admin/                    → Admin panel (role-gated)
   ```

4. **Define the webhook schema** (if the product emits webhooks):
   - List all webhook event types: `[resource].[action]` (e.g., `subscription.created`, `invoice.paid`)
   - Payload envelope: `{ event, version, timestamp, data }`
   - Retry policy and delivery guarantees

5. **Multi-tenant considerations**:
   - How is tenant context carried? (subdomain? URL param? JWT claim?)
   - How are tenant boundaries enforced at the route level?

---

## Product type: API / Backend service

### API — Architecture process

1. **Define the resource hierarchy** — organize resources logically:
   ```
   /api/v1/
   ├── auth/
   │   ├── POST   /register
   │   ├── POST   /login
   │   └── POST   /refresh
   ├── users/
   │   ├── GET    /                → List (admin only)
   │   ├── GET    /:id             → Get by ID
   │   ├── PATCH  /:id             → Update
   │   └── DELETE /:id            → Delete
   ├── [resource]/
   │   ├── GET    /               → List (with pagination, filters)
   │   ├── POST   /               → Create
   │   ├── GET    /:id            → Get by ID
   │   ├── PATCH  /:id            → Update
   │   └── DELETE /:id           → Delete
   └── webhooks/
       └── POST   /[provider]     → Incoming webhooks
   ```

2. **Define the versioning strategy**:
   - URL versioning (`/api/v1/`) — recommended for external APIs
   - Header versioning (`Accept: application/vnd.api.v1+json`) — for internal APIs
   - Deprecation policy: How long before a version is sunset? How are consumers notified?

3. **Document the auth flow**:
   - For REST APIs: JWT access token (15min) + refresh token (30 days, httpOnly cookie)
   - For service-to-service: API key with scopes + rate limiting per key
   - Draw the token lifecycle: issue → use → refresh → revoke

4. **Define pagination, filtering, and sorting conventions**:
   - Cursor-based (preferred for large datasets) or offset-based
   - Filter syntax: `?filter[status]=active&filter[role]=admin`
   - Sort syntax: `?sort=-createdAt,name`

5. **Document rate limiting and error envelope**:
   ```json
   {
     "error": {
       "code": "VALIDATION_ERROR",
       "message": "Human-readable message",
       "details": [{ "field": "email", "message": "Invalid format" }],
       "requestId": "req_xxxx"
     }
   }
   ```

---

## Output format (all product types)

Save `docs/07-product-architecture.md` with these sections, adapted to the product type:

```markdown
# Product Architecture — [Project Name]

## Product type: [web | mobile | saas | api]
## Target platform: [web | ios | android | cross-platform | backend]

## Navigation / Structure overview
[The top-level structure — page hierarchy, screen hierarchy, API resource tree]

## Detailed map
[Product-type-specific detail: full URL map, full screen tree, full endpoint map]

## Entry points
[Every way a user can enter the product: homepage URL, app store link, deep link, API endpoint, webhook]

## Auth boundaries
[Which pages/screens/endpoints require authentication and at what role level]

## [Product-specific section]
For web: Internal linking strategy + sitemap plan
For mobile: Deep link schema + push notification trigger map
For SaaS: Role × feature matrix + webhook schema
For API: Versioning strategy + error envelope + rate limiting

## Navigation components
[What components implement the navigation: header nav, footer, sidebar, bottom tab bar, drawer, breadcrumbs]

## Phase 8 design targets
[Ordered list of screens/pages to design, with each one's primary job — this is the direct input to Phase 8]
```

Update `docs/context.md` with: navigation structure, entry points, auth boundaries.

## Gate — Critères de passage vers Phase 8

### Input contract
- [ ] `docs/03-product-strategy.md` existe et liste les pages/screens/endpoints (Phase 3)
- [ ] `docs/06-architecture.md` existe avec la stack et les contraintes de routing (Phase 6)
- [ ] `docs/06-tech-rules.md` existe (Technical Constitution, Phase 6)

### Output contract

#### Automatiques (Claude vérifie)
- [ ] `docs/07-product-architecture.md` existe et contient : structure de navigation, carte complète (URLs/screens/endpoints), limites d'auth
- [ ] Chaque page/screen/endpoint de `docs/03-product-strategy.md` apparaît dans l'architecture
- [ ] La section "Phase 8 design targets" existe avec la liste ordonnée pour Phase 8
- [ ] `docs/context.md` contient la section "Phase 7 — Product Architecture"

#### Validation utilisateur (bloquant)
- [ ] L'utilisateur a validé la navigation principale (header/footer pour web, tab bar pour mobile, sidebar pour SaaS)
- [ ] L'utilisateur a validé les URL patterns ou le deep link schema

### Verdict: PASS | WARN | FAIL
- **PASS**: Tous les critères remplis
- **WARN**: Architecture validée, sections secondaires incomplètes (ex: sitemap pas encore généré, webhooks non documentés) — documenter les manques, compléter au début de Phase 9
- **FAIL**: Navigation principale non validée OU pages/screens manquants dans la carte — bloquer avant de continuer

## Checkpoint

Present the product architecture. When validated: **"Phase 7 complete. Moving to Phase 8 — UI Design."** Then read `steps/08-ui-design.md`.
