# superadvisor

**단일 진입점 라우터 스킬**. 스킬 하나만 부르면 요청을 읽고 가장 적합한 대상 하나를 골라, **그 대상만 로드**한 뒤 넘긴다. [garrytan/gstack](https://github.com/garrytan/gstack)의 라우터 패턴과 cskwork `supergoal`의 방식("`SKILL.md`가 라우터, `reference/`가 절차, 단계에 필요한 것만 로드")을 따른다.

## 왜

기존엔 20개 페르소나 에이전트와 10개 `super*` 스킬이 각각 개별 등록되어 세션마다 전부 로드됐다. `superadvisor`는 이들에 하나의 지능형 진입점을 주고, 요청에 필요한 대상만 온디맨드로 끌어온다.

## 무엇으로 라우팅하나

| 분류 | 대상 | 방식 |
|---|---|---|
| **super\*** 스킬 | supergoal, superdesign, superqa, superpm, supermarketer, superoffice, superinterview, superlearning, supertutor, superhacker | Skill 툴 (직접 호출도 유지) |
| **페르소나** | `agents/*.md`의 20개 전문가 (analyst, architect, debugger, executor, code-reviewer, security-reviewer …) | Agent 툴 |
| **gstack 어댑터** | ship, land-and-deploy, canary, office-hours, plan-ceo-review, careful, freeze, guard, document-release, document-generate, retro | `reference/gstack/*.md` read |

전체 라우팅 표와 우선순위 규칙은 [`SKILL.md`](./SKILL.md)에 있다.

## 구조

```
superadvisor/
├── SKILL.md                 # 라우터: 디스패치 표 + 우선순위 + reference map
├── agents/*.md              # 20개 페르소나 정의
└── reference/
    ├── personas.md          # 스폰 메커니즘 (등록형 vs 라우터 내부)
    ├── super-skills.md      # 도메인 경계 + 중복 타이브레이크
    └── gstack/*.md          # 원본 충실 thin 어댑터 11개 (MIT, garrytan/gstack)
```

## 등록 모델 (Hybrid)

- **super\*** 스킬은 등록 유지 — `/superqa` 등 직접 호출 계속 가능. 라우터는 Skill 툴로 디스패치.
- **페르소나**: `architect`, `code-reviewer`, `planner`, `security-reviewer`는 `~/.claude/agents/`에 유지(전역 규칙이 이름으로 호출). 나머지 16개는 파일 본문을 서브에이전트 프롬프트로 넘겨 여기서 스폰; 컨텍스트 절감을 실제로 얻으려면 `~/.claude/agents/`에서 등록 해제. `reference/personas.md` 참고.
- **gstack** 스킬은 설치·등록하지 않음 — 얇은 어댑터로 증류해 라우터가 필요 시 read.

## 설치

```bash
ln -s /Users/danny/Documents/PARA/Resource/superadvisor ~/.claude/skills/superadvisor
```

## 사용

```
/superadvisor <요청>          # 또는 그냥 작업을 설명
```

라우터는 넘기기 전에 선택을 `route: <대상>` 한 줄로 출력한다. 이미 특정 `/super*` 스킬을 직접 호출했다면 라우터는 양보한다(이중 라우팅 없음).

## 출처

gstack 어댑터는 [garrytan/gstack](https://github.com/garrytan/gstack)(MIT)에서 증류. `super*` 스킬은 cskwork 소유. 페르소나 세트는 사용자가 설치한 에이전트 팩.
