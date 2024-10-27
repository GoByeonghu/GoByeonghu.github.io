---
layout: post
title: 중간테이블에 복합키를 적용할 것인가
subtitle: 스프링을 중심으로 살펴보는 중간테이블 후보키 선택 전략
categories: 
  - Database
tags: [db, composite key, java]
---

## 서론
---

나는 많은 프로젝트에서 ERD를 설계하며 다대다 테이블 혹은 일대다 관계의 엔티티를 정규화하여 표현하기 위해 중간 테이블을 두었다.
이때, 매핑관계가 레코드와 레코드가 고유하게 연결된다면 기본키 선젱에 있어 두 가지의 선택지가 생긴다.

1. 관리를 위한 id 키를 추가한다.
2. 고유하게 식별 가능한 두개 이상의 컬럼을 묶어 복합키를 기본키로 설정한다.

위 두 가지 선택지의 특징과 장단점을 확인해본다.

## 본론
---

### 복합키(Composite Key)란

- 두 개 이상의 칼럼으로 구성된 기본키
- 행의 고유성은 보장하지만 복합키 개별 컬럼에 대한 고유성은 보장하지 않는다.
- 하나의 칼럼만으로는 레코드의 고유성을 보장하기 어려울 때, 두 개 이상의 칼럼을 결합하여 복합키를 정의하여 사용하고는 한다. 중복도와 반대되는 개념이다.

### 복합키의 특징

대부분의 RDBMS는 PK에 대하여 인덱스 생성을 자동적으로 수행한다.(PK Index) 복합키의 경우 복합키 인덱스를 생성하는데 조회 성능을 고려하여 생성해야한다.
**복합키의 PK Index는 카디널리티가 높은 곳에서 낮은곳으로 인덱스를 구성해야한다.**

>**카디널리티(Cardinality)** : 카디널리티는 전체 행에 대한 특정 컬럼의 중복 수치를 나타내는 지표이다. 높으면 고유한것이고 낮으면 중복될 가능성이 높은 칼럼이다. DISTINCT를 사용했을 때의 출력값이 많을수록(사용 전후가 차이가 적을수록) 카디널리티도 높다고 생각할 수 있다.

- **장점**
  1. **엄격한 기본키 제약조건 적용 가능**: 두 외래키의 조합이 유일해야 함을 보장할 수 있다.
  2. **공간 절약**: ID 칼럼을 추가하지 않아 공간을 절약할 수 있다.
  3. **성능 최적화**: 복합 키 인덱스를 사용해 관계를 바로 검색할 수 있어 기본적으로 조회 성능이 향상된다.
  4. **ORM 복잡성**: 복합키는 ORM에서 사용하기 복잡하다.

- **단점**
  1. **쓰기 성능 저하**: 복합키에 대하여 복합 인덱스를 적용하면 삽입/수정/삭제 성능이 감소할 수 있다. 복합키 인덱스 갱신은 단일 키 인덱스보다 더 많은 작업이 필요하기 때문이다. (복합 키 인덱스가 생성되어 검색 성능이 향상되는 반면, 삽입, 삭제, 갱신 작업에 대한 오버헤드는 증가할 수 있다.)
  2. **컬럼 순서에 따른 조회 성능 차이**: 인덱스 성능은 컬럼의 순서에 따라 달라진다.
  3. **제약 조건 변경시 PK 수정 필요**: 제약 조건의 변경은 PK 변경이라는 위험한 행위로 이어진다.


### 스프링에서 복합키 사용 방법

**주의**  
JPA는 복합키를 생성할 때 컬럼명의 알파벳 순으로 생성함. 카디널리티를 고려하여 인덱스를 명시적으로 설정하자(DML을 직접 이용하던지 등등).

1. **복합키 클래스를 생성한다.**

   ```java
   import java.io.Serializable;
   import javax.persistence.Embeddable;

   @Embeddable
   public class CompositeKey implements Serializable {
       private Long firstKey;
       private Long secondKey;

       // 기본 생성자
       public CompositeKey() {}

       // 생성자
       public CompositeKey(Long firstKey, Long secondKey) {
           this.firstKey = firstKey;
           this.secondKey = secondKey;
       }

       // equals() 및 hashCode() 메서드 구현
       @Override
       public boolean equals(Object o) {
           if (this == o) return true;
           if (!(o instanceof CompositeKey)) return false;
           CompositeKey that = (CompositeKey) o;
           return firstKey.equals(that.firstKey) && secondKey.equals(that.secondKey);
       }

       @Override
       public int hashCode() {
           return Objects.hash(firstKey, secondKey);
       }

       // Getter 및 Setter
       public Long getFirstKey() {
           return firstKey;
       }

       public void setFirstKey(Long firstKey) {
           this.firstKey = firstKey;
       }

       public Long getSecondKey() {
           return secondKey;
       }

       public void setSecondKey(Long secondKey) {
           this.secondKey = secondKey;
       }
   }
   ```
2. `@IdClass`를 사용하여 복합키 클래스를 엔티티 클래스에 추가한다.
   ```java
   import javax.persistence.*;

   @Entity
   @IdClass(CompositeKey.class)
   public class MyEntity {
      @Id
      private Long firstKey;

      @Id
      private Long secondKey;

      private String someField;

      // 기본 생성자
      public MyEntity() {}

      // 생성자
      public MyEntity(Long firstKey, Long secondKey, String someField) {
         this.firstKey = firstKey;
         this.secondKey = secondKey;
         this.someField = someField;
      }

      // Getter 및 Setter
      public Long getFirstKey() {
         return firstKey;
      }

      public void setFirstKey(Long firstKey) {
         this.firstKey = firstKey;
      }

      public Long getSecondKey() {
         return secondKey;
      }

      public void setSecondKey(Long secondKey) {
         this.secondKey = secondKey;
      }

      public String getSomeField() {
         return someField;
      }

      public void setSomeField(String someField) {
         this.someField = someField;
      }
   }

   ```

3. `@EmbeddedId`를 사용하는 방법
   `@IdClass` 대신 `@EmbeddedId`를 사용할 수도 있다.
   ```java
   import javax.persistence.*;

   @Entity
   public class MyEntity {
      @EmbeddedId
      private CompositeKey compositeKey;

      private String someField;

      // 기본 생성자
      public MyEntity() {}

      // 생성자
      public MyEntity(CompositeKey compositeKey, String someField) {
         this.compositeKey = compositeKey;
         this.someField = someField;
      }

      // Getter 및 Setter
      public CompositeKey getCompositeKey() {
         return compositeKey;
      }

      public void setCompositeKey(CompositeKey compositeKey) {
         this.compositeKey = compositeKey;
      }

      public String getSomeField() {
         return someField;
      }

      public void setSomeField(String someField) {
         this.someField = someField;
      }
   }

   ```

위 코드로 JPA에서 복합키를 설정할 수 있다. @IdClass 또는 @EmbeddedId를 사용하여 엔티티 클래스에서 복합키를 설정할 수 있으며, 필요한 경우 추가 설정(예: 인덱스 등)을 DDL로 관리할 수 있다.


## 결론 
---

1. ID 칼럼 사용이 유리한 경우
   1. 클라이언트 쪽에서 복합키의 정보를 모두 알지 않아도 간편하게 리소스 관리가 가능할때
   2. 식별가능한 복합키의 값이 변경 가능할 때
   3. 메타데이터를 다루는 등 특정 레코드에 대한 조회가 빈번하게 일어날때
2. 복합키 사용이 유리한 경우 
   1. 조회 로직이 명확하여 인덱스를 적용하기 쉬울 때
   2. 추가적인 메타데이터 없이 관계만을 파알할 때
   3. 쓰기에 비해 읽기 행위가 압도적으로 많은 경우

### 비교 표

| 구분                      | 장점                                                         | 단점                                                         |
|-------------------------|------------------------------------------------------------|------------------------------------------------------------|
| **복합 키만 사용하는 경우** | - 중복 방지: 두 외래 키를 결합하여 데이터의 유일성을 보장<br>- 테이블 간 간결한 관계: 불필요한 칼럼이 없어 테이블이 간단해짐<br>- 성능 최적화: 복합 키 인덱스를 사용해 검색 성능 향상  | - 참조 복잡성: 두 개의 키를 모두 사용해야 하므로 복잡해짐<br>- 확장성 한계: 메타데이터 추가 시 관리 어려움   |
| **별도의 ID 칼럼 추가**    | - 참조 용이성: 단일 ID로 참조 가능해 쿼리와 관리가 간단해짐<br>- 유연한 확장성: 추가 메타데이터 쉽게 추가 가능<br>- ORM 지원: ID가 있으면 각 관계를 별도의 엔티티로 관리 가능 | - 약간의 저장 공간 증가: ID 칼럼 추가로 공간 늘어남<br>- 유일성 추가 관리 필요: 중복 방지를 위한 제약 필요할 수 있음 |


## 참조
- [스프링 복합키 관리 전략](https://jiwon.oopy.io/1169c055-50a0-4ce8-8d67-6a0610babe5f)
- [복합키와 id PK 비교](https://prohannah.tistory.com/175)