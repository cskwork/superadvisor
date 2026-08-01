# gstack: document-generate (adapter)

> Faithful thin adapter of garrytan/gstack `document-generate` (MIT). Source: https://github.com/garrytan/gstack/blob/main/document-generate/SKILL.md
> Condensed method, not the verbatim skill. superadvisor reads this and follows the procedure.

**Purpose:** Generate missing documentation from scratch for a feature, module, or whole project using the Diataxis framework (tutorial / how-to / reference / explanation). Standalone, or called by `/document-release` to fill flagged gaps.

**superadvisor routes here when:** "write docs", "generate documentation", "document this feature", "create a tutorial", "write a how-to", "explain this module", "docs for this project".

Philosophy: research the whole, then write the parts — survey the full code surface before writing any doc.

## Procedure
0. Scope & intent. Determine target (specific target / whole project / gap entities from a coverage map). AskUserQuestion for doc target: A) inline in existing files, B) standalone `docs/` files, C) Both (recommended — discoverability + depth). Output format: follow an existing `docs/` convention or framework (Nextra / Docusaurus / MkDocs / VitePress), else plain Markdown in `docs/`.
1. Codebase Archaeology (research phase — the most important step, do not rush). Map structure (`find`). Read entry points (README/ARCHITECTURE/CONTRIBUTING/CLAUDE/AGENTS, package.json/Cargo.toml/pyproject.toml/go.mod, main entry files, config). For each target, read the implementation end-to-end + its tests + related modules + inline `NOTE:`/`DESIGN:`/`WHY:` comments. Build a concept map (Purpose / Key concepts / Public surface / Dependencies / Dependents / Edge cases / Design decisions). Output a research summary.
2. Diataxis Partitioning. Per entity, pick quadrants via the decision matrix (not every entity needs all four). Output the partition plan. If >5 docs to create, AskUserQuestion to confirm; smaller scopes proceed directly.
3. Write Reference FIRST (the foundation — establishes vocabulary): what it is + API/interface + options/configuration + examples + related. Accuracy over elegance; types/defaults/constraints; real copy-pasteable examples; do not explain why.
4. Write Explanation (why it works this way): problem -> approach -> trade-offs -> alternatives considered. Lead with the problem; ASCII diagrams for architecture; name trade-offs explicitly; link, do not repeat reference.
5. Write How-to guides (task-oriented): prerequisites -> steps -> verification -> troubleshooting. Title starts with "How to"; every step actionable; verification mandatory; troubleshooting required if the task can fail.
6. Write Tutorials (learning-oriented): what you'll build -> what you'll need -> numbered steps -> what you built. Time to first result <3 steps; every step a visible change/output; exact commands; error paths shown inline.
7. Cross-document linking & discoverability. Cross-link quadrants (reference<->how-to; tutorials -> both). Update entry points (README / CLAUDE / AGENTS / index / sidebar). Reachable within 2 clicks of README. Grep for broken `](` links.
8. Quality self-review — three gates. Accuracy: examples run, APIs match signatures, no stale refs. Completeness: reference covers 100% of public surface, how-tos cover the top 3 tasks, tutorials reach a result in <=3 steps, explanations name trade-offs. Voice: for a smart reader who hasn't seen the code, gloss jargon on first use, active voice, "You can now..." not "The system provides...". Fix any failure before proceeding.
9. Commit & output. Stage new files by name (never `git add -A`/`.`). Redaction-scan staged docs; block on a HIGH-format credential. Commit `docs: generate [scope] documentation (Diataxis)`; push. If a PR exists, add a `## Documentation Generated` table (File / Quadrant / Description). Output a structured summary (scope, files new/updated, per-quadrant coverage, gate pass/fail).

## Gates & rules
- Research before writing — Step 1 is not optional; thin research yields surface-level docs.
- Accuracy is non-negotiable — every example must work, every API description must match code; if unsure, reread the source, do not guess.
- Diataxis quadrants serve different readers — never mix tutorial content into reference, or reference into how-tos.
- Reference first, before tutorials or how-tos.
- Tutorial time-to-first-result <=3 steps, or restructure.
- Cross-link everything — isolated docs are undiscoverable docs.
- Completeness over minimalism — AI makes comprehensive docs cheap; boil the ocean.
