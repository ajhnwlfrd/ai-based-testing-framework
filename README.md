# Testing Framework for Chrome Extensions

A four-stage prompt framework for using an LLM or coding agent to test an existing Chrome extension responsibly — without letting it rewrite the app, chase artificial coverage, or replace human judgement.

Each stage is a standalone, reusable prompt. They are meant to be run in sequence, feeding the output of one stage into the next.

```text
1. Architecture Scouting with a Testing Lens
   → What is this system, how testable is it, and what do current tests prove?

2. Gauging the Hop
   → What is the remaining gap a human tester must cross before trusting release?

3. Taking the Leap
   → Which parts of that gap can a coding agent safely take over?

4. Final Landing
   → Turn all of the above into an executive briefing and decision.
```

## Why this exists

Asking an LLM to "write tests" for an existing codebase tends to produce shallow, high-volume tests that pad coverage numbers without protecting real behaviour. This framework forces a different order of operations: understand the system, score the confidence that already exists, identify what only a human can verify, and only then decide what a coding agent is safe to automate.

## The three stages

### 1. [Architecture Scouting with a Testing Lens](architecture-scouting-with-testing-lens.md)

Treats the extension as a small distributed system (popup, options page, background service worker, content scripts, storage, messaging, alarms, permissions) rather than a normal frontend app. The LLM scouts the architecture and behaviours, audits existing tests for real confidence (not just presence), maps risks, and proposes a lean testing strategy and first batch of tests — without writing any code.

### 2. [Gauging the Hop](gauging-the-hop.md)

Takes the scouting output and asks: what is the gap between "the tests pass" and "a human tester trusts this product in the real browser"? Produces a gap matrix, human exploratory test charters, a prioritised list of missing tests, and a release-confidence assessment split into current / after-proposed-tests / after-human-testing.

### 3. [Taking the Leap](taking-the-leap.md)

Takes the human testing gap and classifies each item into what a coding agent can safely offload now, what it can attempt with human review, what must stay human-led, and what should be avoided or deferred. Produces a batched coding-agent backlog with guardrails and required evidence for every change.

## Core principles

- **Understand before touching.** Each stage is a planning/analysis exercise; implementation only happens at the very end of stage 1, and only for the first safe batch.
- **Confidence over coverage.** Tests are scored on whether they protect real user behaviour and runtime risk, not on whether they exist.
- **Judgement stays human.** Trust, wording, UX comprehension, and policy interpretation are explicitly kept out of the coding agent's hands.
- **Small, reviewable changes.** No rewrites, no architecture redesigns, no brittle tests added just to raise numbers.

## How to use

1. Run the Stage 1 prompt against your extension's codebase.
2. Feed its full output into the Stage 2 prompt.
3. Feed the Stage 2 output into the Stage 3 prompt to get a delegation plan and coding-agent backlog.
