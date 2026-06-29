# AI-DOS

**AI Development Operating System** — 모든 프로젝트에서 재사용하는 공통 개발 운영체계

---

## AI-DOS란?

AI-DOS는 SaaS 제품이 아니다. 코드 저장소가 아니라 **문서와 개발 프로세스를 관리하는 표준 저장소**다.

새 프로젝트를 시작할 때 이 저장소의 규칙, 템플릿, 프롬프트를 그대로 복사하면 팀 전체가 동일한 방식으로 개발할 수 있다.

| 역할 | 도구 | 담당 |
|------|------|------|
| 의사결정 | CEO | 방향과 우선순위 |
| 기술 검토 | ChatGPT (CTO) | 설계·리뷰·출시 판단 |
| 구현 | Cursor (Senior Engineer) | 코드·문서 작성 |
| 버전 관리 | GitHub | 저장·협업·배포 |

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

---

## Repository 구조

```
AI-DOS/
├── README.md                 # 이 문서 — AI-DOS 전체 가이드
├── docs/
│   ├── WORKFLOW.md           # CEO → CTO → Engineer → Release 흐름
│   ├── DEVELOPMENT_RULES.md  # 공통 개발 원칙
│   └── VERSIONING.md         # AI-DOS 버전 정책
├── templates/
│   ├── PROJECT_STATE.md      # 프로젝트 현재 상태 템플릿
│   ├── CHANGELOG.md          # 변경 이력 템플릿
│   ├── VISION.md             # 제품 철학 템플릿
│   └── README_TEMPLATE.md    # 새 프로젝트 README 템플릿
├── prompts/
│   ├── REVIEW.md             # CTO 코드 리뷰 프롬프트
│   ├── QA.md                 # 버그 리뷰 프롬프트
│   ├── RELEASE.md            # 출시 판단 프롬프트
│   └── FEATURE_REQUEST.md    # 기능 개발 요청 프롬프트
└── .cursor/
    └── rules/
        └── ai-dos.mdc        # Cursor 공통 규칙
```

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
cp -r AI-DOS/prompts my-project/

# 2. 프로젝트에 맞게 문서 작성
# VISION.md, README.md, PROJECT_STATE.md 내용을 채운다
```

Windows PowerShell:

```powershell
New-Item -ItemType Directory -Force -Path my-project\.cursor\rules
Copy-Item AI-DOS\.cursor\rules\ai-dos.mdc my-project\.cursor\rules\
Copy-Item AI-DOS\templates\VISION.md, AI-DOS\templates\PROJECT_STATE.md, AI-DOS\templates\CHANGELOG.md my-project\
Copy-Item AI-DOS\templates\README_TEMPLATE.md my-project\README.md
Copy-Item AI-DOS\prompts my-project\prompts -Recurse
```

### 2. Cursor 규칙 활성화

`.cursor/rules/ai-dos.mdc`가 프로젝트 루트에 있으면 Cursor가 자동으로 규칙을 적용한다.

### 3. 필수 문서 작성 순서

1. `VISION.md` — 왜, 누구를 위해, 무엇을 하지 않을지
2. `README.md` — 프로젝트 소개와 실행 방법
3. `PROJECT_STATE.md` — 현재 스프린트와 작업 상태
4. `CHANGELOG.md` — 변경 이력 (Keep a Changelog 형식)

### 4. 일상 작업

| 상황 | 사용할 문서/프롬프트 |
|------|---------------------|
| 새 기능 개발 | `prompts/FEATURE_REQUEST.md` |
| 구현 완료 후 리뷰 | `prompts/REVIEW.md` |
| 버그 발견 | `prompts/QA.md` |
| 출시 전 점검 | `prompts/RELEASE.md` |
| 작업 시작/완료 | `PROJECT_STATE.md` 업데이트 |

---

## 기본 Workflow

```
CEO (방향 결정)
  ↓
ChatGPT / CTO (기능 정의, 리뷰, 출시 판단)
  ↓
Cursor / Engineer (구현, 문서 업데이트)
  ↓
GitHub (커밋, PR, 태그)
  ↓
Review (prompts/REVIEW.md)
  ↓
Release (prompts/RELEASE.md)
```

상세 흐름은 [docs/WORKFLOW.md](docs/WORKFLOW.md)를 참고한다.

---

## 사용 예시

### 예시 1: 로그인 기능 추가

1. **CEO** — "이번 스프린트에 이메일 로그인을 넣는다"고 `PROJECT_STATE.md`의 Current Goal에 기록
2. **CTO (ChatGPT)** — `prompts/FEATURE_REQUEST.md`로 목표·요구사항·완료 조건 정의
3. **Engineer (Cursor)** — 구현 후 `PROJECT_STATE.md`, `CHANGELOG.md` 업데이트
4. **CTO** — `prompts/REVIEW.md`로 리뷰, Cursor Task 출력
5. **Engineer** — 리뷰 피드백 반영
6. **CTO** — `prompts/RELEASE.md`로 출시 가능 여부 판단

### 예시 2: 버그 수정

1. 버그 재현 및 `PROJECT_STATE.md`의 Known Issues에 기록
2. `prompts/QA.md`로 심각도 분류 및 해결 방향 정의
3. Cursor로 수정, `CHANGELOG.md`에 Fixed 항목 추가
4. `PROJECT_STATE.md`에서 Known Issues 제거

---

## 버전 정책

AI-DOS 자체의 버전은 [docs/VERSIONING.md](docs/VERSIONING.md)를 따른다.

| 버전 | 의미 |
|------|------|
| v1.0 | 최초 안정 버전 (현재) |
| v1.x | 문서 보완, 템플릿 개선 (하위 호환) |
| v2.0 | 구조 변경, 워크플로우 개편 (하위 호환 없을 수 있음) |

개별 프로젝트의 버전은 각 프로젝트 `CHANGELOG.md`와 Git 태그로 관리한다.

---

## 관련 문서

- [Workflow](docs/WORKFLOW.md)
- [Development Rules](docs/DEVELOPMENT_RULES.md)
- [Versioning](docs/VERSIONING.md)

---

## 라이선스

이 저장소의 문서와 템플릿은 자유롭게 복사·수정하여 사용할 수 있다.
