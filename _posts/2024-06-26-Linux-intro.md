---
layout: post
title: Linux 명령어
subtitle: Linux에서 자주 사용되는 명령어 정리
categories: CS/OS
tags: [linux]
---

## 자주 사용되는 명령어

### 명령어 정리

- **sudo** : 관리자 권한으로 명령을 실행
- **apt** : Debian 기반 리눅스 배포판(ububtu 등)에서 소프트웨어 패키지 관리
- **apt-get** 
  - install : 패키지 설치
  - -y : 모든 질문에 자동 yes응답
- **apt-cache**
  - search : 캐시에서 주어진 검색어와 일치하는패키지 검색
- **apt-key** : APT관련 키 관리
  - adv : 고급 옵션 사용
    - --keyserver : 키서버의 주소 지정
    - --recv-keys : 수신할 키 ID 지정
- **curl** : 데이터를 전송하기 위해 사용
  - s : 'silent' 모드로 실행하라는 의미입니다. 진행 상황이나 오류 메시지를 출력하지 않습니다.
  - O : 지정된 URL에서 파일을 다운로드하여 현재 디렉토리에 저장하라는 의미입니다. 파일 이름은 원래의 이름을 사용
- **source** : 현재 쉘에서 지정된 파일을 읽고 실행

<br/>

- **groupadd** [그룹명] : 새로운 그룹을 생성하는 명령어
- **useradd** [유저명]: 새로운 사용자 추가
  - -s 자용자 로그인 쉘을 설정(뒤에오는 경로 이용한 사용자 로그인 막는다.)
  - -g 사용자 초기그룹 설정
  - -d 홈 디렉토리 설정
- **wget** : 웹을 통해 파일 다운로드
- **tar** : tar 압축파일 다루는 명령
  - x : 압추 해제
  - f : 파일 이름 지정
  - z : gzip 압축형식 사용
  - -C : 압축 풀 디렉토리
  - --strip-components=1 : 압축해제시 최상단 부모 디렉토리 무시하고 내용물만 지정된 경로에 위치
- **chown**
  - -R [사용자]:[디렉토리] : 디렉토리 및  그 하위 모든 파일 소유자를 사용자로 바꿈
- **chmod** : 파일 권한 변경
  - +x [파일]: 파일에 실행권한 추가
- [사용자이름] **su** : 사용자 바꾸어 쉘 세션 전환
- **exit** : 세션 종료

<br/>

- **systemctl** : systemd(init 시스템의 후계자)를 사용하는 최신 리눅스 배포판에서 사용되는 서비스 관리 도구
  - daemon-reload : 시스템 서비스 설정 다시로드(설정파일 변경되어을때)
  - --now : 즉시시작
  - enable [서비스]: 해당 서비스 부팅시 자동시작
- **service** : 서비스 관리 명령어(리눅스 배포판에서 호환성을 유지하기 위해 지원되고 있습니다. System V는 systemd 이전에 사용되었던 초기 부팅 시스템)
  - restart : 재시작 

> hkp는 HTTP 키 서버 프로토콜
<br/>

> GPG 키란 :  GNU Privacy Guard(GPG)에서 사용하는 암호화 키입니다. GPG는 데이터 통신과 파일을 암호화하고 서명하는 데 사용되는 소프트웨어로, 개인 정보 보호 및 데이터 무결성을 보장한다.

<details>

  <summary>사용예시(톰캣과 Nginx설치)</summary>

  
  ```bash
  # apt에 java zulu repository의 인증키를 추가한다
  sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys ~~
  ```

  ```bash
  # zulu repository package 다운로드 및 설치
  curl -sO https://cdn.azul.com/zulu/bin/zulu-repo_1.0.0-3_all.deb
  sudo apt-get install -y ./zulu-repo_1.0.0-3_all.deb
  ```

  ```bash
  # zulu 패키지가 잘 추가되었는지 확인 후 설치
  sudo apt-cache search zulu17

  ```

  ```bash
  # zulu17-jdk 설치
  sudo apt-get install zulu17-jdk -y
  # 제대로 설치 됐는지 확인
  java -version
  ```

  ```bash
  # 현재 서버에 'JAVA_HOME' path설정
  sudo vim /etc/profile
  # 다음 페이지에 vim 으로 열어둔 파일에 아래 라인 추가
  export JAVA_HOME=/usr/lib/jvm/zulu17
  export PATH="$PATH:$JAVA_HOME/bin"
  ```

  ```bash
  # /etc/profile 에 적혀있는 내용을 서버에 즉시 반영함
  source /etc/profile
  ```

  ```bash
  # 그룹 생성 및 유저 추가.
  sudo groupadd tomcat
  sudo useradd -s /bin/false -g tomcat -d /app/tomcat tomcat
  ```

  ```bash
  wget -q https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.24/bin/apache-tomcat-10.1.24.tar.gz
  sudo tar xfz apache-tomcat-10.1.24.tar.gz -C /app/tomcat --strip-components=1
  # 유저 설정 -> chmod는 sudo 만으로는 *.sh 명령어가 작동하지 않음
  # sudo su 사용해서 root 유저로 스위칭 후 명령어 실행
  sudo chown -R tomcat: /app/tomcat
  sudo su
  chmod +x /app/tomcat/bin/*.sh
  exit
  ```

  ```bash
  # 톰캣 작동 여부 확인
  sudo /app/tomcat/bin/version.sh
  ```

  ```bash
  sudo vim /etc/systemd/system/tomcat.service
  ```

  ```bash
  sudo systemctl daemon-reload
  sudo systemctl --now enable tomcat
  sudo systemctl status tomcat --no-pager -l
  ```

  ```bash
  sudo apt update
  sudo apt install nginx -y
  ```

  ```bash
  sudo vim /etc/nginx/sites-available/default
  ```

  ```bash
  sudo nginx -t
  ```

  ```bash
  sudo service nginx restart
  ```


</details>