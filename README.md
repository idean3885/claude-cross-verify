# Cross-Verify Plugin

Developer-driven cross-verification agent — 4-axis verification for decisions, design, documentation, and implementation.

## Why "Cross" Verification?

When AI writes code fast, the question shifts from "can I build this?" to "is this right?" Existing automation tools (Ralph, BMAD, Agent Teams) let **AI verify AI**. This plugin flips that — **the developer checkpoints AI's judgment**.

The "cross" works on two levels:

**1. 4 axes × single target** — one piece of work, examined from 4 perspectives.

| Axis | Core Question | Output |
|------|--------------|--------|
| Decision | Why this choice? What are the alternatives and trade-offs? | Decision rationale + trade-off summary |
| Design | Are edge cases considered? Is it consistent with policy? | Verification checklist |
| Documentation | Is it readable? Is the tone appropriate? Any information loss? | Quality report |
| Implementation | Does code accurately reflect the design? | Consistency check |

**2. AI judgment × developer judgment** — the developer watches and participates in the verification process.

> Automated verification determines "right or wrong."
> Cross-verification is the process of understanding "why right and why wrong" together with the developer.

---

## Core Principles

- **No auto-fix** — decisions belong to the developer
- **The goal is shared visibility into the verification process**
- **Complementary to existing tools (lint, test, CI)**
- **Focuses on "semantic judgment" that tools cannot measure**

---

## Install

```bash
# 1. Add marketplace
/plugin marketplace add https://github.com/idean3885/claude-cross-verify.git

# 2. Install plugin
/plugin install cross-verify
```

---

## Usage

```
/verify

# Or trigger keywords
"교차 검증해줘"
"크로스 체크"
"cross verify"
```

### Input Examples

```
/verify src/auth/login.ts
"cross verify the recent commit"
"크로스 체크" + wiki URL
"verify the decision to use in-memory cache instead of Redis"
```

---

## Profile Customization

Profiles customize verification per project. Place them in `~/.claude/cross-verify/profiles/`:

```json
{
  "version": "1.0",
  "project": "my-project",
  "conventions": "docs/CONVENTIONS.md",
  "designDocs": "docs/",
  "focusAxes": ["decision", "design", "documentation", "implementation"],
  "customChecks": {
    "decision": ["Is there a documented rationale?"],
    "implementation": ["Are there hardcoded environment values?"]
  }
}
```

See `profiles/example.json` for a complete template.

---

## Structure

```
claude-cross-verify/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── agents/
│   └── cross-verifier.md     # Verification agent (sonnet, read-only)
├── profiles/
│   └── example.json           # Profile template
└── skills/
    └── verify/
        └── SKILL.md           # 4-axis verification skill
```

---

## Background

Blog post coming soon.
