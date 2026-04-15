# Gate — Launch Readiness

This gate runs between Phase 9 (Code) and Phase 10 (Optimization & Launch). It verifies the product is production-ready before any public exposure.

## When to run

Run this gate after Phase 9 completes and before starting Phase 10. If invoked manually (e.g., `--from 10`), run this gate first before executing any Phase 10 steps.

---

## Check 1 — Integration tests

**Criterion:** All integration tests pass on the latest build.

Verify:
- `docs/09-development-plan.md` references integration test results
- No failing tests in the CI output (or local run)
- E2E flows covered: happy path for each critical user journey (auth, payment, core feature)

**Failure condition:** Any failing integration test, or no integration tests exist for a critical flow (payment, auth).

---

## Check 2 — Security review

**Criterion:** No Critical severity issues open.

Verify:
- Security review was run during Phase 9 (evidence in `docs/09-development-plan.md` or a dedicated security report)
- Zero Critical issues unresolved
- High issues: documented with accepted-risk rationale or fix plan

**Failure condition:** Any open Critical issue. High issues without documented rationale also block.

---

## Check 3 — Performance baseline

**Criterion:** Baseline captured; critical thresholds met.

Verify that `docs/10-launch/performance-baseline.md` exists OR that a preliminary baseline was captured at end of Phase 9.

Minimum thresholds (override in `docs/context.md` if product has different SLAs):

| Product type | Threshold |
|---|---|
| Web / Landing page | Lighthouse Performance ≥ 80, LCP < 2.5 s, CLS < 0.1 |
| SaaS web app | Lighthouse Performance ≥ 70 (app shell), LCP < 3 s |
| Mobile app | Cold startup < 3 s, frame rate ≥ 60 fps on primary scroll |
| API | p95 response time < 500 ms under expected load |

**Failure condition:** Thresholds not met, or no baseline captured at all.

---

## Check 4 — Legal pages

**Criterion:** Required legal pages exist and are linked from the product.

**Web / SaaS:**
- [ ] Privacy policy page exists (URL: `/privacy` or equivalent)
- [ ] Terms of service page exists (URL: `/terms` or equivalent)
- [ ] Both linked in the site footer or app settings
- [ ] Cookie consent implemented if targeting EU users (GDPR)

**Mobile:**
- [ ] Privacy policy URL set in App Store / Play Console listing
- [ ] Privacy policy accessible from within the app (Settings screen)

**Failure condition:** Missing privacy policy (always blocking). Missing ToS is blocking for SaaS and e-commerce; WARN for simple landing pages.

---

## Check 5 — Analytics configured

**Criterion:** At least one analytics tool is configured and receiving events.

Verify:
- Analytics SDK / script is present in the codebase
- At least one meaningful event tracked beyond page views (e.g., CTA click, signup, purchase)
- Test event visible in the analytics dashboard (or documented as verified)

**Failure condition:** No analytics configured. Deploying without analytics means flying blind post-launch.

---

## Check 6 — Error monitoring configured

**Criterion:** Error monitoring is set up and operational (if applicable).

Applicable to: SaaS, mobile apps, e-commerce, APIs. Optional but recommended for landing pages.

Verify:
- Error monitoring tool integrated (Sentry, Bugsnag, Datadog, Crashlytics, etc.)
- A test error was triggered and appeared in the dashboard
- Alerting rules set for critical errors (or documented as pending)

**Failure condition (blocking for SaaS / mobile / API):** Error monitoring not configured. A production crash with no visibility is unacceptable for these product types.

**Failure condition (WARN for landing pages):** Not configured — document as accepted risk.

---

## Verdict

Evaluate all 6 checks and produce a verdict:

### PASS
All 6 checks pass. Proceed to Phase 10 immediately.

```
GATE: Launch Readiness — PASS

✓ Integration tests: all passing
✓ Security review: 0 Critical issues
✓ Performance baseline: [score / startup time] — thresholds met
✓ Legal pages: privacy + ToS present and linked
✓ Analytics: configured and receiving events
✓ Error monitoring: configured and operational

→ Cleared for Phase 10 — Optimization & Launch
```

### WARN
All Critical criteria pass; one or more non-critical criteria have issues. Can proceed with user acknowledgment.

```
GATE: Launch Readiness — WARN

✓ Integration tests: all passing
✓ Security review: 0 Critical issues
⚠ Performance baseline: LCP 2.8 s (threshold: 2.5 s) — acceptable risk documented
✓ Legal pages: present and linked
✓ Analytics: configured
⚠ Error monitoring: not configured (accepted risk — landing page)

→ Proceed to Phase 10 with acknowledgment. Fix WARN items in Phase 10 if time permits.
```

Requires explicit user acknowledgment: "I accept the WARN items and want to proceed."

### FAIL
One or more Critical criteria fail. Phase 10 is blocked.

```
GATE: Launch Readiness — FAIL

✗ Integration tests: 2 tests failing (checkout flow, email confirmation)
✓ Security review: 0 Critical issues
✓ Performance baseline: thresholds met
✗ Legal pages: privacy policy missing
✓ Analytics: configured
✓ Error monitoring: configured

→ BLOCKED. Fix the following before Phase 10:
  1. Fix failing integration tests (checkout flow + email confirmation)
  2. Create and link privacy policy page

Re-run this gate after fixes are applied.
```

---

## How to re-run this gate

After fixing FAIL items, re-run by reading this file and evaluating all 6 checks against the current state of `docs/` and the codebase. A gate re-run takes priority over starting Phase 10 steps.
