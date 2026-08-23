---
name: tanstack-init
description: Scaffold a new TanStack Start project with the TanStack CLI, defaulting to shadcn ui, Tailwind CSS, and Neon Postgres. Use only when the user asks to start or scaffold a new TanStack Start project, or says "/tanstack-init". Do not use for questions about TanStack, or for work inside a project that already exists.
---

# TanStack init

Scaffold a new TanStack Start project with `npx @tanstack/cli@latest create`.

Ask one question, run one command. Only widen to the full question set if the
user turns down the default stack.

## Default stack

- shadcn ui, via the `shadcn` add-on
- Tailwind CSS, which every standard scaffold enables on its own
- Neon Postgres, via the `neon` add-on

The CLI covers all three. Nothing gets installed on top.

## Steps

1. **Work out the project name and target directory.** Take them from the user's
   request if they are already there. Otherwise ask. Then check the target:
   - If it does not exist or is empty, carry on.
   - If it has files in it, stop. Say what is already there and ask the user what
     they want. Never pass `-f` unless they ask for it in so many words.

2. **Read the add-on catalog before offering anything.** Run
   `npx @tanstack/cli@latest create --list-add-ons --framework <framework>`, using
   React unless the user already named a framework.
   - If the command fails, stop and report it. Do not guess add-on IDs and do not
     scaffold.
   - Match the default stack against what comes back. `shadcn` and `neon` are the
     expected IDs, and Tailwind should not be there at all because it is automatic.
   - If a default-stack item has no matching add-on, say so and ask the user how
     they want to proceed. Do not pass an ID the catalog did not list, do not
     install a package instead, and do not quietly drop the item.

3. **Offer the default stack.** Show the three items and ask the user to take
   them or configure the project themselves. Do not scaffold before they answer.
   - Anything the user already specified counts as answered. If they said pnpm,
     or said they want Solid, or said no database, do not ask about it again.
   - If they take the default, go straight to step 5.

4. **If they decline, ask the rest one question at a time.** Ask in the
   conversation, one per turn, with the real choices for each option:
   - framework: `React` or `Solid`. Ask this one first. The catalog is
     per-framework, so if the answer is not what step 2 listed against, run
     `--list-add-ons` again for the chosen framework before asking about add-ons.
   - package manager: `npm`, `yarn`, `pnpm`, `bun`, or `deno`
   - add-ons: whatever the catalog returned for the chosen framework
   - deployment: `cloudflare`, `netlify`, `nitro`, or `railway`
   - toolchain: `biome` or `eslint`
   - example pages, git repo, and TanStack Intent agent config, each on or off

   If the user stops answering, use the CLI's own default for what is left and
   tell them which defaults you used.

5. **Scaffold in one non-interactive run.** Build a single command carrying every
   resolved choice:

   ```
   npx @tanstack/cli@latest create <name> --framework React --package-manager <pm> --add-ons shadcn,neon -y
   ```

   Keep it on one line. A backslash continuation breaks on Windows shells.

   Add `--target-dir`, `--deployment`, `--toolchain`, `--no-examples`, `--no-git`,
   or `--no-intent` when the answers call for them.

   If the CLI exits with an error, report its output as it came out. Do not
   install anything, and do not rerun with different options on your own.

6. **Report and stop.** Give the user:
   - where the project is
   - the stack that actually landed
   - the next command to run, usually `cd <name> && npm run dev`
   - anything still needing configuration, by name and location

   Neon writes `.env.example` with `DATABASE_URL` and `DATABASE_URL_POOLER` left
   empty. Point at that file. Never invent a connection string and never ask the
   user to paste one into the chat.

## Rules

- Run only when the user asks for a new project. A question about TanStack Start,
  or work inside a project that already exists, is not a trigger.
- Read the catalog before offering choices, every time. Never hardcode add-on IDs
  in your head. The catalog changes.
- Never pipe keystrokes into the CLI's interactive prompts. Ask in chat, then pass
  flags. Driving the TUI breaks the moment the CLI reorders a prompt.
- Run no install command of your own. The CLI installs the dependencies. If the
  catalog cannot cover something the user wants, that is theirs to decide, not a
  gap to paper over with `npm i`. An add-on wires up config and clients, so a
  same-named package would leave a project that looks configured and is not.
- Never overwrite a directory that has files in it.
- Stop once the scaffold is reported. No dev server, no commit, no application
  code.
