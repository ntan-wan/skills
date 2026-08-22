# Claude Skills Collection

A personal collection of custom [Claude Code](https://code.claude.com/docs) skills,
distributed as a plugin marketplace. Skills are grouped into plugins by category, so
you install only the categories you want.

## What's in here

| Plugin   | Skills   | What it does                                                  |
| -------- | -------- | ------------------------------------------------------------- |
| `code` | `commit` | Writes a Conventional Commits message from your staged changes and commits it after you approve the draft. |
| `code` | `eli5` | Explains anything in plain words with one everyday comparison, no jargon. |

## Install

Add the marketplace, then install the plugin you want:

```
/plugin marketplace add <path-or-git-url-to-this-repo>
/plugin install code@wang-skills
```

Use the local path (for example `/plugin marketplace add c:\Users\User\skills`) while
developing, or the git URL once it is hosted.

After installing, `commit` activates when you ask Claude to commit, ask it to write or
improve a commit message, or type `/commit`. It drafts a message and waits for your
approval — it will not commit on its own.

To pick up later changes to this repository:

```
/plugin marketplace update wang-skills
```

## Layout

```
.claude-plugin/
└── marketplace.json      # the catalog: which plugins exist and where their skills live
skills/
└── <plugin>/             # one directory per plugin (category)
    └── <skill-name>/     # one directory per skill, kebab-case
        ├── SKILL.md      # required
        ├── references/   # optional
        ├── scripts/      # optional
        └── assets/       # optional
README.md
LICENSE
```

`SKILL.md` is the only required file in a skill directory. Create `references/`,
`scripts/`, or `assets/` **only when a skill actually has content for them** — don't
commit them empty.

`openspec/` holds the planning artifacts for this repository. It is not referenced by
the manifest, so it is never installed as a skill.

## Add a new skill

1. Create `skills/<plugin>/<skill-name>/SKILL.md`, where `<skill-name>` is kebab-case
   and matches the `name` in the frontmatter.
2. Give it YAML frontmatter delimited by plain `---` lines:

   ```markdown
   ---
   name: my-skill
   description: What the skill does. Use when <the situations that should trigger it>.
   ---

   # My Skill

   ...instructions for Claude...
   ```

   The `description` is what Claude matches against to decide whether to load the
   skill, so state both **what it does** and **when to use it**.
3. If the skill belongs to a plugin that already exists, you're done — the plugin's
   `skills` path covers the whole category directory. Bump that plugin's `version` in
   `marketplace.json` so installed copies pick the change up.
4. Run `/plugin marketplace update <marketplace-name>` to reload.

## Add a new plugin

1. Create the category directory, e.g. `skills/finance/`, and put at least one skill in it.
2. Add an entry to the `plugins` array in `.claude-plugin/marketplace.json`:

   ```json
   {
     "name": "finance",
     "source": "./",
     "skills": ["./skills/finance"],
     "description": "Valuation models and invoice parsing",
     "version": "0.1.0",
     "license": "MIT",
     "strict": false
   }
   ```

   - `name` must be kebab-case and unique in this marketplace — users type it as
     `/plugin install <name>@<marketplace-name>`.
   - `source` stays `"./"` (the repo root) and `skills` names that plugin's own
     directory. This is how several plugins share one `skills/` folder without
     loading each other's skills.
   - `strict: false` means the marketplace entry is the complete definition, so the
     plugin directory needs no `plugin.json` of its own.
3. Bump the `version` whenever the plugin's skills change: minor for a new skill,
   patch for edits to an existing one. Users only receive updates when it changes.

## License

MIT — see [LICENSE](LICENSE).
