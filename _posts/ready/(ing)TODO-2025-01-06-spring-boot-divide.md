---
layout: post
title: Spring Boot에서 Dev, Prod 환경 분리하기
subtitle: Spring Boot에서 환경분리하기 가이드라인
categories: 
  - Spring
tags: [spring]
---

### Spring 프로젝트에서 `application.properties`와 `.env`를 활용해 환경(dev/prod)을 간편하게 변경하는 방법

Spring Boot는 **프로파일(profile)** 기능을 기본으로 제공하며, `.env` 파일을 사용하면 환경 변수를 간편하게 설정 및 관리할 수 있다. 이를 조합하면 환경 변경이 더 유연해진다.

---

#### 1. 환경별 설정 파일 생성
`src/main/resources` 디렉토리 아래에 환경별 설정 파일을 생성한다.

```
src/main/resources
├── application.properties        # 기본 설정
├── application-dev.properties    # dev 환경 설정
└── application-prod.properties   # prod 환경 설정
```

##### 예시: `application-dev.properties`
```
spring.datasource.url=jdbc:mysql://localhost:3306/dev_db
spring.datasource.username=dev_user
spring.datasource.password=${DB_PASSWORD}
server.port=8081
```

##### 예시: `application-prod.properties`
```
spring.datasource.url=jdbc:mysql://prod-db.example.com:3306/prod_db
spring.datasource.username=prod_user
spring.datasource.password=${DB_PASSWORD}
server.port=8080
```

---

#### 2. `.env` 파일 생성
루트 디렉토리에 `.env` 파일을 만들어 환경 변수를 정의한다. 

##### 예시: `.env`
```
SPRING_PROFILES_ACTIVE=dev
DB_PASSWORD=secret_password
```

##### 운영 환경(prod)에서는 다음과 같이 환경에 맞게 `.env`를 설정한다.
```
SPRING_PROFILES_ACTIVE=prod
DB_PASSWORD=prod_secret_password
```

---

#### 3. Spring에서 `.env` 파일 읽기
`.env` 파일을 Spring에서 읽으려면 **Spring Boot dotenv** 라이브러리를 추가한다.

##### 1) Maven에 의존성 추가
```xml
<dependency>
    <groupId>io.github.cdimascio</groupId>
    <artifactId>dotenv-java</artifactId>
    <version>3.0.0</version>
</dependency>
```

##### 2) `.env` 파일을 읽도록 설정
`.env` 파일의 내용을 Spring 프로젝트에서 사용할 수 있도록 `application.properties`에서 참조한다.

##### 예시: `application.properties`
```
spring.profiles.active=${SPRING_PROFILES_ACTIVE}
```

---

#### 4. 프로파일 활성화
다양한 방법으로 활성화할 프로파일을 변경할 수 있다.

##### 1) **JVM 옵션 사용**
애플리케이션 실행 시 `-Dspring.profiles.active=prod` 옵션을 추가해 환경을 변경한다.
```
java -jar -Dspring.profiles.active=prod myapp.jar
```

##### 2) **환경 변수 사용**
운영 환경에서 환경 변수를 설정한다.
```
export SPRING_PROFILES_ACTIVE=prod
```

##### 3) **.env를 활용**
`.env` 파일에 환경을 지정하고, 라이브러리가 이를 읽어 동작하도록 설정한다.
```
SPRING_PROFILES_ACTIVE=prod
```

---

#### 5. 환경별로 `.env`와 프로파일을 함께 활용한 예시
`application.properties`와 `application-{profile}.properties` 파일을 조합하고 `.env` 파일을 활용해 다음과 같은 워크플로우를 구성할 수 있다.

##### 예시: `.env`
```
SPRING_PROFILES_ACTIVE=dev
DB_PASSWORD=development_password
```

##### 예시: `application.properties`
```
spring.profiles.active=${SPRING_PROFILES_ACTIVE}
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
logging.level.org.springframework=INFO
```

##### 예시: `application-dev.properties`
```
spring.datasource.url=jdbc:mysql://localhost:3306/dev_db
spring.datasource.username=dev_user
spring.datasource.password=${DB_PASSWORD}
server.port=8081
```

##### 예시: `application-prod.properties`
```
spring.datasource.url=jdbc:mysql://prod-db.example.com:3306/prod_db
spring.datasource.username=prod_user
spring.datasource.password=${DB_PASSWORD}
server.port=8080
```

---

### 요약

1. 환경별 `application-{profile}.properties` 파일과 `.env` 파일을 함께 사용하면 환경 구성이 간단해진다.
2. `.env` 파일은 환경 변수(`DB_PASSWORD`, `SPRING_PROFILES_ACTIVE`)를 관리하고, Spring Boot에서 이를 프로파일과 결합한다.
3. Spring Boot dotenv 라이브러리를 사용하면 `.env` 파일의 변수를 손쉽게 읽을 수 있다.
4. 이 방식은 개발 환경과 운영 환경 모두에서 유연하게 적용할 수 있다.
