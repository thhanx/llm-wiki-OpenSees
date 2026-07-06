---
tags: [solver, parallel, reference]
updated: 2026-07-06
sources: [opensees-petscsoe-header-source, opensees-tn2007-16-parallel-processing, opensees-doc-system-command]
---

# PETSc Solver

```
system Petsc
```

PETSc 라이브러리 기반의 병렬 희소 솔버. TN-2007-16은 이를 Mumps·(Distributed)SuperLU와 함께 **실제로 방정식을 병렬로 푸는 3대 솔버** 중 하나로 꼽으며 "시스템에 존재할 가능성이 높다(probably available)"고 설명한다 ([출처: opensees-tn2007-16-parallel-processing](../sources/opensees-tn2007-16-parallel-processing.md)).

> ⚠️ **데이터 공백**: 신구 공식 문서 사이트(opensees.github.io, 구 opensees.berkeley.edu/wiki) 양쪽 모두에서 PETSc 전용 사용자 문서 페이지를 찾지 못했다(404) — [system 명령어](system-command.md) 목록에도 PETSc가 빠져 있다 ([출처: opensees-doc-system-command](../sources/opensees-doc-system-command.md)). 문서 이관 과정에서 누락됐거나, 실제 사용 빈도가 낮아 문서화 우선순위에서 밀렸을 가능성이 있다. 옵션·파라미터 상세는 확보하지 못함.

## 소스 코드 수준 확인 사항

`PetscSOE` 클래스(`OpenSees/SRC/system_of_eqn/linearSOE/petsc/PetscSOE.h`, 원작성자 McKenna/Fenves/Filippou, 1998년 작성)는 `LinearSOE`의 서브클래스로 PETSc의 `Mat`/`Vec`을 감싼다. `processID`, `numProcesses`, `numChannels`/`theChannels`, `startRow`/`endRow` 필드가 노출되어 있어, 행렬이 프로세스별로 행 단위 분산 저장·연산됨을 코드 수준에서 확인할 수 있다 ([출처: opensees-petscsoe-header-source](../sources/opensees-petscsoe-header-source.md)).

## 관련 페이지

- [system 명령어](system-command.md), [Mumps Solver](mumps-solver.md), [SuperLU Solver](superlu-solver.md)
- [OpenSees 병렬 컴퓨팅 개념](../concepts/opensees-parallel-computing.md)
