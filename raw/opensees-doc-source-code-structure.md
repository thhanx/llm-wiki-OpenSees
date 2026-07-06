---
title: "1. Source Code — OpenSees Documentation"
source_url: https://opensees.github.io/OpenSeesDocumentation/developer/sourceCode.html
retrieved: 2026-07-06
type: official-doc
method: WebFetch
note: 이 페이지는 소스코드 아키텍처 설명이 아니라 GitHub 기여(fork/PR) 정책 안내임. 실제 아키텍처는 opensees-cluster-build-blog 및 opensees-domainpartitioner-api.md, opensees-evolution-history-scott2014.pdf 등에서 보완.
---

# OpenSees 소스 코드

## 1. 소스 코드

OpenSees의 소스 코드는 OpenSees Github link에서 획득할 수 있습니다. GitHub는 버전 관리 및 협업을 위한 코드 호스팅 플랫폼으로, 어디서나 프로젝트를 함께 작업할 수 있게 해줍니다.

### 포크(Fork) 권장사항
OpenSees를 기본 코드로 사용하거나 협업할 계획이라면 자신의 계정으로 저장소를 포크하여 작업하는 것을 강력히 권장합니다. 메인 OpenSees 저장소에 직접 푸시(push)할 수 없습니다.

### 풀 리퀘스트(Pull Request) 정책
- 포크된 저장소에서 메인 저장소로의 풀 리퀘스트만 고려됩니다
- 충돌이 없는 경우에만 허용됩니다
- 충돌을 야기하는 풀 리퀘스트는 즉시 거부됩니다

### 충돌 방지 권장사항
포크를 메인 OpenSees 저장소와 정기적으로 동기화하여 충돌을 사전에 방지하는 것이 중요합니다. 이를 위해서는 병합(merge) 또는 리베이스(rebase) 작업이 필요합니다.

### 참고 자료
- GitHub 사용법 지침: Atlassian의 포킹 워크플로우
- 변경사항 저장: Atlassian의 saving changes 페이지
- Git 심화 학습: Atlassian의 전체 git 튜토리얼

---

**저작권**: © 2022, The Regents of the University of California
**제작**: Sphinx 및 Read the Docs 테마
