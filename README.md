# agent-skills

Agent skills for [Claude Code](https://claude.com/claude-code) and [Warp](https://www.warp.dev).

Skills are grouped by platform. The same skill may exist for both — sharing a design, but with platform-specific orchestration.

```
claude/   # Claude Code skills  -> ~/.claude/skills/
warp/     # Warp skills         -> ~/.agents/skills/
```

## Skills

### focus-group — `claude/` `warp/`

Convene a five-persona panel (Champion, Contrarian, Pragmatist, Strategist, User Advocate) to stress-test an idea instead of validating it. Round 1 runs the personas as independent agents, blind to each other, to reduce anchoring; Round 2 resumes each one with a digest of the others so they can genuinely update; a moderator then synthesizes a verdict that preserves real dissent rather than smoothing it into a reassuring middle.

Built to counter sycophancy — the reflex to mirror your framing and inflate a mediocre idea. Each persona names its own skew and calibrates its confidence, so you can weight the input instead of taking it at face value.

The two versions share the design and differ only in orchestration: the Claude Code build uses the Agent tool plus `SendMessage`; the Warp build uses a `run_agents` batch with addressable local agents.

## Installing

Copy the skill directory for your platform:

```sh
# Claude Code
cp -R claude/focus-group ~/.claude/skills/

# Warp
cp -R warp/focus-group ~/.agents/skills/
```

Or symlink it, so edits in this repo are picked up live (no re-copying):

```sh
# Claude Code
ln -s "$PWD/claude/focus-group" ~/.claude/skills/focus-group

# Warp
ln -s "$PWD/warp/focus-group" ~/.agents/skills/focus-group
```

Either way the skill is discovered automatically. Trigger it with `/focus-group <your idea>`.
