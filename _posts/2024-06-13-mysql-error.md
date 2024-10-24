---
layout: post
title: MySQL nodejs 에러
subtitle: Mysql user 설정 & Sequelize createdA 에러
categories: 
  - tteodeulX2/Issue
tags: [mysql, express, nodejs, sequelize]
---

## 1. MySQL의 user설정시 localhost는 %와 다르다.

'name'@'%' 유저가 'name'@'host'로는 접속 불가
%는 모든 ip에 대해 허용하지만 localhost는 허용하지 않는다.
그러므로'name'@'localhost'추가 설정 필요

## 2. Sequelize createdAt, updatedAt 자동생성

**Sequelize를 자동생성하면 createdAt과 updatedAt 필드를 자동으로 생성하여 내가 설정한 created_at등을 추가하지 않는다. 그러므로 필드명에 오류가 난다.**

Sequelize가 기본적으로 createdAt과 updatedAt 필드를 자동으로 추가하기 때문입니다. 하지만 당신의 데이터베이스 스키마에는 createdAt과 updatedAt 대신 created_at과 updated_at이라는 필드가 있습니다.



Sequelize 모델을 정의할 때, 필드 이름을 데이터베이스 스키마에 맞추어야 합니다. 이를 위해 다음과 같이 createdAt과 updatedAt 필드의 이름을 명시적으로 지정할 수 있습니다:

```js
const Sequelize = require('sequelize');
module.exports = function(sequelize, DataTypes) {
  return sequelize.define('users', {
    id: {
      type: DataTypes.STRING(36),
      allowNull: false,
      defaultValue: Sequelize.Sequelize.fn('uuid'),
      primaryKey: true
    },
    nickname: {
      type: DataTypes.STRING(30),
      allowNull: false,
      unique: "nickname"
    },
    password: {
      type: DataTypes.STRING(30),
      allowNull: false
    },
    email: {
      type: DataTypes.STRING(40),
      allowNull: false,
      unique: "email"
    },
    last_login: {
      type: DataTypes.DATE,
      allowNull: true
    },
    profile_image: {
      type: DataTypes.TEXT,
      allowNull: true
    },
    created_at: {
      type: DataTypes.DATE,
      allowNull: false,
      defaultValue: Sequelize.Sequelize.literal('CURRENT_TIMESTAMP')
    },
    updated_at: {
      type: DataTypes.DATE,
      allowNull: true
    }
  }, {
    sequelize,
    tableName: 'users',
    timestamps: false,  // 자동 생성되는 timestamps 필드를 사용하지 않음
    createdAt: 'created_at',  // createdAt 필드 이름을 created_at으로 변경
    updatedAt: 'updated_at',  // updatedAt 필드 이름을 updated_at으로 변경
    indexes: [
      {
        name: "PRIMARY",
        unique: true,
        using: "BTREE",
        fields: [
          { name: "id" },
        ]
      },
      {
        name: "nickname",
        unique: true,
        using: "BTREE",
        fields: [
          { name: "nickname" },
        ]
      },
      {
        name: "email",
        unique: true,
        using: "BTREE",
        fields: [
          { name: "email" },
        ]
      },
    ]
  });
};
```

이렇게 하면 Sequelize가 createdAt과 updatedAt 대신 created_at과 updated_at 필드를 사용하게 됩니다.






