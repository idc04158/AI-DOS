# Versioning

AI-DOS 저장소와 AI-DOS 기반 프로젝트의 버전 정책이다.

---

## AI-DOS 버전 정책

AI-DOS는 [Semantic Versioning](https://semver.org/)을 따른다.

```
v<MAJOR>.<MINOR>.<PATCH>
```

| 구분 | 변경 시점 | 예시 |
|------|-----------|------|
| MAJOR | 구조 변경, 워크플로우 개편, 하위 호환 없음 | v1.0 → v2.0 |
| MINOR | 문서 보완, 템플릿 추가, 프롬프트 개선 | v1.0 → v1.1 |
| PATCH | 오타 수정, 표현 개선, 사소한 수정 | v1.0.0 → v1.0.1 |

---

## 버전 히스토리

### v1.1.0 (2026-06-29) — 현재

자율 개발 워크플로우 통합. 모든 SaaS 프로젝트 공통 운영체계.

**핵심 변경:**

- Feature → QA → UI/UX Sprint 자동 사이클
- User Experience QA (실행·화면 검증) 필수
- SaaS 품질 기준, High Priority Bug 정의
- 자동 진행 (승인 질문 금지), 5항목 보고
- 중단 조건: CTO Task, 아키텍처 변경, 외부 결정

**마이그레이션 (v1.0.x → v1.1):**

```bash
cp AI-DOS/.cursor/rules/ai-dos.mdc my-project/.cursor/rules/
cp -r AI-DOS/.ai my-project/
# PROJECT_STATE.md에 Sprint Progress, High Priority Bugs 필드 추가
# VISION.md 등 프로젝트 고유 문서는 유지
```

```powershell
Copy-Item AI-DOS\.cursor\rules\ai-dos.mdc my-project\.cursor\rules\ -Force
Copy-Item AI-DOS\.ai my-project\.ai -Recurse -Force
```

### v1.0.x (2026-06-29)

v1.0.0–v1.0.5 단계적 안정화. v1.1에서 통합.

- v1.0.0: 최초 안정 버전
- v1.0.1: `.ai/prompts/` 경로 분리
- v1.0.2–v1.0.3: 역할 정의, Feature/QA Sprint
- v1.0.4–v1.0.5: UI/UX Sprint, 연속 자율 작업

### v1.0.1 (2026-06-29)

프로젝트 AI Prompt와의 경로 충돌 방지.

**변경 내용:**

- `prompts/` → `.ai/prompts/`로 이동
- AI-DOS 관련 파일을 `.ai/` 네임스페이스로 분리
- 모든 문서 경로 참조 업데이트

**마이그레이션 (v1.0.0 → v1.0.1):**

```bash
# 기존 prompts/ 제거 후 .ai/ 복사
rm -rf prompts/
cp -r AI-DOS/.ai my-project/
```

```powershell
Remove-Item my-project\prompts -Recurse -Force -ErrorAction SilentlyContinue
Copy-Item AI-DOS\.ai my-project\.ai -Recurse -Force
```

### v1.0.0 (2026-06-29)

최초 안정 버전.

**포함 내용:**

- README, WORKFLOW, DEVELOPMENT_RULES, VERSIONING
- 템플릿 4종 (PROJECT_STATE, CHANGELOG, VISION, README_TEMPLATE)
- 프롬프트 4종 (`.ai/prompts/` — v1.0.1부터)
- Cursor 규칙 (ai-dos.mdc)

---

## 업데이트 원칙

### MINOR 업데이트 (v1.x)

다음에 해당하면 MINOR 버전을 올린다.

- 새 템플릿 추가
- 기존 프롬프트 개선 (출력 형식 변경 없음)
- 문서 보완 (구조 변경 없음)
- Cursor 규칙 항목 추가 (기존 규칙 변경 없음)

**하위 호환:** 기존 프로젝트는 선택적으로 반영한다. 강제 업데이트 없음.

### MAJOR 업데이트 (v2.0)

다음에 해당하면 MAJOR 버전을 올린다.

- 디렉터리 구조 변경
- 워크플로우 단계 추가/제거/순서 변경
- 템플릿 필수 항목 변경
- 프롬프트 출력 형식 변경
- Cursor 규칙의 기존 항목 수정 또는 삭제

**하위 호환:** 기존 프로젝트는 수동으로 마이그레이션한다. CHANGELOG에 마이그레이션 가이드를 포함한다.

### PATCH 업데이트 (v1.0.x)

- 오타, 문법 수정
- 예시 코드 수정
- 링크 수정

---

## 프로젝트 버전 정책

AI-DOS를 적용한 **개별 프로젝트**는 자체 버전을 관리한다.

### 규칙

- `CHANGELOG.md`가 버전의 단일 진실 공급원(Single Source of Truth)
- Git 태그는 CHANGELOG 버전과 일치
- Release 전에 CHANGELOG의 Unreleased를 버전 섹션으로 이동

### 권장 형식

```
v0.1.0  — MVP 초기 출시
v0.x.x  — MVP 단계 개선
v1.0.0  — 정식 출시 (MVP 완료)
v1.x.x  — 정식 출시 후 개선
v2.0.0  — 주요 변경 (Breaking change)
```

### MVP 단계 (v0.x)

- 기능 추가가 빈번하므로 MINOR를 자유롭게 올린다
- Breaking change가 있어도 v0.x 내에서는 MAJOR를 올리지 않는다
- v1.0.0은 CEO + CTO가 `.ai/prompts/RELEASE.md`로 판단한다

---

## AI-DOS 업데이트 반영 방법

기존 프로젝트에 AI-DOS 업데이트를 반영할 때:

### MINOR / PATCH

1. 변경된 파일만 선택적으로 복사
2. `CHANGELOG.md`에 "AI-DOS v1.x 반영" 기록
3. 프로젝트 고유 문서(VISION, PROJECT_STATE)는 덮어쓰지 않는다

### MAJOR

1. AI-DOS CHANGELOG에서 마이그레이션 가이드 확인
2. 구조 변경에 맞게 프로젝트 디렉터리 조정
3. Cursor 규칙 수동 병합
4. `CHANGELOG.md`에 마이그레이션 기록

---

## 버전 결정 체크리스트

AI-DOS 저장소를 업데이트할 때:

- [ ] 구조가 바뀌었는가? → MAJOR
- [ ] 워크플로우가 바뀌었는가? → MAJOR
- [ ] 새 템플릿/프롬프트가 추가되었는가? → MINOR
- [ ] 기존 문서가 보완되었는가? → MINOR
- [ ] 오타만 수정되었는가? → PATCH
