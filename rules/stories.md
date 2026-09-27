# Stories

- A story is a placeholder for a conversation and the context of conversations already had. It is not a spec.
- We write a story for every feature, bug and chore, and we discuss it (the `bacon:example-mapping` skill) before pulling it into development.
- Stories state the **what** and the **why**, not the how. They are never technically prescriptive.
- Stories include everything at a high level, not every detail.
- Every story follows INVEST: Independent, Negotiable, Valuable, Estimatable, Small, Testable.
- Acceptance criteria are Gherkin: GIVEN [context] WHEN [action] THEN [reaction]. Several ANDs in one criterion mean the story should be split.
- Titles name the persona: `[Persona] should (not) be able to [action]`.

Three types, tracked with bacon-tracker:
- **Feature** — the smallest increment that delivers user value. Estimated by complexity, not time.
- **Bug** — a defect in an accepted feature. Provides no new value, so it is not estimated. Never use a bug to describe new functionality.
- **Chore** — necessary work with no direct user value (tech debt, dependencies, tooling). Not estimated. If it feels like user value, it is a feature.

Templates and the fields each type needs: the `bacon:story-writing` skill.
