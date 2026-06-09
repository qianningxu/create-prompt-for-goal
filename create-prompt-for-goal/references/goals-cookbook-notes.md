# Goals Cookbook Notes

Source: OpenAI Cookbook, "Using Goals in Codex", https://developers.openai.com/cookbook/examples/codex/using_goals_in_codex

## Core Idea

A Goal is a persistent, thread-scoped completion contract. It should state what success looks like, how success will be checked, what constraints apply, what resources are in bounds, how to iterate, and when to stop as blocked.

## Best-Fit Tasks

Good candidates include:

- Performance optimization with benchmarks.
- Flaky test investigation.
- Dependency migrations.
- Bug hunts that require reproduction.
- Multi-step refactors.
- Benchmark-driven tuning.
- Research tasks that require a final artifact or audit.

Avoid Goals for one-line edits, simple explanations, short code reviews, and questions where the user wants one answer and then a stop.

## Strong Goal Ingredients

Use these six ingredients:

1. Outcome: the desired end state.
2. Verification surface: tests, benchmarks, reports, artifacts, command output, or source materials that prove the outcome.
3. Constraints: behavior, checks, or scope that must not regress.
4. Boundaries: files, tools, data, repositories, or resources Codex may use.
5. Iteration policy: how to choose the next useful action after each attempt.
6. Stop and report: when to stop and report no valid path remains.

## Readiness Standards

Before drafting a final Goal, pressure-test every ingredient:

- Outcome should include the concrete end state, scope, and measurable or inspectable result.
- Verification surface should name exact evidence, not a generic "test it" instruction.
- Constraints should identify what must stay true or what risks matter most.
- Boundaries should make allowed scope explicit, even when the allowed scope is broad.
- Iteration policy should guide the next attempt after partial success or failure.
- Stop and report should define when continued work would become speculation, looping, or overclaiming.

If any ingredient is missing or vague, ask targeted follow-up questions and rescore the Goal before drafting. Do not draft from assumptions when a required field remains weak or missing. `Stop and report` is required by default, but may be omitted if the user explicitly insists on ignoring it.

## Reusable Pattern

```text
/goal <desired end state> verified by <specific evidence> while preserving <constraints>. Use <allowed inputs, tools, or boundaries>. Between iterations, <how Codex should choose the next best action>. If blocked or no valid paths remain, <what Codex should report>.
```

## Formatting Long Goal Prompts

Prefer a sectioned prompt instead of one dense paragraph:

```text
/goal
Outcome: <desired end state>

Verification surface: <specific evidence>

Constraints: <what must not regress>

Boundaries: <allowed files, tools, data, and exclusions>

Iteration policy: <how to choose the next action>

Stop and report: <when to stop and what to report>
```

The headings are part of the prompt. Do not add extra explanation around them. If the user explicitly waives `Stop and report`, omit only that heading.

## Common Transformations

Weak:

```text
/goal Improve performance
```

Stronger:

```text
/goal Reduce p95 latency below 120 ms on the checkout benchmark while keeping the correctness test suite green.
```

Weak:

```text
/goal Write docs for this feature
```

Stronger:

```text
/goal Produce a docs page for the feature that explains the lifecycle, command surface, and two examples. Verify that the page builds locally and that all referenced commands match current CLI behavior.
```

## Research Goals

For research or reproduction work, define the evidence standard before work begins. The Goal should distinguish exact reproduction, partial reconstruction, proxy evidence, blocked claims, and remaining uncertainty. Ask Codex to produce an audit that separates confirmed claims from support-only evidence and blockers.

## Completion Discipline

A Goal should be marked complete only after checking the objective against concrete evidence. Budget limits, plausible progress, and "probably done" are not completion. If the evidence cannot be obtained under the current boundaries, the Goal should end with a blocker report rather than an overclaimed success.

When this skill is only drafting a Goal prompt, it must not run the Goal, start the Goal, or perform any task described by the Goal.
