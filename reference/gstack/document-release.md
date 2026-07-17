# gstack: document-release (adapter)

> Faithful thin adapter of garrytan/gstack `document-release` (MIT). Source: https://github.com/garrytan/gstack/blob/main/document-release/SKILL.md
> Condensed method, not the verbatim skill. superadvisor reads this and follows the procedure.

**Purpose:** Post-ship documentation update — after `/ship`, before the PR merges: make every doc match what actually shipped, and build a Diataxis coverage map that flags gaps (does not fill them).

**superadvisor routes here when:** "update the docs", "sync documentation", "post-ship docs", "make docs match what shipped", proactively after a PR merges or code ships.

## Procedure
Mostly automated: make obvious factual updates directly; stop only for risky/subjective calls.

0. Detect platform (GitHub / GitLab / unknown, from remote URL or `gh`/`glab auth`) and the base branch. Substitute the base branch into every git command.
1. Pre-flight & diff analysis. Abort if on the base branch ("run from a feature branch"). Gather `git diff <base>...HEAD --stat`, `git log <base>..HEAD --oneline`, `git diff <base>...HEAD --name-only`. Discover docs: `find . -maxdepth 2 -name "*.md"` (skip .git/node_modules/.gstack/.context). Classify changes: new features / changed behavior / removed functionality / infrastructure. Summarize.
1.5. Coverage Map (blast-radius; Diataxis used as an AUDIT lens, not a generator). Extract public-surface changes from the diff (new exports, commands, CLI flags, config options, endpoints, skills; renamed/removed surface; new env vars/feature flags). For each item, mark coverage across the four Diataxis quadrants — Reference / How-to / Tutorial / Explanation. Zero coverage = critical gap (fix in Step 3); reference-only = common gap (note in PR body). Architecture-diagram drift: pull entity names from ASCII/Mermaid blocks, cross-reference the diff, flag any renamed/split/removed/moved. Flag gaps only; never auto-generate; suggest `/document-generate`.

Then Read `sections/release-body.md` and run Steps 2-9:

2. Per-file audit vs the diff: README, ARCHITECTURE (conservative — only clear contradictions), CONTRIBUTING (walk it as a brand-new contributor), CLAUDE.md, any other .md. Classify each update as auto-update (factual, from the diff) vs ask-user (narrative / removal / security model / large rewrite >~10 lines / ambiguous).
3. Apply auto-updates. Never auto-update README/ARCHITECTURE philosophy or security-model text; never remove whole sections.
4. AskUserQuestion for the risky/questionable changes.
5. CHANGELOG voice polish (sell-test rubric). Read the whole file; Edit within existing entries via exact `old_string`; never `Write`, never delete/reorder/replace/regenerate entries.
6. Cross-doc consistency & discoverability: README vs CLAUDE.md capability lists; ARCHITECTURE vs CONTRIBUTING structure; CHANGELOG latest vs VERSION; every doc reachable from README or CLAUDE.md.
7. TODOS.md: mark completed items; ask before adding new ones.
8. VERSION bump — always AskUserQuestion (`git diff <base>...HEAD -- VERSION`), even if `/ship` already bumped it (confirm it covers full scope).
9. Commit `docs: update project documentation for vX.Y.Z.W`; push. Redaction-scan, then update the PR/MR body (idempotent, race-safe) with a `## Documentation` section: doc diff preview + documentation debt from the coverage map + diagram drift. Sync the PR title to a `v<VERSION>` prefix. Output a documentation-health + coverage summary.

Finally: Codex Documentation Review runs default-on — an independent cross-model pass checking docs against what shipped. Off only via `gstack-config set codex_reviews disabled`.

## Gates & rules
- Runs after `/ship`, before merge; abort if on the base branch.
- Never clobber CHANGELOG: polish wording only, Edit with exact `old_string`, never `Write` or regenerate.
- Never bump VERSION silently — always ask.
- Read a file in full before editing it.
- Coverage map informs, never generates; suggest `/document-generate` for gaps.
- Diagram drift is advisory — flag in the PR body, do not auto-edit ASCII/Mermaid.
- Discoverability: every doc file must be reachable from README or CLAUDE.md.
- Only stop for: risky doc changes, VERSION bump, new TODOS items, narrative cross-doc contradictions. Never stop for factual corrections, path/count/version fixes, CHANGELOG voice polish, or marking TODOS complete.
