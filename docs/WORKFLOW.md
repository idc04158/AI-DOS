# Workflow

AI-DOS의 기본 개발 흐름이다. 모든 프로젝트는 이 순서를 따른다.

---

## 전체 흐름

```
CEO
 ↓
ChatGPT (CTO)
 ↓
Cursor (Senior Engineer)
 ↓
GitHub
 ↓
Review
 ↓
Release
```

---

## 1. CEO — 방향 결정

**역할:** 무엇을 만들지, 무엇을 하지 않을지 결정한다.

**입력:** 시장, 사용자 피드백, 비즈니스 목표

**산출물:**

- `VISION.md`의 핵심 방향 확인 또는 수정 요청
- `PROJECT_STATE.md`의 Current Goal, Current Sprint 업데이트
- 우선순위 결정 (MVP 범위 내에서)

**하지 않는 것:**

- 기술 스택 선택
- 구현 세부사항 지시
- 코드 리뷰

**체크리스트:**

- [ ] 이번 스프린트 목표가 MVP와 일치하는가?
- [ ] 하지 않을 것(Non-goals)이 명확한가?
- [ ] PM이 읽고 이해할 수 있는 수준인가?

---

## 2. ChatGPT (CTO) — 기술 리더십

**역할:** CEO의 방향을 기술적으로 구체화하고, 품질을 검증한다.

**도구:** `prompts/` 디렉터리의 프롬프트

**주요 작업:**

| 단계 | 프롬프트 | 목적 |
|------|----------|------|
| 기능 시작 | `FEATURE_REQUEST.md` | 목표·요구사항·완료 조건 정의 |
| 구현 완료 | `REVIEW.md` | 코드·UX·보안 등 종합 리뷰 |
| 버그 발견 | `QA.md` | 심각도 분류 및 해결 방향 |
| 출시 전 | `RELEASE.md` | 출시 가능 여부 판단 |

**산출물:**

- Cursor에게 전달할 명확한 Task 목록
- 리뷰 결과 (통과 / 수정 필요 / 보류)
- 출시 판단 (Release / Hold)

**원칙:**

- Cursor에게 모호한 지시를 내리지 않는다
- 리뷰 마지막에는 **실행 가능한 Task만** 출력한다
- MVP 범위를 벗어난 기능은 Hold한다

---

## 3. Cursor (Senior Engineer) — 구현

**역할:** CTO의 Task를 코드와 문서로 구현한다.

**규칙:** `.cursor/rules/ai-dos.mdc` 자동 적용

**작업 시작 전 (필수):**

1. `README.md` 읽기 — 프로젝트 구조와 실행 방법 파악
2. `VISION.md` 읽기 — 제품 방향과 Non-goals 확인
3. `PROJECT_STATE.md` 읽기 — 현재 목표와 진행 중 작업 확인

**작업 중:**

- MVP 범위 내에서만 구현
- 불필요한 라이브러리 추가 금지
- 복잡한 설계보다 단순하고 유지보수 가능한 코드

**작업 완료 후 (필수):**

1. `PROJECT_STATE.md` 업데이트 — Completed, In Progress, Next Tasks
2. `CHANGELOG.md` 업데이트 — Added / Changed / Fixed / Removed
3. 새 기능이면 `README.md` 업데이트
4. 변경 내용 요약 출력

---

## 4. GitHub — 버전 관리

**역할:** 코드와 문서의 변경 이력을 관리한다.

**브랜치 전략 (권장):**

```
main          ← 안정 버전 (Release)
develop       ← 개발 통합 (선택)
feature/*     ← 기능 개발
fix/*         ← 버그 수정
```

**커밋 규칙:**

```
<type>: <description>

type: feat | fix | docs | refactor | chore
```

**PR 규칙:**

- PR 설명에 관련 `PROJECT_STATE.md` 항목 참조
- 문서 변경이 코드 변경과 함께 포함되어야 함
- CTO 리뷰 통과 후 merge

**태그:**

- Release 시 `v1.0.0` 형식으로 태그
- `CHANGELOG.md`의 해당 버전과 일치

---

## 5. Review — 품질 검증

**담당:** ChatGPT (CTO)

**프롬프트:** `prompts/REVIEW.md`

**평가 항목:**

- 기능 완성도
- UX / UI
- Business Logic
- Security
- Performance
- Error Handling
- AI Prompt Quality
- Maintainability
- SaaS Quality
- Release 가능 여부

**결과:**

| 결과 | 의미 | 다음 단계 |
|------|------|-----------|
| Pass | 출시 가능 수준 | Release 단계로 |
| Revise | 수정 필요 | Cursor Task 반영 후 재리뷰 |
| Hold | 범위 초과 또는 불안정 | CEO와 범위 재조정 |

---

## 6. Release — 출시

**담당:** ChatGPT (CTO) 판단 + CEO 최종 승인

**프롬프트:** `prompts/RELEASE.md`

**출시 전 필수 조건:**

- [ ] REVIEW Pass
- [ ] Known Issues에 Critical/High 없음
- [ ] CHANGELOG 업데이트 완료
- [ ] README 실행 방법 검증 완료
- [ ] PROJECT_STATE의 MVP Progress 반영

**출시 후:**

1. Git 태그 생성 (`v1.0.0`)
2. `CHANGELOG.md`에 Release 날짜 기록
3. `PROJECT_STATE.md`의 Completed에 반영
4. Next Sprint 목표 설정 (CEO)

---

## 역할별 요약

| 역할 | 도구 | 핵심 책임 |
|------|------|-----------|
| CEO | — | 방향, 우선순위, 최종 출시 승인 |
| CTO | ChatGPT | 설계, 리뷰, Task 정의, 출시 판단 |
| Engineer | Cursor | 구현, 문서 동기화 |
| GitHub | — | 저장, 이력, 태그 |

---

## 예외 처리

### 긴급 버그 (Hotfix)

```
버그 발견 → QA.md로 분류 → Critical이면 즉시 fix/* 브랜치
→ 수정 → REVIEW (간소) → main 직접 merge → 태그
```

### 범위 변경

CEO가 Current Goal을 변경하면 CTO가 `FEATURE_REQUEST.md`를 다시 작성하고, Engineer는 `PROJECT_STATE.md`를 업데이트한다.

### 문서만 변경

코드 변경 없이 문서만 수정할 때도 `CHANGELOG.md`의 Changed 항목에 기록한다.
