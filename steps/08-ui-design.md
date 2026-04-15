# Phase 8 — UI Design

**Skill principal :** `stitch-design` (pour Stitch) OU `/ui-ux-pro-max` (Claude natif)

## Purpose

This phase creates the visual identity of the product — before any code is written. Separating design from code means the user validates what the product looks and feels like before investing time in implementation. If the design doesn't resonate, we iterate here, not after 500 lines of code.

This phase works for any product type — mobile apps, SaaS dashboards, e-commerce stores, marketing sites, developer tools. The design approach adapts to the product type and audience from prior phases. For APIs with no UI, this phase is skipped — note it in the gate.

## Step 0 — Load and synthesize prior deliverables

Read all prior deliverables. This synthesis step is the most important preparation — the quality of the mockups depends entirely on how well you absorb the strategic work from Phases 1-7.

### From Phase 1 — Discovery (`docs/01-discovery.md`)
Extract:
- **Product type and platform** — mobile (iOS/Android/cross-platform), web, SaaS, e-commerce? This determines the design archetype and platform conventions.
- **Key constraints** — budget, timeline, existing brand assets, existing design system
- **User's references** — any apps or sites they mentioned as visual inspiration

### From Phase 2 — Positioning (`docs/02-positioning.md`)
Extract:
- **Target persona** — demographics, psychographics, context of use. A mobile app for busy parents looks nothing like a developer dashboard. The persona shapes every visual choice: density, font size, contrast, gesture affordance.
- **Brand voice and tone** — warm/cold, formal/casual, playful/serious, premium/accessible. This translates directly to typography mood, color warmth, spacing density, and motion style.
- **Positioning statement** — the core promise. Must be visually prominent in the hero/main screen.
- **Competitive differentiators** — what makes this product unique? The design should reinforce these visually.

### From Phase 3 — Product Strategy (`docs/03-product-strategy.md`)
Extract:
- **Complete screen/page list with jobs** — each screen has a job (convert, inform, enable, delight). A conversion screen (bold CTAs, urgency cues) is designed differently from an information-dense screen (readability, hierarchy, data scanning).
- **Screen/page priority** — design the most important ones first.
- **Lead capture or onboarding mechanism** — newsletter, free trial, app store download? This needs a dedicated design component.

### From Phase 4 — Marketing Psychology (`docs/04-psychology.md`)
Extract:
- **Psychological levers per screen/section** — social proof, scarcity, authority, reciprocity. Each lever maps to a specific visual treatment.
- **Visual hierarchy guidance** — where the eye should go first on key screens.

### From Phase 5 — Content & Copy (`docs/05-copy/`)
Extract:
- **Real headlines for each screen/page** — not "Lorem ipsum". The actual headline (with 2-3 variants). Use the primary variant in mockups.
- **CTAs with exact wording** — "Télécharger l'app", "Commencer gratuitement", "Voir les tarifs". These drive button design.
- **Content structure per screen** — the copy reveals the natural flow: hero → value → proof → CTA. This IS the screen structure for the mockup.

### From Phase 6 — Tech Architecture (`docs/06-architecture.md`)
Extract:
- **Chosen stack** — React Native/Expo, Flutter, SwiftUI, Next.js, etc. This determines which design constraints apply (platform conventions, safe areas, navigation patterns).
- **Integrations with UI surface** — Stripe checkout UI, auth screens, dashboard widgets. These need design treatment.

### From Phase 7 — Product Architecture (`docs/07-product-architecture.md`)
Extract:
- **Navigation structure** — tab bar items, sidebar sections, header nav — and in what order.
- **Screen/page hierarchy** — which screens are root, which are detail/modal.
- **"Phase 8 design targets"** section — the ordered list of screens/pages to design. Use this as the design queue.
- **Auth boundaries** — which screens need auth state indicators (logged-in avatar, login button, gated content).

### From existing visual assets (if any)
If the project has a logo, brand guidelines, or existing design system assets in the repository, read them. They inform the color direction and visual language. If none exist, this phase establishes the visual direction from scratch.

### Synthesis: build the design brief

Before choosing a path, synthesize all extracted information into a concise **design brief** (keep in working memory, don't save as a file):

1. **Product archetype**: [mobile app / SaaS dashboard / storefront / editorial site / portfolio / developer tool]
2. **Platform**: [iOS / Android / cross-platform / web / desktop web]
3. **Audience**: [persona summary in one sentence]
4. **Visual mood**: [3-5 adjectives from brand voice]
5. **Color direction**: [from brand assets if any, or derive from voice and audience]
6. **Screens/pages to design**: [ordered list from Phase 7 design targets, with each screen's job]
7. **Key components**: [navigation pattern, primary CTA style, data display patterns, form style, social proof blocks]
8. **Content density**: [data-heavy / editorial long-form / punchy short sections / action-focused]
9. **Psychological emphasis**: [where visual weight goes on key screens]
10. **Platform-specific constraints**: [safe areas, status bar, bottom nav, gesture hints, notch — for mobile]

---

## Step 1 — Ask the user: how do you want to design?

Present two options:

> "Pour le design du produit, tu as deux options :
>
> 1. **Je gère le design** — je crée le design system et les maquettes directement à partir de tout le travail stratégique qu'on a fait. Tu valides le résultat.
> 2. **On utilise Stitch** — un outil de Google qui génère des maquettes haute-fidélité à partir de descriptions. Les résultats sont souvent plus visuels et plus proches d'un vrai produit."

- **Option 1** → Path A (Claude natif)
- **Option 2** → Path B (Stitch) — ask a follow-up question (see Step 1b)

### Step 1b — If Stitch: manual or automatic?

> "Tu préfères :
>
> **a) Je te prépare les prompts** — tu les colles toi-même dans stitch.withgoogle.com, tu génères, tu ajustes, et tu reviens avec les exports (screenshots, code HTML, ou DESIGN.md).
>
> **b) Je pilote Stitch directement** — je génère les maquettes automatiquement via Stitch, je télécharge les résultats, et je te fais valider chaque écran."

- **Option a** → Path B1 (prompts manuels)
- **Option b** → Path B2 (Claude pilote Stitch via MCP)

---

## Path A — Claude gère le design

Use this path when the user wants to stay entirely within Claude, without external tools. Works for any product type. Fastest path.

### A1 — Design direction with `/ui-ux-pro-max`

Invoke the `/ui-ux-pro-max` skill via the Skill tool. Feed it the **design brief** synthesized in Step 0 and ask it to recommend:
- **Style architectural** — based on product type and audience: editorial, minimal, glassmorphism, brutalist, material, cupertino, dashboard-dense, etc.
- **Color palette** — specific hex codes with semantic roles (primary, secondary, accent, background, surface, text-primary, text-secondary, error, success)
- **Font pairing** — heading + body fonts with reasoning tied to tone and platform (system fonts for mobile when appropriate)
- **UX guidelines** — spacing strategy, component patterns, visual hierarchy rules, density guidelines relevant to the product type

For mobile products, explicitly ask `/ui-ux-pro-max` for:
- Safe area handling patterns
- Touch target minimum sizes (44px iOS / 48dp Android)
- Bottom navigation design (iOS tab bar vs Android bottom nav)
- Status bar treatment (light/dark content, colored/transparent)
- Gesture affordance cues (swipe handles, drag indicators)

### A2 — Synthesize the design system into DESIGN.md

Take the `/ui-ux-pro-max` recommendations and synthesize them into a complete `DESIGN.md`. Every value must be copy-pasteable into code. "A warm color" is not enough — `#C5A55A, Rich Gold` is.

The DESIGN.md must follow this structure:

```markdown
# Design System: [Project Name]

## 1. Visual Theme & Atmosphere
[1-2 paragraphs: mood, aesthetic direction, audience, layout philosophy, platform context]

## 2. Color Palette & Roles
* **[Name]** (#hex) — [Role: where and when to use it]
* ...
### Color Rules
[Specific rules: gradients, forbidden patterns, contrast requirements (WCAG AA minimum)]

## 3. Typography Rules
* **Display:** [Font], [Weight], [Size range] — [Usage]
* **Headline:** [Font], [Weight], [Size range] — [Usage]
* **Body:** [Font], [Weight], [Size range] — [Usage, line-height]
* **Caption:** [Font], [Weight], [Size range] — [Usage]
* Max line length: [desktop] / [mobile]
* [For mobile: system font fallbacks, Dynamic Type support if iOS]

## 4. Component Stylings
### Navigation
[Web: header nav, footer — Mobile: tab bar, top nav bar, drawer — SaaS: sidebar]
### Buttons
* **Primary:** [Background, text color, radius, padding, hover/pressed state]
* **Secondary:** [Background, text color, border, hover/pressed state]
* **Destructive:** [For delete/cancel actions]
### Cards
[Background, radius, shadow, hover/pressed state, spacing between cards]
### Inputs / Form fields
[Background, radius, focus state, error state, minimum touch height]
### [Product-specific components]
[Any unique component this product type needs: data tables, chart containers, media players, step indicators, etc.]

## 5. Platform-specific tokens (mobile only)
* **Safe area top:** [how to handle notch/island]
* **Safe area bottom:** [home indicator clearance]
* **Status bar:** [light-content / dark-content, when to switch]
* **Bottom navigation height:** [value + safe area offset]
* **Touch target minimum:** [44pt iOS / 48dp Android]
* **Gesture zones:** [edges reserved for system gestures]

## 6. Signature Elements
[The 1-2 visual elements that make this design unique and memorable]

## 7. Elevation & Depth
[Shadow strategy, layering approach, blur/glow effects]
[For mobile: elevation hierarchy per Android Material / shadow per iOS]

## 8. Motion & Transitions (if applicable)
[Transition style: slide / fade / scale — Duration guidelines — Easing curves]

## 9. Do's and Don'ts
### Do
[3-5 specific positive instructions]
### Don't
[3-5 specific prohibitions with alternatives]
```

### A3 — Present and validate

Present the design system clearly:
- Colors with their roles
- Typography choices with reasoning tied to brand voice and platform
- Component style descriptions with platform-specific notes
- For mobile: annotated description of key screens (navigation bar treatment, tab bar design, primary action patterns)

### A4 — Save deliverables

Save `DESIGN.md` in the project root. This is the authoritative design reference for Phase 9.

→ Skip to **Deliverable** section below.

---

## Path B1 — Stitch with manual prompts

Use this path when the user wants to drive Stitch themselves. Gives maximum creative control.

### B1.1 — Generate optimized prompts

Locate and read the `stitch-design` skill's SKILL.md. Follow its **Prompt Enhancement Pipeline** to craft one prompt per screen/page.

Each prompt must be **grounded in the prior phases**. Structure every prompt like this:

1. **Opening mood** — from Phase 2 brand voice and positioning. Specific, not generic. "A mobile finance app with clean, trustworthy data visualization and calm blue tones for users who feel overwhelmed by money" not "a nice app."

2. **DESIGN SYSTEM block** — typography mood from Phase 2, component style hints from Phase 4 psychology, platform conventions (iOS vs Android vs web).

3. **SCREEN STRUCTURE with real content** — for each section of the screen:
   - Use the **actual headline** from `docs/05-copy/`
   - Use the **actual CTA text**
   - Describe the section's **job** (from Phase 3: convert, inform, enable)
   - Note the **psychological lever** active (from Phase 4)
   - Reference any **existing visual asset** if applicable

4. **Navigation** — from Phase 7: exact nav items, tab bar labels, sidebar items, or header structure.

5. **For mobile screens**: specify device frame (iPhone 15 Pro, Pixel 8), status bar style, bottom safe area, tab bar if present.

**Screen/page order** — design in this order:
- For web: Homepage first (tone-setter), then primary conversion page, then supporting pages
- For mobile: Main tab screen first (e.g., Home/Feed), then 2-3 key flow screens, then remaining
- For SaaS: Dashboard first (sets the data density and nav tone), then key feature pages

### B1.2 — Present prompts to the user

For each screen/page, present the prompt in a code block:

```
Voici le prompt optimisé pour [screen/page name].
Copie-le et colle-le dans stitch.withgoogle.com :

---
[THE PROMPT]
---

Une fois satisfait du résultat, exporte le design
(screenshot, code HTML, ou DESIGN.md) et partage-le ici.
```

### B1.3 — Collect and validate

Wait for the user to return with exports for each screen:
- Screenshots of the generated design
- HTML code exported from Stitch
- A `DESIGN.md` file if Stitch generated one
- Feedback on what they like or want to change

If changes needed, adjust the prompt's mood keywords, structure, or content — not specific colors or fonts (let Stitch make those choices). Have them regenerate.

Save any exported HTML or screenshots to `.stitch/designs/` for reference.

### B1.4 — Synthesize the design system

Once all screens are validated, create a unified `DESIGN.md` from the Stitch outputs:
- Extract the color palette, typography, and component styles Stitch chose
- For mobile: document safe area handling and platform-specific patterns visible in the designs
- Document everything in the DESIGN.md format from Path A (Section A2)

→ Skip to **Deliverable** section below.

---

## Path B2 — Claude pilots Stitch via MCP

Use this path when the user wants automation — Claude drives Stitch directly, generates screens, downloads results, and presents them for validation.

### B2.0 — Check MCP availability

Before starting, verify the Stitch MCP server is accessible by calling `list_projects`.

**If MCP is not available** — don't ask the user to re-choose. Inform them and fall back to B1:
> "Le serveur Stitch MCP n'est pas accessible. Je vais te préparer les prompts à coller manuellement dans Stitch à la place."

Then follow Path B1 from step B1.1.

### B2.1 — Set up the Stitch project

Use the `stitch-design` skill (locate with `**/skills/stitch-design/SKILL.md`). Follow its workflows:

1. Check if a Stitch project already exists (`list_projects`)
2. If not, create one (`create_project`) with a name matching the product
3. If a `.stitch/DESIGN.md` exists, use it as context for generation consistency
4. If not, the first screen generated establishes the visual direction

### B2.2 — Generate screens in 3 waves

**Screen registry:** Maintain `.stitch/registry.json` tracking the validated `screenId` for each screen/page. This is essential — if a download fails or a file goes missing, the registry provides the exact screen to re-fetch.

```json
{
  "projectId": "projects/xxxx",
  "screens": {
    "home": { "screenId": "abc123", "validated": true },
    "onboarding-1": { "screenId": "def456", "validated": true },
    ...
  }
}
```

For every screen in every wave, follow the same process:
1. **Build the enhanced prompt** using the `stitch-design` skill's Prompt Enhancement Pipeline — real copy from Phase 5, psychological levers from Phase 4, navigation from Phase 7, **platform context for mobile**.
2. **Generate the screen** via `generate_screen_from_text`
3. **Present to the user** — show the screenshot, surface any AI suggestions
4. **Wait for validation**
5. **On validation:**
   - Record the `screenId` in `.stitch/registry.json` with `"validated": true`
   - Download screenshot and HTML to `.stitch/designs/[screen-slug]/screenshot.png` and `.stitch/designs/[screen-slug]/index.html`

If the user wants changes, use `edit_screens` for targeted adjustments rather than regenerating from scratch. Only download and update the registry after the user approves the final result.

#### Wave 1 — Anchor screen (alone)

**For web products:** Generate the homepage first — it defines the visual tone for the entire site.
**For mobile apps:** Generate the main tab screen (Home/Feed) — it anchors density, typography, component style.
**For SaaS:** Generate the main dashboard — it anchors data density, sidebar, and widget patterns.
**For e-commerce:** Generate the product listing/storefront — it anchors the product card, grid, and CTA style.

Present to the user and wait for validation. This is the most important validation moment — the anchor screen locks the design system. **If the user wants design system changes** (different colors, mood, density), iterate here. It's cheap — only one screen to redo.

Once validated: the design system is **anchored**.

#### Wave 2 — 2-3 key screens (in parallel)

Generate in parallel (using subagents or concurrent MCP calls):

**For web:** Primary conversion page + Social proof page + About/Story page
**For mobile:** 
- Primary user flow screen (the action the app is built around)
- Onboarding screen (if the app has one)
- Profile or settings screen (tests a different visual register)

**For SaaS:** Key feature page + Settings/Billing page + User profile page
**For e-commerce:** Product detail page + Cart/Checkout page + Category/Filter page

These 3 screens stress-test the design system across different layouts and content densities. Present all 3 to the user together.

**Two types of feedback:**
- **Screen-specific changes** → use `edit_screens` on that screen only. No cascade.
- **Design system changes** → fix on the problematic screen, then regenerate only the other Wave 2 screens that are affected. The Wave 1 anchor is only revisited if the change is fundamental.

Once validated: the design system is **locked**. No more design system changes after this point.

#### Wave 3 — All remaining screens (in parallel)

Generate all remaining screens in parallel. The design system is stable — these should be consistent automatically.

Present as a batch. At this stage, only **screen-specific** feedback is expected. Handle with `edit_screens`. If a design system issue surfaces, fix with `edit_screens` rather than reopening the system.

### B2.3 — Verify all validated assets are present

Read `.stitch/registry.json` and for each entry with `"validated": true`:
1. Check `.stitch/designs/[screen-slug]/screenshot.png` exists and is non-empty
2. Check `.stitch/designs/[screen-slug]/index.html` exists and is non-empty
3. If any file missing — use the `screenId` to call `get_screen` and re-download

Cross-reference the registry against `docs/07-product-architecture.md` "Phase 8 design targets". If a screen exists in the architecture but has no registry entry, flag it.

If everything checks out:
> "Toutes les maquettes validées sont présentes dans `.stitch/designs/`. [N] screens, [N] screenshots, [N] HTML. Tout correspond aux versions validées. Prêt pour synthétiser le design system."

### B2.4 — Synthesize the design system

1. Use the `stitch-design` skill's `generate-design-md` workflow to analyze the Stitch project and synthesize `.stitch/DESIGN.md`
2. For mobile products, augment the synthesis with platform-specific tokens (safe areas, touch targets, status bar) observed in the generated screens
3. Copy or adapt this as the project's `DESIGN.md` in the root directory

→ Continue to **Deliverable** section below.

---

## Deliverable

Regardless of which path was taken, save:

1. **`DESIGN.md`** in the project root — the authoritative design system reference for Phase 9. Contains: color palette (hex), typography (names + weights + sizes), component styles, layout principles. For mobile: also contains platform-specific tokens.

2. **`docs/08-ui-design.md`** with:
   - Design approach used (Path A, B1, or B2)
   - Product type and platform-specific design notes
   - If Path A: the full design system produced via `/ui-ux-pro-max`
   - If Path B1: the prompts used + reference to exported assets in `.stitch/designs/`
   - If Path B2: Stitch project ID + reference to downloaded assets in `.stitch/designs/`
   - For mobile: platform-specific sections (iOS vs Android if cross-platform)

3. **Update `docs/context.md`** with: design approach chosen, key visual choices (palette, typography mood, layout style, platform tokens if mobile), path to DESIGN.md.

## Gate — Critères de passage vers Phase 9

### Input contract
- [ ] `docs/07-product-architecture.md` existe avec la section "Phase 8 design targets" (Phase 7)
- [ ] `docs/05-copy/` contient les vraies headlines et CTAs pour les screens principaux (Phase 5)
- [ ] `docs/06-tech-rules.md` existe (Technical Constitution, Phase 6)

### Output contract

#### Automatiques (Claude vérifie)
- [ ] `DESIGN.md` existe à la racine avec : palette (hex codes), typographies (noms + poids + tailles), composants (boutons, cards, inputs, navigation)
- [ ] Pour mobile : `DESIGN.md` contient une section "Platform-specific tokens" avec safe areas, touch targets, status bar
- [ ] `docs/08-ui-design.md` existe et documente l'approche (Path A, B1 ou B2)
- [ ] Chaque screen/page des "Phase 8 design targets" de `docs/07-product-architecture.md` a : un fichier HTML non vide dans `.stitch/designs/` (Path B1/B2) OU une description de composition dans DESIGN.md (Path A)
- [ ] `docs/context.md` mis à jour avec la section "Phase 8 — UI Design"

#### Validation utilisateur (bloquant)
- [ ] L'utilisateur a validé le design de l'écran ancre (homepage / main tab / dashboard)
- [ ] L'utilisateur a confirmé la palette, les typos et l'ambiance générale
- [ ] Pour mobile : l'utilisateur a validé le bottom tab bar et les patterns de navigation

### Verdict: PASS | WARN | FAIL
- **PASS**: Tous les critères remplis
- **WARN**: Design system validé, screens secondaires non encore designés — documenter les manques, les traiter en début de Phase 9 via `/frontend-design`
- **FAIL**: Écran ancre non validé OU DESIGN.md manquant OU palette/typos non confirmées — bloquer avant de continuer. C'est le gate le plus critique du workflow — changer le design après le code est coûteux.

## Checkpoint

This is the most critical checkpoint in the entire workflow. Changing the design after Phase 9 (Code) is expensive. Make sure the user is genuinely satisfied — not just "it's fine", but "yes, this is what I want to build."

When validated: **"Phase 8 complete. Moving to Phase 9 — Code."** Then read `steps/09-code/workflow.md`.
