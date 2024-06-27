---
layout: post
title: express Redis 연동
subtitle: express와 Redis를 연동하는 방법
categories: 
  - Node.js
tags: [nodejs, express, redis]
---

## 요약
Key-Value형식의 NoSQL의 장점을 활용하여 데이터 검색이 빈번한 SessionID를
저장하는 db를 구현하다.

## Mac Redis 설치, 운용

### 설치하기

#### 1. 설치
  
```bash
brew install redis
sudo apt-get install redis-server
```

#### 2. 버전확인

```bash
redis-server --version

```

#### 3. 비밀번호 설정
  
apt-get설치 :/etc/redis/redis.conf 경로
Homebrew를 통해 설치:/opt/homebrew/etc/redis.conf

require your_redis_password
부분 주석제거하고 설정

#### 4. ACL설정
   
Redis 6.0 이후 버전에서 ACL(Access Control Lists) 기능이 도입

```bash
# 기본 사용자 비활성화
user default off

# 읽기 전용 사용자
user readOnlyUser on >password1 ~* +@read

# 읽기/쓰기 사용자
user readWriteUser on >password2 ~* +@all
```

### 실행하기

#### 3. Redis 백그라운드 실행(launchd이용)

```
brew services start redis

sudo service redis-server start
```

#### 3-1. 일반적인 포그라운드 실행은

  ```
  redis-server
  ```

### 클라언트 접속하기

```
redis-cli
```



### 운용하기

#### 1. 재실행

```
brew services restart redis

sudo service redis-server restart
```

#### 2. 중지

```
brew services stop redis
```

#### 3. 삭제

```
brew uninstall redis
```

#### 4. 데이터 저장

```
set KEY VALUE
```

#### 5. 데이터 조회

```
get KEY
```

#### 6. 키 조회

```
keys PATTERN
```

#### 7. 데이터 삭제

```
del KEY
```


## express에서 redis 사용하기

### 패키지 설치

```bash
npm install connect-redis redis
```

### 서버 설정

- app.js

```js
const express = require('express');
const session = require('express-session');
const RedisStore = require('connect-redis')(session);
const redis = require('redis');
const { v4: uuidv4 } = require('uuid');

const app = express();
const port = 3000;

// Redis 클라이언트 설정
const redisClient = redis.createClient({
  host: 'localhost',
  port: 6379
});

redisClient.on('error', (err) => {
  console.error('Redis error: ', err);
});

// 세션 설정
app.use(session({
  genid: (req) => {
    return uuidv4(); // 세션 ID로 UUID 생성
  },
  store: new RedisStore({ client: redisClient }),
  secret: 'your-secret-key', // 강력한 비밀 키 사용
  resave: false,
  saveUninitialized: false,
  cookie: { secure: false } // HTTPS를 사용할 경우 true로 설정
}));

app.use(express.json());
app.use(express.urlencoded({ extended: true }));

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});

```


- 사용방법

```js
// 라우트 설정
app.get('/', (req, res) => {
  res.send('Welcome to the Node.js Redis session example');
});

app.post('/login', (req, res) => {
  const { username, password } = req.body;

  // 간단한 인증 로직 (예: 사용자 이름이 "admin"이고 비밀번호가 "password"인 경우 인증 성공)
  if (username === 'admin' && password === 'password') {
    req.session.user = { username }; // 세션에 사용자 정보 저장
    res.send('Login successful');
  } else {
    res.status(401).send('Invalid credentials');
  }
});

app.get('/protected', (req, res) => {
  if (req.session.user) {
    res.send(`Hello ${req.session.user.username}, you are authenticated`);
  } else {
    res.status(401).send('You are not authenticated');
  }
});

app.post('/logout', (req, res) => {
  req.session.destroy((err) => {
    if (err) {
      return res.status(500).send('Could not log out.');
    } else {
      res.send('Logout successful');
    }
  });
});
```


- ACL 적용

```js
// 읽기/쓰기 Redis 클라이언트 설정
const readWriteClient = redis.createClient({
  host: 'localhost',
  port: 6379,
  username: 'readWriteUser', // 설정한 사용자 이름
  password: 'password2'      // 설정한 비밀번호
});

readWriteClient.on('error', (err) => {
  console.error('Redis error (read-write): ', err);
});

// 세션 설정
app.use(session({
  genid: (req) => {
    return uuidv4(); // 세션 ID로 UUID 생성
  },
  store: new RedisStore({ client: readWriteClient }), // 세션 저장은 읽기/쓰기 클라이언트 사용
  secret: 'your-secret-key', // 강력한 비밀 키 사용
  resave: false,
  saveUninitialized: false,
  cookie: { secure: false } // HTTPS를 사용할 경우 true로 설정
}));
```

### 추가: 오류 해결

redis ver6.X부터 새로운 마이그레이션이 등장하여 오류가 발생했다.

제일 큰 변경사항은 아래의 1번과 2번입니다.

더이상 redis client의 lagacy 모드를 지원하지 않습니다.
타입스크립트 코드 기반으로 재작성되어 더이상 기존 @types/connect-redis를 사용하지 않습니다.
빌드는 이제 CJS와 ESM을 모두 지원합니다. 노드 14에 대한 지원이 제거되었습니다.

그러므로
RedisStore 초기화는 더 이상 express-session을 사용하지 않는다.
별도의 RedisStore 초기화 과정없이 바로 import된 RedisStore를 사용하면 된다.

```js
// 기존
const RedisStore = require("connect-redis")(sessions);

// 현재
const RedisStore = require("connect-redis").default;
```


접근방법: redis-cli -h localhost -p 6379 -a "Tunnel325?" -u Decide4Me

redis-cli
auth Decide4Me Tunnel325?

주의
현재(2024.06.14) nodejs에는 Redis ACL의 client name을 지원하는 모듈이 없다. 그러므로 디폴트 유저로만 접근 가능하다. 디폴트 유저에 비밀번호 설정되는것에 만족하자
참조: https://stackoverflow.com/questions/63432302/connecting-to-managed-redis-with-auth-username-password-nodejs


express-session은 connect.sid라는 이름의 쿠키를 사용

## 참조

1. [sql 사용법 장 정리된 블로그](https://velog.io/@hsg5533/Node.js-express-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC%EC%99%80-MySQL%EC%9D%84-%EC%97%B0%EB%8F%99%ED%95%98%EC%97%AC-CRUD-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0)

2. [버전오류](https://techbless.github.io/2023/04/01/express-session%EC%97%90%EC%84%9C-redis%EC%82%AC%EC%9A%A9%EC%9D%84-%EC%9C%84%ED%95%9C-connect-redis-v7-%EB%B3%80%EA%B2%BD-%EC%82%AC%ED%95%AD/)

## 반영안했지만 좋은자료

1. [redis ACL 자세한 사용법](https://blog.joe-brothers.com/redis-acl/)