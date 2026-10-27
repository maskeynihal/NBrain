# Loop Harness

Loop Harness is a reusable, agent-friendly process for taking software work from a fuzzy problem to a release decision. It provides prompts and workflow guidance for Kiro and Claude Code; it is not a background orchestration service and does not run agents by itself.

## Lifecycle
Discover → Specify → Review → Plan tickets → Implement → Adversarial QA → Third-eye review → Release gate.

### Discovery
Understand the problem, current behavior, constraints, users, and unknowns before committing to a solution.

### Specification and review
Create requirements and testable acceptance criteria. Document UX, technical approach, risks, data/security concerns, and important decisions. Review the spec for ambiguity and missing cases.

### Ticket planning
Break the approved scope into small, dependency-aware tasks with clear completion criteria and verification steps.

### Implementation
Make incremental changes consistent with repository conventions. Add or update tests and documentation.

### Adversarial QA and third-eye review
Check behavior against the specification, run relevant checks, investigate regressions, and assess the product experience independently of implementation rationale.

### Release gate
Summarize requirement coverage, test commands and actual results, known risks, migration and rollout steps, and unresolved blockers.

## Repository integrations
- Kiro steering: `.kiro/steering/loop-harness.md`
- Kiro workflows: `.kiro/workflows/loop-start.md`, `loop-plan.md`, and `loop-qa.md`
- Claude Code skill: `.claude/skills/loop-orchestrator/SKILL.md`
- Core reusable skill: `skills/loop-harness/SKILL.md`

## Operating principles
Never invent requirements or test results. Ask the user about consequential trade-offs. Preserve existing repository conventions. Keep artifacts usable by a different agent. Treat the release decision as evidence-based, not as a formality.
