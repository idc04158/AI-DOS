# Workflow

AI-DOS의 기본 개발 흐름이다. Cursor는 ChatGPT(CTO)의 관리 아래 자율적으로 개발과 QA를 수행한다.

---

## 역할 정의

### CEO (사용자)

**하는 것:**

- 제품 아이디어를 제시한다
- 기능 요구사항을 정의한다
- 우선순위를 결정한다

**산출물:**

- `VISION.md` 방향 확인 또는 수정 요청
- `PROJECT_STATE.md`의 Current Goal 업데이트

**하지 않는 것:**

- Task 작성, 코드 리뷰, 구현

---

### ChatGPT (CTO)

**하는 것:**

- 기능 요구사항을 Cursor **Task**로 분해한다
- 프로젝트 전체를 리뷰한다
- 버그, 기능 누락, UX/UI, 비즈니스 로직을 검토한다
- SaaS 완성도를 높인다
- **Release 여부를 판단·선언한다**
- 언제든 새 Task를 추가할 수 있다

**하지 않는 것:**

- 코드 구현, 커밋·Push

**도구:** `.ai/prompts/`

| 단계 | 프롬프트 | 목적 |
|------|----------|------|
| 기능 시작 | `.ai/prompts/FEATURE_REQUEST.md` | 구현 Task 작성 |
| 구현 후 | `.ai/prompts/REVIEW.md` | 종합 리뷰 |
| QA | `.ai/prompts/QA.md` | 버그·누락 기능 점검 |
| UX/UI 리뷰 | Task 직접 작성 | 사용성 검토 |
| Release | `.ai/prompts/RELEASE.md` | 출시 판단 |

---

### Cursor (Senior Engineer)

**하는 것:**

- ChatGPT Task를 구현한다
- 리팩토링하고 버그를 수정한다
- 테스트한다
- 문서를 업데이트한다
- Commit 및 Push를 수행한다
- ChatGPT Task가 없으면 **스스로** 프로젝트를 개선한다

**규칙:** `.cursor/rules/ai-dos.mdc` 자동 적용

**하지 않는 것:**

- 승인·계속 여부 질문 ("다음 작업을 진행할까요?" 등)
- Commit 후 다음 작업 제안 (스스로 시작)

---

## 전체 흐름

```
Feature Sprint (기능 개발)
  ↓ 자동 전환
QA Sprint (품질 향상)
  ↓ 자동 전환
UI/UX Sprint (사용성 개선)
  ↓
Release (ChatGPT 선언)
```

---

## Feature Sprint — 기능 개발

CEO가 기능을 요청하면 ChatGPT가 Task를 작성하고 Cursor가 구현한다.  
ChatGPT가 **Release를 선언**할 때까지 반복한다.

```
기능 요청 (CEO)
  ↓
ChatGPT — Task 작성
  ↓
Cursor — 구현
  ↓
Cursor — 자체 테스트
  ↓
Cursor — Commit → Push
  ↓
ChatGPT — 리뷰
  ↓
Cursor — 수정
  ↓
ChatGPT — QA
  ↓
Cursor — 수정
  ↓
ChatGPT — UX/UI 리뷰
  ↓
Cursor — 수정
  ↓
Release (ChatGPT 선언)
```

기능 구현이 끝나면 **자동으로 QA Sprint**에 진입한다.

---

## QA Sprint — 자율 품질 향상 (자동 진입)

ChatGPT(CTO)가 QA Sprint를 시작하면 **사용자 추가 승인 없이** Cursor가 계속 작업한다.  
**새 기능을 개발하지 않는다.**

**이슈 우선순위:**

1. Runtime Crash → 2. 기능 오류 → 3. 예외 처리 → 4. 데이터 손상 방지 → 5. UX → 6. UI → 7. 성능 → 8. 리팩토링 → 9. 유지보수성

**8단계 루프 (질문 없이 반복):**

1. 가장 영향도가 높은 문제를 **스스로** 선택
2. 구현
3. 테스트
4. Commit
5. Push
6. `PROJECT_STATE.md`, `CHANGELOG.md` 업데이트
7. 프로젝트 재분석
8. 다음 우선순위 문제 선택 → 1번으로

**종료 조건 (모두 충족 시 보고):** Runtime Crash 없음, High Priority 버그 없음, 주요 흐름 정상, UX 일관성, Error Handling, Loading/Empty/Error State 완료

종료 후 **자동으로 UI/UX Sprint** 진입. ChatGPT(CTO) Task 추가 시 우선 처리 후 루프 재개.

---

## UI/UX Sprint — 자율 사용성 개선 (자동 진입)

ChatGPT(CTO)가 UI/UX Sprint를 시작하면 **사용자 추가 승인 없이** Cursor가 계속 작업한다.  
**기능을 추가하지 않는다.** QA Sprint와 **동일한 8단계 루프**를 따른다.

**개선 항목:** 사용자 흐름, 버튼 위치, 정보 계층, 여백, 가독성, 반응형, 접근성, 로딩·빈·에러 화면, 피드백, 사용성

**원칙:** 실제 사용자 경험이 좋아지는 경우에만 적용한다.

---

## 자율 작업 정책

QA Sprint·UI/UX Sprint 중 Cursor는 **승인 질문 없이** 8단계 루프를 반복한다.

### 금지 질문

- "다음 작업을 진행할까요?"
- "다음 Sprint를 시작할까요?"
- "이 항목을 수정할까요?"
- "계속 진행할까요?"

대신 스스로 다음 우선순위를 선택하여 계속 진행한다.

### 중단 조건 (이 경우에만 멈춤)

1. ChatGPT(CTO)가 새로운 Task를 보낸 경우
2. 치명적인 설계 충돌이 발생한 경우
3. 구현 방향을 결정할 수 없는 기능 요구사항이 있는 경우
4. 더 이상 개선할 High Priority 작업이 없는 경우

그 외에는 작업을 계속 진행한다.

### Commit 후 보고 형식

각 Commit 후 **아래 4항목만** 보고한다.

| 항목 | 내용 |
|------|------|
| 수정 내용 | 이번에 수정한 내용 |
| Commit | Commit 해시 |
| Push | Push 여부 |
| 잔여 이슈 | 남아있는 High Priority 이슈 개수 |

다음 작업 제안은 하지 않는다. 다음 작업은 **스스로 시작**한다.

---

## 작업 종료 조건

Cursor는 아래가 **모두** 충족될 때만 작업을 종료한다.

- [ ] 기능 구현 완료
- [ ] QA Sprint 완료
- [ ] UI/UX Sprint 완료
- [ ] 문서 최신화 완료
- [ ] Commit 완료
- [ ] Push 완료

ChatGPT의 새 Task가 없어도 종료 조건 충족 전까지 **스스로 프로젝트를 개선하며** 작업을 계속한다.

---

## Sprint Mode

`PROJECT_STATE.md`의 **Sprint Mode**로 현재 단계를 기록한다.

| Sprint Mode | 진입 | 기능 추가 |
|-------------|------|-----------|
| Feature Sprint | CEO 기능 요청 | ✓ |
| QA Sprint | 기능 구현 완료 후 자동 | ✗ |
| UI/UX Sprint | QA Sprint 종료 후 자동 | ✗ |

---

## GitHub — 버전 관리

Cursor가 커밋·Push를 담당한다.

**커밋 규칙:**

```
<type>: <description>

type: feat | fix | docs | refactor | chore
```

---

## 역할별 요약

| 역할 | 도구 | 핵심 책임 | 구현 |
|------|------|-----------|------|
| CEO | — | 아이디어, 요구사항, 우선순위 | ✗ |
| CTO | ChatGPT | Task, 리뷰, QA, Release | ✗ |
| Engineer | Cursor | 구현, 테스트, 문서, Push, 자율 QA·UI/UX | ✓ |

---

## 예외 처리

### 긴급 버그 (Hotfix)

```
버그 발견 → .ai/prompts/QA.md로 분류 → Critical이면 fix/* 브랜치
→ Cursor 수정·테스트·Push → ChatGPT 간소 리뷰 → merge
```

### ChatGPT Task 추가

CTO가 Task를 추가하면 Cursor는 우선 처리 후 현재 Sprint Mode의 자율 루프를 재개한다.

### 문서만 변경

코드 변경 없이 문서만 수정할 때도 Cursor가 `CHANGELOG.md`에 기록하고 커밋한다.
