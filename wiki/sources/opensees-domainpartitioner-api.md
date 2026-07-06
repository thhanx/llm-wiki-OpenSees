---
tags: [parallel, architecture, api]
updated: 2026-07-06
sources: []
---

# DomainPartitioner Class Reference (Doxygen)

- 원본: [raw/opensees-domainpartitioner-api.md](../../raw/opensees-domainpartitioner-api.md)
- 출처: https://opensees.berkeley.edu/OpenSees/api/doxygen2/html/classDomainPartitioner.html (`OpenSees/SRC/domain/partitioner/`)

## 핵심 요지

- `DomainPartitioner`는 `GraphPartitioner`(그래프 분할 알고리즘)와 `LoadBalancer`(부하 분산)를 조합해 도메인을 분할하는 클래스.
- 핵심 메서드: `partition(numParts, ...)`(지정 개수로 분할), `balance(graph)`(재분배), `getPartitionGraph`/`getColoredGraph`, `swapVertex`/`swapBoundary`/`releaseVertex`/`releaseBoundary`(정점 단위 재배치로 부하 재조정).
- 생성 시점: 2006년, Doxygen 1.5.0 — 매우 오래된 문서지만 클래스 인터페이스 자체는 OpenSees 병렬 처리의 핵심 골격으로 지금도 유효.

## 기여한 위키 페이지

- [DomainPartitioner](../reference/domain-partitioner.md)
- [Domain Decomposition](../concepts/domain-decomposition.md)
