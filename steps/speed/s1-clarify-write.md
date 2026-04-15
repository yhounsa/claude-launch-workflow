# Speed Mode — Phase S1: Clarify & Write (15 min)

**Mode:** Speed (`--speed`)
**Time budget:** 15 minutes
**Output:** `docs/context.md` (condensed) + `docs/speed-brief.md`

---

## Purpose

Compress discovery, positioning, and copywriting into one focused pass. No separate phases, no psychology framework, no content strategy document. Ask the right questions, determine the product type, write the copy. Everything else flows from here.

The core principle is preserved: **copy always comes before design.**

---

## Step 1 — Ask 5 key questions

Ask these questions in a single message. Do not ask them one at a time.

```
To get you to a working product as fast as possible, I need 5 answers:

1. What is it? (Describe the product in 1-2 sentences)
2. Who is it for? (Primary user — be specific: "freelance designers" not "people")
3. Why does it matter to them? (The real problem it solves)
4. What makes it different? (One honest differentiator)
5. What platform? (Web, iOS, Android, cross-platform, API, SaaS — pick one)
```

Wait for answers before proceeding.

---

## Step 2 — Determine product type and select stack

Based on the answers, classify the product and auto-select the stack:

| Product type | Auto-selected stack | Override trigger |
|---|---|---|
| Landing page / marketing site | Astro + Tailwind CSS | User mentions CMS → Astro + Contentlayer |
| Mobile app | React Native + Expo | User says "native performance" → ask preference |
| SaaS web app | Next.js + Tailwind CSS | User says "already have backend" → Next.js frontend only |
| API / backend service | NestJS + TypeScript | User says "simple" → Express + TypeScript |
| E-commerce | Next.js + Tailwind + Stripe | — |

State the detected product type and stack. If ambiguous, pick the most likely one and ask for a quick confirmation (one word: "yes" or tell me what you prefer).

---

## Step 3 — Positioning in 1 sentence

Write a positioning statement using this formula:

> For [specific user] who [problem], [product name] is a [category] that [key benefit]. Unlike [alternative], we [differentiator].

This is internal scaffolding — not necessarily published copy. It anchors everything written in Step 4.

---

## Step 4 — Screen / page list (1–3 max)

Based on the product type, define the minimal screen or page set:

**Landing page:** 1 page (homepage only). No blog, no about, no pricing sub-pages unless essential.

**Mobile MVP:** 3 screens max (onboarding/splash, core action screen, confirmation/result screen).

**SaaS MVP:** 3 pages max (landing page, signup/login, core dashboard).

**API:** 1 page (documentation / landing) or 0 (no frontend).

List each screen/page with its single job (what it must make the user do or feel).

---

## Step 5 — Write complete copy

Write COMPLETE copy for every screen or page identified in Step 4. No placeholders. No "[Your headline here]". Real words.

For each screen / page, write:

**Headline** — The primary attention-grabber. Benefit-led. 6–12 words.

**Subheadline** — Expands on the headline. Addresses the skeptic. 1–2 sentences.

**Body copy** — Only if needed. Maximum 3 short paragraphs. Cut everything that doesn't earn its place.

**Primary CTA** — The one action. Specific verb + outcome (e.g., "Start my free trial" not "Submit").

**Secondary CTA** (if applicable) — Lower-commitment alternative (e.g., "See how it works").

**Trust element** (if applicable) — One social proof line, a stat, or a risk-reversal statement.

**Mobile additional:** Write microcopy for empty states, error messages, and the onboarding value prop (shown on first launch).

**SaaS additional:** Write the pricing section copy (plan names, feature bullets, CTA per plan).

---

## Step 6 — Save outputs

Save two files:

### `docs/context.md` (condensed format for speed mode)

```markdown
# Project Context

## Product type
[web-landing | mobile | saas | api | e-commerce]

## Target platform
[web | ios | android | cross-platform | backend]

## Stack
[Selected stack]

## Target audience
[One-line persona]

## Core message
[Positioning statement from Step 3]

## Screens / Pages
[List with job for each]

## Speed Mode
true

## Phase S1 — Clarify & Write
Completed. Copy ready in docs/speed-brief.md.
```

### `docs/speed-brief.md`

Contains:
- Product type and stack decision (with 1-line rationale)
- Positioning statement
- Screen/page list with jobs
- Full copy for every screen/page

---

## Gate (informal)

No formal PASS/WARN/FAIL. Ask the user one question:

> "Copy is written. Does this capture what you're building, or should I adjust anything before I design and code?"

If they say yes / good / proceed → move to S2.
If they correct something → update `docs/speed-brief.md` and move to S2.

Do not ask multiple clarifying questions. One question, one answer, move on.

---

## What this phase intentionally skips

- Competitor analysis
- Multiple personas
- Psychology framework mapping
- Content strategy (blog, SEO depth)
- ADRs (Architecture Decision Records)
- Sprint planning

These exist in Full mode. Speed mode trades depth for time-to-ship.
