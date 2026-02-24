---
name: setup
description: 프로젝트별 교차 검증 설정 초기화. .claude/cross-verify.json 생성.
---

# 교차 검증 설정 초기화

현재 프로젝트에 교차 검증 설정을 초기화합니다.

## 트리거

- "교차 검증 설정", "verify setup"
- verify 스킬 실행 시 설정 파일이 없으면 안내

## 절차

### 1. 기존 설정 확인

```bash
test -f .claude/cross-verify.json && echo "EXISTS" || echo "NOT_EXISTS"
```

이미 존재하면 사용자에게 알리고 덮어쓸지 확인.

### 2. 프로젝트 컨텍스트 탐색

자동으로 프로젝트 내 관련 파일을 탐색:

```bash
# 컨벤션 문서
ls CONVENTIONS.md CONTRIBUTING.md docs/CONVENTIONS.md docs/conventions.md 2>/dev/null

# ADR (Architecture Decision Records)
ls -d docs/adr/ docs/ADR/ adr/ ADR/ docs/decisions/ 2>/dev/null

# 테스트 설정
ls jest.config.* vitest.config.* .mocharc.* pytest.ini setup.cfg tox.ini 2>/dev/null

# 프로젝트 유형 감지
ls package.json pom.xml build.gradle go.mod Cargo.toml requirements.txt pyproject.toml 2>/dev/null
```

### 3. 사용자에게 질문

AskUserQuestion으로 다음을 확인:

**질문 1: 컨벤션 문서**
- 탐색된 파일이 있으면 제시하고 선택
- 없으면 경로 또는 위키 URL 입력 요청
- "없음"도 가능

**질문 2: 설계 의사결정 기록 위치**
- ADR 디렉토리가 있으면 제시
- 위키 URL 입력 가능
- "없음"도 가능

**질문 3: 중점 검증 축**
- 4축 중 이 프로젝트에서 특히 중요한 축 선택 (복수 선택 가능)
- 기본값: 전체

**질문 4: 프로젝트별 커스텀 체크 (선택)**
- 이 프로젝트에서 항상 확인해야 할 사항이 있는지
- 예: "모든 API 호출에 timeout 설정 확인", "DB 쿼리에 인덱스 사용 여부"
- 건너뛰기 가능

### 4. 설정 파일 생성

`.claude/cross-verify.json` 생성:

```json
{
  "version": "1.0",
  "conventions": "docs/CONVENTIONS.md",
  "decisionLog": "https://nhnent.dooray.com/wiki/...",
  "focusAxes": ["설계", "구현"],
  "customChecks": {
    "의사결정": [],
    "설계": ["API 호출 시 retry/timeout 패턴 적용 여부"],
    "문서": [],
    "구현": ["하드코딩된 환경별 값이 없는지 확인"]
  }
}
```

**필드 설명:**

| 필드 | 타입 | 필수 | 설명 |
|------|------|------|------|
| `version` | string | O | 설정 스키마 버전 |
| `conventions` | string/null | X | 컨벤션 문서 경로 또는 URL |
| `decisionLog` | string/null | X | ADR/설계문서 위치 |
| `focusAxes` | string[] | X | 중점 축 (`의사결정`, `설계`, `문서`, `구현`). 생략 시 전체 |
| `customChecks` | object | X | 축별 커스텀 체크 항목 |

### 5. .gitignore 확인

```bash
# 개인 설정이므로 git에 포함하지 않음
grep -q 'cross-verify.json' .gitignore 2>/dev/null || echo '.claude/cross-verify.json' >> .gitignore
```

> `.claude/` 전체가 이미 gitignore 되어 있다면 별도 추가 불필요.

### 6. 완료 메시지

```
교차 검증 설정 완료.

설정 파일: .claude/cross-verify.json
- 컨벤션: {conventions 값}
- 설계문서: {decisionLog 값}
- 중점 축: {focusAxes 값}
- 커스텀 체크: {개수}개

이제 /verify 실행 시 프로젝트 맥락이 반영됩니다.
설정 변경: /setup 재실행 또는 .claude/cross-verify.json 직접 편집
```
