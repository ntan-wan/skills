---
name: tanstack
description: Scaffold a TanStack Start project with the TanStack CLI using fixed defaults (ESLint, Cloudflare, shadcn, Drizzle, Neon, Better Auth, PostgreSQL, no demo pages). Use when the user asks to start or create a TanStack Start app, or says "/tanstack".
---

# TanStack

Create a TanStack Start project with the TanStack CLI. Use these defaults unless the
user explicitly says otherwise:

| Choice | Default | CLI flag |
| ------ | ------- | -------- |
| Framework | React | `--framework React` |
| Package manager | pnpm | `--package-manager pnpm` |
| Toolchain | ESLint | `--toolchain eslint` |
| Deployment | Cloudflare | `--deployment cloudflare` |
| Demo pages | none | `--no-examples` |
| Add-ons | shadcn, Drizzle, Neon, Better Auth | `--add-ons shadcn,drizzle,neon,better-auth` |
| Database | PostgreSQL | `--add-on-config '{"drizzle":{"database":"postgresql"}}'` |

## Steps

1. Inspect the target folder. If `package.json` already depends on
   `@tanstack/react-start`, skip scaffolding and work on the existing project.
2. Ask for the project name if the user has not given one.
3. Run the CLI non-interactively, so no prompt needs answering:

```
npx @tanstack/cli@latest create <name> -y --framework React --package-manager pnpm \
  --toolchain eslint --deployment cloudflare --no-examples \
  --add-ons shadcn,drizzle,neon,better-auth \
  --add-on-config '{"drizzle":{"database":"postgresql"}}'
```

4. Apply any override the user stated by changing the matching flag. Run
   `npx @tanstack/cli@latest create --list-add-ons --framework React --json` to look up
   add-on IDs, and `--addon-details <id>` for an add-on's options.
5. If an add-on or flag is rejected, stop and tell the user which one. Do not
   substitute another add-on or hand-wire it.
6. If the output reports that `@tanstack/intent install` failed, run
   `pnpm dlx @tanstack/intent install` inside the new project.
7. Tell the user what was created, and list the env vars the add-ons expect (for
   example `BETTER_AUTH_SECRET` and the database URL) without printing any values.

## Rules

- PostgreSQL is a Drizzle option, not a separate add-on. Never pass `postgres` to
  `--add-ons`.
- Neon and Drizzle are compatible: Neon is the database, Drizzle is the ORM.
- Never overwrite a non-empty folder. Do not pass `--force` unless the user asks.
