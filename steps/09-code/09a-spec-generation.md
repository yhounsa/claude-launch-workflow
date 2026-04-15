# 09a — Spec Generation

**Input:** Deliverables loaded in workflow.md Step 0
**Output:** `docs/09-development-plan.md` with user stories, Given/When/Then acceptance criteria, technical specs, and FR Coverage Map
**Gate:** User validates before any code is written

---

## Step 1 — Extract user stories from deliverables

Read the deliverables and extract user stories. Use all four sources:

| Source | What it yields |
|---|---|
| `docs/03-product-strategy.md` | Each page job → story ("As a visitor, I want to read testimonials so that I trust the product") |
| `docs/05-copy/` | Each CTA → user action ("As a reader, I want to order the book online") |
| `docs/06-architecture.md` | Each integration → user need ("As a customer, I want to pay securely") |
| `docs/04-psychology.md` | Each psychological lever → user need ("As a hesitant visitor, I want social proof to feel confident") |

Read all four sources before drafting stories. A single feature may have contributions from multiple sources — merge them into one story rather than creating duplicates.

---

## Step 2 — Write stories with Given/When/Then acceptance criteria

For each user story, use this format:

```markdown
### US[N]: [Story title]

As a [persona], I want [goal], so that [benefit].

**Acceptance Criteria:**

**Given** [precondition — what state the system or user is in]
**When** [action — what the user does]
**Then** [expected outcome — what should happen]
**And** [additional outcome if needed]

**Specs:**

| Spec | Category | Critical | TDD |
|---|---|---|---|
| [Spec description] | Backend | Yes | Auto |
| [Spec description] | Frontend | No | Only with --tdd |
```

**Persona vocabulary:** Use the personas from `docs/01-discovery.md` or `docs/02-positioning.md` if defined. Otherwise use generic terms: visitor, user, customer, admin.

**One story = one user goal.** Don't bundle unrelated goals into one story. If a story requires more than 6 specs across categories, consider splitting it.

**Multiple acceptance criteria:** A story can have multiple Given/When/Then blocks if it covers distinct scenarios (happy path + error path + edge case). Number them:

```markdown
**Scenario 1 — Happy path:**
**Given** [...]
**When** [...]
**Then** [...]

**Scenario 2 — Error path:**
**Given** [...]
**When** [...]
**Then** [...]
```

---

## Step 3 — Categorize specs

Group all specs from all stories into 4 categories:

| Category | What it covers | When it applies |
|---|---|---|
| **Infra** | Scaffold, config, env vars, CI/CD, deployment, monorepo setup | Every project |
| **Backend** | DB schema, API endpoints, auth, business logic, services | Projects with a backend |
| **Frontend** | UI components, pages, responsive layout, images | Every project |
| **Integrations** | Payment, email, analytics, third-party APIs, storage | Projects with external services |

If a category has no specs (e.g., static site with no backend), mark it as "N/A — not applicable" rather than omitting it.

---

## Step 4 — Mark critical specs

Specs that touch any of the following are **critical** — they receive TDD automatically, even without the `--tdd` flag:

- **Money:** payment processing, checkout, pricing calculations, refunds, coupons
- **Authentication:** login, logout, sessions, JWT, password hashing, permissions, roles
- **User data:** forms collecting personal information, GDPR/RGPD compliance, data deletion
- **Database writes:** migrations, data mutations that affect data integrity

Mark critical specs with `Yes` in the Critical column and `Auto` in the TDD column. This is not optional — bugs in these areas have real consequences (financial loss, data breach, legal liability).

---

## Step 5 — Build the FR Coverage Map

After all stories are written, generate a FR (Functional Requirements) Coverage Map. This matrix verifies:
1. Every feature from Phase 3 is covered by at least one story
2. Every story traces back to at least one feature from Phase 3

```markdown
## FR Coverage Map

| Feature (Phase 3) | Covered by | Story count |
|---|---|---|
| [Feature from docs/03-product-strategy.md] | US3, US7 | 2 |
| [Feature] | US1 | 1 |
| [Feature] | — | 0 ← GAP |

| Story | Traces to |
|---|---|
| US1 | Feature A |
| US2 | Feature B, Feature C |
| US5 | — ← ORPHAN (no Phase 3 rationale) |
```

**Gaps** (features with 0 stories): Either add a missing story or explicitly document why the feature won't be implemented.

**Orphans** (stories with no Phase 3 trace): Either trace to a feature (maybe it was implicit) or flag as scope creep for user review.

---

## Step 6 — Present the development plan

Save the full plan to `docs/09-development-plan.md` with this structure:

```markdown
# Development Plan — Phase 9

## User Stories

### US1: [Story title]
As a [persona], I want [goal], so that [benefit].

**Acceptance Criteria:**
**Given** [...]
**When** [...]
**Then** [...]

**Specs:**
| Spec | Category | Critical | TDD |
|---|---|---|---|
| ... | ... | ... | ... |

[repeat for all stories]

---

## FR Coverage Map

[matrix from Step 5]

---

## Spec Summary

| Category | Total specs | Critical specs | TDD auto |
|---|---|---|---|
| Infra | N | N | N |
| Backend | N | N | N |
| Frontend | N | N | N |
| Integrations | N | N | N |
| **Total** | **N** | **N** | **N** |

Pages with design mockups: N / N total pages

---

## Sprint Log

[Empty at this stage — filled in during 09b-sprint-planning.md]

---

## Spec Variance Log

[Empty at this stage — filled in during 09c-dev-cycle.md]

---

## Integration Test Results

[Empty at this stage — filled in during 09d-integration-tests.md]

---

## Sprint Retrospectives

[Empty at this stage — filled in during 09f-retro.md]
```

Present a summary to the user:

> "Voici le plan de développement. [N] user stories, [N] specs au total, [N] critiques (TDD auto). Couverture FR : [N]/[N] features de Phase 3 couvertes. Des ajustements avant de planifier les sprints ?"

Wait for the user to validate before proceeding to `09b-sprint-planning.md`. They may:
- Reprioritize or defer specs
- Add missing requirements
- Flag orphan stories
- Request story splits or merges
