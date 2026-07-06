# Log

시간순 append-only 작업 기록. 형식: `## [YYYY-MM-DD] <동작> | <제목>`
(동작: ingest | query | lint | schema)

## [2026-07-06] schema | 위키 스캐폴딩 구축

- `raw/`, `wiki/`(sources/reference/concepts/guides) 디렉터리 구조 생성
- `AGENTS.md`(스키마), `CLAUDE.md`, `index.md`, `log.md`, `overview.md` 초기 작성
- 로컬 스킬 3종 생성: `.claude/skills/llm-wiki-{ingest,query,lint}`

## [2026-07-06] ingest | OpenSees 최적화·병렬화 자료 17종 일괄 ingest

병렬화/최적화 프로젝트 사전 조사로 raw/에 수집된 17개 소스(4개 PDF + 13개 웹 클립)를
사용자 승인 하에 확인 단계 없이 일괄 처리. 상세 소스 목록은 `raw/SOURCES.md`.

- **sources/** 17개 페이지 신규 작성 (raw 파일명과 1:1 대응)
- **reference/** 9개 페이지 신규 작성: opensees-sp, opensees-mp, domain-partitioner,
  system-command, mumps-solver, superlu-solver, petsc-solver, parallel-numberer,
  openseespy-parallel-commands
- **concepts/** 3개 페이지 신규 작성: domain-decomposition, opensees-architecture,
  opensees-parallel-computing(허브 — SP/MP 선택 기준 및 16-프로세스 확장성 한계 종합)
- **guides/** 3개 페이지 신규 작성: building-opensees-with-mpi, running-opensees-on-hpc,
  building-a-cluster-for-opensees
- **overview.md** 전면 갱신 — 현재 초점(최적화·병렬화)과 핵심 가설 반영
- **index.md** 전면 갱신 — 전체 카탈로그(sources 17, reference 9, concepts 3, guides 3)

주요 발견: OpenSeesSP 실측 벤치마크(Humboldt Bay Bridge)와 José Abell의 클러스터
구축 경험이 독립적으로 "약 16개 프로세스 초과 시 선형 확장성 상실"을 보고 —
최적화 프로젝트의 1순위 조사 가설로 `opensees-parallel-computing.md`에 기록.

데이터 공백: McKenna 1997 박사논문(페이월), PETSc 사용자 문서(404), GPU 가속 연구
본문(미확보) — `raw/SOURCES.md`와 `overview.md`에 기록.

## [2026-07-06] lint | 위키 전체

기계적 점검 전부 통과: 깨진 링크 0, 고아 페이지 0, index 불일치 0,
frontmatter 비허용 키 0, log 형식 정상. 내용 점검에서 발견된 사항은
사용자 결정으로 **수정 보류** (기록만 남김):

- **출처 오귀속** `reference/openseespy-parallel-commands.md` — mpi4py 단락이
  `openseespy-doc-parallel-commands` 소스를 인용하지만 해당 raw 파일에 mpi4py
  내용이 없음 (실제 출처는 미확보된 DesignSafe openseesPy 페이지의 검색 스니펫).
  → 추후 `⚠️ 미검증` 표시 또는 DesignSafe openseesPy 가이드 ingest로 해소할 것.
- **미출처 주장** `reference/domain-partitioner.md` — "(예: METIS 계열)"이
  ingest된 소스에 없음. → 추후 `⚠️ 미검증` 표시 또는 Metis.cpp Doxygen ingest.
- **문장 오류(경미)** `concepts/opensees-architecture.md` — G2/G3 전신 관계
  문장이 꼬여 있음.
- **데이터 공백**: (1) ~16 프로세스 확장성 한계의 원인 분석 자료 없음(핵심 가설),
  (2) DesignSafe openseesPy 가이드, (3) Metis.cpp Doxygen, (4) OpenSeesPy 병렬
  명령 13종 상세 시그니처, (5) KSC 클러스터 MPI 모듈 선택(사이트 특화),
  (6) OpenSeesRT 커버리지 전무.
- **누락 페이지 후보(보류)**: PartitionedDomain/ActorSubdomain — 소스 부족으로
  스텁 수준이라 추가 ingest 후 생성 권장.
