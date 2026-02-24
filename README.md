# Cross-Verify Plugin

개발자 참여형 교차 검증 에이전트 — 의사결정·설계·문서·구현 4축 통합 검증.

## 개요

자동화가 아닌, **개발자가 의도적으로 멈추고 확인하는 행위를 구조화한 플러그인**.
lint, test, CI가 잡지 못하는 "의미적 판단" 영역에 집중합니다.

### 4축 검증 체계

| 축 | 핵심 질문 | 산출물 |
|----|----------|--------|
| 의사결정 | 왜 이 선택인가? 대안은 검토했는가? | 의사결정 근거 정리 |
| 설계 검증 | 엣지 케이스는? 정책과 일치하는가? | 검증 포인트 목록 |
| 문서 품질 | 가독성은? 톤은 적절한가? 정보 유실은 없는가? | 문서 품질 리포트 |
| 구현 검증 | 코드가 설계를 정확히 반영하는가? | 정합성 체크 결과 |

---

## 설치

### 마켓플레이스 설치 (권장)

```bash
# Claude Code 실행 후

# 1. 마켓플레이스 추가
/plugin marketplace add https://github.nhnent.com/dongyoung-kim/claude-cross-verify-plugin.git

# 2. 플러그인 설치
/plugin install cross-verify
```

### settings.json 직접 등록

```json
{
  "extraKnownMarketplaces": {
    "cross-verify-plugin": "https://github.nhnent.com/dongyoung-kim/claude-cross-verify-plugin.git"
  },
  "plugins": {
    "cross-verify": {
      "marketplace": "cross-verify-plugin",
      "enabled": true
    }
  }
}
```

---

## 사용법

### 전체 검증 (4축 모두)

```
/cross-verify:verify

# 또는 트리거 키워드
"교차 검증해줘"
"이거 크로스 체크"
"cross verify this"
```

### 부분 검증 (특정 축만)

```
"의사결정만 검증해줘"    → 축 1만 실행
"설계 검증"              → 축 2만 실행
"문서 품질 확인"         → 축 3만 실행
"구현 검증"              → 축 4만 실행
```

### 입력 예시

```
# 파일 대상
/cross-verify:verify src/auth/login.ts

# 커밋 범위
"최근 커밋 교차 검증해줘"

# 설계 문서
"이 설계 문서 크로스 체크" + 위키 URL

# 자유 설명
"Redis 대신 in-memory 캐시를 선택한 결정을 검증해줘"
```

---

## 핵심 원칙

- **자동 수정 금지** — 판단은 개발자에게
- **검증 과정을 개발자가 함께 보는 것이 목적**
- **기존 도구(Ralph, lint, test)와 보완 관계**
- **도구 측정 불가능한 "의미적 판단"에 집중**

---

## 구조

```
claude-cross-verify-plugin/
├── .claude-plugin/
│   ├── plugin.json            # 플러그인 메타데이터
│   └── marketplace.json       # 마켓플레이스 레지스트리
├── agents/
│   └── cross-verifier.md     # 교차 검증 에이전트 정의
└── skills/
    └── verify/
        └── SKILL.md           # 4축 통합 검증 스킬
```

---

## 배경

위키 "[AI 협업에서의 교차 검증 — 속도가 아닌 판단을 자동화하기](https://nhnent.dooray.com)"에서 정의한 사상을 Claude Code 플러그인으로 구현한 것입니다.
