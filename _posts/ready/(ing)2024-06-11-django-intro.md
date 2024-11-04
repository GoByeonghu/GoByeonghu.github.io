---
layout: post
title: Django Intro
subtitle: Django를 시작해 보자
categories: 
  - Django
tags: [nodejs, express, mysql]
---

# Django의 동작 원리

Django는 파이썬으로 작성된 고급 웹 프레임워크로, 신속한 개발과 간결한 코드 작성을 목표로 한다. Django의 아키텍처는 MTV(Model-Template-View) 패턴을 기반으로 하며, 이 문서에서는 Django의 동작 원리를 상세히 설명한다.

## 1. Django 아키텍처

Django의 아키텍처는 다음 세 가지 주요 구성 요소로 나뉜다:

- **모델(Model)**: 데이터베이스의 구조를 정의하고, 데이터베이스와의 상호작용을 담당하는 부분이다. Django의 ORM(Object-Relational Mapping)을 사용하여 데이터베이스 쿼리를 작성하지 않고도 데이터베이스 조작이 가능하다.
  
- **템플릿(Template)**: 사용자에게 표시할 HTML을 구성하는 부분이다. Django는 템플릿 엔진을 통해 동적인 웹 페이지를 생성하는 기능을 제공한다.

- **뷰(View)**: 사용자의 요청을 처리하고 적절한 응답을 반환하는 역할을 한다. 뷰는 모델에서 데이터를 가져오고, 템플릿에 데이터를 전달하여 최종 사용자에게 HTML을 렌더링한다.

## 2. 요청과 응답 처리 흐름

Django의 동작 과정은 다음과 같은 요청과 응답 처리 흐름으로 진행된다.

### 2.1. HTTP 요청 수신

사용자가 웹 브라우저를 통해 URL에 접근하면, Django 애플리케이션의 WSGI(Web Server Gateway Interface) 서버가 HTTP 요청을 수신한다. 이 요청은 Django의 `urls.py` 파일에 정의된 URL 패턴과 매칭되어 적절한 뷰로 전달된다.

### 2.2. URL 라우팅

Django는 요청된 URL을 `urls.py` 파일에 정의된 URLConf에 따라 분석한다. URLConf는 요청된 URL에 매핑된 뷰를 찾는 역할을 한다. 예를 들어, 사용자가 `/articles/`라는 URL에 접근하면, Django는 해당 URL에 매핑된 뷰 함수를 호출한다.

### 2.3. 뷰 처리

뷰 함수는 사용자의 요청을 처리하고 필요한 데이터를 모델에서 가져온다. 이 과정은 다음과 같다:

1. **요청 데이터 처리**: 뷰 함수는 `HttpRequest` 객체를 통해 요청 데이터를 처리한다. GET, POST 등의 요청 방식을 구분하여 필요한 데이터를 추출할 수 있다.

2. **모델 상호작용**: 뷰는 필요한 정보를 데이터베이스에서 가져오기 위해 모델을 사용한다. Django의 ORM을 통해 간편하게 쿼리를 작성하고, 데이터베이스로부터 결과를 반환받는다.

3. **템플릿 렌더링**: 뷰는 데이터를 템플릿에 전달하여 HTML 페이지를 생성한다. Django는 `render` 함수를 사용하여 템플릿과 데이터를 결합하고 최종 HTML을 생성한다.

### 2.4. HTTP 응답 반환

뷰에서 생성된 HTML은 HTTP 응답 객체로 포장되어 사용자에게 반환된다. Django는 `HttpResponse` 객체를 통해 최종 결과를 전달하며, 이 과정에서 HTTP 상태 코드, 헤더, 본문 등을 설정할 수 있다.

## 3. 데이터베이스와 ORM

Django는 객체 관계 매핑(Object-Relational Mapping) 기술을 통해 데이터베이스와의 상호작용을 간소화한다. ORM을 사용하면 SQL 쿼리를 작성하지 않고도 데이터베이스 작업을 수행할 수 있다. 모델은 파이썬 클래스 형태로 정의되며, Django가 자동으로 SQL 쿼리를 생성하여 데이터베이스에 접근한다.

### 3.1. 모델 정의

모델 클래스는 `django.db.models.Model`을 상속받아 정의된다. 각 속성은 데이터베이스의 필드를 나타내며, Django의 다양한 필드 유형을 사용하여 데이터 구조를 설정할 수 있다.

```python
from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    published_date = models.DateTimeField(auto_now_add=True)
```

### 3.2. 마이그레이션

모델을 정의한 후, makemigrations 명령어를 사용하여 마이그레이션 파일을 생성하고, migrate 명령어를 통해 데이터베이스에 반영한다. 이 과정은 모델의 구조를 데이터베이스와 동기화하는 역할을 한다.

## 4. 관리자 인터페이스

Django는 기본적으로 제공하는 관리자 인터페이스를 통해 모델 데이터를 쉽게 관리할 수 있다. 모델을 등록하면 자동으로 CRUD(Create, Read, Update, Delete) 기능이 제공된다.

```python
from django.contrib import admin
from .models import Article

admin.site.register(Article)
```

## 5. 중간웨어(Middleware)

Django는 요청과 응답 사이에 중간웨어를 사용하여 특정 작업을 수행할 수 있다. 중간웨어는 요청 처리 흐름의 각 단계에서 추가적인 기능을 구현할 수 있는 유연한 구조를 제공한다. 예를 들어, 인증, 세션 관리, CORS 처리 등의 기능을 중간웨어로 구현할 수 있다.

### 5.1. 중간웨어 구성

중간웨어는 파이썬 클래스로 구현되며, `__init__`, `__call__`, `process_request`, `process_response` 등의 메서드를 오버라이드하여 동작한다. 각 중간웨어는 요청이 들어오고 응답이 반환되기 전에 수행할 작업을 정의할 수 있다. Django 설정 파일의 `MIDDLEWARE` 리스트에 중간웨어를 추가하여 활성화할 수 있다.

### 5.2. 중간웨어의 예시

중간웨어의 대표적인 예로는 다음과 같은 기능들이 있다:

- **세션 관리**: 사용자의 세션을 유지하고 관리하는 기능을 제공한다.
  
- **사용자 인증**: 요청이 들어올 때 사용자의 인증 정보를 확인하고, 인증되지 않은 사용자를 적절히 처리하는 기능을 제공한다.
  
- **CORS 처리**: Cross-Origin Resource Sharing(CORS) 정책을 설정하여 외부 도메인에서의 요청을 관리할 수 있다.

- **로깅**: 요청 및 응답 정보를 로깅하여 디버깅 및 모니터링에 활용할 수 있다.

중간웨어는 요청 처리 과정에서 매우 유용하게 사용되며, 기능을 모듈화하고 재사용성을 높이는 데 기여한다.




## 징고의 비동기 처리 문제

### 데이터 베이스 비동기 처리

asgi로 비동기 처리를 요청하더라도 데이터베이스 접근이 동기처리라면 큰의미가 없다.
이를 위해 데이터 베이스 비동기 처리 방식을 알아보자

장고는 기본적으로 동기 방식으로 데이터베이스에 접근하는 ORM(Object-Relational Mapping) 프레임워크이다. 그러나 장고 3.1 버전부터 비동기 기능이 추가되었으며, 이를 통해 비동기 뷰에서 비동기적으로 데이터베이스와 상호작용할 수 있는 가능성이 열렸다. 이 글에서는 장고의 비동기 데이터베이스 접근 방식과 함께 MySQL과 PostgreSQL의 비교를 통해 각 데이터베이스에서의 비동기 처리 차이를 설명하고자 한다.

### 장고의 비동기 기능 개요

장고 3.1부터 비동기 뷰와 비동기 미들웨어를 지원하게 되었으며, 이를 통해 비동기적으로 HTTP 요청을 처리할 수 있다. 비동기 프로그래밍을 통해 요청 처리 중에 I/O 작업을 병렬로 처리함으로써 응답 시간을 단축하고, 서버의 효율성을 높일 수 있다.

장고의 비동기 ORM 기능은 `database_sync_to_async`와 같은 유틸리티를 활용하여 동기 ORM 메서드를 비동기 코드에서 사용할 수 있게 한다. 이는 기존의 동기적 데이터베이스 접근 방식을 비동기 컨텍스트로 사용할 수 있게 해준다.

#### 비동기 ORM 사용 예시

```python
from asgiref.sync import database_sync_to_async
from .models import MyModel

@database_sync_to_async
def get_data():
    return MyModel.objects.all()

async def my_view(request):
    data = await get_data()
    # 응답 처리

```

위의 예시에서 get_data 함수는 동기적으로 데이터베이스에서 데이터를 가져오지만, database_sync_to_async를 통해 비동기 함수로 래핑되어 비동기 뷰에서 호출할 수 있게 된다.

###MySQL과 PostgreSQL의 비동기 처리 비교

장고에서 MySQL과 PostgreSQL을 사용할 때 비동기 데이터베이스 접근에 있어 몇 가지 차이점이 존재한다. 각 데이터베이스의 특성과 장단점은 다음과 같다.

#### MySQL

- **비동기 드라이버**: MySQL은 `aiomysql`와 같은 비동기 드라이버를 통해 비동기 연결을 지원한다. 그러나 장고의 비동기 기능은 MySQL에 대한 공식 지원이 미흡하다. MySQL과 함께 사용할 경우 데이터베이스 접근에서 비동기 처리에 한계가 있을 수 있다.
- **성능**: MySQL은 많은 데이터베이스 작업에서 좋은 성능을 보여주지만, 비동기 처리 시 여러 연결을 관리해야 하는 오버헤드가 발생할 수 있다. 따라서 높은 동시성을 요구하는 어플리케이션에서는 성능이 저하될 수 있다.

#### PostgreSQL

- **비동기 드라이버**: PostgreSQL은 `asyncpg`와 같은 비동기 드라이버를 제공하여 비동기 데이터베이스 접근을 최적화할 수 있다. 이러한 드라이버를 활용하면 장고의 비동기 ORM과 잘 통합될 수 있다.
- **성능**: PostgreSQL은 ACID(Atomicity, Consistency, Isolation, Durability) 원칙을 준수하면서 복잡한 쿼리 및 데이터베이스 작업에 대해 뛰어난 성능을 발휘한다. 비동기 드라이버를 사용하면 연결 관리 및 쿼리 성능이 향상되어, 높은 동시성을 처리하는 데 유리하다.

### 결론

장고의 비동기 데이터베이스 접근 기능은 장고 3.1 버전부터 제공되며, 비동기 뷰와 비동기 ORM 기능을 통해 데이터베이스와 비동기적으로 상호작용할 수 있다. MySQL과 PostgreSQL은 비동기 처리에서 서로 다른 특성을 지니고 있으며, PostgreSQL이 비동기 드라이버 및 성능 측면에서 장점을 가진다. 비동기 API와 데이터베이스 접근을 최적화하기 위해서는 적절한 드라이버 선택과 함께 데이터베이스 성능을 고려하는 것이 중요하다.


## 결론

Django는 웹 애플리케이션 개발을 효율적으로 수행할 수 있는 강력한 프레임워크이다. MTV 패턴에 기반한 구조, ORM을 통한 데이터베이스 처리, 유용한 관리 인터페이스 및 중간웨어의 활용은 Django가 많은 개발자에게 인기를 얻는 이유이다. 이 문서는 Django의 동작 원리를 심층적으로 이해하는 데 도움을 줄 것이며, 추가적인 학습과 개발을 위한 기초를 제공할 것이다.
