# AGENTS.md — llm-wiki-OpenSees 운영 규칙 (스키마)

이 저장소는 OpenSees(Open System for Earthquake Engineering Simulation)에 관한
**LLM 유지 위키**다. Karpathy의 llm-wiki 패턴을 따른다:
사람은 소스를 큐레이션하고 질문하며, LLM(너)이 위키의 작성·갱신·상호참조·정리를
전부 담당한다. 이 문서가 그 운영 규칙이며, 사용자와 협의해 계속 발전시킨다.

## 3계층 구조

| 계층 | 위치 | 소유자 | 규칙 |
|------|------|--------|------|
| Raw sources | `raw/` | 사용자 | **불변.** LLM은 읽기만 한다. 수정·이동·삭제 금지 |
| Wiki | `wiki/` | LLM | LLM이 생성·갱신·유지. 사용자는 읽는다 |
| Schema | `AGENTS.md` | 공동 | 사용자 승인 하에 개정. 개정 시 log에 `schema` 엔트리 기록 |

## 디렉터리 구조

```
raw/                  불변 원본 소스 (문서, 논문, 매뉴얼 페이지, 예제 스크립트 등)
raw/assets/           소스에 딸린 이미지·데이터 파일
wiki/
  index.md            콘텐츠 카탈로그 — 모든 페이지의 링크 + 한 줄 요약 (매 ingest마다 갱신)
  log.md              시간순 append-only 작업 기록
  overview.md         OpenSees 전체 개요 — 위키가 성장하며 갱신되는 종합(synthesis) 페이지
  sources/            소스당 1개의 요약 페이지
  reference/          개체(entity) 페이지 — 명령어, 재료(material), 요소(element),
                      단면(section), 알고리즘, integrator, 수렴 판정(test) 등
  concepts/           개념 페이지 — 해석 유형, 모델링 기법, 수치해석 이론 등
  guides/             하우투, 비교, 분석 — 쿼리 답변 중 보존 가치가 있는 것을 파일링
```

## 작성 컨벤션

- **언어**: 본문은 한국어. 기술 용어(명령어, 재료·요소 이름, API, 파라미터 등)는
  영어 원문 그대로 유지한다. 예: "`uniaxialMaterial Steel02`는 Giuffré-Menegotto-Pinto
  모델 기반의 철근 재료다."
- **파일명**: 영어 kebab-case (`steel02.md`, `pushover-analysis.md`).
  sources/ 페이지는 raw 파일명과 대응되게 짓는다.
- **링크**: 상대 경로의 표준 마크다운 링크 (`[Steel02](../reference/steel02.md)`).
  GitHub과 Obsidian 양쪽에서 렌더링되도록 `[[wikilink]]`는 쓰지 않는다.
- **frontmatter** (선택, 쓸 경우 이 키만): `tags`, `updated`(YYYY-MM-DD),
  `sources`(참조한 sources/ 페이지 목록).
- **출처 표기**: 모든 실질적 주장에는 근거를 단다 — 해당 `sources/` 페이지 링크
  또는 `raw/` 파일 경로. 소스 없이 LLM 자체 지식으로 쓴 내용은 페이지 상단에
  `> ⚠️ 미검증: 아직 ingest된 소스로 뒷받침되지 않은 내용` 블록으로 표시한다.
- **모순 처리**: 소스 간 주장이 충돌하면 숨기지 말고 양쪽을 `> ⚠️ 상충:` 블록으로
  나란히 기록하고 각각 출처를 단다.

## 운영 워크플로

세부 절차는 `.claude/skills/` 아래 스킬 3종에 정의되어 있다. 요약:

- **Ingest** (`/llm-wiki-ingest <raw 파일>`): 소스를 읽고 → 핵심을 사용자와 확인하고 →
  `sources/` 요약 페이지 작성 → 관련 `reference/`·`concepts/` 페이지 생성·갱신 →
  `index.md` 갱신 → `log.md` 기록. 소스 하나가 여러 페이지를 건드리는 것이 정상이다.
- **Query** (`/llm-wiki-query <질문>`): `index.md`를 먼저 읽고 관련 페이지로 파고들어
  인용과 함께 답한다. 보존 가치 있는 답변은 사용자 동의 하에 `guides/`에 파일링한다.
- **Lint** (`/llm-wiki-lint`): 모순, 낡은 주장, 고아 페이지, 누락된 상호참조,
  데이터 공백을 점검해 보고하고, 승인된 항목을 수정한다.

## log.md 엔트리 형식

grep으로 파싱 가능하도록 접두사를 통일한다:

```
## [YYYY-MM-DD] <동작> | <제목>
```

`<동작>`은 `ingest` | `query` | `lint` | `schema` 중 하나.
본문에는 무엇을 했고 어떤 페이지를 만들거나 고쳤는지 목록으로 적는다.
최근 작업 확인: `grep "^## \[" wiki/log.md | tail -5`

## 일반 원칙

- 위키 페이지를 수정하면 반드시 `index.md`의 해당 항목(한 줄 요약, 날짜)도 맞춘다.
- 페이지를 새로 만들면 관련 기존 페이지들에서 그 페이지로의 링크도 추가한다
  (고아 페이지를 만들지 않는다).
- 확신이 없는 구조적 결정(새 카테고리 추가, 페이지 분할·병합 등)은 사용자와
  상의한다. 일상적 갱신은 상의 없이 진행한다.
- 커밋은 사용자가 요청할 때만 한다.
