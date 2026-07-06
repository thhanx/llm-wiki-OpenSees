---
tags: [solver, parallel, source-code]
updated: 2026-07-06
sources: []
---

# PetscSOE.h 소스 (Doxygen)

- 원본: [raw/opensees-petscsoe-header-source.md](../../raw/opensees-petscsoe-header-source.md)
- 출처: https://opensees.berkeley.edu/OpenSees/api/doxygen2/html/PetscSOE_8h-source.html (`OpenSees/SRC/system_of_eqn/linearSOE/petsc/PetscSOE.h`)

## 핵심 요지

- `PetscSOE`는 `LinearSOE`의 서브클래스로, PETSc의 `Mat`/`Vec`/`petscksp.h`를 감싸 OpenSees 연립방정식 인터페이스에 연결.
- 원작성자: Frank McKenna, Gregory L. Fenves, Filip C. Filippou (1998년 작성, 2005년 개정).
- 분산 처리 관련 필드 노출: `processID`, `numProcesses`, `numChannels`/`theChannels`(프로세스 간 통신 채널), `startRow`/`endRow`(행 단위 분산) — PETSc 솔버가 실제로 행렬을 프로세스 간에 분산 저장·계산함을 코드 수준에서 확인시켜줌.
- 공식 문서 사이트(신구 모두)에 PETSc 전용 사용자 문서 페이지가 없어, 이 소스 헤더가 현재 확보한 자료 중 PETSc 관련 유일한 1차 자료.

## 기여한 위키 페이지

- [PETSc Solver](../reference/petsc-solver.md)
