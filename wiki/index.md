# Index

위키 전체 카탈로그. 매 ingest마다 갱신된다. _마지막 갱신: 2026-07-06_

## Overview

- [overview.md](overview.md) — OpenSees 개요 + 현재 초점(최적화·병렬화)과 핵심 가설(16-프로세스 확장성 한계) 종합

## Sources (17)

| 페이지 | 요약 |
|---|---|
| [opensees-tn2007-16-parallel-processing](sources/opensees-tn2007-16-parallel-processing.md) | TN-2007-16 — 병렬 처리 공식 기술노트 (가장 포괄적) |
| [opensees-workshop-openseessp-slides](sources/opensees-workshop-openseessp-slides.md) | OpenSeesSP 워크숍 슬라이드 — Humboldt Bay Bridge 실측 벤치마크 |
| [opensees-workshop-parallel-developers-slides](sources/opensees-workshop-parallel-developers-slides.md) | 개발자용 병렬 확장 가이드 — send/recvSelf, SP/MP 내부 구조 |
| [opensees-evolution-history-scott2014](sources/opensees-evolution-history-scott2014.md) | OpenSees 역사·라이선스·규모 (Michael Scott, 2014) |
| [opensees-parallel-overview](sources/opensees-parallel-overview.md) | 공식 Parallel 개요 페이지 |
| [opensees-doc-system-command](sources/opensees-doc-system-command.md) | system 명령어 솔버 종류 목록 (공식) |
| [opensees-doc-mumps-solver](sources/opensees-doc-mumps-solver.md) | Mumps solver 공식 문서 |
| [opensees-doc-superlu-solver](sources/opensees-doc-superlu-solver.md) | SuperLU system 공식 문서 |
| [opensees-doc-build-application](sources/opensees-doc-build-application.md) | 현행 CMake 빌드 가이드 (Win/Mac/Ubuntu) |
| [opensees-doc-source-code-structure](sources/opensees-doc-source-code-structure.md) | GitHub 기여(fork/PR) 정책 |
| [openseespy-doc-parallel-commands](sources/openseespy-doc-parallel-commands.md) | OpenSeesPy 병렬 명령어 목록 |
| [designsafe-guide-openseessp](sources/designsafe-guide-openseessp.md) | DesignSafe OpenSeesSP 실행 가이드 |
| [designsafe-guide-opensees](sources/designsafe-guide-opensees.md) | DesignSafe OpenSees 일반 가이드 |
| [wikipedia-opensees](sources/wikipedia-opensees.md) | Wikipedia 개요 |
| [opensees-cluster-build-blog-abell2022](sources/opensees-cluster-build-blog-abell2022.md) | 8노드 Beowulf 클러스터 구축 실전기 (José Abell, 2022) |
| [opensees-domainpartitioner-api](sources/opensees-domainpartitioner-api.md) | DomainPartitioner 클래스 API (Doxygen) |
| [opensees-petscsoe-header-source](sources/opensees-petscsoe-header-source.md) | PetscSOE.h C++ 헤더 원문 |

## Reference (9)

| 페이지 | 요약 |
|---|---|
| [opensees-sp](reference/opensees-sp.md) | OpenSeesSP — 자동 도메인 분할, 대형 단일 모델용, Humboldt Bay Bridge 벤치마크 |
| [opensees-mp](reference/opensees-mp.md) | OpenSeesMP — 파라미터 스터디 자동화 + 수동 도메인 분할 |
| [domain-partitioner](reference/domain-partitioner.md) | DomainPartitioner 클래스 — partition/balance/swapVertex 등 |
| [system-command](reference/system-command.md) | system 명령어 — 솔버 선택 인터페이스, 병렬/순차 구분 |
| [mumps-solver](reference/mumps-solver.md) | Mumps solver — 최고 성능, 별도 빌드 필요 |
| [superlu-solver](reference/superlu-solver.md) | SuperLU — 항상 포함, 분산판이 실제 병렬 솔버 |
| [petsc-solver](reference/petsc-solver.md) | PETSc solver — 사용자 문서 공백, 소스코드로 일부 확인 |
| [parallel-numberer](reference/parallel-numberer.md) | ParallelPlain/ParallelRCM — 병렬 system과 항상 짝 |
| [openseespy-parallel-commands](reference/openseespy-parallel-commands.md) | OpenSeesPy 병렬 명령 13종 (Linux 전용) |

## Concepts (3)

| 페이지 | 요약 |
|---|---|
| [opensees-parallel-computing](concepts/opensees-parallel-computing.md) | **허브** — SP/MP 선택 기준, 16-프로세스 확장성 한계 종합 |
| [domain-decomposition](concepts/domain-decomposition.md) | 도메인 분할 개념 — 자동(SP) vs 수동(MP), 필수 설정 2가지 |
| [opensees-architecture](concepts/opensees-architecture.md) | 객체지향 아키텍처, send/recvSelf 패턴, 역사, 라이선스 |

## Guides (3)

| 페이지 | 요약 |
|---|---|
| [building-opensees-with-mpi](guides/building-opensees-with-mpi.md) | MPI+MUMPS 포함 빌드 — 현행 CMake 방식 + 구식 Makefile.def 방식 |
| [running-opensees-on-hpc](guides/running-opensees-on-hpc.md) | DesignSafe/XSEDE에서 SP/MP 실행 실무 |
| [building-a-cluster-for-opensees](guides/building-a-cluster-for-opensees.md) | 8노드 Beowulf 클러스터 구축 실전 사례 |
