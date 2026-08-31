# cursor-skills

Personal [Cursor Agent Skills](https://cursor.com/docs/skills) — portable workflows for the agent.

## Skills

| Skill | Description |
| --- | --- |
| [clean-final-state](./clean-final-state/) | Final residue check before delivering code, tests, docs, and PRs. Removes traces of rejected ideas so outputs match the latest accepted requirements. |

## Install

### One skill (global)

```bash
git clone git@github.com:lydiatango/cursor-skills.git /tmp/cursor-skills
cp -R /tmp/cursor-skills/clean-final-state ~/.cursor/skills/
```

### From Cursor UI

1. Open **Customize** → **Rules**
2. Click **Add Rule** → **Remote Rule (GitHub)**
3. Enter `https://github.com/lydiatango/cursor-skills`

### Per-project

Copy a skill folder into your repo:

```text
your-repo/.cursor/skills/clean-final-state/SKILL.md
```

## Usage

Skills are discovered automatically. Invoke explicitly in Agent chat with `/clean-final-state`.
