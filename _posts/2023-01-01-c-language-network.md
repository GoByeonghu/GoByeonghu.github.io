---
layout: post
title: Bidirectional Communication Implementation in C Using Raspberry Pi
subtitle: Establishing Wi-Fi Ad-Hoc Network for Direct Data Exchange
categories: 
  - C
tags: [c, wifi]
---


## 1. Wi-Fi
---

Wi-Fi는 무선 LAN(Wireless Local Area Network) 기술의 일종으로, 사용자들이 인터넷에 무선으로 연결할 수 있게 해주는 표준이다. Wi-Fi 기술은 IEEE 802.11 표준을 기반으로 하며, 다양한 주파수 대역(2.4GHz 및 5GHz)에서 동작한다. Wi-Fi는 고속 데이터 전송 속도를 제공하며, 이동성을 보장하여 사용자가 자유롭게 네트워크에 접속할 수 있도록 한다.

### 1.1 Wi-Fi의 주요 기능

- **무선 접속**: Wi-Fi는 사용자가 물리적 케이블 없이 인터넷에 접속할 수 있게 한다.
- **데이터 전송 속도**: 최신 Wi-Fi 표준은 최대 9.6Gbps의 전송 속도를 지원하여 대용량 데이터 전송이 가능하다.
- **네트워크 보안**: WPA3, WPA2와 같은 보안 프로토콜을 통해 사용자의 데이터를 안전하게 보호한다.

### 1.2 Wi-Fi의 구조

Wi-Fi 네트워크는 일반적으로 다음과 같은 구조로 이루어진다.

- **액세스 포인트(AP)**: 클라이언트 장치가 연결할 수 있는 무선 신호를 제공하는 장치이다.
- **클라이언트 장치**: 노트북, 스마트폰, 태블릿 등 Wi-Fi 신호에 연결하여 인터넷에 접근하는 장치이다.
- **라우터**: 인터넷과 내부 네트워크 간의 트래픽을 관리하고, 데이터 패킷을 라우팅하는 역할을 한다.

### 2. Wi-Fi Ad Hoc 네트워크란 무엇인가?

Wi-Fi Ad Hoc 네트워크는 사전 설정된 인프라 없이도 장치들 간에 직접 연결을 통해 데이터 통신이 이루어지는 형태의 네트워크이다. 이 네트워크는 중앙 관리 장치 없이도 각 장치가 서로 직접 통신할 수 있도록 한다.

#### 2.1 Ad Hoc 네트워크의 특징

- **인프라 필요 없음**: 중앙 집중식 장치 없이도 네트워크를 설정할 수 있다.
- **유연성**: 장치가 추가되거나 제거되는 경우에도 네트워크가 쉽게 구성될 수 있다.
- **동적 연결**: 사용자가 네트워크에 참여하고 떠날 수 있어 변화하는 환경에서도 유연하게 대응할 수 있다.

#### 2.2 Ad Hoc 네트워크의 구조

Ad Hoc 네트워크는 다음과 같은 구조로 이루어진다.

- **피어 투 피어(Peer-to-Peer) 연결**: 모든 장치가 동등한 위치에 있으며, 서로 직접 연결된다.
- **자체 관리**: 각 장치는 독립적으로 네트워크를 구성하고, 연결 상태를 관리한다.

### 3. Wi-Fi와 Wi-Fi Ad Hoc의 차이점

Wi-Fi와 Wi-Fi Ad Hoc 네트워크는 다음과 같은 주요 차이점이 있다.

- **구조적 차이**: Wi-Fi는 액세스 포인트와 라우터를 통해 중앙 집중적으로 관리되는 반면, Ad Hoc 네트워크는 장치 간의 직접 연결로 이루어진다.
- **유연성**: Ad Hoc 네트워크는 인프라 없이 쉽게 설정할 수 있어 이동성이 뛰어난 반면, Wi-Fi는 고정된 인프라에 의존한다.
- **보안**: Wi-Fi는 보안 프로토콜이 구현되어 있지만, Ad Hoc 네트워크는 상대적으로 보안성이 낮을 수 있다.

## Raspberry Pi 3b+에서 Wifi Ad hoc 설정하기
---

### Ad-Hoc 네트워크 구성 방법

#### 1. 라즈베리파이 설정:

두 대의 라즈베리파이에 Wi-Fi 모듈이 내장되어 있으며, Ad-Hoc 모드로 설정할 수 있다. 이를 위해서는 wpa_supplicant 설정 파일을 수정해야 한다.

#### 2. Ad-Hoc 네트워크 설정:

각 라즈베리파이의 `wpa_supplicant.conf` 파일을 수정하여 Ad-Hoc 모드를 설정

```bash
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```

아래 내용 추가

```
network={
    ssid="MyAdHocNetwork"
    mode=1
    frequency=2412
    key_mgmt=NONE
    proto=NONE
}
```

- `ssid`는 Ad-Hoc 네트워크의 이름이다.
- `mode=1`은 Ad-Hoc 모드를 설정한다.
- `frequency`는 주파수입니다. 2412MHz는 2.4GHz 대역의 기본 채널이다.

#### 3. 네트워크 인터페이스 설정:

각 라즈베리파이의 네트워크 인터페이스를 설정한다. dhcpcd.conf 파일을 수정하여 고정 IP를 설정할 수 있다.

```bash
sudo nano /etc/dhcpcd.conf
```

첫번째기기에 아래 내용추가

```
interface wlan0
static ip_address=192.168.1.1/24  # 첫 번째 라즈베리파이

```

두번쨰기기에 아래 내용 추가

```
interface wlan0
static ip_address=192.168.1.2/24  # 두 번째 라즈베리파이
```

#### 4. 재부팅

```bash
sudo reboot
```

## C언어 통신 구현
---

C언어에서는 소켓 API를 사용해 통신을 구현할 수 있다.
TCP 소켓 통신을 구현해 보자

### 서버

```c
// server.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>

#define PORT 8080
#define BUFFER_SIZE 1024

int main() {
    int server_fd, new_socket;
    struct sockaddr_in address;
    int addrlen = sizeof(address);
    char buffer[BUFFER_SIZE] = {0};
    char *response = "Hello from server";

    // 소켓 생성
    if ((server_fd = socket(AF_INET, SOCK_STREAM, 0)) == 0) {
        perror("Socket failed");
        exit(EXIT_FAILURE);
    }

    // 주소 설정
    address.sin_family = AF_INET;
    address.sin_addr.s_addr = INADDR_ANY;
    address.sin_port = htons(PORT);

    // 소켓과 포트를 바인딩
    if (bind(server_fd, (struct sockaddr *)&address, sizeof(address)) < 0) {
        perror("Bind failed");
        close(server_fd);
        exit(EXIT_FAILURE);
    }

    // 연결 대기
    if (listen(server_fd, 3) < 0) {
        perror("Listen failed");
        close(server_fd);
        exit(EXIT_FAILURE);
    }

    printf("Waiting for a connection...\n");

    // 연결 수락
    if ((new_socket = accept(server_fd, (struct sockaddr *)&address, (socklen_t*)&addrlen)) < 0) {
        perror("Accept failed");
        close(server_fd);
        exit(EXIT_FAILURE);
    }

    // 클라이언트의 메시지 수신
    int valread = read(new_socket, buffer, BUFFER_SIZE);
    printf("Received message: %s\n", buffer);

    // 응답 전송
    send(new_socket, response, strlen(response), 0);
    printf("Response sent\n");

    // 소켓 종료
    close(new_socket);
    close(server_fd);
    return 0;
}

```



### 클라이언트

```c
// client.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>

#define PORT 8080
#define BUFFER_SIZE 1024

int main() {
    int sock = 0;
    struct sockaddr_in serv_addr;
    char *message = "Hello from client";
    char buffer[BUFFER_SIZE] = {0};

    // 소켓 생성
    if ((sock = socket(AF_INET, SOCK_STREAM, 0)) < 0) {
        printf("Socket creation error\n");
        return -1;
    }

    // 서버 주소 설정
    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(PORT);

    // 서버 IP 주소를 여기서 설정
    if (inet_pton(AF_INET, "SERVER_IP_ADDRESS", &serv_addr.sin_addr) <= 0) {
        printf("Invalid address/Address not supported\n");
        return -1;
    }

    // 서버에 연결
    if (connect(sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) < 0) {
        printf("Connection failed\n");
        return -1;
    }

    // 서버에 메시지 전송
    send(sock, message, strlen(message), 0);
    printf("Message sent: %s\n", message);

    // 서버의 응답 수신
    int valread = read(sock, buffer, BUFFER_SIZE);
    printf("Received response: %s\n", buffer);

    // 소켓 종료
    close(sock);
    return 0;
}

```


### 응용 계층 프로토콜의 부재 이유

응용 계층 프로토콜이 없는 이유는 이 프로그램의 목적이 단순히 메시지를 주고받는 것이기 때문이다. 여기서 send() 함수로 문자열을 보내고 read() 함수로 서버 응답을 읽어오지만, 데이터의 해석이나 특정 구조로 구분되는 세부 규약이 필요하지 않다. 예를 들어, HTTP나 FTP 같은 응용 계층 프로토콜은 파일 전송, 웹 페이지 요청 등을 위한 구체적인 규약을 포함하지만, 이 코드에서는 이러한 복잡한 규칙이 필요 없다. 데이터가 일반 텍스트 형태로 간단하게 송수신되기 때문에 응용 계층 프로토콜을 별도로 지정하지 않아도 된다.