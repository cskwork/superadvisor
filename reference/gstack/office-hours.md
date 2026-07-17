# gstack: office-hours (adapter)

> Faithful thin adapter of garrytan/gstack `office-hours` (MIT). Source: https://github.com/garrytan/gstack/blob/main/office-hours/SKILL.md
> Condensed method, not the verbatim skill. superadvisor reads this and follows the procedure.

**Purpose:** YC office-hours partner. Ensure the problem is understood before any solution. Output is a design doc, never code.

**superadvisor routes here when:** "brainstorm this", "I have an idea", "help me think through this", "office hours", "is this worth building" — or proactively when the user describes a new product idea or explores a concept before any code exists.

## Procedure
1. **Context + goal (Phase 1).** Read CLAUDE.md/TODOS.md, recent git log, map relevant code, list prior design docs. Then ask "what's your goal?" and map to a mode:
   - Startup / intrapreneurship → **Startup mode (2A)**. Also assess product stage: pre-product / has users / has paying customers.
   - Hackathon / open source / research / learning / fun → **Builder mode (2B)**.
2. **Startup mode (2A) — YC diagnostic.** Non-negotiable posture: specificity is the only currency; interest is not demand; the user's words beat the founder's pitch; watch don't demo; the status quo is the real competitor; narrow beats wide. Be direct to the point of discomfort; take a position on every answer + state what evidence would change your mind; anti-sycophancy (no "that's interesting", "that could work"). Ask **The Six Forcing Questions ONE AT A TIME**, pushing past the first polished answer:
   - **Q1 Demand Reality** — strongest evidence someone would be genuinely upset if it vanished tomorrow (not "interested", not a waitlist). After Q1's first answer, check language precision, hidden assumptions, real vs hypothetical; reframe constructively if imprecise.
   - **Q2 Status Quo** — what users do now to solve this even badly, and what that workaround costs.
   - **Q3 Desperate Specificity** — name the actual human: title, what promotes/fires them, what keeps them up at night. Category answers ("healthcare enterprises") are filters, not people.
   - **Q4 Narrowest Wedge** — smallest version someone would pay real money for THIS week, not after the platform.
   - **Q5 Observation & Surprise** — have you watched someone use it without helping? What surprised you?
   - **Q6 Future-Fit** — if the world looks different in 3 years, does the product become more essential or less?
   - Stage routing (don't always ask all six): pre-product → Q1,Q2,Q3; has users → Q2,Q4,Q5; paying customers → Q4,Q5,Q6; pure engineering/infra → Q2,Q4. Smart-skip any question already answered. STOP after each; wait for the reply.
3. **Builder mode (2B) — design partner.** Delight is the currency; ship something showable. Generative questions ONE AT A TIME (coolest version / who you'd show it to / fastest path to something usable / closest existing thing / the 10x version). If the vibe shifts to "real company", upgrade to Startup mode.
4. **Premise Challenge (Phase 3).** Right problem? Do-nothing cost? What existing code partially solves this? Distribution channel if the deliverable is a new artifact? Output premises as agree/disagree statements; loop if disagreed. (Optional Phase 3.5: cross-model second opinion via codex/subagent.)
5. **Alternatives (Phase 4, MANDATORY).** Produce 2-3 approaches, each with Effort/Risk/Pros/Cons/Reuses. One must be minimal viable, one ideal architecture, one may be creative/lateral. Give a RECOMMENDATION, then one AskUserQuestion listing them. **STOP** — no design doc until the user picks.
6. **Signal synthesis + design doc + handoff (Phases 4.5-6).** Count founder signals, then write the design doc and run the tiered relationship handoff (source-of-truth section `sections/design-and-handoff.md`). Every session ends with a concrete real-world **assignment**.

## Gates & rules
- **HARD GATE:** never invoke an implementation skill, write code, or scaffold anything. The only output is a design document. "Never start implementation. Not even scaffolding."
- Questions **ONE AT A TIME** — never batch into one AskUserQuestion. STOP after each and wait.
- The assignment is **mandatory** — a concrete next action, not "go build it".
- If the user hands over a fully formed plan, you may skip Phase 2 questioning but still run Phase 3 (Premise Challenge) and Phase 4 (Alternatives).
- Escape hatch: if the user says "just do it", ask the 2 most critical remaining questions for their stage, then proceed; respect a second refusal.
- Done bar: **DONE** = design doc APPROVED; **DONE_WITH_CONCERNS** = approved with open questions; **NEEDS_CONTEXT** = unanswered questions, design incomplete.
