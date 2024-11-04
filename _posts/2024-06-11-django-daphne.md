---
layout: post
title: Daphne Intro
subtitle: "Daphne: An ASGI Server for Django" 
categories: 
  - Django
tags: [django]
---

Daphne는 Django Channels 프로젝트의 일부로, ASGI(Asynchronous Server Gateway Interface) 프로토콜을 구현하는 웹 서버이다. Django는 기본적으로 WSGI(Web Server Gateway Interface) 서버로 동작하지만, Daphne는 비동기 기능을 필요로 하는 웹 애플리케이션을 위해 설계되었다. 이 문서에서는 Daphne의 구조, 기능, 설정 및 사용 사례에 대해 자세히 설명한다.

## 1. ASGI와 Daphne의 필요성

### 1.1. 비동기 처리

ASGI는 비동기 처리를 지원하는 웹 서버와 애플리케이션 간의 통신 표준이다. 전통적인 WSGI는 동기식 프로그래밍 모델을 따르기 때문에 비동기 통신이 필요한 WebSocket이나 서버 푸시 기능을 구현하기 어려웠다. Daphne는 이러한 비동기 기능을 지원하여 Django가 WebSocket과 같은 실시간 통신을 구현할 수 있게 한다.

### 1.2. Channels와의 통합

Django Channels는 Django의 기본 기능을 확장하여 비동기 프로그래밍을 지원하는 라이브러리이다. Channels는 WebSocket, HTTP/2와 같은 다양한 프로토콜을 지원하며, Daphne는 이러한 프로토콜을 처리하는 데 적합한 서버이다.

## 2. Daphne의 아키텍처

Daphne는 ASGI 프로토콜을 통해 HTTP와 WebSocket 요청을 처리한다. 이 아키텍처는 다음과 같은 주요 구성 요소로 이루어져 있다.

### 2.1. HTTP 요청 처리

Daphne는 전통적인 HTTP 요청을 처리할 수 있다. 클라이언트로부터 HTTP 요청을 수신하면, 이를 ASGI 애플리케이션에 전달하고 응답을 클라이언트에게 반환한다. 이 과정은 다음과 같다.

1. HTTP 요청 수신
2. ASGI 애플리케이션으로 요청 전달
3. 애플리케이션으로부터 받은 응답을 클라이언트에 전송

### 2.2. WebSocket 요청 처리

Daphne는 WebSocket 연결을 관리할 수 있는 기능도 제공한다. WebSocket 요청이 수신되면, 연결을 수립하고 유지하며 데이터 프레임을 주고받는 과정은 다음과 같다.

1. WebSocket 연결 수립
2. 데이터 프레임 수신 및 처리
3. 클라이언트로 데이터 프레임 전송

## 3. 설치 및 설정

Daphne를 설치하고 설정하는 과정은 다음과 같다.

### 3.1. 설치

Daphne는 Python 패키지로 설치할 수 있으며, 다음 명령어를 사용하여 설치한다.

```bash
pip install daphne
```

### 3.2. Django 프로젝트 설정

Django 프로젝트에서 Daphne를 사용하기 위해서는 ASGI 설정이 필요하다. 다음은 Django 프로젝트의 asgi.py 파일의 예이다.

```python
import os
from django.core.asgi import get_asgi_application
from channels.routing import ProtocolTypeRouter, URLRouter
from channels.auth import AuthMiddlewareStack
from myapp import routing

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'myproject.settings')

application = ProtocolTypeRouter({
    "http": get_asgi_application(),
    "websocket": AuthMiddlewareStack(
        URLRouter(
            routing.websocket_urlpatterns
        )
    ),
})

```

이 설정에서는 HTTP 요청과 WebSocket 요청을 구분하고, 각각의 요청을 적절한 처리기로 전달하는 역할을 한다.

### 3.3. Daphne 실행
Daphne를 실행하기 위해서는 다음 명령어를 사용할 수 있다.
```bash
daphne myproject.asgi:application
```

이 명령어는 Django 프로젝트의 ASGI 애플리케이션을 실행하여 요청을 처리한다.

## 4. 사용 사례

Daphne는 다양한 웹 애플리케이션에서 사용될 수 있으며, 다음과 같은 예시가 있다.

### 4.1. 실시간 채팅 애플리케이션

Daphne를 사용하여 실시간 채팅 기능을 구현할 수 있다. 클라이언트는 WebSocket을 통해 서버와 연결하고, 메시지를 주고받는 것이 가능하다. 이를 통해 사용자는 다른 사용자와 즉시 소통할 수 있으며, 메시지 전송 지연이 거의 없다. 실시간 채팅 시스템은 사용자 경험을 향상시키는 데 중요한 역할을 한다.

### 4.2. 실시간 알림 시스템

사용자에게 실시간 알림을 전송하는 시스템을 구축할 때도 Daphne를 활용할 수 있다. 서버에서 이벤트가 발생하면 WebSocket을 통해 클라이언트에게 즉시 알림을 전달할 수 있다. 예를 들어, 새로운 댓글, 친구 요청 또는 시스템 공지 사항과 같은 실시간 알림은 사용자 참여를 높이고 애플리케이션의 동적 성격을 강화하는 데 기여한다.

### 4.3. 온라인 게임

Daphne는 멀티플레이어 온라인 게임의 서버로도 사용될 수 있다. 클라이언트 간의 실시간 상호작용을 지원하여 게임의 진행 상황을 즉각적으로 반영할 수 있다. 게임 서버는 WebSocket을 통해 플레이어의 움직임, 점수, 아이템 수집 등의 데이터를 실시간으로 동기화하여 사용자에게 원활한 게임 경험을 제공한다.

### 4.4. 데이터 대시보드

Daphne를 사용하여 실시간 데이터 대시보드를 구축할 수 있다. 서버는 실시간 데이터를 수집하고 이를 WebSocket을 통해 클라이언트에 전달하여 사용자에게 최신 정보를 제공한다. 예를 들어, 주식 거래 플랫폼이나 IoT 장치의 모니터링 대시보드는 사용자가 실시간으로 데이터를 확인할 수 있게 하여 의사 결정을 지원한다.

### 4.5. 비디오 스트리밍

Daphne는 비디오 스트리밍 서비스에서도 사용될 수 있다. 사용자는 WebSocket을 통해 실시간으로 비디오 데이터를 전송받을 수 있으며, 이는 낮은 지연 시간을 제공한다. 이와 같은 기술은 라이브 방송, 게임 스트리밍, 교육용 비디오 등 다양한 분야에서 활용된다.

이러한 다양한 사용 사례는 Daphne가 비동기 통신을 필요로 하는 현대 웹 애플리케이션에서 중요한 역할을 수행할 수 있음을 보여준다. 


## 참조

- [django daphne 사용법 공식문서](https://docs.djangoproject.com/ko/5.1/howto/deployment/asgi/daphne/)
- [django channels 공식문서](https://channels.readthedocs.io/en/latest/)