# Raw Sources Manifest

`raw/`에 수집된 원본 소스의 출처 목록. 이 파일 자체는 매니페스트이며 위키 콘텐츠가 아니다
(카탈로그 역할은 `wiki/index.md`가 하고, 이 파일은 `raw/`만의 출처 추적용).
PDF 바이너리는 frontmatter를 가질 수 없어 출처 정보를 여기 기록한다.

수집 목적: OpenSees 최적화·병렬화 프로젝트 사전 자료 조사 (2026-07-06).
**ingest 상태: 아직 `wiki/`로 반영되지 않음** — 다음 단계로 `/llm-wiki-ingest` 대상.

## 병렬 처리 핵심 문서

| 파일 | 제목 | 출처 URL | 형식 |
|---|---|---|---|
| `opensees-tn2007-16-parallel-processing.pdf` | TN-2007-16: Using the OpenSees Interpreter on Parallel Machines | https://opensees.berkeley.edu/OpenSees/parallel/TNParallelProcessing.pdf | PDF (기술 노트) |
| `opensees-workshop-openseessp-slides.pdf` | OpenSeesSP — Frank McKenna, OpenSees Parallel Workshop | https://opensees.berkeley.edu/OpenSees/workshops/parallel/OpenSeesSP.pdf | PDF (워크숍 슬라이드) |
| `opensees-workshop-parallel-developers-slides.pdf` | Parallel OpenSees Applications for Developers — Frank McKenna | https://opensees.berkeley.edu/OpenSees/workshops/parallel/ParallelDevelopers.pdf | PDF (워크숍 슬라이드) |
| `opensees-parallel-overview.md` | OpenSees Parallel Processing (공식 개요) | https://opensees.berkeley.edu/OpenSees/parallel/parallel.php | 웹페이지 |
| `openseespy-doc-parallel-commands.md` | OpenSeesPy Parallel Commands | https://openseespydoc.readthedocs.io/en/latest/src/parallelcmds.html | 웹페이지 (요약, 상세 미포함) |
| `opensees-domainpartitioner-api.md` | DomainPartitioner Class Reference (Doxygen) | https://opensees.berkeley.edu/OpenSees/api/doxygen2/html/classDomainPartitioner.html | API 레퍼런스 |
| `opensees-petscsoe-header-source.md` | PetscSOE.h 소스 (C++ 헤더 원문) | https://opensees.berkeley.edu/OpenSees/api/doxygen2/html/PetscSOE_8h-source.html | 소스 코드 |

## 솔버·빌드 문서

| 파일 | 제목 | 출처 URL | 형식 |
|---|---|---|---|
| `opensees-doc-system-command.md` | system Command (솔버 종류 목록) | https://opensees.github.io/OpenSeesDocumentation/user/manual/analysis/system.html | 웹페이지 |
| `opensees-doc-mumps-solver.md` | Mumps Solver | https://opensees.github.io/OpenSeesDocumentation/user/manual/analysis/system/Mumps.html | 웹페이지 |
| `opensees-doc-superlu-solver.md` | SuperLU System | https://opensees.github.io/OpenSeesDocumentation/user/manual/analysis/system/SuperLU.html | 웹페이지 |
| `opensees-doc-build-application.md` | Building Application (Windows/Mac/Ubuntu, MPI 빌드 포함) | https://opensees.github.io/OpenSeesDocumentation/developer/build.html | 웹페이지 |
| `opensees-doc-source-code-structure.md` | Source Code (GitHub fork/PR 정책) | https://opensees.github.io/OpenSeesDocumentation/developer/sourceCode.html | 웹페이지 |

## 실무 HPC 가이드

| 파일 | 제목 | 출처 URL | 형식 |
|---|---|---|---|
| `designsafe-guide-openseessp.md` | openseesSP — DesignSafe User Guide | https://designsafe-ci.org/user-guide/tools/simulation/opensees/openseesSP/ | 웹페이지 (curl 추출) |
| `designsafe-guide-opensees.md` | OpenSees — DesignSafe User Guide | https://designsafe-ci.org/user-guide/tools/simulation/opensees/opensees/ | 웹페이지 (curl 추출) |
| `opensees-cluster-build-blog-abell2022.md` | Building a computer cluster for OpenSees (José Abell, 2022) | https://joseabell.com/posts/2022/buidling-a-computer-cluster-for-opensees.html | 블로그 |

## 배경·역사

| 파일 | 제목 | 출처 URL | 형식 |
|---|---|---|---|
| `opensees-evolution-history-scott2014.pdf` | The Evolution of OpenSees: Is the Open Source Model a Success? (Michael H. Scott, 2014) | https://portwooddigital.com/wp-content/uploads/2021/07/openseesoverview_2014.pdf | PDF |
| `wikipedia-opensees.md` | OpenSees — Wikipedia | https://en.wikipedia.org/wiki/OpenSees | 웹페이지 |

## 확보 시도했으나 실패한 소스 (참고용)

- Frank McKenna 박사학위논문 "Object-oriented finite element programming: Frameworks for analysis, algorithms and parallel computing" (UC Berkeley, 1997) — ProQuest 유료 접근만 가능, 원문 미확보. OpenSees/G3의 이론적 기반이 되는 가장 근본적인 문서이므로 추후 대학 도서관 접근 등으로 확보 가치가 높음.
- PETSc solver 공식 문서 페이지 — 신규 문서(opensees.github.io)와 구 위키 양쪽에서 404. 소스 코드 헤더(`opensees-petscsoe-header-source.md`)로 일부 대체.
- ResearchGate 논문 3편(GPU 가속, 병렬 비선형 지진해석, Object-Oriented Parallel Structural Analysis) — 초록만 접근 가능한 페이월 추정, 본문 미확보. 제목과 저자 정보는 wiki ingest 시 참고문헌으로만 기록 권장.

## 알려진 한계

- WebFetch로 추출한 페이지 중 일부(`opensees-doc-system-command.md`, `opensees-doc-source-code-structure.md`, `openseespy-doc-parallel-commands.md`)는 소형 모델의 요약을 거쳐 원문보다 축약되었을 수 있음. 정확한 세부사항이 필요하면 출처 URL 원문 대조 필요.
- DesignSafe 페이지 2건은 WebFetch가 403으로 차단되어 curl + 자체 HTML 추출 스크립트로 대체 확보함. 표/코드블록 경계 서식이 일부 손실되었을 수 있음.
