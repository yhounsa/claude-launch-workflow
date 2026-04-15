# Phase 3 — Product Strategy

**Invoke:** `content-strategy` skill for web/e-commerce; direct execution for mobile/saas/api

## Purpose

Product strategy decides what the product contains — what pages, screens, features, or endpoints need to exist, what each one's job is, and how users flow through the product from first touch to core value. Without this phase, you build things nobody needs or miss the critical paths that drive conversion and retention.

This is the most divergent phase in the workflow. The output format changes completely based on `product_type`. Read Phase 1's `product_type` before doing anything else.

## Universal Step: Core User Flows

Regardless of product type, **always define 3-5 core user flows first**. A user flow is a named path a specific user takes through the product to accomplish a specific goal.

Format for each flow:
```
Flow name: [short name]
User: [persona name from Phase 2]
Goal: [what they want to accomplish]
Entry point: [where they start — search result, app store, referral link, etc.]
Steps: [numbered list of interactions]
Success state: [what the user has achieved]
Failure states: [where users typically drop off and why]
```

These flows serve as the north star for screen/page/feature decisions. If a screen or feature doesn't appear in a flow, question whether it's needed.

## Product-Type Execution

Read `docs/context.md` for `product_type`, then follow the matching section below.

---

### web — Page Strategy

**Invoke:** `content-strategy` skill (coreyhaines31/marketingskills)

Feed it the positioning from Phase 2. Ensure the strategy covers:

- **Site pages**: Which pages are needed and what each one's job is (convert, inform, build trust, rank in search, etc.)
- **Blog/content marketing**: Whether ongoing content makes sense, and if so, what topics and frequency
- **Lead capture**: Newsletter, free resource, waitlist, demo request — the mechanism to capture visitors not ready to act yet
- **Social media tie-in**: How the site connects to the user's social presence and distribution channels

Output is sized to the project — a solo freelancer doesn't need the same content plan as a funded startup.

Deliverable additions: page list with job, content calendar decision, lead capture mechanism.

---

### mobile — Screen Strategy

Do not invoke the content-strategy skill. Execute directly.

**Step 1: Screen inventory**

List every screen in the app. For each screen:
```
Screen: [name]
Route/identifier: [e.g., /onboarding/step-1, TabBar > Home]
Job: [one sentence — what does this screen accomplish for the user?]
Appears in flows: [list the flow names from the universal step]
Key interactions: [tap, swipe, input, camera, etc.]
```

**Step 2: Navigation architecture**

Define the top-level navigation pattern:
- Tab bar (mobile standard) — list tabs and what lives in each
- Stack-only (onboarding-heavy apps, tools)
- Drawer (admin/settings-heavy apps)
- Hybrid — specify which areas use which pattern

**Step 3: Onboarding sequence**

List the exact onboarding screens in order. For each:
- What information is collected or communicated
- Whether it is skippable
- What triggers progression to the next screen
- What permission requests are embedded (justify each against user value)

**Step 4: Core loop**

Define the primary repeating interaction loop — the thing users do daily/weekly that creates retention. Map it screen by screen.

Deliverable additions: full screen list with jobs, navigation map, onboarding sequence, core loop definition.

---

### saas — Feature Strategy

Do not invoke the content-strategy skill. Execute directly.

**Step 1: Feature inventory**

List every feature in the product. For each:
```
Feature: [name]
Description: [one sentence]
User role(s): [who can use this]
Tier: [which pricing tier(s) include this]
Appears in flows: [list the flow names from the universal step]
```

**Step 2: User roles and permissions**

Define every role in the system:
```
Role: [name]
Description: [what kind of user this is]
Can do: [list of permissions]
Cannot do: [explicit restrictions]
Upgrade path: [what unlocks more access]
```

**Step 3: Tier breakdown**

For each pricing tier, define:
- Name
- Target persona (who is this tier for?)
- Feature set (reference the feature inventory)
- Limits (seats, API calls, storage, etc.)
- Upgrade triggers (what will make a user on this tier want to upgrade?)

**Step 4: Empty states and activation**

Define the "zero state" for each key feature — what does a user see before they have data? What action are they prompted to take? Empty states are the #1 driver of activation failure in SaaS.

Deliverable additions: feature list with tier assignments, role/permissions matrix, tier narrative, empty state plan.

---

### api — Endpoint Strategy

Do not invoke the content-strategy skill. Execute directly.

**Step 1: Resource inventory**

List every resource in the API:
```
Resource: [name, plural noun — e.g., users, orders, webhooks]
Description: [one sentence]
Operations: [list: GET, POST, PUT/PATCH, DELETE]
Auth required: [yes/no + auth type]
Rate limited: [yes/no + tier]
```

**Step 2: Endpoint specification**

For each resource, define endpoints:
```
Endpoint: [METHOD /path]
Description: [what this does]
Request: [params, body schema — high level]
Response: [shape of success response]
Error cases: [common failure modes and error codes]
```

**Step 3: Versioning strategy**

Define how the API handles versioning:
- URL versioning (`/v1/...`) vs. header versioning vs. date-based
- Deprecation policy (how much notice, how communicated)
- Breaking vs. non-breaking change definitions

**Step 4: Authentication and rate limiting**

Define the auth model (API key, OAuth2, JWT) and rate limit tiers upfront. These are positioning decisions, not just technical ones — they communicate trust and scale expectations.

**Step 5: Developer quickstart path**

Define the minimum number of API calls a developer needs to make to get a meaningful result. This is the "hello world" of your API. Map it as a 3-5 step flow. Everything in the API should make this path as short as possible.

Deliverable additions: resource list, endpoint spec, versioning policy, auth model, quickstart flow.

---

### e-commerce — Catalog + Commerce Strategy

This is a hybrid of web page strategy and a product commerce structure.

**Part A — Page strategy** (same as `web` above):
Apply the `content-strategy` skill for the marketing/informational pages.

**Part B — Product catalog structure**:
```
Category: [name]
Description: [one sentence]
Product types in this category: [list]
Attributes per product: [name, price, variants, images, description length, etc.]
```

**Part C — Checkout flow**:
Map the complete purchase path:
- Cart → Checkout entry
- Address collection
- Shipping options
- Payment
- Order confirmation
- Post-purchase (upsell, email sequence, tracking)

For each step: what information is shown, what is collected, what can cause drop-off.

**Part D — Trust and operations**:
Define upfront: returns policy, shipping promise, customer support access point. These aren't afterthoughts — they are conversion elements on every product page.

Deliverable additions: catalog taxonomy, checkout flow map, trust signals inventory.

---

### hybrid

Apply the relevant sections for each type in scope. Identify shared flows and de-duplicate. Note where the strategies create tension (e.g., a mobile app that is also a SaaS — the feature tier structure must map to the screen structure).

---

## Deliverable

Save `docs/03-product-strategy.md` with:
- Universal: 3-5 core user flows
- Product-type-specific output (see sections above)
- Summary table: what exists at the end of this phase (pages / screens / features / endpoints)

Update `docs/context.md` with:
- List of pages / screens / features / endpoints (names only, for reference)
- Core user flow names
- Section header "Phase 3 — Product Strategy"

## Gate — Critères de passage vers Phase 4

Avant de passer à la phase suivante, vérifier :

### Input contract (vérifié au début de la phase)
- [ ] `docs/02-positioning.md` existe avec persona, positionnement, voix de marque validés
- [ ] `docs/context.md` contient `product_type` et `target_platform`

### Output contract (vérifié à la fin)

#### Automatiques (Claude vérifie)
- [ ] `docs/03-product-strategy.md` existe et contient les 3-5 core user flows
- [ ] La section spécifique au `product_type` est présente et complète (pages / screens / features / endpoints)
- [ ] `docs/context.md` contient la section "Phase 3 — Product Strategy" avec la liste des éléments

#### Validation utilisateur (bloquant)
- [ ] L'utilisateur a validé les core user flows (aucun flow critique manquant)
- [ ] L'utilisateur a validé l'inventaire complet (rien de manquant, rien de superflu)
- [ ] Pour mobile : l'utilisateur a validé la séquence d'onboarding et le core loop
- [ ] Pour SaaS : l'utilisateur a validé la structure des tiers et la matrice des rôles
- [ ] Pour API : l'utilisateur a validé le quickstart path et la stratégie de versioning
- [ ] Pour e-commerce : l'utilisateur a validé le flux de checkout et la taxonomie du catalogue

### Verdict: PASS | WARN | FAIL
- **PASS**: Tous les critères remplis
- **WARN**: Critères critiques OK, issues mineures documentées
- **FAIL**: Au moins un critère critique échoue — corriger avant de continuer

## Checkpoint

Present the product strategy with the universal flows first, then the product-type-specific content. When validated: **"Phase 3 complete. Moving to Phase 4 — Marketing Psychology."** Then read `steps/04-psychology.md`.
