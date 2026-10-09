---
name: stories
description: Turn research and a feature idea into prioritized user stories with acceptance criteria, an MVP cut and non-goals. Use when the user says "/stories", or after `discover` and before the proposal for a new project or large feature.
---

# Stories

Write the user stories that the proposal and specs will be built from.

## Process

1. Read `research.md` from `discover` if it exists. Every "copy" and "improve" verdict should end up covered by a story.
2. Identify personas (usually 1-3). Don't invent roles the product won't have.
3. Write stories, grouped by persona or capability:

```
### S1 - <short title>
As a <persona>, I want <capability> so that <outcome>.
Priority: Must | Should | Could
Source: <research.md behaviour, or "original">

Acceptance criteria
- Given <state>, when <action>, then <result>.
- Given <edge case>, when <action>, then <result>.
```

4. Add three closing sections: **MVP cut** (the Must stories and why), **Non-goals** (what we deliberately won't do), **Open questions**.
5. Save as `openspec/changes/<name>/stories.md`.
6. **Checkpoint**: show the stories and wait for the user to confirm, then offer to run `roast` on them before the proposal.

## Rules

- One behaviour per story; split anything with "and".
- Acceptance criteria must be testable; no "fast", "intuitive" or "user-friendly" without a measure.
- Cover failure and empty states, not only the happy path.
- Keep stories about user outcomes, not implementation.
- Use stable IDs (S1, S2...) so `tasks.md` and specs can reference them.
