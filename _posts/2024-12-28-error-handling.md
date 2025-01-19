---
layout: post
title: Error Handling
subtitle: C++와 Java가 에러 핸들링을 하는 방법
categories: 
  - Languages
tags: [c++, java]
---

## Java 예외처리

### 1. 예외처리 사용방법

Java에서 예외처리는 `try`, `catch`, `finally` 블록을 사용하여 이루어진다. 예외가 발생할 가능성이 있는 코드는 `try` 블록 안에 작성되고, 예외가 발생하면 해당 예외를 처리하는 코드는 `catch` 블록에 작성된다. `finally` 블록은 예외가 발생하든 발생하지 않든 실행되는 블록으로, 자원 정리와 같은 후처리를 할 때 사용된다.

다음은 기본적인 예외처리의 예시이다.

1. `try` 블록 안에서 코드가 실행되고, 예외가 발생하면 `catch` 블록에서 예외를 처리한다.
2. 예외 처리 후 `finally` 블록에서 자원 해제와 같은 작업을 할 수 있다.

예시 코드:

```java
try {
    int result = 10 / 0;  // 예외가 발생할 가능성이 있는 코드
} catch (ArithmeticException e) {
    System.out.println("0으로 나눌 수 없습니다.");
} finally {
    System.out.println("이 블록은 예외 발생 여부와 상관없이 실행됩니다.");
}
```

### 2. 기본 예외 클래스

Java에서는 여러 가지 기본 예외 클래스가 제공된다. 가장 기본적인 예외 클래스는 `Throwable`이며, 이는 `Error`와 `Exception` 두 가지 하위 클래스로 나뉜다.

- **`Throwable` 클래스 계층 구조**

```java
java.lang.Object
  ↳ java.lang.Throwable
      ↳ java.lang.Exception
      ↳ java.lang.Error
```

1. `Error`: 프로그램의 실행을 중단시키는 심각한 오류를 나타낸다. 예를 들어, `OutOfMemoryError`, `StackOverflowError`와 같은 오류가 여기에 속한다. 일반적으로 애플리케이션(프로그램 코드)에서 처리할 수 없다.
2. `Exception`: 프로그램에서 처리 가능한 예외를 나타낸다. `Exception`은 `RuntimeException`(Unchecked Exception)과 `Checked Exception`으로 나뉜다.

- `RuntimeException`(Unchecked Exception): 런타임 중에 발생할 수 있는 예외를 나타낸다. 예를 들어, `NullPointerException`, `ArrayIndexOutOfBoundsException` 등이 있다. 이러한 예외는 프로그램이 실행 중에 발생할 수 있으며, 명시적으로 처리하지 않아도 된다.
- `Checked Exception`: 컴파일 타임에 예외를 처리해야 함을 명시적으로 요구하는 예외이다. 예를 들어, `IOException`, `SQLException` 등이 있다. 이러한 예외는 반드시 `try-catch`로 처리하거나 메서드 선언에서 `throws` 키워드를 사용해 호출자에게 전달해야 한다.

### 3. Throwable 클래스

`try-catch` 문을 사용할 때 예외의 상세한 내용을 확인하기 어려울 수 있다. 이때 모든 예외의 최상위 클래스인 `Throwable`을 활용하면 예외 처리에 대한 상세 내용을 알 수 있다.


```java
public class Main {
    public static void main(String[] args){
        Main m = new Main();
        m.errorArray();
    }
    public void errorArray(){
        int[] intArray = new int[5];
        try{
            System.out.println(intArray[5]); 
            System.out.println("code running");
        } catch (Throwable t){
            System.out.println(t.getMessage());
            System.out.println(t.toString());
            t.printStackTrace();
        }
        System.out.println("code end"); 
    }
}
```

| 메소드 명          | 출력 결과                                                                                         |
|--------------------|---------------------------------------------------------------------------------------------------|
| `getMessage()`     | Index 5 out of bounds for length 5                                                                |
| `toString()`       | java.lang.ArrayIndexOutOfBoundsException: Index 5 out of bounds for length 5                      |
| `printStackTrace()`| java.lang.ArrayIndexOutOfBoundsException: Index 5 out of bounds for length 5<br>at Main.errorArray(Main.java:11)<br>at Main.main(Main.java:6) |


### 4. 커스텀 예외 클래스 만드는 법

Java에서는 자신만의 예외 클래스를 정의할 수 있다. 커스텀 예외를 만들 때는 `Exception` 또는 `RuntimeException` 클래스를 상속받아 정의할 수 있다. `Checked Exception`을 만들고 싶다면 `Exception`을 상속받고, `RuntimeException`을 상속받으면 `Unchecked Exception`을 만들 수 있다.

예시 코드:
```java
class InvalidAgeException extends Exception {
    public InvalidAgeException(String message) {
        super(message);
    }
}
```
커스텀 예외를 던지는 방법은 `throw` 키워드를 사용한다.

```java
public class CustomExceptionExample {
    public static void validateAge(int age) throws InvalidAgeException {
        if (age < 18) {
            throw new InvalidAgeException("나이가 18세 미만입니다.");
        }
    }

    public static void main(String[] args) {
        try {
            validateAge(16);
        } catch (InvalidAgeException e) {
            System.out.println(e.getMessage());
        }
    }
}
```

>이때 서비스에서 사용되는 에러 클래스를 상속을 통해 구조화 해두는 것도 필요함을 유의하자

### 5. 예외처리 체이닝

예외처리 체이닝은 예외를 처리할 때 발생한 예외를 다른 예외로 감싸서 전달하는 기법이다. 이 기법은 예외가 발생한 원인과 그에 대한 정보를 보존하면서, 예외를 상위 레벨로 전파할 수 있게 해준다.

Java에서는 예외를 체이닝할 때 `Throwable` 클래스의 `initCause()` 메서드를 사용하거나, 예외를 생성할 때 생성자에 원래 예외를 전달할 수 있다.

예시 코드:

```java
class OuterException extends Exception {
    public OuterException(String message) {
        super(message);
    }

    public OuterException(String message, Throwable cause) {
        super(message, cause);
    }
}

class InnerException extends Exception {
    public InnerException(String message) {
        super(message);
    }
}

public class ExceptionChainingExample {
    public static void main(String[] args) {
        try {
            throw new InnerException("내부 예외 발생");
        } catch (InnerException e) {
            try {
                throw new OuterException("외부 예외 발생", e);
            } catch (OuterException ex) {
                ex.printStackTrace(); // 예외 체이닝 출력
            }
        }
    }
}
```

위 예시에서 `InnerException`이 발생한 후, 이를 `OuterException`으로 감싸서 던졌다. `OuterException`은 `InnerException`을 `cause`로 전달하여 예외 체이닝을 구현했다.

### 오류전파

**`throws`** 키워드를 사용한다.

>💡 **throw와 throws는 엄연히 다르다.**
>
> `throw`: 특정 예외를 발생시키는 데 사용된다.
> `throws`: 메서드 선언부에서 사용되며, 해당 메서드가 던질 수 있는 예외를 명시한다.


throws 키워드는 예외를 호출한 상위 코드로 전파할 수 있다. 이를 통해 호출자는 해당 예외를 처리해야 함을 명확히 알 수 있다.


```java
public void readFile(String fileName) throws IOException {
    BufferedReader reader = new BufferedReader(new FileReader(fileName));
    // 파일 읽기 작업
    reader.close();
}

public void readFileWrapper() {
    try {
        readFile("/some/file/path/filename.mp4");
    } catch (IOException e) {
        System.out.println("파일을 읽는 도중 오류가 발생했습니다: " + e.getMessage());
    }
}

```

위 코드에서 readFile 메서드는 IOException을 던질 수 있음을 throws로 명시하고 있다. 이를 호출한 readFileWrapper 메서드는 반드시 예외를 처리하거나, 더 상위로 전파해야 한다.


### try-with-resources

try-catch-finally와 다르게 try블록이 종료되면 자동으로 자원 해제를 수행한다.
기존 처럼 따로 finally 구문이나 모든 catch 구문에 개발자가 명시적으로 종료 처리를 할 필요가 없다.


---

## C++의 예외처리

C++에서는 예외처리를 통해 프로그램이 실행 중에 발생할 수 있는 오류를 효과적으로 처리할 수 있다. 예외처리는 `try`, `throw`, `catch` 키워드를 사용하여 구현된다. 예외를 발생시키는 방법과 예외를 처리하는 방법에 대해 상세히 알아보자.

### 예외처리의 기본 구조

C++에서 예외를 처리하는 기본 구조는 다음과 같다.

1. `try` 블록: 예외가 발생할 가능성이 있는 코드를 포함한다. 이 블록 내에서 예외가 발생하면, 예외는 자동으로 `catch` 블록으로 전달된다.
2. `throw` 키워드: 예외를 명시적으로 발생시킨다. 예외는 객체 형태로 전달되며, `throw` 뒤에 예외 객체를 전달한다.
3. `catch` 블록: 발생한 예외를 처리한다. `catch` 블록은 특정한 예외 타입을 처리할 수 있으며, 예외 객체에 대한 정보를 사용할 수 있다.

```cpp
#include <iostream>
#include <stdexcept>

int divide(int a, int b) {
    if (b == 0) {
        throw std::invalid_argument("0으로 나눌 수 없습니다.");
    }
    return a / b;
}

int main() {
    try {
        int result = divide(10, 0);
        std::cout << "결과: " << result << std::endl;
    }
    catch (const std::invalid_argument& e) {
        std::cout << "예외 발생: " << e.what() << std::endl;
    }
    return 0;
}
```

위 예시에서 `divide` 함수는 두 숫자를 나누지만, 만약 두 번째 인자가 0이라면 예외를 발생시킨다. `main` 함수에서는 이 예외를 `catch` 블록에서 처리한다. `throw`와 `catch`를 통해 예외를 발생시키고 처리하는 과정을 볼 수 있다.

### 예외 클래스

C++에서 예외는 `std::exception` 클래스를 기반으로 하는 다양한 예외 클래스들로 처리된다. 이 예외 클래스들은 `std::exception`을 상속받아, 각기 다른 오류 유형에 맞는 예외를 처리할 수 있게 된다. 주요 예외 클래스는 다음과 같다.

#### 1. `std::exception`

`std::exception`은 모든 예외 클래스의 기본 클래스이다. `what()` 함수를 통해 예외에 대한 설명을 문자열로 반환할 수 있다. `std::exception`은 다른 예외 클래스들의 기반이 된다.

#### 2. `std::runtime_error`

`std::runtime_error`는 실행 중에 발생할 수 있는 오류를 처리하는 예외 클래스이다. 예를 들어, 잘못된 입력값이나 파일 입출력 오류 등이 해당된다.

#### 3. `std::logic_error`

`std::logic_error`는 논리적인 오류를 나타내는 예외 클래스이다. 예를 들어, 범위 초과나 잘못된 인수 값 등이 해당된다.

#### 4. `std::invalid_argument`

`std::invalid_argument`는 잘못된 인수 값이 함수에 전달될 때 발생하는 예외이다. `std::logic_error`를 상속받는다.

#### 5. `std::out_of_range`

`std::out_of_range`는 범위를 벗어난 값에 접근할 때 발생하는 예외이다. 예를 들어, 배열의 인덱스가 범위를 초과하는 경우가 해당된다.

#### 6. `std::overflow_error`

`std::overflow_error`는 산술 연산에서 오버플로우가 발생할 때 사용된다. 예를 들어, 더 이상 저장할 수 없는 크기의 값이 연산 결과로 나오는 경우이다.

#### 7. `std::underflow_error`

`std::underflow_error`는 산술 연산에서 언더플로우가 발생할 때 사용된다. 예를 들어, 더 작은 값으로 결과가 나타나는 경우이다.

### 예외 객체

예외는 일반적으로 객체 형태로 전달된다. 예외 객체는 기본적으로 `std::exception` 또는 이를 상속받은 클래스의 인스턴스로 생성된다. 예외 객체는 예외가 발생한 원인에 대한 정보를 포함할 수 있다. 예를 들어, `std::invalid_argument` 예외 클래스의 객체는 잘못된 인수를 포함할 수 있다.

```cpp
#include <iostream>
#include <stdexcept>

void check_positive(int value) {
    if (value <= 0) {
        throw std::invalid_argument("입력값은 양수여야 합니다.");
    }
}

int main() {
    try {
        check_positive(-1);
    }
    catch (const std::invalid_argument& e) {
        std::cout << "예외 발생: " << e.what() << std::endl;
    }
    return 0;
}
```
위 코드에서 `check_positive` 함수는 양수를 입력받아야 하는데 음수가 전달되면 예외를 발생시킨다. 예외 객체 `std::invalid_argument`가 `throw`되고, 이를 `catch` 블록에서 처리하며 `what()` 함수를 통해 예외 메시지를 출력한다.

### stdexcept


`stdexcept`는 C++ 표준 라이브러리의 헤더 파일 중 하나로, 예외 처리와 관련된 여러 클래스들을 정의한 라이브러리이다. 이 라이브러리는 `std::exception`을 비롯한 여러 종류의 표준 예외 클래스를 제공한다. 예외를 처리할 때 유용한 다양한 클래스를 제공하여, 프로그램에서 발생할 수 있는 여러 오류를 처리할 수 있도록 돕는다.

주요 예외 클래스들은 `std::exception`을 상속하며, 프로그램에서 발생할 수 있는 다양한 오류 유형을 처리할 수 있도록 한다.

### Custom Exception

C++에서 커스텀 예외(Custom Exception)를 만들려면, std::exception 클래스를 상속하여 자신만의 예외 클래스를 정의할 수 있다. 이렇게 하면 예외 처리를 보다 세밀하게 제어할 수 있고, 프로그램의 특정 오류 상황에 대해 더 의미 있는 정보를 제공할 수 있다.

#### 과정

1. `std::exception`을 상속하는 새로운 클래스를 정의한다.
2. `what()` 메서드를 오버라이드하여 예외 메시지를 반환하도록 한다.

```cpp
#include <iostream>
#include <exception>

// 커스텀 예외 클래스 정의
class NegativeNumberException : public std::exception {
public:
    // 생성자에서 예외 메시지 설정
    NegativeNumberException(const std::string& message) : message(message) {}

    // what() 메서드 오버라이드하여 예외 메시지 반환
    const char* what() const noexcept override {
        return message.c_str();
    }

private:
    std::string message;  // 예외 메시지를 저장할 변수
};

// 함수 내에서 커스텀 예외 발생
void check_positive(int number) {
    if (number < 0) {
        throw NegativeNumberException("음수는 허용되지 않습니다.");
    }
}

int main() {
    try {
        check_positive(-5);  // 음수 입력으로 예외 발생
    }
    catch (const NegativeNumberException& e) {
        std::cout << "예외 발생: " << e.what() << std::endl;  // 예외 메시지 출력
    }

    return 0;
}

```

- `NegativeNumberException` 클래스는 `std::exception`을 상속받고, 생성자를 통해 예외 메시지를 설정한다.
- `what()` 메서드를 오버라이드하여 예외 메시지를 반환한다.
- `check_positive()` 함수에서 음수가 입력되면 `NegativeNumberException`을 던진다.
- `main()` 함수에서 예외를 처리하고, 예외 메시지를 출력한다.

### 예외 체이닝

C++에서 예외를 throw를 통해 발생시킨 후, 이를 체이닝하여 호출 스택의 하위 함수나 객체로 전달하는 방식은 기본적으로 제공되지 않는다. 그러나 예외 체이닝을 구현하려면, 예외 객체 안에 또 다른 예외 객체를 포함시켜서 하위 함수나 객체로 전달할 수는 있다. 이를 통해 예외가 발생한 원인을 추적하거나, 상위 레벨에서 하위 예외를 처리할 수 있게 할 수 있다.

1. **기본 예외 체이닝**: 예외를 발생시키고 이를 다른 예외로 감싸서 던진다. 이를 통해 예외가 상위 함수로 전달되면서 체이닝이 이루어지게 한다.

2. **`std::exception` 상속을 이용한 체이닝**: 예외 객체 안에 다른 예외 객체를 저장하고, 이를 상위 레벨에서 포착하여 처리한다.

```cpp
#include <iostream>
#include <exception>
#include <string>

// 기본 예외 클래스
class BaseException : public std::exception {
public:
    explicit BaseException(const std::string& message) : message(message) {}

    const char* what() const noexcept override {
        return message.c_str();
    }

private:
    std::string message;
};

// 두 번째 예외 클래스, BaseException을 체이닝
class DerivedException : public BaseException {
public:
    explicit DerivedException(const std::string& message, const BaseException& baseException)
        : BaseException(message), baseException(baseException) {}

    const char* what() const noexcept override {
        return (BaseException::what() + std::string(" -> ") + baseException.what()).c_str();
    }

private:
    BaseException baseException;  // 체이닝된 예외 객체
};

// 하위 함수에서 예외를 던지고 이를 상위 함수에서 처리
void levelOne() {
    try {
        // 첫 번째 예외를 던짐
        throw BaseException("Level One Error");
    }
    catch (const BaseException& e) {
        // 첫 번째 예외를 잡고, 두 번째 예외로 감싸서 던짐
        throw DerivedException("Level Two Error", e);
    }
}

int main() {
    try {
        levelOne();  // 예외가 발생하는 함수 호출
    }
    catch (const DerivedException& e) {
        std::cout << "Caught exception: " << e.what() << std::endl;
    }

    return 0;
}

```

- `BaseException`: 기본 예외 클래스이다. 메시지를 설정하고 what() 메서드를 오버라이드하여 예외 메시지를 반환한다.
- `DerivedException`: BaseException을 상속받고, 생성자에서 BaseException을 인자로 받아 체이닝한다. what() 메서드를 오버라이드하여 예외 메시지를 합친다.
- `levelOne`: BaseException을 던진 후, 이를 잡아 DerivedException으로 감싸서 다시 던진다. 이렇게 예외 객체가 체이닝된다.
- `main`: levelOne 함수에서 발생한 예외를 잡고, 이를 출력한다.