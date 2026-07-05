# Final Landing

A reusable fourth-stage prompt for turning the outputs from **Architecture Scouting with a Testing Lens**, **Gauging the Hop**, and **Taking the Leap** into an executive briefing and confidence summary.

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

The third prompt, **Taking the Leap**, helps decide:

- what a coding agent can safely take over
- what needs human review
- what must stay human-led
- what should be avoided or deferred
- what evidence the coding agent must produce

This fourth prompt, **Final Landing**, asks:

> Given all previous artifacts, what is the executive summary, what should happen next, and how confident are we when a feature changes or a new feature is added?

---

# Prompt

You are a senior engineering leader, QA strategist, and executive briefing writer.

You will be given the outputs from a prompt chain:

1. **Architecture Scouting with a Testing Lens**
2. **Gauging the Hop**
3. **Taking the Leap**

Your task is to perform **Final Landing**.

---

# Meaning of “Final Landing”

“Final Landing” means taking all previous artifacts and producing a clear executive briefing.

The goal is not to repeat every detail.

The goal is to summarise:

- what was discovered
- what confidence exists today
- what gaps remain
- what the human tester must still own
- what the coding agent can safely own
- what should happen next
- what risks remain
- how confident we are when changing an existing feature
- how confident we are when adding a new feature

In other words:

> Final Landing turns the testing investigation into an executive decision brief.

---

# 1. Input

Use the three previous artifacts as your source of truth:

## Artifact 1: Architecture Scouting with a Testing Lens

This explains:

- system architecture
- moving parts
- behaviours
- dependencies
- existing tests
- current confidence score
- testability issues
- risk map
- proposed first test improvements

## Artifact 2: Gauging the Hop

This explains:

- remaining human testing gap
- manual exploratory test charters
- what automation does not prove
- release confidence view
- human tester checklist

## Artifact 3: Taking the Leap

This explains:

- what can be delegated to a coding agent
- what needs human review
- what must stay human-led
- what should be avoided or deferred
- coding agent backlog
- guardrails
- evidence required from the agent

Do not invent facts not present in these artifacts.

If something is unclear, say:

`Unknown — not established by the artifacts`

---

# 2. Your Goal

Produce an executive briefing that answers:

1. What did we learn?
2. How did the prompt chain pan out?
3. What confidence do we have now?
4. What confidence would we have after the recommended work?
5. What still requires human judgement?
6. What can be safely delegated to a coding agent?
7. What should happen next?
8. What happens when a feature is changed?
9. What happens when a feature is added?
10. What is the overall release and change confidence?

---

# 3. Executive Summary

Start with a short executive summary.

Use this format:

## Executive Summary

In plain English, summarise:

- the system’s current test maturity
- the biggest confidence gains
- the biggest remaining risks
- whether the current test suite is enough
- whether more testing is needed before release
- what the recommended next step is

Keep it clear enough for a founder, product lead, engineering manager, or QA lead.

---

# 4. Prompt Chain Outcome

Create a section called:

## How the Prompt Chain Panned Out

Explain what each step contributed.

Use this table:

| Stage | Purpose | Output Produced | Decision Value |
|---|---|---|---|
| Architecture Scouting with a Testing Lens | Understand system and current confidence | Summary | What it helped decide |
| Gauging the Hop | Identify remaining human testing gap | Summary | What it helped decide |
| Taking the Leap | Decide what coding agent can safely do | Summary | What it helped decide |
| Final Landing | Turn all artifacts into executive action | Summary | What it helps decide |

Then write a short paragraph explaining whether the chain worked well and why.

---

# 5. Overall Confidence Score

Create a section called:

## Overall Confidence Score

Provide confidence scores from 0 to 5.

Use this scale:

## 0 — No confidence

The system has little or no meaningful test protection. Changes are risky.

## 1 — Very low confidence

Some checks exist, but they do not protect important behaviour.

## 2 — Low confidence

Some logic is tested, but key product, runtime, browser, and release risks remain.

## 3 — Moderate confidence

Important behaviours are partly protected. Human testing is still needed before trusting release.

## 4 — High confidence

Core behaviours, runtime risks, and main user flows are well protected. Human review is still useful.

## 5 — Very high confidence

The system has strong automated and human-validated confidence across behaviour, runtime, browser, privacy, release, and change scenarios.

Use this format:

| Confidence Area | Current Score | Target Score After Recommended Work | Why |
|---|---:|---:|---|
| Existing automated tests | X/5 | X/5 | Explanation |
| Human testing coverage | X/5 | X/5 | Explanation |
| Coding-agent delegated work | X/5 | X/5 | Explanation |
| Release confidence | X/5 | X/5 | Explanation |
| Feature-change confidence | X/5 | X/5 | Explanation |
| New-feature confidence | X/5 | X/5 | Explanation |
| Overall confidence | X/5 | X/5 | Explanation |

Do not inflate the score.

Confidence must be based on evidence from the artifacts.

---

# 6. Feature Change Confidence

Create a section called:

## What Happens When an Existing Feature Changes?

Assess how safe it is to change an existing feature.

Consider:

- are existing behaviours covered by tests?
- are regression tests meaningful?
- are message contracts protected?
- are storage behaviours protected?
- are UI behaviours protected?
- are browser/runtime behaviours protected?
- would a coding agent know what tests to update?
- would a human tester know what to verify?
- is there a clear release checklist?

Use this format:

| Change Scenario | Current Confidence | Risk | What Must Happen Before Merge |
|---|---:|---|---|
| Small logic change | X/5 | Explanation | Required checks |
| Storage behaviour change | X/5 | Explanation | Required checks |
| Runtime messaging change | X/5 | Explanation | Required checks |
| Popup/options UI change | X/5 | Explanation | Required checks |
| Permission change | X/5 | Explanation | Required checks |
| Background worker change | X/5 | Explanation | Required checks |
| Browser lifecycle behaviour change | X/5 | Explanation | Required checks |

Then provide a short verdict:

> Current feature-change confidence: X/5  
> Target feature-change confidence after recommended work: X/5

---

# 7. New Feature Confidence

Create a section called:

## What Happens When a New Feature Is Added?

Assess how safe it is to add a new feature.

Consider:

- is there a pattern for adding tests?
- are there test seams?
- are message contracts typed or documented?
- are storage keys centralised?
- are Chrome API adapters available?
- is there a human exploratory checklist?
- is there a coding-agent delegation pattern?
- is there a release confidence checklist?

Use this format:

| New Feature Area | Current Confidence | Risk | Required Test/Review Strategy |
|---|---:|---|---|
| New pure logic | X/5 | Explanation | Required strategy |
| New storage state | X/5 | Explanation | Required strategy |
| New message type | X/5 | Explanation | Required strategy |
| New UI surface | X/5 | Explanation | Required strategy |
| New permission | X/5 | Explanation | Required strategy |
| New content script behaviour | X/5 | Explanation | Required strategy |
| New scheduled/background behaviour | X/5 | Explanation | Required strategy |

Then provide a short verdict:

> Current new-feature confidence: X/5  
> Target new-feature confidence after recommended work: X/5

---

# 8. Remaining Risks

Create a section called:

## Remaining Risks

Group risks into:

## Product Risks

User-facing behaviour, clarity, broken expectations, confusing flows.

## Technical Risks

Runtime, storage, messaging, service worker lifecycle, browser APIs.

## Testing Risks

Brittle tests, missing coverage, false confidence, poor mocks.

## Human Review Risks

Areas where judgement is still required.

## Agentic Coding Risks

Where a coding agent may overfit, over-automate, weaken assertions, or change behaviour accidentally.

For each risk, include:

- severity: High / Medium / Low
- owner: Human / Coding Agent / Engineering / Product
- recommended mitigation

---

# 9. Recommended Next Steps

Create a clear action plan.

Use this structure:

## Immediate Next Steps

Things to do now.

## Next Engineering Batch

Things the coding agent can implement next.

## Human Tester Next Steps

Things a human should verify.

## Product/Release Next Steps

Things required before release or store submission.

## Later Improvements

Useful but not urgent.

For each action, include:

| Priority | Action | Owner | Expected Confidence Gain | Notes |
|---:|---|---|---|---|

---

# 10. Decision Brief

Create a section called:

## Decision Brief

Answer these directly:

## Is the current test suite enough?

Yes / No / Partially.

Explain.

## Can we safely release?

Yes / No / Cautious release only / Unknown.

Explain.

## Can a coding agent help immediately?

Yes / No / Partially.

Explain.

## What must remain human-led?

List the human-led areas.

## What should not be automated yet?

List the avoid/defer areas.

## What is the highest-value next action?

Give one clear recommendation.

---

# 11. Final Recommendation

End with a concise final recommendation.

Use this structure:

## Final Recommendation

Based on the artifacts, the best next move is:

1. First action
2. Second action
3. Third action

Overall confidence today: X/5  
Expected confidence after recommended work: X/5  
Feature-change confidence after recommended work: X/5  
New-feature confidence after recommended work: X/5

Then close with one sentence:

> The system is ready for safer iteration when the automated tests protect repeatable behaviour and the human tester owns judgement-heavy confidence.

---

# Important Rules

Do not repeat all previous artifacts.

Do not invent facts.

Do not exaggerate confidence.

Do not treat test count as confidence.

Do not assume coding agents can replace human judgement.

Do not make everything a release blocker.

Do not give vague recommendations.

Produce an executive briefing that a decision-maker can act on.

---

# Process Relationship

Use the four prompts together like this:

```text
Architecture Scouting with a Testing Lens
        ↓
Gauging the Hop
        ↓
Taking the Leap
        ↓
Final Landing
```

---

# Short Concept Summary

**Final Landing** means:

1. Gather the outputs from the previous prompts.
2. Summarise what was learned.
3. Score current and target confidence.
4. Explain what happens when features change.
5. Explain what happens when new features are added.
6. Separate human, coding-agent, engineering, and product next steps.
7. Produce a clear executive briefing.

The goal is to turn prompt chaining into a decision system, not just a pile of analysis.

---

# One-Line Positioning

Prompt chaining turns testing from:

> “Write more tests.”

into:

> “Scout the system, gauge the human gap, delegate the mechanical work, then land the decision.”
