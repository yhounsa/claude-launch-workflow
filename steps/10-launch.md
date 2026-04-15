# Phase 10 — Optimization & Launch

**Invoke:** `page-cro` skill, then `launch-strategy` skill

## Purpose

A product that's built isn't a product that's launched. This final phase does three things: capture a performance baseline, audit every page or screen for conversion optimization, and create a launch plan so the product doesn't go live into silence.

## Product type adaptations

Before running the steps below, identify the product type from `docs/context.md` and apply the relevant track:

| Product type | Key additions |
|---|---|
| **Web / Marketing site** | CRO audit, SEO check, Lighthouse score, sitemap, robots.txt |
| **Mobile app** | ASO (App Store Optimization), screenshots, rating prompt placement, TestFlight / Play Console submission |
| **SaaS** | Onboarding flow audit, trial-to-paid conversion check, pricing page CRO |
| **API / Backend** | Documentation audit, rate limiting check, monitoring and alerting setup |

---

## Step 1 — Performance baseline

Capture this **before** deploying to production. It serves as a reference for regressions.

**Web:**
- Run Lighthouse (or PageSpeed Insights) on the primary landing page and any conversion-critical page
- Record: Performance score, Accessibility score, Best Practices score, SEO score
- Record: LCP, CLS, INP / FID (Core Web Vitals)
- Record: Total bundle size (JS + CSS, gzipped)

**Mobile:**
- Record: Cold startup time (time to interactive)
- Record: Frame rate on primary scroll surfaces (target ≥ 60 fps)
- Record: App binary size (IPA / APK)

Document results in `docs/10-launch/performance-baseline.md`.

Acceptable thresholds (adjust to product needs):
- Lighthouse Performance ≥ 80
- LCP < 2.5 s
- CLS < 0.1
- Bundle size < 200 KB JS (gzipped) for landing pages

If thresholds are not met, fix before proceeding to deployment.

---

## Step 2 — Conversion optimization

Locate and read the `page-cro` skill's SKILL.md (sibling directory to `launch`).

Run a CRO audit on each major page or screen:
- Is the primary CTA visible without scrolling (above the fold)?
- Does the page / screen flow logically toward the desired action?
- Are trust signals (testimonials, credentials, guarantees, social proof) placed effectively?
- Is the copy benefit-led, not feature-led?
- Is the page fast? (Confirmed in Step 1)
- Is the page accessible? (Alt texts, contrast ratio ≥ 4.5:1, keyboard navigation)
- Does it work correctly on mobile / smallest target device?

Apply fixes for any issues found.

**SaaS additional checks:**
- Onboarding: Is time-to-value < 5 minutes for new users?
- Trial: Is the upgrade path visible and frictionless?
- Pricing: Is the recommended plan visually highlighted?

**Mobile additional checks:**
- Is the rating prompt triggered at the right moment (after a success, not on first launch)?
- Are deep links configured and tested?

---

## Step 3 — SEO / ASO final check

**Web:**
- Every page has a unique title tag (50–60 chars) and meta description (120–155 chars)
- `sitemap.xml` is generated, valid, and submitted to Search Console
- `robots.txt` is in place and not blocking critical paths
- Structured data (schema.org) added where relevant (Product, Article, Person, Organization, etc.)
- Open Graph and Twitter card meta tags set on every page
- Canonical tags set where needed

**Mobile:**
- App title (30 chars max on iOS) includes primary keyword
- Subtitle / short description uses secondary keywords
- Long description: first 3 lines are benefit-led (visible before "more")
- Keywords field (iOS) uses all 100 characters
- Screenshots: first screenshot shows the core value proposition, text overlay applied
- Preview video (optional but recommended)

---

## Step 4 — Launch strategy

Locate and read the `launch-strategy` skill's SKILL.md (sibling directory to `launch`).

Create a launch plan covering:
- **Pre-launch** (1–2 weeks before): teaser content, email to existing list, outreach to relevant communities, early access / waitlist activation
- **Launch day**: go-live checklist, announcement posts (Product Hunt, Twitter/X, LinkedIn, relevant communities), monitoring plan
- **Post-launch** (first 30 days): content publishing cadence, engagement plan, metrics to track, iteration based on data

---

## Step 5 — Post-deploy E2E verification

After deploying to production, re-run integration tests against the **production URL** (not localhost or staging).

This catches environment-specific issues that pass in dev but fail in prod:
- CORS headers on API calls
- Environment variables correctly set (keys, endpoints)
- SSL / HTTPS active and certificate valid
- Third-party integrations live (payment, email, analytics, auth)
- All forms functional end-to-end (not just client-side validation)
- Error monitoring receiving events

If any test fails in production, roll back deployment and fix before re-deploying.

---

## Step 6 — Launch checklist

Generate a comprehensive checklist and verify each item:

**Universal:**
- [ ] Domain configured and DNS propagated
- [ ] SSL / HTTPS active
- [ ] Analytics tracking verified (events firing in dashboard)
- [ ] Error monitoring configured and receiving test events
- [ ] All forms tested end-to-end (newsletter, contact, signup)
- [ ] Payment flow tested with live keys (if applicable)
- [ ] Legal pages in place (privacy policy, terms of service)
- [ ] Favicon set
- [ ] OG / social share images set
- [ ] Performance baseline captured and thresholds met
- [ ] Mobile tested on real device (or BrowserStack equivalent)
- [ ] Code in version control, production branch tagged
- [ ] Environment variables confirmed for production environment

**Web additional:**
- [ ] sitemap.xml submitted to Search Console
- [ ] robots.txt verified
- [ ] 404 page styled and helpful

**Mobile additional:**
- [ ] App binary signed with production certificates
- [ ] App Store / Play Console listing complete (screenshots, description, keywords)
- [ ] TestFlight / Internal Testing build validated by at least one tester
- [ ] Privacy policy URL set in store listing
- [ ] Age rating completed

**SaaS additional:**
- [ ] Trial expiry flow tested (what happens when trial ends)
- [ ] Billing webhooks tested (subscription created, payment failed, cancelled)
- [ ] Admin / ops dashboard accessible

---

## Deliverables

Save in `docs/10-launch/`:
- `performance-baseline.md` — scores, bundle size, thresholds
- `cro-audit.md` — optimization report per page / screen
- `launch-checklist.md` — validated checklist (all items checked)
- `launch-plan.md` — full launch calendar
- `social-posts.md` — ready-to-publish launch content

---

## End of workflow

Present the final summary:

```
PROJECT COMPLETE

Product: [name]
Type: [web | mobile | saas | api]
URL / Store: [production URL or store link]
Screens / Pages: [count] built
Performance: [Lighthouse score or startup time]
Bundle / App size: [size]

Next steps:
1. Execute launch plan
2. Monitor error dashboard for first 24 h
3. Review analytics after 48 h and iterate
```

---

## Gate — Launch readiness

Before shipping to production, verify:

### Automatic checks (Claude verifies)
- [ ] `docs/10-launch/performance-baseline.md` exists — thresholds met
- [ ] `docs/10-launch/launch-checklist.md` exists — all items checked
- [ ] `docs/10-launch/cro-audit.md` exists — no critical issue unresolved
- [ ] Meta SEO complete on all pages (or ASO complete for mobile)
- [ ] SSL / HTTPS confirmed
- [ ] All forms functional
- [ ] Post-deploy E2E tests pass against production URL

### User validation (blocking)
- [ ] User has reviewed the CRO audit
- [ ] User has reviewed the performance baseline
- [ ] User has approved the launch plan
- [ ] User has given explicit GO for production deployment

If any criterion fails → fix before deploying. No launch with open critical issues.
