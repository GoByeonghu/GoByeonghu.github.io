---
layout: post
title: Bean 살펴보기
subtitle: bean의 생명주기, 스코프를 알아보자
categories: 
  - spring
tags: [spring]
---

## 빈 생명주기 (bean lifecycle)

### 정의

#### 빈(Bean) 

빈은 스프링 프레임워크에서 생성하고 관리하는 객체를 의미합니다.

#### 빈 생명주기 

빈 객체가 생성되고 소멸되는 과정을 말합니다.

### 로그 확인 방법

- application.yml

```yml
logging:
  level:
    root: INFO
    org.springframework.beans.factory: debug 
```

### 생명주기

#### 1. 빈 인스턴스화(Instantiation)

##### 정의

빈이 처음으로 생성되는 단계

##### 사용이유

빈이 메모리에 로드되고 인스턴스로 생성

##### 사용방법

Spring 컨테이너가 빈 정의(어노테이션)를 읽고 빈을 생성

#### 2. 의존성 주입(Dependency Injection)

##### 정의 

빈이 필요로 하는 다른 빈이나 리소스를 주입받는 단계

##### 사용이유

빈이 제대로 동작하려면 필요한 의존성을 주입받아 빈이 독립적으로 설계되고 유지보수성이 높아진다.

##### 사용방법

Spring 컨테이너가 의존성을 주입한다.(@Autowired, @Value 등을 통해 의존성 주입)

**(1)생성자 주입**

**(2)세터 주입** 

**(3)필드 주입**


#### 3. 초기화(Initialization)

##### 정의 

빈이 생성되고 의존성 주입이 완료된 후 추가 초기화 작업을 수행하는 단계

##### 사용이유

빈이 사용되기 전에 필요한 초기 설정을 완료

##### 사용방법

@PostConstruct 애너테이션이나 InitializingBean 인터페이스의 afterPropertiesSet 메서드를 사용


#### 4. 사용(Usage)

##### 정의

빈이 실제로 애플리케이션에서 사용되는 단계

##### 사용이유

애플리케이션의 비즈니스 로직을 처리

##### 사용방법
빈이 필요할 때마다 Spring 컨테이너에서 빈을 가져와 사용

#### 5. 소멸(Destruction)

##### 정의

애플리케이션이 종료되거나 빈이 더 이상 필요하지 않게 되어 소멸되는 단계입니다.

##### 사용이유

원을 해제하고 메모리를 정리하여 메모리 누수를 방지합니다.

##### 사용방법

@PreDestroy 애너테이션이나 DisposableBean 인터페이스의 destroy 메서드를 사용

### 생명주기 콜백

#### 정의

##### 콜백

특정 이벤트나 조건이 발생했을 때 시스템이나 프레임워크가 자동으로 호출하는 메서드
이는 일반적으로 미리 정의해 둔 함수를 특정 시점이나 조건에서 자동으로 호출하는 방식

##### 빈 생명주기 콜백
빈 생명주기 콜백은 스프링 컨테이너가 빈의 생명주기 중 특정 시점(예: 초기화, 소멸 등)에 자동으로 호출하는 메서드

#### 사용 방법

- @PostConstruct: 빈의 초기화 직후에 호출되는 메서드입니다.

- @PreDestroy: 빈의 소멸 직전에 호출되는 메서드입니다

- InitializingBean: afterPropertiesSet 메서드를 구현하여 빈의 초기화 직후에 호출됩니다.

- DisposableBean: destroy 메서드를 구현하여 빈의 소멸 직전에 호출됩니다.

<details>
  <summary>사용예시</summary>
  <div markdown="1">
  
  ```java

    import org.springframework.beans.factory.InitializingBean;
    import org.springframework.beans.factory.DisposableBean;
    import org.springframework.stereotype.Component;

    @Component
    public class SpecialService implements InitializingBean, DisposableBean {

        @Override
        public void afterPropertiesSet() throws Exception {
            // 초기화 콜백 메서드
            System.out.println("빈이 초기화되었습니다.");
        }

        @Override
        public void destroy() throws Exception {
            // 소멸 콜백 메서드
            System.out.println("빈이 소멸됩니다.");
        }
    }

  ```

  </div>
</details>

## bean scope

### 정의
스프링 컨테이너가 관리하는 객체인 빈이 생성되고 유지되는 범위를 정의

### 종류

*** 스프링 배치, 스프링 클라우드, 스프링 데이터 등등에도 모두 각각 스코프가 존재 ***

#### 스프링 전체 단위

스프링 컨테이너가 실행되고 있는 전체 및 그 이상이 범위입니다.
이 스코프는 일반적인 스프링 애플리케이션 전체 및 그 이상을 의미하며, 웹 애플리케이션에 한정되지 않습니다.

- 싱글톤(Singleton)

    - 정의: 애플리케이션에서 하나의 인스턴스만 생성되는 스코프입니다. 반의 디폴트 입니다.

    - 비유: 주방에 있는 하나뿐인 큰 냄비와 같습니다. 여러 사람이 요리하더라도 하나의 큰 냄비를 공유합니다.

    - 사용 이유: 리소스를 절약하고, 모든 사용자에게 동일한 상태를 제공하기 위해 사용합니다.

<details>
  <summary>사용방법</summary>
  <div markdown="1">
  
  ```java
    @Component
    public class MySingletonBean {
        // 싱글톤 빈
    }

  ```

  </div>
</details>

- 프로토타입(Prototype)

    - 정의: 요청할 때마다 새로운 인스턴스를 생성하는 스코프입니다.

    - 비유: 주방에 있는 컵과 같습니다. 서로 다른 사용자는 모두 서로 다른 컵을 사용합니다.

    - 사용 이유: 각 요청마다 별도의 상태를 유지해야 할 때 사용합니다.

<details>
  <summary>사용방법</summary>
  <div markdown="1">
  
  ```java
    @Scope("prototype")
    @Component
    public class MyPrototypeBean {
        // 프로토타입 빈
    }
  ```

  </div>
</details>


#### 스프링 웹 어플리케이션 단위

스프링 웹에서만 유효한 범위를 가지고 있습니다.
따라서 스프링 웹 어플리케이션 단위의 스코프는 가장 큰 범위가 유저의 요청으로부터 응답이 나가는 그 순간까지 입니다.

- 리퀘스트(Request)

    - 정의: HTTP 요청마다 새로운 인스턴스를 생성하는 스코프입니다.

    - 비유: 레스토랑에서 각 손님이 따로 제공받는 메뉴판과 같습니다.

    - 사용 이유: 각 HTTP 요청마다 별도의 상태를 유지해야 할 때 사용합니다. 하나의 요청에 하나의 빈이 생성되고 소멸됩니다.

<details>
  <summary>사용방법</summary>
  <div markdown="1">
  
  ```java
    //방법 1
    @Component
    @RequestScope
    public class MyRequestBean {
        // 리퀘스트 스코프 빈
    }

    //방법2
    @Component
    @Scope(value = WebApplicationContext.SCOPE_REQUEST, proxyMode = ScopedProxyMode.TARGET_CLASS)
    public class MyRequestBean {
        // 리퀘스트 스코프 빈
    }
  ```

  </div>
</details>

- 세션(Session)

    - 정의: HTTP 세션마다 새로운 인스턴스를 생성하는 스코프입니다.

    - 비유: 호텔에 머무는 동안 손님에게 제공되는 방과 같습니다. 머무는 동안 상태가 유지됩니다.

    - 사용 이유: 각 사용자 세션마다 상태를 유지해야 할 때 사용합니다.

<details>
  <summary>사용방법</summary>
  <div markdown="1">
  
  ```java
    //방법 1
    @Component
    @SessionScope
    public class MySessionBean {
        // 세션 스코프 빈
    }

    //방법2
    @Component
    @Scope(value = WebApplicationContext.SCOPE_SESSION, proxyMode = ScopedProxyMode.TARGET_CLASS)
    public class MySessionBean {
        // 세션 스코프 빈
    }
  ```

  </div>
</details>


- 애플리케이션(Application)

    - 정의: 애플리케이션 전체에서 하나의 인스턴스를 생성하는 스코프입니다.

    - 비유: 호텔 전체에서 공유하는 로비와 같습니다. 모든 손님이 로비를 공유합니다.

    - 사용 이유: 애플리케이션 전체에서 공유되어야 하는 데이터를 위해 사용합니다.

<details>
  <summary>사용방법</summary>
  <div markdown="1">
  
  ```java
    //방법 1
    @Component
    @ApplicationScope
    public class MyApplicationBean {
        // 애플리케이션 스코프 빈
    }

    //방법2
    @Component
    @Scope(value = WebApplicationContext.SCOPE_APPLICATION, proxyMode = ScopedProxyMode.TARGET_CLASS)
    public class MyApplicationBean {
        // 애플리케이션 스코프 빈
    }
  ```

  </div>
</details>

### 상황별 사용하면 좋은 스코프

- 싱글톤: 스프링 IoC 컨테이너 전체에서 빈이 상태를 가지지 않거나, 상태가 공유되어야 하는 경우.

- 프로토타입: 각 사용자가 독립적인 상태를 유지해야 하는 경우.

- 리퀘스트: HTTP 요청마다 다른 상태를 유지해야 하는 경우.

- 세션: 사용자 세션마다 상태를 유지해야 하는 경우.

- 애플리케이션:  웹 애플리케이션 전체에서 (*서블릿 수준에서) 공유 되어야하는 데이터를 관리할 때 사용. (웹 애플리케이션 설정값 같은 것들)


## 스프링 프록시 모드

### 원리

**어떤 객체를 사용하려고 할 때 해당 객체에 직접 요청하는 것이 아닌 중간에 가짜 프록시 객체(대리인)를 두어서 프록시 객체가 대신해서 요청을 받아 실제 객체를 호출해 주도록 하는 것**

- 프록시 모드를 설정하게 되면, 의존성 주입을 통해 주입되는 빈은 실제 빈이 아닌 해당 빈을 상속받은 가짜 프록시 객체이다.

- 스프링은 CGLIB이라는 바이트 코드를 조작하는 라이브러리를 사용해서 프록시 객체를 주입해준다.

- 프록시 객체 내부에는 실제 빈을 요청하는 로직이 들어있어, 클라이언트의 요청이 오면 그때 실제 빈을 호출해준다.(실제 빈의 조회를 필요한 시점까지 지연 처리)

- 프록시 객체는 원래 빈을 상속받아서 만들어지기 때문에 클라이언트 입장에서는 실제 빈을 사용하는 것과 똑같은 방법으로 사용하면 된다.

- @Scope 애노테이션의 proxyMode 옵션을 사용하여 설정할 수 있다.

### 스프링 프록시의 장점

1. **싱글톤 빈과 요청/세션 스코프 빈의 호환성 문제 해결**

싱글톤 빈은 애플리케이션 전체에서 하나의 인스턴스를 공유하지만, 요청/세션 스코프 빈은 각각의 HTTP 요청 또는 세션마다 새로운 인스턴스를 생성합니다.
싱글톤 빈이 요청/세션 스코프 빈을 직접 참조하면, 요청/세션 스코프 빈의 생명주기를 올바르게 관리하기 어렵습니다. (요청/세션 완료 후 소멸하는 특성 때문에)
프록시를 사용하면, 싱글톤 빈이 요청/세션 스코프 빈을 호출할 때마다 새로운 인스턴스를 제공받을 수 있습니다.
프록시가 없다면 스프링 어플리케이션이 실행 되었을 때 요청/세션 스코프는 생성되지 않은 시점이라 반드시 에러가 발생하게 됩니다.
프록시 객체를 통해 객체가 있는척 하고 있다가 실제로 빈이 필요할 때 해당 빈의 프록시 객체를 통해 빈을 생성한 뒤 주입해주는 방식입니다.

2. **지연 로딩(Lazy Initialization)**

프록시를 사용하면 요청/세션 스코프 빈의 실제 인스턴스는 필요할 때까지 생성되지 않습니다.


3. **투명한 스코프 관리**

프록시가 빈의 생명주기를 자동으로 관리하므로, 개발자가 직접 스코프를 처리할 필요가 없습니다.

### 사용하기 좋은 경우

1. 싱글톤 빈이 요청/세션 스코프 빈을 참조할 때: 프록시 모드를 사용하면, 싱글톤 빈이 각 요청 또는 세션마다 새로운 인스턴스를 사용할 수 있습니다.

2. 웹 애플리케이션의 성능을 최적화할 때: 프록시를 사용하면 필요한 시점에만 빈을 생성하여 메모리 사용량을 줄일 수 있습니다.

3. 빈 생명주기를 자동으로 관리할 때: 프록시는 스코프 관리의 복잡성을 줄여주어 개발자가 빈의 생명주기를 직접 관리할 필요가 없습니다


### 사용 방법

프록시 모드는 @Scope 어노테이션의 proxyMode 속성을 설정하여 사용합니다.
주로 클래스 기반 프록시(ScopedProxyMode.TARGET_CLASS)와 인터페이스 기반 프록시(ScopedProxyMode.INTERFACES)가 사용됩니다.

<details>
  <summary>사용방법</summary>
  <div markdown="1">
  
  ```java
    import org.springframework.context.annotation.Scope;
    import org.springframework.context.annotation.ScopedProxyMode;
    import org.springframework.stereotype.Component;

    // 리퀘스트 스코프와 클래스 기반 프록시 사용 예제
    @Component
    @Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS)
    public class RequestBean {
        public String getMessage() {
            return "리퀘스트 빈입니다.";
        }
    }
  ```

  </div>
</details>

### 스프링 프록시의 단점

프록시 모드는 Spring에서 여러 가지 이점이 있지만, 모든 상황에서 항상 최선의 선택은 아니다. 프록시 모드를 사용하지 말아야 하는 경우는 다음과 같다.

1. **성능 오버헤드**:
   프록시는 추가적인 성능 오버헤드를 야기할 수 있다. 프록시 객체는 실제 객체에 대한 호출을 중간에서 가로채고, 추가적인 작업을 수행하기 때문에 순수한 객체 호출에 비해 성능이 떨어질 수 있다. 높은 성능이 요구되는 상황에서는 프록시 사용이 적합하지 않을 수 있다.

2. **복잡성 증가**:
   프록시는 코드의 복잡성을 증가시킬 수 있다. 특히 디버깅이 어려워질 수 있으며, 프록시로 인해 발생하는 문제를 추적하는 것이 복잡해질 수 있다. 코드의 가독성과 유지보수성을 고려할 때 프록시 사용이 적합하지 않을 수 있다.

3. **Self-invocation 문제**:
   프록시 객체는 자기 자신의 메서드를 호출할 때 프록시 기능이 적용되지 않는 문제가 있다. 이는 AOP(Aspect-Oriented Programming)를 사용할 때 특히 문제가 될 수 있다. 프록시가 생성한 객체에서 자기 자신의 메서드를 호출하면, 프록시를 거치지 않고 직접 호출이 이루어져 예상하지 못한 동작이 발생할 수 있다.

4. **타사 라이브러리와의 호환성**:
   일부 타사 라이브러리는 프록시 객체와 호환되지 않을 수 있다. 이러한 라이브러리는 프록시 객체 대신 실제 객체에 대한 참조를 기대할 수 있으며, 프록시 객체를 사용하면 예기치 않은 동작이나 에러가 발생할 수 있다.

5. **프록시 모드의 한계**:
   프록시는 클래스 기반 프록시와 인터페이스 기반 프록시 두 가지 방식이 있다. 인터페이스 기반 프록시는 인터페이스에 정의된 메서드에 대해서만 프록시를 제공하며, 클래스 기반 프록시는 final 메서드에 대해 프록시를 제공할 수 없다. 이러한 한계는 특정 상황에서 프록시 사용을 제약할 수 있다.

따라서, 프록시 모드를 사용할 때는 위와 같은 단점을 고려하여 상황에 맞게 신중하게 결정하는 것이 중요하다. 프록시 모드가 모든 상황에서 최적의 솔루션이 아니므로, 사용 목적과 환경에 맞게 선택하는 것이 바람직하다.

## 참조
[스코프 사용법이 잘 정리된 블로그](https://developer-hm.tistory.com/45)

