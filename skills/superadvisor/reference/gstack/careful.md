# gstack: careful (adapter)

> Faithful thin adapter of garrytan/gstack `careful` (MIT). Source: https://github.com/garrytan/gstack/blob/main/careful/SKILL.md
> Condensed method, not the verbatim skill. superadvisor reads this and follows the procedure.

**Purpose:** Safety guardrails for destructive commands. Warn before `rm -rf`, `DROP TABLE`, force-push, `git reset --hard`, `kubectl delete`, and similar; the user can override each warning.

**superadvisor routes here when:** "be careful", "safety mode", "prod mode", "careful mode", "warn before destructive". Also when touching prod, debugging live systems, or working in a shared environment.

## Procedure
1. Activating turns safety mode ON: every Bash command is checked for destructive patterns BEFORE it runs (a `PreToolUse` Bash hook, `careful/bin/check-careful.sh`).
2. On a destructive match, the hook returns `permissionDecision: "ask"` with a warning; the user chooses proceed or cancel. The warning can always be overridden.
3. Protected patterns (pattern — risk):
   - `rm -rf` / `rm -r` / `rm --recursive` — recursive delete
   - `DROP TABLE` / `DROP DATABASE` — data loss
   - `TRUNCATE` — data loss
   - `git push --force` / `-f` — history rewrite
   - `git reset --hard` — uncommitted work loss
   - `git checkout .` / `git restore .` — uncommitted work loss
   - `kubectl delete` — production impact
   - `docker rm -f` / `docker system prune` — container/image loss
4. Safe exceptions (allowed with no warning): `rm -rf` of `node_modules`, `.next`, `dist`, `__pycache__`, `.cache`, `build`, `.turbo`, `coverage`.
5. Session-scoped: deactivate by ending the conversation or starting a new one.

## Gates & rules
- Every detected destructive command requires an explicit decision before it runs — never execute a matched pattern silently. The user can always override and proceed.
- When a destructive confirmation is rendered as prose instead of a tool gate, make the gate stronger: require an explicit typed confirmation (the exact letter/word), state plainly what is irreversible, and never proceed on a vague reply ("ok"/"sure"/silence) — re-ask.
- Guardrail, not a hard block: the point is a conscious pause on one-way doors (delete, drop, force-push, overwrite), not preventing the user from acting.
