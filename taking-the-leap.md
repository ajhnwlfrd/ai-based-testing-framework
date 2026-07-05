# Taking the Leap

A reusable third-stage prompt for deciding what a coding agent can safely take over after **Architecture Scouting with a Testing Lens** and **Gauging the Hop**.

---

## Purpose

The first prompt, **Architecture Scouting with a Testing Lens**, helps an LLM understand:

- what the system is
- how it is structured
- what tests already exist
- what confidence those tests provide
- where the architectural risks are

The second prompt, **Gauging the Hop**, helps identify:

- what automated tests already prove
- what they do not prove
- what a real human tester still needs to verify
- what manual exploratory testing remains
- what confidence gap remains before release

This third prompt, **Taking the Leap**, asks:

> From the remaining testing gap, what can a coding agent safely take over, and what must remain with the human tester?

---

# Prompt

You are a senior QA automation strategist and coding-agent planner.

You will be given the output from a previous exercise called:

**Gauging the Hop**

That output contains:

- remaining human testing gaps
- gap matrix
- human exploratory test charters
- missing tests
- priority ranking
- release confidence assessment
- human tester checklist
- final recommendation

Your task is to perform **Taking the Leap**.

---

# Meaning of “Taking the Leap”

“Taking the Leap” means carefully deciding which parts of the remaining testing work can be safely offloaded to a coding agent or LLM.

The goal is not to automate everything.

The goal is to separate:

1. work that requires human judgement, product intuition, trust assessment, and visual/UX interpretation, from
2. work that a coding agent can safely perform with low human intervention.

In other words:

> The human tester identifies the gap.  
> The coding agent helps cross the parts of the gap that are mechanical, repeatable, and testable.

---

# 1. Input

Use the **Gauging the Hop** output as your source of truth.

Do not invent new architecture details.

Do not repeat the full previous report.

If something is unclear, mark it as:

`Unknown — needs human confirmation`

---

# 2. Your Goal

Produce a clear coding-agent delegation plan.

Focus on:

- which missing tests can be implemented by a coding agent
- which manual test charters can be converted into automated tests
- which checks should remain human-led
- which tests are too risky or too ambiguous for an LLM to automate
- what the coding agent should do first
- what guardrails the coding agent must follow
- what evidence the coding agent must produce
- what a human must review before accepting the work

Do not write code unless explicitly asked.

This is a planning prompt for deciding what to offload.

---

# 3. Core Question

Answer this question:

> From the remaining testing gap, what can a coding agent safely take over, and what must remain with the human tester?

---

# 4. Classification Framework

Classify every missing test, checklist item, and exploratory charter from the Gauging the Hop output into one of these categories:

---

## A. Offload Now

Work that a coding agent can safely do with low human intervention.

Usually includes:

- unit tests
- storage helper tests
- message contract tests
- API adapter tests
- manifest validation scripts
- lint/typecheck improvements
- deterministic integration tests
- simple E2E smoke tests
- test data setup
- CI test command wiring

These should be:

- deterministic
- easy to assert
- based on existing code behaviour
- low ambiguity
- low UX judgement
- safe to review as code

---

## B. Offload With Human Review

Work that a coding agent can draft or automate, but a human must review carefully.

Usually includes:

- browser E2E flows
- popup/options UI tests
- permission behaviour tests
- Chrome Web Store policy checks
- privacy claim validation
- accessibility checks
- edge-case automation
- tests involving real browser lifecycle behaviour

These may need human review because:

- assertions may be too shallow
- browser behaviour may be flaky
- UX interpretation may be involved
- policy/trust judgement may be involved
- the coding agent may overfit to implementation details

---

## C. Human-Led Only

Work that should stay with a human tester.

Usually includes:

- “does this feel right?”
- “is this wording clear?”
- “does the permission request feel trustworthy?”
- “does the popup tell the truth?”
- visual comprehension
- first-run experience judgement
- confusing edge cases
- product expectation mismatch
- Chrome Web Store listing truthfulness
- privacy trust assessment
- real-world exploratory testing

These require human cognition, judgement, and product intuition.

---

## D. Avoid or Defer

Work that should not be automated now.

Usually includes:

- brittle UI snapshot tests
- over-specific DOM tests
- low-value coverage chasing
- tests that duplicate lower-level tests
- tests for unstable implementation details
- broad E2E suites too early
- automation that is more expensive than the risk it reduces

---

# 5. Delegation Matrix

Create a delegation matrix.

Use this format:

| Item | Source Gap / Charter | Category | Why | Suggested Agent Task | Human Review Needed | Risk |
|---|---|---|---|---|---|---|
| Example | Storage corruption handling | Offload Now | Deterministic and assertable | Add storage default/fallback tests | Low | Medium |
| Example | Permission wording feels trustworthy | Human-Led Only | Requires human judgement | N/A | High | High |

For each item, explain why it belongs in that category.

---

# 6. Coding Agent Task Backlog

Create a practical task backlog for the coding agent.

Group tasks into:

---

## Batch 1 — Safe Mechanical Wins

Examples:

- add missing unit tests
- add storage helper tests
- add message contract tests
- add manifest validation
- add test command to package scripts
- improve mocks for browser APIs

For each task, include:

- objective
- files likely involved
- expected output
- acceptance criteria
- what the coding agent must not change
- required human review

---

## Batch 2 — Runtime Confidence

Examples:

- background worker handler tests
- alarm/scheduled behaviour tests
- storage persistence integration tests
- Chrome API error-path tests
- permission missing/denied tests

For each task, include:

- objective
- files likely involved
- expected output
- acceptance criteria
- what the coding agent must not change
- required human review

---

## Batch 3 — Browser Smoke Confidence

Examples:

- install/load extension in Chromium
- popup opens
- main flow works once
- options page saves settings
- storage survives reload

For each task, include:

- objective
- files likely involved
- expected output
- acceptance criteria
- what the coding agent must not change
- required human review

---

## Batch 4 — Human-Assisted Automation

Examples:

- convert selected exploratory charters into E2E scripts
- add accessibility checks
- add policy checklist script
- add visual/manual checklist support

For each task, include:

- objective
- files likely involved
- expected output
- acceptance criteria
- what the coding agent must not change
- required human review

---

# 7. Human vs Agent Decision Rules

Use these decision rules:

---

## Give to the coding agent when:

- the expected result is clear
- the behaviour is deterministic
- the assertion can be written precisely
- the test reduces a known risk
- the test can run in CI
- the agent can use existing code seams
- the work is repetitive or mechanical
- the test does not require product taste

---

## Keep with the human when:

- the question is about trust
- the question is about user comprehension
- the question is about wording
- the question is about visual clarity
- the behaviour depends on messy real-world browsing
- the output needs judgement, not just assertion
- Chrome Web Store policy interpretation is involved
- privacy expectations need human judgement

---

## Use human review when:

- the coding agent writes E2E tests
- assertions could be shallow
- mocks may hide real runtime behaviour
- tests may lock in bad implementation details
- failures may be flaky
- UI behaviour is involved
- permission or privacy behaviour is involved

---

# 8. Agent Guardrails

When assigning work to a coding agent, include these guardrails:

- Do not redesign the extension.
- Do not rewrite large parts of the app.
- Do not chase 100% coverage.
- Do not add brittle tests just to increase numbers.
- Do not mock away the behaviour being tested.
- Do not assert implementation details when behaviour can be asserted.
- Do not introduce heavy tools without justification.
- Do not hide failures by weakening assertions.
- Do not delete existing tests unless they are clearly misleading and replacement tests are added.
- Do not change product behaviour unless a test reveals an existing bug and the change is explicitly justified.
- Prefer small, reviewable commits.
- Explain every test added and the confidence it provides.

---

# 9. Evidence Required from the Coding Agent

For every batch of work, the coding agent must produce:

- summary of tests added
- behaviours covered
- risks reduced
- tests intentionally not added
- mocks used
- assumptions made
- commands to run tests
- before/after confidence estimate
- files changed
- any refactors made
- any human review required

Use this format:

| Change | Behaviour Covered | Risk Reduced | Confidence Gain | Human Review Needed |
|---|---|---|---|---|

---

# 10. Output Format

Return your answer in this structure:

# Taking the Leap

## 1. Summary

Explain what can and cannot be delegated to a coding agent.

## 2. Delegation Matrix

Classify all relevant gaps into:

- Offload Now
- Offload With Human Review
- Human-Led Only
- Avoid or Defer

## 3. Coding Agent Backlog

Provide Batch 1, Batch 2, Batch 3, and Batch 4 tasks.

## 4. Offload Now

List the safest immediate coding-agent tasks.

## 5. Offload With Human Review

List tasks the agent can attempt, but a human must review.

## 6. Human-Led Only

List checks that must remain human-led.

## 7. Avoid or Defer

List tests or automation that should not be done now.

## 8. Agent Guardrails

Give clear rules for the coding agent.

## 9. Required Evidence

List what the agent must report back after implementation.

## 10. Final Recommendation

Clearly state:

- what the coding agent should do first
- what the human tester should keep
- what should be reviewed carefully
- what should not be automated yet
- what confidence improvement is realistic

---

# Important Rules

Do not automate everything.

Do not treat human judgement as a bug.

Do not turn exploratory testing into shallow scripted testing.

Do not ask the coding agent to prove things it cannot observe.

Do not ask the human to do repetitive mechanical checks that code can do.

The goal is a healthy split:

> Let the coding agent handle the repeatable proof.  
> Let the human tester handle the judgement.

---

# Process Relationship

Use the three prompts together like this:

```text
1. Architecture Scouting with a Testing Lens
   → What is this system, how testable is it, and what do current tests prove?

2. Gauging the Hop
   → What is the remaining gap a human tester must cross before trusting release?

3. Taking the Leap
   → Which parts of that gap can a coding agent safely take over?
```

---

# Short Concept Summary

**Taking the Leap** means:

0. Read the architecture scouting output.
1. Read the Gauging the Hop output.
2. Identify the remaining testing gaps.
3. Classify each gap by whether a coding agent can safely handle it.
4. Separate mechanical proof from human judgement.
5. Create a coding-agent backlog.
6. Add guardrails so the agent does not over-automate or rewrite the product.
7. Require evidence from the agent after implementation.

The goal is not to replace the tester.

The goal is to decide which parts of the tester’s burden are mechanical enough to delegate.
