---
title: "3.2.3.4. SuperLU System — OpenSees Documentation"
source_url: https://opensees.github.io/OpenSeesDocumentation/user/manual/analysis/system/SuperLU.html
retrieved: 2026-07-06
type: official-doc
method: WebFetch
---

# 3.2.3.4. SuperLU System

## 개요

이 명령어는 희소 행렬 방정식 시스템을 위한 SparseGEN 선형 시스템 객체를 생성합니다. SuperLU는 희소 행렬 해석에 사용되며, 솔루션 계산은 SuperLU 라이브러리를 통해 수행됩니다.

## 기본 문법

```
system SuperLU
```

## 주요 특징

**방정식 재정렬**: 빠른 솔버 성능을 위해 소프트웨어가 자동으로 방정식을 재정렬하므로, "Plain numberer" 외에 다른 설정은 불필요합니다.

**이전 명령어**: 원래 명령어인 `system SparseGEN`도 현재까지 작동합니다.

## 사용 예제

### Tcl 코드
```tcl
system SuperLU
```

### Python 코드
```python
system('SuperLU')
```

## 참고문헌

"A supernodal approach to sparse partial pivoting" - James W. Demmel et al., SIAM J. Matrix Analysis and Applications, 20(3), 720-755, 1999.

---

**개발자**: fmk
