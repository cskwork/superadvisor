# gstack: land-and-deploy (adapter)

> Faithful thin adapter of garrytan/gstack `land-and-deploy` (MIT). Source: https://github.com/garrytan/gstack/blob/main/land-and-deploy/SKILL.md
> Condensed method, not the verbatim skill. superadvisor reads this and follows the procedure.

**Purpose:** Merge the PR, wait for CI + deploy, verify production health via single-pass canary. Picks up after `/ship`. Offer revert at every failure.

**superadvisor routes here when:** "merge", "land", "deploy", "merge and verify", "land it", "ship it to production". Korean: "머지해줘", "랜딩", "배포하고 확인", "프로덕션에 올려줘". Args: `/land-and-deploy [#NNN] [<url>]`.

## Procedure
Mostly automated; stop only at the listed gates. GitHub only — GitLab/unknown → STOP ("run `/ship` to make the MR, merge via web UI").

0. Detect platform + base branch. GitLab/unknown → STOP.
1. Pre-flight: `gh auth status` (not auth'd → STOP: `gh auth login`). Detect/parse PR (`gh pr view --json number,state,...`). No PR → STOP (run `/ship` first). MERGED → nothing to deploy. CLOSED → reopen first. OPEN → continue.
1.5. First-run dry-run validation (FIRST_RUN or CONFIG_CHANGED, keyed off `~/.gstack/projects/$SLUG/land-deploy-confirmed`): detect deploy infra (fly/render/vercel/netlify/heroku/railway + workflows), validate commands, detect staging, preview readiness, show infra table, confirm via AskUserQuestion. A → save fingerprint, continue; B/C → STOP. CONFIRMED → skip to Step 2. Validation failures are WARNINGs, not blockers (except `gh auth`).
2. Pre-merge checks: `gh pr checks`. Any required check FAILING → STOP (won't merge un-passed CI). PENDING → wait Step 3. `gh pr view --json mergeable`; CONFLICTING → STOP.
3. Wait for CI: `gh pr checks --watch --fail-fast`, 15-min timeout. Fail → STOP. Timeout → STOP.
3.4. VERSION drift: if a sibling PR landed ahead (`BRANCH_VERSION < NEXT_SLOT`) → STOP; rerun `/ship` to reconcile. Do NOT auto-bump here.
3.5. Pre-merge readiness gate (merge is irreversible): review staleness (offer inline quick review if STALE 4+ commits / NOT RUN); run free tests (fail → BLOCKER, cannot merge); E2E + LLM evals (warnings); PR-body accuracy; doc-release check. Build readiness report; AskUserQuestion: A merge / B hold (STOP with next steps) / C merge anyway.
4. Merge: `gh pr merge --auto --delete-branch` first, else `gh pr merge --squash --delete-branch`. Permission error → STOP. After ANY non-zero exit: query `gh pr view --json state,mergeCommit,...` — NEVER call `gh pr merge` twice (server state is authoritative). Merge queue → poll state every 30s up to 30 min.
5. Deploy strategy detection: read persisted CLAUDE.md config / auto-detect platform; `gstack-diff-scope` classify. Docs-only → skip verify. 5a staging-first option if staging detected.
6. Wait for deploy: GH Actions poll (`gh run view`) / Fly-Render-Heroku CLI / Vercel-Netlify wait 60s / custom hook. Poll 30s, progress every 2 min, 20-min timeout. Deploy failure → AskUserQuestion (logs / revert / continue anyway).
7. Canary verification (single-pass, depth by diff scope): `$B goto` (200 + real content), `console --errors` (no critical), `perf` (<10s), `snapshot` (evidence). All pass → HEALTHY. Any fail → show evidence, AskUserQuestion (expected / revert / investigate).
8. Revert (if chosen): `git revert <merge-sha> --no-edit && git push origin <base>`; branch protection → open a revert PR instead.
9-10. Deploy report (`.gstack/deploy-reports/`, JSONL to dashboard); suggest follow-ups (`/canary`, `/benchmark`, `/document-release`).

## Gates & rules
- Always stop for: first-run dry-run; pre-merge readiness gate; gh not authenticated; no PR found; CI failures / merge conflicts; permission denied on merge; deploy failure (offer revert); prod health issues from canary (offer revert).
- Never force push (`gh pr merge` is safe). Never skip CI — failing checks stop the flow. Never call `gh pr merge` a second time after a non-zero exit. Revert is always an escape hatch at every failure point. Single-pass verification only — use `/canary` for continuous monitoring. Narrate every step; auto-detect everything, ask only when it can't be inferred.
