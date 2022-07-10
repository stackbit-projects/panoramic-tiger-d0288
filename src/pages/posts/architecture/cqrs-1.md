---
title: CQRS 데이터 동기화 방법 
excerpt: >-

date: "2022-07-10"
thumb_img_path: images/architecture-thumbnail.png
thumb_img_alt: reactLogo
content_img_path: images/architecture-thumbnail.png
content_img_alt: reactLogo
seo:
  title: CQRS 데이터 동기화 방법 
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
      value: images/reactLogo.png
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

CQRS를 공부하다 보면 이벤트 소싱과 연결되어 설명하는 것을 많이 볼 수 있다.  
`write` 시 이벤트를 발생해 `read` 저장소에 데이터를 동기화하는 데에 사용하는 것 같다.

이 부분에서 궁금한 점이 생겼다.  
보통 조회 속도를 높이기 위해 In-Memory 저장소를 많이 사용하는 것 같은데 이를 어떻게 저장하는 건지 궁금해졌다.

이를 위해서 Redis의 데이터 저장방식에 대해 알아보아야겠다.