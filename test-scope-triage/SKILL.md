---
name: test-scope-triage
description: Decide whether a code change needs a new test and, if so, the smallest test scope that can catch its realistic failure. Use before adding or changing tests for features, bug fixes, and refactors.
---

# Test Scope Triage

Make one test decision per changed behavior before writing or changing a test. Understand the behavior, failure mode, and existing coverage first. A test earns its place when it could fail for a realistic regression that existing checks would miss. Respect explicit test requests and project requirements.

## Decide whether to add a test

- Add a focused regression test for a bug when it can reproduce the failure and protect the fix. Reuse or strengthen an existing test when that is enough. If the failure cannot be reproduced reliably in an automated test, explain the limit and verify it another way.
- Add a test for new behavior when a meaningful branch, edge case, or contract could break silently. Public API changes deserve coverage at the contract boundary, unless existing tests already exercise the changed behavior.
- For a behavior-preserving refactor, rely on relevant passing tests unless coverage leaves the changed behavior unverified.
- Skip a new test for docs, generated output, simple configuration or wiring, and visual-only edits when a focused test would add no useful signal. Use the relevant existing checks, build, preview, or visual inspection instead.
- Do not add a test merely because code changed. If uncertain, identify the plausible regression and the check that would catch it; add a test only if that check is missing and worth keeping.

## Choose the narrowest scope that catches the failure

- **Unit:** Call the real logic directly when the failure does not depend on a real boundary. Mock collaborators only when the mock cannot conceal the behavior being tested.
- **Integration:** Use real participating components when the risk lies at their boundary: routing, persistence, serialization, filesystem, queues, or similar interactions. Keep unrelated services out.
- **End to end:** Use the full UI or service path when the failure requires it, especially for a critical journey or an explicitly requested browser flow. Prefer an existing end-to-end test that can be extended. Do not duplicate lower-level coverage without a distinct failure mode.

Test cost matters, but fidelity comes first. A cheap test that cannot fail for the reported bug is the wrong test. Add the fewest cases that cover the risk; follow the project's test conventions. Verify any integration or end-to-end target is local or disposable before running a check that can mutate data.

## State the decision

Before writing or changing a test, or when deciding to add none, say once per changed behavior:

`Test decision: <none | unit | integration | e2e> — <specific failure or reason existing checks suffice>`

Revise the decision if implementation reveals a different failure mode. This decision concerns *new test coverage*; still run relevant existing checks. If a test is added, confirm it catches the intended failure when practical, then run it against the completed change.
