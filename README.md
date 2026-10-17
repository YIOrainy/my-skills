# my-skills

My personal collection of [agent skills](https://skills.sh/) for Claude Code and
other agents. Each skill lives in `skills/<name>/SKILL.md`.

## Skills

| Skill | What it does |
|---|---|
| [`teach-me`](skills/teach-me/SKILL.md) | A tutor that builds genuine, durable understanding of a topic from the ground up — maps the topic, interviews you, then teaches one layer at a time. |
| [`ui-expert`](skills/ui-expert/SKILL.md) | `/ui-expert <screen>` — researches a screen's job on Mobbin, then publishes a page of phone-frame design options (Today + A–D) to pick from, one new page per round, logged in `docs/design-rounds/`. Stops at the pick; building is a separate session. |

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
matches the skill's trigger (e.g. "teach me how X works"). `ui-expert` is
slash-only: type `/ui-expert <screen>` (e.g. `/ui-expert captain My Deliveries`).

## Manage

```bash
npx skills check     # see which installed skills have updates
npx skills update    # pull the latest version of installed skills
```
