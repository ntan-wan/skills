# Learning record format

Learning records live in `learning-records/` and use sequential numbering:
`0001-slug.md`, `0002-slug.md`. Create the directory only when writing the first one.

They are the teaching equivalent of architectural decision records. They capture
non-obvious lessons, key insights, and stated prior knowledge that steer future
sessions, and they are how you calculate the zone of proximal development.

## Template

```md
# {Short title of what was learned or established}

{1-3 sentences: what was learned, or what prior knowledge was established, and why it
matters for future sessions.}
```

That is the whole format. A single paragraph is fine. The value is in recording that
this is now known and why it changes what to teach next.

## Optional sections

Add these only when they earn their place. Most records will not need them.

- `Status` frontmatter (`active` or `superseded by LR-NNNN`), for when an earlier
  understanding turns out to be wrong.
- Evidence: how the user demonstrated the understanding. Useful when the claim might
  be revisited.
- Implications: what this unlocks or rules out. Worth recording when non-obvious.

## Numbering

Scan `learning-records/` for the highest existing number and add one.

## When to write one

1. The user demonstrated real understanding of something non-trivial. Not exposure,
   evidence they can use the concept correctly. This raises the floor.
2. The user disclosed prior knowledge. Record it, and the depth claimed, so future
   sessions do not re-teach it.
3. A misconception was corrected. These are the highest value records: they predict
   where the user will stumble on related topics.
4. The mission shifted in response to learning. Update `MISSION.md` too.

## What does not qualify

- Material merely covered. Coverage is not learning. Wait for evidence.
- Anything already captured as a term in `GLOSSARY.md`. Do not duplicate.
- Session activity logs. These are decision-grade insights, not a journal.

## Supersession

When a later record contradicts an earlier one, mark the old one
`Status: superseded by LR-NNNN` instead of deleting it. How the understanding evolved
is itself useful signal.
