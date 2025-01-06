---
layout: post
title: Java Intro
subtitle: 자바의 동작원리에 대한 탐수
categories: 
    - Java
tags: [java]
---

---

## 기초

### 역사

제임스고슬링 개발


### 특징

- 엄격한 객체지향
소프트웨어의 모듈성, 유지보수성, 그리고 재사용성을 증가시켜, 대규모 소프트웨어 개발에 매우 적합하게 만듭니다.

- JVM
자바는 "한 번 작성하면 어디서나 실행 가능(Write Once, Run Anywhere, WORA)"이라는 철학을 가지고 있다.
자바로 작성된 프로그램이 어떠한 하드웨어 또는 플랫폼에서도 동일한 결과물을 실행할 수 있도록 보장합니다. 
이러한 특징은 이후 동작 방식에서 배울 자바 가상 머신(JVM)의 역할 덕분에 가능합니다.

### JDK와 JRE

![jdk]({{site.url}}/PostImages/2024-05-24-java-intro/jdk.png)

### JDK (Java Development Kit)

JDK는 자바 개발을 위해 필요한 모든 것을 가지고 있는 소프트웨어 환경입니다.

자바 애플리케이션을 **개발**하고 실행하기 위해 필요한 모든 도구를 가지고 있습니다.

### JRE (Java Runtime Environment)

JRE는 자바 실행을 위한 환경입니다. JDK 내부에 포함되어 있으며, JRE만 독립적으로도 설치할 수 있습니다.

여러분이 개발을 진행할 때는 JDK를 설치하면 JRE가 포함되어 자바를 실행할 수 있으며, 배포 환경일 때에는 JRE만 설치해서 개발 도구를 제외한 실행에 필요한 환경을 구성할 수 있습니다.

### 컴파일

![jdk]({{site.url}}/PostImages/2024-05-24-java-intro/compile.png)

### 컴파일(Compile)

컴파일은 JDK에 포함된 Compiler(javac)를 통해 Java Source(.java)를 Byte Code(.class)로 변환하는 과정을 의미합니다.

Javac은 컴파일 당시 매우 엄격한 정적 코드 점검을 수행합니다. 점검 시 오류를 발생하면 컴파일을 중지하며 개발자에게 경고를 보여줍니다.

### 런타임

런타임은 컴파일 과정을 거쳐 생성된 Byte Code(.class) 파일을 기계어 코드(OS별 다름)로 변경 후 JVM을 통해 실행하는 과정을 의미합니다.

위 예시 그림에서 Class Loader, Runtime Data Area, Execution Engine은 모두 JVM에 포함된 환경입니다.

### JVM

## JVM (Java Virtual Machine)

JVM의 아키텍처는 3가지 핵심 구성을 두고 있습니다. 지금은 이것만 기억해도 충분합니다.

- Class Loader : 바이트 코드를 가지고 옴
- Runtime Data Area : 메모리에 올리고
- Excution Engine : 실행

### ClassLoader

클래스 로더는 실행을 위한 Byte Code를 가져오는 역할을 하고 있습니다. Javac을 통해 컴파일된 바이트 코드를 Runtime Data Area로 적재합니다.

이는 동적 로딩(Dynamic Loading)으로 필요한 바이트 코드만 실시간으로 Runtime Data Area에 적재하는데 3가지 과정으로 구분됩니다.

**로딩**

바이트 코드 형태인 `.class` 파일을 찾아 메모리에 로드합니다.

**링크**

로딩된 `.class` 파일은 JVM에서 실행할 수 있도록 검증(안전한지), 준비(변수와 기본값을 위한 메모리), 해석(실제 메모리 주소로 변환) 과정을 거쳐 실행될 준비를 마칩니다.

**초기화**

클래스 및 정적 코드 블록(바로 실행해도 문제없는)을 실행합니다.


### Runtime data Area

![jdk]({{site.url}}/PostImages/2024-05-24-java-intro/rda.png)

Runtime Data Area는 JVM이 프로그램을 실행하기 위해 사용하는 메모리 영역입니다. 해당 영역은 프로그램 실행 중 생성되는 다양한 데이터를 저장하고 관리하는데 사용됩니다.

**PC Register**

각 스레드가 어떤 부분(코드)을 어떤 명령(CPU)으로 실행해야 할지 관리하고 기록합니다.

**JVM Stack (JVM 스택 영역)**

JVM 스택은 스레드마다 고유하게 존재하는 영역입니다. 각종 메서드, 지역 변수, 임시 데이터를 관리합니다.

**Native Method Area**

해당 영역은 JVM이 아닌 네이티브 코드를 실행하는 영역입니다. 자바가 아닌 다른 언어로 된 메소드(C, C++ 등)을 실행합니다.

**Heap (힙 영역)**

힙은 JVM 메모리가 관리하는 부분 중 가장 큰 부분입니다. 모든 스레드가 공유하며 자바 애플리케이션이 생성한 모든 객체와 배열이 저장됩니다.

힙 영역에서 사용되지 않는 객체는 GC(가비지 컬렉터)에 의해 제거되어 메모리 공간을 확보합니다.

**Method Area (메소드 영역)**

이 영역은 모든 스레드가 공유하는 영역입니다. 클래스 정보나 인터페이스 정보 모든 바이트 코드가 로드됩니다.

### **Execution** Engine

실행 엔진은 바이트 코드를 실행하는 역할을 담당합니다.

엔진은 자바 클래스가 JVM에 로드된 이후에 바이트 코드를 실제 기계어로 OS에 맞게 변환하고 Runtime Data Area에서 필요한 데이터를 가져와 실행합니다.

**인터프리터(Interpreter)**

인터프리터는 바이트코드를 한 줄씩 읽어서 기계어로 변환하고 실행합니다.

이 방법은 간단하고 구현하기 쉬우나, 같은 코드를 반복해서 실행할 때마다 매번 다시 변환해야 하므로 실행 속도가 느립니다.

인터프리터의 이러한 단점은 JIT 컴파일러의 도입으로 많은 최적화가 이루어졌습니다.

**Just-In-Time (JIT) 컴파일러**

이 컴파일러는 실행 중인 애플리케이션의 반복적으로 실행되는 부분을 감지하여, 그 부분만 기계어로 미리 컴파일합니다.

이렇게 미리 컴파일된 코드는 고속으로 실행할 수 있으며, 프로그램의 전체 실행 속도를 향상합니다.

**가비지 컬렉터(Garbage Collector)**

**가비지 컬렉터**는 힙 메모리 영역에서 더 이상 참조되지 않는 객체들을 자동으로 검출하고 제거합니다.

이 과정은 메모리를 효율적으로 관리하며, 메모리 누수와 같은 문제를 방지하는 데 중요한 역할을 합니다.






-----------------------------------------------------------------------------------------------------------------

## 기초 문법

기본 자료형 (Primitive Data Types, 원시 데이터 타입)
자바에서 사용되는 원시 데이터 타입(Primitive Data Types)은 여러 종류가 있으며, 각각의 특징과 사용 범위가 다릅니다.
데이터의 종류에 따라 적절하게 사용되어 메모리 사용을 최적화하고, 프로그램의 효율성을 높일 수 있습니다.
타입
바이트 크기
값의 범위
설명
byte
1
-128 to 127
매우 작은 정수 저장용. 
네트워크 데이터 처리에 유용
short
2
-32,768 to 32,767
작은 정수 저장용. 
byte 보다 큰 범위 제공
int
4
-2,147,483,648 to 2,147,483,647
일반적으로 사용되는 정수 저장용.
가장 자주 사용됨
long
8
-9,223,372,036,854,775,808 to 9,223,372,036,854,775,807
매우 큰 정수 저장용
float 
4
대략 ±3.40282347E+38F (6-7 significant decimal digits)
실수 저장용. 부동소수점 연산에 사용
double
8
대략 ±1.79769313486231570E+308 (15 significant decimal digits)
더 큰 실수 저장용.
정밀한 부동소수점 연산에 사용
char
2
'\u0000' to '\uffff' (0 to 65,535)
단일 문자 저장용. 유니코드 문자 표현 가능
boolean
1 bit
true, false
논리값 저장용. 조건문과 제어문에 주로 사용



# 자바의 변수, 메서드 구조


# 정의

---

| 메서드 구조 | 변수: 값을 저장하는 주소값을 가진 공간
메서드 : 클래스 내부에 정의된 특정 작업을 수행하는 코드 묶음 |
| --- | --- |

# 알아야하는 이유

---

| 메서드 구조 | 각 언어별로 변수, 메서드를 사용하는 방법이 조금씩 차이가 있어 혼동이 없이 인지 후 작성 |
| --- | --- |

프로그래밍 언어는 큰 틀에서는 모두 비슷한 원리를 가지고 있습니다. 하지만 실제 사용법에 있어서는 조금씩 차이가 있습니다.

자바를 배울 경우 다른 언어들보다 엄격한 경우가 많아 코드 작성이 어려워 보이지만, 우리가 알고 있는 개념과 크게 다르지 않습니다.

그러면 자바에서 변수와 메서드를 사용하는 방법을 알아보도록 하겠습니다.

# 동작 방식

---

| 메서드 구조 | 변수는 타입과 함께 선언, 메서드는 접근제어자, 리턴 타입, 메소드명, 매개 변수 사용 |
| --- | --- |

## 변수

```java
/* 원시 타입 선언

타입  변수명    값  */
int number = 6;
String name = "Yaro";
double weight = 10.1;
```

자바의 경우 변수를 사용해 줄 때 타입을 선언과 함께 선언해 주며 선언한 타입 외에는 값으로 할당할 수 없습니다.

### 기본 자료형 (Primitive Data Types, 원시 데이터 타입)

자바에서 사용되는 원시 데이터 타입(Primitive Data Types)은 여러 종류가 있으며, 각각의 특징과 사용 범위가 다릅니다.

데이터의 종류에 따라 적절하게 사용되어 메모리 사용을 최적화하고, 프로그램의 효율성을 높일 수 있습니다.

| 타입 | 바이트 크기 | 값의 범위 | 설명 |
| --- | --- | --- | --- |
| `byte` | 1 | -128 to 127 | 매우 작은 정수 저장용. 
네트워크 데이터 처리에 유용 |
| `short` | 2 | -32,768 to 32,767 | 작은 정수 저장용. 
`byte` 보다 큰 범위 제공 |
| `int` | 4 | -2,147,483,648 to 2,147,483,647 | 일반적으로 사용되는 정수 저장용.
가장 자주 사용됨 |
| `long` | 8 | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 | 매우 큰 정수 저장용 |
| `float`  | 4 | 대략 ±3.40282347E+38F (6-7 significant decimal digits) | 실수 저장용. 부동소수점 연산에 사용 |
| `double` | 8 | 대략 ±1.79769313486231570E+308 (15 significant decimal digits) | 더 큰 실수 저장용.
정밀한 부동소수점 연산에 사용 |
| `char` | 2 | '\u0000' to '\uffff' (0 to 65,535) | 단일 문자 저장용. 유니코드 문자 표현 가능 |
| `boolean` | 1 bit | true, false | 논리값 저장용. 조건문과 제어문에 주로 사용 |

```java
public class PrimitiveExamples {
    public static void main(String[] args) {
        // 정수형
        byte myByte = 100;    // 1바이트 정수
        short myShort = 5000; // 2바이트 정수
        int myInt = 100000;   // 4바이트 정수
        long myLong = 15000000000L; // 8바이트 정수, 숫자 끝에 'L'을 붙임

        // 부동소수점형
        float myFloat = 5.75f; // 4바이트 부동소수점, 숫자 끝에 'f'를 붙임
        double myDouble = 19.99; // 8바이트 부동소수점

        // 문자형
        char myChar = 'A'; // 2바이트 유니코드 문자

        // 논리형
        boolean myBoolean = true; // true 또는 false 값을 가짐

        // 값 출력
        System.out.println("Byte: " + myByte);
        System.out.println("Short: " + myShort);
        System.out.println("Int: " + myInt);
        System.out.println("Long: " + myLong);
        System.out.println("Float: " + myFloat);
        System.out.println("Double: " + myDouble);
        System.out.println("Char: " + myChar);
        System.out.println("Boolean: " + myBoolean);
    }
}

```

### 참조 자료형

```java
public class SomeClass {
	String text;
	
	//      생성자      매개변수
	public SomeClass(String text){
		this.text = text;
	}
	
	public void greet(){
		System.out.println(this.text);
	}
}

/*
클래스명    변수명    생성자   클래스명   매개변수*/
SomeClass object1 = new SomeClass("Hello");

/*
객체/인스턴스 메서드호출 */
object1.greet();

```

참조 자료형이란 기본 자료형을 제외한 모든 것을 의미합니다. 클래스, 인터페이스, 배열과 같은 자료형들이 있습니다.

사용 방법은 `new` 예약어를 통해 사용할 수 있습니다. 이후 클래스 수업에서 깊게 배우겠지만,
new 예약어는 생성자(constructor)를 호출하는 것을 의미합니다.

또한 public이라는 키워드는 접근 제어자를 의미합니다. 이후 접근 제어자 수업에서 더욱 깊게 배워 보도록 하고, 지금은 public은 누구든지 쓸 수 있다. 라고, 생각해 주세요.

## 메서드

```java
/*
접근제어자 리턴타입 메소드명  매개변수 */    
public boolean greet(String name){
	// 실행 코드 블록
	return true;
}
```

메서드는 접근제어자, 리턴 타입, 메소드명, 매개 변수, 실행할 코드 블록으로 구분됩니다.

접근 제어자의 경우 public으로 누구든지 쓸 수 있다고 말씀드렸습니다.

리턴 타입은 해당 메서드가 종료되고 반환되는 데이터의 타입을 의미합니다.

해당 예시에서는 boolean 타입이 리턴 타입으로 설정되어 있어, `true or false` 값이 아니면 오류가 발생합니다.

## 자바 코드 예시

```java
public class Main{
	int a = 1;
	int b = 2;
	String c = "hello world";
	
	public static void main(String[] args){
		int sum = a + b;
		System.out.println(sum);
		
		greet(Main.c);
	}
	
	public static void greet(String text){
		System.out.println(text);
	}
}
```

## 접근 제어자

자바에서 사용되는 접근제어자(Access Modifiers)는 클래스, 메서드, 변수 등의 접근 범위를 제한하는 키워드입니다. 

| 접근제어자 | 클래스 내 | 패키지 내 | 하위 클래스 | 전역 |
| --- | --- | --- | --- | --- |
| `public` | O | O | O | O |
| `protected` | O | O | O | X |
| `default` | O | O | X | X |
| `private` | O | X | X | X |

### **public**

어떤 클래스 에서라도 접근할 수 있습니다. 가장 높은 접근 수준을 제공합니다.

### **protected**

같은 패키지 내의 클래스 또는 다른 패키지의 하위 클래스에서 접근할 수 있습니다.

### **default (접근제어자를 명시하지 않음)**

같은 패키지 내의 클래스에서만 접근할 수 있습니다. 패키지-프라이빗 접근 수준이라고도 합니다.

### **private**

해당 클래스 내에서만 접근할 수 있습니다. 가장 제한적인 접근 수준을 제공하여, 외부에서는 접근할 수 없습니다.


## 자바의 변수 종류

지역 변수
메서드 영역
메서드 내부에서 선언되고 메서드가 종료 시 소멸되는 변수 
매개 변수
메서드 영역
매서드 호출 시 ‘전달 되는 값’  메서드 종료 시 제거 됨
인스턴스 변수
클래스 영역
객체간 고유한 값을 저장하고 싶을 때 사용
클래스 변수
클래스 영역
static 예약어를 사용하는 변수 여러 객체에서 공유하고 싶을 때 사용. 

## Static

```java
public class Dog {
	// 생략 ..
	
	public static void bark(){
		System.out.println("StaticMethod");
	}
}

// 생략

Dog.bar(); // StaticMethod
```

Static은 객체를 생성하지 않아도 사용할 수 있게 해주는 자바의 예약어
 static을 하나의 객체로 생각해 전역적으로 사용할 수 있게 해준다




-----------------------------------------------------------------------------------------------------------------
### 컬렉션 (Java Collections Framework, JCF)

자바에서 데이터 구조를 다루기 위해 제공되는 클래스와 인터페이스의 집합


## **주요 인터페이스 및 클래스**

### **`Collection` 인터페이스**

- 모든 컬렉션의 기본 인터페이스
- 요소 추가, 제거, 탐색 등의 메서드를 제공

### **`Set` 인터페이스**

- 중복을 허용하지 않는 컬렉션
- 구현 클래스: **`HashSet`**, **`LinkedHashSet`**, **`TreeSet`** 등

### **`List` 인터페이스**

- 순서가 있는 컬렉션, 중복 요소를 허용
- 구현 클래스: **`ArrayList`**, **`LinkedList`**, **`Vector`**, **`Stack`** 등

### **`Queue` 인터페이스**

- FIFO(First In, First Out) 구조를 가지는 컬렉션
- 구현 클래스: **`LinkedList`**, **`PriorityQueue`** 등

### **`Map` 인터페이스**

- 키와 값의 쌍으로 이루어진 컬렉션
- 구현 클래스: **`HashMap`**, **`LinkedHashMap`**, **`TreeMap`**, **`Hashtable`** 등

JCF를 통해 만들어진 자료형

graph LR
    Collection("Collection\nJCF 관점 전체 컬렉션 구조")
    List("List\n순서가 있는 자료 접근")
    Set("Set\n중복없이 자료 접근")
    Map("Map\n키와 값으로 데이터 접근")

    LinkedList("LinkedList\n연결리스트로 구현된 리스트")
    Stack("Stack\n스택구조로 접근")
    Vector("Vector\n동기화 지원")
    ArrayList("ArrayList\n동적 배열로 접근")

    HashSet("HashSet\n동적 해시 테이블로 구현")
    SortedSet("SortedSet\n정렬된 자료 접근 가능한 인터페이스")
    TreeSet("TreeSet\n트리 구조로 정렬된 자료 접근")

    Hashtable("Hashtable\n동기화 지원하는 Map 자료구조")
    HashMap("HashMap\n동적 해시맵으로 데이터 접근")
    SortedMap("SortedMap\n정렬된 자료로 Map 자료 접근")
    TreeMap("TreeMap\n트리 구조로 정렬된 Map 자료 접근")

    Collection --> List
    Collection --> Set
    Collection --> Map

    List --> LinkedList
    List --> Stack
    List --> Vector
    List --> ArrayList

    Set --> HashSet
    Set --> SortedSet
    SortedSet --> TreeSet

    Map --> Hashtable
    Map --> HashMap
    Map --> SortedMap
    SortedMap --> TreeMap


### Collection의 상속 구조 관점

- 실제 코드 관점에서 JCF 내부적으로 어떤식으로 상속이 되어 있는지
graph LR
    Collection("Collection\nJCF 관점 전체 컬렉션 구조")
    List("List\n순서가 있는 자료 접근")
    Set("Set\n중복없이 자료 접근")


    LinkedList("LinkedList\n연결리스트로 구현된 리스트")
    Stack("Stack\n스택구조로 접근")
    Vector("Vector\n동기화 지원")
    ArrayList("ArrayList\n동적 배열로 접근")

    HashSet("HashSet\n동적 해시 테이블로 구현")
    SortedSet("SortedSet\n정렬된 자료 접근 가능한 인터페이스")
    TreeSet("TreeSet\n트리 구조로 정렬된 자료 접근")
    

    Collection --> List
    Collection --> Set
		
    List --> LinkedList
    List --> Stack
    List --> Vector
    List --> ArrayList

    Set --> HashSet
    Set --> SortedSet
    SortedSet --> TreeSet
    


graph LR
    Map("Map\n키와 값으로 데이터 접근")
    Hashtable("Hashtable\n동기화 지원하는 Map 자료구조")
    HashMap("HashMap\n동적 해시맵으로 데이터 접근")
    SortedMap("SortedMap\n정렬된 자료로 Map 자료 접근")
    TreeMap("TreeMap\n트리 구조로 정렬된 Map 자료 접근")

    Map --> Hashtable
    Map --> HashMap
    Map --> SortedMap
    SortedMap --> TreeMap


[](https://en.wikipedia.org/wiki/Java_collections_framework)




----------------------------------------------------
## Interface

### 사용방법

```
[visibility] interface InterfaceName [extends other interfaces] {
        constant declarations
        (default is abstract) method declarations
        static method declarations
}

//(ex)
public interface InterfaceName extends OtherInterfaces {
  int someIntValue;
  abstract void someMethod();
  static void someStaticMethod();
}

```


예시 코드
1. 기본 인터페이스
public interface Vehicle {
    int MAX_SPEED = 120; // 상수 선언

    void start(); // 추상 메서드 선언
    void stop();
}
​
2. 인터페이스 상속
public interface ElectricVehicle extends Vehicle {
    void chargeBattery(); // 추가 추상 메서드 선언
}
​
3. 정적 메서드 포함
public interface Gadget {
    int WARRANTY_PERIOD = 1; // 상수 선언

    void turnOn(); // 추상 메서드 선언
    void turnOff();

    static void resetSettings() { // 정적 메서드 선언
        System.out.println("설정을 초기화합니다.");
    }
}

----------------------------------------------------
### Trhread

Thread? Runnable?
hread 는 Runnable 의 구현체이다.

public class MainSimpleCase1Thread {
    public static void main(String[] args) {
        Thread thread = new Thread();
        thread.start();
    }
}

레이스컨디션 해결 방법 (동시성 문제와 레이스 컨디션 차이)

1.synchronized(=mutex)
### **메서드 동기화 스타일**
블록 동기화 스타일

=> 데드락 주의

2. volatile(volatile 키워드는 변수의 원자성을 보장하지 않음.)


- `synchronized` , `atomic` , `lock` , `mutex`
- 메모리 가시성 이슈는 `volatile`

3. 
semaphore

### **사용 방법**

자바에서 세마포어는 **`java.util.concurrent`** 패키지에 포함되어 있으며, **`Semaphore`** 클래스를 사용하여 구현할 수 있습니다. **`Semaphore`** 클래스는 두 가지 주요 메서드인 **`acquire()`**와 **`release()`**를 제공합니다.

1. **`acquire()`**: 세마포어의 허용 가능한 값이 0보다 큰 경우 값을 감소시키고 자원을 획득합니다. 만약 값이 0이면, 자원이 반환될 때까지 대기합니다.
2. **`release()`**: 세마포어의 값을 증가시키고, 대기 중인 스레드가 있다면 자원을 사용할 수 있도록 허용합니다.

-바이너리 세마포어
-카운팅 세마포어

----------------------------------------------------


Lambda

lambda 키워드는 익명 함수(anonymous function)를 정의하는 데 사용

 프로그래밍 언어에서 함수를 일급 객체(first-class citizen)로 취급하는 개념의 기반

 >1. **변수에 할당할 수 있다**
함수는 변수에 할당될 수 있습니다. 이를 통해 함수 자체를 변수처럼 사용할 수 있습니다.
2. **함수의 매개변수로 전달할 수 있다**
함수는 다른 함수의 매개변수로 전달될 수 있습니다. 이를 통해 함수의 동작을 매개변수로 전달하여 더 유연한 코드 구성을 할 수 있습니다.
3. **함수의 반환값으로 사용할 수 있다**
함수는 다른 함수의 반환값으로 사용할 수 있습니다. 이를 통해 함수를 생성하고 반환하는 함수, 즉 고차 함수(higher-order function)를 만들 수 있습니다.


- 함수형 패러다임을 공부하는 시간이 아니므로 간단하게만 알아보고 넘어가겠습니다.
    
    <aside>
    💡 여기선 Stream API의 맛만 살짝 보고 람다식을 이어서 공부하겠습니다.
    
    </aside>
    
    우리는 이미 자바스크립트를 사용하면서 Promise 를 사용했습니다.
    
    - fetch 함수, Promise, `.then()` 등을 활용한 메서드 체이닝
    
    이러한 문법이 자바의 **`Stream API`** 에도 똑같은 문법으로 존재합니다.
    
    이하 **`Stream API`** 를 스트림이라고 부르겠습니다.
    
    - 자바에서 함수를 일급 객체로 다룰 때에는 크게 Function API와 Stream API 두 가지가 존재하나 여기선 스트림만 다룹니다.
    
    ### **스트림의 주요 특징**
    
    1. **선언형 처리**: 스트림을 사용하면 반복문 대신 선언형으로 데이터를 처리할 수 있습니다.
        
        [선언형? 명령형?](https://www.notion.so/a2413fdbf08f40dabff5c4261323de83?pvs=21)
        
    2. **체이닝**: 여러 연산을 체인 형태로 연결할 수 있습니다.
        1. 이미지와 같이 함수를 계속해서 연결해 나갈 수 있다.
            
            ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/cf024025-486d-4514-84ae-3a7c5951c17c/95598104-0f21-44d2-9d9d-3eb759798dc8/Untitled.png)
            
    3. **지연 연산**: 중간 연산은 지연(lazy) 연산이며, 최종 연산이 호출될 때까지 실행되지 않습니다.
    4. **병렬 처리**: 쉽게 병렬 스트림을 생성하여 병렬 처리를 수행할 수 있습니다.
    
    [Stream API 를 사용한 유저 아이디 조회](https://www.notion.so/Stream-API-e82a772c2fc540ddb3e49e9b3edb4c50?pvs=21)



### Stream API

스트림 API는 람다식을 포함한 함수형 인터페이스를 이용하여 데이터 소스(컬렉션, 배열, 난수, 파일 등)를 처리하고, 데이터를 조작, 가공, 변환하여 원하는 결과를 얻을 수 있게 해주는 자바의 인터페이스입니다

## 사용 이유

스트림 API를 사용하는 이유는 스트림을 통해 선언형 방식으로 컬렉션 데이터를 처리할 수 있기 때문입니다. 
스트림을 사용하면 반복문과 조건문을 여러 줄로 작성하지 않고도 간결하고 직관적인 코드로 작성할 수 있습니다.
또한, 스트림 API는 다양한 데이터 소스에 대해 일관된 작업을 수행하고, 병렬 처리를 통해 성능을 최적화할 수 있는 유연성을 제공합니다.







```java
/* 기본 형식 */
(parameters) -> expression
(parameters) -> { statements; }
```

```java

/* 실제 코드 표현식 */

// 매개변수가 없고 반환값도 없는 람다 표현식
() -> System.out.println("Hello, World!");

// 하나의 매개변수를 받고, 그 매개변수를 출력하는 람다 표현식
(x) -> System.out.println(x);

// 두 개의 매개변수를 받아 그 합을 반환하는 람다 표현식
(a, b) -> a + b;

// 여러 줄의 코드가 있는 람다 표현식
(a, b) -> {
    int sum = a + b;
    return sum;
};
```


import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        // 리스트 생성
        List<String> names = Arrays.asList("김철수", "이영희", "박민수", "최지우", "한예슬");

        // 스트림 생성 및 중간 연산과 최종 연산 적용
        List<String> filteredNames = names.stream()
                                          .filter(name -> name.startsWith("김"))
                                          .toList();

        // 결과 출력
        filteredNames.forEach(System.out::println); // 출력: 김철수
    }
}
​
예제 설명
List<String> 타입의 names 리스트를 생성합니다.
names.stream()을 호출하여 스트림을 생성합니다.
filter 중간 연산을 사용하여 이름이 "김"으로 시작하는 항목만 필터링합니다.
collect 최종 연산을 사용하여 필터링된 결과를 리스트로 수집합니다.
forEach를 사용하여 결과 리스트의 각 요소를 출력합니다.
중간 연산과 최종연산이 무엇인가요?

중간 연산은 또 다른 스트림을 반환하며, 최종 연산이 호출될 때까지 실제로 수행되지 않습니다. 이를 지연 평가라고 합니다.

최종 연산은 스트림을 소모하여 결과를 만드는 연산입니다. 최종 연산이 호출되면 스트림의 요소들이 실제로 처리되며, 최종 연산 후에는 스트림을 더 이상 사용할 수 없습니다.

아래 예시를 통해 이해할 수 있을 것입니다.
import java.util.*;
import java.util.stream.*;

public class StreamExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David", "Anna");

        // 스트림을 생성하고 중간 연산과 최종 연산을 수행
        List<String> result = names.stream()
            .filter(name -> name.startsWith("A"))   // 중간 연산: 필터링
            .map(String::toUpperCase)               // 중간 연산: 대문자로 변환
            .sorted()                               // 중간 연산: 정렬
            .collect(Collectors.toList());          // 최종 연산: 리스트로 수집

        System.out.println(result); // 출력: [ALICE, ANNA]
    }
}
​
파이프라인 설명때 조금 더 깊은 설명을 해보도록 하겠습니다.





조금 더 깊게 이해하기 (추상화와 파이프라인)
Stream API는 데이터 소스를 추상화하여 일련의 연산(필터링, 매핑 등)을 처리할 수 있게 해줍니다. 
스트림은 주로 컬렉션에서 파이프라인(pipeline) 형태로 사용됩니다. 

추상화
"추상화"는 복잡한 시스템을 단순화하여 중요한 세부 사항만을 노출하는 개념을 의미합니다.
Java Stream API에서 "추상화"란 데이터를 처리하는 복잡한 로직을 숨기고, 간단하고 직관적인 방법으로 데이터를 다룰 수 있게 해주는 것을 말합니다.
이를 통해 개발자는 데이터 소스의 세부 사항을 신경 쓰지 않고 데이터 처리 작업에 집중할 수 있습니다.
예를 들어, 리스트에서 특정 조건에 맞는 항목을 필터링하고 변환하는 작업을 전통적인 방법과 스트림을 사용한 방법으로 비교해 보겠습니다.
전통적인 방법
먼저 전통적인 방법을 살펴보겠습니다. 
이 방법은 명령형 프로그래밍 스타일로, 데이터 처리의 각 단계를 명확하게 작성해야 합니다.
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class TraditionalExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("김철수", "이영희", "박민수", "최지우", "한예슬");
        List<String> result = new ArrayList<>();

        // 필터링 및 변환 작업
        for (String name : names) {
            if (name.startsWith("이")) {
                result.add(name.toUpperCase());
            }
        }

        // 결과 출력
        for (String name : result) {
            System.out.println(name);
        }
    }
}
​
스트림을 사용한 방법
스트림을 사용한 방법은 선언형 프로그래밍 스타일로, 데이터를 어떻게 처리할지 선언만 하면 됩니다. 
스트림 API는 데이터 처리의 복잡한 로직을 추상화하여 간단하고 직관적인 방법으로 제공해줍니다.
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("김철수", "이영희", "박민수", "최지우", "한예슬");

        // 스트림 파이프라인: 필터링 및 변환 작업
        List<String> result = names.stream()               // 스트림 생성
                                   .filter(name -> name.startsWith("이")) // 필터링
                                   .map(String::toUpperCase)  // 변환
                                   .collect(Collectors.toList()); // 결과 수집

        // 결과 출력
        result.forEach(System.out::println); // 출력: 이영희
    }
}
​
스트림을 이용해서 추상화를 달성했습니다.
인터페이스 역시 추상화 입니다. 

## 파이프라인

- 자바에서만 쓰이는 용어가 아닙니다. CS 전반에 걸쳐서 나오는 단어입니다!

파이프라인(pipeline)은 데이터를 처리하는 연속적인 단계의 흐름을 의미합니다.
Java Stream API에서는 여러 중간 연산(Intermediate Operation)과 최종 연산(Terminal Operation)을 연결하여 데이터를 처리할 수 있습니다.
이러한 연결된 연산의 흐름을 파이프라인이라고 합니다.

```java
// 데이터 소스: 이름 리스트 생성
List<String> names = Arrays.asList("김철수", "이영희", "박민수", "최지우", "한예슬");

// 스트림 파이프라인
List<String> result = names.stream()       // 스트림 생성 (소스)
  .filter(name -> name.startsWith("이"))   // 중간 연산: '이'로 시작하는 이름 필터링
  .map(String::toUpperCase)               // 중간 연산: 이름을 대문자로 변환
  .collect(Collectors.toList());         // 최종 연산: 결과를 리스트로 수집

// 결과 출력
result.forEach(System.out::println);      // 출력: 이영희
```

스트림 파이프라인은 주로 다음과 같은 세 단계로 구성됩니다:

1. *소스(Source)
2. *중간 연산(Intermediate Operation)
3. *최종 연산(Terminal Operation)

### ***소스(Source)**

스트림을 생성하는 데이터 소스 (예: 리스트, 배열).

### ***중간 연산 (Intermediate Operations)**

중간 연산은 스트림을 변환하거나 필터링하는 작업을 수행하며, 또 다른 스트림을 반환합니다.

이러한 연산은 지연 평가(Lazy Evaluation)되어 최종 연산이 호출될 때까지 실제로 수행되지 않습니다.

중간 연산은 여러 개 연결(chain)할 수 있으며, 파이프라인의 각 단계에서 데이터를 처리합니다.

[주요 중간연산](https://www.notion.so/e103ef1711b545c9b2ca2924fee38842?pvs=21)

### ***최종 연산 (Terminal Operations)**

최종 연산은 스트림 파이프라인의 마지막 단계에서 실행되며, 스트림을 소비하고 결과를 생성합니다. 최종 연산이 호출되면 스트림의 모든 요소가 처리되고, 더 이상 스트림을 사용할 수 없습니다.

[주요 최종연산](https://www.notion.so/24843a8915de4271a86178c6f49ae8f9?pvs=21)

### 소스 + 중간연산 + 최종연산 결합해서 사용해보기

중간 연산과 최종 연산을 조합하여 스트림 파이프라인을 구성하는 예제를 살펴보겠습니다.

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamPipelineExample {
    public static void main(String[] args) {
        // 데이터 소스: 이름 리스트 생성
        List<String> names = Arrays.asList("김철수", "James", "박민수", "최지우", "한예슬");

        // 스트림 파이프라인: 필터링 및 변환 작업
        List<String> result = names.stream()                   // 스트림 생성
                         .filter(name -> name.startsWith("J")) // 중간 연산: 'J'로 시작하는 이름 필터링
                         .map(String::toUpperCase)             // 중간 연산: 이름을 대문자로 변환
                         .sorted()                             // 중간 연산: 정렬
                         .collect(Collectors.toList());        // 최종 연산: 결과를 리스트로 수집

        // 결과 출력
        result.forEach(System.out::println); 
        
        // 출력: JAMES
    }
}

```

이 예제에서는 **`filter`**, **`map`**, **`sorted`** 중간 연산과 **`collect`** 최종 연산을 사용하여 데이터를 처리합니다. 각 중간 연산은 스트림을 변환하고, 최종 연산은 결과를 리스트로 수집합니다.








참고

# 선언형? 명령형?

프로그래밍에서는 선언형 프로그래밍(declarative programming)과 명령형 프로그래밍(imperative programming)이라는 두 가지 주요 패러다임이 있습니다.

이 두 패러다임은 프로그램을 작성하고 데이터를 처리하는 방식에서 근본적으로 다릅니다.

### **명령형 프로그래밍**

명령형 프로그래밍은 프로그램이 실행되는 방법을 명시적으로 설명하는 방식입니다.

여기에는 상태의 변화와 제어 흐름이 포함되며, 프로그램이 수행할 작업을 순차적으로 기술합니다.

전통적인 절차적 프로그래밍이 이에 해당하며, C, Java, Python과 같은 많은 프로그래밍 언어에서 명령형 스타일로 코드를 작성할 수 있습니다.

명령형 프로그래밍의 주요 특징:

- **순차적 실행**: 코드가 위에서 아래로 순차적으로 실행됩니다.
- **상태 변화**: 변수의 값을 변경하며 상태를 관리합니다.
- **제어 구조**: 반복문, 조건문 등을 사용하여 제어 흐름을 관리합니다.

### **예제: 명령형 프로그래밍**

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class ImperativeExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("김철수", "이영희", "박영수", "김영수", "이철수");
        List<String> result = new ArrayList<>();

        for (String name : names) {
            if (name.startsWith("김")) {
                result.add(name.toUpperCase());
            }
        }

        for (String name : result) {
            System.out.println(name);
        }
    }
}

```

위의 예제에서 우리는 **`for`** 루프를 사용하여 리스트의 각 요소를 순차적으로 처리하고, 조건에 맞는 요소를 새로운 리스트에 추가하고, 그 결과를 출력합니다.

명령형 프로그래밍 방식은 로직을 명확하게 드러내지만 사이드 이펙트에 취약해집니다.

### **선언형 프로그래밍**

선언형 프로그래밍은 프로그램이 무엇을 수행할지 설명하는 방식입니다.

여기서는 수행 방법보다는 수행할 작업을 정의하는 데 초점을 맞춥니다.

함수형 프로그래밍과 논리 프로그래밍이 대표적인 예이며, 그리고 자바의 스트림 API, SQL, HTML 같은 기술들이 선언형 프로그래밍의 예입니다.

[HTML, SQL 은 왜 선언형인가](https://www.notion.so/HTML-SQL-736017efcb90430ca83a53ae3765689f?pvs=21)

선언형 프로그래밍의 주요 특징:

- **고차 함수 사용**: 함수나 메서드를 사용하여 작업을 정의합니다.
- **불변성**: 상태를 변경하지 않고, 불변 데이터를 사용합니다.
- **코드 간결성**: 코드가 간결하고 읽기 쉽습니다.

### **예제: 선언형 프로그래밍**

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class DeclarativeExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("김철수", "이영희", "박영수", "김영수", "이철수");

        List<String> result = names.stream()
            .filter(name -> name.startsWith("김"))
            .map(String::toUpperCase)
            .collect(Collectors.toList());

        result.forEach(System.out::println);
    }
}
```

위의 예제에서 우리는 스트림을 사용하여 리스트를 필터링하고, 조건에 맞는 요소를 대문자로 변환한 후, 결과를 출력합니다. 스트림 API를 사용하면 명령형 스타일보다 더 간결하고 선언적으로 코드를 작성할 수 있습니다.

### **주요 차이점 요약**

- **명령형 프로그래밍**:
    - **어떻게** 해야 하는지 기술
    - 반복문, 조건문, 상태 변경 등을 사용
    - 코드가 절차적이며, 제어 흐름을 명확히 이해해야 함
- **선언형 프로그래밍**:
    - **무엇을** 해야 하는지 기술
    - 고차 함수와 메서드를 사용하여 작업 정의
    - 코드가 간결하며, 수행할 작업의 의도를 명확히 전달











----------------------------------------------------

# Annotation

<aside>
💡 현 시대의 자바에서 애너테이션은 Spring AOP 와 함께 쓸때 진가를 발휘합니다.

AOP 없이 커스텀 애너테이션을 사용한다면 복잡도가 올라 가고 유지보수가 더 어려워 질 수도 있습니다.

</aside>

애노테이션(Annotation)은 라틴어 "annotatio"에서 유래한 용어입니다.

라틴어로 "annotatio"는 "덧붙여 놓은 주석"이라는 뜻입니다.

영어의 "annotation"도 동일한 어원을 가지고 있으며, "주석을 다는 행위" 또는 "주석 자체"를 의미합니다.

Java에서는 이 개념을 차용하여, 코드에 대한 메타데이터를 제공하는 특별한 형태의 문법으로 애노테이션을 도입했습니다. 애노테이션은 “**@”** 기호와 함께 사용되며, 클래스, 메서드, 변수, 매개변수 등에 대한 부가 정보를 제공합니다.

예를 들어:

```java
@Override
public String toString() {
   // ...
}

```

여기서 `@Override`는 해당 메서드가 상위 클래스의 메서드를 오버라이드하고 있음을 나타내는 애노테이션입니다. 이렇게 애노테이션을 통해 코드에 대한 메타데이터를 작성하고, 이를 컴파일러나 런타임에서 활용할 수 있게 됩니다.

 자바에서는 다양한 내장 애노테이션이 제공되며, 사용자 정의 애노테이션도 만들 수 있습니다.

## **자바 내장 애노테이션**

자바는 몇 가지 기본 애노테이션을 내장하고 있습니다. 이들은 주로 코드의 동작이나 의미를 명확히 하기 위해 사용됩니다.

1. **@Override**
    - 메서드가 수퍼클래스의 메서드를 오버라이드하고 있음을 나타냅니다.
    
    ```java
    public class SuperClass {
        public void display() {
            System.out.println("SuperClass Method display()!");
        }
    }
    
    public class SubClass extends SuperClass {
        **@Override**
        public void display() {
            System.out.println("SubClass display()!");
        }
    }
    ```
    
2. **@Deprecated**
    - 특정 요소(클래스, 메서드 등)가 더 이상 사용되지 않음을 나타내며, 다른 대안이 있음을 알립니다.
    - 이 어노테이션은 오픈소스 혹은 사내 자체 라이브러리가 있으면 쓸 일이 있으나 그렇지 않은 경우 쓸 일이 없었다.
    
    ```java
    public class Example {
        **@Deprecated**
        public void oldMethod() {
            System.out.println("This method is deprecated");
        }
    
        public void newMethod() {
            System.out.println("This method is the replacement");
        }
    }
    ```
    

## 커스텀 애노테이션

### **사용 이유**

1. **메타데이터 제공**: 런타임이나 컴파일 타임에 필요한 정보를 제공하여 동적으로 동작을 제어할 수 있습니다.

- **MyCustomAnnotation.java**
    
    ```java
    import java.lang.annotation.Retention;
    import java.lang.annotation.RetentionPolicy;
    import java.lang.annotation.ElementType;
    import java.lang.annotation.Target;
    
    //  이 어노테이션은 런타임에도 유지됩니다.
    //  이는 JVM이 런타임에 어노테이션을 읽을 수 있음을 의미합니다.
    //  이를 통해 런타임에 어노테이션 정보를 처리하는 코드를 작성할 수 있습니다. 
    @Retention(RetentionPolicy.RUNTIME)
    
    // 이 어노테이션은 메소드에만 적용될 수 있습니다.
    // 이는 MeasureTime 어노테이션을 메소드 선언부에만 붙일 수 있음을 의미합니다.
    @Target(ElementType.METHOD)
    public @interface MyCustomAnnotation {
        String value() default "기본 값";
        int number() default 0;
    }
    ```
    
- **AnnotationProcessor.java**
    
    ```java
    import java.lang.reflect.Method;
    
    public class AnnotationProcessor {
    
        public void processAnnotations(Object obj) {
            try {
                Method[] methods = obj.getClass().getDeclaredMethods();
    
                for (Method method : methods) {
                    if (method.isAnnotationPresent(MyCustomAnnotation.class)) {
                        MyCustomAnnotation annotation = method.getAnnotation(MyCustomAnnotation.class);
    
                        System.out.println("메서드 이름: " + method.getName());
                        System.out.println("value: " + annotation.value());
                        System.out.println("number: " + annotation.number());
    
                        method.invoke(obj);
                    }
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
    ```
    
- **MyClass.java**
    
    ```java
    public class MyClass {
    
        @MyCustomAnnotation(value = "테스트", number = 10)
        public void myMethod() {
            System.out.println("myMethod가 실행되었습니다.");
        }
    }
    ```
    
- **Main.java**
    
    ```java
    public class Main {
        public static void main(String[] args) {
            MyClass myClassInstance = new MyClass();
            AnnotationProcessor processor = new AnnotationProcessor();
            processor.processAnnotations(myClassInstance);
        }
    }
    ```
    

### 아쉬운 점

`new MyClass().myMethod()` 를 호출했을 때 함수 실행 되기 전에 `@MyCustomAnnotation` 이 자동으로 호출되길 기대했을 수도 있습니다. (파이썬처럼)

하지만 자바는 기본적으로 이러한 동작 방식을 지원하지 않습니다.

특정 메서드 머리에 달려있는 애너테이션이 자동으로 실행되게 하려면 Spring 의 AOP 기능을 사용해야 하지만 여기서는 논외로 하겠습니다.

---

## 조금 더 깊게 이해하기

<aside>
💡 런타임에 클래스를 동적으로 가져올 수 있다. 스프링은 리플렉션으로 만들어졌다고 해도 과언이 아니다.

</aside>

리플렉션(Reflection)은 자바에서 실행 시간에 클래스, 메서드, 필드, 인터페이스 등을 동적으로 검사하고 조작할 수 있는 기능입니다.

이를 통해 코드가 실행되는 동안에도 클래스의 구조나 상태를 분석하고 수정할 수 있습니다.

### **용어 정의**

- **리플렉션(Reflection)**: 런타임 시점에 클래스나 객체의 메타데이터(클래스의 구조, 메서드, 필드 등)를 동적으로 접근하고 조작할 수 있는 기능입니다.

### **사용 이유**

리플렉션은 다음과 같은 이유로 사용됩니다:

| **사용 이유** | **설명** |
| --- | --- |
| **동적 동작** | 실행 시점에 클래스나 객체의 정보를 얻고, 이를 바탕으로 동적으로 동작을 결정할 수 있습니다. |
| **프레임워크 및 라이브러리 개발** | 다양한 타입의 객체를 동적으로 생성하고 처리해야 하는 프레임워크나 라이브러리에서 자주 사용됩니다. |
| **디버깅 및 테스트** | 클래스의 내부 구조를 검사하여 디버깅이나 테스트를 더 효과적으로 수행할 수 있습니다. |
| **접근 제한 무시** | **`private`**, **`protected`** 등 접근 제한자를 무시하고 필드나 메서드에 접근할 수 있습니다. |

### **사용 방법**

리플렉션을 사용하여 클래스의 메타데이터를 동적으로 접근하고 조작하는 방법을 예제를 통해 알아보겠습니다.

## 리플렉션의 다양한 사용 방법

### **1. 클래스 정보 얻기**

클래스 정보를 얻기 위해 **`Class`** 객체를 사용합니다.

```java
public class ReflectionExample {
    public static void main(String[] args) {
        try {
            // 클래스 객체를 가져오는 방법 1: Class.forName()
            Class<?> clazz = Class.forName("com.example.MyClass");

            // 클래스 객체를 가져오는 방법 2: 클래스명.class
            Class<?> clazz2 = MyClass.class;

            // 클래스 객체를 가져오는 방법 3: 객체의 getClass() 메서드 사용
            MyClass myClassInstance = new MyClass();
            Class<?> clazz3 = myClassInstance.getClass();

            System.out.println("Class name: " + clazz.getName());
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

### **2. 필드 접근**

리플렉션을 사용하여 클래스의 필드에 접근하고 값을 설정할 수 있습니다.

```java
import java.lang.reflect.Field;

public class ReflectionExample {
    public static void main(String[] args) {
        try {
            MyClass myClassInstance = new MyClass();

            // 클래스 객체를 가져옴
            Class<?> clazz = myClassInstance.getClass();

            // 필드를 가져옴
            Field field = clazz.getDeclaredField("privateField");

            // 접근 가능하도록 설정
            field.setAccessible(true);

            // 필드 값 설정
            field.set(myClassInstance, "새로운 값");

            // 필드 값 출력
            System.out.println("Updated Field Value: " + field.get(myClassInstance));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

class MyClass {
    private String privateField = "기본 값";
}
```

### **3. 메서드 호출**

리플렉션을 사용하여 클래스의 메서드를 호출할 수 있습니다.

```java
import java.lang.reflect.Method;

public class ReflectionExample {
    public static void main(String[] args) {
        try {
            MyClass myClassInstance = new MyClass();

            // 클래스 객체를 가져옴
            Class<?> clazz = myClassInstance.getClass();

            // 메서드를 가져옴
            Method method = clazz.getDeclaredMethod("privateMethod");

            // 접근 가능하도록 설정
            method.setAccessible(true);

            // 메서드 호출
            method.invoke(myClassInstance);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

class MyClass {
    private void privateMethod() {
        System.out.println("privateMethod가 호출되었습니다.");
    }
}
```