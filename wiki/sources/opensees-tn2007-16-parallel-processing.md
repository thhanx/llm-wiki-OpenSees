---
tags: [parallel, solver, hpc]
updated: 2026-07-06
sources: []
---

# TN-2007-16: Using the OpenSees Interpreter on Parallel Computers

- 원본: [raw/opensees-tn2007-16-parallel-processing.pdf](../../raw/opensees-tn2007-16-parallel-processing.pdf)
- 저자: Frank McKenna, Gregory L. Fenves (UC Berkeley) — NEESit Technical Note, 2008-04-17, v1.0
- 종류: 공식 기술 노트 (16쪽) — OpenSees 병렬 처리에 관한 가장 포괄적인 공식 문서

## 핵심 요지

- OpenSees는 두 가지 병렬 인터프리터를 제공한다: **OpenSeesSP**(단일 병렬 인터프리터, 자동 도메인 분할)와 **OpenSeesMP**(다중 병렬 인터프리터, 사용자 제어 병렬화).
- OpenSeesSP는 `analyze()` 최초 호출 시점에 모델을 자동으로 파티셔닝한다. 파티셔닝 전까지는 P0만 모델을 소유하고 나머지 프로세스는 ActorSubdomain으로 대기.
- 병렬 솔버는 Mumps, Petsc, (Distributed)SuperLU 세 가지가 있으며 이 중 실제로 방정식을 병렬로 푸는 것은 이 세 개뿐 — 나머지 솔버는 시스템을 P0로 모아 순차적으로 푼다. 도메인 분할 시 **반드시 병렬 system과 병렬 numberer를 지정**해야 한다.
- Mumps가 일반적으로 성능이 가장 좋지만 저작권 문제로 별도 설치가 필요하다. SuperLU는 항상 포함되어 있다.
- OpenSeesMP는 `-par parName parFile` 커맨드라인 옵션으로 파라미터 스터디를 modulo 방식으로 자동 병렬화할 수 있고, `getPID`/`getNP`/`send`/`recv`/`barrier` 명령으로 사용자가 직접 도메인 분할도 구현할 수 있다.
- 레코더 출력 시 `-file` 대신 `-xml`을 써야 한다(병렬 실행 시 컬럼 순서가 순차 실행과 다를 수 있음).
- 성능 팁: 파일시스템 병목 방지를 위해 반복 읽기 파일은 로컬 `/scratch`에 복사하고, 모델을 반복 생성할 땐 `wipe` 대신 `reset`+`remove loadPattern`+`remove recorders` 사용.
- 실제 대규모 실행 예시로 `mpirun -np 1024`가 명시되어 있어 대규모 파라미터 스터디에 실전 사용된 정황을 보여줌.

## 기여한 위키 페이지

- [OpenSeesSP](../reference/opensees-sp.md), [OpenSeesMP](../reference/opensees-mp.md)
- [Domain Decomposition](../concepts/domain-decomposition.md), [Parallel Numberer](../reference/parallel-numberer.md)
- [Mumps Solver](../reference/mumps-solver.md), [SuperLU Solver](../reference/superlu-solver.md), [PETSc Solver](../reference/petsc-solver.md), [system 명령어](../reference/system-command.md)
- [OpenSees 병렬 컴퓨팅 개념](../concepts/opensees-parallel-computing.md)
- [HPC에서 OpenSees 실행하기](../guides/running-opensees-on-hpc.md)
