---
tags: [parallel, architecture, development]
updated: 2026-07-06
sources: []
---

# Parallel OpenSees Applications for Developers — Frank McKenna 워크숍 슬라이드

- 원본: [raw/opensees-workshop-parallel-developers-slides.pdf](../../raw/opensees-workshop-parallel-developers-slides.pdf)
- 저자: Frank McKenna, UC Berkeley — OpenSees Parallel Workshop, Berkeley, CA (13쪽 슬라이드, 개발자 대상)

## 핵심 요지

- 새 클래스(재료·요소 등)를 병렬 처리와 호환되게 추가하려면: (1) `sendSelf`/`recvSelf` 구현, (2) `FEM_ObjectBrokerAllClasses.cpp` 수정, (3) `SRC/classTags.h`에 고유 classTag 추가.
- `Truss::sendSelf`/`recvSelf` 실제 코드 예시: tag, dimension, numDOF, 단면적 A, 밀도 rho, 재료의 classTag+dbTag를 `Channel`의 `sendVector`/`sendID`로 직렬화하고, 내부 재료 객체 자신의 `sendSelf`를 재귀 호출한다. 이 send/recvSelf 패턴이 도메인 분할 시 서브도메인 간 객체 이동의 실제 메커니즘이다.
- **OpenSeesSP** 메인(`SRC/tcl/mpiMain.cpp`): rank 0만 `g3TclMain` 인터프리터를 실행하고, 나머지는 `theMachine.runActors()`로 대기하는 master/actor 비대칭 구조.
- **OpenSeesMP** 메인(`SRC/tcl/mpiParameterMain.cpp`): 모든 rank가 동일하게 `g3TclMain(argc, argv, ..., np, rank)`를 실행하는 대칭 구조 — SP와의 근본적 아키텍처 차이.
- 대부분의 병렬 커맨드/수정은 `SRC/tcl/commands.cpp`에 있으며, OpenSeesSP 코드는 `#ifdef _PARALLEL_PROCESSING`, OpenSeesMP 코드는 `#ifdef _PARALLEL_INTERPRETERS` 전처리 지시자로 분리되어 있다.
- 자체 병렬 애플리케이션을 작성하려면 "기존 것에서 시작하라(Start with an existing one!)"는 것이 유일한 실전 조언.

## 기여한 위키 페이지

- [OpenSees 아키텍처](../concepts/opensees-architecture.md) (send/recvSelf 패턴, classTag 레지스트리)
- [OpenSeesSP](../reference/opensees-sp.md), [OpenSeesMP](../reference/opensees-mp.md) (내부 구현 차이)
- [DomainPartitioner](../reference/domain-partitioner.md)
