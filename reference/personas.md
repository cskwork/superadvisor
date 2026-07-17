# Personas — spawn mechanics

Personas are single-role specialists defined in `agents/<name>.md`. `superadvisor` spawns one, hands it the task, and relays the result. It never runs a persona inline.

## Two spawn paths (Hybrid registration)

**Registered** — still live in `~/.claude/agents/`, callable by name:
`architect`, `code-reviewer`, `planner`, `security-reviewer`.
```
Agent(subagent_type="code-reviewer", prompt="<task>")
```
These stay registered because the user's global rules (`~/.claude/rules/common/agents.md`, `security.md`) invoke them by name.

**Router-internal** — the other 16 live only here (de-registered to keep the session lean). The Agent tool cannot resolve them by name, so pass the definition as the prompt prefix:
```
persona = Read("agents/<name>.md")            # strip YAML frontmatter, keep the body
Agent(subagent_type="general-purpose",
      prompt = persona_body + "\n\n---\n\nTask:\n<task>")
```
Router-internal set: `analyst, code-simplifier, critic, debugger, designer, document-specialist, executor, explore, git-master, qa-tester, scientist, stitch-kit, test-engineer, tracer, verifier, writer`.

> If a router-internal persona were ever re-registered in `~/.claude/agents/`, prefer the `subagent_type="<name>"` path — it is equivalent and cheaper.

## Read-only advisors (no Write/Edit)

Spawn these for analysis/verdicts only; apply their output yourself or via `executor`:
`analyst, architect, critic, document-specialist, explore, scientist, security-reviewer`.
`verifier` and `tracer` are evidence/verdict roles — treat their output as findings, not applied changes.

## Notes

- Preserve each persona's frontmatter intent even when passing the body as a prompt: if it says read-only, do not grant it Write in the task framing.
- Some persona bodies reference `oh-my-claudecode:*` subagents for cross-validation. That is a pre-existing optional dependency; skip if the plugin is absent.
- Personas are building blocks. For a full multi-step objective, prefer routing to a **super\*** workflow (which orchestrates its own roles) instead of hand-wiring personas — see `super-skills.md`.
