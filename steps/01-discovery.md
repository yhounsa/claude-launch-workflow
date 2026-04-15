# Phase 1 — Discovery

**Invoke:** `brainstorming` skill (obra/superpowers)

## Purpose

Before building anything, understand what you're building and for whom. This phase exists because skipping discovery leads to products that look polished but miss the mark — wrong audience, wrong tone, wrong features, wrong platform. The brainstorming skill uses Socratic questioning to uncover the user's real needs, not just their surface-level request.

This phase also performs **product type detection** — the output of this phase determines which platform-specific paths all subsequent phases follow.

## How to run this phase

1. Locate and read the `brainstorming` skill's SKILL.md. It is a sibling skill — find it by reading the file at the same level as the `launch` directory (e.g., if this step file is under `.../skills/launch/steps/`, the brainstorming skill is at `.../skills/brainstorming/SKILL.md`). If you can't find it via Glob, try reading the path directly. Follow its methodology.

2. Adapt the brainstorming to cover these project-specific areas:

   - **The business**: What does the user sell or offer? What's their story? What makes them different?
   - **The audience**: Who are the ideal users/customers? What problems do they have? Where do they find products like this?
   - **The product goals**: Sell directly? Generate leads? Build an audience? Enable a workflow? Some combination?
   - **Existing assets**: Do they have branding, content, social media, an email list, existing codebase, testimonials, a design system?
   - **Constraints**: Budget, timeline, technical preferences, team size, must-have features, compliance requirements.
   - **References**: Products they admire or want to emulate (include direct competitors).
   - **Platform preferences**: Does the user have a preferred platform (web, mobile, API)? Do they have existing infrastructure? Are there technical or business reasons to prefer a specific stack?

3. After gathering answers, explore 2-3 possible product directions (e.g., minimal landing page vs. full SaaS vs. mobile-first app) and help the user pick one.

## Product Type Detection

At the end of discovery, classify the product using exactly these two fields. Both values are **required** and will be referenced by all subsequent phases.

### Product Type
Choose one:
- `web` — marketing site, landing page, portfolio, blog, content platform
- `mobile` — native or cross-platform mobile application
- `saas` — subscription software product with user accounts and persistent state
- `e-commerce` — product catalog, cart, checkout, order management
- `api` — developer-facing service, backend product, SDK
- `hybrid` — combination of two or more types (specify which, e.g., `saas + mobile`)

### Target Platform
Choose one:
- `web` — browser-based (desktop and/or mobile web)
- `ios` — Apple devices only (Swift/SwiftUI)
- `android` — Android only (Kotlin/Jetpack Compose)
- `cross-platform` — iOS + Android via React Native or Flutter
- `backend` — server-side only (API, CLI, service)
- `multi` — multiple platforms in scope (specify in notes)

These two values appear prominently in `docs/01-discovery.md` and `docs/context.md`. Every subsequent phase reads them to adapt its output accordingly.

## Deliverable

Save `docs/01-discovery.md` with:
- Summary of all answers
- Chosen direction with rationale
- Key constraints and requirements
- **Product type**: one value from the Product Type list above
- **Target platform**: one value from the Target Platform list above
- Platform rationale: why this type and platform were chosen

Update `docs/context.md` with:
- `product_type` (one of the 6 values)
- `target_platform` (one of the 6 values)
- Audience snapshot
- Chosen direction summary
- Section header "Phase 1 — Discovery"

## Gate — Critères de passage vers Phase 2

Avant de passer à la phase suivante, vérifier :

### Input contract (vérifié au début de la phase)
- [ ] L'utilisateur a fourni une description initiale du produit à construire

### Output contract (vérifié à la fin)

#### Automatiques (Claude vérifie)
- [ ] `docs/01-discovery.md` existe et contient : type de projet, audience cible, objectifs, contraintes, `product_type`, `target_platform`
- [ ] `docs/context.md` contient les champs `product_type` et `target_platform` avec des valeurs valides
- [ ] `docs/context.md` contient la section "Phase 1 — Discovery"

#### Validation utilisateur (bloquant)
- [ ] L'utilisateur a confirmé la direction choisie (type de produit, fonctionnalités clés)
- [ ] L'utilisateur a confirmé le `product_type` et `target_platform` détectés

### Verdict: PASS | WARN | FAIL
- **PASS**: Tous les critères remplis
- **WARN**: Critères critiques OK, issues mineures documentées
- **FAIL**: Au moins un critère critique échoue — corriger avant de continuer

## Checkpoint

Present the discovery summary including product type and target platform prominently. When the user validates, announce: **"Phase 1 complete. Moving to Phase 2 — Positioning."** Then read `steps/02-positioning.md`.
