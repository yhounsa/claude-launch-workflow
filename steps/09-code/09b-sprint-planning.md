# 09b — Sprint Planning

**Input:** Validated `docs/09-development-plan.md` from 09a
**Output:** Sprint plan appended to `docs/09-development-plan.md`, validated by user
**Gate:** User validates sprint plan before any code is written

---

## Step 1 — Group stories into sprints by theme

Look at all user stories in the dev plan. Group them into sprints based on functional theme and technical dependencies. Aim for 2–4 sprints total. Each sprint should deliver something demonstrable.

**Suggested themes (adapt to project reality):**
- Sprint 1: "Foundation & Core Experience" — Infra scaffold + core user flows (auth if applicable, main content pages)
- Sprint 2: "Content & Engagement" — content pages, media, interactive features
- Sprint 3: "Commerce & Integrations" — payment, email, third-party APIs
- Sprint 4: "Admin & Ops" — admin interfaces, analytics, operational tools

**Rules for grouping:**
- Infra specs always go in Sprint 1 (everything else depends on them)
- Auth specs always go in Sprint 1 if auth is required (everything else that needs auth depends on it)
- Payment specs always go last among sprints with external dependencies (most complex, highest risk)
- A sprint should have a coherent theme — it should be possible to describe it in one sentence

---

## Step 2 — Define each sprint

For each sprint, document:

```markdown
### Sprint [N]: [Name]

**Theme:** [One sentence description of what this sprint delivers]

**Stories included:** US[X], US[Y], US[Z]

**Categories involved:**
- Infra: [N] specs
- Backend: [N] specs
- Frontend: [N] specs
- Integrations: [N] specs

**Critical spec count:** [N] specs requiring TDD auto

**Estimated deliverables:**
- [N] pages / screens
- [N] API endpoints
- [N] integrations configured

**Dependencies:**
- Depends on: Sprint [X] (requires [specific system, e.g., "auth system", "DB schema"])
- Blocks: Sprint [X] (provides [specific output])
```

A sprint with 0 dependencies is standalone and could theoretically run first. However, respect the Infra-first ordering rule above regardless.

---

## Step 3 — Present sprint plan for user validation

Present the complete sprint plan before doing anything else:

> "Voici le plan des sprints. [N] sprints au total. Sprint 1 livre [theme], Sprint 2 livre [theme], etc. Est-ce que cette organisation te convient ? Des stories à déplacer entre sprints ?"

Wait for the user's response. They may:
- Reorder priorities (move a story from Sprint 3 to Sprint 1)
- Merge sprints (if scope is small enough)
- Split a sprint (if it's too large)
- Defer stories entirely (scope cut)

Update the sprint plan based on feedback before proceeding.

---

## Step 4 — Adversarial review of the plan

After generating the sprint plan (and after user validation), challenge it with these four questions. Answer each question honestly — if a problem is found, fix it before proceeding to coding.

### Question 1: What user flows are NOT covered by any story?

Walk through the complete user journey from first visit to final conversion (or task completion). For each step in the journey, verify it is covered by at least one story.

Common gaps to check:
- Error states (what happens when payment fails? when login fails?)
- Empty states (what does a new user see before they have any data?)
- Loading states (what does the user see while data is fetching?)
- Edge cases (what if a user submits a form twice? refreshes mid-checkout?)
- Mobile-specific interactions (if the product has mobile users)

For each gap found: either add a new story/spec, or document that it is out of scope with explicit approval.

### Question 2: Which stories have undeclared dependencies?

Look for stories where the implementation of Story B implicitly requires Story A to be done first, but the sprint plan doesn't reflect this ordering.

Common patterns:
- A "view order history" story that assumes an "create order" story exists (but is in a later sprint)
- A "send confirmation email" story that assumes a "create user account" story (but the user creation story is vague)
- A Frontend story that calls a Backend API that isn't in the same or earlier sprint

For each undeclared dependency found: either reorder the sprints/stories, or make the dependency explicit in the sprint plan.

### Question 3: What edge cases are missing in the acceptance criteria?

Review the Given/When/Then criteria in 09a for completeness. For each critical story (money, auth, user data), verify:
- Is there an error scenario (unhappy path)?
- Is there a boundary case (what happens at the limit)?
- Is there a concurrent-use scenario if relevant (two users doing the same thing simultaneously)?

For each missing scenario: add a new Given/When/Then block to the relevant story in `docs/09-development-plan.md`.

### Question 4: Are sprint sizes balanced?

Review the estimated deliverables per sprint. If one sprint has 3x the specs of another:
- Either rebalance by moving stories
- Or explicitly note that the sprint is intentionally large (and adjust expectations)

---

## Step 5 — Finalize and update the dev plan

After the adversarial review, update `docs/09-development-plan.md` with:
1. The sprint plan (from Step 2)
2. Any new stories or acceptance criteria added during adversarial review
3. Any scope deferrals explicitly approved by the user

Then confirm to the user:

> "Plan de sprints finalisé. Review adversariale complète — [N] gaps trouvés et résolus. Prêt à démarrer Sprint 1 ?"

Proceed to `09c-dev-cycle.md` with Sprint 1.
