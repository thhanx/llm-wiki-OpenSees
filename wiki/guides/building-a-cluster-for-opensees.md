---
tags: [hpc, cluster, guide]
updated: 2026-07-06
sources: [opensees-cluster-build-blog-abell2022]
---

# OpenSees용 클러스터 구축하기 — 실전 사례

José A. Abell(UANDES 토목공학 실험실, 칠레)이 2022년에 기록한 실제 8노드 Beowulf 클러스터 구축 경험 ([출처: opensees-cluster-build-blog-abell2022](../sources/opensees-cluster-build-blog-abell2022.md)).

## 설계 결정의 출발점 — 확장성 한계

저자와 ASDEA의 Massimo Petarca가 공유한 실무 판단: **"OpenSees는 16개 프로세스를 초과하면 선형 확장성을 잃는다."** 이는 [OpenSeesSP 워크숍 슬라이드의 Humboldt Bay Bridge 실측 벤치마크](../reference/opensees-sp.md)와 독립적으로 일치하는 결론이다 (자세한 논의는 [OpenSees 병렬 컴퓨팅 개념](../concepts/opensees-parallel-computing.md) 참고).

이 판단으로부터 **"하나의 큰 서버보다 다수의 저렴한 노드로 분산 메모리 시스템을 구성하는 것이 낫다"**는 하드웨어 전략이 도출됐다 — 프로세스당 16개 근처에서 이득이 꺾인다면, 한 서버에 코어를 더 우겨넣기보다 여러 서버에 나눠 여러 개의 "16개 이하" 작업을 동시에 돌리는 편이 총처리량(throughput) 관점에서 유리하기 때문이다.

## 프로세서 선택

| 후보 | 판단 |
|---|---|
| Intel Xeon | MKL·ECC·안정성 장점, 비용이 높고 단일 보드 성능 향상 제한적 |
| AMD Threadripper(32코어) | 코어 수 과다 |
| **AMD Ryzen 9 5950X(16코어)** ← 채택 | 클록 속도 높고 비용 효율적, **Intel MKL이 AMD에서도 최적화 작동 확인**되어 장벽 해소 |
| Intel i9 | 성능 코어/효율 코어 혼재로 순수 연산에 부적합 |

메모리는 DDR5가 아직 미성숙·고비용이라 DDR4 선택.

## 최종 사양

- **계산노드 8대**: 수랭 Ryzen 9 5950X(16코어, 5.0GHz+ 오버클럭), 64GB DDR4, 500GB NVMe, 2.5GbE
- **로그인/메인 노드**: Ryzen 9 5900X(12코어), 32GB RAM, 2TB NVMe(홈 폴더)
- **GPU: 사용 안 함** ("What GPU?") — 이 클러스터는 순수 CPU/MPI 병렬화 전략

## 네트워크

기가비트 스위치로는 NVMe의 실제 쓰기 대역폭을 감당 못해, QNAP QSW-M2108-2C(2.5GbE, 10GbE SFP+ 업링크 2포트) 스위치 2대로 보강.

## 소프트웨어 스택

Ubuntu 22.04 + Ansible(프로비저닝) + **SLURM**(작업 스케줄러) + **OpenMPI** + NFS(네트워크 공유 폴더). 자체 개발 도구 `BeowulfInstaller`로 설치 자동화.

## 검증 현황 (포스트 작성 시점)

관련 시뮬레이션 도구 ShakeMaker는 128코어 전체에서 선형 확장성을 확인했으나, OpenSees 자체의 전체 노드 벤치마크는 작성 시점엔 예정 상태였음(이후 결과는 미확보).

> ⚠️ **참고**: 이 사례는 특정 연구 그룹(칠레 산티아고 근거리 지진 연구)의 예산·용도에 맞춘 결정이다. SLURM/OpenMPI 조합, 코어당 4GB RAM 배분, GPU 미사용 등은 참고할 만한 패턴이지만 이 프로젝트의 목적(OpenSees 최적화/병렬화)에 그대로 적용하기 전에 자체 요구사항(문제 규모, 예산, 기존 인프라)을 따져봐야 한다.

## 관련 페이지

- [OpenSees 병렬 컴퓨팅 개념](../concepts/opensees-parallel-computing.md) — 확장성 한계의 두 번째 독립 근거
- [HPC에서 OpenSees 실행하기](running-opensees-on-hpc.md)
- [MPI로 OpenSees 빌드하기](building-opensees-with-mpi.md)
