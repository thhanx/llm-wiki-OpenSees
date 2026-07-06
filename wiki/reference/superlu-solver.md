---
tags: [solver, reference]
updated: 2026-07-06
sources: [opensees-doc-superlu-solver, opensees-tn2007-16-parallel-processing]
---

# SuperLU Solver

```
system SuperLU     # 구 명칭: system SparseGEN (여전히 작동)
```

희소 행렬을 위한 SparseGEN 선형 시스템 객체를 생성하며, 실제 풀이는 SuperLU 라이브러리가 수행한다. 방정식을 자동으로 재정렬하므로 "Plain numberer" 외 별도 설정이 불필요하다(순차 실행 시) ([출처: opensees-doc-superlu-solver](../sources/opensees-doc-superlu-solver.md)).

## 순차 SuperLU vs Distributed SuperLU — 병렬 처리 시 중요한 구분

TN-2007-16에 따르면 병렬 머신에서 `SparseGEN`을 스크립트에 지정하면 **자동으로 병렬판(Distributed SuperLU)으로 전환**되어 실제로 방정식을 분산 처리한다. 반면 일반 `system SuperLU`를 병렬 도메인 분할 상황에서 그대로 쓰면(병렬 numberer 없이) 시스템이 P0로 모여 순차적으로 풀릴 수 있다 — 이는 [Mumps](mumps-solver.md)/[PETSc](petsc-solver.md)와 함께 "실제로 병렬로 방정식을 푸는 세 솔버" 중 하나로 SuperLU(분산판)를 꼽는 이유다 ([system 명령어](system-command.md)의 병렬/순차 구분 참고).

- 항상 기본 배포에 포함(Mumps처럼 별도 설치 불필요).
- 참고문헌: Demmel et al., "A supernodal approach to sparse partial pivoting", SIAM J. Matrix Analysis and Applications, 20(3), 1999.

## 관련 페이지

- [system 명령어](system-command.md), [Mumps Solver](mumps-solver.md), [PETSc Solver](petsc-solver.md)
- [Domain Decomposition](../concepts/domain-decomposition.md)
