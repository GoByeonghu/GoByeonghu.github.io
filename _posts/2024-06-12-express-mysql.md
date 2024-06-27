---
layout: post
title: express Mysql 연동
subtitle: express와 Mysql을 연동하는 방법
categories: 
  - Node.js
tags: [nodejs, express, mysql]
---

## 요약
다른 web framework와 마찬가지로 express도 sql을 직접사용하는 방법과 ORM을 이용한 방법이 있다.
트래픽이 크지 않은 서비스라면 ORM을 이용한 방법을 추천하며,
ORM을 위한 앤티티 객체를 작성할때 자동생성하는 라이브러리를 사용하는 방법까지 추천하다.

## 서버 코드에서 SQL을 직접 이용하는 방법

### 연결하기

1. mysql2 패키지 설치
  
```bash
npm install mysql2
```

1. mysql연동 모듈 생성
   
```js
var db_info = {
  host: "localhost",
  port: "3306",
  user: "root",
  password: "1234",
  database: "react_node_board",
};

module.exports = {
  init: function () {
    return mysql.createConnection(db_info);
  },
  connect: function (conn) {
    conn.connect(function (err) {
      if (err) console.error("mysql connection error : " + err);
      else console.log("mysql is connected successfully!");
    });
  },
};
```

1. app.js에서 연동모듈 호출
   
```js
const db = require("./config/mysql.js"); //연결모듈 호출
const conn = db.init(); //연동
```


### API에서 MySQL사용하기

- API형식 : query(sql, params, callback)

- 사용 예시

```js
app.get("/view", function (req, res) {
  var sql = "select * from board";
  conn.query(sql, function (err, result) {
    if (err) console.log("query is not excuted: " + err);
    else res.send(result);
  });
});
```

```js
var sql = "select * from board where bnum=" + req.params.bnum;
  conn.query(sql, function (err, result) {
    if (err) console.log("query is not excuted: " + err);
    else res.send(result);
  });
```

```js
// 게시글 수정
app.post("/update/:bnum", function (req, res) {
  var body = req.body;
  var sql =
    "update board set id=?, title=?, content=? where bnum=" + req.params.bnum;
  var params = [body.id, body.title, body.content];
  conn.query(sql, params, function (err) {
    if (err) console.log("query is not excuted: " + err);
    else res.sendStatus(200);
  });
});
```

## ORM을 사용하는 방법

express의 ORM인 sequelize를 사용하여 보다 간단하게 수행한다

### 연결

1. 패키지 설치

```
npm install mysql2 sequelize sequelize-cli 
```

주의: sequelize는 commonjs로 제공된다. 서버에서 동작함을 가정하므로 사실 당연한 것이다.

2. db용 폴더 만들기, npx sequelize init 수행

```bash
mkdir database
cd database
npx sequelize init
```
결과: config, migrations, models, seeders 폴더생성

- config.json

```json
{
  "development": {
    "username": "root",
    "password": null,
    "database": "database_development",
    "host": "127.0.0.1",
    "dialect": "mysql"
  },
  "test": {
    "username": "root",
    "password": null,
    "database": "database_test",
    "host": "127.0.0.1",
    "dialect": "mysql"
  },
  "production": {
    "username": "root",
    "password": null,
    "database": "database_production",
    "host": "127.0.0.1",
    "dialect": "mysql"
  }
}
```

연결을 담당한다. 개발용으로만 사용할거라면 development만 바꾼다.

- 참고: sequelize-cli 이용하면 데이터베이스 생성도 담당해준다.

- 참고
  - npx: node package eXecute
  - npm 5.2.0이상부터 자동설치
  - npm이 제공하는 추가 도구이다., 일회용 패키지로 작용한다.

- models/index.js
  
```js
const Sequelize = require('sequelize');
const env = process.env.NODE_ENV || 'development';
const config = require('../config/config')[env];
const db = {};
const sequelize = new Sequelize(config.database, config.username, config.password, config);
db.sequelize = sequelize;
module.exports = db;
```

- 참고
  - Model Definition또는  Model Extending을 사용하여 모델을 만든느것도 가능하다. 하지만 나는 모델을 연결할 생각이다.

1. 모델 연결

```
project-root/
├── config/
│   └── database.js
├── models/
│   └── index.js
│   └── user.js
├── routes/
│   └── userRoutes.js
├── controllers/
│   └── userController.js
├── app.js
```

- config/database.js

```js
const { Sequelize } = require('sequelize');

const sequelize = new Sequelize('database_name', 'username', 'password', {
  host: 'localhost',
  dialect: 'mysql'
});

module.exports = sequelize;

```

models/index.js

```js
const Sequelize = require('sequelize');
const sequelize = require('../config/database');

const User = require('./user');

const db = {};

db.Sequelize = Sequelize;
db.sequelize = sequelize;

db.User = User;

module.exports = db;

```

- models/user.js

- 모델정의

```js
const { DataTypes } = require('sequelize');
const sequelize = require('../config/database');

const User = sequelize.define('User', {
  id: {
    type: DataTypes.INTEGER,
    autoIncrement: true,
    primaryKey: true
  },
  nickname: {
    type: DataTypes.STRING,
    allowNull: false,
    unique: true
  },
  password: {
    type: DataTypes.STRING,
    allowNull: false
  },
  email: {
    type: DataTypes.STRING,
    allowNull: false,
    unique: true
  },
  createdAt: {
    type: DataTypes.DATE,
    defaultValue: DataTypes.NOW
  },
  updatedAt: {
    type: DataTypes.DATE
  }
});

module.exports = User;

```

- sequelize-auto이용해서 모델정의 쉽게 하는 방법
  
    - 설치
    
    ```
    npm install sequelize sequelize-auto
    ```
    
    - 모델생성
    
    ```
    npx sequelize-auto -o "./models" -d DB이름 -h localhost -u root -p 3306 -x 'password' -e mysql
    ```
    
  - o "./models": 생성된 모델 파일을 저장할 디렉터리입니다.
  - d Decide4Me_DB: 데이터베이스 이름입니다.
  - h localhost: 데이터베이스 호스트입니다.
  - u root: 데이터베이스 사용자 이름입니다.
  - p 3306: 데이터베이스 포트입니다.
  - x 'password': 데이터베이스 비밀번호입니다. ''로 감싸져야함을 주의하자
  - e mysql: 사용할 데이터베이스 엔진입니다.
  - 
  - 명령어를 실행하면 models 디렉터리에 데이터베이스 테이블에 대한 모델 파일이 생성됩니다
    
  - 생성된 모델 파일들을 models/index.js에서 불러와 사용할 수 있도록 수정
        
        1. 모델들을 직접 넣고 관계도 직접 설정
   
        ```
        // Sequelize 객체 가져오기
        const Sequelize = require('sequelize');

        // model 가져오기
        const Team = require('./team')
        const User = require('./user')

        // config 가져오기
        const env = process.env.NODE_ENV || 'development';
        const config = require('../config/config')[env];

        // db 설정
        const db = {};
        const sequelize = new Sequelize(config.database, config.username, config.password, config);
        db.sequelize = sequelize;
        db.Team = Team
        db.User = User

        // association 설정
        // Team과 User는 1:N 관계
        Team.init(sequelize)
        User.init(sequelize)
        Team.hasMany(User)
        User.belongsTo(Team)

        module.exports = db;
        module.exports = db;
        ```

        2. init_Models만 등록하여 해결 
        
        ```
        const Sequelize = require('sequelize');
        const sequelize = require('../config/database');
        const initModels = require('./init-models');

        const models = initModels(sequelize);

        const db = {};

        db.Sequelize = Sequelize;
        db.sequelize = sequelize;

        db.models = models;

        module.exports = db;

        ``` 
        
        관계도 모두 정의되어있어 편리하다

        단 creatdAt, updatedAt등 몇가지 필드는 자동생성이 디폴트라 내가 정한 필드와 이름이 다를 수 있으니. 꼭 완성된 앤티티객체를 꼼꼼히 확인해보자

1. CRUD사용
   
컨트롤러 수행 권장

```
const db = require('../database/models');
const User = db.models.users;

exports.createUser = async (req, res) => {
  try {
    const user = await User.create(req.body);
    res.status(201).json(user);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
};

exports.getUsers = async (req, res) => {
  try {
    const users = await User.findAll();
    res.status(200).json(users);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
};

exports.getUserById = async (req, res) => {
  try {
    const user = await User.findByPk(req.params.id);
    if (user) {
      res.status(200).json(user);
    } else {
      res.status(404).json({ message: 'User not found' });
    }
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
};

exports.updateUser = async (req, res) => {
  try {
    const [updated] = await User.update(req.body, {
      where: { id: req.params.id }
    });
    if (updated) {
      const updatedUser = await User.findByPk(req.params.id);
      res.status(200).json(updatedUser);
    } else {
      res.status(404).json({ message: 'User not found' });
    }
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
};

exports.deleteUser = async (req, res) => {
  try {
    const deleted = await User.destroy({
      where: { id: req.params.id }
    });
    if (deleted) {
      res.status(204).json({ message: 'User deleted' });
    } else {
      res.status(404).json({ message: 'User not found' });
    }
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
};

```

주의
Node.js의 모듈 로딩
Node.js에서는 모듈을 require할 때 다음과 같은 순서로 파일을 찾습니다:

정확한 파일 경로가 주어졌을 때 (require('./someFile.js'))
디렉토리가 주어졌을 때 해당 디렉토리 내의 index.js 파일을 로드 (require('./someDirectory') → someDirectory/index.js)
따라서, require('../models')는 사실상 require('../models/index.js')와 동일한 효과를 가집니다.

## 추가

- 이미지 업로드 폴더 자동생성(multer이용)
  
```js
const upload = multer({
  storage: multer.diskStorage({
    destination: function (req, file, callback) {
      console.log(file),
        fs.existsSync("./uploads/") ||
          fs.mkdirSync("./uploads/", { recursive: !0 }),
        callback(null, "./uploads/");
    },
    filename: function (req, file, callback) {
      callback(null, file.originalname);
    },
  }),
});

```

- cors사용 방법

```js
app.use(cors());
```

- ip설정 방법

```js
app.set("port", process.env.PORT || 3000); // 포트 설정
app.set("host", process.env.HOST || "0.0.0.0"); // 아이피 설정
```

## 회고

Django를 많이 사용하다가 아주 간만에 nodejs, express를 사용해 rest api서버를 구성해 보았다.
ORM, 앤티티 객체 자동생성은 사실 이전에는 있는지도 몰라서 사용하지 못했었는데
역시 하나의 프레임워크를 깊게 공부하고 보니 다른 프레임 워크를 사용할때도 도움이된다.
Django에는 이런게있었는데 express는 없나? 라는 생각으로 여러가지 편리한 라이브러리를 찾을 수 있었다.

## 참조

1. [sql 사용법 장 정리된 블로그](https://velog.io/@hsg5533/Node.js-express-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC%EC%99%80-MySQL%EC%9D%84-%EC%97%B0%EB%8F%99%ED%95%98%EC%97%AC-CRUD-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0)

2. [Sequlize 사용법 잘 정리된 블로그 글](https://growth-coder.tistory.com/275)