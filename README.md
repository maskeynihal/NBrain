# NBrain

Reusable AI-assisted engineering workflows and skills.

## Loop Harness

Loop Harness provides a disciplined workflow from discovery to release evidence, with native entry points for Kiro and Claude Code.

- [Core Loop Harness skill](skills/loop-harness/SKILL.md)
- [Claude Code orchestrator skill](.claude/skills/loop-orchestrator/SKILL.md)
- [Kiro steering](.kiro/steering/loop-harness.md)
- [Kiro workflows](.kiro/workflows/)
- [Process documentation](docs/loop-harness.md)

### Lifecycle

Discover → Specify → Review → Plan tickets → Implement → Adversarial QA → Third-eye review → Release gate.

These files guide agent behavior; they are not a standalone background agent runner. Use the relevant skill or workflow in your coding environment and retain evidence for each release decision.
