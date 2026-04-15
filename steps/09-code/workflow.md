# Phase 9 — Code (Spec-Driven Development)

**Skills:** `/code`, `stack-web-frontend`, `stack-web-backend`, `code-flutter` (Flutter projects), `stack-ios-swift`, `stack-android-kotlin`, `stack-mobile-rn`, `image-gen`, `webapp-testing`, `/simplify`, `security-review`, `security-review-mobile` (mobile projects), `/frontend-design` (safety net)
**Optional:** `test-driven-development` (with `--tdd`)

## Purpose

All strategic and design work is complete. This phase builds the product — faithfully implementing what was designed, not reinventing it. The workflow is structured: spec generation → sprint planning → dev cycles → integration tests → visual fidelity → retro.

Each sub-step is its own file. Read and execute them one at a time in order.

---

## Step 0 — Load all deliverables

Before reading any sub-step file, load the full context from prior phases:

- `docs/01-discovery.md` through `docs/07-product-architecture.md` — strategic context
- `DESIGN.md` — design system tokens (colors, fonts, spacing, components)
- `docs/08-ui-design.md` — design approach used (Path A, B1, or B2)
- `docs/06-architecture.md` — tech stack, project structure, integrations, services
- `docs/06-tech-rules.md` — stack versions, critical rules, forbidden patterns
- `docs/07-product-architecture.md` — navigation, URLs, page hierarchy
- `docs/05-copy/` — all page copy (headlines, CTAs, body text)

### Step 0b — Identify design references based on Phase 8 path

Read `docs/08-ui-design.md` to determine which path was used:

**If Path B2 (Claude piloted Stitch via MCP):**
- Read `.stitch/registry.json` to get the list of validated screens
- Screenshots in `.stitch/designs/[page-slug]/screenshot.png` are the composition reference for each page
- HTML in `.stitch/designs/[page-slug]/index.html` for CSS value consultation
- `DESIGN.md` provides the design system tokens

**If Path B1 (user drove Stitch manually):**
- Same as B2 — screenshots are composition reference, HTML for CSS details, DESIGN.md for tokens
- If only screenshots (no HTML), rely on screenshot + DESIGN.md

**If Path A (Claude generated the design system):**
- Only `DESIGN.md` exists — it is the sole reference for both tokens and composition
- Copy files in `docs/05-copy/` define page structure — section ordering IS the page layout
- If DESIGN.md lacks detail about page composition, ask the user before coding

Store the detected path (A / B1 / B2) in working memory. Every sub-step that touches design references will need it.

---

## Execution order

Read and execute each sub-step file in sequence. Do not skip ahead. Do not start the next sub-step until the current one is complete and any user validation has been received.

```
1. Read and execute: 09a-spec-generation.md
2. Read and execute: 09b-sprint-planning.md
3. Read and execute: 09b2-test-plan.md  ← Generate the interactive test plan BEFORE coding
4. For each sprint:
   a. Read and execute: 09c-dev-cycle.md
   b. Read and execute: 09d-integration-tests.md  ← Execute tests + auto-fix + report
   c. Read and execute: 09f-retro.md
5. After all sprints:
   a. Read and execute: 09e-visual-fidelity.md  (if project has UI)
6. Run Phase 9 gate (see below)
```

**Note on 09b2-test-plan.md:** Generates the complete interactive test plan from analysis docs (Phases 3-5) BEFORE any code is written. This plan drives 09d — developers know what will be tested, and the test runner has a complete script.

**Note on 09e-visual-fidelity.md:** Run it once after all sprints are complete, not after each sprint. The goal is a final comprehensive visual review, not incremental checkpoints per sprint.

**Note on 09d-integration-tests.md:** Run it after each sprint. Executes the test plan from 09b2, auto-fixes failures using the appropriate platform skill, retests, and produces a report for the user. Also triggered by `--add-feature` and change requests.

---

## Gate — Critères de passage vers Phase 10

Run this gate after all sprints, integration tests, and visual fidelity are complete.

### Automatiques (Claude vérifie)
- [ ] L'application démarre sans erreur (`npm run dev` ou équivalent)
- [ ] `docs/09-development-plan.md` existe avec user stories, specs, et statut des sprints
- [ ] Toutes les pages de `docs/07-product-architecture.md` sont accessibles (pas de 404, pas de crash)
- [ ] Les specs critiques (paiement, auth, données perso, DB writes) ont des tests
- [ ] `security-review` ne remonte aucun issue "Critical"
- [ ] Les integration tests passent (voir `docs/09-development-plan.md` section Integration Tests)
- [ ] `docs/context.md` mis à jour avec la section Phase 9

### Validation utilisateur (bloquant)
- [ ] L'utilisateur a validé chaque sprint (après 09c-dev-cycle de chaque sprint)
- [ ] L'utilisateur a validé la fidélité visuelle (après 09e-visual-fidelity)

### Verdict: PASS | WARN | FAIL

| Verdict | Condition | Action |
|---|---|---|
| **PASS** | Tous les critères automatiques OK + validations utilisateur reçues | Passer à Phase 10 |
| **WARN** | Issues non-critiques restantes, documentées dans 09-spec-variance.md | Lister les variances, obtenir l'accord utilisateur, passer à Phase 10 |
| **FAIL** | Un critère critique échoue (app ne démarre pas, issue sécurité Critical, tests critiques manquants) | Corriger avant de continuer |

When validated: **"Phase 9 complete. Moving to Phase 10 — Optimization & Launch."** Then read `steps/10-launch.md`.
