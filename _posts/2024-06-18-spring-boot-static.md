---
layout: post
title: Spring Boot static 파일 제공하기
subtitle: Spring Boot에서 정적파일 업로드, 다운로드 하기
categories: 
  - Spring
tags: [spring]
---

### 기본 구조

Spring Boot에서 정적 정적 리소스를 제공하는 기본적인 방법은 `src/main/resources/static` 디렉토리를 사용하는 것이다. 
Spring Boot는 기본적으로 이 디렉토리에 있는 파일을 자동으로 정적 리소스로 제공할 수 있다.

1. src/main/resources/static 디렉토리 사용
Spring Boot는 src/main/resources/static 디렉토리의 콘텐츠를 기본적으로 정적 리소스로 제공한다. 
예를 들어, src/main/resources/static/images 디렉토리에 이미지를 저장하면, 해당 이미지는 http://localhost:8080/images/your-image.jpg와 같은 URL로 접근할 수 있다.

- 디렉토리 구조

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

이 디렉토리 구조를 사용하면 your-image.jpg 파일은 http://localhost:8080/images/your-image.jpg URL을 통해 접근할 수 있다.

2. Controller를 사용하여 정적 리소스 반환
정적 리소스를 제공하는 또 다른 방법은 컨트롤러를 통해 이미지를 반환하는 것이다.

- 컨트롤러 예제

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

### 세부 구현 방법

1. 설정 파일 수정
   
application.properties 파일에 정적 리소스 위치를 설정할 수 있다.

- application.properties
```
spring.mvc.static-path-pattern=/static/**
spring.resources.static-locations=classpath:/static/,file:./uploads/
```

이 설정을 통해 정적 리소스는 /static/** 경로 패턴으로 접근할 수 있으며, 파일 시스템의 uploads 디렉토리에서도 정적 리소스를 제공할 수 있다.



### 요약

- 기본 정적 디렉토리 사용: src/main/resources/static 디렉토리의 파일은 기본적으로 정적 리소스로 제공된다.
- 컨트롤러 사용: 더 많은 제어가 필요한 경우 컨트롤러를 사용하여 정적 리소스를 반환할 수 있다.
- 설정 파일 수정: application.properties 파일을 통해 정적 리소스의 경로를 설정할 수 있다.




1. `spring.mvc.static-path-pattern=/static/**`

    이 설정은 Spring MVC에서 정적 리소스를 제공할 URL 경로 패턴을 정의한다.
    - `/static/**`: 이 경로 패턴은 `/static/` 경로 아래의 모든 하위 경로를 포함한다.
    - 정적 리소스 URL: 클라이언트가 http://localhost:8080/static/your-image.jpg와 같은 URL로 정적 리소스에 접근할 수 있게 한다.
  
2. `spring.resources.static-locations=classpath:/static/,file:./uploads/`
이 설정은 Spring Boot가 정적 리소스를 어디에서 찾을지를 정의한다. 여러 위치를 지정할 수 있으며, 콤마로 구분한다.
- `classpath:/static/`: 이는 JAR 파일이나 클래스패스 내의 static 디렉토리를 가리킨다. 즉, `src/main/resources/static` 디렉토리 내의 파일을 의미한다. 이 경로는 배포 시 JAR 파일 안에 포함된다.
- `file:./uploads/`: 이는 파일 시스템의 현재 디렉토리(애플리케이션이 실행되는 디렉토리) 아래의 uploads 디렉토리를 가리킨다.
예를 들어, 애플리케이션이 /home/user/myapp에서 실행되고 있다면, uploads 디렉토리는 /home/user/myapp/uploads에 위치하게 된다.

-**요약**

클라이언트는 http://localhost:8080/static/your-image.jpg와 같은 URL을 통해 정적 리소스에 접근할 수 있다.
Spring Boot는 정적 리소스를 두 곳에서 찾는다:
JAR 파일 내의 static 디렉토리 (classpath:/static/)
파일 시스템의 현재 디렉토리 내의 uploads 디렉토리 (file:./uploads/)


- 예제

JAR 파일 내의 정적 리소스:
예를 들어, src/main/resources/static/images/logo.png 파일이 있다면, 이 파일은 classpath:/static/ 경로에 포함된다.
클라이언트는 http://localhost:8080/static/images/logo.png URL을 통해 이 파일에 접근할 수 있다.

파일 시스템의 정적 리소스:
애플리케이션이 실행되는 디렉토리에 uploads 디렉토리가 있고, 그 안에 profile.jpg 파일이 있다면, 이 파일은 file:./uploads/ 경로에 포함된다.
클라이언트는 http://localhost:8080/static/profile.jpg URL을 통해 이 파일에 접근할 수 있다.
이 설정을 통해 Spring Boot 애플리케이션은 두 가지 위치에서 정적 리소스를 제공할 수 있으며, 경로 패턴을 통해 클라이언트가 정적 리소스에 접근할 수 있도록 한다다.

- **두 설정의 차이점**

1. `spring.mvc.static-path-pattern=/static/**`
이 설정은 클라이언트가 정적 리소스에 접근할 URL 경로 패턴을 정의한다.
이 설정이 없이도 기본 경로 패턴은 잘 작동하지만, 경로 패턴을 변경하거나 커스터마이징해야 하는 경우에만 필요하다.
기본 경로 패턴은 /로 설정되어 있기 때문에 이 설정을 생략할 수 있다.

1. `spring.resources.static-locations=classpath:/static/,file:./uploads/`
이 설정은 Spring Boot가 정적 리소스를 어디에서 찾을지를 정의한다.
두 위치를 설정한 이유는 클래스패스 내의 정적 리소스와 파일 시스템의 정적 리소스를 모두 제공하기 위함이다.

만약 한 위치만 사용하려면:

클래스패스 내의 정적 리소스만 사용:

`spring.resources.static-locations=classpath:/static/`
이 경우, src/main/resources/static 디렉토리에 있는 정적 리소스만 제공된다.
예를 들어, src/main/resources/static/images/logo.png 파일이 있다면, http://localhost:8080/images/logo.png로 접근할 수 있다.

파일 시스템의 정적 리소스만 사용:

`spring.resources.static-locations=file:./uploads/`
이 경우, 파일 시스템의 현재 디렉토리 아래의 uploads 디렉토리에 있는 정적 리소스만 제공된다. 
예를 들어, 애플리케이션이 /home/user/myapp에서 실행되고 있고, uploads/profile.jpg 파일이 있다면, http://localhost:8080/profile.jpg로 접근할 수 있다.


### 요약

#### spring.mvc.static-path-pattern=/static/**

요청 url 패턴을 명시

#### spring.resources.static-locations=classpath:/static/,file:./uploads/

정적파일 제공장소를 명시

- classpath는 resource 내부의 폴더(즉 jar내부의 폴더이다)
- file는 내가 지정한 임의에 폴더(src폴더 기준 상대경로)
  
>file: 로 외부 디렉토리 만들어야 서버jar에 종속되지않는다.
>resource/static 이용해 버리면 서버에 종속됨으로 이미지 저장하고 반영하려면 서버 다시시작해야한다.(jar 다시 만들어야한다) 단, 같은 이름의 파일이 있으면 맨 먼저 언급한 곳에서만 찾는다.


### Spring Boot 2.4의 변경사항

`Spring Boot 2.4`부터는
정적 리소스 관련 설정이 spring.resources에서 spring.web.resources로 변경되었다.
따라서, 이전 버전에서는 spring.resources.static-locations를 사용했지만, 
**2.4 이상에서는 spring.web.resources.static-locations를 사용해야한다.**



### 이미지 업로드 구현시 주의사항

#### 주의 : 파일사이즈 디폴트 한계는 1MB밖에 안된다.

아래처럼 설정해주어 업로드 리밋을 조정한다.

```
#file size
spring.servlet.multipart.maxFileSize=10MB
spring.servlet.multipart.maxRequestSize=10MB
```

