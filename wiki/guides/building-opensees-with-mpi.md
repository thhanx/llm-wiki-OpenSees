---
tags: [build, mpi, hpc, guide]
updated: 2026-07-06
sources: [opensees-doc-build-application, opensees-tn2007-16-parallel-processing]
---

# MPI로 OpenSees 빌드하기

병렬 애플리케이션(OpenSeesSP/OpenSeesMP)을 쓰려면 MPI와 MUMPS를 포함해 직접 빌드해야 한다. 두 세대의 빌드 절차가 존재한다 — 최신 CMake 기반 방식과, HPC 클러스터에서 여전히 마주칠 수 있는 구식 Makefile.def 방식.

## 현행 방식 (CMake + Conan/Homebrew)

공식 빌드 문서([출처: opensees-doc-build-application](../sources/opensees-doc-build-application.md)) 기준 요구사항:

- CMake 3.20+, Git, C/C++/Fortran 컴파일러, Python 3.11(3.12+ 미지원)
- **MUMPS — OpenSees의 병렬 솔버로서 필수 요구사항**
- Linux: gcc/g++/gfortran + Conan 1.64.1(2.0 미지원) + `libopenmpi-dev`, `liblapack-dev`, `libscalapack-openmpi-dev`, `libmkl-rt`

### MUMPS 먼저 빌드

```bash
git clone https://github.com/OpenSees/mumps.git
cd mumps && mkdir build && cd build
cmake .. -Darith=d
cmake --build . --config Release --parallel 4
```

### OpenSees 빌드 (Linux 예시)

```bash
git clone https://github.com/OpenSees/OpenSees.git
cd OpenSees && mkdir build && cd build
$HOME/.local/bin/conan install .. --build missing
cmake .. -DMUMPS_DIR=$PWD/../../mumps/build
cmake --build . --target OpenSees -j8
cmake --build . --target OpenSeesPy -j8
mv ./lib/OpenSeesPy.so ./opensees.so
```

`-j8`처럼 `-j` 옵션으로 병렬 빌드 가능(코어 수에 맞춰 조정). Windows/Mac 절차는 컴파일러(MSVC+Intel oneAPI / AppleClang+gfortran)와 세부 명령만 다르고 구조는 동일 — 상세는 [opensees-doc-build-application 소스](../sources/opensees-doc-build-application.md) 참고.

### 흔한 문제

- 빌드에 쓴 컴파일러와 실행 시 Python이 불일치하면 segmentation fault 발생 — 반드시 같은 Python으로 통일.
- Conan의 zlib 버전 불일치 오류: `tcl` 패키지의 `conanfile.py`에서 `self.requires("zlib/1.2.13")`로 직접 수정.
- MPI 라이브러리는 OpenSeesMP/OpenSeesMPI 타깃을 만들 때 모든 플랫폼에서 필수.

## 구식 방식 — Makefile.def (오래된 클러스터/HPC 환경)

TN-2007-16([출처](../sources/opensees-tn2007-16-parallel-processing.md), 2008년 문서)에 기술된 방식으로, conan/cmake가 없는 구형 HPC 환경(당시 예시: SDSC Datastar, TeraGrid, UC Davis 클러스터)에서는 지금도 유사한 접근이 필요할 수 있다:

1. Tcl/Tk 소스 확보(`which tclsh`로 기존 설치 확인).
2. OpenSees 소스 확보.
3. `OpenSees/MAKES/` 아래 자신의 플랫폼과 가장 가까운 예시(`Makefile.def.DATASTAR`, `Makefile.def.TERAGRID`, `Makefile.def.LINUX_CLUSTER` 등)를 `Makefile.def`로 복사 후 `HOME`, `TCL_LIBRARY`, `TCL_INCLUDES`, `CC++`/`CC`/`FC`, `C++FLAGS` 등을 자신의 경로에 맞게 수정.
4. **`PROGRAMMING_MODE` 설정이 핵심 분기점**:
   - OpenSeesSP: `PROGRAMMING_MODE = PARALLEL`
   - OpenSeesMP: `PROGRAMMING_MODE = PARALLEL_INTERPRETERS`
5. `HOME` 아래 `lib`/`bin` 디렉터리 생성 후 `make`.
6. 64비트 컴파일이 필요한 구형 시스템(예: Datastar)은 `setenv OBJECT_MODE 64` 필요.

두 `PROGRAMMING_MODE` 값은 소스 코드 내부적으로 `#ifdef _PARALLEL_PROCESSING`(SP)과 `#ifdef _PARALLEL_INTERPRETERS`(MP) 전처리 분기와 그대로 대응된다 — [OpenSees 아키텍처](../concepts/opensees-architecture.md) 참고.

## 관련 페이지

- [Mumps Solver](../reference/mumps-solver.md)
- [OpenSees 병렬 컴퓨팅 개념](../concepts/opensees-parallel-computing.md)
- [HPC에서 OpenSees 실행하기](running-opensees-on-hpc.md)
- [OpenSees용 클러스터 구축하기](building-a-cluster-for-opensees.md)
