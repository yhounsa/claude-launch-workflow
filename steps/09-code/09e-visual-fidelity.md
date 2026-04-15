# 09e — Visual Fidelity Review

**Input:** All sprints complete, application running locally
**Output:** Every screen reviewed, issues fixed, user sign-off received
**When:** Once, after all sprints are complete (not per sprint)

---

## Step 1 — Capture screenshots of every screen

For web apps, use the `webapp-testing` skill (Playwright) to systematically capture the full application:

For each page in `docs/07-product-architecture.md`:
1. Navigate to the page (use the local dev URL from `docs/06-architecture.md` or equivalent)
2. Wait for `networkidle` — all assets, fonts, and images must be loaded
3. Capture full-page screenshot to `.test/screenshots/[page-slug].png`
4. Capture additional state screenshots if the page has interactive states:
   - Forms: empty state, filled state, error state
   - Modals/drawers: open state
   - Tabs/accordions: expanded state

### Mobile screenshot capture

For mobile apps, Playwright cannot capture native screens. Use platform-specific approaches:

**React Native:**
- Use Detox `device.takeScreenshot('screen-name')`
- Or use Maestro `- takeScreenshot: screen-name`
- Screenshots saved to `.test/screenshots/[screen-slug].png`

**Flutter:**
- Use `integration_test` with `await binding.takeScreenshot('screen-name')`
- Or use `patrol` screenshot capture
- Screenshots saved to `.test/screenshots/[screen-slug].png`

**iOS native:**
- XCTest: `let screenshot = XCUIScreen.main.screenshot()`
- Or Xcode Simulator: `xcrun simctl io booted screenshot`

**Android native:**
- Espresso: `Screenshot.capture(activity)`
- Or ADB: `adb exec-out screencap -p > screenshot.png`

**If no automated capture is possible:**
- Document which screens need manual screenshot review
- Ask the user to provide screenshots from their device/simulator
- Compare manually against `.stitch/designs/` mockups

The visual comparison process (side-by-side, minor vs structural fixes) remains the same regardless of how screenshots are captured.

For mobile apps, capture on multiple device sizes:
- Phone portrait (default): primary capture
- Phone landscape (if supported by the app)
- Tablet (if the app targets tablets)

**Log any pages that fail to load** (404, crash, blank screen) — these are blockers that must be fixed before the visual review.

---

## Step 2 — Visual comparison by Phase 8 path

### If Phase 8 used Path B2 or B1 (Stitch mockups exist)

For each page, present a **side-by-side comparison**:
- Left: the Stitch mockup from `.stitch/designs/[page-slug]/screenshot.png`
- Right: the captured screenshot from `.test/screenshots/[page-slug].png`

Walk through every page in order. For each page:

> "Voici la maquette Stitch validée (gauche) à côté du rendu développé (droite). Ça correspond ? Quelque chose à ajuster ?"

Review specifically:
- Layout structure: section order, grid columns, proportions
- Colors: match DESIGN.md tokens exactly — no approximations
- Typography: font families, sizes, weights, line heights
- Spacing: padding, margins, gaps between elements
- Images: correct placement, correct aspect ratios
- Interactive states: hover styles, focus rings, active states

### If Phase 8 used Path A (no Stitch mockups)

For each page, present the relevant section from `DESIGN.md` alongside the captured screenshot:

> "Voici les specs du design system (gauche) et le rendu développé (droite). Le rendu correspond à ce que tu avais validé ?"

Review against the DESIGN.md specs: component descriptions, color assignments, layout directions.

---

## Step 3 — Fix issues

Classify each flagged issue by severity:

**Minor** — wrong color, spacing off by more than the design intent, wrong font weight, icon size incorrect, button border radius doesn't match:
- Fix inline with `/code` targeting the specific component or page
- Re-capture the screenshot after fix

**Structural** — layout fundamentally different from mockup (wrong number of columns, sections in wrong order, component missing entirely, mobile breakpoint broken):
- Invoke `/frontend-design` on that specific page to rework the composition
- Provide the mockup screenshot as the reference
- Re-capture after fix and re-present for validation

**Content** — copy doesn't match `docs/05-copy/` (wrong headline, missing CTA, placeholder text still present):
- Fix with `/code` — replace the incorrect content with the correct copy
- Re-capture after fix

After each fix: re-capture the affected screenshot and re-present it for confirmation before moving to the next page.

---

## Step 4 — Final responsive check

After all individual pages are approved, run a responsive sweep:

**Web apps:**
1. Resize to mobile (375px width) — verify no horizontal scroll, no overlapping elements, no text overflow
2. Resize to tablet (768px width) — verify layout adapts correctly (not just scaled-down desktop)
3. Resize to desktop (1280px+) — verify no layout breaks on wide screens

**Mobile apps:**
1. Phone portrait (default) — primary orientation, must be pixel-perfect
2. Phone landscape (if supported) — verify layout reflows correctly, no clipped content
3. Tablet (if the app targets tablets) — verify layout uses the extra space (not just scaled phone UI)

Capture and present responsive screenshots for any page where the user requested specific responsive behavior.

---

## Step 5 — Sign-off

Once all pages are reviewed and the user confirms:

> "Fidélité visuelle validée. Toutes les pages correspondent aux maquettes. Passage au gate Phase 9."

Update `docs/09-development-plan.md` with a visual fidelity summary:

```markdown
## Visual Fidelity — APPROVED

- Pages reviewed: [N]
- Issues found: [N]
- Issues fixed: [N]
- Issues deferred: [N] (listed in Spec Variance Log)
- User sign-off: [date]
```

Proceed to the Phase 9 gate in `workflow.md`.
