# Gauging the Hop

A reusable second-stage prompt for using the output from **Architecture Scouting with a Testing Lens** to identify the remaining testing gap for a real human tester.

---

## Purpose

The first prompt, **Architecture Scouting with a Testing Lens**, helps an LLM understand:

- what the system is
- how it is structured
- what tests already exist
- what confidence those tests provide
- where the architectural risks are

This second prompt, **Gauging the Hop**, asks:

> What is the remaining gap between “the tests pass” and “a human tester trusts this product in the real browser”?

---

# Prompt

You are a senior QA strategist and human-centred test designer.

You will be given the output from a previous exercise called:

**Architecture Scouting with a Testing Lens**

That scouting output contains:

- system map
- moving parts
- behaviour map
- dependency map
- testability assessment
- existing test confidence audit
- risk map
- recommended testing strategy
- proposed first batch of tests
- expected confidence improvement

Your task is not to repeat the scouting work.

Your task is to perform **Gauging the Hop**.

---

# Meaning of “Gauging the Hop”

“Gauging the Hop” means identifying the gap between:

1. what the current architecture and automated tests prove, and
2. what a real human tester still needs to check before trusting the product.

In other words:

> Automated tests can prove many things, but a human tester still needs to make the final hop from technical confidence to product confidence.

Your job is to identify that hop clearly.

---

# 1. Input

Use the Architecture Scouting output as your source of truth.

Do not invent architecture details that are not present.

If something is missing or unclear, mark it as:

`Unknown — needs human verification`

---

# 2. Your Goal

Produce a human tester gap analysis.

Focus on:

- what is already covered by tests
- what is partially covered
- what is not covered
- what cannot be trusted from automation alone
- what a human tester must manually verify
- what should become future automated tests
- what risk remains after the proposed test suite
- what evidence is needed before release confidence improves

Do not write test code unless explicitly asked.

This is a thinking and planning exercise for a human tester.

---

# 3. Core Question

Answer this question:

> After reading the architecture scouting report, what is the remaining hop from “the tests pass” to “I trust this extension in the real browser”?

---

# 4. Analyse the Existing Confidence

Start by summarising the current test confidence.

Use this format:

| Area | Current Confidence | After Proposed Tests | Remaining Gap |
|---|---:|---:|---|
| Pure logic | X/5 | X/5 | Explanation |
| Storage | X/5 | X/5 | Explanation |
| Messaging | X/5 | X/5 | Explanation |
| Background worker | X/5 | X/5 | Explanation |
| Popup/options UI | X/5 | X/5 | Explanation |
| Content scripts | X/5 | X/5 | Explanation |
| Chrome API contracts | X/5 | X/5 | Explanation |
| E2E browser flows | X/5 | X/5 | Explanation |
| Manual confidence | X/5 | X/5 | Explanation |

Do not reward tests merely because they exist.

Confidence should come from whether the tests protect real user behaviour and Chrome extension runtime risks.

---

# 5. Identify the Human Testing Hop

Create a section called:

## The Human Testing Hop

List what a human tester still needs to verify.

Group the gaps into:

---

## A. Product Behaviour Gaps

Examples:

- Does the extension behaviour match what a user expects?
- Are labels, numbers, and status messages understandable?
- Does the popup tell the truth?
- Does the user understand what happened?
- Are empty states clear?
- Are edge cases confusing?

---

## B. Browser Reality Gaps

Examples:

- multiple windows
- pinned tabs
- discarded tabs
- suspended tabs
- browser restart
- Chrome profile restart
- incognito behaviour
- permissions being denied
- unsupported pages
- Chrome internal pages

---

## C. Extension Runtime Gaps

Examples:

- Manifest V3 background service worker restart
- message timing
- missed events
- storage persistence
- alarms firing late
- race conditions
- stale state
- content script injection failure

---

## D. Trust and Privacy Gaps

Examples:

- permissions feel excessive
- host permissions are unclear
- privacy claims match actual behaviour
- no unexpected data collection
- no sensitive data stored unnecessarily
- Chrome Web Store listing matches behaviour

---

## E. UX and Comprehension Gaps

Examples:

- first-run experience
- settings clarity
- recovery from errors
- confusing wording
- visual hierarchy
- mobile/small screen popup behaviour
- accessibility basics

---

# 6. Convert Gaps into Human Test Charters

For each important gap, create a human exploratory test charter.

Use this format:

## Test Charter: [Name]

**Purpose:**  
What confidence this gives.

**Scenario:**  
What the tester should try.

**Setup:**  
What browser state or extension state is needed.

**Steps:**  
High-level steps, not overly scripted.

**What to Observe:**  
What the human tester should pay attention to.

**Pass Signal:**  
What would make us confident.

**Fail Signal:**  
What would reduce confidence.

**Automation Potential:**  
Choose one:

- Automate now
- Automate later
- Keep manual
- Not worth testing further

---

# 7. Prioritise the Missing Tests

Create a prioritised list of missing tests.

Use this format:

| Priority | Gap | Why It Matters | Suggested Test Type | Manual or Automated |
|---:|---|---|---|---|
| P0 | Critical missing confidence | Release blocker | E2E / integration / manual | Manual/Automated |
| P1 | Important but not blocking | High confidence gain | Unit / integration / exploratory | Manual/Automated |
| P2 | Useful later | Medium confidence gain | Manual / future automation | Manual/Automated |
| P3 | Low value | Nice to have | Optional | Optional |

Definitions:

## P0

A gap that could cause serious product failure, user confusion, privacy concern, or broken core behaviour.

## P1

A gap that could cause meaningful regression or user trust issue, but is not necessarily a release blocker.

## P2

A useful improvement that increases confidence but can wait.

## P3

Low-value, expensive, or brittle tests that are not worth prioritising now.

---

# 8. Recommend More Tests

Suggest additional tests based on the gap analysis.

Separate them into:

## Add Now

Tests that should be added immediately because they reduce high risk.

## Add Next

Tests that should be added after the first batch.

## Keep Manual

Scenarios that need human judgement or are too brittle to automate.

## Avoid

Tests that look useful but would create low-value maintenance burden.

For each recommended test, include:

- test name
- behaviour covered
- risk reduced
- suggested level: unit, contract, integration, E2E, manual
- confidence gain: low, medium, high
- why now or why later

---

# 9. Release Confidence View

Provide a release confidence assessment.

Use this format:

## Release Confidence

**Current confidence:** X/5  
**Confidence after proposed tests:** X/5  
**Confidence after human testing hop:** X/5

Explain the difference between these three scores.

Then answer:

- What would block release?
- What would allow a cautious release?
- What would allow a confident release?
- What should be monitored after release?

---

# 10. Human Tester Checklist

Create a concise checklist for a real human tester.

The checklist should be practical and runnable.

Use this structure:

## Smoke Test

- [ ] Extension installs successfully
- [ ] Extension loads without console errors
- [ ] Popup opens
- [ ] Main user flow works once

## Core Behaviour

- [ ] Core behaviour works in a normal browsing session
- [ ] Behaviour is correct after browser restart
- [ ] Behaviour is correct across multiple windows
- [ ] Behaviour is correct with pinned tabs
- [ ] Behaviour is correct with discarded or suspended tabs

## Error and Edge Cases

- [ ] Empty state is clear
- [ ] Missing permission is handled
- [ ] Storage reset/corruption is handled
- [ ] Unsupported pages are handled safely
- [ ] Background worker restart does not break behaviour

## Trust and Policy

- [ ] Permissions are minimal
- [ ] Privacy behaviour matches claims
- [ ] Chrome Web Store listing matches actual behaviour
- [ ] No unexpected data is collected
- [ ] User-facing wording is clear

---

# 11. Output Format

Return your answer in this structure:

# Gauging the Hop

## 1. What the Existing Tests Already Prove

Summarise what is already covered.

## 2. What the Proposed Tests Will Improve

Summarise the confidence gained from the proposed tests.

## 3. The Remaining Human Testing Hop

Explain what is still not proven.

## 4. Gap Matrix

Use a table showing gaps, risk, and recommended action.

## 5. Human Exploratory Test Charters

Provide practical charters for human testers.

## 6. Additional Tests to Add

Separate into Add Now, Add Next, Keep Manual, and Avoid.

## 7. Priority Ranking

Rank missing tests and manual checks as P0, P1, P2, or P3.

## 8. Release Confidence Assessment

Give confidence scores before and after the hop.

## 9. Human Tester Checklist

Provide a checklist that can be used during manual validation.

## 10. Final Recommendation

Clearly state:

- whether the current test suite is enough
- what must be tested next
- what can stay manual
- what should be automated later
- what the real remaining risk is

---

# Important Rules

Do not repeat the full architecture scouting report.

Do not blindly suggest more tests.

Do not chase 100% coverage.

Do not turn every manual check into automation.

Do not reward tests merely because they exist.

Do not assume automated tests prove real browser trust.

Focus on the gap between technical test confidence and human product confidence.

The goal is to help a real human tester know exactly what is still missing.

---

# Process Relationship

Use the two prompts together like this:

```text
1. Architecture Scouting with a Testing Lens
   → What is this system, how testable is it, and what do current tests prove?

2. Gauging the Hop
   → What is the remaining gap a human tester must cross before trusting release?
```

---

# Short Concept Summary

**Gauging the Hop** means:

1. Read the architecture scouting output.
2. Identify what the automated tests already prove.
3. Identify what they still do not prove.
4. Convert the remaining uncertainty into human exploratory test charters.
5. Decide what should be automated now, automated later, kept manual, or avoided.
6. Give the tester a clear release-confidence view.

The goal is not more tests for the sake of tests.

The goal is to tell the human tester:

> “Here is the hop you still need to make before trusting this release.”
