---
name: debrief
description: Teach the user something from the code in front of them. First ask what to learn - how the codebase works, what the recent changes (including ones AI just made) do, or a topic the user names - then read the code and teach techniques used, better approaches missed, and concepts worth studying. Use when the user says "/debrief", or asks what they can learn from their recent work, branch, changes, or this codebase.
---

# Debrief

Turn real code into a lesson. Ask what the user wants to learn, read the code that
answers it, then teach what is worth taking from it.

## Steps

1. Pick the focus. If the user's message already names one, use it and skip the
   question. Otherwise ask with the `AskUserQuestion` tool (fall back to a plain
   question if the tool is missing), one question with these options:
   - **Codebase logic**: how this codebase works, its main flows, and why it is
     built that way.
   - **Current changes**: what the commits on this branch and the uncommitted work
     do, including changes AI made in this session.
   - The tool adds a free-text "Other" option on its own. Treat that answer as a
     user-defined focus: a feature, file, flow, concept, or question.
   Wait for the answer before reading anything.
2. Read the code for that focus, using the matching section under "Focus modes".
3. Read the surrounding code for anything you cannot explain from what you have
   already read. Never teach from a guess about what a function does.
4. Pick 5 to 7 lessons across the lesson kinds, ranked by how much each would change
   the user's next piece of work. Drop the trivia: renames, formatting, dependency
   bumps, generated files.
5. Write the output in the format below.

## Focus modes

### Codebase logic

1. Read the project's own map first: `README`, `CLAUDE.md` or `AGENTS.md`, and the
   manifest (`package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, or similar).
2. Find the entry points (main files, route or command registrations, exported
   modules) and list the top level directories.
3. Trace the one or two flows that carry the most weight end to end, from entry to
   output. Read the files on that path in full.
4. Scope line for the output: `Codebase: <name>, traced <flow names>`.

### Current changes

1. Work out the range, unless the user named one.
   - `git symbolic-ref --short HEAD` for the current branch.
   - If that branch *is* main or master, skip straight to `HEAD~10..HEAD`. On main,
     `git merge-base HEAD main` returns HEAD itself and the range comes back empty.
   - Otherwise find the base: try `git merge-base HEAD main`, then
     `git merge-base HEAD master`, then the upstream from
     `git rev-parse --abbrev-ref --symbolic-full-name @{u}`.
   - If no base resolves, fall back to `HEAD~10..HEAD`.
2. Read the work. Run these together:
   - `git log --format='%h %s' <base>..HEAD`
   - `git diff --stat <base>..HEAD`, then `git diff <base>..HEAD`
   - `git status --short`, `git diff`, and `git diff --staged` for uncommitted work
   - If the diff is large, read the stat first and pull full diffs only for the files
     carrying the interesting changes.
3. If AI edited files earlier in this conversation, make sure those files are in what
   you read, and weight lessons toward them. The user did not write that code, so
   explain what it does and why, not only what could be better.
4. If nothing is in range (no commits and a clean tree), stop and say so. Do not
   invent a debrief from the repo at large.
5. Scope line for the output: `Range: <base>..HEAD (<n> commits, <n> files) +
   uncommitted changes`.

### User-defined focus

1. Restate the focus in one line so the user can correct it before you go far.
2. Find the code behind it with search: names, strings, routes, config keys. Read
   those files and whatever they call into until you can explain the whole path.
3. If the focus is a concept with no code behind it in this repo, say so, then teach
   it from the closest code that does exist. If nothing is close, stop and ask the
   user to point you at a file or feature.
4. Scope line for the output: `Focus: <the user's topic>, read <n> files`.

## Lesson kinds

| Kind | What qualifies |
| ---- | -------------- |
| Technique | A pattern, API, or language feature the code actually uses, named so the user can reach for it again. |
| Design choice | Why the code is shaped the way it is: a boundary, data flow, or trade-off, and what it buys. |
| Missed better way | A simpler or more idiomatic approach that existed at the time. Show the alternative as code, not as a verdict. |
| Concept | Background the code brushes against that is worth reading up on next, with what to look up. |

## Output format

Lead with the scope line and the table, then one section per row.

```
**<scope line from the focus mode>**

| # | Kind | Lesson | Where |
| - | ---- | ------ | ----- |
| 1 | Technique | <one line, concrete> | `path/to/file.ts:42` |
```

Then one section per row, same order, headed by the lesson line: what the code does,
why it matters, and a short before/after snippet for every "missed better way". Keep
each section under about 150 words.

## Rules

- Teach from the code you read. Never invent a lesson the code does not support.
- Cite a real file and line for every row. No row without a location.
- One lesson per row. Do not merge two ideas to fill the table.
- Say when a choice was already the right one. A debrief is not a roast. If the code
  is genuinely clean, a two-row table is the honest answer.
- Never edit files, stage, or commit. This skill only reads and explains.
- Also applies to `code:unslop` output rules: no puffery, no AI vocabulary, no em
  dashes. Straight quotes, sentence case headings.
