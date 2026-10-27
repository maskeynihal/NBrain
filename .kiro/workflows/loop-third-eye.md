# Loop Third Eye: Independent User-Focused Review

Review the result as a skeptical reviewer who cares about the user outcome, not the implementation author's reasoning.

1. Start from the original problem and approved acceptance criteria.
2. Inspect the user-visible flows and changed behavior; do not assume the implementation is correct because tests pass.
3. Check discoverability, confusing states, empty/loading/error states, keyboard/accessibility behavior, permissions, data integrity, and regressions as relevant.
4. Try plausible misuse and boundary cases.
5. For each finding, state severity, affected user, reproduction steps, evidence, and expected behavior.
6. Separate confirmed issues from questions and improvement ideas.
7. Recommend release, release with documented risks, or block pending fixes.
