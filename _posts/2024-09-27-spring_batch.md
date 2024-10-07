---
layout: post
title: Spring Batch
subtitle: Spring Batch
categories: 
    - Spring
tags: [spring]
---

# Spring Scheduler vs Spring Batch

Spring Scheduler와 Spring Batch는 Spring Framework에서 제공하는 작업 스케줄링 및 배치 처리 도구로, 각각의 목적과 기능이 다르다. 두 가지를 비교하고 차이점 및 장단점을 살펴본다.

## 1. Spring Scheduler

Spring Scheduler는 특정 시간에 작업을 자동으로 실행하기 위한 간단한 스케줄링 도구다. 주기적으로 수행해야 하는 작업을 간단하게 설정하고 실행할 수 있다.

### 주요 기능
- 특정 주기(초, 분, 시간, 일 등)나 크론 표현식을 통해 스케줄링 가능
- 주기적으로 반복해야 하는 단일 작업에 적합
- 트랜잭션, 작업 재시도, 병렬 처리 등을 직접 처리할 필요 없음

### 사용 예시

```java
import org.springframework.scheduling.annotation.Scheduled;  
import org.springframework.stereotype.Service;  

@Service  
public class SimpleSchedulerService {  
    @Scheduled(cron = "0 0 0 * * ?")  
    public void executeTask() {  
        System.out.println("작업이 실행되었습니다.");  
    }  
}  
```

### 장점
- 구현이 간단: 코드가 매우 단순하고 설정이 필요 없다.
- 빠른 설정: 주기적 작업을 어노테이션으로 설정할 수 있다.
- 경량: 간단한 작업에 적합하며 오버헤드가 거의 없다.
- 유연한 스케줄 설정: 크론 표현식 또는 고정 주기로 작업을 설정할 수 있다.

### 단점
- 대용량 데이터 처리에 부적합: 대량 데이터나 복잡한 트랜잭션 작업에 적합하지 않다.
- 오류 처리 및 재시도 부족: 예외 처리나 재시도는 직접 구현해야 한다.
- 상태 관리 부족: 작업 중단 시 자동 복구나 재시도가 어렵다.

## 2. Spring Batch

Spring Batch는 대규모 데이터 처리, 트랜잭션 관리, 작업 재시도 및 상태 추적 등을 위해 설계된 배치 처리 도구다. 주로 대량의 데이터를 주기적으로 처리하거나 비즈니스 프로세스를 자동화하는 데 사용된다.

### 주요 기능
- 대량 데이터를 처리할 수 있는 배치 작업에 적합
- 트랜잭션 관리, 작업 상태 추적, 재시도 및 복구 기능 제공
- 멀티스레드 및 병렬 처리 지원

### 사용 예시

```java
import org.springframework.batch.core.Job;  
import org.springframework.batch.core.Step;  
import org.springframework.batch.core.configuration.annotation.JobBuilderFactory;  
import org.springframework.batch.core.configuration.annotation.StepBuilderFactory;  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  

@Configuration  
public class BatchConfiguration {  
    @Bean  
    public Job rankingJob(JobBuilderFactory jobBuilderFactory, StepBuilderFactory stepBuilderFactory) {  
        Step step = stepBuilderFactory.get("step")  
                .tasklet((contribution, chunkContext) -> {  
                    System.out.println("랭킹 계산 작업 수행");  
                    return null;  
                }).build();  

        return jobBuilderFactory.get("rankingJob")  
                .start(step)  
                .build();  
    }  
}  
```

### 장점
- 대용량 데이터 처리 최적화: 페이징이나 청크(chunk) 방식으로 대량 데이터를 효율적으로 처리
- 트랜잭션 관리 및 복구: 작업 실패 시 트랜잭션 롤백 및 작업 재시도 가능
- 작업 상태 관리: 작업 성공, 실패, 중단 상태를 저장하고 중단된 작업을 재개할 수 있음
- 확장성: 병렬 처리 및 다중 스레드 처리 가능
- 재사용 가능한 작업 정의: 여러 단계로 나뉘는 복잡한 작업 정의 가능

### 단점
- 설정 복잡도: Spring Scheduler에 비해 설정과 구현이 복잡
- 오버헤드: 작은 규모의 작업에는 과도한 오버헤드가 발생
- 실시간 처리 부족: 실시간 처리가 필요한 작업에는 적합하지 않음

## 3. 차이점 요약

| **특징**                   | **Spring Scheduler**        | **Spring Batch**                |
|----------------------------|-----------------------------|---------------------------------|
| **주요 목적**               | 간단한 스케줄링 작업        | 대규모 배치 작업                |
| **대상 작업**               | 단순 작업 (정기적인 실행)    | 대량 데이터 처리, 복잡한 트랜잭션 |
| **오류 처리**               | 직접 구현 필요              | 자동 재시도 및 복구 기능 제공   |
| **상태 관리**               | 작업 상태 추적 없음          | 작업 상태 저장 및 재개 기능     |
| **성능 최적화**             | 소규모 작업에 적합           | 대규모 데이터 처리에 최적화     |
| **구현 복잡도**             | 간단한 설정                 | 복잡한 설정 필요               |
| **주기적 처리**             | 주기적 작업 스케줄링         | 비즈니스 배치 처리              |
| **병렬 처리**               | 기본적으로 지원하지 않음     | 멀티스레드, 병렬 처리 가능      |

## 결론

- Spring Scheduler는 주기적으로 간단한 작업을 수행할 때 적합하며, 빠르게 구현할 수 있다.
- Spring Batch는 대량 데이터 처리나 복잡한 트랜잭션 관리가 필요한 경우에 적합하며, 오류 처리 및 상태 추적을 자동화할 수 있다.

작업의 복잡도와 규모에 따라 두 가지 중 적합한 방식을 선택하면 된다.