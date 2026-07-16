# claude-skills

A collection of [Claude Code](https://claude.com/claude-code) skills.

## Skills

- **[focus-group](focus-group/)** — Convene a 5-persona panel (Champion, Contrarian, Pragmatist, Strategist, User Advocate) to stress-test an idea instead of validating it. Round 1 runs as five blind parallel subagents to reduce anchoring; Round 2 cross-pollinates; a moderator synthesizes an honest verdict that preserves genuine dissent. Built to counter sycophancy and confirmation bias.

## Installing a skill

Copy a skill directory into your Claude Code skills folder:

```sh
cp -R focus-group ~/.claude/skills/
```

Or symlink it, so edits in this repo are picked up live (no re-copying):

```sh
ln -s "$PWD/focus-group" ~/.claude/skills/focus-group
```

Either way, Claude Code discovers it automatically. Trigger it with `/focus-group <your idea>`.
