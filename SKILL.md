---
name: superadvisor
description: Router that dispatches a request to the single best specialist — a super* skill, a persona, or a gstack workflow — and loads only that. Reach for it as the default entry point when a task is substantial, spans domains, or no single skill clearly owns it.
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
| UI · 화면 · 컴포넌트 · 디자인 시스템 · make it beautiful | `superdesign` |
| Web QA · 브라우저 테스트 · 회귀 · side-effect 점검 | `superqa` |
| PRD · 전략 · OKR · 로드맵 · 리서치 · GTM · 가격 · 포지셔닝 | `superpm` |
| 마케팅 전략 · 카피 · 광고 · SEO · 캠페인 · 라이프사이클 | `supermarketer` |
| 회사 문서: 보고서 · 제안서 · PPT · 엑셀 · 한글(hwpx) · PDF 변환 | `superoffice` |
| 면접 준비 · 모의면접 · 답변 채점 · 시스템디자인/행동/코딩 면접 | `superinterview` |
| 학습 정착 · 기억 · 5감 학습 · 게임화 | `superlearning` |
| 튜터링 · 개념 설명 · 이해도 점검 · 초심자→숙달 | `supertutor` |
| 보안 감사 · 펜테스트 · 취약점 트리아지 · 레드팀 · CTF · IR/포렌식 | `superhacker` |

Invoke with `Skill(skill="<name>")`. Each stays directly invocable (`/superqa`) too.

## B. Personas — spawn via Agent tool

Definitions in `agents/<name>.md`; spawn mechanics in `reference/personas.md` (registered names use `subagent_type`, the rest pass the file body as the prompt).

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
