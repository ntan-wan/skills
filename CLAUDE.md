# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A Claude Code plugin marketplace. There is no build, test, or lint step. The
deliverable is Markdown. Every `SKILL.md` is a prompt that a future Claude session
loads and follows, so editing one changes agent behavior directly. Read a skill in
full before editing it.

## Architecture

Three files stay in sync, and a change usually touches all of them:

1. `.claude-plugin/marketplace.json` is the catalog. Each entry in `plugins` is a
   complete plugin definition, so `strict: false` and no per-plugin `plugin.json`.
2. `skills/<plugin>/<skill-name>/SKILL.md` is the content. Every plugin points
   `source` at the repo root `"./"` and scopes itself with
   `skills: ["./skills/<plugin>"]`. That scoping is the only thing stopping plugins
   from loading each other's skills out of the shared `skills/` folder, so never
   broaden a `skills` path to `./skills`.
3. `README.md` holds the table of plugins and skills.

The easy mistake: installed copies only pick up a change when the plugin's `version`
in `marketplace.json` changes. Editing a `SKILL.md` without bumping that version
ships nothing. Minor bump for a new skill, patch bump for edits to an existing one.

## Frontmatter contract

`name` is kebab-case and matches the directory name exactly. `description` is the
only text Claude reads when deciding whether to load the skill, so it states both
what the skill does and when it fires, including the literal slash-command form
(`says "/commit"`) when the skill is meant to be invocable that way. A description
that only says what the skill does will never trigger.

## Untracked directories

`.gitignore` excludes `openspec/` and `.claude/`. They exist locally but are not part
of the published repo, and neither belongs in a plugin's `skills` path.

- `openspec/` holds planning artifacts (proposals, specs, tasks) for work on this
  repo. The manifest never references it, so it is never installed as a skill.
- `.claude/` holds local settings plus a local copy of the openspec skills and
  commands.

## Conventions

- Commits follow Conventional Commits with the plugin name as scope:
  `feat(code): add roast skill`, `fix(code): correct unslop reference in eli5`.
- Only create `references/`, `scripts/`, or `assets/` in a skill directory when there
  is real content for them. `SKILL.md` alone is the normal case.
- Write skill instructions as imperative steps addressed to Claude ("Inspect state",
  "Stop and tell the user"), not as documentation about the skill.
- `skills/code/unslop` is marked "Must always apply". Its rules govern prose written
  in this repo, including new `SKILL.md` bodies and README edits. No em dashes,
  sentence case headings, straight quotes.
