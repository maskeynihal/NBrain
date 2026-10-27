---
name: loop-harness
description: Run a disciplined AI engineering loop from discovery through specifications, ticket planning, implementation, adversarial QA, blind review, and evidence-based release.
---

# Loop Harness

Use this skill when starting or continuing a software feature, change, refactor, or bug fix. Keep the work evidence-driven and pause for user decisions when requirements are ambiguous or trade-offs materially affect scope.

## Workflow

1. **Discover**: inspect the repository, identify the user problem, current behavior, constraints, and unknowns. Ask focused questions rather than inventing requirements.
2. **Specify**: write or update the product brief, acceptance criteria, UX notes, technical design, risks, and test strategy. Record consequential choices as ADRs.
3. **Review the plan**: challenge assumptions, missing cases, security/privacy concerns, migration risks, and observability. Resolve important open questions before implementation.
4. **Plan tickets**: break work into small, testable tasks with dependencies, clear outcomes, and explicit acceptance criteria.
5. **Implement**: make the smallest coherent changes. Follow repository conventions, update documentation, and add tests alongside behavior.
6. **Adversarial QA**: review the implementation against requirements as if trying to find defects. Run relevant tests and static checks; report exact commands and results. Create or fix tickets for failures.
7. **Blind review**: assess the user-visible behavior independently from implementation rationale. Check usability, edge cases, regressions, and whether the outcome actually solves the problem.
8. **Release gate**: summarize requirement-to-evidence coverage, test results, known risks, migration/release steps, and any unresolved blockers. Never claim a check passed unless it was run.

## Operating rules

- Do not fabricate user intent, test results, integrations, or repository state.
- Inspect existing code and conventions before proposing structural changes.
- Keep decisions, specifications, and implementation separate enough that another agent can take over.
- Prefer independently reviewable stages; do not silently skip a stage. Explain when a stage is not applicable.
- Treat generated plans as proposals until user approval where approval is needed.
- Never expose secrets in logs, docs, tickets, or commits.
- If blocked, state the precise blocker and provide the smallest useful next action.

## Deliverables

Use the repository's existing conventions where possible. Otherwise place artifacts under `docs/` and include:
- problem statement and non-goals
- requirements and acceptance criteria
- technical approach and ADRs for significant decisions
- implementation tickets and dependencies
- test/QA evidence
- release checklist and known limitations
