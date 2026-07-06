---
tags: [hpc, parallel]
updated: 2026-07-06
sources: []
---

# openseesSP — DesignSafe User Guide

- 원본: [raw/designsafe-guide-openseessp.md](../../raw/designsafe-guide-openseessp.md)
- 출처: https://designsafe-ci.org/user-guide/tools/simulation/opensees/openseesSP/ (curl + 자체 HTML 추출로 확보, WebFetch는 403)

## 핵심 요지

- OpenSeesSP를 "적은 부하 케이스를 가진 대형 모델"용으로 소개 — 순차 스크립트를 그대로 파싱하되 상태 결정과 방정식 풀이만 병렬화.
- 장점: HPC에서 실행 가능, 자동 도메인 분할이라 파티셔닝 관련 추가 스크립팅/지식 불필요.
- 단점: HPC 큐 대기, 모든 OpenSees 객체(요소·재료 등)가 안정적으로 지원되는 건 아님(sendSelf/recvSelf 구현 완성도에 의존), 다중 노드/요소 레코더 파일의 행 순서가 순차 실행과 다를 수 있음.
- 스크립트 변경 필요사항: `system Mumps` 또는 `system Diagonal`로 변경, 레코더 `-file` 대신 `-xml` 사용.
- Frank McKenna가 GitHub에 올린 예제 스크립트(`EXAMPLES/LargeSP/Example.tcl`) 인용.

## 기여한 위키 페이지

- [HPC에서 OpenSees 실행하기](../guides/running-opensees-on-hpc.md)
- [OpenSeesSP](../reference/opensees-sp.md)
