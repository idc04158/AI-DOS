# AI-DOS

**AI Development Operating System** — 모든 프로젝트에서 재사용하는 공통 개발 운영체계

---

## AI-DOS란?

AI-DOS는 SaaS 제품이 아니다. 코드 저장소가 아니라 **문서와 개발 프로세스를 관리하는 표준 저장소**다.

새 프로젝트를 시작할 때 이 저장소의 규칙, 템플릿, 프롬프트를 그대로 복사하면 팀 전체가 동일한 방식으로 개발할 수 있다.

| 역할 | 도구 | 담당 |
|------|------|------|
| CEO (사용자) | — | 제품 아이디어·기능·우선순위 결정, 최종 의사결정 |
| ChatGPT (CTO) | ChatGPT | 개발 방향 관리, Task 작성, QA·리뷰, 출시 판단 (구현 안 함) |
| Cursor (Senior Engineer) | Cursor | Task 구현, 리팩토링, 버그 수정, 문서·커밋·Push |
| 버전 관리 | GitHub | 저장·협업·배포 |

---

## 역할 정의

### CEO (사용자)

- 제품 아이디어와 기능을 결정한다
- 우선순위를 결정한다
- 최종 의사결정을 한다

### ChatGPT (CTO)

- 제품 관점에서 개발 방향을 관리한다
- 기능 요구사항을 Cursor가 수행 가능한 **Task**로 분해한다
- 기능이 완성될 때까지 반복적으로 QA를 수행한다
- QA Sprint 중에는 Cursor 자율 작업을 보완하는 Task를 추가할 수 있다
- 버그, 누락 기능, UX/UI, 비즈니스 로직을 검토한다
- SaaS 완성도를 높인다
- Cursor에게 전달할 **Task만** 생성한다 — **구현은 하지 않는다**

### Cursor (Senior Engineer)

- ChatGPT가 작성한 Task를 구현한다
- 리팩토링하고 버그를 수정한다
- 문서를 최신 상태로 유지한다
- 커밋과 Push를 수행한다

---

## 개발 모드

모든 스프린트는 아래 두 모드 중 **하나**만 선택한다.  
CEO가 `PROJECT_STATE.md`의 **Sprint Mode**에 기록한다.

| 모드 | 목적 | 기능 추가 |
|------|------|-----------|
| **Feature Sprint** | 새로운 기능 개발 | ✓ |
| **QA Sprint** | 기능 추가 없이 품질 향상 | ✗ |

### Feature Sprint

- CEO가 새 기능과 우선순위를 결정한다
- ChatGPT가 `.ai/prompts/FEATURE_REQUEST.md`로 구현 Task를 작성한다
- 구현 → 리뷰 → QA → UX → Release 사이클을 따른다

### QA Sprint

- **새 기능을 개발하지 않는다**
- 프로젝트 전체를 분석하여 **가장 영향도가 큰 문제부터 스스로** 해결한다
- ChatGPT(CTO)가 중간에 Task를 추가할 수 있다 — 새 Task가 없으면 Cursor가 QA Sprint를 **자동으로 계속**한다
- 종료 조건 충족 시에만 QA Sprint 종료를 보고한다 ("다음 Sprint를 진행할까요?" 질문 금지)

**우선순위 (높음 → 낮음):**

1. Runtime Crash
2. 기능 동작 오류
3. 데이터 손상 및 예외 처리
4. UX 흐름 개선
5. UI 일관성 및 사용성 개선
6. 성능 개선
7. 리팩토링
8. 유지보수성 향상

**작업 루프 (문제당 1회):**

```
가장 우선순위가 높은 문제 선택 → 수정 → 자체 테스트
→ PROJECT_STATE.md · CHANGELOG.md 업데이트 → Commit & Push → 반복
```

**종료 조건 (모두 충족 시 보고):**

- Runtime 오류가 모두 제거됨
- 치명적인 기능 오류가 없음
- UX가 일관된 상태
- 더 이상 High Priority 이슈가 없음

---

## 왜 만들었는가?

프로젝트마다 규칙이 달라지면 다음 문제가 반복된다.

- AI에게 매번 다른 지시를 내려야 한다
- 문서가 코드와 어긋난다
- 리뷰 기준이 사람마다 다르다
- 출시 판단이 감에 의존한다

AI-DOS는 이 문제를 **한 번 정의하고, 모든 프로젝트에 복사**하는 방식으로 해결한다.

---

## 철학

1. **단순함** — 복잡한 프로세스보다 실행 가능한 최소 규칙
2. **유지보수** — 빠른 개발보다 오래 쓸 수 있는 구조
3. **문서 우선** — 코드보다 문서가 먼저, 문서와 코드는 항상 동기화
4. **AI 친화** — Cursor, ChatGPT가 바로 이해하고 실행할 수 있는 형식
5. **PM 가독성** — 비개발자도 흐름과 상태를 파악할 수 있는 수준
6. **불필요한 기능 금지** — 필요할 때만 추가한다
7. **프로젝트 비침범** — AI-DOS는 프로젝트 코드·문서와 경로가 충돌하지 않는다
8. **독립 운영** — AI-DOS는 프로젝트 내부에서 독립적인 운영 시스템이다
9. **`.ai/` 네임스페이스** — AI-DOS 관련 파일은 `.ai/` 아래에 둔다

---

## Repository 구조

```
AI-DOS/
├── README.md                 # 이 문서 — AI-DOS 전체 가이드
├── CHANGELOG.md              # AI-DOS 변경 이력
├── docs/
│   ├── WORKFLOW.md           # CEO → CTO → Engineer → Release 흐름
│   ├── DEVELOPMENT_RULES.md  # 공통 개발 원칙
│   └── VERSIONING.md         # AI-DOS 버전 정책
├── templates/
│   ├── PROJECT_STATE.md      # 프로젝트 현재 상태 템플릿
│   ├── CHANGELOG.md          # 변경 이력 템플릿
│   ├── VISION.md             # 제품 철학 템플릿
│   └── README_TEMPLATE.md    # 새 프로젝트 README 템플릿
├── .ai/
│   └── prompts/
│       ├── REVIEW.md         # CTO 코드 리뷰 프롬프트
│       ├── QA.md             # 버그 리뷰 프롬프트
│       ├── RELEASE.md        # 출시 판단 프롬프트
│       └── FEATURE_REQUEST.md # 기능 개발 요청 프롬프트
└── .cursor/
    └── rules/
        └── ai-dos.mdc        # Cursor 공통 규칙
```

> **경로 분리:** 프로젝트의 AI Prompt는 `prompts/`에 자유롭게 둔다.  
> AI-DOS 운영 프롬프트는 `.ai/prompts/`에만 위치하므로 충돌하지 않는다.

---

## 프로젝트 적용 방법

### 1. 새 프로젝트 시작

```bash
# 1. AI-DOS에서 필요한 파일을 새 프로젝트로 복사
mkdir -p my-project/.cursor/rules
cp AI-DOS/.cursor/rules/ai-dos.mdc my-project/.cursor/rules/
cp AI-DOS/templates/VISION.md my-project/
cp AI-DOS/templates/PROJECT_STATE.md my-project/
cp AI-DOS/templates/CHANGELOG.md my-project/
cp AI-DOS/templates/README_TEMPLATE.md my-project/README.md
cp -r AI-DOS/.ai my-project/

# 2. 프로젝트에 맞게 문서 작성
# VISION.md, README.md, PROJECT_STATE.md 내용을 채운다
```

Windows PowerShell:

```powershell
New-Item -ItemType Directory -Force -Path my-project\.cursor\rules
Copy-Item AI-DOS\.cursor\rules\ai-dos.mdc my-project\.cursor\rules\
Copy-Item AI-DOS\templates\VISION.md, AI-DOS\templates\PROJECT_STATE.md, AI-DOS\templates\CHANGELOG.md my-project\
Copy-Item AI-DOS\templates\README_TEMPLATE.md my-project\README.md
Copy-Item AI-DOS\.ai my-project\.ai -Recurse
```

### 2. Cursor 규칙 활성화

`.cursor/rules/ai-dos.mdc`가 프로젝트 루트에 있으면 Cursor가 자동으로 규칙을 적용한다.

### 3. 필수 문서 작성 순서

1. `VISION.md` — 왜, 누구를 위해, 무엇을 하지 않을지
2. `README.md` — 프로젝트 소개와 실행 방법
3. `PROJECT_STATE.md` — 현재 스프린트와 작업 상태
4. `CHANGELOG.md` — 변경 이력 (Keep a Changelog 형식)

### 4. 일상 작업

| 상황 | Sprint Mode | 사용할 문서/프롬프트 |
|------|-------------|---------------------|
| 새 기능 개발 | Feature Sprint | `.ai/prompts/FEATURE_REQUEST.md` |
| 품질 향상 (자율 QA) | QA Sprint | Cursor 자율 분석·수정 (CTO Task 병행 가능) |
| 구현 완료 후 리뷰 | 공통 | `.ai/prompts/REVIEW.md` |
| 버그 발견 | 공통 | `.ai/prompts/QA.md` |
| 출시 전 점검 | 공통 | `.ai/prompts/RELEASE.md` |
| 작업 시작/완료 | 공통 | `PROJECT_STATE.md` 업데이트 |

---

## 기본 Workflow

스프린트 모드에 따라 흐름이 달라진다. `PROJECT_STATE.md`의 **Sprint Mode**를 먼저 확인한다.

### Feature Sprint — 새 기능 개발

기능 하나가 충분한 품질에 도달할 때까지 아래 사이클을 반복한다.

```
기능 요청 (CEO)
  ↓
ChatGPT — 구현 Task 작성 (.ai/prompts/FEATURE_REQUEST.md)
  ↓
Cursor — 구현
  ↓
ChatGPT — 리뷰 → QA → UX/UI → Release Review
  ↓
Cursor — 수정 (각 단계마다)
  ↓
Release (CEO 최종 승인)
```

### QA Sprint — 자율 품질 향상 (기능 추가 없음)

```
QA Sprint 선언 (CEO) — Sprint Mode: QA Sprint
  ↓
Cursor — 프로젝트 전체 분석, 최우선 문제 1건 선택·수정·테스트
  ↓
Cursor — PROJECT_STATE · CHANGELOG 업데이트 → Commit & Push
  ↓
(반복) 종료 조건 충족까지 자동 진행
  ↓
Cursor — QA Sprint 종료 보고

ChatGPT(CTO) — 언제든 새 Task 추가 가능
  → Cursor는 Task 처리 후 QA Sprint 자율 루프 재개
```

**핵심 원칙:**

- ChatGPT는 제안이 아닌 **실행 가능한 Task**만 출력한다
- 구조 변경·리팩토링·폴더 변경도 필요 시 Task로 생성한다
- 개발 중에는 완벽한 코드보다 **빠른 검증(MVP)** 을 우선한다
- **사용자 경험**을 항상 최우선으로 한다
- "충분히 좋다"는 수준에 도달하면 Release를 권장한다

상세 흐름은 [docs/WORKFLOW.md](docs/WORKFLOW.md)를 참고한다.

---

## 사용 예시

### 예시 1: 로그인 기능 추가 (Feature Sprint)

1. **CEO** — `PROJECT_STATE.md`에 Sprint Mode: **Feature Sprint**, Current Goal: "이메일 로그인"
2. **CTO (ChatGPT)** — `.ai/prompts/FEATURE_REQUEST.md`로 Task 작성 → Cursor에 전달
3. **Engineer (Cursor)** — 구현, 문서 업데이트, 커밋·Push
4. **CTO** — `.ai/prompts/REVIEW.md`로 리뷰 → Cursor Task 출력
5. **Engineer** — Task 반영, 커밋·Push
6. **CTO** — `.ai/prompts/QA.md`로 버그·누락 기능 점검 → Cursor Task 출력
7. **Engineer** — Task 반영
8. **CTO** — UX/UI 개선 Task 작성
9. **Engineer** — Task 반영
10. **CTO** — `.ai/prompts/RELEASE.md`로 출시 판단 → CEO 최종 승인

### 예시 2: QA Sprint (자율 품질 향상)

1. **CEO** — Sprint Mode: **QA Sprint** 설정
2. **Engineer (Cursor)** — 프로젝트 분석, Runtime Crash 1건 발견·수정·테스트
3. **Engineer** — 문서 업데이트, Commit & Push
4. **Engineer** — 다음 우선순위(기능 동작 오류) 자동 선택·수정·반복
5. **CTO (ChatGPT)** — (선택) 추가 Task 전달 → Cursor 처리 후 자율 루프 재개
6. **Engineer** — 종료 조건 충족 시 QA Sprint 종료 보고 (중간에 "다음 Sprint?" 질문 없음)

### 예시 3: Feature Sprint 완료 후 QA Sprint 전환

1. **CEO** — MVP 기능 완료 후 Sprint Mode: **QA Sprint** 전환
2. **Engineer** — 자율 QA 루프로 품질 목표 달성
3. **CTO** — `.ai/prompts/RELEASE.md`로 출시 판단 → CEO 최종 승인

---

## 버전 정책

AI-DOS 자체의 버전은 [docs/VERSIONING.md](docs/VERSIONING.md)를 따른다.

| 버전 | 의미 |
|------|------|
| v1.0.0 | 최초 안정 버전 |
| v1.0.1 | `.ai/prompts/` 경로 분리 |
| v1.0.2 | 역할 정의 및 반복 개발 프로세스 명확화 |
| v1.0.3 | Feature/QA Sprint 모드, 자율 QA Sprint 워크플로우 (현재) |
| v1.x | 문서 보완, 템플릿 개선 (하위 호환) |
| v2.0 | 구조 변경, 워크플로우 개편 (하위 호환 없을 수 있음) |

개별 프로젝트의 버전은 각 프로젝트 `CHANGELOG.md`와 Git 태그로 관리한다.

---

## 관련 문서

- [Workflow](docs/WORKFLOW.md)
- [Development Rules](docs/DEVELOPMENT_RULES.md)
- [Versioning](docs/VERSIONING.md)
- [Changelog](CHANGELOG.md)

---

## 라이선스

이 저장소의 문서와 템플릿은 자유롭게 복사·수정하여 사용할 수 있다.
