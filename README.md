# superadvisor

**Live:** [cskwork.github.io/superadvisor](https://cskwork.github.io/superadvisor/)

A single front-door **router skill** for the toolkit. You invoke one skill; it reads the request, picks the single best target, loads only that, and hands off. Modeled on [garrytan/gstack](https://github.com/garrytan/gstack)'s router pattern and cskwork's `supergoal` ("`SKILL.md` is the router; `reference/` carries procedure; load only what the phase needs").

## Why

The toolkit had 20 persona agents and 10 `super*` skills, each registered separately. `superadvisor` gives them one intelligent entry point and pulls in only the target a request needs — instead of keeping the whole catalog top-of-mind.

## What it routes to

| Class | Targets | Mechanism |
|---|---|---|
| **super\*** skills | supergoal, superdesign, superqa, superpm, supermarketer, superoffice, superinterview, superlearning, supertutor, superhacker | Skill tool (stay individually invocable) |
| **Personas** | 20 specialists in `agents/*.md` (analyst, architect, debugger, executor, code-reviewer, security-reviewer, …) | Agent tool |
| **gstack adapters** | ship, land-and-deploy, canary, office-hours, plan-ceo-review, careful, freeze, guard, document-release, document-generate, retro | read `reference/gstack/*.md` |

Full routing tables and precedence rules live in [`SKILL.md`](./SKILL.md).

## Structure

```
superadvisor/
├── SKILL.md                 # the router: dispatch tables + precedence + reference map
├── agents/*.md              # 20 persona definitions
└── reference/
    ├── personas.md          # spawn mechanics (registered vs router-internal)
    ├── super-skills.md      # domain boundaries + overlap tie-breaks
    └── gstack/*.md          # 11 faithful thin adapters (MIT, from garrytan/gstack)
```

## Registration model (Hybrid)

- **super\*** skills stay registered — `/superqa` etc. still work directly. The router dispatches to them via the Skill tool.
- **Personas**: `architect`, `code-reviewer`, `planner`, `security-reviewer` stay in `~/.claude/agents/` (global rules call them by name). The other 16 are dispatched from here by passing their file body as the subagent prompt; de-register them from `~/.claude/agents/` to realize the context saving. See `reference/personas.md`.
- **gstack** skills are not installed or registered — they are distilled into thin adapters the router reads on demand.

## Install

```bash
ln -s /Users/danny/Documents/PARA/Resource/superadvisor ~/.claude/skills/superadvisor
```

## Use

```
/superadvisor <request>        # or just describe the task
```

The router prints its choice as a one-line `route: <target>` before handing off. If you already invoked a specific `/super*` skill, it defers — no double-routing.

## Credits

gstack adapters are condensed from [garrytan/gstack](https://github.com/garrytan/gstack) (MIT). `super*` skills are cskwork's own. Persona set is the user's installed agent pack.
