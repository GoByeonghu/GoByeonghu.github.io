---
layout: post
title: 
subtitle: Classroom 테이블에서 Select Query 병목 문제를 해결하기 위한 최적화 전략 탐구 (라이브 강의 여부(is_live) 판단 쿼리의 성능 개선 방안)
categories: 
    - OceanAcademy
tags: [spring, mysql]
---

## 동기

관계형 데이터 베이스로 구현한 데이터베이스 중 Classroom 테이블에 많은 Select query가 예상된다.
이 테이블에 대한 최적화 방법을 탐구하고 각 방안에 대해 테스트를 진행한다.

## 원인

Classroom 테이블은 페이지에서 가장 많은 api요청을 감당한다. 특히 is_live라는 라이브 강의 진행중인지 여부를 판단하는 칼럼이 있는데, 메인 페이지에 라이브 중인 강의를 표기하며, 라이브 강의 여부를 판단하는 쿼리가 다수 예상된다.

## 예상되는 해결 방안(탐구 대상)

캐쉬
스프링의 JPA에서 SQL문 삽입으로 개선
셀렉트쿼리 최적화
인덱싱