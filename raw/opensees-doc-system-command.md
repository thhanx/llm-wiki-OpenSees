---
title: "3.2.3. system Command — OpenSees Documentation"
source_url: https://opensees.github.io/OpenSeesDocumentation/user/manual/analysis/system.html
retrieved: 2026-07-06
type: official-doc
method: WebFetch (요약 추출됨 — 각 solver 상세는 하위 페이지 참고)
note: 이 페이지 자체는 목록/링크 위주라 WebFetch가 개별 하위 페이지 링크만 반환함. Mumps·SuperLU 하위 페이지는 별도 raw 파일로 확보함(opensees-doc-mumps-solver.md, opensees-doc-superlu-solver.md).
---

# OpenSees system Command 요약

## 명령어 기본 구조

```
system systemType? arg1? ...
```

## 목적

선형 방정식 시스템(Ax=b)을 저장하고 풀기 위해 LinearSOE와 LinearSolver 객체를 구성하는 데 사용됩니다.

## 지원되는 System Types

페이지에서는 다음 시스템 유형이 나열되어 있습니다 (각각 하위 페이지에 상세 문서):

1. **BandGeneral System**
2. **BandSPD System**
3. **ProfileSPD System**
4. **SuperLU System**
5. **Umfpack System**
6. **FullGeneral System**
7. **SparseSYM Solver**
8. **Mumps Solver**
9. **Diagonal & MPIDiagonal System** (Mass Lumping 옵션 포함)
10. **PythonSparse System**

참고: 구버전 OpenSeesWiki(opensees.berkeley.edu/wiki)에는 PETSc System도 별도 문서화되어 있었으나, 이 신규 문서 사이트(opensees.github.io/OpenSeesDocumentation)의 system 목록에는 PETSc 항목이 보이지 않음 — 이 부분은 문서 이관 과정의 공백일 가능성이 있음. PETSc는 소스 코드 수준(PetscSOE 클래스)에서는 여전히 존재함 (opensees-petscsoe-header-source.md 참고).
