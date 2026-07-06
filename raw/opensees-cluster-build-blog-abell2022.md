---
title: "Building a computer cluster for OpenSees"
source_url: https://joseabell.com/posts/2022/buidling-a-computer-cluster-for-opensees.html
author: José A. Abell (jaabell), UANDES 토목공학 실험실
retrieved: 2026-07-06
date_published: 2022-06-07
type: blog
method: WebFetch
---

# OpenSees용 컴퓨터 클러스터 구축

**작성자:** jaabell | **날짜:** 2022년 6월 7일
**태그:** #opensees #hpc #cluster

## 프로젝트 개요

칠레 산티아고의 근거리 지진 영향 연구를 위한 연구비 지원을 받아 소규모 Beowulf 클러스터를 구축하는 프로젝트입니다.

### 주요 설계 고려사항

- 예산 범위 내 최대 프로세서 코어 확보
- 코어당 3~4GB RAM 할당
- Ubuntu Linux 22.04 기반 운영 체제
- Ansible을 통한 소프트웨어 프로비저닝
- SLURM 및 OpenMPI 설치
- OpenSees 모든 의존성 구성
- NFS를 활용한 네트워크 공유 폴더
- SSH 및 원격 데스크톱 연결

## 하드웨어 아키텍처 결정 배경

### 확장성 고려사항

저자의 경험과 ASDEA의 Massimo Petarca와의 상담 결과, **"OpenSees는 16개 프로세스를 초과할 때 선형적 확장성을 잃는다"**는 결론에 도달했습니다. 따라서 단일 대형 서버보다 분산 메모리 시스템이 더 효율적이라고 판단되었습니다.

### 프로세서 선택 과정

**Intel Xeon 검토:**
- 장점: MKL(Math Kernel Library) 지원, ECC 메모리, 높은 안정성
- 단점: 높은 비용, 단일 마더보드에서의 제한적 성능 향상

**AMD 검토:**
- Threadripper: 32코어로 과다
- Ryzen 9 5950X: 16코어, 높은 클록 속도, 비용 효율적
- 이점: Intel MKL도 AMD에서 호환 가능

**Intel i9 검토:**
- 장점: 성능 좋음
- 단점: 성능 코어(5.0GHz+)와 효율성 코어(4.0GHz) 혼합
- 결론: 순수 성능 코어가 필요하므로 부적합

**메모리 기술:**
- DDR5는 기술적으로 미성숙하고 비용이 높음
- DDR4가 더 실용적이고 안정적

**최종 선택 근거:**
Intel MKL은 AMD 프로세서에서도 최적화 작동 가능함이 확인되어 AMD 선택의 장벽이 제거되었습니다.

## 최종 하드웨어 사양

### 계산 노드 (8대)

- **프로세서:** 액체 냉각식 Ryzen 9 5950X (16코어)
- **오버클로킹:** 5.0GHz 이상 달성 목표
- **메모리:** 64GB DDR4
- **마더보드:** 고급 Aorus
- **네트워크:** 2.5GbE 카드
- **스토리지:** 500GB M.2 NVMe PCIe x16 SSD (OS 및 로컬 저장소용)

### 메인(로그인) 노드

- **프로세서:** Ryzen 9 5900X (12코어)
- **메모리:** 32GB RAM
- **용도:** 로그인, 컴파일, 전후처리
- **추가 스토리지:** 2TB NVMe 드라이브 (홈 폴더용)
- **계획:** 향후 RAID SSD 추가 예정

### GPU

문서에서 "GPU you're asking? What GPU?"라고 명시하여 GPU를 사용하지 않습니다.

## 네트워크 인프라

**초기 구성:** 보유 중이던 48포트 기가비트 스위치
- **문제:** 500GB/s NVMe 쓰기 성능(이론치 7.2Gbit/s)을 지원할 수 없음

**최적화 구성:**
- QNAP QSW-M2108-2C 2.5GbE 관리형 스위치 2대 추가 구매
- 각 스위치에 10GbE SFP+ 포트 2개 포함
- 메인 노드에 듀얼 포트 SFP+ 이더넷 카드 설치
- 성능 벤치마크 진행 예정

## 소프트웨어 스택

### 설치 도구

**BeowulfInstaller** 사용
- CS 학생들이 개발
- Ubuntu 22.04에서 원활하게 설치됨
- NFS를 통한 대용량 스토리지 공유 기능 추가

### 소프트웨어 구성 요소

- Ubuntu Linux 22.04
- Ansible (프로비저닝)
- SLURM (작업 스케줄러)
- OpenMPI (메시지 전달 인터페이스)
- OpenSees 의존성 전체
- NFS (네트워크 파일 시스템)

## 성능 검증

### 현황

- **ShakeMaker:** 모든 128코어에서 선형 확장성 달성
- **OpenSees:** 곧 테스트 예정

### 벤치마크 공유

모든 커스텀 벤치마크는 GitHub 저장소에서 개발 및 공유 중입니다:
- GitHub: ClusterBenchmarks 저장소
- ShakeMaker 저장소도 함께 관리 중

## 팀 기여자

**영구 팀원:** Carlos Castex

**Team v.1 (초기):** José Luis Assadi, Cristobal Griffero

**Team v.2:** Sebastián Baixas (BeowulfInstaller 원작자), Joaquin Fernandez, José Tomás Gutiérrez

**Team v.3 (현재):** Sebastián Baixas, Omar Oyarce, Alberto Hurtado, José Luis Larenas

## 구축 프로세스 스냅샷

- 5월 17일: 초기 설정 및 Ubuntu 설치 시작
- 5월 18일: 기본 3개 노드(메인 + 2개 계산) 및 스위치 구성 완료
- 5월 25일: 추가 2개 노드 설치
- 6월 2일: 전체 8개 노드 완성
- 향후: QNAP 스위치 성능 측정 및 4개 노드 추가 예정

**주요 특징:** UANDES 토목공학 실험실에서 진행되었으며, 팀 협력과 지속적인 개선이 강조됩니다.
