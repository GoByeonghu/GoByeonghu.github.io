---
layout: post
title: Java Optional
subtitle: Java에서 Optional을 이용하여 null을 안전하게 처리하자
categories: 
    - Java
tags: [java]
---

### Optional이란 무엇인가

Optional은 Java 8에서 도입된 클래스이다. 이는 null이 반환될 수 있는 메서드의 반환 타입으로 사용된다. Optional을 사용하면 값이 존재할 수도 있고, 존재하지 않을 수도 있는 상황을 명시적으로 처리할 수 있다. Optional은 다음과 같은 상황에서 유용하다:

- 명시적 Null 처리: Optional을 사용하면 값이 존재할 때와 존재하지 않을 때를 명시적으로 처리할 수 있다.
- NullPointerException 방지: Optional을 사용하면 코드에서 NullPointerException 발생 가능성을 줄일 수 있다.

### 왜 사용하는가

Optional을 사용하는 주요 이유는 다음과 같다:

- 명시적이고 안전한 null 처리: Optional은 값이 없는 경우를 명시적으로 표현하므로, null이 반환되는 상황에서 발생할 수 있는 문제를 예방한다.
- 읽기 쉬운 코드: Optional을 사용하면 값을 검사하고 처리하는 코드를 더 읽기 쉽고 유지보수하기 쉽게 만들 수 있다.
- 불변성: Optional은 불변 객체이다. 값이 한 번 설정되면 변경할 수 없다.

### .orElse(null)는 무엇인가

Optional 클래스의 .orElse() 메서드는 Optional 객체가 값을 포함하고 있을 때는 그 값을 반환하고, 그렇지 않으면 지정된 기본 값을 반환한다. .orElse(null)는 Optional 객체에 값이 없을 때 null을 반환하도록 한다.

예를 들어:

```java
Optional<String> optionalValue = Optional.ofNullable(getValue());

// optionalValue가 값이 있으면 해당 값을 반환하고, 없으면 null을 반환한다.
String value = optionalValue.orElse(null);
```

이 코드는 getValue() 메서드가 null을 반환할 수 있는 경우를 안전하게 처리한다. optionalValue가 값을 가지고 있으면 그 값을 반환하고, 그렇지 않으면 null을 반환한다.

### 예제코드

```java
import java.util.Optional;

public class OptionalExample {

    public static void main(String[] args) {
        OptionalExample example = new OptionalExample();
        
        // 값이 존재하는 경우
        String presentValue = example.getValue(true);
        System.out.println("Present value: " + presentValue); // 출력: Present value: Hello, World!
        
        // 값이 존재하지 않는 경우
        String absentValue = example.getValue(false);
        System.out.println("Absent value: " + absentValue); // 출력: Absent value: null
    }
    
    public String getValue(boolean returnValue) {
        Optional<String> optionalValue = Optional.ofNullable(returnValue ? "Hello, World!" : null);
        return optionalValue.orElse(null);
    }
}

```

이 예제에서는 Optional을 사용하여 값이 존재할 때와 존재하지 않을 때를 처리하는 방법을 보여준다. orElse(null)은 Optional에 값이 없을 때 null을 반환하도록 한다.