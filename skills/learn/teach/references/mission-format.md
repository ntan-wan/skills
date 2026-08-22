# MISSION.md format

`MISSION.md` lives at the workspace root. It captures why the user is learning this
topic. Every teaching decision traces back to it: what to teach next, which resources
to surface, which exercises to design.

## Template

```md
# Mission: {Topic}

## Why
{1-3 sentences. The concrete real-world goal the user is chasing. What changes in
their life or work once they have this skill? Avoid abstract framings like
"to understand X". Push for the underlying outcome.}

## Success looks like
- {A specific, observable thing the user will be able to do}
- {Another specific thing}

## Constraints
- {Time, budget, prior commitments, learning preferences, anything that bounds the
  approach}

## Out of scope
- {Adjacent topics the user does not want to chase right now}
```

## Rules

- One mission per workspace. Two unrelated topics means two workspaces.
- Concrete over abstract. "Run a half marathon by October" beats "get fitter".
  "Ship a Rust CLI to my team" beats "learn Rust".
- Push back on vagueness. If the user cannot say why, interview them before writing
  anything. A bad mission is worse than no mission.
- Revise when reality shifts. When the goal moves, update this file. A stale mission
  steers every future session wrong.
- Keep it short. Past one screen it has stopped being a compass and become a plan.
