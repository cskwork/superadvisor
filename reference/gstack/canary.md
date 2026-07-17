# gstack: canary (adapter)

> Faithful thin adapter of garrytan/gstack `canary` (MIT). Source: https://github.com/garrytan/gstack/blob/main/canary/SKILL.md
> Condensed method, not the verbatim skill. superadvisor reads this and follows the procedure.

**Purpose:** Post-deploy monitoring loop. Watch the live app for console errors, performance regressions, and page failures via the browse daemon (`$B`); screenshot and compare against pre-deploy baselines; alert on anomalies.

**superadvisor routes here when:** "monitor deploy", "canary", "post-deploy check", "watch production", "verify deploy". Korean: "배포 후 모니터링", "카나리 체크", "프로덕션 감시", "배포 확인". Args: `/canary <url> [--duration 5m] [--baseline] [--pages /,/dashboard] [--quick]`.

## Procedure
Start monitoring within 30s of invocation; default duration 10 min.

1. Setup: `mkdir -p .gstack/canary-reports/{baselines,screenshots}`.
2. Baseline capture (`--baseline`, run BEFORE deploying): per page `$B goto` + `snapshot -i -a -o` + `console --errors` + `perf` + `text`; save `baseline.json` (screenshot, console_errors, load_time_ms per page). STOP: tell user to deploy, then run `/canary <url>`.
3. Page discovery (if no `--pages`): `$B goto`/`links`/`snapshot -i`; take top 5 internal nav links + homepage; AskUserQuestion which pages to monitor.
4. Pre-deploy snapshot (if no `baseline.json` exists): quick reference snapshot per page (`goto`+`snapshot`+`console`+`perf`) — becomes the regression reference.
5. Continuous monitoring loop: every 60s per page `goto`+`snapshot`+`console --errors`+`perf`. Compare vs baseline:
   - page load failure / timeout → CRITICAL
   - new console errors (not in baseline) → HIGH
   - load time > 2x baseline → MEDIUM
   - new 404s → LOW
   On CRITICAL/HIGH → emit CANARY ALERT (time, page, type, finding, evidence screenshot, baseline vs current) and AskUserQuestion: A investigate now / B continue (transient) / C rollback / D dismiss.
6. Health report: summary HEALTHY / DEGRADED / BROKEN + per-page results; save `.gstack/canary-reports/{date}-canary.{md,json}`; write JSONL to the review dashboard.
7. Baseline update: if healthy, AskUserQuestion to update baseline with current screenshots.

## Gates & rules
- Alert on CHANGES vs baseline, not absolutes or industry standards (3 baseline errors staying 3 is fine; one NEW error alerts).
- Don't cry wolf: only alert on patterns that persist across 2+ consecutive checks — a single transient blip is not an alert.
- Every alert includes a screenshot path — evidence, no exceptions.
- Performance is relative: 2x baseline = regression; 1.5x may be normal variance.
- Baseline is king — without one, canary is only a health check; encourage `--baseline` before deploy.
- Read-only: observe and report; do not modify code unless the user explicitly asks to investigate and fix.
