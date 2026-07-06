---
tags: [parallel, python]
updated: 2026-07-06
sources: []
---

# Parallel Commands — OpenSeesPy Documentation (공식)

- 원본: [raw/openseespy-doc-parallel-commands.md](../../raw/openseespy-doc-parallel-commands.md)
- 출처: https://openseespydoc.readthedocs.io/en/latest/src/parallelcmds.html

## 핵심 요지

- 병렬 명령어는 현재 **Linux 버전에서만** 작동.
- 실행: `mpiexec -np np python filename.py`, `import openseespy.opensees as ops`로 사용.
- 흔한 3가지 문제: 불일치하는 send/recv로 인한 교착상태, 여러 프로세서의 동일 파일 동시 쓰기로 인한 경쟁 조건, 부적절한 모델 분해로 인한 부하 불균형.
- 관련 명령어 13개: getPID, getNP, barrier, send, recv, Bcast, setStartNodeTag, domainChange, Parallel Plain Numberer, Parallel RCM Numberer, MUMPS Solver, Parallel DisplacementControl, partition.
- 이 WebFetch 추출본에는 각 명령어의 상세 시그니처가 빠져 있음 — 필요시 원본 URL에서 개별 확인 필요.

## 기여한 위키 페이지

- [OpenSeesPy 병렬 명령어](../reference/openseespy-parallel-commands.md)
- [OpenSeesMP](../reference/opensees-mp.md)
