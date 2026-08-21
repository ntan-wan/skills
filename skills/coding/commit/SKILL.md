---
name: commit
description: Write a good git commit message for the currently staged changes, following Conventional Commits best practice. Use when the user asks to commit, write/improve a commit message, or says "/commit".
---

# Commit

Write a commit message from the **staged** changes only, then commit.

## Steps

1. Inspect state — run these together:
   - `git status --short`
   - `git diff --staged --stat`
   - `git diff --staged` (if huge, read the stat plus diffs of the most important files)
   - `git log --format='%s' -n 20` (match the repo's existing style)
2. If nothing is staged, stop and tell the user. Do **not** `git add -A` unless they asked.
3. Understand *why* the change was made, not just what lines moved. Group the diff into one logical change; if the staged diff clearly contains several unrelated changes, say so and suggest splitting.
4. Draft the message and **show it to the user in a fenced code block. Stop there — do not commit yet.** Ask them to approve, or reply with edits.
   - Never run `git commit` in the same turn as the draft, not even with `--dry-run` as a substitute for asking.
   - If they request changes, redraft and ask again.
5. Only after the user approves, commit with `git commit -m "..."` (repeated `-m` for body/footer).
6. Report the resulting `git log -1 --stat` briefly.

## Format

```
<type>(<optional scope>): <subject>

- <optional body, max 3 bullets, one line each — why, not what>

<optional footer — BREAKING CHANGE: ..., Refs #123>
```

**Types:** `feat`, `fix`, `refactor`, `perf`, `docs`, `style`, `test`, `build`, `ci`, `chore`, `revert`

**Subject rules**
- Imperative mood: "add", "fix", "remove" — not "added"/"adds".
- Lowercase start, no trailing period, ≤ 50 chars (hard cap 72).
- Say the effect, not the file list. Bad: `update CommentSelectRow.vue`. Good: `filter comments by asset when attaching to a DF`.
- Scope = the affected area (module, feature, package), only if it adds clarity.

**Body — keep it short; most commits don't need one**
- Default to **no body**. Add one only when the subject can't carry the *why*.
- Point form only: `- ` bullets, never prose paragraphs.
- **Max 3 bullets. One line each (≤ 72 cols) — if a bullet wraps, cut it down.**
- Say the reason or the catch, not a summary of the diff. The diff is already
  in the commit.
- Don't list files, don't enumerate everything the change touches.

**Footer**
- `BREAKING CHANGE: <description>` for incompatible changes (or `!` after the type/scope).
- Issue refs: `Refs #123`, `Fixes #123`.

## Rules

- **Always get explicit approval on the drafted message before committing.** No exceptions, even for a one-line trivial change.
- Never invent a ticket number, cause, or intent not visible in the diff or conversation.
- Never mention tooling that produced the change unless the user asks.
- Don't add co-author or generated-by trailers unless the repo/user requires it.
- If the repo's history clearly uses a different convention, follow the repo.

## Examples

```
fix: prevent duplicate asset rows in comment picker

- Dedupe ran before the asset filter, so one asset per linked deal.
- Now keyed on reference id.

Fixes #482
```

```
refactor(queue): move retry config out of the job classes
```

```
feat(auth)!: require MFA for admin logins

BREAKING CHANGE: existing admin sessions are invalidated on deploy.
```
