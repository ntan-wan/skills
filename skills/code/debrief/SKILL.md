---
name: debrief
description: Read the commits on the current branch plus uncommitted changes, then teach what can be learned from them - techniques used, better approaches missed, and concepts worth studying. Use when the user says "/debrief", or asks what they can learn from their recent work, branch, or changes.
---

# Debrief

Turn the user's own recent work into a lesson. Read what they wrote, then teach what
is worth taking from it.

## Steps

1. Work out the range, unless the user named one in their message.
   - `git symbolic-ref --short HEAD` for the current branch.
   - If that branch *is* main or master, skip straight to `HEAD~10..HEAD`. On main,
     `git merge-base HEAD main` returns HEAD itself and the range comes back empty.
   - Otherwise find the base: try `git merge-base HEAD main`, then
     `git merge-base HEAD master`, then the upstream from
     `git rev-parse --abbrev-ref --symbolic-full-name @{u}`.
   - If no base resolves, fall back to `HEAD~10..HEAD`. Say which range you used in
     the output either way.
2. Read the work. Run these together:
   - `git log --format='%h %s' <base>..HEAD`
   - `git diff --stat <base>..HEAD`, then `git diff <base>..HEAD`
   - `git status --short`, `git diff`, and `git diff --staged` for uncommitted work
   - If the diff is large, read the stat first and pull full diffs only for the files
     carrying the interesting changes.
3. If nothing is in range - no commits and a clean tree - stop and say so. Do not
   invent a debrief from the repo at large.
4. Read the surrounding code for any file whose change you cannot explain from the
   diff alone. Never teach from a guess about what a function does.
5. Pick 5 to 7 lessons across the three kinds, ranked by how much each would change
   the user's next piece of work. Drop the trivia: renames, formatting, dependency
   bumps, generated files.
6. Write the output in the format below.

## Lesson kinds

| Kind | What qualifies |
| ---- | -------------- |
| Technique | A pattern, API, or language feature the diff actually uses, named so the user can reach for it again. |
| Missed better way | A simpler or more idiomatic approach that existed at the time. Show the alternative as code, not as a verdict. |
| Concept | Background the diff brushes against that is worth reading up on next, with what to look up. |

## Output format

Lead with the table, then one section per row.

```
**Range**: `<base>..HEAD` (<n> commits, <n> files) + uncommitted changes

| # | Kind | Lesson | Where |
| - | ---- | ------ | ----- |
| 1 | Technique | <one line, concrete> | `path/to/file.ts:42` |
```

Then one section per row, same order, headed by the lesson line: what the code does,
why it matters, and a short before/after snippet for every "missed better way". Keep
each section under about 150 words.

## Rules

- Teach from what is in the diff. Never invent a lesson the code does not support.
- Cite a real file and line for every row. No row without a location.
- One lesson per row. Do not merge two ideas to fill the table.
- Say when a choice was already the right one. A debrief is not a roast. If the branch
  is genuinely clean, a two-row table is the honest answer.
- Never edit files, stage, or commit. This skill only reads and explains.
- Also applies to `code:unslop` output rules: no puffery, no AI vocabulary, no em
  dashes. Straight quotes, sentence case headings.
