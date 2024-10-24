---
layout: post
title: 
subtitle: Classroom 테이블에서 Select Query 병목 문제를 해결하기 위한 최적화 전략 탐구 (라이브 강의 여부(is_live) 판단 쿼리의 성능 개선 방안)
categories: 
    - OceanAcademy
tags: [spring, mysql]
---






## 동기

이력서, 개발 공부 정리, 프로젝트 정리용 블로그가 필요했다.  
특히 공부한 내용을 체계적으로 정리하고 필요 시 꺼내올 수 있는 곳이 필요했다.  
기존에 드라이브에 파일로 정리하는 방식은 비효율적이었다.  
내 마음대로 구조와 템플릿을 만들 수 있는 깃헙을 사용하기로 결정했다.  
프론트 공부를 최근 하였는데 그 지식을 적용해 프로젝트 형식으로 블로그를 만들어 보고 싶다.

## 요구사항

- 다양한 템플릿의 페이지
  1. 이력서
  2. 프로젝트
  3. CS 정리
- 트리 형식의 페이지 구조 가능
- 댓글 가능
- SEO 적용
- 원하는 페이지, 원하는 위치에 광고 적용
- 조회수 확인 가능
- 마크업 여부: 그냥

## 구현 방법 모색

**조건: GitHub Pages 이용**  
**GitHub Pages**: GitHub에서 제공하는 정적 웹페이지(static webpage) 호스팅 서비스

### 방법

1. **Jekyll**  
   홈페이지: [Jekyll](https://jekyllrb-ko.github.io/)  
   특징: 마크다운, Liquid, HTML & CSS 등 정적 페이지 바로 호스팅. GitHub Pages가 이것을 이용한 것이다.  
   Jekyll은 GitHub 설립자 중 한 명이 Ruby 언어를 통해 개발한 프레임워크이다.  
   GitHub 자체적으로 Jekyll Contents Management System을 내장하고 있어 호스팅에 적합하다.  
   Jekyll은 개발자들이 애용하는 GitHub에서 개발한 툴로 이미 잘 알려진 WordPress의 강력한 경쟁자로 성장하고 있다.  
   Jekyll의 핵심 역할은 텍스트 변환 엔진으로, HTML/Markdown 등의 마크업 언어로 글을 작성하면 미리 정의해 놓은 규칙에 따라 다양한 레이아웃으로 포장하여 정적 웹사이트를 만들어 준다.  
   사용자는 `_config.yml` 또는 `_posts` 폴더 등의 수정 및 추가를 통해 원하는 기능을 구현할 수 있다.  
   출처: [Today I Learned @cheers_hena](https://cheershennah.tistory.com/214)

- Jekyll 테마 선택하여 포크해서 이용한다.  
  [테마 포크하기](https://velog.io/@shg4821/%EA%B9%83%ED%97%88%EB%B8%8C-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0-1)

### 참고할 만한 블로그

- [적용 방법](https://supermemi.tistory.com/entry/%EB%82%98%EB%A7%8C%EC%9D%98-github-%EB%B8%94%EB%A1%9C%EA%B7%B8-Jekyll-%EC%9C%BC%EB%A1%9C-%EA%BE%B8%EB%A9%B0-%EB%B3%B4%EC%9E%90-gitHubio)  
- [적용 방법 2 (로컬 설치)](https://supermemi.tistory.com/146)  
- [SEO 적용법](https://deku.posstree.com/ko/jekyll/seo/)

## 공부

### 루비란

- **Ruby**: 언어
- **레일즈**: 프레임워크
- **Gem**: 라이브러리

루비 개념: [루비 개념](https://blog.naver.com/potter777777/220605570577)  
윈도우 Gem 및 Jekyll 설치 가이드: [설치 가이드](https://blog.psangwoo.com/coding/2017/04/02/install-jekyll-on-windows.html)

**Liquid 템플릿 언어**: 웹 템플릿을 만들고 처리하는 데 사용되는 Ruby 기반의 간단하고 안전한 템플릿 언어. 주로 Shopify와 같은 e-commerce 플랫폼에서 사용된다.  
변수, 필터, 태그 등의 요소를 사용하여 동적으로 콘텐츠를 생성하고 표시할 수 있다. Liquid는 쉬운 문법을 가지고 있어 사용자가 템플릿을 유연하게 조작할 수 있도록 도와준다.

**번들러(Bundler)**: 웹 애플리케이션을 개발하기 위해 필요한 HTML, CSS, JS 등의 모듈화된 자원들을 모아서 하나 혹은 최적의 소수 파일로 결합(번들링)하는 도구.  
브라우저는 모듈화된 자바스크립트는 읽지 못하기 때문에 브라우저에서 코드를 실행하려면 반드시 번들러가 필요하다.

나는 `gem install bundler`를 통해 설치할 예정이다.

```
gem install bundler
gem install jekyll
```

이제 원하는 페이지로 이동해서 `gem new ./`를 실행하면 Jekyll 초기 세팅이 된다.

### 실행 방법

```
bundle install (번들 실행)
bundle exec jekyll serve (로컬 실행)
```
### GitHub Pages 브랜치 바꾸어 배포하는 방법

[브랜치 변경 배포 방법](https://data-scientist-techlog.tistory.com/entry/main-branch%EA%B0%80-%EC%95%84%EB%8B%8C-%EB%8B%A4%EB%A5%B8-branch%EB%A5%BC-%EB%B0%94%EB%9D%BC%EB%B3%B4%EB%8F%84%EB%A1%9D-%EC%84%A4%EC%A0%95-githubio-%EB%B8%94%EB%A1%9C%EA%B7%B8%EB%A7%8C%EB%93%A4%EA%B8%B0-3%ED%8E%B8)


