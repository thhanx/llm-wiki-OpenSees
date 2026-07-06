---
title: "3.2.3.8. Mumps Solver — OpenSees Documentation"
source_url: https://opensees.github.io/OpenSeesDocumentation/user/manual/analysis/system/Mumps.html
retrieved: 2026-07-06
type: official-doc
method: WebFetch
---

# 3.2.3.8. Mumps Solver

## 개요

이 명령어는 [Mumps](http://mumps-solver.org/) 솔버를 사용하는 희소 연립방정식 시스템을 구성하는 데 사용됩니다.

## 명령어 문법

```
system Mumps
```

## 주의사항

현재 병렬 **OpenSeesSP** 및 **OpenSeesMP** 애플리케이션으로 제한되어 있습니다.

## 사용 예제

### Tcl 코드

```tcl
system Mumps
```

### Python 코드

```python
system('Mumps')
```

## 추가 정보

- **개발자**: fmk
- **참고**: 해당 문서에는 Mumps solver의 추가 옵션이나 파라미터가 명시되어 있지 않습니다. 기본 사용법만 제공됩니다.
