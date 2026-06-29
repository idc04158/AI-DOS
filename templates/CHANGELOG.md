# Changelog

이 프로젝트의 모든 주요 변경 사항을 기록한다.

형식은 [Keep a Changelog](https://keepachangelog.com/ko/1.1.0/)를 따르고,  
버전은 [Semantic Versioning](https://semver.org/)을 따른다.

---

## [Unreleased]

### Added

- 

### Changed

- 

### Fixed

- 

### Removed

- 

---

## [0.1.0] - YYYY-MM-DD

### Added

- 초기 프로젝트 설정
- VISION.md, PROJECT_STATE.md, README.md 작성

---

## 작성 가이드

### 섹션 설명

| 섹션 | 사용 시점 |
|------|-----------|
| **Added** | 새 기능, 새 파일, 새 API |
| **Changed** | 기존 기능의 동작 변경 |
| **Fixed** | 버그 수정 |
| **Removed** | 기능·파일·API 제거 |

### 규칙

- 사용자 관점에서 작성한다
- 내부 리팩터링은 Changed에 간략히 기록한다
- Breaking change는 Changed에 `**Breaking:**` 접두사를 붙인다
- Release 시 Unreleased 내용을 새 버전 섹션으로 이동하고 Unreleased를 비운다

### 예시

```markdown
## [1.2.0] - 2026-07-15

### Added
- 이메일 로그인 기능
- 비밀번호 재설정 페이지

### Changed
- 로그인 폼 UI 개선
- **Breaking:** API `/auth/login` 응답 형식 변경

### Fixed
- 회원가입 시 중복 제출 방지
- 모바일에서 메뉴가 잘리는 문제

### Removed
- 더 이상 사용하지 않는 `/legacy/login` 엔드포인트
```
