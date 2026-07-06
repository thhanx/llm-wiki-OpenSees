---
tags: [hpc, cluster, parallel]
updated: 2026-07-06
sources: []
---

# Building a computer cluster for OpenSees (José A. Abell, 2022)

- 원본: [raw/opensees-cluster-build-blog-abell2022.md](../../raw/opensees-cluster-build-blog-abell2022.md)
- 저자: José A. Abell (UANDES 토목공학 실험실, 칠레), 2022-06-07 블로그 포스트

## 핵심 요지

- 칠레 산티아고 근거리 지진 영향 연구용 소규모 Beowulf 클러스터(8 계산노드 + 1 로그인노드) 실제 구축기.
- **"OpenSees는 16개 프로세스를 초과하면 선형 확장성을 잃는다"**는 실무 경험(저자 + ASDEA의 Massimo Petarca 자문) — [OpenSeesSP 워크숍 슬라이드](opensees-workshop-openseessp-slides.md)의 Humboldt Bay Bridge 벤치마크(16개에서 이미 체감 곡선)와 독립적으로 일치하는 결론.
- 이 판단에 따라 단일 대형 서버보다 다수의 저렴한 노드로 분산 메모리 시스템을 구성하는 게 낫다고 결론.
- 하드웨어: 계산노드 8대 각각 수랭 Ryzen 9 5950X(16코어, 5.0GHz 오버클럭), 64GB DDR4, 500GB NVMe. 로그인노드는 Ryzen 9 5900X(12코어)+32GB. **GPU 미사용**("What GPU?").
- Intel Xeon(비쌈) vs AMD(Intel MKL과도 호환 확인되어 장벽 해소) vs Intel i9(P-코어/E-코어 혼재로 부적합) 비교 후 AMD Ryzen 선택.
- 네트워크: 기가비트 스위치로는 NVMe 쓰기 대역폭을 못 받쳐줘서 2.5GbE 스위치 + 10GbE SFP+ 업링크로 보강.
- 소프트웨어 스택: Ubuntu 22.04 + Ansible(프로비저닝) + SLURM(스케줄러) + OpenMPI + NFS. 자체 개발 `BeowulfInstaller` 도구로 설치 자동화.
- ShakeMaker(관련 시뮬레이션 도구)는 128코어 전체에서 선형 확장성 달성 확인, OpenSees 자체 테스트는 포스트 작성 시점엔 예정 상태.

## 기여한 위키 페이지

- [OpenSees용 클러스터 구축하기](../guides/building-a-cluster-for-opensees.md)
- [OpenSees 병렬 컴퓨팅 개념](../concepts/opensees-parallel-computing.md) (확장성 한계의 2차 독립 근거)
