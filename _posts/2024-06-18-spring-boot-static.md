---
layout: post
title: Spring Boot static 파일 제공하기
subtitle: Spring Boot에서 정적파일 업로드, 다운로드 하기
categories: 
  - Spring
tags: [spring]
---

## Spring Boot에서 정적 파일 제공하기

### 기본 구조

Spring Boot는 `src/main/resources/static` 디렉토리를 통해 정적 리소스를 제공한다. 디렉토리에 파일을 저장하면 HTTP 요청을 통해 접근할 수 있다. 예를 들어, `static/images` 디렉토리에 이미지를 저장하면 URL로 접근할 수 있다.

#### 디렉토리 구조
```
src
├── main
│   ├── java
│   └── resources
│       ├── static
│       │   └── images
│       │       └── your-image.jpg
│       └── application.properties
└── test
```

예시: `http://localhost:8080/images/your-image.jpg`로 접근 가능하다.

#### 컨트롤러를 통한 정적 리소스 반환
컨트롤러를 사용하면 정적 리소스를 더 유연하게 반환할 수 있다.

```java
package com.example.myapp.controller;

import org.springframework.core.io.Resource;
import org.springframework.core.io.UrlResource;
import org.springframework.http.HttpHeaders;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import java.net.MalformedURLException;
import java.nio.file.Path;
import java.nio.file.Paths;

@RestController
@RequestMapping("/api/images")
public class ImageController {

    private final Path fileStorageLocation = Paths.get("uploads").toAbsolutePath().normalize();

    @GetMapping("/{fileName:.+}")
    public ResponseEntity<Resource> getImage(@PathVariable String fileName) {
        try {
            Path filePath = fileStorageLocation.resolve(fileName).normalize();
            Resource resource = new UrlResource(filePath.toUri());
            if (resource.exists()) {
                return ResponseEntity.ok()
                        .header(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=\"" + resource.getFilename() + "\"")
                        .body(resource);
            } else {
                return ResponseEntity.notFound().build();
            }
        } catch (MalformedURLException e) {
            return ResponseEntity.badRequest().build();
        }
    }
}
```

### 설정 파일 수정

`application.properties` 파일에서 정적 리소스의 경로와 URL 패턴을 설정할 수 있다.

- `spring.mvc.static-path-pattern=/static/**`: 요청 URL 패턴을 정의한다.
- `spring.resources.static-locations=classpath:/static/,file:./uploads/`: 정적 리소스를 제공할 위치를 설정한다.

#### 예시
```
spring.mvc.static-path-pattern=/static/**
spring.resources.static-locations=classpath:/static/,file:./uploads/
```

이 설정은 정적 리소스를 두 위치에서 제공한다:
1. `classpath:/static/` (JAR 내부의 static 디렉토리, resource 내부의 폴더이다.)
2. `file:./uploads/` (파일 시스템의 uploads 디렉토리, 내가 지정한 임의에 폴더로 c폴더 기준 상대경로를 의미한다.)
  
> `file:` 로 외부 디렉토리 만들어야 서버jar에 종속되지않는다.
> `classpath:`를 이용하여 resource/static 경로를 이용해 버리면 서버에 종속됨으로 이미지 저장하고 반영하려면 서버를 다시 시작(빌드)해야 새로운 파일이 적용된다.(jar 다시 만들어야한다) 
> 단, 같은 이름의 파일이 있으면 맨 먼저 언급한 file 혹은 classpath에서 찾는다.

### Spring Boot 2.4 변경사항

Spring Boot 2.4부터 정적 리소스 관련 설정이 `spring.resources`에서 `spring.web.resources`로 변경되었다. 따라서 2.4 이상에서는 아래 설정을 사용해야 한다.
```
spring.web.resources.static-locations=classpath:/static/,file:./uploads/
```

### 이미지 업로드 시 유의사항

기본적으로 Spring Boot의 파일 업로드 제한은 1MB이다. 이를 늘리려면 다음 설정을 추가해야 한다.

```
# 파일 크기 제한 설정
spring.servlet.multipart.maxFileSize=10MB
spring.servlet.multipart.maxRequestSize=10MB
```

### 요약

1. 기본적으로 `src/main/resources/static` 디렉토리를 통해 정적 리소스를 제공한다.
2. 컨트롤러를 사용해 동적으로 리소스를 제공할 수 있다.
3. 설정 파일에서 정적 리소스 경로와 URL 패턴을 정의할 수 있다.
4. Spring Boot 2.4 이상에서는 설정 키가 변경되었다.
5. 업로드 파일 크기 제한을 적절히 설정해야 한다.
