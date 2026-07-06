---
title: "2. Building Application — OpenSees Documentation"
source_url: https://opensees.github.io/OpenSeesDocumentation/developer/build.html
retrieved: 2026-07-06
type: official-doc
method: WebFetch
---

# OpenSees 빌드 가이드

## 개요

OpenSees 애플리케이션과 Python 모듈 빌드를 위한 문서입니다. CMake를 사용하여 Windows, MacOS, Ubuntu에서 일관된 빌드 프로세스를 제공합니다.

## 필수 요구사항

### 공통 요구사항
- **CMake**: 3.20 이상 권장
- **Git**: 소스 코드 관리
- **C, C++, Fortran 컴파일러**
- **Python 3.11**: OpenSeesPy 및 Conan 설치용 (3.12 이상 미지원)
- **MUMPS**: OpenSees의 병렬 솔버

### 플랫폼별 컴파일러
- **Windows**: Microsoft Visual Studio + Intel oneAPI Fortran 컴파일러
- **MacOS**: AppleClang + gfortran
- **Linux**: gcc, g++, gfortran

### 패키지 매니저
- **Windows/Linux**: Conan 1.64.1 (Conan 2.0 미지원)
- **MacOS**: Homebrew

### 필수 라이브러리
- Eigen
- HDF5
- Tcl (Windows용)
- Zlib (Windows용)
- Intel MPI Library (Windows용)
- Open-MPI (MacOS, Linux용)
- ScaLAPACK (MacOS, Linux용)
- LAPACK (Linux용)
- Intel oneAPI Math Kernel Library (Windows용)
- MKL (Linux용)

## 소스 코드 획득

```bash
git clone https://github.com/OpenSees/OpenSees.git
```

기여 계획 시 개인 포크 후:
```bash
git clone https://github.com/YOUR_USER_NAME/OpenSees.git
```

업데이트:
```bash
git pull
```

---

## Windows 10 빌드

### 소프트웨어 설치

#### CMake
https://cmake.org/download/ 에서 설치 (3.20 이상)

#### Git
https://gitforwindows.org/ 에서 설치

#### Microsoft Visual Studio
Community Edition에서 다음 워크로드 설치:
- Desktop development with C++
- Visual Studio extension development

**경고**: MSVC 2022.2는 Intel OneAPI와 호환되지 않음. 2022.1 또는 2019 버전 사용

#### Intel oneAPI 기본 & HPC 툴킷
Base Toolkit에서:
- Intel oneAPI Math Kernel Library

HPC Toolkit에서:
- Intel MPI Library
- Intel Fortran Compiler & Intel Fortran Compiler Classic

**주의**: Visual Studio 설치 후 설치하고 Visual Studio 통합 활성화

#### Python 3.11
https://www.python.org/downloads/windows/ 에서 설치 (3.12 이상 미지원)

#### Conan 1.x 설치
```bash
pip install conan<2.0
```

#### MUMPS 빌드
```bash
git clone https://github.com/OpenSees/mumps.git
cd mumps
mkdir build
cd build
call "C:\Program Files (x86)\Intel\oneAPI\setVars.bat" intel64 mod
cmake .. -Darith=d -DCMAKE_MSVC_RUNTIME_LIBRARY="MultiThreaded" -G Ninja
cmake --build . --config Release --parallel 4
```

**주의**: Intel oneAPI 환경 변수는 Command Prompt에서만 유효 (PowerShell 불가)

### OpenSees 빌드 절차

```bash
mkdir build
cd build
call "C:\Program Files (x86)\Intel\oneAPI\setVars.bat" intel64 mod
conan install .. --build missing --settings compiler.runtime="MT"
cmake .. -DBLA_STATIC=ON -DMKL_LINK=static -DMKL_INTERFACE_FULL=intel_lp64 -DMUMPS_DIR="..\..\mumps\build"
cmake --build . --config Release --target OpenSees -j8
cmake --build . --config Release --target OpenSeesPy -j8
move ./bin/OpenSeesPy.dll ./bin/opensees.pyd
copy C:\Program Files (x86)\Intel\oneAPI\compiler\2024.1\bin\libiomp5md.dll ./bin/
```

결과: `build/bin` 디렉토리에 OpenSees, OpenSeesMP, OpenSeesMPI 실행파일 및 opensees.pyd 생성

### 주요 설정 사항

#### Python 모듈 사용
1. **pip 설치 버전 교체**: 다음으로 설치 위치 찾기
   ```python
   python3
   import opensees
   import inspect
   inspect.getfile(opensees)
   ```
   기존 파일 백업 후 교체

2. **PYTHONPATH 설정**:
   - 환경 변수에 bin 폴더 경로 추가
   - python39.dll 또는 python310.dll을 bin 폴더에 복사 필요 (3.8 이상)

#### libiomp5md.dll 필수
opensees.pyd와 동일 폴더에 위치 필요 (없으면 ImportError 발생)

#### Conan 문제 해결

**zlib 불일치 오류**:
- `$HOME/.conan/data/tcl/8.6.10/_/_/export/conanfile.py` 51번 줄 수정
- `self.requires("zlib/1.2.13")`으로 변경

**다른 Conan 문제**:
```bash
git clone https://github.com/conan-io/conan.git conan-io
cd conan-io
pip install -e .
```

#### 병렬 빌드
`-j` 옵션으로 병렬화 (예: `-j8`은 8개 코어 사용)

---

## MacOS 빌드

### 소프트웨어 설치

#### Xcode Command Line Tools
```bash
xcode-select --install
```

**주의**: OS 업데이트 후 재설치 필요 시
```bash
sudo rm -rf /Library/Developer/CommandLineTools
sudo xcode-select --install
```

#### Homebrew 및 의존성
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
brew install cmake
brew install eigen
brew install gfortran
brew install hdf5
brew install open-mpi
brew install scalapack
```

#### Eigen 심볼릭 링크
```bash
sudo ln -sf /usr/local/include/eigen3/Eigen /usr/local/include/Eigen
```

**주의**: Eigen 설치 위치는 `/opt/homebrew/include/eigen3/Eigen`일 수 있음

#### MUMPS 빌드
```bash
git clone https://github.com/OpenSees/mumps.git
cd mumps
mkdir build
cd build
cmake .. -Darith=d
cmake --build . --config Release --parallel 4
```

### OpenSees 빌드 절차

```bash
mkdir build
cd build
cmake .. -DMUMPS_DIR=$PWD/../../mumps/build
cmake --build . --target OpenSees -j8
cmake --build . --target OpenSeesPy -j8
mv ./OpenSeesPy.dylib ./opensees.so
```

결과: opensees.so 생성

### 주요 설정 사항

#### Python 버전
Apple Silicon Mac에서는 시스템 기본 Python 피하고 brew로 설치
```bash
brew install python@3.11
```
`python3.11` 명령으로 호출

#### Python 모듈 사용
1. **pip 설치 버전 교체**: Windows와 동일 방식
2. **PYTHONPATH 설정**:
   ```bash
   export PYTHONPATH=$PWD
   # 또는 기존 경로에 추가
   PYTHONPATH=$PWD:$PYTHONPATH
   ```

#### 병렬 빌드
`-j` 옵션으로 코어 수 지정

---

## Ubuntu 빌드

### 소프트웨어 설치

```bash
sudo apt-get update
sudo apt install -y cmake
sudo apt install -y gcc g++ gfortran
sudo apt install -y python3-pip
sudo apt install -y liblapack-dev
sudo apt install -y libopenmpi-dev
sudo apt install -y libmkl-rt
sudo apt install -y libmkl-blacs-openmpi-lp64
sudo apt install -y libscalapack-openmpi-dev
```

#### Conan 1.x 설치
```bash
pip install conan<2.0
```

### OpenSees 빌드 절차

```bash
mkdir build
cd build
$HOME/.local/bin/conan install .. --build missing
cmake ..
cmake --build . --target OpenSees -j8
cmake --build . --target OpenSeesPy -j8
mv ./lib/OpenSeesPy.so ./opensees.so
```

결과: opensees.so 생성

### 주요 설정 사항

#### Python 모듈 사용
Windows/MacOS와 동일한 두 가지 방식 제공:
1. pip 설치 버전 교체
2. PYTHONPATH 환경 변수 설정

#### Conan zlib 문제
Linux에서도 동일한 zlib 불일치 해결법 적용

#### 병렬 빌드
4개 이상 코어 시 `-j` 값 증가 권장

---

## 공통 주의사항

1. **빌드 컴파일러와 Python 일치**: 다른 Python 실행파일 사용 시 segmentation fault 발생
2. **CMake 출력 확인**: 사용된 Python 라이브러리 확인
3. **MPI/병렬 빌드**: 모든 플랫폼에서 MPI 라이브러리 필수 설치 (OpenSeesMP/OpenSeesMPI 빌드용)
4. **환경 변수**: 플랫폼별로 컴파일러 환경 변수 설정 필수 (특히 Windows의 Intel oneAPI)
