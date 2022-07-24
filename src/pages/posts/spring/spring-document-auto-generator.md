---
title: Spring RestDocs with OpenAPI3
excerpt: >-

date: "2022-07-24"
thumb_img_path: images/springBootLogo-3.jpeg
thumb_img_alt: reactLogo
content_img_path: images/springBootLogo-3.jpeg
content_img_alt: reactLogo
seo:
  title: Spring RestDocs with OpenAPI3 
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

```groovy
plugins {
//    id 'org.asciidoctor.convert' version '1.5.8' # 예전 버전
    id 'org.asciidoctor.jvm.convert' version '3.3.2'
}
```
Spring RestDocs를 적용하다보면 ascii문서를 html로 변경하기 위한 플러그인을 사용하게 된다.
이때 위에 보이듯 2가지 종류가 있는데 `'org.asciidoctor.convert'`는 Gradle 6.7 이전(?)에 쓰이던 플러그인 인 것 같다 자세한 건 더 찾아보자