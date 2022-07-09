---
title: Spring Security의 WebSecurityConfigurerAdapter Deprecated
excerpt: >-

date: "2022-03-28"
thumb_img_path: images/jpaLogo.png
thumb_img_alt: reactLogo
content_img_path: images/jpaLogo.png
content_img_alt: reactLogo
seo:
  title: Spring Security의 WebSecurityConfigurerAdapter Deprecated
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

기존 스프링에서 인증을 구현하기 위해 많이들 사용하는 `Spring Security`의 Config 설정 방식이 변경되었다.
이전에는 Config 파일에 WebSecurityConfigurerAdapter를 상속받아 구현하는 방식으로 사용했었는데, 
Spring Security 5.7.0-M2 버전 부터는 이 설정방식이 deprecated 되었다.

## 관련 포스팅
[스프링 커뮤니티 블로그글](https://spring.io/blog/2022/02/21/spring-security-without-the-websecurityconfigureradapter)