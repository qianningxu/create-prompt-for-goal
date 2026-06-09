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
6. Blocked stop condition: when to stop and report no valid path remains.

## Reusable Pattern

```text
/goal <desired end state> verified by <specific evidence> while preserving <constraints>. Use <allowed inputs, tools, or boundaries>. Between iterations, <how Codex should choose the next best action>. If blocked or no valid paths remain, <what Codex should report>.
```

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
