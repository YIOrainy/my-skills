# my-skills

My personal collection of [agent skills](https://skills.sh/) for Claude Code and
other agents. Each skill lives in `skills/<name>/SKILL.md`.

## Skills

| Skill | What it does |
|---|---|
| [`teach-me`](skills/teach-me/SKILL.md) | A tutor that builds genuine, durable understanding of a topic from the ground up — maps the topic, interviews you, then teaches one layer at a time. |

## Install

Install a single skill globally (user-level, available in every project):

```bash
npx skills add YIOrainy/my-skills@teach-me -g
```

Install **all** skills from this repo:

```bash
npx skills add YIOrainy/my-skills -g
```

Drop the `-g` flag to install into the current project only (`./.claude/skills/`).

## Use

After installing, type `/teach-me` in Claude Code, or just phrase a request that
matches the skill's trigger (e.g. "teach me how X works").

## Manage

```bash
npx skills check     # see which installed skills have updates
npx skills update    # pull the latest version of installed skills
```

## Add a new skill to this repo

1. Create a folder: `skills/<your-skill-name>/`
2. Add a `SKILL.md` with YAML frontmatter (`name`, `description`) followed by the
   skill instructions.
3. Commit and push. It's instantly installable via
   `npx skills add YIOrainy/my-skills@<your-skill-name>`.
