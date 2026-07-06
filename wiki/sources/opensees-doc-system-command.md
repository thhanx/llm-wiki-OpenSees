---
tags: [solver]
updated: 2026-07-06
sources: []
---

# system Command — OpenSees Documentation (공식)

- 원본: [raw/opensees-doc-system-command.md](../../raw/opensees-doc-system-command.md)
- 출처: https://opensees.github.io/OpenSeesDocumentation/user/manual/analysis/system.html

## 핵심 요지

- `system` 명령어는 선형방정식 시스템(Ax=b)을 위한 LinearSOE/LinearSolver 객체를 구성.
- 지원 유형 10종 나열: BandGeneral, BandSPD, ProfileSPD, SuperLU, Umfpack, FullGeneral, SparseSYM, Mumps, Diagonal/MPIDiagonal, PythonSparse.
- 이 신규 문서 사이트에는 PETSc 항목이 목록에 없음(구 위키에는 있었음) — 문서 이관 공백으로 추정.
- 각 유형의 상세 옵션은 개별 하위 페이지에 문서화(WebFetch로는 목록만 확보, 일부는 별도 raw 소스로 추가 확보).

## 기여한 위키 페이지

- [system 명령어](../reference/system-command.md)
