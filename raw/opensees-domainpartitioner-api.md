---
title: "OpenSees: DomainPartitioner Class Reference (Doxygen)"
source_url: https://opensees.berkeley.edu/OpenSees/api/doxygen2/html/classDomainPartitioner.html
retrieved: 2026-07-06
type: api-reference
method: WebFetch
---

# OpenSees: DomainPartitioner 클래스 참조

## 개요

`DomainPartitioner` 클래스는 OpenSees의 병렬 도메인 분할을 관리하는 클래스입니다. 헤더 파일은 `DomainPartitioner.h`이며, 구현은 `DomainPartitioner.cpp`에 있습니다.
소스 위치: `OpenSees/SRC/domain/partitioner/`

## 공개 멤버 함수

### 생성자 및 소멸자

**DomainPartitioner (GraphPartitioner 및 LoadBalancer 포함)**
- `DomainPartitioner(GraphPartitioner &theGraphPartitioner, LoadBalancer &theLoadBalancer)`
- 정의: `DomainPartitioner.cpp` 107번 줄

**DomainPartitioner (GraphPartitioner만)**
- `DomainPartitioner(GraphPartitioner &theGraphPartitioner)`
- 정의: `DomainPartitioner.cpp` 99번 줄

**소멸자**
- `virtual ~DomainPartitioner()`
- 정의: `DomainPartitioner.cpp` 118번 줄

## 주요 멤버 함수

### setPartitionedDomain
```cpp
virtual void setPartitionedDomain(PartitionedDomain &theDomain)
```
정의: `DomainPartitioner.cpp` 129번 줄. `PartitionedDomain::partition()`에서 참조됨.

### partition
```cpp
virtual int partition(int numParts, bool useMainDomain=false, int mainPartition=0)
```
정의: `DomainPartitioner.cpp` 135번 줄. 도메인을 지정된 개수의 부분 도메인으로 분할.
참조 항목: `TaggedObjectStorage::addComponent()`, `Domain::addElement()`, `NodeLocations::addPartition()`, `GraphPartitioner::partition()` 등.
`PartitionedDomain::partition()` 및 `releaseVertex()`에서 호출됨.

### balance
```cpp
virtual int balance(Graph &theWeightedSubdomainGraph)
```
정의: `DomainPartitioner.cpp` 507번 줄. 참조: `LoadBalancer::balance()`, `Domain::domainChange()`.
`PartitionedDomain::commit()`에서 호출됨.

### getNumPartitions
```cpp
virtual int getNumPartitions(void) const
```
정의: `DomainPartitioner.cpp` 540번 줄. 현재 분할된 부분 도메인의 개수 반환.

### getPartitionGraph
```cpp
virtual Graph& getPartitionGraph(void)
```
정의: `DomainPartitioner.cpp` 548번 줄. 부분 도메인 그래프 반환.

### getColoredGraph
```cpp
virtual Graph& getColoredGraph(void)
```
정의: `DomainPartitioner.cpp` 559번 줄. 색상이 지정된 요소 그래프 반환.

### swapVertex
```cpp
virtual int swapVertex(int from, int to, int vertexTag, bool adjacentVertexNotInOther=true)
```
정의: `DomainPartitioner.cpp` 572번 줄. 한 부분 도메인에서 다른 부분 도메인으로 정점 이동.

### swapBoundary
```cpp
virtual int swapBoundary(int from, int to, bool adjacentVertexNotInOther=true)
```
정의: `DomainPartitioner.cpp` 909번 줄. 경계 노드 교환.

### releaseVertex
```cpp
virtual int releaseVertex(int from, int vertexTag, Graph &theWeightedPartitionGraph,
                          bool mustReleaseToLighter=true, double factorGreater=1.0,
                          bool adjacentVertexNotInOther=true)
```
정의: `DomainPartitioner.cpp` 1290번 줄. 한 부분에서 정점을 해제하고 더 가벼운 부분으로 이동.

### releaseBoundary
```cpp
virtual int releaseBoundary(int from, Graph &theWeightedPartitionGraph,
                            bool mustReleaseToLighter=true, double factorGreater=1.0,
                            bool adjacentVertexNotInOther=true)
```
정의: `DomainPartitioner.cpp` 1374번 줄. 경계 노드 해제.

## 관련 클래스

- **GraphPartitioner**: 그래프 분할 알고리즘 구현
- **LoadBalancer**: 부하 분산
- **PartitionedDomain**: 분할된 도메인
- **Graph**: 그래프 구조
- **Vertex**: 그래프의 정점

## 문서 생성 정보

생성일: 2006년 10월 23일, Doxygen 1.5.0
