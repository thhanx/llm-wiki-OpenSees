---
tags: [solver, parallel, reference]
updated: 2026-07-06
sources: [opensees-doc-mumps-solver, opensees-tn2007-16-parallel-processing, opensees-doc-build-application]
---

# Mumps Solver

```
system Mumps          # Tcl
system('Mumps')        # Python
```

[MUMPS](http://mumps-solver.org/)(MUltifrontal Massively Parallel Sparse direct Solver)를 사용하는 희소 연립방정식 솔버. 현재 병렬 [OpenSeesSP](opensees-sp.md)/[OpenSeesMP](opensees-mp.md) 애플리케이션으로만 제한된다 ([출처: opensees-doc-mumps-solver](../sources/opensees-doc-mumps-solver.md)).

## 성능·설치 특성

- TN-2007-16에 따르면 사용 가능한 병렬 솔버(Mumps/Petsc/SuperLU) 중 **일반적으로 성능이 가장 좋아 가장 먼저 시도해볼 솔버**로 권장된다.
- 다만 **저작권 문제로 기본 배포에 포함되지 않고 별도로 빌드해야 한다** ([출처: opensees-tn2007-16-parallel-processing](../sources/opensees-tn2007-16-parallel-processing.md)). 실제로 현행 공식 빌드 가이드에서도 MUMPS는 `github.com/OpenSees/mumps`를 별도 클론해 미리 빌드한 뒤 `-DMUMPS_DIR`로 연결해야 하는 필수 의존성으로 명시된다 ([출처: opensees-doc-build-application](../sources/opensees-doc-build-application.md), [MPI로 OpenSees 빌드하기](../guides/building-opensees-with-mpi.md)).
- 방정식 재정렬(equation reordering)을 자동으로 수행 — OpenSeesSP 실측 벤치마크(Humboldt Bay Bridge)도 `system Mumps`로 수행되었다.

## 관련 페이지

- [system 명령어](system-command.md) — 병렬 solver 선택 인터페이스
- [SuperLU Solver](superlu-solver.md), [PETSc Solver](petsc-solver.md) — 대안 병렬 솔버
- [MPI로 OpenSees 빌드하기](../guides/building-opensees-with-mpi.md) — MUMPS 빌드 절차
- [OpenSeesSP](opensees-sp.md) — 실측 벤치마크에 사용된 솔버
