# gstack: plan-ceo-review (adapter)

> Faithful thin adapter of garrytan/gstack `plan-ceo-review` (MIT). Source: https://github.com/garrytan/gstack/blob/main/plan-ceo-review/SKILL.md
> Condensed method, not the verbatim skill. superadvisor reads this and follows the procedure.

**Purpose:** CEO/founder-mode plan review. Rethink the problem, find the 10-star product, challenge premises, expand scope only when it makes a better product. Review only — no code changes.

**superadvisor routes here when:** "think bigger", "expand scope", "strategy review", "rethink this plan", "is this ambitious enough", "크게 생각해줘", "스코프 넓혀줘", "전략 리뷰" — or proactively when the user questions a plan's scope or ambition.

## Procedure
1. **Pre-review system audit (before Step 0).** git log/diff/stash, TODO/FIXME scan, recently-touched files; read CLAUDE.md, TODOS.md, architecture docs. Check for an `/office-hours` design doc and a CEO handoff note; if no design doc, offer to run `/office-hours` first. Detect frontend/UI scope, run retrospective + landscape checks.
2. **Step 0A Premise Challenge.** Right problem? Actual outcome vs a proxy problem? What if we do nothing — real pain or hypothetical?
3. **Step 0B Existing Code Leverage.** Map every sub-problem to existing code; is this rebuilding something that already exists?
4. **Step 0C Dream State Mapping.** `CURRENT STATE → THIS PLAN → 12-MONTH IDEAL`. Does the plan move toward the ideal or away?
5. **Step 0C-bis Implementation Alternatives (MANDATORY).** 2-3 approaches, each Effort/Risk/Pros/Cons/Reuses. One minimal viable, one ideal architecture — **equal weight** (do not default to the smaller). RECOMMENDATION + one AskUserQuestion. **STOP** for explicit approval before mode selection.
6. **Step 0D Mode-specific analysis — the 10-star product reframe** (run in EXPANSION / SELECTIVE EXPANSION): dream the platonic ideal first, then distill concrete opt-in proposals.
   - **10x check** — the version 10x more ambitious delivering 10x value for 2x the effort, described concretely.
   - **Platonic ideal** — if the best engineer had unlimited time and perfect taste, what would this be and what would the user FEEL? Start from experience, not architecture.
   - **Delight opportunities** — list at least 5 adjacent ~30-minute improvements ("oh nice, they thought of that").
   - **Opt-in / cherry-pick ceremony** — present each expansion as its OWN AskUserQuestion: A) Add to scope, B) Defer to TODOS.md, C) Skip. Accepted items become scope for all later sections; rejected go to "NOT in scope". (HOLD SCOPE: complexity check + minimum change set. SCOPE REDUCTION: ruthless minimum viable + follow-up split.)
7. **Step 0D-POST.** In EXPANSION / SELECTIVE EXPANSION, persist the CEO plan to `~/.gstack/projects/$SLUG/ceo-plans/{date}-{slug}.md`, then run the adversarial Spec Review Loop (fresh-context subagent scores 5 dimensions — completeness, consistency, clarity, scope, feasibility; fix and re-dispatch, max 3 iterations).
8. **Step 0E Temporal Interrogation** (EXPANSION / SELECTIVE / HOLD). Surface implementation decisions to resolve NOW; present both human and CC time scales.
9. **Step 0F Mode Selection.** Present the four modes via AskUserQuestion (kind-note, no completeness score), confirm which 0C-bis approach applies, then commit.
10. **11-section deep review.** Read and execute `sections/review-sections.md` in full (source of truth), then write the review report.

## The four modes
- **SCOPE EXPANSION** — build a cathedral; push scope UP ("10x better for 2x effort?"); recommend enthusiastically, but the user opts in to every expansion.
- **SELECTIVE EXPANSION** — hold current scope as the bulletproof baseline; separately surface every expansion and present each individually; neutral posture; user cherry-picks.
- **HOLD SCOPE** — scope is accepted; make it bulletproof (failure modes, edge cases, observability, error paths); do not silently expand or reduce.
- **SCOPE REDUCTION** — surgeon; find the minimum viable that achieves the core outcome; cut everything else, ruthlessly.
- Context defaults: greenfield → EXPANSION; enhancement/iteration → SELECTIVE EXPANSION; bug/hotfix or refactor → HOLD SCOPE; >15 files → suggest REDUCTION; "go big"/"cathedral" → EXPANSION; "cherry-pick"/"show me options" → SELECTIVE EXPANSION.

## Gates & rules
- **Do NOT make code changes or start implementation.** Review only, in every mode.
- **The user is 100% in control.** Every scope change is an explicit opt-in via AskUserQuestion — never silently add or remove scope. Once a mode is selected, COMMIT; do not drift toward another mode.
- **STOP** after 0C-bis (alternatives) and after 0F (mode) — one AskUserQuestion per issue, never batched; a "clearly winning" option still needs explicit approval before it lands in the plan.
- Prime directives to enforce in review: zero silent failures; every error has a name (catch-all handlers are a smell); trace the happy path plus three shadow paths (nil / empty / upstream error); observability is scope, not afterthought; everything deferred is written to TODOS.md.
- **EXIT PLAN MODE GATE (blocking):** before ExitPlanMode, the plan file's LAST `## ` heading must be `## GSTACK REVIEW REPORT`, carrying a Runs/Status/Findings table + a VERDICT line, and ending with the unresolved-decisions status (`NO UNRESOLVED DECISIONS` or an `**UNRESOLVED DECISIONS:**` block). Body prose does not satisfy it.
