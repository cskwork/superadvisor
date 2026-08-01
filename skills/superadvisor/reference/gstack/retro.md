# gstack: retro (adapter)

> Faithful thin adapter of garrytan/gstack `retro` (MIT). Source: https://github.com/garrytan/gstack/blob/main/retro/SKILL.md
> Condensed method, not the verbatim skill. superadvisor reads this and follows the procedure.

**Purpose:** Team-aware weekly engineering retrospective from git history — metrics, work patterns, and code-quality signals, with per-person praise + growth, persistent history, and trend tracking.

**superadvisor routes here when:** "/retro", "weekly retro", "what did we ship", "engineering retrospective", end of a work week or sprint. Cross-project variant: "/retro global".

**Arguments:** `/retro` (7d default), `/retro 24h`, `/retro 14d`, `/retro 30d`, `/retro compare [Nd]`, `/retro global [Nd]`. Windows are midnight-aligned; report in the user's local timezone (never set `TZ`). First arg `global` -> skip repo Steps 1-14, run the Global flow (needs no git repo).

## Procedure (repo-scoped, Steps 1-14)
- Prior learnings: search gstack learnings (prompt for cross-project on first run). Optional non-git context from `~/.gstack/retro-context.md`.
- Step 0.5 Guard: stale-base / bad-today-anchor pre-flight. Fetch `origin/<default>`; BLOCK if the latest commit predates the window (today is stale, or the worktree is behind remote) rather than fabricating a narrative from zero commits. Skip paths (no remote / detached HEAD / fetch failed) proceed with a disclosure line.
- Step 1 Gather: fetch origin; identify the runner (`git config user.name` = "you"; all others = teammates). Run the git queries against `origin/<default>` in parallel (commits+shortstat, numstat by author, timestamps, file hotspots, PR/MR numbers, per-author hotspots, shortlog, greptile-history, TODOS.md, test-file counts, regression-test commits, skill-usage.jsonl).
- Step 2 Metrics: summary table (features shipped, commits, weighted commits, contributors, PRs, logical SLOC, raw LOC, test-LOC ratio, version range, active days, sessions, LOC/session-hour, test health) + per-author leaderboard with the current user first as "You (name)". Optional rows: greptile signal, backlog health, skill usage, eureka moments.
- Step 3 Commit-time distribution: hourly histogram; call out peak/dead zones, bimodal vs continuous, late-night clusters.
- Step 4 Session detection: 45-minute gap threshold; classify deep (50+ min) / medium (20-50) / micro (<20); report active time, avg length, LOC/hour.
- Step 5 Commit-type breakdown: feat/fix/refactor/test/chore/docs percentages; flag fix ratio >50%.
- Step 6 Hotspot analysis: top 10 changed files; flag churn (5+ changes), test vs prod, VERSION/CHANGELOG frequency.
- Step 7 PR size distribution: Small <100 / Medium 100-500 / Large 500-1500 / XL 1500+ LOC.
- Step 8 Focus score (% of commits in the single most-changed top-level dir) + Ship of the Week (highest-LOC PR).
- Step 9 Team member analysis: per contributor — commits/LOC, focus areas, type mix, session patterns, test discipline, biggest ship. Current user gets the deepest, first-person treatment. Each teammate: 2-3 sentences + Praise (1-2 specific, anchored in commits) + one growth opportunity (framed as leveling up). Solo repo -> personal only. Co-Authored-By: credit human co-authors; track AI co-authors separately as "AI-assisted commits".
- Capture learnings: log genuine, reusable discoveries only.
- Step 10 Week-over-week trends (only if window >= 14d).
- Step 11 Streak tracking: team + personal consecutive commit-day streaks (full history).
- Step 12 Load history & compare: read newest `.context/retros/*.json` -> "Trends vs Last Retro" deltas (skip on first run).
- Step 13 Save snapshot: `.context/retros/${today}-${next}.json` (metrics/authors/version_range/streak/tweetable; include greptile/backlog/test_health only when data exists).
- Step 14 Narrative: tweetable summary as the first line, then Summary Table / Trends / Time & Session Patterns / Shipping Velocity / Code Quality / Test Health / Plan Completion / Focus & Highlights / Your Week / Team Breakdown / Top 3 Team Wins / 3 Things to Improve / 3 Habits for Next Week.

## Global mode (`/retro global [Nd]`)
Cross-project retro across all AI coding tools; runs from any directory, no git repo required.
- G1 compute window. G2 run `gstack-global-discover` (0 sessions -> suggest a longer window and stop). G3 git-log each discovered repo (local-only vs remote). G4 global shipping streak = union of commit dates across all repos. G5 context-switching metric (distinct repos/day). G6 per-tool productivity patterns. G7 aggregate -> shareable personal card first (screenshot-friendly box, current user only, full repo names), then the full team/project breakdown. G8 load & compare `~/.gstack/retros/global-*.json` (same window value only). G9 save `~/.gstack/retros/global-${today}-${next}.json`.

## Compare mode (`/retro compare [Nd]`)
Current window vs the immediately prior same-length window (midnight-aligned, non-overlapping `--since`/`--until`); side-by-side deltas; save only the current-window snapshot.

## Gates & rules
- Query `origin/<default>`, never stale local main. Local timezone; never override `TZ`.
- The Step 0.5 guard BLOCKS on a stale window instead of confidently misreporting.
- Zero commits in window -> say so and suggest a different window.
- Output goes to the conversation only; the ONLY file written is the `.context/retros/` snapshot (`~/.gstack/retros/` in global mode).
- Self-contained: do not read CLAUDE.md. Round LOC/hour to nearest 50; treat merge commits as PR boundaries.
- Tone: candid, not coddling; anchor in real commits; praise like a 1:1, growth like investment advice; never compare teammates negatively; ~3000-4500 words.
