---
layout: post
title: Spring gradle jar 생성
subtitle: Spring gradle에서 jar생성해 배포하기
categories: 
    - Spring
tags: [spring]
---

## jar 파일 빌드, 실행, 삭제
---

### jar 생성

 build/libs안에 jar파일 생성

#### 윈도우

    ```
    gradlew.bat build 
    ```

#### 리눅스

    ```
    ./gradlew build
    ```

  > IntelliJ에서 UI로 실행 : Gradle 탭에서 Tasks > build 안에 실행 가능한 bootJar 스크립트를 더블클릭


### jar실행하기

#### 윈도우

  ```
  java -jar [ jar name ].jar
  ```

#### 리눅스

  ```
  java -jar build/libs/[ jar name ].jar
  ```


### jar을 포함한 빌드 삭제
  
Gradle 프로젝트의 빌드 디렉토리를 삭제하여 기존의 빌드 산출물(아티팩트)을 제거(build 디렉토리를 및 jar 삭제)

#### 윈도우

  ```
  gradlew.bat clean
  ```

#### 리눅스

  ```
  ./gradlew clean
  ```

## jar 파일 배포하는 방법
---
  
- 릴리즈 브랜치 만들어 jar만 남긴다.
- jar 파일만 전송한다.


## 참조

[슬기로운 개발생활:티스토리](https://dev-coco.tistory.com/68)