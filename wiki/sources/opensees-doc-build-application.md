---
tags: [build, mpi, hpc]
updated: 2026-07-06
sources: []
---

# Building Application — OpenSees Documentation (공식, 현행)

- 원본: [raw/opensees-doc-build-application.md](../../raw/opensees-doc-build-application.md)
- 출처: https://opensees.github.io/OpenSeesDocumentation/developer/build.html

## 핵심 요지

- CMake(3.20+) + Conan(1.64.1, Windows/Linux) 또는 Homebrew(Mac) 기반의 현행 빌드 체계. Python 3.11 필수(3.12+ 미지원).
- 플랫폼별 컴파일러: Windows(MSVC+Intel oneAPI Fortran), Mac(AppleClang+gfortran), Linux(gcc/g++/gfortran).
- **MUMPS는 OpenSees의 병렬 솔버로서 필수 요구사항**으로 명시 — 별도로 클론·빌드(`github.com/OpenSees/mumps`) 후 `-DMUMPS_DIR`로 연결.
- MPI 라이브러리(Windows: Intel MPI, Mac/Linux: Open-MPI)는 OpenSeesMP/OpenSeesMPI 빌드에 모든 플랫폼에서 필수.
- 각 플랫폼별 상세 빌드 절차(패키지 설치 → MUMPS 빌드 → OpenSees/OpenSeesPy 타깃 빌드)와 흔한 문제(Conan zlib 불일치, Python 버전 불일치로 인한 segfault, libiomp5md.dll 누락 등) 정리.
- 병렬 빌드는 `-j` 옵션(예: `-j8`)으로 가속.

## 기여한 위키 페이지

- [MPI로 OpenSees 빌드하기](../guides/building-opensees-with-mpi.md)
- [Mumps Solver](../reference/mumps-solver.md)
