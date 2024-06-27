---
layout: post
title: Version의 종류
subtitle: 버젼의 종류를 알아보자
categories: 
   - ETC
tags: [spring]
---

## 버전의 종류

1. **Semantic Versioning (SemVer)**
   - **MAJOR.MINOR.PATCH**: 주 버전, 부 버전, 패치 버전으로 구성됩니다.
     - **MAJOR**: 주요 변경사항이나 호환되지 않는 API 변경이 있을 때 증가합니다.
     - **MINOR**: 기능이 추가되었지만 하위 호환성을 유지할 수 있는 변경일 때 증가합니다.
     - **PATCH**: 기존 기능의 버그 수정과 같은 하위 호환성을 유지하는 변경일 때 증가합니다.
   - 예: `1.0.0`, `2.1.3`

2. **Snapshot 버전**
   - 개발 중인 버전이나 릴리즈 이전의 임시 빌드를 나타냅니다.
   - 예: `1.0-SNAPSHOT`, `2.0.0-SNAPSHOT`

3. **Alpha, Beta, Release Candidate (RC)**
   - 개발 초기 단계에서 사용되며, 테스트를 위한 빌드를 식별합니다.
   - **Alpha**: 초기 개발 단계, 기능이 완전하지 않을 수 있음.
   - **Beta**: 거의 완료된 상태이지만 일부 피드백을 받기 위한 릴리즈.
   - **RC (릴리즈 후보)**: 최종 릴리즈에 앞서 마지막 테스트를 위한 빌드.
   - 예: `1.0.0-alpha`, `2.1.0-beta`, `3.0.0-RC1`
