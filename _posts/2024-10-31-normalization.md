---
layout: post
title: Normalization in Relational Databases
subtitle: A Comprehensive Guide for Doctoral Students in Relational Database Management
categories: 
  - Database
tags: [db]
---

### 관계형 데이터베이스 정규화 과정

관계형 데이터베이스에서 정규화(Normalization)는 데이터의 중복을 최소화하고 무결성을 유지하기 위해 데이터 구조를 체계적으로 구성하는 과정이다. 정규화는 여러 단계로 나누어지며, 각 단계는 특정한 목표를 가지고 있다. 정규화는 주로 다음과 같은 정규형(Normal Form)으로 구분된다: 제1정규형(1NF), 제2정규형(2NF), 제3정규형(3NF), 보이스-코드 정규형(BCNF), 제4정규형(4NF) 등이다.

### 1. 제1정규형 (1NF)

제1정규형은 모든 속성이 원자값(Atomic Value)을 가져야 함을 요구한다. 즉, 각 속성은 더 이상 분해할 수 없는 단일 값을 가져야 한다. 또한, 각 행은 유일해야 하며, 순서가 없어야 한다.

#### 예시
원래의 `Students` 테이블은 다음과 같다:

| StudentID | Name      | Subjects               |
|-----------|-----------|------------------------|
| 1         | Alice     | Math, Science          |
| 2         | Bob       | English, History        |
| 3         | Charlie   | Math, Science, English  |

위 테이블은 `Subjects` 속성이 원자값이 아니므로 제1정규형을 위반한다. 이를 해결하기 위해, `Subjects`를 분해하여 각 학생에 대한 행을 별도로 생성한다.

변경된 `Students` 테이블:

| StudentID | Name    | Subject   |
|-----------|---------|-----------|
| 1         | Alice   | Math      |
| 1         | Alice   | Science   |
| 2         | Bob     | English    |
| 2         | Bob     | History    |
| 3         | Charlie | Math      |
| 3         | Charlie | Science   |
| 3         | Charlie | English    |

### 2. 제2정규형 (2NF)

제2정규형은 제1정규형을 만족하면서 부분적 종속성(Partial Dependency)을 제거해야 한다. 즉, 기본 키의 일부에만 의존하는 비기본 속성을 제거하여 모든 비기본 속성이 기본 키 전체에 의존하도록 해야 한다.

#### 예시
변경된 `Students` 테이블에서 `StudentID`가 기본 키라면, 다음과 같이 학생의 이름이 학생 ID에만 의존하게 된다. 이를 `Students`와 `Subjects` 테이블로 분리하여 종속성을 제거한다.

`Students` 테이블:

| StudentID | Name    |
|-----------|---------|
| 1         | Alice   |
| 2         | Bob     |
| 3         | Charlie |

`Subjects` 테이블:

| StudentID | Subject   |
|-----------|-----------|
| 1         | Math      |
| 1         | Science   |
| 2         | English    |
| 2         | History    |
| 3         | Math      |
| 3         | Science   |
| 3         | English    |

### 3. 제3정규형 (3NF)

제3정규형은 제2정규형을 만족하면서 이행적 종속성(Transitive Dependency)을 제거해야 한다. 즉, 비기본 속성이 다른 비기본 속성에 의존하지 않도록 한다.

#### 예시
이제 `Students` 테이블에 `StudentID`, `Name`, `DepartmentID`가 있다고 가정하자. `DepartmentID`가 다른 테이블에서 부서의 이름을 가져온다고 할 때, `DepartmentID`가 `DepartmentName`에 의존할 수 있다. 이는 이행적 종속성이므로, 이를 제거해야 한다.

`Students` 테이블:

| StudentID | Name    | DepartmentID |
|-----------|---------|--------------|
| 1         | Alice   | 10           |
| 2         | Bob     | 20           |
| 3         | Charlie | 10           |

`Departments` 테이블:

| DepartmentID | DepartmentName |
|--------------|----------------|
| 10           | Computer Science|
| 20           | Humanities     |

### 4. 보이스-코드 정규형 (BCNF)

보이스-코드 정규형은 제3정규형을 만족하면서 결정자(Determinator)가 후보 키가 아니면 안 되는 규칙이다. 즉, 모든 결정자가 후보 키여야 한다.

#### 예시
만약 `Courses` 테이블이 있다면, 각 과정의 교수와 강좌명이 있을 때, 특정 교수는 특정 강좌만 가르칠 수 있다고 가정할 수 있다. 이를 나타내는 테이블은 다음과 같다:

| CourseID | CourseName | ProfessorID |
|----------|------------|-------------|
| 1        | Math       | 101         |
| 2        | Science    | 102         |
| 3        | English    | 101         |

이 경우 `ProfessorID`가 `CourseName`을 결정할 수 있지만, `ProfessorID`는 후보 키가 아니므로 BCNF를 위반한다. 이를 해결하기 위해 `Courses`와 `Professors`를 분리한다.

`Courses` 테이블:

| CourseID | CourseName |
|----------|------------|
| 1        | Math       |
| 2        | Science    |
| 3        | English    |

`Professors` 테이블:

| ProfessorID | CourseID |
|-------------|----------|
| 101         | 1        |
| 101         | 3        |
| 102         | 2        |

### 5. 제4정규형 (4NF)

제4정규형은 다치 종속성(Multi-Valued Dependency)을 제거하는 것이다. 즉, 한 속성이 두 개 이상의 독립적인 다치 종속성을 가질 수 없도록 해야 한다.

#### 예시
예를 들어, 학생이 여러 과목을 수강하고 여러 동아리에 가입할 수 있다고 가정하자. 원래 테이블은 다음과 같을 수 있다:

| StudentID | Subjects       | Clubs        |
|-----------|----------------|--------------|
| 1         | Math           | Chess        |
| 1         | Science        | Drama        |
| 2         | English        | Debate       |

이 경우 `Subjects`와 `Clubs`는 독립적이며, 이 둘을 분리해야 한다.

`StudentSubjects` 테이블:

| StudentID | Subject   |
|-----------|-----------|
| 1         | Math      |
| 1         | Science   |
| 2         | English    |

`StudentClubs` 테이블:

| StudentID | Club     |
|-----------|----------|
| 1         | Chess    |
| 1         | Drama    |
| 2         | Debate   |

### 결론

정규화는 데이터베이스 설계를 통해 데이터의 중복을 최소화하고 무결성을 높이는 데 필수적이다. 각 정규형은 데이터의 특정한 종속성을 다루며, 이를 통해 체계적이고 효율적인 데이터베이스 구조를 구축할 수 있다. 따라서, 정규화 과정은 데이터베이스 설계의 기초가 되며, 데이터베이스 관리 시스템(DBMS)에서 데이터를 효과적으로 관리하고 운영하는 데 중요한 역할을 한다.


