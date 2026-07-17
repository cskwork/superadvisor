# gstack: ship (adapter)

> Faithful thin adapter of garrytan/gstack `ship` (MIT). Source: https://github.com/garrytan/gstack/blob/main/ship/SKILL.md
> Condensed method, not the verbatim skill. superadvisor reads this and follows the procedure.

**Purpose:** Sync base branch, run tests, audit coverage, review, bump VERSION, update CHANGELOG, commit, push, open PR. Never push on red.

**superadvisor routes here when:** "ship it", "create a PR", "push to main", "deploy this", "merge and push", "get it deployed", "code is ready". Korean: "배포해줘", "PR 올려줘", "푸시해줘", "이거 올려줘".

## Procedure
Fully automated, non-interactive. `/ship` means DO IT — run straight through, output the PR URL. Re-running re-runs the WHOLE checklist: every verification step (tests, coverage, plan, review, VERSION/CHANGELOG, TODOS) runs every invocation; only actions are idempotent (skip bump if bumped, skip push if pushed, update PR if it exists).

0. Detect platform (GitHub/GitLab/unknown) + base branch. Substitute detected base in all git/PR commands.
1. Pre-flight: if on the base branch → ABORT ("Ship from a feature branch"). `git status` (never `-uall`); uncommitted changes always included. `git diff <base>...HEAD --stat`, `git log <base>..HEAD --oneline`. Review-readiness dashboard (reviews never block shipping).
2. Distribution pipeline check: new artifact (cmd/main.go, bin/, package.json) → check for a release workflow; if none, ask to add.
3. Merge base branch BEFORE tests: `git fetch origin <base> && git merge origin/<base> --no-edit`. Unresolvable conflict → STOP, show conflicts.
4-6. Run test suites (+ eval suites if prompt files changed) → sections/tests.md.
7. Coverage audit of the diff → sections/test-coverage.md. Hard gate: coverage below min threshold blocks (user override allowed). Generated coverage tests must pass before commit.
8. Plan completion / verification / scope-drift → sections/plan-completion.md. Plan items NOT DONE blocks (no override). 8.1 verification failures block.
9. Pre-landing review + specialist dispatch → sections/review-army.md. ASK items need user judgment.
10. Greptile comments (if PR exists) → sections/greptile.md. Every reply carries evidence (never vague).
11. Adversarial review + learnings → sections/adversarial.md.
12. Version bump auto-decide: MICRO/PATCH auto-picked; MINOR/MAJOR → ask. 4-digit `MAJOR.MINOR.PATCH.MICRO`; write BOTH VERSION and package.json.
13. CHANGELOG entry (date `YYYY-MM-DD`) → sections/changelog.md.
14. TODOS.md auto-update; ask only to create/reorganize.
15. Commit: 15.0 squash `WIP:` checkpoint commits (NEVER blind `git reset --soft` when non-WIP commits exist). 15.1 bisectable commits — infra first; each = one logical change, independently buildable; <50 lines/<4 files → one commit OK.
16. Verification gate — IRON LAW: no completion claim without fresh evidence. If code changed after Step 5, re-run tests + build; paste fresh output. Tests fail here → STOP, do not push, return to Step 5.
17. Push: credential pre-push guard; idempotency check (`ALREADY_PUSHED` → skip); `git push -u origin <branch>`. Not done after push.
18-19. Sync docs + create/update PR → sections/pr-body.md. PR title MUST start with `v<NEW_VERSION>` (format `v<NEW_VERSION> <type>: <summary>`).
20-21. Persist ship metrics; one-time plan-tune nudge.

## Gates & rules
- Only stop for: on base branch (abort); unresolvable merge conflict; in-branch test failures; pre-landing review ASK items; MINOR/MAJOR bump; Greptile comments needing a decision; coverage below threshold (hard gate, override); plan items NOT DONE; plan verification failures; TODOS create/reorganize.
- Never stop for: uncommitted changes; PATCH/MICRO bump; CHANGELOG content; commit-message approval; multi-file split; auto-fixable findings; coverage within threshold.
- Never skip tests — if tests fail, stop. Never skip the pre-landing review. Never force push (regular `git push` only). Never push without fresh verification evidence.
