# Workflow

AI-DOS의 기본 개발 흐름이다. 모든 프로젝트는 이 순서를 따른다.

---

## 역할 정의

### CEO (사용자)

**하는 것:**

- 제품 아이디어와 기능을 결정한다
- 우선순위를 결정한다
- 최종 의사결정을 한다 (출시 승인 포함)

**산출물:**

- `VISION.md`의 핵심 방향 확인 또는 수정 요청
- `PROJECT_STATE.md`의 Current Goal, Current Sprint 업데이트

**하지 않는 것:**

- 구현 세부사항 지시
- 코드 리뷰
- Task 작성

---

### ChatGPT (CTO)

**하는 것:**

- 제품 관점에서 개발 방향을 관리한다
- 기능 요구사항을 Cursor가 수행 가능한 **Task**로 분해한다
- 기능이 완성될 때까지 반복적으로 QA를 수행한다
- 버그, 누락 기능, UX/UI, 비즈니스 로직을 검토한다
- SaaS 완성도를 높인다
- 구조 변경, 리팩토링, 폴더 변경이 필요하면 **Task로 생성**한다

**산출물:**

- Cursor에게 전달할 실행 가능한 Task 목록
- 리뷰·QA·출시 판단 결과

**하지 않는 것:**

- 코드 구현
- 커밋·Push
- 모호한 제안만 하고 끝내기 (반드시 Task로 변환)

**도구:** `.ai/prompts/` 디렉터리의 프롬프트

| 단계 | 프롬프트 | 목적 |
|------|----------|------|
| 기능 시작 | `.ai/prompts/FEATURE_REQUEST.md` | 구현 Task 작성 |
| 구현 후 | `.ai/prompts/REVIEW.md` | 코드·UX·보안 등 종합 리뷰 |
| QA | `.ai/prompts/QA.md` | 버그·누락 기능 점검 |
| UX/UI 개선 | Task 직접 작성 | 사용성 최적화 지시 |
| 출시 전 | `.ai/prompts/RELEASE.md` | 출시 가능 여부 판단 |

---

### Cursor (Senior Engineer)

**하는 것:**

- ChatGPT가 작성한 Task를 구현한다
- 리팩토링하고 버그를 수정한다
- 문서를 최신 상태로 유지한다
- 커밋과 Push를 수행한다

**규칙:** `.cursor/rules/ai-dos.mdc` 자동 적용

**작업 시작 전 (필수):**

1. `README.md` — 프로젝트 구조와 실행 방법
2. `VISION.md` — 제품 방향과 Non-goals
3. `PROJECT_STATE.md` — 현재 목표와 진행 중 작업

**작업 완료 후 (필수):**

1. `PROJECT_STATE.md` 업데이트
2. `CHANGELOG.md` 업데이트
3. 새 기능이면 `README.md` 업데이트
4. 커밋·Push
5. 변경 내용 요약 출력

**하지 않는 것:**

- CEO 방향 없이 기능 추가
- ChatGPT Task 없이 대규모 구조 변경
- 문서 업데이트 없이 커밋

---

## 개발 프로세스

기능 하나가 **충분한 품질**에 도달할 때까지 아래 사이클을 반복한다.

```
기능 요청 (CEO)
  ↓
ChatGPT — 구현 Task 작성
  ↓
Cursor — 구현
  ↓
ChatGPT — 리뷰
  ↓
Cursor — 수정
  ↓
ChatGPT — QA
  ↓
Cursor — 수정
  ↓
ChatGPT — UX/UI 최적화 (Task 작성)
  ↓
Cursor — 수정
  ↓
ChatGPT — Release Review
  ↓
Release (CEO 최종 승인)
```

### 사이클 원칙

1. **개발 중 적극 개선** — ChatGPT는 기능 개발 중 개선사항을 적극적으로 찾고 Task로 만든다
2. **제안이 아닌 Task** — 구조 변경, 리팩토링, 폴더 변경도 Task로 생성한다
3. **반복 QA·UX** — 기능이 완성될 때까지 QA와 UX 개선을 반복한다
4. **충분히 좋으면 Release** — "충분히 좋다"는 수준에 도달하면 Release를 권장한다
5. **MVP 우선** — 개발 중에는 완벽한 코드보다 빠른 검증(MVP)을 우선한다
6. **UX 최우선** — 항상 사용자 경험을 최우선으로 한다
7. **실행 가능한 Task** — 모든 리뷰 결과는 Cursor가 바로 실행할 수 있는 Task 형태로 작성한다

---

## 1. 기능 요청 — CEO

CEO가 기능 아이디어와 우선순위를 결정한다.

**체크리스트:**

- [ ] 이번 스프린트 목표가 MVP와 일치하는가?
- [ ] `VISION.md`의 Non-goals를 위반하지 않는가?
- [ ] `PROJECT_STATE.md`의 Current Goal에 기록했는가?

---

## 2. Task 작성 — ChatGPT (CTO)

CEO의 방향을 받아 Cursor가 실행할 Task를 작성한다.

**프롬프트:** `.ai/prompts/FEATURE_REQUEST.md`

**Task 작성 기준:**

- 파일명과 변경 내용이 구체적이어야 한다
- 완료 조건이 검증 가능해야 한다
- MVP 범위를 벗어나지 않아야 한다

---

## 3. 구현 — Cursor

ChatGPT Task를 순서대로 구현한다.

- MVP 범위 내에서 빠르게 검증한다
- 불필요한 라이브러리 추가 금지
- 완료 후 문서 업데이트, 커밋·Push

---

## 4. 리뷰 — ChatGPT (CTO)

**프롬프트:** `.ai/prompts/REVIEW.md`

**평가 항목:** 기능 완성도, UX/UI, Business Logic, Security, Performance, Error Handling, Maintainability, SaaS Quality

| 결과 | 의미 | 다음 단계 |
|------|------|-----------|
| Pass | 기본 품질 충족 | QA 단계로 |
| Revise | 수정 필요 | Cursor Task 반영 후 재리뷰 |
| Hold | 범위 초과 | CEO와 범위 재조정 |

---

## 5. QA — ChatGPT (CTO)

**프롬프트:** `.ai/prompts/QA.md`

- 버그를 찾고 심각도를 분류한다
- 누락된 기능을 찾는다
- Critical/High가 있으면 Release 차단

QA 통과 전까지 Cursor 수정 → QA 반복.

---

## 6. UX/UI 최적화 — ChatGPT (CTO)

리뷰·QA 이후 사용성을 개선한다.

- UX/UI 개선사항을 **Task**로 작성한다
- Cursor가 수정 후 다시 검토한다
- "충분히 좋다"는 수준까지 반복

---

## 7. Release Review — ChatGPT (CTO) + CEO

**프롬프트:** `.ai/prompts/RELEASE.md`

**출시 전 필수 조건:**

- [ ] 리뷰·QA·UX 최적화 완료
- [ ] Known Issues에 Critical/High 없음
- [ ] CHANGELOG 업데이트 완료
- [ ] README 실행 방법 검증 완료

**출시 후 (Cursor):**

1. Git 태그 생성
2. `CHANGELOG.md`에 Release 날짜 기록
3. `PROJECT_STATE.md` Completed 반영
4. CEO가 Next Sprint 목표 설정

---

## GitHub — 버전 관리

Cursor가 커밋·Push를 담당한다.

**브랜치 전략 (권장):**

```
main          ← 안정 버전 (Release)
feature/*     ← 기능 개발
fix/*         ← 버그 수정
```

**커밋 규칙:**

```
<type>: <description>

type: feat | fix | docs | refactor | chore
```

---

## 역할별 요약

| 역할 | 도구 | 핵심 책임 | 구현 |
|------|------|-----------|------|
| CEO | — | 아이디어, 우선순위, 최종 승인 | ✗ |
| CTO | ChatGPT | Task 작성, QA, 리뷰, 출시 판단 | ✗ |
| Engineer | Cursor | 구현, 문서, 커밋·Push | ✓ |
| GitHub | — | 저장, 이력, 태그 | — |

---

## 예외 처리

### 긴급 버그 (Hotfix)

```
버그 발견 → .ai/prompts/QA.md로 분류 → Critical이면 즉시 fix/* 브랜치
→ Cursor 수정·커밋 → ChatGPT 간소 리뷰 → main merge → 태그
```

### 범위 변경

CEO가 Current Goal을 변경하면 ChatGPT가 `.ai/prompts/FEATURE_REQUEST.md`로 Task를 다시 작성한다.

### 문서만 변경

코드 변경 없이 문서만 수정할 때도 Cursor가 `CHANGELOG.md`에 기록하고 커밋한다.
