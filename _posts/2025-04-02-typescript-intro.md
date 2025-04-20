---
layout: post
title: TypeScript intro
subtitle: Node.js를 TypeScript로 안전하게
categories: 
  - Node.js
tags: []
---

## 정의& 구성 방법
---

TypeScript는 Microsoft에서 개발한 오픈소스 프로그래밍 언어로, 자바스크립트의 확장된 버전이다.
TypeScript는 JavaScript에 타입 시스템을 추가한 언어로, 코드 작성 시 타입 안전성을 높여준다. 
Node.js에서 TypeScript를 적용하는 방법을 정리해보겠다.

### 1. TypeScript 설치
TypeScript를 사용하려면 먼저 TypeScript를 프로젝트에 설치해야 한다. npm을 이용해 TypeScript를 설치할 수 있다.

```bash
npm install typescript --save-dev
```

또한, TypeScript와 함께 Node.js에서 타입을 지원하는 `@types/node` 패키지를 설치하는 것이 좋다.

```bash
npm install @types/node --save-dev
```

### 2. `tsconfig.json` 설정
`tsconfig.json` 파일은 TypeScript 컴파일러 옵션을 설정하는 파일이다. 프로젝트 루트에 `tsconfig.json`을 생성한다.

```bash
npx tsc --init
```

생성된 `tsconfig.json` 파일에서 주요 옵션은 다음과 같다:

- `target`: 컴파일된 JavaScript의 버전을 설정한다. (예: `"target": "ES6"`)
- `module`: 사용할 모듈 시스템을 설정한다. (예: `"module": "commonjs"`)
- `outDir`: 컴파일된 JavaScript 파일이 저장될 디렉토리를 설정한다. (예: `"outDir": "./dist"`)
- `rootDir`: TypeScript 소스 파일의 루트 디렉토리를 설정한다. (예: `"rootDir": "./src"`)

### 3. TypeScript 코드 작성
Node.js에서 TypeScript 파일을 작성하려면 `.ts` 확장자를 사용한다. 예를 들어, `app.ts`라는 파일을 작성할 수 있다.

```typescript
const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello, World!');
});

server.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

### 4. TypeScript 컴파일
TypeScript 파일을 JavaScript로 컴파일하려면 다음 명령어를 실행한다.

```bash
npx tsc
```

이 명령어는 `tsconfig.json` 파일을 기준으로 TypeScript 코드를 컴파일하여 설정된 `outDir` 디렉토리에 `.js` 파일을 생성한다.

### 5. Node.js에서 TypeScript 실행
TypeScript로 작성한 코드를 Node.js에서 실행하려면 `ts-node`를 사용할 수 있다. `ts-node`는 TypeScript 코드를 직접 실행할 수 있게 해준다.

```bash
npm install ts-node --save-dev
```

그 후, `ts-node`로 TypeScript 파일을 실행한다.

```bash
npx ts-node src/app.ts
```

### 6. 타입 정의 사용
Node.js와 같은 JavaScript 라이브러리에서 TypeScript의 타입 시스템을 활용하려면, 해당 라이브러리에 맞는 타입 정의를 추가해야 한다. 예를 들어, `express`를 사용할 경우 `@types/express`를 설치해야 한다.

```bash
npm install express
npm install @types/express --save-dev
```

### 7. 빌드 및 실행 자동화
TypeScript 프로젝트에서 빌드 및 실행을 자동화하려면 `npm scripts`를 설정할 수 있다. 예를 들어, `package.json`에 다음과 같은 스크립트를 추가할 수 있다.

```json
"scripts": {
  "start": "node dist/app.js",
  "dev": "ts-node src/app.ts",
  "build": "tsc"
}
```

`npm run build`로 TypeScript를 컴파일하고, `npm run start`로 실행할 수 있다.

### 8. 타입 검사
TypeScript는 코드를 실행하기 전에 타입을 검사한다. 예를 들어, 다음과 같이 잘못된 타입을 사용하면 컴파일 시 오류가 발생한다.

```typescript
let message: string = 123;  // 오류: 'number' 타입을 'string' 타입에 할당할 수 없다.
```

이와 같이 TypeScript는 타입을 엄격히 검사하므로, 개발 중에 실수를 줄일 수 있다.

### 9. 추가적인 설정
`tsconfig.json`에서 `strict` 옵션을 활성화하면 더 엄격한 타입 검사를 수행할 수 있다.

```json
"strict": true
```

이 옵션을 활성화하면 `null` 및 `undefined` 타입을 더 철저하게 검사하고, 더 안전한 코드를 작성할 수 있다.


## 기초 문법
---

TypeScript는 JavaScript에 타입을 추가한 언어로, 타입 안전성을 보장하며 코드의 품질을 높이는 데 유용하다. TypeScript의 주요 문법과 개념을 JavaScript와 비교하여 쉽게 설명해보겠다.

### 1. **TypeScript 기본 개념**
TypeScript는 JavaScript의 슈퍼셋으로, JavaScript의 모든 코드가 TypeScript에서 유효하다. TypeScript는 정적 타입 시스템을 제공하여 런타임 전에 타입 오류를 잡을 수 있다. 이를 통해 코드 작성 시 오류를 예방하고, 더 안전하고 예측 가능한 코드를 작성할 수 있다.

### 2. **타입 선언 (Type Annotation)**
TypeScript에서 변수, 함수 매개변수 및 반환 값의 타입을 명시할 수 있다. 이는 JavaScript와 가장 큰 차이점이다.

```typescript
let name: string = "Alice";
let age: number = 25;
let isStudent: boolean = true;
```

### 3. **기본 타입**
TypeScript에는 JavaScript에서 사용하는 기본 타입 외에도 몇 가지 추가적인 타입이 있다.

- **string**: 문자열
- **number**: 숫자
- **boolean**: true 또는 false
- **null**: 값이 없음
- **undefined**: 선언되었지만 값이 없는 상태
- **any**: 타입을 지정하지 않거나 어떤 타입도 허용
- **void**: 함수에서 값을 반환하지 않는 경우
- **never**: 절대 값이 반환되지 않는 함수의 반환 타입

예시:
```typescript
let message: string = "Hello, TypeScript!";
let total: number = 100;
let isActive: boolean = false;
let nothing: null = null;
let notDefined: undefined = undefined;
```

### 4. **배열과 튜플 (Array and Tuple)**
배열은 `type[]` 또는 `Array<type>` 형태로 선언할 수 있다. 튜플은 고정된 길이와 타입의 배열을 의미한다.

```typescript
let numbers: number[] = [1, 2, 3];  // 배열
let person: [string, number] = ["Alice", 25];  // 튜플
```

### 5. **객체 타입 (Object Type)**
객체는 `type` 또는 `interface`를 사용해 정의할 수 있다. `type`과 `interface`는 비슷하지만, `interface`는 객체 구조를 명확히 정의하고 확장할 수 있다는 점에서 유용하다.

```typescript
// 객체 타입 정의
let user: { name: string, age: number } = { name: "Bob", age: 30 };

// 인터페이스 사용
interface Person {
  name: string;
  age: number;
}

let user2: Person = { name: "Alice", age: 25 };
```

### 6. **함수 타입 (Function Type)**
함수의 매개변수와 반환값의 타입을 정의할 수 있다. TypeScript는 함수 타입에 대한 자세한 검사를 지원하여 오류를 방지한다.

```typescript
function greet(name: string): string {
  return `Hello, ${name}`;
}

let add = (a: number, b: number): number => a + b;
```

### 7. **타입 추론 (Type Inference)**
TypeScript는 변수의 값을 보고 타입을 자동으로 추론할 수 있다. 명시적으로 타입을 지정하지 않아도, TypeScript는 변수의 타입을 알아낸다.

```typescript
let num = 10;  // TypeScript는 num을 number로 추론
num = "Hello"; // 오류 발생 (타입이 일치하지 않음)
```

### 8. **Union 타입 (Union Type)**
Union 타입을 사용하면 하나의 변수나 함수 매개변수가 여러 타입을 가질 수 있다. `|` 기호를 사용하여 여러 타입을 결합할 수 있다.

```typescript
let value: string | number = "Hello";
value = 100;  // OK
value = true;  // 오류 (boolean은 포함되지 않음)
```

### 9. **Intersection 타입 (Intersection Type)**
Intersection 타입은 여러 타입을 하나로 결합할 때 사용한다. `&` 기호를 사용하여 여러 타입을 합칠 수 있다.

```typescript
interface A {
  name: string;
}

interface B {
  age: number;
}

let person: A & B = { name: "Alice", age: 25 };
```

### 10. **Optional과 Default 매개변수**
함수 매개변수에 기본값을 설정하거나 선택적으로 만들 수 있다. `?` 기호를 사용하여 선택적 매개변수를 정의하고, `=` 기호를 사용하여 기본값을 설정할 수 있다.

```typescript
function greet(name: string, age?: number): string {
  return age ? `Hello, ${name}. You are ${age} years old.` : `Hello, ${name}.`;
}

function sum(a: number, b: number = 5): number {
  return a + b;
}
```

### 11. **Enum (열거형)**
`enum`은 상수 값을 정의하는 데 사용된다. 숫자 또는 문자열을 기반으로 한 상수 집합을 정의할 수 있다.

```typescript
enum Direction {
  Up = 1,
  Down,
  Left,
  Right
}

let move: Direction = Direction.Up;
```

### 12. **인터페이스 (Interface)**
인터페이스는 객체 구조를 정의하는 데 사용된다. `interface`는 `type`과 비슷하지만, 객체의 구조를 명확히 정의하고, 객체가 해당 구조를 따르도록 강제한다. 또한, 다른 인터페이스를 확장할 수 있는 특징이 있다.

```typescript
interface Animal {
  name: string;
  sound: string;
}

interface Dog extends Animal {
  breed: string;
}

let dog: Dog = {
  name: "Buddy",
  sound: "Bark",
  breed: "Labrador"
};
```

### 13. **클래스 (Class)**
TypeScript는 JavaScript의 클래스 문법을 확장하여 사용할 수 있다. 클래스는 객체 지향 프로그래밍(OOP)의 기본적인 구성 요소로, 생성자(constructor), 속성, 메서드를 포함할 수 있다.

```typescript
class Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  greet(): string {
    return `Hello, my name is ${this.name}`;
  }
}

let person1 = new Person("Alice", 25);
console.log(person1.greet());
```

### 14. **제네릭 (Generics)**
제네릭은 함수나 클래스에서 다양한 타입을 처리할 수 있게 해준다. 이를 통해 코드의 재사용성을 높이고, 타입 안전성을 보장할 수 있다.

```typescript
function identity<T>(arg: T): T {
  return arg;
}

let numberIdentity = identity(5);  // T는 number
let stringIdentity = identity("Hello");  // T는 string
```

### 15. **타입 가드 (Type Guards)**
타입 가드는 특정 코드 블록 내에서 타입을 좁히는 데 사용된다. `typeof`나 `instanceof`를 사용하여 객체나 변수의 타입을 확인하고, 조건문 내에서 타입을 좁힐 수 있다.

```typescript
function isString(value: any): value is string {
  return typeof value === "string";
}

let result: any = "Hello";

if (isString(result)) {
  console.log(result.toUpperCase());  // result는 string으로 타입이 좁혀짐
}
```

### 16. **모듈과 네임스페이스**
TypeScript는 코드의 구조화를 위해 모듈과 네임스페이스를 사용할 수 있다. 모듈은 코드 파일을 나누어 모듈화하는 방법이고, 네임스페이스는 동일한 이름을 가진 코드를 그룹화하는 방법이다.

- **모듈**: `import`와 `export`를 사용하여 다른 파일에서 코드를 가져오거나 내보낼 수 있다.
- **네임스페이스**: 동일한 이름을 가진 코드 그룹을 묶는 데 사용된다.

```typescript
// math.ts
export function add(a: number, b: number): number {
  return a + b;
}

// app.ts
import { add } from './math';

console.log(add(2, 3));
```

## JS와 TS 분리방법
---

맞다, TypeScript에서는 JavaScript와 TypeScript 파일을 분리하여 사용할 수 있다. JavaScript 파일과 TypeScript 파일을 함께 사용할 때 몇 가지 방법을 활용할 수 있다. TypeScript는 기본적으로 JavaScript와 호환되지만, TypeScript에서 제공하는 타입 시스템을 적극적으로 활용하려면 TypeScript 파일(`.ts`)을 사용하는 것이 좋다.

### 1. **JavaScript 파일과 TypeScript 파일 분리**
TypeScript 프로젝트에서는 JavaScript 파일과 TypeScript 파일을 분리할 수 있다. 이때, 기존의 JavaScript 파일을 그대로 사용하면서 TypeScript를 점진적으로 도입할 수 있다.

#### 예시:
- `app.js` (JavaScript 파일)
- `app.ts` (TypeScript 파일)

### 2. **TypeScript에서 JavaScript 파일 사용**
TypeScript 프로젝트에서 기존의 JavaScript 파일을 사용할 때, TypeScript는 기본적으로 JavaScript 파일도 사용할 수 있도록 허용한다. `.ts` 파일에서 `.js` 파일을 `import` 할 수 있다.

#### 예시:
`app.js` (JavaScript 파일):
```javascript
// app.js
function greet(name) {
  return "Hello, " + name;
}
```

`main.ts` (TypeScript 파일):
```typescript
// main.ts
import { greet } from './app'; // app.js에서 greet 함수 사용

console.log(greet("Alice"));
```

위와 같이 TypeScript 파일에서 JavaScript 파일을 `import` 할 수 있다.

### 3. **TypeScript 파일에서 `allowJs` 설정**
TypeScript는 `tsconfig.json`에서 `allowJs` 옵션을 설정하여 JavaScript 파일을 TypeScript 프로젝트에 포함시킬 수 있다. 이 옵션을 활성화하면 TypeScript 컴파일러가 JavaScript 파일도 함께 처리하게 된다.

#### `tsconfig.json` 예시:
```json
{
  "compilerOptions": {
    "allowJs": true,  // JavaScript 파일을 허용
    "outDir": "./dist",  // 컴파일된 파일의 출력 경로
    "target": "ES6",  // 자바스크립트 버전 설정
    "module": "commonjs"
  },
  "include": [
    "src/**/*.ts",  // TypeScript 파일 경로
    "src/**/*.js"   // JavaScript 파일 경로
  ]
}
```

이렇게 설정하면, TypeScript가 `.js` 파일도 함께 컴파일하며, TypeScript 프로젝트에서 JavaScript 파일을 혼합하여 사용할 수 있다.

### 4. **점진적인 TypeScript 도입**
TypeScript 프로젝트에서 JavaScript 파일을 사용하면서 점진적으로 TypeScript로 마이그레이션하는 방법도 있다. 이 방식은 기존 JavaScript 코드에 대한 변환 작업을 점차적으로 할 수 있도록 도와준다.

#### 예시:
- 기존의 JavaScript 파일은 그대로 두고, 새로운 파일만 TypeScript로 작성.
- 점진적으로 기존 JavaScript 파일을 TypeScript로 변경.
- `allowJs`를 설정하여 JavaScript 파일을 TypeScript 파일과 함께 컴파일.

### 5. **TypeScript에서 JavaScript 코드 타입 정의**
TypeScript에서 기존의 JavaScript 코드에 대해 타입을 명시하고 싶다면, `.d.ts` 파일을 사용할 수 있다. 이 파일은 "타입 선언 파일"이라고 하며, JavaScript 코드에 대한 타입 정보를 제공하는 용도로 사용된다.

#### 예시:
`app.js` (JavaScript 파일):
```javascript
// app.js
function greet(name) {
  return "Hello, " + name;
}
```

`app.d.ts` (타입 정의 파일):
```typescript
// app.d.ts
declare function greet(name: string): string;
```

이렇게 `.d.ts` 파일을 사용하여 JavaScript 코드에 대한 타입을 정의하면, TypeScript 파일에서 해당 JavaScript 파일을 사용할 때 타입 체크를 받을 수 있다.

### 6. **컴파일 후 JavaScript 코드 사용**
TypeScript 코드를 컴파일하면 `.ts` 파일은 `.js`로 변환된다. TypeScript 파일을 컴파일하여 생성된 `.js` 파일은 기존 JavaScript 프로젝트에서 그대로 사용할 수 있다. 이를 통해 TypeScript로 작성된 코드를 다른 JavaScript 프로젝트에 통합하거나, Node.js에서 사용할 수 있다.

### 결론
JavaScript 파일과 TypeScript 파일을 분리하는 방법은 다음과 같다:

- **점진적 도입**: `allowJs`를 사용하여 기존의 JavaScript 파일을 TypeScript 프로젝트에 통합할 수 있다.
- **타입 정의 파일 사용**: JavaScript 코드에 대한 타입 정의를 `.d.ts` 파일로 제공하여 TypeScript에서 타입 검사 기능을 사용할 수 있다.
- **컴파일 후 사용**: TypeScript 파일을 컴파일하여 생성된 JavaScript 파일을 다른 JavaScript 코드와 함께 사용할 수 있다.

이와 같은 방법으로 TypeScript와 JavaScript 파일을 혼용하여 사용할 수 있으며, 프로젝트 규모나 팀의 요구사항에 맞게 점진적으로 TypeScript를 도입할 수 있다.


## Quiz
---

### ✅ Type과 Interface의 차이
`type`은 타입에 별칭을 지정하는 방식이고, `interface`는 구조를 선언하는 방식이다.  
`interface`는 주로 객체의 구조를 정의할 때 사용되며 `extends`로 확장이 가능하다.  
`type`은 유니언, 튜플, 프리미티브 등 다양한 타입에 대해 별칭을 줄 수 있다.  
`interface`는 병합(Declaration Merging)이 가능하지만, `type`은 불가능하다.  
최근에는 대부분의 상황에서 `type`이 더 유연하여 더 많이 사용되는 추세이다.


### ✅ 타입 추론
타입스크립트는 변수나 함수의 리턴값 등에 대해 명시적으로 타입을 선언하지 않아도, 초기값을 기반으로 타입을 자동으로 유추하는 기능을 제공한다.  
예: `let name = "John"`이라고 하면 `name` 변수의 타입은 자동으로 `string`으로 추론된다.


### ✅ 타입스크립트를 사용하는 이유
JavaScript는 동적 타입 언어이기 때문에 런타임 오류가 자주 발생한다. TypeScript는 컴파일 타임에 타입 오류를 방지해 주므로 안정성과 유지보수성이 높아진다.  
IDE의 자동완성과 리팩토링 지원도 향상되며, 대규모 프로젝트에서 협업에 매우 유리하다.


### ✅ 제네릭 (Generic)
제네릭은 함수, 클래스, 인터페이스 등을 다양한 타입에서 재사용할 수 있도록 만든 기능이다.  
예: `function identity<T>(arg: T): T { return arg; }`처럼 타입에 의존하지 않고 재사용 가능한 구조를 제공한다.


### ✅ Public, Private, Protected
클래스 멤버의 접근 제어자를 의미한다.  
- `public`: 어디서든 접근 가능  
- `private`: 클래스 내부에서만 접근 가능  
- `protected`: 클래스 내부와 상속받은 클래스에서 접근 가능  


### ✅ Static
`static` 키워드를 사용하면 해당 속성이나 메서드는 클래스 인스턴스가 아니라 클래스 자체에 속하게 된다.  
예: `User.count`는 `new User()`가 아닌 `User`를 통해 접근해야 한다.


### ✅ Enums
열거형은 관련된 상수들을 하나의 그룹으로 묶는 방법이다.  
`enum Direction { Up, Down, Left, Right }`와 같이 선언하며, 값은 자동 증가하거나 수동으로 지정할 수 있다.  
런타임에도 존재하는 JS 코드로 변환되며, 코드 가독성과 유지보수에 유리하다.


### ✅ Union Types
하나의 변수에 여러 타입 중 하나가 올 수 있도록 정의하는 타입이다.  
예: `let value: string | number`처럼 선언하면 `string` 또는 `number` 타입을 모두 받을 수 있다.


### ✅ TypeScript의 주요 특징
- 정적 타이핑: 코드 작성 시점에 타입을 검사할 수 있다.
- 인터페이스와 클래스: 명확한 구조화가 가능하다.
- 제네릭 타입: 다양한 타입에 대해 재사용 가능한 코드 작성이 가능하다.
- 데코레이터: 클래스에 메타프로그래밍을 가능하게 한다.
- 네임스페이스와 모듈: 코드 구조를 체계적으로 관리할 수 있게 한다.
- 튜플 타입: 고정된 길이의 배열에 각 요소의 타입을 지정할 수 있다.


### ✅ TypeScript와 JavaScript의 가장 큰 차이점
TypeScript는 정적 타입 언어이며 컴파일 언어이다.  
JavaScript는 동적 타입의 인터프리터 언어이다.  
TypeScript는 JavaScript의 상위 집합으로, 정적 타입과 컴파일 단계의 오류 검사를 통해 개발 효율성과 안정성을 향상시킨다.


### ✅ TypeScript의 데이터 타입 종류
- 기본 타입: `string`, `number`, `boolean`, `null`, `undefined`
- 배열, 튜플, 열거형
- 객체 타입
- 유니언, 인터섹션
- 제네릭
- any, unknown, never
- 사용자 정의 타입 (`type`, `interface`)


### ✅ TypeScript의 컴파일 / 컴파일러 (tsc)
TypeScript는 `.ts` 파일을 `.js` 파일로 변환하여 실행하는 언어이다.  
이 작업은 TypeScript 컴파일러(`tsc`)에 의해 수행된다.  
`tsconfig.json`을 통해 컴파일 옵션과 대상 파일 등을 설정할 수 있다.


### ✅ 타입 단언 (Type Assertion)
개발자가 변수의 타입을 명확히 알고 있다는 가정 하에 컴파일러에게 타입을 알려주는 문법이다.  
`as` 키워드를 사용하며, 예: `value as string`


### ✅ 인터페이스와 클래스의 차이점
`interface`는 객체의 구조만 정의하며 구현이 없다.  
`class`는 필드, 생성자, 메서드를 모두 포함하는 구현체이다.  
`interface`는 `implements`를 통해 클래스가 따를 구조를 명세할 수 있다.


### ✅ 네임스페이스와 모듈의 차이점
네임스페이스는 전통적인 JavaScript의 전역 스코프 충돌 방지를 위한 방법이고, 모듈은 ES6 기반의 파일 단위 코드 분할 방식이다.  
현재는 모듈 사용이 권장된다.


### ✅ 추상 클래스 (Abstract Class)
일부 메서드만 구현하고 나머지는 자식 클래스에서 구현하도록 강제하는 클래스이다.  
`abstract` 키워드로 선언하며, 객체를 직접 생성할 수 없다.


### ✅ 타입 가드 (Type Guard)
`typeof`, `instanceof`, 사용자 정의 함수를 사용하여 런타임에 변수의 타입을 좁히는 기술이다.  
예:  
```ts
function isString(value: unknown): value is string {
  return typeof value === "string";
}
```

### ✅ 비구조화 할당 (구조 분해 할당)
객체나 배열에서 값을 추출하여 변수에 할당하는 문법이다.  
```ts
const { name, age } = person;
const [a, b] = [1, 2];
```


### ✅ Ambient Module Declaration
타입이 정의되지 않은 외부 라이브러리 또는 JS 모듈에 대한 타입 정보를 선언하는 방식이다.  
보통 `@types`가 없거나 커스텀 JS를 사용할 때 사용하며, `declare module '패키지명' {}` 형태로 작성한다.


### ✅ 열거형 (Enum)
앞서 설명한 Enums와 동일하며, 내부적으로 숫자 또는 문자열 기반 열거형을 구성한다.  
문자열 열거형의 경우 명시적으로 값을 설정해야 하며, 컴파일 후에도 해당 객체가 유지된다.

### ✅ type과 interface 차이
`type`과 `interface` 모두 타입을 정의하는 기능이지만, 차이점이 존재한다.  
`type`은 유니온 타입이나 튜플, 매핑 타입 등 복잡한 표현이 가능하다.  
`interface`는 선언 병합, 상속(extends)에 더 적합하다.  
실제 코드에서는 유연한 확장을 위해 `interface`가, 복잡한 조합에는 `type`이 자주 쓰인다.


### ✅ any 타입과 unknown 타입

`any`는 타입 검사 없이 어떤 타입도 허용한다. 타입 안전성이 없고, 최대한 피해야 한다.  
`unknown`은 모든 타입을 받을 수 있지만, 실제 사용 전 타입 검사를 요구한다.  
따라서 `unknown`은 `any`보다 더 안전한 타입이다.  
예를 들어, 함수 인자로 외부 데이터를 받을 때는 `unknown`을 쓰고 내부에서 검증 후 처리하는 방식이 좋다.

확장 방식에서 차이가 있다. interface는 같은 이름으로 선언할 경우 확장이 되고(선언적 확장) type은 같은 이름으로 선언할 수 없다. type은 &연산자를 이용해서 확장해야 한다.
정리하면 interface는 extends 또는 같은 이름으로 선언을 해서 타입을 확장할 수 있고 type은 &연산자를 이용해서 확장해야 한다.
type은 { [key in names]: string }와 같이 computed value의 사용이 가능하지만 interface는 불가능하다.
개발팀의 상황과 컨벤션에 따라둘을 유연하게 사용해야 한다.

## 참조
---

- [타입스크립트 면접 질문 정리 블로그](https://s-saramnim.tistory.com/entry/TypeScript-%EB%A9%B4%EC%A0%91-%EC%98%88%EC%83%81-%EC%A7%88%EB%AC%B8)