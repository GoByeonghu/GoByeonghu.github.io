---
layout: post
title: C++ Smart Pointer
subtitle: About Smart Pointer
categories: 
    - C++
tags: [c++]
---

## C++의 메모리 할당
---

C++에서의 메모리 할당은 크게 두 가지 방식으로 이루어진다. 스택 메모리와 힙 메모리이다. 스택 메모리는 자동으로 관리되며 함수가 종료되면 메모리가 해제되지만, 힙 메모리는 프로그래머가 명시적으로 메모리를 할당하고 해제해야 한다. 아래에서는 C++에서 메모리 할당을 수행하는 방법에 대해 예시와 함께 자세히 설명하겠다.

### 1. 스택 메모리 할당
스택 메모리는 함수가 호출될 때 자동으로 할당되고, 함수가 종료되면 자동으로 해제된다. 스택 메모리는 속도가 빠르지만, 할당할 수 있는 메모리의 크기가 제한적이다.

#### 예시

```c++
#include <iostream>

void stackMemoryExample() { int a = 10; // 스택에 정수 변수 a를 할당 
std::cout << "Value of a: " << a << std::endl; // a의 값 출력 // a는 stack에서 자동으로 메모리가 관리되므로 해제할 필요가 없음 
}

int main() { stackMemoryExample(); // 함수 호출 
return 0; }
```


### 2. 힙 메모리 할당
힙 메모리는 프로그래머가 명시적으로 할당하고 해제해야 하는 메모리이다. 힙 메모리는 `new` 연산자를 사용하여 할당하고, `delete` 연산자를 사용하여 해제한다. 힙 메모리는 스택 메모리에 비해 더 많은 메모리를 할당할 수 있지만, 할당 해제를 잊어버릴 경우 메모리 릭이 발생할 수 있다.

#### 예시

```c++
#include <iostream>

void heapMemoryExample() { 
    int* ptr = new int; // 힙에 정수 변수 할당 
    *ptr = 20; // 포인터를 통해 값 저장 
    std::cout << "Value pointed by ptr: " << *ptr << std::endl; // 포인터가 가리키는 값 출력
    delete ptr; // 할당한 메모리 해제
}

int main() { heapMemoryExample(); // 함수 호출 
    return 0; 
}
```

### 3. 배열의 힙 메모리 할당
C++에서 배열을 힙에 할당할 수도 있다. 배열을 힙에 할당할 때는 `new[]` 연산자를 사용하고, 해제할 때는 `delete[]` 연산자를 사용한다.

#### 예시
```c++
#include <iostream>

void heapArrayExample() { int* arr = new int[5]; // 힙에 정수 배열 할당 
for (int i = 0; i < 5; ++i) { arr[i] = i * 10; // 배열에 값 저장 }
std::cout << "Array values: ";
for (int i = 0; i < 5; ++i) {
    std::cout << arr[i] << " "; // 배열 값 출력
}
std::cout << std::endl;

delete[] arr; // 할당한 배열 메모리 해제
}

int main() { 
    heapArrayExample(); // 함수 호출 
    return 0; 
}

```


### 4. 스마트 포인터

C++11부터 도입된 스마트 포인터는 메모리 관리를 보다 안전하게 해준다. `std::unique_ptr`, `std::shared_ptr`와 같은 스마트 포인터는 자동으로 메모리를 해제하므로 메모리 릭의 위험을 줄일 수 있다.

#### 예시

```cpp
#include <iostream> #include <memory>

void smartPointerExample() { std::unique_ptr<int> smartPtr(new int); // 스마트 포인터로 힙 메모리 할당 
*smartPtr = 30; // 값 저장 
std::cout << "Value pointed by smartPtr: " << *smartPtr << std::endl; // 값 출력 // smartPtr가 범위를 벗어나면 자동으로 메모리 해제됨 
}

int main() { 
    smartPointerExample(); // 함수 호출 
    return 0; 
}
```

## C++의 스마트 포인터
---

C++의 스마트 포인터는 메모리 관리와 자원 관리를 보다 안전하고 편리하게 하기 위해 도입된 클래스 템플릿이다. 스마트 포인터는 메모리 해제를 자동으로 처리하며, 메모리 릭과 댕글링 포인터와 같은 문제를 줄여준다. C++11부터 제공되는 주요 스마트 포인터에는 `std::unique_ptr`, `std::shared_ptr`, 그리고 `std::weak_ptr`가 있다. 각각의 스마트 포인터의 동작 방식과 사용 예제를 자세히 살펴보겠다.

### 1. std::unique_ptr
`std::unique_ptr`는 소유권을 독점적으로 가지는 스마트 포인터이다. 하나의 `std::unique_ptr` 인스턴스만이 특정 자원을 소유할 수 있으며, 다른 포인터로 복사할 수 없다. 하지만 소유권을 이동(이전)할 수 있다. 메모리는 `std::unique_ptr`의 범위를 벗어날 때 자동으로 해제된다.

#### 사용 예시

```cpp
#include <iostream>
#include <memory>

void uniquePtrExample() {
    std::unique_ptr<int> uptr(new int); // 메모리 할당
    *uptr = 10; // 값 저장
    std::cout << "Value pointed by unique_ptr: " << *uptr << std::endl; // 값 출력
    // uptr의 범위를 벗어나면 자동으로 메모리 해제됨
}

int main() {
    uniquePtrExample(); // 함수 호출
    return 0;
}
```

### 2. std::shared_ptr
`std::shared_ptr`는 여러 포인터가 동일한 자원을 공유할 수 있는 스마트 포인터이다. 내부적으로 참조 카운트를 유지하여, 마지막으로 `std::shared_ptr` 인스턴스가 파괴될 때 메모리를 해제한다. 따라서 `std::shared_ptr`를 사용하면 여러 객체가 동일한 자원에 접근할 수 있다.

#### 사용 예시

```cpp
#include <iostream>
#include <memory>

void sharedPtrExample() {
    std::shared_ptr<int> sptr1(new int); // 메모리 할당
    *sptr1 = 20; // 값 저장
    {
        std::shared_ptr<int> sptr2 = sptr1; // sptr1을 sptr2로 공유
        std::cout << "Value pointed by shared_ptr: " << *sptr2 << std::endl; // 값 출력
        std::cout << "Reference count: " << sptr1.use_count() << std::endl; // 참조 카운트 출력
    } // sptr2의 범위가 끝나면 참조 카운트가 감소

    std::cout << "Reference count after sptr2 is out of scope: " << sptr1.use_count() << std::endl; // 참조 카운트 출력
}

int main() {
    sharedPtrExample(); // 함수 호출
    return 0;
}
```

### 3. std::weak_ptr

`std::weak_ptr`는 `std::shared_ptr`와 함께 사용되며, 자원의 소유권을 가지지 않고 참조만 한다. 참조 카운트를 증가시키지 않기 때문에, `std::shared_ptr`가 소멸된 후에도 여전히 자원에 접근할 수 있는 방법을 제공한다. `std::weak_ptr`는 자원의 존재 여부를 확인할 수 있는 방법도 제공한다.

#### 사용 예시

```cpp
#include <iostream>
#include <memory>

void weakPtrExample() {
    std::shared_ptr<int> sptr(new int); // 메모리 할당
    *sptr = 30; // 값 저장
    std::weak_ptr<int> wptr = sptr; // weak_ptr로 shared_ptr 참조

    if (auto sptr2 = wptr.lock()) { // 유효성 확인
        std::cout << "Value pointed by weak_ptr: " << *sptr2 << std::endl; // 값 출력
    } else {
        std::cout << "Resource has been released." << std::endl;
    }

    sptr.reset(); // shared_ptr를 리셋
    if (auto sptr2 = wptr.lock()) { // 유효성 확인
        std::cout << "Value pointed by weak_ptr: " << *sptr2 << std::endl; // 이 부분은 실행되지 않음
    } else {
        std::cout << "Resource has been released." << std::endl; // 자원이 해제되었음을 출력
    }
}

int main() {
    weakPtrExample(); // 함수 호출
    return 0;
}
```

### 결론

스마트 포인터는 C++에서 메모리 관리를 간소화하고 안전성을 높여준다. `std::unique_ptr`는 독점적인 소유권을 가지며, `std::shared_ptr`는 자원을 공유할 수 있는 방법을 제공하고, `std::weak_ptr`는 자원의 유효성을 확인하는 데 유용하다. 스마트 포인터를 적절히 활용하면 메모리 릭과 댕글링 포인터의 위험을 줄일 수 있다.
