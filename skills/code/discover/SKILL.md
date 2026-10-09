---
name: discover
description: Research how existing software already behaves for a feature, then decide per behaviour whether to copy, improve, compare or skip it. Use when the user says "/discover", or before writing user stories or a proposal for a new project or a large new feature.
---

# Discover

Find out how existing software already handles the feature, so we build on proven behaviour instead of guessing.

## Process

1. **Frame**: restate the feature, the target user and the problem in 2-3 lines. If any of it is unclear, ask before researching.
2. **Pick references**: 3-6 products, open-source projects or libraries that do this well or differently. Include the best-in-class, a popular open-source one and a lightweight alternative where they exist.
3. **Research behaviour, not marketing**: use sub-agents with WebSearch/WebFetch (docs, changelogs, demos, issue trackers, user complaints) to learn what the software actually does: flows, states, edge cases, defaults, limits, error handling. Keep raw research out of the main context.
4. **Decide per behaviour**: for each notable behaviour, pick one verdict and say why:
   - **Copy**: proven, no reason to differ.
   - **Improve**: they do it, but there's a clear weakness (cite the complaint or gap).
   - **Compare**: references disagree; lay out the options and recommend one.
   - **Skip**: not relevant to our users or scope.
5. **Write** `openspec/changes/<name>/research.md` (or `docs/research/<feature>.md` if no change exists yet):

```
## Framing
## References (name, link, license, why it's relevant)
## Behaviours
| Behaviour | Reference(s) | What they do | Verdict | Our approach / reason |
## Reuse opportunities (libraries or templates we could adopt instead of building)
## Open questions
```

6. **Checkpoint**: show the verdict table and wait for the user to confirm or override before moving on.

## Rules

- Borrow behaviour and ideas freely. Never copy code, copy, assets or branding from references; check the license before suggesting we adopt any code.
- Cite a source for every behaviour claim. Mark anything unverified as such.
- Prefer current sources (check dates); products change.
- If the framework or UI choices come up, defer them to the normal stack and `impeccable` rules.
