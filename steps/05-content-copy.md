# Phase 5 — Content & Copy

**Invoke:** `copywriting` skill (coreyhaines31/marketingskills) for web/e-commerce marketing pages; direct execution for screens, in-app copy, docs

## Core Principle

**Copy before design. Always.**

No wireframes, no mockups, no design system decisions until the words are written and validated. Design that precedes copy produces beautiful containers with placeholder text — and placeholder text is where product clarity goes to die. Every pixel in Phase 6+ must serve copy that already exists.

This principle applies to all product types, without exception.

## How to run this phase

1. Read `docs/context.md` to load all prior decisions: `product_type`, positioning (Phase 2), screen/page/feature list (Phase 3), and psychology mapping (Phase 4).

2. Apply the **product-type-specific execution** below. Each section specifies which sub-skills to invoke and what to produce.

3. For every piece of copy produced, trace it back to:
   - The positioning statement (does this copy reinforce it?)
   - The psychology map (does this copy apply the right lever for this location?)
   - The core user flows (does this copy move the user forward?)

4. Always produce multiple headline variants (2-3 options) at decision points. Let the user choose rather than picking for them.

## File Naming Convention

All copy lives in `docs/05-copy/`. Use this structure:

```
docs/05-copy/
  index.md          ← master copy index (what files exist and what each covers)
  [slug].md         ← one file per page or screen group or doc section
  meta-seo.md       ← all meta titles and descriptions (web and mobile app store)
  notifications.md  ← all push/email notifications copy (mobile and SaaS)
  emails.md         ← all transactional and marketing email copy
  errors.md         ← all error messages and empty states
```

For mobile apps with many screens, group related screens: `onboarding.md`, `home-tab.md`, `profile.md`, etc.

---

## Product-Type Execution

### web

**Invoke:** `copywriting` skill for all pages.

Feed it:
- Positioning statement and brand voice (Phase 2)
- Page list with each page's job (Phase 3)
- Psychology levers per page (Phase 4)

For each page, produce:
- **H1 headline**: 2-3 variants. Short (under 10 words for hero), benefit-led, positioned.
- **Subheadline**: Elaborates on the H1 promise without repeating it.
- **Body copy**: Follows the skill's methodology. Uses the brand voice from Phase 2.
- **Primary CTA**: Wording + 1-2 alternatives. Must be action-verb-led. Must be specific (not just "Submit").
- **Secondary CTA** (if applicable): For pages with two calls to action.
- **Meta title**: Under 60 characters. Primary keyword-first.
- **Meta description**: Under 155 characters. Benefit-led. Contains a soft CTA.

If Phase 3 included a blog strategy, write the first 3-5 article outlines: title variants, intro paragraph, H2 section headings, conclusion hook.

Save: one file per page in `docs/05-copy/` plus `meta-seo.md`.

---

### mobile

Do not invoke the copywriting skill for screen copy. Execute directly.

**Onboarding screens** (`docs/05-copy/onboarding.md`):

For each onboarding screen identified in Phase 3:
```
Screen: [name]
Headline: [primary text — short, benefit-led]
Body: [supporting sentence — optional, max 2 lines]
Primary CTA: [button label — action verb + object]
Secondary CTA: [skip / back / learn more — if applicable]
Permission dialog (if any):
  Title: [system dialog title — must be honest]
  Purpose text: [shown before the system dialog — explains WHY]
  Deny consequence: [what the user loses — be honest]
```

**Per-screen copy** (one file per tab/section):

For each screen in the app:
```
Screen: [name]
Navigation title: [title bar text — short, contextual]
Empty state headline: [shown when no content exists]
Empty state body: [one sentence explaining what goes here + CTA]
Empty state CTA: [action to fill the empty state]
Error states: [list of error conditions + messages for this screen]
Loading state: [loading message or skeleton behavior]
```

**Push notification templates** (`docs/05-copy/notifications.md`):

For each notification category defined in Phase 4:
```
Category: [name]
Trigger: [what event sends this]
Title: [max 50 chars — notification title]
Body: [max 110 chars — notification body]
Deep link: [where tapping the notification goes]
Variants: [A/B test variants if applicable]
Frequency guardrail: [max frequency + suppression logic]
```

**App store listing** (`docs/05-copy/app-store.md`):
- **App name**: Primary keyword in name if possible (iOS allows 30 chars, Android 50)
- **Subtitle** (iOS): 30 chars — secondary value proposition
- **Short description** (Android): 80 chars — appears in search results
- **Long description**: First 3 lines are above the fold (the "more" truncation point) — these must stand alone. Full description: keyword-rich, benefit-led, uses spacing and short paragraphs for scanability.
- **Keywords** (iOS): 100 characters, comma-separated, no spaces after commas, no brand names, no repeated words from title/subtitle
- **Screenshot captions**: 5-10 screenshots with headline captions for each (the captions often carry more weight than the screenshots themselves)
- **What's new** (update copy): Template for patch notes that don't sound like they were written by an engineer

**In-app messaging** (`docs/05-copy/in-app-messages.md`):
- Feature discovery tooltips and coachmarks
- Upgrade prompts (for apps with free/paid tiers)
- Rating request prompt (timing and wording — this is a high-stakes UX moment)
- Re-engagement messages for users who haven't opened in N days

**Error messages** (`docs/05-copy/errors.md`):
Every error must answer three questions: What happened? Why? What can the user do next? Write errors in plain language — no error codes, no technical jargon exposed to users.

---

### saas

**Part A — Marketing site copy** (`docs/05-copy/homepage.md`, `pricing.md`, etc.):

Invoke the `copywriting` skill for all public-facing marketing pages (same as `web` above). These include: homepage, features page, pricing page, about/mission page, case studies.

For the pricing page specifically:
- Tier names: do they communicate value or just tiers? (Revisit Phase 2 tier naming)
- Feature list copy: benefits, not feature names ("Unlimited team seats" not "RBAC")
- FAQ section: write 5-8 questions that address real pre-purchase anxieties

**Part B — In-app copy** (`docs/05-copy/in-app.md`):

For each major view in the product (reference the feature list from Phase 3):
```
View/feature: [name]
Empty state: headline + body + CTA
Success state: [what the user sees when this works correctly]
Loading state: [what communicates "this is working"]
Tooltips/hints: [list of contextual hints for non-obvious interactions]
```

**Part C — Email sequences** (`docs/05-copy/emails.md`):

Write full copy for these critical SaaS emails:

1. **Welcome email**: Sent immediately after signup. Job: get the user to first meaningful action.
   - Subject line (3 variants)
   - Preview text
   - Body (short — under 150 words, single CTA)
   - CTA: takes them to the most important onboarding step

2. **Activation nudge** (Day 2-3, if user hasn't completed activation):
   - Subject line (3 variants)
   - Body: empathy-first, specific action-led
   - One CTA only

3. **Trial expiration warning** (3 days before, 1 day before):
   - Subject line (3 variants per email)
   - Body: loss-aversion framing is appropriate here — be direct about what they lose
   - Upgrade CTA + list of features at risk

4. **Post-trial lapse** (if not converted — 3 days after trial ends):
   - Subject line (2 variants — one direct, one curiosity-led)
   - Body: low-pressure, offer something (extended trial, demo, discount — whatever the business decides)

5. **Onboarding milestone emails** (sent when users complete key actions):
   - One email per milestone (define 2-3 milestones with the user)
   - Job: celebrate progress, introduce the next feature

**Part D — Error and empty state copy** (`docs/05-copy/errors.md`):
Same format as mobile — plain language, three questions answered.

---

### api

Do not invoke the copywriting skill. Developer-facing copy requires a different mode.

**README** (`docs/05-copy/readme.md`):

Structure:
1. **One-line description**: What this API does. No adjectives.
2. **Why this API exists**: The problem it solves. Honest, specific.
3. **Quickstart** (5 lines of code or fewer to a successful response): The code must actually work.
4. **Installation** (if SDK exists)
5. **Authentication**: Exactly how to get and use credentials
6. **Core concepts**: 3-5 terms the developer needs to understand — defined in plain language
7. **Links**: Full docs, API reference, status page, support channel

**Getting started guide** (`docs/05-copy/getting-started.md`):

This is the narrative walk-through of the quickstart flow defined in Phase 3. Write it as a task-completion sequence:
- Each step has a goal, a code example, and the expected output
- Every step ends with a success state the developer can verify
- Include the error that most developers hit at each step + how to fix it

**API reference copy** (`docs/05-copy/api-reference.md`):

For each endpoint in Phase 3:
- Endpoint summary (one sentence — what it does, not how)
- Parameter descriptions (plain language — what each param means and when to use it)
- Response field descriptions (what each field in the response represents)
- Error codes (the exact text of each error message + what causes it + how to fix it)
- Code example (working, copy-paste ready, in the primary language of the target developer)

**Developer-facing error messages** (`docs/05-copy/errors.md`):

Developer errors are different from user errors — they can be more technical, but they must still be actionable:
- What went wrong (specific, not generic)
- Why it went wrong (the cause, not a restatement of the error)
- How to fix it (the exact next step)
- Where to get help (docs link, support channel)

---

### e-commerce

**Part A — Marketing pages** (invoke `copywriting` skill):
Homepage, category landing pages, about/story page, returns/shipping policy pages (these are conversion elements — write them in plain, reassuring language, not legalese).

**Part B — Product page copy** (`docs/05-copy/product-page-template.md`):

Write a product page copy template that applies to all products in the catalog:
- **Product name**: format convention
- **Hero headline**: benefit-led, not feature-led
- **Short description**: 1-3 sentences, appears below the product name. Answers: what is this, for whom, primary benefit.
- **Bullet point features**: 3-5 bullets. Format: `[Feature] — [Benefit]`. Never list features without benefits.
- **Long description**: The story of the product. Answers: why does this exist, how was it made, who is it for.
- **Social proof placement**: where reviews and ratings appear relative to the CTA
- **Trust signals**: returns policy, shipping estimate, secure checkout badge — write the actual copy for each

**Part C — Checkout and transactional copy** (`docs/05-copy/checkout.md`):

For each step in the checkout flow (Phase 3):
- Form field labels and placeholder text
- Error messages (specific, not "invalid input")
- Helper text (where to find a promo code, what address format is expected, etc.)
- CTA at each step (not just "Continue" — make it specific: "Continue to shipping", "Place your order")
- Order confirmation copy: headline, order summary intro, "what happens next" sequence

**Part D — Email sequences** (`docs/05-copy/emails.md`):
- Order confirmation email
- Shipping confirmation email (with tracking link)
- Delivery confirmation email
- Cart abandonment email (1-2 hours after: value reminder, 24 hours after: gentle nudge, 72 hours after: optional incentive)
- Post-purchase review request (timing: after delivery + buffer for product use)
- Return confirmation / refund processed

**Part E — Error and empty states** (`docs/05-copy/errors.md`):
- Out of stock messages (include back-in-stock notification CTA)
- Payment failure messages (specific, non-alarmist, actionable)
- Address validation errors
- Search with no results (include recommendation or alternative action)

---

### hybrid
Execute the relevant sections for each type in scope. Keep copy files organized by product area — don't combine mobile screen copy with marketing page copy in the same file.

---

## Deliverable

Save all copy in `docs/05-copy/` — one file per page/screen/section as specified above, plus:
- `docs/05-copy/index.md` — master list of all copy files and what each covers
- `docs/05-copy/meta-seo.md` — all meta titles, descriptions, and (for mobile) app store listing elements
- `docs/05-copy/errors.md` — all error messages and empty states

Update `docs/context.md` with:
- Primary headline (the H1 of the primary landing surface)
- Main CTA wording
- Key messages per page/screen (one sentence each — what is the core message of each surface?)
- Section header "Phase 5 — Content & Copy"

## Gate — Critères de passage vers Phase 6

Avant de passer à la phase suivante, vérifier :

### Input contract (vérifié au début de la phase)
- [ ] `docs/04-psychology.md` existe avec les leviers psychologiques mappés et validés
- [ ] `docs/context.md` contient la liste complète des screens/pages/features de la Phase 3
- [ ] Aucune décision de design n'a encore été prise (copy AVANT design)

### Output contract (vérifié à la fin)

#### Automatiques (Claude vérifie)
- [ ] `docs/05-copy/` existe et contient `index.md`
- [ ] `docs/05-copy/` contient au moins un fichier .md par page/screen/section listée dans `docs/03-product-strategy.md`
- [ ] `docs/05-copy/errors.md` existe avec les états d'erreur et empty states
- [ ] Chaque page/screen a au minimum : un headline, un CTA, du body copy
- [ ] `docs/context.md` contient la section "Phase 5 — Content & Copy"
- [ ] Pour web/e-commerce : `docs/05-copy/meta-seo.md` existe avec meta titles et descriptions
- [ ] Pour mobile : `docs/05-copy/app-store.md` existe et `docs/05-copy/notifications.md` existe
- [ ] Pour SaaS : `docs/05-copy/emails.md` existe avec les 5 séquences email critiques
- [ ] Pour API : `docs/05-copy/readme.md` et `docs/05-copy/getting-started.md` existent

#### Validation utilisateur (bloquant)
- [ ] L'utilisateur a validé le copy de la surface principale (homepage, app home screen, product landing page, README)
- [ ] L'utilisateur a validé les CTAs principaux (wording exact)
- [ ] L'utilisateur a validé les headlines (variante choisie pour chaque surface critique)
- [ ] Pour mobile : l'utilisateur a validé la séquence d'onboarding complète et les push notification templates
- [ ] Pour SaaS : l'utilisateur a validé les emails critiques (welcome, trial expiration)
- [ ] Pour API : l'utilisateur a validé le quickstart et les messages d'erreur API

### Verdict: PASS | WARN | FAIL
- **PASS**: Tous les critères remplis
- **WARN**: Critères critiques OK, issues mineures documentées
- **FAIL**: Au moins un critère critique échoue — corriger avant de continuer

## Checkpoint

Present the copy surface by surface, starting with the primary landing surface. For each surface with multiple headline variants, present the variants and ask the user to choose before moving on. When all copy is validated: **"Phase 5 complete. Moving to Phase 6 — Technical Architecture."** Then read `steps/06-tech-architecture.md`.
