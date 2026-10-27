# Loop Harness Engineering Standard

Use a staged, evidence-driven engineering loop for features, refactors, and non-trivial bug fixes.

## Stages
1. Discover the user problem, current behavior, constraints, and unknowns.
2. Specify requirements, acceptance criteria, UX, technical approach, risks, and test strategy.
3. Review the specification independently and resolve important open questions.
4. Break approved work into small tickets with dependencies and observable acceptance criteria.
5. Implement the smallest coherent change and add tests.
6. Run adversarial QA, investigate failures, and fix or ticket defects.
7. Perform a third-eye review focused on user-visible behavior, edge cases, and regressions.
8. Gate release on evidence, known risks, migration notes, and test results.

## Rules
- Inspect repository conventions before changing architecture.
- Do not invent requirements, test results, or external system state.
- Ask the user when a decision materially changes scope or has meaningful trade-offs.
- Keep specs, decisions, tickets, implementation, and QA evidence understandable to a new agent.
- Never claim a test passed unless it was actually run.
- Keep secrets out of source, logs, tickets, and commits.
- If a stage is skipped, explain why. Treat a plan as a proposal until required approvals are given.

## Default artifacts
Use existing project conventions. Otherwise store planning artifacts under docs/: problem statement, requirements, acceptance criteria, technical design, ADRs, tickets, test/QA evidence, release checklist, and known limitations.
