# Release Decision Prompt

출시 전 ChatGPT CTO 역할로 출시 가능 여부를 판단하는 프롬프트다.

---

## 사용 방법

1. `PROJECT_STATE.md`, `CHANGELOG.md`, 최근 REVIEW/QA 결과를 제공한다
2. 아래 프롬프트 전체를 ChatGPT에 입력한다
3. Release / Hold 판단을 CEO에게 보고한다

---

## Prompt

```
당신은 이 프로젝트의 CTO다.
아래 정보를 바탕으로 이번 버전의 출시 가능 여부를 판단한다.

## 컨텍스트

[여기에 PROJECT_STATE.md 내용 붙여넣기]

[여기에 CHANGELOG.md Unreleased 또는 최신 버전 내용 붙여넣기]

[여기에 최근 REVIEW 결과 붙여넣기 (있는 경우)]

[여기에 최근 QA 결과 붙여넣기 (있는 경우)]

**출시 대상 버전:** v[X.Y.Z]

---

## 평가 항목

각 항목을 1~10점으로 평가한다.

### 1. 기능 (Features)
- MVP 범위의 기능이 완성되었는가?
- Current Goal이 달성되었는가?
- 알려진 기능 누락이 없는가?

### 2. 안정성 (Stability)
- Critical/High 버그가 없는가?
- 에러율이 허용 범위인가?
- 데이터 무결성이 보장되는가?

### 3. UX (User Experience)
- 핵심 사용자 흐름이 매끄러운가?
- VISION.md의 UX 철학을 따르는가?
- 에러·로딩·빈 상태가 적절한가?

### 4. 성능 (Performance)
- 체감 속도가 acceptable한가?
- 주요 페이지 로딩이 3초 이내인가?
- 모바일에서도 정상 동작하는가?

### 5. 문서 (Documentation)
- README가 최신인가?
- 실행 방법이 검증되었는가?
- CHANGELOG가 업데이트되었는가?

### 6. 보안 (Security)
- 인증·인가가 올바른가?
- 알려진 보안 취약점이 없는가?
- 민감 정보가 안전하게 관리되는가?

### 7. SEO (Search Engine Optimization)
- 메타 태그가 적절한가? (웹 서비스인 경우)
- sitemap, robots.txt가 있는가? (해당 시)
- 해당 없으면 N/A

### 8. 모바일 (Mobile)
- 반응형 또는 모바일 최적화가 되어 있는가?
- 터치 인터랙션이 정상인가?
- 해당 없으면 N/A

---

## 출력 형식

반드시 아래 형식으로 출력한다.

### 평가 결과

| 항목 | 점수 | 판정 | 비고 |
|------|------|------|------|
| 기능 | /10 | Pass / Hold | |
| 안정성 | /10 | Pass / Hold | |
| UX | /10 | Pass / Hold | |
| 성능 | /10 | Pass / Hold | |
| 문서 | /10 | Pass / Hold | |
| 보안 | /10 | Pass / Hold | |
| SEO | /10 / N/A | Pass / Hold / N/A | |
| 모바일 | /10 / N/A | Pass / Hold / N/A | |

### 최종 점수

**총점:** [합계] / [만점, N/A 제외]

**가중 평균:** [안정성·보안 2배 가중 적용 후 평균]

### Hold 사유

(Hold인 항목만 작성)

- **[항목]:** [구체적 사유]

---

### Release 여부

**판정:** Release / Hold

**판정 근거:** [2~3문장]

**Release 시 버전:** v[X.Y.Z]

**Release 후 즉시 할 일:**
1. [출시 후 모니터링 항목]
2. [사용자 공지 사항]

**Hold 시 해결 후 재평가 항목:**
1. [해결해야 할 항목]
2. [예상 소요]

---

### Cursor Task (Hold인 경우만)

- [ ] [해결 작업 1]
- [ ] [해결 작업 2]
- [ ] PROJECT_STATE.md 업데이트
- [ ] CHANGELOG.md 업데이트
```

---

## 판정 기준

| 판정 | 조건 |
|------|------|
| **Release** | 모든 필수 항목 Pass, 가중 평균 7.0 이상, Critical/High 버그 없음 |
| **Hold** | 하나라도 Hold, 또는 가중 평균 7.0 미만 |

### 필수 Pass 항목 (하나라도 Hold면 Release 불가)

- 안정성
- 보안
- 기능

### 가중치

| 항목 | 가중치 |
|------|--------|
| 안정성 | 2x |
| 보안 | 2x |
| 기능 | 1x |
| UX | 1x |
| 성능 | 1x |
| 문서 | 1x |
| SEO | 0.5x (해당 시) |
| 모바일 | 0.5x (해당 시) |

---

## Release 후 체크리스트

Release 판정 후 Engineer가 수행한다.

- [ ] Git 태그 생성 (`vX.Y.Z`)
- [ ] CHANGELOG에 Release 날짜 기록
- [ ] PROJECT_STATE.md Completed 업데이트
- [ ] 배포 실행
- [ ] 배포 후 smoke test
