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

`/launch` orchestrates other skills when available. For best results, install:

- `brainstorming` — Phase 1 Discovery
- `product-marketing-context` — Phase 2 Positioning
- `content-strategy` — Phase 3 Product Strategy
- `marketing-psychology` — Phase 4 Psychology
- `copywriting` — Phase 5 Content & Copy
- `site-architecture` — Phase 7 Product Architecture
- `stitch-design` or `/ui-ux-pro-max` — Phase 8 UI Design
- `code`, `code-flutter`, `simplify`, `security-review` — Phase 9 Build
- `page-cro`, `launch-strategy` — Phase 10 Launch

Missing skills are fine — `/launch` degrades gracefully and uses built-in expertise from each step file.

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
