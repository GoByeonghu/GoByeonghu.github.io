---
layout: post
title: Spring Boot JPA
subtitle: Spring Boot JPA 사용하기
categories: 
    - Spring
tags: [spring]
---

## 설정

- 루트 계정 접속

```

```

- 유저 생성 및 권한 설정

```

```

- db 로그인 테스트



## 사용

- bulid.gradle

```
```


- 롬복
implementation 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'


- model


import javax.persistence.EntityManager;
이 아래로 바뀜
import jakarta.persistence.*;


-디비에서 필드값 자동 생성 되어있는경우 스프링에서 이를 인지하게 해줌
@GeneratedValue(strategy = GenerationType.IDENTITY)


-Spring Data JPA는 CRUD (Create, Read, Update, Delete) 기능을 기본적으로 제공하는 JpaRepository 인터페이스를 통해 쉽게 리포지토리를 생성할 수 있습니다.

이렇게 primary key가 아닌 필드로 값을 찾는 매서드는 따로 리파지토리에 선언을 해줘야 spring data jpa에서 해당 함수를 사용할수있는가?
ChatGPT
네, Spring Data JPA에서 primary key가 아닌 필드로 값을 찾는 메소드를 사용하려면 해당 메소드를 리포지토리에 선언해줘야 합니다. Spring Data JPA는 메소드 이름을 기반으로 쿼리를 자동 생성하는데, 이 기능을 사용하려면 리포지토리 인터페이스에 메소드를 선언해야 합니다.

@Query 어노테이션 사용: 커스텀 쿼리를 작성하고 싶다면 @Query 어노테이션을 사용할 수 있습니다. 예를 들어:

java
코드 복사
@Query("SELECT u FROM User u WHERE u.nickname = :nickname")
User findByNickname(@Param("nickname") String nickname);

Optional<> 을 사용하면 .orElse(null);를 사용할수있다.

기본 제공 함수는 기본키로인한 검색만 제공하므로 ㄱ 이외의 기능이 필요하다면 인터페이스 안에 명시해야한다.
해당 함수명을 기반으로 함수내용을 작성해준다.

기본제공 함수는 모두 optional형태이다.


- 리파지토리

리파지토리에서 spring data을 사용하면 굳이 함수 선언 안해도 사용가능하다
하지만 하는 경우도 있는데 주로 아래의 이유이다.

Spring Data JPA는 메서드 이름을 기반으로 쿼리를 자동으로 생성하는 기능을 제공하므로, findByPostId(Long postId)와 같은 메서드는 실제로 선언하지 않아도 사용할 수 있습니다. 선언하지 않아도 Spring Data JPA가 메서드 이름을 파싱하여 자동으로 구현해 줍니다. 따라서, 명시적으로 선언할 필요는 없습니다.

이러한 메서드를 명시적으로 선언한 이유는 다음과 같습니다:

명시적 설명: 코드 읽는 사람이 이 리포지토리가 어떤 메서드를 제공하는지 명확하게 알 수 있습니다.
코드 완성 지원: IDE에서 코드 자동 완성 기능을 사용할 때 유용할 수 있습니다.
문서화: 리포지토리 인터페이스 자체가 어떤 쿼리 메서드를 제공하는지에 대한 문서 역할을 합니다.

- 서비스

Optional<Post>를 반환 타입으로 사용하는 이유는 Post 엔티티가 존재하지 않을 수 있기 때문입니다. Optional을 사용하면 반환 값이 존재하지 않을 경우를 안전하게 처리할 수 있으며, null을 반환하는 것보다 명시적인 방법입니다. 이는 null로 인한 NullPointerException을 방지하는 데 도움이 됩니다.

그러나 반환 값을 Post로 변경하려면, 존재하지 않는 경우에 대한 처리를 서비스 계층에서 직접 구현해야 합니다.


- 컨트롤러


@PathVariable("nickname") String nickname
@RequestParam("target") String target,

@RequestBody User user

@RequestPart("userData") User user,                           
@RequestPart(value = "profile_image", required = false) MultipartFile profileImage

MultipartFile-> 이미지를 임시경로에 저장해두고 호출되면 메모리에 불러서 쓴다.

@PostMapping(consumes = "multipart/form-data") 이렇게 해도 되지만 consumes = "multipart/form-data" 가 없어도 되던데?
ChatGPT
consumes = "multipart/form-data" 어노테이션을 명시적으로 지정하지 않아도 Spring이 요청의 Content-Type을 자동으로 감지하고 적절하게 처리할 수 있는 이유는 Spring MVC의 기본 설정과 요청 매핑 처리 방식 때문입니다.

기본 동작 방식
Spring MVC는 다양한 Content-Type을 지원하며, 요청의 Content-Type을 기반으로 자동으로 적절한 HttpMessageConverter를 선택합니다. 아래는 이 동작 방식을 설명합니다:

HttpMessageConverter 자동 등록: Spring은 MultipartFile을 처리하기 위해 MultipartHttpMessageConverter를 자동으로 등록합니다. 이 변환기는 multipart/form-data 요청을 처리할 수 있습니다.
자동 Content-Type 감지: Spring은 요청의 Content-Type 헤더를 기반으로 적절한 HttpMessageConverter를 자동으로 선택합니다. 따라서, consumes = "multipart/form-data"를 명시하지 않아도, 요청이 multipart/form-data 형식이라면 Spring은 이를 자동으로 감지하고 처리할 수 있습니다.
명시적 설정의 장점
그러나, consumes = "multipart/form-data"를 명시적으로 지정하는 것은 여전히 유용할 수 있습니다:

문서화: 이 설정은 메서드가 어떤 유형의 요청을 처리할 수 있는지 명확하게 문서화해줍니다.
명확한 의도 표현: 명시적으로 설정함으로써 해당 메서드가 오직 multipart/form-data 요청만을 처리하도록 의도를 명확히 할 수 있습니다.
확장성: 추후 코드가 확장되거나 변경될 때, 명시적으로 지정된 Content-Type은 코드의 의도를 명확히 하고 유지보수를 용이하게 합니다.


시리얼라이저로  Post post = objectMapper.readValue(postData, Post.class); 이렇게 해도 된다.



Spring Boot와 같은 프레임워크에서는 Java 객체를 JSON으로 직렬화하고 이를 HTTP 응답으로 반환하는 과정을 자동으로 처리합니다. 이를 가능하게 하는 주요 요소는 Spring Boot의 Jackson 라이브러리입니다. 아래에서 그 과정에 대해 자세히 설명하겠습니다.

1. **Spring Boot와 Jackson**  
Spring Boot는 기본적으로 Jackson이라는 라이브러리를 사용하여 Java 객체를 JSON 형식으로 직렬화합니다. Jackson은 `ObjectMapper`라는 클래스를 사용하여 이 작업을 수행합니다.

2. **@RestController와 @ResponseBody**  
`@RestController` 어노테이션은 `@Controller`와 `@ResponseBody` 어노테이션이 결합된 형태입니다. 이는 컨트롤러가 JSON 응답을 반환할 수 있게 합니다.

    - `@ResponseBody`: 메서드에서 반환된 객체를 HTTP 응답 본문으로 변환합니다. Spring은 반환된 객체를 Jackson을 사용하여 JSON으로 변환합니다.

3. **Spring Boot의 자동 설정**  
Spring Boot는 Jackson과 같은 라이브러리를 자동으로 구성하여 사용할 수 있게 해줍니다. 별도의 설정 없이도 Jackson 라이브러리가 classpath에 존재하면 Spring Boot가 이를 자동으로 설정하여 사용합니다.

4. **예시 코드 분석**

    ```java
    @RestController
    @RequestMapping("/api/users")
    public class UserController {

        private final UserService userService;

        @Autowired
        public UserController(UserService userService) {
            this.userService = userService;
        }

        @GetMapping
        public List<User> getAllUsers() {
            return userService.getAllUsers();
        }

        @GetMapping("/{nickname}")
        public User getUserByNickname(@PathVariable("nickname") String nickname) {
            return userService.getUserByNickname(nickname);
        }

        @PostMapping
        public User createUser(@RequestPart("userData") User user,
                               @RequestPart(value = "profile_image", required = false) MultipartFile profileImage) {
            return userService.createUser(user);
        }

        @PutMapping("/{id}")
        public User updateUser(@PathVariable String id, @RequestBody User user) {
            return userService.updateUser(id, user);
        }

        @DeleteMapping("/{id}")
        public void deleteUser(@PathVariable String id) {
            userService.deleteUser(id);
        }
    }
    ```

    위 코드에서 `getAllUsers()`, `getUserByNickname(String nickname)`, `createUser(User user, MultipartFile profileImage)`, `updateUser(String id, User user)` 메서드들은 모두 `User` 또는 `List<User>` 객체를 반환합니다. 이 객체들은 Jackson에 의해 JSON으로 자동 직렬화됩니다.

**요약**

- **Jackson 사용**: Spring Boot는 Jackson 라이브러리를 사용하여 Java 객체를 JSON으로 직렬화합니다.
- **@RestController와 @ResponseBody**: 이 어노테이션들은 컨트롤러 메서드가 반환하는 객체를 JSON 응답으로 변환합니다.
- **자동 구성**: Spring Boot는 Jackson을 자동으로 구성하여 별도의 설정 없이도 JSON 직렬화를 가능하게 합니다.

이렇게 해서 반환값이 `User` 클래스임에도 불구하고 JSON으로 직렬화하여 응답을 보낼 수 있습니다.



-- 응답 커스텀 하는 방법

Spring Boot에서 커스텀 JSON 응답을 반환하고, HTTP 상태 코드를 변경하고자 할 때는 ResponseEntity를 사용하면 됩니다. ResponseEntity를 사용하면 HTTP 응답의 본문, 상태 코드, 헤더 등을 모두 제어할 수 있습니다. 이를 통해 반환값을 커스텀한 JSON 형식으로 바꾸고, 원하는 메시지와 상태 코드를 추가할 수 있습니다.

다음은 예제입니다:

1. 사용자 정의 응답 객체 만들기
먼저 사용자 정의 응답 객체를 정의합니다. 이 객체는 원하는 메시지를 포함할 수 있습니다.

java
코드 복사
package com.sfz.rest_api.dto;

public class CustomResponse {
    private String message;
    private Object data;

    public CustomResponse(String message, Object data) {
        this.message = message;
        this.data = data;
    }

    // Getters and Setters
    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }

    public Object getData() {
        return data;
    }

    public void setData(Object data) {
        this.data = data;
    }
}
2. 컨트롤러 메서드 수정
컨트롤러 메서드를 수정하여 ResponseEntity를 사용하도록 합니다.

java
코드 복사
package com.sfz.rest_api.controller;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import org.springframework.beans.factory.annotation.Autowired;
import java.util.List;

import com.sfz.rest_api.model.User;
import com.sfz.rest_api.service.UserService;
import com.sfz.rest_api.dto.CustomResponse;
import org.springframework.web.multipart.MultipartFile;

@RestController
@RequestMapping("/api/users")
public class UserController {

    private final UserService userService;

    @Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }

    @GetMapping("/{nickname}")
    public ResponseEntity<CustomResponse> getUserByNickname(@PathVariable("nickname") String nickname) {
        User user = userService.getUserByNickname(nickname);
        
        // 원하는 커스텀 메시지를 포함한 응답 생성
        CustomResponse response = new CustomResponse("User found successfully", user);
        
        // ResponseEntity를 사용하여 상태 코드와 함께 응답 반환
        return new ResponseEntity<>(response, HttpStatus.OK);
    }

    @PostMapping
    public ResponseEntity<CustomResponse> createUser(@RequestPart("userData") User user,
                           @RequestPart(value = "profile_image", required = false) MultipartFile profileImage) {
        User createdUser = userService.createUser(user);
        
        // 원하는 커스텀 메시지를 포함한 응답 생성
        CustomResponse response = new CustomResponse("User created successfully", createdUser);
        
        // ResponseEntity를 사용하여 상태 코드와 함께 응답 반환
        return new ResponseEntity<>(response, HttpStatus.CREATED);
    }

    @PutMapping("/{id}")
    public ResponseEntity<CustomResponse> updateUser(@PathVariable String id, @RequestBody User user) {
        User updatedUser = userService.updateUser(id, user);
        
        // 원하는 커스텀 메시지를 포함한 응답 생성
        CustomResponse response = new CustomResponse("User updated successfully", updatedUser);
        
        // ResponseEntity를 사용하여 상태 코드와 함께 응답 반환
        return new ResponseEntity<>(response, HttpStatus.OK);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<CustomResponse> deleteUser(@PathVariable String id) {
        userService.deleteUser(id);
        
        // 원하는 커스텀 메시지를 포함한 응답 생성
        CustomResponse response = new CustomResponse("User deleted successfully", null);
        
        // ResponseEntity를 사용하여 상태 코드와 함께 응답 반환
        return new ResponseEntity<>(response, HttpStatus.NO_CONTENT);
    }
}
요약
CustomResponse 클래스: 메시지와 데이터를 포함하는 사용자 정의 응답 객체를 생성합니다.
ResponseEntity 사용: ResponseEntity를 사용하여 응답 본문, 상태 코드 등을 설정합니다.
커스텀 메시지 추가: 응답 객체에 원하는 메시지를 추가하여 반환합니다.
이렇게 하면 각 메서드에서 반환값을 커스텀한 JSON 형식으로 바꿀 수 있고, 상태 코드도 원하는 대로 설정할 수 있습니다.


https://velog.io/@hans4257/jsp-MySQL-%EC%97%B0%EB%8F%99-%EC%98%A4%EB%A5%98java.lang.ClassNotFoundException-com.mysql.jdbc.Driver




※ 순환 참조(Circular reference) 해결법
1. @JsonIgnore 
: 이 어노테이션을 붙이면 JSON 데이터에 해당 프로퍼티는 null로 들어가게 된다. 
즉, 데이터에 아예 포함시키지 않는다.
 
2. @JsonManagedReference 와 @JsonBackReference
: 부모 클래스(Posts entity)의 Comment 필드에 @JsonManagedReference를, 자식 클래스(Comment entity)의 Posts 필드에 @JsonBackReference를 추가해주면 순환 참조를 막을 수 있다.
 
3.@JsonIgnoreProperties
: 부모 클래스(Posts entity)의 Comment 필드에 @JsonIgnoreProperties({"posts"}) 를 붙여주면 순환 참조를 막을 수 있다.
 
4. DTO 사용
: 위와 같은 상황이 발생하게된 주원인은 '양방향 매핑'이기도 하지만, 더 정확하게는 Entity 자체를 response로 리턴한데에 있다. entity 자체를 return 하지 말고, DTO 객체를 만들어 필요한 데이터만 옮겨담아 Client로 리턴하면 순환 참조 관련 문제는 애초에 방지 할 수 있다.
 
5. 매핑 재설정
: 양방향 매핑이 꼭 필요한지 다시 한번 생각해볼 필요가 있다. 만약 양쪽에서 접근할 필요가 없다면 단방향 매핑을 해줘서 자연스레 순환 참조 문제를 해결하자.
 
필자는 DTO 사용으로 순환 참조 문제를 해결했다.
이번을 계기로 DTO를 사용해야 하는 이유를 하나 더 알게되었다.
출처: https://dev-coco.tistory.com/133 [슬기로운 개발생활:티스토리]