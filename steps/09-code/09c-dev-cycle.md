# 09c — Development Cycle

**Input:** Validated sprint plan from 09b, current sprint number
**Output:** All specs in the current sprint implemented, tested, reviewed, and validated
**Repeat:** Execute this file once per sprint

---

## Overview

For the current sprint, execute specs in category order: **Infra → Backend → Frontend → Integrations**.

This order matters. Each layer depends on the layer before it: frontend calls backend APIs, integrations connect to both backend services and frontend UI. Do not start Frontend before Backend is done. Do not start Integrations before Frontend and Backend are complete.

If a category has no specs in this sprint, skip it and note it explicitly: "No [Category] specs in Sprint [N] — skipping."

---

## Sprint 1 only: Setup CI/CD immediately

Before coding the first spec of Sprint 1, create the testing infrastructure. Don't wait for Phase 10 — catching bugs early requires CI from day one.

### Pre-commit hook
Create `.git/hooks/pre-commit` that runs unit tests before every commit. If tests fail, the commit is blocked. This prevents broken code from entering the repository.

```bash
#!/bin/sh
# Adapt to stack:
# Web:     npm test -- --watchAll=false
# Flutter: flutter test
# RN:      npx jest --bail
```

### CI workflow
Create `.github/workflows/ci.yml` (or equivalent for the project's CI) that runs on push and PR:
1. Install dependencies
2. Run linter/analyzer
3. Run unit + widget tests
4. (Optional) Run integration tests on simulator/emulator

### Test runner script
Create `scripts/run_tests.sh` — a single command that runs everything:
```bash
#!/bin/bash
# Adapt to stack:
# Flutter: flutter analyze && flutter test && flutter test integration_test/
# Web:     npm run lint && npm test && npx playwright test
# RN:      npx eslint . && npx jest && npx detox test
```

This script is also used by 09d (integration tests) and by the pre-commit hook.

---

## For each category: repeat the 5-step cycle

### Step 1 — Code

Invoke `/code` for each spec in this category.

`/code` automatically loads the appropriate stack skill based on project detection:
- Infra specs: follows architecture from `docs/06-architecture.md` and rules from `docs/06-tech-rules.md`
- Backend specs: loads `stack-web-backend` (or mobile equivalent)
- Frontend specs: loads `stack-web-frontend` (web), `code-flutter` (Flutter — intelligent orchestrator that loads only relevant Flutter skills per spec), `stack-ios-swift` (iOS), `stack-android-kotlin` (Android), or `stack-mobile-rn` (React Native)
- Integration specs: follows the integration-specific SDK docs

**Frontend specs — additional requirements:**

Use the design reference loaded in workflow.md Step 0b:

- **Path B2 / B1:** Match the composition from `.stitch/designs/[page-slug]/screenshot.png` faithfully. Consult `.stitch/designs/[page-slug]/index.html` for exact CSS values. Use `DESIGN.md` tokens for all color/font/spacing variables.
- **Path A:** `DESIGN.md` is the sole reference. Section order in `docs/05-copy/[page].md` defines page layout. If DESIGN.md lacks composition guidance for a page, ask the user before coding it.

For all Frontend specs:
- Use real copy from `docs/05-copy/` — no placeholder text, no lorem ipsum
- Generate images with `image-gen` when a page needs visuals that don't exist yet in `public/images/`
- Apply responsive behavior: verify layout at mobile (< 640px), tablet (640–1024px), desktop (> 1024px)

### Step 1a — Auto-detect and fix errors after coding

After coding each spec, verify the code compiles and runs without errors. If any error is detected, trigger the automatic debug-fix loop:

```
Code written → Verify (build / run / lint)
  ↓
ERROR DETECTED?
  ├── YES → Step A: /error-debugger diagnoses the error
  │         (reads stack trace, investigates root cause, proposes fix)
  │              ↓
  │         Step B: /code applies the fix
  │         (uses the right platform skill + respects docs/06-tech-rules.md)
  │              ↓
  │         Step C: Re-verify (build / run / lint again)
  │              ↓
  │         Still broken? → Loop back to Step A (max 3 attempts)
  │         Fixed? → Continue to next spec
  │              ↓
  │         3 attempts failed? → Present error to user with diagnosis
  │
  └── NO → Continue to next spec
```

**Verification commands by platform:**

| Platform | Build check | Run check |
|----------|-----------|-----------|
| Flutter | `flutter analyze` + `flutter build` | `flutter run --no-hot-reload` (verify startup) |
| Web (Next.js) | `npm run build` | `npm run dev` (verify no console errors) |
| Web (NestJS) | `npm run build` | `npm run start:dev` (verify startup) |
| React Native | `npx tsc --noEmit` | `npx expo start` (verify no red screen) |
| iOS (Swift) | `xcodebuild build` | Simulator launch check |
| Android (Kotlin) | `./gradlew assembleDebug` | Emulator launch check |

**When `/error-debugger` runs:**
- It reads the full error (stack trace, compiler message, lint warning)
- It investigates the files involved
- It proposes a diagnosis (what's wrong and why)
- It hands off to `/code` with a clear fix instruction

**When `/code` fixes:**
- It loads the appropriate platform skill (`code-flutter`, `stack-web-frontend`, etc.)
- It respects `docs/06-tech-rules.md` (the fix must follow the constitution)
- It applies the minimum change needed (no refactoring during a fix)

**Important:** This loop is silent when everything works. The user only sees it if all 3 attempts fail — then the error is presented with the full diagnosis for manual intervention.

---

### Step 1b — Fakes-first (when creating repositories or services)

When a spec requires creating a repository or service with an interface, create the Fake implementation in the same commit. This is not optional — tests cannot be written without Fakes, and waiting to create them later means they never get created.

```
# Example: creating a message repository
lib/features/messaging/repositories/message_repository.dart           ← Interface
lib/features/messaging/repositories/supabase_message_repository.dart  ← Real impl
test/fakes/fake_message_repository.dart                               ← Fake (same commit)
```

The Fake should implement the interface with in-memory storage (a simple `List<T>` or `Map<String, T>`). No network calls, no database — pure logic. This makes tests fast (~1ms) and deterministic.

For mobile projects: also create `FakeSecureStorage` (for auth tokens) and platform-specific fakes (e.g., `FakeLiveKitRoom` for media) when those services are first used.

---

### Step 2 — Tests

**Critical specs** (marked TDD Auto in the dev plan):
Read the `test-driven-development` skill and follow strict red-green-refactor:
1. Write the failing test first (it MUST fail before any implementation)
2. Write the minimum implementation to make it pass
3. Refactor while keeping tests green

**Non-critical specs without `--tdd`:**
Write tests after the implementation. Focus on:
- Components with logic (forms, filters, pagination, cart)
- API endpoints (request validation, response shape, error handling)
- Database queries (data integrity, constraint violations)
- Utility functions and business logic helpers

**All specs with `--tdd` flag:**
Everything follows red-green-refactor, including UI components.

**Purely presentational components** (Hero with static text, Footer, decorative sections):
No unit tests required unless `--tdd` is active. The visual fidelity check in 09e will catch visual regressions.

### Step 2b — Update test plan after each story

After writing tests for a story, update the test plan to track coverage:

1. Open `docs/09-tests/test-plan.md`
2. In the Coverage Tracker table, update the "Tests écrits" column for flows that this story's tests cover
3. If the story introduces a new user flow not in the test plan, create a new flow file in `docs/09-tests/flows/flow-[name].md` and add it to the index

This keeps the test plan as a living document that reflects what's actually implemented — not just what was planned. When 09d runs the global regression suite, it knows exactly which tests exist and which are still pending.

---

### Step 3 — Review Pass 1: Code quality

After all specs in the category are coded and tested, invoke `/simplify`:
- Eliminate duplicated components or logic
- Extract reusable utilities where patterns repeat (3+ uses = extract)
- Verify consistent naming conventions match the project's existing style
- Check file organization matches the project structure from `docs/06-architecture.md`
- Remove dead code or commented-out blocks

---

### Step 4 — Review Pass 2: Security

Read the `security-review` skill (locate it via `**/skills/security-review/SKILL.md`) and run a focused audit on the code produced in this category. Scope the audit to the checks most relevant to the category:

| Category | Security focus |
|---|---|
| **Infra** | Secrets & configuration (no hardcoded keys), dependency vulnerabilities, environment variable exposure |
| **Backend** | Input validation, SQL injection, authentication bypass, CSRF, rate limiting, error message information leakage |
| **Frontend** | XSS via user-generated content, CSRF tokens in forms, sensitive data in client-side storage, CSP headers |
| **Integrations** | Payment security (PCI compliance basics), API key management, webhook signature verification, data privacy (GDPR) |

**Mobile-specific checks (when product type is mobile):**
- **Data storage**: No sensitive data in SharedPreferences/UserDefaults (use Keychain/Keystore)
- **Network**: Certificate pinning configured for API calls
- **Auth tokens**: Stored in secure enclave, not plain storage
- **Deep links**: Validated against hijacking (URL scheme uniqueness)
- **Clipboard**: Sensitive fields disable paste/copy
- **Logging**: No sensitive data in production logs (console.log, print, NSLog)

If the `security-review-mobile` skill is installed, use it instead of `security-review` for mobile projects. If not installed, use the checks above as a minimum.

**Fix all Critical and Warning issues before moving to the next category.** This is not negotiable. Informational findings can be documented and deferred, but Critical and Warning findings represent real vulnerabilities that will be in production.

After fixing issues, document them in `docs/09-spec-variance.md` if the fix caused a deviation from the original spec.

---

### Step 5 — Category validation

Present the completed category to the user:

> "[Category] — Sprint [N] terminée. [N] specs implémentées, [N] tests écrits ([N] TDD auto), review sécurité passée ([N] issues résolus). Prêt à passer à [next category] ?"

Wait for the user's go-ahead. They may:
- Test specific features before continuing
- Request changes or fixes
- Ask for clarification on implementation decisions

Do not proceed to the next category without the user's confirmation.

---

## Spec Variance Tracking

Whenever the implementation deviates from what the spec in the dev plan describes — for any reason (technical constraint, better approach discovered, spec was ambiguous) — record it in `docs/09-spec-variance.md`:

```markdown
# Spec Variance Log

| Story | Spec dit | Code fait | Raison | Approuvé |
|-------|----------|-----------|--------|:--------:|
| US3 | POST /orders returns 201 + order object | Returns 201 + order ID only | Avoids exposing full order data to client | [ ] |
| US7 | Email sent synchronously | Email queued via job | Synchronous email caused timeout on slow SMTP | [ ] |
```

Present all variances to the user at the end of each category validation (Step 5). Get explicit approval (check the Approuvé box) before moving on. Unapproved variances are technical debt.

---

## End of sprint

After all categories in the sprint are complete and user-validated:

1. Update `docs/09-development-plan.md` Sprint Log section with the sprint status:
   ```markdown
   ### Sprint [N]: [Name] — COMPLETE
   - Stories delivered: US[X], US[Y], US[Z]
   - Specs implemented: [N]
   - Tests written: [N] ([N] TDD auto)
   - Security issues resolved: [N]
   - Variances logged: [N]
   ```

2. Proceed to `09d-integration-tests.md` for this sprint.
