---
layout: post
title: JavaScrip intro
subtitle: node를 중심으로 살펴보는 js
categories: 
  - Node.js
tags: []
---


## JavaScript 기초 문법
---

### 1. 변수 선언

JavaScript에서는 `var`, `let`, `const`를 통해 변수를 선언한다.

```javascript
var x = 10;
let y = 20;
const z = 30;
```

- `var`는 함수 스코프를 가지며, 호이스팅된다.
- `let`, `const`는 블록 스코프를 가지며, Temporal Dead Zone에 의해 선언 전 접근 시 ReferenceError가 발생한다.
- `const`는 재할당이 불가능하나, 객체 내부 프로퍼티 변경은 가능하다.

> Temporal Dead Zone (TDZ): let이나 const로 선언된 변수가 선언되기 전까지 접근이 불가능한 구간

> 호이스팅: 자바스크립트는 변수를 먼저 "호이스팅(Hoisting, 끌어올림)"한다. 변수 선언과 함수 선언이 해당 코드 블록의 최상단으로 끌어올려지는 현상을 말한다. 즉, 코드 실행 전에 선언이 자동으로 끌어올려지는 것이다.

> 하지만, let과 const는 **초기화 이전까지는 사용할 수 없는 "죽은 시간대"**에 놓인다.

> var는 호이스팅된 후 undefined로 초기화되지만, let은 선언 시점까지 **접근이 불가능한 "죽은 시간대(Temporal Dead Zone)"**에 존재한다. 이 시점까지는 변수가 아예 존재하지 않는 것처럼 취급된다.

```javascript
const obj = { a: 1 };
obj.a = 2; // 가능
obj = { b: 3 }; // 에러
```

### 2. 데이터 타입

#### 원시 타입 (Primitive Types)

자바스크립트의 원시 타입은 내부적으로 결정된다. 원시 타입은 불변(immutable) 값들을 가지며, 자바스크립트 엔진에 의해 특정한 방식으로 처리된다.


1. 자바스크립트의 원시 타입은 모두 값 자체를 저장하고, 참조가 아닌 값을 직접 다룬다.

   > 그러므로 값의 일부를 바꾸는 문법은 동작하지 않는다.
   > let str = 'hello';
   > str[0] = 'H'; // 변경되지 않음
   > console.log(str); // 'hello'

2. 내부 처리
   
   원시 타입 값들은 자바스크립트 엔진에 의해 특정한 방식으로 처리된다. 예를 들어, string은 유니코드 값으로 처리되고, number는 IEEE 754 표준에 맞춰 처리된다. 각 원시 타입은 내부적으로 특정한 형식과 규칙에 따라 처리된다

3. 원시 타입은 복사할 때 값 자체가 복사되며, 객체는 참조를 복사한다. 이 차이는 원시 타입이 값의 불변성과 독립성을 보장하는 데 중요한 역할을 한다.


- `string`
- `number`
- `boolean`
- `null`
- `undefined`
- `symbol`
- `bigint`

```javascript
let s = "hello";
let n = 123;
let b = true;
let u = undefined;
let nn = null;
```

#### 참조 타입 (Reference Types)

- 객체(Object), 배열(Array), 함수(Function)


### 3. 조건문

```javascript
let x = 5;

if (x > 0) {
  console.log("양수");
} else if (x < 0) {
  console.log("음수");
} else {
  console.log("0");
}
```

삼항 연산자도 사용 가능하다.

```javascript
let result = x > 0 ? "positive" : "non-positive";
```

### 4. 반복문

```javascript
// while
let i = 0;
while (i < 5) {
  console.log(i);
  i++;
}

// for
for (let i = 0; i < 5; i++) {
  console.log(i);
}

// for..of (iterable 객체)
const arr = [1, 2, 3];
for (let v of arr) {
  console.log(v);
}

// for..in (객체 속성)
const obj = { a: 1, b: 2 };
for (let k in obj) {
  console.log(k, obj[k]);
}
```

### 5. 리스트 (배열)

배열은 동적이고, 다양한 타입의 값을 동시에 담을 수 있다.

```javascript
let list = [1, "two", true];

list.push(4);      // 추가
list.pop();        // 마지막 요소 제거
list[1];           // 접근
list.length;       // 길이
```

고차 함수도 풍부하게 제공된다.

```javascript
list.map(x => x + "!");
list.filter(x => typeof x === "number");
list.reduce((a, b) => a + b, 0);
```

### 6. 함수 선언

```javascript
// 선언식
function add(a, b) {
  return a + b;
}

// 표현식
const sub = function (a, b) {
  return a - b;
};

// 화살표 함수
const mul = (a, b) => a * b;
```

기본 인자를 지정할 수 있다.

```javascript
function greet(name = "Guest") {
  return "Hello, " + name;
}
```

### 7. 객체 선언 및 조작

```javascript
const user = {
  name: "Alice",
  age: 25,
  greet() {
    return `Hi, I'm ${this.name}`;
  }
};

user.name;        // 접근
user["age"];      // 접근
user.job = "dev"; // 추가
delete user.age;  // 삭제
```

### 8. 클래스

ES6부터 class 문법이 도입되었다. 실제로는 프로토타입 기반 객체지향이다.

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }

  greet() {
    return `Hello, ${this.name}`;
  }
}

const p = new Person("Bob");
p.greet();
```

상속은 `extends`, 부모 클래스 호출은 `super()`를 사용한다.


### 9. 에러 핸들링

JavaScript는 예외 발생 시 `throw`, `try-catch`를 사용한다.

```javascript
try {
  throw new Error("Something went wrong");
} catch (e) {
  console.error(e.message);
} finally {
  console.log("항상 실행");
}
```

자주 쓰이는 에러 객체에는 `Error`, `TypeError`, `ReferenceError`, `SyntaxError` 등이 있다.


### 10. 비동기 처리 기본

```javascript
// setTimeout: 지연 실행
setTimeout(() => {
  console.log("1초 후 실행");
}, 1000);

// Promise
const p = new Promise((resolve, reject) => {
  resolve("완료");
});

p.then(res => console.log(res)).catch(err => console.error(err));
```

`async/await`는 비동기 코드를 동기식처럼 작성할 수 있게 해준다.
'Promise' 기반의 비동기 코드를 가독성 높게 작성할 수 있도록 도와준다.

```javascript
async function main() {
  try {
    const result = await p;
    console.log(result);
  } catch (err) {
    console.error(err);
  }
}
```


### 11. 모듈

```javascript
// a.js
export const pi = 3.14;

// b.js
import { pi } from './a.js';
```

Node.js에서는 CommonJS 방식이 기본이다.

```javascript
// CommonJS
const fs = require('fs');
```

## JavaScript 변수 선언 키워드: `var`, `let`, `const`
---

### 1. `var` - ES5 이전의 기본 변수 선언 방식

#### 특징

- **함수 스코프(Function Scope)**  
  `var`로 선언된 변수는 **가장 가까운 함수 블록을 기준으로 스코프가 결정**된다. 이는 `{}` 블록에 국한되지 않는다.

```javascript
function example() {
  if (true) {
    var x = 10;
  }
  console.log(x); // 10
}
```

- **호이스팅(Hoisting)**  
  `var`로 선언한 변수는 선언이 **스코프의 최상단으로 끌어올려진다**. 단, 초기화는 호이스팅되지 않는다.

```javascript
console.log(a); // undefined
var a = 10;
```

- **중복 선언 허용**  
  같은 스코프 내에서 `var`는 중복 선언이 가능하다.

```javascript
var x = 1;
var x = 2; // 가능
```

#### 주의점

- 블록 레벨 스코프가 없기 때문에, 의도하지 않은 변수 오염이나 덮어쓰기가 발생할 수 있다.
- `var`는 ES6 이후에는 거의 사용되지 않으며, `let` 또는 `const`가 권장된다.


### 2. `let` - ES6에서 도입된 블록 스코프 변수 선언 방식

#### 특징

- **블록 스코프(Block Scope)**  
  `let`은 `{}`로 감싸진 블록 단위로 스코프가 형성된다.

```javascript
{
  let y = 20;
}
// console.log(y); // ReferenceError
```

- **TDZ(Temporal Dead Zone)**  
  `let` 변수는 **선언 전에 접근하면 ReferenceError가 발생**한다. 선언 전에 변수에 접근하는 것이 차단된다.

```javascript
console.log(b); // ReferenceError
let b = 5;
```

- **중복 선언 불가**  
  동일한 스코프 내에서 중복 선언은 허용되지 않는다.

```javascript
let c = 1;
// let c = 2; // SyntaxError
```

- **재할당 가능**

```javascript
let d = 3;
d = 4; // 가능
```

#### 주의점

- 호이스팅은 되지만, TDZ 때문에 **실제로는 호이스팅된 시점 이전에 접근할 수 없다.**
- `for` 루프 내부에서 비동기 함수와 함께 사용할 때 유용하다. (`var`는 클로저 문제를 일으킴)


### 3. `const` - 상수(immutable binding)를 선언하는 방식

#### 특징

- **블록 스코프**
- **TDZ 적용**
- **재할당 불가**

```javascript
const pi = 3.14;
// pi = 3.14159; // TypeError
```

- **중복 선언 불가**
- 선언 시 반드시 **초기화**되어야 한다.

```javascript
// const x; // SyntaxError
```

#### 주의점

- `const`는 바인딩 자체가 불변이라는 의미이며, 객체의 내부 프로퍼티는 변할 수 있다.

```javascript
const obj = { a: 1 };
obj.a = 2; // 가능
obj = {};  // TypeError
```

- 배열도 마찬가지로 요소의 추가, 삭제는 가능하다.

```javascript
const arr = [1, 2];
arr.push(3); // 가능
```


| 항목              | `var`                  | `let`                  | `const`                |
|-------------------|------------------------|------------------------|------------------------|
| 스코프            | 함수(Function)         | 블록(Block)            | 블록(Block)            |
| 호이스팅          | O (undefined로 초기화) | O (TDZ 발생)           | O (TDZ 발생)           |
| 중복 선언         | O                      | X                      | X                      |
| 재할당            | O                      | O                      | X                      |
| 초기화 필요 여부  | X                      | X                      | O                      |
| 사용 권장 여부    | ❌                     | ✅                     | ✅ (불변일 경우)       |



## Node.js란
---

**Node.js**는 **브라우저 외부에서 JavaScript를 실행할 수 있는 런타임 환경(runtime environment)**이다.  
이는 구글 크롬에서 사용하는 **V8 JavaScript 엔진** 위에 구축되었으며, **이벤트 기반(Event-driven), 논블로킹(non-blocking) I/O 모델**을 채택하여 고성능 네트워크 애플리케이션에 적합하다.
서버 측에서 수많은 요청을 비동기 이벤트 큐로 처리할 수 있어 고성능 구현이 가능하다.

> ✅ 이벤트 기반(Event-driven)
> 자바스크립트는 이벤트가 발생할 때 실행할 콜백 함수를 등록해두고,
> 그 이벤트가 발생하면 해당 함수가 자동으로 실행되는 방식으로 동작한다.
> 대표적으로 클릭, 타이머, 서버 응답 등이 이벤트이다.

> ✅ 논블로킹(non-blocking) I/O
> I/O 작업(예: 파일 읽기, 네트워크 요청 등)이 끝날 때까지 기다리지 않고,다음 코드로 넘어간다.
> 작업이 끝나면 콜백 함수나 Promise를 통해 결과를 처리한다.


<details>
   <summary style="color: #ff6418;">논블로킹과 멀티스레드 비동기처리 차이</summary>
   <div markdown="1">
   
   결론부터 말하면 **겉보기엔 비슷하지만, 작동 방식은 다르다.**

   1. **멀티스레드 비동기 vs 논블로킹 I/O (싱글스레드 기반)**

   | 항목 | 멀티스레드 비동기 | 논블로킹 I/O (ex. Node.js) |
   |------|------------------|---------------------------|
   | 방식 | 작업마다 새로운 스레드 생성 | 싱글 스레드 + 이벤트 루프 사용 |
   | I/O 처리 | 스레드가 병렬로 I/O 수행 | 커널이 I/O 처리 후 이벤트로 알려줌 |
   | 자원 사용 | 스레드 수에 따라 메모리 소비 | 상대적으로 가볍고 효율적 |
   | 문맥 전환 비용 | 높음 (스레드 간 전환 필요) | 거의 없음 |
   | 병렬성 | 하드웨어 코어 수만큼 병렬 | 진정한 병렬은 아님 (논리적 동시성) |


   2. **멀티스레드 방식 (Java, C++ 등)**

   ```cpp
   std::thread t([]() {
   longIOOperation();
   });
   t.join();
   ```

   - I/O 작업마다 스레드를 하나 만들어서 비동기 수행.
   - 동시에 여러 작업이 **진짜 병렬로** 돌아간다.
   - 하지만 스레드가 많아지면 **메모리/CPU 오버헤드**가 커진다.

   3. **논블로킹 방식 (Node.js)**

   ```js
   fs.readFile("file.txt", (err, data) => {
   console.log("파일 읽기 완료");
   });
   console.log("다른 코드 실행");
   ```

   - 싱글 스레드에서 **논블로킹 I/O 요청만 하고**, 바로 다음 작업을 실행.
   - 파일이 다 읽히면 **이벤트 루프가 콜백을 실행**한다.
   - 실제 I/O는 OS 커널이 처리하고, 결과만 **나중에 콜백으로 전달**된다.


   4. **결론**

   - **멀티스레딩은 "진짜 병렬"**이며, 각 작업마다 스레드가 분기된다.
   - **논블로킹은 "병렬처럼 보이는 순차 처리"**이며, 싱글스레드에서 이벤트 루프 기반으로 비동기를 처리한다.
   - 결과적으로 비슷하게 "비동기 처리가 가능"하지만, **방식과 장단점은 다르다.**

   > ✍️ 즉, 논블로킹은 멀티스레드로 분기하는 것이 아니라  
   > **이벤트 루프와 OS의 비동기 지원 기능을 활용한 싱글스레드 모델**이다.

   </div>
</details>


### 1. 구성 요소

#### 1.1. V8 엔진
- 구글에서 개발한 C++ 기반의 JavaScript 엔진
- JavaScript를 머신 코드로 빠르게 컴파일하여 실행
- Node.js는 이 엔진을 사용해 JavaScript 코드를 실행한다

#### 1.2. libuv
- **비동기 I/O 처리 및 이벤트 루프**를 담당하는 라이브러리
- 파일 시스템, TCP/UDP, DNS 등의 비동기 API 제공
- Unix, Windows 등 멀티 플랫폼 지원

#### 1.3. Node.js 자체 API
- 브라우저와는 달리 DOM이나 window 객체는 없음
- 대신 `fs`, `http`, `net`, `crypto` 등의 시스템 레벨 API를 제공


### 2. 동작 모델

#### 2.1. 싱글 스레드 기반 이벤트 루프
- Node.js는 **단일 스레드(single thread)**에서 실행되며, 이벤트 루프를 통해 다수의 비동기 작업을 처리한다.
- Java나 C++의 멀티스레드 모델과 다르며, **병렬 처리가 아닌 비동기 처리 방식**으로 고성능을 달성한다.

```javascript
const fs = require('fs');

fs.readFile('file.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

- 이 예시에서 파일을 읽는 동안 CPU는 블로킹되지 않고 다른 작업을 계속 수행할 수 있다.

#### 2.2. 논블로킹 I/O
- I/O 작업(네트워크 요청, 디스크 접근 등)은 백그라운드 스레드로 넘기고, 결과가 준비되면 콜백이나 프라미스를 통해 처리한다.
- 이는 **대규모 연결 처리**에 적합한 구조이다 (예: 웹 서버, 실시간 채팅 등)


### 3. 특징

| 항목                  | 설명 |
|-----------------------|------|
| 런타임 환경           | V8 기반 JavaScript 실행 환경 |
| I/O 모델              | 논블로킹, 이벤트 기반 |
| 스레드 구조           | 메인 로직은 싱글 스레드, I/O는 백그라운드 쓰레드풀 이용 |
| 주요 용도             | 웹 서버, API 서버, CLI 도구, 마이크로서비스 |
| 핵심 라이브러리       | `fs`, `http`, `crypto`, `events`, `child_process` 등 |
| 패키지 관리자         | npm (Node Package Manager) |
| 모듈 시스템           | CommonJS (`require`) → 이후 ESM(`import`) 지원도 추가됨 |



### 4. 장단점

| 장점 | 단점 |
|------|------|
| 빠른 실행 속도 (V8) | CPU 바운드 작업에 부적합 |
| 비동기 처리에 최적화 | 콜백 지옥, 복잡한 에러 핸들링 |
| npm 생태계 풍부 | 모듈 취약점 가능성 존재 |
| 자바스크립트로 풀스택 가능 | 싱글 스레드 특성 이해 필요 |


## V8

**V8은 구글이 만든 고성능 JavaScript 엔진으로, JavaScript 코드를 해석하고 실행하는 핵심 엔진이다.**  
크롬 브라우저와 Node.js의 핵심 구성요소이기도 하다.


### 컴파일

V8은 JavaScript를 실행하기 위해 다음과 같은 **단계별 처리과정**을 가진다:

#### 1. **파싱(Parsing)**
- JavaScript 소스코드를 구문 트리(Syntax Tree)로 변환한다.

#### 2. **인터프리터 (Ignition)**
- 처음에는 **Ignition이라는 인터프리터**가 코드를 바이트코드(Bytecode)로 변환하여 빠르게 실행한다.
- 이는 초기 실행 속도를 빠르게 하기 위한 것이다.

#### 3. **JIT 컴파일러 (TurboFan)**
- **자주 실행되는 코드(hot code)**에 대해서는 **TurboFan이라는 JIT(Just-In-Time) 컴파일러**가 바이트코드를 네이티브 머신코드로 컴파일한다.
- 이 단계에서 성능 최적화가 이루어진다.

즉, **V8은 "해석기(인터프리터)"와 "컴파일러"를 모두 포함하는 엔진이며**, 컴파일러만을 의미하는 것은 아니다.


| 구분 | 설명 |
|------|------|
| **V8** | JavaScript 엔진 전체, 파서 + 인터프리터 + JIT 컴파일러 포함 |
| **Ignition** | 인터프리터, 바이트코드로 변환하여 빠르게 실행 |
| **TurboFan** | JIT 컴파일러, 바이트코드를 최적화된 네이티브 코드로 변환 |
| **역할** | JavaScript를 해석하고, 실행하고, 최적화까지 수행함 |


### 참고

V8의 발전은 과거 `Crankshaft`, `FullCodegen` 등의 컴포넌트에서 지금은 `Ignition + TurboFan` 조합으로 발전해 왔으며, ES6 이상의 최신 JavaScript 기능도 빠르게 반영하고 있다.


## ES6 vs CommonJS

**ES6(ECMAScript 2015)**와 **CommonJS**는 자바스크립트에서 **모듈 시스템**을 다루는 두 가지 대표적인 방식이다. 둘은 **목적은 같지만, 작동 방식과 철학이 다르다.**

### 1. CommonJS

#### 개요
- **Node.js**의 기본 모듈 시스템
- JavaScript를 **서버 사이드에서 사용하기 위해 만들어진 표준**
- 동기적(synchronous) 로딩 방식
- 모듈을 **즉시 실행하고 객체로 반환**함

#### 문법
```js
// export
function add(a, b) {
  return a + b;
}
module.exports = add;

// import
const add = require('./add');
console.log(add(2, 3)); // 5
```

#### 특징
- `require()`는 **런타임 시점에 모듈을 불러온다**.
- **모든 파일은 기본적으로 모듈**이며, `exports` 객체를 통해 외부 노출이 가능하다.
- **동기 로딩**이기 때문에 **브라우저 환경에는 부적절** (브라우저는 I/O 비용이 크기 때문)


### 2. ES6 Modules (ESM)

#### 개요
- **ECMAScript 2015 (ES6)**에서 도입된 공식 모듈 시스템
- **브라우저 및 Node.js 모두에서 사용 가능**
- **정적(static) 로딩** → 빌드 타임에 모듈 종속성을 파악 가능
- `import`, `export` 키워드 사용

#### 문법
```js
// export
export function add(a, b) {
  return a + b;
}

// import
import { add } from './add.js';
console.log(add(2, 3)); // 5
```

#### 특징
- **정적 구조**: 코드 파싱 시점에 모듈 구조를 파악할 수 있어 최적화에 유리
- **트리 셰이킹(tree shaking)** 가능: 사용하지 않는 export는 번들링 시 제거
- `import`는 **비동기적으로 동작**하므로, 브라우저 환경에서도 적합


### 3. 주요 차이점

| 항목 | CommonJS | ES6 Modules |
|------|----------|--------------|
| 정의된 시기 | 2009년경 | 2015년 (ES6) |
| 실행 시점 | 런타임 | 파싱 타임 |
| 로딩 방식 | 동기적 | 비동기적 |
| 문법 | `require`, `module.exports` | `import`, `export` |
| 사용 환경 | Node.js 전용 | 브라우저 + Node.js |
| 디폴트 export | `module.exports = obj` | `export default obj` |


### 4. Node.js에서의 ES6 지원

- Node.js는 `v13+`부터 ESM을 **정식 지원**함
- 하지만, `.js` 확장자를 그대로 사용할 경우 `type: "module"`을 `package.json`에 명시해야 한다

```json
{
  "type": "module"
}
```

또는 파일 확장자를 `.mjs`로 사용

### 5. 혼용 주의점

- ES6 모듈에서 CommonJS 모듈을 불러오는 것은 가능하지만, **CommonJS에서는 ESM을 직접 import할 수 없음** (특히 동기 방식 문제로 인해)
- 두 시스템은 **동작 방식이 달라 호환성 이슈가 있을 수 있음**  
  → 특히 `__dirname`, `__filename`, `require` 등은 ESM에서 기본 제공되지 않음


### 결론

| 요약 |
|------|
| CommonJS는 Node.js 초기에 탄생한 서버 중심의 동기 모듈 시스템이다. |
| ES6 Modules는 표준화된 정적 모듈 시스템으로, 트리 셰이킹과 브라우저 호환에 유리하다. |
| 현대 프로젝트에서는 ESM을 기본으로 하고, CommonJS는 레거시 지원이나 특정 패키지 사용 시에 함께 쓰인다. |


## 스코프(scope)
---

**JavaScript에서 "스코프(scope)"란 변수나 함수에 접근할 수 있는 _범위_를 의미**한다.

**"어디에서 어떤 변수에 접근할 수 있는가"를 결정하는 규칙**이다.  
이는 코드 실행 중 **변수의 유효 범위**를 이해하는 데 핵심적인 개념이다.

### 1. 스코프의 종류

#### 1) **전역 스코프 (Global Scope)**
- 함수 밖에서 선언된 변수는 **어디서든 접근 가능**
- 전역 객체(window, global 등)의 프로퍼티로 연결됨

```js
var a = 10;

function print() {
  console.log(a); // 10
}
print();
```

#### 2) **함수 스코프 (Function Scope)**
- 함수 내부에서 선언된 변수는 **해당 함수 내부에서만 접근 가능**
- 즉, function 키워드로 정의된 함수 내부에서만 접근
- `var`, `let`, `const` 모두 해당

```js
function test() {
  var x = 5;
  console.log(x); // 5
}
console.log(x); // ReferenceError
```

#### 3) **블록 스코프 (Block Scope)** (ES6부터)
- `{ }`로 감싸진 블록 내에서만 유효
- `let`, `const`는 블록 스코프를 가지며 `var`는 그렇지 않다

```js
if (true) {
  let y = 20;
  const z = 30;
}
console.log(y); // ReferenceError
```

```js
if (true) {
  var w = 50;
}
console.log(w); // 50 ← var는 블록 스코프가 아니다
```

### 2. 렉시컬 스코프 (Lexical Scope)

자바스크립트는 **렉시컬 스코프(Lexical Scope)**, 즉 **정적 스코프**를 따른다.  
이는 "변수를 어디서 선언했는지"에 따라 스코프가 결정된다는 의미이다.

```js
function outer() {
  const a = 1;
  function inner() {
    console.log(a); // 1 ← outer의 스코프에 접근
  }
  inner();
}
outer();
```

- `inner()`는 정의된 위치에 따라 `outer()`의 변수 `a`에 접근할 수 있다.
- 호출 위치가 아니라 **선언 위치 기준으로 스코프를 본다.**


### 3. 중첩 스코프 (Nested Scope)

스코프는 **중첩**될 수 있으며, **안쪽 함수는 바깥 스코프에 접근 가능**하다.

```js
const x = 1;

function outer() {
  const x = 10;
  function inner() {
    console.log(x); // 10 ← 가장 가까운 스코프부터 찾음
  }
  inner();
}
outer();
```

→ **스코프 체인(scope chain)**을 따라 가장 가까운 선언을 먼저 찾는다.

### 4. 주의할 점: `var`, `let`, `const`의 스코프 차이

| 구분 | `var` | `let` / `const` |
|------|-------|----------------|
| 스코프 | 함수 스코프 | 블록 스코프 |
| 중복 선언 | 가능 | 불가능 |
| 호이스팅 | O (초기화 undefined) | O (TDZ 적용) |

```js
console.log(a); // undefined
var a = 1;

console.log(b); // ReferenceError
let b = 2;
```

- `var`는 **호이스팅(끌어올리기)** 되어 초기화 없이 접근 가능하지만 `undefined`이다.
- `let`, `const`는 **Temporal Dead Zone(TDZ)** 구간에 걸려 ReferenceError가 발생한다.


### 요약

| 개념 | 설명 |
|------|------|
| 스코프 | 변수/함수가 **유효한 코드의 범위** |
| 전역 스코프 | 어디서든 접근 가능 |
| 함수 스코프 | 함수 내에서만 유효 |
| 블록 스코프 | `{ }` 내에서만 유효 (`let`, `const`만 해당) |
| 렉시컬 스코프 | **선언 위치 기준으로 스코프 결정** |
| 스코프 체인 | 안쪽 함수는 바깥 스코프 변수에 접근 가능 |


## Promise
---

`Promise`는 자바스크립트의 **비동기 처리를 위한 객체**이다.  
미래에 어떤 작업이 **성공하거나 실패할 것이라는 약속**을 표현한다.  
즉, 아직 값이 없지만 **언젠가는 사용할 수 있는 값에 대한 핸들러**이다.


### 1. 기본 개념

`Promise`는 **세 가지 상태**를 가진다:

- `pending` (대기 중): 아직 결과를 알 수 없음
- `fulfilled` (이행됨): 작업이 성공적으로 완료됨
- `rejected` (거부됨): 작업이 실패함

한 번 `fulfilled` 또는 `rejected`로 바뀌면 상태는 **불변**이다.


### 2. 기본 사용법

```js
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("성공");
    // reject("실패");
  }, 1000);
});

promise.then(result => {
  console.log(result); // "성공"
}).catch(error => {
  console.error(error);
});
```

- `resolve(value)`: 비동기 작업이 성공했을 때 호출
- `reject(reason)`: 실패했을 때 호출
- `.then()`: 성공 시 실행할 콜백
- `.catch()`: 실패 시 실행할 콜백
- `.finally()`: 성공/실패와 무관하게 무조건 실행되는 콜백


### 3. 체이닝

`.then()`은 새 Promise를 반환하므로 **체이닝이 가능**하다.

```js
doSomething()
  .then(result => doSomethingElse(result))
  .then(finalResult => console.log(finalResult))
  .catch(err => console.error(err));
```

이런 체이닝을 통해 **비동기 작업을 순차적으로 처리**할 수 있다.

### 4. 병렬 실행 - `Promise.all`

여러 Promise를 **병렬로 실행**하고, 모두 성공하면 결과 배열을 반환한다.

```js
Promise.all([fetch1(), fetch2()])
  .then(([result1, result2]) => {
    console.log(result1, result2);
  })
  .catch(err => {
    console.error("하나라도 실패함", err);
  });
```


### 5. 순서 무관 병렬 - `Promise.race`, `Promise.any`

- `Promise.race`: **가장 먼저 끝난 Promise의 결과를 반환**  
- `Promise.any`: **하나라도 fulfilled되면 성공으로 간주**, 모두 실패하면 에러 반환


### 6. 예외 처리

`.catch()` 또는 `try/catch` (`async/await`과 함께 사용)로 에러를 처리한다.

```js
new Promise((resolve, reject) => {
  throw new Error("에러 발생");
}).catch(err => {
  console.error(err.message); // 에러 발생
});
```



### 7. 정리

| 개념 | 설명 |
|------|------|
| `Promise` | 비동기 작업의 미래 값을 나타내는 객체 |
| `resolve()` | 작업 성공 |
| `reject()` | 작업 실패 |
| `.then()` | 성공 핸들링 |
| `.catch()` | 실패 핸들링 |
| `.finally()` | 항상 실행되는 마무리 |

### 결론

`Promise`는 비동기 로직을 더 구조화되고 예측 가능하게 만들며,  
콜백 지옥을 해결하는 기반이 된다.  
또한 `async/await`은 `Promise` 위에서 동작하므로, **Promise의 동작 원리를 이해하는 것이 필수적이다.**


## 클로저(Closure)

**클로저는 함수가 생성될 당시의 외부 변수 스코프를 기억하고, 함수가 그 스코프 밖에서 호출되더라도 접근할 수 있는 기능**을 말한다.  
다시 말해, 함수가 **자신이 선언될 때의 렉시컬 스코프(lexical scope)를 기억하는 것**이다.


### 📌 예시 1: 기본적인 클로저

```js
function outer() {
  let x = 10;
  function inner() {
    console.log(x);
  }
  return inner;
}

const closureFunc = outer();
closureFunc(); // 10
```

- `inner()`는 `outer()`의 지역변수 `x`에 접근하고 있다.
- `outer()`는 이미 실행이 끝났지만, `inner()`는 `x`를 기억하고 있다.
- 이것이 **클로저**다.


### 📌 예시 2: 반복문에서 발생하는 클로저 문제

```js
const funcs = [];

for (var i = 0; i < 3; i++) {
  funcs.push(function() {
    console.log(i);
  });
}

funcs[0](); // 3
funcs[1](); // 3
funcs[2](); // 3
```

- 위의 코드에서 모든 `funcs[i]()`가 `3`을 출력하는 이유는 `var`로 선언된 `i`가 **함수 스코프**를 가지기 때문.
- `i`는 하나의 공유된 변수이므로, 루프가 끝났을 때 `i === 3`이 되어 모두 같은 값을 참조하게 된다.

### 클로저 문제의 원인

- 루프나 비동기 환경에서 `var`를 사용할 때, 모든 함수가 **같은 변수를 참조**함.
- 이로 인해 원하지 않는 결과가 발생할 수 있다.


### 해결 방법

1. **`let`을 사용해서 블록 스코프 변수로 선언**

```js
const funcs = [];

for (let i = 0; i < 3; i++) {
  funcs.push(function() {
    console.log(i);
  });
}

funcs[0](); // 0
funcs[1](); // 1
funcs[2](); // 2
```

2. **즉시 실행 함수(IIFE)를 사용하여 클로저 생성**

```js
const funcs = [];

for (var i = 0; i < 3; i++) {
  (function(j) {
    funcs.push(function() {
      console.log(j);
    });
  })(i);
}
```

### 클로저의 장점

- **상태 유지** (private 변수처럼 사용)
- **캡슐화** (외부에서 직접 접근 불가능)
- **함수형 프로그래밍**에 적합한 구조


### 클로저의 단점 또는 주의점

- **메모리 누수**: 불필요한 참조를 유지하면 가비지 컬렉션 되지 않음
- **의도하지 않은 변수 공유**로 인해 버그 발생 가능


### 결론

- **클로저는 함수가 외부 변수의 스코프를 기억하는 현상**이다.
- 주로 콜백, 반복문, 비동기 로직에서 유용하지만, **잘못 사용하면 예상치 못한 동작**을 유발할 수 있다.
- 클로저는 자바스크립트의 강력한 기능이지만, **스코프와 변수 생명 주기에 대한 이해가 필수적이다.**


## 콜백 함수(callback function) 
---

**다른 함수에 인자로 넘겨져서, 특정 시점에 실행되는 함수**를 의미한다.


### 개념 설명

- 자바스크립트는 **함수를 값처럼 취급할 수 있는 일급 객체** 언어다.
- 따라서 함수를 **다른 함수에 인자로 전달하거나, 함수 내부에서 실행할 수 있다.**
- 이렇게 **전달된 함수가 호출되는 구조**를 **콜백(callback)**이라고 한다.

### 예제

```js
function greet(name, callback) {
  console.log("안녕하세요, " + name);
  callback(); // 전달받은 함수 실행
}

function afterGreeting() {
  console.log("환영합니다.");
}

greet("홍길동", afterGreeting);
```

출력:
```
안녕하세요, 홍길동
환영합니다.
```

- `afterGreeting`은 `greet`에 **인자로 전달된 콜백 함수**다.
- `greet`는 이름을 출력한 뒤, **콜백으로 전달된 함수**를 실행한다.


### 콜백은 언제 유용한가?

- **비동기 작업의 완료 시점에 실행할 코드**를 전달할 때 유용하다.
- 예: 파일을 다 읽은 뒤, 서버 응답을 받은 뒤, 버튼을 누른 뒤 등.

### 예: 비동기 콜백

```js
setTimeout(() => {
  console.log("3초 후 실행됨");
}, 3000);
```

- 위 코드에서 `()=>{}`는 `setTimeout`의 콜백이다.
- 3초 뒤 이벤트 루프에 의해 실행된다.


### 정리

- 콜백 함수는 **"나중에 실행하기 위해 전달하는 함수"**다.
- 주로 **비동기 작업이 완료되었을 때 실행**된다.
- Promise, async/await 같은 비동기 패턴도 내부적으로는 콜백 기반으로 구성된다.


> 📌 콜백은 "지금 실행하는 게 아니라, **필요할 때 호출해줘**"라고 함수에 부탁하는 것과 같다.