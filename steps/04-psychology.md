# Phase 4 — Marketing Psychology

**Invoke:** `marketing-psychology` skill (coreyhaines31/marketingskills)

## Purpose

Understanding why people act is what separates a product that gets used from one that gets abandoned. This phase maps psychological principles to specific screens, pages, flows, and moments in the product — not to manipulate, but to communicate value in ways that align with how humans actually make decisions.

Every psychological lever identified in this phase must be pinned to a specific location in the product. Floating psychological principles that aren't anchored to actual UX moments are useless.

## How to run this phase

1. Read `docs/context.md` for `product_type`, audience context, and the screen/page/feature list from Phase 3.

2. Locate and read the `marketing-psychology` skill's SKILL.md (sibling directory to `launch`). Follow its process.

3. Feed the skill:
   - The target persona from Phase 2 (what motivates them, what they fear, what they've already tried)
   - The core user flows from Phase 3 (where the psychological work needs to happen)
   - The product promise (what transformation or outcome is being sold)

4. The skill will identify relevant psychological principles. For each one, document:
   - Where it applies (which page / screen / section / moment in a flow)
   - How to implement it authentically (not with fake urgency or invented testimonials)
   - Why it's appropriate for this specific audience
   - What failure mode to avoid (every lever has a dark pattern version)

5. Apply the **product-type-specific extensions** below based on `product_type` from Phase 1.

6. Include ethical guardrails — the line between persuasion and manipulation matters. Document both the intended use and the guardrail for each principle.

## Universal Psychological Principles (apply to all product types)

These are the core principles that apply regardless of product type. The skill will elaborate on each:

- **Social proof**: Reviews, testimonials, user counts, logos. Anchored to: the first screen/page a user sees.
- **Clarity over cleverness**: Users don't read; they scan. Anchored to: every headline, every CTA, every error message.
- **Loss aversion vs. gain framing**: Decide per screen whether to frame as gain or loss — the choice is strategic, not default.
- **Cognitive load reduction**: Every extra choice is a tax on action. Anchored to: onboarding flows, checkout, form design.
- **Progress and momentum**: Showing users they are moving forward. Anchored to: multi-step flows, onboarding, checkout.
- **Authority and credibility signals**: Credentials, press, certifications. Anchored to: above-the-fold on the primary landing surface.

## Product-Type Extensions

### web
Standard application of all universal principles. Special focus:
- **Above-the-fold psychology**: The first 5 seconds determine whether a visitor stays. Map the specific psychological sequence: grab attention → establish relevance → signal credibility → present the next step.
- **Scroll momentum**: Each section of a long-form page should create a micro-commitment that makes the user want to keep reading. Map this section by section.
- **CTA psychology**: Different CTAs carry different psychological weight ("Get started free" vs. "Try for free" vs. "Start building"). Justify CTA wording choices with a psychological rationale.

### mobile
Add to universal principles:

- **Onboarding psychology**: The first session is the most critical. Map the psychological sequence for onboarding:
  - What is the user's emotional state when they open the app for the first time?
  - What is the fastest path to the "aha moment" (the moment they realize the app's value)?
  - Which permissions are requested before vs. after the aha moment? (Requesting permissions before demonstrating value is a conversion killer.)
  - What value is communicated during loading states and delays?

- **Push notification psychology**: Each notification category the app sends needs a psychological justification:
  - What triggers this notification?
  - What is the user's likely context when they receive it?
  - What action are they prompted to take?
  - What is the risk of notification fatigue if this fires too often?
  - Guardrail: list the notification types that will NOT be sent (what the app refuses to abuse).

- **Habit loop design** (for apps aiming at daily/weekly use): Map the cue → routine → reward loop:
  - Cue: What triggers the user to open the app? (internal cue: anxiety, curiosity; external cue: notification, widget)
  - Routine: What do they do in the app?
  - Reward: What do they get? (variable reward is more powerful than fixed — document which type and why)
  - Investment: What does the user put into the app that makes them more likely to return? (data, preferences, social connections)

- **Gesture and haptic psychology**: Physical interactions create emotional responses. Document where satisfying haptic feedback or gesture animations reinforce positive emotional states.

### saas

- **Trial-to-paid conversion psychology**: The critical psychological moment is when the trial ends. Map the psychology of the upgrade path:
  - What is the user's emotional state at trial expiration?
  - What loss do they experience if they don't upgrade? (Loss aversion is the primary driver here — document what they lose access to specifically.)
  - What is the minimum psychological friction path to upgrade?
  - At what point in the trial does the user typically realize the product's value? (Define this as the "activation milestone" and optimize for reaching it early.)

- **Feature discovery psychology**: Users don't discover features — they need to be shown them at the right moment. Map:
  - Which features are power multipliers that users consistently miss?
  - What is the right moment in the user's journey to surface each one? (Too early = overwhelm; too late = user has already churned or developed workarounds.)
  - What is the psychological framing for feature announcements? ("New feature" vs. "You can now..." vs. "Here's something that solves [known pain]")

- **Pricing page psychology**: The pricing page is one of the highest-stakes pages in SaaS. Map:
  - Anchoring: which tier is shown first and why?
  - Decoy pricing: is there a tier that makes the middle tier look like the obvious choice?
  - Social proof placement on the pricing page specifically
  - The "most popular" or "recommended" badge — where it goes and why

- **Churn prevention psychology**: Before a user cancels, they send signals. Map:
  - What behavioral signals predict churn? (low session frequency, feature non-use, support tickets)
  - What psychological interventions apply at each signal? (re-engagement email, in-app prompt, success check-in)
  - What is the psychology of the cancellation flow itself? (making cancellation hard is not a retention strategy — making the user feel heard is)

### api

- **Developer trust psychology**: Developers evaluate trust differently than consumers. Map:
  - What signals make a developer trust an API before writing a single line of code? (uptime page, GitHub activity, clear versioning, honest error messages)
  - What is the "credibility sequence" on the landing page for developer-facing products?
  - Documentation quality as a psychological signal: what does messy docs communicate vs. well-maintained docs?

- **Time-to-first-value psychology**: A developer's patience has a defined half-life. The longer it takes to get a successful API response, the less likely they are to continue. Map the psychological experience of the quickstart:
  - Where does frustration typically spike in competitor quickstarts?
  - What feedback does the developer receive at each step to maintain momentum? (showing them they are making progress)
  - What is the emotional design of the first successful API call? (many successful APIs make this moment feel celebratory — a 200 response with a well-structured payload IS a UX moment)

- **Upgrade psychology for API tiers**: Developers upgrade because they hit limits. Map:
  - What limit triggers upgrade consideration?
  - What is the psychological framing of limit warnings? ("You've used 80% of your quota" vs. "You're growing fast — here's what comes next")
  - What is the friction of the upgrade path for a developer? (billing pages optimized for marketers alienate developers)

### e-commerce

- **Purchase psychology**: Map the psychological sequence from product discovery to purchase completion:
  - What anxiety does the buyer feel at each step?
  - What reassurance is provided at each step?
  - Where is social proof most impactful? (near the Add to Cart button, not just on the homepage)
  - Price anchoring: how is the product's price framed relative to alternatives?

- **Cart abandonment psychology**: ~70% of carts are abandoned. Map:
  - What causes abandonment at each stage (browsing → cart → checkout)?
  - What is the psychological strategy for re-engagement? (email timing, subject line framing, what is shown in the email)
  - What can be removed from the checkout flow to reduce cognitive friction?

- **Scarcity and urgency**: These are the most abused levers in e-commerce. Document:
  - What genuine scarcity exists in this product? (limited stock, seasonal availability)
  - What genuine urgency exists? (sale ending, shipping deadline for a holiday)
  - Guardrail: explicitly list what will NOT be fabricated (fake countdown timers, fake "X people viewing this" counters)

- **Post-purchase psychology**: The moment after purchase is an opportunity most e-commerce brands waste. Map:
  - What is the buyer's emotional state immediately after purchase? (excitement + anxiety about having made the right choice)
  - What does the order confirmation communicate psychologically? (reinforce the decision, set expectations, open the next action)
  - What is the cross-sell/upsell psychology on the confirmation page?

### hybrid
Apply the extensions for each type in scope. When the same psychological lever applies to multiple contexts (e.g., social proof on a mobile SaaS), document the specific expression of it in each context rather than combining them.

## Deliverable

Save `docs/04-psychology.md` with:
- Universal principles mapped to specific locations in the product
- Product-type-specific extensions fully documented
- For each lever: principle name, location in product, implementation guidance, failure mode / guardrail
- A summary table: `Principle | Where | How | Guardrail`

Update `docs/context.md` with:
- Key psychological levers chosen (names only, for reference)
- Most critical lever (the one that will have the biggest impact on the primary user flow)
- Section header "Phase 4 — Marketing Psychology"

## Gate — Critères de passage vers Phase 5

Avant de passer à la phase suivante, vérifier :

### Input contract (vérifié au début de la phase)
- [ ] `docs/03-product-strategy.md` existe avec les core user flows et l'inventaire validés
- [ ] `docs/context.md` contient `product_type` et la liste des screens/pages/features

### Output contract (vérifié à la fin)

#### Automatiques (Claude vérifie)
- [ ] `docs/04-psychology.md` existe et contient les leviers psychologiques mappés aux locations spécifiques du produit
- [ ] Chaque levier a : un nom, une location précise, une guidance d'implémentation, un guardrail éthique
- [ ] La section spécifique au `product_type` est présente et complète
- [ ] `docs/context.md` contient la section "Phase 4 — Marketing Psychology"

#### Validation utilisateur (bloquant)
- [ ] L'utilisateur a validé les leviers psychologiques choisis
- [ ] L'utilisateur a validé les guardrails éthiques (accord sur ce qui NE sera PAS fait)
- [ ] Pour mobile : l'utilisateur a validé la stratégie de push notifications et le habit loop
- [ ] Pour SaaS : l'utilisateur a validé la stratégie trial-to-paid et la psychologie du pricing
- [ ] Pour e-commerce : l'utilisateur a validé ce qui est genuine scarcity vs ce qui sera évité

### Verdict: PASS | WARN | FAIL
- **PASS**: Tous les critères remplis
- **WARN**: Critères critiques OK, issues mineures documentées
- **FAIL**: Au moins un critère critique échoue — corriger avant de continuer

## Checkpoint

Present the psychology mapping as a table (Principle | Where | How | Guardrail), then elaborate on the product-type-specific sections. When validated: **"Phase 4 complete. Moving to Phase 5 — Content & Copy."** Then read `steps/05-content-copy.md`.
