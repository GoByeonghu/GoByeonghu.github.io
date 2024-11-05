---
layout: post
title: "Redis: Concepts and Caching Techniques"
subtitle: "Understanding Redis for Fast Data Access and Practical Caching Solutions"
categories: 
  - Redis
tags: [redis, spring]
---

## Redis
---

## Redis란
---

### 1. Redis 개요

Redis는 오픈 소스 인메모리 데이터 구조 저장소로, 키-값 쌍 형태로 데이터를 저장하는 NoSQL 데이터베이스이다. Redis는 높은 성능과 유연성을 제공하며, 다양한 데이터 구조를 지원한다. Redis는 데이터베이스, 캐시 및 메시지 브로커로 사용될 수 있다.

### 2. Redis의 특징

#### 2.1 인메모리 데이터 저장소

Redis는 모든 데이터를 메모리에 저장하여 빠른 읽기 및 쓰기 성능을 제공한다. 이는 디스크 기반 데이터베이스에 비해 월등한 속도를 자랑한다. 하지만, 메모리의 제한으로 인해 저장할 수 있는 데이터의 양이 물리적 메모리 용량에 따라 제한된다.

#### 2.2 다양한 데이터 구조 지원

Redis는 다음과 같은 여러 데이터 구조를 지원한다:

- **문자열 (String)**: 가장 기본적인 데이터 구조로, 간단한 값을 저장할 수 있다.
- **리스트 (List)**: 순서가 있는 값의 컬렉션으로, 스택 및 큐의 기능을 지원한다.
- **셋 (Set)**: 중복되지 않는 값의 집합으로, 집합 연산을 수행할 수 있다.
- **정렬된 셋 (Sorted Set)**: 점수를 기반으로 정렬된 값의 집합으로, 특정 순서로 데이터를 정렬할 수 있다.
- **해시 (Hash)**: 필드-값 쌍으로 구성된 데이터 구조로, 객체와 같은 복잡한 데이터를 저장할 수 있다.

#### 2.3 퍼시스턴스

Redis는 데이터를 메모리에 저장하면서도, 데이터를 영속적으로 저장하기 위한 다양한 퍼시스턴스 옵션을 제공한다. RDB 스냅샷 및 AOF(Append-Only File) 로그 방식을 통해 데이터 손실을 방지할 수 있다.

#### 2.4 클러스터링 및 복제

Redis는 수평적 확장을 위한 클러스터링 기능을 제공한다. 여러 Redis 인스턴스를 클러스터링하여 데이터를 분산 저장할 수 있으며, 복제 기능을 통해 고가용성을 유지할 수 있다. 주 서버와 복제 서버 간의 데이터 동기화가 가능하다.

### 3. Redis 캐시 사용법

Redis는 주로 캐시 시스템으로 사용되며, 그 사용법은 다음과 같다:

#### 3.1 데이터 저장

캐시로 사용하기 위해 Redis에 데이터를 저장할 때는 `SET` 명령어를 사용한다. 예를 들어, 웹 페이지의 데이터를 캐시하기 위해 다음과 같이 저장할 수 있다.

```
SET "page" "This is the homepage content."
```


#### 3.2 데이터 조회

저장된 데이터를 조회하기 위해 `GET` 명령어를 사용한다. 캐시에 저장된 데이터를 요청할 때 다음과 같이 사용할 수 있다.

```
GET "page"
```


#### 3.3 만료 시간 설정

Redis는 캐시 데이터에 만료 시간을 설정할 수 있다. 이는 사용자가 정의한 시간 후에 자동으로 데이터가 삭제되도록 하는 기능이다. `EXPIRE` 명령어를 사용하여 설정할 수 있다.

```
EXPIRE "page" 3600 # 1시간 후 만료
```

### 4. 결론

Redis는 높은 성능과 유연성을 제공하는 인메모리 데이터 저장소로, 다양한 데이터 구조와 캐시 사용법을 지원한다. Redis의 장점은 성능뿐만 아니라, 다양한 기능을 통해 개발자들이 쉽게 사용할 수 있도록 돕는 것이다. 이러한 이유로 Redis는 웹 애플리케이션의 캐시 시스템으로 매우 유용하다.

## 캐시(Cache)
---

캐시는 데이터를 보다 빠르게 접근하기 위해 사용하는 메모리의 일종이다. 일반적으로 자주 사용되는 데이터나 연산 결과를 저장하여 데이터 접근 시간을 단축시키는 데 목적이 있다. 캐시는 CPU, 웹 애플리케이션, 데이터베이스 등 다양한 영역에서 사용된다. 캐시는 기본적으로 두 가지 구조, 즉 하드웨어 캐시와 소프트웨어 캐시로 구분할 수 있다.

### 2. 캐시의 동작 원리

캐시는 데이터의 복사본을 임시로 저장하여 빠른 접근을 가능하게 한다. 데이터가 캐시에 저장되는 과정은 다음과 같다:

#### 2.1 캐시 히트(Cache Hit)

캐시 히트는 요청된 데이터가 캐시에 존재할 때 발생한다. 이 경우, 데이터는 캐시에서 즉시 반환되어 빠른 응답을 제공한다. 캐시 히트율이 높을수록 시스템의 성능이 향상된다.

#### 2.2 캐시 미스(Cache Miss)

캐시 미스는 요청된 데이터가 캐시에 존재하지 않을 때 발생한다. 이 경우, 시스템은 메인 저장소(예: 디스크)에서 데이터를 로드해야 하므로, 응답 시간이 증가한다. 캐시 미스는 성능 저하의 주 원인이다.

#### 2.3 캐시 적중률(Cache Hit Rate)

캐시 적중률은 캐시에서 성공적으로 요청된 데이터의 비율을 나타낸다. 이는 캐시 성능의 중요한 지표로, 적중률이 높을수록 캐시의 효율성이 높다고 할 수 있다. 캐시 적중률은 다음과 같이 계산된다:

**Cache Hit Rate = (Cache Hits) / (Cache Hits + Cache Misses)**


### 3. 캐시 메모리의 종류

캐시 메모리는 사용되는 위치에 따라 다양한 종류가 있다. 가장 일반적인 캐시 메모리는 다음과 같다:

#### 3.1 CPU 캐시

CPU 캐시는 프로세서와 메인 메모리 사이에 위치하며, 데이터 접근 속도를 향상시키기 위해 CPU에서 자주 사용되는 데이터와 명령어를 저장한다. CPU 캐시는 일반적으로 L1, L2, L3로 계층화되어 있으며, L1이 가장 빠르고 용량이 적다.

#### 3.2 웹 캐시

웹 캐시는 웹 페이지와 리소스(이미지, 스크립트 등)를 저장하여 사용자에게 빠르게 제공하기 위한 캐시이다. 이는 웹 서버와 클라이언트 간의 대역폭을 절약하고 응답 시간을 줄인다. 웹 캐시는 브라우저 캐시와 프록시 캐시로 구분될 수 있다.

#### 3.3 데이터베이스 캐시

데이터베이스 캐시는 데이터베이스 쿼리 결과를 저장하여 후속 쿼리의 성능을 향상시키는 역할을 한다. 이는 디스크 I/O를 줄이고 쿼리 응답 속도를 높인다. 데이터베이스 캐시는 메모리 내에서 동작하며, 레디스(Redis)나 멤캐시(Memcached)와 같은 시스템을 활용할 수 있다.

### 4. 캐시 전략

캐시의 효과를 극대화하기 위해 다양한 캐시 전략이 사용된다. 주요 캐시 전략은 다음과 같다:

#### 4.1 LRU (Least Recently Used)

LRU 전략은 가장 오랫동안 사용되지 않은 데이터를 캐시에서 제거하는 방식이다. 이 방식은 사용자의 행동 패턴을 기반으로 자주 사용되는 데이터를 유지하고, 불필요한 데이터를 제거한다.

#### 4.2 LFU (Least Frequently Used)

LFU 전략은 가장 적게 사용된 데이터를 제거하는 방식이다. 사용 빈도수를 기반으로 데이터를 관리하며, 사용 빈도가 높은 데이터를 우선적으로 캐시한다.

#### 4.3 TTL (Time to Live)

TTL 전략은 데이터가 캐시에 저장된 후 일정 시간이 지나면 자동으로 만료되도록 설정하는 방식이다. 이 전략은 데이터의 유효성을 보장하고, 오래된 데이터를 제거하여 캐시를 최적화하는 데 도움을 준다.

### 5. 캐시의 장점과 단점

#### 5.1 장점

- **성능 향상**: 캐시는 데이터를 메모리에 저장하여 빠른 접근이 가능하므로 시스템 성능을 크게 향상시킨다.
- **대역폭 절약**: 웹 캐시와 같은 캐시는 서버와 클라이언트 간의 데이터 전송량을 줄여 대역폭을 절약한다.
- **응답 시간 단축**: 캐시의 사용은 데이터 요청 시 응답 시간을 단축시킨다.

#### 5.2 단점

- **메모리 비용**: 캐시는 메모리를 사용하므로, 대량의 데이터를 저장할 경우 메모리 비용이 증가할 수 있다.
- **데이터 일관성 문제**: 캐시에 저장된 데이터와 원본 데이터 간의 일관성 문제를 발생시킬 수 있다. 이는 캐시가 만료되거나 업데이트되지 않았을 때 문제가 된다.


### 6. 동기화 전략

캐시의 동기화 전략은 캐시 데이터와 원본 데이터 간의 일관성을 유지하기 위해 사용되는 방법들이다. 캐시가 원본 데이터와 불일치할 경우, 캐시를 사용할 때 부정확한 정보가 제공될 수 있으므로 적절한 동기화 전략이 필요하다. 
캐시의 동기화 전략은 성능과 데이터 일관성을 유지하는 데 중요한 역할을 한다. 시스템의 요구 사항과 사용 패턴에 따라 적절한 동기화 전략을 선택하고 구현하는 것이 필요하다. 각 전략은 장단점이 있으며, 특정 상황에 따라 최적의 방법을 선택해야 한다.
여기에서는 주요 동기화 전략 몇 가지를 설명하겠다.

#### 1. 캐시 무효화(Invalidation)

캐시 무효화는 원본 데이터가 변경될 때 해당 데이터와 관련된 캐시 항목을 삭제하는 방식이다. 이는 데이터가 변경될 때 즉시 반영될 수 있도록 보장하는 방법이다. 캐시 무효화는 두 가지 방법으로 이루어질 수 있다.

- **명시적 무효화**: 애플리케이션이 원본 데이터를 수정할 때, 해당 데이터와 연관된 캐시 항목을 명시적으로 삭제하는 방법이다. 이 경우 개발자는 캐시와 원본 데이터 간의 동기화를 관리해야 한다.

- **자동 무효화**: 데이터베이스의 트리거와 같은 메커니즘을 사용하여 원본 데이터가 변경될 때 자동으로 캐시 항목이 삭제되도록 설정하는 방법이다.

#### 2. 캐시 업데이트(Updating)

캐시 업데이트 전략은 원본 데이터가 변경될 때 해당 변경 사항을 캐시에 즉시 반영하는 방식이다. 이는 다음과 같은 방법으로 수행될 수 있다.

- **write-through 캐시**: 데이터가 캐시에 저장될 때 동시에 원본 데이터에도 저장되는 방식이다. 이 방법은 캐시와 원본 데이터 간의 일관성을 유지하는 데 유리하지만, 성능 저하가 발생할 수 있다.

- **write-back 캐시**: 데이터가 캐시에 먼저 저장되고, 일정 시간이 지나거나 특정 조건이 충족될 때 원본 데이터에 반영되는 방식이다. 이 방법은 성능을 향상시킬 수 있지만, 원본 데이터와의 일관성 유지가 복잡해질 수 있다.

#### 3. TTL(Time to Live)

TTL은 캐시 항목이 생성된 후 일정 시간이 지나면 자동으로 만료되도록 설정하는 방식이다. 이 전략은 캐시 데이터의 유효성을 보장하고, 오래된 데이터를 제거하여 캐시를 최적화하는 데 도움을 준다. TTL은 캐시 데이터의 변경 주기와 사용 패턴에 따라 적절하게 설정되어야 한다.

#### 4. 주기적 동기화

주기적 동기화는 캐시와 원본 데이터 간의 동기화를 주기적으로 수행하는 방법이다. 이 방법은 캐시 데이터가 일정한 주기로 원본 데이터와 동기화되도록 하여 데이터 일관성을 유지하는 데 도움을 준다. 주기적인 동기화는 대개 백그라운드 작업으로 수행되며, 데이터베이스와의 I/O 작업을 최소화할 수 있다.

#### 5. 읽기-쓰기 동기화

읽기-쓰기 동기화는 캐시에서 데이터를 읽거나 쓸 때마다 캐시와 원본 데이터 간의 일관성을 유지하는 방법이다. 이 전략은 요청 시 캐시에서 먼저 데이터를 찾고, 캐시 미스가 발생할 경우 원본 데이터에서 읽어오는 방식으로 구현될 수 있다. 캐시에서 데이터를 업데이트할 때는 원본 데이터에도 즉시 반영되도록 처리해야 한다.

### 7. 결론

캐시는 현대 컴퓨팅 시스템에서 필수적인 요소로, 성능 최적화 및 리소스 절약에 큰 기여를 한다. 캐시의 효과적인 구현과 관리 방법은 시스템의 전반적인 성능 향상에 중요한 역할을 한다. 따라서 개발자들은 캐시의 원리와 다양한 전략을 이해하고 활용해야 한다.


## Spring에서 Redis cache 사용하기
---

### 설정

- build.gradle

```gradle
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-redis'
}
```

- RedisConfig

```java
@Configuration
public class RedisConfig {

    @Value("${spring.data.redis.host}")
    private String redisHost;

    @Value("${spring.data.redis.port}")
    private int redisPort;

    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        return new LettuceConnectionFactory(new RedisStandaloneConfiguration(redisHost, redisPort));
    }

    @Bean
    public RedisTemplate<String, Object> redisTemplate() {
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(redisConnectionFactory());
        redisTemplate.setKeySerializer(new StringRedisSerializer());

        /* Java 기본 직렬화가 아닌 JSON 직렬화 설정 */
        redisTemplate.setValueSerializer(new GenericJackson2JsonRedisSerializer());

        return redisTemplate;
    }
}
```
Java의 Redis Client에는 Jedis와 Lettuce가 있지만, 스프링부트는 디폴트로 Lettuce를 사용한다. LettuceConnectionFactory를 빈으로 등록한 경우를 가정한다.


- RedisCacheConfig

```java
@EnableCaching
@Configuration
public class RedisCacheConfig {

    @Bean
    @Primary
    public CacheManager monthlyCalendarCacheManager(RedisConnectionFactory redisConnectionFactory) {
        RedisCacheConfiguration redisCacheConfiguration = generateCacheConfiguration()
                .entryTtl(Duration.ofHours(5L));

        return RedisCacheManager.RedisCacheManagerBuilder
                .fromConnectionFactory(redisConnectionFactory)
                .cacheDefaults(redisCacheConfiguration)
                .build();
    }

    private RedisCacheConfiguration generateCacheConfiguration() {
        return RedisCacheConfiguration.defaultCacheConfig()
                .serializeKeysWith(
                        RedisSerializationContext.SerializationPair.fromSerializer(
                                new StringRedisSerializer()))
                .serializeValuesWith(
                        RedisSerializationContext.SerializationPair.fromSerializer(
                                new GenericJackson2JsonRedisSerializer()));
    }
}
```
메서드를 하나 만들고 TTL은 5시간으로 설정했다.
참고로 CacheManager를 빈으로 등록하는 메서드가 여러 개라면, 빈 초기화 과정에서 에러가 발생하므로 `@Primary`로 등록한다.

## 캐시 적용

```java
@Override
@Cacheable(value = "calendarCache", key = "#familyId.concat(':').concat(#yearMonth)", cacheManager = "monthlyCalendarCacheManager")
public ArrayResponse<CalendarResponse> getMonthlyCalendar(String yearMonth, String familyId) {
    ...

    List<CalendarResponse> calendarResponses = getCalendarResponses(familyIds, startDate, endDate);
    return new ArrayResponse<>(calendarResponses);
}
```


> @Cacheable은 캐시 생성 및 전달을 담당하는 어노테이션이다. 
> 캐시에 데이터가 없을 경우에는 기존의 로직을 실행 후 캐시에 데이터를 추가하고, 캐시에 데이터가 있으면 캐시의 데이터를 반환

## 참조
- [redis-cache spring에서 사용법_블로그](https://ji-soo708.tistory.com/31)