---
tags: [parallel, python, reference]
updated: 2026-07-06
sources: [openseespy-doc-parallel-commands]
---

# OpenSeesPy 병렬 명령어

OpenSeesPy의 병렬 처리 기능은 현재 **Linux 버전에서만 작동**한다 ([출처: openseespy-doc-parallel-commands](../sources/openseespy-doc-parallel-commands.md)).

## 실행 방법

```bash
mpiexec -np np python filename.py
```

```python
import openseespy.opensees as ops
```

프로세스 관리는 Python의 `mpi4py` 라이브러리가 담당하며, MPI가 `np`개의 Python 프로세스를 실행하고 각 프로세스가 자신의 OpenSeesPy 라이브러리 인스턴스를 호출하는 구조다. Tcl 기반 [OpenSeesMP](opensees-mp.md)와 개념적으로 동일하지만, 프로세스 간 통신을 Tcl 전용 명령 대신 mpi4py 함수(`Get_size`, `Get_rank` 등)로 처리한다는 점이 다르다.

## 명령어 목록 (13개)

`getPID`, `getNP`, `barrier`, `send`, `recv`, `Bcast`, `setStartNodeTag`, `domainChange`, Parallel Plain Numberer, Parallel RCM Numberer, MUMPS Solver, Parallel DisplacementControl, `partition`.

> ⚠️ **데이터 공백**: 이번에 확보한 WebFetch 추출본에는 각 명령어의 정확한 함수 시그니처와 예제 코드가 빠져 있다. 각 명령어를 실제로 사용하려면 [원본 문서](https://openseespydoc.readthedocs.io/en/latest/src/parallelcmds.html)에서 개별 페이지를 추가로 확인해야 한다.

## 흔한 문제 3가지

1. 불일치하는 send/recv로 인한 교착상태(deadlock)
2. 여러 프로세스가 동일 파일에 동시 쓰기를 시도해 생기는 경쟁 조건(race condition)
3. 부적절한 수동 도메인 분해로 인한 부하 불균형(load imbalance)

이 세 항목은 [OpenSeesMP](opensees-mp.md)의 Tcl 버전 흔한 함정과 정확히 동일하다 — 언어만 다를 뿐 병렬 프로그래밍 위험은 공유된다.

## 관련 페이지

- [OpenSeesMP](opensees-mp.md) — Tcl 기반 대응 애플리케이션
- [Parallel Numberer](parallel-numberer.md), [Mumps Solver](mumps-solver.md)
- [OpenSees 병렬 컴퓨팅 개념](../concepts/opensees-parallel-computing.md)
