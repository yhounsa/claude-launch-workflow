# 09f — Sprint Retrospective

**Input:** Sprint complete (09c + 09d done)
**Output:** Retrospective appended to `docs/09-development-plan.md`
**When:** At the end of each sprint. Short. Under 20 lines. Then move on.

---

## Format

Reflect on the sprint that just finished. Answer three questions honestly.

```markdown
## Retrospective — Sprint [N]: [Sprint Name]

### Ce qui a bien marché
- [bullet 1]
- [bullet 2]
- [bullet 3]

### Ce qui a surpris ou cassé
- [bullet 1]
- [bullet 2]
- [bullet 3]

### Ajustements pour le prochain sprint
- [actionable item 1]
- [actionable item 2]
```

Keep each section to 3–5 bullets. No paragraphs. Actionable items must be specific enough to act on — not "be more careful" but "add input validation to all form handlers before testing, not after."

---

## What to surface

**What went well:** Things that worked as planned. Patterns or approaches worth repeating. Specs that were easier than expected because of good prior planning.

**What surprised or broke:** Specs that took significantly longer than expected. Assumptions that were wrong. Integration failures that weren't anticipated. Security issues that required rework. Things that were missing from the design or copy.

**Adjustments for next sprint:** Concrete changes to the dev cycle, test strategy, or spec detail level based on what was learned. If an integration test revealed a recurring contract mismatch pattern, note it here so the next sprint defines contracts upfront.

---

## After writing

Append the retrospective to `docs/09-development-plan.md` under the Sprint Retrospectives section.

If there are more sprints remaining: proceed to `09c-dev-cycle.md` for the next sprint.

If this was the final sprint: proceed to `09e-visual-fidelity.md`.
