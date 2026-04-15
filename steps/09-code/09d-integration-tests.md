# 09d — Execute Interactive Test Plan + Auto-Fix + Report

**Input:** All categories in the current sprint are complete (09c done) + `docs/09-tests/test-plan.md` exists (from 09b2)
**Output:** Automated test suite + test report presented to user
**When:** Run after each sprint's dev cycle (09c), not just at the end of Phase 9

## Why this step exists

Unit tests verify individual pieces. This step verifies the **full user experience** — every interaction, every transition, every error state, as a real user would encounter them. The test plan from 09b2 was generated from the analysis documents (Phases 3-5) and specs (09a). Now we turn that plan into automated tests, run them, fix what fails, and produce a report.

This step also exists because the book-website project proved that category-level testing misses cross-category bugs (5 critical bugs found post-sprint that integration tests would have caught).

---

## Step 1 — Load the test plan

Read `docs/09-tests/test-plan.md` (generated in 09b2). If it doesn't exist (e.g., project started before 09b2 was added), generate it now by following `09b2-test-plan.md`.

From the Coverage Tracker, identify:
- **Sprint flows:** Flows whose stories were implemented in the current sprint
- **All flows:** Every flow with tests written (for global regression)

---

## Step 2 — Write automated test files

Convert each flow from the test plan into executable test code using the platform's testing tool.

### Testing tool selection

| Platform | Tool | Test file location |
|----------|------|-------------------|
| Web | Playwright (`webapp-testing` skill) | `tests/e2e/flow-[name].spec.ts` |
| React Native | Detox or Maestro | `e2e/flow-[name].test.js` or `flows/[name].yaml` |
| Flutter | `integration_test` + `patrol` | `integration_test/flow_[name]_test.dart` |
| iOS (Swift) | XCTest UI | `UITests/Flow[Name]Tests.swift` |
| Android (Kotlin) | Espresso + UI Automator | `androidTest/Flow[Name]Test.kt` |
| API / Backend | Supertest or HTTP client | `tests/api/flow-[name].test.ts` |

### What each test file must contain

For each flow from the test plan:

```
describe('Flow: [flow name from test plan]', () => {

  describe('Happy Path', () => {
    // One test per step from the test plan table
    // Step 1: Open app → expect home screen
    // Step 2: Scroll to section → expect CTA visible
    // Step 3: Tap CTA → expect navigation
    // ...
  });

  describe('Error Paths', () => {
    // One test per error scenario from the test plan
    // E1: Empty form submit → expect error messages
    // E2: Invalid email → expect validation error
    // E3: Network offline → expect offline message
    // ...
  });

  describe('Edge Cases', () => {
    // One test per edge case from the test plan
    // X1: Long text input → expect truncation
    // X2: Double tap → expect debounce
    // ...
  });
});
```

Each test step should:
- Perform the action described in the test plan
- Assert the expected result from the test plan
- Capture a screenshot at critical moments (for the report)

### Platform-specific patterns

**Web (Playwright):**
```typescript
test('Step 3: Tap "Commander" → navigate to product page', async ({ page }) => {
  await page.click('text=Commander');
  await expect(page).toHaveURL('/produit');
  await expect(page.locator('h1')).toContainText('Commander');
  await page.screenshot({ path: 'test-results/flow-achat/step-3.png' });
});
```

**Flutter (integration_test):**
```dart
testWidgets('Step 3: Tap "Commander" → navigate to product page', (tester) async {
  await tester.tap(find.text('Commander'));
  await tester.pumpAndSettle();
  expect(find.byType(ProductPage), findsOneWidget);
});
```

**React Native (Detox):**
```javascript
it('Step 3: Tap "Commander" → navigate to product page', async () => {
  await element(by.text('Commander')).tap();
  await expect(element(by.id('product-page'))).toBeVisible();
});
```

**iOS (XCTest):**
```swift
func testStep3_tapCommander_navigateToProduct() {
    app.buttons["Commander"].tap()
    XCTAssert(app.navigationBars["Produit"].exists)
}
```

**Android (Espresso):**
```kotlin
@Test
fun step3_tapCommander_navigateToProduct() {
    onView(withText("Commander")).perform(click())
    onView(withId(R.id.product_page)).check(matches(isDisplayed()))
}
```

---

## Step 3 — Run tests in 2 passes

### Pass 1: Sprint tests (current sprint flows only)

Run only the flows whose stories were implemented in this sprint. This is fast and catches issues in the new code.

Order by priority: P0 first, then P1, then P2.

**Commands by platform:**

| Platform | Command |
|----------|---------|
| Web | `npx playwright test tests/e2e/flow-[sprint-flows].spec.ts` |
| Flutter | `flutter test integration_test/flow_[sprint-flows]_test.dart` |
| React Native (Detox) | `npx detox test -o e2e/flow-[sprint-flows].test.js` |
| React Native (Maestro) | `maestro test flows/[sprint-flows].yaml` |
| iOS | `xcodebuild test -scheme UITests -only-testing:Flow[Name]Tests` |
| Android | `./gradlew connectedAndroidTest -Pandroid.testInstrumentationRunnerArguments.class=Flow[Name]Test` |
| API | `npm test -- --testPathPattern=tests/api/flow-[sprint-flows]` |

If any test fails → go to Step 4 (auto-fix) before running Pass 2.

### Pass 2: Global regression suite (ALL flows from ALL sprints)

After Pass 1 is green, run ALL tests from ALL previous sprints + current sprint. This catches regressions — things that worked before but broke because of the new code.

**Commands by platform:**

| Platform | Command |
|----------|---------|
| Web | `npx playwright test tests/e2e/` |
| Flutter | `flutter test integration_test/` |
| React Native (Detox) | `npx detox test --configuration ios.sim.release` |
| React Native (Maestro) | `maestro test flows/` |
| iOS | `xcodebuild test -scheme UITests` |
| Android | `./gradlew connectedAndroidTest` |
| API | `npm test -- --testPathPattern=tests/api/` |

If any test fails → go to Step 4 (auto-fix). A regression in a Sprint 1 flow caused by Sprint 2 code is a high-priority fix.

**If the test tool is not available in this environment**, fall back to:
1. For web: use the `webapp-testing` skill (Playwright via MCP)
2. For mobile: document the test commands and ask the user to run them on their machine/simulator
3. For API: use `curl` or `fetch` to test endpoints directly

---

## Step 4 — Auto-fix failing tests (error-debugger → code loop)

For each failing test, use the same debug-fix loop as 09c Step 1a:

```
Test FAIL → /error-debugger reads the failure (stack trace, assertion error, screenshot)
  → Diagnoses root cause (is it the code or the test?)
  → /code applies the fix (loads right platform skill, respects tech-rules.md)
  → Re-run the failing test
  → Still fails? → Loop (max 3 attempts)
  → 3 failures? → Present to user with full diagnosis
```

### 4a — Diagnose
Use `/error-debugger` to read the failure message and identify the root cause:

| Failure type | Likely cause | Fix approach |
|---|---|---|
| Element not found | Wrong selector, component not rendered | Fix selector or check render condition |
| Wrong text/value | Copy mismatch, state not updated | Sync with `docs/05-copy/` or fix state logic |
| Navigation failure | Route not configured, guard blocking | Fix router config or auth state |
| Timeout | Slow API, missing loading state | Add loading indicator or increase timeout |
| Network error | API endpoint wrong, CORS, env vars | Fix URL, CORS config, or .env |
| Assertion mismatch | Business logic error | Fix the implementation (this is a real bug) |

### 4b — Fix using the appropriate skill

Load the right skill to fix the issue:

| What needs fixing | Skill to use |
|---|---|
| Frontend component (web) | `stack-web-frontend` + `/code` |
| Frontend component (Flutter) | `code-flutter` (routes to the right Flutter skill) |
| Frontend component (RN) | `stack-mobile-rn` + `/code` |
| Frontend component (iOS) | `stack-ios-swift` + `/code` |
| Frontend component (Android) | `stack-android-kotlin` + `/code` |
| Backend API / DB | `stack-web-backend` + `/code` |
| Security issue found during tests | `security-review` (web) or `security-review-mobile` (mobile) |
| Test itself is wrong (not the code) | Fix the test directly |

### 4c — Re-test

After each fix:
1. Re-run the specific failing test to confirm the fix
2. Re-run the full flow (not just the failing step) — fixes can break earlier steps
3. Update the test result

### 4d — Loop until green

Repeat 4a→4c until:
- All tests pass, OR
- A failure is deferred (with user approval + documented rationale)

**Do not skip this loop.** Every deferred failure is technical debt that compounds. Document deferrals clearly:
```markdown
### Deferred: [test name]
**Reason:** [why it can't be fixed now]
**Risk:** [what could go wrong in production]
**Plan:** [when it will be fixed — Sprint N+1, post-launch, etc.]
```

---

## Step 5 — Generate and present test report

After all tests are green (or deferred with approval):

### 5a — Update the Coverage Tracker

Open `docs/09-tests/test-plan.md` and update the Coverage Tracker table:
- "Tests passent" column → update with current results
- "Dernière exécution" column → set to "Sprint [N]" or "Sprint [N] (régression)"

### 5b — Save the report

Save the report to `docs/09-tests/reports/report-sprint-[N].md`. This creates a historical record that `--audit` can review later. Sprint numbers are sequential — if Sprint 1 and Sprint 2 reports exist, the next is Sprint 3.

### 5c — Present to user

Display the report in the conversation AND save it to file:

```markdown
# TEST REPORT — Sprint [N]

**Date:** [today]
**Product:** [from context.md]
**Platform:** [platform]
**Test tool:** [tool]

## Summary

| Metric | Value |
|--------|-------|
| Flows tested | [N] |
| Total test steps | [N] |
| Passed | [N] ✅ |
| Failed → Fixed | [N] 🔧 |
| Deferred | [N] ⏳ |
| Pass rate | [N]% |

## Flow Results

### Flow 1: [name] — PASS ✅
- Happy path: 8/8 steps passed
- Error paths: 4/4 steps passed
- Edge cases: 2/2 steps passed

### Flow 2: [name] — PASS (with fixes) 🔧
- Happy path: 6/6 steps passed
- Error paths: 3/3 steps passed (1 required fix)
- Edge cases: 1/1 passed

**Fixes applied:**
| Test | Issue | Fix | File changed |
|------|-------|-----|-------------|
| E2: Invalid email | No validation error shown | Added FormField validator | lib/features/auth/register_page.dart |

### Flow 3: [name] — PARTIAL ⏳
- Happy path: 5/5 passed
- Error paths: 2/3 passed
- Deferred: E3 (offline mode) — needs offline storage implementation (Sprint 2)

## Deferred Items

| Test | Risk | Planned fix |
|------|------|------------|
| Flow 3 E3: Offline mode | User loses data if offline during form submit | Sprint 2 — offline queue |

## Screenshots

[List of screenshot paths captured during testing — user can review visually]

## Confidence Level

Based on test results: **[HIGH / MEDIUM / LOW]**
- HIGH: All flows pass, < 2 deferred items, no critical-path deferrals
- MEDIUM: All happy paths pass, some error/edge cases deferred
- LOW: Happy path failures exist or critical flows have deferrals
```

Then ask:
> "Voici le rapport de tests — [N] flows testés, [pass rate]% de réussite. [N] corrections appliquées automatiquement. [N] items différés. Tu veux revoir les détails ou on continue ?"

---

## Step 6 — Trigger on add-feature and changes

This test suite is not a one-time thing. It should be re-run when:

### After `--add-feature`
When a new feature is added via `--add-feature`, at Step 5d (Integration), ask the user:
> "La nouvelle feature est intégrée. Tu veux qu'on lance la suite de tests interactifs complète pour vérifier qu'il n'y a pas de régressions ?"

If yes: re-run the full test suite from `docs/09-tests/test-plan.md` + any new tests for the added feature.
If no: run only the new feature's tests.

### After a Change Request
When artifacts are patched via a Change Request, suggest running the affected flows:
> "Le change request a impacté [pages/screens]. Tu veux qu'on relance les tests des flows impactés ?"

### After a bug fix
When `/error-debugger` or `/oneshot` fixes a bug, suggest running the related flow:
> "Le bug est corrigé. Tu veux qu'on relance le flow [name] pour confirmer ?"

---

## Important rules

- **Tests come from the test plan.** Don't invent tests — convert `docs/09-tests/test-plan.md` into code.
- **Fix with the right skill.** Don't patch blindly — load the platform skill that knows the patterns.
- **Re-run full flows after fixes.** A fix in step 3 can break step 1. Always re-run the full flow.
- **Report honestly.** If the pass rate is 60%, say 60%. The user needs truth to make decisions.
- **Screenshot evidence.** Capture at every critical step — the report should be visually verifiable.
