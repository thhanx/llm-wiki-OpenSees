---
tags: [parallel, concept]
updated: 2026-07-06
sources: [opensees-tn2007-16-parallel-processing, opensees-domainpartitioner-api]
---

# Domain Decomposition (도메인 분할)

도메인 분할은 유한요소 모델(노드·요소·하중)을 여러 서브도메인으로 나눠 각 서브도메인을 별도 프로세스에 할당함으로써, 상태 결정(state determination)과 방정식 풀이를 병렬로 수행하는 기법이다. OpenSees 병렬 처리의 핵심 메커니즘이다 ([출처: opensees-tn2007-16-parallel-processing](../sources/opensees-tn2007-16-parallel-processing.md)).

## 두 가지 분할 경로

| | [OpenSeesSP](../reference/opensees-sp.md) | [OpenSeesMP](../reference/opensees-mp.md) |
|---|---|---|
| 분할 시점 | `analyze()` 최초 호출 시 자동 | 사용자가 스크립트에서 직접(`getPID`/`getNP` 기반) |
| 스크립트 변경 | 거의 없음(솔버·레코더만) | 모델 생성 로직 자체를 프로세스별로 분기해야 함 |
| 요구 지식 | 낮음 | 병렬 프로그래밍 지식 필요 |
| 초기 모델 생성 위치 | P0 단일 프로세스(확장성 병목) | 각 프로세스가 처음부터 자기 몫만 생성 |

## 구현 골격 — DomainPartitioner

실제 분할은 [`DomainPartitioner`](../reference/domain-partitioner.md) 클래스가 `GraphPartitioner`(그래프 분할 알고리즘)와 `LoadBalancer`(부하 분산)를 조합해 수행한다. `partition()`으로 초기 분할을, `balance()`/`swapVertex()`/`releaseVertex()` 계열로 실행 중 재분배를 담당한다 ([출처: opensees-domainpartitioner-api](../sources/opensees-domainpartitioner-api.md)).

## 반드시 지정해야 하는 두 가지 설정

도메인 분할 해석을 하려면 다음 두 가지를 **반드시 함께** 지정해야 한다:

1. **병렬 [system](../reference/system-command.md)** — `Mumps`, `Petsc`, 또는 분산 `SuperLU`(`SparseGEN`이 자동 전환됨). 이 셋만 실제로 방정식을 병렬로 푼다. 다른 솔버는 시스템을 P0로 모아 순차 처리한다.
2. **병렬 [numberer](../reference/parallel-numberer.md)** — `ParallelPlain` 또는 `ParallelRCM`.

둘 중 하나라도 빠지면 도메인 분할 해석이 올바르게 동작하지 않는다.

## OpenSeesMP의 사용자 정의 분할 예제 골격

```tcl
set pid [getPID]
set np [getNP]

source modelP.tcl
doModel {$pid $np}          ;# 프로세스별로 자기 몫의 노드/요소만 생성

system ParallelBandGeneral
constraints Transformation
numberer ParallelRCM
test NormDispIncr 1.0e-12 10 3
algorithm Newton
integrator LoadControl 0.1
analysis Static

set ok [analyze 10]
```

## 관련 페이지

- [OpenSees 병렬 컴퓨팅 개념](opensees-parallel-computing.md) — 상위 종합 페이지
- [OpenSeesSP](../reference/opensees-sp.md), [OpenSeesMP](../reference/opensees-mp.md)
- [DomainPartitioner](../reference/domain-partitioner.md), [system 명령어](../reference/system-command.md), [Parallel Numberer](../reference/parallel-numberer.md)
