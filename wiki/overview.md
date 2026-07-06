---
tags: [overview]
updated: 2026-07-06
---

# OpenSees 개요

OpenSees(Open System for Earthquake Engineering Simulation)는 UC Berkeley PEER(Pacific Earthquake Engineering Research Center)에서 개발된 객체지향(C++) 지진공학 시뮬레이션 프레임워크다. Tcl 인터프리터(`OpenSees`)와 Python 패키지(`openseespy`) 두 인터페이스를 제공하며, `Domain`(모델 저장소) — `Analysis`(해석 절차) — `Element`/`Material` 등 개체를 조합해 비선형 정적·동적 해석을 수행한다. 자세한 배경·역사·라이선스는 [OpenSees 아키텍처](concepts/opensees-architecture.md) 참고.

## 이 위키의 현재 초점 — 최적화·병렬화

이 위키는 OpenSees를 HPC 환경에서 최적화·병렬화하는 프로젝트를 준비하기 위해 구축 중이며, 2026-07-06 첫 ingest 배치는 이 주제에 집중되어 있다.

### 병렬 처리의 두 축

OpenSees는 MPI 기반 분산 메모리 병렬 처리를 [OpenSeesSP](reference/opensees-sp.md)(자동 도메인 분할, 대형 단일 모델용)와 [OpenSeesMP](reference/opensees-mp.md)(수동 분할 또는 파라미터 스터디 자동화)라는 두 애플리케이션으로 제공한다. 둘 다 [DomainPartitioner](reference/domain-partitioner.md) 기반의 [Domain Decomposition](concepts/domain-decomposition.md)을 쓰며, 실제 방정식을 병렬로 푸는 솔버는 [Mumps](reference/mumps-solver.md)·[PETSc](reference/petsc-solver.md)·분산 SuperLU([SuperLU](reference/superlu-solver.md)) 세 가지뿐이다. 자세한 비교와 선택 기준은 허브 페이지 [OpenSees 병렬 컴퓨팅 개념](concepts/opensees-parallel-computing.md) 참고.

### 지금까지 확인된 가장 중요한 사실 — 확장성 한계

독립적인 두 출처(McKenna의 실측 벤치마크, José Abell의 실무 경험)가 공통적으로 **"약 16개 프로세스를 넘어서면 선형 확장성을 잃는다"**고 보고한다. 이것이 이 위키가 추적하는 핵심 가설이며, 최적화 프로젝트가 규명해야 할 가장 유력한 출발점이다 — 자세한 내용과 데이터 공백은 [OpenSees 병렬 컴퓨팅 개념](concepts/opensees-parallel-computing.md#핵심-발견-확장성은-대략-16개-프로세스-근처에서-꺾인다) 참고.

### 실전 가이드

빌드([MPI로 OpenSees 빌드하기](guides/building-opensees-with-mpi.md)), 실행([HPC에서 OpenSees 실행하기](guides/running-opensees-on-hpc.md)), 자체 클러스터 구축([OpenSees용 클러스터 구축하기](guides/building-a-cluster-for-opensees.md)) 세 편의 실무 가이드를 확보했다.

### 아직 채워지지 않은 부분

- GPU 가속 관련 연구(제목만 확보, 본문 미확보 — `raw/SOURCES.md` 참고)
- Frank McKenna의 1997년 박사학위논문 원문(OpenSees/G3의 이론적 토대이나 ProQuest 페이월)
- PETSc solver의 사용자 문서(신구 공식 사이트 모두 404 — 소스 헤더로만 일부 대체)
- 16-프로세스 한계의 원인 규명(통신 오버헤드 vs 파티셔닝 품질 vs 솔버 자체 한계) — 아직 어느 자료도 원인을 분석하지 않음
- 일반적인 모델링(재료·요소·해석 유형) 관련 페이지 — 이번 배치는 병렬화/최적화에만 집중했으므로 비어 있음

## 관련 페이지

- [OpenSees 병렬 컴퓨팅 개념](concepts/opensees-parallel-computing.md) (허브)
- [OpenSees 아키텍처](concepts/opensees-architecture.md)
- [Domain Decomposition](concepts/domain-decomposition.md)
