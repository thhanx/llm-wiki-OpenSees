---
tags: [hpc, parallel, guide]
updated: 2026-07-06
sources: [designsafe-guide-openseessp, designsafe-guide-opensees, opensees-parallel-overview, opensees-tn2007-16-parallel-processing]
---

# HPC에서 OpenSees 실행하기

## 실행 커맨드 공통 패턴

```
mpiexec -np numProcs applicationName inputFile
```

`numProcs`는 사용할 프로세서 수, `applicationName`은 `OpenSeesSP` 또는 `OpenSeesMP`, `inputFile`은 Tcl 스크립트다. Windows는 MPICH2, Mac/Linux는 OpenMPI가 필요하다 ([출처: opensees-parallel-overview](../sources/opensees-parallel-overview.md)).

## 접근 경로

- **NSF NHERI DesignSafe-ci** — 무료 가입, OpenSeesSP/MP 애플리케이션을 웹 기반 HPC 환경에서 바로 실행 가능. 실무 가이드가 두 개 있다: [일반 OpenSees 가이드](../sources/designsafe-guide-opensees.md)(806줄, 버전 선택·입출력 처리 등 폭넓게 다룸)와 [OpenSeesSP 전용 가이드](../sources/designsafe-guide-openseessp.md).
- **XSEDE 계열 머신**(TACC Stampede2, Frontera) — 별도 할당(allocation) 신청 필요.
- **자체 클러스터** — [OpenSees용 클러스터 구축하기](building-a-cluster-for-opensees.md) 참고.

## OpenSeesSP로 전환할 때 스크립트에서 바꿔야 할 것

DesignSafe 가이드가 명시하는 변경사항 ([출처: designsafe-guide-openseessp](../sources/designsafe-guide-openseessp.md), TN-2007-16과 일치):

1. `system` 커맨드를 병렬 솔버로: `system Mumps` 또는 `system Diagonal`.
2. 레코더의 `-file` 플래그를 `-xml`로 — 병렬 실행 시 다중 노드/요소 레코더의 행 순서가 순차 실행과 다를 수 있기 때문(메타데이터를 함께 기록해야 컬럼을 식별 가능).

## OpenSeesMP — 파라미터 스터디를 대규모로 돌릴 때

TN-2007-16의 실전 예시는 PEER 지진동 데이터베이스 레코드 7200개를 `-par` 옵션으로 1024개 프로세스에 modulo 분배하는 시나리오다:

```bash
mpirun -np 1024 OpenSeesMP main.tcl -par gMotion records.txt
```

파일시스템이 병목이 될 정도로 큰 스터디에서는 반복 읽는 파일을 로컬 `/scratch`로 복사하고, 매 반복마다 `wipe`로 모델을 다시 만드는 대신 `remove loadPattern`/`remove recorders` + `reset`으로 모델을 한 번만 구성하는 것이 권장된다.

## 관련 페이지

- [OpenSeesSP](../reference/opensees-sp.md), [OpenSeesMP](../reference/opensees-mp.md)
- [Mumps Solver](../reference/mumps-solver.md)
- [OpenSees 병렬 컴퓨팅 개념](../concepts/opensees-parallel-computing.md)
- [MPI로 OpenSees 빌드하기](building-opensees-with-mpi.md)
