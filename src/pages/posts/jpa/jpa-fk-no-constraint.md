---
title: Spring JPA 연관관계 유지하며 ForeignKey 제거하기
excerpt: >-

date: "2022-03-28"
thumb_img_path: images/jpa-logo.png
thumb_img_alt: reactLogo
content_img_path: images/jpa-logo.png
content_img_alt: reactLogo
seo:
  title: React 시작하기
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


## JPA에서 연관관계만 남기고 FK를 제거하려면 어떻게 하지?

Nest.JS에서 사용하던 방식 : https://jojoldu.tistory.com/605

## 결과

스프링에서는 foreignKey 옵션의 `ConstraintMode.NO_CONSTRAINT` 값을 적용하여 사용하면 된다.  
```java
@JoinColumns(
    value = ...,
    foreignKey = @ForeignKey(value = ConstraintMode.NO_CONSTRAINT))

```
출처: https://kim-solshar.tistory.com/70 [김솔샤르의 인사이트]



git pull origin main --allow-unrelated-histories





