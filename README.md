# claude-launch-workflow

A **Claude Code skill** that turns Claude into a full product-development orchestrator — from strategy to code to launch — in one structured pipeline.

> *"Building a product that succeeds requires more than just code. This workflow front-loads the thinking so that by the time you write code, every decision is grounded in strategy."*

---

## What it does

`/launch` guides Claude through a sequence of phases — positioning, copywriting, architecture, design, spec-driven code, and launch — each invoking a specialized skill when available, or falling back to built-in expertise.

It works for **any digital product**: marketing websites, mobile apps (React Native, Flutter, Swift, Kotlin), SaaS platforms, APIs, e-commerce, or hybrid projects.

---

## The three modes

| Mode | Command | When to use |
|------|---------|-------------|
| **Full** | `/launch` | 10 phases + 2 gates. Ambitious multi-feature products. |
| **Light** | `/launch --light` | 8 steps. Medium projects with clear vision. |
| **Speed** | `/launch --speed` | 3 steps. Landing pages, MVPs, prototypes. |

---

## Commands & parameters

```bash
/launch                              # Start Full mode
/launch --light                      # Compressed workflow
/launch --speed                      # Minimum-ceremony workflow
/launch --plan                       # Show phase roadmap without executing
/launch --status                     # Show current workflow state
/launch --from 6                     # Resume from phase 6
/launch --skip 4                     # Skip a specific phase
/launch --tdd                        # Enable strict TDD in Phase 9
/launch --audit                      # Validate existing artifacts (read-only)
/launch --add-feature "description"  # Add a feature to a launched product
```

---

## Full mode — 10 phases

```
Strategy:     Discovery → Positioning → Product Strategy → Psychology → Content & Copy
                                                                           ↓
Architecture:                     Product Architecture ← Tech Architecture
                                           ↓
Design:                               UI Design
                                           ↓
                          ★ Implementation Readiness Gate ★
                                           ↓
Build:                              Code (spec-driven)
                                           ↓
                             ★ Launch Readiness Gate ★
                                           ↓
Ship:                          Optimization & Launch
```

Each phase produces a deliverable in `docs/` and updates `docs/context.md` — the single source of truth carried across all phases.

### Phase list

| # | Phase | Deliverable |
|---|-------|-------------|
| 1 | Discovery | `docs/01-discovery.md` |
| 2 | Positioning | `docs/02-positioning.md` |
| 3 | Product Strategy | `docs/03-product-strategy.md` |
| 4 | Marketing Psychology | `docs/04-psychology.md` |
| 5 | Content & Copy | `docs/05-copy/` |
| 6 | Tech Architecture | `docs/06-architecture.md` + `docs/06-tech-rules.md` |
| 7 | Product Architecture | `docs/07-product-architecture.md` |
| 8 | UI Design | `DESIGN.md` + `docs/08-ui-design.md` |
| ★ | Implementation Readiness Gate | Gate verdict |
| 9 | Code (Spec-Driven) | `docs/09-development-plan.md` + Working product |
| ★ | Launch Readiness Gate | Gate verdict |
| 10 | Optimization & Launch | `docs/10-launch/` |

---

## Installation

### Option 1 — Clone into your Claude skills directory

```bash
git clone https://github.com/yhounsa/claude-launch-workflow.git ~/.claude/skills/launch
```

### Option 2 — Manual install

1. Download the latest release or clone the repo
2. Copy the folder to `~/.claude/skills/launch/`
3. Restart Claude Code

Then invoke in any Claude Code session with `/launch`.

---

## Companion skills (recommended)

`/launch` orchestrates other skills when available. For best results, install the ones relevant to your project.

> **Missing skills are fine** — `/launch` degrades gracefully and uses built-in expertise from each step file. Install only what you need.

### Skills by phase

| Phase | Skill | Source | Install command |
|-------|-------|--------|-----------------|
| 1 — Discovery | `brainstorming` | [superpowers](https://github.com/obra/superpowers) | `claude plugin add obra/superpowers` |
| 2 — Positioning | `product-marketing-context` | [marketingskills](https://github.com/coreyhaines31/marketingskills) | `claude plugin add coreyhaines31/marketingskills` |
| 3 — Product Strategy | `content-strategy` | [marketingskills](https://github.com/coreyhaines31/marketingskills) | _(included above)_ |
| 4 — Psychology | `marketing-psychology` | [marketingskills](https://github.com/coreyhaines31/marketingskills) | _(included above)_ |
| 5 — Content & Copy | `copywriting` | [marketingskills](https://github.com/coreyhaines31/marketingskills) | _(included above)_ |
| 6 — Tech Architecture | _(built-in)_ | — | — |
| 7 — Product Architecture | `site-architecture` | [marketingskills](https://github.com/coreyhaines31/marketingskills) | _(included above)_ |
| 8 — UI Design | `stitch-design` / `ui-ux-pro-max` | Custom skill | See note below |
| 9 — Code | `code`, `code-flutter`, `simplify`, `security-review` | Custom skills | See note below |
| 10 — Launch | `page-cro`, `launch-strategy` | [marketingskills](https://github.com/coreyhaines31/marketingskills) | _(included above)_ |

### Quick install — all plugin sources

Most companion skills come from just **2 plugins**. Install both and you're covered:

```bash
# Marketing skills (Phases 2-5, 7, 10) — 30+ marketing skills
claude plugin add coreyhaines31/marketingskills

# Superpowers (Phase 1 + dev workflow skills)
claude plugin add obra/superpowers
```

### Custom skills (manual install)

These skills are not from a plugin marketplace — install them separately into `~/.claude/skills/`:

| Skill | Phase | Description |
|-------|-------|-------------|
| `code` | 9 | Structured 4-phase coding workflow (analysis → plan → execute → verify) |
| `code-flutter` | 9 | Flutter/Dart development orchestrator |
| `simplify` | 9 | Post-code review for reuse, quality, and efficiency |
| `security-review` | 9 | OWASP Top 10 security audit |
| `security-review-mobile` | 9 | OWASP Mobile Top 10 audit (React Native, Flutter, Swift, Kotlin) |
| `stitch-design` | 8 | Design system + high-fidelity screen generation via Stitch MCP |
| `ui-ux-pro-max` | 8 | UI/UX design intelligence (50+ styles, 161 palettes, 10 stacks) |

### Product type compatibility

Not every skill is needed for every project. Here's what matters by product type:

| Skill | Website | Mobile App | SaaS | API | E-commerce |
|-------|:-------:|:----------:|:----:|:---:|:----------:|
| `brainstorming` | Yes | Yes | Yes | Yes | Yes |
| `product-marketing-context` | Yes | Yes | Yes | — | Yes |
| `content-strategy` | Yes | — | Yes | — | Yes |
| `marketing-psychology` | Yes | Yes | Yes | — | Yes |
| `copywriting` | Yes | Yes | Yes | — | Yes |
| `site-architecture` | Yes | — | Yes | — | Yes |
| `stitch-design` / `ui-ux-pro-max` | Yes | Yes | Yes | — | Yes |
| `code` | Yes | Yes | Yes | Yes | Yes |
| `code-flutter` | — | Flutter | — | — | — |
| `security-review` | Yes | — | Yes | Yes | Yes |
| `security-review-mobile` | — | Yes | — | — | — |
| `page-cro` | Yes | — | Yes | — | Yes |
| `launch-strategy` | Yes | Yes | Yes | Yes | Yes |

---

## Quick start — example session

Here's what a real `/launch` session looks like from start to finish:

### 1. Start a new project

```
> /launch

Claude: "What are we building? Tell me about your product, your audience, and your goals."

> I want to build a SaaS dashboard for freelancers to track their invoices,
  expenses, and tax estimates. Target: freelance designers and developers in the US.
```

### 2. Claude runs through the phases

```
Phase 1 — Discovery ✅    → docs/01-discovery.md (user personas, pain points, competitors)
Phase 2 — Positioning ✅  → docs/02-positioning.md (unique value prop, messaging)
Phase 3 — Strategy ✅     → docs/03-product-strategy.md (features, pricing, roadmap)
Phase 4 — Psychology ✅   → docs/04-psychology.md (decision triggers, objection handling)
Phase 5 — Copy ✅         → docs/05-copy/ (homepage, pricing page, onboarding)
Phase 6 — Tech Arch ✅    → docs/06-architecture.md + docs/06-tech-rules.md
Phase 7 — Product Arch ✅ → docs/07-product-architecture.md (sitemap, navigation)
Phase 8 — UI Design ✅    → DESIGN.md + docs/08-ui-design.md
  ★ Implementation Gate — PASS
Phase 9 — Code ✅         → Working product (spec-driven, sprint by sprint)
  ★ Launch Gate — PASS
Phase 10 — Launch ✅      → docs/10-launch/ (CRO, launch strategy)
```

Between each phase, Claude **pauses for your approval** before moving on. You can course-correct at any checkpoint.

### 3. Add a feature later

```
> /launch --add-feature "Add Stripe payment integration for premium tier"

Claude reads all existing artifacts, patches strategy docs, generates specs,
designs the UI, builds the feature, and integrates it — without replaying
the full 10-phase workflow.
```

### 4. Check project status anytime

```
> /launch --status

Mode: Full | Phase: 9 (Code) | Sprint: 2/3 | Stories: 14/22 done
Artifacts: 8/10 validated | Gate 8→9: PASS | Blockers: 0
Next: Complete Sprint 2 Backend specs, then Frontend
```

---

## Key concepts

### Gate system
Each phase ends with a gate: **PASS**, **WARN**, or **FAIL**. You can't advance on FAIL; WARN lets you proceed with acknowledgment. Light mode uses PASS/FAIL only.

### Artifact contracts
Every phase declares what it **requires** (input contract) and **guarantees** (output contract). The gate enforces both.

### Rollback protocol
If a later phase exposes a fundamental problem in an earlier one, `/launch` rolls back with cascading re-validation. Previous artifacts are preserved with `.v1`, `.v2` suffixes — never deleted.

### Add-feature mode
After launch, `--add-feature "description"` reads all existing artifacts, patches strategy docs, generates specs, designs, builds, and integrates — without replaying the full workflow.

---

## Folder structure

```
launch/
├── SKILL.md                  # Entry point — orchestrates everything
├── steps/
│   ├── 01-discovery.md
│   ├── 02-positioning.md
│   ├── ...
│   ├── 09-code/              # Phase 9 sub-steps (09a → 09f)
│   ├── 10-launch.md
│   ├── add-feature.md
│   ├── audit.md
│   ├── gate-implementation-readiness.md
│   ├── gate-launch-readiness.md
│   ├── light/                # Light-mode sub-steps
│   └── speed/                # Speed-mode sub-steps
└── templates/
```

---

## License

MIT — free to use, modify, and share.

---

## Author

Built by [Yvon](https://github.com/yhounsa) — senior developer, software architect, and tech entrepreneur.

Contributions and forks welcome.
