---
tags: [parallel, hpc, concept, hub]
updated: 2026-07-06
sources: [opensees-tn2007-16-parallel-processing, opensees-parallel-overview, opensees-workshop-openseessp-slides, opensees-cluster-build-blog-abell2022]
---

# OpenSees 병렬 컴퓨팅 개념 (허브 페이지)

OpenSees는 메시지 전달(MPI) 기반의 분산 메모리 병렬 처리를 지원한다. 병렬 컴퓨터는 프로세스 간 통신 비용이 성능을 좌우하며, 소규모 작업은 통신 비용이 병렬화 이득을 상쇄해 오히려 단일 머신이 더 빠를 수 있다 ([출처: opensees-tn2007-16-parallel-processing](../sources/opensees-tn2007-16-parallel-processing.md)).

이 문서는 병렬화/최적화 프로젝트를 위해 지금까지 ingest된 자료를 잇는 허브 역할을 한다.

## 두 애플리케이션 — 언제 무엇을 쓸까

| 상황 | 선택 |
|---|---|
| 대형 모델 하나를 빨리 풀고 싶고, 병렬 지식은 최소화하고 싶다 | [OpenSeesSP](../reference/opensees-sp.md) |
| 다수의 독립적 해석(파라미터 스터디, 다중 지진동)을 병렬로 돌리고 싶다 | [OpenSeesMP](../reference/opensees-mp.md) `-par` 모드 |
| 대형 모델을 직접 세밀하게 분할·제어하고 싶다(병렬 프로그래밍 지식 있음) | [OpenSeesMP](../reference/opensees-mp.md) 사용자 정의 분할 모드 |
| Python 워크플로에 통합하고 싶다 | [OpenSeesPy 병렬 명령어](../reference/openseespy-parallel-commands.md) (Linux 전용) |

두 경로 모두 [Domain Decomposition](domain-decomposition.md)이라는 동일한 하부 메커니즘([DomainPartitioner](../reference/domain-partitioner.md))을 쓰지만, 분할이 **자동(SP)**인지 **수동(MP)**인지가 근본적 차이다.

## 핵심 발견: 확장성은 대략 16개 프로세스 근처에서 꺾인다

지금까지 ingest한 자료 중 **두 개의 독립적인 출처**가 같은 결론에 도달한다:

1. **실측 벤치마크** — OpenSeesSP 워크숍 슬라이드의 Humboldt Bay Bridge 모델: 1개 프로세서 ~315분 → 16개 ~48분. 곡선이 16개 근처에서 이미 뚜렷하게 체감(diminishing returns)한다 ([출처: opensees-workshop-openseessp-slides](../sources/opensees-workshop-openseessp-slides.md)).
2. **실무 경험담** — José Abell(클러스터 구축자)이 ASDEA의 Massimo Petarca와 상의한 결과: "OpenSees는 16개 프로세스를 초과하면 선형 확장성을 잃는다"는 결론에 도달, 이를 근거로 대형 단일 서버보다 다수의 저비용 노드로 구성된 분산 메모리 클러스터를 선택했다 ([출처: opensees-cluster-build-blog-abell2022](../sources/opensees-cluster-build-blog-abell2022.md)).

> ⚠️ **미검증**: 이 "16개" 한계가 어떤 문제 유형(도메인 분할 방식, 솔버 종류, 통신 대역폭)에 좌우되는지, 최신 OpenSees 버전에서도 여전히 유효한지는 아직 확인되지 않았다. 두 출처 모두 특정 사례(Humboldt Bay Bridge 모델, 저자의 특정 클러스터 하드웨어)에 기반한 것이라 일반화에는 주의가 필요하다. **최적화 프로젝트의 유력한 첫 가설**로 다룰 만하다 — 병목이 통신인지, 파티셔닝 품질(load imbalance)인지, 솔버 자체의 병렬 확장성인지 규명이 필요하다.

## 최적화·병렬화 프로젝트 관점에서의 시사점

- 확장성 병목 후보 지점: (a) [DomainPartitioner](../reference/domain-partitioner.md)의 그래프 분할 품질(불균등 분할 시 load imbalance), (b) 병렬 솔버(Mumps/Petsc/분산 SuperLU) 자체의 통신 오버헤드, (c) OpenSeesSP의 "P0가 전체 모델을 먼저 만든다"는 구조적 병목(→ 이 문제는 OpenSeesMP에는 없음).
- 파일시스템도 대규모 파라미터 스터디에서 실질적 병목이 될 수 있다(로컬 `/scratch` 활용, `reset`/`remove` 활용 권장 — [OpenSeesMP](../reference/opensees-mp.md) 참고).
- GPU 가속은 이번 ingest 범위 밖(raw/에 관련 논문 제목만 확보, 본문 미확보 — `raw/SOURCES.md`의 "확보 시도했으나 실패한 소스" 참고). 향후 조사 대상.
- HPC 클러스터를 직접 구축/운영할 경우 [OpenSees용 클러스터 구축하기](../guides/building-a-cluster-for-opensees.md), 기존 HPC(DesignSafe/XSEDE류)를 쓸 경우 [HPC에서 OpenSees 실행하기](../guides/running-opensees-on-hpc.md) 참고.

## 관련 페이지

- [Domain Decomposition](domain-decomposition.md), [OpenSees 아키텍처](opensees-architecture.md)
- [OpenSeesSP](../reference/opensees-sp.md), [OpenSeesMP](../reference/opensees-mp.md), [OpenSeesPy 병렬 명령어](../reference/openseespy-parallel-commands.md)
- [system 명령어](../reference/system-command.md), [Mumps](../reference/mumps-solver.md)/[SuperLU](../reference/superlu-solver.md)/[PETSc](../reference/petsc-solver.md) Solver
- [MPI로 OpenSees 빌드하기](../guides/building-opensees-with-mpi.md), [HPC에서 OpenSees 실행하기](../guides/running-opensees-on-hpc.md), [OpenSees용 클러스터 구축하기](../guides/building-a-cluster-for-opensees.md)
