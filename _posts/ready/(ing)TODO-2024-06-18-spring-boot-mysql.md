---
layout: post
title: Spring Boot MySQL 연결하기
subtitle: Spring Boot에서 MySQL 연결하기
categories: 
  - Spring
tags: [spring, mysql]
---

## MySQL 설정

- 루트 계정 접속

```
mysql -h localhost -u root -p
```

- 유저 생성 및 권한 설정

```
CREATE USER guestbook@localhost IDENTIFIED BY 'connect123!@#';
GRANT ALL PRIVILEGES ON connectdb.* TO 'guestbook'@'localhost';
FLUSH PRIVILEGES:
```

- db 로그인 테스트

```
//동일도메인 접속
mysql -u name -p

//외부에서 접속
mysql -u jini -p -h 123.xxx.xxx.xxx
```

## Spring 설정

- bulid.gradle

```groovy
dependencies {
implementation 'mysql:mysql-connector-java'
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
}
```

- application.properties

```
# MySQL 설정
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver 

# DB Source URL
spring.datasource.url=jdbc:mysql://<IP>:<Port/<DB>?useSSL=false&useUnicode=true&serverTimezone=Asia/Seoul 

# DB username
spring.datasource.username=<username> 

# DB password
spring.datasource.password=<password> 

# true 설정시 JPA 쿼리문 확인 가능
spring.jpa.show-sql=true 

# DDL(create, alter, drop) 정의시 DB의 고유 기능을 사용할 수 있다.
spring.jpa.hibernate.ddl-auto=update 

# JPA의 구현체인 Hibernate가 동작하면서 발생한 SQL의 가독성을 높여준다.
spring.jpa.properties.hibernate.format_sql=true
```

## 주의

- com.mysql.cj.jdbc.Driver 에러

```
java.lang.ClassNotFoundException: com.mysql.cj.jdbc.Driver at java.base/jdk.internal.loader.BuiltinClassLoader.loadClass(BuiltinClassLoader.java:641) at java.base/jdk.internal.loader.ClassLoaders$AppClassLoader.loadClass(ClassLoaders.java:188) at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:525) at java.base/java.lang.Class.forName0(Native Method) at java.base/java.lang.Class.forName(Class.java:467)
```

- 해결법

버전이 바뀌며 라이브러리의 소속과 이름이 바뀌었다.

하단에 

```
runtimeOnly 'com.mysql:mysql-connector-j'
testImplementation 'org.springframework.boot:spring-boot-starter-test'
```
를 추가한다.



## 참조

연결방법

https://velog.io/@sians0209/Spring-Spring-gradle-MySQL-JPA-%EC%97%B0%EB%8F%99


권한 생성 및 부여
https://velog.io/@eigenkyeong/MySQL-%EC%82%AC%EC%9A%A9%EC%9E%90-%EC%A1%B0%ED%9A%8C-%EC%83%9D%EC%84%B1-%EC%A0%9C%EA%B1%B0-%EA%B6%8C%ED%95%9C%EB%B6%80%EC%97%AC


https://blog.jiniworld.me/72#a02 