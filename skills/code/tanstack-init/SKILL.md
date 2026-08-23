---
name: tanstack-init
description: Scaffold a new TanStack Start project with the TanStack CLI, then build the application inside it against a fixed architectural spec. Defaults to shadcn ui and Tailwind CSS. Use only when the user asks to start or scaffold a new TanStack Start project, or says "/tanstack-init". Do not use for questions about TanStack, or for work inside a project that already exists.
---

# TanStack init

Two phases, in order. The CLI lays down the skeleton, then you build the
application inside it. Neither phase replaces the other, so do not stop after
the scaffold and do not hand-roll a skeleton the CLI would have generated.

## Default stack

- shadcn ui, via the `shadcn` add-on
- Tailwind CSS, which every standard scaffold enables on its own

Everything else is opt in, databases included. Ask before adding one, and add it
as a CLI add-on rather than a package.

## Phase 1: scaffold the skeleton

1. **Work out the project name and where it goes.** Scaffold into the current
   directory by default, using `--target-dir .`. Take the name from the user's
   request if it is there, otherwise ask.
   - The name is optional alongside `--target-dir .`. Leave it out and the CLI
     names the package after the current folder. Pass it to set the name yourself.
   - If the user would rather have the project in a new subdirectory, drop
     `--target-dir .` and pass the name on its own.
   - Check the target either way. If it has files in it, stop. Say what is there
     and ask. Never pass `-f` unless they ask for it in so many words. The CLI
     refuses a non-empty directory too, but it exits noisily, so check first.

2. **Read the add-on catalog before offering anything.** Run
   `npx @tanstack/cli@latest create --list-add-ons --framework <framework>`, using
   React unless the user already named a framework.
   - If the command fails, stop and report it. Do not guess add-on IDs and do not
     scaffold.
   - Match the default stack against what comes back. `shadcn` is the expected ID,
     and Tailwind should not be there at all because it is automatic.
   - If a default-stack item has no matching add-on, say so and ask the user how
     they want to proceed. Do not pass an ID the catalog did not list, do not
     install a package instead, and do not quietly drop the item.

3. **Offer the default stack.** Show the two items and ask the user to take them
   or configure the project themselves. Do not scaffold before they answer.
   - Anything the user already specified counts as answered. If they said pnpm,
     or said they want Solid, or asked for a database, do not ask about it again.
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
   npx @tanstack/cli@latest create <name> --target-dir . --framework React --package-manager <pm> --add-ons shadcn -y
   ```

   Keep it on one line. A backslash continuation breaks on Windows shells.

   Drop `--target-dir .` if the user asked for a new subdirectory instead. Add
   `--deployment`, `--toolchain`, `--no-examples`, `--no-git`, or `--no-intent`
   when the answers call for them.

   If the CLI exits with an error, report its output as it came out. Do not
   install anything, and do not rerun with different options on your own.

## Phase 2: build the application

This is the spec. Every build satisfies it:

> Build a TanStack Start application with file-based TanStack Router routes,
> validated search params, route loaders, typed server functions, full-document
> SSR, and streaming. Keep server-only work behind explicit boundaries, choose
> the appropriate SSR mode per route, and target the deployment runtime without
> changing the application model.

6. **Read what the CLI left behind first.** The scaffold writes `README.md`,
   `.cta.json`, and, unless `--no-intent` was passed, an `AGENTS.md` or
   `CLAUDE.md` wired up by TanStack Intent with skill mappings for the exact
   libraries it installed. Those files match the installed versions. Trust them
   over anything you remember about the API.

7. **Work out what the app actually does.** The spec above says how to build, not
   what to build. Take the subject from the user's request. If they have not said,
   ask before writing code.

8. **Build it, holding to every clause of the spec.**
   - File-based routes, one file per route under the router's route directory.
     Do not hand-register routes the file convention would generate.
   - Search params validated on the routes that read them. A plain validation
     function is enough. Zod is not in the default stack, so ask before adding a
     schema library rather than assuming one.
   - Route loaders fetch data. Do not fetch in component effects instead.
   - Server functions typed on both sides, called from routes and loaders.
   - Full-document SSR, with streaming for the parts that are slow.
   - Server-only work behind an explicit boundary so it cannot reach a client
     bundle. Secrets and database access live there and nowhere else.
   - SSR mode chosen per route, not set once globally and forgotten.
   - Target the deployment runtime through the CLI's deployment adapter. Do not
     reshape the application model to suit a host.

9. **Report.** Give the user:
   - where the project is and what the app does
   - the routes and server functions you created
   - the stack that actually landed
   - the next command to run, which is `npm run dev` when the scaffold went into
     the current directory, or `cd <name> && npm run dev` when it made a subdirectory
   - anything still needing configuration, by name and location

   A database add-on writes an `.env.example` with the connection variables left
   empty. Point at that file. Never invent a connection string and never ask the
   user to paste one into the chat.

## Rules

- Run only when the user asks for a new project. A question about TanStack Start,
  or work inside a project that already exists, is not a trigger.
- Read the catalog before offering choices, every time. Never hardcode add-on IDs
  in your head. The catalog changes.
- Never pipe keystrokes into the CLI's interactive prompts. Ask in chat, then pass
  flags. Driving the TUI breaks the moment the CLI reorders a prompt.
- Let the CLI install the dependencies. Do not run an install of your own in phase
  1, and do not reach for `npm i` in phase 2 without asking. An add-on wires up
  config and clients, so a same-named package would leave a project that looks
  configured and is not.
- Never overwrite a directory that has files in it.
- Do not start a dev server or commit anything unless the user asks.
