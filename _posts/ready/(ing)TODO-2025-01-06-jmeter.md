---
layout: post
title: JMeter
subtitle: JMeter로 로드테스트를 수행하다
categories: 
  - Backend
tags: []
---

## JMeter란

Apache JMeter 애플리케이션은 기능 동작을 로드하고 성능을 측정하도록 설계된 100% 순수 Java 애플리케이션인 오픈 소스 소프트웨어이다. 



### JMeter 설치하기

- [설치 파일 다운로드](https://jmeter.apache.org/download_jmeter.cgi)


원하는 위치에 압축해제를 수행한다


bin에 들어가 mac은 jmeter를 window는 jmeter.bat을 실행시킨다


터미널창과 gui가 나오면 성공

이 gui는 cmd창 세션에 종속되어있음으로 cmd가꺼지면 gui도 꺼진다.


혹시 

Not able to find Java executable or version. Please check your Java installation.
errorlevel=2

이러한 오류가 발생한다면 java의 버전이 낮거나, java가 설치되지 않았거나, 'java'를 환경변수에 등록해두지 않아서이다.


## 참조

- [Apache JMeter github](https://github.com/apache/jmeter)
- [Jlog:티스토리](https://artistjay.tistory.com/2)