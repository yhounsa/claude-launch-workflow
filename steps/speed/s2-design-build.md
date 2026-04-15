# Speed Mode — Phase S2: Design & Build (30–60 min)

**Mode:** Speed (`--speed`)
**Time budget:** 30 minutes for 1-page product, 60 minutes for 2–3 screens
**Input required:** `docs/speed-brief.md` (from S1)
**Output:** Working product

---

## Purpose

Design and code the product in a single pass, using the copy from S1 verbatim. No separate design approval loop, no sprint planning, no category-by-category dev cycles. Start to finish, one run.

---

## Step 1 — Generate design

Read `docs/speed-brief.md` before generating anything. Use the real copy, real screen list, and product type from S1.

**Path A — `/ui-ux-pro-max` skill (default for speed mode):**

Locate and read the `ui-ux-pro-max` skill's SKILL.md. Use it to generate `DESIGN.md`:
- Style: infer from the product type and audience, or ask in one question
- Color palette: select one, do not offer options
- Typography: select one pairing, do not offer options
- Component list: derived from the screen/page list in `docs/speed-brief.md`

Save result to `DESIGN.md`.

**Path B — Stitch MCP (use only if MCP is available and confirmed active):**

If the Stitch MCP is available, use a single wave for all screens in parallel. Do not use the multi-wave model from Full mode — that's for thoroughness, not speed.

1. Locate and read the `stitch-design` skill's SKILL.md
2. Create a single Stitch project
3. Generate all screens simultaneously in one batch call
4. Save the Stitch project ID in `docs/context.md`
5. Export the design system and save to `DESIGN.md`

Do not wait for screen review between screens. Generate all, then proceed to Step 2.

---

## Step 2 — Code everything in one pass

Read `DESIGN.md` and `docs/speed-brief.md`. Code the entire product in sequence, using the auto-selected stack from S1.

Rules:
- Use **real copy from `docs/speed-brief.md`**. Zero placeholders (`[Your headline]`, `Lorem ipsum`, etc.).
- Follow the design system in `DESIGN.md` exactly (colors, typography, spacing, component patterns).
- Mobile-first if web. Responsive is non-negotiable.
- No category separation (no "backend first, then frontend"). Code the full vertical slice for each screen/page, then move to the next.

Code order:
1. Project scaffold (stack setup, config, routing)
2. Shared components (layout, nav, footer, design tokens)
3. Screen/page 1 — full implementation
4. Screen/page 2 — full implementation (if applicable)
5. Screen/page 3 — full implementation (if applicable)
6. Forms, integrations, env vars

---

## Step 3 — Tests: critical specs only

Write tests only for flows that are unrecoverable if they break:

| Flow | Test type |
|---|---|
| Payment / checkout | Integration test — happy path + decline |
| Auth (signup, login, logout) | Integration test — happy path + invalid credentials |
| Form submission (lead capture, contact) | Integration test — submit + error state |
| API endpoints (if applicable) | Contract test — response shape + status codes |

Skip unit tests on pure UI components. Skip snapshot tests. Skip testing copy content.

If none of the above flows exist in this product, skip tests entirely. State "No critical flows requiring tests in this product."

---

## Step 4 — Security: one global pass

Run one pass of the `security-review` skill (locate its SKILL.md in the sibling skills directory). Apply it globally across the codebase — not per-category.

Focus only on:
- Exposed secrets / API keys in code or committed env files
- XSS vectors in user-rendered content
- CSRF on state-changing forms
- Auth bypass if auth exists

Fix any issues found immediately. Do not defer.

Skip: rate limiting analysis, GDPR deep audit, advanced infrastructure review. Those are Full mode concerns.

---

## Step 5 — Quick visual check

Open the product in a browser (or describe how to do so) and verify:

- [ ] Primary CTA is visible without scrolling on desktop
- [ ] Primary CTA is visible without scrolling on mobile (375px viewport)
- [ ] Copy matches `docs/speed-brief.md` (no stray placeholders)
- [ ] No obvious layout breaks at 375px, 768px, 1280px

This is a visual spot-check, not a formal side-by-side against mockups. No screenshot comparison tool. Just eyes on the output.

If a layout break is found, fix it before proceeding to S3.

---

## Output

At the end of this phase:
- `DESIGN.md` — design system (style, palette, typography, components)
- Working product (all screens/pages coded, tested, security-checked)
- `docs/context.md` updated with:

```markdown
## Phase S2 — Design & Build
Design path: [Path A: ui-ux-pro-max | Path B: Stitch — project ID: XXXXXX]
Stack: [stack used]
Screens built: [count]
Tests written: [what was tested, or "none — no critical flows"]
Security: global pass complete
Visual check: passed
```

Tell the user the product is ready and how to run it locally. Then proceed to S3.

---

## What this phase intentionally skips

- Formal design approval before coding
- Sprint planning and sprint retrospectives
- Per-category security review
- Visual fidelity comparison (mockup vs. screenshot)
- Adversarial spec review
- Given/When/Then user stories

All of the above exist in Full mode. Speed mode ships first, iterates after.
