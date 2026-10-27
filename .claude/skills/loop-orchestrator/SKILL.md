---
name: loop-orchestrator
description: Orchestrate a disciplined engineering loop from discovery and specification through tickets, implementation, adversarial QA, independent review, and release evidence.
---

# Loop Orchestrator

Use this skill for a feature, refactor, or non-trivial bug fix. Inspect repository-specific instructions first.

## Sequence
1. Discover the problem and current behavior; record constraints, assumptions, unknowns, goals, and non-goals.
2. Specify requirements, measurable acceptance criteria, UX/API/data implications, technical approach, risks, and test strategy.
3. Independently challenge the specification; resolve important ambiguities with the user.
4. Decompose the approved scope into small tickets with dependencies and verification steps.
5. Implement incrementally, following local conventions and adding tests.
6. Conduct adversarial QA against the requirements; fix confirmed in-scope defects and rerun checks.
7. Conduct a third-eye review focused on the actual user experience and regressions, not the author's rationale.
8. Produce a release gate report mapping requirements to evidence, reporting exact test results, known risks, and rollout/migration steps.

## Guardrails
- Separate facts, assumptions, and recommendations.
- Ask before making consequential product or architectural trade-offs.
- Do not invent requirements, tool results, or test outcomes.
- Never claim checks passed unless they were run.
- Keep decisions and artifacts clear enough for another agent to continue.
- Protect credentials and sensitive data.
- Explain any skipped stage and why it was not applicable.

Prefer existing project conventions. Otherwise place artifacts in docs/ and significant decisions in ADRs.
