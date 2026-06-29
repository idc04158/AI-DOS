# AI-DOS

**AI Development Operating System v1.1** — 모든 SaaS 프로젝트의 공통 개발 운영체계

---

## AI-DOS란?

AI-DOS는 SaaS 제품이 아니다. **문서와 개발 프로세스를 관리하는 표준 저장소**다.

규칙·템플릿·프롬프트를 프로젝트에 복사하면 CEO, ChatGPT(CTO), Cursor가 동일한 방식으로 협업한다.

| 역할 | 도구 | 담당 |
|------|------|------|
| CEO | — | 기능 요구사항, 우선순위 |
| ChatGPT (CTO) | ChatGPT | Task, 리뷰, QA, Release (구현 안 함) |
| Cursor | Cursor | 구현, 테스트, 문서, Commit·Push, 자율 QA·UI/UX |

---

## 개발 사이클

```
Feature Sprint → QA Sprint (자동) → UI/UX Sprint (자동) → Release
```

### Feature Sprint

```
ChatGPT Task → Cursor 구현 → 테스트 → Commit → Push
→ ChatGPT 리뷰 → Cursor 수정 → Release
```

### QA Sprint (자동)

새 기능 없음. 우선순위 기반 자율 수정. **User Experience QA** 필수 — 매 Commit마다 앱 실행·화면 확인.

### UI/UX Sprint (자동)

새 기능 없음. 판매 가능한 SaaS 수준까지 UI/UX·산출물 품질 개선.

### 자동 진행

승인 질문 없이 연속 작업. Commit 후 5항목만 보고하고 **스스로** 다음 작업 시작.

상세: [docs/WORKFLOW.md](docs/WORKFLOW.md)

---

## 철학

1. **단순함** — 실행 가능한 최소 규칙
2. **문서 우선** — 코드와 문서 항상 동기화
3. **SaaS 품질** — 개발용 툴이 아닌 판매 가능한 제품
4. **자율 운영** — Cursor가 CTO 관리 아래 스스로 QA·UI/UX 수행
5. **프로젝트 비침범** — `.ai/` 네임스페이스로 경로 분리

---

## Repository 구조

```
AI-DOS/
├── README.md
├── CHANGELOG.md
├── docs/
│   ├── WORKFLOW.md
│   ├── DEVELOPMENT_RULES.md
│   └── VERSIONING.md
├── templates/
├── .ai/prompts/
│   ├── FEATURE_REQUEST.md
│   ├── REVIEW.md
│   ├── QA.md
│   └── RELEASE.md
└── .cursor/rules/ai-dos.mdc
```

> 프로젝트 AI Prompt는 `prompts/`, AI-DOS는 `.ai/prompts/` — 경로 충돌 없음.

---

## 프로젝트 적용

```bash
mkdir -p my-project/.cursor/rules
cp AI-DOS/.cursor/rules/ai-dos.mdc my-project/.cursor/rules/
cp AI-DOS/templates/VISION.md my-project/
cp AI-DOS/templates/PROJECT_STATE.md my-project/
cp AI-DOS/templates/CHANGELOG.md my-project/
cp -r AI-DOS/.ai my-project/
```

PowerShell:

```powershell
New-Item -ItemType Directory -Force -Path my-project\.cursor\rules
Copy-Item AI-DOS\.cursor\rules\ai-dos.mdc my-project\.cursor\rules\
Copy-Item AI-DOS\templates\VISION.md, AI-DOS\templates\PROJECT_STATE.md, AI-DOS\templates\CHANGELOG.md my-project\
Copy-Item AI-DOS\.ai my-project\.ai -Recurse
```

### 필수 문서

1. `VISION.md` — 제품 방향
2. `README.md` — 실행 방법
3. `PROJECT_STATE.md` — Sprint Mode, 진행률
4. `CHANGELOG.md` — 변경 이력

### AI-DOS 업데이트 (기존 프로젝트)

```bash
cp AI-DOS/.cursor/rules/ai-dos.mdc my-project/.cursor/rules/
cp -r AI-DOS/.ai my-project/
# VISION.md, PROJECT_STATE.md는 덮어쓰지 않음
```

---

## 버전

| 버전 | 내용 |
|------|------|
| v1.0.x | 초기 안정화 (역할, Sprint, 자율 QA) |
| **v1.1** | 자율 개발 워크플로우 통합, UX QA, SaaS 품질 기준 (현재) |

[docs/VERSIONING.md](docs/VERSIONING.md) · [CHANGELOG.md](CHANGELOG.md)

---

## 관련 문서

- [Workflow](docs/WORKFLOW.md)
- [Development Rules](docs/DEVELOPMENT_RULES.md)
- [Versioning](docs/VERSIONING.md)

---

## 라이선스

문서와 템플릿은 자유롭게 복사·수정하여 사용할 수 있다.
