---
name: teach
description: Teach the user a topic across multiple sessions in a stateful learning workspace, with a mission, lessons, reference docs, and learning records. Use when the user says "/teach", asks you to teach them something, or wants to learn a subject over time rather than get a one-off answer.
---

# Teach

The user wants to learn a topic over multiple sessions. Treat the current directory
as a teaching workspace and keep its state up to date every session.

For a one-off "what does this mean" question, use `eli5` instead. This skill is for
learning that spans sessions.

## Workspace layout

Create these lazily, only when there is content for them.

- `MISSION.md` at the root. Why the user wants this topic. Ground every teaching
  decision in it. Format: [mission-format.md](references/mission-format.md).
- `RESOURCES.md` at the root. Trusted sources for knowledge, and communities for
  wisdom. Format: [resources-format.md](references/resources-format.md).
- `GLOSSARY.md` at the root. The canonical language of the workspace. Format:
  [glossary-format.md](references/glossary-format.md).
- `learning-records/0001-<dash-case-name>.md`. What the user has actually learned,
  numbered in sequence. These drive what to teach next. Format:
  [learning-record-format.md](references/learning-record-format.md).
- `lessons/0001-<dash-case-name>.html`. The lessons themselves, numbered in sequence.
- `reference/*.html`. Compressed cheat sheets the user returns to.
- `assets/*`. Components shared across lessons, starting with a stylesheet.
- `NOTES.md`. Scratchpad for user preferences and working notes.

## Start of every session

1. Read `MISSION.md`, `NOTES.md`, `GLOSSARY.md`, and the `learning-records/`.
2. If `MISSION.md` is missing or vague, stop and interview the user about why they
   want this topic. Do not teach anything first. Without a mission you cannot judge
   what to teach next, and lessons come out abstract.
3. If `RESOURCES.md` is thin, go find high-quality sources before teaching. Never
   teach from memory alone.
4. Pick the next thing to teach from the zone of proximal development, unless the
   user named a specific thing.

## The three parts of learning

**Knowledge** comes from high-trust sources. **Skills** come from interactive lessons
you design on top of that knowledge. **Wisdom** comes from practising in the real
world with other people.

The balance shifts by topic. Theoretical physics leans on knowledge. Yoga leans on
skills. Read the topic and weight accordingly.

## Fluency vs storage strength

Fluency is retrieval in the moment. Storage strength is what the user still has in
six months. Fluency feels like mastery and is not, so design for storage strength
through desirable difficulty:

- Retrieval practice. Make the user recall from memory, not recognise from a list.
- Spacing. Revisit earlier material in later lessons.
- Interleaving. Mix related topics inside a single practice set. Skills practice only.

## Zone of proximal development

Each lesson should feel like a stretch the user can just about make. Work it out
from the learning records plus the mission: the most mission-relevant thing they are
now ready for. Too easy wastes the session. Too hard burns working memory on
confusion instead of understanding.

## Lessons

A lesson is one self-contained HTML file in `lessons/`, teaching one tightly scoped
thing tied to the mission. It is the main thing you produce.

- Keep it short. Working memory is small. One tangible win per lesson.
- Teach the knowledge first, then make the user practise the skill.
- Every practice section needs a feedback loop, as tight and as automatic as you can
  make it. Quizzes, in-browser tasks, or a checklist of real-world steps to perform.
- Cite sources throughout. Link claims back to `RESOURCES.md` entries.
- Recommend one primary source to read or watch: the best thing you found.
- Link to related lessons and reference docs with HTML anchors.
- End with a reminder that the user can ask you followup questions.
- Make it beautiful. Clean typography, generous spacing, prints well. Think Tufte.
  The user will come back to these.
- Open the file for the user with a CLI command when you can.

For quizzes, write every answer at the same word count, and character count if you
can manage it. Formatting differences leak the answer.

## Assets

Reuse is the default. Read `assets/` before writing a lesson and build from what is
there. When a lesson needs something a future lesson could reuse, write it as a
component in `assets/` and link to it. Never inline something you will duplicate.

The shared stylesheet is the first component to write. It is what makes the lessons
read as one course instead of a pile of one-offs.

## Reference documents

Lessons get read once. Reference docs get read many times. After a lesson, compress
its essence into `reference/` in a format built for quick lookup: syntax tables,
algorithms, flowcharts, pose sequences, routines.

The glossary is the reference doc that matters most. Once a term is in `GLOSSARY.md`,
use that term everywhere, including inside other definitions.

## Wisdom and communities

When the user asks something that needs real-world judgement, answer as best you can,
then point them at a community where they can test it: a well-moderated forum, a
subreddit, a local class, an interest group. Find high-reputation ones.

If the user says they do not want to join a community, respect it and record that in
`RESOURCES.md` so future sessions stop suggesting it.

## Keeping state

- Write a learning record when the user demonstrates real understanding, discloses
  prior knowledge, corrects a misconception, or shifts the mission. Not for material
  merely covered.
- Add a glossary term only once the user can use it correctly.
- Update `NOTES.md` whenever the user states a preference about how they want to be
  taught.
- Missions change as understanding grows. That is normal. Confirm with the user, then
  update `MISSION.md` and write a learning record for the change.
