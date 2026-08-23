# Claude skills collection

A personal collection of custom [Claude Code](https://code.claude.com/docs) skills,
distributed as a plugin marketplace. Skills are grouped into plugins by category, so
you install only the categories you want.

## Skills

| Plugin | Skill    | What it does                                                                       |
| ------ | -------- | ---------------------------------------------------------------------------------- |
| `code` | `commit` | Drafts a Conventional Commits message from your staged changes, then commits it after you approve. |
| `code` | `eli5`   | Explains something in plain words with one everyday comparison.                    |
| `code` | `roast`  | Interrogates a plan round by round until nothing is left assumed.                   |
| `code` | `tanstack-init` | Scaffolds a new TanStack Start project with shadcn ui, Tailwind CSS, and Neon Postgres. |
| `code` | `unslop` | Cuts AI tells from writing.                                                        |
| `learn` | `teach` | Teaches a topic across sessions in a workspace of missions, lessons, and learning records. |

## Install

```
/plugin marketplace add <path-or-git-url-to-this-repo>
/plugin install code@wang-skills
```

Use the local path (for example `/plugin marketplace add c:\Users\User\skills`) while
developing, or the git URL once it is hosted.

Pick up later changes with:

```
/plugin marketplace update wang-skills
```

## Add a skill

1. Create `skills/<plugin>/<skill-name>/SKILL.md`. The directory name is kebab-case
   and matches `name` in the frontmatter.
2. Give it YAML frontmatter:

   ```markdown
   ---
   name: my-skill
   description: What the skill does. Use when <the situations that should trigger it>.
   ---

   # My skill

   ...instructions for Claude...
   ```

   `description` is the only text Claude reads when deciding whether to load the
   skill, so state both what it does and when it should fire.
3. Bump that plugin's `version` in `.claude-plugin/marketplace.json`. Installed
   copies ignore the change otherwise.
4. Add a row to the table above.

For a new plugin, create `skills/<plugin>/` and copy the existing entry in the
`plugins` array of `.claude-plugin/marketplace.json`, changing `name`, `skills`, and
`description`.

## License

MIT. See [LICENSE](LICENSE).
