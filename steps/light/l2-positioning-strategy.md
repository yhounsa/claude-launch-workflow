# Light Mode — Phase L2: Positioning + Product Strategy

**Mode:** Light (`--light`)
**Merges:** Full Mode Phase 2 (Positioning) + Phase 3 (Product Strategy)
**Input required:** `docs/01-discovery.md` (from L1 Discovery)
**Output:** `docs/02-positioning-strategy.md`

---

## Purpose

Combines positioning and product strategy into one focused pass. One persona (not multiple), one positioning statement, a clear screen/page list with jobs, and the core user flows. Efficient but not superficial — the output still anchors all downstream phases.

---

## Step 1 — Define the primary persona

Define one persona. The primary user who will determine whether this product succeeds or fails.

Structure:

```
Name: [Give them a real name]
Role / Context: [Who they are in one sentence]
Primary goal: [What they're trying to achieve]
Primary frustration: [What's blocking them right now]
Trigger: [What makes them look for a solution today, not next month]
```

Do not build a full empathy map. Do not define secondary or tertiary personas. One persona, five fields.

---

## Step 2 — Positioning statement

Write a positioning statement in 1–2 sentences using this structure:

> [Product name] helps [persona] [achieve goal] by [key mechanism] — without [main friction or cost of alternatives].

Validate it against these three questions:
1. Does it describe a real person with a real problem? (not a demographic)
2. Does it imply a clear reason to choose this over the alternative?
3. Is it specific enough to be falsifiable? (i.e., "helps developers ship faster by auto-generating tests" — not "empowers teams to achieve their goals")

If any answer is no, rewrite until all three are yes.

---

## Step 3 — Brand voice in 3 adjectives

Choose 3 adjectives that define how the product should feel to interact with. These will guide copy tone in L3 and visual direction in the design phase.

Format:
```
Voice: [Adjective 1] · [Adjective 2] · [Adjective 3]
Not: [Opposite of Adjective 1] · [Opposite of Adjective 2] · [Opposite of Adjective 3]
```

Example:
```
Voice: Direct · Warm · Credible
Not: Vague · Cold · Over-hyped
```

---

## Step 4 — Screen / page list with jobs

List every screen or page the product will have. For each, state its single job — the one thing it must make the user do or feel.

Format:
```
[Screen / Page name] → [Job: what it must make the user do or feel]
```

Examples:
```
Homepage → Convince the visitor the product is worth 60 more seconds
Pricing → Remove the "is it worth it?" hesitation and trigger signup
Onboarding (Step 1) → Get the user to the first meaningful action in < 2 minutes
Dashboard → Show the user the value they got since last session
```

**Guidance by product type:**

- Web / Marketing site: 3–8 pages typical. Include all pages, even simple ones (404, thank-you).
- Mobile app: 5–12 screens typical. Include onboarding, core screens, empty states.
- SaaS: Include marketing pages + core app screens. Split clearly (marketing vs. app).
- API: 1–2 pages (docs landing, quickstart). Or 0 if no frontend.

---

## Step 5 — Core user flows (3 max)

Define the 3 most important user journeys — the paths that, if broken, would make the product fail.

Format:
```
Flow 1: [Name]
[Step 1] → [Step 2] → [Step 3] → [Outcome]

Flow 2: [Name]
[Step 1] → [Step 2] → [Outcome]

Flow 3: [Name]
[Step 1] → [Step 2] → [Outcome]
```

Each step is a screen or action, not a technical function. Write them from the user's perspective.

---

## Step 6 — Content and lead capture decision

Answer these questions briefly. Each answer should be 1 sentence.

**If web / SaaS:**
- Blog / SEO content: yes or no? If yes, what is the primary topic cluster?
- Lead capture mechanism: newsletter, waitlist, free trial, demo request — which one, and where does it appear first?

**If mobile:**
- Lead capture before install: yes (App Store pre-order, landing page) or no?
- In-app onboarding mechanism: account creation required, or value-first (show value before asking for account)?

**If API / backend:**
- Developer docs: hosted (Readme, Mintlify, custom) or none?
- Lead mechanism: API key self-serve, or contact-for-access?

---

## Step 7 — Save output

Save `docs/02-positioning-strategy.md` with all sections:

```markdown
# Positioning & Product Strategy

## Persona
[From Step 1]

## Positioning statement
[From Step 2]

## Brand voice
[From Step 3]

## Screens / Pages
[From Step 4 — full list with jobs]

## Core user flows
[From Step 5]

## Content & lead capture
[From Step 6]
```

Then update `docs/context.md`:

```markdown
## Phase L2 — Positioning + Strategy
Core message: [positioning statement, one line]
Primary persona: [Name — role summary]
Screens/pages: [count]
Lead mechanism: [what was decided]
Brand voice: [3 adjectives]
```

---

## Gate — PASS / FAIL

### PASS
- Positioning statement is specific and falsifiable
- Persona has all 5 fields completed
- Every screen/page has a job defined
- Core flows documented (minimum 1)

```
GATE L2: PASS
→ Proceed to L3 — Psychology + Copy
```

### FAIL
State which criterion failed and what needs to be fixed. Do not proceed to L3 until fixed.

```
GATE L2: FAIL
Issue: [specific criterion that failed]
Required fix: [what must change]
```

---

## Trade-offs vs Full mode

| Full mode | Light mode |
|---|---|
| Multiple personas with full empathy maps | One persona, 5 fields |
| Separate positioning phase with iteration cycle | One-pass positioning, no separate iteration |
| Competitive analysis and positioning map | No competitive analysis |
| Content strategy as a full separate phase | Content/lead capture decision in one section |
| PASS / WARN / FAIL gate | PASS / FAIL only |

If the product has multiple distinct user types with conflicting needs, Full mode is recommended. Light mode assumes one primary user type dominates.
