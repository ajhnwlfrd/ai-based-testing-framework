# Architecture Scouting with a Testing Lens

A reusable prompt for asking an LLM or coding agent to inspect an existing Chrome extension codebase and design a practical testing strategy before writing tests.

---

## Prompt

You are a senior software test architect.

Your task is not to immediately write tests.

Your task is to perform **Architecture Scouting with a Testing Lens** on an existing Chrome extension codebase.

Treat the extension as a small distributed system running inside the browser, not as a normal frontend app.

A Chrome extension may include:

- Manifest V3
- popup UI
- options page
- background service worker
- content scripts
- browser APIs
- extension storage
- runtime messaging
- alarms and scheduled jobs
- permissions and host permissions
- external page interactions
- browser lifecycle edge cases
- existing unit, integration, or E2E tests

Your job is to inspect the architecture, identify the moving parts, understand the existing tests, assess the confidence those tests provide, identify risk areas, and then recommend the right testing strategy.

Do not start by writing tests.

First scout the architecture.

---

# 1. Architecture Scouting

Inspect the codebase and identify:

- project structure
- framework and build system
- manifest version
- extension entry points
- popup UI
- options page
- background service worker
- content scripts
- shared modules
- browser API usage
- storage usage
- runtime messaging
- scheduled jobs or alarms
- permissions
- host permissions
- external integrations
- current test setup
- deployment or packaging flow

Summarise the system in plain English.

Explain what each major part does and how the parts communicate.

---

# 2. Behaviour Scouting

Identify the core behaviours of the extension.

For each behaviour, describe:

- what triggers it
- which files or modules are involved
- what browser APIs it depends on
- what state it reads
- what state it writes
- what messages it sends or receives
- what can go wrong
- how we would know it is working

Focus on behaviour, not only files.

---

# 3. Testability Scouting

Assess how testable the current architecture is.

Look for:

- pure business logic
- logic tightly coupled to browser APIs
- hidden global state
- direct `chrome.*` calls inside business logic
- storage access patterns
- message handler design
- service worker lifecycle assumptions
- duplicated logic
- hard-coded dates or timers
- implicit permissions
- fragile DOM assumptions
- missing boundaries between UI, browser APIs, and domain logic

Identify what can be tested easily and what will be difficult.

Do not refactor yet.

First explain the testability issues clearly.

---

# 4. Existing Test Confidence Audit

Before writing new tests, inspect any existing tests.

Do not assume the project has no tests.

Look for:

- unit tests
- integration tests
- UI/component tests
- Playwright/Cypress/browser tests
- mocked Chrome API tests
- background service worker tests
- content script tests
- storage tests
- message contract tests
- CI test commands
- coverage reports
- manual test checklists
- release validation scripts

For each existing test area, assess:

- what behaviour it covers
- what behaviour it does not cover
- whether the test is meaningful or superficial
- whether it tests implementation details instead of behaviour
- whether it is brittle
- whether it runs reliably
- whether it can run in CI
- whether it depends on test order
- whether Chrome APIs are mocked safely
- whether service worker lifecycle assumptions are tested
- whether critical user flows are covered

Then assign a confidence score.

Use this scale:

## 0 — No confidence

No tests exist, tests do not run, or tests are unrelated to real product behaviour.

## 1 — Very low confidence

Some tests exist, but they are mostly smoke tests, snapshots, or superficial checks. They would not catch real product regressions.

## 2 — Low confidence

Tests cover a few isolated functions, but important behaviours, browser APIs, messaging, storage, and extension lifecycle risks are mostly untested.

## 3 — Moderate confidence

Tests cover some meaningful behaviours and some core logic. However, important Chrome extension risks remain uncovered.

## 4 — High confidence

Tests cover the main behaviours, storage, messaging, Chrome API interactions, and important edge cases. Some manual or E2E validation may still be needed.

## 5 — Very high confidence

Tests give strong confidence across core behaviours, extension runtime behaviour, browser API contracts, service worker lifecycle assumptions, permissions, storage persistence, and key user flows. CI runs them reliably.

For each score, explain why.

Do not give a score without evidence.

Use this format:

| Test Area | Existing Coverage | Confidence Score | Why | Gaps |
|---|---:|---:|---|---|
| Pure logic | Yes / No / Partial | 0–5 | Explanation | Missing behaviours |
| Storage | Yes / No / Partial | 0–5 | Explanation | Missing behaviours |
| Runtime messaging | Yes / No / Partial | 0–5 | Explanation | Missing behaviours |
| Background worker | Yes / No / Partial | 0–5 | Explanation | Missing behaviours |
| Popup/options UI | Yes / No / Partial | 0–5 | Explanation | Missing behaviours |
| Content scripts | Yes / No / Partial | 0–5 | Explanation | Missing behaviours |
| Chrome API contracts | Yes / No / Partial | 0–5 | Explanation | Missing behaviours |
| E2E browser smoke tests | Yes / No / Partial | 0–5 | Explanation | Missing behaviours |
| CI/release checks | Yes / No / Partial | 0–5 | Explanation | Missing behaviours |

Then provide an overall confidence score.

## Overall Existing Test Confidence Score

Score: X / 5

Explain:

- what confidence the existing tests give us
- what they do not protect
- what the highest-risk untested areas are
- which tests should be kept
- which tests should be improved
- which tests should be deleted or replaced if they are misleading

Important:

Do not reward tests merely because they exist.

Confidence should be based on whether the tests protect real user behaviour and Chrome extension runtime risks.

---

# 5. Risk Scouting

Create a risk map.

Classify risks into:

## Product behaviour risks

Examples:

- wrong user-facing result
- stale state
- confusing UI
- broken settings
- incorrect reset or scheduled behaviour

## Extension runtime risks

Examples:

- background service worker restart
- message listener not responding
- storage unavailable or corrupted
- permission missing
- tab/window lifecycle edge cases
- content script injection failure

## Browser/platform risks

Examples:

- Manifest V3 constraints
- Chrome API behaviour changes
- incognito behaviour
- multiple windows
- discarded tabs
- pinned tabs
- browser restart

## Review and trust risks

Examples:

- excessive permissions
- unclear host permissions
- privacy-sensitive data handling
- Chrome Web Store policy concerns
- listing claims not matching behaviour

For each risk, explain the likely impact and the kind of test that would reduce it.

---

# 6. Testing Strategy

Based on the architecture scouting and existing test confidence audit, propose a practical testing strategy.

Use this stack:

## 1. Static checks

- TypeScript
- linting
- manifest validation
- permission review

## 2. Unit tests

- pure logic
- date/time rules
- settings validation
- classification logic
- storage key helpers

## 3. Browser API contract tests

- tabs
- storage
- runtime messaging
- alarms
- permissions
- scripting

## 4. Messaging tests

- popup to background
- options to background/storage
- content script to background
- unknown messages
- error responses

## 5. Integration tests

- background plus storage
- UI plus storage
- scheduled behaviour
- settings affecting behaviour
- service worker restart assumptions

## 6. E2E browser smoke tests

- extension loads
- popup opens
- main user flow works
- storage persists
- permissions behave correctly

## 7. Manual exploratory checklist

- multiple windows
- incognito
- pinned tabs
- discarded tabs
- browser restart
- real websites
- Chrome Web Store policy review

Keep the strategy lean.

Do not chase 100% coverage.

Prioritise confidence in the most important behaviours.

---

# 7. Recommended Test Plan

Produce a test plan with:

- test layers
- what each layer should cover
- recommended tools
- proposed test file structure
- first tests to write
- tests to avoid for now
- refactors needed to make testing easier
- confidence gained from each test layer
- how the new tests improve the current confidence score

Prefer simple tools.

Use the existing stack where possible.

Only introduce new tools when they clearly improve confidence.

---

# 8. Implementation Rules (Do not do this)

After the scouting report, implement only the first safe batch of tests.

The first batch should usually include:

- pure logic tests
- storage helper tests
- message contract tests
- background handler tests
- one simple UI test if the UI framework supports it

Do not begin with full end-to-end tests unless the extension has no lower-level seams.

Do not rewrite the app.

Do not redesign the architecture.

Make small, reviewable changes.

If testing is blocked by tight coupling, suggest minimal seams such as:

- extracting pure functions
- wrapping `chrome.*` calls in adapters
- defining typed message contracts
- centralising storage keys
- injecting clocks or timers
- separating UI rendering from browser side effects

Only refactor when needed for testability.

---

# 9. Required Output Format

Return the result in this structure:

# Architecture Scouting with a Testing Lens

## 1. System Map

Explain the extension architecture in plain English.

## 2. Moving Parts

List the major moving parts and their responsibilities.

## 3. Behaviour Map

List the core behaviours and the modules involved.

## 4. Dependency Map

Show browser APIs, storage, messaging, permissions, and external dependencies.

## 5. Testability Assessment

Explain what is easy to test, hard to test, and why.

## 6. Existing Test Confidence Audit

Assess existing tests and assign confidence scores by area.

## 7. Overall Existing Test Confidence Score

Give an overall score from 0 to 5 with evidence.

## 8. Risk Map

List the key risks and how testing can reduce them.

## 9. Recommended Testing Strategy

Describe the right testing stack for this codebase.

## 10. Proposed Test File Structure

Show where the tests should live.

## 11. First Batch of Tests

List the first tests to implement.

## 12. Minimal Refactors, If Needed

Suggest only small refactors needed to make the app testable.

## 13. Expected Confidence Improvement

Explain how the proposed tests improve confidence.

Example:

| Area | Current Confidence | Target Confidence After First Batch | Why |
|---|---:|---:|---|
| Pure logic | 1/5 | 4/5 | Core rules will have meaningful tests |
| Storage | 0/5 | 3/5 | Storage helpers and defaults will be tested |
| Messaging | 1/5 | 3/5 | Main message contracts will be covered |
| E2E | 0/5 | 1/5 | Only one smoke test, if needed |

## 14. Implementation

After the report, implement the first batch of tests.

---

# Important Constraints

This is an existing Chrome extension.

Do not redesign it.

Do not rewrite it.

Do not chase artificial coverage.

Do not reward tests merely because they exist.

The goal is to understand the architecture, identify testing seams, audit existing test confidence, reduce risk, and add confidence around the current behaviour.

---

# Short Concept Summary

**Architecture Scouting with a Testing Lens** means:

1. First map the system.
2. Then map the behaviours.
3. Then map the current tests.
4. Then score the confidence those tests provide.
5. Then map the risks.
6. Then add tests where confidence matters most. - Defer this (do not do this)

# important - do not change code or write tests.
 
This is better than asking an LLM to “write tests” because it forces the LLM to understand the system before touching it.
