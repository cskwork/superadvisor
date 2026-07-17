# gstack: guard (adapter)

> Faithful thin adapter of garrytan/gstack `guard` (MIT). Source: https://github.com/garrytan/gstack/blob/main/guard/SKILL.md
> Condensed method, not the verbatim skill. superadvisor reads this and follows the procedure.

**Purpose:** Full safety mode = `/careful` (destructive-command warnings) + `/freeze` (directory-scoped edits) in a single command. Maximum safety when touching prod or debugging live systems.

**superadvisor routes here when:** "guard mode", "full safety", "lock it down", "maximum safety", "guard against mistakes", "안전 모드", "실수 방지" — especially before working on production or a live system.

## Procedure
1. Ask which directory to restrict edits to via AskUserQuestion, text input: "Guard mode: which directory should edits be restricted to? Destructive command warnings are always on. Files outside the chosen path will be blocked from editing."
2. Resolve and persist the freeze boundary (identical to /freeze):
   ```bash
   FREEZE_DIR=$(cd "<user-provided-path>" 2>/dev/null && pwd)
   FREEZE_DIR="${FREEZE_DIR%/}/"
   eval "$(~/.claude/skills/gstack/bin/gstack-paths)"
   STATE_DIR="$GSTACK_STATE_ROOT"
   mkdir -p "$STATE_DIR"
   echo "$FREEZE_DIR" > "$STATE_DIR/freeze-dir.txt"
   ```
3. Tell the user guard mode is active, with its two protections:
   1. Destructive command warnings — rm -rf, DROP TABLE, force-push, etc. warn before executing (override allowed).
   2. Edit boundary — file edits restricted to `<path>/`; edits outside are blocked.
   Removal: `/unfreeze` drops the edit boundary; end the session to deactivate everything.

## Gates & rules
- Composes two skills, so it wires three `PreToolUse` hooks: `Bash` → `careful/bin/check-careful.sh` (warn before destructive commands, user can override); `Edit` and `Write` → `freeze/bin/check-freeze.sh` (block edits outside the boundary, a hard deny per /freeze).
- Asymmetry to preserve: destructive-command protection WARNS (overridable); the freeze boundary BLOCKS (not overridable). Destructive warnings are always on; the edit boundary is the directory the user picks.
- Dependency note: both the `/careful` and `/freeze` skill directories must be installed (gstack setup installs them together).
- See `/careful` for the full destructive-command pattern list and safe exceptions; see `/freeze` for how boundary enforcement works.
