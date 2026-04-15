# Add Feature — Post-Launch Feature Addition

**Skills:** `/code`, `/simplify`, `security-review`, `stitch-design` or `/ui-ux-pro-max`, `image-gen`, `webapp-testing`

## Purpose

The product is live. The user wants to add a new feature without replaying the entire 10-phase workflow. This mode inherits all existing project context (positioning, design system, tech rules, architecture) and runs a focused 6-step cycle for the new feature only.

This is NOT for bug fixes (use `/oneshot` or `/error-debugger`). This is for genuine new capabilities: a referral system, a chat feature, a new page, a new API endpoint, a new screen in the app.

---

## Prerequisite

This mode requires a completed (or substantially completed) `/launch` workflow. It reads existing artifacts to understand the product. If these files don't exist, suggest running `/launch` first.

**Required artifacts:**
- `docs/context.md` — product type, audience, positioning
- `docs/03-product-strategy.md` — current feature/screen/page list
- `docs/06-architecture.md` — tech stack and structure
- `docs/06-tech-rules.md` — technical constitution
- `DESIGN.md` — design system

**Optional but valuable:**
- `docs/07-product-architecture.md` — navigation structure
- `docs/05-copy/` — existing copy files
- `docs/09-development-plan.md` — existing stories and specs
- `.stitch/registry.json` — existing Stitch screens

If any required artifact is missing, list what's missing and ask the user if they want to proceed anyway (with reduced context) or run the relevant phase first.

---

## Step 1 — Understand the request and assess impact

Read ALL existing artifacts listed above. Then:

1. **Clarify the feature.** Ask the user to describe what they want. If the description is vague, ask targeted questions:
   - What should the user be able to do?
   - Who uses this feature? (existing persona or new?)
   - Does it interact with existing features? Which ones?
   - Any external integrations needed? (payment, API, notifications)

2. **Assess impact on existing artifacts.** Determine which documents need patching:

| Artifact | Needs update if... |
|----------|-------------------|
| `docs/03-product-strategy.md` | New screen, page, endpoint, or user flow |
| `docs/05-copy/` | New screen/page needs copy |
| `docs/06-architecture.md` | New integration, new service, new DB table |
| `docs/06-tech-rules.md` | New pattern or convention introduced |
| `docs/07-product-architecture.md` | New navigation entry, new route, new deep link |
| `DESIGN.md` | New component type not in the design system |

3. **Present the impact assessment:**

```
Feature: "Système de parrainage"

Artifacts to patch:
- docs/03-product-strategy.md → add referral to screen list + new user flow
- docs/05-copy/referral.md → write copy for referral page
- docs/07-product-architecture.md → add /referral route, add to nav

Artifacts unchanged:
- docs/06-architecture.md (no new integration)
- DESIGN.md (existing components sufficient)
- docs/06-tech-rules.md (no new patterns)

Estimated scope: 1 user story, 3-5 specs
```

Wait for user validation before proceeding.

---

## Step 2 — Patch existing artifacts

For each artifact identified in Step 1:

1. Read the current artifact
2. Add the new feature's information — surgically, without rewriting existing content
3. Save the updated artifact

**Patching rules:**
- Append, don't rewrite. Add the new feature to existing lists, don't restructure the document.
- Mark additions clearly with a comment: `<!-- Added via --add-feature: [feature name] [date] -->`
- If the feature needs copy, write complete copy (headlines, CTAs, body) in `docs/05-copy/[feature-slug].md` — same quality as the original Phase 5 output.
- The copy-before-design principle still applies: write copy BEFORE designing any new screens.

Update `docs/context.md` with a new section:

```markdown
## Feature Addition — [Feature name] ([date])
**Description:** [what was added]
**Artifacts patched:** [list]
**New specs:** [count]
```

---

## Step 3 — Generate specs (Given/When/Then)

Write user stories for the new feature using the same format as Phase 9:

```markdown
### US-ADD-1: [User story title]
As a [persona], I want [goal], so that [benefit].

**Acceptance Criteria:**
**Given** [precondition]
**When** [action]
**Then** [expected outcome]
**And** [additional criteria]

**Specs:**
| Spec | Category | Critical | TDD |
|---|---|---|---|
| [description] | Backend | Yes/No | Auto/Manual/--tdd only |
```

Apply the same critical spec rules: money, auth, user data, DB writes → TDD automatic.

Append these stories to `docs/09-development-plan.md` under a new section:

```markdown
## Feature Addition: [Feature name] ([date])

### User Stories
[stories here]

### Build Order
[Infra →] Backend → Frontend [→ Integrations]
```

Present to user for validation before coding.

---

## Step 4 — Design new screens (if needed)

**If the feature adds new screens/pages:**

1. Read `DESIGN.md` for design system tokens
2. Use existing copy from Step 2

**If Stitch MCP is available and was used previously (check `.stitch/registry.json`):**
- Generate new screens in the existing Stitch project using `generate_screen_from_text`
- Use the same design system and style as existing screens
- Add new screens to `.stitch/registry.json`
- Download screenshots to `.stitch/designs/[new-page-slug]/`

**If Path A was used (no Stitch):**
- `DESIGN.md` is the reference — no new mockups needed
- Copy structure defines layout

**If no new screens needed:**
- Skip this step entirely

---

## Step 5 — Build

Execute the development cycle from Phase 9, but scoped to this feature only:

### 5a — Code
- Invoke `/code` for each spec
- Follow `docs/06-tech-rules.md` strictly (technical constitution)
- For frontend: match `DESIGN.md` tokens, use real copy, reference mockups if they exist
- Generate images with `image-gen` if needed

### 5b — Tests
- Critical specs: TDD (red-green-refactor)
- Non-critical: tests after code
- Focus on: does the new feature work AND does it not break existing features?

### 5c — Review
- `/simplify` for code quality
- `security-review` scoped to the new feature's categories

### 5d — Integration with existing product
This is the most important step for `--add-feature`. The new feature must integrate cleanly.

**First**, write new test scenarios for the added feature (following the format from `docs/09-tests/test-plan.md`) and append them to the test plan.

**Then**, ask the user:
> "La feature est intégrée. Tu veux qu'on lance la suite de tests interactifs complète pour vérifier qu'il n'y a pas de régressions ? (recommandé) Ou seulement les tests de la nouvelle feature ?"

**If full suite:** Execute ALL tests from `docs/09-tests/test-plan.md` (existing + new). Follow `09d-integration-tests.md` process: run → auto-fix with right skill → retest → report.

**If new feature only:** Execute only the new feature's tests. Faster but doesn't catch regressions.

**If any test fails:**
1. Load the appropriate platform skill to fix (see 09d Step 4b)
2. Fix the issue
3. Re-run the full flow
4. Repeat until green

**Present the test report to the user** (same format as 09d Step 5) before proceeding to Step 6.

---

## Step 6 — Validate and close

### Visual check (if new screens were added)
- Present new screenshots alongside existing design system
- Verify visual consistency with the rest of the product

### Present summary to user:

```
FEATURE ADDED: [Feature name]

New screens/pages: [count]
Specs implemented: [count]
Tests written: [count]
Artifacts patched: [list]
Regressions found: [count] (all fixed)

The feature is integrated and ready for deployment.
```

### Gate — Feature completion

#### Automatic checks
- [ ] All new specs implemented
- [ ] Critical specs have tests
- [ ] Existing test suite still passes (no regressions)
- [ ] Security review: no Critical issues
- [ ] New screens match design system (`DESIGN.md`)
- [ ] `docs/09-development-plan.md` updated with new stories
- [ ] `docs/context.md` updated with feature addition entry

#### User validation (blocking)
- [ ] User has tested the new feature
- [ ] User confirms no visible regressions in existing features

### Verdict: PASS | FAIL

When validated: **"Feature '[name]' added successfully. Ready for deployment."**
