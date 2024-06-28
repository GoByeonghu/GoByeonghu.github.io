---
layout: post
title: Spring Boot 시작하기
subtitle: Spring Boot의 기본 개념
categories:  
    - Spring
tags: [spring]
---
## Spring Boot란

**스프링 프레임워크를 더 쉽게 사용할 수 있도록 만들어진 확장 프레임워크로, 초기 설정을 자동화하고 내장 웹 서버를 제공하여 빠르게 애플리케이션을 개발할 수 있게하는 도구**

### Spring Boot의 특징

- **관습에 의한 설정(Convention over Configuration):** 스프링 프레임워크의 복잡한 설정을 자동화한다. 관습적으로 개발자들이 사용하는 방식을 차용하였다.

- **초기 설정의 간소화:** 스프링부트는 복잡한 설정을 자동으로 처리

- **내장 웹 서버 제공:** 톰캣(Tomcat), 제티(Jetty) 등 내장 웹 서버를 제공

- **프로덕션 준비:** 프로덕션 환경[1]에서 필요한 여러 설정과 도구들을 기본으로 제공


- **의존성 관리:** 스타터(Starter) 패키지를 통해 필요한 의존성들을 쉽게 관리할


### Spring의 다형성

**스프링은 다형성을 극대화한다. 제어의 역전(IoC), 의존관계주입(DI)은 다형성을 활용해서 역할과 구현을 편리하게 다룰 수 있도록 지원한다.**

- **제어의 역전 (Inversion of Control, IoC):** 전통적인 프로그래밍에서는 객체가 직접 다른 객체를 생성하고 사용하는 방식이 IoC에서는 제어 흐름이 역전되어, 객체의 생성 및 의존성 관리가 개발자가 아닌 프레임워크나 컨테이너에 의해 관리된다. (개발자가 아닌 컨테이너가 담당)
    - 정의: "객체의 생성 및 관리 책임을 프레임워크에 위임"
    - 적용내용: 스프링 컨테이너에서 Bean의 생명주기(생성 -> 의존성 설정 -> 초기화 -> 소멸)를 전부 관리

    <details>
    <summary>IOC의 장점</summary>
    <div markdown="1">

    - 유연성 증가: 의존 관계가 느슨해져 변경에 유연하게 대응할 수 있습니다. 

    - 재사용성 증가: 인터페이스 기반으로 개발하여 코드 재사용성을 높일 수 있습니다.

    - 관심사 분리: 객체 생성과 사용을 분리하여 코드를 더욱 명확하게 만들 수 있습니다.
    - 
    </div>
    </details>

<br/>

- **의존성주입(Dependency Injection, DI):** 컨테이너가 객체 간의 의존 관계를 자동으로 연결하여 객체 간 결합도를 낮추고 유연성을 높인다.
    - 정의: "객체간의 관계를 외부에서 결정함 "
    - 적용내용: Spring에서 @Autowired사용해서 인터페이스의 구현체 결정함
    - 종류
        - 생성자 주입 
        - 세터 주입
        - 필드 주입
            <details>
            <summary>Spring은 생성자 주입을 권장한다.</summary>
            <div markdown="1">

            - 불변성 보장 
            - 순환참조를 컴파일 타임에 잡아 낼 수 있다.
            - 테스트 케이스 작성 시 목킹(Mocking) 하기도 쉽다.
            
            </div>
            </details>

<br/>

- **관점 지향 프로그래밍(Aspect Oriented Programming,AOP):** 핵심 로직과 부가 기능을 분리하여 모듈화하고, 코드 중복을 줄여 유지 보수를 용이하게 합니다.

<br/>

### Container

1. Spring Container

- 애플리케이션에서 사용하는 객체(Bean)의 생성, 관리, 의존성 주입 등을 담당한다. 
  
- 스프링 컨테이너는 객체들의 생명 주기를 관리하고, 필요한 시점에 객체를 생성하고 연결하여 애플리케이션을 실행한다.

- IoC, DI, AOP의 주체 

2. 임베디드 웹 서버 (Embedded Web Server) 컨테이너

- 스프링 부트에 내장된 Tomcat, Jetty, Undertow 등의 웹 서버

- 구조

![spring embeded server]({{site.url}}/PostImages/2024-06-18-spring_boot_intro/1.png)

  <details>
    <summary>Details</summary>
    <div markdown="1">

    1. 사용자(User): 사용자가 웹 애플리케이션에 요청을 보냅니다.

    2. 임베디드 웹 서버(Embedded Web Server): 스프링부트의 내장 웹 서버(Tomcat, Jetty, Undertow)가 요청을 받아들입니다.

    3. 서블릿(Servlet): 내장 웹 서버는 서블릿 컨테이너를 통해 요청을 처리합니다.

    4. 디스패처 서블릿(DispatcherServlet): 서블릿 컨테이너는 요청을 스프링의 DispatcherServlet으로 전달합니다. DispatcherServlet은 요청의 중심 허브 역할을 합니다.

    5. 핸들러 매핑(HandlerMapping): DispatcherServlet은 HandlerMapping을 사용하여 어떤 컨트롤러가 요청을 처리할지 결정합니다.

    6. 컨트롤러(@Controller): 핸들러 매핑에 따라 요청을 처리할 컨트롤러가 결정되고, 해당 컨트롤러가 요청을 처리합니다.

    7. 서비스 레이어(Service Layer): 컨트롤러는 비즈니스 로직을 처리하기 위해 서비스 레이어를 호출합니다.

    8. 레포지토리 레이어(Repository Layer): 서비스 레이어는 데이터베이스 작업을 위해 레포지토리 레이어를 호출합니다.

    9. 뷰 리졸버(ViewResolver): 요청 처리 후, DispatcherServlet은 ViewResolver를 사용하여 응답할 뷰를 결정합니다.

    10. 뷰(View): 최종적으로 결정된 뷰를 통해 사용자에게 응답이 전송됩니다.

    </div>
  </details>

    <br/>

### 자동 설정(Auto Configuration) 

스프링 부트 애플리케이션을 실행할 때, 클래스패스에 있는 라이브러리와 설정된 빈(Bean)을 기반으로 스프링 컨텍스트를 자동으로 구성하는 기능입니다.

스프링 부트는 대부분의 기본 설정을 자동으로 구성하여 개발자가 직접 설정할 필요를 줄여준다.

자동 설정을 통해 개발자는 복잡한 설정을 직접 작성할 필요 없이, 애플리케이션에 필요한 기본 설정을 스프링 부트가 자동으로 처리하게 할 수 있습니다. 이는 개발 생산성을 크게 향상시키고, 설정 오류를 줄여줍니다.


- 동작원리
    - 클래스패스 스캐닝: 애플리케이션을 시작할 때 스프링 부트는 클래스패스를 스캔하여 다양한 라이브러리와 애노테이션을 감지한다.

- 사용방법
    - @SpringBootApplication 사용
    - AppConfig만들어 명시적으로 컴포넌트 스캔안해도 알아서 동작한다.
    - @EnableAutoConfiguration을 포함하고 있어, 애플리케이션 시작 시 자동 설정을 활성화

  <details>
    <summary>사용예시</summary>
    <div markdown="1">

    ```java
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class SpringBootApplication {
        public static void main(String[] args) {
            SpringApplication.run(MySpringBootApplication.class, args);
        }
    }
    ```
    </div>
  </details>


  <details>
    <summary>Details</summary>
    <div markdown="1">

    - @SpringBootApplication
        - 역할
            - 프로그램의 메인 함수가 포함된 클래스의 상단에 포함시키는 어노테이션
            - @EnableAutoConfiguration을 포함하고 있어, 애플리케이션 시작 시 **자동 설정을 활성화**한다.

    - SpringApplication.run(MySpringBootApplication.class, args);
        - 역할
            - 스프링 부트 애플리케이션을 실행하기 위한 핵심 메서드
            - 스프링 부트 애플리케이션을 시작
            - 스프링 컨텍스트(ApplicationContext)를 초기화 
            - 필요한 모든 자동 설정을 구성

        - 수행 과정

            1. 스프링 애플리케이션 초기화
            SpringApplication 클래스는 스프링 부트 애플리케이션의 부트스트래핑(Bootstrapping, 시동시킨다는 뜻)과 실행을 담당하는 클래스입니다. run 메서드는 애플리케이션을 시작하고 실행하는 역할을 합니다.

            2. 애플리케이션 컨텍스트 생성
            SpringApplication.run() 메서드는 스프링 애플리케이션 컨텍스트(ApplicationContext)를 생성하고 초기화합니다. 애플리케이션 컨텍스트는 스프링의 IoC 컨테이너로, 빈(Bean)을 관리하고 의존성을 주입하는 역할을 합니다.

            3. 애플리케이션 클래스 인식
            MySpringBootApplication.class는 스프링 부트 애플리케이션의 메인 클래스를 지정합니다. 이 클래스는 보통 @SpringBootApplication 애노테이션이 붙어 있으며, 애플리케이션의 설정을 정의합니다.

            4. 명령줄 인수 처리
            args는 메인 메서드에서 전달된 명령줄 인수입니다. 이 인수들은 스프링 부트 애플리케이션의 설정에 사용할 수 있습니다.

            5. 자동 설정 및 빈 등록
            SpringApplication.run() 메서드는 자동 설정을 활성화하고, 애플리케이션 컨텍스트에 필요한 빈을 등록합니다. 이를 통해 개발자는 최소한의 설정으로 애플리케이션을 시작할 수 있습니다.
    </div>
  </details>


## Spring Boot 사용방법

### Spring Annotation

스프링 부트에서는 애노테이션을 활용하여 빈을 정의하고, 의존성 주입을 수행한다.

- Properties
    - @Value :프로퍼티 값을 변수로 주입

- Auto Configuration
    - @SpringBootApplication
        : 프로그램의 메인 함수가 포함된 클래스의 상단에 포함시키는 어노테이션

- bean
    - @Component : 일반적인 스프링 빈을 정의할 때 사용합니다.
    - @Service : 서비스 레이어의 빈을 정의할 때 사용합니다.
    - @Repository : 데이터 액세스 레이어의 빈을 정의

- DI
    - @Autowired : 의존성 주입을 수행할 때 사용
    - @Qualifier("V2) : 인터페이스 사용하면 구현체 선택가능(하나의 구현체에 여러 인터페이스 있을때)
        - @Service("v2") 이렇게 이름 지정

### 스타터 (Starter)

- 정의
    - 특정 기능을 구현하는 데 필요한 의존성들을 모아놓은 패키지
    - 의존성 관리를 편리하게 하고, 자동 설정 기능을 제공
- 종류
    - spring-boot-starter-web: 웹 개발
    - spring-boot-starter-data-jpa: JPA 기반 데이터베이스 연동
    - spring-boot-starter-security: 스프링 시큐리티
    - spring-boot-starter-test: 테스트
    - [설정 쉽게 해 주는 사이트](https://start.spring.io/)

- 사용 방법
    - build.gradle (Gradle) 파일에 의존성 추가
    - pom.xml (Maven) 


### 프로퍼티 (Properties)

- 정의
    - 애플리케이션의 설정 정보를 저장하는 파일
- 형식
    - application.properties
    - application.yml
- 사용 방법
    - @Value 어노테이션을 사용하여 프로퍼티 값을 변수로 주입
    - Environment 객체를 사용하여 프로퍼티 값을 가져오기


### 사용 예시

<details>
  <summary>예시코드</summary>
  <div markdown="1">

    ```java
    // UserService.java
    @Service
    public class UserServiceImpl implements UserService {
        private final UserRepository userRepository;
            private final PostService postService;
            
            // 이게 끝이다.
        @Autowired
        public UserService(UserRepository userRepository, PostService postService) {
            this.userRepository = userRepository; // 스프링이 UserRepositoryImpl 클래스를 주입해준다.
            this.postService = postService; // 스프링이 PostServiceImpl 클래스를 주입해준다.
        }
        // ... some code~~~~
    }

    // UserRepository.java
    @Repository
    public interface UserRepository {
        void findById(int id);
        // some code.. ~~
    }

    // UserService.java
    public interface UserService {
        // ... some code ~~~
    }

    // PostService
    public interface PostService {
        // ... some code ~~~
    }

    // PostServiceImpl
    @Service
    public class PostServiceImpl implements PostService {
            // .. do something!
    }

    ```
  </div>
</details>






[1] 프로덕션 환경: 실제 사용자가 애플리케이션을 사용하는 환경으로 로컬 환경과 반대의미이다. 



