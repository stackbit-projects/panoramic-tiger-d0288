---
title: Gradle 멀티모듈 시스템 구성 시 의존성 관리 방법 
excerpt: >-

date: "2022-07-18"
thumb_img_path: images/jpaLogo.png
thumb_img_alt: reactLogo
content_img_path: images/jpaLogo.png
content_img_alt: reactLogo
seo:
  title: Gradle 멀티모듈 시스템 구성 시 의존성 관리 방법 
  description: >-
    react start
  extra:
    - name: "og:type"
      value: article
      keyName: property
    - name: "og:title"
      value: React 시작하기
      keyName: property
    - name: "og:description"
      value: >-
        react start
      keyName: property
    - name: "og:image"
      value: images/jpaLogo.png
      keyName: property
      relativeUrl: true
    - name: "twitter:card"
      value: summary_large_image
    - name: "twitter:title"
      value: React 시작하기
    - name: "twitter:description"
      value: >-
        react start
    - name: "twitter:image"
      value: images/reactLogo.png
      relativeUrl: true
template: post
---


## 개요

Gradle7, Spring 환경에서 멀티모듈을 활용해 프로젝트를 구성하는 과정에서 의존성을 관리하는 것에 궁금함이 있었습니다.  
관련된 궁금점에 대해 해결이 된 부분을 정리합니다.

Gradle 7 이전에는 compile 명령을 활용해 의존성을 불러왔음.
그러나 이제는 `api`, `implementation` 명령을 통해 의존성을 관리함.  
- api : 불러오는 라이브러리가 의존하고 있는 관련된 다른 라이브러리들까지 같이 불러옴
- implementation : 불러오는 라이브러리만 불러옴

멀티모듈 프로젝트 구성시 공통모듈(common), 도메인 모듈(domain) 등의 모듈을 구축하게 된다.  
이러한 모듈들은 api 명령을 활용해 어플리케이션 모듈에서 해당 모듈들을 불러올 시 관련된 의존성을 한꺼번에 불러올 수 있도록 하는 것이 좋다.
반면에 어플리케이션 모듈에서 의존성을 불러올 경우에는 implementation을 활용해 별도의 하위 의존성을 노출하지 않도록 합니다.  

## 관련 포스팅
[우아한 형제들 기술블로그 - 멀티모듈 설계 이야기](https://techblog.woowahan.com/2637/)