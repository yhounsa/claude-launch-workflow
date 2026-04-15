# Phase 2 — Positioning

**Invoke:** `product-marketing-context` skill (coreyhaines31/marketingskills)

## Purpose

Positioning determines how users perceive the product relative to alternatives. Without clear positioning, the product tries to speak to everyone and resonates with no one. This phase creates the strategic foundation that every piece of copy, every design decision, and every feature priority will reference.

Positioning is always universal in principle — but its *expression* differs by product type. A mobile app positions itself in an app store. A SaaS positions itself against a pricing page. An API positions itself in a developer README. This phase adapts accordingly.

## How to run this phase

1. Read `docs/context.md` to load Phase 1 decisions, especially `product_type` and `target_platform`.

2. Locate and read the `product-marketing-context` skill's SKILL.md (sibling directory to `launch`). Follow its process. This skill typically creates a `.agents/product-marketing-context.md` file — let it do that, but also save a copy in `docs/`.

3. The skill will guide you through defining:
   - Target persona (demographics, psychographics, pain points, jobs-to-be-done)
   - Product/service promise and unique value proposition
   - Competitive landscape and differentiators
   - Brand voice and tone

4. If the skill asks questions the user already answered in Phase 1, feed those answers in rather than asking again — respect the user's time.

5. Apply the **product-type-specific extensions** below based on the `product_type` from Phase 1.

## Product-Type Extensions

### web
Standard positioning applies directly. Focus on:
- Above-the-fold message for the homepage
- How the site differs from competitor sites in search results
- The single sentence a visitor should understand in 5 seconds

### mobile
Add to standard positioning:
- **App store angle**: The app store description must be written with the primary search keyword in mind. Define the keyword strategy alongside the positioning statement.
- **Category positioning**: Which app store category? What's the rank ambition? (chart-topper in niche vs. broad category presence)
- **Permission justification**: Every permission the app requests (camera, notifications, contacts) must map to a positioning promise — users grant access to products they trust

### saas
Add to standard positioning:
- **Pricing positioning**: Where does the product sit? (free tier to drive adoption? freemium with upgrade triggers? premium-only? enterprise-only?) Define this before writing any copy — pricing communicates positioning.
- **Trial strategy**: Free trial, freemium, demo-only, or paid-only? Each sends a different signal about the product's confidence in its own value.
- **Tier narrative**: If there are multiple tiers, each tier's name and scope communicates positioning. "Starter / Pro / Enterprise" vs. "Solo / Team / Scale" — different stories.

### api
Add to standard positioning:
- **Developer experience positioning**: Is this positioned as the simplest API in its category? The most powerful? The most reliable? DX is the product.
- **Integration angle**: What ecosystem does this API fit into? (Zapier-friendly? Enterprise webhook standard? OpenAPI-first?) Developers evaluate integratability before correctness.
- **Documentation-as-positioning**: The quality and philosophy of the docs IS the positioning signal. Define the documentation tone and depth as part of positioning.

### e-commerce
Add to standard positioning:
- **Brand vs. commodity**: Is this a brand (buy here because of story/quality/values) or a commodity (buy here because of selection/price/convenience)? These require fundamentally different positioning strategies.
- **Trust anchors**: What signals will the buyer see that tell them it's safe to give a credit card? (Returns policy, certifications, reviews platform, social proof) — define these as positioning elements, not just page features.

### hybrid
Apply the extensions for each type in scope. Note conflicts (e.g., SaaS pricing positioning + mobile app store positioning may pull in different directions) and document the resolution.

## Deliverable

Save `docs/02-positioning.md` with:
- Complete positioning document (persona, positioning statement, differentiators, brand voice)
- Product-type-specific positioning extensions
- One-sentence positioning statement: "For [persona], [product] is the [category] that [differentiator] because [reason to believe]."

Update `docs/context.md` with:
- Target audience summary
- Core positioning statement (one sentence)
- Brand voice descriptors (3-5 adjectives with brief explanations)
- Product-type-specific notes (pricing tier, app store keyword, DX promise, etc.)
- Section header "Phase 2 — Positioning"

## Gate — Critères de passage vers Phase 3

Avant de passer à la phase suivante, vérifier :

### Input contract (vérifié au début de la phase)
- [ ] `docs/01-discovery.md` existe avec `product_type` et `target_platform` définis
- [ ] `docs/context.md` contient la section "Phase 1 — Discovery"

### Output contract (vérifié à la fin)

#### Automatiques (Claude vérifie)
- [ ] `docs/02-positioning.md` existe et contient : persona, positionnement (une phrase), différenciateurs, voix de marque
- [ ] `docs/02-positioning.md` contient la section d'extension spécifique au `product_type`
- [ ] `docs/context.md` contient la section "Phase 2 — Positioning" avec persona, core message, brand voice

#### Validation utilisateur (bloquant)
- [ ] L'utilisateur a validé le persona principal
- [ ] L'utilisateur a validé le positionnement (la phrase de positionnement en une ligne)
- [ ] L'utilisateur a validé la voix de marque
- [ ] L'utilisateur a validé les décisions spécifiques au type (pricing tier, app store strategy, DX promise, etc.)

### Verdict: PASS | WARN | FAIL
- **PASS**: Tous les critères remplis
- **WARN**: Critères critiques OK, issues mineures documentées
- **FAIL**: Au moins un critère critique échoue — corriger avant de continuer

## Checkpoint

Present the positioning document with product-type-specific sections clearly labeled. When validated: **"Phase 2 complete. Moving to Phase 3 — Product Strategy."** Then read `steps/03-product-strategy.md`.
