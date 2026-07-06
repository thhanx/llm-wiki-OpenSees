---
tags: [solver]
updated: 2026-07-06
sources: []
---

# SuperLU System — OpenSees Documentation (공식)

- 원본: [raw/opensees-doc-superlu-solver.md](../../raw/opensees-doc-superlu-solver.md)
- 출처: https://opensees.github.io/OpenSeesDocumentation/user/manual/analysis/system/SuperLU.html

## 핵심 요지

- 문법: `system SuperLU` (구 명칭 `system SparseGEN`도 여전히 작동).
- 방정식 재정렬을 자동으로 수행하므로 "Plain numberer" 외 별도 설정 불필요(단, 병렬 도메인 분할 시엔 병렬 numberer 필요 — [TN-2007-16](opensees-tn2007-16-parallel-processing.md) 참고).
- 참고문헌: Demmel et al., "A supernodal approach to sparse partial pivoting", SIAM J. Matrix Analysis and Applications, 1999.

## 기여한 위키 페이지

- [SuperLU Solver](../reference/superlu-solver.md)
