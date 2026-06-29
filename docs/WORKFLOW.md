# Workflow

AI-DOS v1.1 개발 흐름. 모든 SaaS 프로젝트의 공통 운영체계다.

---

## 역할

### CEO (사용자)

- 기능 요구사항 정의
- 우선순위 결정

### ChatGPT (CTO)

- 기능을 Cursor **Task**로 분해
- 코드 리뷰, QA, UX/UI 리뷰
- SaaS 품질 향상, Release 판단
- **구현하지 않음**

### Cursor (Senior Engineer)

- 구현, 테스트, 버그 수정, 리팩토링
- 문서 관리, Commit, Push
- CTO 관리 아래 **자율적** QA·UI/UX 수행

---

## 전체 사이클

```
Feature Sprint
  ↓ (자동)
QA Sprint
  ↓ (자동)
UI/UX Sprint
  ↓
Release (ChatGPT 선언)
```

`PROJECT_STATE.md`의 **Sprint Mode**로 현재 단계를 기록한다.

---

## Feature Sprint

```
기능 요청 (CEO)
  ↓
ChatGPT — Task
  ↓
Cursor — 구현 → 자체 테스트 → Commit → Push
  ↓
ChatGPT — 리뷰
  ↓
Cursor — 수정
  ↓
Release (ChatGPT 선언)
```

Feature Sprint 종료 후 **자동으로 QA Sprint** 시작.

---

## QA Sprint

- **새 기능 추가 금지**
- 영향도가 가장 큰 문제부터 반복 수정
- **사용자 승인 없이** 연속 작업

### 우선순위

1. Runtime Crash
2. 기능 오류
3. 예외 처리
4. 데이터 무결성
5. UX 개선
6. UI 개선
7. 성능 개선
8. 리팩토링
9. 유지보수성

### 작업 루프

```
문제 선택 → 구현 → 테스트(실행·화면 확인) → Commit → Push
→ PROJECT_STATE · CHANGELOG 업데이트 → 재분석 → 반복
```

### User Experience QA (필수)

매 Commit마다 앱을 **실행**하고 주요 화면을 확인한다. 코드 리뷰만으로 통과시키지 않는다.

| 검사 항목 | High Priority Bug |
|-----------|-------------------|
| HTML/JSON/Markdown/Debug/Stack Trace 노출 | ✓ |
| Placeholder, 테스트 데이터, TODO, 개발자 용어 노출 | ✓ |
| 버튼·입력 비정상, 화면·레이아웃 깨짐 | ✓ |
| 글자 잘림, 여백 이상, 색상 불일치, 모바일 비정상 | ✓ |
| Loading / Empty / Error 화면 부자연스러움 | ✓ |

### SaaS 품질 기준

> "처음 보는 사용자가 실제 돈을 내고 쓸 SaaS처럼 느끼는가?"

아니면 개선한다. 디자인·UX·산출물 품질도 QA 대상이다.

### QA Sprint 종료 조건

- Runtime Crash 없음
- High Priority Bug 없음
- 주요 사용자 흐름 정상
- Error Handling, Loading/Empty/Error State 완료

종료 후 **자동으로 UI/UX Sprint** 시작.

---

## UI/UX Sprint

- **새 기능 추가 금지**
- QA Sprint와 동일 작업 루프
- 목표: **판매 가능한 SaaS** 수준 (개발용 툴 수준 탈피)

### 개선 항목

사용자 흐름, 정보 구조, 버튼 위치, 여백, 타이포그래피, 디자인 일관성, 컬러 시스템, 반응형, 접근성, Loading/Empty/Error UX, **핵심 산출물 디자인 품질**

실제 사용자 경험이 개선될 때만 적용한다.

---

## 자동 진행 정책

### 금지 질문

- "계속할까요?"
- "다음 작업을 진행할까요?"
- "다음 Sprint를 진행할까요?"
- "이 항목을 수정할까요?"

### 중단 조건

1. ChatGPT(CTO) 새 Task 전달
2. 프로젝트 전체 아키텍처 변경 필요
3. 외부 API 또는 사용자(CEO) 결정 필요

그 외에는 **계속 개선**한다. CTO Task 처리 후 자율 루프 재개.

### Commit 후 보고 (5항목만)

| # | 항목 |
|---|------|
| 1 | 수정 내용 |
| 2 | Commit 해시 |
| 3 | Push 완료 여부 |
| 4 | 남아있는 High Priority Bug 개수 |
| 5 | 현재 Sprint 진행률(%) |

다음 작업 제안·승인 요청 금지. **스스로 다음 작업 시작.**

---

## 작업 종료 조건

- 기능 구현 완료
- QA Sprint 완료
- UI/UX Sprint 완료
- Runtime Crash·High Priority Bug 없음
- HTML/JSON/Debug 노출 없음
- UX·UI 일관성, Loading/Empty/Error UX 완료
- 문서·Commit·Push 완료

---

## ChatGPT(CTO) 프롬프트

| 용도 | 파일 |
|------|------|
| 기능 Task | `.ai/prompts/FEATURE_REQUEST.md` |
| 리뷰 | `.ai/prompts/REVIEW.md` |
| QA | `.ai/prompts/QA.md` |
| Release | `.ai/prompts/RELEASE.md` |

---

## GitHub

Cursor가 Commit·Push 담당. 형식: `feat | fix | docs | refactor | chore`
