---
layout: post
title: 람다 함수를 사용한 커스텀 비교 함수(C++)
subtitle: 람다 함수와 C++
categories: 
  - C++
tags: [c++]
---

## 람다 함수란

**람다 함수**는 코드에서 직접 정의할 수 있는 익명 함수이다.
C++의 경우 C++11부터 람다 함수를 사용가능하다.

## std::sort()

###  sort( RandomIt first, RandomIt last );

std::sort는 <algorithm> 헤더에 정의된 템플릿 함수입니다. 이를 통해 다양한 타입의 컨테이너와 요소를 정렬할 수 있습니다. 기본적으로 정렬할 데이터의 타입을 따로 지정하지 않고도 동작하도록 템플릿으로 구현되어 있습니다.

```cpp
template< class RandomIt >
void sort( RandomIt first, RandomIt last );
```

위 기본 형태에서 두 인자 first, last는 정렬할 범위를 나타내는 반복자이다. 
std::vector처럼 랜덤 접근이 가능한 컨테이너라면 사용할 수 있다.

디폴트로 오름차순 정렬을 수행한다. 이를 위해 < (less than) 연산자를 사용하여 두 요소를 비교한다.

## sort( RandomIt first, RandomIt last, Compare comp );

```cpp
template< class RandomIt, class Compare >
void sort( RandomIt first, RandomIt last, Compare comp );
```

세 번째 인자로 비교 함수(혹은 함수 객체)를 전달할 수 있다. 이 비교 함수는 두 요소를 받아서 bool 값을 반환하는 함수여야한다. 즉, 이 함수가 참을 반환하면 첫 번째 요소가 두 번째 요소보다 앞에 와야 한다고 판단한다.

## 예제

STL의 sort 함수를 lamda함수를 이용하여 커스텀해보자

```cpp
int getFirstDigit(int num) {
    // 음수 처리, 절대값으로 변환
    num = abs(num);
    // 첫 번째 자리수를 추출
    while (num >= 10) {
        num /= 10;
    }
    return num;
}

int main() {
    std::vector<int> numbers = {123, 4, 56, 789, 12, 987, 654};
    
    std::sort(numbers.begin(), numbers.end(), [](int a, int b) {
        // 숫자의 첫 번째 자리수를 비교하여 정렬
        return getFirstDigit(a) < getFirstDigit(b);
    });
    
    // 정렬 결과 출력
    for (int num : numbers) {
        std::cout << num << " ";
    }
    
    return 0;
}
```