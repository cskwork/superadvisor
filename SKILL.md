---
name: superadvisor
description: Single front-door router for Danny's toolkit. Dispatches one request to the best super* skill, specialist persona, or adapted gstack workflow, then loads only that. Use when you want one entry point or aren't sure which tool fits — build/fix/spec/prototype/review/QA/debug a feature; UI & design systems; PRD/strategy/roadmap/pricing; marketing copy/campaigns; business docs (보고서·PPT·엑셀·한글·PDF); interview prep; teaching/learning; security audit/pentest; deploy/ship/canary; product reframing; destructive-command safety; docs & retro. Invoke as "/superadvisor <request>" or just describe the task.
---

# About

`superadvisor` is a **router**, not a worker. Its one job: read the request, pick the **single best target**, load only that, and hand off. `SKILL.md` holds routing rules only; the procedures live in the targets themselves. Load nothing you are not routing to.

Three target classes:
- **super\*** skills — full gated workflows (registered; invoke via the **Skill tool**).
- **Personas** — one specialist, one job (spawn via the **Agent tool**; definitions in `agents/*.md`).
- **gstack adapters** — specific workflows the super\* suite lacks (read `reference/gstack/<name>.md`).

## Dispatch (do this every time)

1. **Defer to explicit invocation.** If the user already invoked a specific `/super*` skill, or a specific skill's own trigger clearly fires, let that skill run — do **not** double-route. `superadvisor` is for a single entry point or an ambiguous / cross-domain request.
2. **Match the request to one target** using the tables below.
3. **Precedence when signals overlap:**
   - Substantial multi-step objective (build / fix end-to-end / spec / prototype / verify a whole thing) → **super\*** workflow (they carry the gates).
   - One atomic specialist task, or a building block for a larger run → **persona**.
   - Deploy · product-reframing · destructive-command safety · docs/retro (not covered by super\*) → **gstack adapter**.
4. **Bias: when in doubt, invoke the specific target.** A wrong route is cheaper to correct than answering inline and skipping a real workflow's checklist.
5. State the route in one line before handing off, e.g. `route: superqa (web QA)` or `route: persona/debugger`.

## A. super* skills — invoke via Skill tool

| Request signal | Skill |
|---|---|
| General objective: build / fix / spec / prototype / code-review / architecture-improve / verify | `supergoal` |
| UI · 화면 · 컴포넌트 · 디자인 시스템 · make it beautiful | `superdesign` |
| Web QA · 브라우저 테스트 · 회귀 · side-effect 점검 | `superqa` |
| PRD · 전략 · OKR · 로드맵 · 리서치 · GTM · 가격 · 포지셔닝 | `superpm` |
| 마케팅 전략 · 카피 · 광고 · SEO · 캠페인 · 라이프사이클 | `supermarketer` |
| 회사 문서: 보고서 · 제안서 · PPT · 엑셀 · 한글(hwpx) · PDF 변환 | `superoffice` |
| 면접 준비 · 모의면접 · 답변 채점 · 시스템디자인/행동/코딩 면접 | `superinterview` |
| 학습 정착 · 기억 · 5감 학습 · 게임화 | `superlearning` |
| 튜터링 · 개념 설명 · 이해도 점검 · 초심자→숙달 | `supertutor` |
| 보안 감사 · 펜테스트 · 취약점 트리아지 · 레드팀 · CTF · IR/포렌식 | `superhacker` |

Invocation: `Skill(skill="<name>")`. These stay individually invocable (`/superqa` etc.) — route here only when the user came through `superadvisor`.

## B. Personas — spawn via Agent tool

Definitions in `agents/<name>.md`. Spawning depends on registration (see `reference/personas.md`):
- **Registered** (`architect`, `code-reviewer`, `planner`, `security-reviewer`): `Agent(subagent_type="<name>")`.
- **Router-internal** (all others): spawn `Agent(subagent_type="general-purpose")` with the file's contents as the prompt prefix, then the task.

| Request signal | Persona |
|---|---|
| 요구사항 분석 · 사전 기획 상담 | `analyst` |
| 아키텍처 결정 · 설계/디버깅 자문 (read-only) | `architect` |
| 코드 리뷰 (계획/표준 대비) | `code-reviewer` |
| 작업 계획 · 인터뷰형 기획 | `planner` |
| 다관점 비평 · plan/code 크리틱 | `critic` |
| 디버깅 · 근본원인 · 스택트레이스 · 회귀 격리 | `debugger` |
| 인과 추적 (경쟁 가설 · 증거 원장) | `tracer` |
| UI/UX 구현 | `designer` |
| 구현 실행 (focused executor) | `executor` |
| 코드/파일 탐색 | `explore` |
| git 원자적 커밋 · rebase · history | `git-master` |
| CLI 테스트 (tmux 세션) | `qa-tester` |
| 데이터 분석 · 리서치 실행 | `scientist` |
| 보안 취약점 탐지 (OWASP · secrets) | `security-reviewer` |
| 테스트 전략 · 통합/e2e · flaky 강화 | `test-engineer` |
| 검증 전략 · 증거 기반 완료 체크 | `verifier` |
| 문서 작성 (README · API docs) | `writer` |
| 외부 문서 · 레퍼런스 조사 | `document-specialist` |
| 코드 단순화 · 정리 | `code-simplifier` |
| Stitch 디자인 · design-to-code | `stitch-kit` |

## C. gstack adapters — read reference/gstack/<name>.md

Faithful thin adapters distilled from garrytan/gstack (MIT). Read the file, then follow its procedure. Included because the super\* suite doesn't cover these.

| Request signal | Adapter |
|---|---|
| 테스트 후 푸시 · PR 오픈 · sync main | `reference/gstack/ship.md` |
| PR 머지 → CI 대기 → 프로덕션 헬스 확인 | `reference/gstack/land-and-deploy.md` |
| 배포 후 콘솔 에러 · 회귀 모니터링 루프 | `reference/gstack/canary.md` |
| 새 아이디어 · 6가지 강제질문 · 제품 재구성 | `reference/gstack/office-hours.md` |
| 문제 재정의 · 10-star 제품 찾기 | `reference/gstack/plan-ceo-review.md` |
| 파괴적 명령(rm -rf · DROP · force-push) 사전 경고 | `reference/gstack/careful.md` |
| 편집을 한 디렉터리로 제한 | `reference/gstack/freeze.md` |
| careful + freeze 결합 | `reference/gstack/guard.md` |
| 배포된 것에 맞춰 문서 갱신 | `reference/gstack/document-release.md` |
| 누락 문서 생성 (Diataxis) | `reference/gstack/document-generate.md` |
| 주간 회고 · retro | `reference/gstack/retro.md` |

## Reference map (load only what the route needs)

| Read this | When |
|---|---|
| `reference/personas.md` | routing to a persona — spawn mechanics (registered vs file-based), model/tool notes |
| `reference/super-skills.md` | routing to a super\* skill — domain boundaries, overlap tie-breaks |
| `reference/gstack/<name>.md` | routing to a gstack workflow — that adapter only |

If a request splits across classes (e.g. "design **and** ship it"), route the primary objective now and name the follow-up target in your handoff line — do not preload both.
