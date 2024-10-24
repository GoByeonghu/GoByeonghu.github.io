---
layout: post
title: C++의 가상함수
subtitle: 오버라이딩, 가상함수, 순수가상함수 비교
categories: 
    - C++
tags: [c++, virtual]
---

## 오버라이딩과 가상 함수

### 오버라이딩 (Overriding)

#### 정의 및 특징

자식 클래스가 부모 클래스의 함수를 새롭게 정의하는 것

#### override 명시

`override` 키워드는 필수는 아니지만, 사용하는 것이 좋다. `override` 키워드는 컴파일러에게 해당 메서드가 기본 클래스의 가상 메서드를 오버라이드한다는 것을 명시적으로 알려준다. 이를 통해 몇 가지 장점이 있다:

- **오타 방지**: 기본 클래스의 메서드 이름이나 시그니처를 잘못 입력했을 때, 컴파일러가 이를 감지하고 오류를 발생시킨다. `override` 키워드가 없다면, 새 메서드로 간주되어 의도하지 않은 동작을 할 수 있다.
- **코드 가독성**: 다른 개발자가 코드를 읽을 때 해당 메서드가 오버라이드된 메서드임을 명확히 알 수 있다. 이는 코드의 가독성과 유지보수성을 높인다.

### 가상 함수 (virtual)

#### 정의 및 특징

자식 클래스가 가상 함수를 오버라이딩 하면 부모 클래스 포인터로 자식 클래스를 참조할 때 자식 클래스의 함수를 사용할 수 있다.

다형성(polymorphism)을 구현하기 위해 사용된다. 비록 일반 함수도 파생 클래스에서 오버라이딩할 수 있지만, 가상 함수는 기본 클래스 포인터나 참조를 통해 파생 클래스의 메서드를 호출할 수 있도록 해준다. 이는 런타임 다형성(runtime polymorphism)을 가능하게 한다.

#### 사용법

`virtual void set_order_list(const vector<string>& order_list);`는 가상 함수로, 기본 클래스에서 구현될 수 있으며 파생 클래스에서 재정의될 수 있다.

#### 예시 코드

```cpp
class Base {
public:
    virtual void show() {
        cout << "Base class" << endl;
    }
};

class Derived : public Base {
public:
    void show() override {
        cout << "Derived class" << endl;
    }
};

int main() {
    Base* b;
    Derived d;
    b = &d;

    b->show(); // "Derived class" 출력
    return 0;
}

```

### 순수 가상 함수

#### 정의 및 특징

파생 클래스에서 반드시 재정의되어야 한다.


#### 사용법

`virtual void set_order_list(const vector<string>& order_list) = 0;`