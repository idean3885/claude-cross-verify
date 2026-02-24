# Cross-Verify Plugin

개발자 참여형 교차 검증 에이전트 — 의사결정·설계·문서·구현 4축 통합 검증.

## 왜 "교차" 검증인가

내 작업이 진짜 맞는지, 다른 시선으로 다시 확인하는 것 — 교차 검증의 출발점입니다.

AI 코딩 도구는 코드 작성을 빨리 해줍니다. 하지만 그로 인해 오히려 "이게 맞는가?"라는 질문을 더 많이 던지게 됩니다. 기존 자동화 도구(Ralph, BMAD, Agent Teams)는 **AI끼리 검증**합니다. 이 플러그인은 정반대 — **AI의 판단을 개발자가 중간 검문**합니다.

이 "교차"는 두 층위로 확장됩니다:

**1. 4축 × 동일 대상** — 하나의 작업을 4가지 관점에서 교차로 봅니다.

| 축 | 핵심 질문 | 산출물 |
|----|----------|--------|
| 의사결정 | 왜 이 선택인가? 대안은 검토했는가? | 의사결정 근거 정리 |
| 설계 검증 | 엣지 케이스는? 정책과 일치하는가? | 검증 포인트 목록 |
| 문서 품질 | 가독성은? 톤은 적절한가? 정보 유실은 없는가? | 문서 품질 리포트 |
| 구현 검증 | 코드가 설계를 정확히 반영하는가? | 정합성 체크 결과 |

**2. AI 판단 × 개발자 판단** — AI가 확인하는 과정을 개발자가 함께 봅니다.

> 자동화된 검증은 "맞는지 틀린지"를 판별합니다.
> 교차 검증은 "왜 맞고 왜 틀린지"를 개발자가 함께 이해하는 과정입니다.

---

## 핵심 원칙

- **자동 수정 금지** — 판단은 개발자에게
- **검증 과정을 개발자가 함께 보는 것이 목적**
- **기존 도구(Ralph, lint, test)와 보완 관계**
- **도구 측정 불가능한 "의미적 판단"에 집중**

---

## 설치

```bash
# 1. 마켓플레이스 추가
/plugin marketplace add https://github.nhnent.com/dongyoung-kim/claude-cross-verify-plugin.git

# 2. 플러그인 설치
/plugin install cross-verify
```

> **자동완성 참고**: Claude Code CLI 버그로 플러그인 스킬이 `/` 자동완성에 표시되지 않을 수 있습니다.
> 워크어라운드: `~/.claude/skills/`에 심링크 생성
> ```bash
> mkdir -p ~/.claude/skills
> ln -sf ../plugins/marketplaces/claude-cross-verify-plugin/skills/verify ~/.claude/skills/verify
> ln -sf ../plugins/marketplaces/claude-cross-verify-plugin/skills/setup ~/.claude/skills/verify-setup
> ```

---

## 사용법

### 프로젝트 설정 (선택)

```
/verify-setup
```

프로젝트별 컨벤션, 설계문서 위치, 중점 축을 `.claude/cross-verify.json`에 설정합니다.
설정 없이도 범용 검증이 가능합니다. 설정이 있으면 프로젝트 맥락이 반영됩니다.

### 전체 검증 (4축 모두)

```
/verify

# 또는 트리거 키워드
"교차 검증해줘"
"이거 크로스 체크"
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
/verify src/auth/login.ts
"최근 커밋 교차 검증해줘"
"이 설계 문서 크로스 체크" + 위키 URL
"Redis 대신 in-memory 캐시를 선택한 결정을 검증해줘"
```

---

## 프로젝트별 설정

`/verify-setup` 실행 시 `.claude/cross-verify.json`이 생성됩니다:

```json
{
  "version": "1.0",
  "conventions": "docs/CONVENTIONS.md",
  "decisionLog": "https://nhnent.dooray.com/wiki/.../ADR",
  "focusAxes": ["설계", "구현"],
  "customChecks": {
    "설계": ["API 호출 시 retry/timeout 패턴 적용 여부"],
    "구현": ["하드코딩된 환경별 값이 없는지 확인"]
  }
}
```

| 필드 | 설명 | 기본값 |
|------|------|--------|
| `conventions` | 컨벤션 문서 경로 또는 URL | null (범용) |
| `decisionLog` | ADR/설계문서 위치 | null |
| `focusAxes` | 중점 축 | 전체 4축 |
| `customChecks` | 축별 커스텀 체크 항목 | 빈 객체 |

설정 없이도 동작합니다. 설정이 있으면 해당 프로젝트의 컨벤션과 설계문서를 참조하여 검증합니다.

---

## 구조

```
claude-cross-verify-plugin/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── agents/
│   └── cross-verifier.md     # 교차 검증 에이전트 (sonnet, read-only)
└── skills/
    ├── verify/
    │   └── SKILL.md           # 4축 통합 검증 스킬
    └── setup/
        └── SKILL.md           # 프로젝트별 설정 초기화
```

---

## 배경

[AI 협업에서의 교차 검증 — 속도가 아닌 판단을 자동화하기](https://nhnent.dooray.com/wiki/3598984247241267301/4274984876242373871)
