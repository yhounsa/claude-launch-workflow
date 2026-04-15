# Step 09b2 — Interactive Test Plan Generation

## Purpose

Before writing a single line of code, generate a complete interactive test plan that describes every user action, every screen transition, every error state, and every edge case — all derived from the analysis documents (Phases 3-5) and the specs (09a).

This plan serves two purposes:
1. **During development (09c):** Developers know exactly what will be tested, so they code defensively
2. **During testing (09d):** The test runner has a complete script to automate — no guessing

## Step 1 — Read all analysis sources

Load these documents and extract testable behaviors:

| Source | What to extract |
|--------|----------------|
| `docs/03-product-strategy.md` | Core user flows (3-5 journeys from start to end) |
| `docs/04-psychology.md` | Lever-to-screen mapping (each lever = a moment to test user engagement) |
| `docs/05-copy/*.md` | Every CTA = an interactive action to test. Every form = inputs to validate. |
| `docs/07-product-architecture.md` | Navigation structure = transitions to test. Every route = a page that must load. |
| `docs/09-development-plan.md` | User stories Given/When/Then = the acceptance criteria to verify |

## Step 2 — Generate the Interactive Test Plan

For each core user flow from Phase 3, write a detailed test scenario covering:
- **Happy path** — the ideal journey from start to finish
- **Error paths** — what happens when things go wrong (invalid input, network failure, permission denied)
- **Edge cases** — empty states, long text, special characters, rapid actions

### Test scenario format

```markdown
## Flow: [Flow name from Phase 3]
**Persona:** [From Phase 2]
**Entry point:** [Screen/page where the flow starts]
**Goal:** [What the user wants to achieve]

### Happy Path
| # | Action | Expected Result | Screen |
|---|--------|----------------|--------|
| 1 | Open app/site | Home screen loads in < 2s, no console errors | Home |
| 2 | Scroll to [section] | [Section] is visible with [CTA text from copy] | Home |
| 3 | Tap/Click "[CTA text]" | Navigate to [target screen] | [target] |
| 4 | Fill [field] with "[valid value]" | Field accepts input, no error | [form screen] |
| 5 | Tap/Click "[Submit CTA]" | Loading indicator → success state → [next screen] | [form screen] |
| ... | ... | ... | ... |

### Error Paths
| # | Action | Expected Result | Screen |
|---|--------|----------------|--------|
| E1 | Submit form with all fields empty | Error messages on each required field | [form screen] |
| E2 | Enter invalid email "[bad@]" | "Invalid email" message appears | [form screen] |
| E3 | Network goes offline during submit | "Check your connection" message | [form screen] |
| E4 | Server returns 500 | "Something went wrong, try again" message | [form screen] |
| ... | ... | ... | ... |

### Edge Cases
| # | Action | Expected Result | Screen |
|---|--------|----------------|--------|
| X1 | Enter 500-character name | Field truncates or shows max length warning | [form screen] |
| X2 | Double-tap submit button | Only 1 request sent (debounce) | [form screen] |
| X3 | Navigate back during async operation | Operation cancelled cleanly, no orphaned state | [screen] |
| ... | ... | ... | ... |
```

### What to cover for each product type

**Web:**
- Page load times (every page loads < 3s)
- Navigation (every link in header/footer works)
- Responsive (each flow works at mobile 375px, tablet 768px, desktop 1280px)
- Forms (validation on blur, on submit, error clearing on re-type)
- SEO (every page has title + meta description — verify via DOM)
- Accessibility (keyboard navigation through forms, focus states visible)

**Mobile (React Native, Flutter, Swift, Kotlin):**
- App startup (cold start < 3s, warm start < 1s)
- Navigation (stack push/pop, tab switching, drawer open/close, deep links)
- Gestures (swipe back, pull to refresh, long press, pinch to zoom if applicable)
- Orientation (portrait → landscape → portrait if supported)
- Permissions (camera, location, notifications — test grant + deny)
- Background/foreground (app goes to background, comes back — state preserved)
- Offline mode (actions queue or show error, no crashes)
- Push notifications (tap notification → correct screen opens)
- Keyboard (opens/closes without layout breaking, submit on keyboard "done")

**SaaS:**
- Multi-user flows (user A creates → user B sees)
- Role-based access (admin sees X, regular user doesn't)
- Subscription flows (free tier limits, upgrade prompt, downgrade)
- Session management (timeout, concurrent sessions, remember me)

**API:**
- Every endpoint: valid request → 200, invalid → 400, unauthorized → 401
- Rate limiting (burst requests → 429)
- Pagination (first page, last page, empty page)
- Webhook delivery (event triggers → webhook fires → payload correct)

## Step 3 — Map tests to automation tools

Based on `product_type` and `target_platform` from `docs/context.md`:

| Platform | Automation tool | Test file format |
|----------|----------------|------------------|
| Web | Playwright (`webapp-testing` skill) | `tests/e2e/flow-[name].spec.ts` |
| React Native | Detox or Maestro | `e2e/flow-[name].test.js` or `flows/[name].yaml` |
| Flutter | `integration_test` + `patrol` | `integration_test/flow_[name]_test.dart` |
| iOS (Swift) | XCTest UI | `UITests/Flow[Name]Tests.swift` |
| Android (Kotlin) | Espresso + UI Automator | `androidTest/Flow[Name]Test.kt` |
| API | Supertest or HTTP client | `tests/api/flow-[name].test.ts` |

## Step 4 — Save the test plan

Save to `docs/09-tests/` using this structure:

```
docs/09-tests/
├── test-plan.md              ← Index global + coverage tracker
├── flows/
│   ├── flow-[name-1].md      ← Tests détaillés du flow 1
│   ├── flow-[name-2].md      ← Tests détaillés du flow 2
│   └── ...
└── reports/                   ← Créé plus tard par 09d
```

### `docs/09-tests/test-plan.md` (index global)

```markdown
# Interactive Test Plan — [Project Name]

**Generated from:** Phases 3, 4, 5, 7 + User Stories (09a)
**Product type:** [type]
**Platform:** [platform]
**Automation tool:** [tool]

## Coverage Tracker

| Flow | Priority | Stories | Tests écrits | Tests passent | Dernière exécution |
|------|:--------:|---------|:------------:|:-------------:|:------------------:|
| [flow-1] | P0 | US1, US3 | 0/[N] | — | — |
| [flow-2] | P0 | US2 | 0/[N] | — | — |
| [flow-3] | P1 | US4, US5 | 0/[N] | — | — |

**Priority levels:**
- **P0 (Critical):** Money, auth, core value — ALWAYS run
- **P1 (Important):** Secondary features — run per sprint
- **P2 (Edge cases):** Nice-to-have — run at end of all sprints

## Flow Index

| Flow | File | Happy steps | Error steps | Edge steps | Total |
|------|------|:-----------:|:-----------:|:----------:|:-----:|
| [flow-1] | `flows/flow-[name-1].md` | [N] | [N] | [N] | [N] |
| [flow-2] | `flows/flow-[name-2].md` | [N] | [N] | [N] | [N] |

**Total:** [N] flows, [N] steps
```

### `docs/09-tests/flows/flow-[name].md` (un fichier par flow)

```markdown
# Flow: [Flow name]

**Priority:** P0 | P1 | P2
**Stories covered:** US1, US3
**Entry point:** [screen/page]
**Goal:** [what user achieves]

## Happy Path
| # | Action | Expected Result | Screen |
|---|--------|----------------|--------|
| 1 | ... | ... | ... |

## Error Paths
| # | Action | Expected Result | Screen |
|---|--------|----------------|--------|
| E1 | ... | ... | ... |

## Edge Cases
| # | Action | Expected Result | Screen |
|---|--------|----------------|--------|
| X1 | ... | ... | ... |
```

This structure is important because:
- **One flow per file** keeps context lean — 09d loads only the flows it needs per sprint
- **Priority levels** let 09d run P0 first, then P1, then P2
- **Coverage tracker** is updated by 09c after each story and by 09d after each sprint execution
- **Reports folder** stores historical results for `--audit` to review later

## Step 5 — Generate manual test checklist (UX & subjective)

Some tests cannot be automated — they require human judgment. Generate `docs/tests-manuels.md` alongside the automated test plan. This document is for a human tester to follow, checking things that code can't verify.

### What goes in manual tests (NOT in automated)

| Category | Examples |
|----------|---------|
| **Animation quality** | "Le scroll est fluide à 60fps", "La transition est naturelle (pas saccadée)" |
| **UX feel** | "Le bouton paraît réactif au tap", "Le feedback haptic est perceptible" |
| **Visual subjective** | "Les couleurs rendent bien sur écran OLED", "Le contraste est suffisant en plein soleil" |
| **Accessibility humaine** | "Un lecteur d'écran peut naviguer l'app de bout en bout", "Les tailles de texte sont lisibles pour un senior" |
| **Sound & haptics** | "Le son de notification est audible mais pas agressif", "Le haptic feedback accompagne les actions importantes" |
| **Real device edge cases** | "L'app fonctionne avec le mode sombre système", "Le notch/dynamic island ne masque pas de contenu" |

### Format

```markdown
# Tests Manuels — [Project Name]

**Testeur:** [nom]
**Date:** [date]
**Device:** [model + OS version]

## UX & Animations
| # | Test | Écran | OK | NOK | Notes |
|---|------|-------|:--:|:---:|-------|
| M1 | Scroll fluide sans saccade | Home | | | |
| M2 | Transition entre onglets < 300ms | Navigation | | | |
| M3 | Pull-to-refresh animation naturelle | Lists | | | |

## Accessibilité
| # | Test | OK | NOK | Notes |
|---|------|:--:|:---:|-------|
| M10 | VoiceOver/TalkBack peut lire chaque écran | | | |
| M11 | Taille de texte x2 ne casse pas le layout | | | |

## Rendu visuel
| # | Test | OK | NOK | Notes |
|---|------|:--:|:---:|-------|
| M20 | Mode sombre : pas de texte illisible | | | |
| M21 | OLED : pas de gris trop clair sur fond noir | | | |
```

This document complements the automated tests. Together they provide 100% coverage: automated tests catch regressions, manual tests catch subjective issues.

## Step 6 — Present to user

Show the test plan summary and ask:
> "Voici le plan de tests — [N] flows automatisables ([N] P0, [N] P1, [N] P2) + [N] tests manuels pour l'UX et l'accessibilité. Tu veux ajuster les priorités ou les scénarios avant qu'on commence à coder ?"

Wait for validation. The user may:
- Promote a P1 flow to P0 (they consider it critical)
- Remove edge cases they don't care about
- Add missing scenarios
- Adjust priorities
- Add manual tests they want verified

Once validated, proceed to `09c-dev-cycle.md`. The test plan is a **living document** — 09c updates it per story, 09d executes and updates it per sprint.
