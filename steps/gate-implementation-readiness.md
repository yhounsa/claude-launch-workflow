# Gate — Implementation Readiness Check

**When:** Between Phase 8 (Design) and Phase 9 (Code). Run automatically at the start of Phase 9 before any code is written.

**Purpose:** Validate that all strategic and design artifacts are aligned and complete. A single gap discovered mid-sprint costs 10x more than catching it here.

---

## The 6 Checks

### Check 1 — Copy Coverage

**Question:** Does every screen/page from Phase 3 have corresponding copy in `docs/05-copy/`?

1. Read `docs/03-product-strategy.md` — extract the list of all screens/pages defined
2. List all files in `docs/05-copy/` — each file should correspond to one screen/page
3. For each screen in Phase 3: verify a matching copy file exists and is non-empty

**Pass:** Every screen has a copy file with headlines, body text, and at least one CTA.
**Fail:** Any screen is missing copy, or copy file is a stub (no actual content).

---

### Check 2 — Architecture Completeness

**Question:** Do `docs/06-architecture.md` and `docs/06-tech-rules.md` exist and contain required content?

1. Check `docs/06-architecture.md` exists — must contain: tech stack, project structure, integrations list, services/dependencies
2. Check `docs/06-tech-rules.md` exists — must contain: stack + exact versions, critical rules, forbidden patterns
3. Verify both documents are coherent (stack declared in architecture matches rules)

**Pass:** Both files exist, both have substantive content, no contradictions.
**Fail:** Either file is missing. Partial fail: file exists but is a stub or missing sections.

---

### Check 3 — Product Architecture Alignment

**Question:** Does every screen/page in Phase 7 (site map) map to an entry in Phase 3 (content strategy)?

1. Read `docs/07-product-architecture.md` — extract every URL/page declared
2. Read `docs/03-product-strategy.md` — extract every screen/page defined
3. For each page in the site map: verify it traces to a Phase 3 entry (exact match or clear parent-child)

Orphan pages (in site map but not in content strategy) are a red flag — they have no strategic rationale and no copy.

**Pass:** Every page in the site map traces to Phase 3. Utility pages (404, redirects) are exempt.
**Fail:** Any public-facing page exists in the site map without a Phase 3 counterpart.

---

### Check 4 — Design Coverage

**Question:** Does every public screen/page have a design reference?

1. Read `docs/08-ui-design.md` to determine the Phase 8 path used:
   - **Path B2 / B1:** Read `.stitch/registry.json` — every public page must have a validated screen entry
   - **Path A:** Read `DESIGN.md` — must contain composition guidance (section order, layout) for every page, not just tokens

2. Cross-reference against the site map from Check 3.

**Pass:**
- Path B1/B2: Every public page has a `.stitch/designs/[slug]/screenshot.png`
- Path A: `DESIGN.md` covers every page's composition (not just global tokens)

**Fail:** Any public page has no design reference. The developer would be inventing the design during coding — a scope risk.

---

### Check 5 — Cross-Artifact Consistency

**Question:** Are strategic decisions visible end-to-end across artifacts?

Run three consistency sub-checks:

**5a — Integrations have CTAs:**
- Read `docs/06-architecture.md` — list all integrations (payment, email, analytics, etc.)
- For each integration, verify at least one CTA in `docs/05-copy/` triggers or references it
- Example: Stripe integration → checkout CTA exists in copy. Email integration → newsletter signup CTA exists.

**5b — Psychology levers are reflected in copy AND design:**
- Read `docs/04-psychology.md` (Full mode) or `docs/04-psychology-copy.md` (Light mode) — list the psychological levers identified (social proof, scarcity, authority, etc.)
- For each lever: verify it appears in at least one copy file AND is visible in the design (mockup element or DESIGN.md component)
- Example: "social proof" lever → testimonials section exists in copy AND in at least one Stitch screen

**5c — No phantom features:**
- Any component in `DESIGN.md` or Stitch mockups that has no corresponding copy and no Phase 3 rationale is a phantom — it was invented during design without strategic grounding
- List any phantoms found

**Pass:** All integrations have CTAs. All psychology levers are reflected in both copy and design. No phantom features, or phantoms are explicitly approved.
**Fail:** An integration has no copy entry. A lever appears only in strategy but not in copy or design. Unapproved phantoms exist.

---

### Check 6 — Tech Rules Present

**Question:** Does `docs/06-tech-rules.md` specify the stack with exact versions and define critical rules?

Verify the document contains ALL of:
- [ ] Framework name + version (e.g., "Next.js 14.2.x")
- [ ] Runtime + version (e.g., "Node.js 20 LTS")
- [ ] Database + ORM + versions if applicable
- [ ] At least 3 critical rules (things that MUST or MUST NOT be done)
- [ ] Forbidden patterns (e.g., "no raw SQL", "no inline styles", "no `any` in TypeScript")

**Pass:** All 5 items are present and specific (no "latest" version references).
**Fail:** Any item is missing or uses vague version references ("latest", "current").

---

## Scoring and Verdict

Count the checks that pass (each of the 6 checks is binary: pass or fail).

| Score | Verdict | Meaning |
|---|---|---|
| 6/6 | **READY** | All checks pass. Proceed to Phase 9 coding. |
| 4–5/6 | **NEEDS WORK** | Minor gaps. Document the issues, fix them, then proceed. |
| < 4/6 | **NOT READY** | Critical gaps. Do not start coding until resolved. |

**Special rule:** A FAIL on Check 2 (architecture missing) or Check 6 (tech rules missing) is automatically NOT READY regardless of other scores — you cannot code without knowing what stack to use.

---

## Output

Present the results as a table:

```
## Implementation Readiness Check

| # | Check | Result | Notes |
|---|---|---|---|
| 1 | Copy Coverage | PASS / FAIL | [details] |
| 2 | Architecture Completeness | PASS / FAIL | [details] |
| 3 | Product Architecture Alignment | PASS / FAIL | [details] |
| 4 | Design Coverage | PASS / FAIL | [details] |
| 5 | Cross-Artifact Consistency | PASS / FAIL | [details] |
| 6 | Tech Rules Present | PASS / FAIL | [details] |

**Verdict: READY / NEEDS WORK / NOT READY**
```

**If NEEDS WORK:** List the specific gaps and ask the user to address them or explicitly approve skipping each one before continuing.

**If NOT READY:** Stop. Do not proceed to code generation. List all gaps and return to the appropriate phase to fix them.

**If READY:** State "Implementation readiness confirmed. Loading Phase 9 workflow." and continue to `09-code/workflow.md`.
