---
layout: post
title: 
subtitle: 
categories: 
  - WEB
tags: []
---

## 웹상에서 이미지 전송 방식
---

프론트엔드에서 백엔드로 이미지 파일을 통신하는 방식은 여러 가지가 있으며, 주로 HTTP 프로토콜을 사용하여 이미지 데이터를 전송하게 된다. 아래는 프론트엔드가 백엔드로 이미지 파일을 전송하는 방법을 설명하는 몇 가지 주요 방식을 정리한 것이다.

### 1. HTTP POST 요청과 MIME을 통한 파일 전송

가장 일반적으로 사용되는 방법은 HTTP POST 요청을 통해 이미지 파일을 전송하는 것이다. 이 방법은 클라이언트(프론트엔드)가 서버(백엔드)에 파일을 전송하는 기본적인 방식이다.

- **Content-Type**: 전송할 때 `Content-Type` 헤더를 `multipart/form-data`로 설정하여 파일을 전송한다. 이 방식은 파일과 데이터를 함께 전송하는데 적합하다.
- **데이터 포맷**: `FormData` 객체를 사용하여 파일을 포함한 데이터를 생성하고 전송한다. 이는 브라우저가 자동으로 MIME 타입과 경계를 처리해준다.

`multipart/form-data`는 MIME(Multipurpose Internet Mail Extensions)의 일종이다. 이 유형의 MIME는 주로 웹 폼을 통해 파일과 데이터를 함께 전송할 때 사용된다. 

#### MIME과 multipart/form-data :

- **MIME 타입**: MIME은 인터넷에서 파일의 유형을 정의하기 위해 사용되는 표준이다. `multipart/form-data`는 여러 개의 부분으로 나뉘어진 데이터를 포함할 수 있는 MIME 타입으로, 각 부분은 자체적으로 독립적인 콘텐츠 타입을 가질 수 있다.
  
- **사용 용도**: 일반적으로 HTML 폼에서 파일 업로드와 같은 다양한 데이터 타입을 전송하기 위해 사용된다. 예를 들어, 이미지 파일, 텍스트 데이터 등을 함께 전송할 수 있다.

- **구조**: `multipart/form-data`는 각 데이터 부분이 `boundary`로 구분되며, 각 부분은 헤더와 본문으로 구성된다. 헤더는 해당 부분의 메타데이터를 포함하고, 본문은 실제 데이터이다.

따라서 `multipart/form-data`는 MIME의 한 종류이며, 주로 웹에서 파일 업로드와 같은 작업에 널리 사용된다.

#### 예시 코드 (JavaScript):

```javascript
const formData = new FormData(); formData.append('image', selectedFile); // selectedFile은 사용자가 선택한 파일 객체

fetch('/upload', { method: 'POST', body: formData, }) .then(response => response.json()) .then(data => { console.log('Success:', data); }) .catch((error) => { console.error('Error:', error); });
```


### 2. Base64 인코딩을 통한 이미지 전송

이미지를 Base64 인코딩하여 문자열로 변환한 후, 이 문자열을 HTTP 요청의 본문에 포함하여 전송하는 방법이다. 이 방식은 이미지 데이터가 상대적으로 작을 때 유용하다.

- **인코딩**: 이미지를 Base64로 인코딩하면 바이너리 데이터가 문자열 형식으로 변환된다.
- **전송**: 이 문자열을 JSON 형태로 포함하여 전송할 수 있다.

#### 예시 코드 (JavaScript):

```javascript
const reader = new FileReader(); reader.onloadend = function() { const base64data = reader.result; // Base64 인코딩된 이미지 데이터 fetch('/upload', { method: 'POST', headers: { 'Content-Type': 'application/json', }, body: JSON.stringify({ image: base64data }), }) .then(response => response.json()) .then(data => { console.log('Success:', data); }) .catch((error) => { console.error('Error:', error); }); }; reader.readAsDataURL(selectedFile); // selectedFile은 사용자가 선택한 파일 객체
```


### 3. WebSocket을 통한 이미지 전송

WebSocket을 사용하면 클라이언트와 서버 간의 지속적인 연결을 유지하면서 이미지를 전송할 수 있다. 이는 실시간 통신이 필요한 애플리케이션에서 유용하다.

- **전송**: WebSocket을 통해 바이너리 데이터를 전송할 수 있으며, 이미지 파일을 Blob 형태로 전송할 수 있다.

#### 예시 코드 (JavaScript):

```javascript
const socket = new WebSocket('ws://yourserver.com/socket');

socket.addEventListener('open', function (event) { const reader = new FileReader(); reader.onload = function() { const arrayBuffer = reader.result; // ArrayBuffer로 읽어들인다. socket.send(arrayBuffer); // 이미지 데이터 전송 }; reader.readAsArrayBuffer(selectedFile); // selectedFile은 사용자가 선택한 파일 객체 });
```

### 4. 파일전송 프로토콜 사용(FTP)

FTP (File Transfer Protocol)는 파일 전송을 위한 표준 네트워크 프로토콜로, 여러 클라이언트와 서버 간에 파일을 전송할 수 있도록 한다. FTP의 보안성을 높이기 위한 방법으로 SFTP와 FTPS가 있다. 아래에서는 FTP, SFTP, FTPS의 개념과 특징을 설명하겠다.

### 1. FTP (File Transfer Protocol)

FTP는 클라이언트와 서버 간에 파일을 전송하는 데 사용되는 프로토콜이다. FTP는 TCP/IP 프로토콜 위에서 동작하며, 주로 다음과 같은 기능을 제공한다.

- **파일 전송**: 클라이언트에서 서버로 또는 서버에서 클라이언트로 파일을 전송할 수 있다.
- **디렉터리 목록**: 서버의 파일 및 디렉터리 목록을 조회할 수 있다.
- **파일 삭제 및 이름 변경**: 서버에서 파일을 삭제하거나 이름을 변경할 수 있다.

FTP는 기본적으로 암호화되지 않으므로 보안에 취약하다.

#### 2. SFTP (SSH File Transfer Protocol)

SFTP는 SSH(Secure Shell) 프로토콜을 통해 파일 전송을 수행하는 프로토콜이다. SFTP는 FTP와는 달리 보안성을 고려하여 설계되었으며, 주요 특징은 다음과 같다.

- **암호화**: 모든 데이터 전송이 SSH를 통해 암호화되므로 보안성이 높다.
- **단일 연결**: 데이터와 제어 신호가 같은 연결을 통해 전달된다.
- **방화벽 친화적**: 포트가 하나만 사용되기 때문에 방화벽에서 관리하기가 용이하다.

SFTP는 비밀번호 인증 외에도 공개키 기반 인증을 지원하여 보안성을 더욱 강화할 수 있다.

#### 3. FTPS (FTP Secure)

FTPS는 FTP 프로토콜에 SSL/TLS 보안 프로토콜을 추가하여 보안성을 강화한 것이다. FTPS는 두 가지 모드, 즉 명시적(Explicit)과 암시적(Implicit) 모드로 운영된다.

- **명시적 FTPS**: 클라이언트가 FTP 서버에 연결한 후, 명시적으로 SSL/TLS 암호화를 요청한다. 일반 FTP 포트(21번)를 사용한다.
- **암시적 FTPS**: 클라이언트가 SSL/TLS 연결을 위해 전용 포트(990번)로 직접 연결한다. 연결이 설정될 때부터 암호화된다.

FTPS는 기존의 FTP와 호환성이 있으나, 추가적인 보안 계층을 제공하여 전송 중 데이터의 기밀성을 보장한다.

#### 4. 비교

| 특성              | FTP                     | SFTP                    | FTPS                    |
|-------------------|------------------------|-------------------------|-------------------------|
| 보안 수준         | 낮음                   | 높음                    | 높음                    |
| 암호화            | 없음                   | SSH로 암호화           | SSL/TLS로 암호화       |
| 포트 사용         | 21번 (제어)            | 22번                    | 21번 (명시적), 990번 (암시적) |
| 연결 구조         | 별도 제어 및 데이터 채널 | 단일 채널              | 별도 제어 및 데이터 채널 |
| 방화벽 친화성     | 낮음                   | 높음                    | 중간                    |

- **FTP**는 기본 파일 전송 프로토콜이지만 보안성이 부족하다.
- **SFTP**는 SSH를 기반으로 하여 높은 보안성을 제공한다.
- **FTPS**는 SSL/TLS를 사용하여 FTP의 보안성을 강화한 방법이다.
- SFTP와 FTPS는 모두 안전한 파일 전송을 위해 널리 사용된다.


### 요약

- **HTTP POST 요청**: `multipart/form-data` 형식을 사용하여 파일을 전송한다.
- **Base64 인코딩**: 이미지를 문자열로 변환하여 JSON 형태로 전송한다.
- **WebSocket**: 지속적인 연결을 통해 바이너리 데이터를 전송할 수 있다.






## 백엔드 프론트엔드 이미지 스토리지 간의 이미지 전송 방식
---

PresignedURL





## 참조
- [우아한기술블로그](https://techblog.woowahan.com/11392/)
