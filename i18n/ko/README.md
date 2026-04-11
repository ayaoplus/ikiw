**언어:** [English](../en/README.md) | [中文](../../README.md) | 한국어 | [日本語](../ja/README.md) | [Español](../es/README.md)

# ikiw

LLM 기반의 개인 지식 베이스 skill. 프레임워크, 데이터베이스, 벡터 검색 없이 순수 prompt 기반으로 동작하며, 파일 읽기/쓰기가 가능한 어떤 LLM agent에서든 사용할 수 있습니다.

[Karpathy의 LLM Wiki 패턴](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)에서 영감을 받았습니다.

## 핵심 아이디어

글을 하나의 디렉토리에 넣으면, LLM이 각 글에 대한 요약을 생성하고 인덱스 파일에 통합합니다. 조회 시 LLM이 인덱스를 읽고 해당 글을 찾아 원문을 읽은 뒤 종합적으로 답변합니다. 이게 전부입니다.

태그도 없고, 분류도 없고, 데이터베이스도 없습니다. 요약 자체가 가장 입체적인 "태그"입니다.

## 지식 베이스 구조

```
my-wiki/
├── raw/              # 원본 글, 읽기 전용
├── summaries.md      # 요약 인덱스, 시스템의 핵심
├── wiki/             # 필요에 따라 생성되는 지식 페이지
└── SCHEMA.md         # 지식 베이스 설정 (커스텀 prompt)
```

## 기능

| 명령 | 설명 |
|------|------|
| 요약 생성 | 글을 읽고 요약을 생성하여 summaries.md에 추가 |
| 일괄 요약 | 미처리된 글을 감지하여 일괄 생성 |
| 조회 | 요약 인덱스 읽기 → 글 찾기 → 원문 읽기 → 종합 답변 |
| Wiki 생성 | 주제별 지식을 구조화된 페이지로 정리, 필요 시 생성 |
| 스타일 글쓰기 | 콘텐츠 + 글쓰기 스타일 템플릿 기반으로 글 출력 |
| 스타일 출력 | design-md로 styled HTML 렌더링 |
| 새 글 등록 | 요약 생성 및 wiki 업데이트 확인 |
| 초기화 | 지식 베이스 생성, 대화를 통해 SCHEMA.md 생성 |
| 요약 도우미 | 대화를 통해 사용자의 요약 prompt 정의 지원 |

## 3단계 출력

동일한 콘텐츠에 단계별로 서로 다른 템플릿을 적용할 수 있습니다:

1. **콘텐츠 레이어** — SCHEMA.md의 wiki prompt가 무엇을 쓸지 결정
2. **스타일 레이어** — `styles/` 디렉토리의 글쓰기 스타일 템플릿이 어떻게 쓸지 결정
3. **비주얼 레이어** — `designs/` 디렉토리의 design-md가 어떻게 보여줄지 결정

## 빠른 시작

1. 이 저장소를 skill로 LLM agent에 로드합니다 (Claude Code, Codex, OpenCode 등)
2. agent에게 "지식 베이스를 만들어 줘"라고 말합니다
3. agent가 대화를 통해 글 유형과 관심사를 파악하고, 자동으로 SCHEMA.md를 생성합니다
4. `raw/` 디렉토리에 글을 넣습니다
5. agent에게 "요약을 생성해 줘"라고 말합니다
6. 조회를 시작합니다

## 프로젝트 구조

```
ikiw/
├── SKILL.md              # skill 정의 (agent의 진입점)
├── SCHEMA.template.md    # 지식 베이스 설정 템플릿
├── designs/              # 비주얼 스타일 (design-md)
│   ├── notion.md
│   ├── claude.md
│   ├── linear.app.md
│   ├── stripe.md
│   ├── vercel.md
│   └── mintlify.md
└── styles/               # 글쓰기 스타일 템플릿 (사용자가 직접 추가)
```

## 규모

- 만 건 이내: summaries.md를 한 번에 읽을 수 있으며, 추가 인프라가 필요 없음
- 만 건 초과: 벡터 검색을 연동하여 1차 필터링, 요약 데이터를 embedding에 직접 활용 가능

## 설계 원칙

- **코드 없음** — 전체 시스템은 prompt 조합이며, 프로그램이 아님
- **원문 불변** — raw/ 디렉토리는 읽기 전용
- **사전 생성 없음** — wiki는 필요 시 생성, 낭비 없음
- **플랫폼 독립** — 파일 읽기/쓰기가 가능한 어떤 LLM agent에서든 사용 가능
- **과도한 설계 없음** — 필요할 때 기능을 추가

## 비주얼 스타일 추가

```bash
npx getdesign@latest add <style-name>
# 생성된 DESIGN.md의 이름을 변경한 후 designs/ 디렉토리에 배치
```

60개 이상의 사용 가능한 스타일: https://github.com/VoltAgent/awesome-design-md

## 감사의 말

- [Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — LLM Wiki 패턴
- [VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md) — 비주얼 스타일 라이브러리
