---
title: Spring Data & JPA Transaction 알아보기
excerpt: >-

date: "2022-08-19"
thumb_img_path: images/jpa-logo.png
thumb_img_alt: reactLogo
content_img_path: images/jpa-logo.png
content_img_alt: reactLogo
seo:
  title: Spring Data & JPA Transaction 알아보기
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


## JPA 트랜젝션
`@Transactional` 어노테이션의 옵션
- propagation : 전파 옵션
- isolation : 고립 옵션
- rollbackFor : 롤백 대상 지정
- readOnly : 생성, 수정, 삭제 제한 옵션

## ACID
`원자성(Atomicity)`, `일관성(Consistency)`, `격리성(Isolation)`, `지속성(Durability)`으로 이루어진 원칙

## JPA Persistence Context

## Dirty Checking!


참고자료 : 
- [김영한님 스프링캠프 강의 - Persistence Context](https://www.youtube.com/watch?v=xqEVS8LzxZM&t=1350s&ab_channel=springcamp.io)
- [우태코 셀리님 발표 - 트랜젝션](https://www.youtube.com/watch?v=aX9c7z9l_u8&ab_channel=%EC%9A%B0%EC%95%84%ED%95%9CTech)
- [우태코 예지니어스님 발표 - 트랜젝션](https://www.youtube.com/watch?v=e9PC0sroCzc&t=308s&ab_channel=%EC%9A%B0%EC%95%84%ED%95%9CTech)


