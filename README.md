# Claude skills collection

A personal collection of custom [Claude Code](https://code.claude.com/docs) skills,
distributed as a plugin marketplace. Skills are grouped into plugins by category, so
you install only the categories you want.

## Skills

| Plugin | Skill    | What it does                                                                       |
| ------ | -------- | ---------------------------------------------------------------------------------- |
| `code` | `commit` | Drafts a Conventional Commits message from your staged changes, then commits it after you approve. |
| `code` | `debrief` | Asks what you want to learn (how the codebase works, what recent changes do, or your own topic), then teaches from the code and ends with a list of what to learn next. |
| `code` | `discover` | Researches how existing software handles a feature, then decides per behaviour whether to copy, improve, compare, or skip it. |
| `code` | `eli5`   | Explains something in plain words with one everyday comparison.                    |
| `code` | `roast`  | Interrogates a plan round by round until nothing is left assumed.                   |
| `code` | `stories` | Turns research and a feature idea into prioritized user stories with acceptance criteria, an MVP cut, and non-goals. |
| `code` | `tanstack` | Builds a TanStack Start app to a fixed architectural spec: file-based routes, validated search params, loaders, typed server functions, SSR, and streaming. |
| `code` | `unslop` | Cuts AI tells from writing.                                                        |
| `learn` | `teach` | Teaches a topic across sessions in a workspace of missions, lessons, and learning records. |

## Third-party skills

Skills maintained in other people's repos, not in this one.

### Listed in this marketplace

These install like the plugins above, for example
`/plugin install emil-design@wang-skills`. Each one pulls from its upstream repo, so
updates come from there.

| Plugin | Source | What it does |
| ------ | ------ | ------------ |
| `emil-design` | [emilkowalski/skills](https://github.com/emilkowalski/skills) | Design and animation skills for designers and engineers. |
| `mengto-design` | [MengTo/Skills](https://github.com/MengTo/Skills) | Web design, UI, media, and game development skills, the four categories this marketplace pulls from that repo. |
| `elaya-design` | [elayadesign/ai-design-skills](https://github.com/elayadesign/ai-design-skills) | Landing page design skill. |
| `impeccable` | [pbakaus/impeccable](https://github.com/pbakaus/impeccable) | Frontend design skill with `/impeccable` commands such as `critique`, `audit`, and `polish`, plus checks for common AI design patterns. See [impeccable.style](https://impeccable.style/). |

`impeccable` ships its own hooks and a subagent, so this marketplace loads the whole
upstream plugin instead of pointing at a skills folder.

### Installed separately

#### openspec

[OpenSpec](https://github.com/Fission-AI/OpenSpec) is a spec-driven workflow: you
propose a change, it writes the proposal, specs, and tasks, then you apply and
archive it. Its skills (`openspec-propose`, `openspec-apply-change`,
`openspec-archive-change`, and more) drive the `openspec` CLI, so install the CLI
first. It is not a plugin marketplace, so `/plugin marketplace add` will not work
on it.

```
npm install -g @fission-ai/openspec@latest
cd your-project
openspec init
```

`openspec init` scaffolds the `openspec/` directory and writes the skills and slash
commands into that project's `.claude/`. To pull only the skills into a skills.sh
compatible agent, run `npx skills add Fission-AI/OpenSpec` instead, and install the
CLI separately.

#### agent-reach

[Agent Reach](https://github.com/Panniantong/Agent-Reach) lets an agent read and
search the web through one CLI: Twitter, Reddit, YouTube, GitHub, Bilibili,
XiaoHongShu, and more, with no paid APIs. It ships a `SKILL.md` that the CLI
registers with your agent. It is not a plugin marketplace, so install it by asking
your agent to follow the upstream guide:

```
Help me install Agent Reach: https://raw.githubusercontent.com/Panniantong/agent-reach/main/docs/install.md
```

Some channels need your cookies or a logged-in browser session (Twitter, XiaoHongShu,
LinkedIn), so read the guide before granting them.

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
