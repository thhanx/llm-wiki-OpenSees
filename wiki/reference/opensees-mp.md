---
tags: [parallel, reference]
updated: 2026-07-06
sources: [opensees-tn2007-16-parallel-processing, opensees-workshop-parallel-developers-slides, openseespy-doc-parallel-commands]
---

# OpenSeesMP

**OpenSeesMP** ("Multiple Parallel Interpreter")는 OpenSees의 두 병렬 애플리케이션 중 하나로, **소~중규모 모델의 다수 해석(파라미터 스터디)** 또는 **사용자가 직접 제어하는 도메인 분할**을 위해 설계되었다 ([출처: opensees-tn2007-16-parallel-processing](../sources/opensees-tn2007-16-parallel-processing.md)).

## 동작 방식

[OpenSeesSP](opensees-sp.md)와 달리 master/actor 비대칭 구조가 아니라, **모든 프로세스가 동일하게 수정된 인터프리터를 실행**하는 대칭 구조다. 내부적으로 SP는 `SRC/tcl/mpiMain.cpp`(rank 0만 `g3TclMain` 실행, 나머지는 `runActors()` 대기)를 쓰지만, MP는 `SRC/tcl/mpiParameterMain.cpp`에서 모든 rank가 `g3TclMain(argc, argv, ..., np, rank)`를 동일하게 실행한다 ([출처: opensees-workshop-parallel-developers-slides](../sources/opensees-workshop-parallel-developers-slides.md)). 이 때문에 OpenSeesSP의 "단일 프로세스가 전체 모델을 먼저 만든다"는 확장성 병목이 OpenSeesMP에는 없다.

## 두 가지 사용 모드

### 1) 파라미터 스터디 모드 (`-par` 옵션)

커맨드라인에 `-par parName parFile`을 지정하면, 각 프로세스가 파라미터 조합을 modulo 방식(고유 번호 % 전체 프로세스 수)으로 나눠 맡아 자동 병렬화한다.

```
mpirun -np 1024 OpenSeesMP main.tcl -par gMotion records.txt
```

TN-2007-16의 실제 예제는 PEER 지진동 데이터베이스의 7200개 레코드를 1024개 프로세스로 나눠 처리하는 시나리오를 보여준다.

### 2) 사용자 정의 도메인 분할 모드

추가 명령어로 각 프로세스가 자신의 rank/전체 프로세스 수를 알고, 이를 바탕으로 직접 서브도메인을 만든다:

- `getPID` — 0 ~ [`getNP`-1] 범위의 고유 프로세스 번호 반환
- `getNP` — 전체 프로세스 수 반환
- `send -pid $pid $data` / `recv -pid $pid variableName` — 프로세스 간 데이터 송수신 (recv의 pid를 ANY로 지정하면 아무 프로세스에서나 수신)
- `barrier` — 모든 프로세스가 이 지점에 도달할 때까지 대기

이 모드를 쓸 경우 병렬 `system`(예: `ParallelBandGeneral`, `Mumps`, `Petsc`)과 병렬 `numberer`(`ParallelPlain`, `ParallelRCM`)를 **반드시** 지정해야 한다 ([Domain Decomposition](../concepts/domain-decomposition.md), [Parallel Numberer](parallel-numberer.md) 참고).

OpenSeesPy에서도 동일한 패턴이 mpi4py 기반으로 제공되며, 13개 병렬 명령(getPID, getNP, barrier, send, recv, Bcast, setStartNodeTag, domainChange, Parallel Plain/RCM Numberer, MUMPS Solver, Parallel DisplacementControl, partition)이 대응한다 — 단 **Linux 버전에서만 작동**한다 ([출처: openseespy-doc-parallel-commands](../sources/openseespy-doc-parallel-commands.md), [OpenSeesPy 병렬 명령어](openseespy-parallel-commands.md)).

## 흔한 함정

- 불일치하는 send/recv 조합으로 인한 교착상태(deadlock)
- 여러 프로세스가 동일 파일에 동시 쓰기를 시도해 발생하는 경쟁 조건(race condition)
- 부적절한 수동 도메인 분해로 인한 부하 불균형(load imbalance)

## 성능 팁

파일시스템이 병목이 되는 대형 파라미터 스터디에서는:
- 반복해서 읽는 파일은 실행 전에 로컬 `/scratch`로 복사
- 같은 모델을 반복 구성할 땐 `wipe` 후 재소싱하는 대신 `remove loadPattern`/`remove recorders` + `reset`을 사용해 모델을 한 번만 만들기

## 관련 페이지

- [OpenSeesSP](opensees-sp.md) — 대안 애플리케이션 (자동 분할, 단일 대형 모델)
- [OpenSeesPy 병렬 명령어](openseespy-parallel-commands.md)
- [Domain Decomposition](../concepts/domain-decomposition.md), [DomainPartitioner](domain-partitioner.md)
- [Parallel Numberer](parallel-numberer.md), [system 명령어](system-command.md)
- [OpenSees 병렬 컴퓨팅 개념](../concepts/opensees-parallel-computing.md)
