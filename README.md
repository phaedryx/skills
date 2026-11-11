# skills

Agentic skills for Claude Code. Each top-level directory is a self-contained skill: copy or symlink it into `~/.claude/skills/`.

| Skill | What it does |
|---|---|
| [kaleidonym](kaleidonym/SKILL.md) | Names (or renames) one specific thing by reframing it with ideonomy's operators and dimension-prompts, then picks a candidate. |

## Install

```bash
git clone https://github.com/phaedryx/skills.git
ln -s "$PWD/skills/kaleidonym" ~/.claude/skills/kaleidonym
```
