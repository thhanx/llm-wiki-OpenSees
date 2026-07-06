---
tags: [parallel, architecture, reference]
updated: 2026-07-06
sources: [opensees-domainpartitioner-api]
---

# DomainPartitioner

`DomainPartitioner`는 OpenSees에서 [도메인 분할](../concepts/domain-decomposition.md)을 관리하는 핵심 클래스다. 소스 위치: `OpenSees/SRC/domain/partitioner/` (`DomainPartitioner.h`/`.cpp`). `GraphPartitioner`(그래프 분할 알고리즘)와 `LoadBalancer`(부하 분산 전략)를 조합해 동작한다 ([출처: opensees-domainpartitioner-api](../sources/opensees-domainpartitioner-api.md)).

## 핵심 멤버 함수

| 함수 | 역할 |
|---|---|
| `partition(numParts, ...)` | 도메인을 지정된 개수의 서브도메인으로 분할 |
| `balance(theWeightedSubdomainGraph)` | 가중치 그래프를 기준으로 부하 재분배 |
| `getNumPartitions()` | 현재 파티션 개수 반환 |
| `getPartitionGraph()` / `getColoredGraph()` | 파티션 그래프 / 색칠된 요소 그래프 반환 |
| `swapVertex(from, to, vertexTag, ...)` | 한 파티션에서 다른 파티션으로 정점(노드/요소) 이동 |
| `swapBoundary(from, to, ...)` | 경계 노드 교환 |
| `releaseVertex(from, vertexTag, theWeightedPartitionGraph, ...)` | 정점을 해제해 더 가벼운 파티션으로 이동 |
| `releaseBoundary(from, theWeightedPartitionGraph, ...)` | 경계 노드를 일괄 해제 |

`swapVertex`/`releaseVertex` 계열 함수들은 정적 분할 이후에도 실행 중 동적으로 부하를 재조정할 수 있는 여지를 제공한다 — 다만 이 문서(2006년 Doxygen)에는 실제 실행 시점의 자동 재분배 정책까지는 나와 있지 않다.

## 관련 클래스

- `GraphPartitioner` — 실제 그래프 분할 알고리즘(예: METIS 계열)을 구현하는 인터페이스
- `LoadBalancer` — 부하 분산 정책
- `PartitionedDomain` — 분할된 도메인을 감싸는 컨테이너 (`OpenSeesSP`/`OpenSeesMP` 양쪽에서 사용)
- `ActorSubdomain` — 각 프로세스에서 자신이 맡은 서브도메인을 실행하는 액터 객체 ([OpenSeesSP](opensees-sp.md)에서 P0 이외 프로세스가 대기하는 형태로 등장)

## 관련 페이지

- [Domain Decomposition](../concepts/domain-decomposition.md) — 이 클래스가 구현하는 상위 개념
- [OpenSeesSP](opensees-sp.md), [OpenSeesMP](opensees-mp.md) — 이 클래스를 사용하는 두 병렬 애플리케이션
- [OpenSees 아키텍처](../concepts/opensees-architecture.md) — send/recvSelf 기반 객체 이동 메커니즘과의 연결
