---
layout: post
title: Modern C++
subtitle: Comparison of Modern C++ and Traditional C++
categories: 
  - C++
tags: []
---

### Moder C++란?

모던 C++는 C++11 표준부터 C++14, C++17, C++20, C++23과 같은 후속 표준에 도입된 고급 기능과 기법, 관용구들을 포함하는 C++ 언어의 최신 형태이다. 이 표준들은 언어의 효율성, 가독성, 안전성을 크게 개선하였다. 주요 기능은 다음과 같다:

- **스마트 포인터** (`std::unique_ptr`, `std::shared_ptr` 등)를 통해 동적 메모리를 더 안전하게 관리할 수 있다.
- **람다 표현식**을 통해 익명 함수를 코드에 직접 작성할 수 있다.
- **이동 의미론**으로 자원을 효율적으로 다룰 수 있다.
- **동시성 지원** (`std::thread`, `std::async` 등)을 통해 멀티스레딩이 더 간단해진다.
- **표준 라이브러리 개선**으로 `std::filesystem`, `std::optional`, `std::variant` 같은 다양한 기능이 추가되었다.
- **템플릿 메타프로그래밍**의 향상으로 콘셉트와 `constexpr` 같은 기능을 사용할 수 있다.

모던 C++는 코드의 안전성을 높이고 속도를 개선하며, 더 표현력 있는 코드 작성을 목표로 한다. 이러한 기능을 통해 낮은 수준의 수동 메모리 관리를 줄이고, 깨끗하고 모듈화된 디자인을 장려한다.

> Modern C++의 반대는 Classic C++, Legacy C++, Traditional C++이다. 주로 C++ 98, C++ 03을 표준으로 하는 경우를 의미한다. C++11 이전의 C++ 표준을 주로 사용하던 방식과 스타일을 뜻하며, C++98과 C++03 같은 초기 표준을 따른다. 전통적인 C++에서는 모던 C++에서 제공되는 스마트 포인터, 이동 의미론, 람다 표현식, 동시성 관련 기능 등이 포함되지 않으며, 메모리 관리나 자원 관리를 수동으로 처리해야 하는 경우가 많다.

### Modern C++의 주요 기능

모던 C++은 C++11 이후 여러 표준에 걸쳐 도입된 다양한 새로운 기능들을 포함하고 있으며, 이로 인해 이전에 비해 훨씬 더 강력하고 안전하게 C++ 프로그래밍을 수행할 수 있다. C++11, C++14, C++17, C++20, 그리고 C++23까지 계속해서 추가된 모던 C++의 핵심 기능들을 알아보자.

#### 1. 스마트 포인터

스마트 포인터는 자원의 자동 수명을 관리하기 위한 도구이다. 이전 C++에서는 `new`와 `delete`를 사용해 동적으로 메모리를 할당하고 해제해야 했으나, 스마트 포인터를 사용하면 메모리 누수 위험을 크게 줄일 수 있다. 대표적인 스마트 포인터로는 `std::unique_ptr`, `std::shared_ptr`, `std::weak_ptr` 등이 있다.

예시로 `std::unique_ptr`는 소유권이 단일한 포인터이다. 예를 들어:
```cpp
std::unique_ptr<int> ptr = std::make_unique<int>(10); // ptr이 10을 가리킴 
// ptr이 범위를 벗어나면 자동으로 메모리 해제
```

`std::shared_ptr`는 포인터의 소유권을 여러 곳에서 공유할 때 사용한다. 
```cpp
std::shared_ptr<int> ptr1 = std::make_shared<int>(10); 
std::shared_ptr<int> ptr2 = ptr1; // 참조 횟수 증가 
// 모든 참조가 사라지면 메모리가 자동으로 해제된다
```


#### 2. 이동 의미론과 R값 참조

모던 C++에서는 이동 의미론을 통해 불필요한 복사 비용을 줄일 수 있다. R값 참조(`&&`)를 활용하여 객체의 자원을 효율적으로 이동시킬 수 있다. 이를 통해 성능을 개선할 수 있으며, 특히 대규모 데이터 구조에서 유용하다.

```cpp
std::vector<int> vec1 = {1, 2, 3}; 
std::vector<int> vec2 = std::move(vec1); // vec1의 자원이 vec2로 이동
```

이와 같은 이동은 복사와 달리 원본 객체의 내용을 비워서 성능을 높인다.

#### 3. 람다 표현식

람다 표현식은 코드에서 익명 함수를 간편하게 작성할 수 있는 방법이다. 함수 객체나 간단한 함수 작성 시 코드의 가독성과 유지보수성을 높여준다.

```cpp
auto add = [](int a, int b) { return a + b; }; 
int result = add(3, 4); // result는 7
```
람다는 캡처를 통해 주변 변수를 사용할 수 있다.

```cpp
int factor = 2; 
auto multiply = [factor](int a) { return a * factor; }; 
int result = multiply(5); // result는 10
```

#### 4. `constexpr`과 컴파일 타임 계산

`constexpr` 키워드는 상수를 컴파일 시간에 계산할 수 있도록 해 준다. 이를 통해 실행 속도가 빨라질 수 있으며, 반복적인 계산을 줄일 수 있다.

```cpp
constexpr int square(int x) { return x * x; } int result = square(5); // 컴파일 시에 계산됨, result는 25\
```

#### 5. 동시성 지원 (Concurrency)

모던 C++에서는 동시성 프로그래밍을 위해 다양한 도구를 제공한다. `std::thread`, `std::mutex`, `std::async` 등이 대표적이다. `std::thread`를 사용해 새로운 스레드를 만들 수 있다.

```cpp
std::thread t( 
  { 
    // 스레드에서 실행할 코드 
    std::cout << "Hello from thread!" << std::endl; 
  }
); 

t.join(); // 메인 스레드가 t 스레드가 종료될 때까지 대기
```

#### 6. 범위 기반 `for` 루프

모던 C++에서는 범위 기반 `for` 루프를 제공하여 코드 가독성을 높였다. 주로 배열이나 `std::vector`와 같은 컨테이너에서 유용하게 사용할 수 있다.


```cpp
std::vector<int> numbers = {1, 2, 3, 4}; 
for (int num : numbers) { 
  std::cout << num << " "; 
} 
// 출력: 1 2 3 4
```

#### 7. `std::optional`과 `std::variant`

`std::optional`은 값이 존재할 수도, 존재하지 않을 수도 있는 상황을 표현하기 위한 도구이다. 함수의 리턴 타입으로 많이 사용되며, 값이 없을 경우 `std::nullopt`로 표현할 수 있다.


```cpp
std::optional<int> maybeValue; 
maybeValue = 42; 
if (maybeValue) { 
  std::cout << "Value is " << *maybeValue << std::endl; 
} 
else { 
  std::cout << "No value" << std::endl; 
}
```


`std::variant`는 여러 타입 중 하나를 가질 수 있는 객체로, 안전하게 다형성을 처리할 수 있게 도와준다.

```cpp
std::variant<int, std::string> data; 
data = 10; 
data = "hello"; // 현재 값을 안전하게 접근하려면 std::get을 사용하거나, std::visit으로 방문한다
```


#### 8. 파일 시스템 라이브러리 (`std::filesystem`)

모던 C++에는 파일 시스템을 다룰 수 있는 표준 라이브러리가 추가되었다. `std::filesystem`을 통해 파일 및 디렉터리 조작을 간단하게 수행할 수 있다.


```cpp
std::filesystem::path p = "/some/directory"; 
if (std::filesystem::exists(p)) { 
  std::cout << "Directory exists." << std::endl; 
}
```

#### 9. `auto` 타입 추론

`auto` 키워드는 컴파일러가 변수의 타입을 자동으로 추론하게 해준다. 이를 통해 코드 가독성이 높아지고 유지보수가 쉬워진다. 복잡한 타입을 명시적으로 선언하지 않고도 사용할 수 있어 편리하다.

예를 들어, 복잡한 반복자 타입 대신 `auto`를 사용하면 코드가 훨씬 간결해진다.

```cpp
std::vector<int> numbers = {1, 2, 3, 4}; 
for (auto it = numbers.begin(); it != numbers.end(); ++it) { 
  std::cout << *it << " "; 
} 
// 출력: 1 2 3 4
```

또한, 함수의 반환값을 알기 어려운 경우에도 유용하게 사용할 수 있다.

```cpp
auto result = someComplexFunction(); // 함수가 반환하는 타입을 추론하여 result에 할당
```

모던 C++의 이러한 기능들은 C++ 프로그래머가 더 안전하고, 효율적이며, 유지보수하기 쉬운 코드를 작성할 수 있도록 도와준다. 이를 통해 많은 프로젝트에서 코드 품질과 성능을 크게 향상시킬 수 있다.

## 참조

- [C++11 공식 문서](https://en.cppreference.com/w/cpp/11)
- [매우 잘 정리된 블로그](https://tango1202.github.io/categories/cpp/)