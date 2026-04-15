# Audit — Validate All Artifacts Without Re-Executing

**Mode:** Read-only. This step reads every deliverable and checks content completeness + cross-phase consistency. It does NOT create, modify, or re-execute any phase.

## Purpose

Use `--audit` when you want to verify that all workflow artifacts are complete and consistent — after a pause, after manual changes, before resuming work, or before Phase 9 kicks off. It catches gaps that would cause problems downstream without spending time re-running phases.

---

## Step 1 — Discover existing artifacts

Read `docs/context.md` to determine:
- Which phases have been completed (look for "Phase N" sections)
- The `product_type` and `target_platform`

Then scan for all expected artifacts:

```
docs/01-discovery.md
docs/02-positioning.md
docs/03-product-strategy.md
docs/04-psychology.md
docs/05-copy/*.md
docs/06-architecture.md
docs/06-tech-rules.md
docs/07-product-architecture.md
docs/08-ui-design.md
DESIGN.md
docs/09-development-plan.md
docs/09-spec-variance.md
docs/10-launch/
docs/context.md
```

For each file: note if it exists or is missing. Missing files for incomplete phases are expected — only flag missing files for phases that `context.md` says are complete.

---

## Step 2 — Content validation per phase

For each completed phase, read the artifact and check its content against the required elements. Grade each check as PASS, WARN, or FAIL.

### Phase 1 — Discovery (`docs/01-discovery.md`)
- [ ] `product_type` explicitly defined (web | mobile | saas | e-commerce | api | hybrid)
- [ ] `target_platform` explicitly defined (web | ios | android | cross-platform | backend)
- [ ] Target audience described (who they are, what problem they have)
- [ ] Website/app goals listed (sell | lead gen | audience | credibility)
- [ ] Constraints documented (budget, timeline, technical preferences)
- [ ] Direction chosen with rationale

### Phase 2 — Positioning (`docs/02-positioning.md`)
- [ ] Primary persona defined with at least 5 fields (name/role, frustrations, goals, behavior, demographics)
- [ ] Positioning statement in 1-2 clear sentences
- [ ] Brand voice described (3 adjectives or equivalent)
- [ ] At least 2 differentiators listed vs alternatives
- [ ] For mobile: app store positioning angle present
- [ ] For SaaS: pricing positioning mentioned (free tier? freemium? enterprise?)

### Phase 3 — Product Strategy (`docs/03-product-strategy.md`)
- [ ] Complete list of screens/pages/endpoints with job of each one
- [ ] 3-5 core user flows described (start → end)
- [ ] For web: blog/content decision documented
- [ ] For web: lead capture mechanism defined
- [ ] For mobile: onboarding flow described
- [ ] For SaaS: features by tier/role listed
- [ ] For API: endpoints with operations listed

### Phase 4 — Psychology (`docs/04-psychology.md`)
- [ ] At least 3 psychological levers identified
- [ ] Each lever mapped to a specific screen/page/section (not vague)
- [ ] Ethical guardrails documented (what NOT to do)
- [ ] For mobile: onboarding psychology addressed
- [ ] For e-commerce: purchase/cart psychology addressed

### Phase 5 — Content & Copy (`docs/05-copy/`)
- [ ] One .md file exists for every screen/page listed in Phase 3
- [ ] Each file has: headline (or screen title), CTA (or primary action), body copy
- [ ] `meta-seo.md` exists (if web product)
- [ ] For mobile: app store description exists
- [ ] No placeholder text ("Lorem ipsum", "[TODO]", "placeholder")
- [ ] CTAs match the user flows from Phase 3

### Phase 6 — Tech Architecture (`docs/06-architecture.md` + `docs/06-tech-rules.md`)
- [ ] Stack chosen with exact technology names
- [ ] Project directory structure defined
- [ ] Integrations listed with environment variables
- [ ] Deployment plan documented
- [ ] Cost estimate present
- [ ] At least 2 ADRs from adversarial review (scalability, alternatives, risks)
- [ ] `docs/06-tech-rules.md` exists with:
  - [ ] Stack table with version numbers
  - [ ] MUST section (at least 3 rules)
  - [ ] MUST NOT section (at least 2 rules)
  - [ ] Naming conventions
  - [ ] Error handling contract

### Phase 7 — Product Architecture (`docs/07-product-architecture.md`)
- [ ] Screen/page hierarchy defined
- [ ] Navigation structure (header/footer, tab bar, drawer — depends on product type)
- [ ] URL/route patterns listed
- [ ] All screens/pages from Phase 3 appear in the architecture
- [ ] For mobile: deep link schema defined
- [ ] For mobile: navigator tree (stack/tab/drawer) described
- [ ] "Phase 8 design targets" section exists (ordered list for design)

### Phase 8 — UI Design (`DESIGN.md` + `docs/08-ui-design.md`)
- [ ] `DESIGN.md` exists at project root with:
  - [ ] Color palette with hex codes (at least primary, secondary, background, text)
  - [ ] Typography (font names, weights, sizes for headings/body/small)
  - [ ] Component styles (buttons, cards, inputs, navigation)
  - [ ] Tailwind/theme config or equivalent
- [ ] `docs/08-ui-design.md` exists documenting the approach (Path A, B1, or B2)
- [ ] Every public screen/page from Phase 7 has either:
  - A Stitch screenshot in `.stitch/designs/[slug]/` (Paths B1/B2), OR
  - A composition description in DESIGN.md (Path A)

### Phase 9 — Code (`docs/09-development-plan.md`)
- [ ] User stories in Given/When/Then format
- [ ] Specs categorized (Infra/Backend/Frontend/Integrations)
- [ ] Critical specs marked (money, auth, user data, DB writes)
- [ ] FR Coverage Map present (every Phase 3 feature → at least 1 story)
- [ ] Sprint plan with names and story groupings
- [ ] Sprint retrospectives documented (if sprints completed)

### Phase 10 — Launch (`docs/10-launch/`)
- [ ] `launch-checklist.md` exists
- [ ] `cro-audit.md` exists with no unresolved critical issues
- [ ] Performance baseline documented
- [ ] Launch plan with pre-launch, launch day, post-launch sections

---

## Step 3 — Cross-phase consistency checks

These checks verify that artifacts reference each other correctly.

### Copy ↔ Strategy alignment
Read `docs/03-product-strategy.md` and list every screen/page/endpoint. Then check `docs/05-copy/` — every item from Phase 3 must have a corresponding copy file.
- **FAIL** if any screen/page has no copy file
- **WARN** if copy file exists but is incomplete (missing CTA or body)

### Navigation ↔ Strategy alignment
Read `docs/07-product-architecture.md` and verify every screen/page from Phase 3 appears in the navigation or route map.
- **FAIL** if any screen/page is missing from navigation
- **WARN** if screen exists in nav but with different name/slug

### Design ↔ Navigation alignment
Read Phase 7's design targets. For each public screen/page, verify a design reference exists (Stitch screenshot or DESIGN.md description).
- **FAIL** if a public page has no design reference at all
- **WARN** if design exists but only as DESIGN.md tokens (no mockup)

### Tech rules ↔ Platform skill alignment
Read `docs/06-tech-rules.md`. If `code-flutter` is the platform skill, check that tech rules mention the same state management, navigation, and architecture patterns as `code-flutter` conventions.
- **WARN** if tech rules say "BLoC" but platform skill defaults to "Riverpod"
- **PASS** if aligned or if tech rules explicitly override with an ADR

### Stories ↔ Strategy alignment (FR Coverage Map)
If `docs/09-development-plan.md` exists, check the FR Coverage Map. Every feature from Phase 3 must be covered by at least one user story.
- **FAIL** if any Phase 3 feature has zero stories
- **WARN** if a feature has stories but no acceptance criteria

### Psychology ↔ Copy alignment
Read `docs/04-psychology.md` lever-to-page mapping. For each mapped lever, check that the corresponding copy file contains language that implements that lever (social proof → testimonials, urgency → time-limited language, etc.).
- **WARN** if a lever is mapped but not visible in the copy (subjective — flag for human review)

### Context.md completeness
Read `docs/context.md`. Verify it has a section for every completed phase. Check that `product_type` and `target_platform` are set.
- **FAIL** if product_type or target_platform missing
- **WARN** if a completed phase has no section in context.md

---

## Step 4 — Generate audit report

Produce the report in this format and present it to the user. Do NOT save it to a file — display it directly.

```markdown
# AUDIT REPORT — [Project Name]
**Date:** [today]
**Product type:** [from context.md]
**Platform:** [from context.md]
**Phases completed:** [N] / 10

## Per-Phase Results

| Phase | Artifact | Status | Issues |
|-------|----------|:------:|--------|
| 1 Discovery | docs/01-discovery.md | PASS | — |
| 2 Positioning | docs/02-positioning.md | PASS | — |
| 3 Strategy | docs/03-product-strategy.md | WARN | 1 flow missing end state |
| 4 Psychology | docs/04-psychology.md | PASS | — |
| 5 Copy | docs/05-copy/ | FAIL | referral.md missing |
| 6 Architecture | docs/06-architecture.md | PASS | — |
| 6 Tech Rules | docs/06-tech-rules.md | WARN | No error handling contract |
| 7 Navigation | docs/07-product-architecture.md | FAIL | /referral not in routes |
| 8 Design | DESIGN.md | PASS | — |
| 9 Code | docs/09-development-plan.md | N/A | Phase not started |
| 10 Launch | docs/10-launch/ | N/A | Phase not started |

## Cross-Phase Consistency

| Check | Status | Detail |
|-------|:------:|--------|
| Copy ↔ Strategy | FAIL | referral.md missing for /referral page |
| Nav ↔ Strategy | FAIL | /referral route not in Phase 7 |
| Design ↔ Nav | PASS | All public pages have mockups |
| Tech rules ↔ Platform | PASS | Riverpod aligned with code-flutter |
| Stories ↔ Strategy | N/A | Phase 9 not started |
| Psychology ↔ Copy | PASS | All levers visible in copy |
| Context.md | WARN | Phase 4 section is sparse |

## Summary

- **Critical (FAIL):** 2 issues — must fix before proceeding
- **Warnings (WARN):** 2 issues — should fix, can proceed
- **Passing:** 8 phases clean

## Recommended Actions

1. Create `docs/05-copy/referral.md` — write copy for the referral page
2. Add `/referral` route to `docs/07-product-architecture.md`
3. Consider: add error handling contract to `docs/06-tech-rules.md`
4. Consider: expand Phase 4 section in `docs/context.md`

To fix: run `/launch --from 5` (copy), then `/launch --from 7` (navigation).
Or use the Change Request mechanism for surgical patches.
```

---

## Important rules

- **Read-only mode.** Do not create, modify, or delete any file. This is an audit, not a fix.
- **Grade honestly.** If something is incomplete, say so. The user wants truth, not reassurance.
- **Be specific.** "FAIL: referral.md missing" is useful. "FAIL: some copy missing" is not.
- **Suggest fixes.** For every FAIL and WARN, recommend the exact action (which file, which phase to re-run, or use Change Request).
- **Skip incomplete phases.** If a phase hasn't been run yet (no artifact + no context.md section), mark it N/A — don't flag it as FAIL.
