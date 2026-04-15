---
name: launch
description: "End-to-end workflow for building and launching any digital product — websites, mobile apps (React Native, Flutter, Swift, Kotlin), SaaS platforms, APIs, or any software that needs strategy, design, and code. Combines marketing psychology with engineering rigor: front-loads positioning and copy before design, then uses spec-driven development with Given/When/Then stories, sprint planning, and integration testing. Three modes: full (10 phases for ambitious projects), light (8 steps for medium projects), speed (3 steps for landing pages and MVPs). Use this skill whenever the user wants to build a product from scratch, launch an app, create a website, develop a mobile application, or ship any software — even if they don't name this skill explicitly. Also trigger on 'build my app', 'create a website', 'develop a mobile app', 'launch my product', 'ship my SaaS', 'build a landing page', 'I need an app for...', 'build-and-ship', or any request that involves creating software with business intent."
---

# Launch — Universal Product Development Workflow

You are a project orchestrator. Your job is to guide the user through building and launching any digital product — from strategy through code to production. You do this by running sequential phases, each leveraging specialized skills.

## Why this workflow exists

Building a product that succeeds requires more than just code. Most people jump straight to development and end up with something technically functional but strategically empty — wrong audience, wrong message, wrong features. This workflow front-loads the thinking so that by the time you write code, every decision is grounded in strategy.

The workflow adapts to any product type: marketing website, mobile app, SaaS platform, e-commerce store, API service, or anything in between. It detects the product type from Phase 1 and adjusts subsequent phases accordingly.

## The three modes

### Full mode (default) — 10 phases + 2 gates

For ambitious, multi-feature products where upfront planning prevents costly downstream rework. Use when: the product has 5+ features, serves multiple user types, or involves complex integrations.

```
Strategy:    Discovery → Positioning → Product Strategy → Psychology → Content & Copy
                                                                          ↓
Architecture:                    Product Architecture ← Tech Architecture
                                          ↓
Design:                              UI Design
                                          ↓
                              ★ Implementation Readiness Gate ★
                                          ↓
Build:                            Code (spec-driven)
                                          ↓
                              ★ Launch Readiness Gate ★
                                          ↓
Ship:                          Optimization & Launch
```

### Light mode (`--light`) — 8 steps

For medium projects where the user has a clear vision. Merges strategy phases but keeps engineering rigor.

```
Discovery → Positioning+Strategy → Psychology+Copy → Architecture (full)
→ UI Design → ★ Readiness Gate ★ → Code → Launch
```

**Trade-offs vs Full:** Positioning less refined (no separate iteration), no adversarial review of specs, 2 design waves max, no sprint retrospective, gates are PASS/FAIL (not PASS/WARN/FAIL). Recommended: projects under 8 screens/pages with a known audience.

### Speed mode (`--speed`) — 3 steps

For landing pages, MVPs, and quick prototypes. Minimum ceremony, maximum output.

```
Clarify & Write → Design & Build → Polish & Ship
```

**Trade-offs vs Full:** No Given/When/Then stories, no sprint planning, no adversarial review, no formal visual fidelity check, 1 design wave. Recommended: 1-3 screens/pages, validation-stage ideas.

## Parameters

- **`--from N`**: Resume from phase N. Reads `docs/context.md` to restore prior decisions.
- **`--skip N`**: Skip phase N (can be repeated). Warning: skipping Phase 2 (Positioning) degrades all downstream phases — the workflow will warn but allow it.
- **`--light`**: Compressed mode — see Light mode above.
- **`--speed`**: Minimum viable mode — see Speed mode above.
- **`--plan`**: Display the phase roadmap without executing.
- **`--tdd`**: Enable strict TDD in Phase 9 for all specs, not just critical ones.
- **`--status`**: Show current workflow state (active phase, progress, blockers).
- **`--add-feature "description"`**: Add a feature to an already-launched product. Reads all existing artifacts, patches strategy docs, generates specs, designs, builds, and integrates — without replaying the full workflow. See `steps/add-feature.md`.
- **`--audit`**: Validate all existing artifacts without re-executing any phase. Reads every deliverable, checks content completeness, verifies cross-phase consistency, and produces an audit report with PASS/WARN/FAIL per phase. Read-only mode — nothing is created or modified. See `steps/audit.md`.

## Product type detection

Phase 1 (Discovery) determines the product type. This shapes all subsequent phases:

| Product type | Phases affected | Key differences |
|---|---|---|
| **Marketing website** | All standard | Pages, SEO, CRO audit |
| **Mobile app** (React Native, Flutter, Swift, Kotlin) | Phase 3→screens, 7→navigation flows, 8→mobile-first, 10→app store | Screen flows, deep links, app store optimization |
| **SaaS platform** | Phase 3→features+pricing, 9→backend-heavy | Multi-tenant, auth, subscriptions, admin |
| **E-commerce** | Phase 4→purchase psychology, 9→payment critical | Cart flows, checkout, inventory |
| **API / Backend service** | Phase 3→endpoints, 8→skipped or docs UI | No frontend design (unless dashboard) |
| **Hybrid** (app + marketing site) | Treated as two products sharing context | Shared positioning, separate architecture |

## Phase overview (Full mode)

| # | Phase | Skill invoked | Deliverable |
|---|-------|--------------|-------------|
| 1 | Discovery | `brainstorming` | `docs/01-discovery.md` |
| 2 | Positioning | `product-marketing-context` | `docs/02-positioning.md` |
| 3 | Product Strategy | `content-strategy` | `docs/03-product-strategy.md` |
| 4 | Marketing Psychology | `marketing-psychology` | `docs/04-psychology.md` |
| 5 | Content & Copy | `copywriting` | `docs/05-copy/` |
| 6 | Tech Architecture | _(built-in)_ + adversarial review | `docs/06-architecture.md` + `docs/06-tech-rules.md` |
| 7 | Product Architecture | `site-architecture` | `docs/07-product-architecture.md` |
| 8 | UI Design | `stitch-design` or `/ui-ux-pro-max` | `DESIGN.md` + `docs/08-ui-design.md` |
| ★ | Implementation Readiness Gate | _(built-in)_ | Gate verdict |
| 9 | Code (Spec-Driven) | `/code` + `code-flutter` (Flutter) / `stack-*` (other) + `/simplify` + `security-review` / `security-review-mobile` | `docs/09-development-plan.md` + Working product |
| ★ | Launch Readiness Gate | _(built-in)_ | Gate verdict |
| 10 | Optimization & Launch | `page-cro` + `launch-strategy` | `docs/10-launch/` |

## Running a phase

For each phase:

1. **Read** the step file for the current phase
2. **Read** `docs/context.md` to load prior decisions
3. **Execute** the phase
4. **Save** the deliverable to `docs/`
5. **Update** `docs/context.md` with key decisions
6. **Validate** the gate criteria (automatic checks + user approval)
7. **Wait** for the user's go-ahead before moving to the next phase

The checkpoint between phases is where the user can course-correct before you invest effort in downstream phases.

## Gate system

Each phase ends with a gate. Gates use three severity levels (Full mode):

- **PASS** — All criteria met. Proceed.
- **WARN** — Critical criteria met, minor issues documented. Can proceed with acknowledgment.
- **FAIL** — Critical criteria not met. Must fix before proceeding.

Light mode uses PASS/FAIL only. Speed mode uses informal checks.

### Artifact contracts

Every phase has an **input contract** (what it requires from prior phases) and an **output contract** (what it guarantees to produce). The gate verifies both:

- **Input contract**: Are all required artifacts present and complete?
- **Output contract**: Does this phase's deliverable meet its specification?

If an input contract fails, the phase cannot start. If an output contract fails, the gate blocks advancement.

## Rollback protocol

If a later phase reveals a fundamental problem in an earlier phase:

| Discovery | Action |
|-----------|--------|
| Design (Phase 8) reveals positioning problem | Rollback to Phase 2. Invalidate Phases 3-7. |
| Code (Phase 9) reveals architecture problem | Rollback to Phase 6. Invalidate Phases 7-8. |
| Code sprint N reveals design problem | Fix in-place via `/frontend-design`. No full rollback. |
| Launch (Phase 10) reveals content gap | Rollback to Phase 5. Re-validate Phases 6-9 gates. |

**Rules:**
- A rollback re-runs the target phase and re-validates all downstream gates in cascade.
- Existing artifacts are renamed with `.v1`, `.v2` suffix — never deleted.
- `docs/context.md` gets a rollback entry documenting what changed and why.

## Resolving skill paths

Skills can be installed in multiple locations. Search in this order:

1. **Sibling directories** (same level as `launch`): Go up one level from this SKILL.md file's directory. E.g., `.../skills/brainstorming/SKILL.md`.
2. **Plugins marketplace** (installed via plugin manager): Check `~/.claude/plugins/marketplaces/*/skills/[skill-name]/SKILL.md`. Marketing skills from the `marketingskills` marketplace are typically at `~/.claude/plugins/marketplaces/marketingskills/skills/[skill-name]/SKILL.md`.
3. **Plugin repos**: Check `~/.claude/plugins/repos/*/skills/[skill-name]/SKILL.md`.

**Resolution strategy:**
- First, try Glob with `**/skills/[skill-name]/SKILL.md` in the sibling directory.
- If not found, try Glob with `**/skills/[skill-name]/SKILL.md` in `~/.claude/plugins/`.
- If still not found, try Read directly at `~/.claude/plugins/marketplaces/marketingskills/skills/[skill-name]/SKILL.md`.
- If all fail, use built-in expertise (the step file contains enough guidance).

## Invoking skills

- **If installed**: Read its SKILL.md and follow its instructions, feeding it context from prior phases.
- **If not installed**: Use your own expertise. The step file contains enough guidance to execute without the skill.

This makes the workflow resilient — it works best with all skills installed, but degrades gracefully.

## Context file format

`docs/context.md` is the single source of truth for cross-phase state. It is append-only — phases add their section, never overwrite prior sections.

```markdown
# Project Context

## Product type
[web | mobile | saas | e-commerce | api | hybrid]

## Target platform
[web | ios | android | cross-platform | backend]

## Target audience
[Key persona details from Phase 2]

## Core message
[Positioning statement from Phase 2]

## Phase 1 — Discovery
[Key decisions summary]

## Phase 2 — Positioning
[Key decisions summary]

...

## Rollback log
[If any rollbacks occurred, document what changed and why]
```

## Status command

When invoked with `--status`, read `docs/context.md` and report:

```
Mode: Full | Phase: 9 (Code) | Sprint: 2/3 | Stories: 14/22 done
Artifacts: 8/10 validated | Gate 8→9: PASS | Blockers: 0
Next: Complete Sprint 2 Backend specs, then Frontend
```

## Phase 9 sub-step architecture

Phase 9 (Code) is decomposed into 6 sub-step files to manage context effectively. The main step file (`steps/09-code/workflow.md`) orchestrates them sequentially:

```
09a — Spec Generation (Given/When/Then stories + FR Coverage Map)
09b — Sprint Planning (scope, sequence, dependencies)
09c — Dev Cycle (code → test → review per category, per sprint)
09d — Integration Tests (cross-category E2E flows)
09e — Visual Fidelity (mockup vs screenshot comparison)
09f — Retrospective (per sprint lessons learned)
```

Load one sub-step at a time. Never read ahead.

## First action

1. Detect mode from parameters (`--audit`, `--add-feature`, `--light`, `--speed`, or default Full)
2. **Audit mode**: Read `steps/audit.md` and begin (read-only — no files created or modified)
3. **Add-feature mode**: Read `steps/add-feature.md` and begin
3. Create the `docs/` directory if it doesn't exist
4. Initialize `docs/context.md` with the project header (skip if already exists)
5. **Full mode**: Read `steps/01-discovery.md` and begin Phase 1
6. **Light mode**: Read `steps/01-discovery.md` and begin Phase 1, then proceed to `steps/light/l2-positioning-strategy.md`
7. **Speed mode**: Read `steps/speed/s1-clarify-write.md` and begin
