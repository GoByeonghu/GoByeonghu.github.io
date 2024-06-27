---
layout: post
title: Spring Boot web
subtitle: Spring Boot로 web 기능을 사용하는 방법
categories:  
    - Spring
tags: [spring]
---

## Spring Boot Web

### build.gradle 설정

- dependencies 블럭
  
```groovy
implementation 'org.springframework.boot:spring-boot-starter-web'
```



## 사용 예시

### 기본 사용 법

- 코드예시

    - Main.java
  
    ```java
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class MySpringBootApplication {
        public static void main(String[] args) {
            SpringApplication.run(MySpringBootApplication.class, args);
        }
    }
    ```

    - HelloController.java
  
    ```java
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class HelloController {

        @GetMapping("/hello")
        public String sayHello() {
            return "안녕하세요, 스프링 부트!";
        }
    }
    ```

### 자주 사용되는 구조 (서비스 인터페이스, 서비스 구현체, 컨트롤러, 어플리케이션(Main))

- 프로젝트 구조
  
```
src
└── main
    ├── java
    │   └── com
    │       └── example
    │           └── calculator
    │               ├── CalculatorApplication.java
    │               ├── service
    │               │   ├── CalculatorService.java
    │               │   └── CalculatorServiceImpl.java
    │               └── controller
    │                   └── CalculatorController.java
    └── resources
        └── application.properties

```


#### Applicaation(Main)

<details>
  <summary>CalculatorApplication.java</summary>
  <div markdown="1">
  
  ```java
    package com.example.calculator;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class CalculatorApplication {
        public static void main(String[] args) {
            SpringApplication.run(CalculatorApplication.class, args);
        }
    }
  ```

  </div>
</details>

####  ServiceImpl

<details>
  <summary>CalculatorServiceImpl.java</summary>
  <div markdown="1">
  
  ```java
    package com.example.calculator.service;

    import org.springframework.stereotype.Service;

    @Service
    public class CalculatorServiceImpl implements CalculatorService{
        public int add(int a, int b) {
            return a + b;
        }

        public int subtract(int a, int b) {
            return a - b;
        }

        public int multiply(int a, int b) {
            return a * b;
        }

        public int divide(int a, int b) {
            if (b != 0) {
                return a / b;
            } else {
                throw new IllegalArgumentException("Cannot divide by zero");
            }
        }
    }
  ```

  </div>
</details>

####  Service

<details>
  <summary>CalculatorService.java</summary>
  <div markdown="1">
  
  ```java
    public interface CalculatorService {
        int add(int a, int b);
        int subtract(int a, int b);
        int multiply(int a, int b);
        int divide(int a, int b);
    }
  ```

  </div>
</details>

####  Controller

<details>
  <summary>CalculatorController.java</summary>
  <div markdown="1">
  
  ```java
    package com.example.calculator.controller;

    import com.example.calculator.service.CalculatorService;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class CalculatorController {
        private final CalculatorService calculatorService;

            @Autowired
        public CalculatorController(CalculatorService calculatorService) {
            this.calculatorService = calculatorService;
        }

        @GetMapping("/add")
        public int add(@RequestParam int a, @RequestParam int b) {
            return calculatorService.add(a, b);
        }

        @GetMapping("/subtract")
        public int subtract(@RequestParam int a, @RequestParam int b) {
            return calculatorService.subtract(a, b);
        }

        @GetMapping("/multiply")
        public int multiply(@RequestParam int a, @RequestParam int b) {
            return calculatorService.multiply(a, b);
        }

        @GetMapping("/divide")
        public int divide(@RequestParam int a, @RequestParam int b) {
            return calculatorService.divide(a, b);
        }
    }
  ```

  </div>
</details>

### 자주사용되는 예시2 (MVC를 적용한 경우)

####  프로젝트 구조
  
```
src
├── main
│   ├── java
│   │   └── com
│   │       └── example
│   │           └── myapp
│   │               ├── MyAppApplication.java
│   │               ├── controller
│   │               │   ├── UserController.java
│   │               │   └── PostController.java
│   │               ├── model
│   │               │   ├── User.java
│   │               │   └── Post.java
│   │               ├── repository
│   │               │   ├── UserRepository.java
│   │               │   └── PostRepository.java
│   │               ├── service
│   │               │   ├── UserService.java
│   │               │   ├── UserServiceImpl.java
│   │               │   ├── PostService.java
│   │               │   └── PostServiceImpl.java
│   └── resources
│       └── application.properties
└── test
    ├── java
    │   └── com
    │       └── example
    │           └── myapp
    │               ├── UserControllerTests.java
    │               └── PostControllerTests.java
    └── resources

```


####  레이어 별 예시코드


####  Application

<details>
  <summary>MyAppApplication.java</summary>
  <div markdown="1">
  
  ```java

    package com.example.myapp;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class MyAppApplication {
        public static void main(String[] args) {
            SpringApplication.run(MyAppApplication.class, args);
        }
    }

  ```

  </div>
</details>

####  Controller

<details>
  <summary>controller/UserController.java</summary>
  <div markdown="1">
  
  ```java

    package com.example.myapp.controller;

    import com.example.myapp.model.User;
    import com.example.myapp.service.UserService;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.web.bind.annotation.*;

    import java.util.List;

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

        @GetMapping("/{id}")
        public User getUserById(@PathVariable Long id) {
            return userService.getUserById(id);
        }

        @PostMapping
        public User createUser(@RequestBody User user) {
            return userService.createUser(user);
        }

        @PutMapping("/{id}")
        public User updateUser(@PathVariable Long id, @RequestBody User user) {
            return userService.updateUser(id, user);
        }

        @DeleteMapping("/{id}")
        public void deleteUser(@PathVariable Long id) {
            userService.deleteUser(id);
        }
    }

  ```

  </div>
</details>




<details>
  <summary>controller/PostController.java</summary>
  <div markdown="1">
  
  ```java

    package com.example.myapp.controller;

    import com.example.myapp.model.Post;
    import com.example.myapp.service.PostService;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.web.bind.annotation.*;

    import java.util.List;

    @RestController
    @RequestMapping("/api/posts")
    public class PostController {

        private final PostService postService;

        @Autowired
        public PostController(PostService postService) {
            this.postService = postService;
        }

        @GetMapping
        public List<Post> getAllPosts() {
            return postService.getAllPosts();
        }

        @GetMapping("/{id}")
        public Post getPostById(@PathVariable Long id) {
            return postService.getPostById(id);
        }

        @PostMapping
        public Post createPost(@RequestBody Post post) {
            return postService.createPost(post);
        }

        @PutMapping("/{id}")
        public Post updatePost(@PathVariable Long id, @RequestBody Post post) {
            return postService.updatePost(id, post);
        }

        @DeleteMapping("/{id}")
        public void deletePost(@PathVariable Long id) {
            postService.deletePost(id);
        }
    }


  ```

  </div>
</details>


####  model

<details>
  <summary>model/User.java</summary>
  <div markdown="1">
  
  ```java

    package com.example.myapp.model;

    import javax.persistence.*;

    @Entity
    public class User {

        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        private String name;
        private String email;

        // getters and setters
    }



  ```

  </div>
</details>

<details>
  <summary>model/Post.java</summary>
  <div markdown="1">
  
  ```java

    package com.example.myapp.model;

    import javax.persistence.*;

    @Entity
    public class Post {

        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        private String title;
        private String content;

        // getters and setters
    }




  ```

  </div>
</details>

####  repository

<details>
  <summary>repository/UserRepository.java</summary>
  <div markdown="1">
  
  ```java
    package com.example.myapp.repository;

    import com.example.myapp.model.User;
    import org.springframework.data.jpa.repository.JpaRepository;
    import org.springframework.stereotype.Repository;

    @Repository
    public interface UserRepository extends JpaRepository<User, Long> {
    }

  ```

  </div>
</details>

<details>
  <summary>repository/PostRepository.java</summary>
  <div markdown="1">
  
  ```java
    package com.example.myapp.repository;

    import com.example.myapp.model.Post;
    import org.springframework.data.jpa.repository.JpaRepository;
    import org.springframework.stereotype.Repository;

    @Repository
    public interface PostRepository extends JpaRepository<Post, Long> {
    }

  ```

  </div>
</details>

####  Service

<details>
  <summary>service/UserService.java</summary>
  <div markdown="1">
  
  ```java
    package com.example.myapp.service;

    import com.example.myapp.model.User;

    import java.util.List;

    public interface UserService {
        List<User> getAllUsers();
        User getUserById(Long id);
        User createUser(User user);
        User updateUser(Long id, User user);
        void deleteUser(Long id);
    }

  ```

  </div>
</details>


<details>
  <summary>service/UserServiceImpl.java</summary>
  <div markdown="1">
  
  ```java
    package com.example.myapp.service;

    import com.example.myapp.model.User;
    import com.example.myapp.repository.UserRepository;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Service;

    import java.util.List;

    @Service
    public class UserServiceImpl implements UserService {

        private final UserRepository userRepository;

        @Autowired
        public UserServiceImpl(UserRepository userRepository) {
            this.userRepository = userRepository;
        }

        @Override
        public List<User> getAllUsers() {
            return userRepository.findAll();
        }

        @Override
        public User getUserById(Long id) {
            return userRepository.findById(id).orElse(null);
        }

        @Override
        public User createUser(User user) {
            return userRepository.save(user);
        }

        @Override
        public User updateUser(Long id, User user) {
            User existingUser = userRepository.findById(id).orElse(null);
            if (existingUser != null) {
                existingUser.setName(user.getName());
                existingUser.setEmail(user.getEmail());
                return userRepository.save(existingUser);
            }
            return null;
        }

        @Override
        public void deleteUser(Long id) {
            userRepository.deleteById(id);
        }
    }

  ```

  </div>
</details>


<details>
  <summary>service/PostService.java</summary>
  <div markdown="1">
  
  ```java
    package com.example.myapp.service;

    import com.example.myapp.model.Post;

    import java.util.List;

    public interface PostService {
        List<Post> getAllPosts();
        Post getPostById(Long id);
        Post createPost(Post post);
        Post updatePost(Long id, Post post);
        void deletePost(Long id);
    }

  ```

  </div>
</details>


<details>
  <summary>service/PostServiceImpl.java</summary>
  <div markdown="1">
  
  ```java
    package com.example.myapp.service;

    import com.example.myapp.model.Post;
    import com.example.myapp.repository.PostRepository;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Service;

    import java.util.List;

    @Service
    public class PostServiceImpl implements PostService {

        private final PostRepository postRepository;

        @Autowired
        public PostServiceImpl(PostRepository postRepository) {
            this.postRepository = postRepository;
        }

        @Override
        public List<Post> getAllPosts() {
            return postRepository.findAll();
        }

        @Override
        public Post getPostById(Long id) {
            return postRepository.findById(id).orElse(null);
        }

        @Override
        public Post createPost(Post post) {
            return postRepository.save(post);
        }

        @Override
        public Post updatePost(Long id, Post post) {
            Post existingPost = postRepository.findById(id).orElse(null);
            if (existingPost != null) {
                existingPost.setTitle(post.getTitle());
                existingPost.setContent(post.getContent());
                return postRepository.save(existingPost);
            }
            return null;
        }

        @Override
        public void deletePost(Long id) {
            postRepository.deleteById(id);
        }
    }

  ```

  </div>
</details>