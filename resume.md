---
layout: resume
title: "About Me"
categories: AboutMe
---

<div style="border-radius: 50%; overflow: hidden; width: 180px; height: 180px;">
  <img src="assets/images/profile/gbh.jpg" alt="Profile Image" style="width: 100%; height: auto;">
</div>

## 고병후 Go Byeonghu
## Backend Developer
📅 1998.03.25  
📍 Incheon, Korea  

### **contact**  
📧 byeonghu@gmail.com 

📱 +82-10-6746-6704  

### **channel** 
`GitHub` [https://github.com/GoByeonghu](https://github.com/GoByeonghu) 

`Blog` [https://gobyeonghu.github.io/](https://gobyeonghu.github.io/)

---

<br/>

## About Me

개발에 대한 즐거움을 동기로, 지속적인 성장을 추구하는 개발자 고병후입니다. 제가 개발한 기능이 프로젝트에 기여하고, 사용자들에게 긍정적인 가치를 제공하는 데 큰 보람을 느낍니다. 이러한 목표를 달성하기 위해 두 가지 관점에서 노력을 기울여 왔습니다.

첫째, **지속적인 배움을 추구하고 있습니다.** 경희대학교 컴퓨터 공학과를 졸업하고 카카오 부트캠프를 수강하며 학식과 지식이 뛰어난 교수님, 강사님들에게 SW 지식을 배워왔습니다. 또한 추가적인 관심과 호기심이 생긴 주제들에 대해서는 서적과 공식 문서, 블로그 등을 통해 학습하여 개인 블로그에 정리해 오고 있습니다.

둘째, **여러 프로젝트에 직접 기여하고 있습니다.** 이를 위해 졸업 이후에도 직접 지인과 관련 커뮤니티를 통해 SFZ라는 팀을 구성하여 익명 고민 상담 SNS 등 다양한 개발 활동을 지속하고 있습니다. 특히 **백엔드 분야**에서 주로 활동하며 이를 통해 실무적인 경험을 쌓고, 협업 능력을 기르기 위해 끊임없이 노력하고 있습니다.

<br/>

---

<br/>

## Education

**컴퓨터공학 학사**  
경희대학교 국제캠퍼스, 컴퓨터공학과  
2017.03.01 ~ 2023.08.16

**SW 부트캠프 수료**  
카카오 테크 부트캠프 클라우드 in JEJU
2024.04.02 ~ 2024.10.11  

<br/>

---

<br/>

## Key Skills

- Programming Languages: **C++**, **Java**, **Python**
- Web Development: **Spring Boot**, **Django**
- Database Management: **MySQL**
- Infrastructure: **AWS EC2**
- Version Control: **Git**, **GitHub**


## Experienced Skills

- Programming Languages: **JavaScript**
- Web Development: **HTML**, **CSS**, **React.js**, **Node.js**, **express**
- Database Management: **Redis**
- Infrastructure: **AWS S3**, **Nginx**
- DevOps : **Docker**, **Kubernetes**, **GitHub Actions**
  
<br/>

---

<br/>

## Projects

  
### **LectBox**

***대학생을 위한 공유드라이브***

<iframe width="560" height="315" src="https://www.youtube.com/embed/aDn9ul5CLJE?si=X0Q7yYaWVrgXF4Yd" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

`기간` 2022.03~2022.06

`내용` 

대학생을 위한 강의용 공유 드라이브로, DropBox를 모티브로 개발된 SaaS입니다. 강의자와 수강자가 강의 자료, 과제 제출물 등에 해당하는 파일들을 공동으로 관리하며, 각 역할에 따라 폴더와 파일에 관한 접근 권한을 구분하여 편리하고 안전한 자료관리를 가능하게 합니다. 강의자가 강의실을 만들고 학생을 초대하면 학생은 강의자가 만든 '과제' 타입의 폴더에만 파일을 업로드, 다운로드 할 수 있습니다.
각 강의실과 파일에는 사용 가능 용량과 현재 사용 용량이 표기되어 클라우드 요금을 효율적으로 관리할 수 있게 합니다.

`주요기능`

- 강의자, 수강자 분리된 유저 관리 및 폴더 CRUD 접근 권한 제어
- 파일 업로드, 다운로드 및 미리보기 기능
- 유저 별 사용가능 데이터 용량 및 잔여 용량 표기

`인원` 5명 (FE 2명, BE 3명)

`역할` 
- 백엔드 개발
- AWS를 이용한 배포
- DB 설계

`사용 기술` 
- Back-End: Django, DRF, Amazon EC2
- Database: SQLite3
- Front-End: React, Amazon S3 & CloudFront 
- Storage: Amazon S3

`GitHub` [https://github.com/GoByeonghu/LectBox_back](https://github.com/GoByeonghu/LectBox_back)

<br/>

### **OpenSesame**

***ARM Trustzone을 활용한 보안 도어락 시스템***

`기간` 2022.09~2022.12

`특징` 2022 한국소프트웨어종합학술대회 제1저자 논문등록 및 발표자 선정작

`내용` 

키패드를 누르지 않아도 동작하거나 스마트폰과 연동하여 잠금을 해제하는 등의 기능을 하는 스마트 도어락을 보안성 있게 설계한 프로젝트입니다.
보안 성능을 높인 스마트 도어락의 구현을 위해 ARM TrustZone을 사용하여 일반 실행 영역인 REE와 보안 실행 영역인 TEE를 구분하여 부팅하고 기능을 그 민감성에 따라 분리하여 배치해 설계하였습니다. 또한 하드웨어적 기반이 다른 모바일 기기와 IoT 기기 두 단말의 TrustZone 간의 보안성을 확보한 통신 방식을 설계하기 위해 PGP를 도입하였습니다.

`아키텍처`

- Mobile system architecture overview

![image](https://github.com/capstone1-OpenSesame/OpenSesame/assets/92240138/c0d440b8-88e7-4c40-9127-6b06b9cab693)

- Mobile system architecture overview

![image](https://github.com/capstone1-OpenSesame/OpenSesame/assets/92240138/01ad2ffa-46fb-438e-b474-fe6be7cc2992)


`인원` 2명

`역할` 
- OpenSSL을 이용한 사용자 정보 암복호화 모듈 및 사용자 명령 유효성 판단 컴포넌트 개발
- 비대칭키(RSA)로 인증을 수행하고 해쉬(sha256)로 무결성을 보장하며 대칭키(AES)로 기밀성을 부여하는 PGP 구현
- 사용기술 연구 및 아키텍쳐 설계

`사용 기술` 
- Language: C
- OS: Ubuntu20.04 LTS
- Communication Method:  WIFI Ad-Hoc 
- Library: OpenSSL

`GitHub` [https://github.com/GoByeonghu/OpenSesame](https://github.com/GoByeonghu/OpenSesame)

<br/>

### **Bottles**

***익명 고민상담 SNS***

<iframe width="560" height="315" src="https://www.youtube.com/embed/qV8Ypq2U-DE?si=BYnQ10R0Z08WNDOH" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

`기간` 2023.09~2024.02

`내용` 

이미지, 비디오, 텍스트로 자유롭게 구성한 게시글을 올리고 서로 댓글을 달며 유저간에 팔로우를 할 수 있는 SNS입니다. 추가로 익명 게시글 기능을 추가하여,
익명 게시글로 작성된 피드는 작성자를 익명으로 유지한 체 랜덤한 유저에게 노출됩니다. 이를 수신한 유저는 마찬가지로 익명의 상태로 댓글을 달고,
서로 간에 실시간 채팅을 수행할 수 있습니다.

`주요기능`
- 유저간 팔로우 가능
- 이미지, 비디오, 텍스트로 자유롭게 순서와 내용을 구성한 피드 생성 가능
- 유저 아이디 기반 검색 및 자동완성 검색어 추천 기능
- 실시간 채팅 기능
- 게시글 랜덤 매칭 기능

`인원` 5명 (BE 2명, FE 2명, Android 1명)

`역할` 
- REST API 서버 개발
- Websocket 실시간 채팅 개발
- DBA 
- 백엔드 배포 담당
- 도메인 네임 연결 및 HTTPS, WSS 연결 관리

`문서`

[API 문서 다운로드](https://github.com/SFZ-Bottles/Bottles_BE/files/14855737/Bottles.rest.API.ver.1.2.0.pdf)

[DB 문서 다운로드](https://github.com/SFZ-Bottles/Bottles_BE/files/14855850/Bottles_DB.Modeling.ver.1.2.0.pdf)

`사용 기술`
- Back-End: Django, DRF, Amazon EC2,  Nginx
- Database: MySQL
- Front-End: React, Amazon S3 & CloudFront 
- Communication Method: WebSocket

`GitHub` [https://github.com/GoByeonghu/Bottles_BE](https://github.com/GoByeonghu/Bottles_BE)

<br/>

### **Decide4Me**

***결정을 도와주는 투표 웹사이트***

`기간` 2024.05~2024.06

`내용` 

사용자가 결정이 고민되는 사안에 대하여 원하는 만큼의 선택지를 두고 데드라인을 설정하여 게시글을 올리면 다른 유저가 투표를 진행하고,
데드라인을 넘어가면 최다 득표를 표기하는 결정도움 사이트입니다.

`인원` 1명

`역할` 
- 기획
- Spring 이용한 백엔드 개발
- Spring Security 이용한 인증 인가 개발
- React 이용한 프론트엔드 개발
- DBA
- Docker 관리

`사용 기술` 
- Back-End: Spring Boot, Spring Security, JPA,  Nginx
- Database: MySQL, Redis
- Front-End: React

`GitHub` [https://github.com/GoByeonghu/Decide4Me_BE](https://github.com/GoByeonghu/Decide4Me_BE)

<br/>

---

<br/>

## Awards

### **카카오X구름 제주 해커톤 카카오 대표이사상(3위) **
- 수여 기관: **주식회사 카카오**
- 수상 일자: 2024.08.07
- 수상 내용: 제주 유기견 문제를 해결하기 위하여 나와 닮은 유기견 매칭 서비스 "멍피"를 개발하여 수상

<br/>

---

<br/>

## Certifications

- **정보처리기사**
- **OPIc(English) IM1**