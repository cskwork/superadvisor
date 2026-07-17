# gstack: freeze (adapter)

> Faithful thin adapter of garrytan/gstack `freeze` (MIT). Source: https://github.com/garrytan/gstack/blob/main/freeze/SKILL.md
> Condensed method, not the verbatim skill. superadvisor reads this and follows the procedure.

**Purpose:** Lock file edits to one directory for the session; any Edit or Write outside it is blocked, not just warned.

**superadvisor routes here when:** "freeze", "restrict edits", "only edit this folder", "lock down edits" — or when debugging and you want to prevent accidentally "fixing" unrelated code, or scope changes to one module.

## Procedure
1. Ask which directory to restrict edits to. Text input, not multiple choice: "Which directory should I restrict edits to? Files outside this path will be blocked from editing."
2. Once the user gives a path, resolve it to an absolute path:
   ```bash
   FREEZE_DIR=$(cd "<user-provided-path>" 2>/dev/null && pwd)
   ```
3. Ensure a trailing slash and save to the freeze state file:
   ```bash
   FREEZE_DIR="${FREEZE_DIR%/}/"
   eval "$(~/.claude/skills/gstack/bin/gstack-paths)"
   STATE_DIR="$GSTACK_STATE_ROOT"
   mkdir -p "$STATE_DIR"
   echo "$FREEZE_DIR" > "$STATE_DIR/freeze-dir.txt"
   ```
4. Confirm: "Edits are restricted to `<path>/`. Any Edit or Write outside this directory will be blocked. To change the boundary, run `/freeze` again. To remove it, run `/unfreeze`."

## Gates & rules
- Enforcement is a `PreToolUse` hook (`freeze/bin/check-freeze.sh`) on matchers `Edit` and `Write`. It reads `file_path` from the tool input JSON; if the path does not start with the freeze directory it returns `permissionDecision: "deny"` — a hard block, not a warning.
- The trailing `/` is load-bearing: it stops `/src` from matching `/src-old`.
- Scope boundary: freeze governs ONLY the Edit and Write tools. Bash commands (e.g. `sed`) can still modify files outside the boundary — do not rely on freeze to sandbox shell writes.
- The boundary persists for the session via `freeze-dir.txt`. `/unfreeze` clears it.
