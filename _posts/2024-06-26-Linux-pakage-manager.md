---
layout: post
title: Linux 패키지 관리자
subtitle: Linux 기반 OS에서 사용가능한 패키지 관리자
categories: CS/OS
tags: [linux]
---

## 패키지 관리자

### wget
파일 다운로드 명령어

### Homebrew
Homebrew, 일반적으로 "brew"라고 불리는 이 도구는 macOS 및 Linux에서 소프트웨어 패키지를 설치하고 관리하는 데 사용되는 패키지 관리자입니다. Homebrew는 특히 macOS에서 널리 사용됩니다.

### Debian 기반 배포판 (예: Ubuntu)

#### apt
- **용도**: 패키지 설치, 업데이트, 제거, 검색
- **주요 명령어**:
  - `apt update`: 패키지 목록 업데이트
  - `apt upgrade`: 설치된 패키지 업그레이드
  - `apt install <package_name>`: 패키지 설치
  - `apt remove <package_name>`: 패키지 제거

#### dpkg
- **용도**: Debian 패키지 파일(.deb) 관리
- **주요 명령어**:
  - `dpkg -i <package.deb>`: 패키지 설치
  - `dpkg -r <package_name>`: 패키지 제거
  - `dpkg -l`: 설치된 패키지 목록 표시

#### apt-get
- **용도**: apt의 하위 명령어로, 패키지 설치, 업데이트, 제거, 검색
- **주요 명령어**:
  - `apt-get update`: 패키지 목록 업데이트
  - `apt-get upgrade`: 설치된 패키지 업그레이드
  - `apt-get install <package_name>`: 패키지 설치
  - `apt-get remove <package_name>`: 패키지 제거

#### apt-cache
- **용도**: 패키지 캐시 조회 및 검색
- **주요 명령어**:
  - `apt-cache search <package_name>`: 패키지 검색
  - `apt-cache show <package_name>`: 패키지 정보 표시

### Red Hat 기반 배포판 (예: Fedora, CentOS)

#### yum
- **용도**: 패키지 설치, 업데이트, 제거, 검색
- **주요 명령어**:
  - `yum update`: 모든 패키지 업데이트
  - `yum install <package_name>`: 패키지 설치
  - `yum remove <package_name>`: 패키지 제거
  - `yum search <package_name>`: 패키지 검색

#### dnf
- **용도**: yum의 차세대 패키지 관리 도구, 패키지 설치, 업데이트, 제거, 검색
- **주요 명령어**:
  - `dnf update`: 모든 패키지 업데이트
  - `dnf install <package_name>`: 패키지 설치
  - `dnf remove <package_name>`: 패키지 제거
  - `dnf search <package_name>`: 패키지 검색

#### rpm
- **용도**: Red Hat 패키지 매니저, .rpm 파일 관리
- **주요 명령어**:
  - `rpm -i <package.rpm>`: 패키지 설치
  - `rpm -e <package_name>`: 패키지 제거
  - `rpm -qa`: 설치된 패키지 목록 표시

### Arch Linux 기반 배포판

#### pacman
- **용도**: 패키지 설치, 업데이트, 제거, 검색
- **주요 명령어**:
  - `pacman -Syu`: 시스템 업데이트
  - `pacman -S <package_name>`: 패키지 설치
  - `pacman -R <package_name>`: 패키지 제거
  - `pacman -Ss <package_name>`: 패키지 검색

### OpenSUSE

#### zypper
- **용도**: 패키지 설치, 업데이트, 제거, 검색
- **주요 명령어**:
  - `zypper refresh`: 패키지 목록 업데이트
  - `zypper update`: 모든 패키지 업데이트
  - `zypper install <package_name>`: 패키지 설치
  - `zypper remove <package_name>`: 패키지 제거
  - `zypper search <package_name>`: 패키지 검색

### FreeBSD

#### pkg
- **용도**: 패키지 설치, 업데이트, 제거, 검색
- **주요 명령어**:
  - `pkg update`: 패키지 목록 업데이트
  - `pkg upgrade`: 설치된 패키지 업그레이드
  - `pkg install <package_name>`: 패키지 설치
  - `pkg remove <package_name>`: 패키지 제거
  - `pkg search <package_name>`: 패키지 검색
