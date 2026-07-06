---
tags: [parallel, reference]
updated: 2026-07-06
sources: [opensees-tn2007-16-parallel-processing, openseespy-doc-parallel-commands]
---

# Parallel Numberer (ParallelPlain / ParallelRCM)

도메인 분할(사용자 정의 파티셔닝) 해석을 할 때는 방정식 자유도(DOF) 번호를 프로세스 간에 일관되게 매기기 위해 **병렬 전용 numberer를 반드시 지정**해야 한다 ([출처: opensees-tn2007-16-parallel-processing](../sources/opensees-tn2007-16-parallel-processing.md)).

```
numberer ParallelPlain
numberer ParallelRCM
```

- `ParallelPlain` — 별도 최적화 없이 단순 순서로 번호 부여
- `ParallelRCM` — Reverse Cuthill-McKee 알고리즘으로 대역폭(bandwidth)을 줄이도록 재정렬 — TN-2007-16의 도메인 분할 예제 스크립트에서 실제로 사용됨

OpenSeesPy의 병렬 명령어 목록에도 "Parallel Plain Numberer"와 "Parallel RCM Numberer"가 그대로 대응된다 ([출처: openseespy-doc-parallel-commands](../sources/openseespy-doc-parallel-commands.md)).

## 병렬 system과의 짝

병렬 numberer는 항상 병렬 [system 명령어](system-command.md)(`ParallelBandGeneral`, `Mumps`, `Petsc` 등)와 함께 지정되어야 한다. 둘 중 하나만 지정하면 도메인 분할 해석이 올바르게 동작하지 않는다.

## 관련 페이지

- [Domain Decomposition](../concepts/domain-decomposition.md), [OpenSeesMP](opensees-mp.md)
- [system 명령어](system-command.md)
