---
layout: post
title: Functions as Arguments in C++
subtitle: C++에서 함수를 함수의 인자로 전달하는 방법
categories: 
  - C++
tags: []
---

C++에서 `sort` 함수와 같은 알고리즘을 사용할 때, 비교 함수(comparison function)를 인자로 전달할 수 있다. 비교 함수는 두 개의 인자를 비교하여 그들의 순서를 결정하는 역할을 한다. 예를 들어, `std::sort` 함수에서는 정렬 기준을 정의하는 비교 함수가 필요하다. 이 비교 함수는 단순히 두 값을 비교하는 함수이지만, 왜 함수가 인자로 전달될 수 있는지에 대한 개념을 이해하기 위해서는 C++의 함수 포인터, 람다 함수, 그리고 함수 객체에 대한 이해가 필요하다.

## 함수 포인터

C++에서 함수는 특정한 타입을 가진 객체로 취급된다. 함수 포인터는 특정 타입의 함수 주소를 저장할 수 있는 변수로, 이를 통해 함수를 인자로 전달할 수 있다. 함수 포인터는 함수의 주소를 변수에 저장하고, 이를 다른 함수에 인자로 넘겨주어 해당 함수가 다른 함수에서 호출될 수 있게 만든다.

### 함수 포인터 예시

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

bool compare(int a, int b) {
    return a < b;
}

int main() {
    std::vector<int> vec = {5, 2, 8, 1, 4};
    
    // 함수 포인터를 이용한 sort 호출
    std::sort(vec.begin(), vec.end(), compare);
    
    for (int num : vec) {
        std::cout << num << " ";
    }
    
    return 0;
}
```

위 코드에서 `compare` 함수는 `std::sort` 함수에 비교 함수로 전달된다. `compare` 함수는 두 인자 `a`와 `b`를 비교하여 `a < b`일 때 `true`를 반환한다. 이와 같이 함수의 이름은 함수 포인터로 취급되며, 이를 인자로 넘길 수 있다.


## 람다 함수

C++11부터는 람다 함수가 도입되었고, 이를 통해 함수를 간결하게 정의하고, 즉석에서 비교 함수와 같은 일시적인 함수 객체를 생성할 수 있다. 람다 함수는 그 자체로 함수 포인터처럼 사용할 수 있다.

### 람다 함수 예시

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> vec = {5, 2, 8, 1, 4};
    
    // 람다 함수를 이용한 sort 호출
    std::sort(vec.begin(), vec.end(), [](int a, int b) {
        return a < b;
    });
    
    for (int num : vec) {
        std::cout << num << " ";
    }
    
    return 0;
}

```

위 코드에서 `std::sort`에 전달된 람다 함수는 `a`와 `b`를 비교하여 정렬 기준을 정의한다. 람다 함수는 코드 블록 내에서 바로 비교 함수의 역할을 하며, 코드가 더 간결하고 이해하기 쉬운 형태로 작성된다.

## 함수 객체

C++에서 함수 객체(function object)는 연산자 ()를 오버로드한 클래스로, 객체를 함수처럼 호출할 수 있는 특성을 가진다. 함수 객체는 상태를 가질 수 있기 때문에, 더 복잡한 비교 함수나 사용자 정의 비교 기준을 만들 때 유용하다.

### 함수 객체 예시

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

class Compare {
public:
    bool operator()(int a, int b) {
        return a < b;
    }
};

int main() {
    std::vector<int> vec = {5, 2, 8, 1, 4};
    
    // 함수 객체를 이용한 sort 호출
    std::sort(vec.begin(), vec.end(), Compare());
    
    for (int num : vec) {
        std::cout << num << " ";
    }
    
    return 0;
}

```

위 코드에서 `Compare` 클래스는 `operator()`를 오버로드하여 함수처럼 동작하는 객체를 만든다. `std::sort`는 이 함수 객체를 인자로 받아서 정렬을 수행한다. 함수 객체는 상태를 가질 수 있어, 동일한 함수 객체를 여러 번 사용하거나 더 복잡한 로직을 구현할 때 유용하다.

## 함수 인자로 함수 받는 원리

C++에서 함수는 일종의 객체로 취급되기 때문에, 함수 포인터나 함수 객체, 람다 함수 등은 모두 함수 인자로 전달될 수 있다. 함수 포인터는 함수의 주소를 저장하는 변수이고, 람다 함수는 즉석에서 정의되는 익명 함수 객체이며, 함수 객체는 상태를 가지는 클래스 인스턴스로, 이들 모두가 std::sort와 같은 함수의 인자로 전달되어 기능을 수행한다.

함수 포인터는 특정 타입의 함수 주소를 저장할 수 있는 변수이며, 이를 통해 다른 함수에 함수를 전달할 수 있다. 함수 포인터를 사용하면 호출할 함수의 종류를 실행 중에 동적으로 선택할 수 있어 매우 유용하다. 이번에는 함수 포인터로 함수를 인자로 받는 함수의 구조를 살펴보며, 그 사용 방법에 대해 설명한다.

## 함수 포인터를 받는 함수

먼저, 함수 포인터를 받는 함수가 어떻게 작동하는지 설명하겠다. 함수 포인터는 함수의 주소를 저장하는 변수이므로, 이를 함수의 매개변수로 전달하면 호출할 함수의 종류를 동적으로 지정할 수 있다.

### 함수 포인터를 인자로 받는 예시

```cpp
#include <iostream>

bool compare(int a, int b) {
    return a < b;
}

void sort(int arr[], int size, bool (*cmp)(int, int)) {
    // 비교 함수 포인터를 이용하여 정렬 작업을 한다.
    for (int i = 0; i < size - 1; ++i) {
        for (int j = i + 1; j < size; ++j) {
            if (cmp(arr[i], arr[j])) {
                std::swap(arr[i], arr[j]);
            }
        }
    }
}

int main() {
    int arr[] = {5, 2, 8, 1, 4};
    int size = sizeof(arr) / sizeof(arr[0]);

    // 함수 포인터를 인자로 전달하여 정렬을 수행한다.
    sort(arr, size, compare);

    for (int i = 0; i < size; ++i) {
        std::cout << arr[i] << " ";
    }

    return 0;
}
```

위 코드에서 `sort` 함수는 `cmp`라는 함수 포인터를 매개변수로 받는다. `cmp`는 두 개의 `int` 값을 받아서 비교하는 함수로, 이 함수 포인터를 사용해 정렬 기준을 정의한다. `main` 함수에서는 `compare` 함수를 `sort` 함수에 전달하여 배열을 정렬한다.


### 함수 포인터 매개변수
sort 함수에서 사용되는 `bool (*cmp)(int, int)` 부분은 함수 포인터를 정의하는 문법이다. 여기서 `(*cmp)`는 함수 포인터를 나타내며, `int, int`는 비교 함수가 받는 인자의 타입을 나타낸다. 즉, `cmp`는 두 개의 `int` 값을 비교하는 함수의 주소를 저장하는 변수이다.

함수 포인터는 그 자체로 변수이기 때문에, `sort` 함수는 이를 통해 정렬 기준을 동적으로 설정할 수 있다. `cmp`는 `compare` 함수나 다른 함수로 바꿀 수 있으며, 함수 포인터를 사용하면 유연한 정렬을 할 수 있다.
