# kyle_cardnews — 설교 → SNS 카드뉴스 스킬 스위트 (v2.0)

설교 자료를 **인스타그램 카드뉴스 10장(1080×1350·4:5) + 캡션 + 카카오톡 공유 메시지**로
전자동 변환하는 Claude Code 스킬 스위트입니다. 디딤교회 주간 콘텐츠 자동화 시스템에서
실운영으로 검증된 워크플로우를 범용화해 공개합니다.

## 아키텍처 — 대표 1 + 하위 5

사용자는 **대표 스킬(`sns-cardnews`)과만 대화**합니다. 대표 스킬이 하위 스킬 5종을
목적에 맞게 스스로 호출하고, 전 과정을 Orchestration Trace로 보여줍니다.

```
사용자 ── sns-cardnews (대표 · 단일 창구)
              ├─ sns-cardnews-context   설교 자료에서 CMT·FCF·HP 자동 추출
              ├─ sns-cardnews-design    슬라이드별 디자인 브리프 (트렌드 리서치 포함)
              ├─ sns-cardnews-copy      10장 텍스트 + 캡션 + 카톡 메시지
              ├─ sns-cardnews-build     HTML 조립 + Puppeteer PNG 캡처
              └─ sns-cardnews-verify    독립 검증 (성경 인용 대조·복음 비트·규격)
```

- 하위 스킬은 전부 `disable-model-invocation: true` — 대표 스킬의 오케스트레이션으로만
  실행됩니다(직접 발동 불가·모델 부담 감소).
- **작성자·검증자 분리**: 성경 인용 정확성·복음 비트 생존·규격은 만든 스킬이 아니라
  `sns-cardnews-verify`가 독립 판정합니다. 통과 전에는 완성으로 인정되지 않습니다.
- 선택이 결과를 가르는 지점(표지 제목 2안·디자인 방향 분기·브랜드 미설정)에서는 대표
  스킬이 **진행 전에 질문**합니다. 임의 확정·하드코딩 없음.

## 빠른 시작

1. `skills/` 아래 6개 폴더를 프로젝트의 `.claude/skills/`로 복사:
   ```bash
   cp -r skills/* <프로젝트>/.claude/skills/
   ```
2. `config/`·`scripts/`를 프로젝트로 복사하고 `config/brand-guide.md`를 자기 교회 값으로
   채운 뒤 `config/logo.png` 추가
3. PNG 캡처 의존성 설치: `npm install`
4. Claude Code에서: **"이 설교로 카드뉴스 만들어줘"** + 설교 원고(전문·요약·전사 무엇이든)

설교 요약(sermon-context.md)이 없어도 됩니다 — `sns-cardnews-context`가 원고에서 자동
추출합니다.

## 구조

```
kyle_cardnews/
├── skills/
│   ├── sns-cardnews/            ← 대표 (여기에만 명령)
│   ├── sns-cardnews-context/
│   ├── sns-cardnews-design/
│   ├── sns-cardnews-copy/
│   ├── sns-cardnews-build/
│   └── sns-cardnews-verify/
├── rules/
│   ├── instagram.md             ← 캡션 세부 규칙 (300자·해시태그)
│   └── kakaotalk.md             ← 카톡 세부 규칙 (150자)
├── config/
│   └── brand-guide.md           ← 교회 브랜드 템플릿 (교회명·색상·해시태그·로고)
├── scripts/
│   └── capture-cardnews.js      ← HTML → 1080×1350 PNG (Puppeteer)
└── package.json
```

## 용어

| 약어 | 뜻 |
|------|-----|
| CMT | Central Message of the Text — 본문의 중심 진리 |
| FCF | Fallen Condition Focus — 본문이 다루는 인간의 문제 |
| HP | Hope/Gospel Point — 그리스도 안에서의 복음적 해답 |

(Bryan Chapell의 그리스도 중심 설교학 용어 체계)

## v1 → v2 변경

v1(단일 SKILL.md + 참조 문서)은 커밋 이력에 보존되어 있습니다. v2는 대표/하위 스킬
분리·자동 컨텍스트 추출·독립 검증 스킬·Orchestration Trace가 추가된 전면 재설계입니다.

## 요구 환경

- Claude Code (또는 SKILL.md 스킬 체계를 지원하는 LLM 에이전트)
- Node.js 18+ (PNG 캡처 시)

## 라이선스

MIT
