---
title: Gradle plugins vs build script 차이점 정리
excerpt: >-

date: "2022-07-24"
thumb_img_path: images/gradleLogo.png
thumb_img_alt: reactLogo
content_img_path: images/gradleLogo.png
content_img_alt: reactLogo
seo:
  title: Gradle plugins vs build script
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
사이드 프로젝트에서 gradle.build 파일을 수정하면서 생긴 궁금점을 정리합니다.

기존에는 apply plugin 이라는 명령을 통해 gradle에서 프로젝트에 Task를 추가했다.
그러나 현제 사용하고 있는 Gradle v.7 이후에서는 plugins 라는 명령을 통해 Task를 관리하고 있다.

참고자료 : 
https://docs.gradle.org/current/userguide/plugins.html
https://doughman.tistory.com/19