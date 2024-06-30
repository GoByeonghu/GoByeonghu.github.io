---
layout: post
title: Spring Boot and Data(미완성)
subtitle: Spring Boot에서 데이터를 다루는 방법
categories: 
    - spring
tags: [spring]
---

JPA, QueryDSL 함께 사용하고 유저 관련 기능은 jdbcTemplate 

JPA vs  vs JDBC

## 요약

JDBC

JDBC template

JPA

Spring data

Hibernate

JPQL

QueryDsl

--------------------------------------------------------------

## JDBC

Java에서 SQL을 직접 사용하게 해준다.

### 사용방법

#### 설정

- build.gradle

```Groovy
implementation 'mysql:mysql-connector-java'
```

#### DAO(Data Access Object) 

데이터베이스에 직접 접근하는 객체이다. 쿼리를 모아두고 주로 테이블 별로 설정한다.

- UserDAO

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class UserDAO {
    private final String url = "jdbc:mysql://localhost:3306/jeff_db";
    private final String user = "jeff";
    private final String password = "123123..";

    public void insertUser(String name, String email) throws SQLException {
        try (Connection connection = DriverManager.getConnection(url, user, password); // try with resource 구문이라고 합니다. 여기선 논외!
             PreparedStatement preparedStatement = connection.prepareStatement("INSERT INTO users (name, email) VALUES (?, ?)")) {
            preparedStatement.setString(1, name);
            preparedStatement.setString(2, email);
            preparedStatement.executeUpdate();
        }
    }

    public void getUsers() throws SQLException {
        try (Connection connection = DriverManager.getConnection(url, user, password);
             PreparedStatement preparedStatement = connection.prepareStatement("SELECT * FROM users")) {
            ResultSet resultSet = preparedStatement.executeQuery();
            while (resultSet.next()) {
                int id = resultSet.getInt("id");
                String name = resultSet.getString("name");
                String email = resultSet.getString("email");
                System.out.println("ID: " + id + ", 이름: " + name + ", 이메일: " + email);
            }
        }
    }

    public void updateUserEmail(String name, String newEmail) throws SQLException {
        try (Connection connection = DriverManager.getConnection(url, user, password);
             PreparedStatement preparedStatement = connection.prepareStatement("UPDATE users SET email = ? WHERE name = ?")) {
            preparedStatement.setString(1, newEmail);
            preparedStatement.setString(2, name);
            preparedStatement.executeUpdate();
        }
    }

    public void deleteUser(String name) throws SQLException {
        try (Connection connection = DriverManager.getConnection(url, user, password);
             PreparedStatement preparedStatement = connection.prepareStatement("DELETE FROM users WHERE name = ?")) {
            preparedStatement.setString(1, name);
            preparedStatement.executeUpdate();
        }
    }
}
```

> preparedStatement: 자바 변수를 쿼리 스트링에 대입 시켜주는 역할을 한다. SQL Injection 을 막을 수 있어 쿼리를 직접 입력할때 필수적이다.

### 단점

#### 1. 코드의 반복

- **패러다임의 불일치**가 발생한다.  (객체중심인 자바 vs 데이터중심인 db)

#### 2. SQL 의존


--------------------------------------------------------------

## JDBC template


--------------------------------------------------------------

## JPA(Java Persistence API)

객체지향 프로그래밍과 관계형 데이터베이스 사이의 패러다임 불일치를 해결하기 위한 자바 ORM(Object-Relational Mapping) 기술이다.
객체와 데이터베이스 테이블 간의 매핑을 정의하고, 객체를 통해 데이터를 조작할 수 있다. 이를 통해 개발자는 SQL 쿼리를 직접 작성하지 않고도 데이터베이스와 상호작용할 수 있게된다.

- **JPA**: Java Persistence API의 약자로, 자바 객체를 관계형 데이터베이스 테이블과 매핑하기 위한 ORM(Object-Relational Mapping) 기술.
- **Entity**: 데이터베이스의 테이블에 매핑되는 자바 객체.
- **ORM**: 객체-관계 매핑(Object-Relational Mapping)의 약자로, 객체 지향 프로그래밍의 객체를 관계형 데이터베이스의 데이터로 변환하는 기술.

### 동작원리 (더 많은 조사 필요)

**주의 : 내부적인 동작원리이다. 우리는 spring boot data jpa를 사용하여 이를 더 쉽게 설정하고 사용가능하다.**

JPQL 이라는 SQL과 거의 유사하게 만든 JPA 만의 쿼리 언어사용한다.
영속성 컨텍스트, 엔티티 매니저 팩토리, 엔티티 매니저가 사용된다.

#### 영속성 컨텍스트(Persistence Context)

JPA(Java Persistence API)에서 엔티티(Entity)의 생명주기를 관리하는 일종의 "저장소"이다.(애플리케이션이 데이터베이스와 상호작용할 때 사용되는 임시 데이터 저장 공간)
엔티티의 변화를 추적하고 저장하여 데이터베이스에 한 번에 반영한다.

##### 사용 방법

- build.gradle

```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    runtimeOnly 'com.mysql:mysql-connector-j'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

- resources/META-INF/persistence.xml

```xml
<!-- resources/META-INF/persistence.xml -->
<!-- 경로를 잘보자! -->
<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_1.xsd"
             version="2.1">
    <persistence-unit name="example-unit">
        <!-- 데이터베이스 설정 -->
        <properties>
            <!-- JPA 구현체: Hibernate 사용 -->
            <property name="javax.persistence.jdbc.driver" value="com.mysql.cj.jdbc.Driver"/>
            <property name="javax.persistence.jdbc.url" value="jdbc:mysql://localhost:3306/your_database"/>
            <property name="javax.persistence.jdbc.user" value="your_username"/>
            <property name="javax.persistence.jdbc.password" value="your_password"/>

            <!-- Hibernate 설정 -->
            <property name="hibernate.dialect" value="org.hibernate.dialect.MySQL8Dialect"/>
            <property name="hibernate.hbm2ddl.auto" value="update"/> <!-- 개발 단계에서는 "update" 사용, 배포 시 "validate" 권장 -->
            <property name="hibernate.show_sql" value="true"/> <!-- SQL 쿼리 출력 -->
            <property name="hibernate.format_sql" value="true"/> <!-- SQL 쿼리 포맷팅 -->
        </properties>
    </persistence-unit>
</persistence>
```

>resources 폴더는 main 폴더 바로 아래에 위치해 있다. 그 폴더에 META-INF 폴더를 만들고 그 안에 persistence.xml 파일을 만들면 된다.
우리가 spring boot data jpa 를 사용한다면 이와 같은 XML 설정은 application.yml 파일에서 가독성있게 작성할 수 있다.

- Main.java

```java
// Main.java
import jakarta.persistence.EntityManager;
import jakarta.persistence.EntityManagerFactory;
import jakarta.persistence.Persistence;


public class Main {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("example-unit"); // 엔티티매니저팩토리 생성
        EntityManager em = emf.createEntityManager(); // 엔티티매니저 생성

        em.getTransaction().begin();  // 트랜잭션 시작을 선언함
        Student st = new Student();  // Entity 클래스를 인스턴스화
        st.setName("홍박사");  // 인스턴스화 된 객체에 속성을 설정
        em.persist(st);  // 영속성 컨텍스트에 저장함 (영속성 컨텍스트라는 메모장에서 저장해둠)
        em.getTransaction().commit();  // 메모장에 저장된 쿼리를 모두 변기통 물내리듯이 데이터베이스로 flush 한다. (여기까지가 트랜잭션이며 COMMIT 을 시작함)
        em.close();
        emf.close();
    }
}
```

st 객체가 em 객체에 의해서 영속성 컨텍스트에 저장되었다가 커밋과 함께 데이터베이스에 자료가 저장된다.

##### 영속성 컨텍스트의 생명주기

- **엔티티의 상태**

    - **비영속(new/transient)** [생성]
데이터베이스에 저장되지 않았고, 영속성 컨텍스트에 관리되지 않습니다.
예: new 키워드를 통해 생성된 객체.
    - **영속(managed)** [사용 및 소멸]
데이터베이스에 저장되었고, 영속성 컨텍스트가 엔티티의 상태 변화를 추적합니다.
예: persist(), find(), merge() 메서드를 통해 영속성 컨텍스트에 추가된 객체.
    - **준영속(detached)** [사용]
데이터베이스에는 저장되어 있지만, 영속성 컨텍스트는 상태 변화를 추적하지 않습니다.
예: detach(), clear(), close() 메서드를 통해 영속성 컨텍스트에서 분리된 객체.
    - **삭제(removed)** [소멸]
속성 컨텍스트가 관리하지만, 삭제하기로 표시된 상태입니다.
트랜잭션이 커밋되면 데이터베이스에서 삭제됩니다.
예: remove() 메서드를 통해 삭제가 요청된 객체.

![entity status diagram]({{site.url}}/PostImages/2024-06-18-spring_boot_data/1.png)

- 예제 코드

```java
import jakarta.persistence.*;

public class Main {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("example-unit");
        EntityManager em = emf.createEntityManager();
        EntityTransaction transaction = em.getTransaction();

        // 비영속 상태
        Student student = new Student();  // New Transient (비영속) 상태입니다.
        student.setName("홍길동");

        // 트랜잭션 시작
        transaction.begin();

        // 엔티티를 영속 상태로 전환
        em.persist(student); // 이제 student는 Managed(영속) 상태입니다.

        // 엔티티 조회
        // 영속 상태에서 조회. `student.getId()` 변수는 유저 요청 값에 포함되어 있을 것이다.
        Student managedStudent = em.find(Student.class, student.getId()); 
        System.out.println(managedStudent.getName());

        // 영속성 컨텍스트에서 분리
        em.detach(managedStudent); // 이제 managedStudent는 Detached(준영속) 상태입니다.

        // 엔티티를 다시 영속 상태로 전환
        em.merge(managedStudent); // 이제 managedStudent는 다시 Managed(영속) 상태입니다.

        // 엔티티 삭제
        em.remove(managedStudent); // 이제 managedStudent는 Removed(삭제) 상태입니다.

        // 트랜잭션 커밋
        transaction.commit();  // 내부적으로 flush() 함수가 포함되어 있다.

        // 영속성 컨텍스트 종료
        em.close();
        emf.close();
    }
}
```

##### 영속성 컨텍스트의 특징

1. 식별자 값이 필수이다. 영속성 컨텍스트는 엔티티의 @Id 어노테이션을 붙인 속성 값을 기준으로 엔티티를 구분한다.

2. JPA는 보통 트랜잭션을 커밋하는 순간 영속성 컨텍스트에 새로 저장된 엔티티를 데이터베이스에 반영하는데 이 것을 플러시(flush()) 라고 한다.

##### 영속성 컨텍스트가 엔티티를 관리할 때 장점 (추가 조사 필요)

1. 1차 캐시

2. 동일성 보장

3. 트랜잭션을 지원하는 쓰기 지연

4. 변경 감지

5. 지연 로딩 (Lazy Loading)

6. 더티체킹

#### 엔티티 매니저 팩토리(EMF) & 앤티티 매니저(EM)

데이터베이스 연결 하나당 엔티티 매니저 팩토리 하나를 사용

엔티티 매니저를 생성, 관리한다.

실제로 엔티티 매니저는 정말 필요한 시점이 아니면 데이터베이스의 커넥션을 얻지 않습니다.
필요한 시점은 보통 트랜잭션이 시작되는 시점이며 이 때 커넥션을 획득해서 데이터베이스와 연결하게 됩니다.
(중요) EntityManager 인스턴스는 하나의 영속성 컨텍스트를 관리합니다.
영속성 컨텍스트는 엔티티를 저장하고, 변경사항을 추적하며, 데이터베이스와 동기화하는데 사용되는 1차 캐시입니다.
엔티티 매니저는 JPA 에서 엔티티의 생명주기를 관리하는 객체


### 장점

1. 직접 SQL 쿼리를 작성할 필요 없이 자바 코드로 데이터베이스 작업을 수행할 수 있다.

2. 코드의 가독성과 유지보수성이 높아진다.

3. 객체 지향적인 접근으로 데이터베이스 작업을 할 수 있다(패러다임 불일치 해결)

### 사용방법

#### 어노테이션

- **@Entity** : 엔티티임을 명시

- **@Table(name = "department")** : 연결될 테이블 명시

- **@Id** : 기본키임을 명시(엔티티 당 하나가 반듯이 있어야한다.)

- **@GeneratedValue(strategy = GenerationType.IDENTITY)** : 기본키 자동생성

- **@Column(name = "department_name")** : 칼럼과 연결

- **@OneToOne** : 매핑

- **@JoinColumn(name = "id_user", nullable = false)**


#### 예시코드

- user.class

```java

@Entity
@Table(name = "users")
public class Users {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    @Column(name = "name", nullable = false)
    private String name;

    @Column(name = "email", nullable = false)
    private String email;

    @Column(name = "phone", nullable = false)
    private String phone;

    @OneToOne(mappedBy = "user", cascade = CascadeType.ALL)
    private Department department;

    // 기본 생성자
    public User() {}

    // 기타 생성자 및 Setter 등..
}

```

##

--------------------------------------------------------------

## Spring Data

--------------------------------------------------------------


## 참조


## 추가 (lombok)

getter/setter 를 자동으로 생성

@Getter 
@Setter



