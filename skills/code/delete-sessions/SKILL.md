---
name: delete-sessions
description: Delete Claude Code session transcripts for the current project, either all of them except the one running now or a specific set the user picks. Use when the user asks to delete, clear, clean up, or prune their Claude sessions or chat history, or says "/delete-sessions".
---

# Delete sessions

Sessions are transcript files on disk. Deleting one is permanent, so the whole
skill is built around showing the user exactly what goes before anything goes.

Scope is the current project only. Never touch another project's folder, even if
the user's wording sounds broad. If they want a different project, ask them to
run the skill from that directory.

## Steps

1. **Find the project folder.** Session files live in the Claude home directory,
   under `.claude/projects/<slug>/`. Resolve the home directory from the
   environment rather than typing a path, and note that the separator inside it
   differs by platform.
   - The slug is the project's absolute path with every character that cannot
     appear in a folder name replaced by `-`. That covers the path separator on
     any platform, and the drive colon on Windows.
   - Do not build the slug by hand and assume it exists. List what is actually in
     the projects directory and match it against the current working directory,
     comparing case insensitively. Filesystems differ on case, so a literal
     comparison misses folders that are really the same project.
   - Collect every folder that matches, not just the first. One project can end
     up with more than one folder, and each holds real sessions.
   - If nothing matches, say so and stop. Do not go looking in other folders.

2. **Identify the current session and rule it out.** Every turn appends to the
   running session's transcript, so it is the `.jsonl` with the newest
   modification time across the matched folders.
   - Confirm it. Pick a string that only this conversation could contain, such as
     the user's most recent message, and search the candidate file for it.
   - If the search does not find it, stop and tell the user you cannot tell which
     session is the live one. Do not guess and do not fall back to mtime alone.
     Deleting the running session is the one unrecoverable mistake here.

3. **List everything else.** Build a numbered table of the remaining sessions:
   - number
   - the first 8 characters of the file's uuid
   - last modified time
   - size
   - a title, taken from the first user message in the transcript, trimmed to one
     line. If the file has no user message, say `(empty)`.

   Sort newest first. If the only session is the current one, say there is
   nothing to delete and stop.

4. **Offer the two options.** Ask which one, and wait:
   - delete all except the current one
   - delete specific sessions, by number

   For the second option, take the numbers from the user and read them back as
   the actual sessions they map to. If a number is out of range, say which one
   and ask again rather than dropping it.

5. **Confirm before deleting.** Show the final list of files that will go, with
   the count, and state plainly that this is permanent and there is no undo. Ask
   the user to reply `delete` to go ahead.
   - Anything other than that explicit confirmation means stop. A "sure", a
     "yes ok", or silence is not the confirmation. Ask once more, then drop it.
   - Never widen the list after the confirmation. If the user changes their mind
     about which sessions, go back to step 4 and confirm the new list.

6. **Delete.** For each confirmed session, remove `<uuid>.jsonl` and, if one
   exists beside it, the directory named `<uuid>`. Some sessions carry that
   sidecar directory and leaving it behind orphans it.
   - Delete one at a time and note any that fail. A locked or missing file is not
     a reason to abandon the rest.

7. **Report.** Give the count deleted, the count that failed with the reason, and
   what is left, including the current session named as the one kept.

## Rules

- Never delete the session that is running. If step 2 cannot prove which one it
  is, the skill stops.
- Never delete without the explicit confirmation from step 5. Showing the list is
  not asking, and asking is not the same as hearing yes.
- Never expand scope past the current project's folders.
- Do not delete anything else in the Claude home directory, including settings,
  plugins, or the projects folder itself. Only `<uuid>.jsonl` files and their
  sidecar directories.
- Do not archive, move, or back up as a substitute. The user asked for deletion.
  If they want a copy first, they will say so.
