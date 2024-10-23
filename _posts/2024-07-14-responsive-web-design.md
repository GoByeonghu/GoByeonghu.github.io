---
layout: post
title: 반응형 웹 디자인(responsive web design, RWD)
subtitle: 반응형 웹 디자인을 구현하는 방법(in vanilla, react)
categories: 
  - CSS
tags: [css, react, rwd]
---

## 반응형 웹 디자인 (responsive web design, RWD)

### 정의

**하나의 웹사이트에서 PC, 모바일, 태블릿 등 접속하는 디스플레이의 종류에 따라 화면의 크기가 자동으로 변하도록 만든 웹페이지 접근 기법**

### 구현 방법

#### 1. vanilla

- **media query**

  - 사용 방법

    ```css
    @media [미디어 타입] and [미디어 특성] {
      /* 스타일 */
    }
    ```

  - 구성요소

    `미디어 타입`: screen, print, speech 등의 미디어 타입을 지정 (생략 가능)

    `미디어 특성`: 뷰포트의 크기, 해상도, 기기 방향 등의 미디어 특성을 지정 (width, height, min-width, max-width, orientation)


  - 사용 예시

    ```css
    .container {
    width: 100%;
    max-width: 600px;
    margin: 0 auto;
    padding: 1rem;
    }

    @media (min-width: 768px) {
      .container {
        max-width: 800px;
      }
    }

    @media (min-width: 992px) {
      .container {
        max-width: 1000px;
      }
    }
    ```

#### 2. react

**react-responsive**를 사용한다.

- 설치

  ```bash
  npm install react-responsive
  npm install --save @types/react-responsive
  ```

- 컴포넌트

  리엑트 훅인 useMediaQuery를 사용한다.

  ```js
  import { useMediaQuery, MediaQuery } from 'react-responsive';

  function MyComponent() {
    const isSmallScreen = useMediaQuery({ maxWidth: 767 });
    return (
      <div>
        {isSmallScreen ? (
          <p>This is a small screen.</p>
        ) : (
          <p>This is a large screen.</p>
        )}
      </div>
    );
  }

  ```

- 적용된 컴포넌트 사용

  ```js
  function MyComponent() {
    return (
      <div>
        <MediaQuery minWidth={768}>
          <p>This is a large screen.</p>
        </MediaQuery>
        <MediaQuery maxWidth={767}>
          <p>This is a small screen.</p>
        </MediaQuery>
      </div>
    );
  }
  ```


## 참조

[잘 정리된 블로그](https://velog.io/@ou9999/React-%EB%B0%98%EC%9D%91%ED%98%95-%EC%9B%B9)

