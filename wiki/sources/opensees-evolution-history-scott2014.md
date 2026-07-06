---
tags: [history, architecture, licensing]
updated: 2026-07-06
sources: []
---

# The Evolution of OpenSees: Is the Open Source Model a Success? (Michael H. Scott, 2014)

- 원본: [raw/opensees-evolution-history-scott2014.pdf](../../raw/opensees-evolution-history-scott2014.pdf)
- 저자: Michael H. Scott, Oregon State University — 3rd International Conference on Geohazard Information Zonation (GIZ 2014), 2014-10-20, Medan, Indonesia (33쪽)

## 핵심 요지

- **기원**: OpenSees는 Frank McKenna의 1997년 UC Berkeley 박사학위논문 "Object oriented finite element analysis: frameworks for analysis algorithms and parallel computing"(지도교수 Gregory Fenves)에서 출발. PEER(1997년 NSF 자금으로 설립) 산하 워킹그룹 번호를 따 처음엔 "G3"로 불렸고, 2000년에 "OpenSees"로 개명. Fenves 교수의 교육용 MATLAB 객체지향 프로그램 "G2"가 그 전신.
- 1998년 시점 가용 모델: 비선형 트러스·탄성 보, Elastic/EPP/Parallel 단축재료, Newton-Raphson/Modified Newton, Newmark 적분, profile symmetric PD 솔버.
- 2000년 개명 시점까지의 핵심 확산 요인: force-based beam-column 정식화(비반복적, 1997년 발표, Remo de Souza 구현), 보 단면의 일반 fiber discretization(de Souza, CE224 수업 프로젝트), Steel01/02·Concrete01/02·Hysteretic 재료(Filippou의 FEDEAS 프로그램에서 FORTRAN을 번역), ZeroLength 요소(Greg Fenves 구현 — SSI의 p-y 모델과 base isolation의 기반이 됨).
- 2003년 Armen der Kiureghian과 박사과정생 Terje Haukaas가 불확실성 정량화 모듈(Monte Carlo, FORM, importance sampling, fragility analysis) 추가.
- 버전 관리 역사: G3 시절엔 McKenna가 소스를 zip 디스크에 보관, 이후 2006년 이전까지 CVS, 이후 SVN(체크인 권한은 소수 개발자만 보유).
- **2014년 현재 규모**: 단축재료 100종 이상(콘크리트 15, 철강 8, 이력거동 20종—DRAIN/SNAP/FEDEAS 포함, p-y/q-z/t-z 15종, series/parallel/min-max/static condensation용 유틸리티 래퍼 10종), 선형방정식 솔버 약 12종(희소/밀집, 대칭/비대칭, 직접/반복법 — 문제 유형에 따라 행렬 위상 결정), 시간적분기·근찾기 알고리즘 각 약 12종.
- **라이선스**: UC Regents 소유. 교육·연구·비영리 목적 비상업적 사용/복제/수정/배포는 무료 허용, 그 외 단체는 내부 목적 사용/복제/수정만 허용(재배포 불가), 수정 후 재포장해 판매하는 것은 금지.
- 응용 분야 확장 사례: bridge rating(자동화 스크립트), gusset plate 평가, soil-structure interaction, base isolator 모델(flat slider, single/triple friction pendulum), **고층건물 해석에 HPC 병행 사용**(TBI Building 2, Shanghai Tower 사례 — Xinzheng Lu), fire attack 열해석(A. Usmani), tsunami/storm surge용 Particle FEM 기반 fluid-structure interaction.
- 검증 사례: RC 교각 실험(41명 분석가의 blind prediction) — 시뮬레이션 drift의 평균 COV 39%, 잔류변위는 시뮬레이션이 대체로 과소평가.
- 최다 인용 저널 논문: McKenna, Scott, Fenves, "Nonlinear finite-element analysis software architecture using object composition", J. Computing in Civil Engineering 24(1), 2009.
- 부족한 점(2014 시점): 풍공학 커뮤니티에 정착 못함, 목재 구성모델 부재. 오픈소스 모델의 한계: 개발자들이 논문 출판 대기·크레딧 우려·단순 비공유 의지로 코드 기여를 꺼림, "소스코드가 곧 문서"라는 통념으로 문서화 부족.

## 기여한 위키 페이지

- [OpenSees 아키텍처](../concepts/opensees-architecture.md) (역사, 라이선스, 최다 인용 논문)
- [OpenSees 개요](../overview.md)
