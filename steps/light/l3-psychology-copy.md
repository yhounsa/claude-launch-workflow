# Light Mode — Phase L3: Psychology + Content & Copy

**Mode:** Light (`--light`)
**Merges:** Full Mode Phase 4 (Marketing Psychology) + Phase 5 (Content & Copy)
**Input required:** `docs/02-positioning-strategy.md` (from L2)
**Output:** `docs/04-psychology-copy.md` + `docs/05-copy/`

---

## Purpose

Pick 3–5 psychological levers, then write all copy with those levers naturally integrated. No exhaustive lever-per-section mapping. Psychology informs voice and structure; copywriting produces the actual words. Both happen in one pass.

The core principle holds: **copy must be complete before design begins.** No placeholders.

---

## Step 1 — Pick 3–5 psychological levers

Read `docs/02-positioning-strategy.md`. Based on the product type, audience, and brand voice, choose 3–5 levers from this list.

**Levers:**

| Lever | What it does | Best for |
|---|---|---|
| **Social proof** | Others have done it — reduces perceived risk | Products with testimonials, ratings, user counts |
| **Loss aversion** | Reframe around what they lose by not acting | Productivity tools, security, health |
| **Specificity** | Concrete numbers are more credible than vague claims | Any product with measurable outcomes |
| **Reciprocity** | Give something valuable before asking for the sale | Freemium, content-led, free trial products |
| **Authority** | Expertise signals that you can be trusted | B2B, professional tools, health/finance |
| **Scarcity / urgency** | Deadlines and limits drive action | Launches, limited-seat offers, events |
| **Clarity of outcome** | Show the exact before/after transformation | Any product where the result is tangible |
| **Friction removal** | Address the specific objection blocking signup | High-consideration products, enterprise |
| **Identity** | "This is for people like me" — belonging | Community products, niche audiences |
| **Curiosity** | Open a question the CTA closes | Content, newsletters, educational products |

Choose the 3–5 that fit the product and audience. State why each was chosen (one sentence per lever).

Do not map levers to sections. The levers will naturally appear in the copy — they don't need a grid.

---

## Step 2 — Write copy for every screen / page

Read the screen/page list from `docs/02-positioning-strategy.md`. Write complete, final copy for every item. Use the brand voice (3 adjectives from L2) and integrate the selected levers naturally.

**No placeholders. No `[Your headline here]`. Real words only.**

For each screen / page, write:

**Headline** — 6–12 words. Benefit-led, not feature-led. The selected levers should be visible here.

**Subheadline** — 1–2 sentences. Expands on the headline. Addresses the most common objection a first-time visitor would have.

**Body copy** — Only if needed for comprehension. Maximum 3 short paragraphs. Cut anything that doesn't earn its place.

**Primary CTA** — Specific action verb + outcome. Not "Submit" or "Click here". Examples: "Start my free trial", "Get my first report", "Book a 20-minute demo".

**Secondary CTA** (if applicable) — Lower-commitment alternative. Examples: "See how it works", "Read a case study".

**Trust element** — One social proof line, a credibility stat, a guarantee, or a named testimonial. Specific beats vague ("4.9/5 across 1,200 reviews" beats "loved by thousands").

**Mobile additional:** Write microcopy for:
- Empty states (what the user sees before they have any data)
- Error messages (in plain language — not "Error 422")
- Onboarding value prop (the proposition shown before they create an account)

**SaaS additional:** Write:
- Pricing plan names (3 words or fewer, benefit-led: "Starter", "Growth", "Scale")
- Feature bullets per plan (3–5 bullets, written as outcomes not features)
- CTA per plan ("Start free", "Start free — no card needed", "Talk to sales")

---

## Step 3 — Meta SEO (web) or App Store description (mobile)

**Web — for every page that will be indexed:**

Write:
- `<title>`: 50–60 characters. Primary keyword near the front.
- `<meta name="description">`: 120–155 characters. Benefit-led sentence that would make someone click.

**Mobile:**

Write:
- App name (30 chars max on iOS): primary keyword included
- Subtitle (30 chars max on iOS): secondary benefit, distinct from name
- Short description (80 chars for Play Store): one punchy benefit sentence
- Long description (4,000 chars max): first 3 lines are the hook (visible before "more") — benefit first, features second. Natural keyword integration. No keyword stuffing.

---

## Step 4 — Save outputs

### `docs/04-psychology-copy.md`

```markdown
# Psychology + Copy

## Selected levers
1. [Lever name] — [Why chosen, 1 sentence]
2. [Lever name] — [Why chosen, 1 sentence]
3. [Lever name] — [Why chosen, 1 sentence]
(+ 4 and 5 if applicable)

## Lever integration notes
[Brief note on how levers appear in the copy — 2–4 sentences total. Not a per-section grid.]
```

### `docs/05-copy/` directory

Create one file per screen or page:

- `docs/05-copy/01-homepage.md`
- `docs/05-copy/02-pricing.md`
- `docs/05-copy/03-[screen-name].md`
- ...

Each file contains the full copy for that screen/page (from Step 2).

Also create:
- `docs/05-copy/seo-meta.md` (web) — title tags and meta descriptions for all pages
- `docs/05-copy/app-store.md` (mobile) — full app store listing copy

### Update `docs/context.md`:

```markdown
## Phase L3 — Psychology + Copy
Levers: [list of 3–5 chosen levers]
Copy: complete for [count] screens/pages
SEO/ASO: [complete | n/a]
```

---

## Gate — PASS / FAIL

### PASS
- 3–5 levers selected with rationale
- Copy exists for every screen/page in `docs/02-positioning-strategy.md`
- No placeholders anywhere in `docs/05-copy/`
- SEO meta or ASO copy complete

```
GATE L3: PASS
→ Proceed to L4 — Architecture
```

### FAIL
State which criterion failed.

```
GATE L3: FAIL
Issue: [specific criterion that failed]
Required fix: [what must change]
```

---

## Trade-offs vs Full mode

| Full mode | Light mode |
|---|---|
| Full psychology framework mapped to every section | 3–5 levers, naturally integrated |
| Separate content strategy phase (blog, SEO depth, editorial calendar) | Content/lead decision already made in L2; no editorial calendar |
| Psychology and copy in separate phases with review loop | One-pass combined phase |
| PASS / WARN / FAIL gate | PASS / FAIL only |

If the product requires a content-led growth strategy (blog, SEO, newsletter) as its primary acquisition channel, Full mode is recommended. Light mode does not produce an editorial calendar or content plan.
