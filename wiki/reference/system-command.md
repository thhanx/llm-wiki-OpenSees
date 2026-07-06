---
tags: [solver, reference]
updated: 2026-07-06
sources: [opensees-doc-system-command, opensees-tn2007-16-parallel-processing]
---

# system 명령어

`system` 명령어는 선형방정식 시스템(Ax=b)을 풀기 위한 `LinearSOE`/`LinearSolver` 객체를 구성한다.

```
system systemType? arg1? ...
```

## 지원 유형

공식 문서(opensees.github.io)에 나열된 유형: BandGeneral, BandSPD, ProfileSPD, SuperLU, Umfpack, FullGeneral, SparseSYM, Mumps, Diagonal/MPIDiagonal, PythonSparse ([출처: opensees-doc-system-command](../sources/opensees-doc-system-command.md)). 구 위키에는 PETSc 항목도 별도 문서화되어 있었으나 신규 문서 사이트에는 빠져 있다 — [PETSc Solver](petsc-solver.md) 참고.

## 병렬 처리와의 관계 — 중요

TN-2007-16에 따르면 병렬 머신에서 실제로 **방정식을 병렬로 푸는 것은 Mumps, Petsc, DistributedSuperLU 세 가지뿐**이다. 그 외 솔버는 시스템을 P0 하나로 모아 순차적으로 푼다. **도메인 분할(사용자 정의 파티셔닝)을 할 때는 반드시 병렬 system을 지정해야 한다** — 대표적으로 `ParallelBandGeneral`, `ParallelProfileSPD` 같은 병렬 전용 변형이 존재한다 ([출처: opensees-tn2007-16-parallel-processing](../sources/opensees-tn2007-16-parallel-processing.md)).

`SparseGEN`을 스크립트에 지정하면 병렬 머신에서 실행 시 자동으로 병렬판 SuperLU로 전환된다.

## 관련 페이지

- [Mumps Solver](mumps-solver.md) — 일반적으로 최고 성능, 별도 설치 필요(저작권)
- [SuperLU Solver](superlu-solver.md) — 항상 포함, 순차/분산 두 버전 존재
- [PETSc Solver](petsc-solver.md) — 시스템에 존재할 가능성이 높음
- [Parallel Numberer](parallel-numberer.md) — 병렬 system과 항상 짝을 이뤄야 하는 설정
- [OpenSeesSP](opensees-sp.md), [OpenSeesMP](opensees-mp.md)
- [OpenSees 병렬 컴퓨팅 개념](../concepts/opensees-parallel-computing.md)
