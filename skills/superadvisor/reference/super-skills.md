# super* skills — domain boundaries & tie-breaks

All ten remain registered and individually invocable. Route here via `Skill(skill="<name>")` when the user came through `superadvisor` or the domain is ambiguous. Each carries its own gated workflow — prefer it over hand-wiring personas for a full objective.

| Skill | Owns | Source |
|---|---|---|
| `supergoal` | End-to-end coding objective: build/fix/spec/prototype/review/verify with red-green gates | github.com/cskwork/supergoal-skill |
| `superdesign` | UI generation, design systems, "make it beautiful", anti-slop gate | cskwork |
| `superqa` | Web/browser QA, regression, side-effect detection (console/network/dialogs) | github.com/cskwork/superqa-skill |
| `superpm` | PM artifacts: PRD, strategy, OKR, roadmap, pricing, positioning, GTM | cskwork |
| `supermarketer` | Marketing strategy & execution: copy, ads, SEO, campaigns, lifecycle | cskwork |
| `superoffice` | Business office docs: docx/pptx/xlsx/hwpx/pdf, Korean templates | cskwork |
| `superinterview` | Interview prep: mock interviews, answer grading, prep plans | cskwork |
| `superlearning` | Make learning stick: multi-sense, memory palace, gamified | cskwork |
| `supertutor` | Teach to mastery: Feynman/Socratic, explain-back grading | cskwork |
| `superhacker` | Scoped security engagements: pentest, triage, red team, IR, CTF | github.com/cskwork/superhacker-skill |

## Overlap tie-breaks (pick the right side)

- **Whole objective vs one role** → `supergoal` runs the full loop and spawns its own builder/verifier; a **persona** (`code-reviewer`, `debugger`, `executor`) does exactly one step. "Fix this bug and prove it" → `supergoal`; "just review this diff" → `code-reviewer`.
- **Web QA vs CLI test vs bug-fix** → `superqa` = browser/web; `qa-tester` persona = CLI/tmux; `supergoal` DEBUG = actually change code to fix.
- **Design skill vs designer persona** → `superdesign` = generate screens / design system / taste; `designer` persona = implement a UI in existing code.
- **Security engagement vs code scan** → `superhacker` = scoped offensive/IR/CTF engagement; `security-reviewer` persona = OWASP/secrets scan on a diff before commit.
- **PM artifact vs impl plan vs product reframe** → `superpm` = PRD/strategy docs; `planner` persona = implementation plan for a coding task; gstack `office-hours`/`plan-ceo-review` = reframe the product itself.
- **Business doc vs code doc vs repo doc** → `superoffice` = reports/PPT/Excel/HWP/PDF; `writer` persona = README/API docs in code; gstack `document-release`/`document-generate` = sync repo docs to shipped code (Diataxis).

## Safety

super\* workflows own their own gates (verification, destructive-step consent). Do not strip those when routing. If a request is destructive or irreversible, the target skill's consent gate applies; layer gstack `careful`/`guard` when the user is running risky shell commands outside a skill.
