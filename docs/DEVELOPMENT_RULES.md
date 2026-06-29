# Development Rules

모든 AI-DOS 기반 프로젝트가 따르는 공통 개발 원칙이다.

---

## 1. MVP 우선

- 출시 가능한 최소 기능부터 만든다
- "나중에 필요할 것 같다"는 이유로 기능을 추가하지 않는다
- 완성도 80%의 MVP가 완성도 100%의 미출시 제품보다 낫다
- `VISION.md`의 "하지 않을 것"을 반드시 확인한다

---

## 2. 기능보다 UX 우선

- 기능이 많아도 사용하기 어려우면 실패다
- 사용자가 다음 행동을 고민하지 않아야 한다
- 에러 메시지는 기술 용어가 아닌 사용자 언어로 작성한다
- 로딩, 빈 상태, 에러 상태를 모두 디자인한다

---

## 3. 유지보수 우선

- 빠른 구현보다 6개월 후에도 이해할 수 있는 코드를 쓴다
- 한 파일이 300줄을 넘으면 분리를 검토한다
- 이름만 보고 역할을 알 수 있어야 한다
- 주석은 "왜"를 설명하고, "무엇"은 코드로 표현한다

---

## 4. 단순한 설계

- 추상화는 두 번 이상 반복될 때만 도입한다
- 디자인 패턴을 보여주기 위해 쓰지 않는다
- 설정 파일보다 코드 내 명시적 값을 우선한다 (설정이 3개 이하일 때)
- 의존성 방향은 단방향으로 유지한다

---

## 5. 불필요한 라이브러리 금지

- 표준 라이브러리나 기존 코드로 해결 가능하면 라이브러리를 추가하지 않는다
- 라이브러리 추가 시 README에 이유를 한 줄로 기록한다
- 마지막 업데이트가 1년 이상 된 라이브러리는 사용을 피한다
- 같은 목적의 라이브러리를 두 개 쓰지 않는다

---

## 6. 문서 최신화

- 코드를 변경하면 같은 PR에서 문서도 변경한다
- 문서와 코드가 어긋나면 **문서를 먼저** 수정하거나, 코드를 문서에 맞춘다
- `README.md`는 신규 팀원이 30분 안에 프로젝트를 실행할 수 있는 수준이어야 한다
- API, 환경 변수, 배포 방법이 바뀌면 README에 즉시 반영한다

---

## 7. 프로젝트 상태 관리

`PROJECT_STATE.md`는 프로젝트의 **유일한 현재 상태 문서**다.

**규칙:**

- 작업 시작 전 반드시 읽는다
- 작업 완료 후 반드시 업데이트한다
- 한 곳에서만 상태를 관리한다 (이슈 트래커, 슬랙, PROJECT_STATE 중복 금지)
- Completed 항목은 날짜를 함께 기록한다

**업데이트 시점:**

| 이벤트 | 업데이트 항목 |
|--------|---------------|
| 스프린트 시작 | Current Sprint, Current Goal |
| 작업 시작 | In Progress |
| 작업 완료 | Completed, In Progress 제거 |
| 버그 발견 | Known Issues |
| 기술 부채 인지 | Technical Debt |
| 범위 변경 | Next Tasks, MVP Progress |

---

## 8. Git Commit 규칙

### 형식

```
<type>: <description>

[optional body]
```

### Type

| Type | 사용 시점 |
|------|-----------|
| `feat` | 새 기능 |
| `fix` | 버그 수정 |
| `docs` | 문서만 변경 |
| `refactor` | 동작 변경 없는 코드 개선 |
| `chore` | 빌드, 설정, 의존성 |

### 규칙

- 한 커밋에 한 가지 변경만 포함한다
- description은 명령형 현재형 (`add login`, `fix null check`)
- 50자 이내로 작성한다
- Breaking change는 body에 `BREAKING:` 명시

### 예시

```
feat: add email login form

fix: prevent duplicate submission on signup

docs: update environment variable list
```

---

## 9. CHANGELOG 작성 규칙

[Keep a Changelog](https://keepachangelog.com/ko/1.1.0/) 형식을 따른다.

### 구조

```markdown
## [1.1.0] - 2026-06-29

### Added
- 새로 추가된 기능

### Changed
- 기존 기능의 변경

### Fixed
- 버그 수정

### Removed
- 제거된 기능
```

### 규칙

- 날짜는 ISO 8601 (`YYYY-MM-DD`)
- 사용자 관점에서 작성한다 (내부 리팩터링은 Changed에 간략히)
- Unreleased 섹션에 진행 중인 변경을 먼저 기록한다
- Release 시 Unreleased 내용을 버전 섹션으로 이동한다

---

## 10. AI 작업 규칙

Cursor, ChatGPT 등 AI 도구 사용 시 추가 규칙이다.

### 역할 분리

| 도구 | 역할 | 출력물 |
|------|------|--------|
| ChatGPT | CTO | Cursor Task (구현 안 함) |
| Cursor | Senior Engineer | 코드, 문서, 커밋·Push |

### ChatGPT (CTO) 규칙

- 모든 출력은 Cursor가 바로 실행할 수 있는 **Task 형태**로 작성한다
- 제안만 하고 끝내지 않는다 — 구조 변경, 리팩토링, 폴더 변경도 Task로 생성한다
- 기능 개발 중 적극적으로 개선사항을 찾고 Task로 만든다
- QA와 UX 개선을 기능 완성까지 반복한다
- "충분히 좋다"는 수준에 도달하면 Release를 권장한다
- `VISION.md`와 `PROJECT_STATE.md`를 컨텍스트로 활용한다

### Cursor (Engineer) 규칙

- ChatGPT Task만 구현한다 (Task 없는 대규모 변경 금지)
- 작업 전 `VISION.md`와 `PROJECT_STATE.md`를 읽는다
- 작업 후 문서를 업데이트하고 커밋·Push한다
- AI가 생성한 코드에도 동일한 품질 기준을 적용한다

### 공통

- AI 프롬프트 자체도 리뷰 대상이다 (`.ai/prompts/REVIEW.md`의 AI Prompt Quality)
- CEO(사용자)가 최종 의사결정한다

---

## 11. 개발 프로세스 원칙

기능 하나를 완성할 때 따르는 원칙이다.

1. **빠른 검증 우선** — 개발 중에는 완벽한 코드보다 MVP 검증을 우선한다
2. **UX 최우선** — 기능보다 사용자 경험을 우선한다
3. **반복 QA·UX** — Release 전까지 QA와 UX 개선을 반복한다
4. **Task 기반 소통** — ChatGPT ↔ Cursor 간 모든 지시는 Task 형태다
5. **충분히 좋으면 출시** — 과도한 완벽주의보다 적시 Release를 권장한다

---

## 우선순위 충돌 시

규칙이 충돌할 때 다음 순서로 판단한다.

```
1. 사용자 안전 (Security)
2. 사용자 경험 (UX)
3. 유지보수성 (Maintainability)
4. 개발 속도 (Speed)
5. 기능 완성도 (Feature completeness)
```
