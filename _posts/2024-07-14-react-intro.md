---
layout: post
title: React intro
subtitle: React를 배워보자
categories: 
  - React.js
tags: [react]
---

## 리엑트 프로젝트 시작하기

리엑트 프로젝트를 어떻게 구성하는지 기본 사용방법들을 알아보자

### 사용기술

- react

- style component

- redux

- router-dom

- Link

- NavLink

- 반응형 웹디자인 (데스크탑, 모바일)

### 프로젝트 구조

```
Decide4Me/
├── public/
│   ├── index.html
│   └── ...
├── src/
│   ├── assets/
│   │   ├── images/
│   │   └── styles/
│   │       ├── global.css
│   │       └── ...
│   ├── components/
│   │   ├── Header.js
│   │   ├── Navbar.js
│   │   ├── PostList.js
│   │   └── ...
│   ├── pages/
│   │   ├── LoginPage.js
│   │   ├── SignupPage.js
│   │   ├── MainPage.js
│   │   ├── UserEditPage.js
│   │   └── ...
│   ├── redux/
│   │   ├── store.js
│   │   ├── actions/
│   │   ├── reducers/
│   │   └── ...
│   ├── styles/
│   │   ├── GlobalStyles.js
│   │   ├── themes.js
│   │   └── ...
│   ├── App.js
│   ├── index.js
│   └── ...
├── package.json
└── ...



```

# Redux 기본 사용법

https://redux.js.org/tutorials/typescript-quick-start

Redux는 JavaScript 앱의 상태를 중앙에서 관리하는 라이브러리입니다. Redux를 사용하면 상태의 일관성을 유지하고, 상태 변화를 예측 가능하게 하며, 애플리케이션을 디버깅하기 쉬워집니다. 아래는 Redux의 기본 사용법에 대한 정리입니다.

## 설치

Redux와 React-Redux를 설치합니다

```bash
npm install redux react-redux
```

## 기본 개념
Redux는 세 가지 핵심 요소로 구성됩니다:

- 스토어(Store): 애플리케이션의 상태를 보관합니다.
- 액션(Action): 상태에 어떤 변화가 일어나야 하는지 설명하는 객체입니다.
- 리듀서(Reducer): 액션에 따라 상태를 업데이트하는 함수입니다.

## 스토어 생성

```js
import { createStore } from 'redux';
import rootReducer from './reducers';

const store = createStore(rootReducer);
```

## 액션
액션은 상태에 어떤 변화가 일어나야 하는지 설명하는 객체입니다. 액션 객체는 반드시 type 속성을 가져야 합니다.

```js
// 액션 타입 정의
const INCREMENT = 'INCREMENT';
const DECREMENT = 'DECREMENT';

// 액션 생성자 함수
const increment = () => ({ type: INCREMENT });
const decrement = () => ({ type: DECREMENT });

```

## 리듀서
리듀서는 상태와 액션을 받아서 새로운 상태를 반환하는 함수입니다.

```js
const initialState = { count: 0 };

const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    case INCREMENT:
      return { count: state.count + 1 };
    case DECREMENT:
      return { count: state.count - 1 };
    default:
      return state;
  }
};

export default counterReducer;

```

## 리액트와 연결
React-Redux를 사용하여 React 컴포넌트를 Redux 스토어와 연결할 수 있습니다.

```js
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import { createStore } from 'redux';
import counterReducer from './reducers';
import App from './App';

const store = createStore(counterReducer);

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);

```

## React 컴포넌트에서 Redux 사용

React 컴포넌트에서 Redux 상태와 액션을 사용하려면 useSelector와 useDispatch 훅을 사용합니다.

```js
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement } from './actions';

const Counter = () => {
  const count = useSelector(state => state.count);
  const dispatch = useDispatch();

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(decrement())}>-</button>
    </div>
  );
};

export default Counter;

```

## 결론

Redux를 사용하면 애플리케이션의 상태를 효율적으로 관리할 수 있습니다. Redux의 핵심 개념인 스토어, 액션, 리듀서를 이해하고, React-Redux를 통해 React 컴포넌트와 연결하는 방법을 알면 Redux를 쉽게 사용할 수 있습니다.



## 출처

[공식 사이트](https://react.dev/)
