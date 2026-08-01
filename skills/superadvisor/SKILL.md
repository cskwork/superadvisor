---
name: superadvisor
description: Router to the best super* skill, persona, or gstack workflow. Use when a task spans domains or no skill owns it.
---

# superadvisor — router

Read the request, pick the **single best target**, load only that, hand off. This file is the index; the procedure lives in the target. Load nothing you are not routing to.

Targets:
- **super\*** skill — a full gated workflow (**Skill tool**).
- **persona** — one specialist, one job (**Agent tool**; `agents/<name>.md`).
- **gstack adapter** — a workflow the super\* suite lacks (read `reference/gstack/<name>.md`).

## Route

1. Match the request to one target in the tables below.
2. Precedence when signals overlap:
   - whole multi-step objective (build / fix end-to-end / spec / prototype / verify) → **super\*** (it owns the gates)
   - one atomic specialist step, or a block of a larger run → **persona**
   - deploy · product-reframe · destructive-command safety · docs/retro → **gstack adapter**
3. When in doubt, invoke the specific target — a wrong route is cheaper than skipping a real workflow's gates. If nothing fits, answer directly.
4. Announce the choice as `route: <target>`, then hand off. Route one primary target per turn; name any follow-up instead of preloading it.

## A. super* skills — invoke via Skill tool

| Request signal | Skill |
|---|---|
| General objective: build / fix / spec / prototype / code-review / architecture-improve / verify | `supergoal` |
| UI · screens · components · design systems · make it beautiful | `superdesign` |
| Web QA · browser testing · regression · side-effect checks | `superqa` |
| PRD · strategy · OKR · roadmap · research · GTM · pricing · positioning | `superpm` |
| marketing strategy · copy · ads · SEO · campaigns · lifecycle | `supermarketer` |
| business docs: reports · proposals · PPT · Excel · HWP (hwpx) · PDF conversion | `superoffice` |
| interview prep · mock interviews · answer grading · system-design/behavioral/coding | `superinterview` |
| make learning stick · memory · five-sense learning · gamified | `superlearning` |
| tutoring · concept explanation · comprehension checks · beginner→mastery | `supertutor` |
| security audit · pentest · vuln triage · red team · CTF · IR/forensics | `superhacker` |

Invoke with `Skill(skill="<name>")`. Each stays directly invocable (`/superqa`) too.

## B. Personas — spawn via Agent tool

Definitions in `agents/<name>.md`; spawn mechanics in `reference/personas.md` (registered names use `subagent_type`, the rest pass the file body as the prompt).

| Request signal | Persona |
|---|---|
| requirements analysis · pre-planning consult | `analyst` |
| architecture decisions · design/debugging advisory (read-only) | `architect` |
| code review (against plan/standards) | `code-reviewer` |
| work planning · interview-style planning | `planner` |
| multi-perspective critique · plan/code critic | `critic` |
| debugging · root cause · stack traces · regression isolation | `debugger` |
| causal tracing (competing hypotheses · evidence ledger) | `tracer` |
| UI/UX implementation | `designer` |
| implementation (focused executor) | `executor` |
| code/file exploration | `explore` |
| git atomic commits · rebase · history | `git-master` |
| CLI testing (tmux sessions) | `qa-tester` |
| data analysis · research execution | `scientist` |
| security vuln detection (OWASP · secrets) | `security-reviewer` |
| test strategy · integration/e2e · flaky hardening | `test-engineer` |
| verification strategy · evidence-based completion checks | `verifier` |
| docs writing (README · API docs) | `writer` |
| external docs · reference research | `document-specialist` |
| code simplification · cleanup | `code-simplifier` |
| Stitch design · design-to-code | `stitch-kit` |

## C. gstack adapters — read reference/gstack/<name>.md

Faithful thin adapters distilled from garrytan/gstack (MIT). Read the file, then follow its procedure. Included because the super\* suite doesn't cover these.

| Request signal | Adapter |
|---|---|
| test then push · open PR · sync main | `reference/gstack/ship.md` |
| merge PR → wait CI → verify prod health | `reference/gstack/land-and-deploy.md` |
| post-deploy console-error · regression monitoring loop | `reference/gstack/canary.md` |
| new idea · six forcing questions · product reframe | `reference/gstack/office-hours.md` |
| reframe the problem · find the 10-star product | `reference/gstack/plan-ceo-review.md` |
| warn before destructive commands (rm -rf · DROP · force-push) | `reference/gstack/careful.md` |
| restrict edits to one directory | `reference/gstack/freeze.md` |
| careful + freeze combined | `reference/gstack/guard.md` |
| sync docs to what shipped | `reference/gstack/document-release.md` |
| generate missing docs (Diataxis) | `reference/gstack/document-generate.md` |
| weekly retro | `reference/gstack/retro.md` |

## Reference map (load only what the route needs)

| Read this | When |
|---|---|
| `reference/personas.md` | routing to a persona — spawn mechanics (registered vs file-based), model/tool notes |
| `reference/super-skills.md` | routing to a super\* skill — domain boundaries, overlap tie-breaks |
| `reference/gstack/<name>.md` | routing to a gstack workflow — that adapter only |
