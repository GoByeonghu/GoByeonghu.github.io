---
layout: post
title: Ranking Algorithm으로 Top 10 인기 강의를 구현하다 
subtitle: 인기강의 TOP10을 Hacker News Algorithm으로 구현하다.
categories: 
    - OceanAcademy
tags: [spring, mysql]
---

## 요약



## 개요

### 문제 상황

'바다서원'프로젝트를 진행하던 중 Top10 강의를 제공하는 기능 구현이 요구되었다.
**인기 강의 랭킹을 정하여 상위 10개를 반환하는 API를 개발**해야한다.

**[요구사항]**
수강자들에게 인기가 많으면 증가하고 시간이 지나면 점차 순위가 낮아지는 랭킹을 구현하라

### 해결방안 후보(Ranking Algorithm)

#### 랭킹 알고리즘

**1. Hacker News Algorithm**

![Hacker_News_Algorithm]({{site.url}}/PostImages/2024-09-23-Hacker-News-Algorithm/Hacker_News_Algorithm.png)

- Y Combinator가 운영하는 뉴스 공유 사이트인 Hacker News에서 사용하는 랭킹 알고리즘
- 사용자가 게시물에 '추천(upvote)'을 하는 행동과 게시물이 생성된 시간을 이용하여 랭킹을 도출
- 즉 최근에 순간적으로 많은 추천을 받은 경우 유리하다

- **구성 요소**
- `p`: 게시물에 대한 추천 수. 1을 빼는 이유는 작성자 자신의 글을 추천하는 경우를 제외하기 위함
- `t`: 게시물이 게시된 시간과 현재 시간 사이의 차이. 이 값은 시간 단위로 표시되며, 게시물이 2시간 전에 게시되었다면 t는 2가된다.
- `G`: 중력 계수로, 시간이 지남에 따라 게시물의 순위가 어떻게 변화할지를 결정한다. Hacker News에서는 기본적으로 1.8의 값을 사용

**2. Reddit Ranking Algorithm**

![Reddit_Ranking_Algorithm]({{site.url}}/PostImages/2024-09-23-Hacker-News-Algorithm/Reddit_Ranking_Algorithm.png)

- Reddit이 초창기에 사용한 인기순 공식이다. 이는 컨텐츠를 원하는 시점까지 피드에 노출시킬 수 있는 가능성이 있다.
- 시간의 개념을 사용할때 해당 컨텐츠가 DecayPoint를 지나면 인기도가 급감하게 된다.
- decayPoint가 존재하지 않는다면 해당 컨텐츠는 계속 노출되어 계속 힘을 얻게 된다. 하지만, decayPoint를 적용하면 기 시점을 지나는 순간 급속도로 인기도가 하강하게 된다.

- **구성 요소**
- `score`: 기준에 의해 생성된 점수
- `coolingRate` : 얼마나 더 빨리 나이를 먹일 것인가 (음수이며, 기본모델의 값은 -8이다) 
- `TimeDiff`: 현재로부터 컨텐츠가 얼마나 지났는가 (시간/일/주/달 등 서비스에 맞게 계산하면된다.)
- `DecayPoint` : 컨텐츠 인기도가 떨어지는 시점(인기 피드에서 감춰지는 시점)

**3. Highly Rated**

#### 배치 프로그램

정해진 시간마다 랭킹을 계산하는 프로세스를 생성한다.

**1. 정해진 시간에 한번씩 랭킹 계산 (배치 처리):**

`방식`: 
특정 주기(예: 1시간마다, 하루에 한 번)마다 좋아요 수를 기반으로 모든 강의의 랭킹을 다시 계산하는 방식.
`장점`:
시스템 부하 적다
실시간 업데이트가 필요하지 않기 때문에 서버 자원을 절약할 수 있다.
`단점`:
좋아요가 누적되더라도 즉시 반영되지 않는 실시간성 문제.

1) 배치(Batch)란?
배치(Batch) : 일괄처리
사용자와 상호작용 없이 여러 개의 작업을 미리 정해진 순서에 따라 중단 없이 처리하는 것
→ Spring Batch 등
특징: 대용량데이터, 견고성, 성능, 자동화, 신뢰성

2) 스케쥴러(Scheduler)란?
특정한 시간에 등록한 작업을 자동으로 실행시키는 것
→ Spring Scheduler, Quartz 등

**2. 좋아요마다 부분 업데이트 (증분 업데이트):**

`방식`: 
좋아요가 발생할 때마다 랭킹을 바로 재계산하는 대신, 해당 강의의 점수만 변경하고 전체 순위를 완전히 다시 계산하는 것이 아니라, 필요한 부분만 업데이트한다.
`장점`:
순위 변동이 자주 발생하는 상위 몇 개 강의에만 영향을 주므로, 오버헤드가 크지 않다.
실시간 순위 변동을 감지할 수 있다.
`단점`:
자주 업데이트할 경우, 여전히 일부 부하가 발생할 수 있다.

**3. 하이브리드 방식 (배치 + 실시간 업데이트):**

`방식`: 
좋아요가 적은 경우에는 주기적으로 배치 처리하지만, 특정 조건(예: 좋아요 수가 급격히 증가할 때)에서는 실시간으로 해당 강의의 랭킹을 재조정하는 방식.
`장점`: 
필요할 때만 실시간 업데이트를 수행하므로 오버헤드를 줄이면서도 실시간성을 어느 정도 유지할 수 있다.
`단점`: 
복잡한 로직 구현이 필요할 수 있다.

#### 선택

**1. Hacker News Algorithm**

: 시간에 따른 순위변동과 급격한 좋아요를 받은 대상에 대하여 높은 순위를 배정하기 위해 사용한다.  
단, 좋아요 대신 라이브 강의 입장을 대상으로 한다.

**2. Spring Batch**

: 기존 프로세스에 영향을 주지 않으면서 전체 트랜젝션을 보장하고 대용량 처리에 유리한 Spring batch를 이용하여 수행한다.

**3. Redis**

: 빠른 읽기/쓰기가 가능하고, 인메모리 데이터베이스이므로 순위나 스코어 데이터를 저장하기에 적합gk다. 
특히, Redis의 Sorted Set 자료구조를 사용하면, 자동으로 순위를 계산하고 관리할 수 있어 편리하다.

## 진행 내용

### 선행기술 연구

**1. Hacker News Algorithm**


**2.Spring Batch**

[Spring Batch 설명]({{site.url}}/spring/2024/09/27/spring_batch.html)


### 프로젝트 설계

**대상 테이블**
![ClassRoomERD]({{site.url}}/PostImages/2024-09-23-Hacker-News-Algorithm/image.png/ClassRoomERD.png)

**인덱스 사용**

순위 결정을 위한 score 칼럼에 인덱스를 적용하면, 순위 계산 시 성능이 향상된다. 
인덱스는 데이터를 빠르게 정렬하고 검색하는 데 도움이 될것이다.
인덱스는 삽입/업데이트 성능에 약간의 오버헤드를 가져올 수 있지만 하루에 한 번 업데이트되는 상황에서는 큰 문제가 없을 것이다.

**캐시 사용**

하루에 한 번 순위를 계산한 후 자주 조회될 순위 데이터를 Redis에 캐시해두면 API응답 속도가 빨라질것


**로직**

1. 수강생 수를 좋아요 수로 판단한다.
2. 배치를 이용하여 하루 1번(오후6시) 순위를 계산한다.
   : 순위계산은 하나의 트렌젝션으로 모두 수행되거나 전혀 수행되지 않아야한다.
3. 순위는 순위테이블에 저장한다.

### 적용

1. 순위 테이블 생성
2. 

## 결론

## 참조

[Hacker News Algorithm 소개](https://dkswnkk.tistory.com/738)

[Ranking Algorithm 자세한](https://destiny738.tistory.com/581)

[Hacker News Algorithm](https://medium.com/hacking-and-gonzo/how-hacker-news-ranking-algorithm-works-1d9b0cf2c08d)

[Reddit Ranking Algorithm](https://medium.com/jp-tech/how-are-popular-ranking-algorithms-such-as-reddit-and-hacker-news-working-724e639ed9f7)

[Spring Batch와 Spring Scheduler의 차이점 소개(블로그)](https://yermi.tistory.com/entry/Spring-Batch%EC%99%80-Scheduler%EC%9D%98-%EC%B0%A8%EC%9D%B4-Spring-Scheduler-%EC%82%AC%EC%9A%A9%EB%B0%A9%EB%B2%95)