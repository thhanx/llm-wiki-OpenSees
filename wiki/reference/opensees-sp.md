---
tags: [parallel, reference]
updated: 2026-07-06
sources: [opensees-tn2007-16-parallel-processing, opensees-workshop-openseessp-slides, designsafe-guide-openseessp]
---

# OpenSeesSP

**OpenSeesSP** ("Single Parallel Interpreter")는 OpenSees의 두 병렬 애플리케이션 중 하나로, **대형 모델을 적은 부하 케이스로 해석**하는 용도로 설계되었다. 설계 목표는 (1) 입력 스크립트 변경을 최소화(가능하면 0)하고 (2) 사용자에게 병렬 처리 지식을 거의 요구하지 않는 것이다 ([출처: opensees-workshop-openseessp-slides](../sources/opensees-workshop-openseessp-slides.md)).

## 동작 방식

프로세스 P0가 단일 인터프리터로 입력 스크립트를 해석해 전체 `Domain`을 구성하는 동안, 나머지 프로세스 P1~Pn-1은 `ActorSubdomain` 객체로 대기한다. `analyze()`가 처음 호출되는 시점에 모델이 자동으로 파티셔닝되어 각 서브도메인이 해당 프로세스로 분배되고, 이후 상태 결정(state determination)과 방정식 풀이가 병렬로 수행된다 ([출처: opensees-tn2007-16-parallel-processing](../sources/opensees-tn2007-16-parallel-processing.md)).

모델을 만드는 스크립트 자체는 순차 실행 스크립트와 **완전히 동일**하다 — 별도의 파티셔닝 커맨드가 필요 없다는 점이 OpenSeesMP와의 근본적 차이다 (아래 [OpenSeesMP](opensees-mp.md) 비교 참고).

## 스크립트에서 바꿔야 할 것

- `system` 커맨드를 병렬 솔버로 변경: `system Mumps` 또는 `system Diagonal` 등 ([system 명령어](system-command.md), [Mumps Solver](mumps-solver.md) 참고).
- 레코더 출력 플래그를 `-file`에서 `-xml`로 변경 — 병렬 실행 시 다중 노드/요소 레코더의 컬럼(행) 순서가 순차 실행과 다르게 나올 수 있기 때문 ([출처: opensees-workshop-openseessp-slides](../sources/opensees-workshop-openseessp-slides.md)).

## 알려진 제약

- `eigen` 명령은 첫 `analyze()` 호출 이후 작동하지 않으며, 대형 문제에서는 메모리 요구량 때문에 그 이전에도 실패할 수 있다.
- **확장성 한계**: 모든 노드·요소를 P0 하나의 프로세스 주소공간에 우선 생성하므로, 해석 가능한 문제 크기 자체가 이 단일 프로세스의 메모리로 제한된다. [OpenSeesMP](opensees-mp.md)는 처음부터 각 프로세스가 자기 서브도메인만 소유하므로 이 문제가 없다.
- OpenSees 객체(요소·재료 등)의 `sendSelf`/`recvSelf` 구현 완성도에 따라 안정성이 달라진다 — 모든 객체가 완전히 검증된 것은 아니다 ([출처: designsafe-guide-openseessp](../sources/designsafe-guide-openseessp.md)).

## 실측 벤치마크 — Humboldt Bay Bridge 모델

OpenSeesSP 워크숍 슬라이드에 실린 실제 벤치마크(총 실행시간 vs 프로세서 수):

| 프로세서 수 | 총 실행시간(분) |
|---|---|
| 1 | ~315 |
| 2 | ~190 |
| 4 | ~125 |
| 8 | ~75 |
| 16 | ~48 |

곡선이 16개 근처에서 이미 뚜렷하게 체감(diminishing returns)하는 형태다. 이는 [José Abell의 클러스터 구축 경험](../guides/building-a-cluster-for-opensees.md)("16개 프로세스 초과 시 선형 확장성 상실")과 독립적으로 일치하는 결과로, [OpenSees 병렬 컴퓨팅 개념](../concepts/opensees-parallel-computing.md)의 핵심 근거다.

## 실행 방법

```
mpiexec -np numProcs OpenSeesSP inputFile.tcl
```

## 관련 페이지

- [OpenSeesMP](opensees-mp.md) — 대안 애플리케이션 (파라미터 스터디·사용자 정의 분할)
- [Domain Decomposition](../concepts/domain-decomposition.md), [DomainPartitioner](domain-partitioner.md)
- [system 명령어](system-command.md), [Mumps Solver](mumps-solver.md)
- [OpenSees 병렬 컴퓨팅 개념](../concepts/opensees-parallel-computing.md)
- [HPC에서 OpenSees 실행하기](../guides/running-opensees-on-hpc.md)
