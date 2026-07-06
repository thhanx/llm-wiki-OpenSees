---
tags: [architecture, concept, history]
updated: 2026-07-06
sources: [opensees-workshop-parallel-developers-slides, opensees-evolution-history-scott2014, wikipedia-opensees, opensees-doc-source-code-structure]
---

# OpenSees 아키텍처

OpenSees는 **객체지향(C++) 프레임워크**로, 개발자가 재료·요소·해석 알고리즘 등을 위한 인터페이스(추상 클래스)를 구현해 커스텀 애플리케이션을 만들 수 있게 설계되었다. 핵심은 `Domain`(모델 저장소) — `Analysis`(해석 절차) — `Element`/`Material` 등 개체 계층의 조합이다 ([출처: opensees-evolution-history-scott2014](../sources/opensees-evolution-history-scott2014.md), [wikipedia-opensees](../sources/wikipedia-opensees.md)).

## 기원과 역사

OpenSees는 Frank McKenna의 1997년 UC Berkeley 박사학위논문 "Object oriented finite element analysis: frameworks for analysis algorithms and parallel computing"(지도교수 Gregory Fenves)에서 출발했다. PEER(1997년 NSF 지원으로 설립)의 워킹그룹 번호를 따 처음엔 "G3"로 불렸으며, 2000년에 "OpenSees"로 개명됐다. Fenves 교수의 교육용 객체지향 MATLAB 프로그램 "G3" 전신 격인 "G2"가 그 이전 단계였다.

버전 관리는 G3 시절 zip 디스크 → 2006년 이전 CVS → 이후 SVN(체크인 권한은 소수 개발자만 보유)으로 발전했다. 지금은 GitHub(`github.com/OpenSees/OpenSees`)로 이관되어 있으며, 기여는 포크 후 충돌 없는 Pull Request만 허용된다 ([출처: opensees-doc-source-code-structure](../sources/opensees-doc-source-code-structure.md)).

## 병렬 처리를 가능케 하는 핵심 패턴 — send/recvSelf

도메인 분할로 서브도메인을 다른 프로세스로 옮기려면, 그 서브도메인에 속한 모든 객체(노드·요소·재료 등)를 직렬화해 네트워크로 전송할 수 있어야 한다. OpenSees는 이를 위해 최상위 `MovableObject` 클래스에 `sendSelf`/`recvSelf` 가상 함수를 정의하고, 모든 하위 클래스가 이를 구현하게 한다.

```
MovableObject
  ├─ sendSelf(Channel&, ...)
  └─ recvSelf(Channel&, ...)
       └─ Element (getTangent, getResidual, ...)
            └─ Truss (getTangent, getResidual, sendSelf, recvSelf, A, ...)
```

예를 들어 `Truss::sendSelf`는 자신의 tag·dimension·numDOF·단면적·밀도·연결 재료의 classTag/dbTag를 `Channel::sendVector`/`sendID`로 직렬화하고, 내부 재료 객체의 `sendSelf`를 재귀 호출한다. 수신 측 `recvSelf`는 역순으로 복원하되, 재료 객체 자체는 `FEM_ObjectBroker`(classTag로 올바른 클래스를 `new`해주는 팩토리)를 통해 재생성한다 ([출처: opensees-workshop-parallel-developers-slides](../sources/opensees-workshop-parallel-developers-slides.md)).

새 요소/재료 클래스를 병렬 처리와 호환되게 추가하려면:
1. `sendSelf`/`recvSelf` 구현
2. `FEM_ObjectBrokerAllClasses.cpp`에 생성 분기 추가
3. `SRC/classTags.h`에 고유 classTag 상수 추가(예: `#define ELE_TAG_Truss 4001`)

이 패턴이 [DomainPartitioner](../reference/domain-partitioner.md)가 서브도메인을 프로세스 간에 실제로 이동시킬 때 쓰는 하부 메커니즘이다.

## 규모 (2014년 기준)

Michael Scott의 2014년 발표에 따르면 단축재료 100종 이상(콘크리트 15, 철강 8, 이력거동 20종, p-y/q-z/t-z 15종, 조합용 유틸리티 래퍼 10종), 선형방정식 솔버 약 12종(희소/밀집·대칭/비대칭·직접/반복법), 시간적분기·근찾기 알고리즘 각 약 12종 규모였다 ([출처: opensees-evolution-history-scott2014](../sources/opensees-evolution-history-scott2014.md)). 이후로도 계속 성장했을 가능성이 높으나 최신 수치는 아직 ingest된 소스에 없다.

## 라이선스

UC Regents 소유. 교육·연구·비영리 목적의 비상업적 사용·복제·수정·배포는 무료로 허용되며, 그 외 단체는 내부 목적 사용만 허용(재배포 불가), 수정 후 재포장 판매는 금지된다.

## 관련 페이지

- [DomainPartitioner](../reference/domain-partitioner.md), [Domain Decomposition](domain-decomposition.md)
- [OpenSees 병렬 컴퓨팅 개념](opensees-parallel-computing.md)
- [OpenSees 개요](../overview.md)
