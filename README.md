# Cross-Verify 플러그인

개발자 주도의 교차 검증 에이전트 — 의사결정·설계·문서·구현 4축 통합 검증.

## 왜 "교차" 검증인가?

AI가 빠르게 코드를 작성하는 시대, 핵심 질문은 "만들 수 있는가?"에서 "이게 맞는가?"로 바뀌었습니다. 기존 자동화 도구(Ralph, BMAD, Agent Teams)는 **AI가 AI를 검증**합니다. 이 플러그인은 그 반대 — **개발자가 AI의 판단을 검문**합니다.

"교차"는 두 가지 의미입니다:

**1. 4축 × 단일 대상** — 하나의 작업물을 4가지 관점에서 검증합니다.

| 축 | 핵심 질문 | 산출물 |
|----|----------|--------|
| 의사결정 | 왜 이 선택인가? 대안과 트레이드오프는? | 의사결정 근거 + 트레이드오프 정리 |
| 설계 | 엣지 케이스를 고려했는가? 정책과 일치하는가? | 검증 체크리스트 |
| 문서 | 가독성은? 톤은 적절한가? 정보 유실은 없는가? | 품질 리포트 |
| 구현 | 코드가 설계를 정확히 반영하는가? | 정합성 체크 |

**2. AI 판단 × 개발자 판단** — 개발자가 검증 과정에 참여하고 지켜봅니다.

> 자동 검증은 "맞다/틀리다"를 판별합니다.
> 교차 검증은 "왜 맞고 왜 틀린지"를 개발자와 함께 이해하는 과정입니다.

---

## 핵심 원칙

- **자동 수정 없음** — 판단은 개발자의 몫
- **검증 과정의 공유 가시성**이 목표
- **기존 도구(lint, test, CI)와 보완 관계**
- **도구가 측정할 수 없는 "의미적 판단"에 집중**

---

## 설치

```bash
# 1. 마켓플레이스 추가
/plugin marketplace add https://github.com/idean3885/claude-cross-verify.git

# 2. 플러그인 설치
/plugin install cross-verify
```

---

## 사용법

```
/verify

# 또는 트리거 키워드
"교차 검증해줘"
"크로스 체크"
"cross verify"
```

### 입력 예시

```
/verify src/auth/login.ts
"최근 커밋 교차 검증해줘"
"크로스 체크" + 위키 URL
"Redis 대신 인메모리 캐시를 선택한 의사결정 검증해줘"
```

---

## 프로필 커스터마이징

프로필로 프로젝트별 검증을 맞춤 설정합니다. `~/.claude/cross-verify/profiles/`에 배치하세요:

```json
{
  "version": "1.0",
  "project": "my-project",
  "conventions": "docs/CONVENTIONS.md",
  "designDocs": "docs/",
  "focusAxes": ["decision", "design", "documentation", "implementation"],
  "customChecks": {
    "decision": ["문서화된 의사결정 근거가 있는가?"],
    "implementation": ["하드코딩된 환경 값이 있는가?"]
  }
}
```

전체 템플릿은 `profiles/example.json`을 참고하세요.

---

## 구조

```
claude-cross-verify/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── agents/
│   └── cross-verifier.md     # 검증 에이전트 (sonnet, 읽기 전용)
├── profiles/
│   └── example.json           # 프로필 템플릿
└── skills/
    └── verify/
        └── SKILL.md           # 4축 검증 스킬
```

---

## 배경

블로그 포스팅 준비 중.
