---
tags: [parallel, solver, benchmark]
updated: 2026-07-06
sources: []
---

# OpenSeesSP — Frank McKenna 워크숍 슬라이드

- 원본: [raw/opensees-workshop-openseessp-slides.pdf](../../raw/opensees-workshop-openseessp-slides.pdf)
- 저자: Frank McKenna, UC Berkeley — OpenSees Parallel Workshop, Berkeley, CA (13쪽 슬라이드)

## 핵심 요지

- OpenSeesSP의 설계 목표는 두 가지: (1) 입력 스크립트 변경 최소화(가능하면 0), (2) 병렬 처리 지식 요구 최소화.
- P0에서 단일 인터프리터가 입력 파일을 해석해 전체 Domain을 구성하고, P1~Pn-1은 Actor 객체로 대기하다가 지시를 받는다.
- `analyze` 호출 시 시스템 커맨드(예: `system Mumps`)에 따라 방정식이 병렬로 조립·분산되어 풀린다.
- **실제 벤치마크 (Humboldt Bay Bridge 모델)**: 프로세서 수 대비 총 실행시간 — 1개=약 315분, 2개=약 190분, 4개=약 125분, 8개=약 75분, 16개=약 48분. 곡선이 뚜렷하게 체감(diminishing returns)하는 모양으로, 16개 근처에서 이미 확장성이 크게 둔화됨을 실측으로 보여준다.
- 알려진 제약: 레코더 출력 컬럼 순서가 순차 실행과 다름(`-xml` 사용 권장), `eigen` 명령은 첫 `analyze()` 이후 작동하지 않으며 대형 문제에서는 메모리 문제로 그 이전에도 실패할 수 있음.
- **확장성 문제**: OpenSeesSP는 모든 노드/요소를 단일 프로세스 주소공간에 생성하므로 해석 가능한 문제 크기 자체에 상한이 있다. OpenSeesMP는 이 문제가 없다(처음부터 각 프로세스가 자기 서브도메인만 소유).

## 기여한 위키 페이지

- [OpenSeesSP](../reference/opensees-sp.md) (핵심 근거 — Humboldt Bay Bridge 벤치마크 포함)
- [OpenSees 병렬 컴퓨팅 개념](../concepts/opensees-parallel-computing.md) (확장성 한계 논의의 1차 실측 근거)
- [Mumps Solver](../reference/mumps-solver.md)
