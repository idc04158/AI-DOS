# Changelog

AI-DOS 저장소의 변경 이력이다.

형식은 [Keep a Changelog](https://keepachangelog.com/ko/1.1.0/)를 따르고,  
버전은 [Semantic Versioning](https://semver.org/)을 따른다.

---

## [1.0.3] - 2026-06-29

### Changed

- Feature Sprint / QA Sprint 개발 모드 도입
- QA Sprint를 Cursor 자율 워크플로우로 변경
  - 프로젝트 전체 분석 후 영향도 기반 우선순위 8단계 적용
  - 문제당 수정 → 테스트 → 문서 → Commit & Push 반복
  - 종료 조건 충족까지 자동 진행, 중단 질문 금지
- ChatGPT(CTO)가 QA Sprint 중 언제든 Task 추가 가능, 없으면 Cursor 자율 진행
- README, WORKFLOW, DEVELOPMENT_RULES, ai-dos.mdc, PROJECT_STATE 템플릿 업데이트

---

## [1.0.2] - 2026-06-29

### Changed

- CEO, ChatGPT(CTO), Cursor(Senior Engineer) 역할 정의 명확화
- 반복 개발 프로세스 도입 (기능 요청 → Task → 구현 → 리뷰 → QA → UX → Release)
- README, WORKFLOW, DEVELOPMENT_RULES, ai-dos.mdc에 역할·프로세스 반영
- ChatGPT는 Task만 생성하고 구현하지 않음을 명시
- Cursor가 커밋·Push를 담당함을 명시

---

## [1.0.1] - 2026-06-29

### Changed

- `prompts/` → `.ai/prompts/`로 이동하여 프로젝트 AI Prompt와 경로 분리
- AI-DOS 관련 파일을 `.ai/` 네임스페이스로 분리
- 모든 문서 경로 참조 업데이트

---

## [1.0.0] - 2026-06-29

### Added

- README, WORKFLOW, DEVELOPMENT_RULES, VERSIONING
- 템플릿 4종 (PROJECT_STATE, CHANGELOG, VISION, README_TEMPLATE)
- 프롬프트 4종 (.ai/prompts/)
- Cursor 규칙 (ai-dos.mdc)
