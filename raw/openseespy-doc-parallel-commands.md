---
title: "11. Parallel Commands — OpenSeesPy Documentation"
source_url: https://openseespydoc.readthedocs.io/en/latest/src/parallelcmds.html
retrieved: 2026-07-06
type: official-doc
method: WebFetch (명령어 목록만 반환됨 — 각 함수 시그니처/예제는 원본 URL에서 추가 확인 필요)
---

# OpenSeesPy 병렬 명령어 (Parallel Commands)

## 플랫폼 제한사항

"The parallel commands are currently only working in the Linux version"

## 실행 방식

```
mpiexec -np np python filename.py
```
(`np`는 프로세서 수, `python`은 인터프리터, `filename.py`는 스크립트명)

## 임포트 방식

```python
import openseespy.opensees as ops
```

## 주의사항 — 흔한 문제 3가지

1. 불일치하는 송수신으로 인한 교착 상태 (deadlock)
2. 여러 프로세서에서 동일 파일에 동시 쓰기로 인한 경쟁 조건 (race condition)
3. 부적절한 모델 분해로 인한 부하 불균형 (load imbalance)

## 관련 명령어 목록 (13개)

- getPID
- getNP
- barrier
- send
- recv
- Bcast
- setStartNodeTag
- domainChange
- Parallel Plain Numberer
- Parallel RCM Numberer
- MUMPS Solver
- Parallel DisplacementControl
- partition

**제한사항**: 이 WebFetch 추출에는 각 명령어의 상세 함수 시그니처, 설명, 예제 코드가 포함되지 않음. 필요 시 원본 URL(https://openseespydoc.readthedocs.io/en/latest/src/parallelcmds.html)에서 각 명령어 개별 페이지를 추가로 확인할 것.
