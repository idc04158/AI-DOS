# AI-DOS

**AI Development Operating System** — 모든 프로젝트에서 재사용하는 공통 개발 운영체계

---

## AI-DOS란?

AI-DOS는 SaaS 제품이 아니다. 코드 저장소가 아니라 **문서와 개발 프로세스를 관리하는 표준 저장소**다.

새 프로젝트를 시작할 때 이 저장소의 규칙, 템플릿, 프롬프트를 그대로 복사하면 팀 전체가 동일한 방식으로 개발할 수 있다.

| 역할 | 도구 | 담당 |
|------|------|------|
| CEO (사용자) | — | 제품 아이디어·기능 요구사항·우선순위 |
| ChatGPT (CTO) | ChatGPT | Task 분해, 리뷰, QA, UX/UI, Release 판단 (구현 안 함) |
| Cursor (Senior Engineer) | Cursor | 구현, 테스트, 문서, Commit·Push, 자율 QA·UI/UX |
| 버전 관리 | GitHub | 저장·협업·배포 |

---

## 역할 정의

### CEO (사용자)

- 제품 아이디어를 제시한다
- 기능 요구사항을 정의한다
- 우선순위를 결정한다

### ChatGPT (CTO)

- 기능 요구사항을 Cursor **Task**로 분해한다
- 프로젝트 전체를 리뷰한다
- 버그, 기능 누락, UX/UI, 비즈니스 로직을 검토한다
- SaaS 완성도를 높인다
- **Release 여부를 판단한다**
- **구현은 하지 않는다**

### Cursor (Senior Engineer)

ChatGPT(CTO)의 관리 아래 자율적으로 개발과 QA를 수행한다.

- ChatGPT Task를 구현한다
- 리팩토링하고 버그를 수정한다
- 테스트한다
- 문서를 업데이트한다
- Commit 및 Push를 수행한다
- ChatGPT Task가 없으면 **스스로** 프로젝트를 개선한다

---

## 개발 사이클

### 기능 개발 (Feature Sprint)

새 기능은 아래 순서로 진행한다. ChatGPT가 **Release를 선언**할 때까지 반복한다.

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

### QA Sprint (자동 진입)

ChatGPT(CTO)가 QA Sprint를 시작하면 **사용자 추가 승인 없이** Cursor가 계속 작업한다.

- **새 기능을 개발하지 않는다**
- 아래 8단계 루프를 **질문 없이** 반복한다

**우선순위:** Runtime Crash → 기능 오류 → 예외 처리 → 데이터 손상 방지 → UX → UI → 성능 → 리팩토링 → 유지보수성

**8단계 루프:**

1. 가장 영향도가 높은 문제를 스스로 선택
2. 구현
3. 테스트
4. Commit
5. Push
6. `PROJECT_STATE.md`, `CHANGELOG.md` 업데이트
7. 프로젝트 재분석
8. 다음 우선순위 문제 선택 → 1번으로 반복

**종료 조건 (모두 충족 시 보고):** Runtime Crash 없음, High Priority 버그 없음, 주요 흐름 정상, UX 일관성, Error Handling, Loading/Empty/Error State 완료

종료 후 **자동으로 UI/UX Sprint**에 진입한다.

### UI/UX Sprint (자동 진입)

ChatGPT(CTO)가 UI/UX Sprint를 시작하면 **사용자 추가 승인 없이** Cursor가 계속 작업한다.

- **기능을 추가하지 않는다**
- 실제 사용자 경험이 좋아지는 경우에만 개선
- QA Sprint와 **동일한 8단계 루프** 적용 (질문 없이 반복)

**개선 항목:** 사용자 흐름, 버튼 위치, 정보 계층, 여백, 가독성, 반응형, 접근성, 로딩·빈·에러 화면, 피드백, 사용성

### 자율 작업 정책

**절대 하지 않는 질문:**

- "다음 작업을 진행할까요?"
- "다음 Sprint를 시작할까요?"
- "이 항목을 수정할까요?"
- "계속 진행할까요?"

**중단 조건 (이 경우에만 멈춤):**

1. ChatGPT(CTO)가 새로운 Task를 보낸 경우 → Task 처리 후 자율 루프 재개
2. 치명적인 설계 충돌이 발생한 경우
3. 구현 방향을 결정할 수 없는 기능 요구사항이 있는 경우
4. 더 이상 개선할 High Priority 작업이 없는 경우

**Commit 후 보고 (아래 4항목만):**

- 이번에 수정한 내용
- Commit 해시
- Push 여부
- 현재 남아있는 High Priority 이슈 개수

다음 작업 제안은 하지 않는다. 다음 작업은 **스스로 시작**한다.

### 작업 종료 조건

아래가 **모두** 충족될 때만 작업을 종료한다.

- [ ] 기능 구현 완료
- [ ] QA Sprint 완료
- [ ] UI/UX Sprint 완료
- [ ] 문서 최신화 완료
- [ ] Commit 완료
- [ ] Push 완료

그 전까지 ChatGPT의 새 Task가 없어도 **스스로 프로젝트를 개선하며** 작업을 계속한다.

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

| 상황 | Sprint Mode | 담당 |
|------|-------------|------|
| 새 기능 개발 | Feature Sprint | ChatGPT Task → Cursor 구현·테스트·Push |
| 품질 향상 (자동 진입) | QA Sprint | Cursor 자율 분석·수정 |
| UI/UX 개선 (자동 진입) | UI/UX Sprint | Cursor 자율 개선 |
| 리뷰·QA·Release | 공통 | ChatGPT (`.ai/prompts/`) |
| 상태 기록 | 공통 | `PROJECT_STATE.md` |

---

## 기본 Workflow

전체 흐름: **Feature Sprint → QA Sprint (자동) → UI/UX Sprint (자동) → Release**

`PROJECT_STATE.md`의 **Sprint Mode**로 현재 단계를 확인한다.

상세 흐름은 [docs/WORKFLOW.md](docs/WORKFLOW.md)를 참고한다.

**핵심 원칙:**

- QA·UI/UX Sprint 중 **사용자 승인 없이** 자율 반복
- Commit 후 4항목만 보고, 다음 작업은 **스스로 시작**
- 승인·계속 여부 **질문 금지**

---

## 사용 예시

### 예시 1: 로그인 기능 (전체 사이클)

1. **CEO** — 기능 요구사항: "이메일 로그인"
2. **CTO** — Task 작성 → **Cursor** — 구현·테스트·Commit·Push
3. **CTO** — 리뷰 → **Cursor** 수정 → QA → **Cursor** 수정 → UX/UI 리뷰 → **Cursor** 수정
4. **CTO** — Release 선언 → **자동 QA Sprint** 진입
5. **Cursor** — Runtime Crash·기능 오류 등 우선순위대로 자율 수정·Push (질문 없이 반복)
6. QA Sprint 종료 → **자동 UI/UX Sprint** — 여백·로딩 화면 등 개선·Push
7. UI/UX Sprint 종료 → 전체 종료 조건 충족 보고

---

## 버전 정책

AI-DOS 자체의 버전은 [docs/VERSIONING.md](docs/VERSIONING.md)를 따른다.

| 버전 | 의미 |
|------|------|
| v1.0.0 | 최초 안정 버전 |
| v1.0.1 | `.ai/prompts/` 경로 분리 |
| v1.0.2 | 역할 정의 및 반복 개발 프로세스 명확화 |
| v1.0.3 | Feature/QA Sprint, 자율 QA 워크플로우 |
| v1.0.4 | UI/UX Sprint, 자율 개발 사이클 통합 |
| v1.0.5 | 연속 자율 작업 정책, 승인 질문 제거 (현재) |
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
