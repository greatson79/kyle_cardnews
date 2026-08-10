# kyle_cardnews — 설교 → SNS 카드뉴스 스킬

설교의 핵심 메시지를 **인스타그램 카드뉴스 10장(1080×1350·4:5) + 캡션 + 카카오톡 공유
메시지**로 변환하는 Claude Code 스킬입니다. 디딤교회 주간 콘텐츠 자동화 시스템에서
실운영으로 검증된 워크플로우를 범용화해 공개합니다.

## 특징

- **복음 중심 구조**: 표지 → 공감 → 문제 심화 → 본문 전환 → CMT → **복음 적용(불가침)**
  → 인용구 → 적용 → 초대 → 마무리의 10장 고정 구조
- **품질 게이트 내장**: 복음 비트 생존 검증·환각 인용 금지·규격 검증을 스킬이 자가 수행
- **PNG 자동 캡쳐**: Puppeteer로 HTML 미리보기에서 1080×1350 고해상도 슬라이드 추출
- **브랜드 분리**: 교회명·색상·로고·해시태그는 `config/`에서만 관리 — 스킬 본문 수정 불필요

## 빠른 시작

1. 이 저장소를 프로젝트의 `.claude/skills/sns-cardnews/`로 복사(또는 clone)
2. `config/brand-guide.md`를 자기 교회 값으로 채우고 `config/logo.png` 추가
3. PNG 캡쳐용 의존성 설치: `npm install`
4. Claude Code에서 설교 요약(제목·본문·CMT·FCF·HP)과 함께 스킬 실행

입력 스키마와 상세 절차는 [SKILL.md](SKILL.md) 참조.

## 구조

```
kyle_cardnews/
├── SKILL.md                  ← 스킬 본체 (10장 구조·절차·품질 게이트)
├── rules/
│   ├── instagram.md          ← 캡션 규칙 (300자·해시태그)
│   ├── kakaotalk.md          ← 카톡 메시지 규칙 (150자)
│   └── design-agent.md       ← 디자인 브리프 생성 에이전트 (선택)
├── config/
│   └── brand-guide.md        ← 교회 브랜드 템플릿 (교회명·색상·해시태그·로고)
├── scripts/
│   └── capture-cardnews.js   ← HTML → PNG 캡쳐 (Puppeteer)
└── package.json
```

## 용어

| 약어 | 뜻 |
|------|-----|
| CMT | Central Message of the Text — 본문의 중심 진리 |
| FCF | Fallen Condition Focus — 본문이 다루는 인간의 문제 |
| HP | Hope/Gospel Point — 그리스도 안에서의 복음적 해답 |

(Bryan Chapell의 그리스도 중심 설교학 용어 체계)

## 요구 환경

- Claude Code (또는 SKILL.md를 읽을 수 있는 LLM 에이전트)
- Node.js 18+ (PNG 캡쳐 시)

## 라이선스

MIT
