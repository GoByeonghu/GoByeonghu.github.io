---
layout: post
title: Spring 시작하기
subtitle: Spring 시작하기
categories: 
    - Spring
tags: [spring]
---
## Spring이란

**자바 기반의 애플리케이션을 쉽게 만들고 배포할 수 있도록 도와주는 오픈소스 프레임워크**

### Spring 등장 배경
- 엔터프라이즈 급 프로젝트를 위해 탄생
- 이전에는 자바로 엔터프라이즈 서비스 개발 위해 Java EE (Java Platform Enterprise Edition) 라는 API에 포함된 Enterprise Java Beans(이하 EJB) 를 사용해서 개발했지만 문제가 있었다.
    - 비싸다 : EJB를 사용해서 서버를 운영하기 위해선 2000년대 초반 기준 최소 수천 달러에서 수십만 달러를 서버운영비와 라이센스 비용으로 지출해야 했다. 
    - 설정과 배포가 복잡하고 무겁다.
- 로드 존슨은 EJB 진영에 “내가 짜도 너희보단 잘 짜겠다 (I could do it better)"라며 직접 만든 예제 코드 3만줄을 책을통해 공개, 이것이 스프링의 시초이다.
    > "내가 짜도 너희보단 잘 짜겠다 (I could do it better)" 라는 어록을 남겼다.
- 출간 이후 유겐 휠러와 얀 카로프가 로드 존슨에게 오픈소스 프로젝트를 제안하여 지금의 스프링 탄생
- 약력
    - 03년 스프링 프레임워크 1.0 - XML
    - 06년 스프링 프레임워크 2.0 - XML 편의기능
    - 09년 스프링 프레임워크 3.0 - 자바 코드로 설정
    - 13년 스프링 프레임워크 4.0 - 자바 8
    - 14년 스프링 부트 1.0 출시
    - 17년 스프링 프레임워크 5.0, 스프링 부트 2.0 - 리액티브 프로그래밍 지원
    - 비동기 non blocking 개발 - node 처럼 개발 가능해짐
    - 22년 스프링 5.3.x, 스프링 부트 2.6
    - 24년 스프링 프레임워크 6.x, 스프링 부트 3.x
    > 2013년 쯤 스프링4.0이 전자정부 프레임워크로 체댁되며 한국에서 특히 많이 사용된다.

>넷스케이프에서는 웹사이트 개발자를 위해 스크립트 언어를 개발 하던 중에 자바는 너무 무겁고 어렵다는 이유로 브랜든 아이카라는 개발자를 통해 Javascript를 10 일만에 개발해낸다.

### Spring의 특징
- 객체지향 언어가 가진 강력한 특징을 살려내는 프레임워크


## Spring 초기 세팅 및 기본적인 사용방법

### 환경 세팅
- gradle은 groovy사용
  - kts는 코틀린
  - groovy는 자바
- jdk: coretto17
- java17사용
- 패키징은 jar사용
  - war:
  - jar:
- build.gradle 설정
    ```bash
    plugins {
        id 'java'
        id 'org.springframework.boot' version '3.3.0'
        id 'io.spring.dependency-management' version '1.1.5'
    }

    // 이 부분은 프로젝트 패키지 구조다.
    // 프로젝트마다 모두 다르다.
    group = 'org.ej31'   
    version = '1.0-SNAPSHOT'

    java {
        sourceCompatibility = '17'
    }

    configurations {
        compileOnly {
            extendsFrom annotationProcessor
        }
    }

    repositories {
        mavenCentral()
    }

    dependencies {
        implementation 'org.springframework.boot:spring-boot-starter-jdbc'

        developmentOnly 'org.springframework.boot:spring-boot-devtools'
        annotationProcessor 'org.springframework.boot:spring-boot-configuration-processor'
        testImplementation 'org.springframework.boot:spring-boot-starter-test'
        testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
    }

    tasks.named('test') {
        useJUnitPlatform()
    }
    ```
    - 구성요소
        - plugins
        - java
        - configurations
        - repository : 디펜던시의 패키지 가지고 오는 곳
            - mavenCentral : 우리가 선택한 것
            - google : 과거에는 많이 사용했지만 요즘은 mavenCentral로 충분하다
        - dependencies : 네이븐 리파지토리에서  패키지자동으로 다운받아준다
            - spring-boot-starter-jdbc'  :jdbc를 스프링 부트에서 사용하기 쉽게 해준다.
            

- Load Gradle Change(코끼리 버튼) 클릭하여 적용
- 세팅-> annotaion 검색 -> enable 체크

> 스피링부트 설정 사이트에서 하면 한번에 해결가능

### 동작 원리

1. 컨테이너 스캔하면 bean을 컨테이너에 등록한다.(넣는다.)
- 컨테이너 : 스프링 프레임워크에서 "컨테이너"란 스프링 애플리케이션의 모든 객체를 관리하고 조정하는 중앙 제어 단위를 의미한다. 구체적으로는, 애플리케이션에서 사용하는 객체(빈, Bean)를 생성하고, 객체들 간의 의존성을 주입하며, 생명주기를 관리하는 역할
- 빈: 빈은 스프링 프레임워크에서 생성하고 관리하는 객체(Service,Component,Repository 어노테이션 붙은 것)
- context: 컨테이너를 호출한것, 이 안에 bean 집어 넣는다.

2. 컨테이너에 등록된 bean을 필요시에 꺼내어 사용한다.


### 구현

#### 1. 베이스 패키지
   
```java
// AppConfig.java
@Configuration
@ComponentScan(basePackages = "org.ej31") // 패키지 경로는 올바르게 설정하자
public class AppConfig {}
```
- Configuration : 
- ComponentScan : basePackages를 돌며 컴포넌트 들을 스캔한다.
- bean 뭘리파이: 


#### 2. main클레스 (엔트리 포인트)
   
```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class SampleApplication {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
       
        // some code..
              
    }
}
```

#### 3. 인터페이스

```java
public interface Calculator {
    int add(int a, int b);
    int subtract(int a, int b);
    int multiply(int a, int b);
    int divide(int a, int b);
}
```

#### 4. 구현체

```java
import org.springframework.stereotype.Service;

@Service  // 이 어노테이션으로 인해 스프링은 컴포넌트 스캔 때 "아 내 컨테이너에 이 클래스를 추가해야겠다" 라고 판단합니다.
public class CalculatorService implements Calculator {

    @Override
    public int add(int a, int b) {
        return a + b;
    }

    @Override
    public int subtract(int a, int b) {
        return a - b;
    }

    @Override
    public int multiply(int a, int b) {
        return a * b;
    }

    @Override
    public int divide(int a, int b) {
        if (b == 0) {
            throw new IllegalArgumentException("0으로 나누셨군요? 주먹으로 머리 꽝꽝 때리십쇼");
        }
        return a / b;
    }
}
```


#### 5. 메인에서 사용

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class CalculatorApplication {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
				Calculator calculator = context.getBean(Calculator.class);

        System.out.println("덧셈 결과: " + calculator.add(10, 5));
        System.out.println("뺄셈 결과: " + calculator.subtract(10, 5));
        System.out.println("곱셈 결과: " + calculator.multiply(10, 5));
        System.out.println("나눈 결과: " + calculator.divide(10, 5));
    }
}
```
-  ApplicationContext : 컨테이너
- AnnotationConfigApplicationContext(AppConfig.class); // 컨피그 클레스 불러와서 스캔 시작
- .getBean() 이라는 메서드를 통해 스프링 컨테이너 안에 있는 클래스들 (= Bean) 중 하나를 꺼내온다.

### 알수있는 점

- spring을 사용하지 않았다면 main에서 구현체를 직접 인스턴스화한다. : 결합이 강해진다.

- spring을 사용하면 컨테이너가 알아서 연결해줌으로 직접 인스턴스화 할 필요 없다.



## 참고
- 유용한 단축키
  
  - command + shift + . : 파인더에서 보기
  - command + ; : 버전 바꾸기 설정 가능
  - option + enter : 컨텍스트 메뉴
  - command + l (?) : Show Context Menu(import 쉽게 한다.)