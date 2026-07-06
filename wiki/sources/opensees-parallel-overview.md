---
tags: [parallel]
updated: 2026-07-06
sources: []
---

# OpenSees Parallel Processing — 공식 개요 페이지

- 원본: [raw/opensees-parallel-overview.md](../../raw/opensees-parallel-overview.md)
- 출처: https://opensees.berkeley.edu/OpenSees/parallel/parallel.php (공식 사이트)

## 핵심 요지

- OpenSeesSP(대형 모델 해석용)와 OpenSeesMP(파라미터 스터디·사용자 정의 분할용) 두 애플리케이션 소개.
- 두 애플리케이션 모두 NSF 지원 XSEDE 머신(TACC Stampede2, Frontera)에서 실행 가능하며, 비-XSEDE 사용자는 DesignSafe-ci(NSF NHERI 시설, 무료 가입)를 통해 접근 가능.
- 실행 커맨드: `mpiexec -np numProcs applicationName inputFile`.
- 오래된 Windows/Mac 바이너리 다운로드 링크 제공(MPICH2/OpenMPI 필요).
- 경고: "이 애플리케이션들은 새것이며 제한된 예제로만 검증되었다" — 버그 보고 권장.

## 기여한 위키 페이지

- [OpenSees 병렬 컴퓨팅 개념](../concepts/opensees-parallel-computing.md)
- [HPC에서 OpenSees 실행하기](../guides/running-opensees-on-hpc.md)
